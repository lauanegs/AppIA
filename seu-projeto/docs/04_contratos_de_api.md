# Contratos de API

## 1. Objetivo
Definir contratos REST inequívocos para o backend de controle de leitura, alinhados aos requisitos de cadastro, atualização de progresso, listagem, ordenação, busca e paginação.

## 2. Versionamento de API
- Base path: `/api/v1`
- A versão do contrato é explicitada no caminho para permitir evolução futura sem quebra de cliente.

## 3. Convenções gerais
- Formato de corpo: `application/json`
- Todos os endpoints retornam JSON.
- Timestamp em respostas: ISO 8601 UTC, por exemplo `2026-05-15T14:30:00Z`.
- Valores de status de livro usados na API: `to_read`, `reading`, `completed`.
- O campo `author` é opcional; quando omitido, deve ser salvo como `null` no banco.
- O campo `progress` é calculado pelo backend e retornado como inteiro entre `0` e `100`.
- A paginação é controlada pelo backend com `size` fixo em `10` e `page` opcional, 1-based.

## 4. Endpoints

### 4.1 Criar livro
- Método: `POST`
- Rota: `/api/v1/books`
- Descrição: cadastra um novo livro com status inicial `to_read` e `currentPage` igual a `0`.

#### Request body
```json
{
  "title": "O Pequeno Príncipe",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 96
}
```

#### Response 201 Created
```json
{
  "id": 1,
  "title": "O Pequeno Príncipe",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 96,
  "currentPage": 0,
  "progress": 0,
  "status": "to_read",
  "createdAt": "2026-05-15T14:30:00Z",
  "updatedAt": "2026-05-15T14:30:00Z"
}
```

#### Erros
- `400 Bad Request` - `BOOK_001`
  - mensagem: `Campo title é obrigatório e deve ser uma string não vazia.`
- `400 Bad Request` - `BOOK_002`
  - mensagem: `Campo totalPages é obrigatório e deve ser um inteiro maior que zero.`
- `400 Bad Request` - `BOOK_003`
  - mensagem: `Campo author, quando presente, deve ser uma string.`
- `400 Bad Request` - `BOOK_100`
  - mensagem: `Formato JSON inválido.`
- `500 Internal Server Error` - `BOOK_500`
  - mensagem: `Erro interno do servidor.`

### 4.2 Recuperar livro por id
- Método: `GET`
- Rota: `/api/v1/books/{id}`
- Descrição: retorna os dados completos de um livro específico.

#### Response 200 OK
```json
{
  "id": 1,
  "title": "O Pequeno Príncipe",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 96,
  "currentPage": 45,
  "progress": 47,
  "status": "reading",
  "createdAt": "2026-05-15T14:30:00Z",
  "updatedAt": "2026-05-15T15:10:00Z"
}
```

#### Erros
- `404 Not Found` - `BOOK_004`
  - mensagem: `Livro não encontrado.`
- `500 Internal Server Error` - `BOOK_500`
  - mensagem: `Erro interno do servidor.`

### 4.3 Atualizar progresso de leitura
- Método: `PUT`
- Rota: `/api/v1/books/{id}/progress`
- Descrição: atualiza apenas a `currentPage` de um livro, recalculando `progress` e `status` automaticamente.

#### Request body
```json
{
  "currentPage": 96
}
```

#### Response 200 OK
```json
{
  "id": 1,
  "title": "O Pequeno Príncipe",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 96,
  "currentPage": 96,
  "progress": 100,
  "status": "completed",
  "createdAt": "2026-05-15T14:30:00Z",
  "updatedAt": "2026-05-15T16:00:00Z"
}
```

#### Regras de atualização
- `currentPage` deve ser inteiro.
- `currentPage` deve ser maior ou igual a `0`.
- `currentPage` deve ser menor ou igual a `totalPages` do livro.
- O backend recalcula `progress = round((currentPage / totalPages) × 100)` e `status`:
  - `to_read` quando `currentPage == 0`
  - `reading` quando `0 < currentPage < totalPages`
  - `completed` quando `currentPage == totalPages`

#### Erros
- `400 Bad Request` - `BOOK_005`
  - mensagem: `Campo currentPage é obrigatório e deve ser um inteiro entre 0 e totalPages.`
- `404 Not Found` - `BOOK_004`
  - mensagem: `Livro não encontrado.`
- `400 Bad Request` - `BOOK_100`
  - mensagem: `Formato JSON inválido.`
- `500 Internal Server Error` - `BOOK_500`
  - mensagem: `Erro interno do servidor.`

### 4.4 Atualizar dados do livro
- Método: `PUT`
- Rota: `/api/v1/books/{id}`
- Descrição: permite editar os dados do livro, exceto o progresso. O backend ajusta apenas `title`, `author` e `totalPages`.

#### Request body
```json
{
  "title": "O Pequeno Príncipe - Edição Revisada",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 100
}
```

#### Response 200 OK
```json
{
  "id": 1,
  "title": "O Pequeno Príncipe - Edição Revisada",
  "author": "Antoine de Saint-Exupéry",
  "totalPages": 100,
  "currentPage": 96,
  "progress": 96,
  "status": "reading",
  "createdAt": "2026-05-15T14:30:00Z",
  "updatedAt": "2026-05-15T16:30:00Z"
}
```

#### Regras de atualização
- `title` é obrigatório e deve ser uma string não vazia.
- `author` é opcional; quando omitido, deve ser salvo como `null`.
- `totalPages` é obrigatório e deve ser um inteiro maior que `0`.
- Se `totalPages` for reduzido abaixo de `currentPage`, a requisição deve ser rejeitada.
- `progress` e `status` devem ser recalculados se `totalPages` for alterado.

#### Erros
- `400 Bad Request` - `BOOK_008`
  - mensagem: `Campo title é obrigatório e deve ser uma string não vazia.`
- `400 Bad Request` - `BOOK_009`
  - mensagem: `Campo totalPages é obrigatório e deve ser um inteiro maior que zero.`
- `400 Bad Request` - `BOOK_010`
  - mensagem: `totalPages não pode ser menor que currentPage atual do livro.`
- `404 Not Found` - `BOOK_004`
  - mensagem: `Livro não encontrado.`
- `400 Bad Request` - `BOOK_100`
  - mensagem: `Formato JSON inválido.`
- `500 Internal Server Error` - `BOOK_500`
  - mensagem: `Erro interno do servidor.`

### 4.5 Listar livros
- Método: `GET`
- Rota: `/api/v1/books`
- Descrição: lista livros com ordenação alfabética por título e paginação de `10` itens por página.
- Query params:
  - `status` opcional: `to_read`, `reading`, `completed`
  - `search` opcional: texto parcial para buscar no título (case insensitive)
  - `page` opcional: índice de página 1-based, padrão `1`
- Observação: o backend define `size = 10` de forma fixa.

#### Exemplo de requisição
`GET /api/v1/books?status=reading&search=príncipe&page=1`

#### Response 200 OK
```json
{
  "page": 1,
  "size": 10,
  "totalItems": 1,
  "totalPages": 1,
  "items": [
    {
      "id": 1,
      "title": "O Pequeno Príncipe",
      "author": "Antoine de Saint-Exupéry",
      "totalPages": 96,
      "currentPage": 45,
      "progress": 47,
      "status": "reading",
      "createdAt": "2026-05-15T14:30:00Z",
      "updatedAt": "2026-05-15T15:10:00Z"
    }
  ]
}
```

#### Regras de listagem
- Os resultados são sempre ordenados por `title` em ordem crescente.
- Se `status` não for informado, todos os livros são listados.
- Se `search` for informado, a busca deve retornar livros cujo título contenha o termo parcial.
- Se `page` não for informado, retorna a primeira página (`1`).

#### Erros
- `400 Bad Request` - `BOOK_006`
  - mensagem: `Parametro status inválido. Valores válidos: to_read, reading, completed.`
- `400 Bad Request` - `BOOK_007`
  - mensagem: `Parametro page inválido. Deve ser um inteiro maior ou igual a 1.`
- `500 Internal Server Error` - `BOOK_500`
  - mensagem: `Erro interno do servidor.`

## 5. Layout de erro padrão
Todas as respostas de erro devem retornar o mesmo formato:

```json
{
  "status": 400,
  "code": "BOOK_001",
  "message": "Descrição do erro.",
  "details": [
    {
      "field": "title",
      "message": "Campo title é obrigatório."
    }
  ]
}
```

- `status`: código HTTP retornado.
- `code`: código interno único do domínio.
- `message`: texto humano explicando o erro.
- `details`: lista opcional com campos específicos e mensagens de validação.

## 6. Regras de contrato
- `title`: string não vazia.
- `author`: string opcional, pode ficar vazia ou ser omitido.
- `totalPages`: inteiro obrigatório, `> 0`.
- `currentPage`: inteiro obrigatório na atualização, `0 <= currentPage <= totalPages`.
- `progress`: inteiro calculado pelo backend, entre `0` e `100`.
- `status`: campo somente leitura para o cliente; valores válidos: `to_read`, `reading`, `completed`.
- `createdAt` / `updatedAt`: timestamps obrigatórios no formato ISO 8601 UTC.
- `page`: inteiro 1-based opcional, padrão `1`.
- `size`: sempre `10` no backend, não configurável pelo cliente.
- Ordenação: por `title` ascendente em todas as listagens.

## 7. Pontos que precisam de validação antes do desenvolvimento
- Nenhum ponto principal restante. O contrato já define paginação 1-based, `size` fixo em `10`, endpoint de edição de dados do livro e `author` salvo como `null` quando omitido.

## 8. Proposta de adição ao glossário
- Incluir no `docs/09_glossario_dominio.md` os valores de status da API e a convenção `error_code` usada em respostas de erro para garantir consistência entre backend e frontend.

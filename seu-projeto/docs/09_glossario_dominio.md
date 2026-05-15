# Glossário de domínio

## 1. Termos do negócio

| Termo | Definição | Sinônimos proibidos | Exemplo de uso |
|---|---|---|---|
| **Livro** | Entidade principal do sistema. Representa um livro cadastrado pelo usuário, contendo título, autor (opcional), total de páginas e status atual. | obra, item, registro | "O usuário cadastrou um livro com 320 páginas." |
| **Total de páginas** | Número inteiro maior que zero que representa a quantidade total de páginas do livro. Campo obrigatório no cadastro. | tamanho, comprimento, extensão | "O livro tem total de páginas igual a 450." |
| **Página atual** | Número inteiro que representa a última página lida pelo usuário. Deve ser maior ou igual a zero e menor ou igual ao total de páginas. | página corrente, posição, marca-página | "O usuário atualizou a página atual para 150." |
| **Progresso** | Percentual calculado automaticamente pelo backend com base na fórmula `(página atual / total de páginas) × 100`, arredondado para duas casas decimais. | avanço, porcentagem, completion | "O progresso do livro é 47,50%." |
| **Status** | Estado atual do livro no ciclo de leitura do usuário. Assume exatamente um dos três valores definidos: `para ler`, `lendo` ou `concluído`. | situação, fase, etapa, estado | "O status do livro mudou para concluído." |
| **Para ler** | Valor de status atribuído automaticamente a todo livro recém-cadastrado. Indica que o usuário ainda não iniciou a leitura. | na fila, backlog, não iniciado | "O livro foi cadastrado com status para ler." |
| **Lendo** | Valor de status atribuído automaticamente quando o usuário registra a primeira página atual maior que zero. Indica leitura em andamento. | em andamento, em progresso, iniciado | "O status mudou para lendo após o primeiro registro de progresso." |
| **Concluído** | Valor de status atribuído automaticamente pelo backend quando o progresso atinge 100%, ou seja, quando a página atual é igual ao total de páginas. | finalizado, terminado, lido | "O livro foi marcado como concluído automaticamente." |
| **Cadastro de livro** | Ação do usuário de adicionar um novo livro ao sistema informando título, autor (opcional) e total de páginas. | criação, adição, inserção | "O usuário realizou o cadastro de livro com sucesso." |
| **Registro de progresso** | Ação do usuário de informar a página atual de um livro já cadastrado, acionando o recálculo do progresso e a possível mudança de status. | atualização de leitura, check-in, update de página | "O registro de progresso atualizou o percentual para 60%." |

---

## 2. Termos técnicos

| Termo | Definição | Contexto de uso |
|---|---|---|
| **React Native** | Framework obrigatório para desenvolvimento do aplicativo mobile, compatível com Android e iOS. | Frontend / Mobile |
| **Spring Boot** | Framework Java obrigatório para desenvolvimento da API REST do backend. Utiliza Spring MVC para estruturar controllers e o fluxo de requisições. | Backend |
| **Spring MVC** | Módulo do Spring usado para organizar o código backend em camadas: controller, service e repository. | Backend |
| **PostgreSQL** | Banco de dados relacional obrigatório, executado via Docker durante o desenvolvimento. | Banco de dados |
| **Docker** | Ferramenta de containerização obrigatória para executar o PostgreSQL localmente. | Ambiente de desenvolvimento |
| **Docker Compose** | Arquivo de configuração usado para subir o container do PostgreSQL de forma declarativa e reproduzível. | Ambiente de desenvolvimento |
| **Flyway** | Biblioteca de versionamento de schema do banco de dados, integrada ao Spring Boot. Gerencia migrações SQL de forma incremental. | Backend / Banco de dados |
| **Controller** | Camada do Spring MVC responsável por receber as requisições HTTP e delegar para a camada de service. | Backend |
| **Service** | Camada do Spring MVC responsável por aplicar as regras de negócio, como cálculo de progresso e mudança de status. | Backend |
| **Repository** | Camada do Spring MVC responsável pela comunicação com o banco de dados via Spring Data JPA. | Backend |
| **Entity** | Classe Java que representa uma tabela do banco de dados e é gerenciada pelo JPA. | Backend |
| **Endpoint** | Rota da API REST exposta pelo backend e consumida pelo frontend mobile. | Backend / Frontend |
| **REST** | Padrão arquitetural obrigatório para a API, com uso correto dos verbos HTTP (GET, POST, PUT, DELETE) e códigos de status HTTP. | Backend |
| **JSON** | Formato de troca de dados entre o frontend e o backend. Todo payload de requisição e resposta deve estar neste formato. | Backend / Frontend |
| **CORS** | Configuração obrigatória no Spring Boot para permitir que o app React Native consuma a API sem bloqueios de origem cruzada. | Backend |
| **Swagger / OpenAPI** | Ferramenta de documentação automática dos endpoints da API, integrada ao Spring Boot. | Backend |
| **Axios** | Biblioteca recomendada no React Native para realizar chamadas HTTP à API. Alternativa ao `fetch` nativo. | Frontend |

---

## 3. Convenções de nomenclatura

**Entidades (banco de dados e Java):**
- Nome em inglês, no singular e em PascalCase para classes Java: `Book`
- Nome em inglês, no singular e em snake_case para tabelas SQL: `book`

**Campos (banco de dados):**
- snake_case em inglês: `title`, `author`, `total_pages`, `current_page`, `progress`, `status`

**Campos (Java / JSON):**
- camelCase em inglês: `title`, `author`, `totalPages`, `currentPage`, `progress`, `status`

**Valores de status (banco de dados e JSON):**
- Sempre em inglês, snake_case: `to_read`, `reading`, `completed`

| Termo em português | Valor no sistema |
|---|---|
| Para ler | `to_read` |
| Lendo | `reading` |
| Concluído | `completed` |

**Endpoints da API:**
- Prefixo obrigatório: `/api/v1`
- Recurso principal em inglês, plural, kebab-case: `/api/v1/books`
- Exemplo de subrota: `/api/v1/books/{id}/progress`

**Variáveis no React Native:**
- camelCase em inglês: `bookList`, `currentPage`, `readingProgress`

**Arquivos de componente no React Native:**
- PascalCase: `BookCard.tsx`, `BookForm.tsx`, `ProgressBar.tsx`

**Migrations do Flyway:**
- Padrão obrigatório: `V{numero}__{descricao_em_snake_case}.sql`
- Exemplo: `V1__create_book_table.sql`

---

## 4. Termos ambíguos resolvidos

| Termo | Ambiguidade | Decisão final | Data |
|---|---|---|---|
| **Status** | Poderia ser gerenciado pelo usuário manualmente ou automaticamente pelo sistema. | O status é gerenciado automaticamente pelo backend com base no progresso. O usuário não altera o status diretamente. | A definir na inicialização |
| **Concluído** | Poderia ser acionado pelo usuário ou pelo sistema. | É acionado automaticamente pelo backend quando `página atual = total de páginas`, sem ação do usuário. | A definir na inicialização |
| **Autor** | Poderia ser obrigatório ou opcional. | O campo autor é opcional no cadastro do livro. | A definir na inicialização |
| **Progresso** | Poderia ser informado pelo usuário (percentual) ou calculado pelo sistema. | O usuário informa apenas a página atual. O progresso percentual é sempre calculado pelo backend. | A definir na inicialização |

---

## 5. Pedido para o Agente Documentador

Manter este arquivo consistente ao longo de todo o projeto. Toda nova definição, convenção ou resolução de ambiguidade deve ser proposta pelo agente responsável e aprovada pelo humano coordenador antes de ser registrada aqui. Nenhum termo é alterado ou removido sem validação humana. Em caso de conflito entre dois termos, abrir divergência no `08_log_de_evolucao.md` antes de qualquer alteração.
# Requisitos e regras de negócio

## 1. Premissas e riscos
- Premissa 1: O sistema é individual e não precisa de múltiplos usuários nem autenticação nesta fase.
- Premissa 2: O backend em Spring Boot deve calcular automaticamente o progresso com base em página atual e total de páginas.
- Premissa 3: O banco PostgreSQL será executado localmente em Docker Compose durante o desenvolvimento.
- Risco 1: Configuração de Docker Compose e PostgreSQL pode ser um obstáculo para desenvolvedores iniciantes.
- Risco 2: A ausência de autenticação exige cuidado com dados expostos em ambiente de desenvolvimento.
- Risco 3: Divergência entre frontend e contrato de API pode causar erros de integração se a documentação não estiver alinhada.

## 2. Requisitos funcionais
- RF1: O sistema deve permitir cadastrar um livro com título, autor opcional e total de páginas.
- RF2: O sistema deve armazenar o livro com status inicial `para ler` e página atual igual a zero.
- RF3: O sistema deve permitir atualizar a página atual de um livro cadastrado.
- RF4: O sistema deve recalcular o progresso como `(página atual / total de páginas) × 100` e armazenar como valor inteiro arredondado.
- RF5: O sistema deve atualizar o status do livro automaticamente para `lendo` quando página atual > 0.
- RF6: O sistema deve atualizar o status do livro automaticamente para `concluído` quando página atual = total de páginas.
- RF7: O sistema deve listar livros por status (`para ler`, `lendo`, `concluído`), com paginação de 10 itens por página e ordenação alfabética por título, usando parâmetros `page` e `size` fixos no backend.
- RF8: O sistema deve permitir buscar livros por título parcial na lista de livros.
- RF9: O sistema deve expor uma API REST com os endpoints necessários para cadastro, atualização de progresso, listagem, ordenação, busca e paginação.
- RF10: O sistema deve documentar os endpoints via Swagger/OpenAPI.

## 3. Requisitos não funcionais
- RNF1: A comunicação entre frontend e backend deve ser via HTTP/JSON.
- RNF2: A API deve ser simples, clara e adequada a um desenvolvedor iniciante em Spring Boot.
- RNF3: O banco deve ser modelado em PostgreSQL com nomes de tabela e coluna em snake_case.
- RNF4: O projeto deve usar Flyway para versionar o schema do banco de dados.
- RNF5: O serviço PostgreSQL deve ser iniciado com Docker Compose para facilitar a reprodução do ambiente.
- RNF6: A API deve permitir requisições de qualquer origem necessária para o app React Native (CORS configurado).
- RNF7: O backend deve aplicar validações de entrada para evitar gravação de dados inválidos.
- RNF8: O backend deve armazenar timestamps obrigatórios de criação e atualização para cada livro.
- RNF9: O backend deve controlar a paginação usando `page` e `size` fixos e retornar resultados ordenados por título.

## 4. Regras de negócio
- RB1: Todo livro cadastrado começa com status `para ler`.
- RB2: Se `current_page > 0` e `current_page < total_pages`, o status deve ser `lendo`.
- RB3: Se `current_page = total_pages`, o status deve ser `concluído`.
- RB4: `current_page` deve ser maior ou igual a zero e menor ou igual a `total_pages`.
- RB5: `total_pages` deve ser um inteiro maior que zero.
- RB6: `autor` é opcional e pode ficar em branco no cadastro do livro.
- RB7: O usuário não fornece o `progresso` diretamente; ele é calculado pelo backend.
- RB8: O progresso armazenado deve ser um inteiro entre 0 e 100, representando a porcentagem arredondada.
- RB9: A listagem por status deve retornar resultados ordenados alfabeticamente por título.
- RB10: A busca de livros por título deve aceitar texto parcial e retornar livros cujo título contenha o termo pesquisado.

## 5. Casos de uso prioritários
- CU1: Cadastro de livro com título, autor opcional e total de páginas.
- CU2: Atualização de leitura informando a página atual de um livro já cadastrado.
- CU3: Listagem de livros filtrados por status (`para ler`, `lendo`, `concluído`) com paginação de 10 itens por página.
- CU4: Listagem de todos os livros cadastrados com ordenação alfabética por título.
- CU5: Busca de livros por título parcial.
- CU6: Visualização do progresso calculado para cada livro.

## 6. Critérios de aceite
- CA1: Um livro pode ser cadastrado e recuperado pelo endpoint da API.
- CA2: Um livro cadastrado é salvo com `status = para ler` e `current_page = 0`.
- CA3: A atualização de `current_page` recarrega o livro com progresso correto e status atualizado.
- CA4: Quando `current_page = total_pages`, o livro retorna com `status = concluído` e `progress = 100`.
- CA5: A listagem por status retorna apenas livros com o status solicitado, ordenados por título.
- CA6: A listagem deve retornar no máximo 10 itens por página e suportar paginação.
- CA7: A busca por título parcial retorna apenas livros cujo título contenha o termo pesquisado.
- CA8: A API responde em JSON e aceita requisições do app React Native via CORS.
- CA9: O schema do banco é versionado pelo Flyway e o ambiente PostgreSQL sobe via Docker Compose.

## 7. Dependências entre requisitos
- RF1 é pré-requisito para RF2, RF3 e RF4.
- RF3 deve respeitar RB4 e RB5 para manter integridade de dados.
- RF4 depende de RF3 e RB7, pois o progresso não é informado diretamente.
- RF5 e RF6 dependem do cálculo correto do progresso definido em RF4.
- RF7 apoia todos os demais requisitos para manter qualidade e confiabilidade.

## 8. Lacunas e decisões pendentes
- Confirmação se é necessário armazenar timestamps de criação/atualização do livro neste escopo inicial.
- Definição de regras de paginação e ordenação na listagem de livros.
- Validação se será necessário suporte a pesquisa por título nesta fase.

## 9. Pedido para o Agente Arquiteto
Organize estes requisitos e indique se há alguma inconsistência no escopo atual, considerando que o projeto deve permanecer simples e legível para um desenvolvedor iniciante.
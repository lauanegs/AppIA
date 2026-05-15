# Modelagem de banco de dados

## 1. Objetivo da modelagem
Descrever a persistência mínima para suportar cadastro de livros, registro de progresso e listagem por status no PostgreSQL.

## 2. Entidades principais
- `book`: representa o livro cadastrado pelo usuário, com título, autor opcional, total de páginas, página atual, progresso e status.

## 3. Relacionamentos
- Não há relacionamentos entre tabelas neste escopo inicial, pois o sistema é individual e utiliza apenas a entidade `book`.
- A integridade é garantida por restrições de chave primária, validações de domínio e regras de check constraint.

## 4. Normalização e justificativa
- Aplicar até a 3ª forma normal (3NF) com uma única tabela `book` que não contém dados repetidos, atributos calculados armazenados apenas se necessário para facilitar consultas.
- Justificativa: o modelo é simples e evitar múltiplas tabelas mantém a solução clara para um desenvolvedor iniciante.

## 5. Padrões obrigatórios
- Tabela em inglês e snake_case: `book`.
- Colunas em snake_case: `title`, `author`, `total_pages`, `current_page`, `progress`, `status`, `created_at`, `updated_at`.
- Chave primária com identidade serial: `id BIGSERIAL PRIMARY KEY`.
- Valores de `status` devem ser restritos a `to_read`, `reading` e `completed`.
- `total_pages` deve ser `INTEGER NOT NULL CHECK (total_pages > 0)`.
- `current_page` deve ser `INTEGER NOT NULL CHECK (current_page >= 0 AND current_page <= total_pages)`.
- `progress` deve ser `INTEGER NOT NULL DEFAULT 0 CHECK (progress >= 0 AND progress <= 100)`.
- Indexar `status` para consultas de listagem por status.
- Indexar `title` para suportar ordenação alfabética e busca por título.
- Utilizar `created_at` e `updated_at` obrigatórios para facilitar manutenção e auditoria futura.

## 6. Estratégia de migração
- Usar Flyway com arquivo inicial `V1__create_book_table.sql`.
- O Spring Boot deve ser configurado para apontar para o schema padrão do PostgreSQL e aplicar migrações automaticamente ao iniciar.
- Manter a pasta `src/main/resources/db/migration` no backend para as versões do Flyway.

## 7. Script inicial
```sql
CREATE TABLE book (
    id BIGSERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    author TEXT,
    total_pages INTEGER NOT NULL CHECK (total_pages > 0),
    current_page INTEGER NOT NULL DEFAULT 0 CHECK (current_page >= 0 AND current_page <= total_pages),
    progress INTEGER NOT NULL DEFAULT 0 CHECK (progress >= 0 AND progress <= 100),
    status VARCHAR(16) NOT NULL DEFAULT 'to_read' CHECK (status IN ('to_read', 'reading', 'completed')),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_book_status ON book (status);
CREATE INDEX idx_book_title ON book (title);
```

- Observação: o campo `progress` é calculado pelo backend e armazenado para simplificar a leitura do app e evitar cálculos repetidos na consulta.
- Observação: o backend deve atualizar `updated_at` sempre que o registro de progresso for alterado.

## 8. Docker Compose comentado
```yaml
version: '3.9'
services:
  postgres:
    image: postgres:15
    container_name: appia_postgres
    environment:
      POSTGRES_USER: reader
      POSTGRES_PASSWORD: reader
      POSTGRES_DB: reading_app
    ports:
      - '5432:5432'
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U reader']
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

- Comentário: usar `postgres:15` por ser uma versão estável e compatível com Spring Boot.
- Comentário: `POSTGRES_DB` define o banco usado pelo backend; `POSTGRES_USER` e `POSTGRES_PASSWORD` devem ser sincronizados com a configuração do Spring Boot.
- Comentário: o volume `postgres_data` preserva os dados entre reinicializações.

## 9. Pedido para o Agente Arquiteto
Valide se a modelagem atende ao escopo inicial de controle de leitura e se há simplificações ou ajustes no schema que reduzam complexidade sem perder consistência.
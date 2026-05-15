# Visão geral do sistema

## 1. Objetivo do projeto
Desenvolver um sistema multiagente para apoiar a criação, análise e implementação de um aplicativo mobile de controle de leitura, composto por um frontend em React Native e uma API backend em Spring Boot com banco de dados PostgreSQL.

## 2. Problema que o sistema resolve

**Problema que o sistema resolve:**

O sistema atende à dificuldade que muitas pessoas têm em organizar e acompanhar suas leituras. É comum que leitores iniciem livros e não consigam manter consistência, percam o controle da página em que pararam ou não tenham uma visão clara do seu progresso ao longo do tempo.

**Processos automatizados:**

- O cálculo e a atualização visual do progresso de leitura (percentual com base nas páginas lidas).
- A organização dos livros por status (lendo, concluído, para ler).
- O armazenamento e a recuperação das informações dos livros cadastrados, persistidas no banco de dados via API.

**Resultado final esperado:**

Espera-se que o usuário tenha uma ferramenta simples, intuitiva e visualmente agradável para gerenciar suas leituras, permitindo acompanhar seu progresso, manter-se motivado e desenvolver o hábito de leitura de forma mais organizada e consistente.

## 3. Atores envolvidos

O sistema é utilizado pelo próprio usuário, que cadastra livros, atualiza o progresso de leitura e acompanha seu histórico pelo aplicativo mobile.

Não há um administrador nesta versão inicial, pois o sistema é individual e não possui gerenciamento centralizado.

Os dados gerados são consumidos pelo próprio usuário, que os utiliza para visualizar seu progresso, organizar suas leituras e acompanhar seu desempenho ao longo do tempo. Esses dados são armazenados no PostgreSQL e acessados pelo app via API REST.

## 4. Escopo inicial, dentro e fora

Dentro do projeto estão as funcionalidades essenciais para o controle de leitura: cadastro de livros, atualização do progresso (página atual), cálculo automático do percentual lido, listagem de livros por status (lendo, concluído, para ler) e visualização do progresso de forma simples. Também está dentro do escopo a API REST em Spring Boot e o banco de dados PostgreSQL rodando via Docker.

Ficam fora nesta fase inicial funcionalidades mais avançadas, como sistema de login e autenticação, integração com APIs externas de livros, recomendações personalizadas, avaliações detalhadas e compartilhamento de progresso.

## 5. Restrições técnicas

**Frontend:**
- React Native obrigatório para o desenvolvimento do aplicativo mobile.
- O app deve funcionar em Android e iOS.
- A comunicação com o backend deve ser feita via HTTP/JSON, utilizando `axios` ou `fetch`.

**Backend (projeto separado):**
- Spring Boot obrigatório para a construção da API REST.
- A API deve seguir o padrão REST com endpoints bem definidos e documentados.
- O agente pode utilizar Spring MVC para estruturar os controllers e o fluxo de requisições.
- Comunicação entre app e API feita via HTTP/JSON.

**Banco de dados:**
- PostgreSQL obrigatório, rodando via Docker.
- O schema deve ser versionado com Flyway ou Liquibase, já integrados ao Spring Boot.
- Não é permitido usar banco de dados em memória (como H2) em produção.

**Ambiente de desenvolvimento:**
- Docker e Docker Compose obrigatórios para subir o banco localmente.
- O agente não deve propor soluções que exijam conhecimento avançado de infraestrutura, considerando que o desenvolvedor é iniciante.
- Evitar dependências desnecessárias que aumentem a complexidade do projeto.

## 6. Premissas

- O usuário utilizará o app individualmente, sem necessidade de suporte a múltiplos usuários nesta versão.
- A autenticação não está no escopo inicial, portanto a API não precisa de camada de segurança por enquanto.
- O progresso de leitura será calculado automaticamente pelo backend com base nas páginas informadas pelo usuário.
- O banco de dados PostgreSQL estará rodando localmente via Docker durante o desenvolvimento.
- Os dois projetos (frontend e backend) serão desenvolvidos e versionados separadamente.
- O agente deve priorizar soluções simples, legíveis e bem documentadas, adequadas para um desenvolvedor iniciante.
- Toda sugestão de código ou estrutura deve vir acompanhada de explicação clara sobre o seu propósito.

## 7. Riscos conhecidos

**Risco 1 — Complexidade do ambiente Docker para iniciantes**
Configurar o Docker Compose com PostgreSQL pode ser um obstáculo inicial. Mitigação: o agente deve fornecer o arquivo `docker-compose.yml` pronto e comentado.

**Risco 2 — Problemas de CORS na integração React Native + Spring Boot**
Erros de CORS e de configuração de rede local são comuns nessa combinação. Mitigação: o agente deve orientar a configuração de CORS no Spring Boot desde o início do projeto.

**Risco 3 — Curva de aprendizado do Spring Boot**
Spring Boot tem bastante configuração implícita, o que pode confundir desenvolvedores iniciantes. Mitigação: preferir estrutura mínima gerada pelo Spring Initializr com apenas as dependências necessárias e explicações claras a cada etapa.

**Risco 4 — Desalinhamento entre frontend e contrato da API**
Sem documentação da API, o frontend pode consumir endpoints de forma incorreta. Mitigação: utilizar Swagger/OpenAPI para documentar os endpoints automaticamente desde o início.

## 8. Pedido para o Agente Arquiteto

Atue como arquiteto de software especialista em projetos para desenvolvedores iniciantes. Analise o cenário descrito neste documento e proponha a melhor estrutura inicial para este sistema, considerando os seguintes pontos:

1. **Arquitetura geral** — como o React Native, o Spring Boot e o PostgreSQL vão se comunicar.
2. **Estrutura de pastas** — sugira uma organização clara para o projeto Spring Boot (controllers, services, repositories, models) e para o projeto React Native.
3. **Módulos e responsabilidades** — quais são as principais entidades (ex: Livro, Leitura) e o que cada camada deve fazer.
4. **Contrato da API** — proponha os endpoints REST iniciais necessários para o app funcionar (cadastrar livro, atualizar progresso, listar livros por status).
5. **Banco de dados** — sugira o schema inicial das tabelas no PostgreSQL e forneça um `docker-compose.yml` comentado e pronto para uso.
6. **Dependências recomendadas** — liste apenas o necessário no Spring Initializr (Spring Web, Spring Data JPA, PostgreSQL Driver, Flyway, Lombok, Swagger).
7. **Riscos e ordem de implementação** — sugira por onde começar, considerando que o desenvolvedor é iniciante e precisa de um caminho claro e progressivo.

Priorize clareza, simplicidade e praticidade em todas as sugestões.
# Visão geral do sistema

## 1. Objetivo do projeto
Desenvolver um sistema multiagente para apoiar a criação, análise e implementação de um software.

## 2. Problema que o sistema resolve
Descrever com clareza qual dor o sistema atende, quais processos serão automatizados e qual resultado final é esperado.

Problema que o sistema resolve:

O sistema atende à dificuldade que muitas pessoas têm em organizar e acompanhar suas leituras. É comum que leitores iniciem livros e não consigam manter consistência, percam o controle da página em que pararam ou não tenham uma visão clara do seu progresso ao longo do tempo.

Processos automatizados:

o cálculo e a atualização visual do progresso de leitura (percentual com base nas páginas lidas).
a organização dos livros por status (lendo, concluído, para ler).
o armazenamento e recuperação das informações dos livros cadastrados.

Resultado final esperado:

Espera-se que o usuário tenha uma ferramenta simples, intuitiva e visualmente agradável para gerenciar suas leituras, permitindo acompanhar seu progresso, manter-se motivado e desenvolver o hábito de leitura de forma mais organizada e consistente.


## 3. Atores envolvidos
Informar quem usa o sistema, quem administra e quem consome os dados gerados.

O sistema é utilizado pelo próprio usuário, que cadastra livros, atualiza o progresso de leitura e acompanha seu histórico.

Não há um administrador nesta versão inicial, pois o sistema é individual e não possui gerenciamento centralizado.

Os dados gerados são consumidos pelo próprio usuário, que os utiliza para visualizar seu progresso, organizar suas leituras e acompanhar seu desempenho ao longo do tempo.

## 4. Escopo inicial, dentro e fora
Definir o que está dentro do projeto e o que fica fora nesta fase inicial.

Dentro do projeto estão as funcionalidades essenciais para o controle de leitura, como cadastro de livros, atualização do progresso (página atual), cálculo automático do percentual lido, listagem de livros por status (lendo e concluído) e visualização do progresso de forma simples.

Ficam fora nesta fase inicial funcionalidades mais avançadas, como integração com APIs externas, sistema de login e autenticação, recomendações de livros, avaliações detalhadas, compartilhamento.

## 5. Restrições técnicas
Informar a stack obrigatória, o banco de dados, o framework, o ambiente de desenvolvimento e qualquer regra que não pode ser violada.

## 6. Premissas
Descrever as decisões já tomadas e as suposições que a IA deve respeitar.

## 7. Riscos conhecidos

## 8. Pedido para o Agente Arquiteto
Atue como arquiteto de software. Analise o cenário e proponha a melhor estrutura inicial, considerando arquitetura, módulos, dependências e riscos.
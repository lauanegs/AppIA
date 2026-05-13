# Instruções persistentes para o Copilot

Este projeto opera sob o modelo multiagente descrito em
docs/00_orientacao_agentes.md. Antes de qualquer resposta, considere as
seguintes regras universais.

1. Você está atuando como um agente especializado. Seu papel é definido
   no primeiro prompt da sessão atual. Nunca extrapole esse papel.
2. Não invente informação. Em caso de dúvida, abra divergência seguindo
   o protocolo descrito na seção 6 do guia.
3. Use somente termos do glossário em docs/09_glossario_dominio.md.
4. Toda resposta termina com bloco RESUMO PARA VALIDAÇÃO HUMANA.
5. Não altere arquivos fora do escopo de escrita do agente atual.

Se a sessão começar sem prompt de agente especializado, pergunte qual
agente você deve assumir antes de prosseguir.
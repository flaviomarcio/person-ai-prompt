### A.I. Claude 
- Backup & Restore
   1. Script de backup
      ```bash
      ```
   2. Script de restore
      ```bash
      ```
- MCP Jira no Claude
   - Adicione o MCP e faça o **login**
      1. Registrar o Jira no Claude Code (roda no seu terminal, fora desta sessão):
         ```bash
         $ claude mcp add --transport sse --scope user atlassian https://mcp.atlassian.com/v1/sse
         
         #output   
         Added SSE MCP server atlassian with URL: https://mcp.atlassian.com/v1/sse to user config
         File modified: ${HOME}/.claude.json
         ```
      2. Autenticar:
         - Reabra o Claude Code
         - Digite **/mcp → selecione atlassian → Authenticate**
         - Abre o navegador → faça login com sua conta da empresa → autorize
   - Gerando Token
      1. Acesse: https://id.atlassian.com/manage-profile/security/api-tokens 
      2. Create API token
         - Passo 1 - Criando Token
            ![Criando token](images/a.i.claude-create-token-1.1.png)
         - Passo 2 - Copiando Token
            ![Criando token](images/a.i.claude-create-token-1.2.png)
      2. Registrando as credenciais do jira na sessão
         - Adicione em ${HOME}/.bashrc
            ```bash
            export JIRA_EMAIL="flavio.portela@ciss.com.br"
            export JIRA_TOKEN="COLE-AQUI-O-TOKEN-COPIADO"
            ```
         - Recarregue a sessão
            ```bash
            source ${HOME}/.bashrc
            ```
         - Entre novamente no claude
            ```bash
            claude --resume
            ```
   - Teste no Claude, escolha um card com comentários e anexos
      1. Use o **prompt** substituindo o numero da task:
         ```
         1. Teste o acesso ao jira.
         2. Teste leitura do card ABC-1234 
         3. Teste leitura de comentários
         4. Teste download de anexos
         ```
         > Nota: Possível bloqueio: se a empresa restringir apps de terceiros no Atlassian, o login vai falhar com "não autorizado" e aí o admin do Jira precisa liberar o app "Atlassian Rovo MCP Server".
# Prompts para criação de planos de ação

## Plano de ação baseado em XML de uma task

Tem por objetivo analise a tarefa juntamente com o codigo fonte da aplicação e gerando assim relatorios no formato markdown como:
- Aprendizado da A.I.
- Analise da tarefa
- Plano ação
- Relatório final

### Prompts
1. Analise da task vs repositorio do projeto
	```text
	DEFINICOES INICIAIS
	===================
		Parametros do usuário
			Se o usuário tem limitações registradas na memoria adeque as respostas a cada limitção.
			Limitaçõs: ex: TDA, TDAH, dislexia, etc.
		TASK-NAME
			Representacao de uma tarefa real
			Ferramenta: JIRA
			Identificador: TTT-1234 
		Diretorio do Workspace
			Local: ${HOME}/work/spaces
		Diretorio workspace da task
			Salvar tudo relacionado a task, como xml da task, anexos e resultados gerados durante a sessão como planos de ação, relatorios, etc...
			Local: ${HOME}/work/spaces/TASK-NAME 
		Diretorio do Projeto
			Local: Diretorio corrente (onde comando é executado)
	
	FLUXO DE EXECUCAO
	=================
	FASE 0: VALIDACAO E AUTORIZACAO (INICIO)
		0.1
			Crie se necessario o diretorio:
			- ${HOME}/work/spaces
			- ${HOME}/work/spaces/TASK-NAME
		0.2
			Solicite permissao para ler e gravar arquivos
			Locais: ${HOME}/work, ${HOME}/work/spaces, ${HOME}/work/spaces/TASK-NAME e diretorio atual	
		0.3
			Solicite permissao para usar o git
			Locais: diretorio corrente da sessão
		0.4 
			Se configurados MCP para acesso boards faça:
			- Existe task TASK-NAME?
			- Download do xml da task para TASK-NAME.xml
			- Download dos anexos e manter nome original
			- Download e leitura de tudo que é importa para a task
			- Tudo deve ser salvo no workspace da task
		0.5
			VALIDACAO CRITICA: arquivo TASK-NAME.xml deve existir
			Senao interrompa a execucao com erro
		0.6
			Valide se TASK-NAME.xml é formato Jira
			Deve ser Jira obrigatoriamente
			Se nao for Jira, interrompa a execucao
		0.7
			Checklist pre-requisitos:
			- TASK-NAME.xml da task existe?
			- Formato Jira valido?
			- Permissao de leitura em workspace?
			- Permissao de escrita em workspace?
			Se qualquer falhar: INTERROMPA 
	FASE 1: COLETA DE ARTEFATOS
	============================
		1.1
			Localize todos os arquivos *.* no workspace da task
			${HOME}/work/spaces/TASK-NAME
		1.2
			Classifique os arquivos encontrados: 
			1.2.1 XML Principal
				Arquivo: TASK-NAME.xml
				Status: Obrigatorio, ja validado em 0.4
				Acao: Extrair dados da task 
			1.2.2 Arquivos JSON
				Pattern: TASK-NAME*.json
				Importancia: Alta
				Conteudo: Logs, documentacao OpenAPI
				Acao: Validar sintaxe, extrair estrutura
	
			1.2.3 Arquivos de Imagem
				Pattern: TASK-NAME*.png, TASK-NAME*.jpg, etc
				Conteudo: Screenshots, prints de tela
				Acao: Aplicar OCR, extrair texto visivel
	
			1.2.4 Arquivos Markdown
				Pattern: TASK-NAME*.md
				Aviso: Podem ter sido gerados por IA
				Acao: NÃO considerar como evidencia da task
		1.3
			Para cada anexo (JSON, imagem):
			Identifique "palavras-chave" mencionadas
			Valide cruzando com codigo-fonte do repositorio
			Busque no repositorio por essas palavras-chave
			Exemplo: grep -r "PalavraDoLog" ./src
			Marque como "evidencia validada" ou "sem correspondencia"
	FASE 2: ANALISE CRITICA
	=======================
		2.1
			Analise o arquivo TASK-NAME.xml para compreender:
			- Escopo da tarefa
			- Requisitos de aceitacao
			- Criterios de sucesso
			- Dependencias mencionadas 
		2.2
			Para cada anexo de log ou print:
			Extraia informacoes relevantes
			Identifique classes ou metodos mencionados
			Identifique arquivos de configuracao mencionados
			Valide se essas referencias existem no repositorio 
		2.3
			Processe arquivos JSON:
			Valide sintaxe (JSON bem formado?)
			Se JSON invalido: log do erro, continue analise
			Se valido: extraia estrutura e significado
			Busque correspondencia com APIs ou logs do sistema 
		2.4
			Processe imagens via OCR:
			Extraia texto visivel da imagem
			Se OCR falhar: solicite interpretacao manual
			Marque manualmente o que nao foi reconhecido
			Busque palavras-chave no repositorio 
		2.5
			Documente todo aprendizado em:
			Arquivo: TASK-NAME-aprendizado-ai.md
			Conteudo:
			- Resumo do que aprendeu
			- Classes e metodos encontrados
			- Configuracoes relevantes
			- Validacoes cruzadas com repositorio
			- Gaps ou inconsistencias encontradas
	FASE 3: CRIACAO DO PLANO DE ACAO
	================================= 
		3.1
			Crie analise da tarefa com evidencias claras da clausa do problema
			Arquivo: TASK-NAME-analise-tarefa.md
		3.2
			Crie plano de acao executavel
			Arquivo: TASK-NAME-plano-acao.md 
		3.3
			Conteudo do plano deve incluir:
			- Escopo completo da tarefa
			- Classes que precisam ser modificadas
			- Metodos que precisam ser criados ou alterados
			- Arquivos de configuracao afetados
			- Passos executaveis em sequencia
			- Validacoes e testes a realizar
			- Ordem recomendada de implementacao 
		3.4
			Estruture o plano em secoes:		
			3.4.1 Resumo Executivo baseado no arquivo TASK-NAME.xml
				O que vai fazer
				Por que vai fazer
				Impacto esperado 
			3.4.2 Analise de Impacto
				Deixar claro por a analise levou a sugestão
				Deixar claro o impacto na aplicação.
			3.4.3 Passos de Implementacao
				Extrair objetivo da implementação do arquivo TASK-NAME.xml. 
			3.4.4 Validacoes
				Existindo validações então extrai-las do arquivo TASK-NAME.xml  
			3.4.5 Rollback (se necessario)
				Como reverter se der problema
				Sempre confirmar o rollback 
	FASE 4: RELATORIO FINAL 
	================================ 
		4.1
			Crie ou atualize TASK-NAME-relatorio-final.md
		4.2
			Conteudo do RELATORIO deve incluir:
			- Titulo do projeto / modificacao
			- Overview das mudancas
			- Classes documentadas com proposito
			- Metodos principais e assinatura
			- Arquivos de configuracao relevantes
			- Como compilar e testar
			- Exemplos de uso
			- Notas importantes 
	FASE 5: TRATAMENTO DE ERROS
	============================ 
		5.1 JSON Invalido
			Acao: Log do erro, continue analise
			Flag: Marque como "JSON invalido - verificar manualmente" 
		5.2 OCR Falhou
			Acao: Solicite interpretacao manual do print
			Flag: Marque como "OCR falhou - verificar print" 
		5.3 Referencia Nao Encontrada
			Acao: Continuar analise
			Flag: Documentar que classe/metodo nao foi localizado 
		5.4 TASK-NAME.xml Estrutura Inesperada
			Acao: PARAR execucao
			Erro: Reportar qual campo nao foi encontrado
			Requer: Investigacao manual
		5.5 TASK-NAME.xml Sem objetivo claro ou evidencias
			Acao: PARAR execucao
			Erro: Reportar qual campo nao foi encontrado
			Requer: Investigacao manual 
		5.6 Arquivo Corrompido
			Acao: PARAR execucao
			Erro: Informar qual arquivo esta corrompido
			Recomendacao: Recuperar versao valida
	
	FASE 6: CONFIRMACAO FINAL
	==========================
		6.1
			Resuma arquivos criados:
			- TASK-NAME-aprendizado-ai.md (gerado?)
			- TASK-NAME-analise-tarefa.md (gerado?)
			- TASK-NAME-plano-acao.md (gerado?)
			- TASK-NAME-relatorio-final.md (gerado?)
		6.2
			Liste proximos passos para implementacao
		6.3
			Indique se houve alguma interrupcao ou erro 
	RESUMO DO FLUXO
	===============
		Fase 0: Validacoes e Permissoes
		Fase 1: Coleta de Artefatos
		Fase 2: Analise Critica
		Fase 3: Criacao do Plano
		Fase 4: Geracao de Documentacao
		Fase 5: Tratamento de Erros (durante todo processo)
		Fase 6: Confirmacao e Proximos Passos
	```

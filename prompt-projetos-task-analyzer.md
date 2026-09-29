# Prompts para criação de planos de ação

## Análise da tarefa vs repositório do projeto
Tem por objetivo analisar a tarefa juntamente com o código-fonte da aplicação, gerando assim relatórios no formato markdown como:
- Aprendizado da A.I.
- Análise da tarefa
- Plano de ação
- Relatório final

Prompt
```text
DEFINIÇÕES INICIAIS
===================
	Parâmetros do usuário
		Se o usuário tem limitações registradas na memória adeque as respostas a cada limitação.
		Limitações: ex: TDA, TDAH, dislexia, etc.
	TASK-NAME
		Representação de uma tarefa real
		Identificador: TTT-1234 
	Diretório do Workspace
		Local: ${HOME}/work/spaces
	Diretório workspace da tarefa
		Salvar tudo relacionado à tarefa, como xml da tarefa, anexos e resultados gerados durante a sessão como planos de ação, relatórios, etc...
		Local: ${HOME}/work/spaces/tasks/TASK-NAME 
	Diretório do Projeto
		Local: Diretório corrente (onde o comando é executado)
	Conformidade de autorreferência:
		- Não se autorreferenciar nas documentações
		- Em vez de "Gerado por IA a partir de TASK-NAME.xml" usar "Gerado a partir de TASK-NAME.xml"

FLUXO DE EXECUÇÃO
=================
FASE 0: VALIDAÇÃO E AUTORIZAÇÃO (INÍCIO)
	0.1
		Solicite ao usuário o valor para TTT-1234
		Se não informado: INTERROMPA
	0.2
		Crie se necessário o diretório:
		- ${HOME}/work/spaces
		- ${HOME}/work/spaces/tasks
		- ${HOME}/work/spaces/tasks/TASK-NAME
	0.3
		Solicite permissão para ler e gravar arquivos:
		- ${HOME}/work/spaces/tasks/
		- ${HOME}/work/spaces/tasks/TASK-NAME
		- Diretório corrente
	0.4
		Solicite permissão de leitura para usar o git
		Locais: diretório corrente da sessão
	0.5 
		Se houver MCP configurado para acesso aos boards, faça:
		- Existe tarefa TASK-NAME?
		- Download do xml da tarefa para TASK-NAME.xml
		- Download dos anexos e manter nome original
		- Download e leitura de tudo que é importante para a tarefa
		- Tudo deve ser salvo no workspace da tarefa
		- Se a tarefa contiver anexos e o download falhar, tente com JIRA_EMAIL e JIRA_TOKEN.
		Se tarefa contiver anexos e o download não for possível: INTERROMPA 
		Se tarefa não existir no MCP: INTERROMPA 
	0.6
		VALIDAÇÃO CRÍTICA: arquivo TASK-NAME.xml deve existir
		Senão interrompa a execução com erro
	0.7
		Valide se TASK-NAME.xml podem estar em formatos diferentes, identificar Ferramenta, ex: Jira.
		- Deve ser legível como tarefa?
		Se qualquer falhar: INTERROMPA 
	0.8
		Checklist pré-requisitos:
		- TASK-NAME.xml da tarefa existe?
		- Formato TASK-NAME.xml legível e válido?
		- Permissão de leitura em workspace e workspace da tarefa?
		- Permissão de escrita em workspace e workspace da tarefa?
		Se qualquer falhar: INTERROMPA 
FASE 1: COLETA DE ARTEFATOS
============================
	1.1
		Localize todos os arquivos *.* no workspace da tarefa
		${HOME}/work/spaces/tasks/TASK-NAME
	1.2
		Classifique os arquivos encontrados: 
		1.2.1 XML Principal
			Arquivo: TASK-NAME.xml
			Status: Obrigatório, já validado em 0.4
			Ação: Extrair dados da task 
		1.2.2 Arquivos JSON
			Pattern: *.json
			Importância: Alta
			Conteúdo: Logs, documentação OpenAPI
			Ação: Validar sintaxe, extrair estrutura	
		1.2.3 Arquivos de Imagem
			Pattern: *.png, *.jpg, *.jpeg, *.bmp
			Conteúdo: Screenshots, prints de tela
			Ação: Aplicar OCR, extrair texto visível	
		1.2.4 Arquivos de Vídeos
			Pattern: *.gif, *.mp4, *.avi, *.mov, *.webM, *.HEIF
			Conteúdo: Vídeos curtos
			Ação: Aplicar OCR, extrair texto visível
		1.2.5 Arquivos Markdown
			Pattern: TASK-NAME*.md
			Aviso: Podem ter sido gerados por IA
			Ação: NÃO considerar como evidência da tarefa
	1.3
		Para cada anexo:
		Identifique "palavras-chave" mencionadas nos arquivos e ignorando vídeos longos para análise.
		Valide cruzando com código-fonte do repositório
		Busque no repositório por essas palavras-chave
		Exemplo: grep -r "PalavraDoLog" ./src
		Marque como "evidência validada" ou "sem correspondência"
FASE 2: ANÁLISE CRÍTICA
=======================
	2.1
		Analise o arquivo TASK-NAME.xml para compreender:
		- Tipo de tarefa se feat,feature,bugfix,fix, etc
		- Escopo da tarefa
		- Requisitos de aceitação
		- Critérios de sucesso
		- Dependências mencionadas 
	2.2
		Para cada anexo de log ou print:
		Extraia informações relevantes
		Identifique classes ou métodos mencionados
		Identifique arquivos de configuração mencionados
		Valide se essas referências existem no repositório 
	2.3
		Processe arquivos JSON:
		Valide sintaxe (JSON bem formado?)
		Se JSON inválido: log do erro, continue análise
		Se válido: extraia estrutura e significado
		Busque correspondência com APIs ou logs do sistema 
	2.4
		Processe imagens e videos via OCR:
		Extraia texto visível da imagem
		Se OCR falhar: solicite interpretação manual
		Marque manualmente o que não foi reconhecido
		Busque palavras-chave no repositório 
	2.5
		Documente todo aprendizado em:
		Arquivo: TASK-NAME-aprendizado-ai.md
		Conteúdo:
		- Resumo do que aprendeu
		- Classes e métodos encontrados
		- Configurações relevantes
		- Validações cruzadas com repositório
		- Gaps ou inconsistências encontradas
		- Onde a evidência foi encontrada
FASE 3: CRIAÇÃO DO PLANO DE AÇÃO
================================= 
	3.1
		Crie análise da tarefa com evidências claras da causa do problema
		Arquivo: TASK-NAME-analise-tarefa.md
	3.2
		Crie plano de ação executável
		Arquivo: TASK-NAME-plano-acao.md 
	3.3
		Conteúdo do plano deve incluir:
		- Escopo completo da tarefa
		- Classes que precisam ser modificadas
		- Métodos que precisam ser criados ou alterados
		- Arquivos de configuração afetados
		- Passos executáveis em sequência
		- Validações e testes a realizar
		- Ordem recomendada de implementacao 
	3.4
		Estruture o plano em seções:		
		3.4.1 Resumo Executivo baseado no arquivo TASK-NAME.xml
			O que vai fazer
			Por que vai fazer
			Impacto esperado 
		3.4.2 Análise de Impacto
			Deixar claro por que a análise levou a sugestão
			Deixar claro o impacto na aplicação.
		3.4.3 Passos de Implementação
			Extrair objetivo da implementação do arquivo TASK-NAME.xml. 
		3.4.4 Validações
			Existindo validações então extraí-las do arquivo TASK-NAME.xml  
		3.4.5 Rollback (se necessário)
			Como reverter se der problema
			Sempre confirmar o rollback 
FASE 4: RELATÓRIO DE CAUSA 
================================ 
	4.1
		Crie ou atualize TASK-NAME-causa.md
	4.2 Validação do tipo da tarefa:
		- Se a task é bugfix, fix, hotfix, ou análise (tipo compatível?)
		Se o tipo da tarefa não atender o requisito: ignore esta fase
	4.3
		Conteúdo do RELATÓRIO deve incluir:
		- Título do projeto / modificação
		- Relato principal do problema
		- Documentar código com problema
		- Como simular o problema
		- Notas importantes 
FASE 5: TRATAMENTO DE ERROS
============================ 
	5.1 JSON Inválido
		Ação: Log do erro, continue análise
		Flag: Marque como "JSON inválido - verificar manualmente" 
	5.2 OCR Falhou
		Ação: Solicite interpretação manual do print
		Flag: Marque como "OCR falhou - verificar print" 
	5.3 Referência Não Encontrada
		Ação: Continuar análise
		Flag: Documentar que classe/método não foi localizado 
	5.4 TASK-NAME.xml Estrutura Inesperada
		Ação: PARAR execução
		Erro: Reportar qual campo não foi encontrado
		Requer: Investigação manual
	5.5 TASK-NAME.xml Sem objetivo claro ou evidências
		Ação: PARAR execução
		Erro: Reportar qual campo não foi encontrado
		Requer: Investigação manual 
	5.6 Arquivo Corrompido
		Ação: PARAR execução
		Erro: Informar qual arquivo está corrompido
		Recomendação: Recuperar versão válida

FASE 6: CONFIRMAÇÃO FINAL
==========================
	6.1
		Resuma arquivos criados:
		- TASK-NAME-aprendizado-ai.md (gerado?)
		- TASK-NAME-analise-tarefa.md (gerado?)
		- TASK-NAME-plano-acao.md (gerado?)
		- TASK-NAME-relatorio-final.md (gerado?)
	6.2
		Liste próximos passos para implementacao
	6.3
		Indique se houve alguma interrupção ou erro 
RESUMO DO FLUXO
===============
	Fase 0: Validações e Permissões
	Fase 1: Coleta de Artefatos
	Fase 2: Análise Crítica
	Fase 3: Criação do Plano
	Fase 4: Geração de Documentação
	Fase 5: Tratamento de Erros (durante todo processo)
	Fase 6: Confirmação e Próximos Passos
```

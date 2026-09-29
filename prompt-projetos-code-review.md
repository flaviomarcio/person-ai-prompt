# Prompts para code review

## Exclusivo de validação das tarefas feature/bugfix/hotfix
Tem por objetivo validar apenas a modificação documentada na tarefa

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
	0.1 Ativar skill para codeview
	0.2
		Solicite ao usuário o valor para TTT-1234
		Se não informado: INTERROMPA
	0.3
		Solicite permissão para ler e gravar arquivos:
		- ${HOME}/work/spaces/tasks/TASK-NAME
		- Diretório corrente
	0.4
		VALIDAÇÃO CRÍTICA: arquivo TASK-NAME.xml deve existir
		Senão interrompa a execução com erro

FASE 1: COLETA DE ARTEFATOS
============================
	1.1
		Localize a tarefa TASK-NAME.xml em
		Local: ${HOME}/work/spaces/tasks/TASK-NAME
		Se não existir: INTERROMPA


FASE 2: ANÁLISE CRÍTICA
=======================
	2.1
		Analise o arquivo TASK-NAME.xml para compreender:
		- Tipo de tarefa se feat,feature,bugfix,fix, etc
		- Escopo da tarefa
		- Requisitos de aceitação
		- Critérios de sucesso
		- Dependências mencionadas 

FASE 3: EXECUÇÃO DO CODE REVIEW
================================= 
	3.1
		Documente o resultado do code review em:
		Arquivo: TASK-NAME-code-review.md
		Conteúdo:
		- Análise da modificação
		- Classes e métodos analisados e indicando o nome do código fonte
		- Pontos para melhoria com evidências e indicando o nome do código fonte
	3.2
		Desativar skill para codeview

FASE 4: TRATAMENTO DE ERROS
============================ 
	4.1 TASK-NAME.xml Estrutura Inesperada
		Ação: PARAR execução
		Erro: Reportar qual campo não foi encontrado
		Requer: Investigação manual
	4.2 TASK-NAME.xml Sem objetivo claro ou evidências
		Ação: PARAR execução
		Erro: Reportar qual campo não foi encontrado
		Requer: Investigação manual 
	4.3 Arquivo Corrompido
		Ação: PARAR execução
		Erro: Informar qual arquivo está corrompido
		Recomendação: Recuperar versão válida

FASE 5: CONFIRMAÇÃO FINAL
==========================
	5.1 Resuma arquivos criados:
		- TASK-NAME-code-review.md (gerado?)
	5.2 Indique se houve alguma interrupção ou erro 
RESUMO DO FLUXO
===============
	Fase 0: Validações e Permissões
	Fase 1: Coleta de Artefatos
	Fase 2: Análise Crítica
	Fase 3: Execução do code review
	Fase 4: Tratamento de Erros (durante todo processo)
	Fase 5: Confirmação e Próximos Passos
```

## Exclusivo de patterns e arquitetura
Tem por objetivo validar apenas patterns e arquitetura

Prompt
```text
Não implementado
```

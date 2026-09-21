# Prompts para code review

## Exclusivo de validação das tarefas feature/bugfix/hotfix
Tem por objetivo validar apenas a modificação documentada na tarefa

Prompt
```text
DEFINICOES INICIAIS
===================
	Parametros do usuário
		Se o usuário tem limitações registradas na memoria adeque as respostas a cada limitção.
		Limitaçõs: ex: TDA, TDAH, dislexia, etc.
	TASK-NAME
		Representacao de uma tarefa real
		Identificador: TTT-1234 
	Diretorio do Workspace
		Local: ${HOME}/work/spaces
	Diretorio workspace da tarefa
		Salvar tudo relacionado a tarefa, como xml da tarefa, anexos e resultados gerados durante a sessão como planos de ação, relatorios, etc...
		Local: ${HOME}/work/spaces/tasks/TASK-NAME 
	Diretorio do Projeto
		Local: Diretorio corrente (onde comando é executado)
	Conformidade de auto referenciando:
		- Não se auto referenciando nas documentações
		- Em vez de "Gerado por IA a partir de TASK-NAME.xml" usar "Gerado a partir de TASK-NAME.xml"

FLUXO DE EXECUCAO
=================
FASE 0: VALIDACAO E AUTORIZACAO (INICIO)
	0.1 Ativar skill para codeview
	0.2
		Solicite ao usuário o valor para TTT-1234
		Se não informado: INTERROMPA
	0.3
		Solicite permissao para ler e gravar arquivos:
		- ${HOME}/work/spaces/tasks/TASK-NAME
		- Diretorio corrente
	0.4
		VALIDACAO CRITICA: arquivo TASK-NAME.xml deve existir
		Senao interrompa a execucao com erro

FASE 1: COLETA DE ARTEFATOS
============================
	1.1
		Localize a tarefa TASK-NAME.xml em
		Local: ${HOME}/work/spaces/tasks/TASK-NAME
		Se não existir: INTERROMPA


FASE 2: ANALISE CRITICA
=======================
	2.1
		Analise o arquivo TASK-NAME.xml para compreender:
		- Tipo de tarefa se feat,feature,bugfix,fix, etc
		- Escopo da tarefa
		- Requisitos de aceitacao
		- Criterios de sucesso
		- Dependencias mencionadas 

FASE 3: EXECUCAO DO CODE REVIEW
================================= 
	3.1
		Documente o resultado do code review em:
		Arquivo: TASK-NAME-code-review.md
		Conteudo:
		- Analisa da modificação
		- Classes e metodos analisados e indicando o nome do código fonte
		- Pontos para melhoria com envidencias e indicando o nome do código fonte
	3.2
		Desativar skill para codeview

FASE 4: TRATAMENTO DE ERROS
============================ 
	4.1 TASK-NAME.xml Estrutura Inesperada
		Acao: PARAR execucao
		Erro: Reportar qual campo nao foi encontrado
		Requer: Investigacao manual
	4.2 TASK-NAME.xml Sem objetivo claro ou evidencias
		Acao: PARAR execucao
		Erro: Reportar qual campo nao foi encontrado
		Requer: Investigacao manual 
	4.3 Arquivo Corrompido
		Acao: PARAR execucao
		Erro: Informar qual arquivo esta corrompido
		Recomendacao: Recuperar versao valida

FASE 5: CONFIRMACAO FINAL
==========================
	5.1 Resuma arquivos criados:
		- TASK-NAME-code-review.md (gerado?)
	5.2 Indique se houve alguma interrupcao ou erro 
RESUMO DO FLUXO
===============
	Fase 0: Validacoes e Permissoes
	Fase 1: Coleta de Artefatos
	Fase 2: Analise Critica
	Fase 3: Execução do code review
	Fase 4: Tratamento de Erros (durante todo processo)
	Fase 5: Confirmacao e Proximos Passos
```

## Exclusivo de patterns e arquitetura
Tem por objetivo validar apenas patterns e arquitetura

Prompt
```text
Não implementado
```

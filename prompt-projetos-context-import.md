# Prompts para code review

Prompt para importação do contexto criado em um diretório e que se deseja utilizar no diretório local

- Exemplo: Uma investigação de um card AAA-9999 foi executada na main|master, ao final se faz necessário criar uma branch para a nova tarefa, seja ela feature|bugfix|hotfix.

Prompt
```text
DEFINIÇÕES INICIAIS
===================
	Parâmetros do usuário
		Se o usuário tem limitações registradas na memória adeque as respostas a cada limitação.
		Limitações: ex: TDA, TDAH, dislexia, etc.
    BRANCH-CONTEXT
		Representação do nome da branch do contexto a ser reaproveitado
		Identificador: build/descritivo
	Diretório do Workspace onde as branches existem
		Local: ${HOME}/work/spaces
	Diretório do contexto
		Diretório onde o contexto desejado deve existir (Contexto que será reaproveitado)
        Local: ${HOME}/work/spaces/BRANCH-CONTEXT
	Diretório do Projeto
		Local: ${PWD} (onde o contexto será reaplicado)

FLUXO DE EXECUÇÃO
=================
FASE 0: Identificação do diretório destino
	0.1
		Solicite ao usuário o valor para BRANCH-CONTEXT
		Se não informado: INTERROMPA
	0.2
		Solicite permissão para ler e gravar arquivos:
		Local: 
			- ${HOME}/work/spaces/BRANCH-CONTEXT
			- ${PWD}

FASE 2: COLETA DE ARTEFATOS
============================
	2.1
		Importar contexto gerado e trabalhado por A.I. no diretório 
		Local: ${HOME}/work/spaces/BRANCH-CONTEXT (onde o contexto será lido)
		Se não existir: INTERROMPA
	2.2
		Importar contexto gerado no diretório 
		Local: ${DIR} (onde o contexto será reaplicado)
		Se não existir contexto: INTERROMPA

RESUMO DO FLUXO
===============
	Fase 0: Validações e Permissões
	Fase 1: Coleta de Artefatos
```
# Prompts para criação de projetos python

## Api simples usando MVC

Tem por objetivo documentar classes e metodos das aplicações, bem como aquivos de configuração gerando assim um *README.md* para o projeto trabalhado

```text
DEFINICOES INICIAIS
===================
	Objetivo
		Documentar classes, métodos e arquivos de configuração de aplicações Python
		Gerar README.md automaticamente para o projeto trabalhado
	
	Framework
		Python 3.10+
		FastAPI ou Flask
	
	Padrão Arquitetural
		MVC com isolamento de responsabilidades
	
	Cobertura de Testes
		100% obrigatório

ESTRUTURA DO PROJETO
====================
	Diretórios Obrigatórios
		- controllers/
		- configs/
		- dtos/
		- models/
		- services/
		- tests/
		- db/
	Banco de Dados
		Sistema: PostgreSQL		
		Scripts em db/:
			- tables.sql (Definição das tabelas)
			- constraints-pk.sql (Constraints de chave primária)
			- constraints-fk.sql (Constraints de chave estrangeira)
			- indexes.sql (Índices)
			- views.sql (Views)
			- init-data.sql (Dados iniciais)
			- init-db.sql (Script consolidado para inicialização)	
	Containerização
		Dockerfile (Imagem para produção)
		docker-compose.yml (Ambiente local com PostgreSQL)
			Deve inicializar automaticamente com db/init-db.sql
ENTIDADE E DTO
==============
	Entity: Person
		Atributos: id, createdAt, updatedAt, name, active	
	DTO: PersonDto
		Atributos: id, createdAt, updatedAt, name, active
		Uso: Input e Output (mesmo DTO)
ENDPOINTS DA API
================
	GET /persons
		Descrição: Listar todas as pessoas
		Parâmetro: name (String, opcional)
		Retorno: List<PersonDto>	
	GET /persons/{id}
		Descrição: Buscar pessoa por ID
		Parâmetro: id (UUID)
		Retorno: PersonDto	
	POST /persons
		Descrição: Criar nova pessoa
		Parâmetro: person (PersonDto)
		Retorno: PersonDto	
	PUT /persons
		Descrição: Atualizar pessoa existente
		Parâmetro: person (PersonDto)
		Retorno: PersonDto	
	DELETE /persons/{id}
		Descrição: Remover pessoa
		Parâmetro: id (UUID)
		Retorno: Sem retorno (HTTP 204)
FLUXO DE EXECUCAO
=================
FASE 1: CONFIGURACAO DO PROJETO
	1.1 Criar projeto Python com estrutura MVC
	1.2 Definir arquivo requirements.txt com dependências mais recentes
		- Framework (FastAPI/Flask)
		- ORM (SQLAlchemy)
		- Banco de dados (psycopg2)
		- Testes (pytest, pytest-cov)

FASE 2: IMPLEMENTACAO DO BANCO DE DADOS
	2.1 Criar pasta db/ na raiz do projeto	
	2.2 Criar arquivo db/tables.sql
			Incluir tabela Person com campos: id (UUID, PK), createdAt (TIMESTAMP), updatedAt (TIMESTAMP), name (VARCHAR), active (BOOLEAN)	
	2.3 Criar arquivo db/constraints-pk.sql
			Definir constraints de chave primária	
	2.4 Criar arquivo db/constraints-fk.sql
			Definir constraints de chave estrangeira (se aplicável)	
	2.5 Criar arquivo db/indexes.sql
			Criar índices em campos de busca frequente (name, active)	
	2.6 Criar arquivo db/views.sql
			Se necessário, definir views (pode estar vazio inicialmente)	
	2.7 Criar arquivo db/init-data.sql
			Incluir dados iniciais de exemplo	
	2.8 Criar arquivo db/init-db.sql
			Consolidar todos os scripts anteriores em ordem correta de execução
FASE 3: IMPLEMENTACAO DA API
	3.1 Criar models/person.py
            Classe Person mapeada com banco de dados
			Atributos: id, createdAt, updatedAt, name, active	
	3.2 Criar dtos/person_dto.py
			Classe PersonDto com mesmos atributos do Model
			Validações de entrada/saída	
	3.3 Criar services/person_service.py
			Implementar lógica de negócio
			Métodos: list(), find(id), save(person), remove(id)	
	3.4 Criar controllers/person_controller.py
			Implementar endpoints GET, POST, PUT, DELETE
			Chamadas diretas aos serviços	
	3.5 Criar configs/database.py
			Configurar conexão com PostgreSQL
			Usar variáveis de ambiente para credenciais	
	3.6 Criar arquivo principal (main.py ou app.py)
			Inicializar aplicação
			Registrar rotas
			Configurar banco de dados
FASE 4: TESTES
	4.1 Criar tests/test_person_controller.py
			Testes de todos os endpoints
			Cobertura 100%	
	4.2 Criar tests/test_person_service.py
			Testes de lógica de negócio
			Cobertura 100%	
	4.3 Configurar pytest.ini
			Adicionar configuração de cobertura (coverage)
FASE 5: CONTAINERIZACAO
	5.1 Criar Dockerfile
			Imagem base: python:3.10-slim
			Copiar requirements.txt
			Instalar dependências
			Copiar código-fonte
			Expor porta 8000
			CMD para executar aplicação
	5.2 Criar docker-compose.yml
			Serviço app (aplicação Python)
			Serviço postgres (PostgreSQL)
				Volume para persistência de dados
				Variáveis de ambiente
				Health check
			Inicialização automática com db/init-db.sql
			Network compartilhada entre serviços
FASE 6: DOCUMENTACAO
	6.1
		Criar README.md
			Seção: Visão Geral
				Descrição do projeto
				Objetivo
			
			Seção: Pré-requisitos
				Python 3.10+
				Docker e Docker Compose
				PostgreSQL (se executar localmente sem Docker)
			
			Seção: Instalação
				Clone do repositório
				Criar virtualenv
				Instalar dependências (pip install -r requirements.txt)
			
			Seção: Configuração
				Variáveis de ambiente (.env)
				Credenciais de banco de dados
			
			Seção: Execução Local
				Passo a passo para rodar sem Docker
				Inicializar banco de dados manualmente
				Executar servidor Python
				Acessar Swagger/Docs
			
			Seção: Docker Compose
				Como iniciar: docker-compose up
				Explicar que PostgreSQL é inicializado automaticamente
				Explicar que aplicação se conecta ao PostgreSQL
				Como parar: docker-compose down
			
			Seção: Testes
				Como executar testes: pytest
				Verificar cobertura: pytest --cov
				Requisito: 100% de cobertura
			
			Seção: Estrutura de Banco de Dados
				Descrição de cada script em db/
				Ordem de execução
				Como modificar schema
			
			Seção: API Endpoints
				Tabela com métodos, rotas, parâmetros e retornos
				Exemplos de requisição/resposta
				Códigos de status HTTP

FASE 7: VALIDACAO FINAL
	7.1 Verificar se cobertura de testes é 100%	
	7.2 Verificar se Docker Compose sobe sem erros	
	7.3 Verificar se todos os endpoints funcionam	
	7.4 Verificar se README.md está completo e claro	
	7.5 Verificar se todos os scripts de banco de dados estão em db/
```
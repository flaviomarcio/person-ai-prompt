# Prompts para analise e documentação dos projetos

## Documentação de projetos java e node

Tem por objetivo documentar classes e metodos das aplicações, bem como aquivos de configuração gerando assim um *README.md* para o projeto trabalhado

>Nota: antes de executar a documentação execute a geração do ddl com o [Prompt](projetos-database.md).

```text
DEFINICOES INICIAIS
===================
	Objetivo
		Analisar código-fonte, documentar projeto e gerar README.md completo
		Incluir análise de qualidade, configurações e débitos técnicos
	
	Escopo
		Análise de código Java/Spring Boot
		Documentação de classes, métodos, controllers, DTOs
		Geração/atualização de README.md
		Análise de configurações e estrutura de banco de dados

FLUXO DE EXECUCAO
=================
FASE 1: ANALISE DE CODIGO
	1.1 Examinar todos os arquivos de código-fonte do projeto
	1.2 Identificar
		Controllers
		DTOs
		Models/Entities
		Services
		Repositories
		Utils/Helpers
		Adapters
		Enums	
	1.3 Mapear estrutura de pacotes e dependências
FASE 2: DOCUMENTACAO DE CODIGO-FONTE
	2.1
		Adicionar comentários em todas as classes
		Descrever responsabilidade da classe
		Informações sobre padrões utilizados	
	2.2
		Adicionar comentários em todos os métodos
		Descrever objetivo do método
		Documentar parâmetros
		Documentar retorno
		Alertas para métodos com comportamentos específicos	
	2.3
		Documentar adapters
		Descrever padrão de adaptação
		Relacionamento entre objetos adaptados	
	2.4
		Documentar controllers para Swagger
		Adicionar @ApiOperation
		Adicionar @ApiResponse (200, 400, 500, etc)
		Adicionar @ApiParam em cada parâmetro
		Descrever cada endpoint	
	2.5
		Documentar DTOs para Swagger
		Adicionar @ApiModel
		Adicionar @ApiModelProperty em cada atributo
		Descrever validações (constraints)	
	2.6
		Melhorar mensagens de erro para constraints
		Para @NotBlank: Campo não pode estar em branco
		Para @NotNull: Campo é obrigatório
		Para @Size: Campo deve conter entre X e Y caracteres
		Para @Max: Valor máximo permitido é X
		Para @Min: Valor mínimo permitido é X
		Para @NotEmpty: Campo não pode estar vazio
		Para @Pattern: Formato inválido. Esperado XXXX
		Para @Email: Email inválido
		Para @Valid: Validação de objeto aninhado falhou
		Usar mensagens customizadas em cada constraint
FASE 3: GERACAO DE README.md
	3.1 Criar ou sobrescrever README.md	
	3.2 Seção Visão Geral do Projeto
		Descrição breve do projeto
		Objetivo principal
		Principais classes (listar 3-5)
		Enums utilizados (listar com descrição)	
	3.3 Seção Detalhes de Classes
		Para cada classe principal
			- Nome e pacote
			- Responsabilidade
			- Relacionamentos
			- Métodos públicos principais
			- Exemplo de uso (se aplicável)	
	3.4 Seção Débitos Técnicos
		Listar todos os débitos identificados
		Para cada débito
			- Descrição do problema
			- Impacto na aplicação
			- Recomendação de implementação
			- Estimativa de esforço (se possível)
			- Prioridade (Alta/Média/Baixa)
FASE 4: CONFIGURACOES DO PROJETO
	4.1 Verificar existência de arquivos de configuração
		- application.yml / application.properties
		- nginx.conf
		- vite.config.js
		- Dockerfile
		- docker-compose.yml	
	4.2 Documentar application.yml
		Seção Spring Cloud Gateway (se existir)
			Rotas configuradas
			Filtros aplicados
			Balanceamento de carga
			Timeouts e limites de conexão
			Exemplo de configuração		
		Seção Spring Cloud Vault (se existir)
			Conexão com Vault
			Caminhos de secrets
			Rotação de credenciais
			Fallback behavior
			Exemplo de uso		
		Seção SpringDoc OpenAPI (spring.doc) (se existir)
			URL do Swagger UI
			Caminho do JSON de schemas
			Configurações de segurança (autenticação)
			Versão da API
			Exemplo de acesso		
		Seção Configurações Customizadas (app.config)
			Listar todas as propriedades customizadas
			Descrever cada uma
			Valores padrão
			Quando modificar
			Exemplo de uso	
	4.3 Documentar nginx.conf (se existir)
		Seção Configuração de Proxy Reverso
			Servidores upstream
			Balanceamento de carga
			Cache
			Compressão
			Headers customizados
			Rewrite rules
			Exemplo de requisição	
	4.4 Documentar vite.config.js (se existir)
		Seção Build Frontend
			Entry points
			Output paths
			Plugins utilizados
			Alias de importação
			Variáveis de ambiente
			Configuração de dev server
			Exemplo de build
FASE 5: ESTRUTURA DE BANCO DE DADOS
	5.1 Verificar existência de pastas db/ ou ddl/	
	5.2 Se existirem scripts de banco de dados		
			Seção Ordem de Execução
				Listar ordem correta
				1. clear.sql (AVISO: somente DEV/SIT)
				2. drops.sql (AVISO: somente DEV/SIT)
				3. schemas.sql
				4. tables.sql
				5. constraints-pk.sql
				6. constraints-fk.sql
				7. constraints-check.sql
				8. indexes.sql
				9. init-data.sql
				10. fake-data.sql		
			Seção Descrição de Scripts
				clear.sql
					Descrição: Limpa dados sem remover objetos
					Ambiente: SIT/DEV apenas
					Aviso: Cuidado ao usar em produção
					Impacto: Remove todos os registros		
				drops.sql
					Descrição: Remove objetos do banco (tabelas, índices, etc)
					Ambiente: SIT/DEV apenas
					Aviso: Cuidado ao usar em produção
					Impacto: Estrutura do banco é removida		
				schemas.sql
					Descrição: Cria schemas/namespaces
					Ambiente: Todos
					Impacto: Agrupa tabelas logicamente		
				tables.sql
					Descrição: Cria todas as tabelas
					Ambiente: Todos
					Tabelas criadas: (listar todas)				
				constraints-pk.sql
					Descrição: Define chaves primárias
					Ambiente: Todos
					Impacto: Identifica registros únicos				
				constraints-fk.sql
					Descrição: Define chaves estrangeiras
					Ambiente: Todos
					Impacto: Relacionamento entre tabelas				
				constraints-check.sql
					Descrição: Define constraints de validação
					Ambiente: Todos
					Regras: (descrever constraints)				
				indexes.sql
					Descrição: Cria índices para performance
					Ambiente: Todos
					Campos indexados: (listar principais)				
				init-data.sql
					Descrição: Dados iniciais obrigatórios
					Ambiente: Todos
					Dados: (listar o que é inserido)				
				fake-data.sql
					Descrição: Dados de teste/desenvolvimento
					Ambiente: SIT/DEV apenas
					Quantidade de registros: (especificar)		
		Seção Aviso de Segurança
			AVISO CRÍTICO: drops.sql
				Será removida toda a estrutura do banco de dados
				Use apenas em desenvolvimento ou SIT com conhecimento completo
				Irreversível sem backup		
			AVISO CRÍTICO: clear.sql
				Será removido todo conteúdo do banco
				Use apenas em desenvolvimento ou SIT
				Considere backup antes de executar
FASE 6: SCRIPTS E AUTOMACAO
	6.1 Verificar existência de pasta script/ ou scripts/	
	6.2 Se existirem scripts shell/bash		
		Para cada script
			- Nome do script
			- Descrição
			- Parâmetros de entrada (se houver)
			- O que faz
			- Como executar
			- Exemplo de uso
			- Ambientes (DEV/SIT/PROD)
			- Pré-requisitos
FASE 7: CONTAINERIZACAO
	7.1 Verificar existência de Dockerfile	
	7.2 Se existir Dockerfile
			Seção Docker - Build
				Imagem base
				Dependências instaladas
				Porta exposta
				Variáveis de ambiente
				Volumes (se houver)
				Healthcheck
				Como fazer build: docker build -t ...
				Exemplo de execução	
	7.3 Verificar existência de docker-compose.yml	
	7.4 Se existir docker-compose.yml
			Seção Docker Compose
				Serviços definidos (app, banco, cache, etc)
				Volumes e suas finalidades
				Redes
				Variáveis de ambiente
				Ports mappings
				Health checks
				Como iniciar: docker-compose up
				Como parar: docker-compose down
				Como reconstruir: docker-compose up --build
				Exemplo de ambiente local
				Como acessar cada serviço
FASE 8: QUALIDADE DE CODIGO
	8.1 Seção Padrões de Código		
			Requisito: Cobertura de Testes Services
				Descrição: 100% de cobertura obrigatória
				Por quê: Services contêm lógica de negócio crítica
				Exemplo: Apresentar teste unitário com @Test e asserções		
			Requisito: Cobertura de Testes Utils
				Descrição: 100% de cobertura obrigatória
				Por quê: Utils são reutilizadas em vários pontos
				Exemplo: Apresentar teste para função utilitária		
			Requisito: Sem Lógica em DTOs
				Descrição: DTOs devem conter apenas dados
				Violação: Métodos com regras de negócio em DTO
				Correto: Apenas atributos e getters/setters
				Incorreto: Métodos que fazem cálculos
				Exemplo: Apresentar DTO correto e incorreto		
			Requisito: Sem Lógica em Models
				Descrição: Models devem mapear banco de dados
				Regra: Lógica vai em Services
				Correto: Apenas mapeamento com @Entity @Column
				Incorreto: Métodos de negócio na entity
				Exemplo: Apresentar Model correto e incorreto		
			Requisito: Aplicação de SOLID		
				S - Single Responsibility Principle
					Cada classe tem uma única responsabilidade
					Exemplo: CalculoDescontoService responsável apenas por cálculos
					Não: Persistir dados, enviar emails
					Exemplo de classe com boa prática		
				O - Open/Closed Principle
					Aberto para extensão, fechado para modificação
					Exemplo: Interface DescontoStrategy com implementações
					Demonstrar uso de herança e polimorfismo
					Exemplo de código		
				L - Liskov Substitution Principle
					Subclasses podem substituir superclasses sem quebrar código
					Exemplo: Todos os repositórios estendem JpaRepository
					Exemplo de implementação correta			
				I - Interface Segregation Principle
					Múltiplas interfaces específicas, não uma genérica
					Exemplo: Repository específico por entidade
					Evitar: Interface gigante com muitos métodos
					Exemplo de código correto			
				D - Dependency Inversion Principle
					Depender de abstrações, não de implementações
					Exemplo: Injetar repositório por interface
					Usar @Autowired com tipo de interface
					Exemplo de classe com DI correto		
			Requisito: Queries Declarativas em Repositórios
				Descrição: Usar Spring Data Query Methods
				Evitar: Escrever JPQL/SQL explícito
				Correto: findByNomeContainingIgnoreCase, findByEmail, findByAtivoTrue
				Incorreto: @Query com JPQL manualmente escrito
				Exemplo: Repositório com query methods
				Exemplo: Repositório com @Query (evitar)
				Vantagens de usar query methods: type-safe, refactoring automático, performance
FASE 9: GLOSSARIO DE SIGLAS
	9.1 Identificar todas as siglas utilizadas no projeto	
	9.2 Para cada sigla incluir
		Sigla
		Nome completo
		Descrição breve
		Link oficial de referência	
	9.3 Exemplos de siglas comuns
		DTO - Data Transfer Object
		Objeto para transferência de dados entre camadas
		https://www.martinfowler.com/bliki/DataTransferObject.html

		SOLID - S.O.L.I.D. Principles
		Princípios de design de software orientado a objetos
		https://en.wikipedia.org/wiki/SOLID
		
		REST - Representational State Transfer
		Arquitetura para APIs web baseada em HTTP
		https://en.wikipedia.org/wiki/Representational_state_transfer
		
		JWT - JSON Web Token
		Padrão de autenticação e autorização
		https://jwt.io/
		
		CORS - Cross-Origin Resource Sharing
		Mecanismo de segurança web para requisições cross-origin
		https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
		
		AOP - Aspect-Oriented Programming
		Paradigma de programação para separação de concerns
		https://en.wikipedia.org/wiki/Aspect-oriented_programming
		
		ORM - Object-Relational Mapping
		Técnica de mapeamento entre objetos e banco de dados relacional
		https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping
		
		JPA - Java Persistence API
		Especificação Java para persistência de dados
		https://jakarta.ee/specifications/persistence/
		
		SQL - Structured Query Language
		Linguagem de consulta para bancos de dados
		https://en.wikipedia.org/wiki/SQL
		
		YAML - YAML Ain't Markup Language
		Formato de serialização de dados legível
		https://yaml.org/
		
		CI/CD - Continuous Integration / Continuous Deployment
		Práticas de automação em pipeline de desenvolvimento
		https://en.wikipedia.org/wiki/CI/CD
		
		TDD - Test-Driven Development
		Prática de escrever testes antes do código
		https://en.wikipedia.org/wiki/Test-driven_development
		
		POJO - Plain Old Java Object
		Classe Java simples sem dependências de frameworks
		https://en.wikipedia.org/wiki/Plain_old_Java_object
		
		MVC - Model View Controller
		Padrão de arquitetura para separação de concerns
		https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
		
		API - Application Programming Interface
		Interface para comunicação entre sistemas
		https://en.wikipedia.org/wiki/API
		
		JSON - JavaScript Object Notation
		Formato de texto para troca de dados
		https://www.json.org/
```

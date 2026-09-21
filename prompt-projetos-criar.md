# Prompts para criação de projetos python

## Api simples usando MVC

Tem por objetivo criar diversos tipo de projetos

## Prompt para Api simples
Este vai criar uma api usando java Spring

```
DEFINICOES INICIAIS
===================
	Objetivo
		Gerar no diretorio corrente um projeto Java Spring Boot do tipo API BFF / API Gateway
		Manter integralmente a infraestrutura de seguranca e gateway
		Simplificar regras de negocio para um service simbolico com metodo simples
	Escopo
		Java, utilizando sempre o ultimo LTS
		Spring Boot, Spring Cloud, Maven, WebFlux, utilizar versões compativeis entre ambos
		Spring Cloud Gateway com filtros de seguranca e ContextHolder ThreadLocal, utilizar versões compativeis entre ambos
		modelmapper, sempre a mais recente desde que compativel com as versões spring selecionadas
		jackson-base (pom), jackson-core, jackson-annotations, jackson-databind, sempre a mais recente desde que compativel com as 
		drivers de banco de dados sempre os mais atualizados
		versões spring selecionadas
		Service de negocio simbolico com DTOs de request e response
		Testes unitarios, README e arquivos auxiliares
FLUXO DE EXECUCAO
=================
FASE 1: STACK E BUILD
	1.1 Java 21, Maven, parent spring-boot-starter-parent
	1.2 groupId com.app, artifactId app, version 0.0.1-SNAPSHOT, mainClass com.app.Application, layout JAR no spring-boot-maven-plugin
	1.3 dependencyManagement importando spring-cloud-dependencies 2025.0.0 (tipo pom, scope import)
	1.4
		Dependencias obrigatorias
		com.littlecode:core-spring e com.littlecode:business-spring versao
		spring-boot-starter-webflux
		spring-cloud-starter-gateway-server-webflux
		spring-boot-starter-data-jpa
		spring-boot-starter-jdbc
		spring-boot-starter-validation
		spring-boot-starter-actuator
		spring-cloud-starter-vault-config
		lombok
		modelmapper
		jackson-base (pom), jackson-core, jackson-annotations e jackson-databind
		h2
		spring-boot-starter-test (test)
	1.5 Declarar versoes em properties do pom: java.version, littlecode-version, jackson-version, modelmapper-version, spring-boot-version, spring-cloud-version, h2-version
	1.6 Nao adicionar dependencias externas alem das listadas; usar apenas classes nativas do Spring
FASE 2: CLASSE PRINCIPAL
	2.1 com.app.Application anotada com @SpringBootApplication(exclude = RabbitAutoConfiguration.class)
	2.2 Adicionar @EnableJpaAuditing e @EnableJpaRepositories
FASE 3: INFRAESTRUTURA DE SEGURANCA (pacote com.app.security, manter regras completas)
	3.1
		config/SecurityConfig
		@Configuration com @Getter e @Setter, injeta SecurityPaths
		Campos via @Value: app.auth.scope-id (UUID scopeId), app.auth.uri (authUrl), app.auth.api-key (authApiKey), spring.webflux.base-path default "/" (contextPath)
	3.2
		config/SecurityPaths
		@Configuration com @ConfigurationProperties(prefix = "app.security.paths")
		Listas blocked, backend e backendOpen
	3.3
		ContextHolder
		Classe utilitaria estatica com ThreadLocal<Map<String,String>>
		Metodos clear, get/set para scopeId (UUID), userId (UUID), sessionAuthorization (String) e sessionProfile (SecurityUserProfile serializado com ObjectUtil do littlecode)
		Setters lancam IllegalArgumentException para null ou vazio
	3.4
		dto/SecurityUserProfile
		Lombok @Getter @Setter @Builder @NoArgsConstructor @AllArgsConstructor
		Campos id (UUID), name, username, document, dtBirth (LocalDate), email, phoneNumber, validated (Boolean)
	3.5
		service/SecurityCredentialService
		@Service que recebe SecurityConfig e RestTemplate
		Metodo findProfile(String authorization) faz GET em {authUrl}/v1/auth/session/profile com headers x-scope-id, Content-Type application/json e Authorization Bearer
		Retorna SecurityUserProfile em 2xx, null caso contrario
		Loga o comando curl equivalente em debug e erro
	3.6
		service/SecurityService
		@Service que recebe SecurityConfig e SecurityCredentialService
		Expoe getScopeId, extractAccessToken(ServerWebExchange) removendo prefixo "Bearer " e authenticate(ServerWebExchange)
		authenticate busca profile, popula ContextHolder (userId, sessionAuthorization, sessionProfile) e mantem statusCode (HttpStatus) e message da ultima operacao
		Retorna false com UNAUTHORIZED quando profile for null
	3.7
		util/SecurityUrlHelper
		@Service que processa SecurityPaths (distinct e trim)
		Expoe isBlockedPath, isBackendPath e isBackendOpenPath com matching case-insensitive
		Suporta sufixo "**" e "/**" como matching de prefixo
	3.8
		inteceptor/WebInterceptorFilter
		@Component implements WebFilter
		Seta scopeId no ContextHolder
		Path bloqueado retorna 404
		Path backend e nao backendOpen exige authenticate; em falha responde o statusCode do SecurityService
		Limpa ContextHolder em doFinally
	3.9
		inteceptor/WebGatewayFilter
		@Component implements GlobalFilter e Ordered (order -1)
		Remove headers x-scope-id e x-user-id do request e reinsere a partir do ContextHolder quando autenticado
		Loga em debug DNS de origem (Host) e destino (route uri) com o path
FASE 4: REGRAS DE NEGOCIO SIMPLIFICADAS (pacote com.app.business)
	4.1 Nao implementar regras de negocio reais; criar apenas um service simbolico
	4.2
		service/SampleService
		@Service com um unico metodo simples: SampleResponse execute(SampleRequest request)
		Retorna objeto montado a partir do request (echo com id UUID gerado e timestamp)
	4.3
		dto/SampleRequest e dto/SampleResponse
		Lombok @Getter @Setter @Builder @NoArgsConstructor @AllArgsConstructor
		Request com validacao jakarta (@NotBlank) nos campos obrigatorios
	4.4
		controller/SampleController
		@RestController em /api/v1/sample
		POST recebe @Valid @RequestBody SampleRequest e retorna Mono<SampleResponse> via SampleService
	4.5 Manter apenas models e DTOs associados aos services resultantes: SecurityUserProfile, SampleRequest e SampleResponse
	4.6 Nao criar entidades JPA nem repositories
FASE 5: CONFIGURACOES (pacote com.app.business.config)
	5.1 Manter apenas @Configuration com beans usados pelos services resultantes
	5.2 AppBeans: @Configuration com @Bean RestTemplate criado via BeanUtilFactory.createRestTemplate() do littlecode (usado por SecurityCredentialService)
	5.3 GatewayNettyConfig: @Configuration com @Bean HttpClientCustomizer aplicando DefaultAddressResolverGroup.INSTANCE (correcao de resolucao DNS apos deploy)
	5.4 Nao criar classes @Configuration sem beans ou sem vinculo com os services resultantes
FASE 6: APPLICATION.YML
	6.1 server.port 8080; logging root, com.app e org.springframework.cloud.gateway em INFO; logging.file.name /tmp/app.log
	6.2 spring.application.name app; spring.webflux.base-path /; spring.main.allow-bean-definition-overriding true
	6.3 jackson: time-zone ${TZ:America/Sao_Paulo}, fail-on-empty-beans false, fail-on-unknown-properties true, date-format yyyy-MM-dd HH:mm:ss
	6.4 jpa hibernate ddl-auto none, show_sql false, open-in-view false; datasource jdbc:h2:mem:testdb
	6.5
		spring.config.import ${vault_configimport:}
		Bloco spring.cloud.vault totalmente parametrizado por variaveis de ambiente
		Variaveis: vault_schema, vault_host, vault_port, vault_authentication, vault_app_role_role_id, vault_app_role_secret_id, vault_token, vault_enabled, vault_timeout, vault_fastfail, vault_kv_backend, vault_kv_profile_separator, vault_kv_default_context
		enabled default false
	6.6
		spring.cloud.gateway.server.webflux
		httpclient.pool.type DISABLED
		default-filters Retry: retries 3, statuses BAD_GATEWAY,GATEWAY_TIMEOUT,SERVICE_UNAVAILABLE, methods GET, backoff firstBackoff 50ms, maxBackoff 500ms, factor 2
		routes: users (Path=/api/v1/users/** -> ${app.gateway.users.uri}), App-Backend (Path=/v1/backend/** -> ${app.gateway.mcs-app-backend-api.uri}), ui (Path=/** -> ${app.gateway.ui.uri})
	6.7
		routes: users (Path=/api/v1/users/** -> ${app.gateway.users.uri}), App-Backend (Path=/v1/backend/** -> ${app.gateway.mcs-app-backend-api.uri}), ui (Path=/** -> ${app.gateway.ui.uri})
	6.7
		app.auth: scope-id, uri, api-key
		app.gateway: ui.uri, users.uri, mcs-app-backend-api.uri
		app.security.paths.backend com /api/**
		Deixar comentados exemplos de backend-open e blocked
FASE 7: TESTES
	7.1 JUnit 5 e Mockito sem contexto SpringBootTest; @DisplayName em portugues; nomes no padrao deveValidarMetodo_xxx
	7.2 Testes unitarios para ContextHolder, SecurityConfig, SecurityCredentialService (RestTemplate mockado), SecurityService, SecurityUrlHelper, WebInterceptorFilter, WebGatewayFilter, AppBeans, GatewayNettyConfig, SampleService e SampleController
	7.3 Teste de getters e setters dos DTOs
	7.4 Teste de contexto da Application
FASE 8: DOCUMENTACAO E ARQUIVOS AUXILIARES
	8.1 README.md com secoes: Descricao, ADR(s) relacionada, Requirements, Dados sensiveis, Funcionalidades, Referencia do PO, Referencias tecnica, Referencias adicionais, Envolvidos, SRE/DEVOPS (Configuration, Database, Queue, S3, Observacoes), Arquitetura do micro-service, Notas diversas
	8.2 LICENSE e .gitignore padrao Java, Maven e IntelliJ
	8.3 Diretorio docs/src reservado para diagrama C4 (c4.drawio)
FASE 9: GIT
	9.1 Configurando .gitignore:
		- Criar .gitignore compativel com a stack principal
		- Incluir vscode, idea, claude, codex
FASE 10: VALIDACOES FINAIS
	10.1 Todo o codigo em Java com Lombok (@Slf4j, @Getter, @Setter, @RequiredArgsConstructor)
	10.2 Comentarios e Javadoc em portugues
	10.3 Programacao reativa (Mono) nos filtros e controllers; nao usar spring-boot-starter-web
	10.4 Executar mvn clean verify e garantir build verde
```


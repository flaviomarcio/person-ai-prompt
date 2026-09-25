# Definições para codificação da usando A.I para projetos Java.

## Arch type
    - pom.xml
        - Sempre utilizar framework spring e spring cloud
        - Incluir Actuator
        - Sempre deve ter suporte a Api sem webflux, webflux apenas se o programador solicitar
        - Atualmente utilizar spring 3.5.8 e spring cloud 2025.0.3

## Configurações
- Banco de dados
    - Se necessario acessar mais de um banco de dados considerar criar uma arquivo datasource para cada banco de dados, ex: 
        - Se os bancos de dados 
            - Oracle
            - SQLServer
            - Postgres
        - então usar 
            - DsOracle.java
            - DsSQLServer.java
            - DsPostgres.java
## MVC
- Model e DTO
    - Sempre declarar o **@Builder**
    - Não declarar valores default nos atributos
    - Nunca declare construtores com parametros, usar **@AllArgsConstructor**, **@NoArgsConstructor**
    **@Builder**
    - Não deve ter codigo metodo de qualquer natureza
    - Devem ter apenas os atributos
    - Nunca mapear classes, Ex: usando **@Join**, **@OneToOne**, **@ManyToOne**, etc.
    - Não devem utilizar @Data, usar sempre **@Getter** e **@Setter**
    Tipos:
    - Em suas declarações nunca usar tipos primitivos, ex: em vez de:  
        - **int** por **Integer**
        - **short** por **Integer**
        - **long** por **Long**
        - **double** por **Double**, **BigDecimal**
        - **float** por **Double**, **BigDecimal**
- Model
    - Não devem utilizar as anotações **@GeneratedValue**, **@SequenceGenerator**
    - Todo valor de um model deve ser informado na aplicação
    - Considerar que no banco de dados não existe trigger, sequences, auto incrementais e ou valores default
    - Bancos de dados e Models:
        - Tratamento para tipos não suportados no bancos de dados oracle:
            - **UUID**
                ```java
                //PK
                @JdbcTypeCode(SqlTypes.CHAR)
                @Convert(converter = UUIDConverter.class)
                @Id
                @Column(nullable = false, updatable = false)
                private UUID id;

                //ou

                @Convert(converter = UUIDConverter.class)
                @Column(nullable = false)
                private UUID scopeId;
                ```
            - **Boolean** use **BooleanToString.java**
                ```java
                @Convert(converter = BooleanToString.class)
                @Column(nullable = false)
                private Boolean enabled;
                ```
- Respository:
    - Sempre devem ser interfaces herdando de JpaRepository e anotadas com @Repository
    - Nunca Utilizar JdbcTemplate e sempre utilizar o spring data
    - Nunca deve ter metodos implementados
    - Nunca usar esquema em constantes
    - Sempre usar @Query para consultas ou execuções de procedures
        - Nunca concatenar String
        - Sempre utilizar query completas dentro de @Query
    - As respostas de funções sempre devem ser classes anotadas com @Table e @Entity

- Mappers
    - Anota com @UtilyClass
    - Nunca use construtores
    - Agrupar dominios em Mappers por familia de Venda, Produtos, etc..
    - Sempre devem ser classes com metodos estaticas
    - Nunca usar estratégias com anotações para orientar a copia dos dados
    - Na criação do objeto sempre use builders
    - Nas funções do Mapper sempre receba um Model/Entity nunca um ResultSet ou parametros
    - Nunca usar "try cache" para tratar erros

- Exceptions
    - Não criar exceções, se ainda for necessário herdar a classe de exceção de ResponseStatusExcption

- Service
    - Nunca manipular threads dentro dos serviços, considerar que o serviço deve ser utilizado por uma classe Controller como Consumer, Schedule e ou Api.
    - Devem sempre ser anotadas com @Service e @RequiredArgsConstructor
    - Falando de Api, nunca devem retornar @ResponseEntity ou similares
- Adapter
    - Api, Consumer, Schedule
        - Ambos podem manipular threads
    - Api
        - Nunca tratar negocio, apenas retornar a resposta origunda 
        - Response deve serpre ser ResponseEntity<DTO> ou ResponseEntity<Tipo-Primitivo>
        - Sempre deve retornar ResponseEntity<DTO>, nunca adaptar a resposta com WebFlux, exemplo usando Response com Mono<*>
    - Consumer
    - Schedule
- Tests
    - Sempre usar Mockito
    - Sempre incluir @ExtendWith(MockitoExtension.class)
    - Evitar uso de @Mock e @InjectMocks, Se ainda for necessário usar @Mock e @InjectMocks considerar uso do @BeforeEach para iniciar pelos construtores as classes sendo testadas.
    - Para instanciar classes se possivel sempre use o Builder declarado
    - Nunca usar "try cache" para tratar erros
    - Cobertura
        - Model/DTO não deve ser construidos testes
        - Enum devem ter 100% de cobertura
        - Configurações, Services, Mappers, Factories e Utils devem ter 100% de cobertura

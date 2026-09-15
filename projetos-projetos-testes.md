# Prompts para criação de testes para classes

## Teste para serviços e casos de uso
Tem por objetivo criar testes para classes e casos de uso das aplicações

### Java

- Para o projeto
    ```text
    DEFINICOES INICIAIS
    ===================
        Objetivo
            Criar testes para todo o projeto
            Utilizar padrão Mockito sem contexto SpringBootTest
        Escopo
            Testes unitários com Mockito
            Cobertura de services, utils, adapters, controllers, consumers
            Validação de resultados e uso de repositories
    FLUXO DE EXECUCAO
    =================
    FASE 1: ANALISE DO PROJETO
        1.1 Analisar e usar padrão de testes existentes
        1.2 Identificar padrões utilizados
        1.3 Mapear estrutura de testes atual

    FASE 2: CONFIGURACAO BASICA DOS TESTES
        2.1 Não utilizar contexto do SpringBootTest
        2.2 Usar Mockito para mocks e validações
        2.3
            Sempre utilizar nome das classes para acessar métodos envolvidos
            Ao usar any() sempre usar Mockito.any()
            Ao usar assertNotNull() sempre usar Assertions.assertNotNull()
    FASE 3: PADRONIZACAO DOS TESTES
        3.1 Sem indicar @DisplayName o que vai ser testado        
        3.2 Sempre validar resultado se houver resultado        
        3.3 Sempre validar uso de classes como repositories        
        3.4 Usar padrão Arrange, Act, Assert em todos os testes
    FASE 4: TESTES PARA SERVICES
        4.1 Criar testes para lógica de negócio        
        4.2 Mockar repositories        
        4.3 Validar retornos esperados        
        4.4 Validar chamadas aos repositories com Mockito.verify
    FASE 5: TESTES PARA UTILS
        5.1 Criar testes para funções utilitárias
        5.2 Validar todos os cenários possíveis
        5.3 100% de cobertura obrigatória
    FASE 6: TESTES PARA ADAPTERS
        6.1
            Controllers e Consumers
            Fazer apenas cobertura de testes
            Não há necessidade de emular funcionamento do adapter
            Ex: não emular chamadas a API ou tópicos
        6.2 Validar que métodos são chamados corretamente        
        6.3 Mockar dependências externas
    FASE 7: VALIDACOES FINAIS
        7.1 Não tentar entender libs externas
        7.2 Focar em testar código próprio do projeto        
        7.3 Usar Mockito.verify para validar chamadas        
        7.4 Validar comportamento esperado em cada cenário
    ```

- Para algumas classes
    ```text
    DEFINICOES INICIAIS
    ===================
        Objetivo
            Criar testes para classes específicas
            Utilizar padrão Mockito sem contexto SpringBootTest
        Classes Alvo
            ClasseNomeA
            ClasseNomeB        
        Escopo
            Testes unitários com Mockito
            Validação de resultados e uso de repositories
    FLUXO DE EXECUCAO
    =================
    FASE 1: ANALISE DO PROJETO
        1.1 Analisar padrão de testes existentes        
        1.2 Identificar repositories e dependências de ClasseNomeA        
        1.3 Identificar repositories e dependências de ClasseNomeB        
        1.4 Mapear estrutura de testes atual
    FASE 2: CONFIGURACAO BASICA DOS TESTES
        2.1 Não utilizar contexto do SpringBootTest        
        2.2 Usar Mockito para mocks e validações        
        2.3
            Sempre utilizar nome das classes para acessar métodos envolvidos
            Ao usar any() sempre usar Mockito.any()
            Ao usar assertNotNull() sempre usar Assertions.assertNotNull()
    FASE 3: PADRONIZACAO DOS TESTES
        3.1 Sem indicar @DisplayName o que vai ser testado        
        3.2 Sempre validar resultado se houver resultado        
        3.3 Sempre validar uso de classes como repositories        
        3.4 Usar padrão Arrange, Act, Assert em todos os testes
    FASE 4: TESTES PARA ClasseNomeA
        4.1 Criar testes para lógica de negócio        
        4.2 Mockar repositories utilizados        
        4.3 Validar retornos esperados        
        4.4 Validar chamadas aos repositories com Mockito.verify        
        4.5 Testar exceções e casos de erro        
        4.6 Validar comportamento em diferentes cenários
    FASE 5: TESTES PARA ClasseNomeB
        5.1 Criar testes para lógica de negócio        
        5.2 Mockar repositories utilizados        
        5.3 Validar retornos esperados        
        5.4 Validar chamadas aos repositories com Mockito.verify        
        5.5 Testar exceções e casos de erro        
        5.6 Validar comportamento em diferentes cenários
    FASE 6: TESTES PARA ADAPTERS
        6.1
            Se ClasseNomeA ou ClasseNomeB forem controllers ou consumers
            Fazer apenas cobertura de testes
            Não há necessidade de emular funcionamento do adapter
            Ex: não emular chamadas a API ou tópicos
        6.2 Validar que métodos são chamados corretamente
        6.3 Mockar dependências externas
    FASE 7: VALIDACOES FINAIS
        7.1 Não tentar entender libs externas
        7.2 Focar em testar código próprio das classes        
        7.3 Usar Mockito.verify para validar chamadas
        7.4 Validar comportamento esperado em cada cenário
        7.5 Garantir cobertura completa de ambas as classes
    ```
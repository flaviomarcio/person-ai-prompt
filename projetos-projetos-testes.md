# Prompts para criação de testes para classes

## Teste para serviços e casos de uso
Tem por objetivo criar testes para classes e casos de uso das aplicações

### Java

Para o projeto
```text
1. Crie os testes para todo o projeto
2. Considere analisar e usar o padrão de testes existentes
3. Crie um teste usando Mockito
4. Não deve utilizar contexto do SpringBootTest
5. Deve sempre utilizar o nome das classes para acessar os metodos envolvidos nos mocks e validações, ex:
    5.1 Ao usar any()  sempre usar Mockito.any()
    5.2 Ao usar assertNotNull(...)  sempre usar Assertions.assertNotNull(...)
6. Sem indicar o @DisplayName o que vai ser testado
7. Sempre validar o resultado se houver resultado
8. Sempre validar o uso de classes como repositories
9. Testes para adapters
    9.1 Controllers e Consumers
        9.1.1 Fazer apenas cobertura de testes
        9.1.2 Não há necessidade de emular o funcionamento do adapter, ex: api ou tópico
10. Não tentar entender libs externa.
```

Para algumas classes
```text
1. Crie os testes para as classes
    1.1 ClasseNomeA
    1.2 ClasseNomeB
2. Considere analisar e usar o padrão de testes existentes
3. Crie um teste usando Mockito
4. Não deve utilizar contexto do SpringBootTest
5. Deve sempre utilizar o nome das classes para acessar os metodos envolvidos nos mocks e validações, ex:
    5.1 Ao usar any()  sempre usar Mockito.any()
    5.2 Ao usar assertNotNull(...)  sempre usar Assertions.assertNotNull(...)
6. Sem indicar o @DisplayName o que vai ser testado
7. Sempre validar o resultado se houver resultado
8. Sempre validar o uso de classes como repositories
9. Testes para adapters
    9.1 Controllers e Consumers
        9.1.1 Fazer apenas cobertura de testes
        9.1.2 Não há necessidade de emular o funcionamento do adapter, ex: api ou tópico
10. Não tentar entender libs externa.
```
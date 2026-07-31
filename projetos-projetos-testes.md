# Prompts para criação de testes para classes

## Teste para serviços e casos de uso
Tem por objetivo criar testes para classes e casos de uso das aplicações

### Java

```text
1. Crie os testes para este projeto, considere analisar e usar o padrão de testes existentes
2. Crie um teste usando Mockito
3. Não deve utilizar contexto do SpringBootTest
4. Deve sempre utilizar o nome das classes para acessar os metodos envolvidos nos mocks e validações, ex:
    4.1 Ao usar any()  sempre usar Mockito.any()
    4.2 Ao usar assertNotNull(...)  sempre usar Assertions.assertNotNull(...)
5. Sem indicar o @DisplayName o que vai ser testado
6. Sempre validar o resultado se houver resultado
7. Sempre validar o uso de classes como repositories
8. Testes para adapters
    8.1 Controllers
        8.1.1 Fazer apenas cobertura de testes nos controllers 
        8.1.2 Não há necessidade de levantar uma api funcional bem como fazer requests para testes
```
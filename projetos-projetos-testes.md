# Prompts para criação de testes para classes

## Teste para serviços e casos de uso
Tem por objetivo criar testes para classes e casos de uso das aplicações

### Java

```text
1. Crie um teste usando Mockito
2. Não deve utilizar contexto do SpringBootTest
3. Deve sempre utilizar o nome das classes para acessar os metodos envolvidos nos mocks e validações, ex:
    3.1 Ao usar any()  sempre usar Mockito.any()
    3.2 Ao usar ()  sempre usar Mockito.any()
4. Sem indicar o @DisplayName o que vai ser testado
5. Sempre validar o resultado se houver resultado
6. Sempre validar o uso de classes como repositories
```
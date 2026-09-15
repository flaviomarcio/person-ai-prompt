# Prompts para criação de prompts

Tem por objetivo orientar a criar novos prompts

## Criando um prompt do zero
Estrutura Universal para Montar Prompts de Alta Qualidade

- Os 5 Blocos Essenciais

    CONTEXTO → OBJETIVO → REQUISITOS → FORMATO → VALIDAÇÃO
    | Bloco | Responde | Exemplo |
    |-------|----------|---------|
    | Contexto | Por quê? | Papel, limitações (TDA), ambiente, restrições |
    | Objetivo | O quê? | Verbo claro + artefato + escopo exato |
    | Requisitos | Como? | Funcionais, não-funcionais, validações, exclusões |
    | Formato | Aonde? | Stack, estrutura de dirs, exemplo de output |
    | Validação | Pronto? | Checklist de aceição + critérios mensuráveis |

- Regras de Ouro
    1. **Sem ambiguidade** — Cada item deve ser testável
    2. **Com exemplos** — Response JSON exato, comando bash completo
    3. **Checklist claro** — `[ ] Item testável`
    4. **Separado em seções** — Nunca misturar Requisitos com Formato

- Template Rápido
    `````text
    PROMPT: [NOME]

    CONTEXTO:
    =========
    Papel: 
    Limitações: 
    Ambiente: 

    OBJETIVO:
    =========
    Entregar: 

    REQUISITOS:
    ===========
    Funcionais:
    1. 

    Não-Funcionais:
    1. 

    Validações:
    1. 

    Exclusões:
    - 

    FORMATO:
    ========
    Stack: 
    Estrutura: 
    Exemplo: 

    VALIDAÇÃO:
    ==========
    [ ] 
    [ ] 
    `````

## Baseado em um projeto
Este fará a leitura de um projeto seja este qual for, identificará linguamge, frameworks, caracteristicas necessárias para recriar o projeto com os mesmos padrões.
- Java
    ```text
    DEFINICOES INICIAIS
    ===================
        Objetivo
            Criar prompt para gerar projeto similar ao repositório atual no diretório corrente
            Simplificar regras de negócio e classes
        Escopo
            Análise de projeto existente
            Geração de estrutura simplificada
            Criação de novo projeto com padrão semelhante

    FLUXO DE EXECUCAO
    =================
    FASE 1: ANALISE DO REPOSITORIO ATUAL
        1.1 Examinar estrutura do projeto atual
        1.2 Identificar padrões e convenções utilizadas
        1.3 Mapear classes @Service, @Configuration, Models e DTOs

    FASE 2: SIMPLIFICACAO DE REGRAS DE NEGOCIO
        2.1 Substituir regras de classes de negócio em classes anotadas com @Service
            Substituir por método simbólico de Service com método simples
            Remover lógica complexa e redundante
            Manter apenas comportamento essencial
        2.2 Manter regras de classes anotadas com @Service
            Desde que sejam para uso em infraestrutura
            Ex: spring security, swagger, etc
            Classes de configuração de framework
        2.3 Manter apenas classes anotadas com @Configuration
            Que tenham Beans relacionados às classes @Service resultantes
            Remover @Configuration não utilizadas
            Remover Beans órfãos
        2.4 Manter apenas models e DTOs
            Associados às classes @Service resultantes
            Remover entidades não utilizadas
            Remover DTOs órfãos
        2.5 API deve fazer uso de DTO
            DTO de request associados às classes @Service resultantes
            DTO de response associados às classes @Service resultantes
            Manter padrão de entrada e saída

    FASE 3: ESTRUTURACAO DO NOVO PROJETO
        3.1 Criar estrutura de diretórios similares ao projeto atual
        3.2 Aplicar padrões de nomenclatura do projeto original
        3.3 Gerar classes base simplificadas
        3.4 Criar estrutura de configuração

    FASE 4: GERACAO DE ARTEFATOS
        4.1 Gerar pom.xml ou requirements.txt com dependências simplificadas
        4.2 Gerar classes @Configuration necessárias
        4.3 Gerar classes @Service simplificadas
        4.4 Gerar Models e DTOs associados
        4.5 Gerar Controllers com endpoints baseados em Services

    FASE 5: DOCUMENTACAO
        5.1 Gerar README.md com instruções de uso
        5.2 Documentar padrões simplificados utilizados
        5.3 Incluir exemplos de como estender o projeto
    ```
# Prompts para analise e criação dos scripts de bancos de dados dos projetos

## Documentação de projetos java e node

Tem por objetivo analisar e gerar DDL dos models de um projetos.

```text
DEFINICOES INICIAIS
===================
	Objetivo
		Analisar código com foco nos models
		Criar scripts SQL separadamente para cada componente de banco de dados
	
	Escopo
		Análise de models para geração de DDL
		Criação de scripts para clear, drops, schemas, sequences, triggers, tables, constraints e indexes
		Suporte a diferentes bancos de dados com equivalentes de sintaxe

FLUXO DE EXECUCAO
=================
FASE 1: ANALISE DE MODELOS
	1.1 Examinar todos os models do projeto	
	1.2
		Identificar
		- Entidades e relacionamentos
		- Tipos de dados utilizados
		- Conversores customizados registrados
		- Configurações de generators e sequences
		- Validações e constraints nos fields
		- Campos UUID, boolean, String sem length definido
FASE 2: CRIAR ARQUIVO CLEAR
	2.1 Literalmente limpar truncate de cada objeto criado	
	2.2 Usar [if exists] desde que banco de dados tenha suporte	
	2.3 Preservar ordem de execução considerando FKs
FASE 3: CRIAR ARQUIVO DROPS.sql
	3.1 Literalmente drops de cada objeto criado
	3.2 Usar [if exists] ou equivalente desde que banco de dados tenha suporte	
	3.3 Usar equivalente a [select object_id('object_name')] em banco de dados ou comandos que não tenham suporte a [if exists]	
	3.4 Evitar erros onde objeto ou field já foi criado
FASE 4: CRIAR ARQUIVO SCHEMAS.sql
	4.1 Incluir apenas esquemas utilizados para conter objetos	
	4.2 Um schema por linha se múltiplos esquemas
FASE 5: CRIAR ARQUIVO SEQUENCES.sql
	5.1 Incluir sequences indicados no model	
	5.2 Somente se banco de dados tiver suporte a generator ou generators	
	5.3 Considerar configuração de suporte no projeto
FASE 6: CRIAR ARQUIVO TRIGGERS
	6.1 Incluir triggers necessários baseados nos models	
	6.2 Considerar suporte do banco de dados
FASE 7: CRIAR ARQUIVO TABLES.sql
	7.1
		Criar tabelas dos models
		Não deve incluir drop aqui
		Usar [if not exists] ou equivalente a [select object_id('object_name')]	
	7.2
		Estrutura das tabelas
		Tabelas sem definição de PK, FK, tablespace, triggers
		Gerar separadamente create table apenas com field que será PK
		Gerar separadamente com alter table para fields que não são PK	
	7.3
		Particularidades do Oracle	
	7.4
		Tratamento de tipos especiais
		Se field indicar tipo UUID e houver converter registrado ou configuração clara usar varchar(36)
		Se field indicar tipo boolean e houver converter registrado ou configuração clara usar tipo ou conversão indicada
		Se field boolean for necessário campo numérico considerar sempre integer ao invés de bit, smallint ou tipos equivalentes
		Se field for String e não tiver length definido considerar 255
FASE 8: CRIAR ARQUIVO CONSTRAINTS-PK.sql
	8.1 PK isoladas do create table
	8.2 Não deve incluir drop aqui	
	8.3 Usar [if not exists] ou equivalente a [select object_id('object_name')]	
	8.4
		Nomear constraints seguindo padrão
		pk__${table-name}_${fields-names}
FASE 9: CRIAR ARQUIVO CONSTRAINTS-FK.sql
	9.1 FK isoladas do create table	
	9.2 Não deve incluir drop aqui
	9.3 Usar [if not exists] ou equivalente a [select object_id('object_name')]	
	9.4
		Nomear constraints seguindo padrão
		fk__${table-name}_${fields-names}
FASE 10: CRIAR ARQUIVO CONSTRAINTS-CHECK.sql
	10.1 Constraints utilizadas nos fields dos models
	10.2 Validar dados como [1,2,3] ou ['A','B','C']	
	10.3 Check isoladas do create table	
	10.4 Não deve incluir drop aqui	
	10.5 Usar [if not exists] ou equivalente a [select object_id('object_name')]	
	10.6 Criar constraints para objeto cuja model tem configuração direta no field
	10.7
		Nomear constraints seguindo padrão
		ck__${table-name}_${fields-names}_${seq-table-idx}
		Seq-table-idx é incremental iniciando de 1 com zeros à esquerda, ex: 01, 002

FASE 11: CRIAR ARQUIVO INDEXES.sql
	11.1 Indices relacionados a FKs	
	11.2 Não deve incluir drop aqui	
	11.3 Se possível usar create or replace de forma concorrente no banco de dados	
	11.4 Usar [if not exists] ou equivalente a [select object_id('object_name')]	
	11.5 Criar indices para FK nos models	
	11.6
		Criar indices para fields claramente utilizados em consultas
		Analisar order consultada para sugestão de índice mais seletivo
		Ex: 2 funções uma consulta por [nome] outra por [data] e nome, considerar se melhor criar 1 índice para ambos ou apenas [nome] ou [data] ou dois índices	
	11.7
		Considerações de seletividade
		Se [data] for localdate é ótimo campo para índice
		Se [data] for timestamp já tem performance prejudicada, contudo se gravado com hora 00:00:00 volta a ser bom campo
		Se campo [date] for timestamp e filtro controller iniciar com date ou localdate considerar estratégia de ajuste do dado como cast ou converter desde banco tenha suporte
		Ex: Postgres cast de timestamp para date: create index un_table_name on table_name(data::date, name)	
	11.8
		Nomear indices seguindo padrão
		Indices FKs: ix_fk__${table-name}_${fields-names}_${seq-table-idx}
		Indices Uniques: ix_un__${table-name}_${fields-names}_${seq-table-idx}
		Indices Normals: ix_nm__${table-name}_${fields-names}_${seq-table-idx}
		Seq-table-idx é incremental iniciando de 1 com zeros à esquerda, ex: 01, 002
FASE 12: MANTER ARQUIVO INIT.DATA
	12.1 Arquivo contém scripts personalizados	
	12.2 Se arquivo já existe manter sem modificações
FASE 13: MANTER ARQUIVO FAKE-DATA.sql
	13.1 Arquivo contém scripts personalizados	
	13.2 Se arquivo já existe manter sem modificações
```
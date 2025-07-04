# Capítulo 2 – Linguagem de Definição de Dados (DDL)

Após estabelecermos os fundamentos conceituais e históricos da linguagem SQL, é hora de mergulhar na prática de construir a infraestrutura que armazenará nossos dados. Antes de manipular ou consultar qualquer informação, é preciso definir sua estrutura. Essa é a responsabilidade da **Linguagem de Definição de Dados (DDL)**, o conjunto de comandos SQL que nos permite criar, alterar e remover os objetos que compõem um banco de dados. Este capítulo se dedica a explorar esses comandos, detalhando como dar vida a um modelo de dados, desde a criação do contêiner principal do banco até a definição minuciosa de cada tabela e suas colunas.

## A Arquitetura Lógica: Instâncias, Catálogos e Esquemas

Quando um modelo de dados conceitual é projetado, ele eventualmente precisa ser traduzido para uma implementação física em um SGBD. Essa implementação ocorre dentro de uma hierarquia lógica bem definida. No topo dessa hierarquia, temos a **instância do SGBD**, que é o software do sistema de banco de dados em execução. Uma única instância pode gerenciar múltiplos **bancos de dados** (ou **catálogos**). Cada catálogo, por sua vez, contém uma coleção de **esquemas (schemas)**.

Um esquema pode ser visto como um contêiner ou um espaço de nomes que agrupa objetos de banco de dados relacionados, como tabelas, visões, índices e outros construtores que pertencem a uma mesma aplicação ou a um mesmo domínio de negócio. Essa organização lógica é fundamental para a gestão de ambientes complexos, permitindo separar, por exemplo, os objetos do setor de Recursos Humanos dos objetos do setor Financeiro. Cada esquema é identificado por um nome e geralmente está associado a um usuário ou conta de autorização.

<div align="center">
  <img width="700px" src="./img/02-hierarquia-entre-instancias-esquemas-e-objetos.png">
</div>

Dentro de cada catálogo, existe um esquema especial e fundamental, definido pelo padrão SQL: o **INFORMATION_SCHEMA**. Este esquema não armazena dados de negócio, mas sim **metadados**, ou seja, dados sobre os próprios dados. Ele funciona como o **dicionário de dados** do sistema, fornecendo uma descrição detalhada de todos os outros esquemas, tabelas, colunas, restrições e demais objetos definidos no catálogo.

A grande vantagem do `INFORMATION_SCHEMA` é que ele expõe esses metadados através de um conjunto de **visões (views)** somente leitura. Uma visão é uma tabela virtual cujo conteúdo é derivado de uma consulta. Isso significa que é possível usar o próprio comando `SELECT` do SQL para consultar a estrutura do banco de dados. Por exemplo, para descobrir todas as colunas de uma tabela específica, basta fazer uma consulta à visão `COLUMNS` dentro do `INFORMATION_SCHEMA`, filtrando pelo nome da tabela desejada. Essa abordagem padronizada oferece uma maneira poderosa e consistente de inspecionar a arquitetura do banco de dados de forma programática.

Para criar essa estrutura, utiliza-se os comandos DDL. Embora o comando `CREATE DATABASE` não faça parte do padrão SQL ANSI/ISO (que delega a criação do ambiente ao SGBD), ele é uma extensão quase universalmente adotada.

```sql
-- Cria um novo banco de dados (ou catálogo)
CREATE DATABASE nome_do_banco_de_dados;

-- Remove um banco de dados e todos os seus objetos
DROP DATABASE nome_do_banco_de_dados;
```

Uma vez dentro de um banco de dados, podemos criar esquemas para organizar nossos objetos. A criação de esquemas é uma operação privilegiada, normalmente restrita a administradores (DBAs).

```sql
-- Cria um novo esquema
CREATE SCHEMA nome_do_esquema;

-- Remove um esquema e, opcionalmente, todos os objetos contidos nele
DROP SCHEMA nome_do_esquema [ CASCADE | RESTRICT ];
```

A opção `RESTRICT` (padrão) impede a exclusão se o esquema contiver objetos, enquanto `CASCADE` remove o esquema e todos os seus conteúdos.

## Trabalhando com Tabelas: A Estrutura Fundamental

A tabela é o objeto central do modelo relacional, a estrutura onde os dados são efetivamente armazenados. Conforme a definição padrão, uma tabela é uma coleção de linhas, onde cada linha representa uma ocorrência ou uma entidade do mundo real, e todas as linhas seguem a mesma estrutura de colunas. Dois conceitos importantes definem suas dimensões:

- **Grau (Degree):** É o número de colunas (ou atributos) que a tabela possui. O grau define a estrutura da tabela.
- **Cardinalidade (Cardinality):** É o número de linhas (ou tuplas) presentes na tabela em um determinado momento. Uma tabela com cardinalidade zero é chamada de tabela vazia.

<div align="center">
  <img width="580px" src="./img/02-propriedade-das-tabelas.png">
</div>

As tabelas em SQL podem ser classificadas em dois tipos principais: **tabelas base** e **tabelas derivadas**.

- **Tabelas Base (Base Tables):** São as tabelas "reais", cujos dados são fisicamente armazenados no banco de dados. Elas podem ser persistentes (o tipo mais comum) ou temporárias (existindo apenas durante uma sessão ou transação).
- **Tabelas Derivadas (Derived Tables):** São tabelas virtuais, cujo conteúdo não é armazenado diretamente, mas sim calculado ou derivado a partir de uma ou mais tabelas base no momento da consulta. O exemplo mais proeminente de uma tabela derivada é uma **visão (view)**, criada com o comando `CREATE VIEW`. Uma visão pode ser atualizável (permitindo operações de `INSERT`, `UPDATE`, `DELETE` que afetam a tabela base subjacente) ou não atualizável. Geralmente, uma visão é atualizável apenas se for baseada em uma única tabela e incluir a chave primária e todas as colunas que não permitem valores nulos e não possuem um valor padrão. Visões que envolvem junções, agrupamentos ou funções de agregação não são atualizáveis.

### Definindo, Modificando e Removendo Tabelas

O ciclo de vida de uma tabela é gerenciado por um conjunto central de comandos DDL.

#### O Comando `CREATE TABLE`

Este é o principal comando da DDL, usado para definir uma nova tabela, especificando seu nome, suas colunas e as restrições de integridade associadas.

**Sintaxe Geral:**

```sql
CREATE TABLE nome_da_tabela (
    nome_coluna1 tipo_de_dado [restrições_da_coluna],
    nome_coluna2 tipo_de_dado [restrições_da_coluna],
    ...
    [restrições_da_tabela]
);
```

**Exemplo Prático:** Vamos criar uma tabela `ALUNO` para armazenar informações de estudantes.

```sql
CREATE TABLE ALUNO (
    NOME VARCHAR(20) NOT NULL,
    CPF INT PRIMARY KEY,
    SEXO CHAR(1) NOT NULL,
    DATA_NASCIMENTO DATE NOT NULL,
    CIDADE VARCHAR(50) NOT NULL,
    VALOR_PAGO INT NOT NULL
);
```

Após a execução deste comando, a estrutura da tabela `ALUNO` existirá no banco de dados, pronta para receber dados.

**Resultado da Criação:**

| NOME | CPF | SEXO | DATA_NASCIMENTO | CIDADE | VALOR_PAGO |
| --- | --- | --- | --- | --- | --- |
| - | - | - | - | - | - |

#### O Comando `ALTER TABLE`

Frequentemente, é necessário modificar a estrutura de uma tabela existente. O comando `ALTER TABLE` permite adicionar, remover ou modificar colunas.

**Sintaxe (Resumida):**

```sql
-- Adicionar uma nova coluna
ALTER TABLE nome_da_tabela ADD COLUMN nome_nova_coluna tipo_de_dado;

-- Remover uma coluna existente
ALTER TABLE nome_da_tabela DROP COLUMN nome_coluna_existente;

-- Modificar o tipo de dado de uma coluna
ALTER TABLE nome_da_tabela ALTER COLUMN nome_coluna_existente SET DATA TYPE novo_tipo_de_dado;
```

A sintaxe exata para modificar colunas pode variar entre os SGBDs (por exemplo, usando `MODIFY COLUMN` em vez de `ALTER COLUMN`).

**Exemplo de Alterações na Tabela `ALUNO`:**

```sql
ALTER TABLE ALUNO ADD COLUMN EMAIL VARCHAR(40);
ALTER TABLE ALUNO DROP COLUMN SEXO;
ALTER TABLE ALUNO ALTER COLUMN VALOR_PAGO TYPE FLOAT; -- Sintaxe do PostgreSQL, pode variar
```

**Estrutura Resultante:**

| NOME | CPF | DATA_NASCIMENTO | CIDADE | VALOR_PAGO | EMAIL |
| --- | --- | --- | --- | --- | --- |
| - | - | - | - | - | - |

#### O Comando `DROP TABLE`

Para remover completamente uma tabela e todos os seus dados, utiliza-se o comando `DROP TABLE`. É uma ação irreversível que apaga a definição da tabela e todo o seu conteúdo.

**Sintaxe:**

```sql
DROP TABLE nome_da_tabela;
```

**Exemplo:**

```sql
DROP TABLE ALUNO;
```

### Resumo dos Principais Comandos DDL

Os comandos da Linguagem de Definição de Dados são a base para a criação e manutenção da arquitetura de um banco de dados relacional.

|Comando|Descrição|
|---|---|
|**`CREATE`**|Utilizado para criar novos objetos de banco de dados, como `DATABASE`, `SCHEMA`, `TABLE`, `VIEW`, `INDEX`.|
|**`DROP`**|Utilizado para excluir permanentemente um objeto do banco de dados e todos os dados associados a ele.|
|**`ALTER`**|Utilizado para modificar a estrutura de um objeto existente, como adicionar, remover ou alterar colunas de uma tabela.|
|**`TRUNCATE`**|Utilizado para remover rapidamente todas as linhas de uma tabela, mantendo sua estrutura. Geralmente é mais rápido que um `DELETE` sem cláusula `WHERE`, pois não registra as exclusões linha a linha.|
|**`RENAME`**|Utilizado para renomear um objeto, como uma tabela. É uma extensão comum, não presente em todas as versões do padrão SQL.|

<div align="center">
  <img width="760px" src="./img/02-resumo-dos-principais-comandos-ddl.png">
</div>

Com o conhecimento de como criar e gerenciar a estrutura das tabelas, o próximo passo crucial é entender os diferentes tipos de dados que podemos atribuir a cada coluna, garantindo que as informações sejam armazenadas de forma correta e eficiente. Para entendermos melhor como escolher os tipos de dados das nossas colunas, passaremos, a seguir, a analisar os principais tipos de dados de SQL e suas peculiaridades.
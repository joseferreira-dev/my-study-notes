# Capítulo 3 – Álgebra Relacional

Nos capítulos anteriores, exploramos os "o quês" e os "porquês" do design de bancos de dados. Agora, antes de mergulharmos na linguagem SQL para aprender "como" interagir com os dados, é fundamental compreendermos a sua base teórica. A **Álgebra Relacional** é uma linguagem de consulta formal, baseada em fundamentos matemáticos, que nos permite descrever operações sobre relações (tabelas).

Pense na álgebra relacional como a "gramática" por trás do SQL. Ela nos fornece um conjunto de operadores (seleção, projeção, junção, etc.) que atuam sobre uma ou mais tabelas para produzir uma nova tabela como resultado, sem nunca alterar as tabelas originais. Dominar seus conceitos não só é essencial para resolver questões teóricas, mas também para desenvolver uma compreensão mais profunda de como os SGBDs processam e otimizam as consultas que escrevemos em SQL.

Para tornar nosso estudo mais prático, utilizaremos ao longo deste capítulo duas tabelas de exemplo que representam um cenário simples de vendas em uma loja.

**Tabela `PRODUTOS`**

<div align="center">
<img width="700px" src="./img/03-tabela-produtos.png">
</div>

**Tabela `VENDAS`**

<div align="center">
<img width="700px" src="./img/03-tabela-vendas.png">
</div>

## Seleção (σ): Filtrando as Linhas

A **seleção** é uma das operações fundamentais e mais intuitivas da álgebra relacional. Sua função é filtrar as tuplas (linhas) de uma relação, retornando um subconjunto de linhas que satisfazem uma determinada condição lógica (predicado). Ela funciona como um "fatiador" horizontal, que seleciona apenas as linhas de interesse, mas mantém todas as suas colunas originais.

A notação formal para a operação de seleção é:

σ<sub>[predicado]</sub>(R)

Onde:

- **σ** (a letra grega sigma) é o símbolo que representa a operação de seleção.
- **`[predicado]`** é a condição de seleção, a regra que será aplicada a cada linha da tabela.
- **(R)** é a relação (a tabela) sobre a qual a operação será executada.

O predicado pode ser composto por comparações (`=`, `≠`, `>`, `<`, `>=`, `<=`) e pode combinar múltiplas condições usando os operadores lógicos `∧` (E), `∨` (OU) e `¬` (NÃO).

#### Exemplos de Seleção

Vamos aplicar a operação de seleção em nossa tabela `VENDAS`.

**Exemplo 1: Selecionar vendas com valor total maior que 500.**

A expressão em álgebra relacional para esta consulta seria:

σ<sub>VALOR_TOTAL>500</sub>(VENDAS)

O SGBD aplicaria a condição `VALOR_TOTAL > 500` a cada uma das linhas da tabela `VENDAS`. O resultado seria uma nova tabela contendo apenas as linhas que satisfazem essa condição, com todas as suas colunas originais.

<div align="center">
<img width="700px" src="./img/03-resultado-selecao.png">
</div>

**Exemplo 2: Selecionar vendas do produto de ID 4 que ocorreram na data '2024-03-10'.**

Aqui, precisamos combinar duas condições com o operador E (`∧`).

σ<sub>(ID_PRODUTO=4)∧(DATA=′2024−03−10′)</sub>​(VENDAS)

O resultado desta operação seria a seguinte tabela:

|ID|ID_PRODUTO|DATA|QUANTIDADE|VALOR_TOTAL|
|---|---|---|---|---|
|3|4|2024-03-10|1|500|

#### Relação com o SQL

A operação de seleção da álgebra relacional tem uma correspondência direta e fundamental com a cláusula **`WHERE`** da linguagem SQL. A expressão σ<sub>[predicado]</sub>(R) é a base teórica para o comando `SELECT * FROM R WHERE <predicado>;`.

A expressão σ<sub>VALOR_TOTAL>500</sub>(VENDAS) é equivalente a:

```sql
SELECT * FROM VENDAS WHERE VALOR_TOTAL > 500;
```

## Projeção (π): Selecionando as Colunas

Enquanto a operação de seleção atua como um "fatiador horizontal" que filtra as linhas de uma tabela, a **projeção** funciona como um "fatiador vertical". Sua finalidade é selecionar um subconjunto de atributos (colunas) de uma relação, descartando as colunas desnecessárias e gerando uma nova tabela com uma estrutura mais enxuta.

A projeção é extremamente útil para criar visões simplificadas dos dados, mostrando apenas a informação relevante para uma determinada tarefa ou relatório.

A notação formal para a operação de projeção é:

π<sub>[lista_de_atributos]​</sub>(R)

Onde:

- **π** (a letra grega pi) é o símbolo que representa a operação de projeção.
- **`[lista_de_atributos]`** é a lista de colunas, separadas por vírgula, que desejamos manter no resultado.
- **(R)** é a relação (a tabela) sobre a qual a operação será executada.

Um conceito fundamental da álgebra relacional é que o resultado de qualquer operação é sempre uma **relação** válida. E, como vimos no início do capítulo, uma relação é um _conjunto_ de tuplas, o que, por definição, significa que **não pode conter linhas duplicadas**.

Portanto, se a operação de projeção resultar em linhas idênticas, as duplicatas são automaticamente eliminadas, restando apenas uma ocorrência de cada linha.

### Exemplos de Projeção

Vamos aplicar a operação de projeção em nossas tabelas de exemplo.

**Exemplo 1: Listar o nome e o estoque de todos os produtos.**

Queremos selecionar apenas as colunas `NOME` e `ESTOQUE` da tabela `PRODUTOS`.

A expressão em álgebra relacional seria:

π<sub>NOME,ESTOQUE​</sub>(PRODUTOS)

O resultado é uma nova tabela contendo apenas as duas colunas especificadas, com todas as linhas da tabela original.

<div align="center">
<img width="360px" src="./img/03-resultado-projecao.png">
</div>

**Exemplo 2: Listar os IDs de todos os produtos que foram vendidos (demonstrando a eliminação de duplicatas).**

Queremos saber quais produtos únicos foram vendidos, então vamos projetar apenas a coluna `ID_PRODUTO` da tabela `VENDAS`.

A expressão seria:

π<sub>ID_PRODUTO</sub>​(VENDAS)

Analisando a coluna `ID_PRODUTO` na tabela `VENDAS`, os valores são `(1, 2, 4, 3, 2)`. Como o resultado deve ser uma relação (um conjunto), o valor duplicado `2` é eliminado. O resultado final será:

|ID_PRODUTO|
|---|
|1|
|2|
|4|
|3|

### Combinando Seleção e Projeção

O verdadeiro poder da álgebra relacional surge ao combinar operadores. Podemos, por exemplo, primeiro filtrar as linhas com uma seleção e, em seguida, extrair as colunas de interesse do resultado com uma projeção.

**Exemplo 3: Listar o nome e o preço dos produtos que custam mais de R$ 1000.**

**Passo 1 (Seleção):** Primeiro, filtramos a tabela `PRODUTOS` para encontrar apenas as linhas desejadas.

TEMP ← σ<sub>PRECO>1000</sub>(PRODUTOS)

**Passo 2 (Projeção):** Em seguida, projetamos as colunas `NOME` e `PREÇO` da tabela temporária resultante.

RESULTADO ← π<sub>NOME,PRECO</sub>(TEMP)

A expressão completa, aninhando as operações, seria:

π<sub>NOME,PRECO</sub>(σ<sub>PRECO>1000</sub>(PRODUTOS))

O resultado seria a seguinte tabela:

|NOME|PREÇO|
|---|---|
|Notebook|2500|
|Smartphone|1200|

### Relação com o SQL

A operação de projeção da álgebra relacional corresponde diretamente à lista de colunas na cláusula **`SELECT`** do SQL.

A expressão π<sub>NOME,ESTOQUE​</sub>(PRODUTOS) é equivalente a:

```sql
SELECT NOME, ESTOQUE FROM PRODUTOS;
```

Contudo, há uma diferença importante: por padrão, o `SELECT` do SQL **não elimina linhas duplicadas**. Para simular o comportamento exato da projeção e garantir um resultado sem duplicatas, é necessário usar a palavra-chave **`DISTINCT`**.

A expressão π<sub>ID_PRODUTO​</sub>(PRODUTOS) é equivalente a:

```sql
SELECT DISTINCT ID_PRODUTO FROM VENDAS;
```


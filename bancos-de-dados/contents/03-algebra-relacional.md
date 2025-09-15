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


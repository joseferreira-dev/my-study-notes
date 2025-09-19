# Capítulo 7 – Linguagem SQL: Junções

Nos capítulos anteriores, dedicamos um tempo considerável ao processo de **normalização**, aprendendo a decompor grandes conjuntos de informações em tabelas menores, mais enxutas e consistentes. O resultado desse processo é um banco de dados organizado e livre de redundâncias, onde cada tabela armazena dados sobre uma única entidade. No entanto, essa organização nos deixa com um novo desafio: as informações que precisamos para gerar um relatório ou uma visualização útil raramente se encontram em uma única tabela. Para responder a perguntas de negócio como "Qual o nome do cliente que comprou o produto X?", precisamos de uma forma de conectar e combinar os dados que foram logicamente separados. É aqui que entram as **junções**.

## Conceitos Gerais

A cláusula **`JOIN`** é a ferramenta da linguagem SQL que nos permite implementar as operações de junção da Álgebra Relacional. Sua finalidade é combinar linhas de duas ou mais tabelas com base em uma coluna relacionada entre elas, geralmente a chave primária de uma tabela e a chave estrangeira correspondente em outra.

É através das junções que reconstruímos uma visão completa dos dados, unindo as informações que a normalização distribuiu. Existem quatro tipos principais de junções que são essenciais para o nosso estudo:

- **INNER JOIN (Junção Interna)**
- **LEFT JOIN (Junção Externa à Esquerda)**
- **RIGHT JOIN (Junção Externa à Direita)**
- **FULL OUTER JOIN (Junção Externa Completa)**

<div align="center">
<img width="440px" src="./img/07-tipos-de-juncoes.png">
</div>

Utilizando a teoria dos conjuntos como analogia, podemos visualizar o comportamento de cada junção:

- O **`INNER JOIN`** funciona como uma **interseção**, retornando apenas as linhas que possuem uma correspondência em **ambas** as tabelas.
- O **`LEFT JOIN`** retorna **todas** as linhas da tabela à esquerda da cláusula `JOIN` e apenas as linhas correspondentes da tabela à direita. Se não houver correspondência, as colunas da tabela da direita serão preenchidas com `NULL`.
- O **`RIGHT JOIN`** é o inverso do `LEFT JOIN`: retorna **todas** as linhas da tabela à direita e apenas as correspondentes da tabela à esquerda.
- O **`FULL OUTER JOIN`** funciona como uma **união completa**, retornando **todas** as linhas de **ambas** as tabelas. Se houver uma correspondência, as linhas são combinadas. Se não houver, as colunas da tabela sem correspondência são preenchidas com `NULL`.

Para desbravar cada um desses tipos de junção, utilizaremos duas tabelas de exemplo ao longo deste capítulo.

**Tabela `PRODUTO`**

|ID (PK)|NOME|PRECO|
|---|---|---|
|1|Camiseta|25|
|2|Calça Jeans|50|
|3|Tênis|80|
|4|Meia|5|

**Tabela `VENDAS`**

|ID (PK)|ID_PRODUTO (FK)|QUANTIDADE|
|---|---|---|
|1|1|10|
|2|2|5|
|3|1|3|
|4|3|5|

Neste cenário, a tabela `VENDAS` se relaciona com a tabela `PRODUTO` através da coluna `ID_PRODUTO`, que é uma chave estrangeira referenciando o `ID` da tabela `PRODUTO`. Note que o produto "Meia" (ID 4) existe, mas nunca foi vendido. Essa ausência de correspondência será fundamental para entendermos as diferenças entre os tipos de `JOIN`.

## FULL OUTER JOIN: A União Completa

O **`FULL JOIN`**, ou mais formalmente **`FULL OUTER JOIN`**, é o tipo de junção mais completo. Sua finalidade é combinar e retornar **todas as linhas** de **ambas as tabelas** participantes da junção.

<div align="center">
<img width="200px" src="./IMG/07-full-join.png">
</div>

Ele funciona da seguinte maneira:

- Se houver uma correspondência entre as linhas das duas tabelas (com base na condição de junção), ele combina as colunas de ambas em uma única linha no resultado.
- Se uma linha da tabela da esquerda não encontrar uma correspondência na tabela da direita, ela ainda assim será incluída no resultado, e as colunas que viriam da tabela da direita serão preenchidas com valores `NULL`.
- Da mesma forma, se uma linha da tabela da direita não encontrar uma correspondência na tabela da esquerda, ela também será incluída no resultado, e as colunas da tabela da esquerda serão preenchidas com `NULL`.

### Sintaxe da Junção

A estrutura de uma consulta com `JOIN` é uma das mais importantes da linguagem SQL. É crucial entender cada um de seus componentes.

```SQL
SELECT <colunas>
FROM <tabela_esquerda>
FULL OUTER JOIN <tabela_direita>
ON <condicao_de_juncao>;
```

- **`FROM <tabela_esquerda>`**: A primeira tabela mencionada na consulta, que serve como base para a junção.
- **`FULL OUTER JOIN <tabela_direita>`**: O tipo de junção e a segunda tabela que será combinada.
- **`ON <condicao_de_juncao>`**: Esta é a cláusula mais importante. É aqui que especificamos a regra de correspondência, ou seja, as **colunas de equivalência**. A junção ocorrerá apenas para as linhas onde a condição `ON` for verdadeira. Geralmente, esta condição compara a chave primária de uma tabela com a chave estrangeira da outra.

Vamos analisar visualmente a sintaxe aplicada ao nosso exemplo, onde a coluna de correspondência é `PRODUTO.ID` e `VENDAS.ID_PRODUTO`.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-visual.png">
</div>

### Comportamento da Junção Completa

Ao executar um `FULL OUTER JOIN`, o SGBD analisa as linhas de ambas as tabelas e pode se deparar com três cenários distintos para formar o resultado final.

- **Cenário 1: Presença em Ambas as Tabelas (Correspondência Encontrada)**
	Quando um valor na coluna de equivalência da tabela da esquerda corresponde a um valor na coluna da direita, as linhas são combinadas. Se uma chave na tabela da esquerda corresponde a múltiplas chaves na direita (como no caso do `PRODUTO.ID = 1`, que tem duas vendas), a linha da tabela da esquerda será duplicada no resultado para formar um par com cada correspondência.

- **Cenário 2: Presença Somente na Tabela da Esquerda (Sem Correspondência)**
	Quando uma linha da tabela da esquerda não encontra nenhuma correspondência na tabela da direita, ela é mantida no resultado. Suas colunas são preenchidas com seus próprios dados, e todas as colunas que deveriam vir da tabela da direita são preenchidas com `NULL`. Em nosso exemplo, o produto "Meia" (`ID = 4`) se encaixa neste cenário.

- **Cenário 3: Presença Somente na Tabela da Direita (Sem Correspondência)**
	Inversamente, quando uma linha da tabela da direita não encontra nenhuma correspondência na esquerda, ela também é mantida, e as colunas da tabela da esquerda são preenchidas com `NULL`. Embora não tenhamos um exemplo direto em nossos dados, imagine que houvesse uma venda registrada com `ID_PRODUTO = 99`, para um produto que já foi excluído da tabela `PRODUTO`. Essa linha de venda apareceria no resultado do FULL JOIN, com as colunas `ID`, `NOME` e `PRECO` preenchidas com `NULL`.

Vamos analisar como cada linha da tabela se encaixa nas situações acima, graficamente.

### Análise Gráfica Passo a Passo

Para entender como a tabela final de um `FULL OUTER JOIN` é montada, vamos simular o processo que o SGBD realiza, analisando cada linha da tabela da esquerda (`PRODUTO`) e buscando suas correspondências na tabela da direita (`VENDAS`).

**Análise para `PRODUTO.ID = 1` (Camiseta)**

O SGBD pega a primeira linha da tabela `PRODUTO` e procura por todas as linhas na tabela `VENDAS` onde `VENDAS.ID_PRODUTO` seja igual a 1.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-1.png">
</div>

Neste caso, ele encontra duas correspondências: a venda com `ID = 1` e a venda com `ID = 3`. Como resultado, a linha do produto "Camiseta" será duplicada na tabela final para se combinar com cada uma das vendas encontradas.

**Linhas Resultantes para `PRODUTO.ID = 1`:**

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-2.png">
</div>

**Análise para `PRODUTO.ID = 2` (Calça Jeans)**

O processo se repete para o segundo produto. O SGBD busca por vendas onde `ID_PRODUTO = 2`.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-3.png">
</div>

Ele encontra apenas uma correspondência, a venda com `ID = 2`. Portanto, apenas uma linha será gerada no resultado para este produto.

**Linha Resultante para `PRODUTO.ID = 2`:**

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-4.png">
</div>

**Análise para `PRODUTO.ID = 3` (Tênis)**

Novamente, o SGBD busca por vendas onde `ID_PRODUTO = 3`.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-5.png">
</div>

Ele encontra uma única correspondência, a venda com `ID = 4`.

**Linha Resultante para `PRODUTO.ID = 3`:**

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-6.png">
</div>

**Análise para `PRODUTO.ID = 4` (Meia)**

Agora chegamos ao caso especial que define a natureza de um `OUTER JOIN`. O SGBD busca por vendas onde `ID_PRODUTO = 4`.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-7.png">
</div>

Ele não encontra nenhuma correspondência. Em um `INNER JOIN`, esta linha seria simplesmente descartada. No entanto, como estamos em um **`FULL OUTER JOIN`**, a regra é preservar todas as linhas de ambas as tabelas. Portanto, a linha do produto "Meia" é mantida, e todas as colunas que deveriam vir da tabela `VENDAS` são preenchidas com o valor **`NULL`**, indicando a ausência de uma correspondência.

**Linha Resultante para `PRODUTO.ID = 4`:**

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-8.png">
</div>

#### Resultado Final

Ao juntar todas as linhas que geramos em nossa análise passo a passo, obtemos a tabela de resultado final e completa da operação `FULL OUTER JOIN`.

<div align="center">
<img width="700px" src="./IMG/07-full-join-exemplo-9.png">
</div>

Esta tabela final nos dá uma visão completa do universo de dados: os produtos que foram vendidos (e os detalhes de suas vendas) e também os produtos que não tiveram nenhuma venda registrada, tudo em um único conjunto de resultados.
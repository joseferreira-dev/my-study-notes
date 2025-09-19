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


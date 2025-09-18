# Capítulo 5 – Cálculo Relacional

No capítulo anterior, exploramos em detalhe a Álgebra Relacional, uma linguagem formal que nos permite construir consultas através de uma sequência de operações (seleção, projeção, junção, etc.). Vimos que ela é uma linguagem **procedural**, pois especificamos o "como", ou seja, o passo a passo para se chegar ao resultado desejado. Agora, vamos estudar sua contraparte teórica: o **Cálculo Relacional**.

O Cálculo Relacional oferece uma perspectiva fundamentalmente diferente. Em vez de focar nos passos, ele se concentra no resultado. É uma linguagem **declarativa** (ou não-procedural), onde descrevemos as propriedades do conjunto de dados que queremos obter, sem especificar o método para obtê-lo. A analogia é clara: a Álgebra Relacional é como uma receita de bolo, com instruções passo a passo; o Cálculo Relacional é como a foto e a descrição do bolo finalizado, deixando a cargo do "cozinheiro" (o SGBD) a tarefa de descobrir como produzi-lo. É esta filosofia declarativa que serve de inspiração direta para a linguagem SQL.

## Conceitos Gerais

O **Cálculo Relacional (CR)** é uma linguagem de consulta formal para bancos de dados relacionais, baseada diretamente nos princípios da lógica de predicados da matemática. Uma expressão no cálculo relacional é uma "fórmula" que descreve as condições que as tuplas ou os valores do resultado devem satisfazer.

As expressões do cálculo relacional são construídas a partir de elementos como variáveis, constantes, operadores de comparação (`=`, `>`, `<`), operadores lógicos (`∧` para E, `∨` para OU, `¬` para NÃO) e quantificadores lógicos (`∀` para "para todo", `∃` para "existe").

Existem duas formas principais do Cálculo Relacional, que se diferenciam pelo tipo de variável que utilizam em suas fórmulas:

- **Cálculo Relacional de Tuplas (CRT):** Nesta abordagem, as variáveis representam tuplas (linhas) inteiras de uma relação. Uma consulta CRT busca por **tuplas** que satisfazem uma determinada condição.
- **Cálculo Relacional de Domínio (CRD):** Nesta abordagem, as variáveis representam valores do domínio dos atributos (os valores individuais dentro das "células" da tabela). Uma consulta CRD busca por combinações de **valores** que satisfazem uma condição.

Apesar de suas abordagens diferentes, foi provado que a Álgebra Relacional, o CRT e o CRD são **equivalentes em poder de expressão**. Qualquer consulta que pode ser escrita em uma dessas linguagens formais também pode ser escrita nas outras duas. Elas são apenas maneiras diferentes de pensar sobre o mesmo problema de recuperação de dados.

Vamos começar nossa exploração detalhada pelo Cálculo Relacional de Tuplas, o CRT.

## Cálculo Relacional de Tuplas (CRT)

O **Cálculo Relacional de Tuplas (CRT)** é uma abordagem do cálculo relacional onde as fórmulas são construídas em torno de **variáveis de tupla**. Cada variável de tupla, como o nome sugere, é um marcador que representa uma tupla (linha) inteira de uma relação específica. Ela não se refere a um valor individual, mas ao conjunto completo de atributos que compõem aquela linha.

Uma consulta em CRT é especificada através de uma expressão formal que descreve as propriedades das tuplas que desejamos no resultado. A estrutura geral de uma consulta CRT é:

`{ t | P(t) }`

Onde:

- **t** é uma **variável de tupla**.
- **P(t)** é um **predicado**, que é uma fórmula da lógica de predicados que descreve as condições que a tupla `t` deve satisfazer para ser incluída no resultado.

A expressão inteira pode ser lida como: "Retorne o conjunto de todas as tuplas `t` para as quais o predicado `P(t)` é verdadeiro".

### A Variável de Tupla

O conceito central do CRT é a variável de tupla. Vamos usar uma tabela de exemplo para ilustrar.

**Tabela Estudantes**

| matrícula | nome | idade |
| --- | --- | --- |
| 101 | Alice | 20 |
| 102 | Bob | 22 |
| 103 | Carol | 20 |

Se definirmos uma variável de tupla `t` para a relação `Estudantes`, `t` pode assumir, uma de cada vez, o valor de cada uma das tuplas da tabela. Quando `t` representa a primeira linha, `t.matrícula` é `101`, `t.nome` é "Alice" e `t.idade` é `20`.

### Construindo uma Consulta em CRT

Vamos construir uma consulta para responder à seguinte pergunta: "Quais são os nomes dos estudantes que têm 20 anos?".

A expressão em CRT para esta consulta seria:

`{ t.nome | t ∈ Estudantes ∧ t.idade = 20 }`

Vamos dissecar cada parte desta expressão:

- **`{ t.nome | ... }`**: Esta é a **cláusula de alvo**. Ela especifica o que queremos no nosso resultado. Neste caso, não queremos a tupla inteira, mas apenas o valor do atributo `nome` da tupla `t`. Isso é análogo à projeção (π) da Álgebra Relacional e à cláusula `SELECT` do SQL.
- **`t ∈ Estudantes`**: Esta é uma condição que "liga" a variável de tupla `t` à relação `Estudantes`. Lê-se como "`t` é uma tupla que pertence à relação `Estudantes`". Isso é análogo à cláusula `FROM` do SQL.
- **`∧`**: Este é o operador lógico de conjunção ("E"), que conecta as condições do predicado.
- **`t.idade = 20`**: Esta é a condição de filtragem. Ela especifica que a tupla `t` só será considerada se o valor de seu atributo `idade` for igual a 20. Isso é análogo à seleção (σ) da Álgebra Relacional e à cláusula `WHERE` do SQL.

A expressão completa é lida como: "Retorne o atributo `nome` de todas as tuplas `t`, tal que `t` pertença à relação `Estudantes` E o atributo `idade` de `t` seja igual a 20".

O resultado desta consulta seria a seguinte relação:

|nome|
|---|
|Alice|
|Carol|

### Sintaxe Alternativa

Existem formas alternativas de escrever a mesma expressão lógica, que podem ser encontradas em diferentes literaturas. Uma sintaxe comum é:

`{ t.nome | Estudantes(t) ∧ t.idade = 20 }`

Nesta notação, `Estudantes(t)` é usado em vez de `t ∈ Estudantes`, mas o significado é idêntico: "`t` é uma tupla da relação `Estudantes`". O resultado da consulta permanece exatamente o mesmo. A escolha da sintaxe é uma questão de convenção, mas a lógica subjacente do predicado não se altera.



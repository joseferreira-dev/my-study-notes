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




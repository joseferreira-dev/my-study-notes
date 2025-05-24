# Capítulo 11 – Proposições, Álgebra Booleana e Circuitos Lógicos

## Proposições e Tabelas Verdade

Para compreender como os circuitos lógicos dos computadores funcionam, é indispensável, antes de tudo, entender a lógica proposicional. A lógica é a base não apenas da eletrônica digital, mas de toda a computação moderna, desde algoritmos até o funcionamento dos processadores.

### Conceito de Proposição

Uma proposição é, de forma bem direta, uma sentença declarativa que, obrigatoriamente, deve poder ser classificada como **verdadeira (V)** ou **falsa (F)**. Ou seja, ela precisa ter um valor lógico bem definido. Por exemplo:

- “Eu estudo para provas.”
- “Eu gosto de viajar.”
- “Se eu estudo para provas, então eu gosto de viajar.”

Todas essas frases são exemplos de proposições, pois podemos julgá-las como verdadeiras ou falsas. Agora vejamos uma frase que **não é uma proposição**:

- “Boa noite!”

Perceba que essa expressão não pode ser classificada como verdadeira ou falsa. Trata-se apenas de uma saudação, portanto, não é uma proposição no contexto da lógica formal.

Nas representações lógicas, as proposições são normalmente representadas por letras minúsculas, como **p, q, r**, e podem ser combinadas através de **conectivos lógicos**, como veremos a seguir.

### Conectivos Lógicos e suas Tabelas Verdade

Os conectivos lógicos são responsáveis por formar **proposições compostas** a partir de proposições simples. Cada conectivo possui uma regra específica para determinar quando a proposição composta é verdadeira ou falsa. Para isso, utilizamos uma ferramenta fundamental: a **tabela verdade**, que mostra todas as combinações possíveis dos valores lógicos das proposições.

#### Negação (¬)

A negação, representada por símbolos como ¬, ~, NOT ou um traço sobre a proposição, simplesmente inverte o valor lógico da proposição. Se a proposição é verdadeira, sua negação é falsa, e vice-versa.

Exemplo:
Se **p = "Gosto de jogar bola"**, então a negação, representada por **¬p**, é "Não gosto de jogar bola".

|p|¬p|
|---|---|
|V|F|
|F|V|

#### Conjunção (E)

A conjunção é representada pelos símbolos **∧, ^, .** ou pela palavra **AND**. Ela é verdadeira **apenas quando ambas as proposições são verdadeiras**.

Exemplo:  
Se **p = "Gosto de jogar bola"** e **q = "Gosto de estudar"**, então **p ∧ q = "Gosto de jogar bola e gosto de estudar"**.

|p|q|p ∧ q|
|---|---|---|
|V|V|V|
|V|F|F|
|F|V|F|
|F|F|F|

#### Disjunção (OU)

A disjunção, representada por **∨, +** ou **OR**, é verdadeira sempre que **pelo menos uma das proposições for verdadeira**.

Exemplo:  
**p = "Gosto de jogar bola"**  
**q = "Gosto de estudar"**  
**p ∨ q = "Gosto de jogar bola ou gosto de estudar"**

|p|q|p ∨ q|
|---|---|---|
|V|V|V|
|V|F|V|
|F|V|V|
|F|F|F|

#### Disjunção Exclusiva (XOR)

A disjunção exclusiva, representada por **<u>∨</u>, ⊕** ou **XOR**, é verdadeira **somente quando uma das proposições for verdadeira, mas não ambas ao mesmo tempo**.

Exemplo:  
**p = "Gosto de jogar bola"**  
**q = "Gosto de estudar"**  
**p <u>∨</u> q = "Ou gosto de jogar bola, ou gosto de estudar (mas não ambos)"**

|p|q|p <u>∨</u> q|
|---|---|---|
|V|V|F|
|V|F|V|
|F|V|V|
|F|F|F|

#### Condicional (Se... Então)

O condicional é expresso por **→**, e lê-se **"Se p, então q"**. A condicional só é falsa quando a condição (**p**) é verdadeira, mas o resultado (**q**) é falso. Nos demais casos, ela é considerada verdadeira.

Exemplo:  
**p = "Gosto de jogar bola"**  
**q = "Gosto de estudar"**  
**p → q = "Se gosto de jogar bola, então gosto de estudar"**

|p|q|p → q|
|---|---|---|
|V|V|V|
|V|F|F|
|F|V|V|
|F|F|V|

#### Bicondicional (Se e Somente Se)

A bicondicional, representada por **↔**, estabelece que duas proposições são verdadeiras ou falsas **juntas**. Ou seja, só é verdadeira quando ambas possuem o mesmo valor lógico.

Exemplo:  
**p = "Gosto de jogar bola"**  
**q = "Gosto de estudar"**  
**p ↔ q = "Gosto de jogar bola se, e somente se, gosto de estudar"**

|p|q|p ↔ q|
|---|---|---|
|V|V|V|
|V|F|F|
|F|V|F|
|F|F|V|

### Quantidade de Linhas em uma Tabela Verdade

Existe uma fórmula simples que nos diz quantas linhas uma tabela verdade terá. Basta calcular **2 elevado ao número de proposições (n)**:

$$\text{Linhas} = 2^n$$

Por exemplo:

- Para 1 proposição: $21=22^1 = 2$ linhas.
- Para 2 proposições: $22=42^2 = 4$ linhas.
- Para 3 proposições: $23=82^3 = 8$ linhas.

### Tautologias, Contradições e Contingências

Uma **tautologia** ocorre quando uma proposição composta é **sempre verdadeira**, independentemente dos valores das proposições simples.

Exemplo clássico:  
**p ∨ ¬p** → "Gosto de estudar ou não gosto de estudar" → sempre verdadeiro.

|p|¬p|p ∨ ¬p|
|---|---|---|
|V|F|V|
|F|V|V|

Uma **contradição** ocorre quando uma proposição composta é **sempre falsa**, qualquer que seja o valor das proposições simples.

Exemplo clássico:  
**p ∧ ¬p** → "Gosto de estudar e não gosto de estudar" → sempre falso.

|p|¬p|p ∧ ¬p|
|---|---|---|
|V|F|F|
|F|V|F|

Uma **contingência** é uma proposição que pode ser verdadeira ou falsa, dependendo dos valores das proposições que a compõem.

Exemplo:  
**p ∧ q** → "Gosto de estudar e gosto de viajar"

|p|q|p ∧ q|
|---|---|---|
|V|V|V|
|V|F|F|
|F|V|F|
|F|F|F|

### Equivalência Lógica

Duas proposições são **equivalentes** quando possuem tabelas verdade idênticas. Ou seja, para qualquer combinação de valores das proposições simples, o valor da proposição composta é o mesmo.

Um exemplo clássico é a equivalência entre:  

**p → q**: “Se gosto de jogar bola, então gosto de estudar”
**¬p ∨ q**: “Não gosto de jogar bola ou gosto de estudar”

**p → q** ↔ **¬p ∨ q**

Vamos comparar suas tabelas:

|p|q|p → q|¬p|¬p ∨ q|
|---|---|---|---|---|
|V|V|V|F|V|
|V|F|F|F|F|
|F|V|V|V|V|
|F|F|V|V|V|

Perceba que a coluna de **p → q** e a de **¬p ∨ q** possuem exatamente os mesmos valores. Portanto, são logicamente equivalentes.


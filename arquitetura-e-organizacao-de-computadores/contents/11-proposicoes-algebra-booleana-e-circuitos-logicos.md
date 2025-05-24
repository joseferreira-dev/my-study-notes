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

## Álgebra Booleana

A Álgebra Booleana é uma das ferramentas matemáticas mais importantes no universo da Arquitetura e Organização de Computadores. Ela é a base teórica que permite projetar, analisar e construir circuitos digitais. Tudo aquilo que, no tópico anterior, estudamos sob a ótica da lógica proposicional — com proposições, conectivos e tabelas verdade —, agora ganha uma aplicação prática, direta, no funcionamento físico dos circuitos eletrônicos que compõem os processadores e demais dispositivos digitais.

Mas afinal, o que é uma Álgebra Booleana? Trata-se de uma estrutura algébrica composta por um conjunto de elementos e operações que seguem regras específicas. Essas operações são, em essência, versões matemáticas da lógica que usamos no dia a dia — como os conceitos de **"e"**, **"ou"** e **"não"**.

Formalmente, uma Álgebra Booleana é definida como uma **6-upla (X, ∨, ∧, ¬, 0, 1)**, ou seja, um conjunto **X** acompanhado de duas operações binárias (**∨ e ∧**), uma operação unária (**¬**) e dois elementos especiais (**0 e 1**). Vamos compreender cada um desses elementos.

### Operações e Elementos Fundamentais da Álgebra Booleana

A Álgebra Booleana possui três operações fundamentais:

- A **disjunção** (ou lógico), representada por **∨**, por **+**, ou pela palavra **OR**.
- A **conjunção** (e lógico), representada por **∧**, por **.**, por **∗**, ou pela palavra **AND**.
- A **negação**, representada por **¬**, **~**, ou um traço sobre a variável, equivalente ao **NOT**.

Além das operações, existem dois elementos fundamentais:

- O elemento **0**, que representa o valor lógico **Falso** (ou **False**).
- O elemento **1**, que representa o valor lógico **Verdadeiro** (ou **True**).

Esses elementos e operações obedecem a um conjunto de propriedades e regras, chamadas de **axiomas da Álgebra Booleana**, que garantem o seu funcionamento lógico e coerente.

### Axiomas da Álgebra Booleana

Vamos explorar os principais axiomas que regem a Álgebra Booleana, sempre acompanhando com exemplos práticos que facilitam a compreensão.

**Propriedades Associativas:** O agrupamento dos elementos não altera o resultado, seja na operação de **ou** (**∨**) ou de **e** (**∧**).

$$(a ∨ b) ∨ c = a ∨ (b ∨ c)$$
$$(a ∧ b) ∧ c = a ∧ (b ∧ c)$$

Exemplo:  Se a = 1, b = 0 e c = 1, então:

$$(1 ∨ 0) ∨ 1 = 1 ∨ (0 ∨ 1) = 1$$

**Propriedades Comutativas:** A ordem dos operandos não altera o resultado.

$$a ∨ b = b ∨ a$$
$$a ∧ b = b ∧ a$$

Exemplo:

$$1 ∧ 0 = 0 ∧ 1 = 0$$
$$1 ∨ 0 = 0 ∨ 1 = 1$$

**Propriedades Absortivas:** Uma operação entre uma variável e ela mesma combinada com outra não altera o valor da variável.

$$a ∧ (a ∨ b) = a$$
$$a ∨ (a ∧ b) = a$$

Exemplo: Se a = 1 e b = 0:

$$1 ∧ (1 ∨ 0) = 1$$
$$1 ∨ (1 ∧ 0) = 1$$

**Propriedades Distributivas:** Assim como na matemática convencional, as operações podem ser distribuídas.

$$a ∨ (b ∧ c) = (a ∨ b) ∧ (a ∨ c)$$
$$a ∧ (b ∨ c) = (a ∧ b) ∨ (a ∧ c)$$

Exemplo:  Se a = 1, b = 0 e c = 1:

$$1 ∧ (0 ∨ 1) = (1 ∧ 0) ∨ (1 ∧ 1) → 1 = 0 ∨ 1 → 1$$

**Elementos Neutros:** Existe um elemento que, quando usado na operação, não altera o resultado.

$$a ∨ 0 = a$$
$$a ∧ 1 = a$$

Exemplo:

$$1 ∨ 0 = 1$$
$$0 ∧ 1 = 0$$

**Elementos Complementares:** Todo elemento tem seu oposto (complemento), e aplicando operações com ele gera resultados extremos (Verdadeiro ou Falso).

$$a ∨ ¬a = 1$$
$$a ∧ ¬a = 0$$

Exemplo:

$$1 ∨ ¬1 = 1 ∨ 0 = 1$$
$$0 ∧ ¬0 = 0 ∧ 1 = 0$$

### Teoremas Fundamentais da Álgebra Booleana

A partir dos axiomas, podemos derivar teoremas importantes, muito utilizados tanto na simplificação de expressões lógicas quanto na construção de circuitos.

**Idempotência:** Aplicar a mesma operação sobre o mesmo valor não altera o valor.

$$a ∨ a = $$
$$a ∧ a = a$$

Exemplo:

$$1 ∨ 1 = 1$$
$$0 ∧ 0 = 0$$

**Dupla Negação:** Negar duas vezes traz o valor original.

$$¬(¬a) = a$$

Exemplo: Se a = 1, então

$$¬(¬1) = ¬0 = 1$$

**Leis de De Morgan:** Importantíssimas tanto na lógica formal quanto na engenharia de computadores. Elas descrevem como a negação de uma conjunção se transforma em uma disjunção das negações, e vice-versa.

$$¬(a ∨ b) = ¬a ∧ ¬b$$
$$¬(a ∧ b) = ¬a ∨ ¬b$$

Exemplo: Se a = 1 e b = 0:

$$¬(1 ∨ 0) = ¬1 ∧ ¬0 → 0 ∧ 1 = 0$$
$$¬(1 ∧ 0) = ¬1 ∨ ¬0 → 0 ∨ 1 = 1$$

**Absorção:** Permite reduzir expressões complexas.

$$a ∨ (a ∧ b) = a$$
$$a ∧ (a ∨ b) = a$$

Exemplo: Se a = 1, b = 0:

$$1 ∨ (1 ∧ 0) = 1 ∨ 0 = 1$$
$$1 ∧ (1 ∨ 0) = 1 ∧ 1 = 1$$

**Elementos Absorventes:** Algumas combinações anulam ou afirmam diretamente o resultado.

$$a ∨ 1 = 1$$
$$a ∧ 0 = 0$$

Exemplo:

$$1 ∨ 1 = 1$$
$$0 ∧ 1 = 0$$

**Negações de Zero e Um:**

$$¬0 = 1$$
$$¬1 = 0$$

### Definição Formal do XOR na Álgebra Booleana

O operador **XOR** (disjunção exclusiva), que vimos na lógica proposicional, também tem sua definição formal na Álgebra Booleana. Ele pode ser expresso de duas formas equivalentes:

$$a ⊕ b = (a ∨ b) ∧ (¬a ∨ ¬b)$$
$$a ⊕ b = (a ∧ ¬b) ∨ (¬a ∧ b)$$

Ambas as expressões significam o mesmo: o resultado será verdadeiro quando **a** e **b** forem diferentes.

Exemplo com a = 0, b = 0. Primeira definição:

$$(0 ∨ 0) ∧ (¬0 ∨ ¬0) → 0 ∧ (1 ∨ 1) → 0 ∧ 1 = 0$$

Segunda definição:

$$(0 ∧ ¬0) ∨ (¬0 ∧ 0) → (0 ∧ 1) ∨ (1 ∧ 0) → 0 ∨ 0 = 0$$

O mesmo pode ser feito para qualquer combinação de valores.

### Conexão direta com os Circuitos Lógicos

Todo este conjunto de regras e operações que a Álgebra Booleana estabelece não é apenas uma abstração matemática, mas sim a essência do funcionamento dos circuitos digitais. As portas lógicas físicas — como AND, OR, NOT, XOR — implementam essas operações diretamente no hardware, utilizando sinais elétricos que representam os valores 0 e 1. Ou seja, compreender a Álgebra Booleana é um passo essencial para compreender os circuitos que executam as instruções em um computador.

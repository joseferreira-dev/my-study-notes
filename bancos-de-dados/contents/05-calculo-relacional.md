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

Existem formas alternativas de escrever a mesma expressão lógica, que podem ser encontradas em diferentes literaturas. Uma sintaxe comum é:

`{ t.nome | Estudantes(t) ∧ t.idade = 20 }`

Nesta notação, `Estudantes(t)` é usado em vez de `t ∈ Estudantes`, mas o significado é idêntico: "`t` é uma tupla da relação `Estudantes`". O resultado da consulta permanece exatamente o mesmo. A escolha da sintaxe é uma questão de convenção, mas a lógica subjacente do predicado não se altera.

### A Sintaxe e os Operadores do CRT

Como o Cálculo Relacional de Tuplas é derivado da lógica de predicados, ele utiliza uma série de símbolos lógicos para construir suas expressões. Embora uma consulta em CRT pareça complexa à primeira vista, ela se torna mais clara quando compreendemos o papel de cada operador. A familiaridade com esses símbolos é essencial, pois as questões teóricas frequentemente se baseiam neles.

Antes de analisarmos as consultas, é importante consolidar o significado dos principais símbolos da lógica proposicional que encontraremos.

|Símbolo|Nome|Descrição|
|---|---|---|
|**∧**, `AND`|Conjunção (E)|Verdadeiro se ambas as proposições forem verdadeiras.|
|**∨**, `OR`|Disjunção (OU)|Verdadeiro se pelo menos uma das proposições for verdadeira.|
|**¬**, `~`, `NOT`|Negação (NÃO)|Inverte o valor de verdade da proposição.|
|**→**|Implicação (Se... Então)|Verdadeiro, exceto quando a primeira proposição é verdadeira e a segunda é falsa.|
|**↔**|Bicondicional (Se e Somente Se)|Verdadeiro se ambas as proposições forem iguais (ambas verdadeiras ou ambas falsas).|
|**⊤**|Verdadeiro (Tautologia)|Uma proposição que é sempre verdadeira.|
|**⊥**|Falso (Contradição)|Uma proposição que é sempre falsa.|
|**∃**|Quantificador Existencial (Existe)|Afirma que **pelo menos um** elemento no domínio satisfaz a proposição.|
|**∀**|Quantificador Universal (Para Todo)|Afirma que **todos** os elementos no domínio satisfazem a proposição.|

#### Os Quantificadores: Existencial (∃) e Universal (∀)

Os quantificadores são os elementos que dão grande poder expressivo ao Cálculo Relacional.

- O **Quantificador Existencial (∃)** é usado para verificar se "existe pelo menos um". Em consultas a bancos de dados, ele é a base para expressar **junções**, pois nos permite verificar a existência de uma tupla correspondente em outra tabela que satisfaça uma condição.
- O **Quantificador Universal (∀)** é usado para verificar uma condição "para todos". É a base para expressar consultas de **divisão**, que buscam tuplas que se relacionam com todo um conjunto de outras tuplas.

### Exemplos de Consultas em CRT com Dados Reais

Para tornar as consultas em CRT mais tangíveis, vamos definir um conjunto de tabelas e dados que utilizaremos em nossos exemplos.

**Tabela EMPREGADO**

| NSS | NOME | SOBRENOME | SALARIO | DATANASCIMENTO | ENDERECO | NUD |
| --- | --- | --- | --- | --- | --- | --- |
| 111 | Ana | Souza | 8000 | 1985-03-10 | Rua A, 1 | 10 |
| 222 | Bruno| Costa | 3200 | 1990-07-22 | Rua B, 2 | 20 |
| 333 | Carlos| Lima | 4500 | 1988-11-05 | Rua C, 3 | 10 |
| 444 | Diana| Silva | 2800 | 1995-01-30 | Rua D, 4 | 20 |

**Tabela DEPARTAMENTO**

| NUMERODEP | NOMED | NSSGER |
| --- | --- | --- |
| 10 | Informática | 111 |
| 20 | Marketing | 555 |

**Tabela PROJETO**

| NUMEROP | LOCALIZACAO | NUMD |
| --- | --- | --- |
| 101 | São Paulo | 10 |
| 102 | Rio de Janeiro| 10 |
| 103 | São Paulo | 20 |

Agora, vamos reanalisar as expressões de CRT, aplicando-as a estas tabelas e observando o resultado.

**Exemplo 1: Seleção Simples**

- **Objetivo:** Encontrar todas as informações dos empregados cujo salário é maior que R$ 3.500.
- **Expressão CRT:** `{ t | EMPREGADO(t) ∧ t.SALARIO > 3500 }`
- **Análise Lógica:** A consulta busca por todas as tuplas `t` que pertencem à relação `EMPREGADO` e que, adicionalmente (`∧`), satisfazem a condição de que o valor de seu atributo `SALARIO` seja maior que 3500.
    1. A variável `t` assume a primeira linha (Ana). `t.SALARIO` é 8000. `8000 > 3500` é verdadeiro. A tupla é incluída.
    2. A variável `t` assume a segunda linha (Bruno). `t.SALARIO` é 3200. `3200 > 3500` é falso. A tupla é descartada.
    3. A variável `t` assume a terceira linha (Carlos). `t.SALARIO` é 4500. `4500 > 3500` é verdadeiro. A tupla é incluída.
    4. A variável `t` assume a quarta linha (Diana). `t.SALARIO` é 2800. `2800 > 3500` é falso. A tupla é descartada.

- **Resultado:**

| NSS | NOME | SOBRENOME | SALARIO | DATANASCIMENTO | ENDERECO | NUD |
| --- | --- | --- | --- | --- | --- | --- |
| 111 | Ana | Souza | 8000 | 1985-03-10 | Rua A, 1 | 10 |
| 333 | Carlos| Lima | 4500 | 1988-11-05 | Rua C, 3 | 10 |

**Exemplo 2: Consulta com Junção (Quantificador Existencial)**

- **Objetivo:** Listar o nome, sobrenome e endereço de todos os empregados que trabalham no departamento de "Informática".
- **Expressão CRT:** `{ t.NOME, t.SOBRENOME, t.ENDERECO | EMPREGADO(t) ∧ (∃ d) (DEPARTAMENTO(d) ∧ d.NOMED = 'Informática' ∧ d.NUMERODEP = t.NUD) }`
- **Análise Lógica:** A consulta busca pelos atributos `NOME`, `SOBRENOME` e `ENDERECO` de cada tupla `t` da tabela `EMPREGADO`, mas apenas se a condição após o `∧` for verdadeira. A condição `(∃ d) (...)` exige que **exista pelo menos uma** tupla `d` na tabela `DEPARTAMENTO` que se relacione com a tupla `t` do empregado e que seja do departamento de 'Informática'.
    1. **Para `t` = Ana (NUD=10):** Existe uma tupla `d` em `DEPARTAMENTO` onde `d.NUMERODEP` é 10? Sim, a primeira linha. Para essa linha, `d.NOMED` é 'Informática'? Sim. A condição é satisfeita. Os dados de Ana são incluídos.
    2. **Para `t` = Bruno (NUD=20):** Existe uma tupla `d` em `DEPARTAMENTO` onde `d.NUMERODEP` é 20? Sim, a segunda linha. Para essa linha, `d.NOMED` é 'Informática'? Não, é 'Marketing'. A condição falha. Os dados de Bruno são descartados.
    3. **Para `t` = Carlos (NUD=10):** A mesma lógica de Ana se aplica. A condição é satisfeita. Os dados de Carlos são incluídos.
    4. **Para `t` = Diana (NUD=20):** A mesma lógica de Bruno se aplica. A condição falha. Os dados de Diana são descartados.

- **Resultado:**

| NOME | SOBRENOME | ENDERECO |
| --- | --- | --- |
| Ana | Souza | Rua A, 1 |
| Carlos| Lima | Rua C, 3 |

**Exemplo 3: Consulta com Múltiplas Junções**

- **Objetivo:** Para cada projeto localizado em ‘São Paulo’, listar o número do projeto, o número de seu departamento, e o sobrenome, a data de nascimento e o endereço do gerente daquele departamento.
- **Expressão CRT:** `{ p.NUMEROP, p.NUMD, m.SOBRENOME, m.DATANASCIMENTO, m.ENDERECO | PROJETO(p) ∧ EMPREGADO(m) ∧ p.LOCALIZACAO = 'São Paulo' ∧ ((∃ d) (DEPARTAMENTO(d) ∧ p.NUMD = d.NUMERODEP ∧ d.NSSGER = m.NSS)) }`
- **Análise Lógica:** Esta consulta é mais complexa. Ela busca uma combinação de atributos de uma tupla `p` (de `PROJETO`) e uma tupla `m` (de `EMPREGADO`).
    1. A consulta começa filtrando as tuplas `p` de `PROJETO` onde `p.LOCALIZACAO = 'São Paulo'`. As tuplas que satisfazem são as com `NUMEROP` 101 e 103.
    2. Para cada uma dessas tuplas de projeto, a consulta verifica a condição existencial `(∃ d) (...)`.
    3. **Para `p` = Projeto 101 (NUMD=10):** Existe uma tupla `d` em `DEPARTAMENTO` onde `d.NUMERODEP = 10`? Sim, a primeira (`Informática`, `NSSGER=111`). Agora, a consulta procura uma tupla `m` em `EMPREGADO` onde `m.NSS = 111`. Existe? Sim, a tupla de Ana Souza. A condição inteira é satisfeita para a combinação da tupla do Projeto 101 e da tupla da Empregada Ana. Uma linha é adicionada ao resultado.
    4. **Para `p` = Projeto 103 (NUMD=20):** Existe uma tupla `d` em `DEPARTAMENTO` onde `d.NUMERODEP = 20`? Sim, a segunda (`Marketing`, `NSSGER=555`). Agora, a consulta procura uma tupla `m` em `EMPREGADO` onde `m.NSS = 555`. Existe? Não, não há nenhum empregado com esse NSS em nossa tabela. A condição falha. Nenhuma linha é adicionada ao resultado para o Projeto 103.

- **Resultado:**

| NUMEROP | NUMD | SOBRENOME | DATANASCIMENTO | ENDERECO |
| ------- | ---- | --------- | -------------- | -------- |
| 101     | 10   | Souza     | 1985-03-10     | Rua A, 1 |


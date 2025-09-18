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

## Cálculo Relacional de Domínio (CRD)

O **Cálculo Relacional de Domínio (CRD)** é a outra grande formalização do cálculo relacional. Assim como o CRT, é uma linguagem de consulta declarativa e não-procedural. A diferença fundamental entre eles reside no tipo de variável que utilizam: enquanto o CRT usa variáveis que representam tuplas inteiras, o CRD utiliza **variáveis de domínio**, que representam **valores individuais** de um atributo.

Em uma consulta CRD, em vez de buscar por "linhas que satisfazem uma condição", nós buscamos por "combinações de valores que satisfazem uma condição". O resultado da consulta é um conjunto de valores (ou combinações de valores) que atendem aos predicados lógicos definidos.

A sintaxe geral de uma consulta CRD é:

`{ <v1, v2, ..., vn> | P(v1, v2, ..., vn) }`

Onde:

- **`<v1, v2, ..., vn>`** é uma lista de variáveis de domínio. É a cláusula de alvo, que define quais valores aparecerão no resultado.
- **`P(v1, v2, ..., vn)`** é o predicado, uma fórmula que descreve as condições que os valores das variáveis devem satisfazer.

### Exemplo Introdutório de CRD

Vamos usar uma tabela `ALUNOS` para ilustrar o funcionamento do CRD.

**Tabela ALUNOS**

| matricula | nome | idade |
| --- | --- | --- |
| 101 | João | 20 |
| 102 | Maria | 22 |
| 103 | Pedro | 19 |
| 104 | Ana | 21 |

**Objetivo:** Recuperar o nome dos alunos com 20 anos ou mais.

**Expressão CRD:** `{ <N> | (∃ M, I) (ALUNOS(M, N, I) ∧ I ≥ 20) }`

Vamos dissecar esta expressão:

- **`{ <N> | ... }`**: A cláusula de alvo. Queremos retornar apenas os valores da variável de domínio `N`.
- **`(∃ M, I)`**: O quantificador existencial. Afirma que **existem** variáveis `M` e `I`. Elas são necessárias para compor a tupla completa da relação `ALUNOS`, mesmo que não apareçam no resultado final.
- **`ALUNOS(M, N, I)`**: Este é o predicado de pertencimento. Ele afirma que deve existir uma tupla na relação `ALUNOS` onde os valores dos atributos correspondam às variáveis `M`, `N` e `I`. É aqui que as variáveis são "ligadas" às colunas da tabela. **A ordem é crucial**: `M` é associado à primeira coluna (`matricula`), `N` à segunda (`nome`), e `I` à terceira (`idade`).
- **`∧ I ≥ 20`**: A condição de filtragem. Ela restringe o resultado, exigindo que o valor da variável `I` (idade) seja maior ou igual a 20.

A expressão completa é lida como: "Retorne o conjunto de todos os valores de `N` para os quais existem valores `M` e `I` tal que existe uma tupla `(M, N, I)` na relação `ALUNOS` E o valor de `I` é maior ou igual a 20".

O resultado seria:

| nome |
| --- |
| João |
| Maria |
| Ana |

É uma boa prática usar iniciais que correspondam aos nomes dos atributos para facilitar a leitura (como N para nome, I para idade), mas qualquer letra poderia ser usada, desde que a associação posicional em `ALUNOS(M, N, I)` seja mantida.

#### Exemplos Avançados de CRD

Para consultas mais complexas, vamos usar duas novas tabelas.

**Tabela PRODUTOS**

| ID_PRODUTO | NOME | PRECO |
| --- | --- | --- |
| 1 | Televisão | 2000 |
| 2 | Geladeira | 3000 |
| 3 | Microondas | 500 |
| 4 | Máquina de Lavar | 2500 |

**Tabela VENDEDORES**

| ID_VENDEDOR| NOME | ID_PRODUTO |
| --- | --- | --- |
| 101 | Carlos | 1 |
| 102 | Fernanda | 2 |
| 103 | João | 3 |
| 104 | Maria | 4 |

**Exemplo 1: Junção e Filtragem**

- **Objetivo:** Obter o nome do produto e o nome do vendedor para todos os produtos com preço superior a R$ 1.000.
- **Expressão CRD:** `{ <Np, Nv> | (∃ P, Pr, V) (PRODUTOS(P, Np, Pr) ∧ VENDEDORES(V, Nv, P) ∧ Pr > 1000) }`
- **Análise:**
    - `{ <Np, Nv> }`: O alvo do resultado são os valores das variáveis `Np` (nome do produto) e `Nv` (nome do vendedor).
    - `(∃ P, Pr, V)`: Declara a existência das variáveis `P` (ID do produto), `Pr` (preço) e `V` (ID do vendedor).
    - `PRODUTOS(P, Np, Pr)`: Associa as variáveis a uma tupla na tabela `PRODUTOS`.
    - `∧ VENDEDORES(V, Nv, P)`: Associa as variáveis a uma tupla na tabela `VENDEDORES`. A variável `P` é usada em ambos os predicados, criando a condição de junção implícita (`PRODUTOS.ID_PRODUTO = VENDEDORES.ID_PRODUTO`).
    - `∧ Pr > 1000`: Filtra o resultado, mantendo apenas as combinações onde o preço (`Pr`) é maior que 1000.

- **Resultado:**

| NOME_PRODUTO | NOME_VENDEDOR |
| --- | --- |
| Televisão | Carlos |
| Geladeira | Fernanda |
| Máquina de Lavar | Maria |

**Exemplo 2: Junção e Filtro de Texto**

- **Objetivo:** Obter o nome dos produtos que são vendidos por um vendedor cujo nome começa com a letra "C".
- **Expressão CRD:** `{ <Np> | (∃ P, Pr, V, Nv) (PRODUTOS(P, Np, Pr) ∧ VENDEDORES(V, Nv, P) ∧ Nv LIKE 'C%') }`
- **Análise:**
    - `{ <Np> }`: O alvo do resultado é apenas o nome do produto.
    - A junção entre `PRODUTOS` e `VENDEDORES` é estabelecida da mesma forma que no exemplo anterior.
    - `∧ Nv LIKE 'C%'`: A condição de filtragem utiliza o operador `LIKE` sobre a variável `Nv` (nome do vendedor) para selecionar apenas aqueles que começam com "C". Apenas o vendedor "Carlos" satisfaz essa condição. Ele vende o produto de ID 1, que é a "Televisão".

- **Resultado:**

| NOME_PRODUTO |
| --- |
| Televisão |

## Considerações Finais

Neste capítulo, exploramos a outra grande base teórica das linguagens de consulta para bancos de dados relacionais: o **Cálculo Relacional**. Em contraste direto com a Álgebra Relacional, que possui uma natureza procedural e nos guia através do "como" obter um resultado, o Cálculo Relacional nos introduziu à filosofia **declarativa**, focada em especificar "o que" desejamos como resultado.

Aprofundamos nas duas principais vertentes deste formalismo. Primeiramente, o **Cálculo Relacional de Tuplas (CRT)**, onde aprendemos a construir consultas utilizando variáveis que representam tuplas inteiras, descrevendo as propriedades das linhas que buscamos. Em seguida, exploramos o **Cálculo Relacional de Domínio (CRD)**, que adota uma abordagem mais granular, utilizando variáveis que representam valores individuais dos atributos para definir as combinações que devem compor o resultado.

Ao analisar os exemplos, vimos como conceitos da lógica de predicados, como os quantificadores existencial (∃) e universal (∀), são utilizados para expressar operações complexas como junções e divisões de uma forma puramente descritiva.

A principal lição deste capítulo é a compreensão da dualidade entre as abordagens procedural e declarativa. Ambas as linguagens formais, Álgebra e Cálculo Relacional, são equivalentes em seu poder de expressão — o que pode ser feito em uma, pode ser feito na outra. No entanto, é a filosofia declarativa do Cálculo Relacional que serve como a inspiração direta para a sintaxe e a usabilidade da linguagem SQL, onde descrevemos o resultado que queremos, deixando para o SGBD a tarefa de determinar os passos para alcançá-lo.

Com o estudo da Álgebra e do Cálculo Relacional concluído, temos agora a visão teórica completa que fundamenta as operações de consulta. Estamos prontos para avançar e aplicar este conhecimento para dominar as nuances e as capacidades avançadas da linguagem SQL na prática.
# Capítulo 6 – Linguagem SQL: Cláusulas

Nos capítulos anteriores, estabelecemos um alicerce sólido sobre os fundamentos dos bancos de dados e aprendemos a estrutura básica dos comandos SQL, divididos em suas sublinguagens (DDL, DML, DQL, DCL e TCL). Agora, vamos avançar do "o que" cada comando faz para "como" podemos refinar e controlar seu comportamento. É aqui que entram as **cláusulas**.

As cláusulas são componentes essenciais das instruções SQL que atuam como modificadores, permitindo-nos filtrar, ordenar, agrupar e juntar dados com grande precisão. Se os comandos `SELECT`, `UPDATE` ou `DELETE` são o motor de uma consulta, as cláusulas são o volante, o acelerador e os freios — as ferramentas que nos dão controle total sobre o resultado.

Embora sejam mais proeminentes e utilizadas com maior frequência nas sublinguagens DML e DQL, as cláusulas são elementos da linguagem SQL como um todo e podem ser aplicadas em diferentes contextos. A divisão das sublinguagens, como já mencionado, é uma ferramenta didática; na prática, o SGBD interpreta tudo como uma única e coesa linguagem SQL.

Neste capítulo, exploraremos as principais cláusulas, agrupando-as por seu objetivo na construção de uma instrução para facilitar o entendimento.

## A Base das Condições: Operadores

Antes de mergulharmos nas cláusulas propriamente ditas, precisamos entender seus componentes mais atômicos e fundamentais: os **operadores**. Os operadores são símbolos ou palavras-chave que realizam operações específicas sobre os dados, sejam elas matemáticas, de comparação ou lógicas. São eles que formam as expressões e condições que utilizamos dentro de cláusulas como a `WHERE`, constituindo a base para a aplicação de lógica, estatísticas e cálculos em nossas consultas.

Para fins de estudo, podemos classificar os principais operadores em três grandes grupos:

- **Operadores Matemáticos**
- **Operadores Lógicos**
- **Funções de Agregação**

Vamos explorar cada um desses grupos em detalhe.

### Operadores Matemáticos

Os operadores matemáticos são símbolos e funções que permitem a execução de operações aritméticas sobre valores numéricos dentro de uma instrução SQL. Eles são a base para a criação de colunas calculadas, para a aplicação de lógicas de negócio em filtros e para a realização de análises quantitativas diretamente no banco de dados.

Sua utilização é bastante intuitiva, pois segue a mesma lógica da matemática básica que já conhecemos. A tabela a seguir resume os principais operadores.

| Símbolo                | Operador      | Exemplo                                    |
| ---------------------- | ------------- | ------------------------------------------ |
| **+**                  | Soma          | `Preco + Frete`                            |
| **-**                  | Subtração     | `Preco - Custo`                            |
| *****                  | Multiplicação | `Quantidade * Valor_Unitario`              |
| **/**                  | Divisão       | `Total_Vendas / Numero_Vendedores`         |
| **%**                  | Módulo        | `ID_Produto % 2`                           |
| **`POW()`** ou **`^`** | Potenciação   | `POW(Base, Expoente)` ou `Base ^ Expoente` |
| **`SQRT()`**           | Raiz Quadrada | `SQRT(Area)`                               |

#### Operadores Aritméticos

Os quatro operadores básicos (`+`, `-`, `*`, `/`) são frequentemente usados na cláusula `SELECT` para criar novas colunas virtuais no resultado da consulta.

**Exemplo Prático:** Em uma tabela `PRODUTOS` com as colunas `NOME`, `PRECO` e `CUSTO`, podemos calcular o lucro de cada produto em tempo real.

```sql
SELECT 
	NOME, 
	PRECO, 
	CUSTO, 
	(PRECO - CUSTO) AS LUCRO
FROM PRODUTOS;
```

#### Operadores e Funções Especiais

Alguns operadores e funções merecem uma atenção especial.

- **Módulo (`%`):** Esta operação não retorna o resultado da divisão, mas sim o **resto** da divisão inteira entre dois números.
    - `10 % 3` resultaria em `1`, pois 10 dividido por 3 é 3, com resto 1.
    - `10 % 2` resultaria em `0`, pois 10 dividido por 2 é 5, com resto 0.
    - **Uso Prático:** O operador de módulo é extremamente útil para verificar se um número é par ou ímpar. Um número é par se o resto de sua divisão por 2 for 0. A consulta `SELECT * FROM Alunos WHERE ID_Aluno % 2 = 0;` retornaria todos os alunos com ID par.

- **Potenciação (`POW()` ou `^`):** Utilizada para elevar um número a uma potência. A sintaxe pode variar entre os SGBDs. A notação `A ^ B` (A elevado a B) é comum, mas a forma mais padronizada e segura é usar a função `POW(base, expoente)`. Os valores dentro dos parênteses são chamados de **parâmetros** da função.
    - **Exemplo:** Para calcular 4³, a expressão seria `POW(4, 3)`.

- **Raiz Quadrada (`SQRT()`):** É uma função que recebe um único parâmetro e retorna sua raiz quadrada.
    - **Exemplo:** Para encontrar a raiz quadrada de 16, a expressão seria `SQRT(16)`, que resultaria em 4.

#### Um Caso Especial: O Operador de Soma com Textos (Concatenação)

Alguns SGBDs, como o Microsoft SQL Server, sobrecarregam o operador de soma (`+`) para que ele também possa operar com textos (_strings_). Nesse contexto, ele não realiza uma soma matemática, mas sim uma **concatenação**, que é a junção de duas ou mais strings em uma única string resultante.

- **Exemplo:** Se tivermos as colunas `Nome` com o valor "João" e `Sobrenome` сom o valor "Silva", a expressão `Nome + ' ' + Sobrenome` resultaria na string "João Silva".

É crucial saber que este comportamento **não é universal**. Muitos outros SGBDs, como Oracle e PostgreSQL, utilizam o operador de barra dupla (`||`) para a concatenação. A forma mais padronizada e portável entre diferentes bancos de dados é utilizar a função `CONCAT()`.

- **Sintaxe SQL Server:** `SELECT Nome + ' ' + Sobrenome FROM Clientes;`
- **Sintaxe Oracle/PostgreSQL:** `SELECT Nome || ' ' || Sobrenome FROM Clientes;`
- **Sintaxe com Função Padrão:** `SELECT CONCAT(Nome, ' ', Sobrenome) FROM Clientes;`

### Operadores Lógicos

Os operadores lógicos são as ferramentas que utilizamos para combinar ou modificar condições, principalmente dentro da cláusula `WHERE`, permitindo a criação de filtros complexos e precisos. Eles operam com base na lógica booleana, que lida com valores de **Verdadeiro** e **Falso**. Internamente, os computadores frequentemente representam esses valores de forma binária, onde `1` pode representar Verdadeiro e `0`, Falso.

Para o nosso estudo, não é necessário um aprofundamento em lógica matemática, mas é essencial revisar os três conceitos fundamentais que possuem operadores correspondentes diretos na linguagem SQL.

- **Conjunção (E):** Uma expressão de conjunção, como `A E B`, só é considerada verdadeira se **todas** as suas condições (`A` e `B`) forem, individualmente, verdadeiras. Se qualquer uma delas for falsa, a expressão inteira se torna falsa.
- **Disjunção (OU):** Uma expressão de disjunção, como `A OU B`, é considerada verdadeira se **pelo menos uma** de suas condições (`A` ou `B`) for verdadeira. Ela só será falsa se todas as condições forem falsas.
- **Negação (NÃO):** A negação, representada por `NÃO A`, simplesmente inverte o valor lógico de uma condição. O que era verdadeiro se torna falso, e o que era falso se torna verdadeiro.

A linguagem SQL implementa esses conceitos através dos seguintes operadores:

|Operador|Operação Lógica|Exemplo de Uso|
|---|---|---|
|**`AND`**|Conjunção (E)|`Preco > 100 AND Estoque > 0`|
|**`OR`**|Disjunção (OU)|`Cidade = 'São Paulo' OR Cidade = 'Recife'`|
|**`NOT`**|Negação (NÃO)|`NOT Pais = 'Brasil'`|

#### Uso Prático

O operador `AND` é utilizado para criar filtros que exigem que múltiplas condições sejam satisfeitas simultaneamente.

**Exemplo Prático:** Selecionar os produtos que são da categoria 'Notebook' **E** que custam menos de R$ 3.000.

```sql
SELECT Nome, Preco
FROM Produtos
WHERE Categoria = 'Notebook' AND Preco < 3000;
```

Apenas os registros que atenderem a **ambas as condições** serão retornados. Um notebook de R$ 3.500 não apareceria, nem um smartphone de R$ 2.000.

O operador `OR` é utilizado para criar filtros mais abrangentes, onde basta que uma das várias condições seja satisfeita.

**Exemplo Prático:** Selecionar os clientes que moram na cidade de 'Recife' **OU** que possuem um limite de crédito superior a R$ 5.000.

```sql
SELECT Nome, Cidade, LimiteCredito
FROM Clientes
WHERE Cidade = 'Recife' OR LimiteCredito > 5000;
```

O resultado incluirá todos os clientes de Recife (independentemente do limite de crédito) e também todos os clientes de qualquer outra cidade que tenham um limite de crédito alto.

O operador `NOT` é usado para inverter uma condição, selecionando todos os registros que **não** atendem ao critério especificado.

**Exemplo Prático:** Selecionar todos os funcionários que **NÃO** são do departamento de 'Vendas'.

```sql
SELECT Nome, Departamento
FROM Funcionarios
WHERE NOT Departamento = 'Vendas';
```

Este comando é funcionalmente equivalente a `WHERE Departamento <> 'Vendas'`, mas o `NOT` se torna mais poderoso ao negar expressões complexas.

#### Ordem de Precedência

Quando uma expressão na cláusula `WHERE` contém múltiplos operadores lógicos, o SGBD não os avalia simplesmente da esquerda para a direita. Existe uma ordem de precedência fixa para garantir que as expressões sejam resolvidas de forma consistente. A ordem é:

**1º `NOT` >> 2º `AND` >> 3º `OR`**

Essa ordem é extremamente importante. Considere a seguinte consulta:

```sql
SELECT * FROM Produtos WHERE Categoria = 'Notebook' OR Categoria = 'Tablet' AND Preco > 1000;
```

Devido à precedência, o `AND` será avaliado primeiro. O SGBD interpretará a consulta como: "retorne todos os produtos que são `'Notebooks'` OU (são `'Tablets'` E custam mais de `1000`)". O resultado seria **todos os notebooks**, independentemente do preço, mais os tablets caros.

Para garantir que a lógica seja aplicada na ordem que desejamos, devemos usar **parênteses `()`**, assim como na matemática. Os parênteses forçam a avaliação da expressão interna primeiro.

**Consulta Corrigida:** Para encontrar "notebooks ou tablets que custam mais de 1000", a sintaxe correta é:

```sql
SELECT * FROM Produtos 
WHERE (Categoria = 'Notebook' OR Categoria = 'Tablet') AND Preco > 1000;
```

Agora, o SGBD primeiro encontra todos os produtos que são `'Notebook'` ou `'Tablet'` e, somente sobre esse resultado, aplica o filtro de preço, garantindo o resultado correto.

### Operadores de Comparação

Os operadores de comparação são os elementos centrais de qualquer condição de filtragem, sendo utilizados primariamente na cláusula `WHERE`. A função deles é comparar dois valores — que podem ser o valor de uma coluna e um valor literal, ou os valores de duas colunas — e retornar um resultado booleano (Verdadeiro ou Falso) para cada linha. Apenas as linhas para as quais a expressão resulta em Verdadeiro são incluídas no resultado da consulta.

Os principais operadores de comparação, que seguem a lógica matemática padrão, são:

|Símbolo|Comparação|Exemplo de Uso|
|---|---|---|
|**=**|Igual a|`WHERE Cidade = 'São Paulo'`|
|**`<>`** ou **`!=`**|Diferente de|`WHERE Status <> 'Finalizado'`|
|**>**|Maior que|`WHERE Preco > 100.00`|
|**>=**|Maior ou igual a|`WHERE Estoque >= 10`|
|**<**|Menor que|`WHERE Idade < 18`|
|**<=**|Menor ou igual a|`WHERE Desconto <= 0.15`|

Outros operadores de comparação importantes, como `LIKE`, `IN` e `BETWEEN`, possuem um funcionamento mais específico e serão abordados em seções dedicadas a eles.

#### Tratamento de Valores Nulos

Um ponto de atenção fundamental, e que é uma fonte comum de erros, é a comparação de valores com `NULL`. Como já vimos, `NULL` representa a ausência de valor, o "desconhecido". Por essa razão, ele não se comporta como os outros valores.

Uma comparação de igualdade como `= NULL` **nunca** resultará em Verdadeiro. A lógica é que não se pode afirmar que um valor desconhecido é "igual" a outro valor desconhecido. A expressão `NULL = NULL` resulta em `UNKNOWN` (Desconhecido), e a cláusula `WHERE` descarta linhas que resultam em `UNKNOWN`.

Para lidar corretamente com valores nulos, o SQL fornece operadores específicos:

- **`IS NULL`**: Retorna verdadeiro se o valor da coluna for nulo.
- **`IS NOT NULL`**: Retorna verdadeiro se o valor da coluna não for nulo.

**Exemplo Prático:** Para encontrar todos os clientes que ainda não cadastraram um email:

```sql
-- Forma CORRETA
SELECT Nome FROM Clientes WHERE Email IS NULL;

-- Forma INCORRETA (não retornará nenhuma linha)
-- SELECT Nome FROM Clientes WHERE Email = NULL;
```

### Funções de Agregação

As **funções de agregação** são um tipo especial de função que opera sobre um conjunto de linhas e retorna um **único valor de resumo** para aquele conjunto. Elas são a base para a criação de relatórios, painéis de controle (_dashboards_) e qualquer tipo de análise estatística sobre os dados.

Estas funções são quase sempre utilizadas em conjunto com a cláusula `GROUP BY`, que veremos mais adiante, para calcular resumos para diferentes categorias de dados.

|Função|Descrição|Sintaxe de Exemplo|
|---|---|---|
|**`SUM()`**|Soma todos os valores de uma coluna numérica.|`SUM(ValorTotal)`|
|**`AVG()`**|Calcula a média aritmética dos valores de uma coluna numérica.|`AVG(Preco)`|
|**`MAX()`**|Retorna o maior valor encontrado em uma coluna.|`MAX(Salario)`|
|**`MIN()`**|Retorna o menor valor encontrado em uma coluna.|`MIN(DataPedido)`|
|**`COUNT()`**|Conta o número de linhas em um conjunto.|`COUNT(*)`|
|**`VAR()`**|Calcula a variância estatística de um conjunto de valores.|`VAR(Nota)`|

**Exemplos Práticos:**

Utilizando uma tabela hipotética de Vendas:

- Para calcular o faturamento total: `SELECT SUM(Valor) FROM Vendas;`
- Para encontrar a venda mais cara: `SELECT MAX(Valor) FROM Vendas;`

#### As Variações do `COUNT()`

A função `COUNT()` é particularmente versátil e possui três formas principais de uso:

- **`COUNT(*)`**: Conta o **número total de linhas** no conjunto de resultados, sem exceção.
- **`COUNT(nome_da_coluna)`**: Conta o número de linhas onde a coluna especificada (`nome_da_coluna`) **não possui um valor nulo (`NULL`)**.
- **`COUNT(DISTINCT nome_da_coluna)`**: Conta o número de **valores distintos (únicos)** e não nulos na coluna especificada.

#### A Diretiva `DISTINCT`

A palavra-chave **`DISTINCT`** pode ser usada dentro de funções de agregação (e também diretamente na cláusula `SELECT`) para instruir o SGBD a **eliminar valores duplicados antes** de realizar a operação.

**Exemplo Prático:**

Imagine que queremos saber quantos clientes únicos realizaram compras em determinado período, e não o número total de vendas (pois um mesmo cliente pode ter comprado várias vezes).

```sql
SELECT COUNT(DISTINCT ID_Cliente) AS TotalClientesUnicos
FROM Vendas
WHERE DataVenda >= '2025-01-01';
```

Neste caso, o `DISTINCT` garante que cada ID de cliente seja contado apenas uma vez, fornecendo o resultado correto.

## Cláusulas de Filtragem

As **cláusulas de filtragem** são os componentes estruturais de uma instrução SQL que nos permitem especificar a origem dos nossos dados e aplicar critérios para refinar o resultado. Elas são a base sobre a qual construímos nossas consultas, definindo o universo de dados com o qual vamos trabalhar e, em seguida, aplicando "peneiras" para extrair apenas a informação relevante.

Nesta seção, vamos explorar as três cláusulas mais fundamentais e onipresentes: `FROM`, que estabelece a fonte dos dados; `WHERE`, que filtra as linhas; e `LIMIT`, que restringe a quantidade de resultados. Para ilustrar os exemplos, utilizaremos as mesmas tabelas de `PRODUTOS` e `VENDAS` que já conhecemos.

**Tabela `PRODUTOS`**

|ID|NOME|PRECO|ESTOQUE|
|---|---|---|---|
|1|Notebook|2500|15|
|2|Smartphone|1200|20|
|3|Tablet|800|15|
|4|Monitor|500|30|
|5|Impressora|300|25|

**Tabela `VENDAS`**

|ID|ID_PRODUTO (FK)|DATA|QUANTIDADE|VALOR_TOTAL|
|---|---|---|---|---|
|1|1|2024-03-09|2|5000|
|2|2|2024-03-10|3|3600|
|3|4|2024-03-10|1|500|
|4|3|2024-03-11|2|1600|
|5|2|2024-03-11|1|300|

### FROM: Especificando a Origem dos Dados

A cláusula **`FROM`** é o ponto de partida de quase toda consulta de recuperação de dados. Sua função é especificar de qual(is) tabela(s) a consulta deve extrair as informações. Ela, basicamente, define o "universo" de dados com o qual estamos trabalhando.

#### `FROM` com uma Única Tabela

O uso mais simples e direto da cláusula `FROM` é para especificar uma única tabela como fonte.

```sql
SELECT *
FROM PRODUTOS;
```

Neste caso, o universo da nossa consulta é, exclusivamente, o conteúdo da tabela `PRODUTOS`.

#### `FROM` com Múltiplas Tabelas e o Produto Cartesiano

A cláusula `FROM` também pode listar múltiplas tabelas, separadas por vírgula. Quando fazemos isso, o SGBD realiza um **Produto Cartesiano** entre elas, como vimos na Álgebra Relacional. O resultado é uma tabela intermediária que contém todas as combinações possíveis de linhas entre as tabelas listadas.

**Sempre que listamos duas ou mais tabelas no `FROM` separadas por vírgula (`FROM A, B`), o resultado inicial será um Produto Cartesiano entre elas.**

Essa sintaxe, embora válida, é considerada uma prática antiga e perigosa, pois é muito fácil esquecer de aplicar os filtros necessários (na cláusula `WHERE`) para transformar o produto cartesiano em uma junção útil. A abordagem moderna e recomendada é usar a cláusula `JOIN` explícita, que veremos mais adiante. No entanto, é fundamental entender este comportamento.

#### Simplificando com Apelidos (`ALIAS`)

Ao trabalhar com múltiplas tabelas, ou com tabelas de nomes longos, a sintaxe pode se tornar verbosa e repetitiva. Para simplificar, podemos atribuir um **apelido** (ou **`ALIAS`**) a cada tabela diretamente na cláusula `FROM`. Um alias é um nome temporário que existe apenas no escopo daquela consulta.

A sintaxe para criar um alias é simplesmente colocar o nome do apelido após o nome da tabela (a palavra-chave `AS` é opcional e frequentemente omitida para tabelas).

**Exemplo Prático (Produto Cartesiano):** Vamos selecionar o nome e o preço dos produtos e a quantidade de cada venda, listando as duas tabelas no FROM e usando aliases para simplificar.

```sql
SELECT P.NOME, P.PRECO, V.QUANTIDADE
FROM PRODUTOS P, VENDAS V;
```

- `PRODUTOS P`: Atribui o alias `P` à tabela `PRODUTOS`.
- `VENDAS V`: Atribui o alias `V` à tabela `VENDAS`.
- `P.NOME`: Refere-se à coluna `NOME` da tabela `PRODUTOS` (identificada pelo alias `P`).

Como não especificamos nenhuma condição de junção, o SGBD realiza um produto cartesiano. A tabela `PRODUTOS` tem 5 linhas e a tabela `VENDAS` tem 5 linhas. O resultado será uma tabela com 5 x 5 = 25 linhas, combinando cada produto com cada venda, o que não tem um significado prático útil neste caso.

**Resultado Parcial (Produto Cartesiano):**

| NOME | PRECO | QUANTIDADE |
| --- | --- | --- |
| Notebook | 2500 | 2 |
| Notebook | 2500 | 3 |
| Notebook | 2500 | 1 |
| Notebook | 2500 | 2 |
| Notebook | 2500 | 1 |
| Smartphone | 1200 | 2 |
| Smartphone | 1200 | 3 |
| ... | ... | ... |
| Impressora | 300 | 1 |

Este exemplo ilustra o resultado bruto gerado pela cláusula `FROM` ao lidar com múltiplas tabelas desta forma. A "mágica" de transformar este resultado bruto em uma informação útil acontece na próxima etapa, com a aplicação das cláusulas de junção (que veremos em breve) ou de filtragem, com a cláusula `WHERE`.

### WHERE: Filtrando as Linhas com Precisão

A cláusula **`WHERE`** é, possivelmente, a cláusula mais fundamental e utilizada na linguagem SQL. Sua função é filtrar as **linhas** de uma tabela, permitindo que a consulta ou operação atue apenas sobre um subconjunto de dados que atenda a uma condição específica. Ela é a implementação direta da operação de **seleção (σ)** da Álgebra Relacional.

Pense na cláusula `WHERE` como a tradução da palavra "onde" ou "cujo" em uma frase. Uma solicitação como "Mostre-me os produtos **onde** o estoque é menor que 20" ou "Apague os pedidos **cujo** status é 'Cancelado'" é implementada diretamente com a cláusula `WHERE`.

**Exemplo de Tradução (Linguagem Natural para SQL):**

- **Solicitação:** "Selecione o nome e o preço, da tabela `PRODUTOS`, onde o preço é superior a R$ 1.000."
- **Tradução SQL:**

```sql
SELECT NOME, PRECO      -- Selecione o nome e o preço
FROM PRODUTOS         -- da tabela Produtos
WHERE PRECO > 1000;     -- onde o preço é superior a 1000.
```

#### Como Funciona a Cláusula `WHERE`

Quando uma instrução SQL com uma cláusula `WHERE` é executada, o SGBD percorre a(s) tabela(s) especificadas na cláusula `FROM` e avalia a condição do `WHERE` para **cada linha individualmente**.

- Se a condição for avaliada como **VERDADEIRA** para uma determinada linha, essa linha é incluída no conjunto de resultados (para um `SELECT`) ou é marcada para a operação (para um `UPDATE` ou `DELETE`).
- Se a condição for avaliada como **FALSA** ou **DESCONHECIDA (UNKNOWN)** (no caso de comparações com `NULL`), a linha é descartada e ignorada pela instrução.

A força da cláusula `WHERE` reside em sua capacidade de construir condições complexas utilizando os operadores de comparação e lógicos que estudamos anteriormente.

#### Exemplos Práticos com `WHERE`

Vamos utilizar nossa tabela `PRODUTOS` para ilustrar.

**1. Usando Operadores de Comparação:**

- **Objetivo:** Encontrar todos os produtos com estoque igual ou superior a 20 unidades.
- **Consulta:**

```sql
SELECT NOME, ESTOQUE
FROM PRODUTOS
WHERE ESTOQUE >= 20;
```

- **Resultado:**

| NOME | ESTOQUE |
| --- | --- |
| Smartphone | 20 |
| Monitor | 30 |
| Impressora | 25 |


**2. Usando Operadores Lógicos (`AND`, `OR`, `NOT`):**

- **Objetivo:** Encontrar os produtos que custam menos de R$ 1.000 **E** que têm 15 unidades em estoque.
- **Consulta:**

```sql
SELECT NOME, PRECO, ESTOQUE
FROM PRODUTOS
WHERE PRECO < 1000 AND ESTOQUE = 15;
```

- Resultado:

| NOME | PRECO | ESTOQUE |
| --- | --- | --- |
| Tablet | 800 | 15 |

#### `WHERE` em Comandos DML

É crucial reforçar que a cláusula `WHERE` não é exclusiva do `SELECT`. Sua utilização é igualmente vital nos comandos `UPDATE` e `DELETE` para garantir que as modificações e exclusões afetem apenas os registros desejados, evitando a perda acidental de dados.

- **Exemplo com `UPDATE`:** Aumentar em 10% o preço de todos os produtos com estoque baixo (menor que 20).

```sql
UPDATE PRODUTOS
SET PRECO = PRECO * 1.10
WHERE ESTOQUE < 20;
```

- **Exemplo com `DELETE`:** Remover todas as vendas que ocorreram antes de '2024-03-10'.

```sql
DELETE FROM VENDAS
WHERE DATA < '2024-03-10';
```

Em todos esses casos, a cláusula `WHERE` atua como o filtro preciso que direciona a ação do comando SQL para o conjunto correto de dados.

### LIMIT e OFFSET: Controlando a Quantidade e a Paginação dos Resultados

No universo dos bancos de dados, onde as tabelas podem conter dezenas, milhares ou até milhões de registros, raramente é prático ou performático recuperar todos os dados de uma só vez. Para lidar com essa realidade, a linguagem SQL nos oferece um par de cláusulas poderosas para controlar exatamente qual "fatia" de um conjunto de resultados desejamos obter: **`LIMIT`** e **`OFFSET`**.

#### `LIMIT`: Restringindo a Quantidade de Linhas

A cláusula **`LIMIT`** tem uma função direta e indispensável: restringir o **número máximo de linhas** que uma consulta irá retornar. Ela atua como um portão de controle no final do processo de consulta, permitindo que apenas um número especificado de registros passe para o resultado final.

Um dos pontos mais críticos para o uso correto do `LIMIT` é compreender que o modelo relacional não garante, por padrão, nenhuma ordem específica para as linhas de uma tabela. Se executarmos uma consulta sem uma cláusula de ordenação, o SGBD é livre para retornar as linhas na ordem que julgar mais eficiente, e essa ordem pode mudar a cada execução.

Por essa razão, utilizar `LIMIT` de forma isolada pode levar a resultados **não determinísticos**. Uma consulta como `SELECT * FROM Produtos LIMIT 5;` pode retornar os cinco primeiros produtos cadastrados hoje, e cinco produtos completamente diferentes amanhã.

Para que o `LIMIT` tenha um resultado consistente e significativo, ele deve ser quase sempre utilizado em conjunto com a cláusula **`ORDER BY`**, que veremos em detalhe mais adiante. A cláusula `ORDER BY` primeiro estabelece uma ordem previsível para todo o conjunto de resultados e, só então, o `LIMIT` seleciona o número de linhas desejado a partir do topo dessa lista ordenada.

**Exemplo Prático (Consulta "Top-N"):**

- **Objetivo:** Encontrar os 3 produtos mais caros em nosso catálogo.
- **Consulta:**

```SQL
SELECT NOME, PRECO
FROM PRODUTOS
ORDER BY PRECO DESC -- 1º Passo: Ordenar
LIMIT 3;            -- 2º Passo: Limitar
```

- **Análise do Processo:**
    1. **Ordenação:** O SGBD primeiro executa o `FROM PRODUTOS ORDER BY PRECO DESC`. Ele cria uma lista interna de todos os produtos, ordenada do preço mais alto para o mais baixo:
        - Notebook (2500)
        - Smartphone (1200)
        - Tablet (800)
        - Monitor (500)
        - Impressora (300)

    2. **Limitação:** Em seguida, a cláusula `LIMIT 3` é aplicada sobre essa lista já ordenada. Ela simplesmente "corta" a lista após o terceiro item, retornando apenas os três primeiros, que são garantidamente os mais caros.

#### `OFFSET`: Pulando Linhas para Paginação

Enquanto o `LIMIT` define "quantos pegar", a cláusula **`OFFSET`** define "a partir de onde pegar". Sua função é instruir o SGBD a **pular um determinado número de linhas** do conjunto de resultados (já ordenado) antes de começar a aplicar o `LIMIT`. É a combinação das duas que torna a implementação de sistemas de **paginação** em aplicações uma tarefa simples e eficiente.

A sintaxe combinada é:

```sql
... ORDER BY <coluna> LIMIT <tamanho_da_pagina> OFFSET <linhas_a_pular>;
```

O valor do `OFFSET` para uma determinada página é geralmente calculado pela fórmula: `(numero_da_pagina_desejada - 1) * tamanho_da_pagina`.

**Exemplo Prático (Implementando Paginação):**

- **Objetivo:** Paginar nossa tabela `PRODUTOS`, exibindo 2 produtos por página, ordenados por nome.
- **Conjunto de Dados Ordenado (`ORDER BY NOME ASC`):**
    1. Impressora
    2. Monitor
    3. Notebook
    4. Smartphone
    5. Tablet
- **Para obter a Página 1 (Pular 0, pegar 2):**
    - `OFFSET` = (1 - 1) * 2 = 0
    - `SELECT ID, NOME FROM PRODUTOS ORDER BY NOME LIMIT 2 OFFSET 0;`
    - O SGBD pula 0 linhas e retorna as 2 seguintes: (Impressora, Monitor).

- **Para obter a Página 2 (Pular 2, pegar 2):**
    - `OFFSET` = (2 - 1) * 2 = 2
    - `SELECT ID, NOME FROM PRODUTOS ORDER BY NOME LIMIT 2 OFFSET 2;`
    - O SGBD pula as 2 primeiras linhas (Impressora, Monitor) e retorna as 2 seguintes: (Notebook, Smartphone).

- **Para obter a Página 3 (Pular 4, pegar 2):**
    - `OFFSET` = (3 - 1) * 2 = 4
    - `SELECT ID, NOME FROM PRODUTOS ORDER BY NOME LIMIT 2 OFFSET 4;`
    - O SGBD pula as 4 primeiras linhas e retorna as 2 seguintes (neste caso, resta apenas 1): (Tablet).

#### Variações de Sintaxe e Uso em DML

É importante notar que, embora o conceito seja universal, a sintaxe exata pode variar entre os SGBDs:

- **MySQL/PostgreSQL:** Usam a sintaxe `LIMIT ... OFFSET ...`.
- **SQL Server:** Utiliza a sintaxe mais verbosa `OFFSET ... ROWS FETCH NEXT ... ROWS ONLY`.
- **Oracle:** Utiliza a sintaxe `OFFSET ... ROWS FETCH FIRST ... ROWS ONLY`.

Além do uso em `SELECT`, alguns SGBDs (como o MySQL) permitem o uso de `LIMIT` e `OFFSET` com comandos DML. Essa funcionalidade é frequentemente utilizada como uma medida de segurança e controle para executar alterações ou exclusões em lotes ("batches"), evitando sobrecarregar o banco de dados.

- **Exemplo:** Remover 10 usuários, mas pulando os 4 primeiros (removendo do 5º ao 14º).

```sql
DELETE FROM usuarios
ORDER BY data_cadastro ASC
LIMIT 10 OFFSET 4;
```


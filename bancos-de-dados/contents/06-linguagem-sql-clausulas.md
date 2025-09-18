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
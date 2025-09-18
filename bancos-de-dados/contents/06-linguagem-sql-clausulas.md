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


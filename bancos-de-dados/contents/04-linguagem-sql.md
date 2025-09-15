# Capítulo 4 – Linguagem SQL

Nos capítulos anteriores, construímos uma base teórica sólida, explorando desde a modelagem conceitual até a lógica matemática da Álgebra Relacional. Agora, é hora de traduzir essa teoria em prática. A **SQL (Structured Query Language)**, ou Linguagem de Consulta Estruturada, é a ponte que conecta nossas intenções com os dados armazenados no banco.

Desenvolvida na década de 1970 pela IBM com base no modelo relacional de Edgar F. Codd, a SQL se tornou o padrão de fato e de direito (padronizada pela ANSI e ISO) para a grande maioria dos Sistemas de Gerenciamento de Bancos de Dados (SGBDs) relacionais. Embora cada SGBD possua suas próprias variações e extensões (dialetos), o núcleo da linguagem é universal, permitindo que o conhecimento adquirido seja aplicado em diversas plataformas.

É fundamental entender uma característica que define a SQL: ela é uma linguagem **declarativa**. Diferente da Álgebra Relacional, que é procedural (onde especificamos a sequência de passos para obter um resultado), na SQL nós simplesmente **declaramos o que queremos**. Escrevemos uma consulta que descreve o resultado desejado e o SGBD, através de seu otimizador de consultas, se encarrega de descobrir a melhor sequência de operações (o melhor "plano de execução") para nos entregar esses dados de forma eficiente.

### Os Subconjuntos da Linguagem SQL

A SQL é uma linguagem extremamente rica e completa. Para facilitar seu estudo e organização, seus comandos são tradicionalmente agrupados em subconjuntos ou "sublinguagens", de acordo com a função que desempenham.

<div align="center">
<img width="700px" src="./img/04-sql-subdivisoes.png">
</div>

É importante notar que, embora a divisão em cinco categorias (DQL, DML, DDL, DCL, TCL) seja didaticamente muito útil, algumas convenções consideram a DQL (linguagem de consulta) como parte da DML (linguagem de manipulação), resultando em uma divisão de apenas quatro categorias. Na prática, essa diferença é puramente conceitual, pois o SGBD interpreta todos os comandos como parte de uma única linguagem coesa.

Nos próximos tópicos, vamos explorar em detalhe cada um desses subconjuntos e seus principais comandos, começando pela linguagem utilizada para consultar e recuperar os dados do banco de dados.


# Capítulo 5 – SQL Embutido: Além da Consulta Interativa

Até este ponto, nossa exploração da linguagem SQL se concentrou em seu uso interativo, onde os comandos são executados diretamente em uma interface do SGBD para análise de dados, administração ou testes. Embora essa seja uma forma poderosa de interagir com o banco de dados, a grande maioria das aplicações do mundo real — sistemas de gestão, aplicações web, serviços de backend — precisa executar consultas e manipular dados de forma programática, como parte de um fluxo lógico maior.

Para atender a essa necessidade, desde suas primeiras especificações, o padrão SQL previu uma forma de integrar seus comandos diretamente no código-fonte de linguagens de programação de alta-level. A linguagem na qual o código SQL é inserido é chamada de **linguagem hospedeira**, e a técnica de mesclar as duas é conhecida como **SQL Embutido (Embedded SQL)**. Esta foi a abordagem original e padronizada para permitir que linguagens procedurais como C, COBOL, Pascal e Fortran se comunicassem de forma nativa com um banco de dados relacional.

Este capítulo explora os conceitos e a mecânica do SQL Embutido. Entenderemos como os comandos SQL são diferenciados do código da linguagem hospedeira, como os dados são trocados entre a aplicação e o banco de dados e, mais importante, como o paradigma de conjuntos do SQL é reconciliado com o processamento iterativo, linha a linha, das linguagens tradicionais através do uso de **cursores**.

## O Mecanismo do SQL Embutido

A integração do SQL em uma linguagem hospedeira não é mágica; ela depende de um passo de pré-processamento. O desenvolvedor escreve os comandos SQL diretamente no arquivo de código-fonte, mas os envolve em uma sintaxe especial para que possam ser identificados.

### O Pré-processador e a Sintaxe `EXEC SQL`

O fluxo de trabalho do SQL Embutido funciona da seguinte forma: um **pré-processador** específico para o SGBD analisa o código-fonte antes do compilador da linguagem hospedeira. Este pré-processador procura por blocos de código marcados com `EXEC SQL` no início e um finalizador como `END-EXEC`. Ao encontrar um desses blocos, ele traduz o comando SQL embutido em chamadas de função de baixo nível, específicas da API daquele banco de dados. O resultado é um novo arquivo de código-fonte, agora em pura linguagem hospedeira, que é então compilado normalmente.

A sintaxe geral para um comando SQL embutido é:

```sql
EXEC SQL <comando SQL embutido> END-EXEC
```

Para interagir com a aplicação, é fundamental que o SQL possa utilizar os valores das variáveis da linguagem hospedeira. Isso é feito prefixando o nome da variável com dois pontos (`:`).

## Navegando por Resultados com Cursores

Um dos desafios fundamentais ao integrar SQL com linguagens procedurais é a chamada **incompatibilidade de impedância (impedance mismatch)**. O SQL é uma linguagem baseada em conjuntos: uma única consulta `SELECT` pode retornar centenas ou milhares de linhas de uma só vez. As linguagens hospedeiras, por outro lado, tradicionalmente operam de forma iterativa, processando um registro ou uma estrutura de dados de cada vez dentro de um laço (`loop`).

O **cursor** é a ponte que resolve essa incompatibilidade. Ele é um objeto do banco de dados que funciona como um ponteiro para o conjunto de resultados de uma consulta. Em vez de receber todas as linhas de uma vez, a aplicação "abre" um cursor e pode, então, recuperar as linhas uma a uma, navegando pelo conjunto de resultados de forma controlada.

### O Ciclo de Vida de um Cursor

O uso de um cursor segue um ciclo de vida bem definido, com quatro comandos principais.

**1. Declaração (`DECLARE CURSOR`)**

Primeiro, o cursor é declarado, associando-o a uma consulta `SELECT`. Esta etapa apenas define o cursor e a consulta, mas não a executa. Note no exemplo abaixo o uso da variável da linguagem hospedeira `:total` para filtrar a consulta.

```sql
EXEC SQL
  declare c cursor for
  select nome_cliente, cidade_cliente
    from deposito, cliente
    where deposito.nome_cliente = cliente.nome_cliente
      and deposito.saldo > :total
END-EXEC
```

**2. Abertura (`OPEN`)**

O comando `OPEN` instrui o SGBD a executar a consulta associada ao cursor. O banco de dados avalia a consulta, monta o conjunto de resultados completo e o deixa pronto para ser percorrido.

```sql
EXEC SQL open c END-EXEC
```

**3. Recuperação (`FETCH`)**

O comando `FETCH` é o coração do processamento iterativo. Ele avança o cursor para a próxima linha do conjunto de resultados e copia os valores das colunas dessa linha para variáveis especificadas da linguagem hospedeira. Este comando é tipicamente colocado dentro de um laço na aplicação.

```sql
EXEC SQL fetch c into :cn, :an END-EXEC
```

A aplicação pode então processar os valores nas variáveis `:cn` (nome do cliente) e `:an` (cidade do cliente). O laço continua até que uma variável especial, geralmente em uma estrutura chamada SQLCA (SQL Communication Area), indique que não há mais linhas a serem recuperadas (fim do conjunto de dados).

**4. Fechamento (`CLOSE`)**

Após o término do laço, é crucial fechar o cursor com o comando `CLOSE`. Esta ação libera todos os todos os recursos (como memória e bloqueios de banco de dados) que foram alocados para manter o conjunto de resultados ativo.

```sql
EXEC SQL close c END-EXEC
```

## A Evolução: De SQL Embutido a APIs Modernas

Embora o SQL Embutido tenha sido fundamental para a história dos bancos de dados, a abordagem moderna para a integração de aplicações e bancos de dados evoluiu. Hoje, em vez de um pré-processador, as aplicações utilizam **APIs de banco de dados** e **drivers** específicos.

Tecnologias como **JDBC** (para Java), **ODBC** (um padrão para a linguagem C), **DB-API** (para Python) e **ADO.NET** (para a plataforma .NET) fornecem um conjunto de classes e funções que permitem uma integração mais flexível e dinâmica. Com essas APIs, as consultas SQL são construídas como strings e enviadas ao banco de dados através de chamadas de função, e os resultados são recebidos em objetos e estruturas de dados nativas da linguagem, eliminando a necessidade de um passo de pré-compilação e se alinhando melhor com os paradigmas de programação modernos, como a orientação a objetos.

## Considerações Finais

Neste capítulo, exploramos uma faceta diferente da linguagem SQL, saindo do ambiente de consulta interativa para entender como ela se integra a linguagens de programação através da abordagem clássica do **SQL Embutido**. Analisamos o papel fundamental do pré-processador, a sintaxe `EXEC SQL` e o uso de variáveis de hospedeiro para criar uma ponte entre a aplicação e o banco de dados. O conceito central desta exploração foi a solução para a "incompatibilidade de impedância": o **cursor**, que permite que uma linguagem procedural, operando linha a linha, navegue e processe os resultados baseados em conjuntos do SQL de forma controlada.

Compreendemos o ciclo de vida de um cursor — da sua declaração e abertura até a recuperação iterativa com `FETCH` e o eventual fechamento para liberar recursos. Embora o SQL Embutido tenha sido em grande parte suplantado por APIs de banco de dados modernas como JDBC e ODBC, os conceitos que ele introduziu permanecem relevantes. A necessidade de gerenciar conexões, executar declarações, e iterar sobre conjuntos de resultados ainda é o cerne de toda programação de banco de dados.

Com a compreensão de como os dados são acessados tanto interativamente quanto programaticamente, a fundação de nosso conhecimento em SQL está quase completa. Restam, no entanto, dois pilares essenciais que garantem a segurança e a confiabilidade de um sistema de banco de dados multiusuário. O próximo capítulo se dedicará a explorar a **Linguagem de Controle de Dados (DCL)**, focada em gerenciar permissões e o acesso aos dados, e a **Linguagem de Controle de Transação (TCL)**, que nos ensina a agrupar operações em unidades atômicas para garantir a integridade dos dados.
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

## DCL (Data Control Language)

A **DCL (Data Control Language)**, ou Linguagem de Controle de Dados, é o subconjunto da SQL dedicado à **segurança** do banco de dados. Sua finalidade é gerenciar as permissões e os privilégios de acesso dos usuários aos diversos objetos do banco, como tabelas, visões e procedimentos. Através dos comandos DCL, o Administrador do Banco de Dados (DBA) pode definir com precisão quem pode fazer o quê no sistema.

<div align="center">
<img width="360px" src="./img/04-dcl.png">
</div>

Pense na DCL como o sistema de crachás e chaves de um prédio. Ela não lida com o conteúdo dos escritórios (os dados, que são gerenciados pela DML) nem com a construção das salas (a estrutura, gerenciada pela DDL), mas sim com a definição de quais funcionários (usuários) têm a chave para entrar em quais salas (tabelas) e o que eles podem fazer lá dentro (consultar, inserir, apagar). É uma camada essencial para garantir que informações sensíveis sejam acessadas apenas por pessoal autorizado.

Antes de explorarmos os comandos específicos de cada sublinguagem, é importante estabelecer duas convenções de sintaxe fundamentais que se aplicam a toda a linguagem SQL.

1. **Sensibilidade a Maiúsculas e Minúsculas (Case-Sensitivity):** Uma linguagem é dita _case-sensitive_ quando ela diferencia letras maiúsculas de minúsculas (ou seja, `Joao` é diferente de `joao`). Quando a linguagem não faz essa distinção, ela é _case-insensitive_. No universo SQL, o comportamento pode variar de um SGBD para outro. Para fins didáticos e para manter a consistência, adotaremos a convenção do Microsoft SQL Server, que, de forma geral, trata os comandos e nomes de objetos como **case-insensitive**. Assim, `SELECT`, `select` e `SeLeCt` serão interpretados da mesma forma.
2. **O Delimitador de Comandos (;):** O SGBD não interpreta quebras de linha ou parágrafos. Um comando SQL, ou uma "instrução", pode se estender por várias linhas para facilitar a leitura. O que sinaliza para o SGBD que uma instrução terminou e a próxima pode começar é o símbolo de ponto e vírgula (**;**). Ele atua como o ponto final de uma frase, delimitando o fim de um comando.

Com essas convenções em mente, vamos agora analisar os comandos que compõem a DCL.

### GRANT: Concedendo Permissões

O comando **`GRANT`** é utilizado para **conceder** permissões a um usuário ou a um papel (_role_). Através dele, o DBA pode autorizar o acesso e a execução de operações específicas sobre os objetos do banco de dados.

A sintaxe básica para conceder permissões sobre uma tabela é:

```sql
GRANT <privilegio(s)> ON <nome_do_objeto> TO <usuario_ou_papel>;
```

- **`<privilegio(s)>`:** A permissão a ser concedida (ex: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, ou `ALL` para todas as permissões).
- **`<nome_do_objeto>`:** O objeto do banco de dados ao qual a permissão se aplica (ex: a tabela `CLIENTES`).
- **`<usuario_ou_papel>`:** O usuário ou o papel que receberá a permissão.

**Exemplo Prático:** Imagine que temos um usuário chamado joao, que é um analista de dados. Ele precisa apenas consultar as informações da tabela CLIENTES, sem poder alterá-las. O comando para conceder essa permissão seria:

```sql
GRANT SELECT ON CLIENTES TO joao;
```

A partir de agora, o usuário `joao` pode executar consultas `SELECT` na tabela `CLIENTES`, mas qualquer tentativa de usar `INSERT`, `UPDATE` ou `DELETE` resultará em um erro de permissão negada.

### REVOKE: Revogando Permissões

O comando **`REVOKE`** é a operação inversa do `GRANT`. Ele é utilizado para **revogar** (remover) permissões que foram previamente concedidas a um usuário ou papel.

A sintaxe básica é:

```sql
REVOKE <privilegio(s)> ON <nome_do_objeto> FROM <usuario_ou_papel>;
```

**Exemplo Prático:** Após um tempo, o analista joao muda de função e não precisa mais do acesso à tabela CLIENTES. Para remover a permissão que concedemos anteriormente, utilizamos o REVOKE:

```sql
REVOKE SELECT ON CLIENTES FROM joao;
```

É interessante notar a natureza declarativa da linguagem SQL e sua proximidade com o inglês. Quando concedemos algo, concedemos algo **PARA** alguém (`TO joao`). Quando retiramos algo, retiramos algo **DE** alguém (`FROM joao`). Essa lógica torna os comandos mais intuitivos.

### DENY: Negando Permissões Explicitamente

O comando **`DENY`** é uma forma mais forte e explícita de restrição. Enquanto o `REVOKE` apenas remove uma permissão concedida, o `DENY` cria uma **negação explícita**, que se sobrepõe a qualquer outra permissão que o usuário possa ter, inclusive permissões herdadas de um papel.

Em outras palavras, o `DENY` funciona como um "veto". Se um usuário pertence a um papel "Analistas" que tem permissão de `SELECT` na tabela `CLIENTES`, mas o usuário `joao` recebe um `DENY` específico para essa mesma permissão, ele **não poderá** acessar a tabela, pois a negação explícita tem precedência.

A sintaxe básica é:

```sql
DENY <privilegio(s)> ON <nome_do_objeto> TO <usuario_ou_papel>;
```

**Exemplo Prático:** Para garantir que o usuário joao nunca possa consultar a tabela CLIENTES, independentemente de quaisquer outras permissões que ele possa receber no futuro, o DBA executaria:

```sql
DENY SELECT ON CLIENTES TO joao;
```

Para remover essa negação explícita, o DBA precisaria usar o comando `REVOKE` para revogar o `DENY`, e só então poderia conceder a permissão novamente com o `GRANT`.

### A Cláusula `WITH GRANT OPTION`

A cláusula `WITH GRANT OPTION` não é um comando DCL em si, mas um modificador poderoso que pode ser adicionado ao comando `GRANT`. Ela permite que o usuário que está recebendo uma permissão também receba a capacidade de **delegar essa mesma permissão** a outros usuários.

**Exemplo Prático:** Imagine que maria é a gerente do departamento de vendas. O DBA concede a ela a permissão para consultar a tabela VENDAS e também a capacidade de delegar essa permissão para sua equipe:

```sql
GRANT SELECT ON VENDAS TO maria WITH GRANT OPTION;
```

Agora, a própria `maria` pode executar o seguinte comando para dar a mesma permissão ao seu subordinado, `joao`:

```sql
GRANT SELECT ON VENDAS TO joao;
```

Essa cláusula cria uma cadeia de delegação de permissões, o que pode ser útil em grandes organizações, mas exige um controle cuidadoso para evitar a disseminação descontrolada de privilégios. É importante notar que a cláusula `WITH GRANT OPTION` não pode ser utilizada com o comando `DENY`.

## TCL (Transaction Control Language)

A **TCL (Transaction Control Language)**, ou Linguagem de Controle de Transações, é o subconjunto da SQL que nos permite gerenciar explicitamente as transações dentro do banco de dados. Como vimos nos capítulos anteriores, uma transação é um conjunto de operações que devem ser executadas como uma única unidade lógica e atômica. A TCL é a ferramenta que nos permite definir onde uma transação começa, onde ela termina e qual deve ser o seu desfecho: sucesso permanente ou falha total.

<div align="center">
<img width="480px" src="./img/04-tcl.png">
</div>

É através dos comandos TCL que garantimos na prática os princípios de **Atomicidade** e **Durabilidade** do ACID. Eles nos dão o poder de agrupar uma série de comandos DML (`INSERT`, `UPDATE`, `DELETE`) e decidir, ao final, se o conjunto inteiro de alterações deve ser permanentemente salvo no banco de dados ou se deve ser completamente desfeito, como se nunca tivesse acontecido.

A estrutura dos comandos TCL é notavelmente simples. Diferente de um `SELECT` ou `GRANT`, eles geralmente não possuem cláusulas ou parâmetros complexos. São comandos diretos que instruem o SGBD a tomar uma ação definitiva sobre a transação atual. Nos tópicos seguintes, exploraremos cada um desses comandos.

### COMMIT: Tornando as Alterações Permanentes

O comando **`COMMIT`** é o comando de confirmação da TCL. Sua função é **finalizar uma transação com sucesso**, tornando todas as modificações de dados realizadas dentro dela (sejam `INSERT`, `UPDATE` ou `DELETE`) **permanentes** no banco de dados.

Pense em uma transação como um "rascunho" de alterações. Enquanto se está trabalhando, as modificações são visíveis apenas para a sua sessão e ainda podem ser desfeitas. O `COMMIT` é o ato de "salvar o documento final": a partir do momento em que é executado, as alterações se tornam duráveis (garantindo a **Durabilidade** do ACID), visíveis para todos os outros usuários e não podem mais ser desfeitas com um simples comando de reversão. Ele marca a conclusão bem-sucedida da unidade de trabalho atômica, cumprindo o princípio da **Atomicidade**.

Antes de uma transação ser "commitada", suas alterações existem em um estado transitório. Essa invisibilidade temporária para outros usuários é a base do **Isolamento**, um dos pilares do ACID. No entanto, o quão "invisível" essa transação é pode ser configurado. O padrão ANSI SQL define quatro níveis de isolamento, que representam um equilíbrio entre consistência e performance.

#### Níveis de Isolamento

Os níveis de isolamento determinam o grau em que uma transação deve ser isolada das modificações de dados feitas por outras transações concorrentes. Um nível mais alto garante mais consistência, mas pode reduzir a capacidade de múltiplos usuários operarem simultaneamente (concorrência).

<div align="center">
<img width="700px" src="./img/04-tcl-niveis-de-isolamento.png">
</div>

- **READ UNCOMMITTED (Leitura Não Confirmada):** Este é o nível de isolamento mais baixo. Uma transação operando neste nível pode ler dados que foram modificados por outras transações, mas que **ainda não foram confirmados** com `COMMIT`. Isso pode levar a **leituras sujas (_dirty reads_)**, onde se age com base em dados que podem ser desfeitos a qualquer momento. Oferece a maior concorrência, mas a menor garantia de consistência.
- **READ COMMITTED (Leitura Confirmada):** Neste nível, uma transação só pode ler dados que já foram permanentemente salvos por um `COMMIT`. Isso **evita as leituras sujas**. No entanto, ainda pode ocorrer a **leitura não repetível (_non-repeatable read_)**: se uma transação lê o mesmo dado duas vezes, ela pode obter resultados diferentes caso outra transação tenha alterado e "commitado" esse dado no intervalo entre as duas leituras. Este é o nível de isolamento padrão em muitos SGBDs, como Oracle e SQL Server, por oferecer um bom equilíbrio entre consistência e performance.
- **REPEATABLE READ (Leitura Repetível):** Este nível garante que, se uma transação reler a mesma linha múltiplas vezes, ela sempre obterá os mesmos valores. O SGBD cria um "snapshot" dos dados no início da transação, e a transação continua a ler essa versão consistente, ignorando alterações "commitadas" por outras transações. Isso **evita leituras não repetíveis**. Contudo, ainda pode ocorrer a **leitura fantasma (_phantom read_)**: se a transação repetir uma consulta que retorna um _conjunto_ de linhas (ex: `SELECT COUNT(*)`), ela pode obter um número diferente de linhas caso outra transação tenha inserido (e "commitado") novos registros que satisfaçam a condição da consulta. Este é o nível padrão do MySQL (com o motor InnoDB).
- **SERIALIZABLE (Serializável):** O nível de isolamento mais alto e restritivo. Ele garante que o resultado da execução de transações concorrentes seja idêntico ao resultado de executá-las em alguma ordem sequencial (uma após a outra). **Evita todos os tipos de anomalias**, incluindo leituras sujas, não repetíveis e fantasmas. Garante a consistência máxima, mas ao custo da menor concorrência, pois o SGBD utiliza mecanismos de bloqueio mais agressivos para serializar as operações.

#### Sintaxe e Uso do `COMMIT`

Como mencionado, a sintaxe do `COMMIT` é extremamente simples. Ele é um comando autocontido. Geralmente, ele é usado ao final de um bloco de operações DML que formam uma unidade lógica de trabalho.

**Exemplo de Transação:**

```sql
-- Inicia uma transação (em alguns SGBDs, isso é implícito)
START TRANSACTION;

-- Operação 1: Insere um novo pedido
INSERT INTO Pedidos (cliente_id, data_pedido) VALUES (101, '2025-09-15');

-- Operação 2: Atualiza o estoque do produto vendido
UPDATE Produtos SET estoque = estoque - 1 WHERE id_produto = 50;

-- Se ambas as operações foram bem-sucedidas, confirma a transação
COMMIT;
```

Até o comando `COMMIT` ser executado, as alterações na tabela `Pedidos` e `Produtos` existem em um estado transitório, invisível para outros usuários. Após o `COMMIT`, elas se tornam permanentes e visíveis para todo o sistema.

### ROLLBACK: Desfazendo Alterações

O comando **`ROLLBACK`** é a contraparte do `COMMIT`. Sua função é **desfazer** todas as alterações de dados (`INSERT`, `UPDATE`, `DELETE`) realizadas dentro de uma transação que ainda não foi confirmada. Ele reverte o banco de dados para o estado em que se encontrava no momento do último `COMMIT` (ou do início da transação), garantindo a **Atomicidade** ao assegurar que uma transação incompleta ou mal-sucedida não deixe "rastros" de alterações parciais.

É fundamental entender que o `ROLLBACK` só pode ser aplicado a uma transação em andamento. Uma vez que o comando `COMMIT` é executado, as alterações se tornam permanentes e o `COMMIT` funciona como um ponto de salvamento definitivo; as alterações "commitadas" não podem mais ser desfeitas com um `ROLLBACK`. Pense nos "saves" de um videogame: o `COMMIT` é como salvar o jogo, e o `ROLLBACK` é como carregar o último save, descartando todo o progresso feito desde então.

A sintaxe do `ROLLBACK` é tão simples quanto a do `COMMIT`:

```sql
ROLLBACK;
```

**Exemplo de Transação com `ROLLBACK`:** Imagine um cenário de transferência bancária onde ocorre um erro no meio do processo.

```sql
START TRANSACTION;

-- Operação 1: Debita R$ 500 da conta de origem (ID 123)
UPDATE Contas SET saldo = saldo - 500 WHERE id_conta = 123;

-- Operação 2: Tenta creditar na conta de destino (ID 456), mas o ID não existe
UPDATE Contas SET saldo = saldo + 500 WHERE id_conta = 999; -- Erro, conta inexistente

-- Como a operação 2 falhou, a transação está inconsistente. Desfazemos tudo.
ROLLBACK;
```

Ao executar o `ROLLBACK`, a atualização na conta `123` é completamente desfeita. Para o banco de dados, é como se a transação nunca tivesse ocorrido, garantindo que o saldo da conta de origem permaneça intacto e a consistência seja mantida.

### SAVEPOINT: Criando Pontos de Restauração Intermediários

Em transações longas e complexas, desfazer todo o trabalho com um `ROLLBACK` completo pode não ser o ideal. Para um controle mais granular, a TCL oferece o comando **`SAVEPOINT`**.

Um `SAVEPOINT` é um **ponto de salvamento nomeado dentro de uma transação**. Ele permite que se reverta a transação não para o seu início, mas para um ponto intermediário específico, preservando o trabalho realizado antes daquele ponto.

A sintaxe para criar um savepoint é:

```sql
SAVEPOINT <nome_do_savepoint>;
```

Para retornar a um savepoint específico, utiliza-se uma variação do `ROLLBACK`:

```sql
ROLLBACK TO <nome_do_savepoint>;
```

E para remover um savepoint que não é mais necessário (liberando recursos do sistema):

```sql
RELEASE SAVEPOINT <nome_do_savepoint>;
```

**Exemplo Prático:** Considere um processo de matrícula de um aluno em múltiplos cursos.

```sql
START TRANSACTION;

-- Matrícula no Curso A
INSERT INTO Matriculas (aluno_id, curso_id) VALUES (101, 'CURSO-A');

-- Cria um ponto de salvamento após a primeira matrícula bem-sucedida
SAVEPOINT matricula_curso_a;

-- Tenta matricular no Curso B, mas descobre que não há vagas
-- A operação falha ou a lógica da aplicação decide não prosseguir
-- INSERT INTO Matriculas (aluno_id, curso_id) VALUES (101, 'CURSO-B'); -- Falha

-- Em vez de desfazer tudo, voltamos apenas ao ponto antes da tentativa no Curso B
ROLLBACK TO matricula_curso_a;

-- Agora, a matrícula no Curso A ainda está no "rascunho" da transação,
-- mas a tentativa de matrícula no Curso B foi desfeita.
-- Podemos tentar matricular em outro curso (Curso C) ou finalizar a transação.

INSERT INTO Matriculas (aluno_id, curso_id) VALUES (101, 'CURSO-C');

-- Se tudo deu certo, confirma todas as matrículas válidas (A e C)
COMMIT;
```

Os `SAVEPOINT`s oferecem uma flexibilidade imensa para o tratamento de erros em transações complexas, permitindo a implementação de lógicas de "tentativa e erro" sem a necessidade de abortar todo o processo.


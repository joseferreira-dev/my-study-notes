# Capítulo 8 – Transações em Bancos de Dados

Nos capítulos anteriores, focamos em como definir, manipular e consultar dados. Agora, vamos explorar o mecanismo que garante que todas essas operações ocorram de forma segura, consistente e confiável, especialmente em ambientes complexos com múltiplos usuários e a possibilidade de falhas: a **transação**. As transações são o coração pulsante de um banco de dados relacional, o processo que garante que os dados passem de um estado válido para outro sem erros, corrupção ou perda de informação.

## Conceitos Gerais

Uma **transação**, no contexto de bancos de dados, é uma sequência de uma ou mais operações que é executada como uma **única unidade lógica e indivisível de trabalho**. O princípio fundamental de uma transação é o "tudo ou nada": ou todas as operações dentro dela são concluídas com sucesso, ou nenhuma delas é efetivada, e o banco de dados retorna ao estado em que se encontrava antes de a transação começar.

O objetivo principal das transações é garantir a consistência e a integridade dos dados, mesmo em cenários de acesso concorrente (múltiplos usuários operando ao mesmo tempo) ou em caso de falhas de sistema, software ou hardware.

As operações contidas em uma transação são, tipicamente, os comandos DML que formam o acrônimo **CRUD** — Create (Criar), Read (Ler), Update (Atualizar) e Delete (Remover). Uma transação pode ser tão simples quanto um único `UPDATE` ou tão complexa quanto uma série de `INSERT`s, `UPDATE`s e `DELETE`s que representam uma completa operação de negócio.

<div align="center">
<img width="580px" src="./img/08-crud.png">
</div>

### Exemplo de uma Transação de Negócio

Vamos analisar um exemplo prático de uma transação em SQL que registra uma nova venda em um sistema de e-commerce. Esta única operação de negócio requer três passos no banco de dados:

```sql
BEGIN TRANSACTION;

-- Passo 1: Inserir o cabeçalho do pedido na tabela de Pedidos
INSERT INTO Pedidos (id_pedido, id_cliente, data_pedido, valor_total)
VALUES (1001, 200, '2025-04-07', 350.00);

-- Passo 2: Inserir os itens comprados na tabela Itens_Pedido
INSERT INTO Itens_Pedido (id_pedido, id_produto, quantidade, preco_unitario)
VALUES (1001, 501, 2, 175.00); -- O usuário comprou 2 unidades de R$175.00 e não R$100.00 como antes.

-- Passo 3: Atualizar o estoque do produto vendido na tabela Produtos
UPDATE Produtos
SET quantidade_estoque = quantidade_estoque - 2
WHERE id_produto = 501;

COMMIT;
```

Neste exemplo, as palavras-chave `BEGIN TRANSACTION` e `COMMIT` delimitam a unidade de trabalho. Se qualquer um dos três passos falhar (por exemplo, se o `UPDATE` no estoque não for possível por falta de produto), toda a transação seria desfeita, e o `INSERT` em `Pedidos` e `Itens_Pedido` seria revertido. Para entender por que e como isso funciona, precisamos primeiro explorar os princípios que governam todas as transações.

## Princípios ACID

**ACID** é um acrônimo que representa quatro propriedades fundamentais que garantem a confiabilidade das transações em um SGBD: **A**tomicidade, **C**onsistência, **I**solamento e **D**urabilidade.

<div align="center">
<img width="580px" src="./img/08-principios-acid.png">
</div>

Esses princípios formam um "contrato" entre o SGBD e a aplicação, uma promessa de que os dados serão mantidos de forma íntegra e previsível, mesmo diante de erros ou acessos simultâneos. A aderência a estas propriedades é o que distingue um SGBD transacional robusto.

O uso dos princípios ACID é indispensável para manter a integridade dos dados em qualquer sistema onde a precisão e a confiabilidade são críticas, como em sistemas financeiros (transferências bancárias), comerciais (controle de estoque), de saúde (prontuários de pacientes) ou governamentais. A ausência dessas garantias poderia levar à corrupção de dados, inconsistências, perda de informações e falhas severas na operação das aplicações. Portanto, os princípios ACID são a base sobre a qual a confiança nos sistemas de banco de dados é construída.

Vamos agora explorar mais profundamente o significado de cada um desses quatro princípios.

### Atomicidade

A **atomicidade** refere-se ao princípio de que uma transação deve ser tratada como uma operação única e indivisível, do início ao fim. O nome deriva do conceito de "átomo" como uma unidade fundamental que não pode ser quebrada. Aplicado a bancos de dados, isso significa que ou **todas** as operações que compõem a transação são executadas com sucesso, ou **nenhuma** delas é efetivamente aplicada ao banco de dados. Não é permitido que o banco de dados termine em um estado intermediário ou parcial.

**ATOMICIDADE → TODAS AS OPERAÇÕES REALIZADAS COM SUCESSO, OU NÃO HÁ TRANSAÇÃO**

O exemplo clássico para ilustrar a atomicidade é a transferência bancária. Imagine uma transação que consiste em transferir R$ 100 da conta corrente para a conta poupança de uma mesma pessoa. Esta operação de negócio envolve dois passos no banco de dados:

1. **Débito:** Subtrair R$ 100 do saldo da conta corrente.
2. **Crédito:** Adicionar R$ 100 ao saldo da conta poupança.

A atomicidade garante que esses dois passos sejam tratados como um bloco único. Se ocorrer uma falha (uma queda de energia, um erro de sistema) após o primeiro passo, mas antes do segundo, o princípio da atomicidade entra em ação. O SGBD detecta que a transação não foi concluída com sucesso e executa um **`ROLLBACK`** (reversão) automático, desfazendo a operação de débito. Isso garante que o dinheiro não seja "perdido" no sistema.

Na prática, é o SGBD que gerencia este processo. Quando uma transação começa, todas as suas alterações são mantidas em um estado temporário. Se qualquer erro ocorrer, todas essas alterações temporárias são descartadas. Somente se todas as operações forem executadas com sucesso e um comando **`COMMIT`** for emitido, é que as alterações são permanentemente gravadas no banco de dados. O comando `COMMIT`, portanto, é a instrução que confirma a conclusão bem-sucedida da unidade atômica, garantindo que o conjunto de operações seja aplicado de forma indivisível.

### Consistência

O princípio da **consistência** garante que qualquer transação levará o banco de dados de um **estado válido para outro estado válido**. Em outras palavras, a transação deve preservar a integridade e a coerência dos dados, assegurando que todas as regras, restrições e relacionamentos definidos no _schema_ do banco de dados sejam respeitados ao final da operação.

Se a atomicidade garante que a transação aconteça por inteiro ou não aconteça, a consistência garante que o resultado final dessa transação seja "correto" de acordo com as regras de negócio e de dados predefinidas.

**CONSISTÊNCIA → MANTER AS REGRAS E A INTEGRIDADE DOS DADOS APÓS UMA TRANSAÇÃO**

No contexto de sistemas modernos, especialmente com bancos de dados distribuídos, o conceito de consistência também se estende à **consistência de leitura**. Isso significa garantir que, após uma escrita ser concluída, todos os diferentes nós ou réplicas do banco de dados eventualmente leiam a mesma informação atualizada, evitando que diferentes usuários vejam dados conflitantes.

#### As Ferramentas da Consistência: Restrições (Constraints)

A consistência não é uma propriedade mágica; ela é ativamente imposta pelo SGBD através de um conjunto de regras chamadas **restrições (_constraints_)**. As restrições são definidas durante a criação das tabelas (com o comando `CREATE TABLE`) e garantem que qualquer tentativa de inserir, atualizar ou excluir dados que viole essas regras seja rejeitada pelo banco de dados.

As principais restrições que garantem a consistência são:

- **Restrição de Integridade de Domínio:** Esta é a restrição mais básica. Ela garante que os valores inseridos em uma coluna pertençam ao seu domínio definido. Isso inclui:
    - **Tipo de Dado:** Um campo definido como `INT` não pode aceitar o texto "abc". Um campo `DATE` não pode aceitar um valor inválido como '2025-02-30'.
    - **Intervalos ou Formatos:** Podemos definir regras mais específicas, como garantir que uma coluna `Nota` só aceite valores entre 0 e 10.
- **Restrição de Integridade de Entidade:** Esta restrição assegura que cada linha (entidade) em uma tabela seja única e identificável. Ela é imposta pela **chave primária (`PRIMARY KEY`)**. Ao definir uma coluna como chave primária, o SGBD garante automaticamente que nenhum valor nela seja `NULL` ou duplicado.
- **Restrição de Integridade Referencial:** Garante que os relacionamentos entre tabelas sejam sempre válidos e consistentes. É imposta pela **chave estrangeira (`FOREIGN KEY`)**.
    - **Exemplo:** Se a tabela `Pedidos` tem uma chave estrangeira `ID_Cliente` que referencia a tabela `Clientes`, esta restrição impede a inserção de um pedido com um `ID_Cliente` que não exista na tabela `Clientes`. Da mesma forma, impede a exclusão de um cliente se ele ainda tiver pedidos associados.
- **Restrição de Integridade de Vazio (`NOT NULL`):** Uma das restrições mais simples e importantes. Ela define que uma determinada coluna é de preenchimento obrigatório, não permitindo a inserção ou atualização de um registro com um valor nulo (`NULL`) naquele campo.
- **Restrição de Integridade de Unicidade (`UNIQUE`):** Garante que todos os valores em uma coluna (ou conjunto de colunas) sejam únicos, evitando duplicatas. É similar à chave primária, mas com uma diferença crucial: uma restrição `UNIQUE` **permite** a inserção de múltiplos valores nulos (pois `NULL` não é considerado igual a `NULL`), enquanto a chave primária não permite nulos.
    - **Exemplo:** Em uma tabela de `Usuarios`, a coluna `Email` deve ser única, mas podemos não saber o email de todos os usuários no momento do cadastro.
- **Restrição de Integridade de Checagem (`CHECK`):** Esta é uma restrição flexível que permite definir uma regra de negócio (uma condição lógica) que os valores de uma coluna devem satisfazer.
    - **Exemplo:** Em uma tabela `Produtos`, podemos criar uma restrição para garantir que o `Preco` seja sempre maior que zero (`CHECK (Preco > 0)`). Em uma tabela `Funcionarios`, podemos garantir que a `Data_Contratacao` não seja uma data no futuro.

### Isolamento

O princípio do **isolamento** é a propriedade que garante que múltiplas transações, executadas de forma concorrente (ao mesmo tempo), não interfiram umas nas outras. Do ponto de vista de uma transação individual, o isolamento cria a ilusão de que ela está sendo executada sozinha no sistema, em uma "bolha" protegida das atividades das demais.

Uma transação em andamento não pode ver os estados intermediários e ainda não confirmados (não "commitados") de outra transação. O objetivo do isolamento é proteger a integridade do banco de dados contra as inconsistências temporárias que podem surgir do acesso simultâneo aos mesmos dados.

**Analogia:** Imagine duas pessoas editando o mesmo parágrafo em um documento compartilhado. Sem isolamento, uma pessoa veria as frases incompletas e os erros de digitação da outra em tempo real. Com isolamento, uma pessoa só veria as alterações da outra depois que ela terminasse o parágrafo e o salvasse (fizesse o `COMMIT`).

A violação do isolamento pode levar a uma série de problemas de concorrência, como:

- **Leituras Sujas (_Dirty Reads_):** Ler dados de uma transação que ainda não foi confirmada.
- **Leituras Não Repetíveis (_Non-Repeatable Reads_):** Reler o mesmo dado e obter um valor diferente.
- **Leituras Fantasmas (_Phantom Reads_):** Reler um conjunto de dados e encontrar novas linhas que não existiam antes.

Como já vimos, os SGBDs controlam o rigor do isolamento através dos **Níveis de Isolamento** (`READ COMMITTED`, `SERIALIZABLE`, etc.), permitindo um equilíbrio entre a consistência dos dados e a performance do sistema.

### Durabilidade

O princípio da **durabilidade** é a garantia final do SGBD. Ele assegura que, uma vez que uma transação tenha sido confirmada com sucesso (`COMMIT`), suas alterações são **permanentes e persistentes** no banco de dados. Essas alterações sobreviverão a qualquer falha subsequente do sistema, como quedas de energia, erros no sistema operacional ou problemas de hardware.

**Analogia:** Pense na durabilidade como a tinta indelével. Uma vez que a informação é escrita e o `COMMIT` é dado, a alteração não pode ser apagada por uma falha de sistema.

**Exemplo prático:** Se um cliente realiza uma compra em um site de e-commerce e recebe a tela de "Pedido Confirmado", essa confirmação implica que um `COMMIT` foi executado no banco de dados. A durabilidade garante que, mesmo que o servidor do site sofra uma queda de energia um segundo após a confirmação, o registro daquele pedido não será perdido. Ao reiniciar, o banco de dados estará em um estado consistente que reflete a conclusão daquela compra.

#### Como a Durabilidade é Alcançada?

A durabilidade é geralmente implementada pelos SGBDs através de um mecanismo chamado **log de transações** (_transaction log_ ou _write-ahead log - WAL_). O processo funciona da seguinte forma:

1. Quando uma transação realiza uma alteração, essa alteração é primeiro escrita em um arquivo de log sequencial no disco.
2. Quando o comando `COMMIT` é emitido, o SGBD garante que os registros correspondentes no log de transações sejam gravados de forma permanente em um armazenamento não volátil (como um SSD ou HD).
3. Somente **após** a gravação no log ser confirmada é que o SGBD reporta à aplicação que a transação foi bem-sucedida.
4. As alterações nos arquivos de dados principais do banco podem ser feitas posteriormente, de forma otimizada.

Em caso de uma falha, ao reiniciar, o SGBD lê o log de transações e pode "refazer" (_redo_) qualquer transação que foi confirmada mas cujas alterações ainda não haviam sido aplicadas aos arquivos de dados principais, garantindo assim a permanência de todas as operações concluídas.

## Controle de Concorrência e Isolamento

Como vimos no princípio de Isolamento do ACID, um SGBD deve garantir que transações executadas simultaneamente não interfiram umas nas outras. O **controle de concorrência** é o conjunto de mecanismos e técnicas que o SGBD utiliza para implementar este isolamento na prática.

Em qualquer sistema do mundo real, é inevitável que múltiplos usuários e aplicações tentem ler e escrever nos mesmos dados ao mesmo tempo. Sem um controle de concorrência robusto, essas operações paralelas poderiam levar ao caos, resultando em dados inconsistentes, relatórios incorretos e corrupção da integridade do banco de dados.

### Anomalias de Concorrência: Os Efeitos da Falta de Isolamento

Um isolamento inadequado entre as transações pode dar origem a uma série de problemas conhecidos como anomalias de concorrência. É crucial entender essas anomalias para apreciar a importância dos mecanismos de controle.

<div align="center">
<img width="700px" src="./img/08-problemas-de-concorrencia.png">
</div>

- **Leitura Suja (Dirty Read):** Esta anomalia ocorre quando uma Transação A lê dados que foram modificados por uma Transação B que **ainda não foi confirmada (commitada)**.
    - **Exemplo:** A Transação B atualiza o preço de um produto para R$ 50,00. Antes do `COMMIT`, a Transação A lê este novo preço "sujo" de R$ 50,00 para gerar um relatório de vendas. Em seguida, a Transação B falha e executa um `ROLLBACK`, revertendo o preço para seu valor original. Agora, o relatório da Transação A está incorreto, pois foi baseado em um dado que, na prática, nunca existiu de forma permanente.
- **Leitura Não-Repetível (Non-repeatable Read):** Ocorre quando uma transação lê a **mesma linha** duas vezes, mas obtém valores diferentes porque uma outra transação alterou e confirmou essa linha no intervalo entre as duas leituras.
    - **Exemplo:** A Transação A lê o estoque de um produto e encontra 10 unidades. Enquanto a Transação A continua seu processamento, a Transação B vende 2 unidades daquele produto e confirma a alteração (`COMMIT`), atualizando o estoque para 8. Se a Transação A, dentro da mesma operação, reler o estoque daquele produto, ela agora encontrará 8 unidades. O mesmo dado lido duas vezes retornou valores diferentes, o que pode quebrar cálculos complexos.
- **Leitura Fantasma (Phantom Read):** Esta anomalia ocorre quando uma transação executa a mesma consulta com uma cláusula `WHERE` duas vezes, e o **conjunto de linhas** retornado é diferente. Isso acontece porque outra transação inseriu ou excluiu linhas que satisfazem a condição `WHERE` no intervalo entre as duas execuções.
    - **Exemplo:** A Transação A executa uma consulta para contar quantos funcionários existem no departamento de 'Vendas' e obtém o resultado `20`. Enquanto isso, a Transação B contrata um novo vendedor e confirma a inserção no banco. Se a Transação A repetir a mesma contagem, ela agora encontrará o resultado `21`. Uma nova linha "fantasma" apareceu no seu conjunto de resultados.

### Níveis de Isolamento do Padrão ANSI SQL

Para combater essas anomalias, os SGBDs implementam diferentes **níveis de isolamento**, conforme padronizado pela ANSI SQL. Cada nível oferece um grau diferente de proteção, representando um equilíbrio entre consistência e performance. Um nível mais alto de isolamento previne mais anomalias, mas pode reduzir a capacidade do sistema de lidar com muitas transações simultâneas (concorrência) e aumentar a chance de _deadlocks_.

> **Deadlock** (ou impasse) em bancos de dados é uma situação de bloqueio mútuo em que duas ou mais transações ficam permanentemente esperando por recursos que estão sendo mantidos umas pelas outras, impedindo que qualquer uma delas prossiga.

A tabela a seguir resume quais anomalias cada nível de isolamento previne. Um `X` vermelho indica que a anomalia é prevenida, enquanto um `V` verde indica que ela pode ocorrer.

<div align="center">
<img width="700px" src="./img/08-niveis-de-isolamento.png">
</div>

- **Read Uncommitted:** O nível mais baixo de isolamento. Permite a máxima concorrência, mas é vulnerável a todas as anomalias, incluindo leituras sujas. É raramente utilizado em sistemas transacionais.
- **Read Committed:** Garante que uma transação leia apenas dados que já foram confirmados por outras transações, **prevenindo as Leituras Sujas**. No entanto, as leituras não-repetíveis e fantasmas ainda podem ocorrer. É o nível de isolamento padrão na maioria dos SGBDs, como PostgreSQL, Oracle e SQL Server, por oferecer um bom equilíbrio.
- **Repeatable Read:** Um nível mais restritivo que garante que, se uma linha for lida múltiplas vezes dentro da mesma transação, ela sempre retornará os mesmos valores. **Previne Leituras Sujas e Não-Repetíveis**. As leituras fantasmas, no entanto, ainda são possíveis. É o nível padrão no MySQL (com o motor InnoDB).
- **Serializable:** O nível de isolamento mais alto e seguro. Ele garante que o resultado de transações concorrentes seja idêntico a alguma execução sequencial (em série) dessas mesmas transações. **Previne todas as anomalias**. Oferece a máxima consistência, mas ao custo de uma menor concorrência e, potencialmente, um desempenho mais lento.


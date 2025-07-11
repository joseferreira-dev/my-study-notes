# Capítulo 6 – Linguagem de Controle de Transação (TCL)

Nos capítulos anteriores, aprendemos a definir a estrutura dos dados (DDL) e a interagir com eles através de consultas e modificações (DML). Contudo, em um sistema de banco de dados real, onde múltiplos usuários e processos podem tentar alterar os dados simultaneamente, surge uma questão fundamental: como garantir que essas operações sejam realizadas de forma segura, consistente e confiável? A resposta reside no conceito de **Transação**.

Imagine uma tarefa simples como uma transferência de fundos entre duas contas bancárias. Essa única tarefa lógica, na verdade, consiste em múltiplas operações: (1) verificar se a conta de origem tem saldo suficiente, (2) subtrair o valor da conta de origem e (3) adicionar o valor à conta de destino. Se o sistema falhar entre o passo (2) e o (3), o dinheiro simplesmente desaparece, deixando o banco de dados em um estado corrompido e inconsistente. Uma transação é o mecanismo que trata esse bloco de operações como uma unidade de trabalho indivisível, garantindo que ou todos os passos sejam concluídos com sucesso, ou nenhum deles seja.

Este capítulo é dedicado à **Linguagem de Controle de Transação (TCL - Transaction Control Language)**, o conjunto de comandos que nos permite gerenciar o ciclo de vida e o comportamento dessas unidades de trabalho. Iremos explorar em profundidade as propriedades fundamentais que tornam as transações confiáveis (o padrão ACID), os comandos para efetivar ou reverter modificações (`COMMIT` e `ROLLBACK`), e os cruciais níveis de isolamento que governam como as transações interagem umas com as outras em um ambiente de alta concorrência.

## As Propriedades Fundamentais: ACID

A confiabilidade das transações em SGBDs modernos não é acidental; ela é o resultado da adesão a quatro propriedades rigorosas, conhecidas universalmente pelo acrônimo **ACID**. Essas propriedades são a garantia de que os dados permanecerão íntegros, mesmo diante de erros de sistema ou acessos concorrentes.

- **Atomicidade (Atomicity):** Esta propriedade garante que a transação seja uma unidade "atômica", ou seja, indivisível. Para o banco de dados, a transação é um evento único que acontece por completo ou não acontece de forma alguma. Se qualquer uma das operações dentro da transação falhar por qualquer motivo (seja um erro de sintaxe, uma violação de restrição ou uma falha de energia), todas as operações já executadas dentro daquela transação são completamente desfeitas (`ROLLBACK`). É o princípio do "tudo ou nada". O SGBD geralmente consegue isso mantendo um registro detalhado (log de transações) de todas as alterações, que pode ser usado para reverter o estado do banco.
- **Consistência (Consistency):** A consistência garante que uma transação só pode levar o banco de dados de um estado válido para outro. Antes do início da transação, o banco de dados está em um estado consistente, e após a sua conclusão (seja por `COMMIT` ou `ROLLBACK`), ele também deve estar. A transação não pode violar nenhuma das regras de integridade definidas no esquema, como chaves primárias, estrangeiras, restrições `UNIQUE` ou `CHECK`. Se uma operação tentar, por exemplo, inserir um registro com uma chave primária duplicada, a transação inteira falhará para manter a consistência.
- **Isolamento (Isolation):** Em um sistema onde múltiplas transações podem ocorrer simultaneamente, a propriedade de isolamento garante que elas não interfiram umas nas outras. Idealmente, cada transação deve ser executada como se estivesse sozinha no sistema, protegida dos efeitos intermediários e não concluídos de outras transações. Sem isolamento, uma transação poderia ler dados "sujos" que outra transação está no meio de alterar, levando a resultados caóticos e incorretos. Como o isolamento perfeito pode impactar o desempenho, o SQL oferece diferentes "níveis de isolamento" para calibrar essa proteção.
- **Durabilidade (Durability):** Esta propriedade garante que, uma vez que uma transação tenha sido efetivada com sucesso (`COMMIT`), suas alterações são permanentes e sobreviverão a falhas subsequentes do sistema. O SGBD garante a durabilidade ao escrever as alterações em um armazenamento não volátil (como um SSD ou disco rígido) e em seus logs de transação. Uma vez que o comando `COMMIT` é confirmado, pode-se ter a certeza de que os dados estão seguros.

## Gerenciando o Fluxo da Transação com a TCL

O padrão SQL fornece comandos específicos para controlar o início, o fim e os pontos intermediários de uma transação. Em muitos SGBDs, uma transação é iniciada implicitamente com o primeiro comando DML, mas é boa prática gerenciá-la explicitamente.

- **`START TRANSACTION` (ou `BEGIN TRANSACTION`)**: Inicia uma transação de forma explícita.
- **`COMMIT`**: Finaliza a transação com sucesso, tornando todas as suas modificações permanentes.
- **`ROLLBACK`**: Finaliza a transação com falha, desfazendo todas as modificações realizadas desde o seu início.

### Pontos de Salvamento Intermediários: `SAVEPOINT`

Para transações longas, reverter todo o trabalho por um erro pode ser ineficiente. `SAVEPOINT` cria um "marcador" para o qual se pode reverter parcialmente.

- **`SAVEPOINT nome_ponto`**: Cria um ponto de salvamento.
- **`ROLLBACK TO SAVEPOINT nome_ponto`**: Desfaz as alterações feitas após o `SAVEPOINT` especificado, permitindo que a transação continue de um estado conhecido.
- **`RELEASE SAVEPOINT nome_ponto`**: Remove um `SAVEPOINT` que não é mais necessário, liberando recursos do sistema. É útil em transações muito longas com múltiplos savepoints para evitar que o SGBD rastreie pontos de retorno desnecessários.

**Exemplo Prático de `SAVEPOINT`:**

```sql
BEGIN TRANSACTION;

-- Passo 1: Inserir um novo cliente
INSERT INTO clientes (id, nome) VALUES (5, 'Nova Empresa');
SAVEPOINT cliente_inserido;

-- Passo 2: Tentar inserir pedidos
INSERT INTO pedidos (id, id_cliente, valor) VALUES (101, 5, 500.00);
INSERT INTO pedidos (id, id_cliente, valor) VALUES (102, 5, -50.00); -- Erro detectado

-- Ao invés de um ROLLBACK completo, reverte apenas os pedidos
ROLLBACK TO SAVEPOINT cliente_inserido;

-- O cliente 5 ainda está na transação. Podemos tentar novamente com os dados corretos.
INSERT INTO pedidos (id, id_cliente, valor) VALUES (101, 5, 500.00);
INSERT INTO pedidos (id, id_cliente, valor) VALUES (102, 5, 150.00);

-- O savepoint não é mais necessário
RELEASE SAVEPOINT cliente_inserido;

COMMIT; -- Efetiva o cliente e seus pedidos corretos
```

## Configurando o Comportamento da Transação

O SQL permite configurar o comportamento de uma transação antes ou durante sua execução, usando os comandos `SET TRANSACTION` e `SET CONSTRAINTS`.

### `SET TRANSACTION`

Este comando, que deve ser a primeira instrução de uma nova transação, define suas características principais: o modo de acesso e o nível de isolamento.

- **Modo de Acesso:**
    - `READ WRITE` (padrão): A transação pode ler e modificar dados.
    - `READ ONLY`: A transação só pode ler dados. É uma otimização importante, pois sinaliza ao SGBD que ele não precisa se preocupar com bloqueios de escrita, melhorando a concorrência em consultas de relatório.

**Exemplo de Transação Somente Leitura:**

```sql
START TRANSACTION;
SET TRANSACTION READ ONLY;
-- Executa uma série de consultas complexas para gerar um relatório,
-- com a garantia de que nenhum dado será alterado.
SELECT * FROM vendas_consolidadas_trimestre;
COMMIT; -- Apenas encerra a transação.
```

### `SET CONSTRAINTS`

Por padrão, restrições de integridade (como `UNIQUE` ou `FOREIGN KEY`) são verificadas ao final de cada instrução DML. O comando `SET CONSTRAINTS` permite adiar (`DEFER`) essa verificação para o final da transação (no momento do `COMMIT`). Isso é útil em cenários complexos onde uma restrição precisa ser temporariamente violada para se chegar a um estado final consistente. Para que isso funcione, a restrição deve ter sido criada com a propriedade `DEFERRABLE`.

Sintaxe:

```sql
SET CONSTRAINTS { ALL | nome_da_restricao [, ...] } { DEFERRED | IMMEDIATE };
```

**Exemplo:** Suponha que a coluna `gerente_id` na tabela `departamentos` tenha uma restrição `UNIQUE DEFERRABLE` (um gerente não pode liderar dois departamentos). Para trocar os gerentes de dois departamentos (Marketing e Vendas), precisaríamos de uma verificação adiada.

```sql
START TRANSACTION;
SET CONSTRAINTS uk_gerente DEFERRED; -- Adia a verificação da constraint de unicidade.

-- Troca dos gerentes. Se a checagem fosse imediata, a primeira linha falharia.
UPDATE departamentos SET gerente_id = 101 WHERE nome = 'Marketing'; -- O gerente 101 era de Vendas
UPDATE departamentos SET gerente_id = 102 WHERE nome = 'Vendas';    -- O gerente 102 era de Marketing

-- O estado final é consistente.
COMMIT; -- A constraint uk_gerente é verificada aqui. Como está tudo OK, a transação é efetivada.
```

## Controle de Concorrência: Níveis de Isolamento

O nível de isolamento, definido via `SET TRANSACTION ISOLATION LEVEL ...`, é a configuração mais crítica para o controle de concorrência. Ele define o grau em que uma transação é protegida dos efeitos de outras.

- **`READ UNCOMMITTED`:** Permite a **Leitura Suja (Dirty Read)**. Uma transação pode ler dados modificados por outra que ainda não foi efetivada.
- **`READ COMMITTED`:** Previne a Leitura Suja. Contudo, é vulnerável à **Leitura Não Repetível (Non-Repeatable Read)**, onde a mesma consulta pode retornar resultados diferentes dentro da mesma transação se outra transação alterar os dados no meio do caminho.
- **`REPEATABLE READ`:** Previne a Leitura Não Repetível. No entanto, ainda pode sofrer com a **Leitura Fantasma (Phantom Read)**, onde novas linhas inseridas por outra transação aparecem em consultas subsequentes.
- **`SERIALIZABLE`:** O nível mais rigoroso. Garante que o resultado da execução simultânea seja idêntico a alguma execução sequencial das mesmas transações, prevenindo todos os fenômenos de concorrência.

A escolha do nível de isolamento é um balanço crucial entre a garantia de consistência e a necessidade de performance e alta concorrência.

## Considerações Finais

Neste capítulo, mergulhamos em um dos conceitos mais críticos para a confiabilidade de qualquer sistema de banco de dados: a **Transação**. Vimos que, através da **Linguagem de Controle de Transação (TCL)**, podemos agrupar operações SQL em unidades de trabalho lógicas que aderem a um contrato de confiança estrito, definido pelas propriedades **ACID**. A atomicidade, a consistência, o isolamento e a durabilidade não são apenas termos teóricos, mas as garantias práticas de que nossos dados permanecerão íntegros e corretos, mesmo diante de falhas de sistema e acesso concorrente.

Exploramos as ferramentas práticas da TCL, compreendendo o papel definitivo de `COMMIT` para tornar as alterações permanentes e de `ROLLBACK` para descartá-las completamente. Analisamos o uso estratégico de `SAVEPOINT` para criar pontos de retorno parciais em transações complexas e detalhamos os comandos de configuração, como `SET TRANSACTION` e `SET CONSTRAINTS`, que nos permitem ajustar o comportamento transacional. Além disso, dissecamos os **níveis de isolamento**, um conceito fundamental para o controle de concorrência, entendendo os fenômenos que cada nível previne — desde leituras sujas até registros fantasmas — e o balanço inerente entre consistência e desempenho.

Tendo dominado os pilares da DDL, DML e agora TCL, possuímos uma visão abrangente sobre como estruturar, manipular e garantir a integridade dos dados. O nosso foco agora se desloca da **correção** das operações para a sua **eficiência**. Uma consulta que produz o resultado correto, mas leva horas para ser executada, tem pouco valor prático. Os próximos capítulos nos levarão ao domínio da otimização, explorando os mecanismos que garantem o alto desempenho, como **índices**, **planos de execução** e as melhores práticas para escrever SQL de forma eficaz e escalável.
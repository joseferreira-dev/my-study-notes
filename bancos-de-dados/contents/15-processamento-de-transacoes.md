# Capítulo 15 – Processamento de Transações, Controle de Concorrência e Recuperação

## Processamento de Transações

O processamento de transações trata da capacidade que um banco de dados tem de lidar com **operações que representam unidades lógicas de trabalho**. Tais operações **devem ser executadas integralmente ou, em caso de falha, revertidas completamente**, de forma a manter o banco em um estado consistente.

Para entendermos melhor, imagine uma transação bancária. Suponha que um cliente transfira dinheiro de uma conta para outra. Este processo envolve, no mínimo, duas operações: subtrair um valor de uma conta e adicionar este valor à outra. Ambas as operações precisam ocorrer juntas para manter o sistema correto. Se apenas uma for concluída e a outra falhar, surgirá uma inconsistência grave no banco de dados.

A correta implementação de mecanismos de controle de transações protege o banco contra falhas, perdas de dados e inconsistências, garantindo que tudo ocorra da maneira esperada mesmo diante de problemas como quedas de energia, falhas de hardware ou erros de software.

Na figura a seguir, podemos visualizar de forma resumida os elementos básicos envolvidos no ciclo de uma transação:

<div align="center">
  <img width="680px" src="./img/15-transacao-elementos-basicos.png">
</div>

### O Problema

O mundo real é movido por transações. No contexto empresarial ou pessoal, uma transação corresponde a qualquer evento no qual ocorre uma troca de valores, informações ou serviços. Isso inclui compras, vendas, transferências financeiras, atualizações cadastrais, reservas, entre outros.

Os sistemas de processamento de transações têm uma missão desafiadora. Eles devem garantir:

- Alta disponibilidade e confiabilidade.
- Processamento eficiente de grandes volumes de dados.
- Segurança contra erros causados por operações simultâneas.
- Prevenção contra registros inconsistentes ou parciais.
- Capacidade de recuperação após falhas, sem perda de dados.
- Crescimento escalável e distribuído.

Por trás de toda essa robustez estão tecnologias como **logs**, **bloqueios**, **controle de concorrência** e **mecanismos de recuperação**, que estudaremos com profundidade ao longo deste capítulo.

### O Que é uma Transação?

De forma conceitual, uma **transação** é uma **unidade lógica de trabalho**, composta por um ou mais comandos que realizam leitura e/ou modificação de dados no banco. Ela é executada de forma atômica, ou seja, ou todas as suas operações são completadas com sucesso, ou nenhuma delas é aplicada.

O sistema responsável por garantir a execução correta de uma transação dentro do SGBD é conhecido como **gerenciador de transações**, que faz parte do núcleo do banco de dados.

Cada transação passa por um ciclo bem definido, no qual é iniciada, executada e, dependendo do sucesso ou falha de suas operações, pode ser **confirmada** (commit) ou **desfeita** (rollback).

Para que um banco de dados seja confiável e robusto, suas transações devem obedecer a quatro propriedades fundamentais, conhecidas pela sigla **ACID**, formada pelas iniciais dos termos em inglês:

<div align="center">
  <img width="640px" src="./img/15-propriedades-acid.png">
</div>

- **Atomicidade (A)** – A transação deve ser executada integralmente ou não ser executada. Se algum erro ocorrer no meio da execução, todas as operações já realizadas são desfeitas. O banco retorna ao estado anterior ao início da transação.
- **Consistência (C)** – O banco de dados deve passar de um estado consistente para outro estado igualmente consistente. As regras de integridade (chaves, restrições, relacionamentos) devem ser sempre preservadas.
- **Isolamento (I)** – As operações de uma transação não devem ser visíveis para outras transações até que sejam confirmadas. Isso garante que transações concorrentes não interfiram umas nas outras.
- **Durabilidade (D)** – Uma vez que uma transação é confirmada, suas alterações no banco de dados não podem ser perdidas, mesmo em caso de falhas no sistema.

Essas propriedades são essenciais para garantir que o banco de dados reflita a realidade do mundo de forma correta e consistente.

### O Fluxo de uma Transação

Para entender como uma transação se comporta internamente dentro do SGBD, é necessário compreender o seu fluxo, representado por uma **máquina de estados**, que mostra os possíveis estados de uma transação ao longo de sua execução.

<div align="center">
  <img width="640px" src="./img/15-estados-de-uma-transacao.png">
</div>

Este ciclo pode ser descrito da seguinte maneira:

1. **Ativa (Active)** – A transação inicia sua execução após o comando `BEGIN TRANSACTION`. Neste estado, ela realiza operações de leitura (`read`) e escrita (`write`) nos dados.
2. **Parcialmente Efetivada (Partially Committed)** – Ao concluir suas operações, a transação passa para este estado quando executa o `END TRANSACTION`. Aqui, as operações estão registradas no log, mas ainda não foram efetivamente aplicadas ao banco de dados.
3. **Efetivada (Committed)** – Se tudo ocorrer bem, a transação executa um `COMMIT`, consolidando permanentemente todas as mudanças no banco. A partir daqui, as alterações são definitivas e não podem ser revertidas.
4. **Falha (Failed)** – Caso ocorra algum problema durante a execução (como queda de energia, erro de sistema ou falha de integridade), a transação entra no estado de falha. Suas operações são desfeitas por meio de um `ROLLBACK` automático ou manual.
5. **Encerrada (Terminated)** – A transação deixa o sistema, estando completamente finalizada, seja por sucesso (commit) ou por falha (rollback).

### O Log do Sistema

O **log de transações**, também chamado de **journal**, é um dos principais mecanismos utilizados pelos SGBDs para garantir a atomicidade e durabilidade das transações.

Este log é **um arquivo persistente que registra**, em ordem cronológica, **todos os eventos significativos realizados pelas transações**, incluindo operações de leitura, escrita, início, commit e rollback.

O log **deve ser armazenado em um meio não volátil**, que não seja afetado por falhas do sistema, garantindo que as informações nele contidas estejam sempre disponíveis para recuperação do banco de dados em caso de falhas.

Um ponto extremamente relevante sobre o log é que, além de permitir a recuperação das transações, **ele também é capaz de fornecer rastreabilidade e auditoria**, permitindo identificar quais usuários acessaram ou alteraram determinados dados.

As informações registradas no log geralmente seguem o seguinte padrão:

- `[start_transaction, T]` → Indica o início da transação T.
- `[write_item, T, X, old_value, new_value]` → Registra a alteração de um item X pela transação T, especificando o valor antigo e o novo valor.
- `[read_item, T, X]` → (Opcional) Indica que o item X foi lido pela transação T.
- `[commit, T]` → Marca que a transação T foi efetivada.
- `[abort, T]` → Indica que a transação T foi desfeita.

Dependendo do tipo de controle de transações utilizado, os registros de leitura (`read_item`) podem ou não ser obrigatórios. Eles são fundamentais, por exemplo, quando se deseja manter logs de auditoria.

O log também permite que sejam realizadas duas operações fundamentais para a recuperação de dados:

- **UNDO (Desfazer)** – Usado quando uma transação falha. Permite desfazer as alterações feitas por essa transação, retornando os dados aos valores anteriores.
- **REDO (Refazer)** – Usado quando uma transação foi confirmada (`commit`), mas as alterações ainda não haviam sido fisicamente aplicadas no banco no momento de uma falha. O REDO garante que estas alterações sejam reaplicadas corretamente.

Uma característica importante dessas operações é que ambas são consideradas **idempotentes**, ou seja, podem ser aplicadas várias vezes, e sempre produzirão o mesmo resultado, sem efeitos colaterais adicionais.

Imagine, por exemplo, uma alteração salarial onde o salário de um funcionário é atualizado de R$ 10.000 para R$ 20.000. Se uma falha acontecer após o commit, mas antes da escrita definitiva no banco, a operação de REDO garantirá que o salário seja efetivamente atualizado para R$ 20.000, mesmo que essa operação precise ser aplicada mais de uma vez durante o processo de recuperação.

## Ponto de Efetivação: Garantindo a Confiabilidade das Modificações

Em um ambiente de banco de dados robusto, é fundamental garantir que, ao término de uma transação, as alterações realizadas sobre os dados estejam verdadeiramente consolidadas. Essa consolidação ocorre no chamado **ponto de efetivação** (commit point), um marco lógico e técnico fundamental no ciclo de vida de uma transação.

Dizemos que uma transação T alcançou seu ponto de efetivação quando **todas as suas operações que acessam o banco de dados foram executadas com sucesso** e **os efeitos dessas operações estão completamente registrados no log do sistema**. A partir desse momento, a transação é considerada efetivada, e seu efeito torna-se permanente e irrevogável.

Considere o seguinte cenário: se ocorrer uma falha no sistema e uma transação tiver o registro `[start_transaction, T]` no log, mas não tiver o correspondente `[commit, T]`, essa transação **deve ser revertida** — ou seja, todas as alterações feitas por ela são desfeitas, garantindo a atomicidade. Por outro lado, se o log já contiver o registro de commit, então a transação **deve ser refeita**, se necessário, para garantir que suas modificações estejam refletidas no banco.

Esse controle depende de uma estrutura crítica chamada **checkpoint**.

### Checkpoints: Salvaguardando o Estado do Banco

A operação de checkpoint consiste em um **marco periódico de segurança** no log de transações. Ele representa o momento em que o sistema grava, de maneira forçada, todas as alterações pendentes no disco. Esse mecanismo visa garantir que, mesmo em caso de falhas, a recuperação do banco possa ser feita de maneira eficiente, sem precisar varrer um log interminável desde o seu início.

Durante o processo de checkpoint, o SGBD executa as seguintes ações:

1. **Suspende temporariamente a execução de transações**;
2. **Grava em disco todas as modificações realizadas por transações efetivadas** (esvaziando os buffers de memória);
3. **Insere no log um registro `[checkpoint]`**, que pode conter informações adicionais como:
    - Identificadores das transações ativas;
    - Endereços do primeiro e do último registro no log para cada transação;
4. **Força a escrita do log em disco**;
5. **Retoma a execução normal das transações**.

Esse procedimento assegura que o sistema tenha pontos de restauração consistentes, minimizando o tempo e os recursos exigidos em processos de recuperação.

## Planos de Execução: Definindo a Ordem das Operações

Em sistemas concorrentes, várias transações podem ser executadas simultaneamente. Esse paralelismo traz benefícios em desempenho, mas também desafios. A **ordem de execução** das operações dessas transações precisa ser gerenciada com precisão para garantir que o resultado final seja o mesmo de uma execução sequencial.

Essa ordenação das operações é denominada **plano de execução**, ou **escalonamento**. Um plano de execução S, formado por um conjunto de transações T₁, T₂, ..., Tₙ, deve preservar a **ordem interna** de cada transação, ou seja, a sequência de operações originalmente programadas dentro de cada T<sub>i</sub> deve ser respeitada.

No entanto, operações de transações distintas podem ser **intercaladas**, desde que não haja conflito entre elas. Mas o que caracteriza um conflito?

Duas operações de transações diferentes entram em **conflito** quando:

1. Pertencem a transações distintas;
2. Acessam o mesmo item de dado X;
3. Pelo menos uma delas modifica o item (ou seja, é uma operação de escrita).

Por exemplo, se a transação T₁ lê o item X e T₂ deseja escrevê-lo, ou vice-versa, há conflito.

### Planos Completos e as Condições de Validade

Para que um plano de execução seja considerado **completo**, ele deve satisfazer três condições fundamentais:

1. **Encerramento** – Todas as transações no plano devem ser finalizadas com `commit` ou `abort`;
2. **Ordenação** – A ordem original das operações dentro de cada transação deve ser preservada no plano;
3. **Conflito** – Toda operação conflitante entre transações deve ter uma ordem de execução claramente definida.

Observe que, se **duas operações não forem conflitantes**, a ordem entre elas no plano pode ser flexível, caracterizando o que chamamos de **ordenação parcial**.

Esse modelo flexível permite que o sistema otimize a execução das transações, mantendo a consistência e respeitando a independência lógica entre elas.

### Planos Restauráveis: Evitando o Caos Pós-Falha

Imagine que duas transações estão sendo executadas em paralelo. A transação T₂ lê um valor escrito por T₁. T₂ realiza mais algumas operações e então confirma sua execução com um `commit`. Logo em seguida, T₁ sofre uma falha.

Nesse cenário, temos um problema: T₂ depende de um valor produzido por T₁, que não foi efetivado. T₂, então, **construiu seu raciocínio em cima de uma informação que será descartada**. Isso compromete a integridade dos dados.

É justamente para evitar esse tipo de erro que os **planos restauráveis** são importantes. Um plano é considerado restaurável se, **sempre que uma transação T<sub>j</sub> depender de um valor escrito por T<sub>i</sub>, o commit de T<sub>i</sub> ocorre antes do commit de T<sub>j</sub>**.

Em outras palavras, **nenhuma transação deve se comprometer (fazer commit) com base em dados não confirmados por outras transações**.

Se essa regra for desrespeitada, o sistema poderá sofrer uma **reversão em cascata**: uma falha em uma transação pode provocar a anulação de várias outras, formando um efeito dominó.

Para tornar um plano livre de reversão em cascata, uma regra simples pode ser aplicada: **uma transação só pode ler dados que já foram gravados por transações que executaram o commit**.

Essa ideia está relacionada ao **nível de isolamento “read committed”**, onde os dados lidos sempre vêm de transações confirmadas. Assim, mesmo que uma transação falhe, não haverá necessidade de desfazer outras transações que apenas consumiram dados já válidos.

A figura a seguir ilustra esse conceito, contrastando um plano restaurável com um não restaurável.

<div align="center">
  <img width="780px" src="./img/15-planos-restauravel-e-nao-restauravel.png">
</div>

## Serialidade (Serializability)

Ao longo deste capítulo, já compreendemos que o processamento de transações não é simplesmente uma questão de executar comandos de leitura e escrita sobre o banco de dados. Na prática, múltiplas transações podem ser executadas simultaneamente, compartilhando acesso aos mesmos dados. Essa concorrência, embora fundamental para o desempenho dos sistemas modernos, traz consigo riscos significativos para a integridade e a consistência dos dados, caso não seja adequadamente controlada.

Neste contexto, surge um conceito central na teoria dos bancos de dados: a **serialidade**, ou em inglês, **serializability**. A serialidade é uma propriedade teórica que permite garantir que, mesmo executando diversas transações concorrentemente, o resultado final no banco de dados seja equivalente ao resultado que obteríamos se essas mesmas transações fossem executadas sequencialmente, uma de cada vez, sem nenhuma sobreposição.

De maneira simples, podemos dizer que um **plano de execução é serial** quando todas as operações de uma transação são executadas completamente, do início ao fim, antes que qualquer operação de outra transação comece. Em outras palavras, apenas uma transação está ativa por vez, sem nenhuma intercalação de operações. Embora esse modelo seja o ideal do ponto de vista da integridade dos dados, ele se torna impraticável na maioria dos cenários do mundo real, já que elimina os benefícios da concorrência, prejudicando severamente o desempenho dos sistemas.

Por isso, na prática, permitimos que as operações das transações sejam intercaladas. No entanto, essa intercalação não pode ser feita de maneira arbitrária. Surge então a necessidade de estabelecer restrições que assegurem que esse plano intercalado seja **serializável**, isto é, que seu resultado final seja equivalente ao de algum plano serial possível.

### A Importância da Serialidade

Para ilustrar, imagine uma situação muito comum em um sistema bancário. Suponha que duas transações, T₁ e T₂, desejam, ao mesmo tempo, realizar saques de R$100,00 na mesma conta, que possui exatamente R$100,00 de saldo.

Se ambas as transações lerem o saldo da conta antes que qualquer uma delas atualize o valor, ambas concluirão que há saldo suficiente e prosseguirão com o saque. O resultado? A conta, que possuía apenas R$ 100,00, ficará com saldo negativo de R$ -100,00, uma violação grave das regras do negócio e da integridade dos dados.

Perceba que ambas as transações foram executadas até o fim. Portanto, do ponto de vista da atomicidade, elas estão corretas — ou seja, cada uma foi executada de forma completa ou não executada. No entanto, elas falharam em respeitar o **isolamento**, que é outra propriedade fundamental das transações. A execução simultânea dessas transações gerou um resultado que jamais ocorreria em uma execução serial, onde a primeira transação executada zeraria o saldo e impediria que a segunda transação prosseguisse.

A **serialidade**, portanto, não se confunde com atomicidade. Enquanto a atomicidade garante que cada transação seja concluída por inteiro ou desfeita por completo, a serialidade assegura que a execução conjunta de diversas transações, mesmo quando concorrente, preserve a consistência do banco de dados como se essas transações tivessem sido executadas sequencialmente.

### Serialidade e a Propriedade de Isolamento

Existe uma relação direta entre a propriedade de isolamento, uma das quatro propriedades ACID das transações, e a serialidade. Dizemos que um sistema oferece isolamento total se todo plano de execução concorrente for serializável. Isso significa que, do ponto de vista de qualquer transação, é como se ela estivesse sendo executada isoladamente, sem interferência das outras.

Portanto, garantir serialidade é uma forma de garantir isolamento, e, por consequência, de assegurar que o banco de dados permaneça sempre em estados consistentes, independentemente da ordem em que as transações sejam executadas.

### Equivalência de Planos: Como Avaliar a Serialidade

Para determinar se um plano de execução é serializável, é necessário definir critérios de equivalência entre planos.

#### Equivalência de Resultado

Dois planos são considerados equivalentes por resultado se, ao final da execução, o estado do banco de dados for idêntico nos dois casos. Esse critério é bastante intuitivo, mas, na prática, pode ser difícil de verificar, já que exige a execução completa dos planos para comparação.

#### Equivalência de Conflito

Na prática, utiliza-se mais frequentemente o conceito de **equivalência de conflito**, que foca na ordem das operações que podem gerar efeitos diferentes se forem executadas fora de ordem.

Duas operações estão em conflito se:

1. Pertencem a transações diferentes;
2. Acessam o mesmo item de dado;
3. Pelo menos uma dessas operações é uma escrita.

Se dois planos preservam a mesma ordem relativa para todas as operações em conflito, dizemos que eles são equivalentes por conflito.

Dessa forma, um plano é considerado **serializável** se ele é equivalente, por conflito, a algum plano serial possível.

#### Equivalência de Visão

Outro conceito importante é a **equivalência de visão**, que, embora menos restritivo que a equivalência de conflito, ainda assegura resultados consistentes.

Dois planos são equivalentes por visão se:

- As mesmas transações participam dos dois planos;
- Cada operação de leitura lê o mesmo valor em ambos os planos;
- As últimas operações de escrita em cada item de dado são as mesmas nos dois planos.

Em outras palavras, do ponto de vista dos dados que são lidos e dos valores finais armazenados, ambos os planos são indistinguíveis.

### O Grafo de Precedência: Ferramenta para Análise de Serialidade

Uma das ferramentas mais poderosas para verificar se um plano é serializável é o **grafo de precedência**.

<div align="center">
  <img width="240px" src="./img/15-grafo-de-precedencia.png">
</div>

O grafo é construído da seguinte forma:

1. Para cada transação T<sub>i</sub> no plano, criamos um nó no grafo;
2. Sempre que uma operação de leitura de T<sub>j</sub> ocorre depois de uma operação de escrita de T<sub>i</sub> sobre o mesmo item, adicionamos uma aresta T<sub>i</sub> → T<sub>j</sub>;
3. O mesmo ocorre se uma escrita de T<sub>j</sub> ocorre após uma leitura ou escrita de T<sub>i</sub> sobre o mesmo item;
4. Se, após a construção do grafo, **não houver ciclos**, o plano é considerado serializável;
5. Se houver um ciclo, isso indica uma dependência circular que impede a equivalência com qualquer plano serial — logo, o plano não é serializável.

### Serialidade na Prática: Mecanismos de Controle

O que os bancos de dados fazem, então, para garantir a serialidade?

A resposta está nos **protocolos de controle de concorrência**, como:

- Protocolo de bloqueio em duas fases (2PL);
- Timestamp Ordering;
- Serialização baseada em vetores ou versões.

Esses mecanismos têm como objetivo garantir que, mesmo permitindo a execução concorrente, o sistema mantenha a ilusão de um processamento serial, protegendo os dados contra inconsistências e anomalias.

## Suporte a Transações em SQL

Ao longo dos tópicos anteriores, desenvolvemos uma sólida compreensão conceitual sobre o que são transações e como elas se comportam dentro do Sistema Gerenciador de Banco de Dados (SGBD). Agora é o momento de entender como o SQL, a principal linguagem de comunicação com os SGBDs relacionais, implementa o suporte prático às transações.

Desde as primeiras versões da norma SQL ANSI, o conceito de transação foi introduzido como uma estrutura fundamental para garantir a consistência e a integridade dos dados. E isso não mudou. Pelo contrário, ao longo dos anos, os mecanismos de transação foram aperfeiçoados e são hoje um dos pilares dos bancos de dados modernos.

### O Papel das Transações em SQL

Uma transação, dentro do contexto SQL, é definida como uma **unidade lógica de trabalho**, composta por um conjunto de operações que devem ser executadas de forma atômica. Isso significa que ou todas as operações dentro da transação são efetivadas no banco de dados, ou nenhuma delas será. É essa propriedade que protege o banco contra inconsistências, especialmente em ambientes onde múltiplos usuários ou processos atuam simultaneamente.

Sempre que um comando de modificação é emitido — como um `INSERT`, `UPDATE` ou `DELETE` — esse comando faz parte de uma transação. Por padrão, a maioria dos SGBDs adota uma política de **autocommit**, ou seja, cada comando individual é tratado como uma transação que é automaticamente confirmada após sua execução. No entanto, o controle explícito de transações é altamente recomendado sempre que há múltiplas operações interdependentes que precisam ser realizadas de forma conjunta e segura.

Os principais comandos de gerenciamento de transações do SQL padrão estão listados na tabela abaixo:

| Comando | Descrição |
|---|---|
| START (BEGIN) TRANSACTION | Inicializa uma transação SQL e seta as suas características. |
| SET TRANSACTION | Determina as propriedades da próxima transação SQL para o SQL Agent. São três os parâmetros: área de diagnóstico, nível de isolamento e modos de acesso (read/write). |
| SET CONSTRAINTS | Se a transação SQL estiver executando, estabelece o modo de restrições para a transação SQL na sessão corrente. Se não existe nenhuma transação em andamento na sessão, determina ao SQL Agent o modo de execução para a próxima transação. |
| SAVEPOINT | Estabelece um savepoint, ponto intermediário da transação para o qual o rollback deve retornar em caso de falha. |
| RELEASE SAVEPOINT | Destrói um savepoint |
| COMMIT | Termina a transação SQL corrente com um commit |
| ROLLBACK | Termina a transação corrente com um rollback, ou desfaz todas as modificações até o último savepoint. |

### Estrutura e Controle de Transações no SQL

O SQL oferece comandos específicos para que o desenvolvedor tenha controle total sobre o ciclo de vida de uma transação. Estes comandos estabelecem o ponto de início, permitem definir pontos intermediários, e controlam o commit ou rollback da transação.

Assim, uma transação normalmente é iniciada com um comando `BEGIN TRANSACTION` (ou `START TRANSACTION`, dependendo do SGBD) e segue seu fluxo até ser concluída com sucesso por meio de um `COMMIT`, ou, em caso de erro, ser revertida com um `ROLLBACK`.

Por exemplo, imagine um cenário em que uma empresa deseje inserir um novo funcionário no sistema e, simultaneamente, atualizar o salário de todos os empregados de um determinado departamento. Ambas as ações devem ocorrer juntas: se uma falhar, a outra não deve ser aplicada.

O controle dessa operação é feito da seguinte forma:

```sql
BEGIN TRANSACTION;

INSERT INTO EMPREGADO (PNOME, UNOME, SSN, DNO, SALARIO)
VALUES ('Thiago', 'Cavalcanti', 000457878, 2, 12000);

UPDATE EMPREGADO
SET SALARIO = SALARIO * 1.1
WHERE DNO = 2;

COMMIT;
```

Caso ocorra algum erro no meio da execução — por exemplo, uma violação de integridade ou uma falha no sistema — a transação poderia ser revertida com:

```sql
ROLLBACK;
```

### Savepoints: Controle Intermediário

Uma funcionalidade adicional oferecida pelo SQL é a criação de **savepoints**, ou pontos de salvamento dentro de uma transação. Eles permitem que o desenvolvedor defina marcos intermediários e, em caso de erro, execute um rollback apenas até o savepoint, e não necessariamente desfazendo toda a transação.

Imagine o seguinte cenário: dentro de uma transação, você realiza uma série de operações. Uma parte delas é sensível e pode apresentar risco de falha, enquanto outras partes estão corretas e você deseja preservá-las, mesmo se ocorrer um problema.

O uso do savepoint seria assim:

```sql
BEGIN TRANSACTION;

SAVEPOINT inicio;

INSERT INTO EMPREGADO (PNOME, UNOME, SSN, DNO, SALARIO)
VALUES ('Thiago', 'Cavalcanti', 000457878, 2, 12000);

SAVEPOINT depois_do_insert;

UPDATE EMPREGADO
SET SALARIO = SALARIO * 1.1
WHERE DNO = 2;

-- Suponha que esse update gere um erro, podemos fazer:
ROLLBACK TO depois_do_insert;

COMMIT;
```

Perceba que, com isso, preservamos a inserção do novo funcionário e apenas desfazemos as alterações nos salários.

### Níveis de Isolamento em SQL

Ao permitir que várias transações sejam executadas simultaneamente, surge um desafio: como garantir que uma transação não interfira nos dados de outra de maneira indevida? A resposta está nos **níveis de isolamento**, que definem até que ponto uma transação pode ser afetada pelas ações de outras transações em execução.

O SQL ANSI define quatro níveis de isolamento, cada um com diferentes garantias de integridade e diferentes impactos sobre o desempenho:

- **Read Uncommitted:** Este é o nível mais baixo de isolamento. Permite que uma transação leia dados que foram modificados por outra transação, mas que ainda não foram confirmados. Isso pode gerar o fenômeno chamado de **leitura suja (dirty read)**, onde dados temporários ou possivelmente inválidos são visualizados.
- **Read Committed:** Neste nível, uma transação só pode ler dados que já foram confirmados (**committed**) por outras transações. Elimina o problema da leitura suja, mas ainda permite outro tipo de problema chamado **leitura não repetível**, quando um mesmo dado retorna valores diferentes em leituras subsequentes dentro da mesma transação.
- **Repeatable Read:** Aqui, uma transação garante que, se ela leu um dado uma vez, qualquer leitura subsequente daquele dado retornará exatamente o mesmo valor. Elimina a leitura não repetível. Entretanto, ainda permite a ocorrência de **phantom reads**, ou **leituras fantasmas**, quando novos registros são inseridos por outras transações durante a execução.
- **Serializable:** Este é o nível mais alto de isolamento e garante uma execução completamente serializável. Ou seja, o efeito das transações será o mesmo que seria obtido se elas fossem executadas uma após a outra, sem nenhuma sobreposição. Esse nível elimina todos os tipos de anomalias, mas tem um custo de desempenho consideravelmente mais alto, pois impõe o máximo de restrições sobre o acesso concorrente.

Para deixar ainda mais claro, vejamos uma breve explicação dos problemas que os níveis de isolamento tentam resolver:

- **Leitura Suja (Dirty Read):** Ocorre quando uma transação lê dados que foram modificados por outra transação que ainda não foi confirmada. Se essa outra transação for revertida, a primeira terá lido um dado inválido.
- **Leitura Não Repetível (Non-Repeatable Read):** Acontece quando uma transação lê um dado, e antes que ela o leia novamente, outra transação modifica esse mesmo dado e confirma a alteração.
- **Leitura Fantasma (Phantom Read):** Surge quando, durante uma transação, são feitas duas consultas idênticas com condições de filtro, e entre essas duas consultas outra transação insere ou remove linhas que atendem aos mesmos critérios de seleção.

A seguir temos um quadro que resume quais problemas são evitados em cada nível de isolamento:

<div align="center">
  <img width="800px" src="./img/15-niveis-de-isolamento-e-violacoes.png">
</div>

### Modo de Acesso e Área de Diagnóstico

Além dos níveis de isolamento, o SQL permite configurar dois outros aspectos relevantes para o processamento das transações:

- **Modo de Acesso:** A transação pode ser declarada como **READ ONLY**, quando não fará modificações no banco, ou como **READ WRITE**, quando poderá tanto consultar quanto alterar os dados. Essa declaração pode ser usada para otimização, já que o SGBD pode aplicar algoritmos menos custosos para transações apenas de leitura.
- **Área de Diagnóstico:** Essa configuração define o tamanho da área de memória usada para armazenar informações sobre erros, exceções e avisos que ocorrem durante a execução das operações SQL dentro da transação. Permite, assim, que o programa que executa a transação possa tomar decisões mais inteligentes e robustas frente aos erros.


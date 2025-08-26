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

## Controle de Concorrência

### A Importância do Controle de Concorrência

Vivemos em um mundo no qual os sistemas são, em sua grande maioria, multiusuário. Desde o simples acesso simultâneo a uma plataforma de streaming até operações mais críticas, como reservas de passagens aéreas, sistemas de inventário, plataformas bancárias e redes sociais. Todos esses ambientes compartilham uma necessidade fundamental: permitir que várias operações sejam realizadas sobre os mesmos dados, de maneira simultânea, sem que haja perda de integridade ou inconsistências no banco de dados.

Imagine um sistema de reservas de passagens aéreas, no qual vários agentes, tanto nas agências físicas quanto nos sites, estão tentando reservar o mesmo assento em um voo. Ou pense em uma indústria onde vários funcionários, de diferentes setores, estão ao mesmo tempo atualizando os registros de estoque. Se cada operação não for cuidadosamente coordenada, o sistema pode acabar confirmando duas reservas para o mesmo assento, ou informando que há no estoque um item que já foi vendido minutos antes. Esse tipo de problema, se não controlado, pode ter consequências catastróficas tanto para a operação quanto para a credibilidade de uma organização.

<div align="center">
  <img width="800px" src="./img/15-concorrencia-exemplo-passagens.png">
</div>

De fato, quando múltiplas transações acessam os mesmos dados, a situação é relativamente tranquila enquanto essas transações realizam apenas leituras. Afinal, ler não altera o estado dos dados. Porém, a situação muda radicalmente quando diferentes transações precisam atualizar os mesmos registros de maneira concorrente. É aqui que surgem os grandes desafios, que vão desde inconsistências no banco até a perda de atualizações importantes. E é exatamente para resolver esses desafios que entra em cena o **Controle de Concorrência**.

O controle de concorrência é o mecanismo utilizado pelo Sistema Gerenciador de Banco de Dados (SGBD) para garantir que, mesmo com diversas transações executando de forma concorrente, o resultado final seja consistente, como se essas transações tivessem sido executadas uma após a outra, de forma sequencial.

### Por Que Precisamos do Controle de Concorrência?

De maneira sistemática, os problemas que surgem da execução concorrente de transações podem ser classificados em algumas categorias bem definidas. O primeiro e talvez mais evidente deles é a **atualização perdida**, que ocorre quando duas transações leem o mesmo dado e, logo em seguida, ambas escrevem sobre ele, sem considerar a modificação feita pela outra. O resultado? A última gravação simplesmente sobrescreve a anterior, como se ela nunca tivesse existido.

Outro problema típico envolve leituras inconsistentes, nas quais uma transação lê um dado que está em processo de modificação por outra transação, obtendo assim uma informação temporária ou incorreta.

Diante desse cenário, a concorrência, apesar de ser uma propriedade desejável — afinal, melhora a performance geral do sistema, permite um melhor aproveitamento dos recursos computacionais e oferece uma experiência mais fluida aos usuários —, também traz riscos. E é responsabilidade do SGBD administrar esses riscos.

O grande desafio, portanto, está em permitir que múltiplas transações sejam executadas em paralelo, maximizando o desempenho e a escalabilidade, sem abrir mão da consistência e da integridade dos dados.

Para enfrentar esse desafio, o SGBD implementa um conjunto de técnicas, conhecidas como **protocolos de controle de concorrência**, entre as quais se destacam os protocolos de bloqueio (locks), controle por timestamp e técnicas baseadas em grafos. Nesta etapa, focaremos especialmente no mecanismo mais tradicional e amplamente utilizado: o controle por bloqueios.

### Técnicas de Bloqueio: Fundamentando o Controle de Concorrência

O conceito de bloqueio, ou **lock**, é essencial para qualquer discussão sobre controle de concorrência. De forma bastante simplificada, um lock é uma variável associada a um item de dado do banco, que indica se esse item está disponível ou não para determinadas operações — seja uma leitura, seja uma escrita.

Quando uma transação deseja acessar um item, ela precisa, primeiramente, obter um lock sobre esse item. Esse lock sinaliza ao SGBD que a transação está utilizando aquele dado e, portanto, outros acessos simultâneos devem ser controlados de acordo com as regras definidas pelo tipo de lock estabelecido.

#### Bloqueio Binário: O Modelo Mais Simples

No modelo mais simples de bloqueio, conhecido como **bloqueio binário**, cada item do banco de dados pode estar em um de dois estados: **locked** (bloqueado) ou **unlocked** (desbloqueado). Antes de realizar qualquer operação, uma transação precisa solicitar o bloqueio do item. Enquanto ela detiver esse bloqueio, nenhuma outra transação poderá acessar o item — nem para leitura, nem para escrita.

<div align="center">
  <img width="580px" src="./img/15-lock-binario.png">
</div>

Quando a transação conclui suas operações, libera o bloqueio, permitindo que outras transações acessem o dado.

Embora funcional, esse modelo possui uma grande limitação: ele impede até mesmo operações de leitura concorrente, o que, na prática, pode reduzir drasticamente o desempenho do sistema, já que leituras simultâneas não representam risco de inconsistência.

#### Bloqueio Múltiplo: A Evolução Natural

Para superar as limitações do bloqueio binário, surge o conceito de **bloqueio múltiplo**, também conhecido como bloqueio compartilhado/exclusivo.

Neste modelo, os locks podem assumir dois tipos:

- **Bloqueio de Leitura (Read Lock ou Shared Lock):** Permite que múltiplas transações leiam o mesmo dado simultaneamente, desde que nenhuma delas deseje escrever sobre ele.
- **Bloqueio de Escrita (Write Lock ou Exclusive Lock):** Garante acesso exclusivo ao dado. Enquanto uma transação possui um write lock sobre um item, nenhuma outra — seja para leitura, seja para escrita — pode acessá-lo.

<div align="center">
  <img width="580px" src="./img/15-lock-multiplo.png">
</div>

O funcionamento desse modelo é bem intuitivo. Se uma transação deseja apenas ler um dado, ela solicita um read lock. Outras transações podem fazer o mesmo, sem problema algum. Porém, se uma delas quiser modificar esse dado, precisará esperar até que todos os read locks sejam liberados e, então, obter um write lock exclusivo.

#### Incremento e Decremento de Locks

Uma característica interessante do modelo de bloqueio múltiplo é a possibilidade de **incrementar** ou **decrementar** o nível de lock. Imagine uma transação que inicialmente apenas lê um dado e, no decorrer de sua execução, percebe que precisará alterá-lo. Nesse caso, ela pode solicitar um upgrade do seu lock, passando de read lock para write lock — desde que nenhuma outra transação esteja atualmente lendo aquele dado. Este processo é chamado de **incremento de lock**.

O caminho inverso, embora menos comum, também pode ocorrer. Se uma transação detinha um write lock, mas concluiu suas operações de escrita e deseja continuar apenas lendo o dado, ela pode **decrementar o lock**, convertendo-o em um read lock. Isso libera espaço para que outras transações possam também fazer leituras simultâneas sobre aquele item.

### Regras de Funcionamento dos Locks

Para que o sistema de locks funcione de forma eficiente e segura, algumas regras precisam ser rigorosamente seguidas:

1. **Antes de ler, bloqueie para leitura.**  
    Qualquer transação que deseje ler um dado deve primeiro obter um read lock sobre ele.
2. **Antes de escrever, bloqueie para escrita.**  
    Para modificar um dado, é obrigatório que a transação possua um write lock, o que impede que qualquer outra transação acesse aquele item durante o processo.
3. **Ao finalizar, libere os bloqueios.**  
    Assim que uma transação conclui suas operações — seja porque finalizou corretamente, seja porque precisou ser revertida —, ela deve liberar todos os locks que mantém, permitindo que outros processos possam acessar os dados.
4. **Bloqueio exclusivo impede qualquer outro acesso.**  
    Se uma transação detém um write lock sobre um item, nenhuma outra, nem mesmo para leitura, pode acessá-lo até que o lock seja liberado.
5. **Vários bloqueios de leitura são permitidos.**  
    Desde que não haja um bloqueio de escrita ativo, múltiplas transações podem possuir read locks simultaneamente sobre o mesmo dado.

### Limitações dos Locks: Serialização Não Garantida

É importante compreender que, embora o sistema de bloqueios controle o acesso simultâneo aos dados, ele não é, por si só, suficiente para garantir que o plano de execução das transações seja sempre serializável. Isto é, ainda pode ocorrer que uma sequência específica de obtenção e liberação de locks leve a resultados não equivalentes a uma execução sequencial das transações.

Por essa razão, além dos locks, os SGBDs implementam protocolos adicionais, como o **Protocolo de Bloqueio em Duas Fases (2PL)**, que veremos a seguir, projetados especificamente para assegurar a serialização dos planos de execução.

### Protocolo de Bloqueio em Duas Fases (2PL)

Quando falamos sobre garantir a integridade e a consistência dos dados em ambientes de múltiplos usuários, especialmente quando há atualizações concorrentes, é impossível não destacar a importância dos protocolos de bloqueio. Dentre eles, o **Protocolo de Bloqueio em Duas Fases**, conhecido como **2PL (Two-Phase Locking)**, é um dos mais amplamente utilizados pelos Sistemas Gerenciadores de Banco de Dados (SGBDs) comerciais.

O funcionamento do protocolo 2PL é relativamente intuitivo, embora seu impacto seja profundo na preservação da consistência e da propriedade de isolamento das transações. Como o próprio nome sugere, este protocolo se baseia na divisão do ciclo de vida da transação em duas fases bem definidas:

- **Fase de Expansão (Growing Phase)**: nesta etapa, a transação pode solicitar e obter bloqueios conforme for necessário para realizar suas operações. No entanto, ela não pode liberar nenhum bloqueio. Ou seja, ela pode apenas “acumular” locks.
- **Fase de Encolhimento (Shrinking Phase)**: assim que a transação libera seu primeiro bloqueio, ela entra na fase de encolhimento. A partir deste ponto, não pode mais solicitar novos bloqueios; pode apenas liberá-los até concluir sua execução.

O instante exato em que a transação realiza sua primeira operação de liberação de lock define o chamado **ponto de bloqueio**, que marca a transição da fase de expansão para a fase de encolhimento. A figura abaixo ilustra esse conceito de maneira clara:

<div align="center">
  <img width="300px" src="./img/15-ponto-de-bloqueio.png">
</div>

Ao impor essa disciplina no uso dos bloqueios, o protocolo 2PL assegura que os planos de execução sejam **serializáveis**, ou seja, que o resultado da execução concorrente seja equivalente a alguma execução sequencial das transações.

### Variações do Protocolo 2PL

Com o passar dos anos, surgiram algumas variações do protocolo básico de duas fases, cada uma delas tentando resolver desafios específicos ou atender a requisitos operacionais diferentes.

- **2PL Conservador (ou Estático)**: nesta abordagem, a transação deve obter todos os bloqueios de que precisa logo no início de sua execução, antes de realizar qualquer operação sobre os dados. Se algum dos bloqueios não estiver disponível, a transação simplesmente não inicia. Essa estratégia elimina a possibilidade de deadlock, porém, compromete bastante o paralelismo, tornando a execução menos eficiente.
- **2PL Estrito (Strict 2PL)**: considerado o mais utilizado na prática, este protocolo exige que todos os bloqueios de escrita sejam mantidos até que a transação alcance o commit ou o abort. Isso simplifica bastante os mecanismos de recuperação, pois garante que os dados modificados por uma transação só serão visíveis para outras transações após sua efetivação.
- **2PL Rigoroso (Rigorous 2PL)**: é ainda mais restritivo que o estrito, pois impõe que **todos os bloqueios**, tanto de leitura quanto de escrita, sejam mantidos até que a transação finalize, seja com sucesso (commit) ou por falha (rollback). Embora adote uma estratégia extremamente segura, sua principal desvantagem é a redução significativa do grau de concorrência.

<div align="center">
  <img width="540px" src="./img/15-variacoes-do-2pl.png">
</div>

A escolha entre essas variações deve levar em conta as características específicas do ambiente operacional, os requisitos de desempenho e os níveis de risco tolerados em relação a inconsistências e bloqueios.

### Problemas Clássicos no Controle de Concorrência: Deadlock, Livelock e Starvation

Embora os protocolos de bloqueio sejam eficazes para garantir a consistência e a serializabilidade, eles não estão isentos de desafios. O principal deles é o **deadlock**, que ocorre quando duas ou mais transações entram em um ciclo de espera mútua, onde cada uma aguarda que a outra libere um recurso para prosseguir. Nenhuma delas, no entanto, é capaz de avançar.

Imagine três transações A, B e C, e três recursos R, S e T. Se A bloqueia R e tenta obter S, que já está bloqueado por B, e B tenta obter T, que está com C, e C tenta obter R, que está com A, então temos um ciclo de dependência que caracteriza exatamente um deadlock. A figura a seguir ilustra esse cenário:

<div align="center">
  <img width="540px" src="./img/15-deadlock.png">
</div>

#### Estratégias de Prevenção de Deadlock

Algumas técnicas foram desenvolvidas para tentar prevenir a ocorrência de deadlocks. Dentre elas, destacam-se:

- **Protocolo Conservador:** impede que uma transação inicie caso não consiga obter todos os bloqueios de que necessita. Embora eficaz na eliminação de deadlocks, essa abordagem penaliza o paralelismo.
- **Protocolo Ordenado:** impõe uma ordem global sobre todos os itens de dados. As transações devem solicitar os bloqueios seguindo estritamente essa ordem. Isso evita ciclos de espera e, consequentemente, deadlocks.

Outra abordagem envolve o uso de algoritmos específicos que gerenciam conflitos de bloqueios quando eles ocorrem:

- **Wait-Die:** se a transação que solicita o bloqueio é mais antiga que aquela que mantém o bloqueio, ela pode esperar. Caso contrário, ela é abortada (morre) e reiniciada mais tarde com o mesmo timestamp.
- **Wound-Wait:** inverte a lógica do wait-die. Se a transação que solicita o bloqueio é mais antiga, ela força a mais nova a ser abortada (ferida). Se for mais nova, ela simplesmente espera.

Um ponto importante nesses algoritmos é que as transações, quando reiniciadas, mantêm seus timestamps originais, garantindo que não ocorram ciclos infinitos de espera.

#### Detecção e Quebra de Deadlocks

Mesmo com protocolos de prevenção, deadlocks podem ocorrer. Por isso, muitos SGBDs adotam mecanismos de **detecção de deadlocks**, que monitoram o sistema periodicamente ou continuamente. Ao identificar um ciclo de espera no grafo de bloqueios, o sistema seleciona uma das transações envolvidas como vítima e força seu rollback para quebrar o ciclo.

A escolha da vítima pode se basear em critérios como:

- A transação mais recente.
- A transação que mantém o menor número de bloqueios.
- A transação que realizou o menor volume de atualizações até aquele ponto.

O objetivo é minimizar o impacto da resolução do deadlock sobre o restante do sistema.

### Livelock e Starvation: Desafios Adicionais

Se o deadlock é caracterizado por uma paralisação mútua, o **livelock** representa uma situação na qual as transações continuam ativas, mas não conseguem progredir, porque estão constantemente cedendo prioridade umas às outras. Imagine duas pessoas que tentam se desviar no corredor e, ao escolherem sempre o mesmo lado, ficam presas num ciclo infinito de movimentos sem avançar. A analogia é perfeita para ilustrar o livelock.

<div align="center">
  <img width="420px" src="./img/15-livelock.png">
</div>

A solução mais comum para esse problema é adotar políticas como **first-come-first-served** (ordem de chegada) ou aumentar progressivamente a prioridade das transações que estão esperando há mais tempo.

O **starvation**, por sua vez, é um problema relacionado, no qual uma transação é constantemente preterida em favor de outras, geralmente por ter uma prioridade mais baixa. Isso pode ocorrer quando os algoritmos de controle escolhem repetidamente a mesma transação como vítima de deadlocks, fazendo com que ela nunca consiga concluir sua execução.

<div align="center">
  <img width="540px" src="./img/15-starvation.png">
</div>

Técnicas como os próprios protocolos **wait-die** e **wound-wait** ajudam a mitigar o problema de starvation, pois garantem que, com o passar do tempo, as transações mais antigas acabam tendo prioridade para acessar os recursos.

### Protocolo Baseado em Grafos

Uma abordagem alternativa e eficiente para o controle de concorrência é o uso do **protocolo baseado em grafos**, também conhecido como **protocolo de árvore (tree protocol)**. Esse protocolo impõe uma estrutura hierárquica sobre os dados, na forma de um grafo acíclico (geralmente uma árvore), para ordenar e controlar o acesso concorrente.

A regra fundamental desse protocolo é que, depois de bloquear um item de dado, uma transação só pode bloquear outros itens se eles forem descendentes diretos ou indiretos do item previamente bloqueado no grafo. Isso impede que ciclos de espera ocorram, eliminando, portanto, a possibilidade de deadlocks.

O funcionamento do protocolo pode ser descrito da seguinte forma:

- O primeiro bloqueio realizado por uma transação pode ser feito sobre qualquer item de dado no grafo.
- Após o primeiro bloqueio, uma transação só pode solicitar o bloqueio de um item se já tiver bloqueado todos os seus **pais** no grafo.
- Um item de dado pode ser desbloqueado a qualquer momento, não precisando aguardar o término da transação.
- Uma vez que um item tenha sido desbloqueado, ele **não pode ser novamente bloqueado** pela mesma transação.

Esse modelo se adapta muito bem a aplicações onde existe uma hierarquia natural nos dados, como sistemas de arquivos, estruturas de diretórios, ou modelos de dados em árvore.

Na figura abaixo, observamos como um grafo de dados estruturado em árvore organiza os itens de dado. Em seguida, é apresentado um plano de execução (schedule) serializável, construído seguindo estritamente as regras deste protocolo.

<div align="center">
  <img width="420px" src="./img/15-grafo-de-banco-do-dados-em-arvore.png">
</div>

<div align="center">
  <img width="420px" src="./img/15-protocolo-baseado-em-grafos.png">
</div>

O grande benefício deste protocolo é que ele garante a serializabilidade dos planos de execução, **sem a necessidade de uma fase de encolhimento**, como ocorre nos protocolos de bloqueio em duas fases (2PL). Além disso, elimina completamente o risco de deadlocks, ao custo de restringir a ordem na qual os dados podem ser acessados.

## Controle de Concorrência por Ordenação de Timestamp

Outra estratégia bastante poderosa e adotada por alguns sistemas é o controle de concorrência com base em **ordenação de timestamps**. Neste protocolo, cada transação recebe, no momento em que inicia, um **timestamp único e imutável**, que representa sua ordem temporal em relação às demais transações.

Esse timestamp pode ser gerado de duas maneiras principais:

1. **Relógio físico do sistema:** o timestamp corresponde à data e hora exata (em milissegundos, por exemplo) em que a transação foi iniciada.
2. **Contador lógico:** o sistema mantém um contador que é incrementado sempre que uma nova transação é criada. Assim, cada transação recebe um número sequencial crescente como timestamp.

A ideia fundamental desse protocolo é garantir que a execução das operações seja equivalente a uma execução serial ordenada pelos timestamps das transações. Em outras palavras, se uma transação T<sub>i</sub> recebeu um timestamp TS(T<sub>i</sub>) menor que o de T<sub>j</sub> (TS(T<sub>j</sub>)), então T<sub>i</sub> deve parecer ter sido executada antes de T<sub>j</sub> no ponto de vista do banco de dados.

Na prática, cada item de dado Q no banco de dados mantém dois metadados importantes:

- **W-timestamp(Q):** timestamp da última transação que realizou uma operação de escrita sobre Q.
- **R-timestamp(Q):** timestamp da última transação que realizou uma operação de leitura sobre Q.

O protocolo então se baseia em regras específicas para decidir se uma operação de leitura ou escrita pode ser executada, ou se a transação deve ser rejeitada e reiniciada.

**Cenário de Leitura (read(Q)):**

- Se **TS(T<sub>i</sub>) < W-timestamp(Q)**, significa que T<sub>i</sub> está tentando ler um valor que foi sobrescrito por uma transação mais recente. Como isso violaria a ordem de timestamps, a leitura é **rejeitada** e a transação T<sub>i</sub> é **abortada** e reiniciada com um novo timestamp.
- Se **TS(T<sub>i</sub>) ≥ W-timestamp(Q)**, então a leitura é **permitida**, e o sistema atualiza R-timestamp(Q) para o maior valor entre o próprio R-timestamp(Q) e TS(T<sub>i</sub>).

**Cenário de Escrita (write(Q)):**

- Se **TS(T<sub>i</sub>) < R-timestamp(Q)**, significa que algum dado foi lido por uma transação mais recente que deveria, segundo a ordem dos timestamps, ter ocorrido após T<sub>i</sub>. Isso invalida a escrita de T<sub>i</sub>, que então é **abortada**.
- Se **TS(T<sub>i</sub>) < W-timestamp(Q)**, significa que T<sub>i</sub> está tentando sobrescrever um valor que já foi escrito por uma transação mais recente. Isso também é inconsistente com a ordem de timestamps, então a operação é **rejeitada** e T<sub>i</sub> é **abortada**.
- Caso contrário, a operação de escrita é **permitida**, e o sistema atualiza W-timestamp(Q) para TS(T<sub>i</sub>).

Como exemplo, imagine dois caixas de um supermercado operando simultaneamente sobre o mesmo banco de dados. Uma transação T<sub>1</sub> inicia às 10h00 (TS=100) e deseja atualizar o estoque do produto "Leite". Logo após, às 10h01 (TS=101), inicia a transação T<sub>2</sub>, que também lê o estoque do mesmo produto.

Se T<sub>2</sub> faz a leitura antes que T<sub>1</sub> tenha gravado sua atualização, não há problema, pois os timestamps estão na ordem correta. Contudo, se T<sub>1</sub> tenta gravar um valor que T<sub>2</sub> já leu, então essa gravação viola a ordem temporal (T<sub>2</sub> parece estar “no futuro” em relação a T<sub>1</sub>). Nesse caso, T<sub>1</sub> deverá ser abortada e reiniciada, garantindo que a execução seja equivalente a uma serialização ordenada pelos timestamps.

O grande benefício desse protocolo é que ele **não exige bloqueios**, o que elimina qualquer risco de deadlocks. Contudo, ele pode levar a um número elevado de **abortos e reinícios de transações**, especialmente em sistemas altamente concorrentes. Assim, ele é mais eficiente em cenários com muitas operações de leitura e poucos conflitos de escrita.

Perfeito. A seguir, organizei suas anotações do **tópico 15.3 – Recuperação**, estruturando-as com uma linguagem clara, altamente didática e técnica, no padrão de um livro-texto. O conteúdo foi reorganizado para promover fluidez na leitura, mantendo todos os conceitos, figuras e exemplos propostos, sem redução do conteúdo original.

## Recuperação

Todo sistema computacional está sujeito a falhas. Por mais robusto que seja o ambiente tecnológico, existem riscos associados à perda de energia, falhas de hardware, bugs de software e até mesmo desastres naturais. Quando essas falhas ocorrem em sistemas de bancos de dados, podem comprometer diretamente a integridade e a consistência das informações armazenadas.

O processo de **recuperação** ou **restauração após falhas** tem como objetivo garantir que o banco de dados seja capaz de retornar a um estado consistente, anterior ao momento da falha, ou, se possível, completar as operações que já estavam sendo processadas corretamente. Esse processo é um dos pilares fundamentais da propriedade de **durabilidade**, presente no modelo **ACID**.

Recuperar um banco de dados não significa apenas restaurar arquivos a partir de backups. O conceito é bem mais abrangente, envolvendo mecanismos internos que acompanham cada transação e cada modificação feita no sistema, de forma a garantir a capacidade de desfazer ou refazer operações quando necessário.

### Como Funciona a Recuperação: Uma Visão Geral

O processo de recuperação pode ser dividido, essencialmente, em duas fases distintas:

1. **Ações realizadas durante o processamento normal das transações:** Neste momento, o sistema registra informações detalhadas em um arquivo de log, como operações de leitura, escrita, início, commit ou rollback das transações. Este log é vital para permitir uma recuperação posterior.
2. **Ações executadas após uma falha:** Quando uma falha ocorre, o sistema consulta o arquivo de log e, a partir dele, executa as operações necessárias para restaurar o banco de dados a um estado consistente. Isso pode envolver desfazer transações incompletas ou refazer aquelas que foram concluídas com sucesso, mas cujos efeitos ainda não haviam sido refletidos permanentemente no armazenamento não volátil.

### Classificação das Falhas em Sistemas de Banco de Dados

Para compreender como funciona um mecanismo de recuperação, é fundamental entender que nem todas as falhas são iguais. Podemos classificá-las em quatro categorias principais, cada uma exigindo técnicas específicas de recuperação.

**Falha de Transação**: Essa falha ocorre quando uma transação específica não consegue prosseguir normalmente. Isso pode acontecer por diversos motivos, como:

- **Erros lógicos:** Dados incorretos, violações de restrições, estouro de limites ou erros de programação.
- **Erros de sistema:** Problemas como deadlock, livelock ou starvation, que obrigam o sistema a interromper a transação para preservar a integridade do banco de dados.

**Queda do Sistema**: Nessa situação, todo o sistema operacional ou o servidor de banco de dados falha. As principais causas incluem:

- Falha de hardware (queda de energia, falha na CPU ou na memória).
- Bugs no próprio software do banco de dados ou no sistema operacional.

Neste tipo de falha, todo o conteúdo que estava armazenado na **memória volátil** (RAM) é perdido. No entanto, o conteúdo armazenado em **disco**, que é não volátil, permanece preservado, tornando possível a recuperação a partir dos dados e dos registros de log.

**Falha de Disco**: Aqui o problema não está apenas na perda de dados voláteis, mas na **corrupção dos próprios arquivos físicos** armazenados no disco. Essa falha pode ocorrer devido a:

- Defeitos físicos, como queda do cabeçote do disco.
- Setores defeituosos ou falhas nas operações de escrita.

A recuperação desse tipo de falha depende fundamentalmente de estratégias de **redundância e backups externos**, como cópias armazenadas em outros discos ou em mídias de armazenamento externas.

**Falhas Catastróficas**: São situações extremas, como incêndios, enchentes ou desastres naturais que comprometem todo o ambiente físico. A recuperação aqui depende exclusivamente da existência de cópias de segurança em locais remotos.

### Estrutura de Armazenamento: Entendendo o Processo

Para compreender como os bancos de dados realizam a recuperação, é necessário primeiro entender como funciona o armazenamento dos dados.

Basicamente, os dados podem estar armazenados em dois tipos de memória:

- **Memória volátil:** Refere-se à memória principal (RAM), que perde seu conteúdo quando o sistema é desligado ou sofre uma falha.
- **Memória não volátil:** Inclui discos rígidos, SSDs e outras mídias permanentes, nas quais os dados permanecem armazenados independentemente de falhas ou desligamentos.

Além disso, muitos sistemas utilizam mecanismos de redundância, como os arranjos RAID (Redundant Array of Independent Disks), que ajudam a proteger os dados contra falhas físicas nos discos.

O processo de armazenamento e recuperação depende diretamente da forma como os dados são manipulados entre a memória principal e o disco. A figura a seguir ilustra claramente esse processo:

<div align="center">
  <img width="640px" src="./img/15-operacoes-nas-memorias.png">
</div>

Nessa figura, observamos dois grupos fundamentais de operações:

- **Operações de leitura e escrita (Read/Write):** Estas operações são feitas entre o **processador** e a **memória principal** (RAM). Por exemplo, ao executar um comando `READ(A)`, o processador busca o dado A que já esteja carregado na RAM.
- **Operações de entrada e saída (Input/Output):** Quando um dado não está na memória principal, é necessário trazê-lo do disco por meio de uma operação `INPUT(A)`. De maneira análoga, quando uma atualização precisa ser persistida, o dado é enviado da memória principal para o disco por meio de uma operação `OUTPUT(A)`.

Quando ocorre uma transferência entre a memória e o disco, há três cenários possíveis:

1. **Transferência bem-sucedida:** O dado é corretamente copiado da memória para o disco, garantindo a persistência.
2. **Falha parcial:** Uma falha ocorre no meio da transferência. O conteúdo no disco pode ficar inconsistente ou parcialmente corrompido.
3. **Falha total:** A falha acontece antes que qualquer dado seja alterado no disco, mantendo o bloco físico no disco intacto.

Esse entendimento é crucial porque influencia diretamente a estratégia de recuperação a ser adotada. Se a falha ocorreu antes de um dado ser efetivamente gravado no disco, então a operação não precisa ser revertida. No entanto, se a falha aconteceu no meio de uma operação, pode ser necessário realizar um processo de **rollback** (desfazer) ou **redo** (refazer) utilizando as informações armazenadas no log.

### O Uso dos Arquivos de Log: A Base da Recuperação

Imagine a seguinte situação: uma falha ocorre exatamente no momento em que o sistema estava transferindo dados da memória principal para o disco. O que acontece com essas informações? Elas se perdem? Foram gravadas? Estão íntegras?

É exatamente neste cenário que o **log de transações** ganha protagonismo. Todos os sistemas gerenciadores de banco de dados (SGBDs) modernos mantêm um arquivo de log, separado fisicamente dos arquivos de dados, que registra, de forma sequencial, todas as operações que alteram o estado do banco de dados. Este log, muitas vezes encontrado em arquivos com extensão `.LDF` (no SQL Server, por exemplo), é o mecanismo que garante a **durabilidade e a atomicidade**, permitindo a recuperação completa após falhas.

<div align="center">
  <img width="580px" src="./img/15-registro-de-log.png">
</div>

Para que esse processo funcione corretamente, o log precisa estar armazenado em **memória não volátil**, ou seja, em disco. Além disso, o log deve ser atualizado **antes** de qualquer alteração ser aplicada fisicamente aos dados. Essa técnica é conhecida como **Write-Ahead Logging (WAL)**, e é uma exigência fundamental para garantir a capacidade de recuperação.

Cada entrada no log possui, no mínimo, quatro informações essenciais:

- O identificador da transação (**T<sub>i</sub>**);
- O item de dado sendo modificado (**X<sub>j</sub>**);
- O valor antigo do dado (**V<sub>1</sub>**);
- O valor novo do dado (**V<sub>2</sub>**).

Adicionalmente, o log também registra eventos que indicam o estado da transação, como:

- `<Ti start>` – quando a transação é iniciada;
- `<Ti commit>` – quando a transação é efetivada;
- `<Ti abort>` – quando a transação é abortada.

Esses registros permitem que, após uma falha, o sistema possa identificar exatamente quais transações foram concluídas, quais não foram e quais precisam ser refeitas ou desfeitas.

### Terminologias Importantes: Steal, No-Steal, Force e No-Force

O comportamento dos SGBDs em relação à gravação de páginas no disco pode ser classificado de acordo com dois critérios fundamentais: **roubado (steal) ou não roubado (no-steal)** e **forçado (force) ou não forçado (no-force)**. Esses conceitos definem as políticas adotadas para a escrita de dados no armazenamento permanente.

- **Steal (roubado):** Permite que uma página em cache, que tenha sido modificada por uma transação ainda não efetivada, seja escrita no disco. Isso ocorre quando o sistema precisa liberar espaço no buffer para outra transação. A vantagem é um uso mais eficiente da memória, porém, exige mecanismos robustos de recuperação, pois dados não efetivados podem ir para o disco.
- **No-Steal (não roubado):** Impede que páginas modificadas por transações não efetivadas sejam gravadas no disco. Isso evita a necessidade de desfazer alterações no disco após uma falha, mas exige que essas páginas permaneçam no buffer até que a transação finalize, o que pode ser custoso em termos de memória.
- **Force (forçado):** Garante que todas as modificações de uma transação sejam gravadas no disco no momento do commit. Isso simplifica a recuperação, pois não há risco de perder dados efetivados.
- **No-Force (não forçado):** Permite que uma transação seja considerada concluída (commit) mesmo que suas atualizações ainda não tenham sido gravadas no disco. Esse modelo melhora o desempenho, pois não força operações de I/O no momento do commit, mas exige que, em caso de falha, seja possível **refazer (redo)** essas operações com base no log.

É importante notar que a maioria dos SGBDs modernos adota o modelo **steal/no-force**, pois ele oferece um melhor desempenho, embora imponha desafios maiores no processo de recuperação.

### Estratégias de Recuperação: Modificações Adiadas e Modificações Imediatas

A forma como os dados são gravados no disco influencia diretamente a estratégia de recuperação utilizada. Existem duas abordagens principais: **modificações adiadas** e **modificações imediatas**.

#### Modificações Adiadas (Deferred Update)

Na estratégia de **modificação adiada**, nenhuma alteração nos dados é gravada no disco enquanto a transação está em execução. As modificações ficam armazenadas no log e são aplicadas ao banco de dados **apenas após a transação alcançar seu ponto de commit**.

Isso significa que, se uma falha ocorrer antes do commit, não há necessidade de desfazer nenhuma operação, pois nenhuma alteração chegou a ser aplicada no banco de dados.

<div align="center">
  <img width="580px" src="./img/15-modificacoes-adiadas.png">
</div>

Neste modelo, o processo de recuperação executa apenas a operação de **REDO**, ou seja, refazer as operações de transações que foram efetivamente concluídas. Isso torna o protocolo conhecido como **No-Undo/Redo**, pois não há a necessidade de desfazer nada.

Por exemplo, imagine uma transação T1 que atualiza o salário de um funcionário. Se ela chega ao commit, o sistema utiliza o log para aplicar definitivamente o novo salário no banco. Se, por outro lado, a falha ocorre antes do commit, o dado permanece inalterado no banco, e nenhuma ação de desfazer é necessária.

#### Modificações Imediatas (Immediate Update)

A estratégia de **modificação imediata** permite que os dados sejam atualizados no disco **antes mesmo da transação finalizar**. Isso traz uma melhoria no desempenho, mas exige um mecanismo de recuperação mais sofisticado.

<div align="center">
  <img width="580px" src="./img/15-modificacoes-imediatas.png">
</div>

Neste cenário, se uma transação for abortada ou o sistema sofrer uma falha, o banco de dados pode conter dados de transações não efetivadas. Portanto, é necessário executar tanto operações de **UNDO** (para desfazer) quanto de **REDO** (para refazer) durante o processo de recuperação.

Dois protocolos principais são aplicados aqui:

- **UNDO/REDO:** Utilizado quando o banco de dados permite que os dados sejam escritos no disco antes do commit, e nem sempre força a escrita imediatamente no commit. Nesse caso, durante a recuperação, desfazemos os efeitos das transações sem commit (**UNDO**) e refazemos os efeitos daquelas que estavam no log com commit registrado (**REDO**).
- **UNDO/NO-REDO:** Esse protocolo assume que todas as atualizações foram gravadas no disco antes do commit. Portanto, não há necessidade de refazer operações após uma falha. O sistema apenas desfaz as operações das transações que não chegaram ao commit.

O uso da modificação imediata torna o banco de dados mais eficiente durante a operação normal, pois permite maior liberdade na gestão do buffer e dos discos. Entretanto, exige algoritmos mais robustos para garantir a integridade em situações de falha.

Vale ressaltar que tanto as operações de **UNDO** quanto de **REDO** são projetadas para serem **idempotentes**, ou seja, aplicá-las mais de uma vez produz sempre o mesmo resultado, sem efeitos colaterais.

Por exemplo, se uma operação de UNDO desfaz uma alteração no valor do saldo de uma conta de 500 para 400, aplicá-la repetidamente continuará deixando o saldo em 400. Da mesma forma, uma operação de REDO que ajusta o saldo para 500 pode ser executada quantas vezes forem necessárias sem alterar o valor final além do desejado.

Essa característica é essencial para garantir que, mesmo que o processo de recuperação seja interrompido ou reiniciado, o banco de dados sempre será restaurado corretamente.

### Checkpoint: Um Marco Fundamental no Processo de Recuperação

Imagine um cenário em que uma falha no sistema de banco de dados acontece. Nesse momento, o processo de recuperação precisa, então, consultar os registros presentes no log de transações para determinar exatamente quais transações devem ser **refeitas** (REDO) e quais devem ser **desfeitas** (UNDO).

Se não houvesse nenhum tipo de otimização, essa tarefa implicaria, obrigatoriamente, em analisar **todo o histórico do log**, desde o momento em que o banco foi iniciado, o que poderia ser extremamente custoso, tanto em tempo quanto em recursos computacionais. É exatamente para tornar esse processo mais eficiente que entra em cena um conceito essencial na administração de bancos de dados: o **checkpoint**.

O checkpoint funciona como um **marco de segurança** dentro do log. É um ponto de referência que permite ao sistema identificar, a partir dele, quais transações já tiveram suas atualizações corretamente persistidas no disco e quais ainda precisam de algum tipo de ação durante o processo de recuperação.

#### Entendendo o Processo de Checkpoint

De forma simplificada, podemos dizer que o checkpoint cria um "fotografia" do estado atual do banco de dados em determinado momento. A partir dessa fotografia, o SGBD sabe exatamente que todas as operações realizadas até aquele instante estão garantidas no armazenamento permanente, não sendo mais necessário realizar nem operações de REDO nem de UNDO para transações finalizadas antes desse ponto.

O processo de checkpoint envolve três operações fundamentais:

1. **Forçar a escrita no armazenamento permanente de todos os registros do log que estão na memória principal.**
2. **Forçar a escrita no disco de todos os blocos de dados em buffer que foram modificados, mas ainda não estavam gravados no armazenamento não volátil.**
3. **Gravar no próprio log um registro especial de checkpoint, indicando que todas as transações finalizadas até aquele ponto já tiveram suas modificações persistidas no disco.**

Um ponto extremamente relevante é que, enquanto o checkpoint está sendo executado, **não é permitido** que qualquer transação realize ações que alterem dados, como escrita em blocos de buffer ou inserção de novos registros no log. Isso garante que o estado capturado pelo checkpoint seja coerente e consistente.

No entanto, essa pausa temporária na execução das transações, necessária para garantir a integridade do checkpoint, introduz um pequeno atraso no processamento, especialmente em ambientes com alta taxa de transações. Por isso, a operação de checkpoint precisa ser cuidadosamente planejada, tanto em termos de frequência quanto de momento de execução.

#### Passo a Passo de um Checkpoint Convencional

O processo de checkpoint tradicional segue, essencialmente, os seguintes passos:

1. **Suspensão temporária das transações em execução.**  
    Isso garante que nenhuma nova alteração aconteça durante a construção do checkpoint.
2. **Forçar a gravação no disco de todos os blocos de dados que estão no buffer da memória principal e que foram modificados.**  
    Aqui, é garantido que todos os dados alterados estejam definitivamente salvos no armazenamento não volátil.
3. **Gravar um registro especial de checkpoint no arquivo de log.**  
    Esse registro normalmente inclui, além do próprio marcador de checkpoint, informações adicionais, como a lista de transações ativas no momento e os ponteiros para as posições relevantes do log.
4. **Forçar a gravação do próprio log no disco, garantindo que o registro de checkpoint não se perca.**
5. **Retomar o processamento normal das transações.**

Apesar de eficiente, esse procedimento possui uma desvantagem evidente: o tempo necessário para gravar todos os buffers no disco pode atrasar a execução das transações. E, em sistemas com alta demanda, esse atraso, ainda que breve, pode impactar a performance.

#### Fuzzy Checkpoint: Otimizando o Processo

Como alternativa ao checkpoint tradicional, muitos sistemas implementam o chamado **fuzzy checkpoint**. A principal diferença está no fato de que, no fuzzy checkpoint, o sistema **não precisa suspender a execução das transações**.

Neste modelo, a execução do checkpoint pode começar com as transações ainda ativas, desde que seja garantido que, no momento da gravação do registro de checkpoint no log, todos os dados até aquele ponto tenham sido corretamente persistidos. Isso melhora significativamente o desempenho, eliminando a pausa obrigatória.

#### Checkpoints e os Modelos de Atualização

O comportamento do checkpoint está diretamente relacionado ao modelo de atualização adotado pelo SGBD, ou seja, se ele segue o modelo de **modificação adiada** (deferred update) ou de **modificação imediata** (immediate update).

Para entender como isso se reflete na prática, vejamos as figuras e seus respectivos comportamentos.

**Checkpoint no Modelo de Modificação Adiada (Redo/No-Undo)**:

<div align="center">
  <img width="580px" src="./img/15-checkpoint-redo-no-undo.png">
</div>

No modelo de atualização adiada, os dados no banco só são efetivamente gravados no disco após o commit das transações. Como consequência, o processo de recuperação precisa apenas garantir que as transações que tiveram commit registrado sejam refeitas (**REDO**). Não há necessidade de desfazer operações, pois nenhuma alteração parcial chega a ser gravada antes do commit. Portanto, esse modelo é chamado de **Redo/No-Undo**.

**Checkpoint no Modelo de Modificação Imediata (Redo/Undo)**:

<div align="center">
  <img width="580px" src="./img/15-checkpoint-redo-undo.png">
</div>

Neste modelo, as alterações são gravadas no disco assim que acontecem, mesmo que a transação ainda não tenha finalizado. Isso exige que, no processo de recuperação, sejam realizados tanto o **REDO**, para garantir que as alterações de transações com commit sejam refletidas, quanto o **UNDO**, para desfazer os efeitos de transações que estavam em andamento e não chegaram ao commit.

O checkpoint, nesse contexto, serve como um limite que indica até onde o SGBD pode confiar que os dados estão corretamente persistidos. A partir desse ponto, o log é consultado para determinar quais transações ainda precisam ser refeitas ou desfeitas.

**Checkpoint no Modelo No-Redo/Undo**:

<div align="center">
  <img width="580px" src="./img/15-checkpoint-no-redo-undo.png">
</div>

Existe, ainda, uma situação específica em que o SGBD adota uma política tão rigorosa que garante que todas as alterações feitas pelas transações sejam imediatamente refletidas no banco de dados de forma definitiva. Nesse cenário, não é necessário realizar o REDO durante a recuperação, pois todas as atualizações persistiram corretamente no disco.

Entretanto, caso ocorra uma falha, ainda é necessário executar o **UNDO** para desfazer os efeitos das transações que não foram efetivamente concluídas (ou seja, que não chegaram ao commit). Este modelo é conhecido como **No-Redo/Undo**.

### Paginação Shadow

Dentro do contexto de mecanismos de recuperação, uma técnica alternativa às estratégias baseadas em logs é a **paginação shadow**, também chamada de **paginação sombra**. Este método tem como premissa reduzir, teoricamente, a quantidade de acessos ao disco, visto que não depende de registros contínuos de operações em arquivos de log. No entanto, essa técnica apresenta limitações significativas, especialmente quando aplicada a sistemas com transações concorrentes.

A essência do método está na manutenção de duas estruturas chamadas de **tabelas de páginas**. Estas tabelas contêm referências para as páginas de dados do banco, que são de tamanho fixo. Assim, o banco de dados é logicamente dividido em um conjunto de **n páginas**. Para gerenciar essas páginas, mantém-se:

- Uma **tabela de páginas corrente**, que representa o estado atual e pode receber modificações;
- Uma **tabela de páginas shadow**, que permanece inalterada durante a execução da transação, refletindo o estado consistente anterior.

O funcionamento é relativamente simples, porém poderoso em termos de consistência. No início de uma transação, as duas tabelas — corrente e shadow — são idênticas. Toda modificação realizada pela transação é aplicada exclusivamente na tabela corrente, deixando a tabela shadow preservada como uma cópia segura do banco antes do início das operações.

Quando a transação é concluída com sucesso e efetiva suas alterações (commit), a tabela corrente se torna a nova tabela shadow. A partir desse ponto, ela passa a representar o novo estado consistente do banco. O processo de transição pode ser melhor visualizado nas figuras a seguir:

- Na **primeira figura**, temos o **estado inicial**, onde ambas as tabelas são idênticas e refletem o mesmo conjunto de páginas do banco.

<div align="center">
  <img width="540px" src="./img/15-shadow-estado-inicial.png">
</div>

- Na **segunda figura**, vemos as **modificações**, que são aplicadas exclusivamente na tabela corrente, enquanto a tabela shadow continua intacta e disponível para uma possível recuperação, caso a transação venha a falhar.

<div align="center">
  <img width="540px" src="./img/15-shadow-modificacoes.png">
</div>

- Por fim, na **terceira figura**, ocorre a **atualização**, onde, após o commit da transação, a tabela corrente se transforma na nova tabela shadow, representando o estado atualizado e consistente do banco de dados.

<div align="center">
  <img width="540px" src="./img/15-shadow-atualizacoes.png">
</div>

Esse modelo de recuperação apresenta uma vantagem clara: a velocidade no processo de recuperação. Em caso de falha, não há necessidade de executar operações de **undo** ou **redo**. Basta simplesmente descartar a tabela corrente e restaurar a tabela shadow, que permanece imutável, garantindo a consistência do banco até o ponto anterior ao início da transação.

Contudo, essa técnica traz consigo um conjunto significativo de desvantagens, especialmente em sistemas que lidam com alto volume de transações concorrentes:

- **Fragmentação de Dados:** Cada página atualizada é alocada em uma nova posição física no disco, o que leva rapidamente à fragmentação dos dados. Isso prejudica o desempenho em leituras sequenciais e dificulta o agrupamento lógico das páginas relacionadas.
- **Overhead na Efetivação:** Quando a tabela de páginas é grande, o custo computacional para atualizar a tabela shadow durante o commit pode ser expressivo, impactando diretamente a performance.
- **Custo na Gravação:** O tempo necessário para gravar toda a nova tabela de páginas shadow no disco, especialmente em bancos de grande porte, pode ser substancial.
- **Coleta de Lixo (Garbage Collection):** Após o commit, as páginas antigas que não são mais referenciadas precisam ser removidas. Esse processo gera overhead adicional, que não existe em técnicas baseadas exclusivamente em logs.

Além disso, a adaptação desse método para ambientes multiusuários e com controle de concorrência torna-se extremamente complexa. Na prática, é comum que, mesmo quando se utiliza a paginação shadow, o sistema também mantenha algum nível de controle via logs, especialmente quando associado a métodos de controle de concorrência como bloqueios ou timestamp.

Diante desses desafios, a paginação shadow é mais adequada para ambientes simples, de poucos usuários ou aplicações embutidas (como bancos locais, em dispositivos móveis ou sistemas isolados). Nos sistemas de missão crítica, de grande porte e altamente concorrentes, os métodos baseados em logs se mostram mais eficientes, robustos e escaláveis.

### Visão Geral do Algoritmo de ARIES

A evolução das técnicas de recuperação culmina no desenvolvimento de algoritmos altamente robustos e eficientes, sendo o **ARIES** (Algorithm for Recovery and Isolation Exploiting Semantics) um dos mais bem-sucedidos e amplamente adotados na indústria. Esse algoritmo é utilizado por diversos SGBDs comerciais, como IBM DB2, Microsoft SQL Server e outros, justamente por seu equilíbrio entre desempenho e segurança na recuperação de dados.

O ARIES foi projetado para operar sob uma abordagem conhecida como **"steal/no-force"**, o que significa que:

- **Steal:** O sistema permite que páginas modificadas sejam escritas no disco antes que a transação correspondente realize o commit. Isso oferece flexibilidade na gestão de buffer, evitando que o buffer fique cheio e bloqueie novas operações.
- **No-force:** Não é necessário forçar que todas as páginas modificadas sejam gravadas no disco no momento do commit. Com isso, melhora-se significativamente o desempenho do sistema, já que reduz a quantidade de I/O sincronizado no momento crítico do commit.

Para garantir a consistência e integridade dos dados, mesmo nesse cenário flexível, o ARIES se apoia em três princípios fundamentais:

1. **Registro adiantado em log (Write-Ahead Logging - WAL):** Antes de qualquer modificação ser efetivada no disco, ela deve ser registrada no log. Este princípio assegura que qualquer alteração pode ser refeita ou desfeita, mesmo após uma falha catastrófica.
2. **Repetição de histórico (Redo):** Durante o processo de recuperação, o ARIES refaz todas as ações de todas as transações, inclusive das que não chegaram a realizar commit. Isso reconstrói fielmente o estado do banco no exato momento da falha.
3. **Desfazer com logging (Undo):** As transações que estavam ativas no momento da falha são revertidas. E, crucialmente, o próprio processo de desfazer é registrado no log, garantindo que, se uma nova falha ocorrer durante o recovery, não haja retrabalho de operações já desfeitas.

O funcionamento do ARIES se desenvolve em três fases bem definidas:

1. **Fase de Análise**: Essa fase tem como objetivo identificar quais transações estavam ativas no momento da falha e quais páginas de dados estavam modificadas na memória (páginas sujas) mas ainda não haviam sido gravadas no disco. Isso é feito analisando os registros no log, especialmente os checkpoints, que armazenam informações sobre o estado do sistema.
2. **Fase de Refazer (Redo)**: O ARIES reexecuta todas as operações registradas no log desde o último checkpoint até o ponto da falha. Isso assegura que qualquer modificação válida que não havia sido persistida no disco seja devidamente aplicada, garantindo que o banco reflita exatamente o estado que possuía no momento da falha.
3. **Fase de Desfazer (Undo)**: As transações que estavam ativas no momento da falha, ou seja, que não haviam sido concluídas com um commit, têm todas as suas operações desfeitas. Esse processo assegura a propriedade de atomicidade, garantindo que nenhuma transação incompleta afete o estado final do banco de dados.

Para suportar esse processo, o ARIES utiliza três estruturas fundamentais:

- **Log:** Armazena detalhadamente todas as operações realizadas, além dos checkpoints.
- **Tabela de Transações:** Mantém o controle das transações, identificando quais estavam ativas, concluídas ou abortadas no momento da falha.
- **Tabela de Páginas Sujas (Dirty Pages):** Identifica quais páginas estavam modificadas na memória, mas ainda não haviam sido gravadas no disco.

Cada entrada no log é associada a um **Log Sequence Number (LSN)**, que permite rastrear com precisão a sequência dos eventos. A partir dessa sequência, o ARIES executa seu algoritmo de recuperação de forma determinística e segura.

A figura a seguir ilustra o processo de recuperação sob o ARIES:

<div align="center">
  <img width="720px" src="./img/15-algoritmo-de-aries.png">
</div>

Na figura, podemos observar:

- **(a)** O estado do log no exato ponto da falha;
- **(b)** As tabelas de transações e de páginas sujas no momento em que foi realizado o checkpoint anterior à falha;
- **(c)** O estado atualizado das mesmas tabelas após a fase de análise, que antecede os passos de refazer (redo) e desfazer (undo).

## Considerações Finais

Ao longo deste capítulo, exploramos de forma aprofundada os conceitos, mecanismos e desafios associados ao **processamento de transações**, ao **controle de concorrência** e aos **métodos de recuperação em bancos de dados**. Esses três pilares são fundamentais para garantir que os sistemas de banco de dados operem de maneira confiável, consistente e segura, mesmo diante de ambientes de alta concorrência, falhas inesperadas e grande volume de transações.

Iniciamos este estudo compreendendo que uma **transação** é muito mais do que uma simples sequência de comandos. Ela representa uma unidade lógica de trabalho que deve ser executada de forma **atômica, consistente, isolada e durável**, características que formam o conhecido conjunto de propriedades **ACID**. Essas propriedades são indispensáveis para assegurar a integridade dos dados.

Avançamos então para o entendimento dos desafios impostos pela **execução concorrente de transações**, especialmente em sistemas multiusuário. Discutimos os riscos associados, como **leituras sujas, leituras não repetíveis, atualizações perdidas, deadlocks, livelocks e starvation**, que podem comprometer tanto a consistência dos dados quanto a performance do sistema. Para lidar com esses problemas, exploramos em detalhes os principais protocolos de **controle de concorrência**, incluindo técnicas baseadas em **bloqueio (locks)**, em **timestamping** e em **protocolos de grafos**, além dos cuidados necessários com a detecção e prevenção de impasses.

Por fim, nos debruçamos sobre os mecanismos de **recuperação**, essenciais para garantir que, mesmo após falhas de hardware, software ou transações, o banco de dados seja restaurado a um estado consistente. Estudamos desde abordagens mais simples, como o uso de **checkpoint e paginação shadow**, até técnicas avançadas, como o algoritmo **ARIES**, amplamente utilizado nos SGBDs comerciais modernos devido à sua robustez e eficiência.

Com este conhecimento consolidado, estamos preparados para avançar para os próximos tópicos do estudo de bancos de dados, aprofundando ainda mais nossa compreensão sobre seu funcionamento interno e seu papel indispensável no suporte às mais diversas aplicações no mundo real.

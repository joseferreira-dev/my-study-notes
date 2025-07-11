# Capítulo 8 – Lógica Ativa no Banco de Dados: Extensões Procedurais

Ao longo desta jornada, exploramos a linguagem SQL primariamente através de sua natureza declarativa: especificamos "o que" queremos, e o SGBD determina "como" obter. No entanto, desde as primeiras versões do padrão SQL, como o SQL-86, percebeu-se uma limitação: a ausência de estruturas de controle de fluxo (`IF/THEN/ELSE`, laços de repetição) impedia a criação de lógicas de negócio complexas diretamente no banco de dados, forçando que essa inteligência residisse exclusivamente na aplicação cliente.

Para superar essa barreira e transformar o banco de dados de um repositório passivo para uma plataforma de aplicação ativa, o padrão SQL evoluiu para incluir o **SQL/PSM (Persistent Stored Modules)**. Essa extensão introduziu na linguagem SQL os blocos de construção de programação procedural, permitindo que os desenvolvedores criem rotinas robustas que são armazenadas e executadas diretamente no servidor de banco de dados. Essa abordagem não apenas centraliza a lógica de negócio, mas também pode oferecer ganhos significativos de desempenho e segurança, além de reduzir a complexidade nas aplicações clientes.

Este capítulo explora os três tipos principais de módulos persistentes que formam o cerne do SQL/PSM: os **Procedimentos Armazenados (Stored Procedures)**, as **Funções Definidas pelo Usuário (User-Defined Functions - UDFs)** e os **Gatilhos (Triggers)**. Veremos como cada um desses objetos funciona, para que servem e como eles nos permitem embutir uma lógica rica e reativa diretamente onde os dados residem, fazendo do banco de dados um participante ativo na lógica da aplicação.

## Procedimentos Armazenados (Stored Procedures)

Um **procedimento armazenado** é um programa, um conjunto de uma ou mais instruções SQL e lógicas procedurais, que é compilado e armazenado no próprio banco de dados sob um nome específico. Uma vez criado, uma aplicação cliente não precisa mais enviar cada instrução individualmente pela rede; ela simplesmente envia uma única instrução `CALL` para invocar o procedimento, que é então executado inteiramente no servidor.

### As Vantagens dos Procedimentos Armazenados

A adoção de procedimentos armazenados oferece benefícios estratégicos no desenvolvimento de sistemas.

- **Redução do Tráfego de Rede:** Este é um dos benefícios mais diretos. Em vez de uma aplicação enviar dezenas de comandos SQL para executar uma tarefa complexa, ela envia apenas uma chamada: `CALL MeuProcedimento(param1, param2)`. A lógica é processada no servidor, e apenas o resultado final retorna, diminuindo drasticamente a latência da rede.
- **Centralização e Reutilização de Código:** Lógicas de negócio complexas (como registrar um novo pedido, que envolve verificar estoque, inserir na tabela de pedidos e atualizar o inventário) podem ser encapsuladas em um único procedimento. Qualquer aplicação que precise realizar essa tarefa pode simplesmente chamar o mesmo procedimento, garantindo consistência e facilitando a manutenção. Uma mudança na regra de negócio só precisa ser aplicada em um único lugar.
- **Desempenho Aprimorado:** Como os procedimentos são armazenados no servidor, o SGBD pode pré-compilá-los e armazenar em cache seus planos de execução. Quando o procedimento é chamado, grande parte do trabalho de análise e otimização da consulta já foi feito, resultando em uma execução mais rápida em comparação com o envio de SQL dinâmico.
- **Segurança e Controle de Acesso:** É possível conceder a um usuário a permissão para executar (`EXECUTE`) um procedimento sem que ele precise de permissões diretas de `INSERT`, `UPDATE` ou `DELETE` sobre as tabelas que o procedimento manipula. Isso cria uma camada de segurança robusta, onde os usuários interagem com os dados apenas através de interfaces controladas e seguras.

### Estrutura e Invocação

Um procedimento pode receber parâmetros de entrada (**`IN`**), que passam valores para dentro do procedimento; retornar valores através de parâmetros de saída (**`OUT`**); ou ambos (**`INOUT`**). Sua estrutura básica contém um corpo de lógica delimitado por `BEGIN` e `END`.

**Exemplo Prático:** Um procedimento para promover um empregado, atualizando seu cargo (`job_id`) e aplicando um aumento salarial, retornando uma mensagem de status.

```sql
-- A sintaxe exata pode variar (ex: PL/SQL no Oracle, T-SQL no SQL Server)
CREATE PROCEDURE PromoverEmpregado (
    IN p_employee_id INT,
    IN p_new_job_id VARCHAR(20),
    IN p_salary_increase_pct DECIMAL(5, 2),
    OUT p_status_message VARCHAR(255)
)
BEGIN
    DECLARE v_current_salary DECIMAL(10, 2);

    -- Verifica se o empregado existe e obtém o salário atual
    SELECT salary INTO v_current_salary FROM employees WHERE employee_id = p_employee_id;

    IF v_current_salary IS NULL THEN
        SET p_status_message = 'ERRO: Empregado não encontrado.';
    ELSE
        -- Aplica a promoção
        UPDATE employees
        SET
            job_id = p_new_job_id,
            salary = v_current_salary * (1 + p_salary_increase_pct / 100)
        WHERE
            employee_id = p_employee_id;
		
        SET p_status_message = 'Empregado promovido com sucesso.';
    END IF;
END;
```

Para executar este procedimento, utiliza-se o comando `CALL`, passando os parâmetros necessários.

```sql
CALL PromoverEmpregado(101, 'IT_MAN', 15.0, @status);
-- A variável @status conteria a mensagem de sucesso ou erro.
```

## Funções Definidas pelo Usuário (UDFs)

Uma **função definida pelo usuário (UDF)** é outra forma de rotina armazenada, mas com uma distinção fundamental: uma função **deve, obrigatoriamente, retornar um único valor** (seja um escalar como um número ou texto, ou um conjunto de dados como uma tabela) e é projetada para ser usada **dentro** de outras instruções SQL, como um `SELECT` ou `WHERE`.

### Procedimentos vs. Funções: A Escolha Certa

A decisão entre criar um procedimento ou uma função depende inteiramente do objetivo.

|Característica|Procedimento Armazenado|Função (UDF)|
|---|---|---|
|**Retorno**|Não retorna um valor diretamente; usa parâmetros `OUT` para passar resultados.|**Deve** retornar um único valor ou um conjunto de dados (`RETURNS ...`).|
|**Invocação**|Autônoma, com a instrução `CALL`.|Dentro de uma expressão SQL (`SELECT func(), WHERE col = func()`).|
|**Uso Principal**|Executar uma **ação**, um processo com múltiplos passos, modificar dados.|Realizar um **cálculo** ou uma busca específica e retornar o resultado para uso imediato.|
|**Efeitos Colaterais**|Projetado para modificar o estado do banco de dados (DML).|Idealmente, não deve modificar dados (funções em `SELECT` não podem), focando em retornar um valor.|

### Tipos de Funções

- **Funções Escalares:** Retornam um único valor (ex: `INTEGER`, `VARCHAR`, `DATE`). São perfeitas para cálculos reutilizáveis.
- **Funções com Valor de Tabela (Table-Valued Functions):** Retornam um conjunto de resultados (uma tabela). Podem ser usadas na cláusula `FROM` de uma consulta, agindo como uma "visão parametrizada".

**Exemplo de Função Escalar:** Uma função que calcula a idade de um empregado com base em sua data de nascimento.

```sql
CREATE FUNCTION CalcularIdade (
    p_data_nascimento DATE
)
RETURNS INT
BEGIN
    -- Lógica para calcular a diferença em anos entre a data atual e a data de nascimento
    DECLARE v_idade INT;
    SET v_idade = YEAR(CURRENT_DATE) - YEAR(p_data_nascimento);
    -- ... lógica adicional para ajuste de mês/dia ...
    RETURN v_idade;
END;
```

**Invocação da Função Escalar em um `SELECT`:**

```sql
SELECT
    last_name,
    hire_date,
    CalcularIdade(birth_date) AS Idade
FROM
    employees;
```

## Gatilhos (Triggers)

Enquanto procedimentos e funções são passivos e aguardam ser chamados, os **gatilhos (triggers)** são proativos. Um gatilho é um tipo especial de procedimento armazenado que é executado **automaticamente** pelo SGBD em resposta a um evento de modificação de dados (`INSERT`, `UPDATE` ou `DELETE`) em uma tabela específica. Eles são a principal ferramenta para implementar lógica reativa e auditoria no banco de dados.

### A Anatomia Detalhada de um Gatilho

- **Momento (`BEFORE` / `AFTER`):** Define se o gatilho dispara _antes_ da operação DML ser aplicada (útil para validar ou modificar os dados que estão chegando) ou _depois_ que a operação já foi concluída (útil para registrar auditorias ou disparar ações em cascata). A opção `INSTEAD OF` é específica para visões, permitindo torná-las atualizáveis.
- **Evento (`INSERT` / `UPDATE` / `DELETE`):** O comando DML que aciona o gatilho.
- **Escopo (`FOR EACH ROW` / `FOR EACH STATEMENT`):** Um gatilho `FOR EACH ROW` dispara uma vez para cada linha individualmente afetada. Se um `UPDATE` altera 50 linhas, o gatilho executa 50 vezes. Já um gatilho `FOR EACH STATEMENT` dispara apenas uma vez, não importando quantas linhas foram afetadas. `FOR EACH ROW` é o mais comum e poderoso.
- **Referência de Dados (`OLD` e `NEW`):** Dentro de um gatilho `FOR EACH ROW`, o SGBD disponibiliza duas "pseudolinhas" especiais:
    - **`NEW`**: Contém os valores da linha **após** a modificação (para `INSERT` e `UPDATE`).
    - **`OLD`**: Contém os valores da linha **antes** da modificação (para `UPDATE` e `DELETE`).
        
        Essa capacidade de comparar o estado antigo e novo é o que torna os gatilhos tão úteis para lógicas complexas e auditoria.

**Exemplo Prático:** Prevenindo Rebaixamentos Salariais

Vamos criar um gatilho que impede que o salário de um empregado seja reduzido.

```sql
CREATE TRIGGER trg_prevenir_reducao_salario
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
    -- Compara o novo salário proposto com o antigo
    IF NEW.salary < OLD.salary THEN
        -- Se o novo salário for menor, impede a operação lançando um erro.
        -- A sintaxe para sinalizar um erro varia (ex: SIGNAL SQLSTATE no padrão)
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Redução de salário não é permitida.';
    END IF;
END;
```

Com este gatilho ativo, qualquer tentativa de `UPDATE` que diminua o salário de um empregado será automaticamente bloqueada pelo banco de dados.

## Considerações Finais

Ao concluir o estudo sobre as extensões procedurais, finalizamos nossa jornada abrangente pelos múltiplos paradigmas da linguagem SQL. Partimos de sua essência declarativa para definir, manipular e consultar dados; passamos pelos mecanismos de controle que garantem segurança e integridade; e exploramos as estruturas que otimizam e abstraem o acesso. Com o conhecimento de procedimentos, funções e gatilhos, vimos como transformar o banco de dados de um simples repositório de dados em um ambiente dinâmico e inteligente, capaz de encapsular e executar ativamente regras de negócio complexas.

Recapitulamos os três principais objetos procedurais: **Procedimentos Armazenados**, invocados com `CALL` para executar ações; **Funções Definidas pelo Usuário**, utilizadas dentro de expressões SQL para retornar valores; e **Gatilhos**, que reagem automaticamente a eventos de modificação de dados. Cada um serve a um propósito distinto, mas juntos eles compõem um poderoso arsenal para centralizar a lógica, melhorar o desempenho e aumentar a segurança das aplicações.

Esta base completa, que vai da DDL à TCL, passando pela DML, Índices, Visões e agora pelo SQL/PSM, capacita o leitor não apenas a utilizar bancos de dados relacionais, mas a projetar, desenvolver e manter sistemas de dados robustos, eficientes e seguros, prontos para os desafios do mundo real.
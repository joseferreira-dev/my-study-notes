# Capítulo 4 – Visões (Views): Abstraindo a Complexidade e Gerenciando o Acesso

Até o momento, nossa interação com o banco de dados se deu diretamente sobre as tabelas base, onde os dados são fisicamente armazenados. No entanto, à medida que os esquemas crescem e as consultas se tornam mais complexas, acessar diretamente as tabelas nem sempre é a abordagem mais eficiente ou segura. Para resolver isso, o SQL oferece um poderoso objeto de banco de dados conhecido como **Visão (View)**. Uma visão pode ser entendida como uma **tabela virtual**, cujo conteúdo não é armazenado, mas sim dinamicamente gerado a partir de uma instrução `SELECT` predefinida e nomeada.

O propósito fundamental de uma visão é criar uma camada de abstração sobre as tabelas base. Em vez de expor uma estrutura de dados complexa, com múltiplas junções e cálculos, uma visão pode apresentar esses dados de uma forma simples, intuitiva e customizada para uma necessidade específica. Elas funcionam como uma "lente" através da qual os usuários e aplicações podem enxergar os dados, simplificando a interação e, ao mesmo tempo, proporcionando um mecanismo robusto para controle de acesso e consistência.

Neste capítulo, exploraremos em profundidade o conceito de visões. Aprenderemos a sintaxe para criá-las e gerenciá-las, detalharemos seus múltiplos benefícios práticos e, mais importante, investigaremos a distinção crucial entre visões que são apenas para leitura e aquelas que permitem a manipulação de dados. Ao final, teremos o conhecimento para utilizar visões como uma ferramenta estratégica no projeto e na manutenção de sistemas de banco de dados seguros, eficientes e fáceis de usar.

## Criando e Gerenciando Visões

Uma visão é, em sua essência, um comando `SELECT` armazenado no dicionário de dados do SGBD. Sua criação e remoção são operações de DDL.

### A Sintaxe de Criação: CREATE VIEW

A sintaxe para criar uma visão é direta: utiliza-se o comando `CREATE VIEW`, seguido pelo nome desejado para a visão, a palavra-chave `AS` e, por fim, a consulta `SELECT` que definirá seu conteúdo.

**Sintaxe Geral:**

```sql
CREATE VIEW nome_da_view AS
SELECT coluna1, coluna2, ...
FROM tabela_base
WHERE condicao;
```

**Exemplo:** Criar uma visão para listar os dados de contato dos professores que lecionam a disciplina de 'Informática'.

```sql
CREATE VIEW profs_informatica AS
SELECT pf.primeironome, pf.ultimonome, pf.telefone, pf.email
FROM professores pf
NATURAL JOIN disciplina d
WHERE d.nome = 'Informática';
```

Uma vez criada, a visão pode ser utilizada em qualquer cláusula `FROM` como se fosse uma tabela regular. Toda a complexidade da junção e da filtragem fica encapsulada, e o usuário final precisa apenas conhecer o nome da visão.

**Exemplo de Consulta:**

```sql
SELECT * FROM profs_informatica;
```

### Removendo Visões: DROP VIEW

Quando uma visão não é mais necessária, ela pode ser removida do banco de dados com o comando `DROP VIEW`. Esta ação remove apenas a definição da visão; os dados nas tabelas base subjacentes não são afetados de forma alguma.

**Sintaxe:**

```sql
DROP VIEW nome_da_visao;
```

## Os Benefícios das Visões em Detalhe

O uso de visões traz vantagens significativas para o desenvolvimento e a manutenção de um sistema de banco de dados.

- **Simplificação de Consultas Complexas:** Visões podem ocultar a complexidade de junções entre múltiplas tabelas, cálculos e funções. Um usuário ou desenvolvedor pode realizar uma consulta simples sobre a visão, sem precisar conhecer ou reescrever a lógica complexa subjacente a cada vez.
- **Segurança e Controle de Acesso:** Este é um dos usos mais importantes. Uma visão pode ser usada para restringir o acesso dos usuários a um subconjunto específico dos dados. É possível criar uma visão que exponha apenas determinadas colunas (segurança em nível de coluna) ou determinadas linhas (segurança em nível de linha) de uma tabela, de acordo com o perfil do usuário. Por exemplo, uma visão para o departamento de RH pode mostrar a coluna de salário, enquanto outra visão para outros departamentos omite essa informação.
- **Abstração da Lógica de Negócio:** Visões permitem encapsular regras de negócio e cálculos diretamente no banco de dados. Se uma regra de negócio mudar (por exemplo, a fórmula para calcular a comissão de um vendedor), basta modificar a consulta na definição da visão. Todas as aplicações e relatórios que utilizam essa visão refletirão imediatamente a nova lógica, garantindo consistência e facilitando a manutenção.
- **Independência Lógica dos Dados:** Uma visão cria uma camada de separação entre as aplicações e a estrutura física das tabelas. Se as tabelas base precisarem ser reestruturadas (por exemplo, uma tabela ser dividida em duas), é possível manter a visão com a mesma estrutura original. As aplicações continuarão funcionando sem modificação, pois interagem com a visão, que agora extrai seus dados da nova estrutura de tabelas.

## Visões Atualizáveis vs. Visões Somente Leitura

Como uma visão se comporta como uma tabela virtual, é natural perguntar se é possível executar comandos DML (`INSERT`, `UPDATE`, `DELETE`) sobre ela. A resposta é: depende. Algumas visões são **atualizáveis**, o que significa que as modificações feitas na visão são propagadas para a tabela base subjacente. Outras são **somente leitura**.

O princípio fundamental que determina a atualizabilidade de uma visão é a capacidade do SGBD de mapear, sem qualquer ambiguidade, uma linha da visão a exatamente uma linha em **uma única tabela base**. Se esse mapeamento direto não for possível, a visão será, por natureza, somente leitura.

### As Regras da Não Atualização

De acordo com o padrão SQL, uma visão **não é atualizável** se sua definição incluir qualquer um dos seguintes elementos:

- **Múltiplas Tabelas:** A cláusula `FROM` referencia mais de uma tabela (ou seja, a visão é baseada em uma junção).
- **Funções de Agregação:** A lista do `SELECT` contém funções como `SUM()`, `COUNT()`, `AVG()`, `MAX()` ou `MIN()`.
- **Cláusulas de Agrupamento:** A consulta contém as cláusulas `GROUP BY` ou `HAVING`.
- **Valores Distintos:** A consulta utiliza a palavra-chave `DISTINCT`.
- **Colunas Calculadas:** A lista do `SELECT` contém colunas que são resultado de expressões ou cálculos (ex: `salario * 1.15`).

## Garantindo a Integridade com WITH CHECK OPTION

Para visões que são atualizáveis, surge um problema potencial: uma operação de `UPDATE` ou `INSERT` na visão poderia fazer com que a linha modificada ou inserida "desaparecesse" da própria visão. Isso acontece se a modificação fizer com que a linha não satisfaça mais à condição da cláusula `WHERE` da visão.

Para prevenir esse comportamento, pode-se adicionar a cláusula `WITH CHECK OPTION` ao final da definição da visão. Esta cláusula instrui o SGBD a rejeitar qualquer operação de `INSERT` ou `UPDATE` que resulte em uma linha que não seria visível através da própria visão.

**Exemplo:** Criar uma visão que mostra apenas os empregados do departamento 50, garantindo que ninguém possa ser movido para fora deste departamento através da visão.

```sql
CREATE OR REPLACE VIEW vw_empregados_dep50 AS
SELECT employee_id, last_name, job_id, department_id
FROM employees
WHERE department_id = 50
WITH CHECK OPTION;
```

Com essa visão, uma tentativa de executar `UPDATE vw_empregados_dep50 SET department_id = 80 WHERE employee_id = 124;` resultaria em um erro, pois o novo `department_id = 80` viola a condição `WHERE department_id = 50` da visão.

## Considerações Finais

Neste capítulo, expandimos nosso conhecimento para além da manipulação direta das tabelas base, explorando um dos objetos mais versáteis e estratégicos do SQL: a **Visão (View)**. Compreendemos que uma visão não é um contêiner de dados, mas sim uma "lente" predefinida — uma consulta `SELECT` armazenada — através da qual podemos observar e interagir com os dados. Elas representam uma camada de abstração fundamental, que serve como ponte entre a complexidade da estrutura física do banco de dados e as necessidades de simplicidade, segurança e consistência das aplicações e dos usuários.

Detalhamos os múltiplos benefícios das visões, desde a simplificação de consultas complexas, encapsulando junções e cálculos em um nome simples, até seu papel crucial como ferramenta de segurança, permitindo um controle granular sobre quais linhas e colunas são expostas. Analisamos a sintaxe para sua criação e remoção e, mais importante, aprofundamos na distinção crítica entre visões atualizáveis e visões somente leitura, dissecando as regras que governam a capacidade de uma visão propagar operações DML para suas tabelas subjacentes. Por fim, vimos como a cláusula `WITH CHECK OPTION` reforça a integridade dos dados, garantindo que as modificações através de uma visão permaneçam consistentes com sua própria definição.

Com o conhecimento para estruturar (`DDL`), manipular (`DML`) e agora abstrair (`VIEW`) os dados, nossa atenção se volta para a governança e a confiabilidade das operações. É preciso definir quem pode acessar esses objetos — tabelas e visões — e como garantir que as sequências de modificações de dados sejam executadas de forma segura e íntegra. Este é o domínio da **Linguagem de Controle de Dados (DCL)** e da **Linguagem de Controle de Transação (TCL)**, que serão o foco de nossa próxima exploração.
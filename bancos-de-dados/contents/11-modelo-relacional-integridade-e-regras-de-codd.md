# Capítulo 11 – Modelo Relacional: Restrições de Integridade e Regras de Codd

## Restrições de Integridade: Garantindo a Consistência dos Dados

Os sistemas de banco de dados relacionais foram projetados para armazenar dados de maneira estruturada e eficiente. No entanto, armazenar dados não é suficiente: é preciso garantir que os dados permaneçam **consistentes, corretos e significativos ao longo do tempo**. Para isso, o modelo relacional adota um conjunto de **restrições de integridade**, que funcionam como regras impostas ao conteúdo das tabelas para impedir inserções, atualizações ou exclusões incorretas.

Essas restrições são fundamentadas em **regras do negócio**, previamente definidas, e são aplicadas pelo próprio **Sistema de Gerenciamento de Banco de Dados (SGBD)**, que as impõe automaticamente sempre que ocorre alguma modificação nos dados. O objetivo é assegurar que os dados estejam sempre em conformidade com a lógica do sistema e que eventuais erros acidentais, oriundos de ações de usuários autorizados, **não comprometam a integridade da base**.

Uma das grandes vantagens dos bancos relacionais, em comparação com modelos anteriores como o hierárquico ou em rede, está justamente na possibilidade de **definir essas regras diretamente no banco**, de forma declarativa ou procedural.

### Integridade Declarativa e Procedural

As **restrições declarativas** são aquelas definidas diretamente na **linguagem de definição de dados (DDL)** do SQL. Elas se aplicam de forma simples e clara, utilizando palavras-chave como `PRIMARY KEY`, `UNIQUE`, `NOT NULL`, `CHECK` e `FOREIGN KEY`. Essas declarações dizem ao SGBD **quais valores são válidos** e como as tabelas devem se relacionar.

Já as **restrições procedurais** exigem lógica programada e são utilizadas quando as regras de integridade são mais complexas ou não podem ser expressas facilmente por declarações simples. Essa abordagem é feita por meio de **triggers (gatilhos)**, **stored procedures (procedimentos armazenados)** e **assertions (afirmações)**, que são **executados automaticamente** diante de determinados eventos, condições ou operações nos dados.

Para exemplificar, imagine uma empresa que só deve permitir a inclusão de novos funcionários se eles forem aprovados em concurso público. Um **gatilho (trigger)** pode ser configurado para verificar essa condição toda vez que uma nova tupla for inserida na tabela `FUNCIONARIO`. Caso a condição não seja satisfeita, a inserção é cancelada automaticamente, garantindo que os dados permaneçam válidos.

### Tipos Fundamentais de Restrições de Integridade

Podemos agora discutir os principais tipos de restrições adotados em um banco de dados relacional. A seguir, detalharemos cada uma delas com exemplos e observações específicas.

#### Integridade de Domínio

A **integridade de domínio** assegura que os valores atribuídos a um atributo pertençam a um **conjunto permitido**, conhecido como **domínio**. Por exemplo, uma coluna `idade` deve conter apenas valores inteiros não negativos, e uma coluna `email` deve obedecer a um padrão específico de texto. Essa verificação pode ser feita com o uso de **restrições `CHECK`** em SQL.

Além de manter os dados válidos, essa restrição **garante a coerência semântica**: evita, por exemplo, que uma data seja comparada com um número inteiro ou que uma operação de soma seja aplicada a uma string de caracteres.

#### Integridade de Chave (Unicidade)

Essa restrição determina que determinados atributos (ou conjunto de atributos) devem ter **valores únicos** em todas as tuplas da relação. É o caso típico das chaves primárias, mas também pode ser aplicado a colunas alternativas por meio da cláusula `UNIQUE`.

Se tivermos uma tabela `CLIENTES` com a coluna `cpf`, a integridade de chave garante que **nenhum cliente terá o mesmo CPF que outro**, preservando a unicidade.

#### Integridade de Vazio (NULL ou NOT NULL)

Em alguns contextos, pode ser necessário permitir que determinados atributos não tenham valor. A **restrição de vazio** define se um atributo pode receber valores `NULL` (desconhecidos, não aplicáveis ou ausentes) ou se deve **obrigatoriamente conter um valor**.

A decisão de permitir ou não valores nulos depende da **semântica dos dados**. Por exemplo, `data_de_nascimento` deve ser obrigatória (NOT NULL), mas `telefone_secundário` pode ser opcional.

#### Integridade de Entidade

Esta restrição exige que **toda relação possua uma chave primária**, e mais do que isso: **essa chave não pode ter valores nulos**. Afinal, a chave primária é o identificador único de cada tupla, e valores nulos são indefinidos — o que comprometeria a unicidade exigida.

#### Integridade Referencial

Talvez a mais importante no contexto dos relacionamentos entre tabelas, a **integridade referencial** garante que um **valor presente em uma chave estrangeira (FK)** esteja de fato presente em uma **chave primária (PK)** ou em um **atributo `UNIQUE`** da tabela referenciada.

Se tivermos uma tabela `PEDIDOS` com uma coluna `cliente_id`, e esta for uma chave estrangeira apontando para `id_cliente` na tabela `CLIENTES`, o SGBD impedirá a inserção de um `cliente_id` que **não exista** na tabela `CLIENTES`. Isso evita "referências fantasmas".

Valores nulos também são permitidos em chaves estrangeiras, caso o relacionamento seja opcional.

#### Integridade Semântica

A integridade semântica refere-se às **regras de negócio complexas**, que exigem lógica adicional para serem implementadas. Um exemplo seria a proibição de que um aluno se matricule em duas turmas no mesmo horário, ou o controle de estoque que impede a venda de um produto com quantidade zero.

Esse tipo de integridade é implementado por **triggers** ou **assertions**, que funcionam como cláusulas condicionais em nível de sistema. Embora sua aplicação seja mais complexa, ela é essencial para garantir que o banco de dados **reflita corretamente a realidade do domínio da aplicação**.

## As Regras de Codd: Fundamentando o Modelo Relacional

A proposta de Edgar Frank Codd para o modelo relacional não foi apenas uma inovação tecnológica, mas também uma **ruptura conceitual** com os modelos anteriores. Para consolidar sua proposta e estabelecer critérios objetivos para a **definição de um banco de dados verdadeiramente relacional**, Codd formulou, em 1985, um conjunto de **13 regras fundamentais** numeradas de 0 a 12.

Essas regras foram concebidas como **um teste de conformidade**, ou seja, um sistema que deseje ser chamado de relacional **deve satisfazer a todos esses requisitos**. Na prática, poucas implementações comerciais as cumprem integralmente, mas elas continuam sendo **referência teórica obrigatória** para qualquer estudo aprofundado do modelo relacional.

Vamos agora explorar cada uma das regras com o cuidado e a profundidade que merecem.

### Regra 0 – A Regra Fundamental

> “Para que um sistema seja qualificado como relacional, ele deve usar exclusivamente suas próprias capacidades relacionais para gerenciar o banco de dados.”

Essa regra estabelece a **condição base** para todas as demais: o sistema não deve depender de extensões externas, procedimentos não relacionais ou estruturas _ad hoc_ para implementar suas funcionalidades. Todo o **gerenciamento de dados, integridade, segurança e acesso** deve ocorrer **dentro da estrutura do modelo relacional**.

Em outras palavras, **qualquer banco de dados que descumpra essa regra perde sua natureza relacional**.

### Regra 1 – Regra da Informação

> “Toda informação em um banco de dados relacional é representada, logicamente, apenas por valores em tabelas.”

Essa regra consagra o princípio mais básico do modelo relacional: **tudo é representado em tabelas**. Não há listas encadeadas, nós, registros em árvore ou ponteiros visíveis ao usuário. Uma relação é, essencialmente, uma **tabela bidimensional**, composta por linhas (tuplas) e colunas (atributos), e todas as informações, inclusive metadados, seguem essa estrutura.

### Regra 2 – Acesso Garantido

> “Cada valor atômico (datum) em um banco de dados relacional é logicamente acessível por meio da combinação do nome da tabela, da chave primária da linha e do nome da coluna.”

Essa regra reforça a ideia de **acessibilidade completa dos dados**. Não pode haver dados ocultos, não endereçáveis ou "encapsulados". Através da estrutura de tabelas e chaves, o SGBD deve ser capaz de identificar **unicamente** cada valor individual armazenado.

Exemplo: Para acessar a idade do aluno com matrícula 202301 em uma tabela `ALUNOS`, basta informar:

```plaintext
Tabela: ALUNOS
Chave primária: 202301
Atributo: IDADE
```

### Regra 3 – Tratamento Sistemático de Valores Nulos

> “Valores nulos devem ser tratados de maneira uniforme e sistemática, independentemente do tipo de dado.”

O valor **nulo** representa algo **desconhecido ou não aplicável**, e o sistema deve ser capaz de manipulá-lo adequadamente: em operações aritméticas, lógicas, de comparação ou agrupamento. Além disso, é essencial que a distinção entre valores nulos e valores reais (inclusive zero e string vazia) seja **clara e consistente**.

### Regra 4 – Catálogo On-line Ativo

> “O catálogo do banco de dados (metadados) deve ser acessível aos usuários através da linguagem relacional, da mesma forma que os dados normais.”

Aqui, Codd afirma que a **descrição do banco de dados** (isto é, os nomes de tabelas, atributos, tipos de dados, chaves, etc.) deve estar disponível como **tabelas especiais do sistema**, acessíveis por SQL. Os catálogos ou dicionários de dados devem poder ser **consultados, atualizados e interrogados** como qualquer outra relação.

### Regra 5 – Sublinguagem Abrangente de Dados

> “O sistema deve oferecer uma sublinguagem de dados com sintaxe bem definida que permita todas as operações: definição, manipulação, controle de transações e segurança.”

Ainda que um SGBD possa suportar múltiplas linguagens (como interfaces gráficas, APIs, etc.), **deve haver ao menos uma linguagem relacional completa** que possibilite:

- Definir estruturas de dados (`CREATE`, `ALTER`)
- Manipular dados (`INSERT`, `UPDATE`, `DELETE`, `SELECT`)
- Criar e consultar visões (`VIEW`)
- Definir restrições e regras de integridade
- Controlar acesso e permissões (`GRANT`, `REVOKE`)
- Administrar transações (`BEGIN`, `COMMIT`, `ROLLBACK`)

A linguagem **SQL** é o principal exemplo de sublinguagem que atende, ao menos parcialmente, a esse requisito.

### Regra 6 – Atualização de Visualizações

> “Todas as visualizações que forem teoricamente atualizáveis devem também ser atualizáveis pelo sistema.”

Uma **view** ou **visão** é uma tabela virtual construída a partir de consultas sobre outras tabelas. Essa regra estabelece que, se for **lógica e estruturalmente possível** atualizar uma view (isto é, se ela representar dados diretos e não derivados, sem ambiguidade), então o SGBD **deve permitir essas operações**.

Exemplo: Uma visão criada com `SELECT * FROM CLIENTES WHERE ESTADO = 'SP'` deveria ser atualizável, pois se baseia em uma única tabela, sem agregações ou `JOIN`.

### Regra 7 – Inserção, Atualização e Exclusão de Alto Nível

> “A manipulação de dados deve ser possível para conjuntos completos de tuplas e não apenas uma de cada vez.”

Essa regra diferencia os bancos relacionais dos sistemas baseados em registros. Ao invés de exigir operações linha a linha, o modelo relacional deve permitir **operações em conjuntos**, como:

```sql
DELETE FROM FUNCIONARIOS WHERE SALARIO < 2000;
```

Essa abordagem é mais eficiente, segura e compatível com a **semântica do modelo relacional**, que trata as relações como conjuntos.

### Regra 8 – Independência Física dos Dados

> “Modificações na forma física de armazenamento dos dados não devem afetar o acesso lógico aos mesmos.”

O sistema deve permitir **alterações na organização dos dados em disco**, como reindexação, particionamento ou mudança de estrutura interna, **sem impactar os programas e usuários que acessam os dados**. A independência física garante **flexibilidade e manutenção sem rupturas**.

### Regra 9 – Independência Lógica dos Dados

> “Mudanças na estrutura lógica (esquemas) devem provocar pouco ou nenhum impacto nas aplicações.”

Essa regra é ainda mais exigente: se, por exemplo, um atributo for adicionado ou renomeado em uma tabela, os sistemas que utilizam a base de dados **não devem deixar de funcionar**, desde que não dependam diretamente daquele atributo.

### Regra 10 – Independência de Integridade

> “As restrições de integridade devem ser definíveis na linguagem relacional e armazenadas no catálogo, e não embutidas nos aplicativos.”

O sistema deve permitir que as regras de integridade — como chaves primárias, chaves estrangeiras, domínios e restrições — sejam definidas diretamente no banco, sem depender da lógica de aplicativos externos. Isso promove **centralização das regras de negócio**, melhora a **manutenção** e reduz **erros de inconsistência**.

### Regra 11 – Independência de Distribuição

> “A localização geográfica dos dados não deve afetar sua manipulação.”

Em sistemas distribuídos, os dados podem estar fisicamente espalhados por diversos servidores ou data centers. Para o usuário e para os aplicativos, isso **deve ser completamente transparente**. Consultas e operações devem funcionar **como se os dados estivessem localizados em um único ponto**.

### Regra 12 – Não Subversão

> “Não deve ser possível subverter as regras de integridade do modelo relacional usando linguagens de baixo nível.”

Em muitos sistemas, há linguagens de acesso direto aos dados (como APIs de baixo nível, comandos de shell ou utilitários internos). Essa regra exige que **nenhuma dessas ferramentas possa contornar as restrições e garantias oferecidas pelo modelo relacional**. As regras de integridade devem ser **respeitadas independentemente do meio de acesso**.

### Resumindo

As **regras de Codd** não são apenas um marco histórico no desenvolvimento dos bancos de dados, mas continuam sendo um **referencial teórico valioso**. Elas reforçam a importância de uma abordagem rigorosa, orientada a princípios, para garantir **coerência, integridade, independência e segurança** em sistemas relacionais.

Muitos dos SGBDs modernos ainda não cumprem todas as regras integralmente, mas o modelo relacional permanece como **a espinha dorsal da maioria dos bancos de dados corporativos, científicos e governamentais**.

## Considerações Finais

Neste capítulo, exploramos de forma detalhada dois pilares fundamentais do modelo relacional: as **restrições de integridade** e as **regras de Codd**. As restrições de integridade são essenciais para garantir que os dados armazenados no banco estejam sempre **corretos, válidos e consistentes**, conforme as regras do negócio e a lógica da aplicação. Vimos que essas restrições podem ser **declarativas** ou **procedurais**, e abrangem desde domínios de atributos até relacionamentos entre tabelas por meio de chaves estrangeiras.

Além disso, estudamos as 13 regras formuladas por Codd, que estabelecem os **critérios formais** para que um sistema de banco de dados possa ser considerado **verdadeiramente relacional**. Essas regras reforçam princípios como **acesso uniforme aos dados**, **tratamento de valores nulos**, **independência física e lógica dos dados**, e a importância de um **catálogo de metadados acessível**.

O modelo relacional, com sua base teórica sólida e seu foco na integridade, permanece até hoje como o paradigma dominante no armazenamento de dados. As ferramentas modernas podem não implementar todas as regras de Codd em sua totalidade, mas seu legado continua moldando os fundamentos da engenharia de dados.

No próximo capítulo, aprofundaremos o estudo da **álgebra relacional**, um dos mecanismos mais importantes para manipulação de dados em bancos relacionais, e que serve como base para a linguagem SQL.

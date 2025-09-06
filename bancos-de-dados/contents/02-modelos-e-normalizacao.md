# Capítulo 2 – Modelo Relacional, Conceitual e Normalização

No capítulo anterior, estabelecemos os fundamentos sobre o que são dados, bancos de dados e os sistemas que os gerenciam. Vimos que existem diferentes paradigmas para organizar a informação, cada um com suas forças e finalidades. Agora, vamos nos aprofundar no paradigma que revolucionou a tecnologia da informação e que, até hoje, serve como a espinha dorsal da vasta maioria dos sistemas transacionais: o **Modelo Relacional**. Proposto por Edgar F. Codd em 1970, este modelo introduziu uma forma lógica e matematicamente fundamentada de representar dados, trazendo clareza, consistência e simplicidade a um campo que, até então, era complexo e despadronizado.

## Modelo Relacional

O modelo relacional é, de longe, o principal modelo de banco de dados utilizado no mundo. Sua premissa é representar todos os dados de forma estruturada, em um formato intuitivo de tabelas. No jargão técnico do modelo, essas tabelas são chamadas de **relações**.

Cada relação é projetada para armazenar dados sobre um tipo específico de **entidade**. Uma entidade é qualquer objeto ou conceito do mundo real sobre o qual desejamos guardar informações, como um `Cliente`, um `Produto` ou uma `Nota Fiscal`.

A estrutura de uma relação é composta por:

- **Atributos:** São as **colunas** da tabela. Cada atributo representa uma característica ou propriedade da entidade. Para uma entidade `Cliente`, os atributos poderiam ser `CPF`, `Nome` e `Cidade`.
- **Tuplas:** São as **linhas** da tabela, também conhecidas como **registros**. Cada tupla representa uma ocorrência única da entidade, contendo um conjunto de valores, um para cada atributo.

O diagrama a seguir ilustra a anatomia de uma relação.

<div align="center">
<img width="700px" src="./img/02-modelo-relacional.png">
</div>

Neste exemplo, a relação (tabela) armazena dados sobre a entidade "Cliente". `CPF`, `Nome` e `Cidade` são seus **atributos**. Cada linha é uma **tupla** que representa um cliente específico, como "José da Silva". O conjunto de todos os valores em uma linha (`123.456.789-00`, `José da Silva`, `São Paulo`) é a tupla, enquanto cada "célula" individual, como `São Paulo`, é um **datum**, a menor unidade de dado.

Conforme vimos no capítulo anterior, cada atributo possui um **domínio**, que é o conjunto de valores permitidos para ele. Ao criar a tabela, definimos o tipo de dado para cada coluna (ex: `CPF` como `CHAR(14)`, `Nome` como `VARCHAR(100)`), restringindo assim seu domínio e garantindo a integridade dos dados. Essa definição é realizada através de comandos na **SQL (Structured Query Language)**, a linguagem padrão para a criação e manipulação de bancos de dados relacionais.

### As Propriedades de uma Relação

A simplicidade visual de uma tabela esconde um rigoroso fundamento matemático baseado na teoria dos conjuntos. É essa base que confere ao modelo relacional sua consistência. Segundo a teoria, popularizada por autores como C.J. Date, toda relação deve obedecer a um conjunto de propriedades fundamentais:

- **Tuplas não são ordenadas de cima para baixo:** Uma relação é um _conjunto_ de tuplas. Na teoria dos conjuntos, os elementos não possuem uma ordem intrínseca. Portanto, a ordem em que as linhas são exibidas em uma consulta não tem significado, a menos que uma ordenação seja explicitamente solicitada (com o comando `ORDER BY` em SQL).
- **Atributos não são ordenados da esquerda para a direita:** Da mesma forma, a ordem das colunas é irrelevante para o modelo. Os atributos são referenciados por seus nomes, não por sua posição.
- **Cada tupla deve ser única:** Como uma relação é um conjunto, ela não pode conter elementos duplicados. Isso significa que não podem existir duas linhas em uma tabela que sejam exatamente idênticas em todos os seus valores. Na prática, essa unicidade é garantida por uma **chave primária**.
- **Os valores dos atributos são atômicos:** Esta é uma das propriedades mais importantes. Cada "célula" da tabela (a intersecção de uma linha e uma coluna) deve conter um único e indivisível valor. Não é permitido, por exemplo, armazenar uma lista de telefones em um único campo `telefone`. Cada telefone deveria ser um registro separado em outra tabela.
- **Cada tupla contém um valor para cada atributo:** Toda tupla deve ser completa, possuindo um valor (do tipo de dado apropriado) para cada um dos atributos da relação. Um valor pode ser `NULL` (nulo), que é um marcador especial para indicar a ausência de um valor, mas o atributo em si está presente na tupla.

### Chaves: Identificando e Relacionando Dados

A verdadeira força do modelo relacional não está apenas em armazenar dados em tabelas, mas na sua capacidade de criar **relações** lógicas e consistentes entre essas tabelas. Os mecanismos que permitem identificar registros de forma única e construir essas pontes entre as tabelas são as **chaves**. As duas mais importantes são a chave primária, que garante a identidade, e a chave estrangeira, que constrói os relacionamentos.

#### Chave Primária (Primary Key - PK)

Uma **chave primária**, ou **PK** (_Primary Key_), é um atributo (coluna) ou um conjunto de atributos cuja finalidade é **identificar de forma única e inequívoca cada tupla (linha)** dentro de uma relação (tabela). Ela é o "CPF" de cada registro, garantindo que não existam duas linhas com o mesmo identificador.

Por exemplo, em uma tabela de `Cidades`, o nome da cidade poderia se repetir em diferentes estados. Portanto, `nome_cidade` não seria uma boa chave primária. Um código `IBGE`, no entanto, que é único para cada município, seria um excelente candidato.

Para cumprir sua função, toda chave primária deve obedecer a três regras fundamentais:

- **Unicidade:** O valor da chave primária não pode se repetir na mesma tabela. O SGBD impõe essa regra através de uma restrição `UNIQUE`.
- **Não Nulidade:** Uma chave primária jamais pode ter um valor nulo (`NULL`), ou seja, desconhecido ou vazio. Um registro precisa de um identificador para existir e ser localizado. O SGBD impõe essa regra com uma restrição `NOT NULL`.
- **Imutabilidade:** Embora tecnicamente possível, o valor de uma chave primária não deve ser alterado após o registro ser criado. Alterar um identificador exigiria a atualização em cascata de todas as outras tabelas que se referem a ele, um processo arriscado e complexo.

<div align="center">
<img width="700px" src="./img/02-chaves-primarias.png">
</div>

##### Tipos de Chaves Primárias

Existem diferentes abordagens para escolher uma chave primária:

- **Chave Natural:** É um atributo que já existe naturalmente nos dados e que identifica a entidade de forma única. Exemplos incluem o `CPF` para uma pessoa, o `ISBN` para um livro ou o `Chassi` para um veículo.
- **Chave Artificial (ou Surrogate):** É um código, geralmente um número inteiro sequencial, criado artificialmente pelo SGBD com o único propósito de servir como chave primária (ex: `id_cliente`). Esta é a abordagem mais comum e recomendada, pois garante unicidade, é imutável, curta e otimizada para o desempenho das consultas.
- **Chave Composta:** Em alguns casos, uma única coluna não é suficiente para garantir a unicidade. Uma chave primária composta é formada por duas ou more colunas. Por exemplo, em uma tabela de `Itens_de_Pedido`, a chave primária poderia ser a combinação de (`id_pedido`, `id_produto`).

##### Chaves Candidatas

Frequentemente, uma tabela pode ter múltiplos atributos ou conjuntos de atributos que poderiam servir como chave primária. Em uma tabela de `Pessoas`, tanto o `CPF` quanto o `RG` são únicos. O conjunto de todas essas opções (`{CPF}, {RG}`) é chamado de **chaves candidatas**. O projetista do banco de dados escolhe uma delas para ser a chave primária, e as outras podem ser definidas como **chaves alternativas**, mantendo a restrição de unicidade.

#### Chave Estrangeira (Foreign Key - FK)

Uma **chave estrangeira**, ou **FK** (_Foreign Key_), é um atributo ou um conjunto de atributos em uma tabela (a tabela "filha") que estabelece um vínculo com a chave primária de outra tabela (a tabela "pai"). Ela é o mecanismo que efetivamente cria o relacionamento entre os dados.

A chave estrangeira impõe uma regra fundamental chamada **integridade referencial**. Isso significa que o valor de uma chave estrangeira em uma tabela deve, obrigatoriamente, corresponder a um valor de chave primária existente na tabela referenciada, ou ser nulo. Em termos práticos:

- Não é possível matricular um aluno em um curso que não existe.
- Não é possível apagar um cliente da base de dados se ainda existirem pedidos associados a ele.

Diferente das chaves primárias, as chaves estrangeiras **podem ter valores repetidos e, em muitos casos, podem ser nulas**. Isso depende da natureza do relacionamento (a cardinalidade, que veremos adiante).

<div align="center">
<img width="580px" src="./img/02-chaves-estrangeiras.png">
</div>

No exemplo acima, temos duas tabelas. A coluna `codigo` é a chave primária da tabela `curso`. Na tabela `aluno`, a coluna `cd_curso` é uma **chave estrangeira** que "aponta" para a coluna `codigo`. Isso garante que um aluno só possa ser associado a um `cd_curso` que exista na tabela `curso` (neste caso, `1` ou `2`). Note que o valor `2` se repete na coluna `cd_curso`, o que é permitido, pois mais de um aluno pode estar no mesmo curso.

A definição formal e a esmagadora boa prática do modelo relacional ditam que uma chave estrangeira deve sempre referenciar uma **chave candidata** (geralmente a chave primária) da tabela pai. Embora alguns SGBDs possam permitir tecnicamente a criação de uma referência a uma coluna não-única, essa é uma prática que viola a integridade referencial e deve ser evitada a todo custo, pois leva a ambiguidades e inconsistências nos dados.

### As Regras de Codd

Na década de 1980, com a crescente popularidade do modelo relacional, muitos fornecedores de software começaram a rotular seus produtos como "relacionais" sem, de fato, aderir aos princípios fundamentais do modelo. Em resposta, Edgar F. Codd, o "pai" do modelo relacional, publicou um conjunto de 12 regras (que na verdade são 13, começando pela regra 0) para estabelecer um padrão claro e rigoroso.

Essas regras definem o que é necessário para que um SGBD seja considerado verdadeiramente relacional. Mais do que um checklist técnico, elas representam a filosofia por trás do modelo, focada em consistência, simplicidade e independência de dados. Compreender o propósito de cada regra é fundamental para dominar o funcionamento e as vantagens do paradigma relacional.

<div align="center">
<img width="700px" src="./img/02-regras-de-codd.png">
</div>

A seguir, vamos detalhar cada uma das treze regras.

**REGRA 00 - REGRA DA FUNDAÇÃO**

> Para um sistema se qualificar como um SGBD relacional, ele deve gerenciar todas as suas bases de dados inteiramente através de suas capacidades relacionais.

- **Em termos práticos:** O SGBD não pode "trapacear". Ele deve usar o modelo relacional não apenas para armazenar os dados do usuário, mas também para gerenciar a si mesmo, incluindo seu catálogo de metadados.

**REGRA 01 - REGRA DA INFORMAÇÃO**

> Toda a informação em um banco de dados é representada explicitamente no nível lógico e de uma única forma: por valores em tabelas.

- **Em termos práticos:** Não existem "ponteiros" ou caminhos de acesso ocultos que o usuário precise conhecer. Se algo é uma informação, ela está contida em uma coluna de uma linha em uma tabela. Isso garante a simplicidade e a consistência do modelo.

**REGRA 02 - REGRA DO ACESSO GARANTIDO**

> Cada datum (a menor unidade de dado) em um banco de dados relacional é acessível logicamente através de uma combinação de nome da tabela, nome da coluna e valor da chave primária.

- **Em termos práticos:** Todo e qualquer valor no banco de dados tem um "endereço" lógico e único. Não há ambiguidade. Para encontrar qualquer dado, basta saber a tabela, a coluna e a chave primária da linha.

**REGRA 03 - REGRA DO TRATAMENTO SISTEMÁTICO DE VALORES NULOS**

> O SGBD deve permitir que cada campo possa permanecer nulo (vazio), representando "informação ausente" ou "informação inaplicável" de forma sistemática e distinta de todos os valores regulares.

- **Em termos práticos:** `NULL` não é zero, nem uma string vazia. É um marcador especial para ausência de valor. O SGBD deve tratar `NULL` de forma consistente em todas as operações (ex: em comparações, `NULL` não é igual a `NULL`).

**REGRA 04 - REGRA DO CATÁLOGO DINÂMICO ONLINE**

> A descrição do banco de dados (o catálogo de metadados) é representada no mesmo nível lógico que os dados comuns, de forma que usuários autorizados possam aplicar a mesma linguagem relacional a ele.

- **Em termos práticos:** Os metadados são armazenados em tabelas, assim como os dados normais. Um DBA pode usar `SELECT` para consultar o catálogo do sistema e descobrir quais tabelas, colunas e restrições existem no banco de dados.

**REGRA 05 - REGRA DA LINGUAGEM DE SUBDADOS ABRANGENTE**

> Um sistema relacional deve suportar pelo menos uma linguagem que seja completa em suas capacidades de definição de dados (DDL), manipulação de dados (DML), consulta (DQL) e controle de acesso (DCL).

- **Em termos práticos:** Deve existir uma linguagem poderosa e bem definida, como a **SQL**, que permita realizar todas as tarefas necessárias no banco de dados, desde a criação de tabelas até a concessão de permissões.

**REGRA 06 - REGRA DA ATUALIZAÇÃO DE VISÕES**

> Todas as visões (_views_) que são teoricamente atualizáveis também devem ser atualizáveis pelo sistema.

- **Em termos práticos:** Se uma visão é uma representação lógica simples de uma ou mais tabelas base, o sistema deve permitir que operações de `INSERT`, `UPDATE` ou `DELETE` feitas através da visão sejam corretamente refletidas nas tabelas originais.

**REGRA 07 - REGRA DE OPERAÇÕES RELACIONAIS**

> A capacidade de lidar com relações como um único operando aplica-se não apenas à recuperação de dados, mas também à inserção, atualização e exclusão.

- **Em termos práticos:** O sistema deve ser orientado a conjuntos. Um único comando deve ser capaz de modificar múltiplas linhas. Por exemplo, `UPDATE Funcionarios SET salario = salario * 1.1` deve funcionar como uma única operação, sem a necessidade de um laço para percorrer cada funcionário.

**REGRA 08 - REGRA DA INDEPENDÊNCIA FÍSICA DOS DADOS**

> Programas de aplicação e atividades de terminal permanecem inalterados quando são feitas alterações nas representações de armazenamento ou nos métodos de acesso.

- **Em termos práticos:** Esta é a **independência física** que vimos na arquitetura ANSI/SPARC. Um DBA pode adicionar um índice ou mover os arquivos do banco de dados para um novo disco sem que isso exija a alteração de uma única linha de código na aplicação.

**REGRA 09 - REGRA DA INDEPENDÊNCIA LÓGICA DOS DADOS**

> Programas de aplicação e atividades de terminal permanecem logicamente inalterados quando são feitas mudanças nas tabelas base que, teoricamente, permitam a inalteração.

- **Em termos práticos:** Esta é a **independência lógica**. É possível, por exemplo, dividir uma tabela em duas, e criar uma visão com o nome da tabela original que une as novas tabelas. A aplicação continuará a funcionar, consultando a visão, sem perceber a mudança na estrutura lógica.

**REGRA 10 - REGRA DA INDEPENDÊNCIA DE INTEGRIDADE**

> As restrições de integridade devem ser definíveis na linguagem de dados e armazenadas no catálogo, não nos programas de aplicação.

- **Em termos práticos:** Regras de negócio, como "o CPF de um cliente deve ser único" ou "o valor de um pedido não pode ser negativo", devem ser definidas no banco de dados (com `UNIQUE` ou `CHECK constraints`), não em cada aplicação que o acessa. Isso centraliza e garante a aplicação das regras.

**REGRA 11 - REGRA DA INDEPENDÊNCIA DE DISTRIBUIÇÃO**

> O usuário final não deve ser capaz de ver que os dados estão distribuídos por várias localizações. Os usuários devem ter sempre a impressão de que os dados estão localizados em apenas um local.

- **Em termos práticos:** Se o banco de dados está fisicamente distribuído em múltiplos servidores, essa complexidade deve ser totalmente transparente para o usuário, que interage com o sistema como se fosse uma única entidade.

**REGRA 12 - REGRA DA NÃO SUBVERSÃO**

> Se um sistema possui uma linguagem de baixo nível, esse nível não pode ser usado para subverter ou ignorar as regras e restrições de integridade expressas na linguagem de mais alto nível.

- **Em termos práticos:** Não pode haver uma "porta dos fundos". Se existe uma restrição que impede a inserção de salários negativos via SQL, não pode existir outra forma de acesso de mais baixo nível que permita burlar essa regra. A integridade do banco de dados é soberana.

## Modelo Conceitual

No capítulo anterior, estabelecemos que o projeto de um banco de dados evolui através de diferentes modelos, cada um com um nível de detalhe e abstração específico. Agora, vamos focar no ponto de partida desse processo: o **Modelo Conceitual**.

O processo de criação de um banco de dados começa com a definição do **minimundo**, que é a parte específica do mundo real que desejamos representar em nosso sistema. Para uma universidade, o minimundo seriam seus alunos, professores, disciplinas e matrículas; para uma loja, seriam os clientes, produtos e vendas. A partir da análise deste minimundo, levantamos um conjunto de requisitos de dados que servem como base para a modelagem.

É neste ponto que o modelo conceitual entra em cena. Ele é o nível de **mais alta abstração** na representação de um banco de dados, funcionando como uma ponte entre os requisitos de negócio (a linguagem humana) e a estrutura lógica do banco de dados (a linguagem técnica).

<div align="center">
<img width="700px" src="./img/02-niveis-de-abstracao.png">
</div>

O foco do modelo conceitual é criar uma representação de alto nível da informação, que seja clara para todos os envolvidos no projeto (gestores, usuários finais e desenvolvedores). Por essa razão, a modelagem nesta fase é **completamente independente de qualquer tecnologia**. Não importa qual SGBD será usado ou se o paradigma será relacional ou NoSQL; o modelo conceitual se concentra apenas em identificar as principais "peças" de informação e como elas se conectam. A técnica mais comum e universalmente aceita para criar o modelo conceitual é o **Modelo Entidade-Relacionamento (MER)**.

### Modelo Entidade-Relacionamento (MER)

O **Modelo Entidade-Relacionamento (MER)** é uma técnica de modelagem de dados usada para produzir um esquema conceitual de um banco de dados. Proposto por Peter Chen em 1976, o MER oferece uma maneira estruturada de visualizar os dados em termos de **entidades**, os **atributos** que as descrevem, e os **relacionamentos** entre elas.

<div align="center">
<img width="700px" src="./img/02-modelo-entidade-relacionamento.png">
</div>

É importante fazer uma distinção entre o MER e o DER:

- O **Modelo Entidade-Relacionamento (MER)** é a teoria, o conjunto de conceitos e regras abstratas que definem como representar a estrutura dos dados. É a "gramática".
- O **Diagrama Entidade-Relacionamento (DER)** é a representação gráfica e visual do MER. É o "desenho" que utiliza os símbolos e as convenções do modelo para representar um minimundo específico.

Apesar de serem tecnicamente distintos, na prática, os termos MER e DER são frequentemente usados como sinônimos. O DER é a principal ferramenta de comunicação nesta fase do projeto, pois permite visualizar, discutir e validar os requisitos de dados de forma intuitiva.

O diagrama a seguir é um exemplo de um DER simples, que utilizaremos como base para explorar em detalhe cada um de seus componentes.

<div align="center">
<img width="700px" src="./img/02-diagrama-entidade-relacionamento.png">
</div>

Como ilustrado, o diagrama é composto por vários elementos simbólicos. Nos próximos tópicos, vamos dissecar cada um desses conceitos — Entidades, Atributos e Relacionamentos — para compreender como eles se combinam para formar um modelo de dados coeso e completo.

### Entidades

Uma **entidade** é o bloco de construção central do MER. Ela representa um objeto, pessoa, lugar, evento ou conceito do mundo real (o _minimundo_) sobre o qual desejamos armazenar informações. É importante distinguir a **entidade** como um tipo ou classe de objeto (ex: o conceito de Aluno) de uma **ocorrência da entidade**, que é uma instância específica daquele tipo (ex: o aluno específico "José da Silva").

No nosso exemplo do diagrama anterior, `Cliente` e `Produto` são as entidades. Em um sistema acadêmico, as entidades poderiam ser `Alunos`, `Professores`, `Cursos` e `Disciplinas`. No DER, as entidades são representadas graficamente por retângulos.

Ao modelar um banco de dados, é crucial entender que nem todas as entidades são criadas da mesma forma. Elas são classificadas em três tipos principais, com base em sua independência e função no modelo: entidades fortes, fracas e associativas.

#### Entidades Fortes

Uma **entidade forte** representa um conceito que pode existir por si só, de forma independente de outras entidades. Ela é o pilar do modelo de dados. Tecnicamente, sua principal característica é possuir um ou mais atributos próprios que possam servir como **chave primária**, identificando cada ocorrência da entidade de forma única e inequívoca.

- **Exemplos:** `CLIENTE` (identificado pelo `CPF` ou `id_cliente`), `PRODUTO` (identificado pelo `código_de_barras` ou `id_produto`), `BANCO` (identificado pelo `código_bancario`). Cada um desses conceitos existe e pode ser identificado sem depender de nenhuma outra informação no modelo.
- **Representação no DER:** Um retângulo com linha simples.

#### Entidades Fracas

Uma **entidade fraca** é aquela cuja existência e identificação dependem de outra entidade, chamada de entidade proprietária (que é sempre uma entidade forte). Ela não possui atributos suficientes para formar uma chave primária por conta própria.

Para ser unicamente identificada, uma ocorrência de uma entidade fraca precisa da chave primária de sua entidade forte proprietária, combinada com um de seus próprios atributos, que chamamos de **atributo discriminante** (ou chave parcial).

- **Exemplo Clássico:** Considere a relação entre `FUNCIONARIO` (entidade forte) e `DEPENDENTE` (entidade fraca).
    - A entidade `DEPENDENTE` não faz sentido sozinha; um dependente sempre existe _em função de_ um funcionário.
    - O atributo `nome` de um dependente ("Maria") não é único no sistema, pois vários funcionários podem ter dependentes com o mesmo nome.
    - Para identificar uma "Maria" específica, precisamos saber de qual funcionário ela é dependente. Portanto, a chave primária da entidade `DEPENDENTE` seria a combinação da chave estrangeira (`id_funcionario`) com o atributo discriminante (`nome`).
- **Outro Exemplo:** A entidade `AGÊNCIA` é fraca em relação à entidade `BANCO`. O número de uma agência (ex: "0123") só é único dentro do contexto de um banco específico.
- **Representação no DER:** Um retângulo com linha dupla.

#### Entidades Associativas

Uma **entidade associativa** é um construto especial usado para modelar um **relacionamento de muitos-para-muitos (N:M)** entre duas ou mais entidades. Ela surge quando o próprio relacionamento possui atributos próprios. Em vez de ser apenas uma linha de conexão, o relacionamento é "promovido" ao status de entidade.

- **Exemplo:** A relação entre `MÉDICO` e `PACIENTE`. Um médico atende muitos pacientes, e um paciente pode ser atendido por muitos médicos. Onde armazenar a `data` e a `hora` de uma consulta específica? Essa informação não pertence nem ao médico, nem ao paciente, mas sim ao evento da **consulta** que os une.
- Nesse caso, criamos uma entidade associativa chamada `CONSULTA`. Sua chave primária será a combinação das chaves primárias de `MÉDICO` (`id_medico`) e `PACIENTE` (`id_paciente`), e ela poderá ter seus próprios atributos, como `data_hora` e `diagnostico`.
- Retomaremos este conceito com mais detalhes ao falarmos sobre relacionamentos.
- **Representação no DER:** Um retângulo com um losango inscrito.

A tabela a seguir resume os três tipos de entidades:

| Entidade        | Descrição                                                                                                           | Representação                                                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Forte**       | Representa um objeto ou conceito que é independente no modelo, ou seja, não depende de outra entidade para existir. | <div align="center"><img width="160px" src="./img/02-entidade-forte.png"><div>       |
| **Fraca**       | Representa um objeto ou conceito que é dependente de uma entidade forte para existir.                               | <div align="center"><img width="160px" src="./img/02-entidade-fraca.png"><div>       |
| **Associativa** | Utilizada para representar relacionamentos complexos entre duas ou mais entidades fortes.                           | <div align="center"><img width="160px" src="./img/02-entidade-associativa.png"><div> |

É importante notar que a representação gráfica pode variar. Dependendo da notação adotada (como IDEF1X, por exemplo), uma entidade fraca pode ser representada por um retângulo com cantos arredondados. A notação que utilizamos aqui é a de Peter Chen, uma das mais comuns e didáticas.


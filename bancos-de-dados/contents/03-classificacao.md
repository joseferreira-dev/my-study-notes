# Capítulo 3 – Classificação dos Sistemas de Gerenciamento de Bancos de Dados (SGBDs)

Após compreendermos a evolução histórica dos modelos de dados e a importância dos Sistemas de Gerenciamento de Bancos de Dados (SGBDs) no desenvolvimento de aplicações modernas, é essencial estudarmos como esses sistemas podem ser classificados. A classificação dos SGBDs não apenas nos ajuda a entender suas características técnicas, mas também nos orienta na escolha da melhor solução para diferentes contextos e necessidades organizacionais.

Neste capítulo, abordaremos as principais formas de classificação dos SGBDs, incluindo critérios baseados em modelo de dados, número de usuários, arquitetura de distribuição, heterogeneidade e modelo de licenciamento.

## Classificação quanto ao Modelo de Dados

O modelo de dados é o critério mais clássico e fundamental na classificação dos SGBDs. Ele determina a forma como os dados são **organizados**, **armazenados** e **manipulados** internamente. A seguir, apresentamos os principais tipos de modelos de dados adotados por SGBDs ao longo da história, com exemplos de sistemas representativos.

### Modelo Hierárquico

O modelo hierárquico organiza os dados em uma estrutura de árvore, onde cada registro possui um único "pai", mas pode ter vários "filhos". Essa estrutura é eficiente para representar relacionamentos unidirecionais e hierárquicos, como organogramas ou classificações taxonômicas.

**Exemplos**: IMS (IBM), SYSTEM 2K, TDMS.

**Observação**: Estes SGBDs são considerados legados, sendo pouco utilizados atualmente, mas ainda mantidos em sistemas antigos de grande porte, especialmente em ambientes mainframe.

**Exemplo prático**: Suponha um banco de dados de uma empresa em que há departamentos, e cada departamento possui vários funcionários. A estrutura hierárquica se parece com uma árvore, onde o "departamento" é o nó pai e os "funcionários" são os nós filhos.

### Modelo em Rede

Este modelo generaliza o modelo hierárquico, permitindo que um registro tenha múltiplos pais. A estrutura em rede é representada por um grafo, e seus relacionamentos são mais flexíveis. Cada entidade pode estar ligada a várias outras entidades.

**Exemplos**: IDMS, DMS 1100, IMAGE, VAX-SGBD, SUPRA.

**Observação**: Também são considerados sistemas legados. Tiveram seu auge entre as décadas de 1970 e 1980.

**Exemplo prático**: Imagine um banco de dados de projetos em que um funcionário pode estar envolvido em vários projetos, e cada projeto pode ter múltiplos funcionários. Essa estrutura se adapta melhor ao modelo em rede do que ao hierárquico.

### Modelo Relacional

O modelo relacional revolucionou os bancos de dados ao representar as informações na forma de tabelas (ou "relações"). É o modelo mais amplamente utilizado na atualidade, especialmente em aplicações corporativas.

**Exemplos**: Oracle, SQL Server, PostgreSQL, MySQL, DB2, SQLite, Microsoft Access.

**Observação**: Utiliza-se a linguagem SQL (Structured Query Language) para a manipulação de dados.

**Exemplo prático**: Uma tabela chamada `Clientes` contém colunas como `ID`, `Nome`, `Email`. Outra tabela, `Pedidos`, contém colunas como `ID`, `Data`, `ClienteID`. A associação entre pedidos e clientes é feita por meio do campo `ClienteID`.

### Modelo Orientado a Objetos

Neste modelo, os conceitos da programação orientada a objetos são aplicados ao banco de dados. Objetos, herança, encapsulamento e polimorfismo são suportados nativamente.

**Exemplos**: Versant, ObjectStore, db4o, Matisse.

**Observação**: Embora seu uso seja menos disseminado que o modelo relacional, ele é vantajoso em aplicações com estruturas complexas de dados, como sistemas CAD/CAM e aplicações científicas.

**Exemplo prático**: Um objeto `Carro` pode conter propriedades como `modelo`, `ano` e métodos como `ligarMotor()`. Esse objeto pode ser armazenado diretamente no banco de dados com toda sua lógica embutida.

### Bancos de Dados NoSQL

O termo **NoSQL (Not Only SQL)** abrange uma variedade de modelos que se afastam do paradigma relacional. Abaixo, listamos os principais subtipos:

#### Bancos de Dados Orientados a Documentos

Armazenam dados no formato de documentos, geralmente em JSON ou BSON. Cada documento pode ter estrutura diferente, o que oferece alta flexibilidade.

**Exemplos**: MongoDB, CouchDB, Couchbase.

**Exemplo prático**: Um documento JSON com os dados de um cliente pode conter um campo `enderecos`, que é uma lista de múltiplos endereços aninhados dentro do mesmo registro.

#### Bancos de Dados Colunares

Organizam os dados por colunas ao invés de linhas, o que os torna altamente eficientes para consultas analíticas em grandes volumes de dados.

**Exemplos**: HBase, Accumulo.

**Exemplo prático**: Uma consulta que precisa ler apenas as colunas `idade` e `salário` em um banco com milhões de registros pode ser muito mais rápida em um banco colunar.

#### Bancos de Dados Chave-Valor

Armazenam os dados como pares chave-valor, sendo extremamente simples e rápidos.

**Exemplos**: Redis, Cassandra, DynamoDB, Memcached.

**Exemplo prático**: Em um sistema de cache, a chave pode ser o ID de uma sessão e o valor pode conter todas as preferências do usuário.

#### Bancos de Dados Orientados a Grafos

Especializados em armazenar e consultar dados com relações complexas, como redes sociais, cadeias logísticas ou mapas de rotas.

**Exemplos**: Neo4j, JanusGraph, Dgraph, Giraph, TigerGraph.

**Exemplo prático**: Em uma rede social, os nós representam pessoas e as arestas representam relações como "amigo", "segue", "trabalha com", permitindo consultas como: “Quem são os amigos dos amigos do João que moram em São Paulo?”

> **Nota**: Muitos SGBDs modernos combinam mais de um modelo. O MongoDB, por exemplo, pode ser utilizado como orientado a documentos, mas também permite operações de tipo chave-valor. Essas hibridizações são comuns no contexto atual. Para uma visão atualizada sobre os bancos mais usados e suas categorias, recomenda-se visitar o site [https://db-engines.com/en/ranking](https://db-engines.com/en/ranking).

## Classificação quanto ao número de usuários

Outra maneira de classificar os SGBDs é com base na quantidade de usuários simultâneos que o sistema suporta.

### SGBDs Monousuário

Projetados para uso por apenas uma pessoa por vez, geralmente em computadores pessoais. São simples e com poucos recursos de controle concorrente.

**Exemplo**: Microsoft Access (em sua forma mais básica).

**Exemplo prático**: Um pequeno comércio pode utilizar um sistema de vendas local em que apenas uma pessoa faz registros no banco de dados.

### SGBDs Multiusuário

Permitem que múltiplos usuários acessem e manipulem dados simultaneamente. Possuem mecanismos avançados de controle de concorrência, bloqueio e transações.

**Exemplos**: Oracle, PostgreSQL, MySQL, SQL Server.

**Exemplo prático**: Um sistema bancário em que milhares de clientes acessam seus dados ao mesmo tempo, por diferentes canais (caixas eletrônicos, aplicativos, internet banking).

## Classificação quanto à distribuição geográfica

A arquitetura de distribuição dos dados também é um critério essencial para classificar os SGBDs.

### SGBDs Centralizados

Todo o sistema — tanto o SGBD quanto os dados — está instalado em um único computador, mesmo que vários usuários possam acessá-lo via rede.

**Exemplo prático**: Uma empresa pequena com um único servidor que atende a todas as requisições internas.

### SGBDs Distribuídos (SGBDD)

Neste caso, o banco de dados e o software do SGBD estão distribuídos por vários computadores, interligados por uma rede. Essa abordagem aumenta a escalabilidade, tolerância a falhas e disponibilidade.

**Exemplo prático**: Um sistema global de comércio eletrônico com servidores na América, Europa e Ásia, cada um armazenando partes específicas do banco de dados.

#### SGBDs Distribuídos Homogêneos

Utilizam o mesmo SGBD em todos os locais, facilitando a integração e a consistência.

#### SGBDs Distribuídos Heterogêneos

Permitem o uso de diferentes SGBDs em cada local, exigindo maior complexidade de integração, mas oferecendo flexibilidade.

## Classificação quanto ao modelo de licenciamento

Finalmente, os SGBDs também podem ser classificados de acordo com seu modelo de licenciamento.

### SGBDs Proprietários (Pagos)

Exigem a compra de licença de uso. São mantidos por empresas e oferecem suporte técnico, atualizações e garantias.

**Exemplos**: Oracle, SQL Server, DB2.

**Exemplo prático**: Uma grande empresa contrata o Oracle por sua estabilidade, escalabilidade e suporte 24/7.

### SGBDs de Código Aberto (Gratuitos)

São gratuitos e com código-fonte aberto, permitindo customização e uso livre, embora o suporte técnico possa depender da comunidade ou ser contratado à parte.

**Exemplos**: PostgreSQL, MySQL, SQLite.

**Exemplo prático**: Uma startup opta por usar o PostgreSQL para reduzir custos e personalizar funcionalidades conforme sua aplicação cresce.

## Considerações Finais

A classificação dos SGBDs é fundamental para compreender sua estrutura interna, seu comportamento e suas aplicações práticas. Ao conhecer as diferentes categorias e suas implicações, profissionais da área de tecnologia da informação são capazes de tomar decisões mais acertadas quanto à escolha de tecnologias de armazenamento e manipulação de dados. Mais do que uma questão técnica, a escolha do SGBD impacta diretamente na performance, escalabilidade, custos e manutenção de qualquer sistema de informação.

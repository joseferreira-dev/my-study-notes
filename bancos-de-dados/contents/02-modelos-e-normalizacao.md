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


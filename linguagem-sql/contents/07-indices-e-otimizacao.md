# Capítulo 7 – Índices e Otimização de Consultas: Acelerando o Acesso aos Dados

Nos capítulos anteriores, construímos uma base sólida sobre como definir, manipular, proteger e garantir a integridade dos dados em um banco de dados relacional. Dominamos as linguagens que nos permitem assegurar a _correção_ das operações. Agora, nossa atenção se volta para um aspecto igualmente crucial em qualquer sistema de grande escala: a _eficiência_. Uma consulta que retorna o resultado correto, mas leva minutos ou horas para ser executada, tem pouco valor prático. O principal mecanismo que o SQL nos oferece para combater a lentidão e otimizar o acesso aos dados é o **Índice (Index)**.

Um índice, em um banco de dados, é uma estrutura de dados opcional e especializada, associada a uma tabela, cujo único propósito é acelerar a recuperação de registros. A analogia mais comum e precisa é com o índice remissivo de um livro: em vez de folhear o livro inteiro página por página (uma operação lenta chamada de **full table scan** no jargão de banco de dados) para encontrar um tópico, consulta-se o índice, que aponta diretamente para a página correta. De forma semelhante, um índice de banco de dados permite que o SGBD localize os dados desejados de forma rápida e direta, sem precisar varrer toda a tabela.

Este capítulo explora o mundo dos índices. Vamos desvendar como eles funcionam internamente, os diferentes tipos e classificações que existem, o fundamental custo-benefício de sua utilização e, por fim, os comandos DDL para criá-los e gerenciá-los. Compreender os índices é o primeiro e mais importante passo na jornada da otimização de desempenho em SQL.

## Como um Índice Funciona: A Estrutura de Árvore B

Para entender por que um índice é tão eficiente, é preciso ir além da analogia e conhecer sua estrutura interna. A grande maioria dos SGBDs relacionais implementa seus índices utilizando uma estrutura de dados chamada **Árvore B (B-Tree)**. Uma Árvore B é uma estrutura de árvore auto-balanceada, otimizada para sistemas que leem e escrevem grandes blocos de dados, como os bancos de dados em disco.

Uma Árvore B é composta por nós organizados hierarquicamente:

- **Nó Raiz (Root Node):** O ponto de entrada da árvore, o nó mais alto.
    
- **Nós Intermediários (Branch Nodes):** Nós que ficam entre a raiz e as folhas. Eles não contêm ponteiros para os dados da tabela, mas sim ponteiros para outros nós da árvore, guiando a busca com base em faixas de valores.
    
- **Nós Folha (Leaf Nodes):** A camada mais baixa da árvore. É aqui que a "mágica" acontece. Cada nó folha contém os valores da coluna indexada (a chave de pesquisa) e, para cada valor, um ponteiro (um endereço físico) que aponta para a localização exata da linha correspondente na tabela no disco.
    

Quando uma consulta com uma condição `WHERE` em uma coluna indexada é executada (ex: `WHERE id_empregado = 54321`), o SGBD não lê a tabela. Em vez disso, ele navega pela Árvore B do índice: começa na raiz, desce pelos nós intermediários seguindo as faixas de valores e, em pouquíssimos passos, chega ao nó folha correto. Lá, ele encontra o valor `54321` e seu ponteiro, usando-o para acessar diretamente o registro no disco. Este processo, envolvendo poucas operações de leitura de disco (I/O), é ordens de magnitude mais rápido do que uma varredura completa da tabela.

## Tipos e Classificações de Índices

Os índices podem ser classificados de diversas formas, com base em sua composição, estrutura e relação com os dados.

### Índice Simples vs. Composto

- **Índice Simples:** É um índice criado sobre uma única coluna de uma tabela. É útil para acelerar consultas que filtram frequentemente por essa coluna.
    
- **Índice Composto (ou Concatenado):** É um índice criado sobre duas ou mais colunas. A ordem das colunas na definição do índice é crucial. Um índice em `(cidade, estado)` será muito eficiente para buscas por cidade e estado, ou apenas por cidade, mas geralmente não ajudará em buscas que filtram apenas por estado.
    

### Índice Denso vs. Esparso

Esta classificação descreve a relação entre as entradas do índice e as linhas da tabela.

- **Índice Denso:** Contém uma entrada no índice para **cada linha** da tabela. A maioria dos índices não clusterizados (ver abaixo) são densos.
    
- **Índice Esparso:** Contém entradas apenas para **alguns** dos valores ou linhas, geralmente um por bloco de dados da tabela. São mais compactos, mas a busca pode exigir um passo adicional para encontrar a linha exata dentro do bloco.
    

### Índice Clusterizado vs. Não Clusterizado

Esta é uma das distinções mais importantes.

- **Índice Clusterizado (Clustered Index):** Este índice determina a **ordem física** em que os dados são armazenados no disco. As linhas da tabela são ordenadas de acordo com a chave do índice clusterizado. Por essa razão, uma tabela só pode ter **um** índice clusterizado. A chave primária de uma tabela é, por padrão, o candidato ideal e a implementação mais comum de um índice clusterizado.
    
- **Índice Não Clusterizado (Non-Clustered Index):** Neste caso, o índice é uma estrutura completamente separada da tabela. A ordem dos dados no índice é uma, e a ordem física dos dados na tabela é outra. O nó folha de um índice não clusterizado contém um ponteiro para a linha de dados. Uma tabela pode ter múltiplos índices não clusterizados.
    

## O Custo-Benefício de um Índice

A decisão de criar um índice envolve uma análise de custo-benefício. Eles não são uma solução mágica e podem, em alguns casos, prejudicar o desempenho.

- O Benefício: Aceleração de Consultas (SELECT)
    
    O principal benefício é uma melhora drástica no desempenho de consultas SELECT que utilizam colunas indexadas em cláusulas WHERE, JOIN e ORDER BY. Para tabelas grandes, a diferença entre usar um índice e fazer um full table scan pode ser de segundos versus horas.
    
- O Custo: Degradação de Operações de Escrita (INSERT, UPDATE, DELETE)
    
    Este é o contraponto. Um índice é uma estrutura de dados adicional que precisa ser mantida.
    
    - `INSERT`: Para cada nova linha inserida na tabela, uma nova entrada correspondente deve ser inserida no local correto de cada índice, o que pode exigir reorganizações na estrutura da Árvore B.
        
    - `DELETE`: Para cada linha removida, a entrada correspondente em cada índice também deve ser removida.
        
    - `UPDATE`: Se o valor de uma coluna indexada for atualizado, a posição da entrada no índice precisa ser alterada, o que é efetivamente uma operação de exclusão e inserção no índice.
        

Portanto, tabelas que sofrem um volume muito alto de operações de escrita (`INSERT`, `UPDATE`, `DELETE`) e poucas leituras podem ter o desempenho geral prejudicado por um excesso de índices.

## Gerenciando Índices em SQL

A criação e remoção de índices são operações de DDL.

### Criação e Remoção de Índices

A sintaxe padrão para criar um índice é simples:

SQL

```
CREATE INDEX nome_do_indice ON nome_da_tabela (coluna1, coluna2, ...);
```

**Exemplo:** Criar um índice na coluna `department_id` da tabela `employees` para acelerar buscas por departamento.

SQL

```
CREATE INDEX idx_emp_deptid ON employees (department_id);
```

Para remover um índice que não é mais necessário ou que está causando degradação de desempenho, utiliza-se o comando `DROP INDEX`.

SQL

```
DROP INDEX nome_do_indice;
```

### Índices e Unicidade: Uma Nota Histórica

No passado, era comum usar a sintaxe `CREATE UNIQUE INDEX` para impor que todos os valores em uma coluna fossem únicos. Embora essa sintaxe ainda possa existir em alguns SGBDs por razões de compatibilidade, ela foi largamente descontinuada como prática padrão. A abordagem moderna e correta é separar a regra lógica da implementação física. Para garantir a unicidade, deve-se usar uma restrição `UNIQUE` ou `PRIMARY KEY` na definição da tabela (`CREATE TABLE` ou `ALTER TABLE`). O SGBD, então, automaticamente criará um índice único por trás dos panos para garantir a aplicação eficiente dessa restrição.

## Considerações Finais

Neste capítulo, exploramos o principal mecanismo para otimização de desempenho em bancos de dados: os **índices**. Passamos da analogia do índice de um livro para a compreensão de sua estrutura interna mais comum, a Árvore B, que permite ao SGBD localizar dados com uma velocidade e eficiência impressionantes. Dissecamos os diferentes tipos de índices — simples e compostos, clusterizados e não clusterizados — e compreendemos o seu fundamental custo-benefício: a aceleração massiva em operações de leitura (`SELECT`) contra a sobrecarga de manutenção em operações de escrita (`INSERT`, `UPDATE`, `DELETE`).

Ao dominar os pilares da DDL, DML, TCL e agora dos Índices, completamos uma jornada abrangente pelos fundamentos do SQL. Estamos aptos não apenas a interagir com os dados, mas a fazê-lo de forma segura, íntegra e eficiente. Esta base sólida nos capacita a avançar para tópicos de SQL mais avançados e especializados, que constroem sobre esses fundamentos, como as poderosas **funções de janela (window functions)**, as **expressões de tabela comuns (CTEs)** e as **extensões procedurais** (`PL/SQL`, `T-SQL`), que transformam o banco de dados em uma robusta plataforma para o desenvolvimento de aplicações complexas.
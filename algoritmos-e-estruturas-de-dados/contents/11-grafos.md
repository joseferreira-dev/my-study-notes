# Capítulo 11 – Grafos

Nos capítulos anteriores, progredimos das estruturas de dados lineares (Vetores, Listas, Pilhas, Filas) para as estruturas hierárquicas (Árvores). As árvores são um modelo poderoso para representar relações de subordinação (pai-filho), mas falham em modelar redes mais complexas, onde as conexões não são hierárquicas. Para resolver isso, chegamos à estrutura de dados mais genérica e expressiva de todas: os **Grafos**.

**Grafos** são estruturas de dados que representam relações ou conexões entre objetos. Eles são a ferramenta fundamental para modelar qualquer sistema de rede, desde redes sociais (amigos conectados a amigos), redes de computadores (roteadores conectados por cabos), mapas (cidades conectadas por estradas), até sistemas biológicos (proteínas interagindo). Um grafo é composto por um conjunto de **vértices (ou nós)** e **arestas (ou linhas)** que conectam pares de vértices.

Formalmente, um grafo é representado por um par $G = (V, E)$, onde $V$ é um conjunto de vértices e $E$ é um conjunto de arestas (conexões). Este capítulo explorará a terminologia, as formas de representação e os algoritmos fundamentais que operam sobre esta estrutura incrivelmente versátil.

## Terminologias Fundamentais

Para discutir grafos, precisamos de um vocabulário comum. A imagem a seguir apresenta três grafos (A, B, C) que usaremos para ilustrar os conceitos-chave.

<div align="center">
<img width="700px" src="./img/11-graph-01.png">
</div>

- **Vértice (ou Nó):** É o elemento base da estrutura, o "objeto" que queremos conectar. Assim como os nós das árvores, eles armazenam dados. Nos grafos (A) e (B), os vértices são as letras `A`, `B`, `C`, etc. No grafo (C), são os nomes das cidades.
- **Aresta (ou Arco):** É a conexão entre dois vértices. Nas árvores, as arestas são coadjuvantes que definem a hierarquia; em grafos, elas são protagonistas, podendo ter direção e valor.
    - **Peso:** Um valor associado a uma aresta, representando um "custo". No grafo (C), os pesos são as distâncias entre as cidades.
    - **Laço (Loop):** Uma aresta que conecta um vértice a ele mesmo, como visto no vértice `D` do grafo (B). Isso só é comum em grafos direcionados.
- **Vértice Adjacente (Vizinho):** Um vértice é adjacente a outro se houver uma aresta direta conectando-os. No grafo (A), os adjacentes de `E` são `A` e `F`. No grafo (B), os adjacentes de `A` são `B` e `C` (mas `A` _não_ é adjacente de `B` e `C`).
- **Grau (Valência) de um Vértice:** A quantidade de arestas conectadas a um vértice.
    - Em grafos **não direcionados** (como A), o grau de `A` é 3.
    - Em grafos **direcionados** (como B), diferenciamos:
        - **Grau de Saída:** Arestas que _saem_ do vértice. O grau de saída de `A` é 2.
        - **Grau de Entrada:** Arestas que _chegam_ ao vértice. O grau de entrada de `A` é 1.
- **Caminho (Trajeto):** Uma sequência de arestas que se percorre para ir de um vértice a outro. A Teoria dos Grafos foca intensamente em encontrar caminhos com propriedades especiais (como o mais curto).

## Tipos de Grafos

Os grafos são classificados com base nas propriedades de suas arestas e vértices. A imagem a seguir (que é a mesma da seção anterior) ilustra vários desses tipos.

<div align="center">
<img width="700px" src="./img/11-graph-02.png">
</div>

- **Grafo Não Direcionado (A, C):** As arestas não possuem um sentido (direção). A aresta `(A, B)` é a mesma que `(B, A)`. Modelam relações simétricas, como amizade no Facebook.
- **Grafo Direcionado (Dígrafo) (B):** As arestas possuem um sentido, indicado por setas. A aresta `(A, B)` é diferente de `(B, A)` (que pode ou não existir). Modelam relações assimétricas, como "seguir" alguém no Twitter ou o fluxo de ruas de mão única.
- **Grafo Ponderado (C):** As arestas possuem pesos (custos) associados a elas. Essencial para problemas de otimização, como encontrar a rota mais curta em um mapa.
- **Grafo Não Ponderado (A, B):** As arestas servem apenas para indicar conexão, sem custo.
- **Grafo Simples (A, C):** Um grafo não direcionado, sem laços e sem arestas paralelas (múltiplas arestas entre os mesmos dois vértices). O grafo (B) não é simples por ser direcionado e ter um laço.
- **Grafo Conexo (A, C):** Um grafo onde existe um caminho de qualquer vértice para qualquer outro vértice. O grafo (B) é **desconexo**, pois do vértice `C` não se consegue alcançar `A`.
- **Grafo Cíclico (A, B, C):** Um grafo que contém pelo menos um ciclo, ou seja, um caminho que começa e termina no mesmo vértice sem repetir arestas. `A-B-E-D-C-A` é um ciclo em (A). `D-D` é um ciclo em (B).
- **Grafo Acíclico (DAG):** Um grafo que não possui ciclos. Um **Grafo Direcionado Acíclico (DAG)** é uma estrutura muito importante, usada para modelar dependências (ex: `B` deve ser feito antes de `A`).
- **Grafo Completo (C):** Um grafo onde cada vértice está conectado a _todos_ os outros vértices.
- **Grafo Regular (A, C):** Um grafo onde _todos_ os vértices têm o mesmo grau. (Em (A), todos têm grau 2; em (C), todos têm grau 4). Note que todo grafo completo é também regular.
- **Árvore:** Uma árvore é um tipo especial de grafo: é um grafo conexo, acíclico e não direcionado.

## Representações Computacionais de Grafos

A representação visual de um grafo é intuitiva, mas para que um computador possa processá-lo, precisamos de uma representação de dados na memória. As três formas mais comuns são:

### Matriz de Adjacência

Essa abordagem usa uma matriz $N \times N$, onde $N$ é o número de vértices. As linhas e colunas representam os vértices. Uma marcação `1` em `Matriz[i][j]` significa que existe uma aresta do vértice `i` para o `j`. Se não houver, marca-se com `0`.

<div align="center">
<img width="550px" src="./img/11-graph-04.png">
</div>

- **Vantagens:** Verificar se uma aresta `(A, B)` existe é uma operação $O(1)$ (acesso direto à matriz).
- **Desvantagens:** O custo de espaço é sempre $O(N^2)$ (ou $O(V^2)$), mesmo que o grafo tenha poucas arestas (esparso). Encontrar todos os vizinhos de um vértice exige varrer sua linha inteira ($O(N)$).

### Lista de Adjacência

Essa representação utiliza um vetor (ou mapa) onde cada posição corresponde a um vértice. Cada vértice, por sua vez, aponta para uma lista (geralmente uma lista encadeada) contendo todos os seus vizinhos (vértices adjacentes).

<div align="center">
<img width="550px" src="./img/11-graph-05.png">
</div>

A imagem acima mostra uma lista de adjacência para o grafo não direcionado `A-B-C-E-D`. Note que `A` está na lista de `B`, e `B` está na lista de `A`.

A imagem a seguir ilustra a diferença entre a representação de um grafo não direcionado (topo) e um direcionado (base). No grafo direcionado, a lista do vértice `1` contém `2` e `4`, mas a lista do vértice `2` _não_ contém `1`.

<div align="center">
<img width="600px" src="11-graph-06.png">
</div>

- **Vantagens:** Custo de espaço é $O(V + E)$ (proporcional ao número de vértices e arestas). Extremamente eficiente para grafos esparsos (poucas arestas). Encontrar todos os vizinhos de um vértice é $O(Grau(V))$.
- **Desvantagens:** Verificar se uma aresta `(A, B)` existe é $O(Grau(A))$, pois é preciso percorrer a lista de `A`.

### Matriz de Incidência

Esta representação utiliza uma matriz $V \times E$, onde as linhas são os vértices e as colunas são as arestas. A matriz armazena a relação entre vértices e arestas. Um padrão comum para grafos direcionados e ponderados é:

- `-1`: O vértice é a _origem_ da aresta.
- `1`: O vértice é o _destino_ da aresta.
- `0`: O vértice não tem relação com a aresta.

<div align="center">
<img width="600px" src="./img/11-graph-03.png">
</div>

- **Vantagens:** Útil em alguns problemas específicos onde a relação vértice-aresta é mais importante que vértice-vértice.
- **Desvantagens:** Custo de espaço $O(V \times E)$, que é frequentemente o mais ineficiente.

### Comparativo das Representações

A escolha da representação correta depende da densidade do grafo (denso vs. esparso) e das operações mais frequentes.

|**Operação**|**Matriz de Adjacência (V×V)**|**Lista de Adjacência (V+E)**|**Matriz de Incidência (V×E)**|
|---|---|---|---|
|**Espaço**|$O(V^2)$|$O(V + E)$|$O(V \times E)$|
|**Adicionar Vértice**|$O(V^2)$ (reconstruir)|$O(1)$|$O(V \times E)$ (reconstruir)|
|**Adicionar Aresta**|$O(1)$|$O(1)$*|$O(V)$ (adicionar coluna)|
|**Remover Vértice**|$O(V^2)$ (reconstruir)|$O(E)$ (atualizar listas)|$O(V \times E)$ (reconstruir)|
|**Remover Aresta**|$O(1)$|$O(E)$ ou $O(Grau(V))$|$O(V)$ (remover coluna)|
|**Verificar Adjacência (A, B)?**|$O(1)$|$O(Grau(A))$|$O(E)$|
|**Listar Vizinhos de A**|$O(V)$|$O(Grau(A))$|$O(E)$|

\*(Assumindo que $O(1)$ é para adicionar ao início da lista encadeada).

**Conclusão:** Para **grafos densos** (muitas arestas), a **Matriz de Adjacência** é boa pela verificação $O(1)$. Para **grafos esparsos** (poucas arestas), a **Lista de Adjacência** é quase sempre a melhor escolha, sendo a mais usada na prática.

## Algoritmos de Busca

Assim como nas árvores, "percorrer" um grafo (visitar todos os seus nós) é uma operação fundamental. As operações básicas `ADD` (adicionar vértice), `JOIN` (adicionar aresta), `REMOVE` (remover vértice) e `ADJACENT` (identificar vizinhos) são os blocos de construção para algoritmos de travessia mais complexos. As duas principais formas são a Busca em Profundidade (DFS) e a Busca em Largura (BFS).

### Busca em Profundidade (DFS - Depth-First Search)

A DFS explora o grafo "mergulhando" o mais fundo possível em um caminho antes de retroceder (fazer _backtracking_). Ela escolhe um vértice, visita-o, e então escolhe um vizinho _não visitado_ e repete o processo recursivamente. Quando chega a um "beco sem saída" (um nó sem vizinhos não visitados), ela retorna para o nó anterior e tenta outro caminho.

Esta abordagem usa uma **Pilha** (implícita, via recursão) para gerenciar os nós. É mais simples de implementar, mas menos eficiente se o objetivo estiver próximo ao início.

**Pseudocódigo (Recursivo):**

```
conjunto visitados

procedimento DFS(grafo, no_atual)
    visitados.adicionar(no_atual)
    processar(no_atual) // Visita o nó
    
    para cada vizinho de no_atual
        se vizinho não está em visitados
            DFS(grafo, vizinho) // Mergulha recursivamente
        fim_se
    fim_para
fim_procedimento
```

- **Complexidade:** $O(V + E)$ (cada vértice e aresta são visitados uma vez).

### Busca em Largura (BFS - Breadth-First Search)

A BFS explora o grafo em "camadas" ou "níveis". Ela começa em um vértice, visita _todos_ os seus vizinhos diretos, depois _todos_ os vizinhos dos vizinhos, e assim por diante.

Esta abordagem usa uma **Fila** (FIFO) para gerenciar os nós a visitar. Sua principal propriedade é que ela **garante encontrar o caminho mais curto** (em número de arestas) entre o início e qualquer outro nó. Embora mais difícil de implementar que a DFS, é ligeiramente mais performática por evitar retrocessos desnecessários.

**Pseudocódigo (Iterativo):**

```
procedimento BFS(grafo, no_inicial)
    fila = nova Fila()
    conjunto visitados
    
    fila.enfileirar(no_inicial)
    visitados.adicionar(no_inicial)
    
    enquanto fila não está vazia
        no_atual = fila.desenfileirar()
        processar(no_atual) // Visita o nó
        
        para cada vizinho de no_atual
            se vizinho não está em visitados
                visitados.adicionar(vizinho)
                fila.enfileirar(vizinho)
            fim_se
        fim_para
    fim_enquanto
fim_procedimento
```

- **Complexidade:** $O(V + E)$ (assim como a DFS).

## Problemas Clássicos e Aplicações

Grafos são poderosos porque modelam problemas do mundo real.

### Redes Sociais e Máquinas de Estado

- **Redes Sociais:** Como na imagem abaixo, redes sociais são grafos onde vértices são pessoas e arestas são conexões. Perguntas como "sugestões de amizade" são respondidas procurando vizinhos de vizinhos.

<div align="center">
<img width="500px" src="./img/11-graph-07.png">
</div>

- **Máquinas de Estado:** Muitos processos podem ser modelados como grafos direcionados, onde os vértices são "estados" e as arestas são "transições". O fluxo de uma compra online é um exemplo clássico.

<div align="center">
<img width="600px" src="./img/11-graph-08.png">
</div>

### Rotas e Caminho Mínimo (Google Maps)

O Google Maps é um exemplo perfeito de grafo ponderado. Cidades e esquinas são vértices; ruas são arestas; distância ou tempo de trânsito são os pesos.

- **Problema do Caminho Mínimo:** Encontrar o caminho de menor custo (soma de pesos) entre uma origem e um destino.
- **Algoritmo de Dijkstra:** Criado por Edsger Dijkstra, é o algoritmo clássico para resolver o problema do caminho mínimo em grafos com pesos **não negativos**. Ele funciona de forma "gulosa", visitando sempre o nó mais próximo (menor peso acumulado) ainda não visitado.

### Problema do Caixeiro Viajante (TSP)

Este problema clássico busca determinar a menor rota possível que visita _todas_ as cidades (vértices) de um conjunto exatamente uma vez, retornando à origem. É um problema de otimização logística fundamental.

- **Algoritmo do Vizinho Mais Próximo:** Uma heurística "gulosa" (e simples) para _aproximar_ uma solução. Começa em um vértice aleatório e sempre se move para o vizinho não visitado de menor peso (mais próximo). Este método é rápido, mas **não garante a melhor rota** (a rota ótima), pois uma escolha gulosa no início pode levar a um caminho final muito ruim.

### Teorema das Quatro Cores

Um famoso problema da Teoria dos Grafos que afirma que qualquer mapa pode ser colorido com apenas quatro cores, de forma que nenhuma região adjacente (vizinha) tenha a mesma cor.

## Considerações Finais

Os grafos são a estrutura de dados mais expressiva, capaz de modelar desde hierarquias simples (árvores) até redes interconectadas complexas. Por permitirem modelar relações e resolver problemas sofisticados de logística, redes e otimização, eles são fundamentais em quase todos os domínios da computação.

A escolha da representação correta (Matriz de Adjacência para grafos densos, Lista de Adjacência para grafos esparsos) é o primeiro passo crítico. A partir daí, o domínio de algoritmos como BFS (para caminhos mais curtos em passos) e DFS (para travessia e detecção de ciclos), bem como algoritmos clássicos como Dijkstra, permite o desenvolvimento de soluções robustas e eficientes para problemas do mundo real.
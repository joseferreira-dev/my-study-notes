<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Algoritmos de Busca

- [Introdução](#introdução)
- [Busca Sequencial (Linear Search)](#busca-sequencial-linear-search)
- [Busca Binária](#busca-binária)
- [Busca em Profundidade (DFS)](#busca-em-profundidade-dfs)
- [Busca em Largura (BFS)](#busca-em-largura-bfs)
- [Busca A*](#busca-a)
- [Busca em Tabelas Hash](#busca-em-tabelas-hash)

## Introdução

Os algoritmos de busca são métodos utilizados em ciência da computação para encontrar um item específico ou um conjunto de itens dentro de uma estrutura de dados. Existem diferentes tipos de algoritmos de busca, cada um adequado para diferentes tipos de problemas e estruturas de dados. Eles são aplicados em diversas áreas, como recuperação de informações, inteligência artificial, e até mesmo em operações cotidianas de sistemas operacionais e bancos de dados.

A escolha do algoritmo de busca adequado depende do contexto e das características da estrutura de dados. A busca linear é ideal para pequenas listas não ordenadas, enquanto a busca binária é mais adequada para listas ordenadas. Em estruturas hierárquicas, as buscas em árvores oferecem eficiência, especialmente quando balanceadas. Já em grafos, BFS e DFS são escolhidos com base nos requisitos específicos do problema, como a necessidade de encontrar o caminho mais curto ou a utilização eficiente de memória. Entender as vantagens, desvantagens e a complexidade de cada algoritmo permite tomar decisões informadas sobre qual utilizar em diferentes situações, maximizando a eficiência e a eficácia da busca de dados.

## Busca Sequencial (Linear Search)

Busca Sequencial, também conhecida como busca linear, é o algoritmo de busca mais simples. Ele percorre a estrutura de dados (geralmente um array ou lista) de forma sequencial, comparando cada elemento com o item de busca até encontrá-lo ou até que todos os elementos tenham sido verificados.

As principais vantagens dessa algoritmo é sua facilidade de implementação e o fato de que ele pode ser usado em listas desordenadas. Todavia, pode ser lento para listas longas, pois verifica cada elemento individualmente.

Complexidade de tempo: 
- **Melhor Caso**: `O(1)` - O elemento está na primeira posição.
- **Pior Caso**: `O(n)` - O elemento está na última posição ou não está presente.
- **Caso Médio**: `O(n/2)` - Em média, o elemento será encontrado na metade da lista, mas isso ainda é `O(n)`.

## Busca Binária

A busca binária é uma técnica eficiente para encontrar um elemento em uma lista ordenada. Ela funciona dividindo repetidamente a lista ao meio e comparando o elemento do meio com o alvo. Dependendo da comparação, a busca continua na metade esquerda ou direita.

Ele é muito mais rápida que a busca linear para listas grandes. Contudo, para ser aplicada os dados precisam estar ordenados e sua implementação é um pouco mais complexa de implementar corretamente, especialmente para listas muito grandes ou em estruturas de dados complexas.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - O elemento está no meio da lista.
- **Pior Caso**: `O(log n)` - O elemento não está presente ou está em uma extremidade.
- **Caso Médio**: `O(log n)`.

## Busca em Profundidade (DFS)

A busca em profundidade (Depth-First Search) é uma técnica de busca usada em árvores e grafos. Ela começa em um nó raiz e explora o máximo possível ao longo de cada ramo antes de retroceder.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - O elemento é encontrado no primeiro nó visitado.
- **Pior Caso**: `O(V + E)` - Onde `V` é o número de vértices e `E` é o número de arestas.

## Busca em Largura (BFS)

A busca em largura (Breadth-First Search) é outra técnica de busca usada em árvores e grafos. Ela começa no nó raiz e explora todos os nós vizinhos no nível atual antes de mover para o próximo nível.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - O elemento é encontrado no nó raiz.
- **Pior Caso**: `O(V + E)` - Onde `V` é o número de vértices e `E` é o número de arestas.

## Busca A*

A busca A* é um algoritmo de busca heurística usado principalmente em grafos para encontrar o caminho mais curto entre dois nós. Ele usa uma função heurística para estimar o custo do caminho do nó atual ao objetivo.

Complexidade de tempo:
- Depende da heurística usada. Pode ser exponencial no pior caso.

## Busca em Tabelas Hash

A busca em tabelas hash usa uma função hash para mapear chaves para índices em uma tabela. Essa técnica permite buscas extremamente rápidas, desde que a função hash distribua bem as chaves.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - A chave é mapeada diretamente para o índice correto.
- **Pior Caso**: `O(n)` - Todas as chaves colidem em um único índice.

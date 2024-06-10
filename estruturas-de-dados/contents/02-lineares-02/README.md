<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/estruturas-de-dados"><img src="../../banner-ed.png"></a>
</div>
<br>

# Estruturas Lineares - Parte 2: Listas

- [Introdução](#introdução)
- [Listas Encadeadas Simples](#listas-encadeadas-simples)
- [Listas Duplamente Encadeadas](#listas-duplamente-encadeadas)
- [Listas Circulares](#listas-circulares)
- [Listas Circulares Duplamente Encadeadas](#listas-circulares-duplamente-encadeadas)

## Introdução

As listas encadeadas são uma família de estruturas de dados dinâmicas que facilitam a inserção e remoção de elementos. Elas são compostas por uma série de nós, onde cada nó contém um valor e uma referência (ou ponteiro) para o próximo nó na sequência. Vamos explorar as variações mais comuns: listas encadeadas simples, listas duplamente encadeadas, listas circulares e listas circulares duplamente encadeadas.

## Listas Encadeadas Simples

As listas encadeadas simples, ou listas simplesmente ligadas (Singly Linked Lists), são estruturas de dados compostas por uma sequência de elementos chamados nós. Cada nó contém dois componentes: um valor ou dado e um ponteiro (ou referência) para o próximo nó na sequência. O primeiro nó é denominado cabeça (head) e o último nó aponta para `null`, indicando o fim da lista. A lista ligada inteira é acessada a partir de um ponteiro externo que aponta para o primeiro nó na lista. Por ponteiro externo, entendemos que este não está incluído dentro de um nó. Em vez disso, seu valor pode ser acessado diretamente, por referência a uma variável.

<div align="center">
  <img src="01-lista-simples.png">
</div>

As operações básicas em listas encadeadas incluem inserção, remoção e busca. Inserir um novo nó no início da lista é uma operação rápida, de tempo constante `O(1)`, pois basta ajustar o ponteiro da cabeça para o novo nó e o ponteiro do novo nó para o antigo primeiro nó. Inserções no meio ou no final da lista são mais custosas, pois requerem uma travessia da lista para encontrar a posição desejada, resultando em um tempo médio de `O(n)`.

Remover um nó da lista também pode variar em custo. Remover o primeiro nó é uma operação `O(1)`, enquanto remover um nó no meio ou no final da lista requer uma busca prévia, custando `O(n)` em média. A busca por um valor específico na lista também é `O(n)`, já que pode ser necessário percorrer todos os nós.

## Listas Duplamente Encadeadas

As listas duplamente encadeadas (Doubly Linked List) expandem o conceito de listas encadeadas ao adicionar um segundo ponteiro em cada nó, que aponta para o nó anterior. Assim, cada nó contém três componentes: o valor, um ponteiro para o próximo nó e um ponteiro para o nó anterior.

<div align="center">
  <img src="02-lista-dupla.png">
</div>

Essa estrutura proporciona maior flexibilidade na navegação, permitindo percorrer a lista tanto para frente quanto para trás. A inserção e remoção de nós em uma lista duplamente encadeada são mais eficientes em algumas situações, pois não é necessário percorrer a lista a partir da cabeça para acessar o nó anterior.

Inserir um nó em qualquer posição da lista, seja no início, no meio ou no final, tem um custo `O(1)` quando a posição é conhecida, pois apenas os ponteiros dos nós adjacentes precisam ser atualizados. Remover um nó também é `O(1)` se o nó a ser removido é conhecido, pois novamente só os ponteiros precisam ser ajustados, mas a busca permanece `O(n)`. No entanto, a navegação bidirecional pode facilitar certas operações, como a reversão da lista.

## Listas Circulares

Uma lista circular (Circular Linked List) é uma variação da lista encadeada, onde o último nó aponta de volta para o primeiro nó, formando um ciclo. Isso elimina a necessidade de um ponteiro null para indicar o fim da lista. Uma vantagem das listas circulares é que elas podem ser percorridas indefinidamente em loop, o que é útil em algumas aplicações como buffers circulares e listas de tarefas que precisam ser repetidas.

<div align="center">
  <img src="03-lista-circular.png">
</div>

As operações básicas em uma lista circular, como inserção, remoção e busca, têm complexidade semelhante às listas encadeadas simples, com o custo adicional de ajustar os ponteiros para manter a estrutura circular. O custo é `O(1)` para inserção e remoção se a posição é conhecida, e `O(n)` para busca no pior caso, pois pode ser necessário percorrer todos os nós. A principal diferença é que a lista não possui um ponto final natural, o que pode simplificar algumas operações cíclicas.

## Listas Circulares Duplamente Encadeadas

Uma lista circular duplamente encadeada (Doubly Circular Linked List) combina as características das listas duplamente encadeadas e das listas circulares. Cada nó possui dois ponteiros, um para o próximo nó e outro para o nó anterior, e o último nó aponta para o primeiro nó, enquanto o primeiro nó aponta para o último, formando um ciclo bidirecional.

<div align="center">
  <img src="04-lista-circular-dupla.png">
</div>

Essa estrutura é altamente flexível, permitindo inserções e remoções eficientes em qualquer posição (`O(1)` se a posição é conhecida) e navegação bidirecional contínua. Como nas outras variações, a busca por um elemento específico ainda é uma operação `O(n)`.

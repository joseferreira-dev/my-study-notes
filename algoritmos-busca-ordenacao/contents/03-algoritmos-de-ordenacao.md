# Capítulo 3 – Bubble Sort, Insertion Sort, Selection Sort

**Bubble Sort**, **Insertion Sort** e **Selection Sort** são algoritmos de ordenação simples e intuitivos, frequentemente ensinados como introdução aos conceitos de algoritmos de ordenação devido à sua simplicidade. **Bubble Sort** funciona repetidamente trocando elementos adjacentes se estiverem na ordem errada, fazendo com que os maiores elementos "flutuem" para o topo da lista. **Insertion Sort** constrói a lista ordenada um elemento por vez, inserindo cada novo elemento em sua posição correta em relação aos elementos já ordenados. **Selection Sort** percorre a lista repetidamente, selecionando o menor (ou maior) elemento e movendo-o para sua posição final. Embora sejam fáceis de entender e implementar, esses algoritmos são ineficientes para listas grandes, pois possuem complexidade de tempo `O(n²)` no pior caso.

## Bubble Sort

O algoritmo de ordenação Bubble Sort é um dos algoritmos de ordenação mais simples e intuitivos. Ele funciona comparando repetidamente pares de elementos adjacentes em uma lista e trocando-os se estiverem na ordem errada. O processo é repetido até que a lista esteja ordenada. Este algoritmo não é adequado para grandes conjuntos de dados, pois sua complexidade de tempo média e de pior caso é bastante alta.

O passo a passo para a sua realização é o seguinte:

1. **Inicialização**
   - Começa do primeiro elemento da lista.
2. **Comparação e Troca**
   - Compara o primeiro elemento com o segundo.
   - Se o primeiro elemento é maior que o segundo, troca-os
   - Passa para o próximo par (segundo e terceiro elementos), e repete o processo até o final da lista.
3. **Iteração**
   - Após uma passagem completa pela lista, o maior elemento "bubblou" para o final da lista.
   - Repete o processo para o restante da lista, excluindo o último elemento (já ordenado).
4. **Terminação**
   - O algoritmo termina quando nenhuma troca é necessária durante uma passagem completa pela lista.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Bubble Sort:

1. **Primeira Passagem**:
   - Compara `5` e `3`: `5` é maior, então troca. Lista: `[3, 5, 8, 4, 2]`
   - Compara `5` e `8`: não troca. Lista: `[3, 5, 8, 4, 2]`
   - Compara `8` e `4`: `8` é maior, então troca. Lista: `[3, 5, 4, 8, 2]`
   - Compara `8` e `2`: `8` é maior, então troca. Lista: `[3, 5, 4, 2, 8]`
   - Maior elemento `8` "bubblou" para o final.
2. **Segunda Passagem**:
   - Compara `3` e `5`: não troca. Lista: `[3, 5, 4, 2, 8]`
   - Compara `5` e `4`: `5` é maior, então troca. Lista: `[3, 4, 5, 2, 8]`
   - Compara `5` e `2`: `5` é maior, então troca. Lista: `[3, 4, 2, 5, 8]`
   - Elemento `5` está agora na posição correta.
3. **Terceira Passagem**:
   - Compara `3` e `4`: não troca. Lista: `[3, 4, 2, 5, 8]`
   - Compara `4` e `2`: `4` é maior, então troca. Lista: `[3, 2, 4, 5, 8]`
   - Elemento `4` está agora na posição correta.
4. **Quarta Passagem**:
   - Compara `3` e `2`: `3` é maior, então troca. Lista: `[2, 3, 4, 5, 8]`
   - Elemento `3` está agora na posição correta.
5. **Quinta Passagem**:
   - Nenhuma troca é necessária, lista já está ordenada.

Algumas das principais características do Bubble Sort são:

- **Complexidade de Tempo**: O pior caso e o caso médio são `O(n²)`, onde `n` é o número de elementos na lista. No melhor caso, quando a lista já está ordenada, a complexidade é `O(n)`.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place, não requer espaço adicional significativo.
- **Estabilidade**: É um algoritmo estável, ou seja, não altera a ordem relativa de elementos iguais.

O Bubble Sort é mais útil para listas pequenas ou quando a lista está quase ordenada. É um algoritmo muito lento para grandes conjuntos de dados. Além disso, por ser um algoritmo de classificação baseado em comparação, ele requer um operador de comparação para determinar a ordem relativa dos elementos no conjunto de dados de entrada. Isso pode limitar a eficiência do algoritmo em certos casos.

## Insertion Sort

O algoritmo de ordenação Insertion Sort é um método simples e intuitivo para ordenar uma lista que funciona inserindo iterativamente cada elemento de uma lista não classificada em sua posição correta em uma parte classificada da lista. É um algoritmo de classificação estável, o que significa que elementos com valores iguais mantêm sua ordem relativa na saída classificada. Ele é eficiente para listas pequenas ou quase ordenadas. A ideia básica do Insertion Sort é construir a lista ordenada elemento por elemento, inserindo cada novo elemento na posição correta, como classificar as cartas de baralho em suas mãos. Você divide as cartas em dois grupos: as cartas classificadas e as cartas não classificadas. Em seguida, você escolhe uma carta do grupo não classificado e a coloca no lugar certo no grupo classificado.

O passo a passo para a sua realização é o seguinte:

1. **Inicialização**
   - Começa com o segundo elemento da lista (assumindo que o primeiro elemento está na posição correta, pois uma lista com um único elemento já está ordenada).
2. **Inserção**
   - Seleciona o elemento corrente e o compara com os elementos à esquerda até encontrar a posição correta para inseri-lo.
   - Move todos os elementos maiores que o elemento corrente uma posição à direita para abrir espaço para a inserção.
3. **Iteração**
   - Repete o processo para cada elemento da lista até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Insertion Sort:

1. **Inicialização**:
   - Elemento corrente: `3` (segundo elemento)
2. **Primeira Passagem**:
   - Compara `3` com `5`: `3` é menor
   - Move `5` uma posição à direita. Lista: `[5, 5, 8, 4, 2]`
   - Insere `3` na posição correta. Lista: `[3, 5, 8, 4, 2]`
3. **Segunda Passagem**:
   - Elemento corrente: `8` (terceiro elemento)
   - Compara `8` com `5`: `8` é maior, não precisa mover. Lista: `[3, 5, 8, 4, 2]`
4. **Terceira Passagem**:
   - Elemento corrente: `4` (quarto elemento)
   - Compara `4` com `8`: `4` é menor
   - Move `8` uma posição à direita. Lista: `[3, 5, 8, 8, 2]`
   - Compara `4` com `5`: `4` é menor
   - Move `5` uma posição à direita. Lista: `[3, 5, 5, 8, 2]`
   - Insere `4` na posição correta. Lista: `[3, 4, 5, 8, 2]`
5. **Quarta Passagem**:
   - Elemento corrente: `2` (quinto elemento)
   - Compara `2` com `8`: `2` é menor
   - Move 8 uma posição à direita. Lista: `[3, 4, 5, 8, 8]`
   - Compara `2` com `5`: `2` é menor
   - Move 5 uma posição à direita. Lista: `[3, 4, 5, 5, 8]`
   - Compara `2` com `4`: `2` é menor
   - Move `4` uma posição à direita. Lista: `[3, 4, 4, 5, 8]`
   - Compara `2` com `3`: `2` é menor
   - Move `3` uma posição à direita. Lista: `[3, 3, 4, 5, 8]`
   - Insere `2` na posição correta. Lista: `[2, 3, 4, 5, 8]`

Algumas das principais características do Insertion Sort são:

- **Complexidade de Tempo**: O pior caso e o caso médio são `O(n²)`, onde `n` é o número de elementos na lista. No melhor caso, quando a lista já está ordenada, a complexidade é `O(n)`.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place, não requer espaço adicional significativo.
- **Estabilidade**: É um algoritmo estável, ou seja, não altera a ordem relativa de elementos iguais.

O Insertion Sort é frequentemente utilizado em situações onde o número de elementos é pequeno ou como parte de algoritmos mais complexos (por exemplo, o algoritmo de ordenação híbrido Timsort usa o Insertion Sort para pequenas partições). É um algoritmo lento para grandes conjuntos de dados e apresenta um desempenho significativamente melhor que o Bubble Sort, em termos absolutos.

## Selection Sort

O algoritmo de ordenação Selection Sort é um método simples e intuitivo para ordenar uma lista. Ele funciona dividindo a lista em duas partes: a parte ordenada, que é construída do início ao fim da lista, e a parte não ordenada, que contém os elementos restantes. Em cada iteração, o Selection Sort encontra o menor (ou maior, dependendo da ordem desejada) elemento da parte não ordenada e o troca com o primeiro elemento da parte não ordenada, começando a formar a parte ordenada. É um algoritmo cuja versão padrão de implementação é instável, mas também pode ser estável, sendo esta implementação menos usual.

O passo a passo para a sua realização é o seguinte:

1. **Inicialização**
   - Divide a lista em duas partes: a parte ordenada (inicialmente vazia) e a parte não ordenada (inicialmente a lista inteira).
2. **Seleção e Troca**
   - Encontra o menor elemento na parte não ordenada.
   - Troca o menor elemento encontrado com o primeiro elemento da parte não ordenada.
3. **Iteração**
   - Repete o processo movendo o limite entre a parte ordenada e a não ordenada uma posição para a direita, até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Selection Sort:

1. **Inicialização**:
   - Lista: `[5, 3, 8, 4, 2]`
   - Parte ordenada: `[]`
   - Parte não ordenada: `[5, 3, 8, 4, 2]`
2. **Primeira Passagem**:
   - Encontra o menor elemento na parte não ordenada (`2`)
   - Troca `2` com o primeiro elemento (`5`). Lista: `[2, 3, 8, 4, 5]`
   - Parte ordenada: `[2]`
   - Parte não ordenada: `[3, 8, 4, 5]`
3. **Segunda Passagem**:
   - Encontra o menor elemento na parte não ordenada (`3`)
   - Troca `3` com o primeiro elemento da parte não ordenada (`3`). Lista: `[2, 3, 8, 4, 5]`
   - Parte ordenada: `[2, 3]`
   - Parte não ordenada: `[8, 4, 5]`
4. **Terceira Passagem**:
   - Encontra o menor elemento na parte não ordenada (`4`)
   - Troca `4` com o primeiro elemento da parte não ordenada (`8`). Lista: `[2, 3, 4, 8, 5]`
   - Parte ordenada: `[2, 3, 4]`
   - Parte não ordenada: `[8, 5]`
5. **Quarta Passagem**:
   - Encontra o menor elemento na parte não ordenada (`5`)
   - Troca `5` com o primeiro elemento da parte não ordenada (`8`). Lista: `[2, 3, 4, 5, 8]`
   - Parte ordenada: `[2, 3, 4, 5]`
   - Parte não ordenada: `[8]`
6. **Quinta Passagem**:
   - O menor elemento na parte não ordenada é `8`, que já está na posição correta
   - Lista final: `[2, 3, 4, 5, 8]`
   - Parte ordenada: `[2, 3, 4, 5, 8]`
   - Parte não ordenada: `[]`

Algumas das principais características do Selection Sort são:

- **Complexidade de Tempo**: A complexidade é `O(n²)` tanto no pior quanto no melhor caso, onde n é o número de elementos na lista. Isso ocorre porque o algoritmo sempre faz `n - 1` passagens pela lista, cada uma requerendo uma busca linear para encontrar o menor elemento.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place, não requer espaço adicional significativo.
- **Estabilidade**: Não é um algoritmo estável, pois pode trocar elementos iguais, alterando sua ordem relativa.

O Selection Sort é adequado para listas pequenas ou quando a simplicidade da implementação é mais importante do que a eficiência. Seu desempenho costuma ser superior ao Bubble Sort e inferior ao Insertion Sort. Em contextos onde o desempenho é crucial, algoritmos de ordenação mais eficientes como Quick Sort, Merge Sort ou Heap Sort são preferíveis.
# Capítulo 4 – Merge Sort, Quick Sort, Heap Sort e Shell Sort

**Merge Sort**, **Quick Sort**, **Heap Sort** e **Shell Sort** são algoritmos de ordenação mais avançados que oferecem melhor desempenho em comparação aos métodos simples. **Merge Sort** é um algoritmo baseado em divisão e conquista, que divide a lista em sublistas menores, ordena cada sublista e as mescla para formar a lista ordenada final. Este método garante uma complexidade de tempo `O(n log n)`, tornando-o eficiente para grandes conjuntos de dados. **Quick Sort**, também é um algoritmo de divisão e conquista, seleciona um elemento "pivô" e reorganiza a lista de modo que todos os elementos menores que o pivô fiquem antes dele, e todos os maiores, depois. A complexidade média é `O(n log n)`, mas pode atingir `O(n²)` no pior caso se o pivô for mal escolhido. **Heap Sort** utiliza uma estrutura de dados chamada heap para ordenar os elementos. Ele constrói um heap a partir dos dados e extrai repetidamente o maior elemento, reconstruindo o heap até que todos os elementos estejam ordenados. Heap Sort também oferece complexidade `O(n log n)`, mas não é estável. **Shell Sort** é uma generalização do Insertion Sort que compara e troca elementos que estão a uma certa distância (gap) entre si, diminuindo o gap gradualmente até que ele se torne 1. Isso permite que o Shell Sort seja mais eficiente que o Insertion Sort, especialmente em listas maiores.
## Merge Sort

O algoritmo de ordenação Merge Sort é um método eficiente e baseado na abordagem dividir para conquistar. Ele funciona dividindo a lista em sublistas menores, ordenando essas sublistas e, em seguida, mesclando-as de volta em uma lista ordenada. A divisão continua até que cada sublista contenha apenas um elemento, que por definição está ordenado.

O passo a passo para a sua realização é o seguinte:

1. **Divisão**
   - Divide a lista para criar duas sublistas.
   - Repete o processo recursivamente para cada sublista até que cada sublista contenha apenas um elemento.
2. **Conquista (Ordenação)**
   - Ordena cada sublista com um único elemento (implicitamente ordenado).
3. **Combinação (Mesclagem)**
   - Combina (mescla) as sublistas menores em sublistas ordenadas maiores até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Merge Sort:

1. **Divisão**:
   - Lista original: `[5, 3, 8, 4, 2]`
   - Divide em: `[5, 3, 8]` e `[4, 2]`
   - Divide `[5, 3, 8]` em: `[5]` e `[3, 8]`
   - Divide `[3, 8]` em: `[3]` e `[8]`
   - Divide `[4, 2]` em: `[4]` e `[2]`
2. **Combinação e Mesclagem**:
   - `[3]` e `[8]` são mesclados em: `[3, 8]`
   - `[5]` e `[3, 8]` são mesclados em: `[3, 5, 8]`
   - `[4]` e `[2]` são mesclados em: `[2, 4]`
   - `[3, 5, 8]` e `[2, 4]` são mesclados em: `[2, 3, 4, 5, 8]`

Algumas das principais características do Merge Sort são:

- **Complexidade de Tempo**: O Merge Sort tem uma complexidade de tempo `O(n log n)` para todos os casos (pior, médio e melhor), onde `n` é o número de elementos na lista. Isso se deve à divisão repetida da lista e à subsequente mesclagem.
- **Complexidade de Espaço**: `O(n)`, pois requer espaço adicional para armazenar as sublistas durante o processo de mesclagem.
- **Estabilidade**: É um algoritmo estável, o que significa que não altera a ordem relativa de elementos iguais.

O Merge Sort é particularmente útil para ordenar listas grandes ou quando a estabilidade é importante. Ele é amplamente utilizado em várias aplicações de software devido à sua eficiência e robustez, sendo geralmente é mais rápido que algoritmos de complexidade `O(n²)` como Bubble Sort, Insertion Sort, e Selection Sort. Ele requer espaço adicional `O(n)` para a sua implementação, o que pode ser uma desvantagem para listas muito grandes, além de poder ser mais complicado de implementar corretamente em comparação com algoritmos de ordenação mais simples.

## Quick Sort

O algoritmo de ordenação Quick Sort é um método eficiente e amplamente utilizado, baseado na abordagem dividir para conquistar. Ele funciona escolhendo um elemento chamado pivô e particionando a lista de forma que todos os elementos menores que o pivô fiquem à esquerda e todos os elementos maiores que o pivô fiquem à direita. Em seguida, o processo é repetido recursivamente para as sublistas à esquerda e à direita do pivô. 

O passo a passo para a sua realização é o seguinte:

1. **Escolha do Pivô**
   - Escolhe um elemento da lista como pivô. Existem várias estratégias para escolher o pivô, como o primeiro elemento, o último elemento, o elemento do meio ou até mesmo um elemento aleatório.
2. **Particionamento**
   - Reorganiza os elementos da lista de forma que todos os elementos menores que o pivô fiquem à esquerda e todos os elementos maiores que o pivô fiquem à direita. O pivô está então na sua posição final.
3. **Recursão**
   - Aplica recursivamente o algoritmo Quick Sort nas sublistas à esquerda e à direita do pivô até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Quick Sort:

1. **Escolha do Pivô**:
   - Escolhemos o último elemento como pivô: `2`
2. **Particionamento**:
   - Reorganiza a lista em torno do pivô `2`: `[2, 3, 8, 4, 5]`
   - Lista após particionamento (`2` está na posição correta): `[2, 3, 8, 4, 5]`
3. **Recursão**:
   - Aplica Quick Sort na sublista à esquerda do pivô `2` (`[]`), que já está ordenada
   - Aplica Quick Sort na sublista à direita do pivô `2` (`[3, 8, 4, 5]`)
4. **Repetição do Processo**:
   - Escolhe `5` como pivô na sublista `[3, 8, 4, 5]`
   - Particiona a sublista em torno do pivô `5`: `[3, 4, 5, 8]`
   - Aplica Quick Sort na sublista à esquerda do pivô `5` (`[3, 4]`)
      - Escolhe `4` como pivô
      - Particiona `[3, 4]` em torno do pivô `4`: `[3, 4]`
   - Aplica Quick Sort na sublista à direita do pivô `5` (`[8]`), que já está ordenada
5. **Conclusão**:
   - Combina todas as partes ordenadas para obter a lista final: `[2, 3, 4, 5, 8]`

Algumas das principais características do Quick Sort são:

- **Complexidade de Tempo**: a complexidade de tempo é `O(n²)` no pior caso, que ocorre quando o pivô escolhido é sempre o menor ou o maior elemento. Isso pode ser mitigado com uma boa escolha do pivô (e.g., escolha aleatória ou mediana de três). No caso médio e no melhor caso é `O(n log n)`, onde o melhor caso ocorre quando o pivô divide a lista de forma aproximadamente igual a cada passo. Na prática o que acontece comumente é o caso médio
- **Complexidade de Espaço**: Sem considerar a pilha de recursão o espaço é `O(1)` numa versão interativa do algoritmo, mas pode ser `O(log n)` ou `O(n)` devido à profundidade da pilha de recursão em caso médio ou pior caso
- **Estabilidade**: Não é um algoritmo estável, pois pode alterar a ordem relativa de elementos iguais

O Quick Sort é um dos algoritmos de ordenação mais eficientes e amplamente utilizados, especialmente adequado para grandes conjuntos de dados, não sendo o mais ideal para uma pequena quantidade de dados. Com uma boa escolha de pivô, ele oferece um excelente desempenho com complexidade média de `O(n log n)`. No entanto, para garantir estabilidade ou evitar o pior caso de `O(n²)`, pode ser necessário modificações ou combinações com outros algoritmos. Além disso, possui baixa sobrecarga, pois requer apenas uma pequena quantidade de memória para funcionar.

## Heap Sort

O algoritmo de ordenação Heap Sort é um método eficiente que usa uma estrutura de dados chamada heap (mais especificamente, uma max-heap ou min-heap) para ordenar uma lista. Ele é baseado no conceito de uma árvore binária completa, onde cada nó é maior (no caso de uma max-heap) ou menor (no caso de uma min-heap) que seus filhos. O Heap Sort divide a lista em duas partes: a parte ordenada e a parte não ordenada, e reduz gradualmente a parte não ordenada reorganizando-a em um heap e movendo o maior (ou menor) elemento para a parte ordenada.

O passo a passo para a sua realização é o seguinte:

1. **Construção do Heap**
   - Constrói um max-heap (ou min-heap) a partir da lista original. Em um max-heap, o maior elemento está na raiz da árvore.
2. **Ordenação**
   - Troca o maior elemento (raiz do heap) com o último elemento da lista.
   - Reduz o tamanho do heap (ignorando o último elemento, que agora está na posição correta).
   - Reorganiza (heapifica) a lista restante para restabelecer a propriedade do heap.
   - Repete o processo até que todo o heap seja reduzido e a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Heap Sort:

1. **Construção do Max-Heap**:
   - Converte a lista `[5, 3, 8, 4, 2]` em um max-heap:
      - `[8, 5, 2, 4, 3]`
   - O maior elemento (`8`) está na raiz
2. **Ordenação**:
   - Troca `8` (raiz) com `3` (último elemento). Lista: `[3, 5, 2, 4, 8]`
   - Reduz o heap: `[3, 5, 2, 4]` e reorganiza:
      - `[5, 4, 2, 3]`
   - Troca `5` (raiz) com `3` (último elemento). Lista: `[3, 4, 2, 5, 8]`
   - Reduz o heap: `[3, 4, 2]` e reorganiza:
      - `[4, 3, 2]`
   - Troca `4` (raiz) com `2` (último elemento). Lista: `[2, 3, 4, 5, 8]`
   - Reduz o heap: `[2, 3]` e reorganiza:
      - `[3, 2]`
   - Troca `3` (raiz) com `2` (último elemento). Lista: `[2, 3, 4, 5, 8]`

Algumas das principais características do Heap Sort são:

- **Complexidade de Tempo**: em todos os casos, a complexidade é `O(n log n)` porque a construção do heap leva `O(n)` tempo e cada uma das `n` remoções do maior elemento leva `O(log n)` tempo.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place, não requer espaço adicional significativo. Todavia, pode ser `O(log n)`, devido à pilha de chamadas recursiva.
- **Estabilidade**: Não é um algoritmo estável, pois pode alterar a ordem relativa de elementos iguais durante a troca.

O Heap Sort é um algoritmo de ordenação eficiente e robusto com complexidade de tempo garantida de `O(n log n)` e uso de memória `O(1)`. Normalmente é 2 a 3 vezes mais lento que o Quick Sort bem implementado. A razão da lentidão é a falta de localidade de referência. Também não é muito eficiente ao trabalhar com dados altamente complexos. Embora não seja estável por padrão e possa ser mais lento que outros algoritmos `O(n log n)` na prática, é uma excelente escolha quando a memória é uma preocupação ou quando uma complexidade de tempo garantida é necessária.

## Shell Sort

O algoritmo de ordenação Shell Sort é uma generalização do Insertion Sort que melhora a eficiência ao lidar com listas grandes. Ele faz isso permitindo a troca de elementos que estão distantes uns dos outros, em vez de adjacentes. O algoritmo é nomeado após seu inventor, Donald Shell, que o descreveu pela primeira vez em 1959.

O passo a passo para a sua realização é o seguinte:

1. **Escolha dos Gaps**
   - O algoritmo começa escolhendo uma sequência de gaps, que são intervalos entre os elementos a serem comparados e trocados. Uma sequência comum é dividir o tamanho da lista por 2 repetidamente até chegar a 1.
2. **Ordenação com Insertion Sort**
   - Para cada gap, a lista é dividida em sublistas, onde cada sublista é ordenada usando Insertion Sort.
   - O processo é repetido com gaps menores até que o gap seja 1. No final, a lista estará quase ordenada, permitindo que uma última passagem de Insertion Sort finalize a ordenação.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2, 6, 1, 7]` usando o Shell Sort:

1. **Gap 4**:
   - Sublistas formadas: `[5, 2]`, `[3, 6]`, `[8, 1]`, `[4, 7]`
   - Ordenando as sublistas:
      - `[5, 2]` -> `[2, 5]`
      - `[3, 6]` -> `[3, 6]`
      - `[8, 1]` -> `[8, 1]`
      - `[4, 7]` -> `[4, 7]`
   - Lista após gap 4: `[2, 3, 1, 4, 5, 6, 8, 7]`
2. **Gap 2**:
   - Sublistas formadas: `[2, 1, 5, 8]`, `[3, 4, 6, 7]`
   - Ordenando as sublistas:
      - `[2, 1, 5, 8]` -> `[1, 2, 5, 8]`
      - `[3, 4, 6, 7]` -> `[3, 4, 6, 7]`
   - Lista após gap 2: `[1, 2, 3, 4, 5, 6, 8, 7]`
3. **Gap 1 (Final Insertion Sort)**:
   - Sublista completa: `[1, 2, 3, 4, 5, 6, 8, 7]`
   - Ordenando com Insertion Sort:
      - Troca `8` com `7`: `[1, 2, 3, 4, 5, 6, 7, 8]`
   - Lista final ordenada: `[1, 2, 3, 4, 5, 6, 7, 8]`

Algumas das principais características do Shell Sort são:

- **Complexidade de Tempo**: Dependente da sequência de gaps escolhida. A complexidade do pior caso para a sequência de Shell original é `O(n²)`, mas com sequências melhores, como a de Hibbard ou Sedgewick, pode ser melhorada para <code>O(n<sup>3/2</sup>)</code> ou `O(n log²n)`. No caso é geralmente <code>O(n<sup>3/2</sup>)</code> com sequências de gaps comuns. E no melhor caso é `O(n log n)`.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place.
- **Estabilidade**: Não é estável, pois pode alterar a ordem relativa de elementos iguais.

O Shell Sort é uma melhoria significativa em relação ao Insertion Sort, especialmente para listas de tamanho moderado. Sua eficiência depende fortemente da escolha da sequência de gaps. Embora não seja o algoritmo de ordenação mais eficiente para listas muito grandes, é uma boa escolha para listas de tamanho médio onde simplicidade e facilidade de implementação são importantes. Para listas muito grandes, algoritmos como Quick Sort, Merge Sort e Heap Sort são mais eficientes.

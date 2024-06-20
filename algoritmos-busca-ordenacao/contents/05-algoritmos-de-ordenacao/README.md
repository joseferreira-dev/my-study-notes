<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Algoritmos de Ordenação - Parte 4: Counting Sort, Radix Sort, Bucket Sort

- [Introdução](#introdução)
- [Counting Sort](#counting-sort)
- [Radix Sort](#radix-sort)
- [Bucket Sort](#bucket-sort)

## Introdução

**Counting Sort**, **Radix Sort** e **Bucket Sort** são algoritmos de ordenação não comparativos que oferecem alternativas eficientes aos métodos baseados em comparação, especialmente para certos tipos de dados. **Counting Sort** conta o número de ocorrências de cada valor em uma lista e utiliza essa contagem para posicionar os elementos diretamente na lista ordenada. Ele é eficiente para listas onde os valores estão dentro de um intervalo limitado, com complexidade `O(n + k)`, onde `k` é o valor máximo na lista. **Radix Sort** ordena números inteiros processando cada dígito individualmente, do dígito menos significativo para o mais significativo (ou vice-versa), utilizando um algoritmo de ordenação estável como o Counting Sort para cada dígito. Ele é eficiente para listas grandes de números inteiros, com complexidade `O(d ⋅ (n + k))`, onde `d` é o número de dígitos. **Bucket Sort** distribui os elementos em vários baldes, ordena cada balde individualmente (frequentemente com Insertion Sort), e então concatena os baldes ordenados. É eficiente para listas com elementos distribuídos uniformemente, oferecendo complexidade média `O(n + k)`, onde `k` é o número de baldes.

## Counting Sort

O algoritmo de ordenação Counting Sort é um método eficiente para ordenar listas de números inteiros dentro de um intervalo limitado. Ele não compara elementos diretamente, o que o torna particularmente útil para grandes conjuntos de dados com valores repetidos. O Counting Sort conta o número de ocorrências de cada valor e usa essas contagens para determinar as posições finais dos elementos na lista ordenada.

O passo a passo para a sua realização é o seguinte:

1. **Determinação do Intervalo**
   - Encontra o valor máximo e o valor mínimo na lista para determinar o intervalo de valores.
2. **Contagem das Ocorrências**
   - Cria um array de contagem (`count`) onde cada índice representa um valor no intervalo e armazena o número de ocorrências de cada valor da lista original.
3. **Cálculo das Posições**
   - Modifica o array de contagem para que cada posição `i` contenha a soma das contagens dos valores anteriores, indicando a posição final de cada valor na lista ordenada.
4. **Construção da Lista Ordenada**
   - Itera pela lista original de trás para frente, coloca cada elemento na posição correta na lista ordenada e diminui a contagem correspondente no array de contagem.

Vamos realizar um exemplo prático ordenando a lista `[2, 5, 3, 0, 2, 3, 0, 3]` usando o Counting Sort:

1. **Determinação do Intervalo**:
   - Valor mínimo: `0`
   - Valor máximo: `5`
2. **Contagem das Ocorrências**:
   - Cria o array de contagem com tamanho `6` (do valor mínimo `0` ao máximo `5`): `[0, 0, 0, 0, 0, 0]`
   - Conta as ocorrências:
      - `[2, 0, 2, 3, 0, 1]` (onde o índice representa o valor, no caso do índice `0` o valor é `2` pois o `0` aparece duas vezes)
3. **Cálculo das Posições**:
   - Modifica o array de contagem para armazenar as posições finais:
      - `[2, 2, 4, 7, 7, 8]`, neste caso o array armazena o somatório acumulativo dos itens no array final. O primerio item das ocorrências (`0`) deve ser repetido até preencher as duas primeira posições da lista final, já o segundo (`1`) não será repetido nenhuma vez, já que o somatório não aumenta. O `2` será repetido mais duas vezes, até o somatório chegar a `4` e assim por diante.
4. **Construção da Lista Ordenada**:
   - Itera pela lista original de trás para frente e constrói a lista ordenada:
      - `[1, 2, 2, 3, 3, 4, 8]`

Algumas das principais características do Counting Sort são:

- **Complexidade de Tempo**: `O(n + k)`, onde `n`  é o número de elementos na lista e `k` é o intervalo de valores possíveis (diferença entre o valor máximo e mínimo).
- **Complexidade de Espaço**: `O(n + k)`, pois é necessário espaço adicional para o array de contagem e a lista ordenada.
- **Estabilidade**: É estável, preservando a ordem relativa de elementos iguais.

Counting Sort é um algoritmo poderoso para ordenar listas de números inteiros quando o intervalo de valores é limitado. Ele é especialmente útil para listas grandes onde o intervalo de valores (`k`) é pequeno em relação ao número de elementos (`n`), onde se tem muitos valores repetidos, oferecendo uma complexidade de tempo linear `O(n + k)`. No entanto, devido à sua necessidade de espaço adicional, não é adequado para listas com intervalos de valores muito grandes.

## Radix Sort

O algoritmo de ordenação Radix Sort é um método de ordenação não comparativo que ordena números inteiros ou outros tipos de dados ao processar cada dígito individualmente. É particularmente útil para ordenar grandes conjuntos de dados numéricos e pode ser muito eficiente quando combinado com outro algoritmo de ordenação estável, como o Counting Sort.

O passo a passo para a sua realização é o seguinte:

1. **Escolha da Base**
   - Radix Sort processa os números dígito por dígito, começando pelo dígito menos significativo (LSD) ou pelo dígito mais significativo (MSD). A base mais comum é decimal (base 10).
2. **Ordenação dos Dígitos**
   - Ordena a lista de números para cada posição do dígito, usando um algoritmo de ordenação estável, como Counting Sort.
3. **Iteração pelos Dígitos**
   - Repete o processo para cada posição do dígito, desde o dígito menos significativo até o mais significativo (para LSD Radix Sort).

Vamos realizar um exemplo prático ordenando a lista `[170, 45, 75, 90, 802, 24, 2, 66]` usando o Radix Sort:

1. **Ordenação pelo Dígito Menos Significativo (LSD)**:
   - Inicialmente, ordena pela unidade:
      - `[170, 90, 802, 2, 24, 45, 75, 66]`
2. **Ordenação pelo Próximo Dígito**:
   - Em seguida, ordena pela dezena:
      - `[802, 2, 24, 45, 66, 170, 75, 90]`
3. **Ordenação pelo Dígito Mais Significativo**:
   - Finalmente, ordena pela centena:
      - `[2, 24, 45, 66, 75, 90, 170, 802]`

Algumas das principais características do Radix Sort são:

- **Complexidade de Tempo**: `O(d ⋅ (n + k))`, `d` é o número de dígitos no maior número, `n` é o número de elementos na lista e `k` é o intervalo de cada dígito (por exemplo, 0-9 para base 10).
- **Complexidade de Espaço**: `O(n + k)`, devido ao espaço adicional necessário para a ordenação estável dos dígitos.
- **Estabilidade**: É estável, pois a ordenação de cada dígito mantém a ordem relativa dos números.

Radix Sort é um algoritmo de ordenação potente e eficiente para números inteiros, especialmente adequado para grandes conjuntos de dados. Ele utiliza a ordenação estável dos dígitos individuais para alcançar uma complexidade linear em relação ao número de elementos e ao número de dígitos. Embora requeira espaço adicional e não seja aplicável diretamente a todos os tipos de dados, é uma escolha excelente para casos específicos onde números inteiros precisam ser ordenados rapidamente.

## Bucket Sort

O algoritmo de ordenação Bucket Sort é uma técnica de ordenação que distribui os elementos de uma lista em vários "baldes" ou "compartimentos". Cada balde é então ordenado individualmente, normalmente usando outro algoritmo de ordenação, como o Insertion Sort ou até mesmo o Quick Sort. Depois que todos os baldes são ordenados, os elementos são reunidos para formar a lista ordenada final. Este método é eficiente para conjuntos de dados onde os valores são uniformemente distribuídos em um intervalo conhecido.

O passo a passo para a sua realização é o seguinte:

1. **Divisão em Baldes**
   - Distribui os elementos da lista original em vários baldes. A escolha de como os elementos são distribuídos pode variar, mas geralmente é baseada no valor do elemento e na faixa de valores.
2. **Ordenação dos Baldes**
   - Cada balde é ordenado individualmente. Como os baldes tendem a ter poucos elementos, algoritmos simples e eficientes como o Insertion Sort são frequentemente usados.
3. **Reunião dos Baldes**
   - Os elementos ordenados de cada balde são concatenados para formar a lista final ordenada.

Vamos realizar um exemplo prático ordenando a lista `[0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]` usando o Bucket Sort:

1. **Divisão em Baldes**:
   - Suponha que temos 10 baldes, cada um representando um intervalo de 0.1 (0-0.1, 0.1-0.2, ..., 0.9-1.0):
      - Balde 0: `[ ]`
      - Balde 1: `[0.17, 0.12, 0.21, 0.23]`
      - Balde 2: `[0.26]`
      - Balde 3: `[0.39]`
      - Balde 4: `[ ]`
      - Balde 5: `[ ]`
      - Balde 6: `[0.68]`
      - Balde 7: `[0.78, 0.72]`
      - Balde 8: `[ ]`
      - Balde 9: `[0.94]`
2. **Ordenação dos Baldes**:
   - Cada balde é ordenado individualmente:
      - Balde 1: `[0.12, 0.17, 0.21, 0.23]`
      - Balde 2: `[0.26]`
      - Balde 3: `[0.39]`
      - Balde 6: `[0.68]`
      - Balde 7: `[0.72, 0.78]`
      - Balde 9: `[0.94]`
3. **Reunião dos Baldes**:
   - Concatenamos os elementos dos baldes na ordem crescente:
      - Lista final ordenada: `[0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]`

Algumas das principais características do Bucket Sort são:

- **Complexidade de Tempo**: No melhor caso é `O(n + k)`, onde `n` é o número de elementos na lista e `k` é o número de baldes. Isto assume que os elementos são distribuídos uniformemente e o algoritmo de ordenação utilizado nos baldes é eficiente. A complexidade é a mesma para o caso médio, considerando distribuições uniformes. No pior caso é `O(n²)`, se todos os elementos caírem em um único balde, transformando-se em um Insertion Sort ou outro algoritmo de ordenação utilizado nos baldes.
- **Complexidade de Espaço**: `O(n + k)`, devido ao espaço necessário para os baldes além da lista original.
- **Estabilidade**: Depende do algoritmo de ordenação utilizado nos baldes. Se um algoritmo estável for usado, o Bucket Sort será estável.

Bucket Sort é um algoritmo de ordenação eficiente para listas de elementos que são distribuídos de maneira uniforme em um intervalo conhecido. Sua complexidade de tempo pode ser muito próxima de linear, `O(n + k)`, no melhor caso. No entanto, sua eficiência depende da distribuição dos elementos e da escolha do algoritmo de ordenação para os baldes. É especialmente útil em casos onde temos conhecimento prévio sobre a distribuição dos dados.

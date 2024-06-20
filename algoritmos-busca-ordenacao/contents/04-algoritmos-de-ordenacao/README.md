<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Algoritmos de Ordenação - Parte 3: Shell Sort, Comb Sort, Tim Sort

- [Introdução](#introdução)
- [Shell Sort](#shell-sort)
- [Comb Sort](#comb-sort)
- [Tim Sort](#tim-sort)

## Introdução

**Shell Sort**, **Comb Sort** e **Tim Sort** são algoritmos de ordenação que introduzem técnicas avançadas para melhorar a eficiência em comparação aos métodos mais básicos. **Shell Sort** é uma generalização do Insertion Sort que compara e troca elementos que estão a uma certa distância (gap) entre si, diminuindo o gap gradualmente até que ele se torne 1. Isso permite que o Shell Sort seja mais eficiente que o Insertion Sort, especialmente em listas maiores. **Comb Sort** é uma melhoria do Bubble Sort, que também usa um gap para comparar e trocar elementos, começando com um gap maior e diminuindo gradualmente até 1. Comb Sort resolve alguns dos problemas de ineficiência do Bubble Sort, lidando melhor com "tartarugas" (valores pequenos perto do final da lista). **Tim Sort** é um algoritmo híbrido que combina Insertion Sort e Merge Sort, projetado para aproveitar runs (sublistas ordenadas) existentes na lista original, garantindo uma complexidade `O(n log n)` no pior caso e excelente desempenho em muitos casos práticos. Tim Sort é usado em implementações de ordenação padrão em linguagens como Python e Java devido à sua eficiência e estabilidade.

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

## Comb Sort

O algoritmo de ordenação Comb Sort é uma melhoria sobre o Bubble Sort. Ele foi introduzido por Wlodzimierz Dobosiewicz em 1980 e popularizado por Stephen Lacey e Richard Box em um artigo publicado em 1991. O Comb Sort combate uma das principais limitações do Bubble Sort: o fato de que ele compara apenas elementos adjacentes. Comb Sort compara elementos que estão a uma certa "distância" uns dos outros, o que ajuda a eliminar "tartarugas" (elementos pequenos perto do final da lista) mais rapidamente.

O passo a passo para a sua realização é o seguinte:

1. **Definição do Gap Inicial**
   - O gap inicial (distância entre os elementos comparados) é normalmente definido como o tamanho da lista. A cada iteração, o gap é reduzido por um fator (geralmente 1.3).
2. **Comparação e Troca**
   - Percorre a lista comparando elementos que estão a uma distância gap entre si e troca-os se estiverem fora de ordem.
3. **Reduzir o Gap**
   - Após cada passagem, o gap é reduzido pelo fator mencionado (por exemplo, dividido por 1.3). Quando o gap se torna 1, o algoritmo se comporta como o Bubble Sort.
4. **Iteração até o Final**
   - O processo continua até que o gap seja reduzido a 1 e a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[9, 7, 5, 3, 1]` usando o Comb Sort:

1. **Definição do Gap Inicial**:
   - Tamanho da lista é 5, então o gap inicial é igual 5/1.3 ≈ 3.
2. **Comparação e Troca com Gap 3**:
   - Comparar elementos com gap 3:
      - `[9, 7, 5, 3, 1]` (9 e 3, 7 e 1)
      - Troca 9 e 3: `[3, 7, 5, 9, 1]`
      - Troca 7 e 1: `[3, 1, 5, 9, 7]`
3. **Reduzir o Gap**:
   - Novo gap = 3 / 1.3 ≈ 2.
4. **Comparação e Troca com Gap 2**:
   - Comparar elementos com gap 2:
      - `[3, 1, 5, 9, 7]` (3 e 5, 1 e 9, 5 e 7)
      - Troca 1 e 5: `[3, 5, 1, 9, 7]`
      - Troca 1 e 9: `[3, 5, 9, 1, 7]`
      - Troca 9 e 7: `[3, 5, 1, 7, 9]`
5. **Reduzir o Gap**:
   - Novo gap = 2 / 1.3 ≈ 1.
6. **Comparação e Troca com Gap 1 (Bubble Sort)**:
   - `[3, 5, 1, 7, 9]` (3 e 5, 5 e 1, 1 e 7, 7 e 9)
   - Troca 5 e 1: `[3, 1, 5, 7, 9]`
   - Troca 3 e 1: `[1, 3, 5, 7, 9]`
   - A lista agora está ordenada: `[1, 3, 5, 7, 9]`

Algumas das principais características do Comb Sort são:

- **Complexidade de Tempo**: a complexidade de tempo é `O(n log n)` no melhor caso, devido à redução exponencial do gap. O caso médio fica entre `O(n log n)` e `O(n²)`, dependendo do fator de redução do gap. Já no pior caso, a complexidade é `O(n²)`, semelhante ao Bubble Sort, mas geralmente mais rápido em casos práticos.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place.
- **Estabilidade**: Não é estável, pois pode trocar elementos iguais e alterar sua ordem relativa.

Comb Sort é uma melhoria significativa sobre o Bubble Sort, especialmente para listas onde elementos pequenos estão perto do final (tartarugas). Sua capacidade de eliminar tartarugas rapidamente torna-o mais eficiente em muitos casos práticos, embora não atinja a eficiência de algoritmos de ordenação de tempo `O(n log n)`. É uma boa escolha quando se busca uma solução simples que seja mais rápida que o Bubble Sort para listas moderadamente desordenadas, mas ainda não é tão eficiente quanto algoritmos de ordenação `O(n log n)` como Merge Sort ou Quick Sort.

## Tim Sort

Tim Sort é um algoritmo de ordenação híbrido derivado do Merge Sort e do Insertion Sort. Ele foi projetado para funcionar bem em muitos tipos de dados do mundo real. O Tim Sort é utilizado em linguagens de programação como Python (na função `sorted()` e `list.sort()`) e Java (na implementação da classe `Arrays.sort()` para arrays de objetos).

O passo a passo para a sua realização é o seguinte:

1. **Divisão da Lista em Runs**
   - A lista é dividida em pequenas sublistas chamadas "runs". Um run é uma subsequência monotônica (não decrescente ou não crescente). Se a sequência for decrescente, ela é invertida para se tornar crescente. O tamanho mínimo dos runs é geralmente entre 32 e 64. Tim Peters, o criador do Tim Sort, escolheu 32 como um valor padrão para Python, baseado em experimentos empíricos.
2. **Ordenação dos Runs com Insertion Sort**
   - Cada run é ordenado usando Insertion Sort. O uso do Insertion Sort é eficiente para pequenas sublistas.
3. **Mesclagem dos Runs com Merge Sort**
   - Os runs ordenados são então mesclados usando um processo de mesclagem similar ao Merge Sort. A mesclagem é feita de maneira a garantir a eficiência e estabilidade da ordenação.

Vamos realizar um exemplo prático ordenando a lista `[5, 21, 7, 23, 19, 1, 4, 18, 2, 11, 10, 3, 13, 20, 17, 6]` usando o Tim Sort:

1. **Divisão em Runs**:
   - Supomos um run mínimo de tamanho 4. A lista é dividida em runs:
      - `[5, 21, 7, 23]`
      - `[19, 1, 4, 18]`
      - `[2, 11, 10, 3]`
      - `[13, 20, 17, 6]`
2. **Ordenação dos Runs com Insertion Sort**:
   - Cada run é ordenado usando Insertion Sort:
      - `[5, 7, 21, 23]`
      - `[1, 4, 18, 19]`
      - `[2, 3, 10, 11]`
      - `[6, 13, 17, 20]`
3. **Mesclagem dos Runs com Merge Sort**:
   - Os runs ordenados são mesclados:
      - Mesclando `[5, 7, 21, 23]` e `[1, 4, 18, 19]`: `[1, 4, 5, 7, 18, 19, 21, 23]`
      - Mesclando `[2, 3, 10, 11]` e `[6, 13, 17, 20]`: `[2, 3, 6, 10, 11, 13, 17, 20]`
      - Mesclando os dois grandes runs resultantes:
        - `[1, 2, 3, 4, 5, 6, 7, 10, 11, 13, 17, 18, 19, 20, 21, 23]`
   - A lista agora está ordenada: `[1, 2, 3, 4, 5, 6, 7, 10, 11, 13, 17, 18, 19, 20, 21, 23]`.

Algumas das principais características do Tim Sort são:

- **Complexidade de Tempo**: No melhor caso é `O(n)`, quando a lista já está ordenada. No pior caso e no caso médio é `O(n log n)`, similar ao Merge Sort.
- **Complexidade de Espaço**: `O(n)`, devido ao uso de espaço adicional para a mesclagem dos runs.
- **Estabilidade**: É um algoritmo estável, ou seja, mantém a ordem relativa de elementos iguais.

Tim Sort é um algoritmo de ordenação híbrido poderoso e eficiente, projetado para oferecer o melhor desempenho possível em casos do mundo real. Com uma complexidade de tempo `O(n log n)` no pior caso e eficiência adicional em listas parcialmente ordenadas, ele é uma escolha excelente para a ordenação de grandes conjuntos de dados. Sua estabilidade também o torna adequado para muitas aplicações práticas onde a manutenção da ordem relativa dos elementos é importante. Também costuma ter um desempenho equivalente ou superior em comparação aos algoritmos Quick Sort e Merge Sort.

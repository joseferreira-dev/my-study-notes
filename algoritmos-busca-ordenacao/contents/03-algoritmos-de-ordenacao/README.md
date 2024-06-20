<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Algoritmos de Ordenação Instáveis: Selection Sort

- [Introdução](#introdução)


## Introdução



## Selection Sort

O algoritmo de ordenação Selection Sort é um método simples e intuitivo para ordenar uma lista. Ele funciona dividindo a lista em duas partes: a parte ordenada, que é construída do início ao fim da lista, e a parte não ordenada, que contém os elementos restantes. Em cada iteração, o Selection Sort encontra o menor (ou maior, dependendo da ordem desejada) elemento da parte não ordenada e o troca com o primeiro elemento da parte não ordenada, começando a formar a parte ordenada. É um algoritmo cuja versão padrão de implementação é instável, mas também pode ser estável, sendo esta implementação menos usual.

O passo a passo para a sua realização é o seguinte:

1. **Inicialização**
   1. Divide a lista em duas partes: a parte ordenada (inicialmente vazia) e a parte não ordenada (inicialmente a lista inteira).
2. **Seleção e Troca**
   1. Encontra o menor elemento na parte não ordenada.
   2. Troca o menor elemento encontrado com o primeiro elemento da parte não ordenada.
3. **Iteração**
   1. Repete o processo movendo o limite entre a parte ordenada e a não ordenada uma posição para a direita, até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Selection Sort:

1. **Inicialização**:
   1. Lista: `[5, 3, 8, 4, 2]`.
   2. Parte ordenada: `[]`.
   3. Parte não ordenada: `[5, 3, 8, 4, 2]`.
2. **Primeira Passagem**:
   1. Encontra o menor elemento na parte não ordenada (`2`).
   2. Troca `2` com o primeiro elemento (`5`). Lista: `[2, 3, 8, 4, 5]`.
   3. Parte ordenada: `[2]`.
   4. Parte não ordenada: `[3, 8, 4, 5]`.
3. **Segunda Passagem**:
   1. Encontra o menor elemento na parte não ordenada (`3`).
   2. Troca `3` com o primeiro elemento da parte não ordenada (`3`). Lista: `[2, 3, 8, 4, 5]`.
   3. Parte ordenada: `[2, 3]`.
   4. Parte não ordenada: `[8, 4, 5]`.
4. **Terceira Passagem**:
   1. Encontra o menor elemento na parte não ordenada (`4`).
   2. Troca `4` com o primeiro elemento da parte não ordenada (`8`). Lista: `[2, 3, 4, 8, 5]`.
   3. Parte ordenada: `[2, 3, 4]`.
   4. Parte não ordenada: `[8, 5]`.
5. **Quarta Passagem**:
   1. Encontra o menor elemento na parte não ordenada (`5`).
   2. Troca `5` com o primeiro elemento da parte não ordenada (`8`). Lista: `[2, 3, 4, 5, 8]`.
   3. Parte ordenada: `[2, 3, 4, 5]`.
   4. Parte não ordenada: `[8]`.
6. **Quinta Passagem**:
   1. O menor elemento na parte não ordenada é `8`, que já está na posição correta.
   2. Lista final: `[2, 3, 4, 5, 8]`.
   3. Parte ordenada: `[2, 3, 4, 5, 8]`.
   4. Parte não ordenada: `[]`.

Algumas das principais características do Selection Sort são:

- **Complexidade de Tempo**: A complexidade é `O(n²)` tanto no pior quanto no melhor caso, onde n é o número de elementos na lista. Isso ocorre porque o algoritmo sempre faz `n - 1` passagens pela lista, cada uma requerendo uma busca linear para encontrar o menor elemento.
- **Complexidade de Espaço**: `O(1)`, pois é um algoritmo in-place, não requer espaço adicional significativo.
- **Estabilidade**: Não é um algoritmo estável, pois pode trocar elementos iguais, alterando sua ordem relativa.

O Selection Sort é adequado para listas pequenas ou quando a simplicidade da implementação é mais importante do que a eficiência. Em contextos onde o desempenho é crucial, algoritmos de ordenação mais eficientes como Quick Sort, Merge Sort ou Heap Sort são preferíveis. Seu desempenho costuma ser 
superior ao Bubble Sort e inferior ao Insertion Sort.

```
ALGORITMO SELECTION SORT
  ENTRADA: Um vetor L com N posições
  SAÍDA: Um vetor L em ordem crescente
  
  Para i = 0 até n - 2
    Min = i
    Para j = i + 1 até n - 1
      Se L[j] < L[Min]
        Min = j
    Troca(L[i], L[Min])
FIM (SELECTION SORT)
```

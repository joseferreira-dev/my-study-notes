<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Algoritmos de Ordenação - Parte 2: Merge Sort

- [Introdução](#introdução)


## Introdução



## Merge Sort

O algoritmo de ordenação Merge Sort é um método eficiente e baseado na abordagem dividir para conquistar. Ele funciona dividindo a lista em sublistas menores, ordenando essas sublistas e, em seguida, mesclando-as de volta em uma lista ordenada. A divisão continua até que cada sublista contenha apenas um elemento, que por definição está ordenado.

O passo a passo para a sua realização é o seguinte:

1. **Divisão**
   1. Divide a lista para criar duas sublistas.
   2. Repete o processo recursivamente para cada sublista até que cada sublista contenha apenas um elemento.
2. **Conquista (Ordenação)**
   1. Ordena cada sublista com um único elemento (implicitamente ordenado).
3. **Combinação (Mesclagem)**
   1. Combina (mescla) as sublistas menores em sublistas ordenadas maiores até que toda a lista esteja ordenada.

Vamos realizar um exemplo prático ordenando a lista `[5, 3, 8, 4, 2]` usando o Merge Sort:

1. **Divisão**:
   1. Lista original: `[5, 3, 8, 4, 2]`.
   2. Divide em: `[5, 3, 8]` e `[4, 2]`.
   3. Divide `[5, 3, 8]` em: `[5]` e `[3, 8]`.
   4. Divide `[3, 8]` em: `[3]` e `[8]`.
   5. Divide `[4, 2]` em: `[4]` e `[2]`.
2. **Combinação e Mesclagem**:
   1. `[3]` e `[8]` são mesclados em: `[3, 8]`.
   2. `[5]` e `[3, 8]` são mesclados em: `[3, 5, 8]`.
   3. `[4]` e `[2]` são mesclados em: `[2, 4]`.
   4. `[3, 5, 8]` e `[2, 4]` são mesclados em: `[2, 3, 4, 5, 8]`.

Algumas das principais características do Selection Sort são:

- **Complexidade de Tempo**: O Merge Sort tem uma complexidade de tempo `O(n log n)` para todos os casos (pior, médio e melhor), onde `n` é o número de elementos na lista. Isso se deve à divisão repetida da lista e à subsequente mesclagem.
- **Complexidade de Espaço**: `O(n)`, pois requer espaço adicional para armazenar as sublistas durante o processo de mesclagem.
- **Estabilidade**: É um algoritmo estável, o que significa que não altera a ordem relativa de elementos iguais.

O Merge Sort é particularmente útil para ordenar listas grandes ou quando a estabilidade é importante. Ele é amplamente utilizado em várias aplicações de software devido à sua eficiência e robustez, sendo geralmente é mais rápido que algoritmos de complexidade `O(n²)` como Bubble Sort, Insertion Sort, e Selection Sort. Ele requer espaço adicional `O(n)` para a sua implementação, o que pode ser uma desvantagem para listas muito grandes, além de poder ser mais complicado de implementar corretamente em comparação com algoritmos de ordenação mais simples.

```
ALGORITMO MERGE SORT
  ENTRADA: Um vetor L e as posições inicio e fim
  SAÍDA: Um vetor L em ordem crescente da posição inicio até a posição fim
  
  Se inicio < fim
    meio = (inicio + fim) / 2
    Se inicio < meio
      MERGESORT(L, inicio, meio)
    Se meio + 1 > fim
      MERGESORT(L, meio + 1, fim)
    MERGE(L, inicio, meio, fim)
FIM (MERGE SORT)
```

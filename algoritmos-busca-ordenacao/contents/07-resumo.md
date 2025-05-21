<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Resumo das Complexidades de Algoritmos de Busca e Ordenação

- [Introdução](#introdução)
- [Complexidade de Algoritmos de Ordenação](#complexidade-de-algoritmos-de-ordenação)
- [Complexidade de Algoritmos de Busca](#complexidade-de-algoritmos-de-busca)

## Introdução

É importante se ter uma noção geral da complexidade dos diferentes algoritmos de busca e ordenação, além de suas vantagens e desvantagens, para que se possa optar pelo melhor em cada situação.

## Complexidade de Algoritmos de Ordenação

Segue abaixo uma tabela com o resumo das complexidades de tempo dos principais algoritmos de ordenação:

| **Algoritmo**   | **Melhor Caso**  |  **Caso Médio**  |   **Pior Caso**  |**Espaço Adicional**|**Estável**|
|-----------------|------------------|------------------|------------------|--------------------|-----------|
| Bubble Sort     | O(n)             | O(n²)            | O(n²)            | O(1)               | Sim       |
| Insertion Sort  | O(n)             | O(n²)            | O(n²)            | O(1)               | Sim       |
| Selection Sort  | O(n²)            | O(n²)            | O(n²)            | O(1)               | Não       |
| Merge Sort      | O(n log n)       | O(n log n)       | O(n log n)       | O(n)               | Sim       |
| Quick Sort      | O(n log n)       | O(n log n)       | O(n²)            | O(log n)           | Não       |
| Heap Sort       | O(n log n)       | O(n log n)       | O(n log n)       | O(1)               | Não       |
| Shell Sort      | O(n log n)       |O(n<sup>3/2</sup>)| O(n²)            | O(1)               | Não       |
| Comb Sort       | O(n log n)       |O(n²) / O(n log n)| O(n²)            | O(1)               | Não       |
| Tim Sort        | O(n)             | O(n log n)       | O(n log n)       | O(n)               | Sim       |
| Counting Sort   | O(n + k)         | O(n + k)         | O(n + k)         | O(n + k)           | Sim       |
| Radix Sort      | O(d(n + k))      | O(d(n + k))      | O(d(n + k))      | O(n + k)           | Sim       |
| Bucket Sort     | O(n + k)         | O(n + k)         | O(n²)            | O(n + k)           | Sim       |

Notas
- `k` em Counting Sort e Radix Sort refere-se ao intervalo dos valores de entrada ou ao número de dígitos.
- `d` em Radix Sort refere-se ao número de dígitos dos números de entrada.

## Complexidade de Algoritmos de Busca

Segue abaixo uma tabela com o resumo das complexidades de tempo dos principais algoritmos de busca:

| **Algoritmo**          | **Melhor Caso**       | **Pior Caso**         | Notas Adicionais                               |
|------------------------|-----------------------|-----------------------|------------------------------------------------|
| Busca Sequencial       | O(1)                  | O(n)                  | O elemento está na primeira/última posição     |
| Busca Binária          | O(1)                  | O(log n)              | Requer lista ordenada                          |
| Busca em Profundidade  | O(1)                  | O(V + E)              | V é o número de vértices e E é o de arestas    |
| Busca em Largura       | O(1)                  | O(V + E)              | V é o número de vértices e E é o de arestas    |
| Busca A*               | Depende da heurística | Exponencial           | Pode ser muito eficiente com boa heurística    |
| Busca em Tabelas Hash  | O(1)                  | O(n)                  | Depende da função hash e tratamento de colisões|

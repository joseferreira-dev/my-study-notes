<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Introdução aos Algoritmos de Ordenação

- [Introdução](#introdução)
- [Tipos de Técnicas de Ordenação](#tipos-de-técnicas-de-ordenação)
- [Terminologia de Algoritmos de Ordenação](#terminologia-de-algoritmos-de-ordenação)
- [Características dos Algoritmos de Ordenação](#características-dos-algoritmos-de-ordenação)
- [Por que os Algoritmos de Ordenação são Importantes?](#por-que-os-algoritmos-de-ordenação-são-importantes)
- [Vantagens e Desvantagens dos Algoritmos de Ordenação](#vantagens-e-desvantagens-dos-algoritmos-de-ordenação)
- [Informações Adicionais sobre Algoritmos de Ordenação](#informações-adicionais-sobre-algoritmos-de-ordenação)

## Introdução

Ordenação refere-se ao rearranjo de um array ou lista de elementos de acordo com um operador de comparação sobre os elementos. O operador de comparação é usado para decidir a nova ordem dos elementos na respectiva estrutura de dados. Ordenar significa reorganizar todos os elementos em ordem crescente ou decrescente.

Este tipo de operação é útil quando temos uma grande quantidade de dados de forma que pode ser difícil lidar com eles, especialmente quando estão dispostos de forma aleatória. Quando isso acontece, ordenar esses dados torna-se crucial, principalmente para facilitar a realização de buscas.

## Tipos de Técnicas de Ordenação

Aqui está uma tabela em Markdown com o resumo das complexidades de tempo dos algoritmos de ordenação mencionados:

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

# Capítulo 8 – Resumo das Complexidades de Algoritmos de Busca e Ordenação

É fundamental compreender a complexidade dos diferentes algoritmos de busca e ordenação, bem como suas vantagens e desvantagens, para escolher a melhor estratégia de acordo com a natureza dos dados, os requisitos de desempenho e o contexto do problema.

Este capítulo apresenta um resumo das principais características desses algoritmos, incluindo suas complexidades de tempo, espaço e se preservam ou não a estabilidade — isto é, se mantêm a ordem dos elementos iguais após a ordenação.

## Complexidade de Algoritmos de Ordenação

A tabela a seguir resume as complexidades dos principais algoritmos de ordenação, destacando o melhor, o caso médio e o pior cenário, além do espaço adicional necessário e se são estáveis.

|**Algoritmo**|**Melhor Caso**|**Caso Médio**|**Pior Caso**|**Espaço Adicional**|**Estável**|
|---|---|---|---|---|---|
|Bubble Sort|O(n)|O(n²)|O(n²)|O(1)|Sim|
|Insertion Sort|O(n)|O(n²)|O(n²)|O(1)|Sim|
|Selection Sort|O(n²)|O(n²)|O(n²)|O(1)|Não|
|Merge Sort|O(n log n)|O(n log n)|O(n log n)|O(n)|Sim|
|Quick Sort|O(n log n)|O(n log n)|O(n²)|O(log n)|Não|
|Heap Sort|O(n log n)|O(n log n)|O(n log n)|O(1)|Não|
|Shell Sort|O(n log n)|O(n<sup>3/2</sup>) ou O(n log² n)|O(n²)|O(1)|Não|
|Comb Sort|O(n log n)|O(n²) ou O(n log n)|O(n²)|O(1)|Não|
|Tim Sort|O(n)|O(n log n)|O(n log n)|O(n)|Sim|
|Counting Sort|O(n + k)|O(n + k)|O(n + k)|O(n + k)|Sim|
|Radix Sort|O(d(n + k))|O(d(n + k))|O(d(n + k))|O(n + k)|Sim|
|Bucket Sort|O(n + k)|O(n + k)|O(n²)|O(n + k)|Sim|

📌 Notas:

- `n` é o número de elementos na entrada.
- `k` é o intervalo dos valores na entrada (Counting, Radix e Bucket Sort).
- `d` é o número de dígitos ou caracteres, no caso de Radix Sort.
- Algoritmos **estáveis** mantêm a ordem relativa dos elementos com chaves iguais.
- Algoritmos como **Merge Sort** e **Tim Sort** são muito usados na prática devido à sua estabilidade e desempenho garantido.

## Complexidade de Algoritmos de Busca

A tabela a seguir resume as principais características dos algoritmos de busca, incluindo suas complexidades, estruturas onde são aplicados, e se é necessário que os dados estejam ordenados.

|**Algoritmo**|**Melhor Caso**|**Caso Médio**|**Pior Caso**|**Estrutura**|**Ordenação Necessária**|**Notas**|
|---|---|---|---|---|---|---|
|Busca Sequencial|O(1)|O(n)|O(n)|Vetor, Lista|Não|Simples, funciona para dados desordenados|
|Busca Binária|O(1)|O(log n)|O(log n)|Vetor|Sim|Estrutura ordenada é obrigatória|
|Busca DFS|O(1)|O(V + E)|O(V + E)|Grafo, Árvore|Não|Boa para explorar profundamente; V = vértices, E = arestas|
|Busca BFS|O(1)|O(V + E)|O(V + E)|Grafo, Árvore|Não|Ideal para encontrar caminho mais curto em termos de passos|
|Busca A*|Depende da heurística|Depende|Exponencial|Grafo com custos|Não|Muito eficiente com heurísticas admissíveis e consistentes|
|Hash|O(1)|O(1)|O(n)|Tabela Hash|Não|Desempenho depende da função hash e do tratamento de colisões|

📌 Notas:

- A busca em hash é extremamente eficiente se a função hash for bem projetada e houver um bom tratamento de colisões (como encadeamento ou endereçamento aberto).
- A busca binária, embora muito eficiente, só pode ser aplicada em dados previamente ordenados.
- Nos algoritmos de grafos, tanto DFS quanto BFS possuem a mesma complexidade, mas são utilizados em contextos diferentes, de acordo com o objetivo da busca.
- A eficiência do algoritmo A* está diretamente ligada à qualidade da heurística. Quando a heurística é bem ajustada, o A* pode encontrar caminhos ótimos rapidamente; caso contrário, seu desempenho pode degradar exponencialmente.

## Considerações Finais

Compreender a complexidade dos algoritmos de busca e ordenação é essencial para qualquer profissional ou estudante de computação. Saber escolher o algoritmo correto pode impactar significativamente no desempenho e na eficiência de um software, especialmente quando se lida com grandes volumes de dados.

Algoritmos simples, como Bubble Sort ou Busca Sequencial, são didáticos e funcionam bem para pequenas quantidades de dados, mas não são eficientes em larga escala. Por outro lado, algoritmos como Merge Sort, Quick Sort (na média) e Tim Sort oferecem soluções robustas e eficientes para ordenação em diferentes cenários.

No contexto de busca, a escolha entre busca sequencial, binária, em grafos ou em tabelas hash depende diretamente da estrutura dos dados, da necessidade de ordenação e das características específicas do problema.

Portanto, mais do que decorar complexidades, é fundamental entender o comportamento de cada algoritmo, suas vantagens, desvantagens e restrições, para tomar decisões conscientes e eficientes no desenvolvimento de soluções computacionais.

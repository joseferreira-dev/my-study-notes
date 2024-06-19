<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/algoritmos-busca-ordenacao"><img src="../../banner-bo.png"></a>
</div>
<br>

# Introdução aos Algoritmos de Ordenação

- [Introdução](#introdução)
- [Tipos de Técnicas de Ordenação](#tipos-de-técnicas-de-ordenação)

## Introdução

Ordenação refere-se ao rearranjo de um array ou lista de elementos de acordo com um operador de comparação sobre os elementos. O operador de comparação é usado para decidir a nova ordem dos elementos na respectiva estrutura de dados. Ordenar significa reorganizar todos os elementos em ordem crescente ou decrescente.

Este tipo de operação é útil quando temos uma grande quantidade de dados de forma que pode ser difícil lidar com eles, especialmente quando estão dispostos de forma aleatória. Quando isso acontece, ordenar esses dados torna-se crucial, principalmente para facilitar a realização de buscas.

## Tipos de Técnicas de Ordenação

Existem vários algoritmos de ordenação usados em estruturas de dados. Eles podem ser classificados em duas categorias considerando o tipo de técnica de ordenação que utilização.

- **Baseados em Comparação**: Comparamos os elementos em um algoritmo de ordenação baseado em comparação (Exemplos: Bubble Sort, Insertion Sort, Selection Sort, Quick Sort, Merge Sort, Heap Sort).
- **Não Baseados em Comparação**: Não comparamos os elementos em um algoritmo de ordenação não baseado em comparação (Exemplos: Counting Sort, Radix Sort).

<!-- 
 
Por Que os Algoritmos de Ordenação São Importantes

O algoritmo de ordenação é importante na Ciência da Computação porque reduz a complexidade de um problema. Há uma ampla gama de aplicações para esses algoritmos, incluindo algoritmos de busca, algoritmos de banco de dados, métodos de divisão e conquista e algoritmos de estrutura de dados.

Nas seções seguintes, listamos algumas aplicações científicas importantes onde os algoritmos de ordenação são usados:

Quando você tem centenas de conjuntos de dados que deseja imprimir, pode querer organizá-los de alguma forma.
Algoritmo de ordenação é usado para arranjar os elementos de uma lista em uma certa ordem (crescente ou decrescente).
Buscar qualquer elemento em um grande conjunto de dados torna-se fácil. Podemos usar o método de busca binária se tivermos dados ordenados. Portanto, a ordenação torna-se importante aqui.
Eles podem ser usados em software e em problemas conceituais para resolver problemas mais avançados.
Vantagens dos Algoritmos de Ordenação
Eficiência: Os algoritmos de ordenação ajudam a organizar os dados em uma ordem específica, facilitando e acelerando a busca, recuperação e análise de informações.
Melhoria no Desempenho: Ao organizar os dados de maneira ordenada, os algoritmos podem realizar operações de forma mais eficiente, levando a um desempenho aprimorado em várias aplicações.
Análise de Dados Simplificada: A ordenação facilita a identificação de padrões e tendências nos dados.
Redução do Consumo de Memória: A ordenação pode ajudar a reduzir o uso de memória ao eliminar elementos duplicados.
Melhoria na Visualização de Dados: Dados ordenados podem ser visualizados de forma mais eficaz em gráficos e tabelas.
Desvantagens dos Algoritmos de Ordenação
Complexidade de Tempo: Os algoritmos de ordenação podem ter alta complexidade de tempo, especialmente para grandes conjuntos de dados.
Complexidade de Espaço: Alguns algoritmos de ordenação requerem espaço de memória adicional para realizar suas operações.
Estabilidade: Alguns algoritmos de ordenação não preservam a ordem original dos elementos iguais.
Seleção do Algoritmo: Escolher o algoritmo de ordenação mais adequado para um determinado conjunto de dados pode ser desafiador.

Terminologia de Ordenação:

Ordenação in-place: Um algoritmo de ordenação in-place usa espaço constante para produzir o resultado (modifica apenas o array fornecido). Ele ordena a lista apenas modificando a ordem dos elementos dentro da lista. Exemplos: Selection Sort, Bubble Sort, Insertion Sort e Heap Sort.

Ordenação Interna: A ordenação interna ocorre quando todos os dados são colocados na memória principal ou memória interna. Na ordenação interna, o problema não pode aceitar entrada além de seu tamanho. Exemplos: Heap Sort, Bubble Sort, Selection Sort, Quick Sort, Shell Sort, Insertion Sort.

Ordenação Externa: A ordenação externa ocorre quando todos os dados que precisam ser ordenados não podem ser colocados na memória de uma vez; a ordenação é chamada de ordenação externa. A ordenação externa é usada para uma quantidade massiva de dados. Exemplos: Merge Sort, Tag Sort, Polyphase Sort, Four Tape Sort, External Radix Sort, etc.

Ordenação Estável: Quando dois dados iguais aparecem na mesma ordem nos dados ordenados sem alterar suas posições, é chamada de ordenação estável. Exemplos: Merge Sort, Insertion Sort, Bubble Sort.

Ordenação Instável: Quando dois dados iguais aparecem em ordem diferente nos dados ordenados, é chamada de ordenação instável. Exemplos: Quick Sort, Heap Sort, Shell Sort.

Características dos Algoritmos de Ordenação:
Complexidade de Tempo: A complexidade de tempo, uma medida de quanto tempo leva para executar um algoritmo, é usada para categorizar os algoritmos de ordenação. O desempenho no pior caso, no caso médio e no melhor caso de um algoritmo de ordenação pode ser usado para quantificar a complexidade do tempo do processo.

Complexidade de Espaço: Os algoritmos de ordenação também têm complexidade de espaço, que é a quantidade de memória necessária para executar o algoritmo.

Estabilidade: Um algoritmo de ordenação é considerado estável se a ordem relativa dos elementos iguais for preservada após a ordenação. Isso é importante em certas aplicações onde a ordem original dos elementos iguais deve ser mantida.

Ordenação In-Place: Um algoritmo de ordenação in-place é aquele que não requer memória adicional para ordenar os dados. Isso é importante quando a memória disponível é limitada ou quando os dados não podem ser movidos.

Adaptatividade: Um algoritmo de ordenação adaptativo é aquele que aproveita a ordem pré-existente nos dados para melhorar o desempenho.

Aplicações dos Algoritmos de Ordenação:
Algoritmos de Busca: A ordenação é frequentemente uma etapa crucial em algoritmos de busca como busca binária e busca ternária, onde os dados precisam estar ordenados antes de buscar por um elemento específico.

Gerenciamento de Dados: Ordenar dados facilita a busca, a recuperação e a análise.

Otimização de Bancos de Dados: A ordenação de dados em bancos de dados melhora o desempenho das consultas.

Aprendizado de Máquina: A ordenação é usada para preparar dados para o treinamento de modelos de aprendizado de máquina.

Análise de Dados: A ordenação ajuda a identificar padrões, tendências e outliers em conjuntos de dados. Ela desempenha um papel vital na análise estatística, modelagem financeira e outros campos orientados a dados.

Sistemas Operacionais: Algoritmos de ordenação são usados em sistemas operacionais para tarefas como agendamento de tarefas, gerenciamento de memória e organização do sistema de arquivos.

Complementando as Informações:

Diversidade de Algoritmos: Existem vários algoritmos de ordenação, cada um com suas próprias vantagens e desvantagens. Por exemplo, o Quick Sort é geralmente rápido para a maioria dos casos práticos, mas pode ter um desempenho ruim no pior caso, enquanto o Merge Sort garante um tempo de execução O(n log n) consistente, mas requer espaço adicional.

Algoritmos Híbridos: Em muitas implementações práticas, combinações de algoritmos são usadas para otimizar a performance. Um exemplo é o Timsort, que é uma combinação de Merge Sort e Insertion Sort e é usado na biblioteca padrão de ordenação de Python.

Ordenação em Paralelo: Com o advento de processadores multicore, algoritmos de ordenação em paralelo, como o Parallel Merge Sort, se tornaram importantes para melhorar a performance em sistemas modernos.

Ordenação em Bases de Dados: Em sistemas de bancos de dados, técnicas como a ordenação baseada em índices e a ordenação em disco são usadas para lidar com grandes volumes de dados que não cabem na memória principal.

Ordenação de Texto e Dados Complexos: Além de números, algoritmos de ordenação são aplicados em strings, estruturas de dados complexas e mesmo em gráficos, onde as propriedades específicas dos dados devem ser levadas em conta para escolher o algoritmo mais eficiente.

Esses detalhes adicionais fornecem um panorama mais completo e aprofundado sobre a teoria e as práticas relacionadas à ordenação, destacando a importância e a aplicação dos algoritmos de ordenação em diversos contextos tecnológicos e científicos.
 -->
# Capítulo 7 – Algoritmos de Busca

Os algoritmos de busca são métodos utilizados em ciência da computação para localizar um item específico ou um conjunto de itens dentro de uma estrutura de dados. Eles são fundamentais em diversos domínios, como bancos de dados, sistemas operacionais, inteligência artificial, jogos, redes de computadores e recuperação de informações.

Existem diferentes tipos de algoritmos de busca, cada um adequado para cenários específicos. Alguns funcionam bem em listas simples, outros em estruturas complexas, como árvores e grafos. A escolha do algoritmo mais apropriado depende de fatores como o tamanho dos dados, se estão ordenados ou não, a estrutura utilizada, os requisitos de desempenho e o custo de memória.

## Busca Sequencial (Linear Search)

Busca Sequencial, também conhecida como **busca linear**, é o algoritmo de busca mais simples. Ele percorre a estrutura de dados (geralmente um array ou lista) de **forma sequencial**, comparando cada elemento com o item de busca até encontrá-lo ou até que todos os elementos tenham sido verificados.

**Funcionamento**:

- Começa no primeiro elemento.
- Compara o elemento atual com o valor buscado.
- Se for igual, retorna a posição do elemento.
- Caso contrário, continua até o final da lista.
- Se não encontrar, informa que o elemento não está presente.

As principais vantagens dessa algoritmo é sua **facilidade de implementação** e o fato de que ele **pode ser usado em listas desordenadas**. Todavia, pode ser **lento para listas longas**, pois verifica cada elemento individualmente.

Complexidade de tempo: 
- **Melhor Caso**: `O(1)` – O elemento está na primeira posição.
- **Pior Caso**: `O(n)` – O elemento está na última posição ou não está presente.
- **Caso Médio**: `O(n/2)` – Em média, o elemento será encontrado na metade da lista, mas isso ainda é `O(n)`.

Implementação (pseudocódigo):

```
Algoritmo Busca_Sequencial
Var
   Lista : vetor[1..N] de inteiro
   Valor, i : inteiro
Início
   Ler Valor
   Para i de 1 até N faça
      Se Lista[i] = Valor então
         Escreva "Encontrado na posição ", i
         Pare
      FimSe
   FimPara
   Escreva "Valor não encontrado"
Fim
```

## Busca Binária

A busca binária é uma técnica eficiente para encontrar um elemento em uma **lista ordenada**. Ela funciona dividindo repetidamente a lista ao meio e comparando o elemento do meio com o alvo. Dependendo da comparação, a busca continua na metade esquerda ou direita.

**Funcionamento**:

- Verifica o elemento do meio.
- Se for igual ao valor procurado, retorna sua posição.
- Se for maior, busca na metade inferior.
- Se for menor, busca na metade superior.
- Repete até encontrar ou esgotar as possibilidades.

Ele é muito **mais rápida que a busca linear para listas grandes**. Contudo, para ser aplicada **os dados precisam estar ordenados** e sua implementação é um pouco mais complexa de implementar corretamente, especialmente para listas muito grandes ou em estruturas de dados complexas.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - O elemento está no meio da lista.
- **Pior Caso**: `O(log n)` - O elemento não está presente ou está em uma extremidade.
- **Caso Médio**: `O(log n)`.

Implementação (pseudocódigo):

```
Algoritmo Busca_Binaria
Var
   Lista : vetor[1..N] de inteiro
   Valor, Inicio, Fim, Meio : inteiro
Início
   Ler Valor
   Inicio ← 1
   Fim ← N
   Enquanto Inicio <= Fim faça
      Meio ← (Inicio + Fim) / 2
      Se Lista[Meio] = Valor então
         Escreva "Encontrado na posição ", Meio
         Pare
      Senão se Valor < Lista[Meio] então
         Fim ← Meio - 1
      Senão
         Inicio ← Meio + 1
      FimSe
   FimEnquanto
   Escreva "Valor não encontrado"
Fim
```

## Busca em Profundidade (DFS – Depth-First Search)

A busca em profundidade (Depth-First Search) é uma técnica de busca usada em **árvores** e **grafos**. Ela **começa em um nó raiz e explora o máximo possível ao longo de cada ramo** antes de retroceder, utilizando, geralmente, uma **pilha** (explícita ou implícita via recursão) para controlar os nós visitados.

**Funcionamento**:

- Parte de um nó inicial.
- Visita um vizinho e segue até o limite.
- Quando não há mais vizinhos não visitados, retrocede.
- Continua até visitar todos os nós ou encontrar o objetivo.

A grande vantagem da busca em profundidade está no seu **baixo consumo de memória** em comparação à busca em largura, **especialmente em grafos ou árvores muito grandes e profundos (esparsos)**. Além disso, ela é relativamente simples de ser implementada, principalmente de forma recursiva. Entretanto, a DFS possui como desvantagem a **possibilidade de ficar presa em caminhos muito longos ou até mesmo em ciclos** (se não houver controle adequado), além de **não garantir a menor distância ou caminho mais curto até o objetivo**, quando isso é um requisito do problema.

Complexidade de tempo:
- **Melhor Caso**: `O(1)` - O elemento é encontrado no primeiro nó visitado.
- **Pior Caso**: `O(V + E)` - Onde `V` é o número de vértices e `E` é o número de arestas.
- **Caso Médio**: `O(V + E)`.

Implementação recursiva (pseudocódigo):

```
Procedimento DFS(Nó)
   Marcar Nó como visitado
   Para cada vizinho de Nó faça
      Se vizinho não está visitado então
         Chamar DFS(vizinho)
      FimSe
   FimPara
FimProcedimento
```

## Busca em Largura (BFS – Breadth-First Search)

A busca em largura (Breadth-First Search) é uma técnica que também opera sobre **árvores** e **grafos**. Diferente da DFS, ela **começa no nó raiz e explora todos os nós vizinhos no nível atual** antes de avançar para o próximo nível, utilizando geralmente uma **fila** para controlar a ordem dos nós a serem visitados.

**Funcionamento**:
- Parte de um nó inicial.
- Visita todos os vizinhos.
- Depois, os vizinhos dos vizinhos, e assim por diante.

Sua principal vantagem é que ela **garante encontrar o caminho mais curto** (em termos de quantidade de arestas) até um nó alvo, quando este existe. Isso a torna extremamente **útil em problemas como navegação em mapas, jogos e redes sociais**. No entanto, sua desvantagem é o **alto consumo de memória**, uma vez que precisa **armazenar todos os nós de um nível antes de prosseguir para o próximo**. Esse consumo pode se tornar **crítico em grafos muito grandes ou com alta ramificação**.

Complexidade de Tempo:
- **Melhor caso**: `O(1)` – Se encontra no nó inicial.
- **Pior Caso**: `O(V + E)`.
- **Caso geral**: `O(V + E)`.

Implementação (psudocódigo):

```plaintext
Procedimento BFS(Nó_Inicial)
   Criar Fila
   Inserir Nó_Inicial na Fila
   Marcar Nó_Inicial como visitado
   Enquanto Fila não está vazia faça
      Nó ← Remover da Fila
      Para cada vizinho de Nó faça
         Se vizinho não está visitado então
            Inserir vizinho na Fila
            Marcar vizinho como visitado
         FimSe
      FimPara
   FimEnquanto
FimProcedimento
```

## Busca A* (A Estrela)

O algoritmo A* é uma **busca informada**, utilizada principalmente para **encontrar o caminho mais curto em grafos**, especialmente quando **há custos associados às arestas**. Combina os benefícios da busca de custo uniforme com uma heurística que estima a distância até o objetivo.

**Funcionamento**:

- Mantém uma lista de nós a serem avaliados (aberta).
- Escolhe o nó com menor custo estimado (`f(n) = g(n) + h(n)`), onde:
    - `g(n)` = custo do início até o nó atual.
    - `h(n)` = heurística, estimativa do custo até o destino.
- Continua até encontrar o destino ou esgotar os nós.

Sua principal vantagem é a **capacidade de encontrar o caminho mais curto de forma mais eficiente** do que a busca em largura pura, especialmente **quando a heurística utilizada é bem projetada e admissível** (não superestima o custo restante). Isso permite que o algoritmo explore menos nós, reduzindo tempo e consumo de recursos. Contudo, a principal desvantagem do A* está diretamente ligada à qualidade da heurística: **se a heurística for mal projetada, o desempenho pode ser significativamente pior**, chegando até a ser exponencial no pior caso. Além disso, o algoritmo **pode consumir muita memória**, pois mantém conjuntos de nós abertos e fechados durante a execução.

Complexidade de Tempo:
- Depende fortemente da heurística utilizada, podendo variar desde `O(log n)` em casos muito favoráveis até `O(b^d)` no pior caso, onde `b` é o fator de ramificação e `d` é a profundidade da solução.

## Busca em Tabelas Hash

A busca em tabelas hash utiliza uma **função hash** para transformar uma chave de busca em um índice de um array, **permitindo acesso direto ao item armazenado**. Esse tipo de busca é **extremamente eficiente quando a distribuição das chaves é bem balanceada**.

**Funcionamento**:
- A chave do elemento é transformada em um índice usando uma função hash.
- O índice aponta diretamente para a posição na tabela onde o valor está armazenado.

Sua **principal vantagem é a velocidade**, já que, no melhor cenário, a busca ocorre em tempo constante `O(1)`. Isso a torna **ideal para aplicações onde são realizadas muitas buscas, inserções e remoções rápidas**, como em **bancos de dados, caches e dicionários**. Entretanto, suas desvantagens incluem a **possibilidade de colisões**, que ocorrem quando duas ou mais chaves são mapeadas para o mesmo índice. Quando isso acontece, **técnicas adicionais precisam ser utilizadas para resolver essas colisões**, o que pode degradar o desempenho. No pior caso, quando todas as chaves colidem em um único índice, a busca pode chegar a `O(n)`. Além disso, **o design de uma boa função hash é um desafio**, pois ela precisa distribuir uniformemente as chaves.

Complexidade de Tempo:

- Melhor caso: `O(1)` – Acesso direto.
- Pior caso: `O(n)` – Todos os elementos colidem (cenário raro, mas possível).

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

Ao longo deste capítulo, estudamos diversos algoritmos de busca, cada um com suas características, vantagens, limitações e aplicações específicas. Fica evidente que não existe um algoritmo universalmente melhor, mas sim algoritmos mais adequados para determinados tipos de problemas e estruturas de dados.

A busca sequencial destaca-se pela simplicidade, sendo uma excelente escolha quando se trabalha com pequenas coleções de dados não ordenados. Já a busca binária oferece uma performance muito superior em listas ordenadas, sendo extremamente eficiente quando as condições para seu uso são atendidas.

Nas estruturas mais complexas, como árvores e grafos, as buscas em profundidade (DFS) e em largura (BFS) desempenham papéis fundamentais. Enquanto a DFS é mais econômica em termos de memória e útil para explorar profundamente caminhos, a BFS é indispensável quando o problema exige encontrar o caminho mais curto em termos de passos ou níveis.

Por sua vez, o algoritmo A* surge como uma poderosa ferramenta quando se busca não apenas encontrar um destino, mas fazê-lo de forma otimizada, equilibrando custo real e estimativas heurísticas. É amplamente utilizado em áreas como inteligência artificial, jogos, navegação e planejamento de rotas.

Por fim, a busca em tabelas hash representa uma das soluções mais eficientes em termos de tempo para problemas que envolvem grandes volumes de dados, desde que uma boa função hash seja empregada e as colisões sejam bem tratadas.

Portanto, a escolha do algoritmo de busca não deve ser feita de forma arbitrária, mas sim considerando cuidadosamente fatores como a natureza dos dados (ordenados ou não, lineares ou hierárquicos), os requisitos de desempenho (tempo e espaço) e as características específicas do problema. Este entendimento é fundamental para o desenvolvimento de softwares mais eficientes, robustos e alinhados às necessidades reais das aplicações.
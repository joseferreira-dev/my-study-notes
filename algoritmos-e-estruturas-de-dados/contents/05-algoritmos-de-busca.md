# Capítulo 5 – Algoritmos de Busca

Nos capítulos anteriores, dedicamos um esforço considerável para entender, implementar e analisar algoritmos de ordenação. Estabelecemos por que organizar dados é crucial e exploramos os _trade-offs_ entre os métodos $O(n²)$, $O(n \log n)$ e $O(n)$. Agora, estamos prestes a colher os frutos desse trabalho. A ordenação, embora útil por si só, tem como um de seus principais objetivos viabilizar a **busca** eficiente. Algoritmos de busca são métodos computacionais projetados para localizar um item específico, ou um conjunto de itens, dentro de uma estrutura de dados. Eles são a espinha dorsal de incontáveis aplicações, desde a consulta a um banco de dados, a busca por um arquivo em um sistema operacional, até a rota em um GPS ou o rastreamento de conexões em uma rede social.

A escolha do algoritmo de busca ideal é um dos problemas clássicos da Ciência da Computação. A eficiência não depende apenas do algoritmo em si, mas está intrinsecamente ligada à forma como os dados estão organizados. Como veremos, uma busca em uma lista caótica é fundamentalmente diferente de uma busca em uma lista ordenada. Este capítulo explorará os principais algoritmos de busca, desde os mais simples em listas lineares até os mais complexos em grafos e tabelas especializadas.

## Busca em Estruturas Lineares

A forma mais comum de busca ocorre em estruturas de dados lineares, como vetores (arrays) ou listas. A estratégia de busca muda drasticamente dependendo de uma única condição: a lista está ou não ordenada?

### Busca Sequencial (Linear Search)

A Busca Sequencial, também conhecida como **busca linear**, é o algoritmo de busca mais simples, direto e intuitivo. É o método de "força bruta" que usamos naturalmente quando procuramos um livro em uma estante completamente desorganizada. O algoritmo simplesmente percorre a estrutura de dados, elemento por elemento, desde o início, comparando cada item com o valor que estamos procurando.

**Como Funciona:**

1. Começa no primeiro elemento da lista (índice 0).
2. Compara o elemento atual com o valor buscado.
3. Se forem iguais, o algoritmo encontrou o item e retorna sua posição (índice).
4. Se não forem iguais, ele avança para o próximo elemento.
5. O processo se repete até que o item seja encontrado ou até que a lista termine.
6. Se o algoritmo chegar ao final da lista sem encontrar o item, ele informa que o elemento não está presente (geralmente retornando um valor sentinela, como -1).

A grande vantagem da busca sequencial é sua simplicidade e flexibilidade. Ela é **extremamente fácil de implementar** e, o mais importante, **funciona em listas desordenadas**, não exigindo qualquer preparação prévia dos dados. Sua desvantagem é a eficiência: em listas longas, ela pode ser **dolorosamente lenta**, pois, no pior caso, precisa verificar cada elemento.

**Pseudocódigo:**

```
função BuscaSequencial(vetor, valorBuscado)
    n = tamanho(vetor)
    para i de 0 até n-1
        se vetor[i] == valorBuscado então
            // Encontrado! Retorna a posição (índice).
            retornar i
        fim_se
    fim_para
    
    // Não foi encontrado após percorrer todo o vetor.
    retornar -1
fim_função
```

**Boletim da Busca Sequencial:**

- **Complexidade de Tempo (Melhor Caso):** **$O(1)$**. Ocorre quando o elemento procurado é o primeiro da lista.
- **Complexidade de Tempo (Pior Caso):** **$O(n)$**. Ocorre quando o elemento é o último da lista ou não está presente nela.
- **Complexidade de Tempo (Caso Médio):** **$O(n)$**. Em média, o algoritmo precisará percorrer metade da lista ($n/2$), o que, assintoticamente, ainda é $O(n)$.
- **Complexidade de Espaço:** **$O(1)$** (não requer memória adicional).
- **Pré-requisito:** Nenhum. Funciona em dados não ordenados.

### Busca Binária (Binary Search)

A busca binária é a principal recompensa por termos ordenado nossos dados. Este algoritmo é uma técnica de busca dramaticamente eficiente que só funciona em **listas previamente ordenadas**. Sua lógica é a mesma que usamos para encontrar uma palavra em um dicionário ou um nome em uma lista telefônica: em vez de começar do início, abrimos no meio.

**Como Funciona:**

A busca binária funciona eliminando _metade_ do universo de busca a cada comparação.

1. O algoritmo define três "ponteiros": `inicio` (para o índice 0), `fim` (para o índice `n-1`) e `meio`.
2. Enquanto inicio for menor ou igual a fim:
    1. Calcula-se o índice meio (`(inicio + fim) / 2`).
    2. O elemento `vetor[meio]` é comparado com o valor buscado.
    3. Caso 1 (Encontrou): Se `vetor[meio]` for igual ao valor, a busca termina com sucesso, retornando meio.
    4. Caso 2 (Valor é menor): Se o valor buscado for menor que `vetor[meio]`, significa que o item só pode estar na metade esquerda. O algoritmo descarta a metade direita, atualizando `fim = meio - 1`.
    5. Caso 3 (Valor é maior): Se o valor buscado for maior que `vetor[meio]`, o item só pode estar na metade direita. O algoritmo descarta a metade esquerda, atualizando `inicio = meio + 1`.
3. Se o loop terminar (ou seja, `inicio` ultrapassar `fim`), significa que o elemento não está na lista.

A eficiência da busca binária é extraordinária. Em uma lista de 1 milhão de elementos ($2^{20}$), ela encontra qualquer item em, no máximo, 20 comparações. Em uma lista de 1 bilhão ($2^{30}$), em no máximo 30. Isso é uma melhoria exponencial sobre os 1 bilhão de comparações do pior caso da busca sequencial.

**Pseudocódigo (Versão Iterativa):**

```
função BuscaBinaria(vetorOrdenado, valorBuscado)
    inicio = 0
    fim = tamanho(vetorOrdenado) - 1
    
    enquanto inicio <= fim
        meio = floor((inicio + fim) / 2) // Pega o índice do meio
        
        se vetorOrdenado[meio] == valorBuscado então
            // Encontrado!
            retornar meio
        senão se valorBuscado < vetorOrdenado[meio] então
            // Busca na metade esquerda
            fim = meio - 1
        senão
            // Busca na metade direita
            inicio = meio + 1
        fim_se
    fim_enquanto
    
    // Não foi encontrado.
    retornar -1
fim_função
```

**Boletim da Busca Binária:**

- **Complexidade de Tempo (Melhor Caso):** **$O(1)$**. Ocorre quando o elemento procurado é exatamente o do meio na primeira tentativa.
- **Complexidade de Tempo (Pior Caso):** **$O(\log n)$**. O número de comparações cresce logaritmicamente com o tamanho da lista.
- **Complexidade de Tempo (Caso Médio):** **$O(\log n)$**.
- **Complexidade de Espaço:** **$O(1)$** (para a versão iterativa).
- **Pré-requisito:** A lista **deve** estar ordenada.

## Busca em Estruturas Não-Lineares

Muitas vezes, os dados não estão em uma lista simples, mas sim em estruturas complexas que representam redes, hierarquias ou conexões. Nesses casos, usamos algoritmos de travessia (travessia) de grafos e árvores.

### Busca em Profundidade (DFS – Depth-First Search)

A Busca em Profundidade é uma estratégia de travessia que explora o "mais fundo" possível antes de voltar atrás. É como explorar um labirinto seguindo sempre a primeira parede à sua direita: você mergulha em um caminho até bater em um beco sem saída, e só então você retrocede (faz _backtracking_) para tentar o próximo caminho disponível.

A DFS utiliza uma **pilha** (Pilha - LIFO: Último a Entrar, Primeiro a Sair) para gerenciar os nós a visitar. Na implementação recursiva, a própria pilha de chamadas de função do sistema faz esse papel.

**Como Funciona:**

1. Começa em um nó inicial, o marca como "visitado" e o empilha.
2. Olha para o nó no topo da pilha.
3. Visita um vizinho _não visitado_ desse nó, o marca como "visitado" e o empilha.
4. Repete o passo 2. O algoritmo sempre "mergulha" no vizinho mais recente.
5. Se o nó no topo da pilha não tem mais vizinhos não visitados, ele é "desempilhado" (o algoritmo retrocede).
6. O processo continua até que a pilha esteja vazia (todos os nós alcançáveis foram visitados) ou o item seja encontrado.

A DFS é excelente para problemas como detecção de ciclos em grafos ou para encontrar um caminho (não necessariamente o mais curto) entre dois nós. Ela tende a usar **menos memória** que a BFS em grafos "largos", pois armazena apenas o caminho atual, mas pode ficar presa em caminhos muito longos e não garante o caminho mais curto.

**Pseudocódigo (Recursivo):**

```
conjunto visitados

procedimento DFS(no)
    marcar no como visitado
    
    // Processar o nó (ex: verificar se é o item buscado)
    
    para cada vizinho de no
        se vizinho não está em visitados então
            DFS(vizinho) // Mergulha recursivamente
        fim_se
    fim_para
fim_procedimento

// Chamada inicial
DFS(no_raiz)
```

**Boletim da Busca em Profundidade:**

- **Complexidade de Tempo:** **$O(V + E)$**. Onde $V$ é o número de vértices (nós) e $E$ é o número de arestas (conexões). O algoritmo visita cada nó e cada aresta uma vez.
- **Complexidade de Espaço:** **$O(V)$** (pior caso, para armazenar a pilha de recursão em um grafo tipo "linha").
- **Pré-requisito:** Estrutura de Grafo ou Árvore.

### Busca em Largura (BFS – Breadth-First Search)

A Busca em Largura adota a estratégia oposta. Em vez de "mergulhar", ela explora de forma "larga", nível por nível. É como jogar uma pedra em um lago: a busca se expande em ondas concêntricas a partir do ponto inicial. Ela visita o nó inicial, depois _todos_ os seus vizinhos diretos (nível 1), depois _todos_ os vizinhos dos vizinhos (nível 2), e assim por diante.

Para conseguir essa ordem, a BFS utiliza uma **fila** (Fila - FIFO: Primeiro a Entrar, Primeiro a Sair).

**Como Funciona:**

1. Começa em um nó inicial, o marca como "visitado" e o enfileira (adiciona à fila).
2. Enquanto a fila não estiver vazia:
    1. Desenfileira um nó (remove da frente da fila).
    2. Processa esse nó.
    3. Para cada vizinho não visitado desse nó:
	    a. Marca o vizinho como "visitado".
	    b. Enfileira o vizinho (adiciona ao fim da fila).

A propriedade mais importante da BFS é que ela **garante encontrar o caminho mais curto** (em termos de número de arestas ou níveis) entre o nó inicial e qualquer outro nó. Isso a torna ideal para problemas como encontrar a rota mais curta em um mapa (com custos iguais) ou encontrar o "grau de separação" em uma rede social. Sua desvantagem é o **alto consumo de memória**, pois a fila pode ter que armazenar todos os nós de um nível inteiro, o que pode ser massivo em grafos com alta ramificação.

**Pseudocódigo:**

```
procedimento BFS(no_inicial)
    fila = nova Fila()
    conjunto visitados
    
    fila.enfileirar(no_inicial)
    visitados.adicionar(no_inicial)
    
    enquanto fila não está vazia
        no_atual = fila.desenfileirar()
        
        // Processar o no_atual
        
        para cada vizinho de no_atual
            se vizinho não está em visitados então
                visitados.adicionar(vizinho)
                fila.enfileirar(vizinho)
            fim_se
        fim_para
    fim_enquanto
fim_procedimento
```

**Boletim da Busca em Largura:**

- **Complexidade de Tempo:** **$O(V + E)$**. Assim como a DFS, visita cada nó e aresta uma vez.
- **Complexidade de Espaço:** **$O(V)$** (pior caso, a fila pode conter todos os nós de um nível, que no limite pode ser $V$).
- **Pré-requisito:** Estrutura de Grafo ou Árvore.

## Busca Heurística (Informada)

DFS e BFS são buscas "cegas": elas exploram sistematicamente sem nenhuma inteligência sobre _onde_ o objetivo pode estar. As buscas informadas, por outro lado, usam "pistas" (heurísticas) para guiar a busca em direção ao objetivo.

### Busca A\* (A Estrela)

O algoritmo A* (lê-se "A Estrela") é o algoritmo de busca informada mais famoso e amplamente utilizado, especialmente para **encontrar o caminho mais curto em grafos com custos** (onde cada aresta/conexão tem um "peso" ou "distância"). É o algoritmo por trás de muitos sistemas de GPS e IA de jogos.

O A* combina o melhor da BFS (garantia de caminho mais curto) com uma heurística inteligente. Ele decide qual nó explorar a seguir com base em uma função de custo:

$f(n) = g(n) + h(n)$

Onde:

- **$n$** é o próximo nó a ser considerado.
- **$g(n)$** é o **custo real** do caminho percorrido desde o início até o nó $n$.
- **$h(n)$** é a **heurística**, uma _estimativa_ (um "palpite" inteligente) do custo do nó $n$ até o objetivo. Em um GPS, $h(n)$ seria a distância em linha reta da cidade $n$ até o destino.

O A* explora o nó que tem o menor valor $f(n)$, balanceando inteligentemente o custo já pago ($g$) com o custo futuro estimado ($h$).

A principal vantagem do A* é sua **eficiência em encontrar o caminho mais curto**. Se a heurística $h(n)$ for "admissível" (nunca superestimar o custo real), o A* _garante_ encontrar o caminho de menor custo, e o faz explorando muito menos nós do que uma busca "cega". Sua desvantagem é que seu desempenho depende _totalmente_ da qualidade da heurística e pode consumir **muita memória** para manter o registro dos nós abertos e fechados.

**Boletim da Busca A\*:**

- **Complexidade de Tempo:** Depende da heurística. No pior caso (heurística ruim), pode ser exponencial. Com uma heurística perfeita, é quase linear.
- **Complexidade de Espaço:** Depende da heurística, mas pode ser alta, pois armazena muitos nós.
- **Pré-requisito:** Grafo com custos nas arestas e uma função de heurística admissível.

## Busca Otimizada por Chave

Esta categoria de busca não percorre a estrutura. Em vez disso, ela usa uma propriedade da chave de busca para _calcular_ a localização do dado, idealmente em uma única etapa.

### Busca em Tabelas Hash (Hash Tables)

A busca em Tabela Hash (ou Tabela de Dispersão) é uma técnica que oferece um desempenho de busca fenomenal. A ideia central é usar uma **função hash** — uma função matemática — para transformar a própria chave de busca (seja uma string, um número, etc.) em um **índice** de um vetor. Esse índice aponta _diretamente_ para a posição onde o item deveria estar.

- **Inserção:** `indice = funcao_hash(chave)`; `tabela[indice] = valor`;
- **Busca:** `indice = funcao_hash(chave)`; `retornar tabela[indice]`;

Em um cenário ideal, isso permite que a inserção, a remoção e a busca de elementos ocorram em **tempo constante $O(1)$**.

O desafio central das Tabelas Hash é a **colisão**: o que acontece quando a função hash mapeia duas chaves diferentes para o _mesmo_ índice? Existem várias técnicas de tratamento de colisão (como encadeamento, onde cada índice aponta para uma lista, ou endereçamento aberto, onde se procura o próximo espaço vazio).

O desempenho da tabela hash depende inteiramente da qualidade da função hash (que deve espalhar as chaves uniformemente) e de quão bem as colisões são gerenciadas. Em um cenário de muitas colisões, o desempenho pode degradar, no pior caso, para $O(n)$, se todas as chaves caírem no mesmo índice.

**Boletim da Busca em Tabela Hash:**

- **Complexidade de Tempo (Melhor Caso):** **$O(1)$** (acesso direto).
- **Complexidade de Tempo (Pior Caso):** **$O(n)$** (todas as chaves colidem).
- **Complexidade de Tempo (Caso Médio):** **$O(1)$** (com boa função hash e tratamento).
- **Complexidade de Espaço:** **$O(n)$** (para armazenar a tabela).
- **Pré-requisito:** Uma boa função hash e uma estratégia de tratamento de colisões.

## Tabela Comparativa de Algoritmos de Busca

A tabela a seguir resume as características dos algoritmos que estudamos, permitindo uma comparação direta.

|**Algoritmo**|**Melhor Caso**|**Caso Médio**|**Pior Caso**|**Estrutura**|**Ordenação Necessária**|**Notas**|
|---|---|---|---|---|---|---|
|**Busca Sequencial**|$O(1)$|$O(n)$|$O(n)$|Vetor, Lista|Não|Simples, funciona para dados desordenados.|
|**Busca Binária**|$O(1)$|$O(\log n)$|$O(\log n)$|Vetor|**Sim**|Estrutura ordenada é obrigatória.|
|**Busca DFS**|$O(1)$|$O(V + E)$|$O(V + E)$|Grafo, Árvore|Não|Boa para explorar profundamente; V = vértices, E = arestas.|
|**Busca BFS**|$O(1)$|$O(V + E)$|$O(V + E)$|Grafo, Árvore|Não|Ideal para encontrar caminho mais curto em termos de passos.|
|**Busca A***|Depende|Depende|Exponencial|Grafo com custos|Não|Eficiente com heurísticas admissíveis.|
|**Hash**|$O(1)$|$O(1)$|$O(n)$|Tabela Hash|Não|Desempenho depende da função hash e colisões.|

## Considerações Finais

Ao longo deste capítulo, exploramos um leque de algoritmos de busca, cada um com suas próprias forças, fraquezas e contextos de aplicação. A conclusão mais importante é que **não existe um algoritmo de busca universalmente superior**; a escolha correta é ditada pelo problema.

A busca sequencial, com sua simplicidade, é perfeitamente aceitável para coleções pequenas ou não ordenadas. A busca binária, por sua vez, demonstra o poder imenso da ordenação prévia, oferecendo desempenho logarítmico em listas ordenadas.

Quando os dados se tornam redes interconectadas (grafos), a DFS e a BFS oferecem duas estratégias fundamentais de travessia. A DFS, com sua natureza de "mergulho", é ideal para explorar caminhos e economizar memória, enquanto a BFS, com sua exploração em "ondas", é imbatível para encontrar o caminho mais curto em termos de níveis.

A busca A* eleva o nível, introduzindo "inteligência" (heurísticas) para encontrar caminhos ótimos em grafos com custos, sendo uma ferramenta essencial em IA e logística. Por fim, a busca em Tabela Hash nos mostra a possibilidade teórica do acesso instantâneo, $O(1)$, um pilar dos sistemas de cache e bancos de dados de alta performance.

Compreender o _trade-off_ entre a estrutura dos dados (ordenados ou não), os requisitos de desempenho (tempo de busca vs. tempo de preparação) e os recursos (custo de memória) é uma habilidade central no desenvolvimento de software eficiente e robusto.
# Capítulo 3 – Principais Algoritmos de Ordenação (Parte 1)

Após estabelecermos as métricas e o vocabulário para analisar algoritmos no capítulo anterior, estamos prontos para dissecar os algoritmos de ordenação em si. Este capítulo é uma jornada que nos levará dos métodos mais simples e intuitivos aos mais complexos e eficientes, que formam a base da computação moderna. Iniciaremos com os algoritmos quadráticos — **Bubble Sort**, **Insertion Sort** e **Selection Sort** — que, apesar de sua ineficiência para grandes conjuntos de dados, são academicamente fundamentais para a construção do raciocínio algorítmico. Em seguida, faremos a transição para os algoritmos de elite, a família $O(n \log n)$ e similares — **Merge Sort**, **Quick Sort**, **Heap Sort** e **Shell Sort** — que são os verdadeiros "cavalos de batalha" usados em sistemas de software de alto desempenho.

## Algoritmos Quadráticos Simples

O primeiro grupo de algoritmos que exploramos é frequentemente chamado de "quadrático" porque sua complexidade de tempo, tanto no caso médio quanto no pior caso, é de **$O(n²)$**. Isso significa que seu tempo de execução cresce exponencialmente em relação ao tamanho da entrada (dobrar a entrada quadruplica o tempo). Embora ineficientes para listas grandes, eles são extremamente valiosos para fins didáticos, pois sua lógica é fácil de visualizar e implementar.

### Bubble Sort (Ordenação por Flutuação)

O Bubble Sort é talvez o algoritmo de ordenação mais simples e intuitivo de se conceber. Sua lógica é baseada na ideia de "flutuação": ele percorre a lista várias vezes, comparando pares de elementos adjacentes e trocando-os se estiverem na ordem errada. Em cada passagem, o maior elemento "flutua" (ou "borbulha") para sua posição correta no final da lista.

**Como Funciona:**

1. **Inicialização:** O algoritmo inicia no primeiro elemento.
2. **Comparação e Troca:** Ele compara o elemento atual com o próximo (`vetor[i]` com `vetor[i+1]`).
3. Se o elemento atual for maior que o próximo, eles são trocados.
4. O algoritmo avança para o próximo par (agora `vetor[i+1]` e `vetor[i+2]`) e repete o processo.
5. **Fim da Passagem:** Ao final da primeira passagem completa pela lista, o **maior** elemento estará garantidamente na **última** posição.
6. **Iteração:** O processo é repetido para o restante da lista (da posição 0 até $n-2$), depois da 0 até $n-3$, e assim por diante, até que a lista inteira esteja ordenada.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

Observe como o maior elemento de cada passagem "flutua" para o final:

**Passagem 1 (n = 5):**

- `[**5**, **3**, 8, 4, 2]` -> `(5 > 3)` Troca. -> `[3, 5, 8, 4, 2]`
- `[3, **5**, **8**, 4, 2]` -> `(5 < 8)` Não troca. -> `[3, 5, 8, 4, 2]`
- `[3, 5, **8**, **4**, 2]` -> `(8 > 4)` Troca. -> `[3, 5, 4, 8, 2]`
- `[3, 5, 4, **8**, **2**]` -> `(8 > 2)` Troca. -> `[3, 5, 4, 2, **8**]`
- _Fim da Passagem 1. O `8` está na posição correta._

**Passagem 2 (n = 4):**

- `[**3**, **5**, 4, 2, 8]` -> `(3 < 5)` Não troca. -> `[3, 5, 4, 2, 8]`
- `[3, **5**, **4**, 2, 8]` -> `(5 > 4)` Troca. -> `[3, 4, 5, 2, 8]`
- `[3, 4, **5**, **2**, 8]` -> `(5 > 2)` Troca. -> `[3, 4, 2, **5**, 8]`
- _Fim da Passagem 2. O `5` está na posição correta._

**Passagem 3 (n = 3):**

- `[**3**, **4**, 2, 5, 8]` -> `(3 < 4)` Não troca. -> `[3, 4, 2, 5, 8]`
- `[3, **4**, **2**, 5, 8]` -> `(4 > 2)` Troca. -> `[3, 2, **4**, 5, 8]`
- _Fim da Passagem 3. O `4` está na posição correta._

**Passagem 4 (n = 2):**

- `[**3**, **2**, 4, 5, 8]` -> `(3 > 2)` Troca. -> `[2, **3**, 4, 5, 8]`
- _Fim da Passagem 4. O `3` está na posição correta._

A lista final é `[2, 3, 4, 5, 8]`.

**Otimização:** O algoritmo acima sempre executa $n-1$ passagens. Podemos otimizá-lo parando mais cedo. Se em uma passagem completa nenhuma troca for realizada, significa que a lista já está ordenada e podemos encerrar o algoritmo.

**Pseudocódigo (Com Otimização):**

```
função BubbleSort(vetor)
    n = tamanho(vetor)
    repetir
        houveTroca = falso
        para i de 0 até n-2
            se vetor[i] > vetor[i+1]
                trocar(vetor[i], vetor[i+1])
                houveTroca = verdadeiro
            fim_se
        fim_para
        n = n - 1 // O último elemento já está correto
    até que houveTroca == falso
    retornar vetor
fim_função
```

**Boletim do Bubble Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**. Ocorre quando a lista está em ordem inversa.
- **Complexidade de Tempo (Melhor Caso):** **$O(n)$**. Ocorre quando a lista já está ordenada (usando a versão otimizada, ele faz uma passagem, não vê trocas e para).
- **Complexidade de Tempo (Caso Médio):** **$O(n²)$**.
- **Complexidade de Espaço:** **$O(1)$**. É um algoritmo _in-place_.
- **Estabilidade:** **Sim**. Como ele só troca elementos adjacentes, ele nunca saltará um elemento igual sobre o outro, preservando a ordem relativa.

### Insertion Sort (Ordenação por Inserção)

O Insertion Sort é outro algoritmo intuitivo, cuja lógica simula a forma como muitas pessoas organizam cartas de baralho em suas mãos. O algoritmo divide a lista em duas partes: uma sublista ordenada (que começa vazia, à esquerda) e uma sublista não ordenada (à direita). A cada passo, ele "pega" o próximo elemento da sublista não ordenada e o insere em sua posição correta dentro da sublista já ordenada.

**Como Funciona:**

1. **Inicialização:** O algoritmo considera o primeiro elemento (`vetor[0]`) como a sublista ordenada inicial (uma lista de um elemento já está ordenada).
2. **Iteração:** O algoritmo começa no segundo elemento (`vetor[1]`).
3. **Seleção e Deslocamento:** O elemento atual (a "carta" que pegamos) é armazenado em uma variável temporária (`chave`).
4. Compara-se a `chave` com os elementos à sua esquerda (na sublista ordenada), um por um, da direita para a esquerda.
5. Se um elemento da sublista ordenada for maior que a `chave`, ele é deslocado uma posição para a direita, abrindo espaço.
6. **Inserção:** O processo 5 se repete até que se encontre um elemento menor ou igual à `chave`, ou até que se chegue ao início da lista. A `chave` é então inserida no "buraco" (espaço) criado.
7. O algoritmo passa para o próximo elemento da sublista não ordenada e repete o processo.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

**Passagem 1 (Elemento `3`):**

- Sublista ordenada: `[5]`. Chave = `3`.
- Compara `3` com `5`. `(3 < 5)`.
- Desloca `5` para a direita: `[5, 5, 8, 4, 2]`
- Insere `3` no "buraco": `[**3**, **5**, 8, 4, 2]`

**Passagem 2 (Elemento `8`):**

- Sublista ordenada: `[3, 5]`. Chave = `8`.
- Compara `8` com `5`. `(8 > 5)`. A posição correta foi encontrada.
- Insere `8` (ele já está lá): `[3, 5, **8**, 4, 2]`

**Passagem 3 (Elemento `4`):**

- Sublista ordenada: `[3, 5, 8]`. Chave = `4`.
- Compara `4` com `8`. `(4 < 8)`. Desloca `8`: `[3, 5, 8, 8, 2]`
- Compara `4` com `5`. `(4 < 5)`. Desloca `5`: `[3, 5, 5, 8, 2]`
- Compara `4` com `3`. `(4 > 3)`. Posição encontrada.
- Insere `4` no "buraco": `[3, **4**, **5**, 8, 2]`

**Passagem 4 (Elemento `2`):**

- Sublista ordenada: `[3, 4, 5, 8]`. Chave = `2`.
- Compara `2` com `8`. `(2 < 8)`. Desloca `8`: `[3, 4, 5, 8, 8]`
- Compara `2` com `5`. `(2 < 5)`. Desloca `5`: `[3, 4, 5, 5, 8]`
- Compara `2` com `4`. `(2 < 4)`. Desloca `4`: `[3, 4, 4, 5, 8]`
- Compara `2` com `3`. `(2 < 3)`. Desloca `3`: `[3, 3, 4, 5, 8]`
- Chegou ao início.
- Insere `2` no "buraco": `[**2**, **3**, 4, 5, 8]`

Lista final: `[2, 3, 4, 5, 8]`.

**Pseudocódigo:**

```
função InsertionSort(vetor)
    n = tamanho(vetor)
    para i de 1 até n-1
        chave = vetor[i]
        j = i - 1
        
        // Desloca os elementos maiores que a chave para a direita
        enquanto j >= 0 e vetor[j] > chave
            vetor[j+1] = vetor[j]
            j = j - 1
        fim_enquanto
        
        vetor[j+1] = chave // Insere a chave na posição correta
    fim_para
    retornar vetor
fim_função
```

**Boletim do Insertion Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**. Ocorre quando a lista está em ordem inversa (cada elemento tem que ser deslocado até o início).
- **Complexidade de Tempo (Melhor Caso):** **$O(n)$**. Ocorre quando a lista já está ordenada. O loop `enquanto` interno nunca é executado. Isso faz dele um algoritmo **adaptativo**.
- **Complexidade de Tempo (Caso Médio):** **$O(n²)$**.
- **Complexidade de Espaço:** **$O(1)$**. É um algoritmo _in-place_.
- **Estabilidade:** **Sim**. Ao procurar a posição de inserção, ele para no primeiro elemento _igual_ (ou menor), garantindo que não ultrapasse um elemento de mesmo valor, preservando a ordem.

### Selection Sort (Ordenação por Seleção)

O Selection Sort é o terceiro algoritmo simples, e sua lógica é baseada em "seleção". Ele também divide a lista em uma parte ordenada (à esquerda) e uma não ordenada (à direita). A cada passagem, ele varre a sublista _não ordenada_ para encontrar o **menor** elemento. Uma vez encontrado, ele o troca com o primeiro elemento da sublista não ordenada. Isso "trava" o menor elemento em sua posição final correta.

**Como Funciona:**

1. **Inicialização:** A parte ordenada começa vazia. A parte não ordenada é a lista inteira.
2. **Seleção:** O algoritmo varre a parte não ordenada (de `i` até `n-1`) para encontrar o índice do menor elemento (`indiceMenor`).
3. **Troca:** Após encontrar o menor elemento, ele o troca com o elemento na posição `i` (o início da parte não ordenada).
4. **Iteração:** O limite da parte ordenada avança uma posição para a direita (o elemento em `i` agora está ordenado). O processo se repete $n-1$ vezes.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

**Passagem 1 (i = 0):**

- Lista: `[5, 3, 8, 4, 2]`
- Varre `[5, 3, 8, 4, 2]`. Menor elemento é `2`, no índice 4.
- Troca `vetor[0]` (que é 5) com `vetor[4]` (que é 2).
- Lista: `[**2**, 3, 8, 4, 5]`
- _Parte ordenada: `[2]`_

**Passagem 2 (i = 1):**

- Lista: `[2, 3, 8, 4, 5]`
- Varre `[3, 8, 4, 5]`. Menor elemento é `3`, no índice 1.
- Troca `vetor[1]` (que é 3) com `vetor[1]` (que é 3). (Nenhuma mudança real).
- Lista: `[2, **3**, 8, 4, 5]`
- _Parte ordenada: `[2, 3]`_

**Passagem 3 (i = 2):**

- Lista: `[2, 3, 8, 4, 5]`
- Varre `[8, 4, 5]`. Menor elemento é `4`, no índice 3.
- Troca `vetor[2]` (que é 8) com `vetor[3]` (que é 4).
- Lista: `[2, 3, **4**, 8, 5]`
- _Parte ordenada: `[2, 3, 4]`_

**Passagem 4 (i = 3):**

- Lista: `[2, 3, 4, 8, 5]`
- Varre `[8, 5]`. Menor elemento é `5`, no índice 4.
- Troca `vetor[3]` (que é 8) com `vetor[4]` (que é 5).
- Lista: `[2, 3, 4, **5**, 8]`
- _Parte ordenada: `[2, 3, 4, 5]`_

A última passagem (i=4) é desnecessária, pois o último elemento (`8`) já estará na posição correta. Lista final: `[2, 3, 4, 5, 8]`.

**Pseudocódigo:**

```
função SelectionSort(vetor)
    n = tamanho(vetor)
    para i de 0 até n-2
        // Encontra o índice do menor elemento na parte não ordenada
        indiceMenor = i
        para j de i+1 até n-1
            se vetor[j] < vetor[indiceMenor]
                indiceMenor = j
            fim_se
        fim_para
        
        // Troca o menor elemento encontrado com o primeiro elemento da parte não ordenada
        trocar(vetor[i], vetor[indiceMenor])
    fim_para
    retornar vetor
fim_função
```

**Boletim do Selection Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**.
- **Complexidade de Tempo (Melhor Caso):** **$O(n²)$**. O algoritmo não é adaptativo. Ele _sempre_ fará a varredura completa para encontrar o menor, mesmo que a lista já esteja ordenada.
- **Complexidade de Tempo (Caso Médio):** **$O(n²)$**.
- **Complexidade de Espaço:** **$O(1)$**. É um algoritmo _in-place_.
- **Estabilidade:** **Não**. Na implementação padrão, ele não é estável. Considere `[5A, 3, 5B, 2]`. Na primeira passagem, ele encontra o `2` e o troca com o `5A`, resultando em `[2, 3, 5B, 5A]`. A ordem original de `5A` e `5B` foi invertida.

Uma característica única do Selection Sort é que ele minimiza o número de trocas (no máximo $n-1$ trocas). Isso pode ser vantajoso em cenários onde a operação de escrita na memória é muito mais custosa que a de leitura.

## Algoritmos Eficientes

Embora os algoritmos quadráticos sejam simples, sua ineficiência os torna inviáveis para conjuntos de dados que ultrapassam alguns milhares de elementos. Para resolver problemas em escala, recorremos a algoritmos mais avançados, cuja complexidade de tempo média é de **$O(n \log n)$**. Essa classe de algoritmos é drasticamente mais rápida. Para ordenar 1 milhão de itens, um $O(n²)$ faria $\approx 1$ trilhão de operações, enquanto um $O(n \log n)$ faria $\approx 20$ milhões.

Muitos desses algoritmos se baseiam na estratégia de **"Dividir para Conquistar"**:

1. **Dividir:** O problema é quebrado em subproblemas menores.
2. **Conquistar:** Os subproblemas são resolvidos (geralmente por recursão).
3. **Combinar:** As soluções dos subproblemas são combinadas para formar a solução final.

### Merge Sort (Ordenação por Mesclagem)

O Merge Sort é a personificação clássica da estratégia de "Dividir para Conquistar". Sua ideia central é: é trivial ordenar uma lista de um elemento. Portanto, o algoritmo divide recursivamente a lista ao meio, e ao meio, e ao meio, até que restem apenas listas de um elemento. Em seguida, ele "conquista" combinando (mesclando) essas pequenas listas de volta, de forma ordenada, até que a lista inteira esteja ordenada.

O verdadeiro trabalho do Merge Sort está na função de mesclagem. Ela pega duas listas já ordenadas e as combina em uma única lista ordenada.

**Como funciona:**

1. Usamos dois "ponteiros", `i` (início da lista Esquerda) e `j` (início da lista Direita).
2. Comparamos `Esq[i]` e `Dir[j]`.
3. O menor dos dois é pego e colocado na nova lista. O ponteiro correspondente avança.
4. Isso se repete até que uma das listas termine. Os elementos restantes da outra lista são simplesmente copiados para o final.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

**1. Fase de Divisão (Recursiva):**

- `[5, 3, 8, 4, 2]`
- Divide em `[5, 3, 8]` e `[4, 2]`
- Divide `[5, 3, 8]` em `[5]` e `[3, 8]`
- Divide `[3, 8]` em `[3]` e `[8]` (Chegou à base)
- Divide `[4, 2]` em `[4]` e `[2]` (Chegou à base)

**2. Fase de Conquista (Mesclagem):**

- Mescla `[3]` e `[8]` -> `[3, 8]`
- Mescla `[5]` e `[3, 8]` -> `[3, 5, 8]`
- Mescla `[4]` e `[2]` -> `[2, 4]`
- Mescla `[3, 5, 8]` e `[2, 4]` -> `[2, 3, 4, 5, 8]`

**Pseudocódigo:**

```
função MergeSort(vetor)
    n = tamanho(vetor)
    se n <= 1
        retornar vetor // Base da recursão
    fim_se
    
    meio = n / 2
    esquerda = MergeSort(vetor[0...meio-1])
    direita = MergeSort(vetor[meio...n-1])
    
    retornar Merge(esquerda, direita)
fim_função

função Merge(esquerda, direita)
    resultado = []
    i = 0, j = 0
    
    enquanto i < tamanho(esquerda) e j < tamanho(direita)
        se esquerda[i] <= direita[j]
            adicionar esquerda[i] em resultado
            i = i + 1
        senão
            adicionar direita[j] em resultado
            j = j + 1
        fim_se
    fim_enquanto
    
    // Adiciona os elementos restantes (se houver)
    enquanto i < tamanho(esquerda)
        adicionar esquerda[i] em resultado
        i = i + 1
    fim_enquanto
    enquanto j < tamanho(direita)
        adicionar direita[j] em resultado
        j = j + 1
    fim_enquanto
    
    retornar resultado
fim_função
```

**Boletim do Merge Sort:**

- **Complexidade de Tempo (Todos os Casos):** **$O(n \log n)$**. A divisão da lista cria $\log n$ níveis de recursão. Em cada nível, a operação `Merge` processa todos os $n$ elementos.
- **Complexidade de Espaço:** **$O(n)$**. Esta é sua principal desvantagem. A função `Merge` requer um vetor auxiliar de tamanho $n$ para combinar as sublistas.
- **Estabilidade:** **Sim**. Na função `Merge`, quando os elementos são iguais (`esquerda[i] <= direita[j]`), a implementação padrão pega o elemento da esquerda primeiro, preservando a ordem original.

### Quick Sort (Ordenação Rápida)

O Quick Sort é outro algoritmo de "Dividir para Conquistar" e, na prática, é frequentemente o mais rápido de todos os algoritmos de ordenação baseados em comparação. Diferente do Merge Sort, que faz o trabalho pesado na fase de "Combinação", o Quick Sort faz todo o trabalho na fase de "Divisão", através de um processo chamado **particionamento**.

O algoritmo escolhe um elemento da lista para ser o pivô. O objetivo é, então, reorganizar a lista in-place de forma que:

1. O pivô fique em sua posição final correta.
2. Todos os elementos menores que o pivô fiquem à sua esquerda.
3. Todos os elementos maiores que o pivô fiquem à sua direita.

Após essa partição, o algoritmo é chamado recursivamente para a sublista à esquerda do pivô e para a sublista à direita.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

A escolha do pivô é crucial. Vamos usar a estratégia de "pivô no meio" (arredondado).

Lista: [5, 3, 8, 4, 2]. Pivô = vetor[2] (o 8).

**Passagem 1 (Particionamento):**

- Pivô = `8`.
- Usamos dois ponteiros, `i` (início) e `j` (fim).
- `i` procura um elemento `> pivô`. `j` procura um elemento `< pivô`.
- A partição (pode variar por implementação) resulta em algo como: `[5, 3, 2, 4, 8]`
- _Posição final do pivô (8):_ índice 4.
- **Chamada recursiva 1:** QuickSort em `[5, 3, 2, 4]`
- **Chamada recursiva 2:** QuickSort em `[]` (lista vazia à direita do 8)

**Passagem 2 (em `[5, 3, 2, 4]`):**

- Pivô = `vetor[1]` (o `3`).
- Particionando em torno de `3`: `[2, 3, 5, 4]`
- _Posição final do pivô (3):_ índice 1.
- **Chamada recursiva 3:** QuickSort em `[2]` (à esquerda) -> Lista de 1, ordenada.
- **Chamada recursiva 4:** QuickSort em `[5, 4]` (à direita)

**Passagem 3 (em `[5, 4]`):**

- Pivô = `vetor[0]` (o `5`).
- Particionando em torno de `5`: `[4, 5]`
- _Posição final do pivô (5):_ índice 1.
- **Chamada recursiva 5:** QuickSort em `[4]` (à esquerda) -> Lista de 1, ordenada.
- **Chamada recursiva 6:** QuickSort em `[]` (à direita)

A "combinação" é trivial: como tudo foi feito _in-place_, a lista `[2, 3, 4, 5, 8]` está pronta.

**Pseudocódigo:**

```
função QuickSort(vetor, inicio, fim)
    se inicio < fim
        // Particiona o vetor e obtém o índice do pivô
        indicePivo = Particionar(vetor, inicio, fim)
        
        // Ordena recursivamente as duas sublistas
        QuickSort(vetor, inicio, indicePivo - 1)
        QuickSort(vetor, indicePivo + 1, fim)
    fim_se
fim_função

função Particionar(vetor, inicio, fim)
    pivo = vetor[fim] // Estratégia simples: pivô no final
    i = inicio - 1
    
    para j de inicio até fim - 1
        se vetor[j] < pivo
            i = i + 1
            trocar(vetor[i], vetor[j])
        fim_se
    fim_para
    
    trocar(vetor[i+1], vetor[fim]) // Coloca o pivô na posição correta
    retornar i + 1 // Retorna o índice do pivô
fim_função
```

**Boletim do Quick Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**. Ocorre se o pivô escolhido for sempre o menor ou o maior elemento (ex: numa lista já ordenada). Isso cria partições desbalanceadas ($0$ e $n-1$), e a árvore de recursão vira uma lista de profundidade $n$.
- **Complexidade de Tempo (Melhor e Caso Médio):** **$O(n \log n)$**. Ocorre quando o pivô divide a lista em metades razoavelmente balanceadas.
- **Complexidade de Espaço:** **$O(\log n)$** (caso médio) a **$O(n)$** (pior caso). Este espaço não é para dados, mas para a _pilha de chamadas da recursão_.
- **Estabilidade:** **Não**. O processo de particionamento troca elementos que estão distantes, podendo inverter a ordem de elementos iguais.

### Heap Sort (Ordenação por Pilha)

O Heap Sort é um algoritmo inteligente que usa uma estrutura de dados chamada **Heap** (Pilha) para ordenar. Um Heap é uma árvore binária com duas propriedades:

1. É uma árvore "quase completa" (todos os níveis preenchidos, exceto talvez o último).
2. Obedece à "propriedade do heap": Em um **Max-Heap** (Pilha Máxima), todo nó-pai é _maior ou igual_ a seus nós-filhos. Em um Min-Heap, é o oposto.

Para ordenação, usamos um Max-Heap. A beleza do Heap Sort é que podemos representar essa árvore dentro do próprio vetor, sem precisar de memória extra.

**Como Funciona:**

O algoritmo tem duas fases:

1. **Construção do Heap (Build Max-Heap):** O algoritmo primeiro reorganiza o vetor de entrada para que ele satisfaça a propriedade de um Max-Heap. Após esta fase, `vetor[0]` será garantidamente o _maior elemento_ da lista.
2. Ordenação (Extração):
    a. Troca-se o maior elemento (vetor[0]) com o último elemento do heap (vetor[n-1]).
    b. O maior elemento está agora em sua posição final correta. "Removemos" ele do heap (apenas diminuindo o contador de tamanho n).
    c. A troca quebrou a propriedade do heap na raiz (vetor[0]). O algoritmo aplica uma operação heapify (ou sift-down) na raiz para restaurar a ordem.
    d. Após o heapify, o novo vetor[0] é o segundo maior elemento.
    e. Repete-se (a, b, c, d) até que o heap esteja vazio (e a lista ordenada).

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2]`**

**1. Construção do Max-Heap:**

- A lista `[5, 3, 8, 4, 2]` é reorganizada para `[8, 5, 2, 4, 3]`. (O `8` está na raiz).

**2. Fase de Ordenação:**

- **Passo 1:**
    - Heap: `[8, 5, 2, 4, 3]` (Tamanho 5)
    - Troca `8` (índice 0) com `3` (índice 4): `[3, 5, 2, 4, **8**]`
    - `Heapify` em `[3, 5, 2, 4]` (Tamanho 4). Resultado: `[5, 4, 2, 3, **8**]`

- **Passo 2:**
    - Heap: `[5, 4, 2, 3]` (Tamanho 4)
    - Troca `5` (índice 0) com `3` (índice 3): `[3, 4, 2, **5**, 8]`
    - `Heapify` em `[3, 4, 2]` (Tamanho 3). Resultado: `[4, 3, 2, **5**, 8]`

- **Passo 3:**
    - Heap: `[4, 3, 2]` (Tamanho 3)
    - Troca `4` (índice 0) com `2` (índice 2): `[2, 3, **4**, 5, 8]`
    - `Heapify` em `[2, 3]` (Tamanho 2). Resultado: `[3, 2, **4**, 5, 8]`

- **Passo 4:**
    - Heap: `[3, 2]` (Tamanho 2)
    - Troca `3` (índice 0) com `2` (índice 1): `[2, **3**, 4, 5, 8]`
    - `Heapify` em `[2]` (Tamanho 1). Resultado: `[2, **3**, 4, 5, 8]`

- O heap tem tamanho 1. Fim. Lista final: `[2, 3, 4, 5, 8]`.

**Pseudocódigo:**

```
função HeapSort(vetor)
    n = tamanho(vetor)
    
    // 1. Construir o Max-Heap
    // Começa do último nó não-folha e vai até a raiz
    para i de (n / 2) - 1 até 0
        Heapify(vetor, n, i)
    fim_para
    
    // 2. Extrair elementos um por um
    para i de n - 1 até 1
        // Move a raiz atual (maior) para o fim
        trocar(vetor[0], vetor[i])
        
        // Chama o Heapify na sub-pilha reduzida
        Heapify(vetor, i, 0)
    fim_para
    retornar vetor
fim_função

// Função para manter a propriedade do heap (Max-Heap)
função Heapify(vetor, n, i)
    maior = i
    esquerda = 2*i + 1
    direita = 2*i + 2
    
    se esquerda < n e vetor[esquerda] > vetor[maior]
        maior = esquerda
    fim_se
    se direita < n e vetor[direita] > vetor[maior]
        maior = direita
    fim_se
    
    se maior != i
        trocar(vetor[i], vetor[maior])
        Heapify(vetor, n, maior) // Continua a descer recursivamente
    fim_se
fim_função
```

**Boletim do Heap Sort:**

- **Complexidade de Tempo (Todos os Casos):** **$O(n \log n)$**. A construção do heap (`BuildMaxHeap`) leva $O(n)$. Depois, são $n$ extrações, e cada extração envolve um `Heapify`, que leva $O(\log n)$ (a altura da árvore).
- **Complexidade de Espaço:** **$O(1)$**. É um algoritmo _in-place_. (Algumas implementações usam $O(\log n)$ para a recursão do `Heapify`, mas pode ser feito iterativamente).
- **Estabilidade:** **Não**. As trocas de longa distância entre a raiz e o fim do heap destroem a ordem relativa.

### Shell Sort (Ordenação Shell)

O Shell Sort é uma generalização do Insertion Sort. Sua principal observação é que o Insertion Sort é muito rápido ($O(n)$) para listas "quase ordenadas". O Shell Sort tenta tornar a lista "quase ordenada" o mais rápido possível.

Para fazer isso, ele não compara apenas elementos adjacentes. Ele começa com um `gap` (intervalo) grande e ordena elementos que estão a essa distância. Por exemplo, com `gap = 4`, ele ordena `vetor[0]` com `vetor[4]`, `vetor[8]`, etc.; depois `vetor[1]` com `vetor[5]`, `vetor[9]`, etc. (como se fossem várias sublistas independentes, ordenadas por Insertion Sort).

Depois, ele diminui o `gap` (ex: `gap = 2`) e repete o processo. A lista fica ainda mais ordenada. Finalmente, ele usa `gap = 1`, que é, na verdade, um **Insertion Sort** padrão. Como a lista já está "quase ordenada" pelas passagens anteriores, esta última passagem é muito rápida.

**Exemplo Prático: Ordenando `[5, 3, 8, 4, 2, 6, 1, 7]` (usando gaps 4, 2, 1)**

**Passagem 1 (Gap = 4):**

- Sublistas (intercaladas):
    - `[5, 2]` -> (Insertion Sort) -> `[2, 5]`
    - `[3, 6]` -> (Insertion Sort) -> `[3, 6]`
    - `[8, 1]` -> (Insertion Sort) -> `[1, 8]`
    - `[4, 7]` -> (Insertion Sort) -> `[4, 7]`
- Lista combinada após Passagem 1: `[2, 3, 1, 4, 5, 6, 8, 7]` (Observe os elementos intercalados).

**Passagem 2 (Gap = 2):**

- Sublistas (intercaladas):
    - `[2, 1, 5, 8]` -> (Insertion Sort) -> `[1, 2, 5, 8]`
    - `[3, 4, 6, 7]` -> (Insertion Sort) -> `[3, 4, 6, 7]`
- Lista combinada após Passagem 2: `[1, 2, 3, 4, 5, 6, 8, 7]` (Quase ordenada!)

**Passagem 3 (Gap = 1):**

- Sublista: `[1, 2, 3, 4, 5, 6, 8, 7]`
- Roda o Insertion Sort. A lista está quase toda certa, exceto pelo `7` e `8`.
- O `7` é movido para antes do `8`.
- Lista final: `[1, 2, 3, 4, 5, 6, 7, 8]`

**Pseudocódigo:**

```
função ShellSort(vetor)
    n = tamanho(vetor)
    
    // Define a sequência de gaps (ex: Knuth's sequence ou n/2, n/4, ...)
    para gap em Gaps(n) // ex: [..., 7, 3, 1]
        
        // Aplica um "Insertion Sort com gap"
        para i de gap até n-1
            temp = vetor[i]
            j = i
            
            // Compara com elementos a 'gap' posições atrás
            enquanto j >= gap e vetor[j - gap] > temp
                vetor[j] = vetor[j - gap]
                j = j - gap
            fim_enquanto
            
            vetor[j] = temp
        fim_para
    fim_para
    retornar vetor
fim_função
```

**Boletim do Shell Sort:**

- **Complexidade de Tempo:** Depende _inteiramente_ da sequência de gaps.
    - **Pior Caso:** **$O(n²)$** (com a sequência `n/2, n/4...`).
    - **Caso Médio:** **$O(n^{3/2})$** ou **$O(n^{4/3})$** com sequências de gaps melhores (ex: Sedgewick).
    - **Melhor Caso:** **$O(n \log n)$** (com gaps ideais).
- **Complexidade de Espaço:** **$O(1)$**. É um algoritmo _in-place_.
- **Estabilidade:** **Não**. As trocas de longa distância destroem a ordem relativa.

## Tabela Comparativa dos Algoritmos

A tabela a seguir resume as características dos algoritmos que estudamos neste capítulo, permitindo uma comparação direta.

|**Algoritmo**|**Complexidade de Tempo (Melhor Caso)**|**Complexidade de Tempo (Caso Médio)**|**Complexidade de Tempo (Pior Caso)**|**Complexidade de Espaço**|**Estabilidade**|
|---|---|---|---|---|---|
|**Bubble Sort**|$O(n)$ (otimizado)|$O(n²)$|$O(n²)$|$O(1)$|Sim|
|**Insertion Sort**|$O(n)$|$O(n²)$|$O(n²)$|$O(1)$|Sim|
|**Selection Sort**|$O(n²)$|$O(n²)$|$O(n²)$|$O(1)$|Não|
|**Merge Sort**|$O(n \log n)$|$O(n \log n)$|$O(n \log n)$|$O(n)$|Sim|
|**Quick Sort**|$O(n \log n)$|$O(n \log n)$|$O(n²)$|$O(\log n)$|Não|
|**Heap Sort**|$O(n \log n)$|$O(n \log n)$|$O(n \log n)$|$O(1)$|Não|
|**Shell Sort**|$O(n \log n)$|$O(n^{3/2})$|$O(n²)$|$O(1)$|Não|

## Considerações Finais

Neste capítulo, fizemos uma jornada intensiva, saindo dos algoritmos mais simples e didáticos e chegando aos pilares da ordenação eficiente. Vimos que os algoritmos quadráticos (Bubble, Insertion, Selection) são fáceis de entender, mas sua complexidade $O(n²)$ os torna impraticáveis para grandes volumes de dados.

Em seguida, exploramos os algoritmos avançados. O Merge Sort se destaca por sua estabilidade e performance garantida de $O(n \log n)$, ao custo de $O(n)$ de espaço adicional. O Quick Sort brilha pela sua velocidade na prática, operando _in-place_ ($O(1)$ de espaço), mas carrega o risco de um pior caso $O(n²)$ se o pivô for mal escolhido. O Heap Sort oferece o melhor dos dois mundos em termos de complexidade — $O(n \log n)$ garantido e $O(1)$ de espaço — mas na prática tende a ser mais lento que o Quick Sort e não é estável. Por fim, o Shell Sort se posiciona como um "meio-termo", sendo uma melhoria drástica sobre o Insertion Sort e muito mais simples de implementar que os algoritmos de "Dividir para Conquistar".

Compreender o _trade-off_ (a troca de vantagens e desvantagens) entre esses algoritmos é o que define um desenvolvedor de software eficaz. A escolha do algoritmo correto depende do contexto: O tamanho dos dados é grande? A memória é restrita? A estabilidade é um requisito?

Agora que dominamos os algoritmos baseados em comparação, o próximo capítulo explorará uma família completamente diferente: os algoritmos de ordenação linear (não-comparativos), como o Counting Sort e o Radix Sort, que podem, em cenários específicos, superar a "barreira" do $O(n \log n)$.
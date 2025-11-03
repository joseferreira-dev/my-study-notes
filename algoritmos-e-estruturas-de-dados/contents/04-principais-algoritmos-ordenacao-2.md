# Capítulo 4 – Principais Algoritmos de Ordenação (Parte 2)

Nos capítulos anteriores, exploramos os fundamentos da ordenação e dissecamos os algoritmos clássicos. Vimos os métodos quadráticos simples (como o Bubble Sort e o Insertion Sort) e os métodos de "dividir para conquistar" (como o Merge Sort e o Quick Sort). Estabelecemos que existe um limite teórico de **$O(n \log n)$** para qualquer algoritmo de ordenação que se baseie em **comparações** entre elementos.

Neste capítulo, vamos além desse limite, explorando duas famílias de algoritmos que adotam estratégias diferentes. Primeiro, investigaremos os **algoritmos híbridos e otimizados**, como o Comb Sort e o Tim Sort. Eles são projetados para o "mundo real", onde os dados raramente são perfeitamente aleatórios, corrigindo falhas de algoritmos mais simples ou combinando os pontos fortes de vários métodos. Em seguida, mergulharemos nos **algoritmos de ordenação linear** (não-comparativos), como o Counting Sort, o Radix Sort e o Bucket Sort. Esses algoritmos "quebram as regras" — eles não comparam elementos entre si e, sob condições específicas, conseguem atingir a incrível complexidade de tempo linear, $O(n)$.

## Algoritmos Otimizados e Híbridos

Esta classe de algoritmos reconhece que os métodos clássicos, embora elegantes, têm fraquezas. As otimizações aqui apresentadas visam mitigar essas fraquezas para obter um desempenho superior na prática.

### Comb Sort (Ordenação Pente)

O Comb Sort é uma otimização direta e inteligente do Bubble Sort. A principal fraqueza do Bubble Sort é o problema das "tartarugas": elementos pequenos localizados no final da lista, que se movem para o início de forma excruciantemente lenta (apenas uma posição por passagem completa).

O Comb Sort resolve isso introduzindo um **gap (intervalo)** entre os elementos que são comparados. Em vez de comparar apenas elementos adjacentes (gap = 1), ele começa com um gap grande e o reduz progressivamente.

**Como Funciona:**

1. **Definição do Gap Inicial:** O gap é inicializado com o tamanho da lista.
2. **Fator de Encolhimento:** Um "fator de encolhimento" é definido. O valor empírico de **1.3** é considerado o mais eficaz. A cada passagem, o gap é dividido por esse fator.
3. **Comparação com Gap:** O algoritmo percorre a lista comparando elementos `vetor[i]` com `vetor[i + gap]`. Se estiverem fora de ordem, são trocados. Isso permite que "tartarugas" no final da lista sejam trocadas com elementos no início, movendo-as grandes distâncias em uma única operação.
4. **Redução do Gap:** O gap é reduzido ( `gap = gap / 1.3` ) e o processo se repete.
5. **Fase Final:** Eventualmente, o gap se torna 1. Neste ponto, o Comb Sort se transforma efetivamente em um Bubble Sort. No entanto, como as passagens anteriores com gaps maiores já moveram os elementos para perto de suas posições finais, a lista estará "quase ordenada", e esta fase final de Bubble Sort será extremamente rápida (próxima de $O(n)$).

**Exemplo Prático: Ordenando `[9, 7, 5, 3, 1]`**

Esta é uma lista em ordem inversa, o pior caso para o Bubble Sort (uma "tartaruga" em cada posição).

**Passagem 1 (Gap = 5 / 1.3 ≈ 3):**

- Compara (índice 0, 3): `[**9**, 7, 5, **3**, 1]` -> Troca -> `[3, 7, 5, 9, 1]`
- Compara (índice 1, 4): `[3, **7**, 5, 9, **1**]` -> Troca -> `[3, 1, 5, 9, 7]`
- _Lista ao final:_ `[3, 1, 5, 9, 7]`

**Passagem 2 (Gap = 3 / 1.3 ≈ 2):**

- Compara (0, 2): `[**3**, 1, **5**, 9, 7]` -> Não troca.
- Compara (1, 3): `[3, **1**, 5, **9**, 7]` -> Não troca.
- Compara (2, 4): `[3, 1, **5**, 9, **7**]` -> Não troca.
- _Lista ao final:_ `[3, 1, 5, 9, 7]`

**Passagem 3 (Gap = 2 / 1.3 ≈ 1):**

- _O gap agora é 1 (fase de Bubble Sort)._
- `[**3**, **1**, 5, 9, 7]` -> Troca -> `[1, 3, 5, 9, 7]`
- `[1, **3**, **5**, 9, 7]` -> Não troca.
- `[1, 3, **5**, **9**, 7]` -> Não troca.
- `[1, 3, 5, **9**, **7**]` -> Troca -> `[1, 3, 5, 7, 9]`
- _Lista ao final:_ `[1, 3, 5, 7, 9]`

**Passagem 4 (Gap = 1, verificação):**

- Nenhuma troca é feita. O algoritmo termina.

**Pseudocódigo:**

```
função CombSort(vetor)
    n = tamanho(vetor)
    gap = n
    fatorEncolhimento = 1.3
    ordenado = falso
    
    enquanto ordenado == falso
        gap = floor(gap / fatorEncolhimento)
        se gap <= 1
            gap = 1
            ordenado = true // Assume que está ordenado até provar o contrário
        fim_se
        
        i = 0
        enquanto i + gap < n
            se vetor[i] > vetor[i + gap]
                trocar(vetor[i], vetor[i + gap])
                ordenado = falso // Uma troca ocorreu, não está ordenado
            fim_se
            i = i + 1
        fim_enquanto
    fim_enquanto
    retornar vetor
fim_função
```

**Boletim do Comb Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**.
- **Complexidade de Tempo (Melhor Caso):** **$O(n \log n)$**.
- **Complexidade de Tempo (Caso Médio):** Fica entre $O(n \log n)$ e $O(n²)$. Na prática, supera o Bubble Sort e o Shell Sort (com gaps simples).
- **Complexidade de Espaço:** **$O(1)$** (in-place).
- **Estabilidade:** **Não**. As trocas em longa distância podem inverter a ordem de elementos iguais.

### Tim Sort (Ordenação Híbrida)

O Tim Sort é um dos algoritmos de ordenação mais avançados e práticos já criados. Desenvolvido por Tim Peters em 2002, ele é o algoritmo de ordenação padrão em Python, Java (para arrays de objetos) e outras linguagens.

Sua genialidade está em ser um algoritmo **híbrido**, combinando a velocidade do **Merge Sort** (para eficiência $O(n \log n)$) com a velocidade do **Insertion Sort** (para eficiência em listas pequenas e "quase ordenadas"). Ele foi projetado especificamente para dados do "mundo real", que muitas vezes contêm _sequências já ordenadas_ (chamadas "runs").

**Como Funciona:**

1. **Identificação de "Runs":** O algoritmo primeiro varre a lista para encontrar "runs" naturais, que são subsequências já ordenadas (crescentes ou decrescentes). Se um run for decrescente, ele é simplesmente invertido.
2. **Criação de Runs Mínimos:** O algoritmo define um "minrun" (tamanho mínimo de run, geralmente 32 ou 64). Se um run natural encontrado for _menor_ que o minrun, o Insertion Sort é usado para "completar" esse run, pegando elementos adjacentes até que ele atinja o tamanho mínimo. O Insertion Sort é usado aqui por ser extremamente rápido para listas pequenas.
3. **Mesclagem Inteligente (Merge):** Após a lista ser dividida em uma série de runs ordenados (todos com tamanho >= minrun), o Tim Sort começa a mesclá-los usando a lógica do Merge Sort. Ele não mescla cegamente; ele usa uma pilha para gerenciar os runs e realiza mesclagens "inteligentes" para manter os tamanhos dos runs na pilha o mais balanceados possível, otimizando o processo de merge.

**Exemplo Prático: Ordenando `[5, 21, 7, 23, 19, 1, 4, 18, 2, 11, 10, 3, 13, 20, 17, 6]`**

Vamos assumir um `minrun` de 4 para simplificar.

1. **Divisão e Ordenação de Runs (usando Insertion Sort):**
    
    - Run 1: `[5, 21, 7, 23]` -> (Insertion Sort) -> `[5, 7, 21, 23]`
    - Run 2: `[19, 1, 4, 18]` -> (Insertion Sort) -> `[1, 4, 18, 19]`
    - Run 3: `[2, 11, 10, 3]` -> (Insertion Sort) -> `[2, 3, 10, 11]`
    - Run 4: `[13, 20, 17, 6]` -> (Insertion Sort) -> `[6, 13, 17, 20]`
    - _Neste ponto, temos 4 "blocos" ordenados._

2. **Mesclagem dos Runs (Merge):**
    
    - O Tim Sort mescla os dois primeiros runs:
        - `[5, 7, 21, 23]` e `[1, 4, 18, 19]` -> `[1, 4, 5, 7, 18, 19, 21, 23]`

    - Em seguida, mescla os dois próximos:
        - `[2, 3, 10, 11]` e `[6, 13, 17, 20]` -> `[2, 3, 6, 10, 11, 13, 17, 20]`

    - Finalmente, mescla os dois grandes resultados:
        - Resultado 1 e Resultado 2 -> `[1, 2, 3, 4, 5, 6, 7, 10, 11, 13, 17, 18, 19, 20, 21, 23]`

Pseudocódigo:

Um pseudocódigo preciso do Tim Sort é extremamente complexo. A alto nível, o processo é:

```
função TimSort(vetor)
    n = tamanho(vetor)
    minrun = 32 // ou 64
    
    // 1. Dividir e ordenar runs
    para i de 0 até n, pulando de minrun em minrun
        fim = min(i + minrun - 1, n - 1)
        InsertionSort(vetor, i, fim)
    fim_para
    
    // 2. Mesclar runs
    tamanho = minrun
    enquanto tamanho < n
        para esq de 0 até n, pulando de 2*tamanho em 2*tamanho
            meio = min(esq + tamanho - 1, n - 1)
            dir = min(esq + 2*tamanho - 1, n - 1)
            
            se meio < dir // Se houver um segundo run para mesclar
                Merge(vetor, esq, meio, dir)
            fim_se
        fim_para
        tamanho = 2 * tamanho
    fim_enquanto
    retornar vetor
fim_função
```

**Boletim do Tim Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n \log n)$**.
- **Complexidade de Tempo (Melhor Caso):** **$O(n)$**. Se a lista já estiver ordenada (ou inversamente ordenada), ele identifica um único run, faz a inversão (se necessário) e termina.
- **Complexidade de Tempo (Caso Médio):** **$O(n \log n)$**.
- **Complexidade de Espaço:** **$O(n)$** (pois precisa de espaço auxiliar para a mesclagem, como o Merge Sort).
- **Estabilidade:** **Sim**. A estabilidade é herdada tanto do Insertion Sort quanto do Merge Sort.

## Algoritmos de Ordenação Linear (Não-Comparativos)

A "barreira" de $O(n \log n)$ só existe para algoritmos baseados em comparação. A família de algoritmos a seguir não compara elementos entre si. Em vez disso, eles usam propriedades intrínsecas dos dados (como o fato de serem inteiros em um intervalo) para determinar suas posições finais, permitindo que alcancem, em cenários ideais, complexidade de tempo linear.

### Counting Sort (Ordenação por Contagem)

O Counting Sort é um algoritmo de ordenação linear que funciona sob uma restrição rigorosa: ele só pode ordenar dados que consistem em **inteiros dentro de um intervalo conhecido e limitado**. Em vez de comparar, ele _conta_ a frequência de cada elemento e usa essa contagem para construir a lista ordenada.

**Como Funciona:**

1. **Determinação do Intervalo:** O algoritmo primeiro encontra o valor máximo (`max`) e o valor mínimo (`min`) na lista de entrada. O tamanho do intervalo é $k = max - min + 1$.
2. **Contagem de Ocorrências:** Cria-se um "vetor de contagem" auxiliar de tamanho $k$. O algoritmo percorre a lista original e, para cada elemento, incrementa a posição correspondente no vetor de contagem. Ao final, `vetorContagem[i]` conterá o número de vezes que o valor `i + min` apareceu.
3. **Cálculo das Posições (Soma Cumulativa):** O vetor de contagem é modificado. Cada posição passa a armazenar a soma de todas as contagens anteriores. O valor em `vetorContagem[i]` agora significa "quantos elementos são menores ou iguais a `i + min`". Isso indica a _posição final_ (ou índice + 1) de cada elemento.
4. **Construção da Lista Ordenada:** Cria-se um vetor de saída. O algoritmo percorre a lista original **de trás para frente** (para garantir estabilidade). Para cada elemento, ele consulta sua posição final no vetor de posições, coloca o elemento no vetor de saída e decrementa o valor no vetor de posições.

**Exemplo Prático: Ordenando `[2, 5, 3, 0, 2, 3, 0, 3]`**

1. **Intervalo:** `min = 0`, `max = 5`. Tamanho $k = 6$.

2. **Contagem de Ocorrências:**
    - `vetorContagem` (tamanho 6, índices 0-5): `[0, 0, 0, 0, 0, 0]`
    - Após varrer a lista `[2, 5, 3, 0, 2, 3, 0, 3]`:
    - `vetorContagem`: `[2, 0, 2, 3, 0, 1]`
    - _(Significa: 2 zeros, 0 uns, 2 dois, 3 três, 0 quatros, 1 cinco)_

3. **Cálculo das Posições (Soma Cumulativa):**
    - `vetorPosicoes[0] = 2`
    - `vetorPosicoes[1] = 2 + 0 = 2`
    - `vetorPosicoes[2] = 2 + 2 = 4`
    - `vetorPosicoes[3] = 4 + 3 = 7`
    - `vetorPosicoes[4] = 7 + 0 = 7`
    - `vetorPosicoes[5] = 7 + 1 = 8`
    - `vetorPosicoes`: `[2, 2, 4, 7, 7, 8]`
    - _(Significa: Zeros terminam na posição 2. Uns terminam na 2. Dois terminam na 4. Três terminam na 7...)_

4. **Construção da Saída:**
    - `vetorSaida`: `[ , , , , , , , ]` (tamanho 8)
    - Começa do fim da lista original: `...3]`
        - Pega o `3`. `vetorPosicoes[3]` é `7`. Coloca `3` no índice `7-1 = 6`. Decrementa `vetorPosicoes[3]` para `6`.
        - `vetorSaida`: `[ , , , , , , 3, ]`

    - Próximo: `...0, 3]`
        - Pega o `0`. `vetorPosicoes[0]` é `2`. Coloca `0` no índice `2-1 = 1`. Decrementa `vetorPosicoes[0]` para `1`.
        - `vetorSaida`: `[ , 0, , , , , 3, ]`

    - ... (Repetindo o processo para todos os elementos) ...
    - `vetorSaida` final: `[0, 0, 2, 2, 3, 3, 3, 5]`

**Pseudocódigo:**

```
função CountingSort(vetor)
    n = tamanho(vetor)
    max = valor_maximo(vetor)
    min = valor_minimo(vetor)
    k = max - min + 1
    
    vetorContagem = novo_vetor(k, preenchido_com_zeros)
    vetorSaida = novo_vetor(n)
    
    // 2. Contagem de Ocorrências
    para i de 0 até n - 1
        vetorContagem[vetor[i] - min] = vetorContagem[vetor[i] - min] + 1
    fim_para
    
    // 3. Cálculo das Posições (Soma Cumulativa)
    para i de 1 até k - 1
        vetorContagem[i] = vetorContagem[i] + vetorContagem[i - 1]
    fim_para
    
    // 4. Construção da Lista Ordenada
    para i de n - 1 até 0
        elemento = vetor[i]
        indiceContagem = elemento - min
        
        posicaoSaida = vetorContagem[indiceContagem] - 1
        vetorSaida[posicaoSaida] = elemento
        
        vetorContagem[indiceContagem] = vetorContagem[indiceContagem] - 1
    fim_para
    
    retornar vetorSaida
fim_função
```

**Boletim do Counting Sort:**

- **Complexidade de Tempo:** **$O(n + k)$**. ($O(n)$ para contar, $O(k)$ para somar, $O(n)$ para construir).
- **Complexidade de Espaço:** **$O(n + k)$** (para `vetorSaida` e `vetorContagem`).
- **Estabilidade:** **Sim**. Percorrer a lista original de trás para frente garante a estabilidade.

### Radix Sort (Ordenação por Dígitos)

O Radix Sort é um algoritmo não-comparativo que ordena números processando-os dígito por dígito. Ele não compara os números inteiros entre si; em vez disso, ele agrupa os números com base em seus dígitos individuais.

Crucialmente, o Radix Sort **requer um algoritmo de ordenação estável** como sub-rotina. O Counting Sort é a escolha perfeita para isso.

**Como Funciona (Versão LSD - Least Significant Digit):**

1. **Determinação dos Dígitos:** O algoritmo primeiro descobre o número máximo na lista, para saber quantos dígitos (`d`) ele precisa processar.
2. **Iteração pelos Dígitos:** O algoritmo itera `d` vezes, começando pelo dígito menos significativo (unidades), depois dezenas, centenas, etc.
3. **Ordenação Estável por Dígito:** Em cada iteração, ele usa um algoritmo de ordenação estável (como o Counting Sort) para ordenar a lista inteira, _mas considerando apenas o dígito daquela iteração_.

A estabilidade é o que faz a "mágica" acontecer. Quando ele ordena pelas dezenas, os números com a mesma dezena (ex: 75, 70, 72) permanecem na ordem em que estavam, que é a ordem das unidades (70, 72, 75), garantida pela passagem anterior.

**Exemplo Prático: Ordenando `[170, 45, 75, 90, 802, 24, 2, 66]`**

**Passagem 1 (Ordena pelo dígito das Unidades):**

- _Chaves de ordenação (unidades):_ 0, 5, 5, 0, 2, 4, 2, 6
- `[17**0**, 9**0**, 80**2**, **2**, 2**4**, 4**5**, 7**5**, 6**6**]`
- _Resultado (estável):_ `[170, 90, 802, 2, 24, 45, 75, 66]` (Note que 170 vem antes de 90; 802 antes de 2; 45 antes de 75)

**Passagem 2 (Ordena pelo dígito das Dezenas):**

- _Chaves de ordenação (dezenas):_ 7, 9, 0, 0, 2, 4, 7, 6
- `[1**7**0, 0**9**0, 8**0**2, 00**2**, 0**2**4, 0**4**5, 0**7**5, 0**6**6]`
- _Resultado (estável):_ `[802, 2, 24, 45, 66, 170, 75, 90]` (Note que 802 e 2, ambos com dezena 0, mantêm a ordem da passagem 1)

**Passagem 3 (Ordena pelo dígito das Centenas):**

- _Chaves de ordenação (centenas):_ 1, 0, 8, 0, 0, 0, 0, 0
- `[**1**70, **0**90, **8**02, **0**02, **0**24, **0**45, **0**75, **0**66]`
- _Resultado (estável):_ `[2, 24, 45, 66, 75, 90, 170, 802]`
- Lista final ordenada.

**Pseudocódigo:**

```
função RadixSort(vetor)
    max = valor_maximo(vetor)
    d = numero_de_digitos(max)
    
    // Itera do dígito menos significativo (exp=1) ao mais significativo
    para i de 0 até d - 1
        exp = 10^i
        // Ordena estavelmente a lista usando o i-ésimo dígito
        // (CountingSort pode ser adaptado para isso)
        StableSortPorDigito(vetor, exp)
    fim_para
    retornar vetor
fim_função

// StableSortPorDigito seria uma implementação do CountingSort
// que usa (vetor[j] / exp) % 10 como a chave de contagem.
```

**Boletim do Radix Sort:**

- **Complexidade de Tempo:** **$O(d \cdot (n + k))$**. $d$ é o número de dígitos, $n$ é o número de elementos, e $k$ é a base (o intervalo de um dígito, ex: 10 para decimal).
- **Complexidade de Espaço:** **$O(n + k)$** (herdado do Counting Sort).
- **Estabilidade:** **Sim**. Depende da estabilidade do algoritmo interno.

### Bucket Sort (Ordenação por Baldes)

O Bucket Sort é outro algoritmo de ordenação linear que funciona bem sob uma condição: os dados de entrada devem estar **distribuídos de forma (aproximadamente) uniforme** em um intervalo conhecido.

A ideia é "espalhar" os elementos por um número $k$ de "baldes" (buckets). Se a distribuição for uniforme, espera-se que cada balde contenha poucos elementos.

**Como Funciona:**

1. **Criação dos Baldes:** O algoritmo cria $k$ baldes (listas vazias). O número de baldes ($k$) é geralmente escolhido para ser próximo do número de elementos ($n$).
2. **Distribuição (Scatter):** O algoritmo percorre a lista original e, usando uma função de _hash_ (geralmente uma fórmula matemática simples), "espalha" cada elemento em seu balde correspondente.
3. **Ordenação dos Baldes:** O algoritmo percorre cada balde e os ordena individualmente, geralmente usando um algoritmo simples como o **Insertion Sort**.
4. **Coleta (Gather):** Os elementos dos baldes (agora ordenados) são concatenados na ordem dos baldes, formando a lista final ordenada.

**Exemplo Prático: Ordenando `[0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]`**

1. **Criação dos Baldes:** Vamos usar 10 baldes ($k=10$), para os intervalos [0.0, 0.1), [0.1, 0.2), ..., [0.9, 1.0).
2. **Distribuição (Scatter):**
    - `0.78` -> Balde 7
    - `0.17` -> Balde 1
    - `0.39` -> Balde 3
    - `0.26` -> Balde 2
    - ...e assim por diante.
    - _Resultado:_
	    - Balde 0: `[]`
	    - Balde 1: `[0.17, 0.12]`
	    - Balde 2: `[0.26, 0.21, 0.23]`
	    - Balde 3: `[0.39]`
	    - Balde 4: `[]`
	    - Balde 5: `[]`
	    - Balde 6: `[0.68]`
	    - Balde 7: `[0.78, 0.72]`
	    - Balde 8: `[]`
	    - Balde 9: `[0.94]`

3. **Ordenação dos Baldes (usando Insertion Sort):**
    - Balde 1: `[0.12, 0.17]`
    - Balde 2: `[0.21, 0.23, 0.26]`
    - Balde 3: `[0.39]`
    - Balde 6: `[0.68]`
    - Balde 7: `[0.72, 0.78]`
    - Balde 9: `[0.94]`

4. **Coleta (Gather):**
    - Concatenar os baldes em ordem: `[0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]`

**Pseudocódigo:**

```
função BucketSort(vetor, k) // k = número de baldes
    n = tamanho(vetor)
    max = valor_maximo(vetor)
    baldes = criar_lista_de_listas(k)
    
    // 2. Distribuição (Scatter)
    para i de 0 até n - 1
        elemento = vetor[i]
        // Fórmula de hash simples para 0 a max
        indiceBalde = floor(k * elemento / (max + 1)) 
        adicionar elemento em baldes[indiceBalde]
    fim_para
    
    // 3. Ordenação dos Baldes
    para i de 0 até k - 1
        InsertionSort(baldes[i])
    fim_para
    
    // 4. Coleta (Gather)
    vetorSaida = []
    para i de 0 até k - 1
        adicionar todos os elementos de baldes[i] em vetorSaida
    fim_para
    
    retornar vetorSaida
fim_função
```

**Boletim do Bucket Sort:**

- **Complexidade de Tempo (Pior Caso):** **$O(n²)$**. Ocorre se a distribuição não for uniforme e todos os elementos caírem em um único balde. O algoritmo degenera para a complexidade do sort interno (Insertion Sort).
- **Complexidade de Tempo (Melhor e Caso Médio):** **$O(n + k)$**. $O(n)$ para distribuir e $O(k)$ para concatenar, assumindo que a ordenação dos baldes leva tempo constante (se $n$ e $k$ são similares).
- **Complexidade de Espaço:** **$O(n + k)$** (para armazenar os baldes e os elementos).
- **Estabilidade:** **Depende**. É estável se o algoritmo de ordenação usado dentro dos baldes (ex: Insertion Sort) for estável.

## Tabela Comparativa dos Algoritmos

A tabela a seguir resume as características dos algoritmos mais avançados que exploramos neste capítulo.

|**Algoritmo**|**Complexidade de Tempo (Melhor Caso)**|**Complexidade de Tempo (Caso Médio)**|**Complexidade de Tempo (Pior Caso)**|**Complexidade de Espaço**|**Estabilidade**|
|---|---|---|---|---|---|
|**Comb Sort**|$O(n \log n)$|$O(n²)$ ou $O(n \log n)$|$O(n²)$|$O(1)$|Não|
|**Tim Sort**|$O(n)$|$O(n \log n)$|$O(n \log n)$|$O(n)$|Sim|
|**Counting Sort**|$O(n + k)$|$O(n + k)$|$O(n + k)$|$O(n + k)$|Sim|
|**Radix Sort**|$O(d \cdot (n+k))$|$O(d \cdot (n+k))$|$O(d \cdot (n+k))$|$O(n + k)$|Sim|
|**Bucket Sort**|$O(n + k)$|$O(n + k)$|$O(n²)$|$O(n + k)$|Depende|

_(k: intervalo ou nº de baldes; d: nº de dígitos)_

## Considerações Finais

Este capítulo concluiu nossa exploração aprofundada dos principais algoritmos de ordenação, focando em duas categorias que vão além dos métodos clássicos.

Primeiro, vimos os algoritmos **híbridos e otimizados**. O **Comb Sort** nos mostrou como uma simples mudança de perspectiva (a introdução de um _gap_) pode consertar uma falha fundamental em um algoritmo clássico (o problema das "tartarugas" do Bubble Sort). O **Tim Sort**, por sua vez, representa o auge da ordenação prática, um algoritmo híbrido que combina a eficiência do Insertion Sort para dados "quase ordenados" com a robustez $O(n \log n)$ do Merge Sort, resultando em um desempenho excepcional em dados do mundo real.

Segundo, mergulhamos no mundo dos **algoritmos de ordenação linear**. O **Counting Sort**, o **Radix Sort** e o **Bucket Sort** nos provaram que a "barreira" de $O(n \log n)$ não é absoluta. Ao abrir mão das comparações e impor restrições aos dados (intervalo limitado, distribuição uniforme), é possível ordenar em tempo linear. O Counting Sort o faz pela contagem direta, o Radix Sort pela aplicação sucessiva em dígitos, e o Bucket Sort pela distribuição em "baldes".

A jornada pelos algoritmos de ordenação, dos simples $O(n²)$ aos eficientes $O(n \log n)$ e aos especializados $O(n)$, ilustra um dos conceitos centrais da Ciência da Computação: não existe "bala de prata". A escolha do algoritmo correto é sempre uma questão de _trade-offs_, dependendo do volume dos dados, da sua distribuição, dos recursos de memória e da necessidade de estabilidade.
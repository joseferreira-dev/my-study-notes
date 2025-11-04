# Capítulo 7 – Vetores e Matrizes

Após estabelecermos os conceitos fundamentais de Tipos Abstratos de Dados (TADs) e a organização da memória em Stack e Heap, iniciamos nossa jornada pelas estruturas de dados concretas. Começamos com as mais fundamentais e onipresentes em toda a computação: as **estruturas de dados lineares**. Dentre elas, os **vetores** e **matrizes** se destacam por sua simplicidade, eficiência de acesso e vasta aplicabilidade. Eles formam a base para inúmeras soluções, desde o simples armazenamento de uma sequência de números até aplicações complexas em processamento de imagens, simulações científicas e **inteligência artificial**.

Neste capítulo, exploraremos em profundidade o funcionamento, as vantagens, as limitações e os usos práticos dessas estruturas. Analisaremos como sua implementação difere em linguagens como **C**, **Python**, **Java** e **JavaScript**, e, o mais importante, investigaremos _por que_ elas possuem as características de desempenho que as definem.

## Vetores (Arrays Unidimensionais)

Um **vetor**, tecnicamente conhecido como **array unidimensional**, é a estrutura de dados mais simples. É uma coleção de elementos de um **mesmo tipo**, armazenados de forma **contígua na memória**. Esta é a sua característica definidora e a fonte de todas as suas forças e fraquezas.

"Contíguo" significa que os elementos são alocados em sequência, um imediatamente após o outro, sem buracos. Se um vetor de inteiros começa no endereço de memória `1000`, o próximo inteiro estará no endereço `1004` (supondo inteiros de 4 bytes), o seguinte em `1008`, e assim por diante.

<div align="center">
<img width="600px" src="./img/07-vetor.png">
</div>

Na imagem acima, temos um vetor de inteiros com capacidade para 6 elementos. Os valores `2`, `7`, `3` e `1` foram armazenados nas posições (índices) `0`, `1`, `2` e `3`, respectivamente. As posições `4` e `5` estão alocadas, mas ainda não foram preenchidas com dados úteis.

### Eficiência do Acesso

A principal vantagem dos vetores, decorrente de sua alocação contígua, é o **acesso direto em tempo constante**, ou **$O(1)$**. Isso significa que o tempo para acessar qualquer elemento é o mesmo, não importa se é o primeiro, o do meio ou o último.

Isso não é mágica; é matemática simples de hardware. Quando se requisita `vetor[i]`, o computador não percorre a estrutura. Ele calcula o endereço de memória exato instantaneamente usando a fórmula:

$Endereço(vetor[i]) = EndereçoBase + (i \times TamanhoDoElemento)$

Por exemplo, para encontrar `vetor[3]` (o elemento "1" da imagem acima):

- `EndereçoBase`: Onde o vetor começa (ex: `1000`).
- `i`: O índice desejado (`3`).
- `TamanhoDoElemento`: O tamanho de um `int` (ex: `4` bytes).
- `Endereço(vetor[3]) = 1000 + (3 * 4) = 1012`.

O processador "pula" diretamente para o endereço `1012` e lê o valor. Este cálculo leva tempo constante.

### Localidade de Cache

A alocação contígua oferece outra vantagem de desempenho crucial: **localidade de cache**. Processadores modernos não leem dados da RAM byte por byte. Para acelerar o acesso, eles puxam "blocos" inteiros de memória (chamados _cache lines_) para uma memória interna ultrarrápida (o Cache da CPU).

Como todos os elementos de um vetor estão juntos, quando o processador busca `vetor[0]`, ele provavelmente já "puxa" `vetor[1]`, `vetor[2]`, `vetor[3]`, etc., para o cache. Ao percorrer o vetor em sequência (uma operação extremamente comum), os dados subsequentes já estão na memória mais rápida possível, tornando a iteração muito eficiente.

## Operações em Vetores

Vamos analisar as operações básicas em um vetor de tamanho $n$.

### Travessia (Iteração)

Percorrer todos os elementos, seja para ler, modificar ou processar, é a operação mais comum. Ela exige visitar cada um dos $n$ elementos.

- **Complexidade:** **$O(n)$**

**Pseudocódigo:**

```
função PercorrerVetor(vetor)
    n = tamanho(vetor)
    para i de 0 até n-1
        processar(vetor[i])
    fim_para
fim_função
```

### Inserção e Remoção

É aqui que a fraqueza do vetor estático aparece. Como a memória é contígua, não podemos simplesmente "criar" um novo espaço no meio.

- **Inserção no final:** Se o vetor ainda tiver espaço (como nas posições 4 e 5 do exemplo inicial), a inserção no final é $O(1)$, pois basta atribuir o valor à posição `vetor[tamanho_atual]`.
- **Inserção no início ou meio:** Para inserir um elemento no índice `i`, é preciso "abrir espaço". Isso exige deslocar _todos_ os elementos de `i` até $n-1$ uma posição para a direita, antes de inserir o novo valor.
- **Remoção no início ou meio:** Similarmente, para remover um elemento do índice `i`, é preciso "fechar o buraco", deslocando _todos_ os elementos de `i+1` até $n-1$ uma posição para a esquerda.

Ambas as operações de inserção e remoção no meio ou início têm, no pior caso, que mover $n-1$ elementos.

- **Complexidade (Inserção/Remoção):** **$O(n)$**

**Pseudocódigo (Inserção no Meio):**

```
// Assume que o vetor tem capacidade 'n' e 'tamanho_atual' < 'n'
função InserirEm(vetor, tamanho_atual, indice, valor)
    // 1. Deslocar elementos para a direita
    para i de tamanho_atual - 1 até indice
        vetor[i+1] = vetor[i]
    fim_para
    
    // 2. Inserir o novo valor
    vetor[indice] = valor
    retornar tamanho_atual + 1
fim_função
```

## Implementação de Vetores

Embora o conceito de "vetor" seja universal, sua implementação e terminologia variam entre as linguagens.

**C (Vetor Estático Puro):** Em C, um array é exatamente o que descrevemos: um bloco de memória estático e contíguo.

```c
#include <stdio.h>

int main() {
    // Declaração de um vetor estático de 6 inteiros
    int numeros[6] = {2, 7, 3, 1, 0, 0}; // Posições 4 e 5 inicializadas com 0
    numeros[4] = 10;
    
    printf("O elemento na posição 3 é: %d\n", numeros[3]); // Acesso O(1) -> imprime 1

    // Travessia O(n)
    printf("Vetor completo: ");
    for (int i = 0; i < 6; i++) {
        printf("%d ", numeros[i]);
    }
    // Saída: Vetor completo: 2 7 3 1 10 0
    
    return 0;
}
```

**Java (Vetor Estático Puro):** Em Java, os arrays (vetores) também têm tamanho fixo. Eles são, na verdade, objetos, mas a estrutura de dados subjacente é contígua e de tamanho fixo.

```java
public class VetorExemplo {
    public static void main(String[] args) {
        // Declaração de um vetor estático de 4 inteiros
        int[] numeros = {2, 7, 3, 1}; 
        
        System.out.println("O elemento na posição 3 é: " + numeros[3]); // Acesso O(1) -> imprime 1

        // Travessia O(n)
        System.out.print("Vetor completo: ");
        for (int i = 0; i < numeros.length; i++) {
            System.out.print(numeros[i] + " ");
        }
        // Saída: Vetor completo: 2 7 3 1 
    }
}
```

**Python e JavaScript (Vetores Dinâmicos):** Aqui, a situação é diferente. As estruturas nativas list (Python) e Array (JavaScript) não são vetores estáticos. Elas são vetores dinâmicos, que resolvem o problema do tamanho fixo.

```python
# Em Python, a 'list' é um vetor dinâmico
numeros = [2, 7, 3, 1]
print("O elemento na posição 3 é:", numeros[3])  # Acesso O(1) -> imprime 1

# Travessia O(n)
print("Vetor completo:", end=" ")
for num in numeros:
    print(num, end=" ")
# Saída: Vetor completo: 2 7 3 1

# Inserção (mesmo no meio) é fácil, mas custa O(n)
numeros.insert(1, 99) # Insere 99 no índice 1
print("\nVetor após inserção:", numeros)
# Saída: Vetor após inserção: [2, 99, 7, 3, 1]
```

```javascript
// Em JavaScript, 'Array' também é um vetor dinâmico
let numeros = [2, 7, 3, 1];
console.log("O elemento na posição 3 é: " + numeros[3]); // Acesso O(1) -> imprime 1

// Travessia O(n)
console.log("Vetor completo: ");
for (let i = 0; i < numeros.length; i++) {
    console.log(numeros[i]);
}

// Adicionar no final (custo amortizado O(1))
numeros.push(10); 
console.log("Vetor após push:", numeros);
// Saída: Vetor após push: [2, 7, 3, 1, 10]
```

## Vetor Dinâmico (TAD Lista)

Como visto em Python e JavaScript, o "vetor estático" puro é muitas vezes encapsulado por uma estrutura mais flexível: o **Vetor Dinâmico** (ou **Lista Dinâmica**, como o `ArrayList` em Java ou `std::vector` em C++).

Esta estrutura resolve a limitação do tamanho fixo da seguinte forma:

1. **Internamente:** Ela usa um vetor estático.
2. **Capacidade vs. Tamanho:** Ela gerencia duas variáveis: `tamanho` (quantos elementos realmente existem) e `capacidade` (o tamanho do vetor interno).
3. Redimensionamento: Quando o usuário tenta adicionar um item (ex: `push` ou `append`) e o tamanho é igual à capacidade, a estrutura executa uma operação de redimensionamento:
    - Aloca um novo vetor interno, maior (geralmente o dobro do tamanho).
    - Copia todos os $n$ elementos do vetor antigo para o novo.
    - Libera a memória do vetor antigo.
    - Insere o novo elemento.

Esta operação de cópia é $O(n)$, o que parece lento. No entanto, como ela não acontece em toda inserção (só quando a capacidade esgota), o custo é "diluído" ao longo de muitas inserções. O resultado é um custo de inserção no final de **$O(1)$ Amortizado**.

## Matrizes (Arrays Multidimensionais)

Uma **matriz** é uma generalização do vetor para duas ou mais dimensões. Em sua forma mais comum, uma **matriz bidimensional** (2D) organiza os dados em uma tabela de linhas e colunas.

<div align="center">
<img width="400px" src="./img/07-matriz-01.png">
</div>

Para acessar um elemento, precisamos de dois índices: o da linha e o da coluna (ex: `matriz[linha][coluna]`). O acesso também é uma operação $O(1)$.

### Representação em Memória

A forma como uma matriz é armazenada na memória depende da linguagem:

**Bloco Contíguo (Estilo C):** Em C, uma declaração como `int matriz[3][3]` aloca um único bloco contíguo de 9 inteiros na memória. A organização é em row-major order (ordem principal por linha): todos os elementos da linha 0 são seguidos por todos os elementos da linha 1, e assim por diante.

Memória: `[L0C0, L0C1, L0C2, L1C0, L1C1, L1C2, L2C0, L2C1, L2C2]`

O cálculo de endereço ainda é $O(1)$:

$Endereço(M[i][j]) = Base + (i \times NumColunas + j) \times TamanhoElemento$

**Array de Arrays (Estilo Java/Python):** Em Java, Python e JavaScript, uma matriz 2D é, na verdade, um vetor de vetores. A "matriz" é um vetor onde cada posição armazena uma referência (ponteiro) para outro vetor (a linha).

<div align="center">
<img width="700px" src="./img/07-matriz-02.png">
</div>

Esta imagem ilustra perfeitamente o modelo "array de arrays". Temos um vetor principal (vertical, não mostrado) onde cada posição aponta para um dos vetores-linha (horizontais, mostrados como verticais na imagem).

A vantagem desse modelo é a flexibilidade: as linhas não precisam ter o mesmo tamanho (criando "matrizes denteadas" ou _jagged arrays_). A desvantagem é uma leve perda de desempenho em relação ao modelo contíguo, pois o acesso requer duas buscas de referência (encontrar a linha, depois encontrar a coluna) e perde-se a localidade de cache entre as linhas.

## Operações e Implementação de Matrizes

A operação mais comum em matrizes é a travessia, que requer loops aninhados.

**Pseudocódigo (Travessia de Matriz 2D):**

```
função PercorrerMatriz(matriz)
    numLinhas = altura(matriz)
    numColunas = largura(matriz) // Assumindo que não é denteada
    
    para i de 0 até numLinhas - 1
        para j de 0 até numColunas - 1
            processar(matriz[i][j])
        fim_para
    fim_para
fim_função
```

- **Complexidade (Travessia):** **$O(L \times C)$**, onde L é o número de linhas e C o de colunas.

**Exemplo em C (Modelo Contíguo):**

```c
#include <stdio.h>

int main() {
    int matriz[3][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };
    printf("Elemento [1][2]: %d\n", matriz[1][2]); // Imprime 6

    // Travessia
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", matriz[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

**Exemplo em Java (Array de Arrays):**

```java
public class MatrizExemplo {
    public static void main(String[] args) {
        int[][] matriz = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        System.out.println("Elemento [1][2]: " + matriz[1][2]); // Imprime 6

        // Travessia
        for (int i = 0; i < matriz.length; i++) {
            for (int j = 0; j < matriz[i].length; j++) {
                System.out.print(matriz[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

**Exemplo em Python (Lista de Listas):**

```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print("Elemento [1][2]:", matriz[1][2])  # Imprime 6

# Travessia
for linha in matriz:
    for elemento in linha:
        print(elemento, end=" ")
    print()
```

**Exemplo em JavaScript (Array de Arrays):**

```javascript
let matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
console.log("Elemento [1][2]: " + matriz[1][2]); // Imprime 6

// Travessia
for (let i = 0; i < matriz.length; i++) {
    let linha = "";
    for (let j = 0; j < matriz[i].length; j++) {
        linha += matriz[i][j] + " ";
    }
    console.log(linha);
}
```

## Vantagens e Limitações

Os vetores e matrizes (estáticos) são definidos por suas características de memória.

**Vantagens:**

- **Acesso $O(1)$:** Acesso a qualquer elemento por índice é instantâneo.
- **Localidade de Cache:** Excelente desempenho em travessias sequenciais.
- **Eficiência de Espaço:** Nenhuma sobrecarga de memória (sem ponteiros extras por elemento, como em listas ligadas).

**Limitações:**

- **Tamanho Fixo:** O tamanho deve ser conhecido na criação e não pode ser alterado. Isso pode levar ao desperdício de espaço se alocado em excesso, ou falha se alocado de menos.
- **Inserção/Remoção $O(n)$:** Inserir ou remover elementos do meio da estrutura é uma operação cara, pois exige o deslocamento de muitos outros elementos.

## Considerações Finais

Vetores e matrizes são as estruturas de dados lineares mais fundamentais e servem como a base para a implementação de muitas outras. Sua maior força reside na alocação contígua de memória, que garante o acesso $O(1)$ e excelente localidade de cache, tornando-os ideais para algoritmos que realizam muitas leituras indexadas ou travessias sequenciais.

No entanto, sua rigidez (tamanho fixo e custo $O(n)$ para inserção/remoção) é uma limitação significativa. Foi para resolver este exato problema que a próxima estrutura de dados que estudaremos foi concebida: as **Listas Ligadas (ou Encadeadas)**. Elas abrem mão da memória contígua em troca de inserções e remoções $O(1)$, introduzindo um _trade-off_ completamente novo de desempenho que exploraremos no próximo capítulo.
# Capítulo 10 – Vetores e Matrizes (Arrays)

Nos capítulos anteriores, aprendemos a declarar variáveis para armazenar dados individuais, como um número inteiro, um caractere ou um valor de ponto flutuante. No entanto, muitas situações no desenvolvimento de software exigem que trabalhemos com coleções massivas de dados do mesmo tipo. Imagine um sistema para uma estação meteorológica que precisa armazenar a temperatura registrada a cada minuto durante um dia inteiro. Isso resultaria em $60 \times 24 = 1440$ valores. Seria absolutamente impraticável e ineficiente declarar 1440 variáveis distintas, como `temp1`, `temp2`, ..., `temp1440`. Além do esforço braçal, realizar operações simples, como calcular a média dessas temperaturas, exigiria uma fórmula gigantesca e manual.

É para solucionar esse problema de organização e manipulação de dados em massa que as linguagens C e C++ oferecem o conceito de **arrays** (ou arranjos). Eles podem ser unidimensionais, comumente chamados de **vetores**, ou multidimensionais, onde os arrays bidimensionais são frequentemente chamados de **matrizes**.

Os arrays nos permitem agrupar múltiplos valores de um mesmo tipo sob um único identificador (nome), acessando cada elemento individualmente através de um índice numérico. Essa estrutura não apenas organiza o código, mas também permite o uso de laços de repetição para processar grandes volumes de dados com poucas linhas de código.

Neste capítulo, exploraremos em profundidade a anatomia dos vetores e matrizes. Investigaremos como eles são alocados na memória do computador, como acessar e manipular seus elementos e os perigos críticos do acesso fora dos limites (_buffer overflow_). Discutiremos a complexidade de passar arrays para funções — um tópico onde a distinção entre arrays e ponteiros se torna difusa. Por fim, faremos a transição para o C++ moderno, apresentando os contêineres `std::vector` e `std::array`, que oferecem alternativas mais seguras e poderosas aos arrays primitivos do C.

## Vetores (Arrays Unidimensionais)

Um **vetor** em C é uma estrutura de dados linear que armazena uma coleção de tamanho fixo de elementos, todos obrigatoriamente do **mesmo tipo de dado**.

Para visualizar um vetor, imagine uma rua com uma série de casas.

1. **Contiguidade:** As casas são construídas lado a lado, sem espaços vazios entre elas. Da mesma forma, um vetor ocupa um bloco contínuo de memória RAM.
2. **Homogeneidade:** Todas as casas têm a mesma estrutura e tamanho. Um vetor de `int` só armazena inteiros; não é possível misturar `int` e `float` no mesmo vetor nativo.
3. **Indexação:** Cada casa possui um número (endereço) sequencial para identificação.

### Declaração de Vetores

Para declarar um vetor em C, deve-se especificar três componentes: o tipo de dado base, o nome do vetor e o número de elementos que ele conterá (sua dimensão).

A sintaxe geral é:

```
tipo_do_dado nome_do_vetor[tamanho_do_vetor];
```

- **`tipo_do_dado`**: Determina o tamanho de cada "célula" do vetor na memória (ex: 4 bytes para `int`, 8 bytes para `double`).
- **`nome_do_vetor`**: Um identificador válido que será usado para referenciar a coleção inteira.
- **`tamanho_do_vetor`**: Um número inteiro positivo que define quantas células o vetor terá.

**Regras de Tamanho:**

- No padrão **ANSI C (C89)**, o tamanho deve ser uma **expressão constante** conhecida em tempo de compilação (um literal como `10` ou uma constante definida via `#define`).
- O padrão **C99** introduziu os _Variable Length Arrays_ (VLAs), permitindo tamanhos definidos por variáveis em tempo de execução, mas seu uso tem restrições e não é suportado por todos os compiladores C++ (que exigem tamanho constante para arrays estáticos).

**Exemplos de Declaração:**

```c
#define MAX_ALUNOS 50
const int DIAS_SEMANA = 7; // Válido como tamanho em C++ e C99, mas não em C89 puro

int notas[5];                // Vetor de 5 inteiros.
float temperaturas[30];      // Vetor de 30 números de ponto flutuante.
char nome[100];              // Vetor de 100 caracteres (uma string).
double salarios[MAX_ALUNOS]; // Vetor de double com tamanho definido por macro.
```

### Alocação de Memória

Quando o compilador encontra a declaração `int notas[5];`, ele realiza o seguinte cálculo para reservar memória:

$$\text{Memória Total} = \text{Tamanho do Vetor} \times \text{sizeof(Tipo)}$$

Considerando que um `int` ocupe 4 bytes:

$$5 \times 4 \text{ bytes} = 20 \text{ bytes}$$

Esses 20 bytes são alocados sequencialmente. Se o vetor começar no endereço de memória `0x1000`:

- `notas[0]` está em `0x1000`
- `notas[1]` está em `0x1004`
- `notas[2]` está em `0x1008`
- ... e assim por diante.

### Acesso aos Elementos e Indexação Zero

Em C e C++, a indexação de vetores é **zero-based** (baseada em zero). Isso é uma herança direta da forma como o cálculo de endereços é feito (como um deslocamento a partir do início).

- O **primeiro** elemento está no índice **`0`**.
- O **segundo** elemento está no índice **`1`**.
- O **último** elemento de um vetor de tamanho $N$ está no índice **$N - 1$**.

Para acessar, ler ou modificar um elemento específico, utiliza-se o operador de subscrito `[]` após o nome da variável.

**Exemplo Prático:**

```c
#include <stdio.h>

int main() {
    // Declara um vetor de 3 inteiros.
    // A memória contém "lixo" neste momento.
    int numeros[3]; 

    // Atribuição de valores (Escrita)
    numeros[0] = 10; // Acessa a primeira posição
    numeros[1] = 20; // Acessa a segunda posição
    numeros[2] = 30; // Acessa a terceira (e última) posição

    // Acesso a valores (Leitura)
    printf("Primeiro elemento: %d\n", numeros[0]); 
    
    // Podemos usar elementos em expressões matemáticas
    int soma = numeros[0] + numeros[2];
    printf("Soma do primeiro e último: %d\n", soma); // Saída: 40

    // ERRO COMUM:
    // numeros[3] = 50; // ISSO É UM ERRO! O índice 3 está fora dos limites (0, 1, 2).
    
    return 0;
}
```

### Inicialização de Vetores

Em vez de atribuir valores linha por linha após a declaração, a linguagem C permite inicializar o vetor no momento da criação usando uma lista de valores entre chaves `{}`.

**1. Inicialização Completa:** Fornece-se um valor para cada posição alocada.

```c
int primos[5] = {2, 3, 5, 7, 11};
```

**2. Inicialização Parcial:** Se a lista de inicialização contiver menos valores do que o tamanho declarado, os elementos restantes são automaticamente preenchidos com zero. Isso é garantido pelo padrão da linguagem.

```c
int valores[5] = {10, 20}; 
// O vetor conterá: {10, 20, 0, 0, 0}
```

**3. Inicialização Universal com Zero:** Uma técnica muito comum para "limpar" um vetor inteiro é inicializar apenas o primeiro elemento com 0. O restante será preenchido automaticamente com 0.

```c
int contadores[100] = {0}; // Todos os 100 elementos serão 0.
```

**4. Tamanho Automático (Implícito):** Se o tamanho dentro dos colchetes for omitido durante a inicialização, o compilador contará os elementos fornecidos e definirá o tamanho do vetor automaticamente.

```c
float pesos[] = {70.5, 82.0, 55.3}; // O compilador cria um vetor de tamanho 3.
```

**Atenção:** Se um vetor local (dentro de uma função) for declarado sem inicialização (ex: `int v[10];`), seus elementos conterão **valores indefinidos** (lixo de memória residual). Sempre inicialize seus vetores se for lê-los antes de escrever.

### Iterando sobre Vetores

A grande força dos vetores surge quando combinados com estruturas de repetição. A forma mais comum de percorrer (iterar) um vetor é utilizando o laço `for`, usando a variável de controle do laço como índice do vetor.

**Algoritmo: Cálculo da Média de Notas**

**Pseudocódigo:**

```
DEFINIR CONSTANTE TAMANHO = 5
DECLARAR vetor notas[TAMANHO] do tipo REAL
DECLARAR soma = 0.0

// Entrada de Dados
PARA i DE 0 ATÉ TAMANHO - 1 FAÇA
    ESCREVA "Digite a nota " + (i + 1)
    LEIA valor
    notas[i] = valor
    soma = soma + notas[i] // Acumula o valor na soma
FIM-PARA

// Processamento
media = soma / TAMANHO

// Saída de Dados
ESCREVA "Notas digitadas: "
PARA i DE 0 ATÉ TAMANHO - 1 FAÇA
    ESCREVA notas[i] + " "
FIM-PARA
ESCREVA "Média final: " + media
```

**Implementação em C:**

```c
#include <stdio.h>

#define TAMANHO 5 // Constante para facilitar a manutenção

int main() {
    float notas[TAMANHO];
    float soma = 0.0f;
    int i; // Variável de índice

    // 1. Preenchendo o vetor e somando
    printf("Digite as %d notas:\n", TAMANHO);
    for (i = 0; i < TAMANHO; i++) {
        printf("Nota %d: ", i + 1);
        scanf("%f", &notas[i]); // Lê e armazena diretamente na posição 'i'
        soma += notas[i];       // Acumula para a média
    }

    // 2. Exibindo os valores armazenados
    printf("\nBoletim:\n");
    for (i = 0; i < TAMANHO; i++) {
        printf("Nota [%d]: %.1f\n", i, notas[i]);
    }

    // 3. Resultado final
    float media = soma / TAMANHO;
    printf("\nMédia da turma: %.2f\n", media);

    return 0;
}
```

### Acesso Fora dos Limites (Buffer Overflow)

Diferentemente de linguagens modernas como Java ou Python, **C e C++ (em arrays nativos) não verificam limites de acesso**. O compilador confia cegamente no programador.

Se você declarar `int vetor[5]` (índices válidos 0 a 4) e tentar acessar `vetor[10]`, o programa não emitirá um erro de compilação. Em tempo de execução, ele calculará o endereço de memória correspondente ao índice 10 e tentará ler ou escrever lá.

Isso leva a dois cenários catastróficos:

1. **Leitura fora dos limites:** O programa lê dados que pertencem a outras variáveis ou informações sensíveis da memória, resultando em comportamento imprevisível ("lixo").
2. **Escrita fora dos limites (Buffer Overflow):** O programa sobrescreve dados na memória adjacente. Isso pode corromper outras variáveis, alterar o fluxo de execução do programa (sobrescrevendo endereços de retorno na pilha) e é a principal causa de vulnerabilidades de segurança exploráveis por hackers.

Como regra, sempre certifique-se de que seu índice `i` satisfaça a condição `0 <= i < tamanho`.

## Matrizes (Arrays Multidimensionais)

A linguagem C suporta arrays com mais de uma dimensão. O caso mais comum é o **array bidimensional**, conhecido como **matriz**. Uma matriz pode ser visualizada como uma tabela ou planilha, composta por **linhas** e **colunas**.

### Declaração de Matrizes

A sintaxe para declarar uma matriz adiciona um segundo par de colchetes para representar a segunda dimensão.

```
tipo_do_dado nome[numero_de_linhas][numero_de_colunas];
```

**Exemplos:**

```c
int tabuleiro[8][8];     // Um tabuleiro de xadrez (64 inteiros)
float notas[10][4];      // Tabela com 10 alunos (linhas), 4 notas cada (colunas)
int imagem[1080][1920];  // Uma imagem Full HD (linhas x colunas de pixels)
```

### Organização na Memória (Row-Major Order)

Embora visualizemos uma matriz como uma grade retangular, a memória do computador é linear (unidimensional). O C armazena matrizes em **ordem de linha principal (Row-Major Order)**.

Isso significa que os elementos são armazenados linha por linha, sequencialmente:

1. Todos os elementos da Linha 0.
2. Imediatamente seguidos por todos os elementos da Linha 1.
3. E assim sucessivamente.

Para uma matriz `M[2][3]`, a ordem na memória é: `M[0][0]`, `M[0][1]`, `M[0][2]`, `M[1][0]`, `M[1][1]`, `M[1][2]`.

### Acesso aos Elementos

Para acessar um elemento, deve-se fornecer os dois índices: o da linha e o da coluna.

```c
int matriz[3][3];

matriz[0][0] = 10;  // Canto superior esquerdo
matriz[1][1] = 50;  // Centro
matriz[2][2] = 90;  // Canto inferior direito
```

### Inicialização de Matrizes

A inicialização pode ser feita de forma linear ou, para maior clareza, utilizando chaves aninhadas para representar cada linha.

**Inicialização Aninhada (Recomendada):**

```c
int grade[2][3] = {
    {1, 2, 3},   // Valores para a Linha 0
    {4, 5, 6}    // Valores para a Linha 1
};
```

**Inicialização Parcial:**

```c
int matriz_b[2][3] = {
    {10, 20},    // Linha 0: Terceiro elemento será 0
    {30}         // Linha 1: Segundo e terceiro elementos serão 0
};
```

### Iterando sobre Matrizes

Para percorrer uma matriz, utilizamos **laços aninhados**. O laço externo geralmente controla a linha, enquanto o laço interno percorre as colunas daquela linha.

Exemplo: Preenchendo e Exibindo uma Matriz Identidade

Uma matriz identidade é uma matriz quadrada onde os elementos da diagonal principal são 1 e os demais são 0.

```c
#include <stdio.h>

#define DIM 4 // Matriz 4x4

int main() {
    int identidade[DIM][DIM];
    int linha, coluna;

    // 1. Processamento: Preencher a matriz
    for (linha = 0; linha < DIM; linha++) {           // Laço das linhas
        for (coluna = 0; coluna < DIM; coluna++) {    // Laço das colunas
            if (linha == coluna) {
                identidade[linha][coluna] = 1; // Diagonal principal
            } else {
                identidade[linha][coluna] = 0; // Restante
            }
        }
    }

    // 2. Saída: Exibir a matriz formatada
    printf("Matriz Identidade %dx%d:\n", DIM, DIM);
    for (linha = 0; linha < DIM; linha++) {
        printf("| "); // Decoração visual
        for (coluna = 0; coluna < DIM; coluna++) {
            printf("%d ", identidade[linha][coluna]);
        }
        printf("|\n"); // Quebra de linha ao final de cada linha da matriz
    }

    return 0;
}
```

**Saída:**

```
Matriz Identidade 4x4:
| 1 0 0 0 |
| 0 1 0 0 |
| 0 0 1 0 |
| 0 0 0 1 |
```

## Vetores e Matrizes como Parâmetros de Funções

Passar arrays para funções em C é um tópico que exige atenção especial, pois a linguagem utiliza um mecanismo que difere da passagem de variáveis simples (como `int` ou `float`).

### Decaimento para Ponteiro (Decay to Pointer)

Quando passamos um vetor para uma função, o C **não faz uma cópia** de todos os elementos do vetor. Isso seria extremamente ineficiente para vetores grandes. Em vez disso, o nome do vetor "decaí" (é convertido automaticamente) para um **ponteiro para o primeiro elemento**.

Isso tem três implicações fundamentais:

1. **Eficiência:** A passagem é instantânea, independentemente do tamanho do vetor, pois apenas um endereço de memória é copiado.
2. **Referência:** Como a função recebe o endereço da memória original, qualquer alteração feita nos elementos do vetor dentro da função **modificará o vetor original**.
3. **Perda de Informação de Tamanho:** A função recebe apenas o endereço de início. Ela **não sabe onde o vetor termina**. Por isso, é quase sempre obrigatório passar o tamanho do vetor como um argumento adicional.

**Sintaxe para Vetores Unidimensionais:**

O parâmetro pode ser declarado como tipo `nome[]` ou tipo `*nome`. Ambos são interpretados da mesma forma pelo compilador.

```c
#include <stdio.h>

// A função recebe o vetor (ponteiro) e seu tamanho explícito
void dobrar_valores(int vetor[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        vetor[i] = vetor[i] * 2; // Modifica a memória original!
    }
}

void imprimir_vetor(int vetor[], int tamanho) {
    printf("[ ");
    for (int i = 0; i < tamanho; i++) {
        printf("%d ", vetor[i]);
    }
    printf("]\n");
}

int main() {
    int numeros[] = {1, 2, 3, 4, 5};
    // Truque comum para calcular o tamanho de arrays estáticos:
    // Tamanho total em bytes / Tamanho de um elemento em bytes
    int qtd = sizeof(numeros) / sizeof(numeros[0]);

    printf("Original: ");
    imprimir_vetor(numeros, qtd);

    dobrar_valores(numeros, qtd);

    printf("Dobrado:  ");
    imprimir_vetor(numeros, qtd); // Os valores originais foram alterados

    return 0;
}
```

### Passando Matrizes para Funções

Para matrizes (arrays bidimensionais), a regra é um pouco mais rígida. Ao declarar o parâmetro da função, **é obrigatório informar o tamanho de todas as dimensões, exceto a primeira (número de linhas)**.

Por que? O compilador precisa saber quantas colunas existem para calcular corretamente o endereço de memória da próxima linha. Se a matriz tem 4 colunas de inteiros, o início da linha 1 está a exatamente $4 \times 4 = 16$ bytes do início da linha 0. Sem o número de colunas, o compilador não consegue fazer essa aritmética de ponteiros.

```c
#include <stdio.h>

#define COLUNAS 3

// 'linhas' pode ser variável, mas 'COLUNAS' deve ser fixo e conhecido
void imprimir_matriz(int m[][COLUNAS], int linhas) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            printf("%d ", m[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int mat[2][COLUNAS] = {{1, 2, 3}, {4, 5, 6}};
    imprimir_matriz(mat, 2);
    return 0;
}
```

## C++ Moderno: `std::vector` e `std::array`

Embora o C++ suporte totalmente os arrays estilo C ("C-style arrays") que vimos até agora, a biblioteca padrão do C++ (STL) oferece contêineres que resolvem as principais limitações e riscos de segurança dos arrays primitivos. Em C++ moderno, o uso desses contêineres é fortemente preferido.

### `std::vector` (Array Dinâmico)

O `std::vector` (da biblioteca `<vector>`) é o contêiner sequencial mais versátil do C++. Diferente dos vetores em C, ele é **dinâmico**: pode crescer e diminuir de tamanho durante a execução.

- **Gerenciamento de Memória:** Automático. Você não precisa calcular bytes ou liberar memória.
- **Flexibilidade:** Pode-se adicionar elementos com `push_back()`.
- **Segurança:** Oferece o método `.at()` que verifica se o índice é válido (lançando um erro se não for), prevenindo _buffer overflows_ acidentais.
- **Consciência:** O vetor sabe seu próprio tamanho através do método `.size()`.

```cpp
#include <iostream>
#include <vector>

int main() {
    // Cria um vetor vazio de inteiros
    std::vector<int> notas;

    // Adiciona elementos dinamicamente (o vetor cresce automaticamente)
    notas.push_back(10);
    notas.push_back(20);
    notas.push_back(30);

    std::cout << "Tamanho atual: " << notas.size() << std::endl;

    // Acesso seguro com .at()
    try {
        std::cout << "Elemento 1: " << notas.at(1) << std::endl; // Saída: 20
        // notas.at(100) = 50; // Isso lançaria uma exceção, evitando corrupção de memória
    } catch (const std::out_of_range& e) {
        std::cerr << "Erro: Índice fora dos limites!" << std::endl;
    }

    // Iteração moderna (Range-based for loop - C++11)
    std::cout << "Elementos: ";
    for (int n : notas) {
        std::cout << n << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### `std::array` (Array Fixo Seguro)

Introduzido no C++11, o `std::array` (da biblioteca `<array>`) é um substituto direto para os arrays fixos do C (ex: `int v[5]`).

- **Eficiência:** Tem custo zero de performance em relação ao array C (ocupa a mesma memória e é alocado na pilha).
- **Semântica de Valor:** Diferente dos arrays C, um `std::array` não decai para ponteiro. Ele pode ser copiado com `=` e passado para funções por valor ou referência de forma intuitiva.
- **Interface STL:** Oferece métodos como `.size()`, `.at()`, iteradores, etc.

```c
#include <iostream>
#include <array>

// Passagem por referência (para evitar cópia) e const (apenas leitura)
void imprimir(const std::array<int, 3>& arr) { 
    std::cout << "Tamanho conhecido: " << arr.size() << "\nValores: ";
    for (int x : arr) std::cout << x << " ";
    std::cout << std::endl;
}

int main() {
    // Declaração: tipo, tamanho
    std::array<int, 3> a = {10, 20, 30};
    
    imprimir(a);
    
    // Cópia fácil
    std::array<int, 3> b = a; 
    b[0] = 99; // Modifica apenas b, 'a' permanece intacto
    
    return 0;
}
```

## Considerações Finais

Neste capítulo, estabelecemos as bases para o armazenamento de coleções de dados. Começamos desvendando os **vetores (arrays unidimensionais)** em C, compreendendo sua alocação contígua na memória, a importância da indexação baseada em zero e os riscos críticos de segurança envolvidos no acesso fora dos limites. Aprendemos a iterar sobre eles para processar dados em massa.

Expandimos nossa visão para as **matrizes (arrays multidimensionais)**, entendendo sua linearização na memória (row-major order) e como manipulá-las com laços aninhados. Também desmistificamos o comportamento dos arrays ao serem passados para funções, revelando o conceito de "decaimento para ponteiro" e a necessidade de gerenciar tamanhos manualmente em C.

Por fim, conectamos o passado ao futuro apresentando as soluções do C++ moderno: `std::vector`, para arrays dinâmicos e flexíveis, e `std::array`, para arrays fixos com a eficiência do C mas a segurança de tipos do C++. O domínio dessas estruturas é o passo final antes de mergulharmos no conceito mais poderoso e temido da linguagem C, que nos dará controle total sobre a memória: os **Ponteiros**, tema do nosso próximo capítulo.
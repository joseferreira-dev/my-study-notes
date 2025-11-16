# Capítulo 9 – Estruturas de Repetição (Laços)

Nos capítulos anteriores, exploramos como os programas podem tomar decisões e seguir diferentes caminhos de execução com base em condições, utilizando as estruturas de decisão `if` e `switch`. No entanto, muitas tarefas em programação envolvem a repetição de um mesmo conjunto de instruções várias vezes. Realizar essas repetições manualmente seria não apenas tedioso, mas também impraticável. É aqui que entram as **estruturas de repetição**, também conhecidas como **laços** ou **loops**.

As estruturas de repetição são construções da linguagem que permitem que um bloco de código seja executado repetidamente. Elas são fundamentais para automatizar tarefas (como processar todos os itens de uma lista), realizar cálculos iterativos (como somar uma série) e implementar algoritmos que exigem passos sucessivos (como ordenar um array). Sem os laços, a programação de tarefas complexas seria imensamente mais difícil e o código, muito mais verboso.

Neste capítulo, investigaremos as três principais estruturas de repetição disponíveis em C e C++:

1. **`while` (Laço Pré-Teste):** Executa um bloco _enquanto_ uma condição for verdadeira.
2. **`do-while` (Laço Pós-Teste):** Executa um bloco _pelo menos uma vez_ e, então, repete enquanto a condição for verdadeira.
3. **`for` (Laço Controlado):** Uma estrutura compacta ideal para repetições contadas ou com controle explícito de inicialização, condição e passo.

Discutiremos também as instruções `break` e `continue`, que permitem um controle mais fino sobre o fluxo de execução dentro dos laços. Por fim, faremos a transição para C++, apresentando o moderno **laço `for` baseado em intervalo (range-based for)**, que simplifica a iteração sobre coleções.

## Laço `while` (Repetição com Pré-Teste)

A estrutura de repetição **`while`** é a mais fundamental. Ela executa um bloco de código repetidamente **enquanto** uma determinada condição de teste for verdadeira. A característica que define o `while` é ser um **laço pré-teste**: a condição é avaliada **antes** de cada possível execução do bloco de código.

Isso significa que, se a condição for falsa logo na primeira verificação, o bloco de código dentro do `while` **nunca será executado**.

**Pseudocódigo:**

```
ENQUANTO (condicao for verdadeira) FAÇA
    // Bloco de código a ser repetido
FIM-ENQUANTO
// Continua a execução aqui
```

**Sintaxe em C/C++:**

```c
while (condicao) {
    // Bloco de código...
    //
    // É crucial que algo dentro deste bloco
    // (ou um evento externo) eventualmente
    // torne a 'condicao' falsa.
}
// Próxima instrução após o laço
```

Onde:

- `condicao`: É a expressão booleana (ou inteira em C). Enquanto avaliar para `true` (C++) ou _não-zero_ (C), o bloco é executado. Quando avaliar para `false` (C++) ou `0` (C), o laço termina.

**Fluxo de Execução:**

1. Testa a `condicao`.
2. Se a `condicao` for falsa, pula o bloco e vai para a instrução após o `while`.
3. Se a `condicao` for verdadeira, executa o bloco de código.
4. Ao final do bloco, volta para o passo 1 (re-testa a `condicao`).

### Exemplo 1: Laço Controlado por Contador

Este é o uso mais simples, onde usamos uma variável para contar o número de iterações.

```c
#include <stdio.h>

int main() {
    int contador = 1; // 1. Inicialização da variável de controle

    printf("Iniciando contagem com while:\n");
    
    while (contador <= 5) { // 2. Condição de teste
        printf("%d ", contador);
        contador++; // 3. Atualização da variável de controle (crucial!)
    }
    
    printf("\nContagem com while finalizada.\n");
    return 0;
}
```

**Saída:**

```
Iniciando contagem com while:
1 2 3 4 5 
Contagem com while finalizada.
```

**Rastreamento:**

- **Iteração 1:** `contador` é 1. `1 <= 5` é VERDADEIRO. Imprime "1". `contador` vira 2.
- **Iteração 2:** `contador` é 2. `2 <= 5` é VERDADEIRO. Imprime "2". `contador` vira 3.
- ...
- **Iteração 5:** `contador` é 5. `5 <= 5` é VERDADEIRO. Imprime "5". `contador` vira 6.
- **Iteração 6:** `contador` é 6. `6 <= 5` é FALSO. O laço termina.

Se `contador` fosse inicializado com `10`, a condição `10 <= 5` seria falsa na primeira verificação, e o laço não executaria nenhuma vez.

### Exemplo 2: Laço Controlado por Sentinela

Um uso mais comum do `while` é em laços onde não sabemos o número de iterações, mas esperamos por um valor especial (um "sentinela") para parar.

```c
#include <stdio.h>

// Programa que soma números até que o usuário digite 0.
int main() {
    int numero;
    int soma = 0;
    
    printf("Digite um número (ou 0 para parar): ");
    scanf("%d", &numero); // 1. Leitura inicial (priming read)
    
    while (numero != 0) { // 2. Testa pelo valor sentinela
        soma += numero; // Processa o valor
        
        printf("Digite outro número (ou 0 para parar): ");
        scanf("%d", &numero); // 3. Lê o próximo valor antes de re-testar
    }
    
    printf("A soma total é: %d\n", soma);
    return 0;
}
```

## Laço `do-while` (Repetição com Pós-Teste)

O laço **`do-while`** é uma variação do `while`. Sua diferença crucial é que ele é um **laço pós-teste**: a condição de teste é avaliada **após** a execução do bloco de código.

Isso dá ao `do-while` uma garantia única: o bloco de código será executado **pelo menos uma vez**, independentemente de a condição ser verdadeira ou falsa na primeira vez.

**Pseudocódigo:**

```
FAÇA
    // Bloco de código a ser repetido
ENQUANTO (condicao for verdadeira)
```

**Sintaxe em C/C++:**

```c
do {
    // Bloco de código a ser executado.
    // Sempre executa pelo menos uma vez.
} while (condicao); // Ponto e vírgula (;) é OBRIGATÓRIO aqui!
```

**Fluxo de Execução:**

1. Executa o bloco de código.
2. Testa a `condicao`.
3. Se a `condicao` for verdadeira, volta para o passo 1.
4. Se a `condicao` for falsa, o laço termina.

### Exemplo 1: A Garantia de "Executar uma Vez"

Vamos comparar o `while` e o `do-while` com uma condição que já começa falsa.

```c
#include <stdio.h>

int main() {
    int x = 10;
    
    printf("Testando o 'while' (pré-teste):\n");
    while (x < 5) {
        printf("  Dentro do 'while'\n"); // Esta linha NUNCA executa
    }
    
    printf("\nTestando o 'do-while' (pós-teste):\n");
    do {
        printf("  Dentro do 'do-while'\n"); // Esta linha EXECUTA UMA VEZ
    } while (x < 5); // A condição (10 < 5) é testada como FALSA aqui
    
    printf("Fim do programa.\n");
    return 0;
}
```

**Saída:**

```
Testando o 'while' (pré-teste):

Testando o 'do-while' (pós-teste):
  Dentro do 'do-while'
Fim do programa.
```

### Exemplo 2: Validação de Entrada (Menu)

O `do-while` é perfeito para validação de entrada, onde você precisa _primeiro_ pedir os dados ao usuário e _depois_ verificar se eles são válidos. O exemplo do menu do rascunho é excelente.

```c
#include <stdio.h>

int main() {
    char opcao;

    do {
        // 1. O bloco executa primeiro
        printf("\n--- Menu ---\n");
        printf("1 - Iniciar Jogo\n");
        printf("2 - Carregar Jogo\n");
        printf("3 - Opções\n");
        printf("4 - Sair\n");
        printf("Escolha uma opção: ");
        
        // Lê a entrada
        scanf(" %c", &opcao); // O espaço antes de %c consome '\n' pendentes

        // Processa a entrada
        switch (opcao) {
            case '1': printf("Iniciando novo jogo...\n"); break;
            case '2': printf("Carregando jogo salvo...\n"); break;
            case '3': printf("Abrindo opções...\n"); break;
            case '4': printf("Saindo do programa...\n"); break;
            default:  printf("Opção inválida! Tente novamente.\n");
        }
        
    // 2. A condição é testada no final
    } while (opcao != '4'); // Repete enquanto a opção não for '4' (Sair)

    printf("Programa finalizado.\n");
    return 0;
}
```

## Laço `for` (Repetição Controlada)

O laço **`for`** é uma estrutura de repetição poderosa e flexível, otimizada para **repetições contadas**. Ele consolida os três elementos essenciais de um laço controlado por contador em uma única linha de cabeçalho:

1. **Inicialização:** O que fazer antes do laço começar (ex: `int i = 0`).
2. **Condição:** O teste para continuar (ex: `i < 10`).
3. **Incremento/Passo:** O que fazer após cada iteração (ex: `i++`).

**Pseudocódigo:**

```
PARA (inicialização; condição; passo) FAÇA
    // Bloco de código a ser repetido
FIM-PARA
```

**Sintaxe em C/C++:**

```c
for (inicializacao; condicao; passo) {
    // Bloco de código...
}
```

Fluxo de Execução Detalhado:

O for pode parecer complexo, mas seu fluxo é muito preciso:

1. **`inicializacao`**: Esta expressão é executada **apenas uma vez**, no início do laço.
2. **`condicao`**: Esta expressão é avaliada.
3. Se a `condicao` for **falsa**, o laço termina imediatamente (assim como o `while`, o `for` é um laço **pré-teste** e pode nunca executar seu bloco).
4. Se a `condicao` for **verdadeira**, o **Bloco de código** é executado.
5. **`passo`**: _Após_ o bloco de código terminar, a expressão de `passo` (geralmente incremento ou decremento) é executada.
6. O fluxo **volta para o passo 2** (re-avaliação da `condicao`).

### Exemplo: Contagem Progressiva e Regressiva

```c
#include <stdio.h>

int main() {
    int i; // 1. Pode ser declarado fora (padrão C89)

    printf("Contagem progressiva com for (C89 style):\n");
    //     Passo 1    Passo 2, 6, 10...  Passo 4, 8, 12...
    for (i = 1;     i <= 5;             i++) {
        // Passo 3, 7, 11...
        printf("%d ", i);
    }
    printf("\n");
    // 'i' ainda existe aqui e seu valor é 6.
    printf("Valor final de i: %d\n", i); 

    printf("\nContagem regressiva (C99/C++ style):\n");
    // 'j' é declarado no escopo do 'for'
    for (int j = 5; j >= 1; j--) {
        printf("%d ", j);
    }
    printf("\n");
    // printf("Valor de j: %d\n", j); // ERRO! 'j' não existe neste escopo.
    
    printf("Contagem com for finalizada.\n");
    return 0;
}
```

**Saída:**

```
Contagem progressiva com for (C89 style):
1 2 3 4 5 
Valor final de i: 6

Contagem regressiva (C99/C++ style):
5 4 3 2 1 
Contagem com for finalizada.
```

### Escopo da Variável no `for` (C99/C++)

Como mostrado no exemplo, o padrão C99 (e todas as versões de C++) permite declarar a variável de controle dentro da inicializacao.

```c
for (int j = 0; ...)
```

Isso é considerado uma prática muito superior, pois limita o **escopo** da variável `j` apenas ao laço `for`. Ela não "vaza" para o resto da função, evitando conflitos de nomes.

### Variações e Flexibilidade do `for`

Qualquer uma das três expressões no cabeçalho do `for` é opcional.

- **Laço `for` sem inicialização:**
    
    ```c
    int k = 0; // Inicialização fora
    for (; k < 3; k++) { // Primeira parte vazia
        printf("%d ", k);
    }
    // Saída: 0 1 2
    ```
    
- **Laço `for` sem incremento (o incremento deve ocorrer dentro do bloco):**
    
    ```c
    for (int m = 0; m < 10; ) { // Terceira parte vazia
        if (m % 2 != 0) {
            m += 2; // Pula de 1 para 3, 3 para 5...
        } else {
            m++;    // Pula de 0 para 1, 2 para 3...
        }
        printf("%d ", m);
    }
    // Saída: 1 3 5 7 9 11 (Opa, o 11 é impresso antes do teste de m < 10 falhar)
    // Este é um exemplo complexo, mas ilustra a flexibilidade.
    ```
    
- **Laço `for` apenas com condição (Equivalente a um `while`):**
    
    ```c
    int n = 0;
    for (; n < 5 ;) { // Equivalente a while(n < 5)
        printf("%d ", n);
        n++;
    }
    // Saída: 0 1 2 3 4
    ```
    
- **Laço `for` infinito (sem condição):**
    
    ```c
    for (;;) {
        // Laço infinito, precisa de um 'break' interno para sair
        printf("Infinito... ");
    }
    ```
    
- Múltiplas inicializações ou incrementos (usando o operador vírgula): O operador vírgula (`,`) permite avaliar múltiplas expressões onde apenas uma é esperada.
    
    ```c
    #include <stdio.h>
    int main() {
        int x, y;
        // Inicializa x E y; Incrementa x E decrementa y
        for (x = 0, y = 10; x < y; x++, y--) {
            printf("(x=%d, y=%d) ", x, y);
        }
        // Saída: (x=0, y=10) (x=1, y=9) (x=2, y=8) (x=3, y=7) (x=4, y=6) 
        return 0;
    }
    ```

## Controle de Laços com `break` e `continue`

Às vezes, precisamos alterar o fluxo de um laço _durante_ sua execução. As instruções `break` e `continue` servem para isso.

### Instrução `break`

A instrução **`break`** tem dois usos:

1. Sair de uma estrutura `switch`.
2. **Terminar imediatamente a execução do laço _mais interno_** em que ela se encontra (`while`, `do-while` ou `for`).

A execução do programa continua com a primeira instrução _após_ o laço que foi terminado.

**Exemplo: `break` em um Laço Simples (Busca)**

```c
#include <stdio.h>

int main() {
    int numero_procurado = 5;
    printf("Procurando o número %d em um laço de 1 a 10:\n", numero_procurado);
    
    for (int i = 1; i <= 10; i++) {
        printf("%d ", i);
        if (i == numero_procurado) {
            printf("\nNúmero %d encontrado! Saindo do laço.\n", numero_procurado);
            break; // Termina o 'for' imediatamente
        }
    }
    printf("Após o laço.\n");
    return 0;
}
```

**Saída:**

```
Procurando o número 5 em um laço de 1 a 10:
1 2 3 4 5 
Número 5 encontrado! Saindo do laço.
Após o laço.
```

O laço não continua até 10; ele é interrompido no 5.

**Exemplo: break em Laços Aninhados**

Esta é uma "pegadinha" comum: break NÃO sai de todos os laços, apenas do mais interno.

```c
#include <stdio.h>
#include <stdbool.h>

int main() {
    bool item_encontrado = false; // Flag de controle

    for (int i = 1; i <= 3; i++) { // Laço externo
        printf("Laço Externo (i=%d)\n", i);
        
        for (int j = 1; j <= 3; j++) { // Laço interno
            printf("  Laço Interno (j=%d)\n", j);
            if (i == 2 && j == 2) {
                printf("    ACHEI! (i=%d, j=%d). Dando BREAK...\n", i, j);
                item_encontrado = true; // 1. Seta a flag
                break; // 2. Sai APENAS do laço 'j' (interno)
            }
        }
        
        // 3. Verifica a flag para sair do laço 'i' (externo)
        if (item_encontrado == true) {
            break; 
        }
    }
    printf("Fim dos laços.\n");
    return 0;
}
```

**Saída:**

```
Laço Externo (i=1)
  Laço Interno (j=1)
  Laço Interno (j=2)
  Laço Interno (j=3)
Laço Externo (i=2)
  Laço Interno (j=1)
  Laço Interno (j=2)
    ACHEI! (i=2, j=2). Dando BREAK...
Fim dos laços.
```

Observe que o `break` interno (passo 2) apenas parou o `for (j...)`. O `Laço Externo (i=3)` nunca foi executado porque o `break` externo (passo 3) foi ativado pela flag.

### Instrução `continue`

A instrução **`continue`** não termina o laço; ela apenas **interrompe a iteração atual** e **inicia imediatamente a próxima iteração**.

Qualquer código restante na iteração atual após o `continue` é ignorado.

**O que `continue` faz:**

- Em um laço `while` ou `do-while`: Salta diretamente para a avaliação da **condição**.
- Em um laço `for`: Salta diretamente para a expressão de **incremento/passo** (e _depois_ reavalia a condição).

**Exemplo: `continue` em um laço `for` (Imprimir apenas números ímpares)**

```c
#include <stdio.h>

int main() {
    printf("Imprimindo números ímpares de 1 a 10:\n");
    for (int i = 1; i <= 10; i++) {
        if (i % 2 == 0) { // Se 'i' for par
            continue;     // Pula o resto do bloco e vai para o 'i++'
        }
        
        // Esta linha só é alcançada se 'i' for ímpar
        printf("%d ", i); 
    }
    printf("\nFim.\n");
    return 0;
}
```

**Saída:**

```
Imprimindo números ímpares de 1 a 10:
1 3 5 7 9 
Fim.
```

**Armadilha: continue em um `while`**

Cuidado ao usar continue em um `while`. Se você pular a linha de atualização da variável, pode criar um laço infinito.

```c
// EXEMPLO DE BUG
int i = 0;
while (i < 5) {
    if (i == 3) {
        continue; // Pula de volta para o 'while (i < 5)'
    }
    printf("%d ", i);
    i++; // ESTA LINHA É PULADA QUANDO i == 3!
}
// Saída: 0 1 2 (e então entra em loop infinito testando 'while(3 < 5)' para sempre)
```

## Laços Infinitos

Um **laço infinito** ocorre quando a condição de terminação de um laço nunca se torna falsa. Isso faz com que o programa "trave", executando o mesmo bloco de código indefinidamente.

**Causas Comuns:**

- **Condição Sempre Verdadeira:**
    
    ```c
    while (1) { // 1 é sempre verdadeiro (não-zero)
        // ...
    }
    // Em C++
    while (true) {
        // ...
    }
    ```
    
- **`for` sem Condição:**
    
    ```c
    for (;;) { // Forma idiomática em C/C++ para um laço infinito
        // Geralmente tem um 'if(...){ break; }' aqui dentro.
    }
    ```
    
- **Erro de Lógica (Bug):** Esquecer de atualizar a variável de controle (como no exemplo do `continue` acima) ou atualizá-la incorretamente.
    
    ```c
    int i = 10;
    while (i > 5) {
        printf("%d ", i);
        i++; // Erro de lógica! 'i' só aumenta (11, 12, 13...)
             // A condição 'i > 5' será sempre verdadeira.
    }
    ```

Laços infinitos são usados intencionalmente em programação de sistemas (ex: o loop principal de um servidor ou sistema operacional) ou em sistemas embarcados, mas em programas de aplicação geral, eles são quase sempre um bug, a menos que contenham um `break` condicional.

## Laço `for` Baseado em Intervalo (Range-Based For)

C++11 introduziu uma sintaxe de `for` muito mais simples e segura para iterar sobre _coleções_ (como arrays, `std::vector`, `std::string`, etc.). Esta forma de laço elimina a necessidade de gerenciar contadores (`int i = 0;`), condições (`i < tamanho;`) e incrementos (`i++`).

**Sintaxe em C++11:**

```cpp
for (declaracao_elemento : colecao) {
    // ... usa 'declaracao_elemento' ...
}
```

- `colecao`: O array ou contêiner que você quer percorrer.
- `declaracao_elemento`: Uma variável que, a cada iteração, receberá uma _cópia_ de um elemento da coleção.

**Exemplo 1: Iterando sobre um array e `std::vector`**

```cpp
#include <iostream>
#include <vector> // Para std::vector
#include <string> // Para std::string

int main() {
    // 1. Iterando sobre um array C-style
    int numeros_array[] = {1, 2, 3, 4, 5};
    std::cout << "Array: ";
    
    // 'auto' permite ao compilador deduzir o tipo (int)
    for (auto num : numeros_array) { 
        std::cout << num << " ";
    }
    std::cout << std::endl; // Saída: Array: 1 2 3 4 5

    // 2. Iterando sobre um std::vector
    std::vector<std::string> palavras = {"Olá", "Mundo", "C++"};
    std::cout << "Vetor de strings: ";
    
    // 'palavra' será uma CÓPIA de cada string no vetor
    for (std::string palavra : palavras) { 
        std::cout << palavra << " ";
    }
    std::cout << std::endl; // Saída: Vetor de strings: Olá Mundo C++
    
    return 0;
}
```

### Otimização e Modificação: Usando Referências (`&`)

O `range-based for` por padrão _copia_ cada elemento. Isso pode ser lento para objetos grandes (como strings ou vetores).

- **Para Modificar (Referência `&`):** Se você quiser modificar os elementos originais, use uma referência (`&`).
- **Para Ler (Referência Constante `const &`):** Se você quer apenas ler os elementos, mas quer evitar a lentidão da cópia, use uma referência constante (`const &`). Esta é a forma mais comum e eficiente de iterar para leitura.

```c
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<int> numeros = {10, 20, 30};

    // 1. Modificando com Referência (auto&)
    // 'num' AQUI NÃO É UMA CÓPIA, É O PRÓPRIO ELEMENTO DENTRO DO VETOR
    std::cout << "Dobrando valores: ";
    for (auto& num : numeros) { 
        num = num * 2; // Modifica o vetor original
    }
    
    // 2. Lendo com Referência Constante (const auto&) - EFICIENTE
    // 'num' é uma referência, mas 'const' impede a modificação.
    // Evita a cópia, rápido e seguro.
    std::cout << "Valores dobrados: ";
    for (const auto& num : numeros) {
        // num = 5; // ERRO DE COMPILAÇÃO (pois num é const)
        std::cout << num << " "; 
    }
    std::cout << std::endl; // Saída: 20 40 60
    
    return 0;
}
```

## Considerações Finais

Neste capítulo, exploramos as estruturas de repetição, ferramentas indispensáveis para a automação de tarefas e a implementação de algoritmos iterativos. Detalhamos o funcionamento dos três laços principais:

- **`while`**: Um laço com pré-teste, ideal para quando o número de iterações é desconhecido e depende de uma condição que pode ser falsa desde o início.
- **`do-while`**: Um laço com pós-teste, que garante a execução do bloco de código ao menos uma vez, perfeito para validação de entrada e menus.
- **`for` (tradicional)**: Um laço controlado e compacto, ideal para repetições contadas, onde a inicialização, a condição e o passo são claramente definidos.

Discutimos também como as instruções `break` e `continue` permitem um controle mais fino do fluxo, possibilitando a interrupção prematura de um laço ou o salto para a próxima iteração. Alertamos para os perigos dos laços infinitos e a importância de garantir condições de terminação.

Finalmente, apresentamos o laço `for` baseado em intervalo (range-based) do C++11, uma adição que simplifica drasticamente a iteração sobre coleções, e destacamos o uso de referências (`&` e `const &`) para modificação e eficiência.

Com o domínio das estruturas de decisão e de repetição, possuímos agora o conjunto completo de ferramentas para controlar o fluxo de execução. Estamos prontos para organizar nossos dados de forma mais complexa, começando pelo próximo tópico: **Arrays (Vetores)**.
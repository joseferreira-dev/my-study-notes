# Capítulo 9 – Estruturas de Repetição

Nos capítulos anteriores, exploramos como os programas podem tomar decisões e seguir diferentes caminhos de execução com base em condições, utilizando as estruturas de decisão. No entanto, muitas tarefas em programação envolvem a repetição de um mesmo conjunto de instruções várias vezes. Realizar essas repetições manualmente seria não apenas tedioso, mas também impraticável para um grande número de iterações. É aqui que entram as **estruturas de repetição**, também conhecidas como **laços** ou **loops**.

As estruturas de repetição são construções da linguagem que permitem que um bloco de código seja executado repetidamente enquanto uma determinada condição for satisfeita, ou por um número específico de vezes. Elas são fundamentais para automatizar tarefas, processar coleções de dados (como arrays), realizar cálculos iterativos e implementar algoritmos que exigem passos sucessivos. Sem os laços, a programação de tarefas complexas seria imensamente mais difícil e o código, muito mais verboso.

Neste capítulo, investigaremos as principais estruturas de repetição disponíveis em C e C++. Começaremos com o laço `while`, que executa um bloco de código enquanto uma condição de teste permanecer verdadeira. Em seguida, abordaremos o laço `do-while`, similar ao `while`, mas que garante a execução do bloco de código pelo menos uma vez. Depois, exploraremos o laço `for`, uma estrutura versátil e frequentemente utilizada para iterações com contadores ou sobre sequências. Discutiremos também as instruções `break` e `continue`, que permitem um controle mais fino sobre o fluxo de execução dentro dos laços. Ao final, você terá as ferramentas necessárias para implementar algoritmos iterativos e gerenciar tarefas repetitivas de forma eficiente e elegante.

## O Laço `while` (Repetição com Pré-Teste)

A estrutura de repetição **`while`** é uma das mais simples e fundamentais. Ela executa um bloco de código repetidamente **enquanto** uma determinada condição de teste for verdadeira. A condição é avaliada **antes** de cada possível execução do bloco de código. Se a condição for falsa logo na primeira verificação, o bloco de código dentro do `while` nunca será executado.

A sintaxe básica do laço `while` em C é:

```c
while (condicao) {
    // Bloco de código a ser executado
    // enquanto a 'condicao' for verdadeira (não-zero em C)
    // É crucial que alguma instrução dentro deste bloco,
    // ou um evento externo, eventualmente torne a 'condicao' falsa,
    // para evitar um laço infinito.
}
// Próxima instrução após o laço while
```

Onde:

- `condicao`: É uma expressão booleana (ou qualquer expressão que resulte em um valor inteiro em C). Enquanto o valor da `condicao` for diferente de zero (verdadeiro), o bloco de código é executado. Quando a `condicao` se torna zero (falso), o laço termina e a execução prossegue para a próxima instrução após o bloco `while`.
- **Bloco de código:** Uma ou mais instruções que serão repetidas. Assim como no `if`, se for apenas uma única instrução, as chaves `{}` são tecnicamente opcionais, mas seu uso é sempre recomendado para clareza e segurança.

**Exemplo em C: Contagem até 5**

```c
#include <stdio.h>

int main() {
    int contador = 1; // Inicialização da variável de controle do laço

    printf("Iniciando contagem com while:\n");
    while (contador <= 5) { // Condição de teste
        printf("%d ", contador);
        contador++; // Atualização da variável de controle (crucial!)
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

Neste exemplo:

1. `contador` é inicializado com 1.
2. A condição `contador <= 5` é testada.
    - Se verdadeira (1<=5, 2<=5, ..., 5<=5), o bloco é executado: `printf` imprime o valor de `contador`, e `contador` é incrementado.
    - Quando `contador` se torna 6, a condição `6 <= 5` é falsa, e o laço termina.

É fundamental que dentro do bloco do `while`, ou através de alguma interação externa, a condição do laço seja eventualmente alterada para se tornar falsa. Caso contrário, o programa entrará em um **laço infinito**.

**Observação sobre C++:** A sintaxe e o comportamento do laço `while` em C++ são idênticos aos do C. A `condicao` em C++ é avaliada como um valor `bool`.

## O Laço `do-while` (Repetição com Pós-Teste)

O laço **`do-while`** é similar ao `while`, com uma diferença crucial: a condição de teste é avaliada **após** a execução do bloco de código. Isso garante que o bloco de código dentro do `do-while` seja executado **pelo menos uma vez**, independentemente de a condição ser inicialmente verdadeira ou falsa.

A sintaxe básica do laço `do-while` em C é:

```c
do {
    // Bloco de código a ser executado
    // Este bloco sempre executa pelo menos uma vez.
    // Assim como no while, algo aqui deve eventualmente
    // tornar a 'condicao' falsa.
} while (condicao); // Ponto e vírgula é obrigatório aqui!
// Próxima instrução após o laço do-while
```

Onde:

- O **Bloco de código** é executado primeiro.
- Em seguida, a `condicao` é avaliada. Se for verdadeira (não-zero), o bloco de código é executado novamente. Isso continua até que a `condicao` se torne falsa (zero), momento em que o laço termina.
- Note o ponto e vírgula obrigatório após o `while (condicao)`.

**Exemplo em C: Menu Simples** O `do-while` é útil para situações onde uma ação precisa ser realizada antes de se verificar se deve ser repetida, como em menus interativos.

```c
#include <stdio.h>

int main() {
    char opcao;

    do {
        printf("\n--- Menu ---\n");
        printf("1 - Iniciar Jogo\n");
        printf("2 - Carregar Jogo\n");
        printf("3 - Opções\n");
        printf("4 - Sair\n");
        printf("Escolha uma opção: ");
        
        // Limpar buffer de entrada antes de ler o caractere
        // (útil se houver leituras anteriores com scanf que deixaram '\n')
        while(getchar() != '\n' && getchar() != EOF); // Limpa o buffer
        scanf(" %c", &opcao); // O espaço antes de %c também ajuda a consumir newlines

        switch (opcao) {
            case '1':
                printf("Iniciando novo jogo...\n");
                break;
            case '2':
                printf("Carregando jogo salvo...\n");
                break;
            case '3':
                printf("Abrindo opções...\n");
                break;
            case '4':
                printf("Saindo do programa...\n");
                break;
            default:
                printf("Opção inválida! Tente novamente.\n");
        }
    } while (opcao != '4'); // Repete enquanto a opção não for '4' (Sair)

    printf("Programa finalizado.\n");
    return 0;
}
```

Neste exemplo, o menu é exibido pelo menos uma vez. O usuário digita uma opção. Se a opção não for '4', o laço continua e o menu é exibido novamente. Quando o usuário digita '4', a condição `opcao != '4'` se torna falsa, e o laço termina.

**Observação sobre C++:** A sintaxe e o comportamento do laço `do-while` em C++ são idênticos aos do C.

## O Laço `for` (Repetição Contada e Controlada)

O laço **`for`** é uma estrutura de repetição poderosa e flexível, frequentemente utilizada quando o número de iterações é conhecido de antemão ou quando se precisa de um controle mais granular sobre a inicialização, a condição de continuação e o passo de iteração. Ele consolida esses três elementos em uma única linha de cabeçalho.

A sintaxe básica do laço `for` em C é:

```c
for (inicializacao; condicao; incremento_ou_decremento) {
    // Bloco de código a ser executado
    // enquanto a 'condicao' for verdadeira
}
// Próxima instrução após o laço for
```

O funcionamento do laço `for` pode ser descrito da seguinte forma:

1. **`inicializacao`**: Esta expressão é executada **apenas uma vez**, no início do laço. É comumente usada para declarar e/ou inicializar uma variável de controle do laço (contador).
2. **`condicao`**: Esta expressão é avaliada **antes de cada iteração** (incluindo a primeira, após a inicialização). Se a condição for verdadeira (não-zero), o bloco de código do laço é executado. Se for falsa (zero), o laço termina.
3. **Bloco de código**: Se a condição for verdadeira, as instruções dentro do laço são executadas.
4. **`incremento_ou_decremento`**: Esta expressão é executada **ao final de cada iteração**, após a execução do bloco de código e antes da próxima avaliação da `condicao`. É comumente usada para atualizar a variável de controle do laço (ex: incrementá-la ou decrementá-la).
5. O processo retorna ao passo 2 (avaliação da `condicao`).

**Exemplo em C: Contagem com `for`**

```c
#include <stdio.h>

int main() {
    int i; // Variável de controle do laço

    printf("Contagem progressiva com for:\n");
    for (i = 1; i <= 5; i++) { // Inicialização: i=1; Condição: i<=5; Incremento: i++
        printf("%d ", i);
    }
    printf("\n");

    printf("Contagem regressiva com for:\n");
    for (i = 5; i >= 1; i--) { // Inicialização: i=5; Condição: i>=1; Decremento: i--
        printf("%d ", i);
    }
    printf("\nContagem com for finalizada.\n");

    return 0;
}
```

**Saída:**

```
Contagem progressiva com for:
1 2 3 4 5 
Contagem regressiva com for:
5 4 3 2 1 
Contagem com for finalizada.
```

**Declaração da variável de controle no C99 e C++:** No padrão C99 da linguagem C (e em todas as versões de C++), é permitido declarar a variável de controle do laço `for` diretamente na seção de `inicializacao`. O escopo dessa variável fica restrito ao próprio laço `for`.

```c
// Exemplo em C99 ou C++
#include <stdio.h> // ou <iostream> para C++

int main() {
    printf("Contagem com variável declarada no for:\n");
    for (int j = 0; j < 3; j++) {
        printf("j = %d\n", j);
    }
    // 'j' não é acessível aqui fora do laço, pois seu escopo é o laço.
    // printf("%d", j); // ERRO DE COMPILAÇÃO!

    return 0;
}
```

Em versões mais antigas de C (como C89/ANSI C), a variável de controle precisava ser declarada antes do laço `for`.

### Variações e Flexibilidade do `for`

Qualquer uma das três expressões no cabeçalho do `for` (`inicializacao`, `condicao`, `incremento_ou_decremento`) é opcional.

- **Laço `for` sem inicialização:**
    
    ```c
    int k = 0;
    for (; k < 3; k++) { /* ... */ }
    ```
    
- **Laço `for` sem incremento (o incremento deve ocorrer dentro do bloco):**
    
    ```c
    for (int m = 0; m < 10; ) {
        // ...
        if (alguma_condicao_especial) {
            m += 2;
        } else {
            m++;
        }
    }
    ```
    
- **Laço `for` apenas com condição (similar a um `while`):**
    
    ```c
    int n = 0;
    for (; n < 5 ;) {
        printf("%d ", n);
        n++;
    } // Equivalente a: while (n < 5) { printf("%d ", n); n++; }
    ```
    
- **Laço `for` infinito (sem condição, ou com condição sempre verdadeira):**
    
    ```c
    for (;;) {
        // Laço infinito, precisa de um 'break' interno para sair
        printf("Infinito... ");
        // if (condicao_de_saida) break;
    }
    ```
    
- **Múltiplas inicializações ou incrementos usando o operador vírgula:**
    
    ```c
    #include <stdio.h>
    int main() {
        int x, y;
        for (x = 0, y = 10; x < y; x++, y--) {
            printf("x=%d, y=%d\n", x, y);
        }
        return 0;
    }
    ```

**Observação sobre C++:** O laço `for` tradicional em C++ funciona exatamente como em C (incluindo a capacidade de declarar a variável de controle no cabeçalho). Além disso, o C++11 introduziu o **laço `for` baseado em intervalo (range-based for loop)**, que simplifica a iteração sobre elementos de contêineres (como arrays, `std::vector`, `std::string`, etc.).

## Instruções de Desvio de Controle em Laços

Dentro dos blocos de código das estruturas de repetição, as instruções `break` e `continue` oferecem um controle mais granular sobre o fluxo de execução do laço.

### A Instrução `break`

A instrução **`break`** tem dois usos principais em C:

1. Dentro de uma estrutura `switch`, para sair do `switch` após um `case` ser executado.
2. Dentro de qualquer laço (`while`, `do-while`, `for`), para **terminar imediatamente a execução do laço mais interno** em que ela se encontra. A execução do programa continua com a primeira instrução após o laço que foi terminado.

**Exemplo de `break` em um laço `for`:**

```c
#include <stdio.h>

int main() {
    printf("Procurando o número 5 em um laço de 1 a 10:\n");
    for (int i = 1; i <= 10; i++) {
        printf("%d ", i);
        if (i == 5) {
            printf("\nNúmero 5 encontrado! Saindo do laço.\n");
            break; // Termina o laço for imediatamente
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

Quando `i` se torna 5, a condição `i == 5` é verdadeira, a mensagem é impressa, e o `break` faz com que o laço `for` seja encerrado prematuramente.

### A Instrução `continue`

A instrução **`continue`** é usada dentro de laços (`while`, `do-while`, `for`). Quando encontrada, ela **interrompe a iteração atual** do laço e **inicia imediatamente a próxima iteração**. Qualquer código restante na iteração atual após o `continue` é ignorado.

- Em um laço `while` ou `do-while`, `continue` faz com que a execução salte diretamente para a avaliação da condição de teste do laço.
- Em um laço `for`, `continue` faz com que a execução salte para a parte de `incremento_ou_decremento` do cabeçalho do `for`, e então a `condicao` é reavaliada.

**Exemplo de `continue` em um laço `for` (imprimir apenas números ímpares):**

```c
#include <stdio.h>

int main() {
    printf("Imprimindo números ímpares de 1 a 10:\n");
    for (int i = 1; i <= 10; i++) {
        if (i % 2 == 0) { // Se i for par
            continue;     // Pula para a próxima iteração, ignorando o printf abaixo
        }
        printf("%d ", i); // Este printf só executa para números ímpares
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

Quando `i` é par, a condição `i % 2 == 0` é verdadeira, e o `continue` é executado, fazendo com que o `printf("%d ", i);` seja pulado, e o laço prossiga para o incremento de `i` e a próxima verificação da condição.

## Laços Infinitos e Cuidados

Um **laço infinito** ocorre quando a condição de terminação de um laço (`while`, `do-while` ou `for`) nunca se torna falsa (ou, no caso do `for`, a condição é omitida ou sempre verdadeira e não há `break` interno). Isso faz com que o bloco de código do laço seja executado indefinidamente, e o programa pode parecer "travado".

**Exemplos de laços infinitos (a serem evitados ou usados com `break`):**

```c
// Laço while infinito
while (1) { // 1 é sempre verdadeiro
    printf("Infinito while...\n");
}

// Laço for infinito
for (;;) {
    printf("Infinito for...\n");
}

// Erro comum levando a laço infinito: esquecer de atualizar a variável de controle
int contador = 0;
while (contador < 5) {
    printf("Contador: %d\n", contador);
    // ERRO: contador não está sendo incrementado aqui!
}
```

Laços infinitos geralmente são erros de lógica, mas podem ser usados intencionalmente em algumas situações (por exemplo, em sistemas embarcados onde o programa principal roda continuamente, ou em laços de eventos de interface gráfica), desde que haja um mecanismo interno (como uma instrução `break` acionada por um evento) para sair do laço quando necessário.

É crucial garantir que a condição de um laço `while` ou `do-while` eventualmente se torne falsa, e que a condição e a parte de incremento/decremento de um laço `for` estejam corretamente definidas para garantir sua terminação.

## Observações sobre C++: O Laço `for` Baseado em Intervalo

Como mencionado, C++ herda as estruturas `while`, `do-while` e o `for` tradicional do C. Uma adição significativa em C++11 é o **laço `for` baseado em intervalo (range-based for loop)**. Esta forma de laço simplifica enormemente a iteração sobre os elementos de um "intervalo", que pode ser um array, um contêiner da STL (como `std::vector`, `std::list`, `std::string`), ou qualquer objeto que defina os métodos `begin()` e `end()`.

A sintaxe é:

```cpp
for (declaracao_elemento : expressao_intervalo) {
    // Bloco de código, onde 'declaracao_elemento'
    // recebe cada elemento do intervalo a cada iteração
}
```

**Exemplo em C++:**

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Iterando sobre um array C-style
    int numeros_array[] = {1, 2, 3, 4, 5};
    std::cout << "Array: ";
    for (int num : numeros_array) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // Iterando sobre um std::vector
    std::vector<std::string> palavras = {"Olá", "Mundo", "C++"};
    std::cout << "Vetor de strings: ";
    for (const std::string& palavra : palavras) { // Usando const referência para eficiência e segurança
        std::cout << palavra << " ";
    }
    std::cout << std::endl;

    // Iterando sobre os caracteres de uma std::string
    std::string texto = "ABC";
    std::cout << "Caracteres da string: ";
    for (char c : texto) {
        std::cout << c << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

Este tipo de laço é mais conciso e menos propenso a erros de índice do que o `for` tradicional ao iterar sobre coleções, tornando o código mais legível e moderno em C++.

## Considerações Finais

Neste capítulo, exploramos as estruturas de repetição, ferramentas indispensáveis para a automação de tarefas e a implementação de algoritmos iterativos em C e C++. Detalhamos o funcionamento dos laços `while` (com pré-teste da condição), `do-while` (com pós-teste, garantindo ao menos uma execução) e o versátil laço `for` (ideal para repetições contadas ou com controle explícito de inicialização, condição e passo).

Discutimos também como as instruções `break` e `continue` permitem um controle mais refinado do fluxo dentro dos laços, possibilitando a interrupção prematura de um laço ou o salto para a próxima iteração, respectivamente. Alertamos para os perigos dos laços infinitos e a importância de garantir condições de terminação adequadas.

Finalmente, apresentamos uma breve visão do laço `for` baseado em intervalo do C++11, uma adição que simplifica a iteração sobre coleções.

Com o domínio das estruturas de decisão (Capítulo 8) e das estruturas de repetição abordadas aqui, você possui agora um conjunto poderoso de ferramentas para controlar o fluxo de execução dos seus programas. Essas construções são os blocos de construção da lógica algorítmica e serão constantemente utilizadas à medida que avançamos para tópicos mais complexos, como arrays, ponteiros e estruturas de dados.
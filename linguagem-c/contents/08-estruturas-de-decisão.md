# Capítulo 8 – Estruturas de Decisão

No capítulo anterior, desvendamos o universo dos valores booleanos e das expressões lógicas, que são a base para a tomada de decisões em qualquer programa de computador. Compreendemos como as linguagens C e C++ representam e avaliam condições de verdadeiro ou falso. Agora, daremos um passo adiante, explorando as ferramentas que nos permitem utilizar esses resultados booleanos para efetivamente direcionar o fluxo de execução de nossos programas: as **estruturas de decisão**.

As estruturas de decisão, também conhecidas como estruturas condicionais, são construções da linguagem que permitem que um programa escolha entre diferentes caminhos de execução com base no resultado de uma ou mais condições. Sem elas, nossos programas executariam sempre a mesma sequência de instruções, tornando-se inflexíveis e limitados. Com as estruturas de decisão, podemos criar softwares que reagem a entradas do usuário, adaptam-se a diferentes estados e implementam lógicas complexas.

Neste capítulo, investigaremos as principais estruturas de decisão disponíveis em C e C++. Começaremos com a instrução `if`, que permite a execução condicional de um bloco de código. Em seguida, avançaremos para a estrutura `if-else`, que oferece uma alternativa quando a condição inicial não é satisfeita. Exploraremos também o encadeamento `if-else if-else` para cenários com múltiplas condições. Finalmente, apresentaremos a instrução `switch`, uma alternativa elegante para selecionar entre diversos casos baseados no valor de uma expressão. Ao dominar estas estruturas, você estará apto a construir programas muito mais inteligentes e dinâmicos.

## A Estrutura `if` (Seleção Simples)

A estrutura de decisão mais fundamental em C (e também em C++) é a instrução **`if`**. Ela permite que um bloco de código seja executado **somente se** uma determinada condição for verdadeira. Se a condição for falsa, o bloco de código associado ao `if` é simplesmente ignorado, e o programa continua a execução a partir da próxima instrução após o bloco `if`.

A sintaxe básica da instrução `if` em C é:

```c
if (condicao) {
    // Bloco de código a ser executado
    // se a 'condicao' for verdadeira (não-zero em C)
}
// Próxima instrução após o if
```

Onde:

- `condicao`: É uma expressão booleana (ou qualquer expressão que resulte em um valor inteiro em C). Se o valor da `condicao` for diferente de zero (interpretado como verdadeiro), o bloco de código dentro das chaves `{}` é executado. Se for zero (interpretado como falso), o bloco é pulado.
- **Bloco de código:** Uma ou mais instruções que serão executadas se a condição for verdadeira. Se for apenas uma única instrução, as chaves `{}` são tecnicamente opcionais, mas é uma boa prática de programação sempre usá-las para evitar ambiguidades e facilitar a manutenção.

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int idade = 20;

    if (idade >= 18) {
        printf("Você é maior de idade.\n");
    }

    if (idade < 12) {
        printf("Você é uma criança.\n"); // Esta mensagem não será impressa
    }

    printf("Fim do programa.\n");
    return 0;
}
```

Neste exemplo, a primeira condição (`idade >= 18`) é verdadeira (20 é maior ou igual a 18), então a mensagem "Você é maior de idade." será impressa. A segunda condição (`idade < 12`) é falsa, então a mensagem "Você é uma criança." não será impressa. O programa então prossegue para imprimir "Fim do programa.".

**Observação sobre C++:** A sintaxe e o comportamento da instrução `if` em C++ são idênticos aos do C. A principal diferença é que, em C++, a `condicao` é avaliada como um valor `bool` (`true` ou `false`).

## A Estrutura `if-else` (Seleção Dupla)

Frequentemente, não queremos apenas executar um bloco de código se uma condição for verdadeira, mas também executar um _outro_ bloco de código se a condição for falsa. Para isso, utilizamos a estrutura **`if-else`**. Ela oferece dois caminhos de execução, e exatamente um deles será escolhido.

A sintaxe básica da instrução `if-else` em C é:

```c
if (condicao) {
    // Bloco de código A: executado se 'condicao' for verdadeira
} else {
    // Bloco de código B: executado se 'condicao' for falsa
}
// Próxima instrução após o if-else
```

Se a `condicao` for verdadeira (não-zero), o Bloco de Código A é executado, e o Bloco de Código B é ignorado. Se a `condicao` for falsa (zero), o Bloco de Código A é ignorado, e o Bloco de Código B é executado.

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int temperatura = 15;

    if (temperatura > 25) {
        printf("Está calor! Hora de ir à praia.\n");
    } else {
        printf("Não está tão quente. Talvez um casaco seja bom.\n");
    }

    int numero = 7;
    if (numero % 2 == 0) {
        printf("%d é um número par.\n", numero);
    } else {
        printf("%d é um número ímpar.\n", numero); // Esta mensagem será impressa
    }

    return 0;
}
```

No primeiro `if-else`, como `temperatura` (15) não é maior que 25, o bloco `else` será executado. No segundo, como `numero % 2` (o resto da divisão de 7 por 2) é 1 (não-zero, ou seja, a condição `numero % 2 == 0` é falsa), o bloco `else` correspondente será executado.

**Observação sobre C++:** A sintaxe e o comportamento da instrução `if-else` em C++ são idênticos aos do C, com a condição sendo avaliada como `bool`.

## A Estrutura `if-else if-else` (Seleção Múltipla Encadeada)

Quando precisamos escolher entre mais de dois caminhos de execução, podemos encadear múltiplas estruturas `if` e `else`. Isso é comumente feito usando a construção **`if-else if-else`**. O programa avalia as condições em sequência, de cima para baixo. Assim que uma condição verdadeira é encontrada, o bloco de código associado a ela é executado, e todas as condições e blocos `else if` ou `else` subsequentes são ignorados. Se nenhuma das condições `if` ou `else if` for verdadeira, o bloco `else` final (se presente) é executado.

A sintaxe básica em C é:

```c
if (condicao1) {
    // Bloco de código 1: executado se condicao1 for verdadeira
} else if (condicao2) {
    // Bloco de código 2: executado se condicao1 for falsa E condicao2 for verdadeira
} else if (condicao3) {
    // Bloco de código 3: executado se condicao1 e condicao2 forem falsas E condicao3 for verdadeira
}
// ... (pode haver mais 'else if' blocos)
else {
    // Bloco de código N: executado se NENHUMA das condições anteriores for verdadeira (opcional)
}
// Próxima instrução
```

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int nota = 75;

    if (nota >= 90) {
        printf("Conceito: A\n");
    } else if (nota >= 80) {
        printf("Conceito: B\n");
    } else if (nota >= 70) {
        printf("Conceito: C\n"); // Esta mensagem será impressa
    } else if (nota >= 60) {
        printf("Conceito: D\n");
    } else {
        printf("Conceito: F (Reprovado)\n");
    }

    return 0;
}
```

No exemplo, como `nota` é 75:

1. `nota >= 90` (75 >= 90) é falso.
2. `nota >= 80` (75 >= 80) é falso.
3. `nota >= 70` (75 >= 70) é verdadeiro. O bloco `printf("Conceito: C\n");` é executado.
4. As condições `else if (nota >= 60)` e o `else` final são ignoradas.

Embora o encadeamento `if-else if-else` seja poderoso, cadeias muito longas podem se tornar difíceis de ler. Para situações onde se testa o valor de uma única variável ou expressão contra múltiplos valores constantes, a estrutura `switch` (que veremos a seguir) pode ser uma alternativa mais clara.

**Observação sobre C++:** A sintaxe e o comportamento do `if-else if-else` em C++ são idênticos aos do C.

## Blocos de Código e a Importância das Chaves `{}`

Nas estruturas `if`, `else if` e `else`, se o bloco de código a ser executado condicionalmente contiver **apenas uma única instrução**, as chaves `{}` são tecnicamente opcionais em C e C++.

**Exemplo sem chaves (instrução única):**

```c
#include <stdio.h>

int main() {
    int x = 10;

    if (x > 5)
        printf("x é maior que 5.\n"); // Apenas esta linha está sob o if
    else
        printf("x não é maior que 5.\n");

    printf("Esta linha executa sempre, independentemente do if-else.\n");
    return 0;
}
```

No entanto, esta prática pode levar a erros e dificultar a leitura, especialmente quando se adicionam mais instruções ao bloco condicional posteriormente. Se você esquecer de adicionar as chaves ao incluir uma segunda instrução, apenas a primeira instrução ficará sob o controle do `if` ou `else`, o que pode não ser o comportamento desejado.

**Exemplo com erro comum (sem chaves e múltiplas instruções pretendidas):**

```c
#include <stdio.h>

int main() {
    int y = 3;
    if (y < 5)
        printf("y é menor que 5.\n"); // Esta está sob o if
        printf("Este é o valor de y: %d\n", y); // ERRO LÓGICO: Esta linha NÃO está sob o if, será sempre executada!
                                            // A identação engana, mas o compilador só associa a primeira instrução ao if.
    return 0;
}
```

Para evitar esses problemas e garantir a clareza do código, é uma **forte recomendação e boa prática de programação sempre utilizar chaves `{}` para delimitar os blocos de código** associados às estruturas `if`, `else if` e `else`, mesmo que o bloco contenha apenas uma única instrução.

**Exemplo correto com chaves (múltiplas instruções):**

```c
#include <stdio.h>

int main() {
    int z = 7;
    if (z > 0) {
        printf("z é positivo.\n");
        printf("Seu valor é: %d\n", z);
    } else {
        printf("z não é positivo (é zero ou negativo).\n");
        printf("Seu valor é: %d\n", z);
    }
    return 0;
}
```

Esta abordagem é mais robusta, legível e menos propensa a erros.

## A Estrutura `switch` (Seleção Múltipla com Casos)

A instrução `switch` oferece uma maneira de selecionar um entre vários blocos de código para execução com base no valor de uma **expressão integral** (ou seja, uma expressão que resulta em um tipo inteiro, como `int`, `char`, `enum`). Ela é frequentemente usada como uma alternativa mais limpa e legível a longas cadeias de `if-else if-else` quando as decisões se baseiam em comparações de igualdade com valores constantes.

A sintaxe básica da instrução `switch` em C é:

```c
switch (expressao_integral) {
    case valor_constante1:
        // Bloco de código para valor_constante1
        break; // Opcional, mas geralmente necessário
    case valor_constante2:
        // Bloco de código para valor_constante2
        break;
    // ... (mais casos)
    default: // Opcional
        // Bloco de código se nenhum caso corresponder
        // Não precisa de break aqui se for o último
}
```

Onde:

- `expressao_integral`: A expressão cujo valor será testado. Deve resultar em um tipo inteiro (`char` também é promovido a `int` neste contexto).
- `case valor_constanteN`: Cada `case` define um rótulo com um valor constante (literal inteiro ou constante de caractere). Se o valor da `expressao_integral` coincidir com `valor_constanteN`, a execução começa a partir deste ponto.
- `break;`: A instrução `break` é crucial. Quando encontrada, ela causa uma saída imediata da estrutura `switch`, e a execução continua com a primeira instrução após o `switch`. **Se `break` for omitido, a execução "cai" (fall-through) para o próximo `case` e continua executando as instruções dos casos subsequentes até encontrar um `break` ou o final do `switch`**. Este comportamento de "fall-through" pode ser útil em algumas situações específicas, mas geralmente é uma fonte de erros se não for intencional.
- `default:`: O rótulo `default` é opcional. Se presente, o bloco de código associado a ele será executado se o valor da `expressao_integral` não corresponder a nenhum dos rótulos `case`. Se não houver `default` e nenhum `case` corresponder, nenhuma ação é tomada dentro do `switch`.

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    char opcao = 'B';

    printf("Menu:\nA - Adicionar\nB - Buscar\nC - Remover\nEscolha uma opção: %c\n", opcao);

    switch (opcao) {
        case 'A':
        case 'a': // Exemplo de fall-through para tratar 'A' e 'a' da mesma forma
            printf("Você escolheu Adicionar.\n");
            break;
        case 'B':
        case 'b':
            printf("Você escolheu Buscar.\n"); // Esta mensagem será impressa
            break;
        case 'C':
        case 'c':
            printf("Você escolheu Remover.\n");
            break;
        default:
            printf("Opção inválida.\n");
            // break; // Não é necessário aqui, pois é o último
    }

    int dia_semana = 3; // 1=Domingo, 2=Segunda, ..., 7=Sábado
    switch (dia_semana) {
        case 1:
            printf("Domingo\n");
            break;
        case 2:
            printf("Segunda-feira\n");
            break;
        case 3:
            printf("Terça-feira\n"); // Esta mensagem será impressa
            break;
        case 4:
            printf("Quarta-feira\n");
            break;
        case 5:
            printf("Quinta-feira\n");
            break;
        case 6:
            printf("Sexta-feira\n");
            break;
        case 7:
            printf("Sábado\n");
            break;
        default:
            printf("Dia inválido!\n");
    }

    return 0;
}
```

No primeiro `switch`, como `opcao` é 'B', o `case 'B':` é correspondido, e "Você escolheu Buscar." é impresso. O `break` então finaliza o `switch`. Note o uso de "fall-through" para `case 'A':` e `case 'a':`: se `opcao` fosse 'a', a execução começaria em `case 'a':`, não faria nada ali (pois não há instruções antes do próximo `case`) e "cairia" para as instruções de `case 'A':`.

**Observação sobre C++:** A instrução `switch` em C++ é muito similar à do C. Ela também opera sobre expressões integrais e usa `case`, `break` e `default` da mesma maneira. Uma adição no C++17 é o atributo `[[fallthrough]]`, que pode ser usado para indicar explicitamente que a omissão de um `break` e o consequente "fall-through" para o próximo `case` são intencionais, ajudando a evitar avisos do compilador e a tornar o código mais claro.

```c
// Exemplo conceitual de [[fallthrough]] em C++17
switch (valor) {
     case 1:
         fazer_algo();
         [[fallthrough]]; // Indica que o fall-through é intencional
     case 2:
         fazer_outra_coisa(); // Executado para case 1 e case 2
         break;
     // ...
}
```

Além disso, em C++, a expressão no `switch` e os valores nos `case` podem ser de tipos `enum class` (enums fortemente tipados), o que não é diretamente permitido em C da mesma forma.

## Considerações Finais

Neste capítulo, mergulhamos nas estruturas de decisão que conferem inteligência e flexibilidade aos nossos programas em C e C++. Iniciamos com a instrução `if`, a forma mais simples de executar código condicionalmente. Em seguida, exploramos a estrutura `if-else`, que nos permite escolher entre dois caminhos alternativos, e o encadeamento `if-else if-else`, para lidar com múltiplas condições de forma sequencial. Ressaltamos a importância crucial do uso de chaves `{}` para delimitar blocos de código, mesmo para instruções únicas, a fim de garantir clareza e evitar erros lógicos.

Finalmente, apresentamos a instrução `switch`, uma ferramenta poderosa e muitas vezes mais legível para selecionar entre múltiplos casos baseados no valor de uma expressão integral, destacando o papel fundamental da instrução `break` e o comportamento de "fall-through".

O domínio dessas estruturas de decisão é um pilar essencial da programação. Elas são as ferramentas que permitem aos seus programas reagir a diferentes entradas, adaptar-se a diferentes estados e implementar lógicas complexas. Com a capacidade de controlar o fluxo de execução com base em condições, você está agora um passo mais perto de criar aplicações verdadeiramente dinâmicas e interativas. O próximo passo natural em nossa jornada será explorar as estruturas de repetição, que nos permitirão executar blocos de código múltiplas vezes.
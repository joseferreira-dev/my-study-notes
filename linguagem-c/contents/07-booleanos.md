# Capítulo 7 – Booleanos e Expressões Lógicas

Nos capítulos anteriores, exploramos os diversos tipos de dados que nos permitem representar números (`int`, `double`) e texto (`char`, `string`), bem como os operadores para manipulá-los. Agora, voltaremos nossa atenção para um conceito fundamental que permeia quase toda a lógica de programação: os **valores lógicos**, ou **booleanos**. Em quase todas as tarefas úteis, um programa precisa tomar decisões, avaliar condições e alterar seu comportamento com base em uma de apenas duas possibilidades: algo é **verdadeiro** ou é **falso**; uma condição foi satisfeita ou não; uma operação deve prosseguir ou ser interrompida.

Este capítulo se dedica a entender como as linguagens C e C++ representam e utilizam esses valores. Começaremos analisando como a linguagem C, em sua essência, simula o comportamento booleano utilizando tipos inteiros (o paradigma "zero/não-zero"). Veremos como o padrão C99 introduziu o cabeçalho `<stdbool.h>` para formalizar e tornar mais legível essa simulação. Em seguida, faremos a transição para o C++, que adota uma abordagem mais moderna ao incluir o `bool` como um tipo de dado nativo, com os literais `true` e `false`.

Finalmente, exploraremos o conceito de **expressões booleanas** (ou lógicas), que são construções que avaliam para um resultado verdadeiro ou falso, frequentemente utilizando os operadores de comparação e lógicos que vimos anteriormente. Embora as estruturas de controle de fluxo como `if-else` e `while` sejam o principal campo de aplicação dos booleanos e serão detalhadas em capítulos futuros, este capítulo fornecerá a base conceitual indispensável para entender como essas decisões são fundamentadas.

## Booleanos em C

A linguagem C, em suas versões originais (ANSI C ou C89), não possuía um tipo de dado booleano dedicado. Em vez disso, a lógica booleana era (e ainda pode ser) implementada utilizando tipos inteiros, seguindo uma convenção simples, poderosa e onipresente:

- O valor inteiro **`0` (zero)** é interpretado como **FALSO**.
- Qualquer valor inteiro **diferente de zero** (seja `1`, `-1`, `100`, `-42`) é interpretado como **VERDADEIRO**.

Essa convenção é aplicada em todos os contextos onde um valor lógico é esperado, como nas condições de estruturas de controle (`if`, `while`, `for`).

### Lógica de C em Ação

Quando operadores relacionais (como `>` ou `==`) e lógicos (como `&&` ou `!`) são usados em C, eles _produzem_ um resultado inteiro: `0` para falso e `1` para verdadeiro.

```c
#include <stdio.h>

int main() {
    // 1. Operadores produzem 0 ou 1
    int resultado1 = (5 > 3);  // 5 > 3 é verdadeiro, 'resultado1' recebe 1
    int resultado2 = (10 == 20); // 10 == 20 é falso, 'resultado2' recebe 0
    
    printf("Resultado 1 (5 > 3): %d\n", resultado1);
    printf("Resultado 2 (10 == 20): %d\n", resultado2);

    // 2. Estruturas 'if' avaliam 0 ou não-zero
    if (resultado1) { // if (1) -> verdadeiro
        printf("Bloco 1 foi executado (valor 1 é verdadeiro)\n");
    }

    if (resultado2) { // if (0) -> falso
        // Este bloco não é executado
    }

    // 3. Qualquer não-zero é verdadeiro
    if (100) { // 100 não é zero
        printf("Bloco 2 foi executado (valor 100 é verdadeiro)\n");
    }

    if (-5) { // -5 não é zero
        printf("Bloco 3 foi executado (valor -5 é verdadeiro)\n");
    }

    if (0) { // 0 é falso
        // Este bloco não é executado
    }

    return 0;
}
```

Este código ilustra perfeitamente a convenção do C. O programa não avalia se `100` é "verdadeiro", ele apenas avalia se `100` é "zero". Como não é, a condição é satisfeita.

### Padrão C99 e `<stdbool.h>`

Reconhecendo que usar `int` para lógica podia ser confuso (por exemplo, `int temPermissao = 1;`), o padrão C99 introduziu uma forma mais explícita de trabalhar com booleanos através do arquivo de cabeçalho `<stdbool.h>`.

Este cabeçalho **não introduz um tipo booleano "real"**, mas sim um conjunto de macros (substituições de texto) para simular um:

1. Define um novo tipo chamado `_Bool` (que é, fundamentalmente, um tipo inteiro otimizado para armazenar apenas 0 ou 1).
2. Define a macro `bool` como um sinônimo (alias) para `_Bool`.
3. Define a macro `true` como uma constante inteira `1`.
4. Define a macro `false` como uma constante inteira `0`.

Para utilizar esses recursos, basta incluir o cabeçalho:

```c
#include <stdbool.h> // Necessário para bool, true, false em C99
```

**Exemplo usando `<stdbool.h>` em C (requer compilador C99 ou posterior):**

```c
#include <stdio.h>
#include <stdbool.h> // Para bool, true, false

int main() {
    bool estaChovendo = true;  // Compilador substitui 'bool' por '_Bool' e 'true' por '1'
    bool diaEnsolarado = false; // Compilador substitui 'false' por '0'

    if (estaChovendo) {
        printf("Pegue um guarda-chuva (usando stdbool.h).\n");
    }

    if (!diaEnsolarado) { // !false -> !0 -> 1 (verdadeiro)
        printf("Não parece estar ensolarado (usando stdbool.h).\n");
    }

    // Internamente, eles ainda são apenas inteiros 0 e 1
    printf("Valor de estaChovendo (como int): %d\n", estaChovendo); // Saída: 1
    printf("Valor de diaEnsolarado (como int): %d\n", diaEnsolarado); // Saída: 0
    
    printf("Tamanho do tipo 'bool' (em C99): %zu byte\n", sizeof(bool)); // Geralmente 1 byte

    return 0;
}
```

Usar `<stdbool.h>` é a prática moderna padrão em C, pois torna o código muito mais legível e claro sobre sua intenção.

## Booleanos em C++

O C++ tomou um caminho diferente e mais robusto. Em vez de simular com macros, o C++ introduziu **`bool`** como um **tipo de dado fundamental e nativo**, integrado à linguagem. Ele não requer nenhum cabeçalho.

Uma variável do tipo `bool` pode armazenar apenas dois literais de palavra-chave: **`true`** e **`false`**.

```cpp
#include <iostream>

int main() {
    bool estaChovendo = true;
    bool estaFrio = false;
    
    // std::cout por padrão exibe 1 para true e 0 para false
    std::cout << "Está chovendo? " << estaChovendo << std::endl; // Saída: Está chovendo? 1
    std::cout << "Está frio? " << estaFrio << std::endl;     // Saída: Está frio? 0

    // Para imprimir "true" ou "false" textualmente:
    // Usamos o "manipulador de fluxo" std::boolalpha
    std::cout << std::boolalpha;
    std::cout << "Está chovendo (texto)? " << estaChovendo << std::endl; // Saída: ... true
    std::cout << "Está frio (texto)? " << estaFrio << std::endl;     // Saída: ... false
    
    // Para reverter ao padrão (0/1):
    std::cout << std::noboolalpha;
    std::cout << "Está chovendo (número)? " << estaChovendo << std::endl; // Saída: ... 1

    return 0;
}
```

### Conversões Implícitas para `bool` em C++

Embora o C++ tenha um tipo `bool` dedicado, ele mantém total compatibilidade com a filosofia "zero/não-zero" do C por meio de **conversões implícitas**.

**1. De `bool` para `int`:**

- `true` é convertido para o inteiro `1`.
- `false` é convertido para o inteiro `0`.

**2. De `int` (e outros) para `bool`:**

- `0` (inteiro) é convertido para `false`.
- _Qualquer_ valor não-zero (`1`, `100`, `-5`) é convertido para `true`.
- `0.0` (ponto flutuante) é convertido para `false`.
- _Qualquer_ valor de ponto flutuante não-zero (`0.5`, `-3.14`) é convertido para `true`.
- `NULL` ou `nullptr` (ponteiros nulos) são convertidos para `false`.
- _Qualquer_ ponteiro não-nulo é convertido para `true`.

```cpp
#include <iostream>

int main() {
    std::cout << std::boolalpha; // Imprimir "true" ou "false"

    // De int para bool
    bool b1 = 100; // 100 (não-zero) -> true
    bool b2 = -5;  // -5 (não-zero) -> true
    bool b3 = 0;   // 0 -> false

    std::cout << "bool(100) = " << b1 << std::endl; // Saída: true
    std::cout << "bool(-5) = " << b2 << std::endl;  // Saída: true
    std::cout << "bool(0) = " << b3 << std::endl;   // Saída: false

    // De bool para int
    int i1 = true;  // true -> 1
    int i2 = false; // false -> 0

    std::cout << "int(true) = " << i1 << std::endl;  // Saída: 1
    std::cout << "int(false) = " << i2 << std::endl; // Saída: 0

    return 0;
}
```

Essa conversão implícita permite que código C legado, como `if (ponteiro)` (para verificar se um ponteiro não é nulo), funcione perfeitamente em C++.

## Expressões Booleanas (Lógicas)

Uma **expressão booleana** é qualquer expressão em C ou C++ que, quando avaliada, resulta em um valor lógico – ou seja, falso ou verdadeiro (0 ou não-zero em C; `false` ou `true` em C++).

Essas expressões são a espinha dorsal da lógica condicional e são formadas utilizando:

1. **Valores Literais:** `true`, `false` (C++/C99), `0`, `1`, `5` (C/C++).
2. **Variáveis:** `bool minhaFlag;` ou `int minhaFlag;`.
3. **Operadores de Comparação:** (`==`, `!=`, `>`, `<`, `>=`, `<=`).
4. **Operadores Lógicos:** (`&&`, `||`, `!`).
5. **Chamadas de Funções:** Funções que retornam um tipo `bool` ou `int`.

### Expressões com Operadores de Comparação

Como vimos anteriormente, os operadores de comparação avaliam a relação entre dois valores e _produzem_ um resultado booleano.

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int x = 10;
    int y = 9;

    // (x > y) avalia para 1 (verdadeiro), porque 10 é maior que 9
    int resultado_comparacao = (x > y); 
    printf("(x > y) resulta em: %d\n", resultado_comparacao); // Saída: 1
    
    // (x == 10) avalia para 1 (verdadeiro)
    printf("(x == 10) resulta em: %d\n", (x == 10)); // Saída: 1
    
    // (10 == 15) avalia para 0 (falso)
    printf("(10 == 15) resulta em: %d\n", (10 == 15)); // Saída: 0
    return 0;
}
```

**Exemplo em C++:**

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 9;

    std::cout << std::boolalpha; // Para imprimir true/false

    // (x > y) avalia para true
    bool resultado_comparacao = (x > y);
    std::cout << "(x > y) resulta em: " << resultado_comparacao << std::endl; // Saída: true
    
    std::cout << "(x == 10) resulta em: " << (x == 10) << std::endl; // Saída: true
    std::cout << "(10 == 15) resulta em: " << (10 == 15) << std::endl; // Saída: false
    return 0;
}
```

### Expressões com Operadores Lógicos

Os operadores lógicos (`&&` - E, `||` - OU, `!` - NÃO) são usados para combinar ou inverter expressões booleanas.

#### Avaliação de Curto-Circuito (Short-Circuit Evaluation)

Este é um conceito crucial. C e C++ garantem que os operadores && e || são avaliados da esquerda para a direita e param assim que o resultado é conhecido.

- **`expr1 && expr2` (E):** Se `expr1` for avaliada como **falsa**, o resultado da expressão inteira _já é_ falso. Portanto, `expr2` **nunca é executada**.
- **`expr1 || expr2` (OU):** Se `expr1` for avaliada como **verdadeira**, o resultado da expressão inteira _já é_ verdadeiro. Portanto, `expr2` **nunca é executada**.

Isso não é apenas uma otimização, é uma garantia de segurança vital.

```c
// Exemplo de curto-circuito em C
#include <stdio.h>

int main() {
    int x = 5;
    int y = 10;
    int *ptr = NULL; // Um ponteiro nulo (falso)

    // Exemplo de &&
    // A primeira condição (ptr != NULL) é (0 != 0) -> falso.
    // Como o lado esquerdo é falso, o lado direito (*ptr > 5)
    // NÃO É EXECUTADO.
    // Isso nos salva de um crash, pois tentar ler *ptr (que é NULL)
    // causaria uma falha de segmentação.
    if (ptr != NULL && *ptr > 5) {
        printf("Ponteiro aponta para um valor > 5\n");
    } else {
        printf("Ponteiro é nulo ou não aponta para valor > 5\n");
    }

    // Exemplo de ||
    // A primeira condição (y > x) é (10 > 5) -> verdadeiro (1).
    // Como o lado esquerdo é verdadeiro, o lado direito (ptr != NULL)
    // NÃO É EXECUTADO.
    if (y > x || *ptr > 5) {
        printf("Y é maior que X (a segunda condição nem foi testada)\n");
    }
    
    return 0;
}
```

### Expressões com Funções

Uma expressão booleana pode ser o resultado de uma função. Escrever funções que retornam `bool` (em C++) ou `int` (0/1 em C) é uma excelente prática de programação, pois torna o código mais legível.

**Exemplo em C (com `<stdbool.h>`):**

```c
#include <stdio.h>
#include <stdbool.h>

// Função que retorna um valor booleano
bool is_even(int numero) {
    if ((numero % 2) == 0) {
        return true;
    } else {
        return false;
    }
    // Ou, de forma mais concisa:
    // return (numero % 2) == 0;
}

int main() {
    int valor1 = 4;
    int valor2 = 7;
    
    // Usando a função em um 'if'
    if (is_even(valor1)) {
        printf("%d é par.\n", valor1);
    } else {
        printf("%d é ímpar.\n", valor1);
    }
    
    if (is_even(valor2)) {
        printf("%d é par.\n", valor2);
    } else {
        printf("%d é ímpar.\n", valor2);
    }

    return 0;
}
```

**Exemplo em C++ (nativo):**

```c
#include <iostream>

// Função que retorna um valor booleano
bool isEven(int numero) {
    return (numero % 2) == 0;
}

int main() {
    int valor = 4;
    
    std::cout << std::boolalpha;
    
    // O resultado da função é uma expressão booleana
    bool check = isEven(valor);
    
    std::cout << "O resultado de isEven(4) é: " << check << std::endl;
    
    if (isEven(valor)) {
        std::cout << valor << " é par." << std::endl;
    }

    return 0;
}
```

### Exemplo Prático: Verificação de Idade para Votação

Vamos consolidar esses conceitos em um exemplo interativo que usa uma expressão booleana para tomar uma decisão.

**Pseudocódigo:**

```
CONSTANTE INTEIRO idadeParaVotar = 18
VARIAVEL INTEIRO minhaIdade

IMPRIMA "Por favor, digite sua idade: "
LEIA minhaIdade

VARIAVEL BOOLEANA podeVotar = (minhaIdade >= idadeParaVotar)

SE (podeVotar) ENTÃO
    IMPRIMA "Você pode votar!"
SENÃO
    IMPRIMA "Você ainda não pode votar."
FIM-SE
```

**Implementação em C (C99):**

```c
#include <stdio.h>
#include <stdbool.h> // Para C99 bool, true, false

int main() {
    const int idadeParaVotar = 18;
    int minhaIdade;

    printf("Por favor, digite sua idade: ");
    
    // Verificando o retorno do scanf para robustez
    if (scanf("%d", &minhaIdade) != 1) {
        printf("Entrada inválida.\n");
        return 1; // Termina o programa com erro
    }

    // A expressão booleana é avaliada aqui
    bool podeVotar = (minhaIdade >= idadeParaVotar);

    if (podeVotar) {
        printf("Com %d anos, você pode votar! (Expressão avaliada como true/1)\n", minhaIdade);
    } else {
        printf("Com %d anos, você ainda não pode votar. (Expressão avaliada como false/0)\n", minhaIdade);
    }
    return 0;
}
```

**Implementação em C++:**

```cpp
#include <iostream>
#include <limits> // Para limpar o buffer

int main() {
    const int idadeParaVotar = 18;
    int minhaIdade;

    std::cout << "Por favor, digite sua idade: ";
    
    // Verificando a entrada do cin para robustez
    if (!(std::cin >> minhaIdade)) {
        std::cout << "Entrada inválida." << std::endl;
        
        // Limpa o estado de erro e o buffer de entrada
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        
        return 1; // Termina o programa com erro
    }
    
    // A expressão booleana é avaliada aqui
    bool podeVotar = (minhaIdade >= idadeParaVotar);

    std::cout << std::boolalpha; // Imprimir "true" ou "false"
    
    if (podeVotar) {
        std::cout << "Com " << minhaIdade << " anos, você pode votar! (Expressão: " << podeVotar << ")" << std::endl;
    } else {
        std::cout << "Com " << minhaIdade << " anos, você ainda não pode votar. (Expressão: " << podeVotar << ")" << std::endl;
    }
    return 0;
}
```

## Considerações Finais

Neste capítulo, exploramos o conceito de valores booleanos e como eles são tratados nas linguagens C e C++. Vimos que, enquanto o C tradicional utiliza a convenção de inteiros (0 para falso, não-zero para verdadeiro), o padrão C99 introduziu `<stdbool.h>` para uma representação mais explícita. O C++, por sua vez, oferece o tipo `bool` nativo com os literais `true` e `false`, proporcionando maior clareza e segurança de tipo, embora mantenha compatibilidade com a lógica "zero/não-zero" do C através de conversões implícitas.

Discutimos a formação de **expressões booleanas** (ou lógicas) através de operadores de comparação, operadores lógicos e o retorno de funções. Enfatizamos a importância da **avaliação de curto-circuito** (`&&` e `||`) como um mecanismo de segurança fundamental em C e C++.

A lógica booleana é a base para as estruturas de controle de fluxo. A compreensão de como esses valores são gerados (`0`/`1` em C, `false`/`true` em C++) e interpretados (`0`/não-zero) é o pré-requisito fundamental para o próximo capítulo, onde mergulharemos nas estruturas que permitem aos nossos programas desviar seu fluxo de execução, tomar decisões e repetir tarefas.
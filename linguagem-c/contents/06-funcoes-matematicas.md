# Capítulo 6 – Funções Matemáticas

Nos capítulos anteriores, exploramos os tipos de dados, variáveis, operadores e a manipulação de caracteres e strings, construindo uma base sólida para a programação em C e C++. Agora, avançaremos para um domínio essencial em muitas aplicações: as operações matemáticas. Embora os operadores aritméticos básicos (`+`, `-`, `*`, `/`, `%`) sejam suficientes para cálculos simples, frequentemente nos deparamos com a necessidade de realizar operações mais complexas, como cálculo de raízes quadradas, potenciação, funções trigonométricas, logaritmos e arredondamentos.

As linguagens C e C++ fornecem um rico conjunto de funções matemáticas, agrupadas em bibliotecas padrão, que abstraem a complexidade desses cálculos, permitindo que o programador se concentre na lógica da aplicação. Este capítulo se dedicará a explorar essas ferramentas. Iniciaremos discutindo a inclusão dos cabeçalhos necessários (`<math.h>` em C e `<cmath>` em C++) e, em seguida, apresentaremos algumas das funções matemáticas mais utilizadas, como `sqrt` para raiz quadrada, `round` para arredondamento e `log` para logaritmo natural, ilustrando seu uso com exemplos práticos em C.

Posteriormente, forneceremos uma visão mais ampla de outras funções matemáticas relevantes disponíveis, detalhando o propósito de cada uma. Ao final deste capítulo, você estará familiarizado com as principais funções matemáticas e saberá como incorporá-las em seus programas para resolver problemas que exigem cálculos numéricos mais sofisticados.

## A Biblioteca Matemática: `<math.h>` em C e `<cmath>` em C++

Para ter acesso às funções matemáticas em C, é necessário incluir o arquivo de cabeçalho da biblioteca matemática padrão, que é o `<math.h>`:

```c
#include <math.h>
```

Este cabeçalho declara as funções matemáticas, bem como algumas macros e tipos relacionados. A maioria das funções em `<math.h>` opera com argumentos do tipo `double` e retorna um `double`. Existem também versões dessas funções para os tipos `float` (com sufixo `f`, ex: `sqrtf()`) e `long double` (com sufixo `l`, ex: `sqrtl()`), especialmente a partir do padrão C99.

**Observação sobre C++:** Em C++, a biblioteca matemática do C está disponível através do cabeçalho `<cmath>`. É uma prática recomendada em C++ usar `<cmath>` em vez de `<math.h>`.

```c
#include <cmath>
```

Além disso, em C++, muitas dessas funções são sobrecarregadas para aceitar diferentes tipos de argumentos (como `float`, `double`, `long double`) e podem estar dentro do namespace `std` (ex: `std::sqrt()`, `std::round()`).

## Funções Matemáticas Comuns

Vamos explorar algumas das funções matemáticas mais frequentemente utilizadas.

### Raiz Quadrada: `sqrt()`

A função `sqrt()` calcula a raiz quadrada de um número não negativo.

- **Protótipo em C (para `double`):** `double sqrt(double x);`
- Ela retorna a raiz quadrada de `x`. Se `x` for negativo, o comportamento pode ser um erro de domínio (definido pela implementação, frequentemente resultando em `NaN` - Not a Number - e setando a variável global `errno`).

**Exemplo em C:**

```c
#include <stdio.h>
#include <math.h> // Necessário para sqrt()

int main() {
    double numero = 25.0;
    double raiz;

    if (numero >= 0) {
        raiz = sqrt(numero);
        printf("A raiz quadrada de %.2f é %.2f\n", numero, raiz); // Saída: A raiz quadrada de 25.00 é 5.00
    } else {
        printf("Não é possível calcular a raiz quadrada de um número negativo.\n");
    }

    double outro_numero = 16.7;
    printf("A raiz quadrada de %.1f é %f\n", outro_numero, sqrt(outro_numero)); 
    // Saída: A raiz quadrada de 16.7 é 4.086563 (a formatação de saída pode variar)

    return 0;
}
```

### Arredondamento: `round()`

A função `round()` arredonda um número de ponto flutuante para o inteiro mais próximo.

- **Protótipo em C (C99 em diante, para `double`):** `double round(double x);`
- Existem também `roundf()` para `float` e `roundl()` para `long double`.
- Ela retorna o valor de `x` arredondado para o inteiro mais próximo. Se `x` estiver exatamente no meio (ex: 2.5), geralmente arredonda para o inteiro mais distante de zero (ex: 2.5 -> 3.0, -2.5 -> -3.0), mas o comportamento exato pode depender da implementação do modo de arredondamento.

**Exemplo em C (requer compilador com suporte a C99 ou posterior):**

```c
#include <stdio.h>
#include <math.h> // Necessário para round()

int main() {
    double num1 = 16.7;
    double num2 = 16.3;
    double num3 = 16.5;
    double num4 = -16.7;

    printf("round(%.1f) = %.1f\n", num1, round(num1)); // Saída: round(16.7) = 17.0
    printf("round(%.1f) = %.1f\n", num2, round(num2)); // Saída: round(16.3) = 16.0
    printf("round(%.1f) = %.1f\n", num3, round(num3)); // Saída: round(16.5) = 17.0 (ou pode ser 16.0 dependendo do modo)
    printf("round(%.1f) = %.1f\n", num4, round(num4)); // Saída: round(-16.7) = -17.0
    
    // Para obter um resultado inteiro:
    int resultado_inteiro = (int)round(num1);
    printf("Arredondado para o inteiro mais próximo de %.1f: %d\n", num1, resultado_inteiro); // Saída: 17

    return 0;
}
```

Se você estiver usando um compilador C mais antigo que não suporte `round()` (anterior ao C99), pode-se simular o arredondamento para o inteiro mais próximo usando `floor(x + 0.5)` para números positivos e `ceil(x - 0.5)` para números negativos, ou uma lógica condicional.

### Logaritmo Natural: `log()`

A função `log()` calcula o logaritmo natural (base _e_) de um número.

- **Protótipo em C (para `double`):** `double log(double x);`
- Ela retorna o logaritmo natural de `x`. Se `x` for negativo, ocorre um erro de domínio. Se `x` for zero, ocorre um erro de polo (divisão por zero).

**Exemplo em C:**

```c
#include <stdio.h>
#include <math.h> // Necessário para log() e exp()

int main() {
    double valor = 10.0;
    double log_natural = log(valor);
    printf("O logaritmo natural de %.2f é %f\n", valor, log_natural);

    // Para calcular logaritmo em outra base, use a propriedade: log_b(x) = log_e(x) / log_e(b)
    double log_base10 = log(valor) / log(10.0);
    printf("O logaritmo na base 10 de %.2f é %f\n", valor, log_base10);

    // A função exp(x) calcula e^x
    printf("e elevado a %f é aproximadamente %.2f\n", log_natural, exp(log_natural)); // Deve ser próximo de 'valor'

    return 0;
}
```

Para logaritmo na base 10, a biblioteca `<math.h>` também fornece a função `log10()`. Para logaritmo na base 2 (a partir de C99), existe `log2()`.

### Máximo e Mínimo: `max()` e `min()`

**Em C padrão (`<math.h>`):** As funções `max(x,y)` e `min(x,y)` como funções de biblioteca padrão para encontrar o máximo ou mínimo entre dois números **não existem diretamente em C padrão (`<math.h>`)** da mesma forma que em C++. No C99, foram introduzidas as funções `fmax()`, `fmaxf()`, `fmaxl()` e `fmin()`, `fminf()`, `fminl()` para tipos de ponto flutuante, que veremos na próxima seção.

Para inteiros, ou para uma abordagem mais geral em C, você normalmente implementaria isso usando o operador condicional (ternário) ou uma pequena função/macro:

**Exemplo de implementação de máximo e mínimo em C:**

```c
#include <stdio.h>

// Usando macros (cuidado com múltiplos efeitos colaterais se os argumentos tiverem)
#define MAX_C(a, b) ((a) > (b) ? (a) : (b))
#define MIN_C(a, b) ((a) < (b) ? (a) : (b))

// Usando funções (mais seguro em relação a efeitos colaterais)
int max_int(int a, int b) {
    return (a > b) ? a : b;
}

int min_int(int a, int b) {
    return (a < b) ? a : b;
}

int main() {
    int x = 10, y = 20;
    printf("Usando macros:\n");
    printf("O máximo entre %d e %d é %d\n", x, y, MAX_C(x, y));     // Saída: 20
    printf("O mínimo entre %d e %d é %d\n", x, y, MIN_C(x, y));     // Saída: 10

    printf("\nUsando funções:\n");
    printf("O máximo entre %d e %d é %d\n", x, y, max_int(x, y)); // Saída: 20
    printf("O mínimo entre %d e %d é %d\n", x, y, min_int(x, y)); // Saída: 10

    double d1 = 5.5, d2 = 5.1;
    // Para doubles, a macro funcionaria, mas uma função específica seria melhor
    printf("\nO máximo entre %.1f e %.1f (macro) é %.1f\n", d1, d2, MAX_C(d1, d2)); // Saída: 5.5

    return 0;
}
```

**Observação sobre C++:** Em C++, a biblioteca padrão (`<algorithm>` ou, para tipos numéricos, também `<cmath>` pode trazer algumas) oferece `std::max(x,y)` e `std::min(x,y)` que são templates e funcionam para diversos tipos de dados.

```c
// Exemplo em C++
#include <iostream>
#include <algorithm> // Para std::max e std::min

int main() {
    int x = 100, y = 50;
    std::cout << "O máximo entre " << x << " e " << y << " é " << std::max(x, y) << std::endl; // Saída: 100
    std::cout << "O mínimo entre " << x << " e " << y << " é " << std::min(x, y) << std::endl; // Saída: 50
    return 0;
}
```

### Outras Funções Matemáticas Relevantes

A biblioteca `<math.h>` (e `<cmath>` em C++) oferece uma vasta gama de outras funções matemáticas. A tabela abaixo lista algumas das mais populares, seguidas de explicações e exemplos em C.

|Função|Descrição|
|---|---|
|`abs(x)`|Retorna o valor absoluto de um inteiro `x` (Na verdade, `abs` para inteiros está em `<stdlib.h>`. Em `<math.h>` temos `fabs` para `double`)|
|`acos(x)`|Retorna o arco cosseno de `x` (em radianos)|
|`asin(x)`|Retorna o arco seno de `x` (em radianos)|
|`atan(x)`|Retorna o arco tangente de `x` (em radianos)|
|`atan2(y, x)`|Retorna o arco tangente de `y/x`, usando os sinais de `x` e `y` para determinar o quadrante|
|`cbrt(x)`|Retorna a raiz cúbica de `x`|
|`ceil(x)`|Retorna o valor de `x` arredondado para cima para o inteiro mais próximo (teto)|
|`cos(x)`|Retorna o cosseno de `x` (onde `x` está em radianos)|
|`cosh(x)`|Retorna o cosseno hiperbólico de `x`|
|`exp(x)`|Retorna o valor de _e_x (onde _e_ é a constante de Euler)|
|`expm1(x)`|Retorna _e_x - 1, com maior precisão para `x` pequeno|
|`fabs(x)`|Retorna o valor absoluto de um número de ponto flutuante `x`|
|`fdim(x, y)`|Retorna a diferença positiva entre `x` e `y` (`x-y` se `x>y`, senão `0`)|
|`floor(x)`|Retorna o valor de `x` arredondado para baixo para o inteiro mais próximo (piso)|
|`hypot(x, y)`|Retorna sqrt(x2 + y2) (hipotenusa), com cuidado para evitar overflow/underflow|
|`fma(x, y, z)`|Retorna (xy) + z como uma única operação ternária, com maior precisão|
|`fmax(x, y)`|Retorna o maior valor entre os números de ponto flutuante `x` e `y`|
|`fmin(x, y)`|Retorna o menor valor entre os números de ponto flutuante `x` e `y`|
|`fmod(x, y)`|Retorna o resto da divisão de ponto flutuante de `x/y`|
|`log10(x)`|Retorna o logaritmo na base 10 de `x`|
|`log2(x)`|Retorna o logaritmo na base 2 de `x`|
|`pow(x, y)`|Retorna o valor de `x` elevado à potência `y` (xy)|
|`sin(x)`|Retorna o seno de `x` (onde `x` está em radianos)|
|`sinh(x)`|Retorna o seno hiperbólico de `x`|
|`tan(x)`|Retorna a tangente de `x` (onde `x` está em radianos)|
|`tanh(x)`|Retorna a tangente hiperbólica de `x`|
|`trunc(x)`|Retorna a parte inteira de `x`, truncando em direção a zero|

A seguir, detalharemos algumas dessas funções com exemplos em C.

- **`abs(int x)` (de `<stdlib.h>`) e `fabs(double x)` (de `<math.h>`)**: Retornam o valor absoluto (módulo) de `x`.
    
    ```c
    #include <stdio.h>
    #include <stdlib.h> // Para abs()
    #include <math.h>   // Para fabs()
    
    int main() {
        int num_int = -5;
        double num_double = -7.75;
    
        printf("abs(%d) = %d\n", num_int, abs(num_int));     // Saída: abs(-5) = 5
        printf("fabs(%.2f) = %.2f\n", num_double, fabs(num_double)); // Saída: fabs(-7.75) = 7.75
        return 0;
    }
    ```
    
- **`acos(double x)`, `asin(double x)`, `atan(double x)`**: Retornam, respectivamente, o arco cosseno, arco seno e arco tangente de `x`. O valor de `x` para `acos` e `asin` deve estar entre -1 e 1. O resultado é em radianos.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    #define PI_C ACOS(-1.0) // Uma forma de obter PI
    
    int main() {
        double val_cos = 0.5;
        double val_sin = 0.5;
        double val_tan = 1.0;
    
        printf("acos(%.1f) = %f radianos\n", val_cos, acos(val_cos)); // Aprox. 1.0472 (PI/3)
        printf("asin(%.1f) = %f radianos\n", val_sin, asin(val_sin)); // Aprox. 0.5236 (PI/6)
        printf("atan(%.1f) = %f radianos\n", val_tan, atan(val_tan)); // Aprox. 0.7854 (PI/4)
        printf("Valor de PI calculado: %f\n", PI_C);
        return 0;
    }
    ```
    
- **`atan2(double y, double x)`**: Calcula o arco tangente de `y/x`, usando os sinais de ambos os argumentos para determinar o quadrante correto do ângulo resultante (entre -PI e PI).
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        // Ponto (x= -1, y= -1) está no terceiro quadrante
        double angulo = atan2(-1.0, -1.0); 
        printf("atan2(-1.0, -1.0) = %f radianos (aprox. -2.356, ou -3PI/4)\n", angulo);
        return 0;
    }
    ```
    
- **`cbrt(double x)` (C99)**: Retorna a raiz cúbica de `x`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("cbrt(27.0) = %.1f\n", cbrt(27.0)); // Saída: cbrt(27.0) = 3.0
        printf("cbrt(-8.0) = %.1f\n", cbrt(-8.0)); // Saída: cbrt(-8.0) = -2.0
        return 0;
    }
    ```
    
- **`ceil(double x)` e `floor(double x)`**: `ceil(x)` ("teto") retorna o menor valor inteiro que é maior ou igual a `x`. `floor(x)` ("piso") retorna o maior valor inteiro que é menor ou igual a `x`. Ambas retornam um `double`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        double val1 = 4.3;
        double val2 = -2.9;
        printf("ceil(%.1f) = %.1f\n", val1, ceil(val1));   // Saída: ceil(4.3) = 5.0
        printf("floor(%.1f) = %.1f\n", val1, floor(val1)); // Saída: floor(4.3) = 4.0
        printf("ceil(%.1f) = %.1f\n", val2, ceil(val2));   // Saída: ceil(-2.9) = -2.0
        printf("floor(%.1f) = %.1f\n", val2, floor(val2)); // Saída: floor(-2.9) = -3.0
        return 0;
    }
    ```
    
- **`cos(double x)`, `sin(double x)`, `tan(double x)`**: Retornam, respectivamente, o cosseno, seno e tangente de `x`, onde `x` é um ângulo em radianos.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    #ifndef M_PI // Definir M_PI se não estiver definido (comum em math.h)
    #define M_PI 3.14159265358979323846
    #endif
    
    int main() {
        double angulo_rad = M_PI / 4.0; // 45 graus em radianos
        printf("cos(PI/4) = %f\n", cos(angulo_rad)); // Aprox. 0.707
        printf("sin(PI/4) = %f\n", sin(angulo_rad)); // Aprox. 0.707
        printf("tan(PI/4) = %f\n", tan(angulo_rad)); // Aprox. 1.0
        return 0;
    }
    ```
    
- **`cosh(double x)`, `sinh(double x)`, `tanh(double x)`**: Retornam, respectivamente, o cosseno hiperbólico, seno hiperbólico e tangente hiperbólica de `x`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        double val = 1.0;
        printf("cosh(%.1f) = %f\n", val, cosh(val));
        printf("sinh(%.1f) = %f\n", val, sinh(val));
        printf("tanh(%.1f) = %f\n", val, tanh(val));
        return 0;
    }
    ```
    
- **`exp(double x)`**: Retorna e<sup>x</sup> (exponencial natural).
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("exp(1.0) = %f (valor de e)\n", exp(1.0)); // Aprox. 2.718282
        printf("exp(0.0) = %f\n", exp(0.0));             // Saída: 1.000000
        return 0;
    }
    ```

- **`fdim(double x, double y)` (C99)**: Retorna a diferença positiva entre `x` e `y`. Se `x > y`, retorna `x - y`; caso contrário, retorna `0.0`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("fdim(5.0, 3.0) = %.1f\n", fdim(5.0, 3.0)); // Saída: 2.0
        printf("fdim(3.0, 5.0) = %.1f\n", fdim(3.0, 5.0)); // Saída: 0.0
        return 0;
    }
    ```
    
- **`hypot(double x, double y)` (C99)**: Calcula a hipotenusa de um triângulo retângulo com catetos `x` e `y`, ou seja, sqrt(x² + y²). Implementado de forma a evitar overflow ou underflow que poderiam ocorrer ao calcular x² e y² separadamente.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("hypot(3.0, 4.0) = %.1f\n", hypot(3.0, 4.0)); // Saída: 5.0
        return 0;
    }
    ```
    
- **`fma(double x, double y, double z)` (C99)**: Calcula `(x * y) + z` como uma única operação ternária. Isso pode fornecer maior precisão e, em algumas arquiteturas, ser mais rápido do que realizar a multiplicação e a adição separadamente.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        // Exemplo onde fma pode ser mais preciso:
        double x = 1e10, y = 1e10, z = -1e20;
        double resultado_separado = (x * y) + z; // Pode dar 0.0 devido à perda de precisão
        double resultado_fma = fma(x, y, z);    // Pode dar um resultado mais preciso
    
        printf("(1e10 * 1e10) + (-1e20) [separado] = %g\n", resultado_separado);
        printf("fma(1e10, 1e10, -1e20) [fma]       = %g\n", resultado_fma);
    
        printf("fma(2.0, 3.0, 4.0) = %.1f\n", fma(2.0, 3.0, 4.0)); // Saída: 10.0
        return 0;
    }
    ```
    
- **`fmax(double x, double y)` e `fmin(double x, double y)` (C99)**: Retornam, respectivamente, o maior e o menor valor entre os números de ponto flutuante `x` e `y`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("fmax(5.8, 3.1) = %.1f\n", fmax(5.8, 3.1)); // Saída: 5.8
        printf("fmin(5.8, 3.1) = %.1f\n", fmin(5.8, 3.1)); // Saída: 3.1
        return 0;
    }
    ```
    
- **`fmod(double x, double y)`**: Retorna o resto da divisão de ponto flutuante de `x` por `y`. O resultado tem o mesmo sinal de `x`.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("fmod(10.5, 3.0) = %.1f\n", fmod(10.5, 3.0)); // Saída: 1.5 (10.5 = 3*3.0 + 1.5)
        printf("fmod(-10.5, 3.0) = %.1f\n", fmod(-10.5, 3.0)); // Saída: -1.5
        return 0;
    }
    ```
    
- **`pow(double x, double y)`**: Retorna `x` elevado à potência `y` (x<sup>y</sup>).
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("pow(2.0, 3.0) = %.1f\n", pow(2.0, 3.0)); // Saída: 8.0 (2 ao cubo)
        printf("pow(10.0, -1.0) = %.1f\n", pow(10.0, -1.0)); // Saída: 0.1 (10 elevado a -1)
        printf("pow(4.0, 0.5) = %.1f\n", pow(4.0, 0.5));   // Saída: 2.0 (raiz quadrada de 4)
        return 0;
    }
    ```
    
- **`trunc(double x)` (C99)**: Retorna a parte inteira de `x`, truncando (removendo) a parte fracionária, em direção a zero.
    
    ```c
    #include <stdio.h>
    #include <math.h>
    
    int main() {
        printf("trunc(3.7) = %.1f\n", trunc(3.7));   // Saída: 3.0
        printf("trunc(-2.9) = %.1f\n", trunc(-2.9)); // Saída: -2.0
        printf("trunc(5.0) = %.1f\n", trunc(5.0));   // Saída: 5.0
        return 0;
    }
    ```

É importante notar que muitas dessas funções (especialmente as introduzidas no C99) possuem variantes para `float` (com sufixo `f`, ex: `sqrtf`, `roundf`, `sinf`) e para `long double` (com sufixo `l`, ex: `sqrtl`, `roundl`, `sinl`).

## Considerações Finais

Neste capítulo, exploramos o conjunto de funções matemáticas fornecidas pela biblioteca padrão C (`<math.h>`) e, por extensão, pela biblioteca `<cmath>` do C++. Vimos como realizar operações comuns como cálculo de raiz quadrada, arredondamento, logaritmos, e como encontrar valores máximos e mínimos, com exemplos práticos em C.

Além das funções mais conhecidas, apresentamos uma lista abrangente de outras funções matemáticas, detalhando suas finalidades e ilustrando o uso de várias delas, como as trigonométricas (`sin`, `cos`, `tan`, `acos`, `asin`, `atan`, `atan2`), exponenciais (`exp`, `expm1`), de valor absoluto (`fabs`), de arredondamento e truncamento (`ceil`, `floor`, `trunc`), potenciação (`pow`), e outras utilitárias (`cbrt`, `hypot`, `fma`, `fmod`, `fdim`).

A familiaridade com estas funções é crucial para qualquer programador que precise implementar algoritmos numéricos, realizar análises estatísticas, trabalhar com gráficos, simulações físicas, ou qualquer outra aplicação que envolva cálculos matemáticos além da aritmética básica. Ao utilizar as funções da biblioteca padrão, você não apenas economiza tempo de desenvolvimento, mas também se beneficia de implementações otimizadas e testadas, garantindo maior precisão e desempenho para seus programas. Lembre-se sempre de incluir o cabeçalho `<math.h>` (ou `<cmath>` em C++) e de verificar os tipos de dados esperados e retornados por cada função.

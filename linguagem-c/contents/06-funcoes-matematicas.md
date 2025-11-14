# Capítulo 6 – Funções Matemáticas

Nos capítulos anteriores, exploramos os tipos de dados, variáveis, operadores e a manipulação de caracteres e strings. Construímos uma base sólida para a programação em C e C++, focando na aritmética básica e na lógica. No entanto, o mundo real exige mais do que simples adição ou comparação. Frequentemente, nos deparamos com a necessidade de realizar operações complexas, como cálculo de raízes quadradas, potenciação, funções trigonométricas para geometria ou física, logaritmos para medição de complexidade ou crescimento, e arredondamentos para finanças.

Os operadores aritméticos básicos (`+`, `-`, `*`, `/`, `%`) são insuficientes para esses cenários. A filosofia do C é manter o núcleo da linguagem enxuto e fornecer funcionalidades complexas através de bibliotecas. Para operações matemáticas, essa funcionalidade é agrupada na **biblioteca matemática padrão**.

Este capítulo se dedicará a explorar essas ferramentas. Iniciaremos discutindo a inclusão dos cabeçalhos necessários (`<math.h>` em C e `<cmath>` em C++) e uma etapa crucial de compilação (a _linkedição_). Em seguida, apresentaremos as funções matemáticas mais utilizadas, agrupadas por categoria: potência e raízes, arredondamento, funções exponenciais/logarítmicas e trigonométricas. Também abordaremos tópicos que frequentemente causam confusão, como a diferença entre `abs` e `fabs`, e as armadilhas de se criar funções `max`/`min` em C. Ao final, você estará familiarizado com as principais funções e saberá como incorporá-las para resolver problemas que exigem cálculos sofisticados.

## Biblioteca Matemática: `<math.h>` e `<cmath>`

Para ter acesso às funções matemáticas em C, é necessário incluir o arquivo de cabeçalho da biblioteca matemática padrão, que é o `<math.h>`:

```c
#include <math.h>
```

Este cabeçalho declara os protótipos de dezenas de funções matemáticas (como `sqrt`, `pow`, `sin`, etc.), bem como macros e tipos relacionados (como `M_PI` para o valor de Pi em alguns sistemas, embora não seja parte do padrão C, ou `NAN` para "Not a Number").

A maioria das funções em `<math.h>` opera com o tipo `double`, que oferece alta precisão. Elas aceitam `double` como argumentos e retornam um `double`. O padrão C99 expandiu isso, adicionando versões para `float` (com sufixo `f`, ex: `sqrtf()`) e `long double` (com sufixo `l`, ex: `sqrtl()`).

### Abordagem do C++: `<cmath>`

Em C++, embora `#include <math.h>` funcione por razões de compatibilidade, a forma idiomática e preferida é usar o cabeçalho `<cmath>`:

```cpp
#include <cmath>
```

Usar `<cmath>` (em vez de `<math.h>`) traz duas vantagens principais:

1. **Namespace `std`:** As funções são (primariamente) colocadas dentro do namespace `std` (ex: `std::sqrt()`, `std::log()`). Isso evita poluir o escopo global e previne conflitos de nomes.
2. **Sobrecarga de Funções (Overloading):** O C++ usa sobrecarga para fornecer versões da mesma função para diferentes tipos, tornando o código mais limpo.

```cpp
// Em C, você precisa de funções diferentes:
float       raiz_f = sqrtf(4.0f);
double      raiz_d = sqrt(9.0);
long double raiz_l = sqrtl(16.0L);

// Em C++, o compilador escolhe a versão correta:
float       raiz_f = std::sqrt(4.0f);   // Chama std::sqrt(float)
double      raiz_d = std::sqrt(9.0);    // Chama std::sqrt(double)
long double raiz_l = std::sqrt(16.0L);  // Chama std::sqrt(long double)
```

### Armadilha de Compilação: Linkedição com `-lm`

Este é um dos problemas mais comuns enfrentados por iniciantes em C, especialmente em ambientes Linux ou macOS.

Considere este código simples:

**`raiz.c`**

```c
#include <stdio.h>
#include <math.h> // Incluiu o cabeçalho

int main() {
    double r = sqrt(9.0); // Usa a função sqrt
    printf("A raiz é %f\n", r);
    return 0;
}
```

Se você compilar apenas com `gcc raiz.c -o raiz`, o processo de compilação pode falhar na etapa final (linkedição) com um erro como **"undefined reference to `sqrt`"** (referência indefinida para `sqrt`).

**Por quê?**

1. `#include <math.h>` apenas informa ao _compilador_ que a função `sqrt` existe (seu protótipo).
2. O _código_ real da função `sqrt` (o código de máquina) reside em uma biblioteca separada, chamada `libm` (biblioteca matemática).
3. Por padrão, o _linkeditor_ (a parte do `gcc` que junta tudo) não procura nessa biblioteca para economizar tempo.

**Solução:** Você deve instruir manualmente o linkeditor a incluir a biblioteca matemática usando a flag **`-lm`**:

```bash
# O comando correto para compilar:
gcc raiz.c -o raiz -lm
```

- `-l` é a flag para "linkar" (link) uma biblioteca.
- `m` é o nome da biblioteca (`lib**m**`).

Esta etapa não é geralmente necessária em IDEs modernas como Visual Studio, que configuram isso automaticamente, mas é fundamental ao compilar manualmente na linha de comando.

## Funções de Potência e Raízes

Este grupo de funções lida com exponenciação e extração de raízes.

### Potenciação: `pow()`

A função `pow()` é usada para elevar um número (base) a uma potência (expoente).

- **Protótipo:** `double pow(double x, double y);`
- Calcula $x^y$ ($x$ elevado a $y$).
- `powf(float, float)` e `powl(long double, long double)` também existem.

```c
#include <stdio.h>
#include <math.h>

int main() {
    double r1 = pow(2.0, 3.0); // 2 ao cubo
    printf("pow(2.0, 3.0) = %.1f\n", r1); // Saída: 8.0

    double r2 = pow(10.0, -1.0); // 10 elevado a -1
    printf("pow(10.0, -1.0) = %.1f\n", r2); // Saída: 0.1

    double r3 = pow(100.0, 0.5); // 100 elevado a 0.5 (raiz quadrada)
    printf("pow(100.0, 0.5) = %.1f\n", r3); // Saída: 10.0
    
    // pow(base negativa, expoente fracionário) não é real
    double r4 = pow(-4.0, 0.5); // Raiz quadrada de -4
    printf("pow(-4.0, 0.5) = %f\n", r4); // Saída: nan (Not a Number)

    return 0;
}
```

### Raiz Quadrada: `sqrt()`

Embora `pow(x, 0.5)` funcione, `sqrt()` é mais rápida e precisa para calcular a raiz quadrada.

- **Protótipo:** `double sqrt(double x);`
- Calcula $\sqrt{x}$.
- **Restrição:** `x` deve ser não-negativo ( $\ge 0$ ). Se `x` for negativo, a função retorna `NaN` (Not a Number) e define a variável de erro global `errno` como `EDOM` (Erro de Domínio).

```c
#include <stdio.h>
#include <math.h>
#include <errno.h> // Para EDOM

int main() {
    double num_positivo = 25.0;
    double num_negativo = -9.0;

    // Caso 1: Número positivo
    double raiz_pos = sqrt(num_positivo);
    printf("A raiz quadrada de %.2f é %.2f\n", num_positivo, raiz_pos); // Saída: 5.00

    // Caso 2: Número negativo
    errno = 0; // Limpa o indicador de erro
    double raiz_neg = sqrt(num_negativo);
    
    if (errno == EDOM) {
        printf("Erro: Não é possível calcular a raiz de um número negativo (EDOM).\n");
    } else {
        // 'nan' (Not a Number) é um valor especial de ponto flutuante
        printf("A raiz de %.2f é %f\n", num_negativo, raiz_neg); // Saída: nan
    }

    return 0;
}
```

### Raiz Cúbica: `cbrt()` (C99)

A função `cbrt()` calcula a raiz cúbica.

- **Protótipo:** `double cbrt(double x);`
- Ao contrário de `sqrt`, `cbrt` **funciona perfeitamente com números negativos**.

```c
#include <stdio.h>
#include <math.h>

int main() {
    printf("cbrt(27.0) = %.1f\n", cbrt(27.0)); // Saída: 3.0
    printf("cbrt(-8.0) = %.1f\n", cbrt(-8.0)); // Saída: -2.0
    return 0;
}
```

### Hipotenusa: `hypot()` (C99)

A função `hypot()` calcula a hipotenusa de um triângulo retângulo com catetos `x` e `y`.

- **Protótipo:** `double hypot(double x, double y);`
- Calcula $\sqrt{x^2 + y^2}$.

Pode parecer desnecessário, já que `sqrt(pow(x, 2) + pow(y, 2))` faria o mesmo. No entanto, `hypot` é muito superior. Se `x` ou `y` forem números muito grandes (ex: $10^{200}$), `x*x` causaria um **overflow** (estouro), resultando em infinito, mesmo que o resultado final fosse representável. `hypot` usa um algoritmo interno que evita esse overflow, tornando-o numericamente estável e seguro.

```c
#include <stdio.h>
#include <math.h>

int main() {
    printf("hypot(3.0, 4.0) = %.1f\n", hypot(3.0, 4.0)); // Saída: 5.0
    return 0;
}
```

## Funções de Arredondamento e Truncamento

Este grupo de funções é usado para converter valores de ponto flutuante em inteiros (armazenados como `double`).

- **`floor(x)`:** Retorna o maior valor inteiro que é _menor ou igual_ a `x`. (Sempre arredonda para baixo).
- **`ceil(x)`:** Retorna o menor valor inteiro que é _maior ou igual_ a `x`. (Sempre arredonda para cima).
- **`trunc(x)` (C99):** Retorna o valor de `x` com a parte fracionária removida. (Sempre arredonda "em direção a zero").
- **`round(x)` (C99):** Retorna o inteiro _mais próximo_ de `x`. Se `x` estiver exatamente no meio (ex: `2.5`), arredonda para o inteiro mais distante de zero (`2.5` vira `3.0`, `-2.5` vira `-3.0`).

A tabela a seguir demonstra o comportamento dessas funções, que é a melhor forma de entendê-las:

| **Valor de `x`** | **`floor(x)`** | **`ceil(x)`** | **`trunc(x)`** | **`round(x)`** |
| ---------------- | -------------- | ------------- | -------------- | -------------- |
| **2.7**          | 2.0            | 3.0           | 2.0            | 3.0            |
| **2.3**          | 2.0            | 3.0           | 2.0            | 2.0            |
| **2.5**          | 2.0            | 3.0           | 2.0            | 3.0            |
| **-2.7**         | -3.0           | -2.0          | -2.0           | -3.0           |
| **-2.3**         | -3.0           | -2.0          | -2.0           | -2.0           |
| **-2.5**         | -3.0           | -2.0          | -2.0           | -3.0           |

Note que a conversão de tipo (casting) para `(int)` se comporta exatamente como `trunc()` para números positivos, mas pode diferir para negativos (embora em C/C++ modernos `(int)-2.7` seja `-2`, igual `trunc`).

```c
#include <stdio.h>
#include <math.h>

int main() {
    double val_pos = 2.7;
    double val_neg = -2.7;

    printf("Valor Positivo: %.1f\n", val_pos);
    printf("  floor: %.1f\n", floor(val_pos)); // Saída: 2.0
    printf("  ceil:  %.1f\n", ceil(val_pos));  // Saída: 3.0
    printf("  trunc: %.1f\n", trunc(val_pos)); // Saída: 2.0
    printf("  round: %.1f\n", round(val_pos)); // Saída: 3.0
    
    printf("\nValor Negativo: %.1f\n", val_neg);
    printf("  floor: %.1f\n", floor(val_neg)); // Saída: -3.0
    printf("  ceil:  %.1f\n", ceil(val_neg));  // Saída: -2.0
    printf("  trunc: %.1f\n", trunc(val_neg)); // Saída: -2.0
    printf("  round: %.1f\n", round(val_neg)); // Saída: -3.0
    
    // Para obter um resultado 'int'
    int resultado_inteiro = (int)round(val_pos);
    printf("\nResultado de round(%.1f) como int: %d\n", val_pos, resultado_inteiro); // Saída: 3

    return 0;
}
```

## Funções Exponenciais e Logarítmicas

Este grupo lida com a constante de Euler (_e_ ≈ 2.71828) e seus inversos, os logaritmos.

- **`exp(x)`:** Retorna $e^x$ (a função exponencial natural).
- **`log(x)`:** Retorna o logaritmo natural (base _e_) de `x`. ( $\ln x$ ). `x` deve ser positivo.
- **`log10(x)`:** Retorna o logaritmo na base 10 de `x`. `x` deve ser positivo.
- **`log2(x)` (C99):** Retorna o logaritmo na base 2 de `x`. `x` deve ser positivo.
- **`expm1(x)` (C99):** Calcula $e^x - 1$. Por que usar isso? Para valores de `x` muito próximos de zero (ex: `1e-10`), `exp(x)` será `1.000...001`. `exp(x) - 1` pode sofrer um erro de cancelamento catastrófico (perda de precisão). `expm1(x)` é um algoritmo especializado que mantém a precisão nesse caso.

### Fórmula de Mudança de Base

As bibliotecas C/C++ fornecem `log` (base _e_), `log10` e `log2`. E se você precisar de $\log_7(x)$? Você deve usar a fórmula matemática de mudança de base:

$\log_b(x) = \frac{\log_k(x)}{\log_k(b)}$

Onde `k` pode ser qualquer base (como _e_ ou 10).

```c
#include <stdio.h>
#include <math.h>

int main() {
    double valor = 100.0;
    
    // e^x
    printf("exp(1.0) = %f (Valor de 'e')\n", exp(1.0)); // Saída: 2.718282

    // Logaritmos
    printf("log(%.1f) (base e) = %f\n", exp(1.0), log(exp(1.0))); // Saída: 1.000000
    printf("log10(%.1f) = %f\n", valor, log10(valor));       // Saída: 2.000000
    printf("log2(8.0) = %f\n", log2(8.0));                   // Saída: 3.000000
    
    // Mudança de base: Calculando log na base 7 de 100
    double log_base7 = log(100.0) / log(7.0); 
    // ou: log10(100.0) / log10(7.0) -> 2.0 / log10(7.0)
    
    printf("log7(100.0) = %f\n", log_base7); // Saída: 2.366589
    
    // Verificação: pow(7.0, log_base7) deve ser 100
    printf("Verificação: 7^%f = %.1f\n", log_base7, pow(7.0, log_base7)); // Saída: 100.0

    return 0;
}
```

## Funções Trigonométricas

Este conjunto de funções é essencial para geometria, física e gráficos.

### Conceito de Radianos

A armadilha número um das funções trigonométricas em C/C++ é: **elas SEMPRE usam radianos, NUNCA graus.**

- $2\pi \text{ radianos} = 360 \text{ graus}$
- $\pi \text{ radianos} = 180 \text{ graus}$

Para converter:

- `radianos = graus * (PI / 180.0)`
- `graus = radianos * (180.0 / PI)`

O C não define uma constante `PI`. O cabeçalho `math.h` _pode_ definir `M_PI` (um padrão POSIX, mas não C), mas a forma 100% portátil de se obter Pi é usar `const double PI = acos(-1.0);`.

### Funções Diretas

- **`sin(x)`:** Seno de `x` (com `x` em radianos).
- **`cos(x)`:** Cosseno de `x` (com `x` em radianos).
- **`tan(x)`:** Tangente de `x` (com `x` em radianos).
- **`sinh(x)`, `cosh(x)`, `tanh(x)`:** Funções hiperbólicas.

### Funções Inversas (Arco)

- **`asin(x)`:** Arco seno. Retorna o ângulo (em radianos) cujo seno é `x`. `x` deve estar em `[-1.0, 1.0]`. Retorna em `[-PI/2, PI/2]`.
- **`acos(x)`:** Arco cosseno. `x` deve estar em `[-1.0, 1.0]`. Retorna em `[0, PI]`.
- **`atan(x)`:** Arco tangente. Retorna em `[-PI/2, PI/2]`.
- **`atan2(y, x)`:** A função "inteligente" do arco tangente. Ela calcula `atan(y/x)`, mas usa os sinais de `y` e `x` para determinar o quadrante correto, retornando um ângulo em `[-PI, PI]`. Ela também lida com `x=0` (evitando divisão por zero).

```c
#include <stdio.h>
#include <math.h>

int main() {
    const double PI = acos(-1.0);
    double angulo_graus = 45.0;
    
    // 1. Converter graus para radianos
    double angulo_rad = angulo_graus * (PI / 180.0); // 45 graus = PI/4 radianos
    
    printf("PI = %f\n", PI);
    printf("sin(45 deg) = %f\n", sin(angulo_rad)); // Aprox. 0.707
    printf("cos(45 deg) = %f\n", cos(angulo_rad)); // Aprox. 0.707
    printf("tan(45 deg) = %f\n", tan(angulo_rad)); // Aprox. 1.0

    // 2. Por que atan2 é melhor que atan
    double x1 = 1.0, y1 = 1.0;   // Quadrante 1
    double x2 = -1.0, y2 = -1.0; // Quadrante 3
    
    // atan(y/x) não consegue distinguir
    printf("atan(1/1) = %f rad\n", atan(y1/x1));   // Aprox. 0.785 (45 deg)
    printf("atan(-1/-1) = %f rad\n", atan(y2/x2)); // Aprox. 0.785 (45 deg) - ERRADO!
    
    // atan2 sabe o quadrante
    printf("atan2(1, 1) = %f rad\n", atan2(y1, x1));     // Aprox. 0.785 (45 deg)
    printf("atan2(-1, -1) = %f rad\n", atan2(y2, x2)); // Aprox. -2.356 (-135 deg) - CORRETO!

    return 0;
}
```

## Funções de Valor Absoluto

Este é um ponto comum de confusão, pois existem funções diferentes para tipos diferentes.

- **`abs(int x)`:** Fica em `<stdlib.h>`. Retorna o valor absoluto de um **`int`**.
- **`labs(long int x)`:** Fica em `<stdlib.h>`. Retorna o valor absoluto de um **`long int`**.
- **`llabs(long long int x)` (C99):** Fica em `<stdlib.h>`. Retorna o valor absoluto de um **`long long int`**.
- **`fabs(double x)`:** Fica em `<math.h>`. Retorna o valor absoluto de um **`double`**. `float`s são promovidos para `double`.

**O que acontece se você misturar?**

`abs(-5.5)`: O `double -5.5` será truncado para `int -5`. `abs(-5)` retorna `5`. Você perdeu a parte fracionária.

**Em C++:** A biblioteca `<cmath>` fornece `std::abs` que é _sobrecarregado_ para todos os tipos (e `std::fabs` para compatibilidade), simplificando a escolha.

```cpp
#include <stdio.h>
#include <stdlib.h> // Para abs()
#include <math.h>   // Para fabs()

int main() {
    int num_int = -5;
    double num_double = -7.75;

    printf("abs(%d) = %d\n", num_int, abs(num_int));     // Saída: 5
    printf("fabs(%.2f) = %.2f\n", num_double, fabs(num_double)); // Saída: 7.75
    
    // A armadilha
    printf("abs(%.2f) = %d\n", num_double, abs(num_double)); // Errado! Saída: 7
    // O compilador (com warnings) converte -7.75 para -7 (int) *antes* de chamar abs().
    
    return 0;
}
```

## Funções de Máximo e Mínimo

Encontrar o maior ou o menor de dois números é uma operação trivial, mas cheia de armadilhas em C.

**Em C Padrão (C89):** Não existem funções `max(x, y)` ou `min(x, y)`.

**Opção 1: Funções Manuais (Seguro, mas não genérico)**

```c
int max_int(int a, int b) {
    return (a > b) ? a : b;
}
double max_double(double a, double b) {
    return (a > b) ? a : b;
}
```

Isso é seguro, mas você precisa de uma função para cada tipo.

**Opção 2: Macros (Genérico, mas PERIGOSO)**

```c
#define MAX_C(a, b) ((a) > (b) ? (a) : (b))
```

Isso parece funcionar para `int`, `double`, etc.

Armadilha da Macro (Múltipla Avaliação): O que acontece se você fizer `MAX_C(x++, y++)`?

O pré-processador expande para: `((x++) > (y++) ? (x++) : (y++))`

Suponha `x = 5`, `y = 10`.

1. `(5 > 10)` é avaliado.
2. `x` vira `6`. `y` vira `11`.
3. A condição é falsa, então o lado `(y++)` é executado.
4. O valor `11` é retornado. `y` vira `12`. O resultado é `11`, mas `x` foi incrementado uma vez (para `6`) e `y` foi incrementado duas vezes (para `12`). Isso é um bug catastrófico.

**Opção 3: C99 `fmax()` e `fmin()` (Seguro, mas limitado)**

O C99 introduziu `fmax(double x, double y)` e `fmin(double x, double y)` em `<math.h>`.

- Elas são seguras e funcionam corretamente.
- Elas só funcionam para tipos de **ponto flutuante** (`double`, `float` com `fmaxf`, `long double` com `fmaxl`). Elas _não_ são para `int`.

**Opção 4: C++ (Solução Correta)**

C++ oferece `std::max` e `std::min` na biblioteca `<algorithm>`. Elas são templates, o que significa que são genéricas e seguras.

```cpp
// Exemplo em C++
#include <iostream>
#include <algorithm> // Para std::max e std::min

int main() {
    int x = 100, y = 50;
    std::cout << "O máximo é " << std::max(x, y) << std::endl; // Saída: 100
    
    double d1 = 5.5, d2 = 5.1;
    std::cout << "O mínimo é " << std::min(d1, d2) << std::endl; // Saída: 5.1
    
    int a = 5;
    int b = 10;
    // std::max é seguro, 'a' e 'b' são incrementados apenas uma vez.
    std::cout << "Max(a++, b++) = " << std::max(a++, b++); // Saída: 10
    std::cout << "a final = " << a << ", b final = " << b << std::endl; // Saída: a=6, b=11
    return 0;
}
```

## Outras Funções de Ponto Flutuante

A tabela a seguir lista o restante das funções úteis de `<math.h>`, muitas das quais são do C99.

|**Função**|**Protótipo (C99)**|**Descrição**|
|---|---|---|
|`fmod`|`double fmod(double x, double y)`|Retorna o resto da divisão de ponto flutuante de `x/y` (o `%` para `double`s).|
|`fma`|`double fma(double x, double y, double z)`|Calcula `(x * y) + z` como uma única operação (Fused Multiply-Add), para alta precisão.|
|`fdim`|`double fdim(double x, double y)`|Retorna a diferença positiva (`x-y` se `x>y`, senão `0`).|
|`log1p`|`double log1p(double x)`|Calcula `log(1 + x)`. Mais preciso que `log(1+x)` para `x` muito pequeno.|

### Função `fmod(x, y)`

O operador `%` não funciona com `double`. `fmod` é o seu substituto.

```c
#include <stdio.h>
#include <math.h>

int main() {
    printf("fmod(10.5, 3.0) = %.1f\n", fmod(10.5, 3.0)); // Saída: 1.5
    printf("fmod(-10.5, 3.0) = %.1f\n", fmod(-10.5, 3.0)); // Saída: -1.5 (sinal do dividendo)
    return 0;
}
```

### Função `fma(x, y, z)`

Calcula `(x * y) + z`. Por que usar isso?

O `fma` (Fused Multiply-Add) é uma instrução de hardware em muitas CPUs. Ela realiza a multiplicação e a adição com apenas um erro de arredondamento no final. Por exemplo, `resultado = (x * y) + z;` tem dois erros de arredondamento (um no `*`, um no `+`). O `fma` é mais preciso e, muitas vezes, mais rápido.

## Considerações Finais

Neste capítulo, exploramos o conjunto de funções matemáticas fornecidas pela biblioteca padrão C (`<math.h>`) e, por extensão, pela biblioteca `<cmath>` do C++. Vimos que, para além da aritmética simples, a programação C/C++ oferece um arsenal robusto para cálculos científicos, financeiros e gráficos.

Discutimos a importância crítica de **linkar a biblioteca matemática (`-lm`)** em ambientes de linha de comando, uma fonte comum de frustração para iniciantes. Agrupamos e detalhamos as funções por categoria, explorando as nuances de:

- **Potência e Raízes:** `pow`, `sqrt` (e seu erro de domínio com negativos), `cbrt` (que aceita negativos) e `hypot` (para precisão em overflows).
- **Arredondamento:** A diferença fundamental entre `floor` (piso), `ceil` (teto), `trunc` (em direção a zero) e `round` (ao mais próximo).
- **Logaritmos:** O trio `log` (base _e_), `log10` e `log2`, e como usar a fórmula de mudança de base.
- **Trigonometria:** A regra de ouro de usar **radianos** e a superioridade de `atan2` sobre `atan` para determinar quadrantes.
- **Absoluto e Max/Min:** As armadilhas de `abs` (`<stdlib.h>`) vs. `fabs` (`<math.h>`), e o perigo de macros `MAX` com efeitos colaterais (`++`), contrastando com as soluções seguras `fmax` (C99) e `std::max` (C++).

Ao utilizar as funções da biblioteca padrão, economiza-se tempo de desenvolvimento e garante-se o uso de implementações otimizadas e numericamente estáveis. Com esse conhecimento, estamos prontos para avançar para as estruturas de controle, que nos permitirão tomar decisões (`if`/`else`) baseadas nesses cálculos.
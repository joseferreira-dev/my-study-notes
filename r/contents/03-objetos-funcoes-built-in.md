# Capítulo 3 — Funções Built-in

Uma das maiores forças do R é seu vasto conjunto de funções incorporadas (built-in), que permitem realizar desde operações matemáticas simples até cálculos estatísticos e manipulações de dados complexas. A seguir, exploraremos de forma detalhada algumas das funções built-in mais utilizadas, com exemplos práticos e explicações de seus comportamentos.

## Funções Matemáticas Fundamentais

### `abs(x)`

A função `abs(x)` retorna o valor absoluto de `x`, ou seja, transforma números negativos em seus correspondentes positivos, enquanto mantém números positivos inalterados.

```r
abs(-5)  # Resultado: 5
abs(3)   # Resultado: 3
```

O valor absoluto é particularmente útil em contextos onde apenas a magnitude de um número é relevante, como em cálculos de distância.

### `log(x, base)`, `log(x)`, `log10(x)`

O R oferece diversas formas de calcular logaritmos. A função `log(x, base)` calcula o logaritmo de `x` na base especificada. Caso a base não seja informada, a função calcula o logaritmo natural, ou seja, na base `e ≈ 2.718`.

```r
log(8, base = 2)  # Resultado: 3, pois 2³ = 8
log(8)            # Resultado: logaritmo natural de 8
log10(1000)       # Resultado: 3, pois 10³ = 1000
```

O `log10(x)` é uma função específica para calcular o logaritmo na base 10 de `x`, muito utilizado em escalas logarítmicas e ciências aplicadas.

### `exp(x)`

A função `exp(x)` calcula o exponencial de `x`, ou seja, `e` elevado à potência `x`.

```r
exp(1)  # Resultado: 2.718282 (aproximadamente o valor de e)
```

Esta função é essencial em modelos de crescimento exponencial e regressões log-lineares.

## Funções Trigonométricas

R também oferece suporte a funções trigonométricas, com entradas e saídas baseadas em radianos.

- `sin(x)` retorna o seno de `x`.
- `cos(x)` retorna o cosseno de `x`.
- `tan(x)` retorna a tangente de `x`.

```r
sin(pi / 2)  # Resultado: 1
cos(0)       # Resultado: 1
tan(pi / 4)  # Resultado: 1
```

Essas funções são fundamentais em áreas como geometria analítica, física e modelagem de ondas.

## Funções de Arredondamento

A função `round(x, digits = n)` arredonda `x` para `n` casas decimais. Se `n` não for especificado, o valor padrão é `0`, arredondando para o inteiro mais próximo.

```r
round(3.14159, digits = 2)  # Resultado: 3.14
```

Outras duas funções úteis são `ceiling(x)` e `floor(x)`. `ceiling(x)` retorna o menor número inteiro maior ou igual a `x`. Já `floor(x)` retorna o maior número inteiro menor ou igual a `x`.

```r
ceiling(3.2)  # Resultado: 4
floor(3.8)    # Resultado: 3
```

Essas funções são úteis em alocações, contagens de unidades e problemas que exigem discretização para cima ou para baixo.

## Funções de Análise de Vetores

A função `length(x)` retorna o número de elementos em um objeto, geralmente utilizado para vetores e listas.

```r
vetor <- c(1, 2, 3, 4, 5)
length(vetor)  # Resultado: 5
```

As funções `sum(x)` e `prod(x)` retornam, respectivamente a soma e o produtos dos elementos de `x`.

```r
sum(c(1, 2, 3))   # Resultado: 6
prod(c(1, 2, 3))  # Resultado: 6
```

Essas funções são amplamente empregadas em estatísticas descritivas e cálculos agregados.

Outras três funções úteis são `max(x)`, `min(x)` e `range(x)`. `max(x)` e `min(x)` retornam o maior e o menor valor de `x`, enquanto que `range(x)` retorna um vetor com o menor e o maior valor de `x`.

```r
max(c(1, 5, 3))    # Resultado: 5
min(c(1, 5, 3))    # Resultado: 1
range(c(1, 5, 3))  # Resultado: 1 5
```

Essas funções são vitais para análises de amplitude, variação e em processos de normalização de dados.

## Outras Funções Úteis

Além das funções mencionadas, existem muitas outras funções built-in relevantes em R, como:

- `sqrt(x)`: retorna a raiz quadrada de `x`.
- `sign(x)`: indica o sinal de `x` (-1 para negativos, 1 para positivos e 0 para zero).
- `mean(x)`: calcula a média aritmética dos elementos de `x`.
- `var(x)`: calcula a variância dos elementos de `x`.
- `sd(x)`: calcula o desvio padrão dos elementos de `x`.
- `median(x)`: calcula a mediana dos elementos de `x`.
- `factorial(x)`: retorna o fatorial de `x`.

Essas funções tornam o R extremamente poderoso para análise estatística e científica, muitas vezes permitindo que cálculos sofisticados sejam realizados com uma única linha de código.

---

Neste capítulo, exploramos os conceitos de objetos em R e examinamos detalhadamente diversas funções built-in que formam a espinha dorsal de muitas operações básicas. Com esses conhecimentos, já é possível manipular e transformar dados de maneira eficiente, realizando desde operações aritméticas simples até análises estatísticas iniciais. A compreensão sólida destes conceitos é imprescindível para avançarmos para estruturas de dados mais complexas, manipulação de grandes conjuntos de dados e desenvolvimento de funções personalizadas, temas que abordaremos nos próximos capítulos.

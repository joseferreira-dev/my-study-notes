# Capítulo 2 — Variáveis, Tipos de Dados e Operadores

## Atribuição de Variáveis

Em R, uma das primeiras operações que realizamos ao programar é a atribuição de valores a variáveis. O conceito de variável é fundamental em qualquer linguagem de programação, pois ela armazena dados que podem ser utilizados posteriormente em cálculos, manipulação ou visualização.

A atribuição em R é feita através do operador `<-`, que é o método preferido por convenção, embora o sinal de igual `=` também possa ser utilizado. A seguir, vamos ver um exemplo de como atribuir um valor a uma variável em R:

```r
x <- 10
```

Neste caso, o valor `10` foi atribuído à variável `x`. Essa variável pode agora ser usada em qualquer lugar no código, e o R irá referenciar seu valor quando necessário. Alternativamente, podemos usar o sinal de igual:

```r
x = 10
```

Ambos os métodos têm o mesmo efeito, mas o uso do operador `<-` é mais recomendado porque é um padrão na comunidade R, sendo mais facilmente reconhecido como uma operação de atribuição.

Além disso, o R permite a atribuição múltipla, o que significa que podemos atribuir o mesmo valor a várias variáveis ao mesmo tempo, como demonstrado abaixo:

```r
a <- b <- c <- 25
```

Agora, as variáveis `a`, `b` e `c` possuem o valor `25`. Esse tipo de atribuição pode ser útil quando desejamos inicializar várias variáveis com o mesmo valor de forma concisa.

## Nomenclatura de Variáveis

A nomenclatura de variáveis em R segue algumas regras importantes. Em primeiro lugar, o nome de uma variável deve começar com uma letra ou com um ponto (que não seja o primeiro caractere). Depois disso, podem ser utilizados números, letras e o caractere de sublinhado (`_`). Por exemplo, os nomes de variáveis válidos são:

```r
variavel1
x
nome_do_usuario
_dados
```

Por outro lado, nomes de variáveis não podem começar com um número nem conter espaços, como:

```r
1variavel   # Inválido
minha variavel  # Inválido
```

Além disso, há alguns nomes reservados que não podem ser usados como nomes de variáveis, como palavras-chave do R (ex.: `if`, `for`, `function`). Sempre que possível, utilize nomes descritivos para as suas variáveis, o que facilita a legibilidade do código, especialmente em projetos mais complexos. Exemplos:

```r
idade_usuario
preco_produto
dados_clientes
```

Com uma boa nomenclatura, seu código será muito mais claro e fácil de entender, mesmo depois de algum tempo.

## Tipos de Dados Básicos, Coerção e Conversão de Tipos

Em R, as variáveis podem armazenar diferentes tipos de dados, e entender esses tipos é essencial para realizar qualquer tipo de operação corretamente. R possui uma série de tipos de dados básicos que podemos utilizar em nossas variáveis. Vamos explorar os principais:

### Tipos de Dados Básicos:

- **Numérico (numeric)**: Representa números reais (decimais). Este é o tipo padrão quando você define um número no R.
  
  ```r
  numero <- 3.14
  ```

- **Inteiro (integer)**: Representa números inteiros. Para definir um número inteiro, podemos usar o sufixo `L` após o número.

  ```r
  inteiro <- 5L
  ```

- **Caractere (character)**: Representa texto ou sequências de caracteres, que são envoltas por aspas (`"` ou `'`).
  
  ```r
  nome <- "João"
  ```

- **Lógico (logical)**: Representa valores booleanos, ou seja, `TRUE` (verdadeiro) ou `FALSE` (falso).
  
  ```r
  status <- TRUE
  ```

- **Componente complexos (complex)**: Representa números complexos, que podem ser úteis em algumas operações matemáticas avançadas.

  ```r
  numero_complexo <- 3 + 4i
  ```

### Coerção de Tipos (ou conversão automática)

Em R, a coerção de tipos pode ocorrer automaticamente, o que significa que o R tenta converter os dados de um tipo para outro, quando necessário, para realizar operações. Isso é feito de acordo com uma ordem de precedência: sempre que for possível, o R tentará manter o tipo mais geral. Por exemplo:

```r
numero <- 3.14
inteiro <- 5L
resultado <- numero + inteiro
```

Aqui, o R converte automaticamente o número inteiro `5L` para um número numérico `5.0` antes de realizar a soma, resultando em `8.14`. Essa coerção automática é conveniente, mas é importante estar ciente de que nem todas as conversões serão feitas sem erros ou sem perda de dados.

### Conversão de Tipos

Embora o R faça coerção automaticamente, em algumas situações, podemos precisar realizar conversões explícitas de tipos. Para isso, o R fornece funções específicas para cada tipo. Vamos ver alguns exemplos:

- **De numérico para inteiro**:

  ```r
  num <- 3.14
  inteiro <- as.integer(num)
  ```

  Aqui, a função `as.integer()` converte o número `3.14` em `3`, descartando a parte decimal.

- **De inteiro para numérico**:

  ```r
  inteiro <- 5L
  num <- as.numeric(inteiro)
  ```

  A função `as.numeric()` converte um número inteiro em um número numérico.

- **De numérico para caractere**:

  ```r
  num <- 3.14
  texto <- as.character(num)
  ```

  Neste exemplo, `as.character()` converte um número numérico em uma string de texto.

Com essas conversões em mente, você pode manipular tipos de dados no R de maneira mais controlada e específica, o que é essencial em operações mais complexas e em grandes volumes de dados.

## Expressões e Operadores Básicos

Agora que compreendemos como trabalhar com variáveis e tipos de dados, é hora de explorar os operadores que nos permitem realizar operações sobre esses dados. Os operadores em R são divididos em várias categorias: aritméticos, lógicos e de comparação.

### Operadores Aritméticos

Os operadores aritméticos são usados para realizar operações matemáticas. Eles incluem:

- **Soma (`+`)**:

  ```r
  a <- 5
  b <- 3
  soma <- a + b  # Resultado: 8
  ```

- **Subtração (`-`)**:

  ```r
  subtracao <- a - b  # Resultado: 2
  ```

- **Multiplicação (`*`)**:

  ```r
  multiplicacao <- a * b  # Resultado: 15
  ```

- **Divisão (`/`)**:

  ```r
  divisao <- a / b  # Resultado: 1.666...
  ```

- **Exponenciação (`^`)**:

  ```r
  expoente <- a^b  # Resultado: 125
  ```

- **Módulo (`%%`)**: Retorna o restante de uma divisão.

  ```r
  modulo <- a %% b  # Resultado: 2
  ```

- **Divisão inteira (`%/%`)**: Retorna a parte inteira de uma divisão.

  ```r
  div_inteira <- a %/% b  # Resultado: 1
  ```

Esses operadores são usados no cotidiano da programação para cálculos matemáticos simples e complexos.

### Operadores de Comparação

Os operadores de comparação são usados para comparar valores. Eles retornam um valor lógico (`TRUE` ou `FALSE`). Exemplos incluem:

- **Igualdade (`==`)**:

  ```r
  a == b  # Retorna FALSE, pois 5 não é igual a 3
  ```

- **Desigualdade (`!=`)**:

  ```r
  a != b  # Retorna TRUE, pois 5 é diferente de 3
  ```

- **Maior que (`>`)**:

  ```r
  a > b  # Retorna TRUE, pois 5 é maior que 3
  ```

- **Menor que (`<`)**:

  ```r
  a < b  # Retorna FALSE, pois 5 não é menor que 3
  ```

- **Maior ou igual a (`>=`)**:

  ```r
  a >= b  # Retorna TRUE, pois 5 é maior ou igual a 3
  ```

- **Menor ou igual a (`<=`)**:

  ```r
  a <= b  # Retorna FALSE, pois 5 não é menor ou igual a 3
  ```

### Operadores Lógicos

Os operadores lógicos são usados para realizar operações lógicas entre valores lógicos. Eles incluem:

- **E lógico (`&`)**:

  ```r
  TRUE & FALSE  # Retorna FALSE
  ```

- **Ou lógico (`|`)**:

  ```r
  TRUE | FALSE  # Retorna TRUE
  ```

- **Negação (`!`)**:

  ```r
  !TRUE  # Retorna FALSE
  ```

Esses operadores são úteis para realizar decisões condicionais em seu código, como filtrar dados com base em condições específicas.

---

Esse capítulo foi uma introdução detalhada aos fundamentos da linguagem R, abordando como trabalhar com variáveis, tipos de dados e operadores, essenciais para qualquer análise ou projeto. Com os exemplos fornecidos, você pode começar a construir seu código e experimentar as funcionalidades do R em um ambiente interativo e exploratório.

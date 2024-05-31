<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Operadores

- [Introdução](#introdução)
- [Operadores Artiméticos](#operadores-aritméticos)
- [Operadores de Comparação](#operadores-de-comparação)
- [Operadores Lógicos](#operadores-lógicos)
  * [Operador E Lógico `&&`](#operador-e-lógico-)
  * [Operador OU Lógico `||`](#operador-ou-lógico-)
  * [Operador NÃO Lógico `!`](#operador-não-lógico-)
  * [Combinação de Operadores Lógicos](#combinação-de-operadores-lógicos)
  * [Precedência de Operadores Lógicos](#precedência-de-operadores-lógicos)

## Introdução

Operadores são símbolos que informam ao compilador ou intérprete para realizar operações específicas em um ou mais operandos (valores ou variáveis). Em Dart, assim como em outras linguagens de programação, existem diversos tipos de operadores, cada um com uma finalidade específica.

## Operadores Aritméticos

Esses operadores são usados para realizar operações matemáticas comuns.

| Símbolo | Significado                |
|---------|----------------------------|
| `+`     | Soma                       |
| `-`     | Subtração                  |
| `*`     | Multiplicação              |
| `/`     | Divisão                    |
| `~/`    | Divisão inteira            |
| `%`     | Resto da divisão           |

Eles funcionam da seguinte maneira:

```dart
void main() {
  int a = 5;
  int b = 2;

  print(a + b);  // Output: 7
  print(a - b);  // Output: 3
  print(a * b);  // Output: 10
  print(a / b);  // Output: 2.5
  print(a ~/ b); // Output: 2
  print(a % b);  // Output: 1
}
```

## Operadores de Atribuição

Esses operadores são usados para atribuir valores a variáveis.

| Símbolo | Significado                    |
|---------|--------------------------------|
| `=`     | Atribuição                     |
| `+=`    | Atribuição com adição          |
| `-=`    | Atribuição com subtração       |
| `*=`    | Atribuição com multiplicação   |
| `/=`    | Atribuição com divisão         |
| `~/=`   | Atribuição com divisão inteira |
| `%=`    | Atribuição com resto           |

Eles funcionam da seguinte maneira:

```dart
void main() {
  int     a = 5;
  int     b = 2;
  int     c = 0;
  double  d = 12.5;
  
  c += a;
  print(c); // Output: 5
  c -= b;
  print(c); // Output: 3
  c *= a;
  print(c); // Output: 15
  d /= a;
  print(d); // Output: 2.5
  c ~/= a;
  print(c); // Output: 3
  c %= b;
  print(c); // Output: 1
}
```

## Operadores de Comparação

Os operadores de comparação são utilizados para comparar valores. Eles retornam um valor booleano (`true` ou `false`) com base na comparação realizada. São eles:

| Símbolo | Significado                |
|---------|----------------------------|
| `==`    | Igualdade                  |
| `!=`    | Desigualdade               |
| `>`     | Maior que                  |
| `<`     | Menor que                  |
| `>=`    | Maior ou igual que         |
| `<=`    | Menor ou igual que         |

E funcionam da seguinte maneira:

```dart
void main() {
  int a = 5;
  int b = 5;
  int c = 10;

  print(a == b); // true
  print(a == c); // false

  print(a != b); // false
  print(a != c); // true

  print(c > a); // true
  print(a > b); // false

  print(b < c); // true
  print(b < a); // false

  print(b >= a); // true
  print(a >= c); // false

  print(b <= a); // true
  print(c <= b); // false
}
```

## Operadores Lógicos

Os operadores lógicos em Dart são usados para combinar expressões booleanas e tomar decisões com base em várias condições. Eles são essenciais para o controle de fluxo em programas.

### Operador E Lógico `&&`

O operador E lógico retorna `true` se ambas as expressões forem verdadeiras. Caso contrário, retorna `false`.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;

  bool resultado = condicao1 && condicao2; // resultado será false
  print('Resultado de condicao1 && condicao2: $resultado'); // Output: false
}
~~~

### Operador OU Lógico `||`

O operador OU lógico retorna `true` se pelo menos uma das expressões for verdadeira. Caso contrário, retorna `false`.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;

  bool resultado = condicao1 || condicao2; // resultado será true
  print('Resultado de condicao1 || condicao2: $resultado'); // Output: true
}
~~~

### Operador NÃO Lógico `!`

O operador NÃO lógico inverte o valor de uma expressão booleana. Se a expressão for `true`, o operador retorna `false`, e vice-versa.

~~~dart
void main() {
  bool condicao = true;

  bool resultado = !condicao; // resultado será false
  print('Resultado de !condicao: $resultado'); // Output: false
}
~~~

### Combinação de Operadores Lógicos

Pode-se combinar múltiplos operadores lógicos para criar expressões mais complexas.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;
  bool condicao3 = true;

  // Combinação de operadores lógicos
  bool resultado = (condicao1 && condicao3) || condicao2; // resultado será true
  print('Resultado da combinação: $resultado'); // Output: true
}
~~~

### Precedência de Operadores Lógicos

Os operadores lógicos têm uma precedência específica que determina a ordem em que são avaliados. Em Dart, a precedência é a seguinte (da mais alta para a mais baixa):

1. `!` (NÃO lógico)
2. `&&` (E lógico)
3. `||` (OU lógico)

Pode-se usar parênteses para alterar a ordem de avaliação e tornar a expressão mais clara.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;
  bool condicao3 = true;

  // Sem parênteses, && tem precedência sobre ||
  bool resultado1 = condicao1 && condicao2 || condicao3; // resultado1 será true

  // Com parênteses, a avaliação é alterada
  bool resultado2 = condicao1 && (condicao2 || condicao3); // resultado2 será false
  print('Resultado sem parênteses: $resultado1'); // Output: true
  print('Resultado com parênteses: $resultado2'); // Output: false
}
~~~
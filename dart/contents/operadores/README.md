<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Operadores

- [Introdução](#introdução)
- [Operadores Artiméticos](#operadores-aritméticos)
- [Operadores de Atribuição](#operadores-de-atribuição)
- [Operadores de Incremento e Decremento](#operadores-de-incremento-e-decremento)
  - [Operador de Incremento](#operador-de-incremento)
  - [Operador de Decremento](#operador-de-decremento)
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

## Operadores de Incremento e Decremento

Os operadores de incremento e decremento são operadores unários que aumentam ou diminuem o valor de uma variável numérica em uma unidade. Esses operadores são úteis para simplificar expressões que, de outra forma, exigiriam operações aritméticas mais longas.

### Operador de Incremento

O operador de incremento adiciona 1 ao valor da variável. Existem duas formas de usar o operador de incremento `++`.

No pré-incremento, onde a variável é incrementada em 1 antes de ser usada na expressão.

```dart
void main() {
  int a = 5;
  int b = ++a; // a é incrementado para 6, então b recebe o valor 6
  print(a); // Output: 6
  print(b); // Output: 6
}
```

E no pós-incremento, a variável é usada na expressão e, em seguida, é incrementada em 1.

```dart
void main() {
  int a = 5;
  int b = a++; // b recebe o valor 5, então a é incrementado para 6
  print(a); // Output: 6
  print(b); // Output: 5
}
```

### Operador de Decremento

O operador de decremento subtrae 1 do valor da variável. Existem duas formas de usar o operador de decremento `--`.

No pré-decremento, a variável é decrementada em 1 antes de ser usada na expressão.

```dart
void main() {
  int a = 5;
  int b = --a; // a é decrementado para 4, então b recebe o valor 4
  print(a); // Output: 4
  print(b); // Output: 4
}
```

E no pós-decremento, a variável é usada na expressão e, em seguida, é decrementada em 1.

```dart
void main() {
  int a = 5;
  int b = a--; // b recebe o valor 5, então a é decrementado para 4
  print(a); // Output: 4
  print(b); // Output: 5
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

## Operadores de Validação de Tipo

Estes operadores são utilizados para validação e conversão de tipos em tempo de execução, bem como para testar se um objeto pertence a um tipo específico. Eles são ferramentas essenciais para a programação orientada a objetos em Dart, permitindo escrever código mais seguro e expressivo.

### Operador `is`

O operador `is` é usado para verificar se um objeto é de um determinado tipo. Ele retorna `true` se o objeto for uma instância do tipo especificado ou de uma subclasse desse tipo.

```dart
void main() {
  var texto = 'hello';
  
  if (texto is String) {
    print('Texto é uma string');
  } else {
    print('Texto não é uma string');
  }
}
```

### Operador `is!`

O operador `is!` é o oposto do operador `is`. Ele verifica se um objeto não é de um determinado tipo. Ele retorna `true` se o objeto não for uma instância do tipo especificado.


```dart
void main() {
  var numero = 42;
  
  if (numero is! String) {
    print('Número não é uma string');
  } else {
    print('Número é uma string');
  }
}
```

### Operador `as`

O operador `as` é usado para fazer uma conversão de tipo explícita (`type cast`). Ele tenta converter um objeto para o tipo especificado e lança uma exceção (`TypeError`) se a conversão não for possível.

```dart
void main() {
  dynamic obj = 'hello';
  
  var texto = obj as String;
  print(texto.length); // Output: 5
}
```

## Operador Cascade `..`

O operador cascade `..` em Dart é uma ferramenta que permite encadear uma série de operações em um objeto, economizando código e tornando as operações em objetos mais legíveis e concisas. Ele é usado principalmente para chamar vários métodos ou definir várias propriedades no mesmo objeto sem precisar repetir o nome do objeto.

Basicamente, quando se usa o operador cascade, ele retorna o próprio objeto, permitindo que se encadeie várias operações em uma única expressão. Isso é útil para evitar a repetição do nome do objeto.

Considerando-se uma classe `Pessoa`:

```dart
class Pessoa {
  String? nome;
  int? idade;
  
  void saudacao() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}
```

Sem o operador cascade, a configuração de uma instância de `Pessoa` seria algo assim:

```dart
void main() {
  var pessoa = Pessoa();
  pessoa.nome = 'João';
  pessoa.idade = 30;
  pessoa.saudacao(); // Output: Olá, meu nome é João e eu tenho 30 anos.
}
```

Com o operador cascade, isso se torna mais conciso:

```dart
void main() {
  var pessoa = Pessoa()
    ..nome = 'João'
    ..idade = 30
    ..saudacao();  // Output: Olá, meu nome é João e eu tenho 30 anos.
}
```

O operador cascade também é muito útil ao se trabalhar com listas, onde se pode encadear operações consecutivas:

```dart
void main() {
  var lista = [7]
    ..add(4)
    ..add(5)
    ..add(6)
    ..addAll([1, 2, 3])
    ..sort();
  
  print(lista); // Output: [1, 2, 3, 4, 5, 6, 7]
}
```

## Operador Spread `...`

O operador spread `...` é usado para expandir elementos de coleções (como listas, sets e maps) em outra coleção. Ele permite combinar, copiar e construir coleções de forma concisa e legível.

### Operador Spread em Listas

O operador spread é mais comumente utilizado com listas para adicionar os elementos de uma lista a outra lista.

```dart
void main() {
  // Exemplo 1
  var lista1 = [1, 2, 3];
  var lista2 = [4, 5, 6];
  var combinada = [...lista1, ...lista2];
  print(combinada); // Output: [1, 2, 3, 4, 5, 6]
  
  // Exemplo 2
  var listaOriginal = [1, 2, 3];
  var novaLista = [0, ...listaOriginal, 4];
  print(novaLista); // Output: [0, 1, 2, 3, 4]
}
```

### Operador Spread em Sets

O operador spread também pode ser usado com conjuntos para combinar múltiplos conjuntos em um único conjunto, funcionando de forma muito semelhante às listas, exceto pelo fato de que elementos repetidos são eliminados.

```dart
void main() {
  var conjunto1 = {1, 2, 3};
  var conjunto2 = {3, 4, 5};
  var combinado = {...conjunto1, ...conjunto2};
  
  print(combinado); // Output: {1, 2, 3, 4, 5}
}
```

### Operador Spread em Maps

Com mapas, o operador spread permite combinar múltiplos mapas em um único mapa, sendo que caso os maps utilizados possuam chaves iguais, elas serão sobrescritas. Como no exemplo baixo, onde a chave `'b'` no `map2` substitui a chave `'b'` no `map1`.

```dart
void main() {
  var map1 = {'a': 1, 'b': 2};
  var map2 = {'b': 3, 'c': 4};
  var combinado = {...map1, ...map2};
  
  print(combinado); // Output: {a: 1, b: 3, c: 4}
}
```

## Operadores de Nulidade

Os operadores de nulidade em Dart ajudam a lidar com valores que podem ser nulos. Eles oferecem maneiras seguras e concisas de acessar e manipular esses valores, evitando exceções de nulidade.

### Operador Ternário para Nulos `??`

O operador de ternário para nulos `??` retorna o operando do lado esquerdo se ele não for `null`; caso contrário, ele retorna o operando do lado direito.

```dart
void main() {
  String? nome;
  var saudacao = nome ?? 'Olá, visitante!';
  print(saudacao); // Output: Olá, visitante!
}
```

### Operador de Atribuição caso Nulo `??=`

O operador de atribuição caso nulo `??=` atribui um valor à variável somente se a variável for `null`.


```dart
void main() {
  int? numero1;
  int numero2 = 10;
  numero1 ??= 5;
  numero2 ??= 5;
  print(numero1); // Output: 5
  print(numero2); // Output: 10
}
```

### Operador de Acesso caso Nulo `?.`

O operador de acesso caso nulo `?.` permite chamar um método ou acessar uma propriedade de um objeto somente se o objeto não for `null`.

```dart
void main() {
  String? texto;
  print(texto?.length); // Output: null
}
```

### Operador de Indexação caso Nulo `?[]`

O operador de indexação caso nulo `?[]` permite acessar um elemento de uma lista ou map somente se a lista ou o map não forem `null`.

```dart
void main() {
  List<int>? numeros;
  print(numeros?[0]); // Output: null
}
```

### Operador Cascade caso Nulo `?..`

O operador cascade caso nulo `?..` é uma combinação do operador de cascade `..` e o operador de acesso caso nulo `?.`. Ele permite encadear chamadas de métodos ou atribuições de propriedades somente se o objeto não for null.

```dart
class Pessoa {
  String? nome;
  int? idade;
  
  void saudacao() {
    print('Olá, meu nome é $nome e tenho $idade anos.');
  }
}

void main() {
  Pessoa? pessoa;
  pessoa
    ?..nome = 'João'
    ..idade = 30
    ..saudacao(); // Não faz nada, pois pessoa é null
  
  print(pessoa); // Output: null
}
```

### Operador Spread caso Nulo `...?`

O operador null-aware spread `...?` permite que você expanda elementos de uma coleção que pode ser `null`, evitando exceções.

```dart
void main() {
  var lista1 = [1, 2, 3];
  List<int>? lista2;
  
  var combinada = [...lista1, ...?lista2];
  
  print(combinada); // Output: [1, 2, 3]
}
```

Neste exemplo, `lista2` é `null`, mas o operador `...?` impede que isso cause uma exceção, resultando em `combinada` contendo apenas os elementos de `lista1`.

### Operador de Asserção Não Nulo `!`

O operador de asserção não nulo `!` é usado para afirmar que um valor não é `null`. Ele lança uma exceção se o valor for `null`. Este operador é útil quando se tem certeza de que um valor não será `null` em um determinado ponto do código.

```dart
void main() {
  String? texto;
  // print(texto!); // Lança uma exceção se texto for null
  texto = 'Olá';
  print(texto!); // Output: Olá
}
```

## Precedência de Operadores

Os operadores em Dart seguem uma convenção de prioridade na execução que influencia diretamente o resultado das expressões nas quais são utilizados, conforme a tabela a seguir:

| **Precedência** | **Operadores**                                                      | **Associatividade**    |
|-----------------|---------------------------------------------------------------------|------------------------|
| 1               | `[]`, `()`, `?.`, `?.[]`, `.`, `?.`, `++`, `--`, `()`, `[]`         | Esquerda para direita  |
| 2               | `-`, `!`, `~`, `++`, `--`, `await`, `!.`                            | Direita para esquerda  |
| 3               | `*`, `/`, `~/`, `%`                                                 | Esquerda para direita  |
| 4               | `+`, `-`                                                            | Esquerda para direita  |
| 5               | `<<`, `>>`, `>>>`                                                   | Esquerda para direita  |
| 6               | `&`                                                                 | Esquerda para direita  |
| 7               | `^`                                                                 | Esquerda para direita  |
| 8               | `|`                                                                 | Esquerda para direita  |
| 9               | `==`, `!=`, `===`, `!==`, `>=`, `>`, `<=`, `<`, `is`, `is!`,`as`    | Esquerda para direita  |
| 10              | `&&`                                                                | Esquerda para direita  |
| 11              | `||`                                                                | Esquerda para direita  |
| 12              | `??`                                                                | Esquerda para direita  |
| 13              | `? :`                                                               | Direita para esquerda  |
| 14              | `=`, `*=`, `/=`, `+=`, `-=`, `&=`, `^=`, `|=`, `<<=`, `>>=`, `??=`  | Direita para esquerda  |
| 15              | `...`                                                               | Não associativo        |
| 16              | `...?, !..`                                                         | Não associativo        |

A ordem de precedência e a associatividade dos operadores determinam como as expressões são avaliadas. Operadores com uma precedência mais alta são avaliados antes de operadores com uma precedência mais baixa. Se os operadores avaliados tiverem o mesmo nível de precedência, elese rerão avaliados segundo a sua associatividade. Onde:

- **Esquerda para direita**: Operadores são avaliados da esquerda para a direita.
- **Direita para esquerda**: Operadores são avaliados da direita para a esquerda.
- **Não associativo**: Não há associatividade; operadores de mesmo nível de precedência não podem ser usados adjacentes.

Considerando-se, por exemplo, a seguinte expressão:

```dart
void main() {
  print(1 + 6 / 2 * 3 - 6); // Output: 4
}
```

Os operadores `/` e `*` são mais prioritários que `+` e `-`, logo devem executar primeiro. Como a associatividade deles é da esquerda para direita, primeiro é executada a divisão: `1 + 3 * 3 - 6`. Depois a multiplicação: `1 + 9 - 6`. E, por fim, a adição e subtração, resultando em `4`.

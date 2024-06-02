<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Tipos de Dados

- [Introdução](#introdução)
- [Numbers (Números)](#numbers-números)
- [String](#string)
- [Booleans (Booleanos)](#booleans-booleanos)
- [List (Listas)](#list-listas)
- [Map](#map)
- [Set](#set)
- [Null](#null)

## Introdução

Dart, assim como a maioria das linguagens de programação, possui suporte a diversos tipos predefinidos, sendo uma linguagem com tipagem estática. Porém, não há um conceito ou definição clara sobre tipos primitivos, como na linguagem Java, por exemplo. Em Dart, todos os tipos são também objetos. Alguns tipos são mais básicos e outros, mais complexos.

## Tipos de Dados mais Comuns

Estes tipos de dados são mais básicos e mais utilizados.

### Números

O tipo `num` é a superclasse para `int` e `double`, representando números inteiros e de ponto flutuante.

```dart
void main() {
  num numero = 5; // Pode ser int ou double
  print(numero); // Output: 5
  numero = 5.5;
  print(numero); // Output: 5.5
}
```

### Números Inteiros

O tipo `int` representa números inteiros.

```dart
void main() {
  int idade = 25;
}
```

### Números de Ponto Flutuante

O tipo `double` representa números de ponto flutuante (números com casas decimais).

```dart
void main() {
  double preco = 19.99;
  double altura = 1.80;
}
```

### String

O tipo `String` representa uma sequência de caracteres Unicode. Strings são delimitadas por aspas simples (`'`) ou aspas duplas (`"`). No caso de textos multilinha, se utiliza três aspas simples.

```dart
void main() {
  String nome = 'João';
  String saudacao = "Olá, mundo!";
  String textoMultilinha = '''Este é um
  texto de múltiplas
  linhas.''';
}
```

### Runes

Runes representam o código de ponto Unicode de uma string. Elas permitem que se trabalhe com caracteres Unicode que não podem ser representados com um único caractere UTF-16.

```dart
void main() {
  String texto = 'Olá, 🌍';
  Runes runas = texto.runes;
  print(runas); // Output: (79, 108, 225, 44, 32, 127757) -> Iterável com os códigos para cada caracter da String
}
```

### Booleanos

O tipo `bool` representa valores booleanos: `true` ou `false`.

```dart
bool estaChovendo = false;
bool estaAberto = true;
```

### Iterable (Iteráveis)

O tipo `Iterable` representa uma coleção de elementos que podem ser iterados, é a superclasse para os tipos como `List`, `Set`, etc.

```dart
void main() {
  Iterable<int> numeros = [1, 2, 3];
  for (var numero in numeros) {
    print(numero); // Output: 1 2 3
  }
}
```

### List (Listas)

Uma `List` é uma coleção ordenada de objetos. Pode ser homogênea (todos os elementos do mesmo tipo) ou heterogênea.

```dart
List<int> numeros = [1, 2, 3, 4, 5];
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<dynamic> misturado = [1, 'dois', 3.0, true];
```

### Set

Um `Set` é uma coleção de elementos únicos, sem ordem específica, muito semelhante a uma lista, exceto que a lista aceita repetição de elementos.

```dart
Set<int> numerosUnicos = {1, 2, 3, 4, 5};
Set<String> cores = {'vermelho', 'verde', 'azul'};
```

### Map

Um `Map` é uma coleção de pares chave-valor. As chaves e os valores podem ser de qualquer tipo.

```dart
Map<String, int> telefones = {
  'João': 123456789,
  'Maria': 987654321
};

Map<int, String> produtos = {
  1: 'Laptop',
  2: 'Mouse',
  3: 'Teclado'
};
```

### Symbol (Símbolos)

O tipo `Symbol` é usado para representar um símbolo, que é uma string referenciada por seu identificador no tempo de compilação.

```dart
void main() {
  Symbol simbolo = #minhaVariavel;
  print(simbolo); // Output: Symbol("minhaVariavel")
}
```

### Null

Dart tem suporte para o tipo `null`, que é usado para representar a ausência de um valor. Variáveis não podem ser declaradas como nulas, a menos que sejam explicitamente marcadas como nulas.

```dart
String? nomeNulo = null;
int? numeroNulo = null;
```

## Tipos de Dados mais Complexos

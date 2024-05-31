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

Dart é uma linguagem fortemente tipada que oferece uma ampla variedade de tipos de dados. Aqui estão os principais tipos de dados em Dart, junto com exemplos para cada um:

## Numbers (Números)

Dart possui dois tipos de números: `int` (representa números inteiros) e `double` (representa números decimais).

```dart
int idade = 30;
int ano = 2024;

double altura = 1.75;
double preco = 99.99;
```

## String

Uma `String` é usado para representar textos.

```dart
String nome = 'João';
String saudacao = "Olá, mundo!";
String textoMultilinha = '''Este é um
texto de múltiplas
linhas.''';
```

## Booleans (Booleanos)

O tipo `bool` representa valores booleanos: `true` ou `false`.

```dart
bool estaChovendo = false;
bool estaAberto = true;
```

## List (Listas)

Uma `List` é uma coleção ordenada de objetos. Pode ser homogênea (todos os elementos do mesmo tipo) ou heterogênea.

```dart
List<int> numeros = [1, 2, 3, 4, 5];
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<dynamic> misturado = [1, 'dois', 3.0, true];
```

## Map

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

## Set

Um `Set` é uma coleção de elementos únicos, sem ordem específica.

```dart
Set<int> numerosUnicos = {1, 2, 3, 4, 5};
Set<String> cores = {'vermelho', 'verde', 'azul'};
```

## Null

Dart tem suporte para o tipo `null`, que é usado para representar a ausência de um valor. Variáveis não podem ser declaradas como nulas, a menos que sejam explicitamente marcadas como nulas.

```dart
String? nomeNulo = null;
int? numeroNulo = null;
```
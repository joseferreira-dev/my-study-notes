<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner-dart.png"></a>
</div>
<br>

# Conversão entre Tipos

- [Convertendo `String` para `int` e `double`](#convertendo-string-para-int-e-double)
- [Convertendo `int` e `double` para `String`](#convertendo-int-e-double-para-string)
- [Convertendo `List` para `Set` e `Map`](#convertendo-list-para-set-e-map)
- [Convertendo `Set` e `Map` para `List`](#convertendo-set-e-map-para-list)
- [Convertendo `Set`, `Map` e `List` para `String`](#convertendo-set-map-e-list-para-string)

No Dart, é comum precisar converter entre diferentes tipos de dados, como entre inteiros, strings e números de ponto flutuante. Dart oferece várias maneiras de realizar essas conversões.

## Convertendo `String` para `int` e `double`

Para converter uma `String` em um `int` ou em um `double`, usamos os métodos `int.parse` e `double.parse`, respectivamente.

```dart
void main() {
  String numeroStr = '123';
  int numeroInt = int.parse(numeroStr);
  print(numeroInt); // Output: 123

  String numeroStr = '123.45';
  double numeroDouble = double.parse(numeroStr);
  print(numeroDouble); // Output: 123.45
}
```

Todavia, os métodos acima não são tão recomendados, uma vez que sempre lançam uma exceção caso não seja possível converter a `String`. Para evitar isso podemos utilizar os métodos `int.tryParse` e `double.tryParse`, ambos retornam o valor `null` caso a conversão não possa ser realizada.

```dart
void main() {
  String numeroStr = '123a';
  int? numeroInt = int.tryParse(numeroStr);
  print(numeroInt); // Output: null

  String numeroStr = '123.45a';
  double? numeroDouble = double.tryParse(numeroStr);
  print(numeroDouble); // Output: null
}
```

## Convertendo `int` e `double` para `String`

Para convertermos um `int` ou um `double` para `String`, utilizamos o método `toString`.

```dart
void main() {
  int numeroInt = 123;
  String numeroStr = numeroInt.toString();
  print(numeroStr); // Output: 123

  double numeroDouble = 123.45;
  String numeroStr = numeroDouble.toString();
  print(numeroStr); // Output: 123.45
}
```

## Convertendo `List` para `Set` e `Map`

Podemos converter uma lista em um conjunto com o método `toSet`. Também podemos criar um `Set` a partir de uma `List` utilizando o construtor `Set.from`. Em abos os casos, qualquer elemento repetido da lista original é eliminado.

```dart
void main() {
  List<int> listaNumeros = [1, 2, 3, 4, 4, 5];
  Set<int> conjuntoNumeros = listaNumeros.toSet();
  print(conjuntoNumeros); // Output: {1, 2, 3, 4, 5}
  
  List<String> listaLetras = ['a', 'b', 'b', 'c'];
  Set<String> conjuntoLetras = Set.from(listaLetras);
  print(conjuntoLetras); // Output: {a, b, c}
}
```

Já para convertemos em um `Map` utilizamos o método `asMap`, que retorna um mapa onde as chaves são os índices da lista.

```dart
void main() {
  List<String> lista = ['a', 'b', 'c'];
  Map<int, String> mapa = lista.asMap();
  print(mapa); // Output: {0: a, 1: b, 2: c}
}
```

Também é possível se criar um mapa a partir de uma lista de entradas de mapa (`MapEntry`).

```dart
void main() {
  List<MapEntry<String, int>> lista = [MapEntry('one', 1), MapEntry('two', 2)];
  Map<String, int> mapa = Map.fromEntries(lista);
  print(mapa); // Output: {one: 1, two: 2}
}
```

## Convertendo `Set` e `Map` para `List`

Para convertermos um `Set` em uma `List` podemos utilizar o método `toList`.

```dart
void main() {
  Set<int> conjunto = {1, 2, 3, 4, 5};
  List<int> lista = conjunto.toList();
  print(lista); // Output: [1, 2, 3, 4, 5]
}
```

O mesmo pode ser feito no caso de um `Map`, mas devemos fazer a conversão individual das suas chaves e dos seus valores a partir das propriedades `keys` e `values`.

```dart
void main() {
  Map<int, String> mapa = {0: 'a', 1: 'b', 2: 'c'};
  List<int> chaves = mapa.keys.toList();
  List<String> valores = mapa.values.toList();
  print(chaves); // Output: [0, 1, 2]
  print(valores); // Output: [a, b, c]
}
```

Também é possível se criar uma lista de entradas de mapa a partir da propriedade `entries`.

```dart
void main() {
  Map<String, int> mapa = {'one': 1, 'two': 2};
  List<MapEntry<String, int>> lista = mapa.entries.toList();
  print(lista); // Output: [MapEntry(one: 1), MapEntry(two: 2)]
}
```

## Convertendo `Set`, `Map` e `List` para `String`

Os tipos `Set`, `Map` e `List` também possuem o método `toString` para realizar a sua conversão em uma `String`.

```dart
void main() {
  List<String> lista = ['a', 'b', 'c'];
  Set<int> conjunto = {1, 2, 3, 4, 5};
  Map<int, String> mapa = {0: 'a', 1: 'b', 2: 'c'};
  
  String stringLista = lista.toString();
  String stringConjunto = conjunto.toString();
  String stringMapa = mapa.toString();
  
  print(stringLista);    // Output: [1, 2, 3, 4, 5]
  print(stringConjunto); // Output: {1, 2, 3, 4, 5}
  print(stringMapa);     // Output: {0: a, 1: b, 2: c}
}
```

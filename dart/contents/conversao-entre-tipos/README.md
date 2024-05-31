<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Conversão entre Tipos

- [Introdução](#introdução)
- [Convertendo `String` para `int` e `double`](#convertendo-string-para-int-e-double)
- [Convertendo `int` e `double` para `String`](#convertendo-int-e-double-para-string)
- [Convertendo `List` para `Set` e `Map`](#convertendo-list-para-set-e-map)
- [Convertendo `Set` e `Map` para `List`](#convertendo-set-e-map-para-list)

## Introdução

No Dart, é comum precisar converter entre diferentes tipos de dados, como entre inteiros, strings e números de ponto flutuante. Dart oferece várias maneiras de realizar essas conversões de forma segura e eficiente.

## Convertendo `String` para `int` e `double`

Para converter uma `String` em um `int`, pode-se usar o método `int.parse`. Para converter para `double` utiliza-se o método `double.parse`.

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

Os métodos acima sempre lançam uma exceção caso não seja possível converter a `String`. Para evitar isso existem os métodos `int.tryParse` e `double.tryParse`, ambos retornam o valor `null` caso a conversão não possa ser realizada.

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

Para converter um `int` ou um `double` para `String`, basta utilizar o método `toString`.

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

Para converter uma lista (`List`) em um `Set` pode-se utilizar o construtor `Set`. Neste caso, qualquer elemento repetido é eliminado.

```dart
void main() {
  List<int> lista = [1, 2, 3, 4, 4, 5];
  Set<int> conjunto = Set.from(lista);
  print(conjunto); // Output: {1, 2, 3, 4, 5}
}
```

Para converter em um `Map` utiliza-se o método `asMap`, que retorna um map onde as chaves são os índices da lista.

```dart
void main() {
  List<String> lista = ['a', 'b', 'c'];
  Map<int, String> mapa = lista.asMap();
  print(mapa); // Output: {0: a, 1: b, 2: c}
}
```

## Convertendo `Set` e `Map` para `List`

Para converter um `Set` em uma `List` pode-se utilizar o método `toList`.

```dart
void main() {
  Set<int> conjunto = {1, 2, 3, 4, 5};
  List<int> lista = conjunto.toList();
  print(lista); // Output: [1, 2, 3, 4, 5]
}
```

O mesmo pode ser feito no caso de um `Map`, mas deve-se fazer a conversão individual das suas chaves e dos seus valores a partir das propriedades `keys` e `values`.

```dart
void main() {
  Map<int, String> mapa = {0: 'a', 1: 'b', 2: 'c'};
  List<int> chaves = mapa.keys.toList();
  List<String> valores = mapa.values.toList();
  print(chaves); // Output: [0, 1, 2]
  print(valores); // Output: [a, b, c]
}
```
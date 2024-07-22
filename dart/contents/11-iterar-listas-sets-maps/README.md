<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner_dart.png"></a>
</div>
<br>

# Iterar sobre Lists, Sets e Maps

- [Iterar sobre Listas](#iterar-sobre-listas)
  * [Usando `for`, `for-in` e `forEach`](#usando-for-for-in-e-foreach)
  * [Usando `map`](#usando-map)
  * [Usando `where`](#usando-where)
  * [Usando `expand`](#usando-expand)
  * [Usando `reduce` e `fold`](#usando-reduce-e-fold)
  * [Usando `every` e `any`](#usando-every-e-any)
  * [Usando `take` e `skip`](#usando-take-e-skip)
  * [Usando `takeWhile` e `skipWhile`](#usando-takewhile-e-skipwhile)
- [Iterar sobre Conjuntos](#iterar-sobre-conjuntos)
  * [Usando `for`, `for-in` e `forEach`](#usando-for-for-in-e-foreach-1)
  * [Usando `map`, `where`, `reduce`, `fold` e `expand`](#usando-map-where-reduce-fold-e-expand)
  * [Usando `every` e `any`](#usando-every-e-any-1)
  * [Usando `take`, `skip`, `takeWhile` e `skipWhile`](#usando-take-skip-takewhile-e-skipwhile)
- [Iterar sobre Maps](#iterar-sobre-maps)

Em Dart, a iteração é o ato de percorrer elementos de coleções como listas, conjuntos e mapas. Existem formas comuns de percorrer todas as coleções, além de formas específicas para cada coleção e formas mais indicadas.

## Iterar sobre Listas

Em Dart, podemos iterar sobre listas de várias maneiras, utilizando diferentes métodos e técnicas de iteração. Devemos ter atenção ao utilizar alguns desses métodos, pois muitos deles retornam um iterable, que é diferente de uma lista, sendo necessário sua conversão de volta para lista com o método `toList`.

### Usando `for`, `for-in` e `forEach`

Os loops `for`, `for-in` e o método `forEach` são usados para iterar sobre listas ou outras coleções, mas têm diferenças importantes em termos de sintaxe, funcionalidades e uso.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];

  for (int i = 0; i < frutas.length; i++) {
    print(frutas[i]);
  }
  
  for (var fruta in frutas) {
    print(fruta);
  }
  
  frutas.forEach((fruta) {
    print(fruta);
  });
}
```

O loop `for` é muito flexível e permite um controle detalhado sobre o índice da iteração. É útil quando você precisa acessar o índice dos elementos ou manipular o índice durante a iteração. Já o loop `for-in` é mais simples e mais legível quando você só precisa acessar os elementos sem se preocupar com os índices. E o método `forEach` é uma função de ordem superior que aplica uma função callback a cada elemento da lista.

As principais diferenças entre essas três formas são:

- O loop `for` é o único que permite acesso direto e manipulação dos índices da lista;
- O método `forEach` não suporta o uso de `break` e `continue`, o que pode ser feito nos loops `for` e `for-in`;
- O método `for` é o único que permite leitura e modificação dos elementos da lista, enquanto que `for-in` e `forEach` não permitem modificação.

### Usando `map`

O método `map` aplica uma função a cada elemento e retorna uma nova lista com os resultados.

```dart
void main() {
  List<int> numeros = [1, 2, 3];
  List<int> quadrados = numeros.map((numero) => numero * numero).toList();
  print(quadrados); // Output: [1, 4, 9]
}
```

### Usando `where`

O método `where` filtra elementos com base em uma condição e retorna uma nova lista com os elementos que satisfazem a condição.

```dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  List<int> pares = numeros.where((numero) => numero % 2 == 0).toList();
  print(pares); // Output: [2, 4]
}
```

### Usando `expand`

O método `expand` aplica uma função a cada elemento e retorna uma única lista contendo os resultados concatenados.

```dart
void main() {
  List<List<int>> listas = [
    [1, 2, 3],
    [4, 5],
    [6, 7, 8]
  ];
  List<int> todosOsNumerosAoQuadrado = listas
      .expand((lista) => lista.map((n) => n * n).toList())
      .toList();
  print(todosOsNumeros); // Output: [1, 4, 9, 16, 25, 36, 49, 64]
}
```

### Usando `reduce` e `fold`

O método `reduce` combina todos os elementos da lista em um único valor, aplicando uma função binária.

```dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  int soma = numeros.reduce((a, b) => a + b);
  print(soma); // Output: 15
}
```

O método `fold` é semelhante ao `reduce`, mas permite especificar um valor inicial.

```dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  int soma = numeros.fold(10, (anterior, atual) => anterior + atual);
  print(soma); // Output: 25
}
```

### Usando `every` e `any`

O método `every` verifica se todos os elementos satisfazem uma condição, enquanto que `any` verifica se algum dos elementos satisfaz uma condição.

```dart
void main() {
  List<int> numeros = [2, 4, 6, 8, 9];
  
  bool todosPares = numeros.every((numero) => numero % 2 == 0);
  print(todosPares); // Output: false
  
  bool algumPar = numeros.any((numero) => numero % 2 == 0);
  print(algumPar); // Output: true
}
```

### Usando `take` e `skip`

O método `take` pega os primeiros n elementos da lista e `skip` pula os primeiros n elementos da lista e retornar o restante.

```dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5, 6];
  
  var primeirosTres = numeros.take(3).toList();
  print(primeirosTres); // Output: [1, 2, 3]
  
  var depoisDeTres = numeros.skip(3).toList();
  print(depoisDeTres); // Output: [4, 5, 6]
}
```

### Usando `takeWhile` e `skipWhile`

O método `takeWhile` pega os elementos enquanto a condição for verdadeira e `skipWhile` pula os elementos enquanto a condição for verdadeira e retornar o restante.


```dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5, 6];
  
  var menoresQueQuatro = numeros.takeWhile((numero) => numero < 4).toList();
  print(menoresQueQuatro); // Output: [1, 2, 3]
  
  var aPartirDeQuatro = numeros.skipWhile((numero) => numero < 4).toList();
  print(aPartirDeQuatro); // Output: [4, 5, 6]
}
```

## Iterar sobre Conjuntos

Em Dart, podemos iterar sobre um `Set` da mesma maneira que em uma lista, atentando-se ao fato de que alguns métodos retornam um iterable, sendo necessário sua conversão de volta para um `Set` com o método `toSet`.

### Usando `for`, `for-in` e `forEach`

Os loops `for`, `for-in` e o método `forEach` funcionam da mesma forma que nas listas.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};

  for (int i = 0; i < frutas.length; i++) {
    print(frutas.elementAt(i));
  }
  
  for (var fruta in frutas) {
    print(fruta);
  }
  
  frutas.forEach((fruta) {
    print(fruta);
  });
}
```

### Usando `map`, `where`, `reduce`, `fold` e `expand`

Os métodos `map`, `where`, `reduce`, `fold` e `expand` funcionam da mesma forma que e nas listas, ressaltando-se que todos os elementos repetidos são eliminados do `Set` final.

```dart
void main() {
  Set<int> numeros = {1, 2, 3, 4, 5};
  
  Set<int> quadrados = numeros.map((numero) => numero * numero).toSet();
  print(quadrados); // Output: {1, 4, 9, 16, 25}
  
  Set<int> pares = numeros.where((numero) => numero % 2 == 0).toSet();
  print(pares); // Output: {2, 4}
  
  int soma = numeros.reduce((a, b) => a + b);
  print(soma);// Output: 15
  
  soma = numeros.fold(10, (anterior, atual) => anterior + atual);
  print(soma);// Output: 25
  
  Set<int> outrosNumeros = {3, 4, 5, 6, 7};
  
  Set<Set<int>> todosOsNumeros = {
    numeros,
    outrosNumeros,
  };
  
  Set<int> todosOsNumerosAoQuadrado = todosOsNumeros
      .expand((lista) => lista.map((n) => n * n).toSet())
      .toSet();
  print(todosOsNumerosAoQuadrado); // Output: {1, 4, 9, 16, 25, 36, 49}
}
```

### Usando `every` e `any`

Ambos os métodos `every` e `any` funcionam da mesma forma que em listas.

```dart
void main() {
  Set<int> numeros = {2, 4, 6, 8, 9};
  
  bool todosPares = numeros.every((numero) => numero % 2 == 0);
  print(todosPares); // Output: false
  
  bool algumPar = numeros.any((numero) => numero % 2 == 0);
  print(algumPar); // Output: true
}
```

### Usando `take`, `skip`, `takeWhile` e `skipWhile`

Os métodos `take`, `skip`, `takeWhile` e `skipWhile` também funcionam da mesma forma que nas listas.

```dart
void main() {
  Set<int> numeros = {1, 2, 3, 4, 5, 6};
  
  var primeirosTres = numeros.take(3).toSet();
  print(primeirosTres); // Output: {1, 2, 3}
  
  var depoisDeTres = numeros.skip(3).toSet();
  print(depoisDeTres); // Output: {4, 5, 6}
  
  var menoresQueQuatro = numeros.takeWhile((numero) => numero < 4).toSet();
  print(menoresQueQuatro); // Output: {1, 2, 3}
  
  var aPartirDeQuatro = numeros.skipWhile((numero) => numero < 4).toSet();
  print(aPartirDeQuatro); // Output: {4, 5, 6}
}
```

## Iterar sobre Mapas

Em Dart, podemos iterar sobre um `Map` de maneiras muito semelhantes as demais coleções.

### Usando `for`, `for-in` e `forEach`

Os loops `for`, `for-in` e o método `forEach` podem ser usados para iterar sobre os pares chave-valor de um mapa.

```dart
void main() {
  Map<String, int> frutas = {'maçã': 1, 'banana': 2, 'laranja': 3};

  for (int i = 0; i < frutas.length; i++) {
    String chave = frutas.keys.elementAt(i);
    print('Chave: $chave, Valor: ${frutas[chave]}');
  }
  
  for (var chave in frutas.keys) {
    print('Chave: $chave, Valor: ${frutas[chave]}');
  }
  
  frutas.forEach((chave, valor) {
    print('Chave: $chave, Valor: $valor');
  });
}
```

### Usando `map`, `where`, `reduce`, `fold` e `expand`

Os métodos `map`, `where`, `reduce`, `fold` e `expand` também podem ser usados em mapas, porém de forma ligeiramente diferente.

```dart
void main() {
  Map<String, int> frutas = {'maçã': 1, 'banana': 2, 'laranja': 3};
  
  var frutasDobradas = frutas.map((chave, valor) => MapEntry(chave, valor * 2));
  print(frutasDobradas); // Output: {maçã: 2, banana: 4, laranja: 6}
  
  var frutasFiltradas = Map.fromEntries(frutas.entries.where((entry) => entry.value % 2 == 0));
  print(frutasFiltradas); // Output: {banana: 2}
  
  int soma = frutas.values.reduce((a, b) => a + b);
  print(soma); // Output: 6
  
  int somaComInicial = frutas.values.fold(10, (anterior, atual) => anterior + atual);
  print(somaComInicial); // Output: 16
  
  var chavesDuplicadas = frutas.keys.expand((chave) => [chave, chave]).toList();
  print(chavesDuplicadas); // Output: [maçã, maçã, banana, banana, laranja, laranja]
}
```

### Usando `every` e `any`

Os métodos `every` e `any` verificam condições sobre os elementos de um mapa.

```dart
void main() {
  Map<String, int> frutas = {'maçã': 1, 'banana': 2, 'laranja': 3};
  
  bool todosPositivos = frutas.values.every((valor) => valor > 0);
  print(todosPositivos); // Output: true
  
  bool algumPar = frutas.values.any((valor) => valor % 2 == 0);
  print(algumPar); // Output: true
}
```

### Usando `take`, `skip`, `takeWhile` e `skipWhile`

Os métodos `take`, `skip`, `takeWhile` e `skipWhile` podem ser usados para selecionar pares chave-valor de um mapa.

```dart
void main() {
  Map<String, int> frutas = {'maçã': 1, 'banana': 2, 'laranja': 3};
  
  var primeirosDois = Map.fromEntries(frutas.entries.take(2));
  print(primeirosDois); // Output: {maçã: 1, banana: 2}
  
  var depoisDosDois = Map.fromEntries(frutas.entries.skip(2));
  print(depoisDosDois); // Output: {laranja: 3}
  
  var enquantoMenorQueTres = Map.fromEntries(frutas.entries.takeWhile((entry) => entry.value < 3));
  print(enquantoMenorQueTres); // Output: {maçã: 1, banana: 2}
  
  var aPartirDeTres = Map.fromEntries(frutas.entries.skipWhile((entry) => entry.value < 3));
  print(aPartirDeTres); // Output: {laranja: 3}
}
```

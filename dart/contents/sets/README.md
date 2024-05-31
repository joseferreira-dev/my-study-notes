<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Sets

- [Introdução](#introdução)
- [Criar um `Set`](#criar-um-set)
- [Acessar Elementos](#acessar-elementos)
- [Modificar Elementos](#modificar-elementos)
- [Adicionar Elementos](#adicionar-elementos)
- [Remover Elementos](#remover-elementos)
- [Consultas](#consultas)
- [Tamanho de um `Set`](#tamanho-de-um-set)
- [Operações](#operações)

## Introdução

Em Dart, um `Set` é uma coleção não ordenada de elementos únicos. Diferente de uma `List`, um `Set` não permite elementos duplicados. Essa estrutura de dados é útil quando se precisa garantir que não haverá elementos repetidos.

## Criar um `Set`

Pode-se criar um `Set` usando o construtor padrão ou usando a notação literal com chaves `{}`.

```dart
void main() {
  // Usando o construtor padrão
  Set<int> numeros = Set();
  numeros.add(1);
  numeros.add(2);
  numeros.add(3);
  numeros.add(1);
  print(numeros); // Output: {1, 2, 3}

  // Usando a notação literal
  Set<String> frutas = {'maçã', 'banana', 'laranja', 'maçã'};
  print(frutas); // Output: {maçã, banana, laranja}
}
```

## Acessar Elementos

Para um `Set` o método `elementAt` e as propriedades `first` e `last` funcionam da mesma forma que em uma lista.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  print(frutas.elementAt(1)); // Output: banana
  print(frutas.first); // Output: maçã
  print(frutas.last); // Output: laranja
}
```

## Modificar Elementos

Modificar elementos em um `Set` diretamente não é possível porque em Dart eles são coleções de elementos únicos e desordenados. No entanto, pode-se realizar modificações de maneira indireta, removendo um elemento existente e adicionando um novo elemento modificado. Por exemplo:

```dart
void main() {
  Set<int> numeros = {1, 2, 3, 4, 5};

  print("Set: $numeros"); // Output: Set: {1, 2, 3, 4, 5}

  if (numeros.contains(3)) {
    numeros.remove(3);
    numeros.add(30);
  }

  print("Modified Set: $numeros"); // Output: Set: {1, 2, 4, 5, 30}
}
```

## Adicionar Elementos

Para adicionar elementos a um `Set` utiliza-se os métodos `add` e `addAll`, que funcionam da mesma maneira que com listas.

```dart
void main() {
  Set<int> numeros = {1, 2, 3};
  numeros.add(4);
  print(numeros); // Output: {1, 2, 3, 4}
  numeros.addAll({5, 6});
  print(numeros); // Output: {1, 2, 3, 4, 5, 6}
}
```

## Remover Elementos

Os métodos `remove`, `removeWhere` e `clear` também existem no tipo `Set` e funcionam da mesma forma que nas listas.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  frutas.remove('banana');
  print(frutas); // Output: {maçã, laranja}
  frutas.removeWhere((fruta) => fruta == 'laranja');
  print(frutas); // Output: {maçã}
  frutas.clear();
  print(frutas); // Output: {}
}
```

Existe ainda o método `removeAll` que remove um conjunto de elementos.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja', 'uva', 'mamão'};
  frutas.removeAll(['maçã', 'laranja']);
  print(frutas); // Output: {banana, uva, mamão}
  frutas.removeAll({'uva', 'mamão'});
  print(frutas); // Output: {banana}
}
```

## Consultas

O método `contains` também existe nos Sets e funciona da mesma forma que nas listas, assim como as propriedades `isEmpty` e `isNotEmpty`. Existe ainda o método `containsAll` que verifica se o `Set` possui um conjunto de elementos.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  
  print(frutas.isEmpty); // Output: false
  print(frutas.isNotEmpty); // Output: true

  print(frutas.contains('maçã')); // Output: true
  print(frutas.containsAll(['banana', 'uva'])); // Output: false
}
```

## Tamanho de um `Set`

O `Set` também possui a propriedade `length` que retorna seu tamanho.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  print(frutas.length); // Output: 3
}
```

## Operações

Existem quatro operações principais que se pode realizar com Sets. São elas `intersection` (retorna a intersecção de dois Sets), `union` (retorna a união de dois Sets), `difference` (retorna os elementos que existem no `Set` atual mas não no que é enviado de parâmetro) e `lookup` (retorna um elemento do `Set` que é igual ao fornecido ou `null`, se ele não existir).

```dart
void main() {
  Set<int> numeros1 = {1, 2, 3, 4, 5};
  Set<int> numeros2 = {4, 5, 6, 7, 8};
  print(numeros1.intersection(numeros2)); // Output: {4, 5}
  print(numeros1.union(numeros2)); // Output: {1, 2, 3, 4, 5, 6, 7, 8}
  print(numeros1.difference(numeros2)); // Output: {1, 2, 3}
  print(numeros1.lookup(9)); // Output: null
}
```
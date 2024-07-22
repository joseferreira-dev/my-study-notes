<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner_dart.png"></a>
</div>
<br>

# Lists (Listas)

- [Criar uma Lista](#criar-uma-lista)
- [Gerar uma Lista](#gerar-uma-lista)
- [Acessar Elementos](#acessar-elementos)
- [Modificar Elementos](#modificar-elementos)
- [Adicionar e Inserir Elementos](#adicionar-e-inserir-elementos)
- [Remover Elementos](#remover-elementos)
- [Ordenar, Reverter e Embaralhar uma Lista](#ordenar-reverter-e-embaralhar-uma-lista)
- [Tamanho de uma Lista](#tamanho-de-uma-lista)
- [Consultas](#consultas)
- [Criar uma String a partir de uma Lista](#criar-uma-string-a-partir-de-uma-lista)
- [Criar uma Lista a partir de uma String](#criar-uma-lista-a-partir-de-uma-string)

Em Dart, listas são uma coleção ordenada de elementos que podem ser acessados por meio de índices, semelhante ao tipo array de outras linguagens. Listas são muito utilizadas para armazenar sequências de valores de um mesmo tipo.

## Criar uma Lista

Podemos criar uma lista vazia e adicionar elementos posteriormente.

```dart
void main() {
  List<int> numeros = [];
  print(numeros); // Output: []
}
```

Uma lista também pode ser criada com elementos iniciais.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas); // Output: [maçã, banana, laranja]
}
```

Uma lista também pode ser criada sem definir o seu tipo com o operador `var`, o que permite que o tipo seja inferido.

```dart
void main() {
  var numeros = [1, 2, 3, 4, 5];
  print(numeros); // Output: [1, 2, 3, 4, 5]
}
```

## Gerar uma Lista

É possível gerar uma lista de elementos conforme uma instrução específica com o método `List.generate`, preencher uma lista com `List.filled` e criar uma lista nova a partir de outra com os métodos `List.from` e `List.of`.

```dart
void main() {
  List<int> listaGerada = List.generate(10, (index) => index * 3);
  print(listaGerada); // Output: [0, 3, 6, 9, 12, 15, 18, 21, 24, 27]

  List<String> listaPreenchida = List.filled(5, 'A');
  print(listaPreenchida); // Output: [A, A, A, A, A]

  List<int> listaOrigem = [4, 5, 6];
  List<int> listaNova = List.from(listaOrigem);
  print(listaNova); // Output: [4, 5, 6]
  List<int> listaNova2 = List.of(listaOrigem);
  print(listaNova2); // Output: [4, 5, 6]
}
```

## Acessar Elementos

Os elementos da lista são acessados por meio de índices, começando do índice `0`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas[0]); // Output: maçã
  print(frutas[1]); // Output: banana
  print(frutas[2]); // Output: laranja
}
```

Isso também pode ser feito com o método `elementAt`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.elementAt(2)); // Output: laranja
}
```

O primeiro e o último elemento da lista podem ser acessados usando `first` e `last`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.first); // Output: maçã
  print(frutas.last); // Output: laranja
}
```

Também podemos acessar partes da lista (uma sublista) com o método `sublist`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'morango', 'uva'];
  List<String> sublista = frutas.sublist(1, 4);
  print(sublista); // Output: [banana, laranja, morango]
}
```

Também é possível se obter o índice de um elemento específico com os métodos `indexOf` e `lastIndexOf`, sendo que o primeiro retorna o índice da primeira ocorrência do elemento na lista e o segundo a última.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'banana'];
  print(frutas.indexOf('banana')); // Output: 1
  print(frutas.lastIndexOf('banana')); // Output: 3
}
```

## Modificar Elementos

Para modificar os elementos da lista basta atribuir novos valores aos índices correspondentes.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas[1] = 'manga';
  print(frutas); // Output: [maçã, manga, laranja]
}
```

Também podemos modificar os elementos com os métodos `setAll` (define todos os elementos a partir da posição especificada com os elementos do iterável dado como entrada), `fillRange` (preenche a faixa de índices especificada com o valor dado) e `replaceRange` (substitui a faixa de elementos especificada com os elementos do iterável dado).

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'mamão', 'uva'];

  frutas.setAll(1, ['manga', 'melancia']);
  print(frutas); // Output: [maçã, manga, melancia, mamão, uva]

  frutas.fillRange(2, 4, 'banana');
  print(frutas); // [maçã, manga, banana, banana, uva]

  frutas.replaceRange(0, 2, ['pêssego', 'goiaba']);
  print(frutas); // [pêssego, goiaba, banana, banana, uva]
}
```

## Adicionar e Inserir Elementos

O método `add` é utilizado para adicionar um único elemento à lista.

```dart
void main() {
  List<int> numeros = [1, 2, 3];
  numeros.add(4);
  print(numeros); // Output: [1, 2, 3, 4]
}
```

Já o método `addAll` adiciona múltiplos elementos à lista.

```dart
void main() {
  List<int> numeros = [1, 2, 3];
  numeros.addAll([4, 5, 6]);
  print(numeros); // Output: [1, 2, 3, 4, 5, 6]
}
```

Também é possível inserir novos elementos em uma posição específica com os métodos `insert` (um elemento) e `insertAll` (vários elementos).

```dart
void main() {
  List<int> numbers = [1, 2, 3];

  numbers.insert(1, 10);
  print(numbers); // Output: [1, 10, 2, 3]

  numbers.insertAll(2, [20, 30]);
  print(numbers); // Output: [1, 10, 20, 30, 2, 3]
}
```

Também é possível adicionar elementos de forma condicional utilizando outras estruturas, como `if-else`.

```dart
void main() {
  String fruta1 = 'maça';
  String? fruta2 = null;
  String fruta3 = 'laranja';
  
  List<String> frutas = [
    fruta1,
    if(fruta2 != null) 'banana' else 'mamão',
    fruta3
  ];
  
  print(frutas); // Output: [maça, mamão, laranja]
}
```

## Remover Elementos

O método `remove` é usado para remover um elemento específico da lista.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.remove('banana');
  print(frutas); // Output: [maçã, laranja]
}
```

Para remover um elemento em um índice específico pode-se utilizar o método `removeAt`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.removeAt(1);
  print(frutas); // Output: [maçã, laranja]
}
```

Para remover o último elemento da lista usa-se o método `removeLast`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.removeLast();
  print(frutas); // Output: [maçã, banana]
}
```

O método `removeWhere` remove elementos segundo uma condição específica.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'manga'];
  frutas.removeWhere((fruta) => fruta[0] == 'm');
  print(frutas); // Output: [banana, laranja]
}
```

Para remover todos os elementos, ou seja, limpar a lista, usa-se o método `clear`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.clear();
  print(frutas); // Output: []
}
```

## Ordenar, Reverter e Embaralhar uma Lista

Podemos ordenar uma lista com o método `sort` usando uma função de comparação especificada (ou a ordem natural, se nenhuma função for fornecida). Para reverter a ordem dos elementos usa-se o método `reversed`. Já o método `shuffle` embaralha os elementos.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'mamão', 'uva'];
  frutas.sort();
  print(frutas); // Output: [maçã, manga, melancia, mamão, uva]
  print(frutas.reversed); // Output: (maçã, manga, banana, banana, uva)
  print(frutas.reversed.toList()); // Output: [uva, maçã, mamão, laranja, banana]
  frutas.shuffle();
  print(frutas); // [pêssego, goiaba, banana, banana, uva]
}
```

Pode-se perceber que os métodos `sort` e `shuffle` alteram a lista original, enquanto que o método `reversed` somente retorna a lista invertida.

## Tamanho de uma Lista

Usamos a propriedade `length` para obter o número de elementos na lista.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.length); // Output: 3
}
```

## Consultas

É possível fazer diversas verificações acerca de uma lista. O método `contains` verifica se a lista possui um determinado elemento.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.contains('banana')); // Output: true
}
~~~

As propriedades `isEmpty` e `isNotEmpty` verificam se a lista está ou não vazia.

~~~dart
void main() {
  List<String> frutas = [];
  print(frutas.isEmpty); // Output: true
  print(frutas.isNotEmpty); // Output: false
  frutas.add('maça');
  print(frutas.isEmpty); // Output: false
  print(frutas.isNotEmpty); // Output: true
}
~~~

## Criar uma String a partir de uma Lista

O método `join` combina todos os elementos da lista em uma única string, separando-os por um delimitador especificado.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.join(', ')); // Output: maçã, banana, laranja
  print(frutas.join(' | ')); // Output: maçã | banana | laranja
  print(frutas.join(' -> ')); // Output: maçã -> banana -> laranja
}
```

## Criar uma Lista a partir de uma String

O método `split` separa todas as palavras de uma `String` para formar uma lista utilizando como base o separador especificado.

```dart
void main() {
  String texto = "Olá mundo esta é uma string";

  List<String> palavras = texto.split(' ');

  print(palavras); // Output: [Olá, mundo, esta, é, uma, string]
}
```

Também é possível associar este método com o método `trim` e outros para casos em que se tenha delimitadores ou espaços em branco extras.

```dart
void main() {
  String texto = " Olá   mundo  esta é  uma string ";

  // Dividir a string por espaços e remover espaços em branco extras
  List<String> palavras = texto
      .split(' ')
      .map((palavra) => palavra.trim())
      .where((palavra) => palavra.isNotEmpty)
      .toList();

  print(palavras); // Output: [Olá, mundo, esta, é, uma, string]
}
```

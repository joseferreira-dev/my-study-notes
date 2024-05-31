<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="banner.png"></a>
</div>
<br>

# Linguagem Dart

Versão: 3.4

- [Criação e Estrutura de um Projeto](./contents/criacao-e-estrutura-projeto/README.md)
- [Tipos de dados](./contents/tipos-de-dados/README.md)
- [Conversão entre Tipos](./contents/conversao-entre-tipos/README.md)
- [Declaração de variáveis](./contents/declaracao-de-variaveis/README.md)
- [Usando Null Safety com Variáveis](./contents/null-safety-variaveis/README.md)
- [Operadores](./contents/operadores/README.md)
- [Estruturas de Controle](./contents/estruturas-de-controle/README.md)
- 
- [Listas (Arrays)](#listas-arrays)
- [Iterar sobre Listas](#iterar-sobre-listas)
- [Usando Null Safety com Listas](#usando-null-safety-com-listas)
- [Sets](#sets)
- [Iterar sobre Sets](#iterar-sobre-sets)
- [Usando Null Safety com Sets](#usando-null-safety-com-sets)
- [Maps](#maps)
- [Funções](#funções)
- [Enumeradores (Enums)](#enumeradores-enums)
- [Exceções (Exceptions)](#exceções-exceptions)
- [Importações (Imports)](#importações-imports)

## Listas (Arrays)

Em Dart, listas (arrays) são uma coleção ordenada de elementos que podem ser acessados por meio de índices. Listas são muito utilizadas para armazenar sequências de valores de um mesmo tipo.

### Criar uma Lista

É possível criar uma lista vazia e adicionar elementos posteriormente.

~~~dart
void main() {
  List<int> numeros = [];
  print(numeros); // Output: []
}
~~~

Uma lista também pode ser criada com elementos iniciais.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas); // Output: [maçã, banana, laranja]
}
~~~

Uma lista também pode ser criada sem definir o seu tipo com o operador `var`, o que permite que o tipo seja inferido.

~~~dart
void main() {
  var numeros = [1, 2, 3, 4, 5];
  print(numeros); // Output: [1, 2, 3, 4, 5]
}
~~~

### Gerar uma Lista

É possível gerar uma lista de elementos conforme uma instrução específica com o método `List.generate`, preencher uma lista com `List.filled` e criar uma lista nova a partir de outra com os métodos `List.from` e `List.of`.

~~~dart
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
~~~

### Acessar Elementos

Os elementos da lista são acessados por meio de índices, começando do índice `0`.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas[0]); // Output: maçã
  print(frutas[1]); // Output: banana
  print(frutas[2]); // Output: laranja
}
~~~

Isso também pode ser feito com o método `elementAt`.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.elementAt(2)); // Output: laranja
}
~~~

O primeiro e o último elemento da lista podem ser acessados usando `first` e `last`.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.first); // Output: maçã
  print(frutas.last); // Output: laranja
}
```

Também pode-se acessar partes da lista (uma sublista) com o método `sublist`.

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

### Modificar Elementos

Para modificar os elementos da lista basta atribuir novos valores aos índices correspondentes.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas[1] = 'manga';
  print(frutas); // Output: [maçã, manga, laranja]
}
~~~

Também pode-se modificar os elementos com os métodos `setAll` (define todos os elementos a partir da posição especificada com os elementos do iterável), `fillRange` (preenche a faixa de índices especificada com o valor especificado) e `replaceRange` (substitui a faixa de elementos especificada com os elementos do iterável).

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

### Adicionar e Inserir Elementos

O método `add` é utilizado para adicionar um único elemento à lista.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3];
  numeros.add(4);
  print(numeros); // Output: [1, 2, 3, 4]
}
~~~

Já o método `addAll` adiciona múltiplos elementos à lista.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3];
  numeros.addAll([4, 5, 6]);
  print(numeros); // Output: [1, 2, 3, 4, 5, 6]
}
~~~

Também é possível inserir novos elementos em uma posição específica com os métodos `insert` (um elemento) e `insertAll` (vários elementos).

~~~dart
void main() {
  List<int> numbers = [1, 2, 3];

  numbers.insert(1, 10);
  print(numbers); // Output: [1, 10, 2, 3]

  numbers.insertAll(2, [20, 30]);
  print(numbers); // Output: [1, 10, 20, 30, 2, 3]
}
~~~

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

### Remover Elementos

O método `remove` remove um elemento específico da lista.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.remove('banana');
  print(frutas); // Output: [maçã, laranja]
}
~~~

Para remover um elemento em um índice específico pode-se utilizar o método `removeAt`.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.removeAt(1);
  print(frutas); // Output: [maçã, laranja]
}
~~~

Para remover o último elemento da lista usa-se o método `removeLast`.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.removeLast();
  print(frutas); // Output: [maçã, banana]
}
~~~

O método `removeWhere` remove elementos segundo uma condição específica.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja', 'manga'];
  frutas.removeWhere((fruta) => fruta[0] == 'm');
  print(frutas); // Output: [banana, laranja]
}
~~~

Para remover todos os elementos, ou seja, limpar a lista, usa-se o método `clear`.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  frutas.clear();
  print(frutas); // Output: []
}
~~~

### Ordenar, Reverter e Embaralhar uma Lista

Pode-se usar o método `sort` para ordenar a lista usando a função de comparação especificada (ou a ordem natural, se nenhuma função for fornecida). Para reverter a ordem dos elementos usa-se o método `reversed`. Já o método `shuffle` embaralha os elementos.

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

### Tamanho de uma Lista

Usando a propriedade `length` pode-se obter o número de elementos na lista.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.length); // Output: 3
}
```

### Consultas

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

### Criar uma String a partir de uma Lista

O método `join` combina todos os elementos da lista em uma única string, separando-os por um delimitador especificado.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.join(', ')); // Output: maçã, banana, laranja
  print(frutas.join(' | ')); // Output: maçã | banana | laranja
  print(frutas.join(' -> ')); // Output: maçã -> banana -> laranja
}
```

### Criar uma Lista a partir de uma String

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

## Iterar sobre Listas

Em Dart, pode-se iterar sobre listas de várias maneiras, utilizando diferentes métodos e técnicas de iteração. Deve ter atenção ao utilizar alguns desses métodos, pois muitos deles retornam um iterable, que é diferente de uma lista, sendo necessário sua conversão de volta para lista com o método `toList`.

### Usando `for`, `for-in` e `forEach`

Os loops `for`, `for-in` e o método `forEach` são usados para iterar sobre listas ou outras coleções, mas têm diferenças importantes em termos de sintaxe, funcionalidades e uso.

~~~dart
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
~~~

O loop `for` é muito flexível e permite um controle detalhado sobre o índice da iteração. É útil quando você precisa acessar o índice dos elementos ou manipular o índice durante a iteração. Já o loop Um loop `for-in` é mais simples e mais legível quando você só precisa acessar os elementos sem se preocupar com os índices. E o método `forEach` é uma função de ordem superior que aplica uma função callback a cada elemento da lista.

As principais diferenças entre essas três formas são:

- O loop `for` é o único que permite acesso direto e manipulação dos índices da lista;
- O método `forEach` não suporta o uso de break e continue, o que pode ser feito nos loops `for` e `for-in`;
- O método `for` é o único que permite leitura e modificação dos elementos da lista, enquanto que `for-in` e `forEach` não permite modificação;

### Usando `map`

O método `map` aplica uma função a cada elemento e retorna uma nova lista com os resultados.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3];
  List<int> quadrados = numeros.map((numero) => numero * numero).toList();
  print(quadrados); // Output: [1, 4, 9]
}
~~~

### Usando `where`

O método `where` filtra elementos com base em uma condição e retorna uma nova lista com os elementos que satisfazem a condição.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  List<int> pares = numeros.where((numero) => numero % 2 == 0).toList();
  print(pares); // Output: [2, 4]
}
~~~

### Usando `expand`

O método `expand` aplica uma função a cada elemento e retorna uma única lista contendo os resultados concatenados.

~~~dart
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
~~~

### Usando `reduce` e `fold`

O método `reduce` combina todos os elementos da lista em um único valor, aplicando uma função binária.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  int soma = numeros.reduce((a, b) => a + b);
  print(soma); // Output: 15
}
~~~

O método `fold` é semelhante ao `reduce`, mas permite especificar um valor inicial.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5];
  int soma = numeros.fold(10, (anterior, atual) => anterior + atual);
  print(soma); // Output: 25
}
~~~

### Usando `every` e `any`

O método `every` verifica se todos os elementos satisfazem uma condição, enquanto que `any` verifica se algum dos elementos satisfaz uma condição.

~~~dart
void main() {
  List<int> numeros = [2, 4, 6, 8, 9];
  
  bool todosPares = numeros.every((numero) => numero % 2 == 0);
  print(todosPares); // Output: false
  
  bool algumPar = numeros.any((numero) => numero % 2 == 0);
  print(algumPar); // Output: true
}
~~~

### Usando `take` e `skip`

O método `take` pega os primeiros n elementos da lista e `skip` pula os primeiros n elementos da lista e retornar o restante.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5, 6];
  
  var primeirosTres = numeros.take(3).toList();
  print(primeirosTres); // Output: [1, 2, 3]
  
  var depoisDeTres = numeros.skip(3).toList();
  print(depoisDeTres); // Output: [4, 5, 6]
}
~~~

### Usando `takeWhile` e `skipWhile`

O método `takeWhile` pega os elementos enquanto a condição for verdadeira e `skipWhile` pula os elementos enquanto a condição for verdadeira e retornar o restante.

~~~dart
void main() {
  List<int> numeros = [1, 2, 3, 4, 5, 6];
  
  var menoresQueQuatro = numeros.takeWhile((numero) => numero < 4).toList();
  print(menoresQueQuatro); // Output: [1, 2, 3]
  
  var aPartirDeQuatro = numeros.skipWhile((numero) => numero < 4).toList();
  print(aPartirDeQuatro); // Output: [4, 5, 6]
}
~~~

## Usando Null Safety com Listas

Quando trabalha-se com listas, o Null Safety permite que se trabalhe de forma mais assegurada com listas que podem ser nulas ou conter elementos nulos.

### Listas Nulas e Elementos Nulos

Pode-se declarar uma lista que pode ser nula usando o operador `?` após a declaração da lista. Isso significa que a variável pode ser uma lista ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  List<String>  frutas1 = null;   // Não compila
  List<String>? frutas2 = null;   // Compila
  List<String>? frutas3 = [null]; // Não Compila
}
```

Colocando-se o operador `?` após a declaração do tipo da lista define que a lista pode conter elementos nulos, mas elas em si não pode ser nula.

```dart
void main() {
  List<String>  frutas1 = [null];   // Não compila
  List<String?> frutas2 = [null];   // Compila
  List<String?> frutas3 = null;     // Não Compila
}
```

Usando-se as duas formas de declaração unidas pode-se definir uma lista que pode ser nula e que pode conter elementos nulos.

```dart
void main() {
  List<String?>? frutas1 = [null];  // Compila
  List<String?>? frutas2 = null;    // Compila
}
```

### Acesso a Elementos Nulos

O operador de acesso seguro `?.` pode ser usado para acessar elementos de uma lista que podem ser nulos.

```dart
void main() {
  List<String?> frutas = ['maçã', null, 'laranja'];
  
  String? primeiraFruta = frutas[0];
  print(primeiraFruta?.toUpperCase()); // Output: MAÇÃ
  
  String? segundaFruta = frutas[1];
  print(segundaFruta?.toUpperCase()); // Output: null
}
```

### Lista com Valor Padrão

Um valor padrão pode ser atribuido para uma lista que pode ser nula usando o operador `??`.

```dart
void main() {
  List<String>? frutas;

  List<String> frutasSeguras = frutas ?? [];
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar valor padrão a elementos a uma lista caso estes sejam nulos.

```dart
void main() {
  String fruta1 = 'maça';
  String? fruta2 = null;
  String fruta3 = 'laranja';
  
  List<String> frutas = [
    fruta1,
    fruta2 ?? 'banana',
    fruta3
  ];
  
  print(frutas); // Output: [maça, banana, laranja]
}
```

### Iterando com Listas Nulas

É possível utilizar os métodos como `map`, `where` e outros, lidando com elementos nulos de maneira segura.

```dart
void main() {
  List<String?> frutas = ['maçã', null, 'laranja'];
  
  // Remove elementos nulos
  List<String> frutasSemNulos = frutas
    .where((fruta) => fruta != null)
    .map((fruta) => fruta!)
    .toList();

  print(frutasSemNulos); // Output: [maçã, laranja]
}
```

## Sets

Em Dart, um `Set` é uma coleção não ordenada de elementos únicos. Diferente de uma `List`, um `Set` não permite elementos duplicados. Essa estrutura de dados é útil quando você precisa garantir que não haverá elementos repetidos.

### Criar um `Set`

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

### Acessar Elementos

Para um `Set` o método `elementAt` e as propriedades `first` e `last` funcionam da mesma forma que em uma lista.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  print(frutas.elementAt(1)); // Output: banana
  print(frutas.first); // Output: maçã
  print(frutas.last); // Output: laranja
}
```

### Modificar Elementos

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

### Adicionar Elementos

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

### Remover Elementos

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

### Consultas

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

### Tamanho de um `Set`

O `Set` também possui a propriedade `length` que retorna seu tamanho.

```dart
void main() {
  Set<String> frutas = {'maçã', 'banana', 'laranja'};
  print(frutas.length); // Output: 3
}
```

### Operações

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

## Iterar sobre Sets

Em Dart, pode-se iterar sobre um `Set` da mesma maneira que em uma lista, atentando-se ao fato de que alguns métodos retornam um iterable, sendo necessário sua conversão de volta para um `Set` com o método `toSet`.

### Usando `for`, `for-in` e `forEach`

Os loops `for`, `for-in` e o método `forEach` funcionam da mesma forma que nas listas.

~~~dart
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
~~~

### Usando `map`, `where`, `reduce`, `fold` e `expand`

Os métodos `map`, `where`, `reduce`, `fold` e `expand` funcionam da mesma forma qu e nas listas, ressaltando-se que todos os elementos repetidos são eliminados do `Set` final.

~~~dart
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
~~~

### Usando `every` e `any`

Ambos os métodos `every` e `any` funcionam da mesma forma que em listas.

~~~dart
void main() {
  Set<int> numeros = {2, 4, 6, 8, 9};
  
  bool todosPares = numeros.every((numero) => numero % 2 == 0);
  print(todosPares); // Output: false
  
  bool algumPar = numeros.any((numero) => numero % 2 == 0);
  print(algumPar); // Output: true
}
~~~

### Usando `take`, `skip`, `takeWhile` e `skipWhile`

Os métodos `take`, `skip`, `takeWhile` e `skipWhile` também funcionam da mesma forma que nas listas.

~~~dart
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
~~~

## Usando Null Safety com Sets

O Null Safety tmambm permite que se trabalhe de forma mais assegurada com Sets que podem ser nulos ou conter elementos nulos.

### Sets Nulos e Elementos Nulos

Pode-se declarar um `Set` que pode ser nulo usando o operador `?` após a sua declaração. Isso significa que a variável pode ser uma lista ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  Set<String>  frutas1 = null;   // Não compila
  Set<String>? frutas2 = null;   // Compila
  Set<String>? frutas3 = {null}; // Não Compila
}
```

Colocando-se o operador `?` após a declaração do tipo do `Set` define que ele pode conter elementos nulos, mas o `Set` em si não pode ser nulo.

```dart
void main() {
  Set<String>  frutas1 = {null};   // Não compila
  Set<String?> frutas2 = {null};   // Compila
  Set<String?> frutas3 = null;     // Não Compila
}
```

Usando-se as duas formas de declaração unidas pode-se definir um `Set` que pode ser nulo e que pode conter elementos nulos.

```dart
void main() {
  Set<String?>? frutas1 = {null};  // Compila
  Set<String?>? frutas2 = null;    // Compila
}
```

### Acesso a Elementos Nulos

O operador de acesso seguro `?.` pode ser usado para acessar elementos de um `Set` que podem ser nulos.

```dart
void main() {
  Set<String?> frutas = {'maçã', null, 'laranja'};
  
  String? primeiraFruta = frutas.elementAt(0);
  print(primeiraFruta?.toUpperCase()); // Output: MAÇÃ
  
  String? segundaFruta = frutas.elementAt(1);
  print(segundaFruta?.toUpperCase()); // Output: null
}
```

### `Set` com Valor Padrão

Um valor padrão pode ser atribuido a um `Set` que pode ser nulo usando o operador `??`.

```dart
void main() {
  Set<String>? frutas;

  Set<String> frutasSeguras = frutas ?? {};
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar valor padrão a elementos de um `Set` caso estes sejam nulos.

```dart
void main() {
  String fruta1 = 'maça';
  String? fruta2 = null;
  String fruta3 = 'laranja';
  
  Set<String> frutas = {
    fruta1,
    fruta2 ?? 'banana',
    fruta3
  };
  
  print(frutas); // Output: {maça, banana, laranja}
}
```

### Iterando com Sets Nulos

É possível utilizar os métodos como `map`, `where` e outros, lidando com elementos nulos de maneira segura.

```dart
void main() {
  Set<String?> frutas = {'maçã', null, 'laranja'};
  
  // Remove elementos nulos
  Set<String> frutasSemNulos = frutas
    .where((fruta) => fruta != null)
    .map((fruta) => fruta!)
    .toSet();

  print(frutasSemNulos); // Output: {maçã, laranja}
}
```

## Maps

Em Dart, um `Map` é uma coleção de pares chave-valor, onde cada chave única é associada a um valor. Os Maps são úteis para armazenar e manipular dados de maneira eficiente, especialmente quando você precisa de acesso rápido a valores com base em chaves específicas.

### Criar um Map

Pode-se criar um `Map` usando o construtor padrão ou a notação literal com chaves `{}`.

```dart
void main() {
  Map<int, String> frutas = Map();
  frutas[1] = 'maçã';
  frutas[2] = 'banana';
  frutas[3] = 'laranja';

  Map<int, String> animais = {
    1: 'gato',
    2: 'cachorro',
    3: 'hamster'
  };

  print(frutas); // Output: {1: maçã, 2: banana, 3: laranja}
  print(animais); // Output: {1: gato, 2: cachorro, 3: hamster}
}
```

### Acessar e Modificar Elementos

Pode-se acessar e modificar os valores em um `Map` usando suas chaves.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais['Brasil']); // Output: Brasília

  capitais['Brasil'] = 'Brasilia';
  print(capitais['Brasil']); // Output: Brasilia
}
```

Dois métodos específicos para modificar elementos em um Map são `update` e `updateAll`. O método `updateAll` atualiza todos os valores no mapa usando uma função de atualização fornecida. Essa função é aplicada a cada par chave-valor no mapa.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.updateAll((chave, valor) => 'Capital: $valor');

  print(capitais);
  // Output: {Brasil: Capital: Brasília, França: Capital: Paris, Japão: Capital: Tóquio}
}
```

O método `update` atualiza o valor de uma chave específica no map usando uma função de atualização. Se a chave não existir, pode-se opcionalmente fornecer uma função `ifAbsent` para adicionar a chave com um valor padrão.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.update('Brasil', (valor) => 'Brasília');
  
  // Tentar atualizar a capital de um país não existente, fornecendo um valor padrão
  capitais.update('Alemanha', (valor) => 'Berlim', ifAbsent: () => 'Berlim');
  print(capitais);
  // Output: {Brasil: Nova Brasília, França: Paris, Japão: Tóquio, Alemanha: Berlim}
}
```

### Adicionar Elementos

Para adicionar novos elementos existe basta adicionar uma nova chave e o seu valor correspondente.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais['EUA'] = 'Washington';
  print(capitais); // Output: {Brasil: Brasilia, França: Paris, Japão: Tóquio, EUA: Washington}
}
```

Também existe o método addAll, que adiciona vários elementos de uma vez.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.addAll({'EUA': 'Washington', 'Portugal': 'Lisboa'});
  print(capitais);
  // Output: {Brasil: Brasília, França: Paris, Japão: Tóquio, EUA: Washington, Portugal: Lisboa}
}
```

Por fim, existe o método `putIfAbsent`, que só adiciona o valor se a chave informada ainda não existir no `Map`, permitindo ainda a execução de operações com o valor a ser inserido.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.putIfAbsent('Brasil', () => 'Distrito Federal');
  print(capitais); // Output: {Brasil: Brasília, França: Paris, Japão: Tóquio}
}
```

### Remover Elementos

Para um `Map` pode-se usar o método `remove`, que recebe uma chave e remove o elemento correspondente, e o método `clear`, que remove todos os elementos.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.remove('Brasil');
  print(capitais); // Output: {França: Paris, Japão: Tóquio}
  
  capitais.clear();
  print(capitais); // Output: {}
}
```

Pode-se também remover vários elementos que satisfazem uma condição específica com o método `removeWhere`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };
  
  capitais.removeWhere((key, value) => key == 'França');
  print(capitais); // Output: {Brasil: Brasília, Japão: Tóquio}
}
```

### Tamanho de um `Map`

A propriedade `length` de um `Map` corresponde ao número de elementos que ele possui.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.length); // Output: 3
}
```

### Consultas

Nos Maps pode-se consultar a existência de elementos por meio de duas chaves com `containsKey` e por meio de seus valores com `containsValue`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.containsKey('Brasil')); // Output: true
  print(capitais.containsValue('Paris')); // Output: true
}
```

Também pode-se verificar se um `Map` contém ou não elementos com as propriedades `isEmpty` e `isNotEmpty`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.isEmpty); // Output: false
  print(capitais.isNotEmpty); // Output: true
}
```

### Propriedades `keys`, `values` e `entries`

Os Maps possuem três propriedades muito importantes acerca de seus elementos, sendo elas `keys`, `values` e `entries`. A propriedade `keys` retorna um iterável com todas as chaves do `Map`, `values` por sua vez retorna todos os valores, já `entries` retorna um iterável com entradas `MapEntry`, onde cada uma contém uma chave e um valor.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };
  
  Iterable<String> chaves = capitais.keys;
  print(chaves); // Output: (Brasil, França, Japão)

  Iterable<String> valores = capitais.values;
  print(valores); // Output: (Brasília, Paris, Tóquio)
  
  Iterable<MapEntry<String, String>> entradas = capitais.entries;
  print(entradas); // Output: (MapEntry(Brasil: Brasília), MapEntry(França: Paris), MapEntry(Japão: Tóquio))
}
```

### Transformações de Maps

Suponha-se que se quer transformar o mapa de capitais de modo que os valores sejam alterados para incluir uma descrição adicional. Pode-se usar a propriedade `entries` para iterar sobre os pares chave-valor e o método `map` para criar um novo `Map` transformado.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  // Transformar o mapa adicionando uma descrição aos valores
  Map<String, String> capitaisTransformadas = Map.fromEntries(
    capitais.entries.map((entrada) {
      String novaDescricao = '${entrada.value} é a capital de ${entrada.key}';
      return MapEntry(entrada.key, novaDescricao);
    })
  );

  print(capitaisTransformadas);
  // Output: {Brasil: Brasília é a capital de Brasil, França: Paris é a capital de França, Japão: Tóquio é a capital de Japão}
}
```

## Funções

Funções são blocos de código que realizam tarefas específicas. Em Dart, funções são elementos fundamentais que permitem a reutilização de código e a organização lógica de tarefas. 

### Definição de Funções

Para definir uma função em Dart, você precisa especificar um tipo de retorno, um nome, e entre parênteses, os parâmetros (se houver). Aqui está a estrutura básica:

```dart
tipoRetorno nomeFuncao(parâmetros) {
  // corpo da função
}
```

Exemplos de funções simples seriam:

```dart
String nomeCompleto (String nome, String sobrenome) {
  return '$nome $sobrenome';
}

void saudacao(String nomeCompleto) {
  print('Olá, $nomeCompleto!');
}
```

Para chamar funções no código basta utilizar seu nome:

```dart
void main() {
  saudacao(nomeCompleto('Marcos', 'Silva')); // Output: Olá, Marcos Silva!
}
```

### Funções com Parâmetros

Funções podem ou não receber parâmetros para operar.

```dart
void main() {
  int resultado = soma(3, 4);
  print(resultado); // Output: 7
}

int soma(int a, int b) {
  return a + b;
}
```

### Funções com Parâmetros Opcionais

Dart permite definir parâmetros opcionais. Eles podem ser posicionais ou nomeados.

Parâmetros opcionais posicionais são definidos com colchetes:

```dart
void main() {
  print(saudacao("João")); // Output: Olá, João !
  print(saudacao("João", "Silva")); // Output: Olá, João Silva!
}

String saudacao(String nome, [String sobrenome = ""]) {
  return "Olá, $nome $sobrenome!";
}
```

Parâmetros opcionais nomeados são definidos com chaves:

```dart
void main() {
  print(saudacao()); // Output: Olá, Mundo !
  print(saudacao(nome: "João")); // Output: Olá, João !
  print(saudacao(nome: "João", sobrenome: "Silva")); // Output: Olá, João Silva!
}

String saudacao({String nome = "Mundo", String sobrenome = ""}) {
  return "Olá, $nome $sobrenome!";
}
```

### Funções Anônimas (Lambdas)

Funções anônimas ou lambdas são funções que não têm nome. São frequentemente usadas em funções de ordem superior, como em coleções.

```dart
void main() {
  var lista = [1, 2, 3];
  lista.forEach((item) {
    print(item); // Output: 1, 2, 3
  });
}
```

### Arrow Functions

A notação de arrow function `=>` é utilizada para funções que consistem em uma única expressão. Por exemplo, a função:

```dart
int soma(int a, int b) => a + b;
```

Equivale a:

```dart
int soma(int a, int b) {
  return a + b;
}
```

### Funções como Cidadãos de Primeira Classe

Em Dart, funções são cidadãos de primeira classe, o que significa que você pode atribuí-las a variáveis, passá-las como parâmetros para outras funções e retorná-las de funções.

```dart
void main() {
  executaOperacao(3, 4, soma); // Output: 7
}

void executaOperacao(int a, int b, int Function(int, int) operacao) {
  print(operacao(a, b));
}

int soma(int a, int b) => a + b;
```

### Funções Assíncronas

Para operações assíncronas, você pode definir funções usando `async` e `await`.

```dart
Future<String> saudacao() async {
  return "Olá, mundo!";
}

void main() async {
  print(await saudacao()); // Output: Olá, mundo!
}
```

## Enumeradores (Enums)

Em Dart, um enumerador (ou enum) é um tipo especial que permite definir um conjunto de valores nomeados e constantes. Os enums são úteis para representar um conjunto fixo de constantes, como os dias da semana, estados de uma aplicação, tipos de usuário, etc.

### Criar um Enum

Para declarar um `enum`, usa-se a palavra-chave `enum` seguida pelo nome do `enum` e uma lista de valores entre chaves.

```dart
enum DiaDaSemana {
  segunda,
  terca,
  quarta,
  quinta,
  sexta,
  sabado,
  domingo,
}
```

### Usando Enums

Para usar os valores de um `enum` pode-se acessá-los pelo nome do `enum` seguido de um ponto `.`, seguido pelo valor.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;

  switch (hoje) {
    case DiaDaSemana.segunda:
      print('Hoje é segunda-feira.');
      break;
    case DiaDaSemana.terca:
      print('Hoje é terça-feira.');
      break;
    case DiaDaSemana.quarta:
      print('Hoje é quarta-feira.');
      break;
    case DiaDaSemana.quinta:
      print('Hoje é quinta-feira.');
      break;
    case DiaDaSemana.sexta:
      print('Hoje é sexta-feira.');
      break;
    case DiaDaSemana.sabado:
      print('Hoje é sábado.');
      break;
    case DiaDaSemana.domingo:
      print('Hoje é domingo.');
      break;
  }
}
```

### Propriedades e Métodos

Todo `enum` possui a propriedade `values` que corresponde a todos os seus valores.

```dart
void main() {
  print(DiaDaSemana.values); 
  // Output: [DiaDaSemana.segunda, DiaDaSemana.terca, DiaDaSemana.quarta, DiaDaSemana.quinta, DiaDaSemana.sexta, DiaDaSemana.sabado, DiaDaSemana.domingo]
}
```

Cada valor do `enum` tem uma propriedade `index` que retorna a posição do valor na declaração do `enum`.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.index); // Output: 4
}
```

Cada valor também tem a propriedade `name` que retorna o nome do valor.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.name); // Output: sexta
}
```

Existe o método `toString` que retorna uma representação de string do valor do `enum`.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.toString()); // Output: DiaDaSemana.sexta
}
```

### Adicionar Métodos a um Enum

Desde a versão 2.15 do Dart, é possível adicionar métodos e getters a enums.

```dart
enum Cor {
  vermelho,
  verde,
  azul;

  String get nome {
    switch (this) {
      case Cor.vermelho:
        return 'Vermelho';
      case Cor.verde:
        return 'Verde';
      case Cor.azul:
        return 'Azul';
    }
  }
  
  String get codigoHex {
    switch (this) {
      case Cor.vermelho:
        return '#FF0000';
      case Cor.verde:
        return '#00FF00';
      case Cor.azul:
        return '#0000FF';
    }
  }
}

void main() {
  var minhaCor = Cor.verde;
  print(minhaCor.nome); // Output: Verde
  print(minhaCor.codigoHex); // Output: #00FF00
}
```

### Iterar sobre Enums

Pode-se iterar sobre todos os valores de um `enum` usando a propriedade `values`.

```dart
void main() {
  for (var dia in DiaDaSemana.values) {
    print(dia);
  }
  // Output:
  // DiaDaSemana.segunda
  // DiaDaSemana.terca
  // DiaDaSemana.quarta
  // DiaDaSemana.quinta
  // DiaDaSemana.sexta
  // DiaDaSemana.sabado
  // DiaDaSemana.domingo
}
```

## Exceções (Exceptions)

Em Dart, exceções são usadas para sinalizar e tratar erros ou condições inesperadas que ocorrem durante a execução do programa. Exceções permitem que você separe a lógica de tratamento de erro do fluxo normal do código, tornando-o mais robusto e legível.

### Lançando Exceções

Para lançar uma exceção, usa-se a palavra-chave `throw` seguida por um objeto que represente a exceção. Dart tem várias classes de exceção integradas, como `Exception` e `Error`, mas também pode-se definir exceções personalizadas.

```dart
void main() {
  try {
    dividir(10, 0);
  } catch (e) {
    print('Ocorreu um erro: $e'); // Ocorreu um erro: Exception: Divisão por zero não é permitida
  }
}

void dividir(int a, int b) {
  if (b == 0) {
    throw Exception('Divisão por zero não é permitida');
  }
  print(a / b);
}
```

### Capturando Exceções

Pode-se capturar exceções usando um bloco `try-catch`. Se uma exceção for lançada no bloco `try`, o controle será passado para o bloco `catch`.

```dart
void main() {
  try {
    int resultado = dividir(10, 0);
    print('Resultado: $resultado');
  } catch (e) {
    print('Ocorreu um erro: $e');
  }
}

int dividir(int a, int b) {
  if (b == 0) {
    throw Exception('Divisão por zero não é permitida');
  }
  return a ~/ b; // Operador de divisão inteira
}
```

### Usando Bloco Finally

Um bloco `finally` é executado após os blocos `try` e `catch`, independentemente de uma exceção ter sido lançada ou não. É útil para liberar recursos ou executar código de limpeza.

```dart
void main() {
  try {
    int resultado = dividir(10, 2);
    print('Resultado: $resultado'); // Output: Resultado: 5
  } catch (e) {
    print('Ocorreu um erro: $e');
  } finally {
    print('Operação concluída.'); // Output: Operação concluída.
  }
}

int dividir(int a, int b) {
  if (b == 0) {
    throw Exception('Divisão por zero não é permitida');
  }
  return a ~/ b;
}
```

### Exceções Personalizadas

É possível definir classes próprias de exceção para representar erros específicos de uma aplicação.

```dart
void main() {
  try {
    validarIdade(-5);
  } catch (e) {
    print('Ocorreu um erro: $e');
  }
}

void validarIdade(int idade) {
  if (idade < 0) {
    throw IdadeInvalidaException(idade);
  }
  print('Idade válida: $idade');
}

class IdadeInvalidaException implements Exception {
  final int idade;

  IdadeInvalidaException(this.idade);

  @override
  String toString() {
    return 'Idade inválida: $idade';
  }
}
```

### Tratamento Específico de Exceções

Para tratar tipos de erros diferentes de maneiras distintas pode-se atribuir tipos específicos de exceções para cada um.

```dart
void main() {
  try {
    int resultado = dividir(10, 0);
    print('Resultado: $resultado');
  } on IntegerDivisionByZeroException {
    print('Erro: Divisão por zero.');
  } catch (e) {
    print('Ocorreu um erro: $e');
  }
}

int dividir(int a, int b) {
  if (b == 0) {
    throw IntegerDivisionByZeroException();
  }
  return a ~/ b;
}

class IntegerDivisionByZeroException implements Exception {
  @override
  String toString() => 'Divisão por zero não é permitida';
}
```

## Importações (Imports)

Em Dart, um código pode ser organizado em diferentes arquivos, que são importados conforme necessário para reutilizar funções, classes, constantes e outros elementos. Existem diferentes formas de importar bibliotecas e pacotes em Dart.

### Importação Básica

Para importar um arquivo usa-se a palavra-chave `import` seguida pelo caminho do arquivo entre aspas simples ou duplas.

```dart
// lib/main.dart
import 'utils.dart';

void main() {
  saudacao();
}

// lib/utils.dart
void saudacao() {
  print('Olá, mundo!');
}
```

### Importações Relativas e Absolutas

É possível fazer importações relativas ou absolutas em Dart. Nas relativas usa-se caminhos relativos ao local do arquivo atual.

```dart
// lib/main.dart
import 'utils.dart'; // Importa utils.dart do mesmo diretório
import 'subdir/other_utils.dart'; // Importa other_utils.dart do subdiretório subdir
```

Nas importações absolutas usa-se o pacote como base para o caminho. Isso é comum quando se trabalha com pacotes.

```dart
// lib/main.dart
import 'package:meu_pacote/utils.dart';
import 'package:meu_pacote/subdir/other_utils.dart';
```

### Pacotes e Pubspec.yaml

Quando se está usando pacotes de terceiros ou deseja-se organizar seu próprio código como um pacote, deve-se adicionar as dependências no arquivo `pubspec.yaml`.

```yaml
name: meu_pacote
dependencies:
  http: ^0.13.3
```

Depois de adicionar a dependência, pode-se importar o pacote:

```dart
import 'package:http/http.dart' as http;

void main() {
  var url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  http.get(url).then((response) {
    print('Response status: ${response.statusCode}');
    print('Response body: ${response.body}');
  });
}
```

### Alias de Biblioteca

Usa-se aliases para evitar conflitos de nome e tornar o código mais claro.

```dart
import 'package:http/http.dart' as http;
import 'package:meu_pacote/utils.dart' as utils;

void main() {
  utils.saudacao();

  var url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  http.get(url).then((response) {
    print('Response status: ${response.statusCode}');
    print('Response body: ${response.body}');
  });
}
```

### Mostrar e Ocultar Componentes

Pode-se especificar quais partes de uma biblioteca deseja-se importar usando as palavras-chave `show` e `hide`.

```dart
// Importa apenas a função saudacao de utils.dart
import 'utils.dart' show saudacao;

// Importa tudo de utils.dart exceto a função saudacao
import 'utils.dart' hide saudacao;
```

### Exportando Bibliotecas

Usa-se a palavra reservada `export` para reexportar componentes de outras bibliotecas, facilitando a criação de APIs públicas em um pacote.

```dart
// lib/utils.dart
void saudacao() {
  print('Olá, mundo!');
}

// lib/api.dart
export 'utils.dart';

// lib/main.dart
import 'package:meu_pacote/api.dart';

void main() {
  saudacao(); // Saudacao é acessível aqui devido à reexportação
}
```

### Importações Diferenciadas com `deferred as`

Para carregamento tardio (lazy loading), pode-se usar `deferred as` para carregar uma biblioteca somente quando necessário.

```dart
import 'utils.dart' deferred as utils;

void main() async {
  await utils.loadLibrary(); // Carrega a biblioteca
  utils.saudacao();
}
```

### Configurando um Padrão de Exportação em um Projeto

Para configurar o tipo de exportação padrão em um projeto Dart, pode-se usar o linter do Dart junto com as regras recomendadas para exportação, o linter já vem pré-instalado no projeto como dependência de desenvolvimento no arquivo `pubspec.yaml`.

```yaml
dev_dependencies:
  lints: ^2.1.0 # Linter
  test: ^1.24.0
```

O arquivo de configuração para o linter em um projeto Dart é o `analysis_options.yaml`. Este arquivo permite que se defina regras de linting específicas para o projeto, ajudando a manter a consistência e a qualidade do código. Por padrão, o arquivo vem dessa forma:

```yaml
include: package:lints/recommended.yaml

# Uncomment the following section to specify additional rules.

# linter:
#   rules:
#     - camel_case_types
```

Para aplicar um padrão de importação, basta descomentar o `linter` e adicionar a regra específica. Duas opções são possíveis: `prefer_relative_imports` (improtações relativas como padrão) e `always_use_package_imports` (importações absolutas como padrão).

```yaml
include: package:lints/recommended.yaml

linter:
  rules:
    - prefer_relative_imports: true
    # - always_use_package_imports: true
```

## FIM

<!--
```dart

```
-->
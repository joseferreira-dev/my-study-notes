# Linguagem Dart

Versão: 3.4

- [Criando um novo projeto](#criando-um-novo-projeto)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Tipos de dados](#tipos-de-dados)
- [Declaração de variáveis](#declaração-de-variáveis)
- [Usando o Null Safety](#usando-o-null-safety)
- [Estruturas Condicionais](#estruturas-condicionais)
- [Operadores Lógicos](#operadores-lógicos)
- [Listas (Arrays)](#listas-arrays)
- [Iterar sobre Listas](#iterar-sobre-listas)

## Criando um novo projeto

Para se criar um novo projeto Dart básico pelo console deve-se utilizar o comando:

~~~shell
dart create project_name
~~~

É uma convensão da comunidade separar o nome do projeto com `_`. É possível definir um template inicial para o projeto utilizando a flag `-t`.

~~~shell
dart create -t console console_project_name
~~~

Os possíveis templates são: 

- `console`: Cria um projeto de console simples, ideal para aplicativos de linha de comando.
- `package`: Cria um novo pacote Dart, que pode ser uma biblioteca ou um módulo reutilizável.
- `server-shelf`: Cria um projeto de servidor usando o pacote Shelf, uma estrutura de servidor web minimalista para Dart.
- `web`: Cria um projeto para desenvolvimento web, incluindo um servidor de desenvolvimento e a configuração para trabalhar com Dart no navegador.
- `web-simple`: Cria um projeto web simples, que é uma versão mínima de um projeto web.
- `flutter`: Cria um projeto Flutter, que é uma estrutura para construir aplicativos nativos para iOS, Android, web e desktop com uma única base de código.

## Estrutura do projeto

Quando você cria um projeto de console em Dart usando o comando `dart create -t console console_project`, uma estrutura de diretórios e arquivos é gerada para ajudar a começar com um aplicativo de console simples. A estrutura típica do projeto é a seguinte:

~~~shell
console_project/
├── bin/
│   └── console_project.dart
├── lib/
├── test/
├── .gitignore
├── analysis_options.yaml
├── CHANGELOG.md
├── pubspec.yaml
└── README.md
~~~

- `bin/console_project.dart`: Arquivo principal do aplicativo de console, onde se escreve o código para o seu aplicativo. O nome do arquivo geralmente corresponde ao nome do projeto.
- `lib/`: Pasta usada para armazenar qualquer biblioteca ou código que será compartilhado entre diferentes partes do aplicativo. É útil para organizar componentes maiores.
- `test/`: Pasta que contém arquivos de teste para o projeto.
- `.gitignore`: Arquivo usado para especificar quais arquivos e pastas devem ser ignorados pelo controle de versão Git.
- `analysis_options.yaml`: Arquivo que permite configurar regras de análise estática para o projeto, como linter e regras de estilo de código.
- `CHANGELOG.md`: Arquivo de log de alterações onde se pode documentar as mudanças feitas em cada versão do projeto.
- `pubspec.yaml`: Arquivo de especificação do pacote Dart. Ele define o nome do projeto, a versão, as dependências do projeto, as dependências de desenvolvimento e outras informações de configuração. 
- `README.md`: Arquivo de documentação onde se pode descrever o propósito do projeto, como configurá-lo e usá-lo, e qualquer outra informação relevante.

## Tipos de dados

Dart é uma linguagem fortemente tipada que oferece uma ampla variedade de tipos de dados. Aqui estão os principais tipos de dados em Dart, junto com exemplos para cada um:

### Numbers (Números)

Dart possui dois tipos de números: `int` (representa números inteiros) e `double` (representa números decimais).

~~~dart
int idade = 30;
int ano = 2024;

double altura = 1.75;
double preco = 99.99;
~~~

### String

`String` é usado para representar texto.

~~~dart
String nome = 'João';
String saudacao = "Olá, mundo!";
String textoMultilinha = '''Este é um
texto de múltiplas
linhas.''';
~~~

### Booleans (Booleanos)

`bool` representa valores booleanos: `true` ou `false`.

~~~dart
bool estaChovendo = false;
bool estaAberto = true;
~~~

### Lists (Listas)

`List` é uma coleção ordenada de objetos. Pode ser homogênea (todos os elementos do mesmo tipo) ou heterogênea.

~~~dart
List<int> numeros = [1, 2, 3, 4, 5];
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<dynamic> misturado = [1, 'dois', 3.0, true];
~~~

### Maps

`Map` é uma coleção de pares chave-valor. As chaves e os valores podem ser de qualquer tipo.

~~~dart
Map<String, int> telefones = {
  'João': 123456789,
  'Maria': 987654321
};

Map<int, String> produtos = {
  1: 'Laptop',
  2: 'Mouse',
  3: 'Teclado'
};
~~~

### Sets

`Set` é uma coleção de elementos únicos, sem ordem específica.

~~~dart
Set<int> numerosUnicos = {1, 2, 3, 4, 5};
Set<String> cores = {'vermelho', 'verde', 'azul'};
~~~

### Null

Dart tem suporte para o tipo `Null`, que é usado para representar a ausência de um valor. Variáveis podem ser declaradas como nulas, a menos que sejam explicitamente marcadas como não nulas.

~~~dart
String? nomeNulo = null;
int? numeroNulo = null;
~~~

## Declaração de variáveis

Em Dart, a declaração de variáveis pode ser feita de várias maneiras, permitindo diferentes níveis de especificidade e flexibilidade. 

### Declaração Explícita com Tipo

Você pode declarar uma variável especificando explicitamente seu tipo. Isso garante que a variável só pode armazenar valores desse tipo.

~~~dart
int idade = 30;
double altura = 1.75;
String nome = 'João';
bool estaChovendo = false;
~~~

### Declaração com `var`

Quando você usa `var`, o tipo da variável é inferido a partir do valor inicial atribuído a ela. Depois que o tipo é inferido, a variável só pode armazenar valores desse tipo.

~~~dart
var idade = 30; // inferido como int
var altura = 1.75; // inferido como double
var nome = 'João'; // inferido como String
var estaChovendo = false; // inferido como bool
~~~

### Declaração com `dynamic`

O tipo `dynamic` permite que a variável armazene valores de qualquer tipo. Isso pode ser útil em situações onde o tipo de dado pode mudar, mas deve ser usado com cautela, pois elimina a verificação de tipo em tempo de compilação.

~~~dart
dynamic algo = 30;
algo = 'João';
algo = true;
~~~

### Declaração com `final` e `const`

Uma variável declarada com `final` pode ser atribuída apenas uma vez. O valor é determinado em tempo de execução. Já uma variável declarada com `const` é uma constante em tempo de compilação e deve ser inicializada com um valor constante.

~~~dart
final int idadeFinal = 30;
final nomeFinal = 'João'; // inferido como String

const double pi = 3.14159;
const nomeConst = 'Constante'; // inferido como String
~~~

A principal diferença entre as duas formas é que o usando `final` o valor é fixo após a atribuição inicial, mas pode ser determinado em tempo de execução. Já com `const` o valor é fixo e deve ser conhecido em tempo de compilação.

## Usando o Null Safety

Null safety é um recurso introduzido no Dart 2.12 para ajudar a evitar erros comuns relacionados ao uso de valores `null`. Com null safety, o sistema de tipos de Dart ajuda a garantir que valores `null` não sejam utilizados inadvertidamente, prevenindo muitos tipos de exceções em tempo de execução.

### Tipos Não Nulos por Padrão

Por padrão, todos os tipos em Dart são não nulos, o que significa que uma variável não pode conter `null` a menos que explicitamente declarado. Por exemplo:

~~~dart
int numero = 42; // Correto
numero = null;   // Erro de compilação
~~~

### Tipos Nuláveis

Para permitir que uma variável aceite um valor `null`, deve-se usar o operador de interrogação `?` após o tipo. Isso transforma o tipo em um tipo nulável.

~~~dart
int? numero = 42; // Correto
numero = null;    // Correto
~~~

### Operador de Atribuição Condicional `??`

O operador `??` permite fornecer um valor padrão para uma expressão que pode ser `null`.

~~~dart
int? numeroNulavel;
int valor = numeroNulavel ?? 0; // Se numeroNulavel for null, valor será 0
~~~

### Operador de Acesso Seguro `?.`

O operador `?.` permite chamar métodos ou acessar propriedades de um objeto de forma segura, retornando `null` se o objeto for `null`.

~~~dart
String? texto;
int? tamanho = texto?.length; // Se texto for null, tamanho será null
~~~

### Operador de Aserção Não Nula `!`

Se existe certeza de que uma variável nulável não é `null` em um determinado ponto do código, pode-se usar o operador de asserção não nula `!` para forçar o compilador a tratar a variável como não nula. Deve ser usado com cautela, pois se a variável for `null`, ocorrerá uma exceção em tempo de execução.

~~~dart
String? nomeNulavel = 'Dart';
int tamanho = nomeNulavel!.length; // Se nomeNulavel for null, lançará uma exceção
~~~

## Estruturas Condicionais

Em Dart, como em muitas outras linguagens de programação, as estruturas condicionais são usadas para controlar o fluxo do programa com base em condições específicas.

### Estrutura `if`

A instrução `if` é usada para executar um bloco de código se uma condição específica for verdadeira.

~~~dart
void main() {
  int idade = 18;

  if (idade >= 18) {
    print('Você é maior de idade.');
  }
}
~~~

### Estrutura `if-else`

A instrução `if-else` permite executar um bloco de código se a condição for verdadeira e outro bloco de código se a condição for falsa.

~~~dart
void main() {
  int idade = 16;

  if (idade >= 18) {
    print('Você é maior de idade.');
  } else {
    print('Você é menor de idade.');
  }
}
~~~

### Estrutura `else If`

A instrução `else if` permite testar múltiplas condições. Se a primeira condição for falsa, a próxima condição é testada, e assim por diante.

~~~dart
void main() {
  int nota = 85;

  if (nota >= 90) {
    print('Nota A');
  } else if (nota >= 80) {
    print('Nota B');
  } else if (nota >= 70) {
    print('Nota C');
  } else {
    print('Nota F');
  }
}
~~~

### Operador Ternário

O operador ternário `? :` é uma forma concisa de escrever uma expressão if-else. Ele é usado para retornar um valor com base em uma condição.

~~~dart
void main() {
  int idade = 20;

  String mensagem = (idade >= 18) ? 'Você é maior de idade.' : 'Você é menor de idade.';
  print(mensagem);
}
~~~

### Estrutura `switch-case`

O `switch-case` é usado para executar um bloco de código com base no valor de uma expressão. É uma alternativa mais legível ao `else if` quando há muitas condições a serem verificadas.

~~~dart
void main() {
  String cor = 'vermelho';

  switch (cor) {
    case 'vermelho':
      print('A cor é vermelha.');
      break;
    case 'azul':
      print('A cor é azul.');
      break;
    case 'verde':
      print('A cor é verde.');
      break;
    default:
      print('Cor desconhecida.');
  }
}
~~~

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

### Verificar Existência de Elementos

É possível fazer diversas verificações acerca de uma lista. O método `contains` verifica se a lista possui um determinado elemento.

~~~dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.contains('banana')); // Output: true
}
~~~

Os métodos `isEmpty` e `isNotEmpty` verificam se a lista está ou não vazia.

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

### Criar uma String a partir da Lista

O método `join` combina todos os elementos da lista em uma única string, separando-os por um delimitador especificado.

```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  print(frutas.join(', ')); // Output: maçã, banana, laranja
  print(frutas.join(' | ')); // Output: maçã | banana | laranja
  print(frutas.join(' -> ')); // Output: maçã -> banana -> laranja
}
```

## Iterar sobre Listas

Em Dart, pode-se iterar sobre listas de várias maneiras, utilizando diferentes métodos e técnicas de iteração.

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
  List<int> numeros = [1, 2, 3, 4, 5];
  
  var menoresQueQuatro = numeros.takeWhile((numero) => numero < 4).toList();
  print(menoresQueQuatro); // Output: [1, 2, 3]
  
  var aPartirDeQuatro = numeros.skipWhile((numero) => numero < 4).toList();
  print(aPartirDeQuatro); // Output: [4, 5]
}
~~~

<!--
~~~dart

~~~
-->
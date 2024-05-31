<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="banner.png"></a>
</div>
<br>

# Linguagem Dart

Versão: 3.4

- [Criando um novo projeto](#criando-um-novo-projeto)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Tipos de dados](#tipos-de-dados)
- [Conversão entre Tipos](#conversão-entre-tipos)
- [Declaração de variáveis](#declaração-de-variáveis)
- [Usando Null Safety com Variáveis](#usando-null-safety-com-variáveis)
- [Operadores de Comparação](#operadores-de-comparação)
- [Operadores Lógicos](#operadores-lógicos)
- [Estruturas Condicionais](#estruturas-condicionais)
- [Estruturas de Repetição](#estruturas-de-repetição)
- [Listas (Arrays)](#listas-arrays)
- [Iterar sobre Listas](#iterar-sobre-listas)
- [Usando Null Safety com Listas](#usando-null-safety-com-listas)
- [Sets](#sets)
- [Iterar sobre Sets](#iterar-sobre-sets)
- [Usando Null Safety com Sets](#usando-null-safety-com-sets)
- [Maps](#maps)
- [Funções](#funções)
- [Exceções (Exceptions)](#exceções-exceptions)
- [Importações (Imports)](#importações-imports)

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

### List (Listas)

`List` é uma coleção ordenada de objetos. Pode ser homogênea (todos os elementos do mesmo tipo) ou heterogênea.

~~~dart
List<int> numeros = [1, 2, 3, 4, 5];
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<dynamic> misturado = [1, 'dois', 3.0, true];
~~~

### Map

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

### Set

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

## Conversão entre Tipos

No Dart, é comum precisar converter entre diferentes tipos de dados, como entre inteiros, strings e números de ponto flutuante. Dart oferece várias maneiras de realizar essas conversões de forma segura e eficiente.

### Convertendo `String` para `int` e `double`

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

### Convertendo `int` e `double` para `String`

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

### Convertendo `List` para `Set` e `Map`

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

### Convertendo `Set` e `Map` para `List`

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

## Usando Null Safety com Variáveis

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

### Operador de Asserção Não Nula `!`

Se existe certeza de que uma variável nulável não é `null` em um determinado ponto do código, pode-se usar o operador de asserção não nula `!` para forçar o compilador a tratar a variável como não nula. Deve ser usado com cautela, pois se a variável for `null`, ocorrerá uma exceção em tempo de execução.

~~~dart
String? nomeNulavel = 'Dart';
int tamanho = nomeNulavel!.length; // Se nomeNulavel for null, lançará uma exceção
~~~

## Operadores de Comparação

Os operadores de comparação em Dart são utilizados para comparar valores. Eles retornam um valor booleano (`true` ou `false`) com base na comparação realizada. São eles:

| Símbolo | Significado                |
|---------|----------------------------|
| `==`    | Igualdade                  |
| `!=`    | Desigualdade               |
| `>`     | Maior que                  |
| `<`     | Menor que                  |
| `>=`    | Maior ou igual que         |
| `<=`    | Menor ou igual que         |

Os operadores funcionam da seguinte maneira:

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

## Estruturas de Repetição

Em Dart, existem várias estruturas de repetição (ou laços) que permitem iterar sobre blocos de código múltiplas vezes. As principais estruturas de repetição em Dart são `for`, `for-in`, `while`, e `do-while`.

### Loop `for`

O `for` é usado quando sabe-se de antemão quantas vezes deseja-se executar um bloco de código. Ele consiste em três partes: inicialização, condição e incremento.

```dart
void main() {
  for (int i = 0; i < 5; i++) {
    print('Iteração $i');
  }
}
```

### Loop `for-in`

O `for-in` é usado para iterar sobre elementos de uma coleção (como listas, sets ou maps) de forma mais simples e legível.


```dart
void main() {
  List<String> frutas = ['maçã', 'banana', 'laranja'];
  for (var fruta in frutas) {
    print(fruta);
  }
}
```

### Loop `while`

O `while` é usado quando não se sabe de antemão quantas vezes o bloco de código deve ser executado. Ele continua executando enquanto a condição especificada for verdadeira.

```dart
void main() {
  int contador = 0;

  // Imprimir impares menores que 10
  while (contador < 10) {
    if (contador % 2 != 0) {
      print(contador);
    }
    contador++;
  }
}
```

### Loop `do-while`

O `do-while` é semelhante ao `while`, mas a condição é avaliada após a execução do bloco de código, garantindo que o bloco seja executado pelo menos uma vez.

```dart
void main() {
  int contador = 5;

  do {
    print(contador);
    contador++;
  } while (contador < 5);
}
```

### Comando `break` e `continue`

Os comandos `break` e `continue` são utilizados para interromper alguma ação em um loop. O `continue` interrompe a iteração atual e pula para a próxima, enquanto que o `break` interrompe a execução do loop em si.

```dart
void main() {
  for (int i = 0; i < 5; i++) {
    if (i == 2) {
      continue; // Pula a iteração quando i é 2
    }
    if (i == 4) {
      break; // Interrompe o loop quando i é 4
    }
    print('Iteração $i');
  }
}
```

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
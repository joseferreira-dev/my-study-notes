<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Tipos de Dados

- [Introdução](#introdução)
- [Tipos de Dados mais Comuns](#tipos-de-dados-mais-comuns)
  * [Números](#números)
  * [Números Inteiros](#números-inteiros)
  * [Números de Ponto Flutuante](#números-de-ponto-flutuante)
  * [String](#string)
  * [Runes](#runes)
  * [Booleanos](#booleanos)
  * [Iterable (Iteráveis)](#iterable-iteráveis)
  * [List (Listas)](#list-listas)
  * [Set](#set)
  * [Map](#map)
  * [Symbol (Símbolos)](#symbol-símbolos)
  * [Null](#null)
- [Tipos de Dados Complexos](#tipos-de-dados-complexos)
  * [Object](#object)
  * [dynamic](#dynamic)
  * [Function](#function)
  * [Future](#future)
  * [Stream](#stream)
  * [void](#void)
  * [Never](#never)

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

## Tipos de Dados Complexos

Dart ambém possui alguns tipos de dados mais complexos, que são empregados em situações mais específicas, como quando se está lidando com programação assíncrona e genérica.

### Object

O tipo `Object` é a classe raiz de todas as classes em Dart. Todo objeto é uma instância de `Object`, incluindo números, strings, listas, e até mesmo null (se bem que null é uma instância especial).

```dart
void main() {
  Object qualquerCoisa = 42;
  print(qualquerCoisa); // Output: 42
  qualquerCoisa = 'Texto';
  print(qualquerCoisa); // Output: Texto
}
```

### dynamic

O tipo `dynamic` desativa a verificação de tipo em tempo de compilação, permitindo que qualquer tipo de valor seja atribuído a uma variável. É útil quando o tipo de uma variável não é conhecido até o tempo de execução.

```dart
void main() {
  dynamic variavel = 42;
  print(variavel); // Output: 42
  variavel = 'Texto';
  print(variavel); // Output: Texto
}
```

### Function

O tipo `Function` é a superclasse de todas as funções em Dart. Pode ser usado para armazenar e passar funções como valores.

```dart
void executarOperacao(Function operacao, int a, int b) {
  print(operacao(a, b));
}

int soma(int a, int b) => a + b;
int multiplicacao(int a, int b) => a * b;

void main() {
  executarOperacao(soma, 3, 4); // Output: 7
  executarOperacao(multiplicacao, 3, 4); // Output: 12
}
```

### Future

O tipo `Future` representa um valor ou erro que estará disponível em algum momento no futuro. Futures são frequentemente usados para operações assíncronas, como E/S (entrada/saída) de arquivos ou solicitações de rede.

```dart
Future<String> obterDados() async {
  return Future.delayed(Duration(seconds: 2), () => 'Dados recebidos');
}

void main() async {
  print('Esperando dados...');
  String dados = await obterDados();
  print(dados);
  // Output:
  // Esperando dados...
  // Dados recebidos
}
```

### Stream

O tipo `Stream` é uma sequência assíncrona de eventos. Pode ser usada para manipular fluxos de dados contínuos, como eventos de interface do usuário ou dados recebidos de uma conexão de rede.


```dart
Stream<int> contar() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() async {
  await for (int valor in contar()) {
    print(valor); // Output: 1 2 3 4 5 (um por segundo)
  }
}
```

### void

O tipo `void` indica que uma função não retorna um valor. É usado principalmente para funções que executam ações mas não produzem um resultado que precisa ser utilizado.

```dart
void mostrarMensagem(String mensagem) {
  print(mensagem);
}

void main() {
  mostrarMensagem('Olá, mundo!'); // Output: Olá, mundo!
}
```

### Never

O tipo `Never` indica que uma função nunca retorna, ou seja, ela termina lançando uma exceção ou executando um loop infinito. É útil para funções que sempre resultam em um erro ou comportamento indefinido.

```dart
Never lancarErro(String mensagem) {
  throw Exception(mensagem);
}

void main() {
  try {
    lancarErro('Ocorreu um erro!');
  } catch (e) {
    print(e); // Output: Exception: Ocorreu um erro!
  }
}
```

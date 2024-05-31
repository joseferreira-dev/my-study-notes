<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Funções

- [Introdução](#introdução)
- [Definição de Funções](#definição-de-funções)
- [Funções com Parâmetros](#funções-com-parâmetros)
- [Funções com Parâmetros Opcionais](#funções-com-parâmetros-opcionais)
- [Funções Anônimas (Lambdas)](#funções-anônimas-lambdas)
- [Arrow Functions](#arrow-functions)
- [Funções como Cidadãos de Primeira Classe](#funções-como-cidadãos-de-primeira-classe)
- [Funções Assíncronas](#funções-assíncronas)

## Introdução

Funções são blocos de código que realizam tarefas específicas. Em Dart, funções são elementos fundamentais que permitem a reutilização de código e a organização lógica de tarefas. 

## Definição de Funções

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

## Funções com Parâmetros

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

## Funções com Parâmetros Opcionais

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

## Funções Anônimas (Lambdas)

Funções anônimas ou lambdas são funções que não têm nome. São frequentemente usadas em funções de ordem superior, como em coleções.

```dart
void main() {
  var lista = [1, 2, 3];
  lista.forEach((item) {
    print(item); // Output: 1, 2, 3
  });
}
```

## Arrow Functions

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

## Funções como Cidadãos de Primeira Classe

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

## Funções Assíncronas

Para operações assíncronas, você pode definir funções usando `async` e `await`.

```dart
Future<String> saudacao() async {
  return "Olá, mundo!";
}

void main() async {
  print(await saudacao()); // Output: Olá, mundo!
}
```

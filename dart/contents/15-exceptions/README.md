<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Exceções (Exceptions)

- [Introdução](#introdução)
- [Lançando Exceções](#lançando-exceções)
- [Capturando Exceções](#capturando-exceções)
- [Usando Bloco Finally](#usando-bloco-finally)
- [Exceções Personalizadas](#exceções-personalizadas)
- [Tratamento Específico de Exceções](#tratamento-específico-de-exceções)

## Introdução

Em Dart, exceções são usadas para sinalizar e tratar erros ou condições inesperadas que ocorrem durante a execução do programa. Exceções permitem que você separe a lógica de tratamento de erro do fluxo normal do código, tornando-o mais robusto e legível.

## Lançando Exceções

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

## Capturando Exceções

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

## Usando Bloco Finally

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

## Exceções Personalizadas

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

## Tratamento Específico de Exceções

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
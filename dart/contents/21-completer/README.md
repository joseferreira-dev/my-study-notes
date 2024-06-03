<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Concorrência - Parte 3: Completer

- [Introdução](#introdução)
- 

## Introdução

O `Completer` em Dart é uma ferramenta usada para criar e completar manualmente um `Future`. Enquanto `Future` é usado para representar valores assíncronos, o `Completer` permite controlar explicitamente quando um `Future` associado deve ser completado, seja com um valor ou com um erro. Isso é útil em situações onde você precisa de mais controle sobre quando e como um `Future` é completado.

## Como Usar um Completer

Inicialmente, deve-se criar uma instância de `Completer`. Em seguida, se obtém o `Future` associado ao `Completer` usando a propriedade `future`. Depois, completa-se o `Future` manualmente chamando `complete` ou `completeError` no `Completer`.

Aqui está um exemplo simples que demonstra o uso de `Completer`:

```dart
import 'dart:async';

void main() {
  // Cria um Completer
  Completer<String> completer = Completer<String>();

  // Obtém o Future associado
  Future<String> future = completer.future;

  // Registra callbacks no Future
  future.then((value) {
    print('Future completado com valor: $value');
  }).catchError((error) {
    print('Future completado com erro: $error');
  });

  // Completa o Future manualmente com um valor
  completer.complete('Olá, Dart!');

  // Você também pode completar com um erro usando:
  // completer.completeError('Algo deu errado!');
}
```

## Usos Comuns do Completer

O `Completer` é frequentemente usado para integrar APIs baseadas em callbacks com APIs baseadas em `Future`.

```dart
import 'dart:async';

void main() {
  Future<String> fetchData() {
    Completer<String> completer = Completer<String>();

    // Simulando uma API de callback
    someAsyncOperation((result, error) {
      if (error != null) {
        completer.completeError(error);
      } else {
        completer.complete(result);
      }
    });

    return completer.future;
  }
}

void someAsyncOperation(Function(String result, String? error) callback) {
  // Simulando uma operação assíncrona
  Future.delayed(Duration(seconds: 2), () {
    callback('Dados recebidos!', null);
  });
}
```

O `Completer` também é muito utilizado para sincronizar várias tarefas assíncronas.

```dart
import 'dart:async';

Future<void> syncOperations() async {
  Completer<void> completer1 = Completer<void>();
  Completer<void> completer2 = Completer<void>();

  // Simulando operações assíncronas
  Future.delayed(Duration(seconds: 2), () {
    print('Operação 1 completada');
    completer1.complete();
  });

  Future.delayed(Duration(seconds: 3), () {
    print('Operação 2 completada');
    completer2.complete();
  });

  // Aguarda ambas as operações serem completadas
  await Future.wait([completer1.future, completer2.future]);
  print('Todas as operações foram completadas');
}

void main() async {
  await syncOperations();
  // Output:
  // Operação 1 completada
  // Operação 2 completada
  // Todas as operações foram completadas
}
```

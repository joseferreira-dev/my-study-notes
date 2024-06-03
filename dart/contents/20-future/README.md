<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Concorrência - Parte 2: Future (Futuros)

- [Introdução](#introdução)
- [Criando um Future](#criando-um-future)
  * [Utilizando `then()`](#utilizando-then)
  * [Utilizando `catchError()`](#utilizando-catcherror)
  * [Utilizando `whenComplete()`](#utilizando-whencomplete)
- [Encadeando Futuros](#encadeando-futuros)
- [Variações de Future](#variações-de-future)
  * [`Future.sync()`](#futuresync)
  * [`Future.microtask()`](#futuremicrotask)
  * [`Future.value()`](#futurevalue)
  * [`Future.error()`](#futureerror)
  * [`Future.wait()`](#futurewait)
  * [`Future.any()`](#futureany)
- [Registrando Tempo de Execução de um Future](#registrando-tempo-de-execução-de-um-future)

## Introdução

Os `Future` em Dart são uma maneira de representar valores ou erros que estarão disponíveis em algum momento no futuro. Eles são essenciais para trabalhar com operações assíncronas, como chamadas de rede, leitura de arquivos, ou qualquer operação que pode demorar para completar.

## Criando um Future

Inicialmente, deve-se importar a biblioteca `dart:async`. Um `Future` pode ser criado usando o construtor `Future` ou utilizando métodos como `Future.value`, `Future.error`, e `Future.delayed`.

```dart
import 'dart:async';

// Utilizando construtor Future
Future<int> someAsyncFunction() async {
  return 42;
}


void main() {
  print('Início');

  // Utilizando Future.delayed
  Future.delayed(Duration(seconds: 5), () => print('Executando...'));

  print('Final');
  // Output:
  // Início
  // Final
  // Executando...
}
```

### Utilizando `then()`

O método `then()` é usado para registrar callbacks que serão executados quando o `Future` for completado com sucesso (ou seja, quando o valor estiver disponível).

```dart
import 'dart:async';

void main() {
  Future<int> future = Future.delayed(Duration(seconds: 3), () {
    print('Executando...');
    return 42;
   });
  
  future
    .then((value) {
      print('O valor é $value');
    });
  // Output (depois de 3 segundos):
  // Executando...
  // O valor é 42
}
```

### Utilizando `catchError()`

O método `catchError()` é usado para registrar callbacks que serão executados se o `Future` for completado com um erro.

```dart
import 'dart:async';

void main() {
  Future<int> futureWithError = Future.error('Algo deu errado');
  
  futureWithError
    .catchError((error) {
      print('Erro: $error');
    });
  // Output: Não compila com a mensagem => Erro: Algo deu errado 
}
```

### Utilizando `whenComplete()`

O método `whenComplete()` é usado para registrar callbacks que serão executados independentemente de o `Future` ser completado com sucesso ou com erro. Ele é útil para realizar ações de limpeza ou conclusão.

```dart
import 'dart:async';

void main() {
  Future<int> future = Future.delayed(Duration(seconds: 3), () {
    print('Executando...');
    return 42;
   });
  
  future
    .then((value) {
      print('O valor é $value');
    })
    .whenComplete(() {
      print('Future completado');
    });
  // Output (depois de 3 segundos):
  // Executando...
  // O valor é 42
  // Future completado
}
```

## Encadeando Futuros

É possível encadear múltiplos `then()`, `catchError()`, e `whenComplete()` para criar uma sequência de operações assíncronas.

```dart
import 'dart:async';

void main() {
  Future<int> future = Future.delayed(Duration(seconds: 1), () => 42);
  
  future
    .then((value) {
      print('O valor é $value');
      return value * 2;
    })
    .then((newValue) {
      print('O novo valor é $newValue');
    })
    .catchError((error) {
      print('Erro: $error');
    })
    .whenComplete(() {
      print('Future completado');
    });
  // Output (depois de 3 segundos):
  // Executando...
  // O valor é 42
  // O novo valor é 84
  // Future completado
}
```

Um exemplo prático pode ser aplicado como no código a seguir, que retorna o valor dos segundos do horário atual se este for par e retorna um erro caso contrátio:

```dart
import 'dart:async';

void main() {
  Future<int> future = Future.delayed(Duration(seconds: 2), () {
    if (DateTime.now().second % 2 == 0) {
      return DateTime.now().second; // Sucesso
    } else {
      throw 'Erro inesperado'; // Erro
    }
  });

  future
    .then((value) {
      print('O valor é $value');
    })
    .catchError((error) {
      print('Erro: $error');
    })
    .whenComplete(() {
      print('Future completado');
    });
}
```

## Variações de Future

Dart oferece diversas variações da classe `Future` para lidar com diferentes necessidades de operações assíncronas. Aqui está uma explicação detalhada de cada uma dessas variações:

### `Future.sync()`

`Future.sync()` cria um `Future` que é completado sincronicamente, ou seja, ele executa o código imediatamente e retorna um `Future` completado com o valor ou erro resultante. Isso é útil quando se quer garantir que o código dentro do `Future` seja executado sincronicamente e ainda assim fornecer uma interface de `Future`.

```dart
import 'dart:async';

void main() {
  Future<int> syncFuture = Future.sync(() {
    return 42; // Valor retornado imediatamente
  });

  syncFuture
    .then((value) {
      print('O valor é $value');
    }); 
}
```

### `Future.microtask()`

`Future.microtask()` cria um `Future` que é completado no próximo ciclo de microtask do Event Loop. Isso é útil para garantir que o código seja executado assíncronamente, mas ainda antes de qualquer evento da fila de eventos.

```dart
import 'dart:async';

void main() {
  Future<int> microtaskFuture = Future.microtask(() {
    return 42; // Executado como microtask
  });

  microtaskFuture
    .then((value) {
      print('O valor é $value');
    });
}
```

### `Future.value()`

`Future.value()` cria um `Future` completado imediatamente com o valor fornecido. É útil para criar um `Future` que se sabe que já têm um valor.

```dart
import 'dart:async';

void main() {
  Future<int> valueFuture = Future.value(42);

  valueFuture
    .then((value) {
      print('O valor é $value');
    });
}
```

### `Future.error()`

`Future.error()` cria um `Future` completado imediatamente com o erro fornecido. É útil para criar um `Future` que se sabe que já têm um erro.

```dart
import 'dart:async';

void main() {
  Future<int> errorFuture = Future.error('Algo deu errado');

  errorFuture
    .catchError((error) {
      print('Erro: $error');
    });
}
```

### `Future.wait()`

`Future.wait()` aguarda a conclusão de múltiplos `Future` e retorna um novo `Future` que é completado com uma lista de resultados de todos os `Future` fornecidos. Pode-se usar `eagerError` e `cleanUp` para controlar o comportamento em caso de erro.

Se `eagerError` é definido como `true`, o `Future` retornado é completado com o primeiro erro encontrado, em vez de esperar que todos os `Future` sejam completados. Já `cleanUp` é uma função opcional que é chamada para cada `Future` que não foi completado quando ocorre um erro em um dos `Future`.

```dart
import 'dart:async';

void main() {
  Future<int> future1 = Future.delayed(Duration(seconds: 1), () => 1);
  Future<int> future2 = Future.delayed(Duration(seconds: 2), () => 2);
  Future<int> future3 = Future.delayed(Duration(seconds: 3), () => throw 'Erro future3');

  Future
    .wait([future1, future2, future3], eagerError: true)
    .then((values) {
      print('Valores: $values');
    })
    .catchError((error) {
      print('Erro: $error');
    },test: (error) => error is String);
    // Output: Erro: Erro future3
}
```

### `Future.any()`

`Future.any()` retorna um `Future` que é completado com o resultado do primeiro `Future` que é completado entre uma lista de `Future`. É útil quando se deseja o resultado do primeiro `Future` completado, independentemente de qual seja.

```dart
import 'dart:async';

void main() {
  Future<int> future1 = Future.delayed(Duration(seconds: 2), () => 1);
  Future<int> future2 = Future.delayed(Duration(seconds: 1), () => 2);

  Future.any([future1, future2])
    .then((value) {
      print('Primeiro valor completado: $value');
    });
    // Output: Primeiro valor completado: 2
}
```

## Registrando Tempo de Execução de um Future

Para registrar o tempo de execução de um `Future`, pode-se utilizar a classe `Stopwatch` da biblioteca `dart:core`. A ideia é iniciar o `Stopwatch` antes de começar a execução do `Future` e parar o `Stopwatch` quando o `Future` for completado. Dessa forma, é possível calcular e registrar o tempo de execução.

```dart
import 'dart:async';

void main() async {
  // Cria uma instância de Stopwatch
  Stopwatch stopwatch = Stopwatch();

  // Inicia o Stopwatch
  stopwatch.start();

  // Cria um Future que demora 2 segundos para completar
  Future<int> future = Future.delayed(Duration(seconds: 2), () {
    return 42;
  });

  // Aguarda a conclusão do Future
  int? result;
  try {
    result = await future;
  } catch (e) {
    print('Erro: $e');
  } finally {
    // Para o Stopwatch
    stopwatch.stop();
  }

  // Registra o tempo de execução
  print('Resultado: $result');
  print('Tempo de execução: ${stopwatch.elapsedMilliseconds} ms');
}
```

Um exemplo diferente pode ser aplicado considerando-se um `Future` que pode resultar em um erro, usando `then`, `catchError` e `whenComplete`:

```dart
void main() {
  Stopwatch stopwatch = Stopwatch();

  stopwatch.start();

  Future<int> future = Future.delayed(Duration(seconds: 2), () {
    return 42;
  });

  future
    .then((result) {
      print('Resultado: $result');
    })
    .catchError((error) {
      print('Erro: $error');
    })
    .whenComplete(() {
      stopwatch.stop();
      print('Tempo de execução: ${stopwatch.elapsedMilliseconds} ms');
    });
}
```

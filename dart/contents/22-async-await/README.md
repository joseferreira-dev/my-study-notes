<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Concorrência - Parte 4: Async/Await

- [Introdução](#introdução)
- [O que é `Async`/`Await`?](#o-que-é-asyncawait)
- [Como Definir uma Função `async`/`await`](#como-definir-uma-função-asyncawait)
- [Tratando Erros em Funções Assíncronas com `try` e `catch`](#tratando-erros-em-funções-assíncronas-com-try-e-catch)
- [Iterações com `async`/`await`](#iterações-com-asyncawait)

## Introdução

A programação assíncrona com `async`/`await` em Dart simplifica o trabalho com operações assíncronas, permitindo escrever código assíncrono de maneira mais linear e fácil de ler.

## O que é `Async`/`Await`?

Em Dart, `async` e `await` são palavras-chave que facilitam a execução de operações assíncronas. `async` é usada para definir uma função assíncrona, e `await` é usada para pausar a execução dessa função até que um `Future` seja completado.

- **`async`**: Marca uma função como assíncrona. Uma função marcada com `async` sempre retorna um `Future`.
- **`await`**: Pausa a execução da função assíncrona até que o Future seja completado. A função só continua a execução após o Future ser resolvido.

## Como Definir uma Função `async`/`await`

Para definir uma função assíncrona, se adiciona a palavra-chave `async` após a assinatura da função. Dentro dessa função, usa-se `await` para esperar a conclusão de `Future`.

```dart
import 'dart:async';

Future<void> fetchData() async {
  print('Iniciando a busca de dados...');
  await Future.delayed(Duration(seconds: 2)); // Simulando uma operação assíncrona
  print('Dados buscados com sucesso!');
}

void main() async {
  await fetchData();
  print('Finalizado.');
  // Output:
  // Iniciando a busca de dados...
  // Dados buscados com sucesso!
  // Finalizado.
}
```

## Tratando Erros em Funções Assíncronas com `try` e `catch`

Para tratar erros em funções assíncronas, pode-se utilizar o bloco `try-catch-finally`. Isso permite capturar e lidar com exceções que podem ser lançadas durante a execução do código assíncrono.


```dart
import 'dart:async';

Future<void> fetchData() async {
  try {
    print('Iniciando a busca de dados...');
    await Future.delayed(Duration(seconds: 2));
    // Simulando um erro
    throw Exception('Erro ao buscar dados!');
  } catch (e) {
    print('Erro capturado: $e');
  } finally {
    print('Finalizando try/catch.');
  }
}

void main() async {
  await fetchData();
  print('Finalizado.');
  // Output:
  // Iniciando a busca de dados...
  // Dados buscados com sucesso!
  // Erro capturado: Exception: Erro ao buscar dados!
  // Finalizado.
}
```

## Iterações com `async`/`await`

É possível usar `async`/`await` dentro de loops para executar operações assíncronas sequencialmente. Isso é útil quando se precisa processar uma série de operações assíncronas uma após a outra.

```dart
import 'dart:async';

Future<void> fetchData(int id) async {
  await Future.delayed(Duration(seconds: 1));
  print('Dados do item $id buscados com sucesso!');
}

Future<void> fetchAllData() async {
  for (int i = 1; i <= 3; i++) {
    await fetchData(i);
  }
}

void main() async {
  await fetchAllData();
  print('Todos os dados foram buscados.');
  // Output:
  // Dados do item 1 buscados com sucesso!
  // Dados do item 2 buscados com sucesso!
  // Dados do item 3 buscados com sucesso!
  // Todos os dados foram buscados.
}
```

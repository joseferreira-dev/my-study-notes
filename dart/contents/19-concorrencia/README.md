<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Concorrência - Parte 1: Introdução

- [Introdução](#introdução)
- [Event Loop (Laço de Eventos)](#event-loop-laço-de-eventos)
- [Funcionamento do Event Loop](#funcionamento-do-event-loop)

## Introdução

A concorrência em Dart é baseada em um modelo de execução single-thread, o que significa que todo o código Dart é executado em uma única thread de execução. No entanto, Dart utiliza um sistema de gerenciamento de eventos e microtasks para permitir a execução assíncrona e concorrente de código, tornando possível a realização de operações sem bloquear a thread principal.

## Event Loop (Laço de Eventos)

O Event Loop é o mecanismo central que gerencia a execução do código assíncrono em Dart. Ele funciona em um ciclo contínuo, verificando e processando eventos de diferentes fontes. O Event Loop lida com duas filas principais:

- **Fila de Events (Event Queue)**: Contém eventos externos, como cliques de botão, timers, I/O de rede, etc. Quando o Event Loop detecta que a thread principal está ociosa, ele pega o próximo evento da fila de eventos e o processa.
- **Fila de Microtasks (Microtask Queue)**: Contém tarefas internas de alta prioridade que devem ser executadas antes dos eventos da fila de eventos. As microtasks são geralmente geradas pelo próprio código Dart, como as `Future` e `Stream`.

Ressaltando-se a diferença entre os elementos dessas duas filas:

- **Microtasks**: São pequenas tarefas que devem ser executadas o mais rápido possível, antes que o Event Loop volte a processar eventos. Exemplos de microtasks incluem callbacks de `Future.then()` e resoluções de `Completer`.
- **Events**: São tarefas mais gerais que podem incluir interações do usuário, eventos de rede, timers, etc. Eles são processados após todas as microtasks terem sido executadas.

## Funcionamento do Event Loop

O funcionamento Event Loop segue a seguinte lógica de execução:

1. **Executa código síncrono**: A execução começa com o código síncrono no main.
2. **Microtasks**: Após executar o código síncrono, o Event Loop verifica a fila de microtasks. Se houver microtasks na fila, elas são executadas antes de passar para a fila de eventos.
3. **Events**: Se a fila de microtasks estiver vazia, o Event Loop processa o próximo evento na fila de eventos.
4. **Repetição**: O Event Loop continua esse ciclo, alternando entre microtasks e eventos, até que todas as tarefas sejam concluídas.

Aqui está um exemplo simples que demonstra o comportamento do Event Loop em Dart:

```dart
import 'dart:async';

void main() {
  print('Início do main');

  Future(() {
    print('Future #1');
  });

  Future(() {
    print('Future #2');
  }).then((_) {
    print('Future #2 then');
  });

  scheduleMicrotask(() {
    print('Microtask #1');
  });

  print('Fim do main');
}
```

Saída esperada:

```
Início do main
Fim do main
Microtask #1
Future #1
Future #2
Future #2 then
```

Como pode ser visto no exemplo, `Início do main` e `Fim do main` são impressos primeiro porque são parte do código síncrono. `Microtask #1` é impresso antes dos eventos futuros porque as microtasks têm prioridade sobre os eventos. `Future #1` e `Future #2` são impressos em seguida quando as microtasks estão esvaziadas. `Future #2 then` é impresso por último, pois é uma microtask enfileirada pelo callback do `then` do segundo `Future`. Esse modelo permite que Dart seja eficiente e responsivo, especialmente em ambientes onde a latência e a responsividade são importantes, como aplicações web e móveis.


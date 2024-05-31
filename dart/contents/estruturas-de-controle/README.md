<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Estruturas de Controle

- [Introdução](#introdução)
- [Estruturas Condicionais](#estruturas-condicionais)
  - [Estrutura `if`](#estrutura-if)
  - [Estrutura `if-else`](#estrutura-if-else)
  - [Estrutura `else if`](#estrutura-else-if)
  - [Operador Ternário `? :`](#operador-ternário--)
  - [Estrutura `switch-case`](#estrutura-switch-case)
- [Estruturas de Repetição](#estruturas-de-repetição)
  - [Loop `for`](#loop-for)
  - [Loop `for-in`](#loop-for-in)
  - [Loop `while`](#loop-while)
  - [Loop `do-while`](#loop-do-while)
  - [Comandos `break` e `continue`](#comandos-break-e-continue)

## Introdução

As estruturas de controle são fundamentais em qualquer linguagem de programação, pois permitem controlar o fluxo de execução do código com base em condições ou repetição de tarefas. Em Dart, existem várias estruturas de controle que podem ser usadas para fazer decisões, repetir ações e interromper o fluxo de execução.

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

### Estrutura `else if`

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

### Operador Ternário `? :`

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

### Comandos `break` e `continue`

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
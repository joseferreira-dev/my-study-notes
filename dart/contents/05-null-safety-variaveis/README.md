<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Usando Null Safety com Variáveis

- [Introdução](#introdução)
- [Tipos Não Nulos por Padrão](#tipos-não-nulos-por-padrão)
- [Tipos Nuláveis](#tipos-nuláveis)
- [Operador de Atribuição Condicional `??`](#operador-de-atribuição-condicional-)
- [Operador de Acesso Seguro `?.`](#operador-de-acesso-seguro-)
- [Operador de Asserção Não Nula `!`](#operador-de-asserção-não-nula-)

## Introdução

Null safety é um recurso introduzido no Dart 2.12 para ajudar a evitar erros comuns relacionados ao uso de valores `null`. Com null safety, o sistema de tipos de Dart ajuda a garantir que valores `null` não sejam utilizados inadvertidamente, prevenindo muitos tipos de exceções em tempo de execução.

## Tipos Não Nulos por Padrão

Por padrão, todos os tipos em Dart são não nulos, o que significa que uma variável não pode conter `null` a menos que explicitamente declarado. Por exemplo:

~~~dart
int numero = 42; // Correto
numero = null;   // Erro de compilação
~~~

## Tipos Nuláveis

Para permitir que uma variável aceite um valor `null`, deve-se usar o operador de interrogação `?` após o tipo. Isso transforma o tipo em um tipo nulável.

~~~dart
int? numero = 42; // Correto
numero = null;    // Correto
~~~

## Operador de Atribuição Condicional `??`

O operador `??` permite fornecer um valor padrão para uma expressão que pode ser `null`.

~~~dart
int? numeroNulavel;
int valor = numeroNulavel ?? 0; // Se numeroNulavel for null, valor será 0
~~~

## Operador de Acesso Seguro `?.`

O operador `?.` permite chamar métodos ou acessar propriedades de um objeto de forma segura, retornando `null` se o objeto for `null`.

~~~dart
String? texto;
int? tamanho = texto?.length; // Se texto for null, tamanho será null
~~~

## Operador de Asserção Não Nula `!`

Se existe certeza de que uma variável nulável não é `null` em um determinado ponto do código, pode-se usar o operador de asserção não nula `!` para forçar o compilador a tratar a variável como não nula. Deve ser usado com cautela, pois se a variável for `null`, ocorrerá uma exceção em tempo de execução.

~~~dart
String? nomeNulavel = 'Dart';
int tamanho = nomeNulavel!.length; // Se nomeNulavel for null, lançará uma exceção
~~~
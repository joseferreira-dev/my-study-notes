<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Declaração de variáveis

- [Introdução](#introdução)
- [Declaração Explícita com Tipo](#declaração-explícita-com-tipo)
- [Declaração com `var`](#declaração-com-var)
- [Declaração com `dynamic`](#declaração-com-dynamic)
- [Declaração com `final` e `const`](#declaração-com-final-e-const)

## Introdução

Em Dart, a declaração de variáveis pode ser feita de várias maneiras, permitindo diferentes níveis de especificidade e flexibilidade. 

## Declaração Explícita com Tipo

Você pode declarar uma variável especificando explicitamente seu tipo. Isso garante que a variável só pode armazenar valores desse tipo.

~~~dart
int idade = 30;
double altura = 1.75;
String nome = 'João';
bool estaChovendo = false;
~~~

## Declaração com `var`

Quando você usa `var`, o tipo da variável é inferido a partir do valor inicial atribuído a ela. Depois que o tipo é inferido, a variável só pode armazenar valores desse tipo.

~~~dart
var idade = 30; // inferido como int
var altura = 1.75; // inferido como double
var nome = 'João'; // inferido como String
var estaChovendo = false; // inferido como bool
~~~

## Declaração com `dynamic`

O tipo `dynamic` permite que a variável armazene valores de qualquer tipo. Isso pode ser útil em situações onde o tipo de dado pode mudar, mas deve ser usado com cautela, pois elimina a verificação de tipo em tempo de compilação.

~~~dart
dynamic algo = 30;
algo = 'João';
algo = true;
~~~

## Declaração com `final` e `const`

Uma variável declarada com `final` pode ser atribuída apenas uma vez. O valor é determinado em tempo de execução. Já uma variável declarada com `const` é uma constante em tempo de compilação e deve ser inicializada com um valor constante.

~~~dart
final int idadeFinal = 30;
final nomeFinal = 'João'; // inferido como String

const double pi = 3.14159;
const nomeConst = 'Constante'; // inferido como String
~~~

A principal diferença entre as duas formas é que o usando `final` o valor é fixo após a atribuição inicial, mas pode ser determinado em tempo de execução. Já com `const` o valor é fixo e deve ser conhecido em tempo de compilação.
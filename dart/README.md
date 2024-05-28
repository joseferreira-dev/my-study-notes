# Linguagem Dart

Versão: 3.4

- [Criando um novo projeto](#criando-um-novo-projeto)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Tipos de dados](#tipos-de-dados)
- [Declaração de variáveis](#declaração-de-variáveis)
- [Usando o Null Safety](#usando-o-null-safety)
- [Estruturas Condicionais](#estruturas-condicionais)
- [Operadores Lógicos](#operadores-lógicos)

## Criando um novo projeto

Para se criar um novo projeto Dart básico pelo console deve-se utilizar o comando:

~~~shell
dart create project_name
~~~

É uma convensão da comunidade separar o nome do projeto com `_`. É possível definir um template inicial para o projeto utilizando a flag `-t`.

~~~shell
dart create -t console console_project_name
~~~

Os possíveis templates são: 

- `console`: Cria um projeto de console simples, ideal para aplicativos de linha de comando.
- `package`: Cria um novo pacote Dart, que pode ser uma biblioteca ou um módulo reutilizável.
- `server-shelf`: Cria um projeto de servidor usando o pacote Shelf, uma estrutura de servidor web minimalista para Dart.
- `web`: Cria um projeto para desenvolvimento web, incluindo um servidor de desenvolvimento e a configuração para trabalhar com Dart no navegador.
- `web-simple`: Cria um projeto web simples, que é uma versão mínima de um projeto web.
- `flutter`: Cria um projeto Flutter, que é uma estrutura para construir aplicativos nativos para iOS, Android, web e desktop com uma única base de código.

## Estrutura do projeto

Quando você cria um projeto de console em Dart usando o comando `dart create -t console console_project`, uma estrutura de diretórios e arquivos é gerada para ajudar a começar com um aplicativo de console simples. A estrutura típica do projeto é a seguinte:

~~~shell
console_project/
├── bin/
│   └── console_project.dart
├── lib/
├── test/
├── .gitignore
├── analysis_options.yaml
├── CHANGELOG.md
├── pubspec.yaml
└── README.md
~~~

- `bin/console_project.dart`: Arquivo principal do aplicativo de console, onde se escreve o código para o seu aplicativo. O nome do arquivo geralmente corresponde ao nome do projeto.
- `lib/`: Pasta usada para armazenar qualquer biblioteca ou código que será compartilhado entre diferentes partes do aplicativo. É útil para organizar componentes maiores.
- `test/`: Pasta que contém arquivos de teste para o projeto.
- `.gitignore`: Arquivo usado para especificar quais arquivos e pastas devem ser ignorados pelo controle de versão Git.
- `analysis_options.yaml`: Arquivo que permite configurar regras de análise estática para o projeto, como linter e regras de estilo de código.
- `CHANGELOG.md`: Arquivo de log de alterações onde se pode documentar as mudanças feitas em cada versão do projeto.
- `pubspec.yaml`: Arquivo de especificação do pacote Dart. Ele define o nome do projeto, a versão, as dependências do projeto, as dependências de desenvolvimento e outras informações de configuração. 
- `README.md`: Arquivo de documentação onde se pode descrever o propósito do projeto, como configurá-lo e usá-lo, e qualquer outra informação relevante.

## Tipos de dados

Dart é uma linguagem fortemente tipada que oferece uma ampla variedade de tipos de dados. Aqui estão os principais tipos de dados em Dart, junto com exemplos para cada um:

### Numbers (Números)

Dart possui dois tipos de números: `int` (representa números inteiros) e `double` (representa números decimais).

~~~dart
int idade = 30;
int ano = 2024;

double altura = 1.75;
double preco = 99.99;
~~~

### String

`String` é usado para representar texto.

~~~dart
String nome = 'João';
String saudacao = "Olá, mundo!";
String textoMultilinha = '''Este é um
texto de múltiplas
linhas.''';
~~~

### Booleans (Booleanos)

`bool` representa valores booleanos: `true` ou `false`.

~~~dart
bool estaChovendo = false;
bool estaAberto = true;
~~~

### Lists (Listas)

`List` é uma coleção ordenada de objetos. Pode ser homogênea (todos os elementos do mesmo tipo) ou heterogênea.

~~~dart
List<int> numeros = [1, 2, 3, 4, 5];
List<String> frutas = ['maçã', 'banana', 'laranja'];
List<dynamic> misturado = [1, 'dois', 3.0, true];
~~~

### Maps

`Map` é uma coleção de pares chave-valor. As chaves e os valores podem ser de qualquer tipo.

~~~dart
Map<String, int> telefones = {
  'João': 123456789,
  'Maria': 987654321
};

Map<int, String> produtos = {
  1: 'Laptop',
  2: 'Mouse',
  3: 'Teclado'
};
~~~

### Sets

`Set` é uma coleção de elementos únicos, sem ordem específica.

~~~dart
Set<int> numerosUnicos = {1, 2, 3, 4, 5};
Set<String> cores = {'vermelho', 'verde', 'azul'};
~~~

### Null

Dart tem suporte para o tipo `Null`, que é usado para representar a ausência de um valor. Variáveis podem ser declaradas como nulas, a menos que sejam explicitamente marcadas como não nulas.

~~~dart
String? nomeNulo = null;
int? numeroNulo = null;
~~~

## Declaração de variáveis

Em Dart, a declaração de variáveis pode ser feita de várias maneiras, permitindo diferentes níveis de especificidade e flexibilidade. 

### Declaração Explícita com Tipo

Você pode declarar uma variável especificando explicitamente seu tipo. Isso garante que a variável só pode armazenar valores desse tipo.

~~~dart
int idade = 30;
double altura = 1.75;
String nome = 'João';
bool estaChovendo = false;
~~~

### Declaração com `var`

Quando você usa `var`, o tipo da variável é inferido a partir do valor inicial atribuído a ela. Depois que o tipo é inferido, a variável só pode armazenar valores desse tipo.

~~~dart
var idade = 30; // inferido como int
var altura = 1.75; // inferido como double
var nome = 'João'; // inferido como String
var estaChovendo = false; // inferido como bool
~~~

### Declaração com `dynamic`

O tipo `dynamic` permite que a variável armazene valores de qualquer tipo. Isso pode ser útil em situações onde o tipo de dado pode mudar, mas deve ser usado com cautela, pois elimina a verificação de tipo em tempo de compilação.

~~~dart
dynamic algo = 30;
algo = 'João';
algo = true;
~~~

### Declaração com `final` e `const`

Uma variável declarada com `final` pode ser atribuída apenas uma vez. O valor é determinado em tempo de execução. Já uma variável declarada com `const` é uma constante em tempo de compilação e deve ser inicializada com um valor constante.

~~~dart
final int idadeFinal = 30;
final nomeFinal = 'João'; // inferido como String

const double pi = 3.14159;
const nomeConst = 'Constante'; // inferido como String
~~~

A principal diferença entre as duas formas é que o usando `final` o valor é fixo após a atribuição inicial, mas pode ser determinado em tempo de execução. Já com `const` o valor é fixo e deve ser conhecido em tempo de compilação.

## Usando o Null Safety

Null safety é um recurso introduzido no Dart 2.12 para ajudar a evitar erros comuns relacionados ao uso de valores `null`. Com null safety, o sistema de tipos de Dart ajuda a garantir que valores `null` não sejam utilizados inadvertidamente, prevenindo muitos tipos de exceções em tempo de execução.

### Tipos Não Nulos por Padrão

Por padrão, todos os tipos em Dart são não nulos, o que significa que uma variável não pode conter `null` a menos que explicitamente declarado. Por exemplo:

~~~dart
int numero = 42; // Correto
numero = null;   // Erro de compilação
~~~

### Tipos Nuláveis

Para permitir que uma variável aceite um valor `null`, deve-se usar o operador de interrogação `?` após o tipo. Isso transforma o tipo em um tipo nulável.

~~~dart
int? numero = 42; // Correto
numero = null;    // Correto
~~~

### Operador de Atribuição Condicional `??`

O operador `??` permite fornecer um valor padrão para uma expressão que pode ser `null`.

~~~dart
int? numeroNulavel;
int valor = numeroNulavel ?? 0; // Se numeroNulavel for null, valor será 0
~~~

### Operador de Acesso Seguro `?.`

O operador `?.` permite chamar métodos ou acessar propriedades de um objeto de forma segura, retornando `null` se o objeto for `null`.

~~~dart
String? texto;
int? tamanho = texto?.length; // Se texto for null, tamanho será null
~~~

### Operador de Aserção Não Nula `!`

Se existe certeza de que uma variável nulável não é `null` em um determinado ponto do código, pode-se usar o operador de asserção não nula `!` para forçar o compilador a tratar a variável como não nula. Deve ser usado com cautela, pois se a variável for `null`, ocorrerá uma exceção em tempo de execução.

~~~dart
String? nomeNulavel = 'Dart';
int tamanho = nomeNulavel!.length; // Se nomeNulavel for null, lançará uma exceção
~~~

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

### Estrutura `else If`

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

### Operador Ternário

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

## Operadores Lógicos

Os operadores lógicos em Dart são usados para combinar expressões booleanas e tomar decisões com base em várias condições. Eles são essenciais para o controle de fluxo em programas.

### Operador E Lógico `&&`

O operador E lógico retorna `true` se ambas as expressões forem verdadeiras. Caso contrário, retorna `false`.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;

  bool resultado = condicao1 && condicao2; // resultado será false
  print('Resultado de condicao1 && condicao2: $resultado'); // Output: false
}
~~~

### Operador OU Lógico `||`

O operador OU lógico retorna `true` se pelo menos uma das expressões for verdadeira. Caso contrário, retorna `false`.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;

  bool resultado = condicao1 || condicao2; // resultado será true
  print('Resultado de condicao1 || condicao2: $resultado'); // Output: true
}
~~~

### Operador NÃO Lógico `!`

O operador NÃO lógico inverte o valor de uma expressão booleana. Se a expressão for `true`, o operador retorna `false`, e vice-versa.

~~~dart
void main() {
  bool condicao = true;

  bool resultado = !condicao; // resultado será false
  print('Resultado de !condicao: $resultado'); // Output: false
}
~~~

### Combinação de Operadores Lógicos

Pode-se combinar múltiplos operadores lógicos para criar expressões mais complexas.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;
  bool condicao3 = true;

  // Combinação de operadores lógicos
  bool resultado = (condicao1 && condicao3) || condicao2; // resultado será true
  print('Resultado da combinação: $resultado'); // Output: true
}
~~~

### Precedência de Operadores Lógicos

Os operadores lógicos têm uma precedência específica que determina a ordem em que são avaliados. Em Dart, a precedência é a seguinte (da mais alta para a mais baixa):

1. `!` (NÃO lógico)
2. `&&` (E lógico)
3. `||` (OU lógico)

Pode-se usar parênteses para alterar a ordem de avaliação e tornar a expressão mais clara.

~~~dart
void main() {
  bool condicao1 = true;
  bool condicao2 = false;
  bool condicao3 = true;

  // Sem parênteses, && tem precedência sobre ||
  bool resultado1 = condicao1 && condicao2 || condicao3; // resultado1 será true

  // Com parênteses, a avaliação é alterada
  bool resultado2 = condicao1 && (condicao2 || condicao3); // resultado2 será false
  print('Resultado sem parênteses: $resultado1'); // Output: true
  print('Resultado com parênteses: $resultado2'); // Output: false
}
~~~
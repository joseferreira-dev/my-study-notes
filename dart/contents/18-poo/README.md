<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Programação Orientada a Objetos - Parte 2

- [Introdução](#introdução)
- [Interfaces](#interfaces)
  * [Definindo e Implementando Interfaces](#definindo-e-implementando-interfaces)
  * [Múltiplas Interfaces](#múltiplas-interfaces)
  * [Interfaces vs. Classes Abstratas](#interfaces-vs-classes-abstratas)
- [Padronizando Ordenações com a Interface Comparable](#padronizando-ordenações-com-a-interface-comparable)
  * [Implementando Comparable](#implementando-comparable)
  * [Ordenação por Múltiplos Critérios](#ordenação-por-múltiplos-critérios)
- [Mixins](#mixins)
  * [Definindo e Usando Mixins](#definindo-e-usando-mixins)
  * [Restringindo Mixins](#restringindo-mixins)
- [Operator Methods (Métodos Operadores)](#operator-methods-métodos-operadores)
  * [Definindo Operator Methods](#definindo-operator-methods)
- [Referência de Memória para Objetos](#referência-de-memória-para-objetos)
  * [Operator Methods Aplicados a `equals` e `hashCode`](#operator-methods-aplicados-a-equals-e-hashcode)
- [Extensions (Extensões)](#extensions-extensões)
  * [Definindo uma Extensão](#definindo-uma-extensão)
  * [Extensões com Genéricos](#extensões-com-genéricos)
  * [Extensões Privadas](#extensões-privadas)
  * [Extensões com Métodos de mesmo Nome](#extensões-com-métodos-de-mesmo-nome)
- [Generics (Genéricos)](#generics-genéricos)
  * [Generics com Métodos](#generics-com-métodos)
  * [Classes e Métodos Genéricos](#classes-e-métodos-genéricos)
  * [Restrições de Tipo em Generics](#restrições-de-tipo-em-generics)
  * [Usando Generics com Collections](#usando-generics-com-collections)
- [Metadados (Annotations)](#metadados-annotations)
  * [Sintaxe de Annotations](#sintaxe-de-annotations)
  * [Criando e Usando Annotations Personalizadas](#criando-e-usando-annotations-personalizadas)
  * [Reflexão e Annotations](#reflexão-e-annotations)

## Introdução

A Programação Orientada a Objetos (POO) é um paradigma de programação que organiza o software em torno de objetos, que podem conter dados (campos/propriedades) e métodos (funções). Em Dart, a POO é amplamente suportada e facilita a criação de código modular e reutilizável.

## Interfaces

Interfaces são uma maneira de definir um contrato que uma classe deve seguir. Embora Dart não tenha uma palavra-chave específica para interfaces como outras linguagens de programação, qualquer classe abstrata pode ser usada como uma interface. Em Dart, uma interface é definida implicitamente por uma classe abstrata, e outras classes podem implementar essa interface usando a palavra-chave `implements`. Os conceitos básicos de interfaces em Dart são:

- **Contrato**: Uma interface define um conjunto de métodos e propriedades que uma classe deve implementar.
- **Classe Interface**: Qualquer classe pode servir como uma interface.
- **Implementação**: Uma classe que implementa uma interface deve fornecer implementações concretas para todos os métodos e propriedades da interface.

### Definindo e Implementando Interfaces

O exemplo a seguir mostra a utilização básica de uma interface. Inicialmente define-se a classe abstrata `Desenhavel`, que implementa o método abstrato `desenhar`. A classe `Circulo` implementa a interface `Desenhavel` e fornece a implementação do método `desenhar`. No método `main`, uma instância de `Circulo` é criada e o seu método `desenhar` é chamado.

```dart
// Definindo uma interface usando uma classe abstrata
abstract class Desenhavel {
  void desenhar(); // Método abstrato
}

// Classe que implementa a interface Desenhavel
class Circulo implements Desenhavel {
  double raio;

  Circulo(this.raio);

  @override
  void desenhar() {
    print('Desenhando um círculo com raio $raio');
  }
}

void main() {
  Circulo circulo = Circulo(5.0);
  circulo.desenhar(); // Output: Desenhando um círculo com raio 5.0
}
```

### Múltiplas Interfaces

Uma classe pode implementar múltiplas interfaces, obrigando-a a fornecer implementações para todos os métodos e propriedades de todas as interfaces.

```dart
abstract class Desenhavel {
  void desenhar();
}

abstract class Colorivel {
  void colorir(String cor);
}

class Circulo implements Desenhavel, Colorivel {
  double raio;

  Circulo(this.raio);

  @override
  void desenhar() {
    print('Desenhando um círculo com raio $raio');
  }

  @override
  void colorir(String cor) {
    print('Colorindo o círculo com a cor $cor');
  }
}

void main() {
  Circulo circulo = Circulo(5.0);
  circulo.desenhar(); // Output: Desenhando um círculo com raio 5.0
  circulo.colorir('vermelho'); // Output: Colorindo o círculo com a cor vermelho
}
```

### Interfaces vs. Classes Abstratas

- **Classes Abstratas**: Podem ter métodos concretos e abstratos, e também podem conter propriedades com implementações. Elas são usadas quando se quer definir algum comportamento padrão para as subclasses, mas ainda se quer forçar a implementação de certos métodos.
- **Interfaces**: Usadas para definir um conjunto de métodos e propriedades que uma classe deve implementar sem fornecer nenhuma implementação padrão.

## Padronizando Ordenações com a Interface Comparable

Em Dart, a interface `Comparable` é usada para definir uma ordenação natural para objetos de uma classe. Implementando a interface `Comparable`, pode-se especificar como os objetos de sua classe devem ser comparados uns com os outros. Isso é útil para ordenar coleções de objetos de maneira consistente.

### Implementando Comparable

Para implementar `Comparable` é preciso definir o método `compareTo`, que deve retornar:

- Um valor negativo se o objeto for menor que o outro objeto comparado.
- Zero se o objeto for igual ao outro objeto.
- Um valor positivo se o objeto for maior que o outro objeto.

Por exemplo, pode-se criar uma classe `Pessoa` e definir a ordenação natural baseada na idade.

```dart
class Pessoa implements Comparable<Pessoa> {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  @override
  int compareTo(Pessoa other) {
    if (this.idade < other.idade) {
      return -1;
    } else if (this.idade > other.idade) {
      return 1;
    } else {
      return 0;
    }
  }

  @override
  String toString() => 'Pessoa(nome: $nome, idade: $idade)';
}

void main() {
  List<Pessoa> pessoas = [
    Pessoa('Alice', 30),
    Pessoa('Bob', 25),
    Pessoa('Charlie', 35)
  ];

  pessoas.sort(); // Ordena a lista usando compareTo

  print(pessoas);
  // Output: [Pessoa(nome: Bob, idade: 25), Pessoa(nome: Alice, idade: 30), Pessoa(nome: Charlie, idade: 35)]
}
```

A classe `Pessoa` tem dois campos, `nome` e `idade`. Ela implementa `Comparable<Pessoa>`, que obrigatoriamente deve receber o objeto que será ordenado. O método `compareTo` compara a idade da instância atual (`this.idade`) com a idade do outro objeto (`other.idade`). O método `sort` da lista usa `compareTo` para ordenar os objetos `Pessoa`.

### Ordenação por Múltiplos Critérios

Às vezes, pode-se querer ordenar por múltiplos critérios. Por exemplo, se duas pessoas tiverem a mesma idade, o critério seguinte pode ser ordenar pelo nome.

```dart
class Pessoa implements Comparable<Pessoa> {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  @override
  int compareTo(Pessoa other) {
    int result = this.idade.compareTo(other.idade);
    if (result == 0) {
      result = this.nome.compareTo(other.nome);
    }
    return result;
  }

  @override
  String toString() => 'Pessoa(nome: $nome, idade: $idade)';
}

void main() {
  List<Pessoa> pessoas = [
    Pessoa('Alice', 30),
    Pessoa('Bob', 25),
    Pessoa('Charlie', 30),
    Pessoa('Dave', 25)
  ];

  pessoas.sort(); // Ordena a lista usando compareTo

  print(pessoas);
  // Output: [Pessoa(nome: Bob, idade: 25), Pessoa(nome: Dave, idade: 25), Pessoa(nome: Alice, idade: 30), Pessoa(nome: Charlie, idade: 30)]
}
```

Primeiro, os objetos são comparados por `idade`, se as idades forem iguais (`result == 0`), a comparação é feita pelo `nome`. No exemplo, foram usados os métodos `compareTo` de `int` e `String` para facilitar a comparação.

## Mixins

Mixins em Dart são uma forma de reutilizar código em várias classes. Eles permitem que se defina métodos e propriedades que podem ser usados por várias classes sem a necessidade de criar uma hierarquia complexa de herança. Mixins são particularmente úteis quando se deseja adicionar funcionalidade comum a várias classes sem estabelecer uma relação de "é um" que a herança tradicional implica. Ainda, os Mixins não podem ter construtores. 

### Definindo e Usando Mixins

Para definir um mixin, usa-se a palavra-chave `mixin`. Um `mixin` é semelhante a uma classe, mas não pode ser instanciado diretamente.

```dart
mixin Voar {
  void voar() {
    print('Estou voando!');
  }
}

mixin Nadar {
  void nadar() {
    print('Estou nadando!');
  }
}
```

Para usar um `mixin`, usa-se a palavra-chave `with` seguida do nome do mixin. Uma classe pode usar um ou mais mixins.

```dart
class Animal {
  void respirar() {
    print('Estou respirando');
  }
}

class Pato extends Animal with Voar, Nadar {
  void apresentar() {
    print('Eu sou um pato');
  }
}

void main() {
  Pato pato = Pato();
  pato.apresentar(); // Output: Eu sou um pato
  pato.respirar();   // Output: Estou respirando
  pato.voar();       // Output: Estou voando!
  pato.nadar();      // Output: Estou nadando!
}
```

### Restringindo Mixins

Se um mixin precisar usar métodos ou atributos de uma superclasse, deve-se declarar uma superclasse concreta para ele. Deve-se usar a palavra-chave `on` para restringir a aplicação do mixin a subclasses de uma determinada classe.

```dart
class Animal {
  void respirar() {
    print('Estou respirando');
  }
}

mixin Caminhar on Animal {
  void caminhar() {
    print('Estou caminhando');
  }
}

class Cachorro extends Animal with Caminhar {
  void latir() {
    print('Estou latindo');
  }
}

void main() {
  Cachorro cachorro = Cachorro();
  cachorro.respirar(); // Output: Estou respirando
  cachorro.caminhar(); // Output: Estou caminhando
  cachorro.latir();    // Output: Estou latindo
}
```

No exemplo acima, o mixin `Caminhar` só pode ser aplicado a classes que extendem de `Animal`, como é o caso da classe `Cachorro`. Se existisse uma outra classe que não herdasse de `Animal` e tentasse implementar o mixin, ela geraria um erro de compilação.

## Operator Methods (Métodos Operadores)

Em Dart, os Operator Methods (Métodos Operadores) permitem que você defina o comportamento de operadores padrão (como `+`, `-`, `*`, etc.) quando são aplicados a instâncias de suas classes. Isso é útil para criar classes que podem ser manipuladas de maneira intuitiva e matemática, semelhante aos tipos primitivos.

### Definindo Operator Methods

Para definir um método operador, usa-se a palavra-chave `operator` seguida pelo operador que se deseja sobrescrever. Para exemplificar, considera-se uma classe `Vector` que representa um vetor em um espaço 2D e defini-se o comportamento do operador `+` para somar dois vetores.

```dart
class Vector {
  final int x;
  final int y;

  Vector(this.x, this.y);

  // Sobrescrevendo o operador +
  Vector operator +(Vector other) {
    return Vector(this.x + other.x, this.y + other.y);
  }

  @override
  String toString() {
    return 'Vector($x, $y)';
  }
}

void main() {
  Vector v1 = Vector(2, 3);
  Vector v2 = Vector(4, 5);

  Vector v3 = v1 + v2; // Usando o operador + definido
  print(v3); // Output: Vector(6, 8)
}
```

Dessa forma, um `Vector` tem dois campos, `x` e `y`. O método operator `+` recebe outro objeto `Vector` e retorna um novo `Vector` cuja soma é calculada componente a componente. No método `main` tem-se a soma de dois vetores usando o operador `+`, que invoca o método operator `+`.

## Referência de Memória para Objetos

Em Dart, como em muitas linguagens de programação orientadas a objetos, os objetos são gerenciados por referência de memória. Isso significa que, ao criar um objeto e atribuí-lo a uma variável, essa variável armazena um ponteiro (ou referência) para a localização na memória onde o objeto está armazenado, e não o próprio valor do objeto.

```dart
class Pessoa {
  String nome;

  Pessoa(this.nome);
}

void main() {
  Pessoa p1 = Pessoa('Alice');
  Pessoa p2 = p1;

  p2.nome = 'Bob';

  print(p1.nome);     // Output: Bob
  print(p1.hashCode); // Output: 372478463 (aleatório)
  print(p2.hashCode); // Output: 372478463 (aleatório)
}
```

No exemplo, `p1` é uma variável que armazena uma referência ao objeto `Pessoa` criado com o nome `'Alice'`. Já `p1` é atribuído a `p2`, portanto, p2 e p1 referenciam o mesmo objeto na memória. Ao alterar `p2.nome` para `'Bob'` também se altera `p1.nome`, pois ambos apontam para o mesmo objeto. Isso é comprovado pelo código hash de ambos, que referencia o mesmo espaço de memória.

### Operator Methods Aplicados a `equals` e `hashCode`

Os métodos `operator ==` e `hashCode` são importantes para comparar a igualdade de objetos e para o uso eficiente em coleções como `Set` e `Map`.

O operador `==` é usado para comparar se dois objetos são iguais. Por padrão, `==` compara referências de memória (ou seja, se as referências apontam para o mesmo objeto). Para comparar valores de instância, deve-se sobrescrever este operador.

O método `hashCode` retorna um código hash para o objeto, que deve ser consistente com a definição de igualdade fornecida por `==`. Ou seja, se `a == b` é uma expressão verdadeira, então `a.hashCode` deve ser igual a `b.hashCode`.

```dart
class Pessoa {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  // Sobrescrevendo o operador ==
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    if (other is! Pessoa) return false;

    return this.nome == other.nome && this.idade == other.idade;
  }

  // Sobrescrevendo o hashCode
  @override
  int get hashCode => nome.hashCode ^ idade.hashCode;

  @override
  String toString() => 'Pessoa(nome: $nome, idade: $idade)';
}

void main() {
  Pessoa p1 = Pessoa('Alice', 30);
  Pessoa p2 = Pessoa('Alice', 30);
  Pessoa p3 = Pessoa('Bob', 25);

  print(p1 == p2); // Output: true
  print(p1 == p3); // Output: false

  // Usando um Set para armazenar pessoas únicas
  Set<Pessoa> pessoas = {p1, p2, p3};
  print(pessoas); // Output: {Pessoa(nome: Alice, idade: 30), Pessoa(nome: Bob, idade: 25)}
}
```

No exemplo acima, o métodod operador `==` verifica se o outro objeto é uma instância de `Pessoa` e se os valores de nome e idade são iguais. Já o método `hashCode` combina os códigos hash de nome e idade usando o operador bit a bit `^`. Dessa forma, ao se comparar os objetos com o operador `==`, ele retornará `true` se os valores de nome e idade forem iguais, caso contrário retorna `false`. Quando `p1` e `p2` são adicionados ao `Set`, eles são considerados iguais, então apenas um deles é armazenado. `p3` é considerado diferente e é adicionado ao conjunto.

## Extensions (Extensões)

Em Dart, as extensões (ou extensions) são uma funcionalidade poderosa que permite adicionar novos métodos e getters a tipos existentes sem a necessidade de modificar a definição original desses tipos. Isso é especialmente útil quando se quer estender a funcionalidade de classes de bibliotecas ou mesmo tipos nativos como `String` ou `List`. Algumas informações importantes sobre as extensões são:

- Extensões não podem adicionar campos (variáveis de instância) a tipos.
- Métodos de extensão não podem acessar membros privados da classe original.
- Se houver uma colisão de nome entre métodos de extensão e métodos da classe original, a classe original tem precedência.

### Definindo uma Extensão

Para definir uma extensão, usa-se a palavra-chave `extension` seguida por um nome opcional e o tipo que se deseja estender. Por exemplo, para se adicionar um método `reverse` à classe `String` que retorna a string invertida. Neste caso, a extensão não possui um nome.

```dart
extension on String {
  String reverse() {
    return split('').reversed.join('');
  }
}

void main() {
  String text = "hello";
  print(text.reverse()); // Output: olleh
}
```

No caso de uma lista, poderia-se adicionar um método `sum` para calcular a soma dos elementos de uma lista de inteiros utilizando uma extensão chamada `ListExtensions`.

```dart
extension ListExtensions on List<int> {
  int sum() {
    return reduce((value, element) => value + element);
  }
}

void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  print(numbers.sum()); // Output: 15
}
```

### Extensões com Genéricos

É possível se criar extensões genéricas que funcionam com qualquer tipo.

```dart
extension ListUtilities<T> on List<T> {
  void printAll() {
    forEach((element) {
      print(element);
    });
  }
}

void main() {
  List<int> numbers = [1, 2, 3];
  List<String> words = ["hello", "world"];
  
  numbers.printAll();
  words.printAll();
}
```

### Extensões Privadas

Pode-se criar extensões privadas usando o prefixo `_` para evitar conflitos de nome e limitar o escopo.

```dart
extension _PrivateExtensions on String {
  String get capitalized => this[0].toUpperCase() + substring(1);
}

void main() {
  String text = "hello";
  print(text.capitalized); // Output: Hello
}
```

### Extensões com Métodos de mesmo Nome

Se for preciso usar duas extensões que adicionam métodos com o mesmo nome a um tipo, pode-se utilizar os nomes das extensões para declarar os métodos explicitamente.

```dart
extension FirstExtension on String {
  String get first => this[0];
}

extension SecondExtension on String {
  String get firstChar => this[0];
}

void main() {
  String text = "hello";
  print(FirstExtension(text).first); // Output: h
  print(SecondExtension(text).firstChar); // Output: h
}
```

## Generics (Genéricos)

Os generics em Dart permitem criar classes, métodos e funções que funcionam com diferentes tipos de dados sem perder a segurança de tipos. Eles são especialmente úteis para estruturas de dados e algoritmos que devem funcionar de maneira consistente com qualquer tipo de dado, como listas, mapas e árvores. Suas principais vantagens são:

- **Reutilização**: Permite criar componentes altamente reutilizáveis.
- **Segurança de Tipo**: Garante que apenas os tipos esperados sejam usados, reduzindo erros em tempo de execução.
- **Clareza**: Facilita a leitura e manutenção do código, pois deixa claro quais tipos de dados são esperados.

Aqui está um exemplo básico de uma classe genérica em Dart:

```dart
class Caixa<T> {
  T valor;

  Caixa(this.valor);

  void mostrarValor() {
    print(valor);
  }
}

void main() {
  Caixa<int> caixaInt = Caixa<int>(123);
  caixaInt.mostrarValor(); // Output: 123

  Caixa<String> caixaString = Caixa<String>('Olá');
  caixaString.mostrarValor(); // Output: Olá
}
```

Neste exemplo, a classe `Caixa` é genérica e pode armazenar um valor de qualquer tipo especificado ao instanciá-la.

### Generics com Métodos

Também pode-se usar generics em métodos. Aqui está um exemplo de uma função genérica:

```dart
T retornarPrimeiro<T>(List<T> itens) {
  return itens[0];
}

void main() {
  print(retornarPrimeiro<int>([1, 2, 3])); // Output: 1
  print(retornarPrimeiro<String>(['a', 'b', 'c'])); // Output: a
}
```

Neste exemplo, a função `retornarPrimeiro` pode trabalhar com uma lista de qualquer tipo e retorna o primeiro elemento da lista.

### Classes e Métodos Genéricos

É possível combinar generics em classes e métodos para obter maior flexibilidade:

```dart
class Par<K, V> {
  K chave;
  V valor;

  Par(this.chave, this.valor);

  void mostrarPar() {
    print('Chave: $chave, Valor: $valor');
  }
}

void main() {
  Par<String, int> par = Par<String, int>('um', 1);
  par.mostrarPar(); // Output: Chave: um, Valor: 1
}
```

Neste exemplo, a classe `Par` aceita dois parâmetros de tipo genérico, `K` e `V`, permitindo criar pares de chaves e valores de qualquer tipo.

### Restrições de Tipo em Generics

Pode-se impor restrições aos tipos genéricos usando a palavra-chave `extends`. Isso é útil quando se deseja garantir que o tipo genérico herde de uma classe ou implemente uma interface específica:

```dart
class CaixaNumerica<T extends num> {
  T valor;

  CaixaNumerica(this.valor);

  void mostrarValor() {
    print(valor);
  }
}

void main() {
  CaixaNumerica<int> caixaInt = CaixaNumerica<int>(123);
  caixaInt.mostrarValor(); // Output: 123

  CaixaNumerica<double> caixaDouble = CaixaNumerica<double>(45.67);
  caixaDouble.mostrarValor(); // Output: 45.67
}
```

Neste exemplo, a classe `CaixaNumerica` só aceita tipos que estendem `num` (ou seja, `int` e `double`).

### Usando Generics com Collections

Os tipos genéricos são frequentemente usados com coleções, como `List`, `Set` e `Map`:

```dart
void main() {
  List<int> numeros = [1, 2, 3];
  Map<String, int> idades = {
    'Alice': 25,
    'Bob': 30
  };
  Set<double> valores = {1.1, 2.2, 3.3};

  print(numeros); // Output: [1, 2, 3]
  print(idades); // Output: {Alice: 25, Bob: 30}
  print(valores); // Output: {1.1, 2.2, 3.3}
}
```

## Metadados (Annotations)

Metadados (annotations) em Dart são uma forma de associar informações adicionais aos elementos do programa, como classes, métodos, variáveis, etc. Eles são usados para fornecer metadados que podem ser acessados em tempo de execução usando reflexão, ou para alterar o comportamento do compilador e ferramentas.

### Sintaxe de Annotations

Os metadados são definidos usando o símbolo `@` seguido pelo nome da annotation. Eles podem ser aplicados a vários elementos do código, como classes, métodos, campos, e parâmetros. Algumas annotations predefinidas úteis para desenvolvimento e que já existem na linguagem são:

- **@override**: Indica que um método está sobrescrevendo um método da superclasse.
- **@deprecated**: Marca um elemento como obsoleto, indicando que ele será removido em versões futuras.

```dart
class SuperClasse {
  void metodo() {}
}

class SubClasse extends SuperClasse {
  @override
  void metodo() {
    print('Método sobrescrito');
  }

  @deprecated
  void metodoAntigo() {
    print('Este método está obsoleto');
  }
}
```

### Criando e Usando Annotations Personalizadas

Pode-se definir annotations personalizadas para fornecer informações adicionais ao código. Essas annotations são definidas como classes com um construtor `const`.

```dart
import 'dart:mirrors';

// Definindo uma annotation personalizada
class Documentacao {
  final String descricao;

  const Documentacao(this.descricao);
}

// Usando a annotation personalizada
@Documentacao('Esta classe faz X')
class MinhaClasse {
  @Documentacao('Este método faz Y')
  void meuMetodo() {}
}

void main() {
  // Reflexão para acessar as annotations
  var minhaClasse = MinhaClasse();
  var mirror = reflect(minhaClasse);
  var classMetadata = mirror.type.metadata;

  for (var metadata in classMetadata) {
    if (metadata.reflectee is Documentacao) {
      var documentacao = metadata.reflectee as Documentacao;
      print('Descrição da classe: ${documentacao.descricao}');
    }
  }

  var metodoMirror = mirror.type.instanceMembers[Symbol('meuMetodo')];
  var metodoMetadata = metodoMirror?.metadata;

  for (var metadata in metodoMetadata!) {
    if (metadata.reflectee is Documentacao) {
      var documentacao = metadata.reflectee as Documentacao;
      print('Descrição do método: ${documentacao.descricao}');
    }
  }
}
```

### Reflexão e Annotations

Para acessar as annotations em tempo de execução, usa-se a biblioteca de reflexão (`dart:mirrors`). Um exemplo de como pode-se usar reflexão para acessar annotations:

```dart
import 'dart:mirrors';

// Definindo a annotation
class MinhaAnnotation {
  final String descricao;

  const MinhaAnnotation(this.descricao);
}

// Usando a annotation
@MinhaAnnotation('Esta é uma classe anotada')
class MinhaClasse {
  @MinhaAnnotation('Este é um campo anotado')
  String meuCampo = 'valor';

  @MinhaAnnotation('Este é um método anotado')
  void meuMetodo() {}
}

void main() {
  var instancia = MinhaClasse();
  var instanciaMirror = reflect(instancia);
  var classMirror = instanciaMirror.type;

  // Acessando annotations na classe
  for (var metadata in classMirror.metadata) {
    if (metadata.reflectee is MinhaAnnotation) {
      var annotation = metadata.reflectee as MinhaAnnotation;
      print('Classe: ${annotation.descricao}');
    }
  }

  // Acessando annotations no campo
  var fieldMirror = classMirror.declarations[Symbol('meuCampo')] as VariableMirror;
  for (var metadata in fieldMirror.metadata) {
    if (metadata.reflectee is MinhaAnnotation) {
      var annotation = metadata.reflectee as MinhaAnnotation;
      print('Campo: ${annotation.descricao}');
    }
  }

  // Acessando annotations no método
  var methodMirror = classMirror.declarations[Symbol('meuMetodo')] as MethodMirror;
  for (var metadata in methodMirror.metadata) {
    if (metadata.reflectee is MinhaAnnotation) {
      var annotation = metadata.reflectee as MinhaAnnotation;
      print('Método: ${annotation.descricao}');
    }
  }
}
```
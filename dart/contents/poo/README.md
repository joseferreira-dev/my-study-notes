<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Programação Orientada a Objetos

- [Introdução](#introdução)
- [Classes](#classes)
  * [Definindo uma Classe](#definindo-uma-classe)
  * [Instanciando uma Classe](#instanciando-uma-classe)
  * [Métodos e Atributos](#métodos-e-atributos)
- [Atributos e Métodos de Instâncias e de Classes (Estáticos)](#atributos-e-métodos-de-instâncias-e-de-classes-estáticos)
- [Classes, Atributos e Métodos Privados](#classes-atributos-e-métodos-privados)
  * [Classes Privadas](#classes-privadas)
  * [Atributos Privados](#atributos-privados)
  * [Métodos Privados](#métodos-privados)
  * [Getters e Setters](#getters-e-setters)
- [Construtores](#construtores)
  * [Construtor Padrão](#construtor-padrão)
  * [Construtores com Parâmetros Posicionais Opcionais](#construtores-com-parâmetros-posicionais-opcionais)
  * [Construtores com Parâmetros Nomeados Opcionais](#construtores-com-parâmetros-nomeados-opcionais)
  * [Construtores com Parâmetros Nomeados com Valores Padrão](#construtores-com-parâmetros-nomeados-com-valores-padrão)
  * [Construtores com Parâmetros Nomeados Obrigatórios](#construtores-com-parâmetros-nomeados-obrigatórios)
  * [Construtores Nomeados](#construtores-nomeados)
  * [Construtores Nomeados com Inicialização Diferente](#construtores-nomeados-com-inicialização-diferente)
  * [Construtores Nomeados com Parâmetros Opcionais](#construtores-nomeados-com-parâmetros-opcionais)
  * [Construtor Factory](#construtor-factory)
- [Herança](#herança)
  * [Como Implementar Herança](#como-implementar-herança)
  * [Sobrescrita de Métodos](#sobrescrita-de-métodos)
  * [Chamando Métodos da Superclasse](#chamando-métodos-da-superclasse)
  * [Construtores e Herança](#construtores-e-herança)
  * [Herança e Visibilidade](#herança-e-visibilidade)
- [Usando Covariância](#usando-covariância)
  * [Contexto](#contexto)
  * [Uso de `covariant`](#uso-de-covariant)
  * [Covariância em Getters e Setters](#covariância-em-getters-e-setters)
- [Classes Abstratas](#classes-abstratas)
  * [Definindo e Usando Classes Abstratas](#definindo-e-usando-classes-abstratas)
  * [Métodos e Propriedades Abstratas](#métodos-e-propriedades-abstratas)
  * [Usando Classes Abstratas como Interfaces](#usando-classes-abstratas-como-interfaces)

## Introdução

A Programação Orientada a Objetos (POO) é um paradigma de programação que organiza o software em torno de objetos, que podem conter dados (campos/propriedades) e métodos (funções). Em Dart, a POO é amplamente suportada e facilita a criação de código modular e reutilizável.

## Classes

As classes são um dos principais conceitos da programação orientada a objetos (OOP), e Dart utiliza classes para definir tipos de objetos. As classes permitem agrupar dados e comportamentos relacionados em uma única estrutura.

### Definindo uma Classe

Uma classe é definida usando a palavra-chave `class`, seguida pelo nome da classe. Dentro da classe, pode-se definir atributos (ou campos) e métodos (ou funções).

```dart
class Pessoa {
  // Atributos
  String nome;
  int idade;

  // Construtor
  Pessoa(this.nome, this.idade);

  // Método
  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}
```

### Instanciando uma Classe

Para criar uma instância (ou objeto) de uma classe, usa-se o construtor da classe com a palavra-chave `new` (opcional a partir do Dart 2.0).

```dart
void main() {
  var pessoa = Pessoa('João', 30);
  pessoa.apresentar(); // Output: Olá, meu nome é João e eu tenho 30 anos.
}
```

### Métodos e Atributos

Atributos são variáveis que pertencem a uma classe e representam o estado ou as propriedades dos objetos criados a partir dessa classe. Eles podem ser de qualquer tipo, como números, strings, listas, objetos de outras classes, etc.

```dart
class Pessoa {
  // Atributos
  String nome;
  int idade;
  
  // Construtor
  Pessoa(this.nome, this.idade);
}

void main() {
  var pessoa = Pessoa('Alice', 25);
  print(pessoa.nome); // Output: Alice
  print(pessoa.idade); // Output: 25
}
```

Métodos são funções que pertencem a uma classe e definem comportamentos que os objetos da classe podem realizar. Métodos podem acessar e modificar os atributos da classe, além de realizar outras operações.

```dart
class Pessoa {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  // Método
  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }

  // Método que modifica um atributo
  void fazerAniversario() {
    idade += 1;
    print('Parabéns! Agora eu tenho $idade anos.');
  }
}

void main() {
  var pessoa = Pessoa('Alice', 25);
  pessoa.apresentar(); // Output: Olá, meu nome é Alice e eu tenho 25 anos.
  pessoa.fazerAniversario(); // Output: Parabéns! Agora eu tenho 26 anos.
}
```

## Atributos e Métodos de Instâncias e de Classes (Estáticos)

Atributos de instância são variáveis que pertencem a uma instância específica de uma classe. Cada objeto criado a partir da classe tem seu próprio conjunto de atributos de instância. Esses atributos representam o estado do objeto e podem ser diferentes para cada instância da classe.

```dart
class Pessoa {
  String nome; // Atributo de instância
  int idade; // Atributo de instância

  Pessoa(this.nome, this.idade);
}

void main() {
  var pessoa1 = Pessoa('Alice', 25);
  var pessoa2 = Pessoa('Bob', 30);
  
  print(pessoa1.nome); // Output: Alice
  print(pessoa2.nome); // Output: Bob
}
```

Os métodos de instância são funções que operam em instâncias específicas de uma classe. Eles podem acessar e modificar os atributos de instância e geralmente operam no contexto do objeto para o qual foram chamados.

```dart
class Pessoa {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  void apresentar() { // Método de instância
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}

void main() {
  var pessoa1 = Pessoa('Alice', 25);
  var pessoa2 = Pessoa('Bob', 30);
  
  pessoa1.apresentar(); // Output: Olá, meu nome é Alice e eu tenho 25 anos.
  pessoa2.apresentar(); // Output: Olá, meu nome é Bob e eu tenho 30 anos.
}
```

Atributos de classe (ou estáticos) são variáveis que pertencem à própria classe, e não a qualquer instância específica. Todos os objetos da classe compartilham um único valor para cada atributo estático.

```dart
class Pessoa {
  String nome;
  int idade;

  static int contador = 0; // Atributo de classe

  Pessoa(this.nome, this.idade) {
    contador++;
  }
}

void main() {
  var pessoa1 = Pessoa('Alice', 25);
  var pessoa2 = Pessoa('Bob', 30);
  
  print(Pessoa.contador); // Output: 2
}
```

Já os métodos de classe (ou estáticos) são funções que pertencem à própria classe e não a uma instância específica. Eles não podem acessar diretamente os atributos de instância, mas podem acessar os atributos de classe. São definidos usando a palavra-chave `static`.

```dart
class Calculadora {
  static double somar(double a, double b) { // Método de classe
    return a + b;
  }

  static double subtrair(double a, double b) { // Método de classe
    return a - b;
  }
}

void main() {
  double resultado1 = Calculadora.somar(5, 3);
  double resultado2 = Calculadora.subtrair(5, 3);
  
  print(resultado1); // Output: 8
  print(resultado2); // Output: 2
}
```

Em alguns casos é interessante que os atributos estáticos não possam ser alterados, para assim evitar diversos problemas. Para isto, usa-se o modificador `const` na declaração dos atributos de classe.

```dart
class PessoaFranca {
  String nome;
  static String nacionalidade = 'Francês(a)'; // Atributo de classe

  PessoaFranca(this.nome);
}

class PessoaBrasil {
  String nome;
  static const String nacionalidade = 'Brasileiro(a)'; // Atributo de classe

  PessoaBrasil(this.nome);
}

void main() {
  PessoaFranca.nacionalidade = 'Espanhol(a)';
  PessoaBrasil.nacionalidade = 'Espanhol(a)'; // Erro de compilação
}
```

Também é possível fazer com que os atributos das instâncias de uma classe não sejam alterados após a sua inicialização. Para isso usa-se o modificador `final` declaração dos atributos.

```dart
class Pessoa {
  final String nome;
  final int idade;

  Pessoa(this.nome, this.idade);
}

void main() {
  var pessoa = Pessoa('Alice', 25);
  print(pessoa.nome);  // Output: Alice
  print(pessoa.idade); // Output: 25

  pessoa.nome = 'Bob'; // Erro de compilação
}
```

## Classes, Atributos e Métodos Privados

Em Dart, pode-se definir classes, atributos e métodos como privados para controlar o acesso a eles dentro de um módulo (arquivo .dart). A privacidade é implementada usando o prefixo `_` (underscore) antes do nome do atributo, método ou classe. A privacidade ajuda a encapsular detalhes de implementação e a proteger os dados internos da classe contra acessos indevidos. Isso leva a um design mais seguro e modular, onde a interface pública da classe é claramente separada da sua implementação interna.

### Classes Privadas

Classes privadas só podem ser acessadas dentro do mesmo arquivo onde foram definidas.

```dart
// No arquivo `classe_privada.dart`
class _ClassePrivada {
  void _metodoPrivado() {
    print('Método privado.');
  }
  
  void metodoPublico() {
    _metodoPrivado();
  }
}

// No arquivo `main.dart`
void main() {
  var obj = _ClassePrivada();
  obj.metodoPublico();  // Output: Método privado.
  
  obj._metodoPrivado(); // Erro de compilação
}
```

### Atributos Privados

Atributos privados só podem ser acessados dentro da classe em que são definidos.

```dart
// No arquivo `pessoa.dart`
class Pessoa {
  String _nome; // Atributo privado
  int _idade;   // Atributo privado

  Pessoa(this._nome, this._idade);

  String get nome => _nome; // Getter público para o nome
  int get idade => _idade;  // Getter público para a idade
}

// No arquivo `main.dart`
void main() {
  var pessoa = Pessoa('Alice', 25);
  print(pessoa.nome);  // Output: Alice
  print(pessoa.idade); // Output: 25

  print(pessoa._nome); // Erro de compilação
}
```

### Métodos Privados

Métodos privados só podem ser chamados dentro da classe em que são definidos.

```dart
// No arquivo `pessoa.dart`
class Pessoa {
  String _nome;
  int _idade;

  Pessoa(this._nome, this._idade);

  void _mostrarDetalhes() { // Método privado
    print('Nome: $_nome, Idade: $_idade');
  }

  void apresentar() {
    _mostrarDetalhes();
  }
}

// No arquivo `main.dart`
void main() {
  var pessoa = Pessoa('Alice', 25);
  pessoa.apresentar(); // Output: Nome: Alice, Idade: 25

  pessoa._mostrarDetalhes(); // Erro de compilação
}
```

### Getters e Setters

Getters e setters são métodos especiais que permitem controlar o acesso aos atributos de uma classe. Eles são usados para encapsular e proteger os dados, fornecendo uma maneira controlada de acessar e modificar os valores dos atributos.

Um getter é uma função que permite a leitura de um valor de um atributo privado. Getters são definidos usando a palavra-chave `get`, seguida pelo nome do getter e pelo corpo da função. Getters não aceitam parâmetros.

```dart
// No arquivo `pessoa.dart`
class Pessoa {
  String _nome;

  Pessoa(this._nome);

  // Getter
  String get nome => _nome;
}

// No arquivo `main.dart`
void main() {
  var pessoa = Pessoa('Alice');
  print(pessoa.nome); // Output: Alice
}
```

Já um setter é uma função que permite modificar o valor de um atributo privado. Setters são definidos usando a palavra-chave `set`, seguida pelo nome do setter e pelo corpo da função. Setters aceitam um único parâmetro, que é o novo valor a ser atribuído ao atributo.

```dart
// No arquivo `pessoa.dart`
class Pessoa {
  String _nome; // Atributo privado

  Pessoa(this._nome);

  // Setter
  set nome(String novoNome) {
    _nome = novoNome;
  }

  // Getter
  String get nome => _nome;
}

// No arquivo `main.dart`
void main() {
  var pessoa = Pessoa('Alice');
  print(pessoa.nome); // Output: Alice

  pessoa.nome = 'Bob'; // Usando o setter
  print(pessoa.nome); // Output: Bob
}
```

Getters e setters também podem ser usados para adicionar lógica de validação ao acessar ou modificar atributos. Isso garante que os dados sejam consistentes e válidos.

```dart
// No arquivo `retangulo.dart`
class Retangulo {
  double _largura;
  double _altura;

  Retangulo(this._largura, this._altura);

  // Getter para a área
  double get area => _largura * _altura;

  // Setter para largura com validação
  set largura(double largura) {
    if (largura <= 0) {
      throw ArgumentError('A largura deve ser maior que zero');
    }
    _largura = largura;
  }

  // Setter para altura com validação
  set altura(double altura) {
    if (altura <= 0) {
      throw ArgumentError('A altura deve ser maior que zero');
    }
    _altura = altura;
  }

  // Getters para largura e altura
  double get largura => _largura;
  double get altura => _altura;
}

// No arquivo `main.dart`
void main() {
  var retangulo = Retangulo(5, 3);
  print(retangulo.area);  // Output: 15

  retangulo.largura = 4;
  print(retangulo.area);  // Output: 12

  retangulo.largura = -2; //  Erro de compilação
}
```

No exemplo acima, os setters `largura` e `altura` garantem que os valores atribuídos sejam válidos (maiores que zero), adicionando uma camada de validação ao acesso aos atributos.

## Construtores

Em Dart, construtores são métodos especiais que são chamados quando uma nova instância de uma classe é criada. Construtores são usados para inicializar os objetos.

### Construtor Padrão

O construtor padrão é um método que tem o mesmo nome da classe e não tem nenhum tipo de retorno, nem mesmo `void`. Ele é usado para inicializar os atributos da classe.

```dart
class Pessoa {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}

void main() {
  var pessoa = Pessoa('Alice', 25);
  pessoa.apresentar(); // Output: Olá, meu nome é Alice e eu tenho 25 anos.
}
```

### Construtores com Parâmetros Posicionais Opcionais

Parâmetros posicionais opcionais são definidos entre colchetes (`[]`). Se não fornecidos, eles assumem um valor `null` ou um valor padrão definido.

```dart
class Pessoa {
  String nome;
  int? idade; // Idade pode ser null

  // Construtor com parâmetro opcional
  Pessoa(this.nome, [this.idade]);

  void apresentar() {
    if (idade != null) {
      print('Olá, meu nome é $nome e eu tenho $idade anos.');
    } else {
      print('Olá, meu nome é $nome.');
    }
  }
}

void main() {
  var pessoa1 = Pessoa('Alice', 25);
  var pessoa2 = Pessoa('Bob');

  pessoa1.apresentar(); // Output: Olá, meu nome é Alice e eu tenho 25 anos.
  pessoa2.apresentar(); // Output: Olá, meu nome é Bob.
}
```

### Construtores com Parâmetros Nomeados Opcionais

Parâmetros nomeados opcionais são definidos entre chaves (`{}`). Eles são usados para melhorar a legibilidade do código e permitem definir valores padrão. Eles podem ser marcados como obrigatórios usando `required`.

```dart
class Pessoa {
  String nome;
  int idade;
  String? cidade;

  // Construtor com parâmetros nomeados opcionais
  Pessoa(this.nome, this.idade, {this.cidade});

  void apresentar() {
    if (cidade != null) {
      print('Olá, meu nome é $nome, tenho $idade anos e moro em $cidade.');
    } else {
      print('Olá, meu nome é $nome e eu tenho $idade anos.');
    }
  }
}

void main() {
  var pessoa1 = Pessoa('Alice', 25, cidade: 'São Paulo');
  var pessoa2 = Pessoa('Bob', 30);

  pessoa1.apresentar(); // Output: Olá, meu nome é Alice, tenho 25 anos e moro em São Paulo.
  pessoa2.apresentar(); // Output: Olá, meu nome é Bob e eu tenho 30 anos.
}
```

### Construtores com Parâmetros Nomeados com Valores Padrão

Pode-se definir valores padrão para parâmetros nomeados, o que é útil para fornecer um valor de fallback se nenhum valor for fornecido.

```dart
class Pessoa {
  String nome;
  int idade;
  String cidade;

  // Construtor com parâmetros nomeados e valores padrão
  Pessoa(this.nome, this.idade, {this.cidade = 'Desconhecida'});

  void apresentar() {
    print('Olá, meu nome é $nome, tenho $idade anos e moro em $cidade.');
  }
}

void main() {
  var pessoa1 = Pessoa('Alice', 25, cidade: 'São Paulo');
  var pessoa2 = Pessoa('Bob', 30);

  pessoa1.apresentar(); // Saída: Olá, meu nome é Alice, tenho 25 anos e moro em São Paulo.
  pessoa2.apresentar(); // Saída: Olá, meu nome é Bob, tenho 30 anos e moro em Desconhecida.
}
```

### Construtores com Parâmetros Nomeados Obrigatórios

Pode-se marcar parâmetros nomeados como obrigatórios usando a anotação `required`.

```dart
class Pessoa {
  String nome;
  int idade;
  String cidade;

  // Construtor com parâmetros nomeados obrigatórios
  Pessoa({
    required this.nome,
    required this.idade,
    required this.cidade,
  });

  void apresentar() {
    print('Olá, meu nome é $nome, tenho $idade anos e moro em $cidade.');
  }
}

void main() {
  var pessoa = Pessoa(nome: 'Alice', idade: 25, cidade: 'São Paulo');

  pessoa.apresentar(); // Saída: Olá, meu nome é Alice, tenho 25 anos e moro em São Paulo.
  
  var pessoaInvalida = Pessoa(nome: 'Bob', idade: 30); // Erro de compilação, pois 'cidade' é obrigatório.
}
```

### Construtores Nomeados

Construtores nomeados permitem criar múltiplos construtores para uma classe, cada um com um nome diferente. Eles são úteis quando se precisa de diferentes formas de inicializar um objeto.

```dart
class Pessoa {
  String nome;
  int? idade;

  // Construtor padrão
  Pessoa(this.nome, this.idade);

  // Construtor nomeado
  Pessoa.comIdade(this.nome) {
    idade = 0;
  }

  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}

void main() {
  var pessoa1 = Pessoa('Carol', 28);
  var pessoa2 = Pessoa.comIdade('Dave');

  pessoa1.apresentar(); // Output: Olá, meu nome é Carol e eu tenho 28 anos.
  pessoa2.apresentar(); // Output: Olá, meu nome é Dave e eu tenho 0 anos.
}
```

### Construtores Nomeados com Inicialização Diferente

Pode-se usar construtores nomeados para inicializar a classe de maneiras diferentes. Por exemplo, um construtor pode aceitar um mapa de dados.

```dart
class Pessoa {
  String? nome;
  int? idade;

  // Construtor padrão
  Pessoa(this.nome, this.idade);

  // Construtor nomeado que inicializa a partir de um mapa
  Pessoa.fromMap(Map<String, dynamic> data) {
    nome = data['nome'];
    idade = data['idade'];
  }

  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}

void main() {
  var dados = {'nome': 'Carlos', 'idade': 40};
  var pessoa = Pessoa.fromMap(dados);

  pessoa.apresentar(); // Output: Olá, meu nome é Carlos e eu tenho 40 anos.
}
```

### Construtores Nomeados com Parâmetros Opcionais

Construtores nomeados também podem ter parâmetros opcionais, facilitando ainda mais a flexibilidade na criação de instâncias.

```dart
class Retangulo {
  double? largura;
  double? altura;

  // Construtor padrão
  Retangulo(this.largura, this.altura);

  // Construtor nomeado com valores padrão
  Retangulo.quadrado(double tamanho) {
    largura = tamanho;
    altura = tamanho;
  }

  double get area => largura! * altura!;

  void mostrarDimensoes() {
    print('Largura: $largura, Altura: $altura, Área: $area');
  }
}

void main() {
  var retangulo = Retangulo(4, 5);
  var quadrado = Retangulo.quadrado(4);

  retangulo.mostrarDimensoes(); // Saída: Largura: 4.0, Altura: 5.0, Área: 20.0
  quadrado.mostrarDimensoes(); // Saída: Largura: 4.0, Altura: 4.0, Área: 16.0
}
```

### Construtor Factory

Um construtor factory é usado quando se deseja retornar uma instância de uma classe, seja uma nova instância ou uma já existente. O construtor factory permite controlar a criação de objetos, o que pode ser útil para implementar padrões de design como Singleton ou para inicializar objetos a partir de fontes não triviais, como resultados de cálculos ou dados externos.

A definição de um construtor factory é similar à de um construtor regular, mas utiliza a palavra-chave `factory`:

```dart
class NomeDaClasse {
  // Construtor factory
  factory NomeDaClasse() {
    // lógica de criação de instância
    return ...;
  }
}
```

O Singleton é um padrão de design onde apenas uma única instância de uma classe é criada e usada em toda a aplicação. O construtor factory pode ser utilizado para garantir que apenas uma instância de uma classe seja criada.

```dart
class Singleton {
  static final Singleton _instance = Singleton._internal();

  // Construtor privado
  Singleton._internal();

  // Construtor factory
  factory Singleton() {
    return _instance;
  }

  void fazerAlgo() {
    print('Fazendo algo...');
  }
}

void main() {
  var s1 = Singleton();
  var s2 = Singleton();

  print(identical(s1, s2)); // Saída: true
  s1.fazerAlgo(); // Saída: Fazendo algo...
}
```

O construtor `factory` pode ser usado para inicialização complexa de objetos a partir de dados não triviais, como um mapa de dados ou uma resposta de API.

```dart
class Pessoa {
  String nome;
  int idade;

  Pessoa(this.nome, this.idade);

  // Construtor factory para criar Pessoa a partir de um mapa
  factory Pessoa.fromMap(Map<String, dynamic> data) {
    if (data.containsKey('nome') && data.containsKey('idade')) {
      return Pessoa(data['nome'], data['idade']);
    } else {
      throw ArgumentError('Dados inválidos para criar uma Pessoa');
    }
  }

  void apresentar() {
    print('Olá, meu nome é $nome e eu tenho $idade anos.');
  }
}

void main() {
  var dados = {'nome': 'Alice', 'idade': 30};
  var pessoa = Pessoa.fromMap(dados);
  
  pessoa.apresentar(); // Saída: Olá, meu nome é Alice e eu tenho 30 anos.
}
```

## Herança

Herança é um dos pilares da programação orientada a objetos (OOP) e permite que uma classe herde propriedades e métodos de outra classe. Herança é utilizada para criar uma hierarquia de classes, permitindo o reuso de código e a criação de relacionamentos entre classes. Os conceitos básicos de herança em Dart são:

- **Classe Base (Superclasse)**: A classe cujas propriedades e métodos são herdados por outras classes.
- **Classe Derivada (Subclasse)**: A classe que herda as propriedades e métodos da classe base.
- **Herança Simples**: Dart suporta herança simples, ou seja, uma classe pode herdar de apenas uma classe base.

### Como Implementar Herança

Para criar uma subclasse, usa-se a palavra-chave `extends` seguida pelo nome da classe base.

```dart
class Animal {
  void comer() {
    print('Animal está comendo');
  }
}

// Subclasse Cachorro herda de Animal
class Cachorro extends Animal {
  void latir() {
    print('Cachorro está latindo');
  }
}

void main() {
  var cachorro = Cachorro();
  cachorro.comer();  // Método herdado da classe Animal
  cachorro.latir();  // Método da classe Cachorro
}
```

### Sobrescrita de Métodos

A subclasse pode sobrescrever métodos da classe base para fornecer uma implementação específica. Para isso, usa-se a anotação `@override`.

```dart
class Animal {
  void fazerSom() {
    print('Animal faz som');
  }
}

class Cachorro extends Animal {
  @override
  void fazerSom() {
    print('Cachorro late');
  }
}

void main() {
  var animal = Animal();
  var cachorro = Cachorro();

  animal.fazerSom();   // Output: Animal faz som
  cachorro.fazerSom(); // Output: Cachorro late
}
```

### Chamando Métodos da Superclasse

Pode-se chamar um método da superclasse usando a palavra-chave `super`.

```dart
class Animal {
  void fazerSom() {
    print('Animal faz som');
  }
}

class Cachorro extends Animal {
  @override
  void fazerSom() {
    super.fazerSom();  // Chama o método da superclasse
    print('Cachorro late');
  }
}

void main() {
  var cachorro = Cachorro();
  cachorro.fazerSom();
  // Output:
  // Animal faz som
  // Cachorro late
}
```

### Construtores e Herança

Quando uma subclasse é instanciada, o construtor da superclasse é chamado automaticamente antes do construtor da subclasse. Pode-se controlar qual construtor da superclasse é chamado usando uma lista de inicialização no construtor da subclasse.

```dart
class Animal {
  String nome;

  // Construtor da classe Animal
  Animal(this.nome);

  void apresentar() {
    print('Eu sou um $nome');
  }
}

class Cachorro extends Animal {
  String raca;

  // Construtor da classe Cachorro
  Cachorro(String nome, this.raca) : super(nome);

  @override
  void apresentar() {
    super.apresentar();  // Chama o método da superclasse
    print('Eu sou da raça $raca');
  }
}

void main() {
  var cachorro = Cachorro('Rex', 'Labrador');
  cachorro.apresentar();
  // Output:
  // Eu sou um Rex
  // Eu sou da raça Labrador
}
```

### Herança e Visibilidade

Membros privados da superclasse não são herdados pelas subclasses, mas isso só é verdade se as subclasses estiverem em um arquivo diferente da superclasse, se ambas estiverem no mesmo arquivo, o acesso é permitido.

```dart
// No arquivo 'animal.dart'
class Animal {
  String nome;
  int _idade; // Privado

  Animal(this.nome, this._idade);

  void mostrarIdade() {
    print('Idade: $_idade');
  }
}

// No arquivo 'cachorro.dart'
class Cachorro extends Animal {
  String raca;

  Cachorro(String nome, int idade, this.raca) : super(nome, idade);

  // Sobrescrever métodos que tentam acessar membros privados não é possível diretamente
  void apresentar() {
    print('Eu sou um $nome e sou da raça $raca');
    print('Idade: $_idade'); // Erro: _idade é privado na superclasse
  }
}

// No arquivo 'main.dart'
void main() {
  var cachorro = Cachorro('Rex', 5, 'Labrador');
  cachorro.apresentar();
  cachorro.mostrarIdade(); // Método público da superclasse pode ser usado
}
```

## Usando Covariância

Em Dart, a palavra-chave `covariant` é usada para relaxar a verificação de tipos em métodos, permitindo que se substitua parâmetros de métodos em classes derivadas com tipos mais específicos. Isso é útil ao trabalhar com herança e polimorfismo, especialmente quando você precisa que uma subclasse aceite um tipo mais específico do que o tipo declarado na classe base.

### Contexto

Por padrão, Dart aplica regras de substituição estritas para garantir que os tipos permaneçam seguros. Isso significa que, ao sobrescrever um método, os parâmetros do método sobrescrito devem ter o mesmo tipo ou um tipo mais genérico. A palavra-chave `covariant` permite que um parâmetro seja substituído por um tipo mais específico.

### Uso de `covariant`

Np exemplo abaixo, tem-se uma classe `Animal` com um método `fazerSom` e uma classe `Cachorro` que sobrescreve esse método. A função `emitirSom` aceita um parâmetro do tipo `Animal` e chama `fazerSom` no objeto passado.

```dart
class Animal {
  void fazerSom() {
    print('Som genérico de animal');
  }
}

class Cachorro extends Animal {
  @override
  void fazerSom() {
    print('Latido');
  }
}

void emitirSom(Animal animal) {
  animal.fazerSom();
}

void main() {
  var meuCachorro = Cachorro();
  emitirSom(meuCachorro); // Output: Latido
}
```

Agora, considere o caso onde se tem um método que espera um parâmetro do tipo `Animal`, mas se quer sobrescrevê-lo em uma subclasse para aceitar um tipo mais específico, como `Cachorro`.

```dart
class TreinadorDeAnimais {
  void treinar(Animal animal) {
    print('Treinando um animal');
  }
}

class TreinadorDeCachorros extends TreinadorDeAnimais {
  @override
  void treinar(Cachorro cachorro) {
    print('Treinando um cachorro');
  }
}
```

O código acima não compila porque o método `treinar` na classe `TreinadorDeCachorros` tenta substituir o método `treinar` da classe base `TreinadorDeAnimais` com um parâmetro mais específico (`Cachorro` em vez de `Animal`). Para resolver isso, usa-se `covariant`:

```dart
class TreinadorDeAnimais {
  void treinar(covariant Animal animal) {
    print('Treinando um animal');
  }
}

class TreinadorDeCachorros extends TreinadorDeAnimais {
  @override
  void treinar(Cachorro cachorro) {
    print('Treinando um cachorro');
  }
}
```

Agora, o código compila e funciona corretamente:

```dart
void main() {
  var treinador = TreinadorDeCachorros();
  treinador.treinar(Cachorro()); // Saída: Treinando um cachorro
}
```

### Covariância em Getters e Setters

A palavra-chave `covariant` também pode ser usada com getters e setters para permitir tipos mais específicos ao sobrescrever esses métodos.

```dart
class Animal {
  String nome;
  Animal(this.nome);
}

class Cachorro extends Animal {
  Cachorro(String nome) : super(nome);
}

class Coleira {
  Animal animal;
  Coleira(this.animal);
}

class ColeiraParaCachorros extends Coleira {
  ColeiraParaCachorros(Cachorro cachorro) : super(cachorro);

  // Usando covariant no setter para aceitar um tipo mais específico
  @override
  set animal(covariant Cachorro cachorro) {
    super.animal = cachorro;
  }
}

void main() {
  var coleira = ColeiraParaCachorros(Cachorro('Rex'));
  coleira.animal = Cachorro('Buddy');
  print(coleira.animal.nome); // Output: Buddy
}
```

Neste exemplo, `covariant` é usado no setter `animal` na classe `ColeiraParaCachorros` para permitir que o parâmetro seja do tipo mais específico `Cachorro`.

## Classes Abstratas

Classes abstratas são usadas para definir uma estrutura comum que outras classes podem implementar ou estender. Elas não podem ser instanciadas diretamente e são frequentemente utilizadas para fornecer uma base comum de métodos e propriedades que devem ser implementados por subclasses concretas. Suas principais características são:

- **Não Instanciáveis**: Não é possível criar uma instância de uma classe abstrata diretamente.
- **Métodos Abstratos**: Podem conter métodos sem implementação (métodos abstratos), que devem ser implementados pelas subclasses.
- **Métodos Concretos**: Podem também conter métodos com implementação que podem ser usados ou sobrescritos pelas subclasses.
- **Propriedades Abstratas**: Podem definir propriedades que devem ser implementadas pelas subclasses.

### Definindo e Usando Classes Abstratas

As classes abstratas são definidas usando a palavra-chave `abstract`. No exemplo a seguir a classe `Forma` é definida como uma classe abstrata. Ela contém um método abstrato `calcularArea` e um método concreto `mostrarTipo`. A classe `Circulo` estende a classe `Forma` e implementa o método `calcularArea`. No método `main`, é criada uma instância da classe `Circulo` e seus métodos são chamados.

```dart
// Definindo uma classe abstrata
abstract class Forma {
  // Método abstrato
  double calcularArea();

  // Método concreto
  void mostrarTipo() {
    print('Eu sou uma forma geométrica.');
  }
}

// Subclasse concreta que implementa a classe abstrata
class Circulo extends Forma {
  double raio;

  Circulo(this.raio);

  @override
  double calcularArea() {
    return 3.14 * raio * raio;
  }
}

void main() {
  Circulo circulo = Circulo(5);
  print('Área do círculo: ${circulo.calcularArea()}');
  circulo.mostrarTipo();
}
```

### Métodos e Propriedades Abstratas

Uma classe abstrata pode conter métodos e propriedades abstratas que devem ser implementados pelas subclasses.

```dart
abstract class Forma {
  // Propriedade abstrata
  double get area;

  // Propriedade concreta
  String tipo = 'Forma';

  // Método concreto
  void mostrarTipo() {
    print('Eu sou uma $tipo.');
  }
}

class Retangulo extends Forma {
  double largura;
  double altura;

  Retangulo(this.largura, this.altura) {
    tipo = 'Retângulo';
  }

  @override
  double get area => largura * altura;
}

void main() {
  Retangulo retangulo = Retangulo(10, 5);
  print('Área do retângulo: ${retangulo.area}');
  retangulo.mostrarTipo();
}
```

### Usando Classes Abstratas como Interfaces

Dart não tem uma palavra-chave separada para interfaces. Em vez disso, pode-se usar classes abstratas como interfaces.

```dart
abstract class Animal {
  void fazerSom(); // Método abstrato
}

class Cachorro implements Animal {
  @override
  void fazerSom() {
    print('Latido');
  }
}

class Gato implements Animal {
  @override
  void fazerSom() {
    print('Miau');
  }
}

void main() {
  Cachorro cachorro = Cachorro();
  Gato gato = Gato();

  cachorro.fazerSom(); // Output: Latido
  gato.fazerSom();     // Output: Miau
}
```
<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Programação Orientada a Objetos

- [Introdução](#introdução)

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
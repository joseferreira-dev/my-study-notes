# Capítulo 20 – Classes: A Sintaxe Moderna para a Orientação a Objetos

Nos capítulos anteriores, desvendamos as engrenagens internas do JavaScript, mergulhando fundo em objetos e no poderoso, porém complexo, mecanismo de herança via cadeia de protótipos. Vimos como as funções construtoras nos permitiram simular "classes", criando um sistema para gerar objetos que compartilham comportamento. No entanto, para desenvolvedores vindos de linguagens como Java, C# ou Python, essa abordagem sempre pareceu um pouco verbosa e indireta. A necessidade de manipular diretamente a propriedade `.prototype` para adicionar métodos era uma fonte comum de erros e confusão.

Reconhecendo essa barreira, o ECMAScript 2015 (ES6) introduziu uma das suas funcionalidades mais celebradas e transformadoras: a sintaxe de **`class`**. É crucial entender desde o início que as classes em JavaScript **não** introduziram um novo modelo de herança. Elas são, em grande parte, o que chamamos de **"açúcar sintático" (syntactic sugar)** sobre o sistema de herança prototípica existente. Elas não substituem os protótipos; em vez disso, elas fornecem uma maneira muito mais limpa, clara e familiar de se trabalhar com eles.

Neste capítulo, vamos dominar essa sintaxe moderna. Começaremos com a definição básica de uma classe, entendendo o papel central do método `constructor`. Aprenderemos a definir métodos de instância, métodos estáticos que pertencem à classe em si, e a criar propriedades computadas com getters e setters. Em seguida, exploraremos o mecanismo de herança com as palavras-chave `extends` e `super`, que simplificaram drasticamente a criação de hierarquias de objetos. Um foco especial será dado ao tópico do **encapsulamento**, onde investigaremos as abordagens clássicas e, finalmente, a solução moderna com campos de classe privados (`#`), que nos permite esconder detalhes de implementação de forma verdadeiramente eficaz. Ao final, você terá uma visão completa de como usar classes para construir aplicações robustas, organizadas e alinhadas com os melhores padrões da programação orientada a objetos.

## A Anatomia de uma Classe

Uma classe é um "molde" ou "planta" para a criação de objetos. Ela define as propriedades e os métodos que os objetos criados a partir dela (as **instâncias**) terão.

**Sintaxe Básica:**

```js
class Produto {
  // O construtor é um método especial para inicializar as instâncias.
  constructor(nome, preco) {
    // 'this' refere-se à nova instância que está sendo criada.
    this.nome = nome;
    this.preco = preco;
    this.emEstoque = true;
  }

  // Um método da classe
  exibirDetalhes() {
    console.log(`Produto: ${this.nome}, Preço: R$ ${this.preco.toFixed(2)}`);
  }
}

// Para criar um objeto a partir da classe, usamos o operador 'new'.
const produto1 = new Produto("Laptop Gamer", 4500.50);
const produto2 = new Produto("Mouse sem Fio", 150.00);

produto1.exibirDetalhes(); // "Produto: Laptop Gamer, Preço: R$ 4500.50"
produto2.exibirDetalhes(); // "Produto: Mouse sem Fio, Preço: R$ 150.00"
```

**Pontos Importantes sobre Classes:**

- **Classes são Funções:** Por baixo dos panos, uma classe é um tipo especial de função. `typeof Produto` retornaria `"function"`.
- **Não sofrem Hoisting:** Diferente das declarações de função, as declarações de classe não são "içadas". Você deve declarar uma classe antes de poder usá-la.
- **Modo Estrito (Strict Mode):** O código dentro do corpo de uma classe é automaticamente executado em modo estrito.

### O Método `constructor`

O `constructor` é um método especial com um propósito único: inicializar um objeto recém-criado.

- Ele é executado automaticamente quando uma nova instância é criada com `new`.
- Se você não definir um `constructor`, o JavaScript fornecerá um construtor vazio padrão: `constructor() {}`.
- Uma classe pode ter **apenas um** método com o nome `constructor`.

## Métodos: Definindo o Comportamento

Métodos são funções que pertencem a uma classe e definem o comportamento de suas instâncias. A sintaxe para defini-los é limpa e direta, sem a necessidade da palavra-chave `function`.

```js
class Contador {
  constructor() {
    this.valor = 0;
  }

  incrementar() {
    this.valor++;
  }

  resetar() {
    this.valor = 0;
  }
}
```

Esses métodos (`incrementar`, `resetar`) são adicionados automaticamente ao `Contador.prototype`, garantindo que todas as instâncias compartilhem a mesma implementação de método, o que é eficiente em termos de memória.

### Nomes de Métodos Dinâmicos

Assim como em objetos literais, você pode usar a sintaxe de propriedade computada (`[]`) para definir nomes de métodos dinamicamente a partir de variáveis.

```js
const nomeDoMetodo = 'executar';

class Tarefa {
  [nomeDoMetodo](param) {
    console.log(`Tarefa executada com o parâmetro: ${param}`);
  }
}

const minhaTarefa = new Tarefa();
minhaTarefa.executar("Dados Importantes"); // "Tarefa executada com o parâmetro: Dados Importantes"
```

### Encademento de Métodos (Method Chaining)

Um padrão de design poderoso é o encadeamento de métodos, que permite chamar múltiplos métodos em um objeto em uma única instrução. Para habilitar isso, um método simplesmente precisa retornar a própria instância (`this`).

```js
class Calculadora {
  constructor(valorInicial = 0) {
    this.resultado = valorInicial;
  }

  somar(num) {
    this.resultado += num;
    return this; // Retorna a própria instância para permitir o encadeamento
  }

  subtrair(num) {
    this.resultado -= num;
    return this;
  }
}

const calc = new Calculadora();
const resultadoFinal = calc.somar(10).subtrair(3).somar(5); // 10 - 3 + 5

console.log(resultadoFinal.resultado); // 12
```

## Getters e Setters: Propriedades de Acesso

Getters (`get`) e setters (`set`) permitem definir métodos que se comportam como propriedades. Eles são úteis para criar propriedades computadas ou para adicionar validação ao ler ou escrever um valor.

```js
class Usuario {
  constructor(primeiroNome, ultimoNome) {
    this.primeiroNome = primeiroNome;
    this.ultimoNome = ultimoNome;
  }

  // Getter para uma propriedade computada 'nomeCompleto'
  get nomeCompleto() {
    return `${this.primeiroNome} ${this.ultimoNome}`;
  }

  // Setter para permitir a alteração do nome completo
  set nomeCompleto(valor) {
    if (typeof valor !== 'string' || valor.indexOf(' ') === -1) {
      console.error("Erro: Forneça um nome completo com nome e sobrenome.");
      return;
    }
    const [primeiro, ...resto] = valor.split(' ');
    this.primeiroNome = primeiro;
    this.ultimoNome = resto.join(' ');
  }
}

const user = new Usuario("João", "Silva");
console.log(user.nomeCompleto); // Acessado como uma propriedade: "João Silva"

user.nomeCompleto = "Maria Souza Oliveira";
console.log(user.primeiroNome); // "Maria"
console.log(user.ultimoNome); // "Souza Oliveira"
```

## Membros Estáticos: Propriedades da Classe

Membros estáticos (métodos ou propriedades) pertencem à **classe em si**, e não a qualquer instância individual. Eles são chamados diretamente na classe, e não em um objeto criado a partir dela. São ideais para funções utilitárias ou constantes relacionadas à classe.

```js
class Matematica {
  static PI = 3.14159;

  static somar(a, b) {
    return a + b;
  }

  static ehPar(num) {
    return num % 2 === 0;
  }
}

console.log(Matematica.PI); // 3.14159
console.log(Matematica.somar(5, 7)); // 12

// const m = new Matematica();
// m.somar(1, 2); // TypeError: m.somar is not a function
```

## Herança: `extends` e `super`

A herança é um pilar da programação orientada a objetos que permite que uma classe (a **subclasse** ou classe filha) herde propriedades e métodos de outra classe (a **superclasse** ou classe pai). A sintaxe de `class` torna isso extremamente intuitivo.

- **`extends`:** A palavra-chave usada para estabelecer a relação de herança.
- **`super`:** Uma palavra-chave especial que permite acessar membros da classe pai.

**Exemplo de Herança:**

```js
class Veiculo {
  constructor(marca) {
    this.marca = marca;
    this.velocidade = 0;
  }

  acelerar(valor) {
    this.velocidade += valor;
    console.log(`Veículo acelerando para ${this.velocidade} km/h.`);
  }
}

// 'Carro' herda de 'Veiculo'
class Carro extends Veiculo {
  constructor(marca, modelo) {
    // 1. Chamando o construtor da classe pai com super()
    // Isso é OBRIGATÓRIO antes de usar 'this' em uma subclasse.
    super(marca);
    this.modelo = modelo;
  }

  // Sobrescrevendo o método da classe pai
  acelerar(valor) {
    console.log(`Carro ${this.modelo} está acelerando...`);
    // 2. Chamando o método da classe pai com super.metodo()
    super.acelerar(valor);
  }
}

const meuCarro = new Carro("Ford", "Mustang");
meuCarro.acelerar(100);
// Saída:
// Carro Mustang está acelerando...
// Veículo acelerando para 100 km/h.
```

## Encapsulamento: Gerenciando Dados Privados

Encapsulamento é o princípio de agrupar dados (propriedades) e os métodos que operam sobre esses dados, escondendo os detalhes de implementação do mundo exterior. Por muito tempo, o JavaScript não teve um mecanismo nativo para propriedades privadas, levando a diversos padrões.

### Padrão 1: Convenção com Underscore (`_`)

A abordagem mais simples e antiga é uma convenção de nomenclatura. Propriedades que começam com `_` são consideradas "privadas" por outros desenvolvedores, que devem evitar acessá-las ou modificá-las diretamente.

```js
class Conta {
  constructor(saldoInicial) {
    this._saldo = saldoInicial; // _ indica que é 'privada'
  }

  depositar(valor) {
    this._saldo += valor;
  }
}

const minhaConta = new Conta(1000);
minhaConta._saldo = -5000; // Nada impede isso! A privacidade é apenas uma convenção.
```

### Padrões com `WeakMap` ou `Symbol`

Como vimos em capítulos anteriores, `WeakMap` e `Symbol` podem ser usados para simular privacidade de forma mais robusta, mas com uma sintaxe mais complexa.

### Padrão Moderno: Campos de Classe Privados (`#`)

O ECMAScript 2022 introduziu uma sintaxe nativa para membros privados usando o prefixo `#`. Isso garante **privacidade real**. A propriedade não pode ser acessada ou detectada de fora da classe.

- **Campos Privados:** `#nomeDoCampo`
- **Métodos Privados:** `#nomeDoMetodo()`

```js
class ContaBancaria {
  // #saldo é um campo verdadeiramente privado.
  #saldo = 0;

  constructor(saldoInicial) {
    if (saldoInicial > 0) {
      this.#saldo = saldoInicial;
    }
  }

  // Método público que acessa o campo privado
  getSaldo() {
    return this.#saldo;
  }

  depositar(valor) {
    if (valor > 0) {
      this.#saldo += valor;
      this.#logTransacao('Depósito', valor); // Chamando um método privado
    }
  }

  // Método privado
  #logTransacao(tipo, valor) {
    console.log(`${tipo} de R$ ${valor.toFixed(2)} realizado.`);
  }
}

const conta = new ContaBancaria(500);
console.log(conta.getSaldo()); // 500

conta.depositar(150); // "Depósito de R$ 150.00 realizado."
console.log(conta.getSaldo()); // 650

// Tentativas de acesso direto resultam em erro.
// console.log(conta.#saldo); // SyntaxError
// conta.#logTransacao();   // SyntaxError
```

Este é o padrão **recomendado** para se alcançar encapsulamento em código JavaScript moderno.

## Considerações Finais

Neste capítulo, desvendamos a sintaxe de `class` do ES6, uma poderosa abstração que trouxe clareza e uma estrutura familiar para a programação orientada a objetos em JavaScript. Vimos que, por baixo dos panos, as classes são uma maneira mais elegante de se trabalhar com a herança prototípica, automatizando a configuração da cadeia de protótipos e a adição de métodos.

Exploramos a criação de classes com o `constructor`, a definição de `métodos` para comportamento, `getters` e `setters` para propriedades computadas, e `membros estáticos` que pertencem à classe como um todo. Dominamos a herança com as palavras-chave `extends` e `super`, que nos permitem criar hierarquias de classes de forma intuitiva.

O ponto crucial foi nossa exploração do encapsulamento. Discutimos os padrões históricos e celebramos a chegada dos **campos de classe privados (`#`)**, que finalmente nos deram um mecanismo nativo e robusto para esconder detalhes de implementação e proteger o estado de nossos objetos. Com as classes, o JavaScript não apenas se tornou mais acessível para desenvolvedores de outras linguagens, mas também forneceu a todos nós ferramentas mais poderosas para construir aplicações complexas, modulares e de fácil manutenção.
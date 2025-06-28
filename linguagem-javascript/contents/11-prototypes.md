# Capítulo 11 – Prototypes: A Essência da Herança em JavaScript

Nos capítulos anteriores, desvendamos os objetos, a estrutura de dados fundamental do JavaScript. Aprendemos a criá-los, a manipulá-los e a controlar suas propriedades com precisão. Agora, estamos prontos para explorar o mecanismo que está no coração do modelo de orientação a objetos do JavaScript, um conceito que o diferencia da maioria das outras linguagens e que é a chave para a reutilização de código e a organização de sistemas complexos: o **Prototype** (protótipo).

Muitos desenvolvedores vêm de linguagens com um modelo de herança "clássica", onde uma classe herda de outra, criando uma cópia de funcionalidades. O JavaScript, no entanto, opera sob um paradigma diferente e, de muitas formas, mais flexível: a **herança prototípica**. Em vez de copiar, os objetos em JavaScript podem ser **ligados** a outros objetos. Quando você tenta acessar uma propriedade em um objeto, se ela não for encontrada, o motor do JavaScript automaticamente olha para o objeto ao qual ele está ligado — seu protótipo — e continua essa busca para cima, formando uma **cadeia de protótipos (prototype chain)**. É um modelo de delegação, não de cópia.

Este capítulo é uma imersão profunda neste mecanismo central. Vamos dissecar o que é um protótipo e como essa cadeia de delegação funciona na prática. Exploraremos as diferentes maneiras de se criar um objeto com um protótipo específico, desde a abordagem clássica com funções construtoras até a sintaxe moderna e mais direta. Investigaremos como manipular e inspecionar a cadeia de protótipos, e faremos a distinção crucial entre o protótipo de um objeto e a propriedade `.prototype` de uma função. Ao final, o que antes parecia mágico — como um simples objeto ter acesso a métodos como `toString()` — será revelado como uma elegante e poderosa mecânica de delegação que, uma vez dominada, elevará sua compreensão do JavaScript a um novo patamar.

## O Conceito Fundamental: O que é um Protótipo?

Em sua essência, um protótipo é apenas um outro objeto que serve como um "molde" ou "plano de fallback" para um objeto existente. Cada objeto em JavaScript possui uma propriedade interna, formalmente chamada `[[Prototype]]`, que é um link (ou uma referência) para outro objeto, ou para `null`. Este objeto referenciado é o seu protótipo.

O poder desse link se manifesta quando tentamos acessar uma propriedade. O processo funciona da seguinte forma:

1. Você tenta acessar uma propriedade em um objeto (ex: `meuObjeto.minhaProp`).
2. O motor do JavaScript primeiro verifica se a propriedade `minhaProp` existe **diretamente no `meuObjeto`**. Se sim, ele retorna seu valor e a busca termina.
3. Se a propriedade não for encontrada, o motor segue o link `[[Prototype]]` e busca a propriedade no objeto protótipo. Se encontrada lá, seu valor é retornado.
4. Se ainda assim não for encontrada, e se o protótipo tiver seu próprio protótipo, o processo continua, subindo na **cadeia de protótipos** até encontrar a propriedade ou até chegar ao final da cadeia.
5. O final da cadeia é sempre um objeto cujo protótipo é `null`. O objeto que fica no topo da maioria das cadeias é o `Object.prototype`, que contém métodos comuns a todos os objetos, como `toString()`, `hasOwnProperty()`, etc.

Essa delegação é a razão pela qual podemos chamar `meuObjeto.toString()` mesmo sem nunca termos definido esse método em `meuObjeto`. A chamada é delegada para cima na cadeia até ser encontrada em `Object.prototype`.

### Acessando o Protótipo de um Objeto

A propriedade `[[Prototype]]` é interna e não deve ser acessada diretamente. Para interagir com ela, usamos métodos padrão da linguagem:

- **`Object.getPrototypeOf(obj)`:** A forma moderna e recomendada para ler o protótipo de um objeto.
- **`__proto__` (duplo underscore):** Uma propriedade de acesso não-padrão (mas suportada por todos os navegadores modernos) que expõe o `[[Prototype]]`. Embora útil para depuração, **seu uso em código de produção é fortemente desaconselhado**.

```js
const animal = {
  comer() {
    console.log("Comendo...");
  }
};

const cachorro = {
  latir() {
    console.log("Au au!");
  }
};

// Definindo 'animal' como o protótipo de 'cachorro'
Object.setPrototypeOf(cachorro, animal);

// Verificando a cadeia
console.log(Object.getPrototypeOf(cachorro) === animal); // true

// Testando a delegação
cachorro.latir(); // "Au au!" (encontrado no próprio objeto)
cachorro.comer(); // "Comendo..." (delegado para o protótipo 'animal')
```

## Criando Objetos com Protótipos Específicos

Existem várias maneiras de se estabelecer essa ligação prototípica ao criar um objeto.

### `Object.create(proto)`: A Abordagem Direta

Este método, introduzido no ES5, é a forma mais explícita e direta de se criar um novo objeto com um protótipo especificado.

```js
const veiculo = {
  mover() {
    console.log("O veículo está se movendo.");
  }
};

// Cria um novo objeto 'carro' cujo [[Prototype]] é 'veiculo'
const carro = Object.create(veiculo);
carro.marca = "Toyota";

carro.mover(); // "O veículo está se movendo." (herdado)
console.log(carro.marca); // "Toyota" (propriedade própria)

console.log(Object.getPrototypeOf(carro) === veiculo); // true
```

`Object.create()` também pode receber um segundo argumento opcional, um objeto de descritores de propriedade, para adicionar novas propriedades ao objeto recém-criado com controle total.

### Funções Construtoras: A Abordagem "Clássica"

Antes do ES6, a principal forma de se criar objetos com um protótipo compartilhado era através de **funções construtoras**. Essa abordagem simula a criação de "classes" e é fundamental para entender o código JavaScript mais antigo e muitas bibliotecas.

Uma função construtora é uma função normal, mas que é chamada com o operador `new`. Por convenção, seu nome começa com letra maiúscula.

```js
// Função Construtora
function Pessoa(nome, idade) {
  // 'this' refere-se à nova instância que será criada
  this.nome = nome;
  this.idade = idade;
}

// Adicionando um método ao PROTÓTIPO da função
Pessoa.prototype.saudacao = function() {
  console.log(`Olá, meu nome é ${this.nome}.`);
};

// Criando instâncias com o operador 'new'
const pessoa1 = new Pessoa("Ana", 30);
const pessoa2 = new Pessoa("Carlos", 25);

pessoa1.saudacao(); // "Olá, meu nome é Ana."
pessoa2.saudacao(); // "Olá, meu nome é Carlos."
```

O que o operador `new` faz por trás dos panos?

1. Cria um novo objeto vazio: `{}`.
2. Liga (define) o `[[Prototype]]` desse novo objeto para ser o objeto referenciado por `Pessoa.prototype`.
3. Chama a função `Pessoa`, passando o novo objeto como o valor de `this`.
4. Retorna o novo objeto (a menos que a função retorne explicitamente outro objeto).

#### `Constructor.prototype`: O Ponto Chave

Este é um dos pontos mais importantes e que mais causam confusão:

- **Toda função em JavaScript tem uma propriedade especial chamada `.prototype`**.
- Esta propriedade **não** é o protótipo da **função em si**. O protótipo da função é `Function.prototype`.
- A propriedade `FuncaoConstrutora.prototype` é um **objeto** que será atribuído como o `[[Prototype]]` de todas as **instâncias** criadas com `new FuncaoConstrutora()`.

É por isso que adicionamos `saudacao` a `Pessoa.prototype`. Ao fazer isso, garantimos que `pessoa1` e `pessoa2` compartilhem o mesmo método `saudacao` através da cadeia de protótipos, em vez de cada instância ter sua própria cópia da função, o que é muito mais eficiente em termos de memória.

```js
// A propriedade 'saudacao' não está em pessoa1, mas em seu protótipo.
console.log(pessoa1.hasOwnProperty('saudacao')); // false
console.log(Object.getPrototypeOf(pessoa1).hasOwnProperty('saudacao')); // true
```

### ES6 Classes: A Sintaxe Moderna

As classes, introduzidas no ES6, são em grande parte "açúcar sintático" sobre o sistema de herança prototípica existente. Elas não introduziram um novo modelo de objetos, mas forneceram uma sintaxe muito mais limpa e familiar para implementar o mesmo padrão das funções construtoras.

```js
class Animal {
  constructor(nome) {
    this.nome = nome;
  }

  falar() {
    throw new Error("O método 'falar' deve ser implementado.");
  }
}

class Gato extends Animal {
  falar() {
    console.log(`${this.nome} mia.`);
  }
}

const gato = new Gato("Frajola");
gato.falar(); // "Frajola mia."

// A "mágica" ainda é a cadeia de protótipos:
console.log(Object.getPrototypeOf(gato) === Gato.prototype); // true
console.log(Object.getPrototypeOf(Gato.prototype) === Animal.prototype); // true
```

A palavra-chave `class` automatiza o processo de configurar a cadeia de protótipos, tornando o código mais legível e menos propenso a erros.

## Manipulando e Verificando a Cadeia de Protótipos

- **`Object.setPrototypeOf(obj, prototype)`:** A forma padrão para **alterar** o protótipo de um objeto existente. **Atenção:** Mudar o protótipo de um objeto em tempo de execução é uma operação muito lenta e deve ser evitada em código que exige desempenho. Geralmente, o protótipo deve ser estabelecido na criação do objeto.
- **`obj.isPrototypeOf(outroObj)`:** Retorna `true` se `obj` existe na cadeia de protótipos de `outroObj`. É a forma mais robusta de se verificar a relação prototípica.
    
    ```js
    const animal = { tipo: 'mamífero' };
    const cachorro = Object.create(animal);
    
    console.log(animal.isPrototypeOf(cachorro)); // true
    console.log(Object.prototype.isPrototypeOf(cachorro)); // true
    ```
    
- **`instanceof`:** O operador `instanceof` testa se a propriedade `prototype` de uma função construtora aparece em algum lugar na cadeia de protótipos de um objeto.
    
    ```js
    function Cao() {}
    const rex = new Cao();
    
    console.log(rex instanceof Cao); // true
    console.log(rex instanceof Object); // true (pois Object.prototype está na cadeia)
    ```
    
    `instanceof` e `isPrototypeOf` são formas relacionadas de responder à mesma pergunta, mas a partir de perspectivas diferentes.

## Distinção Crucial: Objeto vs. Protótipo

É vital solidificar a diferença entre um objeto e seu protótipo.

- Um **objeto** é uma instância concreta que contém suas **próprias propriedades**. Estas são as características únicas daquela instância (ex: o `nome` e a `idade` de `pessoa1`).
- O **protótipo** é um objeto separado que atua como um repositório de **propriedades e métodos compartilhados**. Todas as instâncias que compartilham o mesmo protótipo delegam a ele a busca por qualquer propriedade que não possuam.

Pense no `cachorro` do nosso primeiro exemplo. Suas características únicas (como nome, raça, idade) estariam diretamente no objeto `cachorro`. As características compartilhadas por todos os animais (como `comer()`, `respirar()`) estariam no protótipo `animal`. A ligação `[[Prototype]]` é o que permite que o `cachorro` saiba "como comer" sem precisar ter essa lógica definida dentro de si mesmo.

## Tópicos Importantes e Padrões

### Shadowing (Sombreamento) de Propriedades

O que acontece se você define uma propriedade em uma instância com o mesmo nome de uma propriedade que existe em seu protótipo? A propriedade da instância **sombreia** a do protótipo.

```js
const pai = {
  nome: 'Pai'
};

const filho = Object.create(pai);
filho.nome = 'Filho'; // Esta propriedade 'sombreia' a do protótipo

console.log(filho.nome); // "Filho" (encontrado diretamente no objeto)
console.log(pai.nome);   // "Pai" (o protótipo permanece inalterado)

delete filho.nome; // Remove a propriedade de sombreamento
console.log(filho.nome); // "Pai" (agora a busca é delegada ao protótipo)
```

### A Propriedade `constructor`

Por padrão, todo objeto `.prototype` criado para uma função construtora recebe automaticamente uma propriedade chamada `constructor`, que aponta de volta para a própria função construtora.

```js
function Carro(marca) {
  this.marca = marca;
}

console.log(Carro.prototype.constructor === Carro); // true

const meuCarro = new Carro('Ford');
console.log(meuCarro.constructor); // ƒ Carro(marca) { ... }
```

Isso pode ser útil para identificar o "tipo" de um objeto ou até para criar novas instâncias do mesmo tipo dinamicamente.

## Considerações Finais

Neste capítulo, desvendamos o mecanismo de herança do JavaScript: a **cadeia de protótipos**. Vimos que, em vez de uma herança clássica baseada em cópias, o JavaScript utiliza um modelo de **delegação**, onde os objetos são ligados uns aos outros e delegam a busca por propriedades não encontradas.

Exploramos as formas de estabelecer essa ligação, desde a abordagem "clássica" com **funções construtoras** e sua propriedade `.prototype`, até a sintaxe moderna e mais clara das **classes** do ES6, entendendo que ambas são, no fundo, abstrações sobre o mesmo sistema de protótipos. Aprendemos a inspecionar e manipular essa cadeia e a diferenciar os conceitos de instância e protótipo.

Compreender a herança prototípica é fundamental para dominar o JavaScript. Ela explica como objetos compartilham funcionalidades de forma eficiente, como a linguagem é estruturada, e por que certos padrões de código funcionam como funcionam. Armado com este conhecimento, você não está mais apenas usando objetos; você está compreendendo e aproveitando a arquitetura central da própria linguagem.
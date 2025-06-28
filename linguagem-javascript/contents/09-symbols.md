# Capítulo 9 – Symbols

Em nossa exploração do JavaScript, já dominamos os tipos primitivos que formam a base da informação: `string`, `number`, `boolean`, `null` e `undefined`. Também mergulhamos fundo nos objetos e arrays, que nos permitem estruturar e agrupar esses dados de formas complexas. No entanto, o ECMAScript 2015 (ES6) introduziu um novo e peculiar tipo primitivo que não serve para armazenar dados do mundo real, mas para resolver um problema fundamental na engenharia de software: a **colisão de chaves de propriedade**.

Imagine um cenário comum: você está utilizando uma biblioteca de terceiros que lhe fornece um objeto, e você precisa adicionar alguma informação de metadados a esse objeto para uso interno em sua aplicação. Se você simplesmente adicionar uma propriedade com uma chave de string, como `objeto.id = 123`, como pode garantir que essa biblioteca, agora ou em uma futura atualização, não decida usar uma propriedade com o mesmo nome `id`, sobrescrevendo seu valor ou quebrando sua lógica? Este é o problema da colisão de propriedades. Para resolvê-lo, o JavaScript nos deu o **Symbol**.

Neste capítulo, vamos desvendar este tipo de dado único. Começaremos por entender sua característica definidora: a capacidade de criar identificadores que são garantidamente únicos em todo o programa. Aprenderemos a criar e utilizar symbols como chaves de propriedade, explorando como eles interagem com mecanismos de iteração e serialização de objetos, o que lhes confere uma natureza "pseudo-privada". Investigaremos o registro global de symbols, que nos permite criar identificadores compartilhados em diferentes partes de uma aplicação. E, o mais fascinante, descobriremos os **Well-Known Symbols**, que nos dão o poder de "enganar" e modificar o comportamento interno e fundamental da própria linguagem JavaScript.

## O Que é um Symbol? A Necessidade de Identificadores Únicos

Um **Symbol** é um tipo de dado primitivo, assim como `string` ou `number`. No entanto, seu propósito é completamente diferente. O valor de um symbol é um **identificador único e imutável**. Cada vez que você cria um novo symbol, você está criando um valor que é garantidamente diferente de qualquer outro valor já criado no programa, incluindo outros symbols.

A principal característica que define um symbol é: `Symbol() !== Symbol()`

Mesmo que dois symbols sejam criados com a mesma descrição, eles nunca serão iguais.

```js
const id1 = Symbol('id');
const id2 = Symbol('id');

console.log(id1 === id2); // false
console.log(typeof id1); // "symbol"
```

Essa característica resolve de forma elegante o problema da colisão de propriedades. Uma chave de propriedade do tipo `string` é comparada pelo seu conteúdo (`'nome' === 'nome'`), mas uma chave do tipo `symbol` é comparada por sua identidade única.

**Exemplo de Colisão com String vs. Segurança com Symbol:**

```js
// Cenário de colisão com string
const usuario = { nome: 'Ana' };
function adicionarIdLib(obj) {
  obj.id = 'lib-xyz-123'; // Biblioteca adiciona seu ID
}
function adicionarIdApp(obj) {
  obj.id = 98765; // Nossa aplicação adiciona seu ID
}

adicionarIdLib(usuario);
adicionarIdApp(usuario); // OPS! Sobrescreveu o ID da biblioteca.
console.log(usuario.id); // 98765

// Cenário seguro com Symbol
const usuario2 = { nome: 'Bia' };
const idDaLib = Symbol('id da biblioteca');
const idDaApp = Symbol('id da aplicação');

function adicionarIdLibComSymbol(obj) {
  obj[idDaLib] = 'lib-xyz-123';
}
function adicionarIdAppComSymbol(obj) {
  obj[idDaApp] = 98765;
}

adicionarIdLibComSymbol(usuario2);
adicionarIdAppComSymbol(usuario2);

// As propriedades coexistem sem colisão
console.log(usuario2[idDaLib]); // 'lib-xyz-123'
console.log(usuario2[idDaApp]); // 98765
```

## Criação e Uso Básico de Symbols

Para criar um symbol, utiliza-se a função global `Symbol()`. Note que ela é uma função, não um construtor, então não se deve usar `new Symbol()`.

A função `Symbol()` pode receber um argumento opcional do tipo `string`, que serve como uma **descrição**. Essa descrição não tem impacto na unicidade do symbol; sua única finalidade é facilitar a depuração.

```js
const symSemDescricao = Symbol();
const symComDescricao = Symbol('uma descrição útil para debug');

console.log(symComDescricao); // Symbol(uma descrição útil para debug)
```

### Usando Symbols como Chaves de Propriedade

Para usar um symbol como chave de propriedade em um objeto, é necessário usar a sintaxe de colchetes `[ ]`. A notação de ponto (`objeto.propriedade`) não funciona, pois ela espera uma chave do tipo string.

```js
const id = Symbol('identificador único');

// Usando em um objeto literal
const produto = {
  nome: 'Laptop',
  preco: 4500,
  [id]: 'prod-a9b8c7' // Chave computada com symbol
};

// Adicionando a um objeto existente
produto[Symbol('estoque')] = 150;

// Acessando a propriedade
console.log(produto[id]); // 'prod-a9b8c7'

// Tentativa de acesso com notação de ponto (não funciona)
// console.log(produto.id); // undefined
```

## A "Privacidade" das Propriedades de Symbol

Uma das características mais interessantes das propriedades com chave de symbol é que elas não são enumeradas pelos mecanismos de iteração mais comuns. Isso lhes confere uma qualidade "escondida" ou "pseudo-privada", tornando-as ideais para armazenar metadados ou funcionalidades internas sem "poluir" o objeto para o código externo que pode não estar ciente delas.

Propriedades de symbol **não** são incluídas nos seguintes métodos:

- Laço `for...in`
- `Object.keys()`
- `Object.getOwnPropertyNames()`
- `JSON.stringify()`

```js
const idSecreto = Symbol('id');
const pessoa = {
  nome: 'Carlos',
  idade: 42,
  [idSecreto]: 'user-xyz-001'
};

// Iterando com for...in
for (const chave in pessoa) {
  console.log(chave); // 'nome', 'idade' (o symbol é ignorado)
}

// Obtendo chaves com Object.keys()
console.log(Object.keys(pessoa)); // ['nome', 'idade']

// Convertendo para JSON
console.log(JSON.stringify(pessoa)); // '{"nome":"Carlos","idade":42}'
```

Essa característica é extremamente útil. Significa que você pode adicionar uma propriedade de symbol a um objeto de terceiros com a confiança de que não irá interferir com laços `for...in` ou serializações JSON que esse código possa realizar.

### Como Acessar Propriedades de Symbol

Se elas são "escondidas", como podemos listá-las? O JavaScript oferece dois métodos para isso:

- **`Object.getOwnPropertySymbols(obj)`:** Retorna um array contendo apenas as chaves de symbol de um objeto.
- **`Reflect.ownKeys(obj)`:** Retorna um array contendo **todas** as chaves próprias de um objeto, tanto strings quanto symbols.

```js
console.log(Object.getOwnPropertySymbols(pessoa)); // [Symbol(id)]

const chaveSymbol = Object.getOwnPropertySymbols(pessoa)[0];
console.log(pessoa[chaveSymbol]); // 'user-xyz-001'

console.log(Reflect.ownKeys(pessoa)); // ['nome', 'idade', Symbol(id)]
```

## O Registro Global de Symbols: `Symbol.for()` e `Symbol.keyFor()`

Vimos que `Symbol()` sempre cria um symbol novo e único. Mas e se precisarmos do **mesmo** symbol em diferentes partes de uma aplicação, ou até mesmo em diferentes `realms` (como a página principal e um `iframe`)? Para isso, existe o **registro global de symbols**.

- **`Symbol.for(chave)`:** Este método procura no registro global por um symbol com a `chave` fornecida.
    - Se um symbol com essa chave já existir, ele o retorna.
    - Se não existir, ele cria um novo symbol com essa chave, o armazena no registro global e o retorna.

Isso garante que, para uma mesma chave, `Symbol.for()` sempre retornará o mesmo symbol.

```js
const s1 = Symbol.for('app.id');
const s2 = Symbol.for('app.id');

console.log(s1 === s2); // true (ambos apontam para o mesmo symbol no registro)

const s3 = Symbol('app.id'); // Cria um symbol local, não registrado
console.log(s1 === s3); // false
```

- **`Symbol.keyFor(symbol)`:** Faz o caminho inverso. Dado um symbol que foi criado com `Symbol.for()`, ele retorna a `chave` (string) associada a ele no registro. Se o symbol não for do registro global, ele retorna `undefined`.

```js
const idApp = Symbol.for('identificador.global.app');
console.log(Symbol.keyFor(idApp)); // "identificador.global.app"

const idLocal = Symbol('id local');
console.log(Symbol.keyFor(idLocal)); // undefined
```

## Well-Known Symbols: Modificando o Comportamento Interno do JavaScript

Esta é, talvez, a aplicação mais avançada e poderosa dos symbols. O próprio JavaScript define uma série de symbols globais e estáticos no objeto `Symbol`, conhecidos como **Well-Known Symbols**. Eles funcionam como "ganchos" (hooks) que permitem aos desenvolvedores redefinir ou customizar o comportamento de operações fundamentais da linguagem em seus próprios objetos.

Ao implementar um método em um objeto usando um Well-Known Symbol como chave, você está dizendo ao motor do JavaScript como ele deve se comportar ao realizar certas operações com aquele objeto.

### `Symbol.iterator`: Tornando um Objeto Iterável

No capítulo anterior, vimos o laço `for...of`. Ele funciona com arrays porque arrays possuem, por padrão, um método definido na chave `Symbol.iterator`. Podemos usar esse mesmo mecanismo para tornar qualquer objeto nosso iterável.

```js
const meuObjeto = {
  dados: ['a', 'b', 'c'],
  [Symbol.iterator]: function* () {
    for (let i = 0; i < this.dados.length; i++) {
      yield this.dados[i];
    }
  }
};

// Agora o objeto é iterável e podemos usar o for...of
for (const item of meuObjeto) {
  console.log(item); // a, b, c
}

// E também o operador Spread
console.log([...meuObjeto]); // ['a', 'b', 'c']
```

### `Symbol.toPrimitive`: Customizando a Conversão para Primitivos

Quando o JavaScript precisa converter um objeto para um valor primitivo (por exemplo, em uma operação matemática `objeto + 10` ou ao exibir com `alert(objeto)`), ele procura por um método na chave `Symbol.toPrimitive`.

```js
class Moeda {
  constructor(valor) {
    this.valor = valor;
  }

  [Symbol.toPrimitive](hint) {
    if (hint === 'number') {
      return this.valor;
    }
    if (hint === 'string') {
      return `R$ ${this.valor.toFixed(2)}`;
    }
    // hint 'default'
    return this.valor;
  }
}

const preco = new Moeda(49.90);

console.log(preco + 10);     // 59.9 (hint foi 'number')
console.log(`O preço é ${preco}.`); // O preço é R$ 49.90. (hint foi 'string')
```

### `Symbol.toStringTag`: Alterando a Tag de Tipo

O método `Object.prototype.toString.call(obj)` retorna uma string como `"[object Array]"`. A parte `Array` é a "tag de tipo". Podemos customizar essa tag para nossas próprias classes usando `Symbol.toStringTag`.

```js
class Validador {
  get [Symbol.toStringTag]() {
    return 'Validador';
  }
}

const meuValidador = new Validador();
console.log(Object.prototype.toString.call(meuValidador)); // "[object Validador]"
```

Isso é extremamente útil para bibliotecas e frameworks, pois permite uma introspecção de tipo mais precisa. Outros Well-Known Symbols incluem `Symbol.hasInstance` (customiza `instanceof`), `Symbol.search`, `Symbol.replace` (permite que um objeto defina sua própria lógica de busca e substituição), e muitos outros.

## Outras Considerações e Métodos

### Convertendo um Symbol em String

Uma tentativa de converter um symbol para string de forma implícita (como na concatenação) resulta em um `TypeError`. Isso é uma medida de segurança para evitar que um identificador único seja acidentalmente transformado em uma chave de string não-única.

```js
const sym = Symbol('teste');
// const texto = "Meu symbol é " + sym; // TypeError: Cannot convert a Symbol value to a string
```

No entanto, a conversão explícita é permitida e útil para depuração:

- **`String(sym)`:** Funciona e retorna a string `Symbol(descrição)`.
- **`sym.toString()`:** Tem o mesmo efeito.
- **`sym.description` (ES2019):** A forma moderna de se obter apenas a descrição do symbol como uma string.

```js
const sym = Symbol('meu-id');
console.log(String(sym));       // "Symbol(meu-id)"
console.log(sym.toString());    // "Symbol(meu-id)"
console.log(sym.description);   // "meu-id"
```

## Aplicações Práticas e Quando Usar Symbols

Resumindo, os principais casos de uso para symbols são:

- **Chaves de Propriedade Não-Colisivas:** Este é o principal motivo de sua existência. Use-os para adicionar propriedades a objetos (especialmente objetos que você não controla) com a garantia de que suas chaves nunca irão colidir com outras chaves existentes ou futuras. É ideal para armazenar metadados ou estado interno.
- **Constantes Únicas (Enums):** Em muitas linguagens, usa-se `enums` para representar um conjunto fixo de valores constantes (ex: estados de uma máquina, tipos de ação). Em JavaScript, symbols são uma excelente forma de criar `enums` seguros, pois, ao contrário das strings, eles não podem ser acidentalmente recriados ou confundidos.
    
    ```js
    const Status = {
      CONECTADO: Symbol('Conectado'),
      DESCONECTADO: Symbol('Desconectado'),
      CONECTANDO: Symbol('Conectando')
    };
    
    function verificarStatus(statusAtual) {
      if (statusAtual === Status.CONECTADO) {
        console.log("Usuário está online.");
      }
      // ...
    }
    
    verificarStatus(Status.CONECTADO);
    ```
    
- **Definir Comportamentos Internos (Well-Known Symbols):** Como vimos, usar Well-Known Symbols permite que seus objetos se integrem profundamente aos mecanismos da linguagem, customizando como eles são iterados, convertidos para primitivos, e muito mais. Este é um caso de uso avançado, mas fundamental para autores de bibliotecas.

## Considerações Finais

Neste capítulo, desvendamos o `Symbol`, o tipo primitivo mais singular do JavaScript. Vimos que, embora não sirva para representar dados convencionais, ele desempenha um papel crucial na arquitetura de software robusto, resolvendo o problema persistente da colisão de chaves de propriedade em objetos.

Exploramos sua característica fundamental — a unicidade garantida — e aprendemos a usá-los como chaves de propriedade "escondidas", que não interferem com a enumeração padrão ou a serialização JSON. Distinguimos entre symbols locais, criados com `Symbol()`, e symbols globais compartilhados, gerenciados por `Symbol.for()`. Finalmente, vislumbramos seu poder máximo ao explorar os **Well-Known Symbols**, que nos dão a capacidade de customizar os comportamentos mais intrínsecos da linguagem para nossos próprios objetos.

Embora você possa não usar symbols todos os dias em código de aplicação simples, compreendê-los é um marco na transição para um nível mais avançado de desenvolvimento em JavaScript. Eles são a chave para escrever bibliotecas extensíveis e seguras e para entender como a própria linguagem funciona por baixo dos panos.
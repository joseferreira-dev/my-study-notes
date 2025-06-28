# Capítulo 10 – Objetos: A Estrutura Fundamental do JavaScript

Se os tipos primitivos são os tijolos e os arrays são as fileiras ordenadas desses tijolos, os **Objetos** são a própria construção. Eles são a estrutura de dados mais fundamental e versátil do JavaScript, a base sobre a qual quase tudo na linguagem é construído. Enquanto arrays são coleções especializadas e ordenadas por um índice numérico, os objetos são coleções flexíveis e não ordenadas de pares **chave-valor**, conhecidos como **propriedades**. Essa estrutura nos permite modelar entidades do mundo real — um usuário, um produto, um carro — com uma riqueza de detalhes e complexidade inigualável. Um objeto é, em essência, um container para dados e funcionalidades relacionadas.

Nos capítulos anteriores, aprendemos a manipular dados individuais e coleções ordenadas. Agora, vamos mergulhar no coração do modelo de dados do JavaScript. Este capítulo é uma exploração exaustiva dos objetos. Começaremos com os fundamentos: como criar e manipular propriedades de um objeto, entendendo as nuances entre a notação de ponto e a de colchetes. Em seguida, abraçaremos as funcionalidades do JavaScript moderno, desvendando o poder da **desestruturação** e dos operadores **spread/rest** para uma manipulação de dados mais limpa e expressiva. Aprofundaremos nosso conhecimento em tópicos cruciais como a clonagem de objetos, diferenciando cópias rasas de cópias profundas. A maior parte do capítulo, no entanto, será dedicada a desvendar o que acontece "por baixo dos panos". Investigaremos os **descritores de propriedade**, que nos darão um controle granular sem precedentes sobre nossas propriedades, permitindo-nos criar atributos somente leitura, não enumeráveis e muito mais. Por fim, exploraremos os múltiplos métodos para iterar, proteger e transformar objetos, capacitando-o a manipular qualquer estrutura de dados com precisão cirúrgica.

## Fundamentos dos Objetos: Criação e Manipulação

Um objeto em JavaScript é uma coleção dinâmica de propriedades. Cada propriedade consiste em uma chave (que é convertida para uma string, ou pode ser um Symbol) e um valor associado, que pode ser de qualquer tipo de dado, incluindo outros objetos ou funções (que, quando parte de um objeto, chamamos de **métodos**).

### Criando um Objeto

Existem diversas maneiras de criar um objeto, mas a sintaxe literal é, de longe, a mais comum e recomendada.

**1. Sintaxe Literal `{}` (Recomendada)**

Esta é a forma mais direta e legível de criar um objeto.

```js
// Um objeto vazio
const pessoa = {};

// Um objeto com propriedades
const carro = {
  marca: "Fiat",
  modelo: "Toro",
  ano: 2025,
  ligado: false,
  ligar: function() { // Um método
    this.ligado = true;
    console.log("Carro ligado!");
  }
};
```

**2. Construtor `new Object()`**

É possível criar um objeto usando o construtor, mas não oferece nenhuma vantagem sobre a forma literal e é mais verboso.

```js
const produto = new Object();
produto.nome = "Smartphone";
produto.preco = 2500.00;
```

### Acessando, Adicionando e Modificando Propriedades

Existem duas maneiras de acessar as propriedades de um objeto:

- **Notação de Ponto (`.`):** A forma mais comum e limpa. `objeto.propriedade`.
- **Notação de Colchetes (`[]`):** Mais poderosa, pois a chave é passada como uma string. `objeto['propriedade']`.

```js
console.log(carro.modelo); // "Toro"

// Acessando um método
carro.ligar(); // "Carro ligado!"
console.log(carro.ligado); // true
```

A notação de colchetes é **obrigatória** em duas situações:

**1. Quando a chave da propriedade é uma variável (Nomes de Propriedade Dinâmicos):**

```js
let atributo = 'marca';
console.log(carro[atributo]); // "Fiat"
```

**2. Quando a chave da propriedade contém caracteres especiais ou palavras reservadas:**

```js
const pedido = {
  'id-do-produto': 'xyz-123',
  'for': 'palavra reservada', // Não recomendado, mas possível
};

console.log(pedido['id-do-produto']); // 'xyz-123'
```

Adicionar ou modificar propriedades é feito através de uma simples atribuição:

```js
// Adicionando uma nova propriedade
carro.cor = "Vermelho";

// Modificando uma propriedade existente
carro.ano = 2026;

console.log(carro.cor); // "Vermelho"
console.log(carro.ano); // 2026
```

## Clonagem de Objetos: Cópias Rasas e Profundas

Copiar um objeto não é tão simples quanto parece. Lembre-se que objetos são tipos de referência. Uma simples atribuição não cria uma cópia; ela apenas cria uma nova variável que aponta para o **mesmo** objeto na memória.

```js
const obj1 = { a: 1 };
const obj2 = obj1; // Não é uma cópia!
obj2.a = 2;
console.log(obj1.a); // 2 (a alteração em obj2 afetou obj1)
```

Para realmente criar um novo objeto, precisamos de uma estratégia de clonagem.

### Clonagem Rasa (Shallow Clone)

Uma clonagem rasa cria um novo objeto, e copia as propriedades do objeto original para ele. No entanto, se uma propriedade for um outro objeto ou array (um tipo de referência), apenas a **referência** é copiada, não o objeto em si.

As formas mais comuns de se fazer uma clonagem rasa são:

**1. Operador Spread `...` (ES6):** A forma mais moderna e limpa.

```js
const original = { nome: 'Ana', endereco: { cidade: 'Recife' } };
const copia = { ...original };

copia.nome = 'Bia'; // Modifica a propriedade primitiva
copia.endereco.cidade = 'Olinda'; // Modifica a propriedade do objeto aninhado

console.log(original.nome); // "Ana" (não foi afetado)
console.log(copia.nome);    // "Bia"

console.log(original.endereco.cidade); // "Olinda" (FOI AFETADO!)
```

Como se pode ver, a clonagem rasa funciona bem para o primeiro nível, mas os objetos aninhados continuam sendo compartilhados.

**2. `Object.assign(target, ...sources)`:** Este método copia todas as propriedades enumeráveis de um ou mais objetos de origem (`sources`) para um objeto de destino (`target`). Ele retorna o objeto de destino.

```js
const original = { a: 1, b: 2 };
const copia = Object.assign({}, original, { c: 3 }); // {} é o alvo, um novo objeto vazio

console.log(copia); // { a: 1, b: 2, c: 3 }
```

`Object.assign` também realiza uma clonagem rasa e é muito útil para mesclar objetos.

### Clonagem Profunda (Deep Clone)

Uma clonagem profunda cria uma cópia de um objeto e, recursivamente, cria cópias de todos os objetos que ele referencia. O resultado é um clone completamente independente.

**1. O "Truque" com JSON (com limitações):** Uma forma comum e rápida é serializar o objeto para uma string JSON e, em seguida, fazer o parse de volta para um objeto.

```javascript
const original = { nome: 'Ana', endereco: { cidade: 'Recife' }, data: new Date() };
const copia = JSON.parse(JSON.stringify(original));

copia.endereco.cidade = 'Olinda';
console.log(original.endereco.cidade); // "Recife" (Não foi afetado, funcionou!)
````

**Atenção:** Este método tem limitações sérias. Ele **perde** tipos de dados que não são representáveis em JSON, como `undefined`, `Symbol`, funções, e converte objetos `Date` para strings.

**2. `structuredClone()` (Moderno e Recomendado):** A plataforma web (navegadores e Node.js) introduziu uma função global `structuredClone()` que realiza uma clonagem profunda de forma correta e eficiente, lidando com tipos complexos e evitando as armadilhas do método JSON.

```js
const original = { nome: 'Ana', data: new Date(), id: Symbol('id') };
const copia = structuredClone(original);

console.log(copia.data instanceof Date); // true (o tipo Date foi preservado)
console.log(original.id !== copia.id); // true (o symbol foi clonado)
```

## Manipulação Moderna: Spread, Rest e Desestruturação

O ES6 revolucionou a forma como interagimos com objetos, tornando o código muito mais conciso.

### Desestruturação de Objetos

A desestruturação (`destructuring`) permite extrair propriedades de um objeto e atribuí-las a variáveis com uma sintaxe limpa.

```js
const usuario = {
  nome: 'Pedro',
  idade: 35,
  endereco: {
    cidade: 'São Paulo',
    estado: 'SP'
  }
};

// Extraindo propriedades para variáveis com o mesmo nome
const { nome, idade } = usuario;
console.log(nome, idade); // "Pedro" 35

// Renomeando variáveis
const { nome: nomeCompleto } = usuario;
console.log(nomeCompleto); // "Pedro"

// Fornecendo valores padrão
const { sobrenome = 'Não informado' } = usuario;
console.log(sobrenome); // "Não informado"

// Desestruturação aninhada
const { endereco: { cidade } } = usuario;
console.log(cidade); // "São Paulo"
```

### Sintaxe Rest para Objetos `...`

Dentro de uma desestruturação, o operador `...` pode ser usado para coletar **todas as propriedades restantes** em um novo objeto.

```js
const { nome, ...restoDoObjeto } = usuario;
console.log(nome); // "Pedro"
console.log(restoDoObjeto); // { idade: 35, endereco: { ... } }
```

## Controle Fino: Descritores de Propriedade

Cada propriedade em um objeto tem, além de seu `value`, um conjunto de atributos "meta" que definem seu comportamento. Esses atributos são chamados de **descritores**. Existem dois tipos de descritores:

- **Descritor de Dados:** Contém as chaves `value`, `writable`, `enumerable` e `configurable`.
- **Descritor de Acesso:** Contém as chaves `get`, `set`, `enumerable` e `configurable`.

### Vendo os Descritores: `Object.getOwnPropertyDescriptor()`

Este método permite inspecionar os descritores de uma propriedade específica.

```js
const obj = { a: 1 };
const descritor = Object.getOwnPropertyDescriptor(obj, 'a');
console.log(descritor);
/*
{
  value: 1,
  writable: true,    // pode ser alterada?
  enumerable: true,  // aparece em laços for...in?
  configurable: true // pode ser deletada ou ter seus descritores alterados?
}
*/
```

### Definindo Descritores: `Object.defineProperty()`

Este poderoso método permite adicionar uma nova propriedade a um objeto ou modificar uma existente, com controle total sobre seus descritores.

**Sintaxe:** `Object.defineProperty(obj, prop, descriptor)`

**1. Criando uma Propriedade Somente Leitura (Read-Only):**

```js
const pessoa = {};
Object.defineProperty(pessoa, 'nome', {
  value: 'Ana',
  writable: false,
  enumerable: true,
  configurable: true
});

console.log(pessoa.nome); // "Ana"
pessoa.nome = 'Bia'; // A alteração falha silenciosamente em modo não-estrito
console.log(pessoa.nome); // "Ana"
```

**2. Criando uma Propriedade Não-Enumerável:**

```js
const segredo = Symbol('id');
Object.defineProperty(pessoa, segredo, {
  value: 'xyz-123',
  enumerable: false // "Esconde" a propriedade da maioria das iterações
});

console.log(Object.keys(pessoa)); // ['nome'] (o symbol não aparece)
console.log(JSON.stringify(pessoa)); // {"nome":"Ana"}
```

**3. Bloqueando a Configuração de uma Propriedade:** Uma vez que `configurable` é definido como `false`, você não pode deletar a propriedade nem alterar seus descritores (exceto mudar `value` se `writable` for `true`).

### Propriedades de Acesso: Getters e Setters

Getters e setters permitem definir funções que são executadas quando uma propriedade é lida (`get`) ou escrita (`set`). Eles permitem a criação de "propriedades computadas" ou a execução de validações.

**1. Sintaxe Literal:**

```js
const usuario = {
  primeiroNome: 'João',
  ultimoNome: 'Silva',
  get nomeCompleto() { // Getter
    return `${this.primeiroNome} ${this.ultimoNome}`;
  },
  set nomeCompleto(valor) { // Setter
    const partes = valor.split(' ');
    this.primeiroNome = partes[0];
    this.ultimoNome = partes.slice(1).join(' ');
  }
};

console.log(usuario.nomeCompleto); // "João Silva"
usuario.nomeCompleto = 'Maria Souza Oliveira';
console.log(usuario.primeiroNome); // "Maria"
console.log(usuario.ultimoNome);   // "Souza Oliveira"
```

**2. Usando `Object.defineProperty()`:**

```js
Object.defineProperty(usuario, 'idade', {
  get: function() { return new Date().getFullYear() - 1990; },
  set: function() { console.error('A idade é calculada, não pode ser definida.'); },
  enumerable: true
});
```

## Iteração sobre Propriedades de Objetos

Existem várias maneiras de iterar sobre as propriedades de um objeto.

- **`for...in`:** Itera sobre todas as propriedades **enumeráveis**, incluindo as do protótipo. **Deve ser usado com cautela** e, geralmente, com `hasOwnProperty` para filtrar apenas as propriedades do próprio objeto.
- **`Object.keys(obj)`:** Retorna um array com as chaves (nomes das propriedades) **enumeráveis e próprias** do objeto. Esta é uma das formas mais comuns e seguras de se iterar.
- **`Object.values(obj)`:** Retorna um array com os valores das propriedades **enumeráveis e próprias**.
- **`Object.entries(obj)`:** Retorna um array de arrays, onde cada sub-array contém um par `[chave, valor]` das propriedades **enumeráveis e próprias**. É a forma mais completa para iteração.
    

```js
const dados = { a: 1, b: 2, c: 3 };

// Iterando sobre as chaves
for (const chave of Object.keys(dados)) {
  console.log(chave); // "a", "b", "c"
}

// Iterando sobre os valores
for (const valor of Object.values(dados)) {
  console.log(valor); // 1, 2, 3
}

// Iterando sobre os pares [chave, valor] (muito poderoso com desestruturação)
for (const [chave, valor] of Object.entries(dados)) {
  console.log(`${chave}: ${valor}`); // "a: 1", "b: 2", "c: 3"
}
```

## Protegendo Objetos

O JavaScript fornece mecanismos para "travar" objetos, impedindo modificações indesejadas.

- **`Object.preventExtensions(obj)`:** Impede que novas propriedades sejam adicionadas ao objeto. Propriedades existentes ainda podem ser modificadas ou deletadas.
- **`Object.seal(obj)`:** Sela um objeto. Impede a adição de novas propriedades e a exclusão/reconfiguração das existentes. Os valores das propriedades existentes ainda podem ser alterados (se forem `writable`).
- **`Object.freeze(obj)`:** Congela um objeto. Faz tudo o que `seal` faz e, adicionalmente, torna todas as propriedades de dados existentes `writable: false`. É a forma mais restritiva de proteção.

**Importante:** Todos esses métodos aplicam uma proteção **rasa**. Se o objeto tiver propriedades que são outros objetos, esses objetos aninhados não serão protegidos e podem ser modificados.

```js
const objCongelado = { nome: 'Fixo', detalhes: { versao: 1 } };
Object.freeze(objCongelado);

objCongelado.nome = 'Alterado'; // Falha
objCongelado.detalhes.versao = 2; // Funciona!

console.log(objCongelado.nome); // "Fixo"
console.log(objCongelado.detalhes.versao); // 2
```

## Considerações Finais

Neste capítulo, realizamos um mergulho profundo na estrutura de dados que define o JavaScript: o Objeto. Vimos que ele é a base para a modelagem de qualquer entidade complexa, funcionando como uma coleção dinâmica de pares chave-valor.

Exploramos desde a criação e manipulação básica até os recursos modernos e poderosos introduzidos pelo ES6, como a **desestruturação** e os operadores **spread/rest**, que tornaram nosso código mais limpo e declarativo. Desvendamos os mistérios da clonagem, aprendendo a diferenciar as cópias rasas das profundas e a escolher a ferramenta certa para cada cenário.

O ponto alto da nossa exploração foi o controle de baixo nível que os **descritores de propriedade** nos oferecem. Com `Object.defineProperty`, ganhamos o poder de criar propriedades com comportamentos customizados — somente leitura, não-enumeráveis e com getters/setters. Por fim, aprendemos a proteger nossos objetos com `freeze` e `seal` e a iterar sobre suas propriedades de forma segura e eficiente com `Object.keys`, `values` e `entries`.

O domínio sobre os objetos não é apenas uma habilidade, é a fluência na própria linguagem JavaScript. Com o conhecimento adquirido neste capítulo, você está preparado para construir, manipular e proteger as estruturas de dados mais complexas, escrevendo um código mais robusto, seguro e profissional.
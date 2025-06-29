# Capítulo 29 – JSON: A Linguagem Universal da Troca de Dados

Em nossa jornada pelo JavaScript, nos tornamos proficientes na criação e manipulação de estruturas de dados complexas, como objetos e arrays. Essas estruturas são perfeitas para organizar informações **dentro** de nossa aplicação. No entanto, o verdadeiro poder do software moderno reside em sua capacidade de se comunicar. Uma aplicação web precisa conversar com um servidor para buscar dados, um servidor pode precisar se comunicar com outro para validar uma informação, e diferentes serviços escritos em linguagens totalmente distintas (como Python, Java ou Go) precisam trocar dados de forma consistente e confiável. Como podemos garantir que um objeto JavaScript enviado de um navegador seja perfeitamente compreendido por um servidor escrito em Python?

A resposta para esse desafio de interoperabilidade é um formato de dados que se tornou a **lingua franca** da web: **JSON (JavaScript Object Notation)**. O JSON é um formato leve, baseado em texto, para a troca de dados, que é fácil para humanos lerem e escreverem, e fácil para máquinas analisarem e gerarem. Embora seu nome e sua sintaxe sejam derivados do JavaScript, o JSON é, na verdade, completamente **independente de linguagem**. Ele especifica um conjunto de regras estritas para a representação de dados que qualquer linguagem de programação pode entender e produzir. É essa universalidade que o tornou o padrão de fato para a comunicação em APIs web, arquivos de configuração e muito mais.

Neste capítulo, faremos um mergulho profundo no mundo do JSON. Começaremos por entender o que ele é e, crucialmente, como sua sintaxe difere dos literais de objeto do JavaScript. Dissecaremos o objeto `JSON` nativo do JavaScript, dominando os dois métodos que formam a espinha dorsal de todo o trabalho com JSON: `JSON.stringify()`, para converter nossos objetos em texto, e `JSON.parse()`, para fazer o caminho inverso. Exploraremos os recursos avançados desses métodos, como as funções `replacer` e `reviver`, que nos dão um controle fino sobre o processo de serialização e desserialização. Também abordaremos as armadilhas comuns, como lidar com referências cíclicas e restaurar tipos de dados complexos, como instâncias de classes.

## O que é JSON? A Sintaxe Estrita da Interoperabilidade

JSON significa **JavaScript Object Notation**. Apesar do nome, é importante pensar nele não como "um objeto JavaScript", mas como um **formato de dados textual**. Uma string JSON é uma sequência de caracteres que segue um conjunto de regras muito estrito para representar dados estruturados.

As regras de sintaxe do JSON são simples, mas inflexíveis:

1. **Estrutura:** Um JSON representa ou um objeto (uma coleção de pares chave-valor) ou um array (uma lista de valores).
2. **Objetos:** São envolvidos por chaves `{}`.
3. **Arrays:** São envolvidos por colchetes `[]`.
4. **Chaves:** Em um objeto, as chaves devem **obrigatoriamente** ser strings e estar envoltas em **aspas duplas (`"`)**. Aspas simples não são permitidas.
5. **Valores:** Os valores podem ser de um dos seguintes tipos:
    - uma string (em aspas duplas)
    - um número (inteiro ou de ponto flutuante)
    - um objeto (seguindo as regras do JSON)
    - um array
    - um booleano (`true` ou `false`)
    - `null`
6. **Proibido:** Funções, `undefined`, `Symbol`, comentários e vírgulas finais (trailing commas) **não são permitidos** na sintaxe JSON.

**Exemplo de uma String JSON Válida:**

```js
{
  "id": 123,
  "produto": "Teclado Mecânico",
  "disponivel": true,
  "preco": 350.50,
  "tags": ["gamer", "rgb", "abnt2"],
  "caracteristicas": {
    "switch": "brown",
    "iluminacao": true
  },
  "revendedor": null
}
```

### JSON versus Literais de Objeto JavaScript

Este é um ponto de confusão comum. Embora a sintaxe seja semelhante, eles não são a mesma coisa.

|Característica|Literal de Objeto JavaScript|String JSON|
|---|---|---|
|**Chaves**|Podem ser strings (com aspas simples, duplas ou sem aspas, se válidas como identificador), números ou symbols.|Devem ser strings e **obrigatoriamente** em aspas duplas.|
|**Valores Permitidos**|Qualquer tipo de dado do JavaScript, incluindo funções, `undefined`, `Symbol`.|Apenas string, número, booleano, array, objeto e `null`.|
|**Comentários**|Permitidos (`//` ou `/* */`).|**Não permitidos**.|
|**Vírgulas Finais**|Permitidas na maioria dos motores JavaScript modernos.|**Não permitidas**. Uma vírgula extra no final de um array ou objeto invalida o JSON.|
|**Natureza**|Uma estrutura de dados em memória, um objeto.|Uma **string**, um formato de texto para troca de dados.|

## O Objeto `JSON` do JavaScript

O JavaScript nos fornece um objeto nativo, chamado `JSON`, que atua como um namespace para as duas funções essenciais que usamos para trabalhar com este formato.

- `JSON.stringify()`: Converte um valor/objeto JavaScript em uma string JSON. (**Serialização**)
- `JSON.parse()`: Converte uma string JSON em um valor/objeto JavaScript. (**Parsing** ou **Desserialização**)

## Serialização: `JSON.stringify()`

A serialização é o processo de converter uma estrutura de dados em memória (como um objeto JavaScript) em um formato de string que pode ser facilmente armazenado ou transmitido.

**Sintaxe:** `JSON.stringify(valor[, replacer[, space]])`

- `valor`: O objeto ou valor JavaScript a ser convertido.
- `replacer` (opcional): Uma função ou um array que altera o processo de serialização.
- `space` (opcional): Adiciona indentação e espaços em branco à string de saída para torná-la mais legível.

**Exemplo Básico:**

```js
const usuario = {
  nome: "Bia",
  idade: 28,
  ativa: true,
  cursos: ["JS", "React"],
  plano: null
};

const jsonString = JSON.stringify(usuario);
console.log(jsonString);
// Saída: {"nome":"Bia","idade":28,"ativa":true,"cursos":["JS","React"],"plano":null}
```

### Valores Ignorados na Serialização

`JSON.stringify` ignora automaticamente certos valores que não têm uma representação em JSON:

- Propriedades cujo valor é `undefined`.
- Propriedades cujo valor é uma `function` ou um `Symbol`.

```js
const objComplexo = {
  a: 1,
  b: undefined,
  c: Symbol('id'),
  d: () => console.log('função')
};

console.log(JSON.stringify(objComplexo)); // {"a":1}
```

### O Argumento `space` para "Pretty-Printing"

Para facilitar a leitura e a depuração, podemos usar o terceiro argumento para formatar a saída.

- Se for um **número**, ele especifica o número de espaços a serem usados para a indentação (limitado a 10).
- Se for uma **string**, essa string é usada como o caractere de indentação.

```js
const usuario = { nome: "Bia", idade: 28, ativa: true };

// Indentação com 2 espaços
console.log(JSON.stringify(usuario, null, 2));
/* Saída:
{
  "nome": "Bia",
  "idade": 28,
  "ativa": true
}
*/

// Indentação com uma string de tabulação
console.log(JSON.stringify(usuario, null, '\t'));
```

### Customizando a Serialização com um `replacer`

O segundo argumento, `replacer`, nos dá um controle fino sobre o que é incluído e como os valores são transformados.

**1. `replacer` como um Array (Whitelist):** Se o `replacer` for um array de strings, apenas as propriedades cujas chaves estão nesse array serão incluídas na string JSON.

```js
const produto = { id: 123, nome: 'Laptop', preco: 4500, categoria: 'Eletrônicos', estoque: 50 };

const replacerWhitelist = ['nome', 'preco', 'categoria'];

const jsonFiltrado = JSON.stringify(produto, replacerWhitelist, 2);
console.log(jsonFiltrado);
/* Saída:
{
  "nome": "Laptop",
  "preco": 4500,
  "categoria": "Eletrônicos"
}
*/
```

**2. `replacer` como uma Função:** Se o `replacer` for uma função, ela será chamada para cada par chave-valor no objeto. O que a função retornar determinará o valor que será serializado. Se ela retornar `undefined`, a propriedade será omitida.

```js
const dadosSensíveis = {
  nome: 'Carlos',
  cpf: '123.456.789-00',
  token: 'xyz-super-secreto',
  idade: 42
};

function replacerFuncao(chave, valor) {
  // Oculta informações sensíveis
  if (chave === 'cpf' || chave === 'token') {
    return '***REMOVIDO***';
  }
  // Omite a propriedade idade se for menor que 18
  if (chave === 'idade' && valor < 18) {
    return undefined;
  }
  return valor;
}

console.log(JSON.stringify(dadosSensíveis, replacerFuncao, 2));
```

### Armadilha: Valores Cíclicos

Se um objeto contém uma referência a si mesmo (uma referência cíclica), `JSON.stringify` não consegue lidar com isso e lança um `TypeError`, pois entraria em um laço infinito.

```js
const objA = { nome: 'A' };
const objB = { nome: 'B' };

objA.amigo = objB;
objB.amigo = objA; // Referência cíclica: A -> B -> A

try {
  JSON.stringify(objA);
} catch (erro) {
  console.error(erro.name); // "TypeError"
  console.error(erro.message); // Ex: "cyclic object value"
}
```

## Parsing (Desserialização): `JSON.parse()`

Parsing é o processo inverso: pegar uma string JSON e convertê-la de volta em uma estrutura de dados JavaScript (objeto ou array).

**Sintaxe:** `JSON.parse(texto[, reviver])`

- `texto`: A string JSON a ser analisada.
- `reviver` (opcional): Uma função que pode transformar os valores durante o processo de parsing.

**Exemplo Básico:**

```js
const jsonString = '{"nome":"Bia","idade":28,"ativa":true}';
const usuarioObj = JSON.parse(jsonString);

console.log(usuarioObj.nome); // "Bia"
console.log(usuarioObj.ativa); // true
```

Se a string não for um JSON válido, `JSON.parse()` lançará um `SyntaxError`.

### Restaurando Dados com uma Função `reviver`

O `reviver` é o equivalente do `replacer` para o parsing. É uma função que é chamada para cada par chave-valor analisado. Ele permite que você transforme os valores antes que eles sejam finalmente retornados no objeto final.

O caso de uso mais clássico e importante é para restaurar tipos de dados que são perdidos na serialização, como datas.

**Exemplo: Restaurando um Objeto `Date`**

```js
const evento = {
  nome: 'Lançamento do Produto',
  data: new Date() // Objeto Date
};

// 1. Serializar: O objeto Date é convertido para uma string ISO.
const eventoJSON = JSON.stringify(evento);
console.log(eventoJSON); // Ex: '{"nome":"Lançamento do Produto","data":"2025-06-29T16:56:00.000Z"}'

// 2. Parsing sem reviver: 'data' permanece como uma string.
const eventoParseado = JSON.parse(eventoJSON);
console.log(typeof eventoParseado.data); // "string"

// 3. Parsing COM reviver para restaurar o objeto Date.
const eventoRestaurado = JSON.parse(eventoJSON, (chave, valor) => {
  if (chave === 'data') {
    // Se a chave for 'data', cria um novo objeto Date a partir da string.
    return new Date(valor);
  }
  return valor; // Retorna todos os outros valores como estão.
});

console.log(typeof eventoRestaurado.data); // "object"
console.log(eventoRestaurado.data.getFullYear()); // 2025
```

### Serializando e Restaurando Instâncias de Classes

Da mesma forma, podemos usar a combinação de um método `toJSON` na classe e uma função `reviver` no parsing para serializar e restaurar instâncias de classes customizadas.

```js
class Usuario {
  constructor(nome, email) {
    this.nome = nome;
    this.email = email;
  }
  // Adiciona um identificador para que o reviver saiba que tipo de objeto é.
  toJSON() {
    return {
      _tipo: 'Usuario',
      nome: this.nome,
      email: this.email
    };
  }
}

const user = new Usuario('Pedro', 'pedro@example.com');
const userJSON = JSON.stringify(user);

const userRestaurado = JSON.parse(userJSON, (chave, valor) => {
  if (valor && valor._tipo === 'Usuario') {
    return new Usuario(valor.nome, valor.email);
  }
  return valor;
});

console.log(userRestaurado instanceof Usuario); // true
```

## Considerações Finais

Neste capítulo, desvendamos o JSON, o formato de dados que serve como a espinha dorsal da comunicação na web moderna. Vimos que, embora sua sintaxe seja inspirada nos objetos JavaScript, o JSON é um padrão de texto estrito, independente de linguagem, projetado para máxima interoperabilidade.

Dominamos as duas ferramentas essenciais para trabalhar com ele: `JSON.stringify()`, para **serializar** nossos objetos em texto, e `JSON.parse()`, para **desserializar** o texto de volta para objetos vivos. Mais importante, exploramos os recursos avançados que nos dão controle total sobre esses processos. Usamos o `replacer` para filtrar e transformar dados durante a serialização e o `reviver` para restaurar tipos de dados complexos, como `Date` e instâncias de classes, durante o parsing.

Compreender o JSON e como manipulá-lo de forma eficaz é uma habilidade não negociável para qualquer desenvolvedor web. Seja consumindo uma API de terceiros, construindo sua própria API ou simplesmente salvando configurações, o JSON será seu companheiro constante. Com o conhecimento adquirido aqui, você está preparado para trocar dados de forma robusta e confiável em qualquer cenário.
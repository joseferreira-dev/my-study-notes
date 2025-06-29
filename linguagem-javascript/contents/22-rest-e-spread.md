# Capítulo 22 – Os Operadores Rest e Spread: Reunindo e Espalhando Dados

No mundo do JavaScript, poucas adições à sintaxe foram tão transformadoras e onipresentes quanto o operador de três pontos (`...`), introduzido no ES6. À primeira vista, essa sintaxe pode parecer confusa, pois é usada para duas operações que são, conceitualmente, o oposto uma da outra. Dependendo do contexto em que é usado, o `...` pode ser o **Operador Spread (Espalhar)** ou o **Operador Rest (Reunir)**. Dominar essa dualidade é fundamental para escrever código JavaScript moderno, conciso e expressivo.

O **Operador Spread** pega uma estrutura de dados iterável (como um array) ou um objeto e "espalha" seus elementos ou propriedades individuais. É como abrir uma caixa e colocar todos os seus conteúdos sobre a mesa, um a um. Ele é usado para criar cópias de arrays, combinar objetos, passar argumentos para funções e muito mais, promovendo um estilo de programação mais funcional e imutável.

Por outro lado, o **Operador Rest** faz o exato oposto. Ele "reúne" múltiplos elementos ou argumentos individuais e os agrupa em uma única estrutura, geralmente um array. É como pegar vários itens espalhados sobre a mesa e guardá-los de volta em uma caixa. Seu uso mais proeminente é na definição de funções que podem aceitar um número variável de argumentos e na desestruturação para capturar "o resto" dos valores.

Neste capítulo, vamos desvendar completamente a sintaxe `...`. Começaremos com o Operador Spread, explorando suas inúmeras aplicações práticas com arrays e objetos. Em seguida, focaremos no Operador Rest, entendendo como ele modernizou a forma como lidamos com os parâmetros de função e como ele se integra perfeitamente com a desestruturação. Por fim, vamos solidificar a diferença entre os dois, fornecendo uma regra clara para saber quando você está "espalhando" e quando está "reunindo". Ao final, a sintaxe `...` deixará de ser uma fonte de confusão e se tornará uma das ferramentas mais poderosas e versáteis do seu repertório.

## O Operador Spread (`...`): Espalhando Elementos

O Operador Spread permite que uma expressão seja expandida em locais onde múltiplos elementos (para arrays) ou múltiplos pares chave-valor (para objetos) são esperados.

### Spread com Arrays

Com arrays e outros iteráveis (como strings ou Sets), o spread expande seus itens em elementos individuais.

#### Caso de Uso 1: Combinando Arrays

Antes do ES6, a forma padrão de combinar arrays era usando o método `concat()`. O spread torna essa operação muito mais limpa e legível.

```js
const parte1 = ['a', 'b'];
const parte2 = ['d', 'e'];

// Antes (ES5)
const combinadoAntigo = parte1.concat('c', parte2); // ['a', 'b', 'c', 'd', 'e']

// Com Spread (ES6)
const combinadoModerno = [...parte1, 'c', ...parte2];

console.log(combinadoModerno); // ['a', 'b', 'c', 'd', 'e']
```

A sintaxe com spread é mais declarativa e visualmente intuitiva.

#### Caso de Uso 2: Copiando Arrays (Shallow Copy)

Como vimos, atribuir um array a uma nova variável não cria uma cópia. O spread é a maneira mais comum e moderna de se criar uma **cópia rasa (shallow copy)** de um array.

```js
const original = [1, 2, 3];
const copia = [...original];

copia.push(4);

console.log(original); // [1, 2, 3] (não foi modificado)
console.log(copia);    // [1, 2, 3, 4]
```

Isso é crucial para a **imutabilidade**, um princípio onde não modificamos dados existentes, mas sim criamos novas versões deles.

#### Caso de Uso 3: Passando Elementos de um Array como Argumentos de Função

Suponha que você tenha uma função que espera múltiplos argumentos, mas seus dados estão em um array. Antes, você usaria `Function.prototype.apply()`. O spread simplifica isso drasticamente.

```js
function somar(x, y, z) {
  return x + y + z;
}

const numeros = [10, 20, 30];

// Antes (ES5)
const resultadoAntigo = somar.apply(null, numeros);

// Com Spread (ES6)
const resultadoModerno = somar(...numeros); // Equivalente a somar(10, 20, 30)

console.log(resultadoModerno); // 60
```

Isso também funciona com qualquer método, como `Math.max()`: `const maiorNumero = Math.max(...numeros); // 30`

#### Caso de Uso 4: Convertendo Iteráveis em Arrays

O spread é uma ferramenta universal para converter qualquer objeto iterável (como uma String, um Set, um Map ou uma NodeList do DOM) em um array verdadeiro.

```js
const palavra = "JavaScript";
const arrayDeLetras = [...palavra];
console.log(arrayDeLetras); // ['J', 'a', 'v', 'a', 'S', 'c', 'r', 'i', 'p', 't']

const conjuntoUnico = new Set([1, 2, 3, 2, 1]);
const arrayDoSet = [...conjuntoUnico];
console.log(arrayDoSet); // [1, 2, 3]
```

### Spread com Objetos (ES2018)

A sintaxe de spread também foi estendida para objetos, permitindo a expansão de seus pares chave-valor.

#### Caso de Uso 1: Mesclando Objetos

Similar à combinação de arrays, o spread é a forma mais limpa de mesclar dois ou mais objetos. A principal regra a se lembrar é a de "último a entrar, vence": se houver propriedades com o mesmo nome, o valor da propriedade do último objeto na sequência prevalecerá.

```js
const configuracaoPadrao = {
  host: 'localhost',
  porta: 3000,
  debug: false
};

const configuracaoUsuario = {
  porta: 8080,
  usuario: 'admin'
};

const configuracaoFinal = { ...configuracaoPadrao, ...configuracaoUsuario };

console.log(configuracaoFinal);
// { host: 'localhost', porta: 8080, debug: false, usuario: 'admin' }
// Note que a 'porta' do configuracaoUsuario sobrescreveu a do padrão.
```

#### Caso de Uso 2: Clonando Objetos (Shallow Clone)

Assim como com arrays, o spread cria uma **cópia rasa** de um objeto.

```js
const pessoaOriginal = { nome: 'Carlos', endereco: { cidade: 'Recife' } };
const pessoaCopia = { ...pessoaOriginal };

pessoaCopia.nome = 'Daniel'; // Modifica a cópia
pessoaCopia.endereco.cidade = 'Olinda'; // Modifica o objeto aninhado

console.log(pessoaOriginal.nome); // 'Carlos' (inalterado)
console.log(pessoaOriginal.endereco.cidade); // 'Olinda' (FOI ALTERADO!)
```

Para uma cópia profunda, métodos como `structuredClone()` devem ser usados.

#### Caso de Uso 3: Atualização Imutável de Objetos

Este é um padrão fundamental em frameworks de front-end como o React. Em vez de modificar um objeto de estado diretamente (mutação), você cria um novo objeto com a propriedade atualizada.

```js
const estadoInicial = {
  logado: false,
  usuario: null,
  tema: 'light'
};

// Simulando um login
const estadoAposLogin = { ...estadoInicial, logado: true, usuario: 'Ana' };

console.log(estadoInicial); // O objeto original permanece intacto
console.log(estadoAposLogin); // Um novo objeto com os dados atualizados
```

## O Operador Rest (`...`): Reunindo Elementos

Enquanto o Spread "desempacota", o Rest "empacota". Ele coleta múltiplos elementos e os agrupa em uma única variável, que é sempre um array. O operador Rest é usado em dois contextos principais: parâmetros de função e desestruturação.

### Parâmetros Rest em Funções

Esta é a maneira moderna de se criar **funções variádicas** (funções que aceitam um número variável de argumentos), substituindo completamente o antigo objeto `arguments`.

**Regras:**

1. Só pode haver **um** parâmetro Rest em uma função.
2. Ele deve ser o **último** parâmetro na definição da função.

```js
function calcularMedia(turma, ...notas) {
  // 'turma' é um argumento normal.
  // 'notas' é um array real contendo todos os argumentos restantes.
  if (notas.length === 0) return 0;
  
  const soma = notas.reduce((acc, nota) => acc + nota, 0);
  console.log(`A média da turma ${turma} é ${(soma / notas.length).toFixed(2)}.`);
  return soma / notas.length;
}

calcularMedia('A', 10, 9, 8.5, 7.5); // A média da turma A é 8.75.
calcularMedia('B', 7, 6, 8);         // A média da turma B é 7.00.
```

**Vantagens sobre o objeto `arguments`:**

- **É um Array Real:** `notas` é uma instância de `Array`, então podemos usar `reduce()`, `map()`, `filter()`, etc., diretamente. O objeto `arguments` não permitia isso sem antes ser convertido para um array.
- **Clareza:** A sintaxe deixa explícito que a função pode receber múltiplos argumentos para aquela posição.
- **Separação:** Permite separar os primeiros argumentos nomeados do "resto".

### Rest na Desestruturação

Como vimos no capítulo anterior, o Rest também pode ser usado do lado esquerdo de uma atribuição de desestruturação para capturar os elementos ou propriedades restantes.

**Com Arrays:**

```js
const placar = ['Brasil', 'Alemanha', 7, 1, 'Copa do Mundo', 2014];

const [timeVencedor, timePerdedor, ...detalhesPartida] = placar;

console.log(timeVencedor);        // 'Brasil'
console.log(timePerdedor);      // 'Alemanha'
console.log(detalhesPartida);   // [7, 1, 'Copa do Mundo', 2014]
```

**Com Objetos:**

```js
const usuario = {
  id: 1,
  nome: 'Mariana',
  email: 'mariana@example.com',
  cidade: 'Porto Alegre'
};

// Separando a propriedade 'id' do resto do objeto
const { id, ...dadosUsuario } = usuario;

console.log(id); // 1
console.log(dadosUsuario); // { nome: 'Mariana', email: '...', cidade: '...' }
```

Este padrão é extremamente útil para criar cópias de um objeto omitindo certas propriedades.

## A Distinção Crucial: Onde o `...` é Usado?

A mesma sintaxe `...` para duas operações opostas pode ser confusa. A regra para diferenciá-los é simples e se baseia no **contexto**:

- **É SPREAD (Espalhar) se...** ...você o usa em um local onde valores individuais são esperados. É uma operação de "desempacotar".
    - Em chamadas de função: `minhaFuncao(...meuArray);`
    - Em literais de array: `const novoArray = [1, 2, ...outroArray];`
    - Em literais de objeto: `const novoObjeto = { a: 1, ...outroObjeto };`
- **É REST (Reunir) se...** ...você o usa em um local onde se declara variáveis ou parâmetros que irão receber múltiplos valores. É uma operação de "empacotar".
    - Em parâmetros de função: `function minhaFuncao(...args) {}`
    - Em desestruturação de array: `const [primeiro, ...resto] = meuArray;`
    - Em desestruturação de objeto: `const { a, ...resto } = meuObjeto;`

Pense assim: se o `...` está sendo usado para **fornecer** múltiplos valores, é Spread. Se está sendo usado para **receber** múltiplos valores, é Rest.

## Considerações Finais

Neste capítulo, desvendamos a poderosa e multifacetada sintaxe `...` do JavaScript. Vimos como, apesar de sua simplicidade, ela encapsula duas das operações mais importantes na manipulação de dados moderna: **Spread (Espalhar)** e **Rest (Reunir)**.

Dominamos o **Operador Spread** como a ferramenta principal para promover a imutabilidade, permitindo-nos criar cópias e combinações de arrays e objetos de forma limpa e declarativa. Ele se tornou o padrão para mesclar estruturas de dados e passar argumentos de forma flexível.

Em contrapartida, dominamos o **Operador Rest** como a solução definitiva para criar funções que aceitam um número variável de argumentos, substituindo o antigo e desajeitado objeto `arguments`. Vimos também seu poder ao ser combinado com a desestruturação, permitindo-nos extrair e agrupar dados com uma elegância sem precedentes.

A compreensão da diferença contextual entre Rest e Spread é um marco no desenvolvimento de um desenvolvedor JavaScript. Essas ferramentas não são apenas conveniências sintáticas; elas promovem um estilo de codificação mais seguro, mais funcional e mais legível, e são absolutamente essenciais para se trabalhar com qualquer framework ou biblioteca moderna.
# Capítulo 8 – Arrays: A Estrutura de Coleções Essencial

Até este ponto em nossa jornada, dominamos os blocos de construção individuais da informação em JavaScript: os números, as strings e os booleanos. Aprendemos a armazená-los em variáveis e a manipulá-los com operadores e expressões regulares. No entanto, a maioria das aplicações do mundo real não lida com dados isolados, mas sim com **coleções** de dados: uma lista de tarefas, um carrinho de compras com múltiplos produtos, as notas de um aluno em um semestre, ou as coordenadas de uma rota em um mapa. Para gerenciar esses conjuntos de dados de forma ordenada e eficiente, o JavaScript nos oferece sua estrutura de dados mais fundamental e versátil: o **Array**.

Um Array em JavaScript é um tipo de objeto especializado, projetado para armazenar uma coleção ordenada de itens. Esses itens podem ser de qualquer tipo — números, strings, objetos, e até mesmo outros arrays —, tornando-o uma ferramenta incrivelmente flexível. Neste capítulo, faremos uma imersão profunda e exaustiva no mundo dos arrays. Começaremos com os fundamentos: como criar arrays, acessar seus elementos e realizar modificações básicas. Em seguida, exploraremos o vasto e poderoso conjunto de métodos que o protótipo `Array` oferece. Dedicaremos uma atenção especial aos métodos de iteração que formam o coração da programação funcional em JavaScript, como `map()`, `filter()` e `reduce()`, que transformaram a maneira como processamos coleções de dados. Abordaremos também tarefas essenciais como ordenação, busca, concatenação e a remoção de duplicatas. Por fim, mergulharemos em funcionalidades modernas do ES6+, como o operador Spread e a desestruturação, que trouxeram ainda mais poder e elegância à manipulação de arrays. Ao final deste capítulo, você terá um domínio completo sobre a principal estrutura de dados para listas do JavaScript, capacitando-o a organizar e manipular coleções de informações com total confiança e eficiência.

## Fundamentos dos Arrays

Antes de mergulharmos nos métodos, é crucial entender a natureza de um array em JavaScript. Um array é um objeto que usa índices numéricos (começando do zero) como chaves para armazenar uma coleção ordenada de valores.

### Criando um Array

Existem duas formas principais de se criar um array, mas uma delas é vastamente preferida.

**1. Sintaxe Literal `[ ]` (Recomendada)**

Esta é a forma mais comum, concisa e legível de criar um array. É a prática padrão na indústria.

```js
// Um array vazio
const tarefas = [];

// Um array com elementos iniciais
const numeros = [1, 1, 2, 3, 5, 8];
const frutas = ["Maçã", "Banana", "Laranja"];
const misto = [1, "texto", true, { id: 1 }]; // Arrays podem conter tipos diferentes
```

**2. Construtor `new Array()`**

A outra forma é usar o construtor `Array`. No entanto, seu comportamento pode ser confuso e, por isso, seu uso é geralmente desaconselhado.

- Se você passar múltiplos argumentos, ele cria um array com esses elementos, assim como a forma literal: `const numeros = new Array(1, 1, 2, 3, 5, 8);`
- **Atenção:** Se você passar um **único argumento numérico**, ele não cria um array com esse número, mas sim um array **vazio** com uma propriedade `length` definida para aquele número.

    ```js
    const arr1 = new Array(5);
    console.log(arr1);       // [ <5 empty items> ]
    console.log(arr1.length); // 5
    ```

Devido a essa ambiguidade, a regra de ouro é: **sempre prefira a sintaxe literal `[ ]` para criar arrays.**

### Acessando e Modificando Elementos

O acesso aos elementos de um array é feito através de seu índice, que começa em `0`.

```js
const nomes = ["Ana", "Bia", "Carlos"];

// Acessando elementos
console.log(nomes[0]); // "Ana" (primeiro elemento)
console.log(nomes[2]); // "Carlos" (terceiro elemento)

// Modificando um elemento
nomes[1] = "Beatriz";
console.log(nomes); // ["Ana", "Beatriz", "Carlos"]
```

### A Propriedade `length`

A propriedade `length` retorna o número de elementos em um array. É uma propriedade de leitura e escrita, e alterá-la pode truncar ou criar espaços vazios no array.

```js
const numeros = [10, 20, 30, 40];
console.log(numeros.length); // 4

// Acessando o último elemento
console.log(numeros[numeros.length - 1]); // 40

// Truncando o array
numeros.length = 2;
console.log(numeros); // [10, 20]
```

## Manipulação de Elementos: Adicionando e Removendo

O JavaScript oferece métodos específicos para adicionar ou remover elementos do início ou do fim de um array. Esses métodos **modificam (mutam)** o array original.

### `push()` e `pop()`: Operações no Final do Array

- **`push(item1, item2, ...)`:** Adiciona um ou mais elementos ao **final** do array. Retorna o novo `length` do array.
- **`pop()`:** Remove o último elemento do array. Retorna o elemento que foi **removido**.

```js
const carrinho = ["Maçã", "Banana"];

carrinho.push("Laranja");
console.log(carrinho); // ["Maçã", "Banana", "Laranja"]

const itemRemovido = carrinho.pop();
console.log(itemRemovido); // "Laranja"
console.log(carrinho); // ["Maçã", "Banana"]
```

### `unshift()` e `shift()`: Operações no Início do Array

- **`unshift(item1, item2, ...)`:** Adiciona um ou mais elementos ao **início** do array. Retorna o novo `length`.
- **`shift()`:** Remove o primeiro elemento do array. Retorna o elemento que foi **removido**.

```js
const fila = ["Cliente A", "Cliente B"];

fila.unshift("Cliente VIP");
console.log(fila); // ["Cliente VIP", "Cliente A", "Cliente B"]

const proximoCliente = fila.shift();
console.log(proximoCliente); // "Cliente VIP"
console.log(fila); // ["Cliente A", "Cliente B"]
```

**Consideração de Performance:** `push` e `pop` são geralmente mais rápidos que `unshift` e `shift`, pois não exigem que todos os outros elementos do array sejam reindexados.

## A Ferramenta Universal: `splice()`

O método `splice()` é o canivete suíço para manipulação de arrays. Ele pode remover, substituir ou adicionar elementos em qualquer posição, modificando o array original.

**Sintaxe:** `array.splice(startIndex, deleteCount, item1, item2, ...)`

- `startIndex`: O índice a partir do qual iniciar a alteração.
- `deleteCount`: O número de elementos a serem removidos.
- `item1, item2, ...` (opcional): Os elementos a serem adicionados no lugar dos removidos.

**Modos de Uso:**

- **Apenas Removendo Elementos:**
    
    ```js
    const letras = ['a', 'b', 'c', 'd', 'e'];
    // Remove 2 elementos a partir do índice 2 ('c' e 'd')
    const removidas = letras.splice(2, 2);
    console.log(letras);    // ['a', 'b', 'e']
    console.log(removidas); // ['c', 'd']
    ```
    
- **Apenas Adicionando Elementos (Inserindo):**
    
    ```js
    const letras = ['a', 'b', 'e'];
    // A partir do índice 2, remove 0 elementos e insere 'c' e 'd'
    letras.splice(2, 0, 'c', 'd');
    console.log(letras); // ['a', 'b', 'c', 'd', 'e']
    ```
    
- **Removendo e Adicionando (Substituindo):**
    
    ```js
    const letras = ['a', 'B', 'C', 'e'];
    // A partir do índice 1, remove 2 elementos e insere 'b' e 'c'
    letras.splice(1, 2, 'b', 'c');
    console.log(letras); // ['a', 'b', 'c', 'e']
    ```

## Criando Novos Arrays a Partir dos Existentes

Muitas vezes, a melhor prática é não modificar o array original, mas sim criar um novo com as alterações desejadas (princípio da imutabilidade).

### Concatenando Arrays: `concat()` e Operador Spread

- **`concat(array2, array3, ...)`:** Retorna um novo array que é a junção do array original com os arrays (ou valores) passados como argumento.
- **Operador Spread `...` (ES6):** A forma moderna e mais flexível de concatenar e copiar arrays.

```js
const arr1 = [1, 2];
const arr2 = [3, 4];

// Usando concat()
const arr3_concat = arr1.concat(arr2, 5, [6, 7]);
console.log(arr3_concat); // [1, 2, 3, 4, 5, 6, 7]

// Usando o operador Spread
const arr3_spread = [...arr1, ...arr2, 5, 6, 7];
console.log(arr3_spread); // [1, 2, 3, 4, 5, 6, 7]
```

### Copiando e Fatiando Arrays: `slice()`

O método `slice(startIndex, endIndex)` retorna uma **cópia rasa (shallow copy)** de uma porção do array em um novo array. O array original não é modificado.

- `startIndex`: O início da extração (inclusivo).
- `endIndex` (opcional): O final da extração (não inclusivo).

```js
const animais = ['pato', 'tucano', 'arara', 'papagaio', 'galinha'];

// Extrai do índice 1 ao 3 (não inclui o 4)
const aves = animais.slice(1, 4);
console.log(aves); // ['tucano', 'arara', 'papagaio']

// Cria uma cópia completa do array
const copiaAnimais = animais.slice();
// Forma moderna com Spread: const copiaAnimais = [...animais];
console.log(copiaAnimais); // ['pato', 'tucano', 'arara', 'papagaio', 'galinha']
```

## Iteração: O Coração das Operações com Arrays

Iterar sobre um array — ou seja, passar por cada um de seus elementos para executar uma operação — é uma das tarefas mais comuns na programação.

### Formas de Iteração

- **Laço `for` Clássico:** A forma tradicional. Oferece controle total sobre o processo.
    
    ```js
    const numeros = [1, 2, 3];
    for (let i = 0; i < numeros.length; i++) {
      console.log(numeros[i]);
    }
    ```
    
- **Laço `for...of` (ES6):** A forma moderna e mais limpa para iterar sobre os **valores** de um iterável (como um array).
    
    ```js
    for (const numero of numeros) {
      console.log(numero);
    }
    ```
    
- **Atenção: `for...in`:** Este laço itera sobre as **chaves (propriedades)** de um objeto. **Não deve ser usado para iterar sobre arrays**, pois pode incluir propriedades inesperadas e não garante a ordem.

### O Trio Funcional: `map`, `filter`, `reduce`

Estes três métodos são a base da programação funcional em JavaScript e são extremamente poderosos. Todos eles recebem uma função de callback como argumento e **não modificam o array original**.

#### Mapeando Valores com `map()`

O método `map()` cria um **novo array** com os resultados da chamada de uma função de callback para cada elemento do array original. É usado para **transformar** cada elemento de um array.

**Callback:** `function(currentValue, index, array)`

```js
const numeros = [1, 4, 9, 16];

// Cria um novo array com a raiz quadrada de cada número
const raizes = numeros.map(num => Math.sqrt(num));
console.log(raizes); // [1, 2, 3, 4]

const usuarios = [
  { nome: 'Ana', idade: 25 },
  { nome: 'Bia', idade: 30 }
];

// Cria um novo array apenas com os nomes dos usuários
const nomes = usuarios.map(usuario => usuario.nome);
console.log(nomes); // ['Ana', 'Bia']
```

#### Filtrando Valores com `filter()`

O método `filter()` cria um **novo array** com todos os elementos que passaram no teste implementado pela função de callback. É usado para **selecionar** elementos de um array. A função de callback deve retornar `true` para manter o elemento ou `false` para descartá-lo.

**Callback:** `function(currentValue, index, array)`

```js
const numeros = [1, 2, 3, 4, 5, 6];

// Cria um novo array apenas com os números pares
const pares = numeros.filter(num => num % 2 === 0);
console.log(pares); // [2, 4, 6]

const produtos = [
  { nome: 'Laptop', preco: 4000, fragil: true },
  { nome: 'Monitor', preco: 1500, fragil: true },
  { nome: 'Mouse', preco: 100, fragil: false }
];

// Filtra produtos caros e frágeis
const carosEFrageis = produtos.filter(p => p.preco > 1000 && p.fragil);
console.log(carosEFrageis); // [{ nome: 'Laptop', ... }, { nome: 'Monitor', ... }]
```

#### Reduzindo Valores com `reduce()`

O método `reduce()` executa uma função "redutora" para cada elemento do array, resultando em um **único valor de retorno**. É o mais flexível dos três, podendo ser usado para somar, multiplicar, achatar arrays, agrupar objetos, etc.

**Sintaxe:** `array.reduce(callback(accumulator, currentValue, currentIndex, array), initialValue)`

- `accumulator` (acumulador): O valor retornado na última invocação do callback ou o `initialValue`, se fornecido. Ele "acumula" o resultado.
- `currentValue` (valor atual): O elemento atual sendo processado.
- `initialValue` (valor inicial, opcional): Um valor para ser usado como o primeiro argumento para a primeira chamada do callback.

```js
const numeros = [1, 2, 3, 4, 5];

// Somando todos os números do array
const soma = numeros.reduce((acumulador, valorAtual) => acumulador + valorAtual, 0);
// 1ª it: 0 + 1 = 1
// 2ª it: 1 + 2 = 3
// 3ª it: 3 + 3 = 6
// ...
console.log(soma); // 15

// Encontrando o maior número
const maior = numeros.reduce((acc, val) => val > acc ? val : acc);
console.log(maior); // 5
```

## Busca e Validação

### Encontrando Elementos

- **`indexOf(item, fromIndex)`:** Retorna o primeiro índice em que um dado elemento pode ser encontrado no array, ou `-1` se não estiver presente.
- **`lastIndexOf(item, fromIndex)`:** Similar ao `indexOf`, mas busca de trás para frente.
- **`includes(item, fromIndex)`:** (ES6) Retorna `true` se um array inclui um determinado elemento, `false` caso contrário. É mais legível que `indexOf() !== -1`.

```js
const frutas = ['Maçã', 'Banana', 'Laranja', 'Banana'];
console.log(frutas.indexOf('Banana'));    // 1
console.log(frutas.lastIndexOf('Banana')); // 3
console.log(frutas.includes('Manga'));     // false
```

- **`find(callback)`:** Retorna o **valor do primeiro elemento** no array que satisfaz a função de teste fornecida. Caso contrário, `undefined` é retornado.
- **`findIndex(callback)`:** Retorna o **índice do primeiro elemento** que satisfaz a função, ou `-1` se nenhum satisfizer.

```js
const usuarios = [
  { id: 1, nome: 'Ana' },
  { id: 2, nome: 'Bia' },
  { id: 3, nome: 'Carlos' }
];

const usuarioBia = usuarios.find(u => u.nome === 'Bia');
console.log(usuarioBia); // { id: 2, nome: 'Bia' }

const indiceCarlos = usuarios.findIndex(u => u.nome === 'Carlos');
console.log(indiceCarlos); // 2
```

### Testando Todos os Elementos

- **`every(callback)`:** Testa se **todos** os elementos no array passam pelo teste implementado pela função fornecida. Retorna um booleano.
- **`some(callback)`:** Testa se **pelo menos um** elemento no array passa pelo teste. Retorna um booleano.

```js
const idades = [22, 35, 18, 41];

const todosMaioresDeIdade = idades.every(idade => idade >= 18);
console.log(todosMaioresDeIdade); // true

const existeMenorDeIdade = idades.some(idade => idade < 18);
console.log(existeMenorDeIdade); // false (neste caso)
```

## Ordenação e Reversão

### `sort()`

O método `sort()` ordena os elementos de um array **no próprio local (in-place)** e retorna o array. A ordenação padrão é baseada na conversão dos elementos para strings e na comparação de suas sequências de valores de UTF-16. **Isso leva a um comportamento inesperado com números!**

```js
const numeros = [10, 2, 5, 1, 21];
numeros.sort();
console.log(numeros); // [1, 10, 2, 21, 5] (Incorreto!)
```

Para ordenar corretamente, você **deve** fornecer uma **função de comparação**. `compareFunction(a, b)`

- Se retornar `< 0`, `a` vem antes de `b`.
- Se retornar `> 0`, `b` vem antes de `a`.
- Se retornar `0`, a ordem de `a` e `b` não muda.

```js
const numeros = [10, 2, 5, 1, 21];

// Ordenação numérica crescente
numeros.sort((a, b) => a - b);
console.log(numeros); // [1, 2, 5, 10, 21]

// Ordenação numérica decrescente
numeros.sort((a, b) => b - a);
console.log(numeros); // [21, 10, 5, 2, 1]
```

A ordenação de um array de objetos é feita da mesma forma, comparando uma propriedade específica.

```js
const produtos = [
  { nome: 'Laptop', preco: 4000 },
  { nome: 'Mouse', preco: 100 },
  { nome: 'Monitor', preco: 1500 }
];

// Ordenando por preço
produtos.sort((a, b) => a.preco - b.preco);
console.log(produtos);
```

### `reverse()`

O método `reverse()` inverte a ordem dos elementos de um array **in-place**. O primeiro elemento torna-se o último, e o último torna-se o primeiro.

```js
const letras = ['a', 'b', 'c', 'd'];
letras.reverse();
console.log(letras); // ['d', 'c', 'b', 'a']
```

## Tópicos Avançados e Dicas Práticas

### Achatando Arrays com `flat()` e `flatMap()`

- **`flat(depth)`:** (ES2019) Cria um novo array com todos os elementos de sub-arrays concatenados nele recursivamente até a profundidade (`depth`) especificada.
    
    ```js
    const arrAninhado = [1, 2, [3, 4, [5, 6]]];
    console.log(arrAninhado.flat());     // [1, 2, 3, 4, [5, 6]] (depth padrão é 1)
    console.log(arrAninhado.flat(2));   // [1, 2, 3, 4, 5, 6]
    console.log(arrAninhado.flat(Infinity)); // Achata todos os níveis
    ```
    
- **`flatMap(callback)`:** (ES2019) É equivalente a executar um `map()` seguido por um `flat(1)`. É útil e mais performático que a combinação dos dois.
    
    ```js
    const frases = ["olá mundo", "javascript é legal"];
    const palavras = frases.flatMap(frase => frase.split(' '));
    console.log(palavras); // ["olá", "mundo", "javascript", "é", "legal"]
    ```

### Desestruturação de Array (Destructuring)

A desestruturação é uma sintaxe do ES6 que permite extrair valores de arrays (ou propriedades de objetos) e atribuí-los a variáveis de forma concisa.

```js
const coordenadas = [10, 20, 30];

const [x, y, z] = coordenadas;
console.log(x, y, z); // 10 20 30

// Pegando apenas os primeiros e o resto com o operador Rest (...)
const [primeiro, ...resto] = ['a', 'b', 'c', 'd'];
console.log(primeiro); // 'a'
console.log(resto);    // ['b', 'c', 'd']
```

### Removendo Duplicatas

A forma mais moderna e eficiente de remover elementos duplicados de um array é combinando `Set` (uma coleção que só permite valores únicos) com o operador Spread.

```js
const numerosRepetidos = [1, 2, 5, 2, 1, 4, 3, 5];
const numerosUnicos = [...new Set(numerosRepetidos)];
console.log(numerosUnicos); // [1, 2, 5, 4, 3]
```

### Convertendo de e para Arrays

- **`String.prototype.split(separator)`:** Converte uma string em um array.
- **`Array.prototype.join(separator)`:** Junta os elementos de um array em uma string.

    ```js
    const csv = "ana,bia,carlos";
    const nomes = csv.split(','); // ["ana", "bia", "carlos"]
    
    const novoCsv = nomes.join(';'); // "ana;bia;carlos"
    ```
    
- **`Array.from(arrayLikeOrIterable)`:** Cria um novo array a partir de um objeto "semelhante a um array" (como uma `NodeList` do DOM) ou um objeto iterável (como um `Map` ou `Set`).
- **`Object.keys()`, `Object.values()`, `Object.entries()`:** Convertem as chaves, valores ou pares `[chave, valor]` de um objeto em um array.
    
    ```
    const usuario = { nome: 'João', idade: 25 };
    console.log(Object.keys(usuario));   // ['nome', 'idade']
    console.log(Object.values(usuario)); // ['João', 25]
    console.log(Object.entries(usuario)); // [['nome', 'João'], ['idade', 25]]
    ```

### Combinando Arrays em Pares Chave-Valor

É possível criar um objeto a partir de dois arrays (um de chaves, outro de valores) usando `reduce` ou o mais moderno `Object.fromEntries()`.

```js
const chaves = ['nome', 'idade', 'cidade'];
const valores = ['Maria', 30, 'Recife'];

const usuarioObj = Object.fromEntries(chaves.map((chave, i) => [chave, valores[i]]));
console.log(usuarioObj); // { nome: 'Maria', idade: 30, cidade: 'Recife' }
```

## Considerações Finais

Neste capítulo, dissecamos a estrutura de dados mais onipresente em JavaScript: o Array. Vimos que ele é muito mais do que uma simples lista; é uma poderosa coleção de ferramentas para manipulação de dados.

Exploramos os métodos que modificam arrays diretamente (mutação), como `push`, `pop` e `splice`, e enfatizamos a importância dos métodos que promovem a imutabilidade, retornando novos arrays, como `slice`, `concat` e o operador Spread. O coração do capítulo foi a exploração do trio funcional — `map`, `filter` e `reduce` — que são essenciais para escrever um código moderno, declarativo e expressivo.

Abordamos também tarefas práticas como ordenação (ressaltando a necessidade da função de comparação), busca, validação e conversão de e para outros tipos de dados. Por fim, vimos como recursos do ES6+, como a desestruturação e o `Set`, simplificaram operações que antes eram mais verbosas.

Dominar os arrays e seu rico ecossistema de métodos é um passo fundamental para se tornar um desenvolvedor JavaScript proficiente. Com este conhecimento, você está agora equipado para lidar com virtualmente qualquer tarefa que envolva coleções de dados, de forma limpa, eficiente e robusta.
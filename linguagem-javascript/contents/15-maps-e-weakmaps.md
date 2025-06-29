# Capítulo 15 – Map e WeakMap: Coleções Chave-Valor Avançadas

Em nossa exploração das estruturas de dados do JavaScript, dedicamos um tempo considerável para entender os Objetos. Vimos que eles são a base de quase tudo na linguagem, funcionando como coleções flexíveis de pares chave-valor. Por muitos anos, os desenvolvedores usaram objetos simples (`{}`) como sua principal ferramenta para criar dicionários ou "hash maps" — estruturas que mapeiam uma chave a um valor. No entanto, usar um objeto para essa finalidade, embora comum, vem com uma série de limitações e armadilhas sutis.

E se precisássemos usar um **outro objeto** como chave? Ou uma função? Com objetos literais, isso não é possível, pois as chaves são coagidas para strings. E o que acontece se, sem querer, usarmos uma chave como `"toString"` e acidentalmente entrarmos em conflito com os métodos herdados de `Object.prototype`? Como podemos, de forma simples e direta, saber quantos itens existem em nosso "mapa"? Para resolver esses e outros problemas, o ECMAScript 2015 (ES6) introduziu uma nova e poderosa estrutura de dados nativa: o **`Map`**.

Neste capítulo, faremos um mergulho profundo no `Map`, a estrutura de dados moderna e preferida para coleções chave-valor. Vamos dissecar sua API, aprendendo a criar, popular, consultar e iterar sobre `Maps` de forma eficiente. Mais importante, vamos entender as vantagens cruciais que ele oferece sobre os objetos tradicionais. Em seguida, exploraremos seu primo mais enigmático, o **`WeakMap`**. Desvendaremos o conceito de referências fracas e por que o `WeakMap` é uma ferramenta indispensável para o gerenciamento de memória em cenários específicos, como o armazenamento de metadados de objetos, evitando vazamentos de memória (memory leaks). Ao final, você terá um entendimento claro de quando e por que escolher `Map`, `WeakMap` ou um objeto simples para suas necessidades de mapeamento de dados.

## O Problema com Objetos como Mapas

Antes de celebrarmos a solução, vamos entender o problema. Usar um objeto literal (`{}`) como um mapa funciona em muitos casos, mas falha em três áreas principais:

**1. Chaves são limitadas a Strings e Symbols:** Qualquer valor que não seja uma string ou um symbol usado como chave de um objeto é implicitamente convertido para uma string. Isso significa que você não pode usar, por exemplo, um objeto como uma chave distinta.

```js
const objChave = { id: 1 };
const mapaDeObjetos = {};

mapaDeObjetos[objChave] = "Este é o valor";

// A chave foi convertida para a string "[object Object]"
console.log(Object.keys(mapaDeObjetos)); // ["[object Object]"]

const outroObjChave = { id: 2 };
mapaDeObjetos[outroObjChave] = "Outro valor";

// Como o outro objeto também vira "[object Object]", ele sobrescreve a chave anterior!
console.log(mapaDeObjetos[objChave]); // "Outro valor"
```

**2. Risco de Colisão com Chaves do Protótipo:** Um objeto literal herda de `Object.prototype`, que contém métodos como `toString`, `constructor`, etc. Se você tentar usar uma dessas palavras como chave, pode ter resultados inesperados ou sobrescrever a funcionalidade herdada.

```js
const dadosUsuario = {};
dadosUsuario.toString = "Informação do usuário";

// Agora, chamar o método original pode quebrar
// dadosUsuario.toString(); // TypeError: dadosUsuario.toString is not a function
```

**3. Dificuldade de Manipulação:** Tarefas simples como obter o número de itens ou iterar sobre as chaves e valores de forma garantida exigem o uso de métodos estáticos como `Object.keys(obj).length` ou `Object.entries(obj)`, o que é menos direto.

## Entra em Cena o `Map`: O Dicionário Moderno

O `Map` foi projetado especificamente para ser uma coleção de pares chave-valor otimizada, resolvendo todas as limitações mencionadas.

### Criando e Inicializando um `Map`

Um `Map` é criado usando o construtor `new Map()`. Opcionalmente, você pode passar um **iterável** (como um array) de pares `[chave, valor]` para inicializá-lo.

```js
// Criando um Map vazio
const mapaVazio = new Map();

// Criando um Map a partir de um array de pares
const usuarios = new Map([
  [1, { nome: 'Ana', email: 'ana@example.com' }],
  [2, { nome: 'Bia', email: 'bia@example.com' }]
]);

console.log(usuarios);
```

### Métodos Essenciais do `Map`

A API do `Map` é simples, intuitiva e poderosa.

#### `set(chave, valor)`: Adicionando e Atualizando Elementos

O método `set()` adiciona ou atualiza um par chave-valor. Ele retorna o próprio objeto `Map`, o que permite encadear chamadas.

```js
const mapaDePrecos = new Map();

// Adicionando novos elementos
mapaDePrecos.set('Maçã', 5.50);
mapaDePrecos.set('Banana', 3.99);

// O encadeamento de chamadas é possível
mapaDePrecos.set('Laranja', 4.00).set('Uva', 8.00);

// Atualizando um valor existente
mapaDePrecos.set('Maçã', 6.00);

console.log(mapaDePrecos);
```

#### `get(chave)`: Obtendo um Valor

O método `get()` retorna o valor associado à chave. Se a chave não existir, ele retorna `undefined`.

```js
console.log(mapaDePrecos.get('Banana')); // 3.99
console.log(mapaDePrecos.get('Pera'));   // undefined
```

#### `has(chave)`: Verificando se uma Chave Existe

O método `has()` retorna um booleano: `true` se a chave existir no `Map`, e `false` caso contrário.

```js
console.log(mapaDePrecos.has('Laranja')); // true
console.log(mapaDePrecos.has('Abacaxi')); // false
```

#### `delete(chave)`: Removendo um Elemento

O método `delete()` remove o par chave-valor associado à chave. Retorna `true` se o elemento existia e foi removido, e `false` se o elemento não existia.

```js
const foiRemovido = mapaDePrecos.delete('Uva');
console.log(foiRemovido); // true
console.log(mapaDePrecos.has('Uva')); // false
```

#### `clear()`: Limpando o `Map`

O método `clear()` remove **todos** os elementos do `Map`, deixando-o vazio.

```js
mapaDePrecos.clear();
console.log(mapaDePrecos.size); // 0
```

#### `size`: Obtendo o Número de Elementos

Diferente dos objetos, que precisam de `Object.keys().length`, o `Map` possui uma propriedade `size` que retorna diretamente o número de elementos.

```js
const mapa = new Map([['a', 1], ['b', 2]]);
console.log(mapa.size); // 2
```

### A Vantagem das Chaves: Qualquer Valor é uma Chave

Esta é a maior vantagem do `Map`. Qualquer valor, seja primitivo ou objeto, pode ser usado como chave, e a identidade do valor é preservada.

```js
const mapaFlexivel = new Map();

const objChave = { id: 1 };
const fnChave = () => console.log('função');

mapaFlexivel.set('uma string', 'valor 1');
mapaFlexivel.set(objChave, 'valor 2');
mapaFlexivel.set(fnChave, 'valor 3');
mapaFlexivel.set(NaN, 'valor 4'); // Até NaN pode ser uma chave!

console.log(mapaFlexivel.get(objChave)); // 'valor 2'
console.log(mapaFlexivel.get(NaN)); // 'valor 4'
```

### Iterando sobre Maps

`Map` é um objeto **iterável**. Isso significa que podemos usar o laço `for...of` e outros mecanismos de iteração de forma direta e previsível. A ordem de iteração é garantida como a ordem de inserção.

```js
const mapaDePaises = new Map([
  ['BR', 'Brasil'],
  ['PT', 'Portugal'],
  ['AO', 'Angola']
]);

// Usando for...of com desestruturação (a forma mais comum)
for (const [chave, valor] of mapaDePaises) {
  console.log(`A chave ${chave} representa o país ${valor}.`);
}
```

O `Map` também fornece três métodos iteradores:

- **`map.keys()`:** Retorna um iterador com apenas as chaves.
- **`map.values()`:** Retorna um iterador com apenas os valores.
- **`map.entries()`:** Retorna um iterador com os pares `[chave, valor]`. O `for...of` sobre o `Map` usa este método por padrão.

Além disso, ele possui um método `forEach`:

- **`map.forEach((valor, chave, mapa) => { ... })`:** Executa uma função de callback para cada par chave-valor. Note a ordem dos argumentos: `valor` vem antes de `chave`.

```js
mapaDePaises.forEach((valor, chave) => {
  console.log(`${chave}: ${valor}`);
});
```

## `WeakMap`: Gerenciando Memória com Referências Fracas

Agora, vamos conhecer o primo especializado do `Map`. O `WeakMap` parece similar à primeira vista, mas opera sob um princípio fundamentalmente diferente que o torna uma ferramenta crucial para o gerenciamento de memória.

### A Necessidade de Referências Fracas e o Risco de Memory Leaks

Quando você adiciona um objeto como chave (ou valor) em um `Map` normal, o `Map` cria uma **referência forte** para esse objeto. Isso significa que, enquanto o `Map` existir, o objeto não poderá ser removido da memória pelo **Coletor de Lixo (Garbage Collector - GC)**, mesmo que nenhuma outra parte do seu programa esteja usando aquele objeto. Isso pode criar um **vazamento de memória (memory leak)**.

**Exemplo de Memory Leak com `Map`:**

```js
let mapaForte = new Map();
let usuario = { nome: 'João' }; // Objeto criado

mapaForte.set(usuario, { metadados: 'informação privada' });

// Em algum momento, não precisamos mais do 'usuario'
usuario = null;

// Mesmo com 'usuario' sendo nulo, o objeto { nome: 'João' } ainda existe na memória
// porque o 'mapaForte' mantém uma referência FORTE a ele. Ele nunca será limpo.
```

### O que é uma Referência Fraca?

O `WeakMap` resolve esse problema usando **referências fracas**. Uma referência fraca a um objeto não impede que o Coletor de Lixo remova o objeto da memória se ele for a **única** referência restante.

Em outras palavras, o `WeakMap` permite que você "associe" dados a um objeto, mas se esse objeto for removido de todas as outras partes do seu programa, o `WeakMap` não o manterá "vivo". A entrada correspondente no `WeakMap` será removida automaticamente pelo GC.

### Trabalhando com `WeakMap`

Devido a essa natureza, o `WeakMap` tem algumas restrições importantes:

1. **As chaves devem ser objetos.** Valores primitivos não são permitidos como chaves.
2. **Não é iterável.** Você não pode usar `for...of`, nem obter uma lista de suas chaves ou valores (`.keys()`, `.values()` não existem). Isso ocorre porque as entradas podem desaparecer a qualquer momento por ação do GC, tornando uma lista de chaves instantaneamente desatualizada.

#### Criando e Usando um `WeakMap`

A API é um subconjunto da API do `Map`.

- `new WeakMap([iterable])`: Cria um novo `WeakMap`.
- `weakMap.set(chaveObjeto, valor)`: Associa um valor a uma chave de objeto.
- `weakMap.get(chaveObjeto)`: Obtém o valor associado.
- `weakMap.has(chaveObjeto)`: Verifica a existência.
- `weakMap.delete(chaveObjeto)`: Remove uma entrada.

**Exemplo Corrigindo o Memory Leak:**

```js
let mapaFraco = new WeakMap();
let outroUsuario = { nome: 'Maria' };

mapaFraco.set(outroUsuario, { dadosDeSessao: 'xyz...' });

console.log(mapaFraco.has(outroUsuario)); // true

// Agora, quando a única outra referência ao objeto é removida...
outroUsuario = null;

// ...o Coletor de Lixo está agora livre para remover o objeto { nome: 'Maria' } da memória,
// e a entrada correspondente em 'mapaFraco' também será limpa automaticamente.
// Não podemos provar isso com código porque não podemos iterar, mas é assim que funciona.
```

## `Map` vs. `WeakMap` vs. `Object`: Quando Usar Cada Um?

- **Use um `Object` quando:**
    - Você tem um conjunto simples e fixo de chaves que são strings ou symbols.
    - Você precisa de uma estrutura de dados leve e facilmente serializável para JSON.
    - É um caso de uso simples, como um objeto de configuração.
- **Use um `Map` quando:**
    - Você precisa de **chaves que não sejam strings** (objetos, funções, etc.).
    - As chaves são dinâmicas ou desconhecidas em tempo de compilação.
    - Você precisa de uma coleção que mantenha a ordem de inserção.
    - Você precisa de uma maneira fácil e performática de obter o tamanho (`.size`) ou iterar.
    - **Este deve ser seu dicionário/hash map padrão na maioria dos casos.**
- **Use um `WeakMap` quando:**
    - Você precisa associar dados a um objeto **sem impedir que esse objeto seja coletado pelo lixo**.
    - Seu caso de uso principal é armazenar metadados, cache ou informações privadas sobre um objeto, onde os dados só são relevantes enquanto o objeto existir.

## Considerações Finais

Neste capítulo, elevamos nossa compreensão sobre coleções chave-valor em JavaScript. Vimos que, embora objetos sejam onipresentes, o **`Map`** é a ferramenta superior e mais robusta para a criação de dicionários, oferecendo flexibilidade total no tipo de chave, uma API intuitiva e um comportamento de iteração previsível.

Desmistificamos o **`WeakMap`**, revelando-o não como uma versão "inferior" do `Map`, mas como uma ferramenta de engenharia de software altamente especializada, projetada com um propósito claro: o gerenciamento de memória. A compreensão de suas referências fracas nos permite escrever aplicações mais eficientes e livres de vazamentos de memória.

A escolha entre `Object`, `Map` e `WeakMap` é uma decisão de arquitetura. Com o conhecimento adquirido aqui, você está agora capacitado a escolher a estrutura de dados certa para o trabalho certo, um sinal de um desenvolvedor JavaScript maduro e consciente.
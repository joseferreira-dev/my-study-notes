# Capítulo 16 – Set e WeakSet: Coleções de Valores Únicos

No universo das estruturas de dados, já exploramos os Arrays para listas ordenadas e os Maps para dicionários chave-valor. No entanto, há uma outra necessidade fundamental na programação: a de gerenciar uma coleção onde cada item é **garantidamente único**. Imagine que você precisa registrar todos os usuários que visitaram uma página, sem duplicatas. Ou talvez você precise de uma lista de todas as tags de categoria usadas em um blog. Usar um array para isso exigiria uma verificação manual (`array.includes(item)`) antes de cada inserção para evitar a duplicidade, um processo que pode ser verboso e ineficiente para grandes volumes de dados.

Para resolver este problema com elegância e performance, o ECMAScript 2015 (ES6) introduziu o **`Set`**. Inspirado diretamente no conceito matemático de um conjunto, um `Set` é uma coleção de valores onde cada valor pode ocorrer apenas uma vez. Ele nos fornece uma API otimizada para adicionar, remover e verificar a existência de itens, abstraindo toda a complexidade da verificação de unicidade.

Neste capítulo, faremos um mergulho profundo nesta poderosa estrutura de dados. Vamos dissecar a API do `Set`, aprendendo como criar, manipular e iterar sobre coleções de valores únicos. Exploraremos como ele pode ser usado para tarefas práticas, como a remoção de duplicatas de um array e a execução de operações de conjunto, como interseção e diferença. Em seguida, assim como fizemos com o `Map`, vamos investigar seu primo mais especializado, o **`WeakSet`**. Analisaremos como suas referências fracas o tornam a ferramenta ideal para rastrear objetos sem criar vazamentos de memória, um padrão avançado para o gerenciamento de recursos em aplicações complexas. Ao final, você terá um entendimento claro de quando um `Set` é a escolha superior a um `Array` e como usar cada uma dessas coleções para escrever um código mais limpo, declarativo e eficiente.

## O que é um `Set`?

Um `Set` é uma coleção de valores únicos. A principal característica que o define é que nenhum valor pode aparecer mais de uma vez dentro dele. Se você tentar adicionar um valor que já existe, a operação será simplesmente ignorada, garantindo que a coleção permaneça livre de duplicatas.

**Quando usar um `Set` em vez de um `Array`?**

- Quando a **unicidade** dos elementos é um requisito fundamental da sua lógica de negócio.
- Quando a performance da verificação de existência de um item (`has`) é crítica. Em um `Set`, essa operação é, em média, muito mais rápida (próxima de $O(1)$) do que o `array.includes()` (que é $O(n)$).
- Quando você precisa realizar operações matemáticas de conjunto, como união, interseção ou diferença.

### Criando um `Set`

Um `Set` é criado usando o construtor `new Set()`. Ele pode ser inicializado vazio ou a partir de qualquer objeto **iterável**, como um array. Ao inicializar com um iterável, todas as duplicatas são automaticamente removidas.

```js
// Criando um Set vazio
const conjuntoVazio = new Set();

// Criando um Set a partir de um array
const numeros = [1, 2, 5, 2, 1, 4, 3, 5];
const conjuntoDeNumeros = new Set(numeros);

console.log(conjuntoDeNumeros); // Set(5) { 1, 2, 5, 4, 3 }
```

Este comportamento torna o `Set` a ferramenta mais idiomática e eficiente para remover duplicatas de um array.

### Métodos Essenciais do `Set`

A API do `Set` é concisa e focada em suas responsabilidades principais.

#### `add(valor)`: Adicionando um Valor

O método `add()` adiciona um novo valor ao `Set`. Se o valor já existir, nada acontece. Ele retorna o próprio objeto `Set`, permitindo o encadeamento de chamadas.

```js
const tags = new Set();

tags.add('JavaScript');
tags.add('Programação');
tags.add('Web');

// Tentando adicionar um valor duplicado
tags.add('JavaScript'); // Ignorado silenciosamente

// Encadeando chamadas
tags.add('Frontend').add('Backend');

console.log(tags); // Set(5) { 'JavaScript', 'Programação', 'Web', 'Frontend', 'Backend' }
```

#### `has(valor)`: Verificando se um Valor Existe

O método `has()` retorna um booleano: `true` se o valor existir no `Set`, e `false` caso contrário.

```js
console.log(tags.has('JavaScript')); // true
console.log(tags.has('Mobile'));     // false
```

#### `delete(valor)`: Removendo um Valor

O método `delete()` remove um valor do `Set`. Retorna `true` se o valor existia e foi removido, e `false` se o valor não existia.

```js
const foiRemovido = tags.delete('Web');
console.log(foiRemovido); // true
console.log(tags.has('Web')); // false
```

#### `clear()`: Limpando o `Set`

O método `clear()` remove **todos** os valores do `Set`.

```js
tags.clear();
console.log(tags.size); // 0
```

#### `size`: Obtendo o Número de Elementos

Similar ao `Map`, o `Set` possui uma propriedade `size` que informa o número de elementos na coleção.

```js
const conjunto = new Set(['a', 'b', 'c']);
console.log(conjunto.size); // 3
```

### O que o `Set` Considera Único?

O `Set` usa um algoritmo chamado **SameValueZero** para comparar valores. Na prática, isso significa que:

- Valores primitivos são comparados por seu valor: `5` é igual a `5`, `'olá'` é igual a `'olá'`.
- `NaN` é considerado igual a `NaN`, ao contrário da comparação com `===`. Um `Set` só pode ter um `NaN`.
- Objetos e arrays são comparados por sua **referência**. Dois objetos com as mesmas propriedades são considerados diferentes.

```js
const conjuntoEspecial = new Set();
conjuntoEspecial.add(5);
conjuntoEspecial.add("5"); // Diferente, pois o tipo é diferente

const obj1 = { id: 1 };
const obj2 = { id: 1 };

conjuntoEspecial.add(obj1);
conjuntoEspecial.add(obj1); // Ignorado, mesma referência
conjuntoEspecial.add(obj2); // Adicionado, referência diferente

console.log(conjuntoEspecial); // Set(4) { 5, '5', { id: 1 }, { id: 1 } }
```

## Trabalhando com `Set`s

### Iterando sobre `Set`s

Assim como o `Map`, o `Set` é um objeto **iterável**, e a ordem de iteração é garantida como a ordem de inserção. A forma mais comum de iteração é o laço `for...of`.

```js
const permissoes = new Set(['ler', 'escrever', 'executar']);

for (const permissao of permissoes) {
  console.log(`- ${permissao}`);
}
```

O `Set` também fornece os métodos `keys()`, `values()`, e `entries()` para compatibilidade com a API do `Map`.

- `set.keys()` e `set.values()`: Ambos retornam um iterador com os valores do `Set`. Eles são funcionalmente idênticos.
- `set.entries()`: Retorna um iterador com pares `[valor, valor]`. Isso pode parecer estranho, mas existe para manter a consistência com a API do `Map`, permitindo que algoritmos que esperam pares `[chave, valor]` funcionem com `Set`s também.

O método `forEach` também está disponível: `set.forEach((valor, chave, set) => { ... })`, onde `valor` e `chave` serão o mesmo.

### Convertendo `Set` para `Array`

É muito comum precisar converter um `Set` de volta para um `Array` para usar métodos específicos de array, como `sort()` ou `map()`. As duas formas principais são:

- **Operador Spread `...` (Recomendado):** `const meuArray = [...meuSet];`
- **`Array.from()`:** `const meuArray = Array.from(meuSet);`

```js
const conjuntoDeLetras = new Set(['c', 'a', 'b']);
const arrayOrdenado = [...conjuntoDeLetras].sort();

console.log(arrayOrdenado); // ['a', 'b', 'c']
```

### Operações de Conjunto: Interseção e Diferença

A verdadeira força dos `Set`s se manifesta ao realizar operações lógicas entre coleções.

#### Interseção: Elementos em Comum

A interseção de dois conjuntos A e B é um novo conjunto contendo todos os elementos que existem **tanto em A quanto em B**.

```js
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

// Para encontrar a interseção, filtramos um dos conjuntos,
// mantendo apenas os itens que também existem no outro.
const intersecao = new Set([...setA].filter(x => setB.has(x)));

console.log(intersecao); // Set(2) { 3, 4 }
```

#### Diferença: Elementos Exclusivos

A diferença entre dois conjuntos A e B (A - B) é um novo conjunto contendo todos os elementos que existem em A, mas **não** existem em B.

```js
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

// Filtramos A, mantendo apenas os itens que NÃO existem em B.
const diferenca = new Set([...setA].filter(x => !setB.has(x)));

console.log(diferenca); // Set(2) { 1, 2 }
```

## `WeakSet`: Coleções de Objetos com Referências Fracas

Assim como o `WeakMap` é uma versão especializada do `Map`, o `WeakSet` é uma versão especializada do `Set`. Ele segue o mesmo princípio de usar **referências fracas** para seus elementos, o que tem implicações diretas no gerenciamento de memória.

### Por que Usar um `WeakSet`?

Um `WeakSet` é uma coleção de **objetos** únicos. As referências que ele mantém para esses objetos são "fracas". Isso significa que se um objeto não tiver mais nenhuma outra referência forte apontando para ele em qualquer outra parte do programa, o Coletor de Lixo (Garbage Collector) pode removê-lo da memória, e ele será **automaticamente removido do `WeakSet`**.

Isso o torna a ferramenta perfeita para **rastrear uma coleção de objetos** sem impedir que eles sejam coletados pelo lixo quando não forem mais necessários, evitando memory leaks.

### Restrições e API do `WeakSet`

As restrições do `WeakSet` derivam diretamente de sua natureza fraca e não iterável:

1. **Só pode conter objetos.** Valores primitivos como strings ou números não são permitidos.
2. **Não é iterável.** Assim como o `WeakMap`, ele não possui os métodos `forEach`, `keys`, `values`, `entries`, e nem a propriedade `size`. É impossível saber "o que está dentro" a qualquer momento, pois o GC pode alterar seu conteúdo.

A API é um subconjunto simples da API do `Set`:

- **`new WeakSet([iterableDeObjetos])`:** Cria um novo `WeakSet`.
- **`weakSet.add(objeto)`:** Adiciona um objeto.
- **`weakSet.has(objeto)`:** Verifica se um objeto existe.
- **`weakSet.delete(objeto)`:** Remove um objeto.

### Caso de Uso Prático: Rastreando Elementos Processados

Imagine que você tem uma função que processa elementos do DOM, e você não quer processar o mesmo elemento duas vezes. Um `WeakSet` é perfeito para isso.

```js
const elementosProcessados = new WeakSet();

function processarElemento(elemento) {
  if (elementosProcessados.has(elemento)) {
    console.log("Este elemento já foi processado.");
    return;
  }

  // Lógica de processamento...
  console.log("Processando o elemento...");

  // Adiciona o elemento ao WeakSet para marcá-lo como processado.
  elementosProcessados.add(elemento);
}

// Supondo que temos dois elementos do DOM
const botao1 = document.querySelector('#botao1');
const botao2 = document.querySelector('#botao2');

processarElemento(botao1); // "Processando o elemento..."
processarElemento(botao1); // "Este elemento já foi processado."

// Se, em algum momento, o botao1 for removido do DOM
// e não tiver mais nenhuma outra referência a ele,
// o GC irá limpá-lo da memória, e a entrada correspondente
// em 'elementosProcessados' desaparecerá junto, sem causar memory leaks.
```

## `Set` vs. `WeakSet` vs. `Array`: Resumo Comparativo

|Característica|`Array`|`Set`|`WeakSet`|
|---|---|---|---|
|**Unicidade**|Não, permite duplicatas.|Sim, valores são sempre únicos.|Sim, objetos são sempre únicos.|
|**Tipos de Dados**|Qualquer tipo.|Qualquer tipo.|Apenas objetos.|
|**Ordem**|Ordenado por índice.|Ordenado pela ordem de inserção.|Não ordenado.|
|**Iteração**|Sim (`for`, `for...of`, `forEach`, etc.).|Sim (`for...of`, `forEach`, etc.).|**Não**.|
|**Acesso a Elementos**|Por índice (ex: `arr[0]`).|Não há acesso direto, apenas `has()`.|Não há acesso direto, apenas `has()`.|
|**Tamanho**|`.length`|`.size`|Não disponível.|
|**Referências**|Fortes.|Fortes.|**Fracas**.|
|**Caso de Uso Ideal**|Listas ordenadas, quando duplicatas são aceitáveis ou desejadas.|Coleções de valores únicos, remoção de duplicatas, operações de conjunto.|Rastrear objetos sem vazar memória, metadados transitórios.|

## Considerações Finais

Neste capítulo, adicionamos duas poderosas estruturas de coleção ao nosso arsenal. Vimos que o **`Set`** é a resposta moderna e eficiente do JavaScript para gerenciar coleções de valores únicos, superando as limitações e a verbosidade do uso de arrays para a mesma finalidade. Sua API simples e a capacidade de realizar operações de conjunto o tornam uma ferramenta indispensável.

Desmistificamos o **`WeakSet`**, entendendo que seu propósito não é ser uma coleção de propósito geral, mas sim uma ferramenta especializada para o gerenciamento de memória. Ao usar referências fracas, ele nos permite rastrear objetos de forma segura, evitando vazamentos de memória em aplicações complexas.

Saber quando usar um `Array`, um `Set` ou um `WeakSet` é um sinal de um desenvolvedor que pensa não apenas na funcionalidade, mas também na performance e na robustez de seu código. Com este conhecimento, você está mais preparado do que nunca para escolher a ferramenta certa para cada desafio de coleção de dados.
# Capítulo 13 – Estruturas de Repetição: Laços e Iterações

Até agora, nossa jornada nos capacitou a escrever programas que executam uma sequência de instruções de cima para baixo, tomando decisões condicionais com `if/else` para alterar seu curso. No entanto, o verdadeiro poder da computação se revela em sua capacidade de executar tarefas repetitivas de forma incansável e precisa. Imagine ter que processar cada um dos mil itens de um carrinho de compras, aplicar um filtro a cada pixel de uma imagem ou simplesmente exibir uma lista de nomes. Escrever uma linha de código para cada item seria impraticável e violaria o princípio mais fundamental da engenharia de software: **Don't Repeat Yourself (DRY)**, ou "Não se Repita".

Para resolver essa necessidade fundamental, todas as linguagens de programação oferecem estruturas de controle de fluxo conhecidas como **laços (loops)**. Um laço é um mecanismo que permite executar um bloco de código repetidamente, seja um número fixo de vezes ou enquanto uma determinada condição for verdadeira. O JavaScript, com sua rica história, fornece um arsenal diversificado de laços, desde os mais tradicionais e flexíveis, herdados de linguagens como C, até as construções mais modernas e declarativas introduzidas nas versões mais recentes do ECMAScript.

Neste capítulo, faremos uma imersão completa no universo dos laços e iterações. Começaremos com o laço `for` clássico, dissecando sua anatomia para entender seu poder e flexibilidade. Em seguida, exploraremos os laços `while` e `do...while`, ideais para situações onde o número de repetições é indeterminado. O foco então se voltará para as abordagens modernas: o laço `for...of`, a maneira preferida para iterar sobre os valores de coleções, e o laço `for...in`, com suas particularidades para a iteração sobre propriedades de objetos. Por fim, dominaremos as instruções `break` e `continue`, que nos dão o poder de controlar o fluxo de execução de dentro de um laço, incluindo o uso avançado de labels para gerenciar laços aninhados. Ao final, você terá um repertório completo para automatizar qualquer tarefa repetitiva com elegância e eficiência.

## Por que Precisamos de Laços?

Um laço permite que você execute uma ou mais instruções quantas vezes forem necessárias. A estrutura de um laço consiste, tipicamente, em três partes:

1. **Inicialização:** Uma expressão que define o estado inicial antes do laço começar (ex: criar uma variável contadora).
2. **Condição:** Uma expressão booleana que é verificada antes de cada repetição. Se a condição for `true`, o laço continua; se for `false`, ele termina.
3. **Expressão Final/Incremento:** Uma expressão que é executada ao final de cada repetição, geralmente para atualizar o estado e aproximar o laço de sua condição de término.

## O Laço `for` Clássico

O laço `for` é, talvez, a estrutura de repetição mais conhecida e flexível. Sua sintaxe compacta encapsula as três partes essenciais de um laço em uma única linha, tornando-o ideal para situações onde o número de iterações é conhecido ou pode ser facilmente calculado, como ao percorrer um array pelo seu índice.

**Sintaxe:** `for (inicialização; condição; expressão_final) {` `// bloco de código a ser executado` `}`

Vamos dissecar cada parte:

- `inicialização`: É executada **uma única vez**, antes de qualquer outra coisa. Geralmente, declara-se e inicializa-se uma variável contadora (ex: `let i = 0`).
- `condição`: É avaliada **antes** de cada iteração. Se for `true`, o bloco de código do laço é executado. Se for `false`, o laço é encerrado.
- `expressão_final`: É executada **após** cada iteração. Comumente usada para incrementar a variável contadora (ex: `i++`).

**Exemplo: Iterando sobre um Array**

```js
const frutas = ['Maçã', 'Banana', 'Laranja', 'Uva'];

console.log("Minha cesta de frutas:");
for (let i = 0; i < frutas.length; i++) {
  console.log(`- ${frutas[i]} (no índice ${i})`);
}
```

Neste exemplo, `let i = 0` é a inicialização. `i < frutas.length` é a condição que mantém o laço rodando enquanto `i` for menor que o tamanho do array. `i++` é a expressão final que incrementa o índice a cada passo. Este padrão dá-nos controle total sobre o processo, incluindo o acesso ao índice, o que pode ser muito útil.

## Os Laços `while` e `do...while`

Enquanto o laço `for` é ótimo para um número definido de iterações, os laços `while` são perfeitos para quando não sabemos quantas vezes o laço precisa rodar, e a continuação depende de uma condição externa que pode mudar a qualquer momento.

### O Laço `while`

O laço `while` é a mais simples das estruturas de repetição. Ele executa um bloco de código repetidamente enquanto uma condição especificada for `true`.

**Sintaxe:** `while (condição) {` `// bloco de código` `}`

A condição é avaliada **antes** de cada iteração. Se a condição for falsa desde o início, o bloco de código nunca será executado.

**Exemplo: Processando uma Fila**

```js
let fila = ['Cliente A', 'Cliente B', 'Cliente C'];

while (fila.length > 0) {
  let clienteAtual = fila.shift(); // remove o primeiro cliente da fila
  console.log(`Atendendo ${clienteAtual}...`);
}

console.log("Fila vazia!");
```

Neste caso, o laço continua enquanto a fila tiver itens. A cada iteração, removemos um item, aproximando a condição `fila.length > 0` de se tornar falsa.

**Cuidado com Laços Infinitos:** Uma armadilha comum com `while` é criar um laço infinito. Se a condição nunca se tornar falsa, o programa ficará preso, consumindo recursos até travar.

```js
// CUIDADO: Laço infinito!
// while (true) {
//   console.log("Preso para sempre!");
// }
```

Sempre garanta que algo dentro do laço `while` eventualmente fará com que a condição se torne falsa.

### O Laço `do...while`

O laço `do...while` é uma variação do `while`. A principal e única diferença é que a condição é verificada **ao final** da iteração, e não no início. Isso garante que o bloco de código seja executado **pelo menos uma vez**, independentemente da condição ser verdadeira ou falsa.

**Sintaxe:** `do {` `// bloco de código` `} while (condição);`

**Exemplo: Validando Entrada de Usuário**

```js
let entrada;
do {
  // A função prompt() não funciona neste ambiente, mas é um bom exemplo conceitual.
  // entrada = prompt("Digite um número maior que 10:");
  entrada = Math.random() > 0.1 ? 5 : 15; // Simulação de entrada
  console.log(`Você digitou: ${entrada}`);
} while (entrada <= 10);

console.log("Número válido inserido!");
```

O bloco `do` executa na primeira vez, solicitando a entrada. Só então a condição `while (entrada <= 10)` é verificada para decidir se o laço deve continuar.

## Iterações Modernas: `for...of` e `for...in`

O ECMAScript 6 introduziu novas formas de iteração que são mais limpas, mais legíveis e menos propensas a erros do que o laço `for` tradicional.

### `for...of`: A Escolha para Iteráveis

O laço `for...of` é a maneira moderna e recomendada para iterar sobre os **valores** de objetos **iteráveis**. Os principais iteráveis nativos são Arrays, Strings, Maps, Sets e NodeLists (do DOM).

**Sintaxe:** `for (const elemento of iteravel) {` `// bloco de código` `}`

**Exemplo com Array:**

```js
const notas = [10, 8.5, 9.3];
let soma = 0;

for (const nota of notas) {
  soma += nota;
}

const media = soma / notas.length;
console.log(`A média das notas é ${media.toFixed(2)}.`);
```

Note como o código é mais limpo. Não precisamos gerenciar um índice (`i`), nem nos preocupar com a condição de parada. Simplesmente declaramos que queremos cada `nota` dentro do array `notas`.

**Exemplo com String:**

```js
const palavra = "JavaScript";
for (const letra of palavra) {
  console.log(letra);
}
```

### `for...in`: Para Propriedades de Objetos

O laço `for...in` itera sobre as **chaves (nomes das propriedades)** que são **enumeráveis** de um objeto.

**Sintaxe:** `for (const chave in objeto) {` `// bloco de código` `}`

**Exemplo de Uso Correto:**

```js
const usuario = {
  nome: 'Ana',
  idade: 28,
  cidade: 'Recife'
};

for (const chave in usuario) {
  console.log(`${chave}: ${usuario[chave]}`);
}
// Saída:
// nome: Ana
// idade: 28
// cidade: Recife
```

#### Atenção: `for...in` não é para Arrays!

É um erro comum e perigoso usar `for...in` para iterar sobre arrays. As razões são:

1. **Ordem não garantida:** A especificação não garante que `for...in` retornará os índices na ordem numérica.
2. **Itera sobre propriedades não-numéricas:** Se alguém adicionar uma propriedade a `Array.prototype` (uma má prática, mas possível), o `for...in` irá iterar sobre ela.
3. **Performance:** Geralmente é mais lento que outros laços para arrays.

**Conclusão:** Use `for...of` para os valores de arrays e iteráveis. Use `for...in` apenas para as chaves de objetos simples.

## Controle de Fluxo Dentro dos Laços: `break` e `continue`

Às vezes, precisamos de um controle mais fino sobre a execução de um laço. `break` e `continue` nos dão esse poder.

### `break`: Interrupção Total

A instrução `break` **termina imediatamente** o laço em que está contida (o laço mais interno) e o programa continua a execução na primeira instrução após o laço.

**Exemplo: Encontrando o Primeiro Número Negativo**

```js
const numeros = [10, 25, 50, -5, 100, 20];
let numeroNegativoEncontrado = null;

for (const numero of numeros) {
  if (numero < 0) {
    numeroNegativoEncontrado = numero;
    break; // Encontrou o que procurava, sai do laço.
  }
  console.log(`Processando ${numero}...`);
}

console.log(`O primeiro número negativo é: ${numeroNegativoEncontrado}`);
```

### `continue`: Pulando uma Iteração

A instrução `continue` **pula o restante do código da iteração atual** e avança para a próxima iteração do laço.

**Exemplo: Processando Apenas Números Pares**

```js
for (let i = 1; i <= 10; i++) {
  if (i % 2 !== 0) {
    continue; // Se for ímpar, pula para a próxima iteração.
  }
  console.log(`${i} é um número par.`);
}
```

## Controle Avançado: Labels para `break` e `continue`

Em situações com laços aninhados (um laço dentro de outro), `break` e `continue` por padrão afetam apenas o laço mais interno. Para controlar laços externos, podemos usar **labels**.

Um label é simplesmente um identificador seguido por dois pontos (`:`), colocado antes de uma estrutura de laço.

### `break` com Label: Saindo de Laços Aninhados

`break <label>` permite que você termine não apenas o laço atual, mas qualquer laço ancestral que tenha o label correspondente.

**Exemplo: Buscando em uma Matriz (2D Array)**

```js
const matriz = [
  [1, 5, 9],
  [4, 13, 2],
  [7, 8, 11]
];

const valorBuscado = 13;
let encontrado = false;

// Label para o laço externo
loopExterno:
for (let i = 0; i < matriz.length; i++) {
  for (let j = 0; j < matriz[i].length; j++) {
    if (matriz[i][j] === valorBuscado) {
      encontrado = true;
      console.log(`Valor encontrado na posição [${i}][${j}]`);
      break loopExterno; // Quebra AMBOS os laços
    }
  }
}

if (!encontrado) {
  console.log("Valor não encontrado.");
}
```

Sem o `break loopExterno`, o `break` normal apenas sairia do laço interno (`j`), e o laço externo (`i`) continuaria sua execução.

### `continue` com Label

De forma análoga, `continue <label>` pula para a próxima iteração do laço identificado pelo label. Este é um padrão de uso muito mais raro, mas possível.

## Considerações Finais

Neste capítulo, exploramos as estruturas fundamentais para a repetição em JavaScript. Vimos que a escolha do laço correto depende da natureza da tarefa: o **laço `for` clássico** nos dá controle total sobre o processo de iteração, o **`while`** é perfeito para quando a condição de parada é desconhecida, e o **`for...of`** se destaca como a maneira moderna, segura e legível de percorrer os valores de arrays e outros iteráveis.

Compreendemos a importante distinção entre `for...of` (para valores) e `for...in` (para chaves de objeto), e aprendemos a evitar as armadilhas de usar `for...in` com arrays. Dominamos as instruções `break` e `continue` para um controle de fluxo preciso e, com o uso de `labels`, descobrimos como gerenciar até mesmo as complexidades de laços aninhados.

A habilidade de automatizar tarefas repetitivas é a espinha dorsal da programação. Com o domínio das estruturas de laço apresentadas aqui, você está agora equipado para construir algoritmos e processar dados de qualquer escala, um passo essencial para o desenvolvimento de aplicações complexas e eficientes.
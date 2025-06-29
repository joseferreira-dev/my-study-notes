# Capítulo 18 – Funções (Parte 1): Os Blocos de Construção da Lógica

Até este ponto, nossa jornada nos equipou com as ferramentas para armazenar dados (variáveis), estruturá-los (objetos, arrays) e controlar o fluxo de execução (laços, condicionais). Agora, chegamos ao conceito que une tudo isso, o elemento mais fundamental para a criação de software modular, reutilizável e organizado: a **Função**. As funções são os verbos da nossa linguagem de programação; elas encapsulam ações e processos em blocos de código nomeados que podem ser executados sob demanda.

Em muitas linguagens, as funções são simplesmente sub-rotinas. No JavaScript, no entanto, elas ocupam um lugar muito mais central e poderoso. Aqui, funções são **cidadãs de primeira classe (first-class citizens)**. Isso significa que elas são tratadas como qualquer outro valor: podem ser atribuídas a variáveis, armazenadas em arrays, passadas como argumentos para outras funções e retornadas como o resultado de uma função. Essa característica abre as portas para padrões de programação incrivelmente expressivos e flexíveis, como a programação funcional.

Este capítulo é a primeira de duas partes de uma imersão profunda no universo das funções. Aqui, vamos construir a base. Começaremos com a anatomia de uma função, aprendendo a declarar e a invocar esses blocos de código. Dissecaremos o fluxo de dados através de parâmetros e da instrução `return`, e entenderemos a distinção crucial entre passar argumentos por valor e por referência. Exploraremos as diferentes formas de lidar com argumentos, desde o objeto `arguments` legado até os modernos parâmetros rest. Mergulharemos no conceito de escopo de função e no enigmático `this`, aprendendo a manipulá-lo com `call`, `apply` e `bind`. Por fim, introduziremos padrões avançados como IIFEs, recursão e composição de funções. Ao final desta primeira parte, você terá uma compreensão robusta dos mecanismos fundamentais que regem as funções em JavaScript, preparando o terreno para os tópicos ainda mais avançados que virão a seguir.

## A Anatomia de uma Função

Uma função é um bloco de código projetado para executar uma tarefa específica. Ela é definida uma vez e pode ser executada (ou "chamada") várias vezes.

### Declaração de Função (Named Functions)

A forma mais tradicional de se definir uma função é através da **declaração de função**. Ela consiste na palavra-chave `function`, seguida pelo nome da função, uma lista de parâmetros entre parênteses e o bloco de código a ser executado entre chaves.

```js
// Declaração de uma função nomeada
function saudar(nome) {
  const mensagem = `Olá, ${nome}!`;
  return mensagem;
}

// Chamando a função
const saudacaoParaAna = saudar("Ana");
console.log(saudacaoParaAna); // "Olá, Ana!"
```

Uma característica importante das declarações de função é que elas sofrem **hoisting (içamento)**. Isso significa que o motor do JavaScript move a declaração para o topo de seu escopo antes da execução do código, permitindo que você chame a função antes de sua linha de declaração.

### Expressão de Função (Functions as a Variable)

Como funções são cidadãs de primeira classe, elas podem ser atribuídas a variáveis. Isso é conhecido como uma **expressão de função**.

```js
const somar = function(a, b) {
  return a + b;
};

const resultado = somar(5, 3);
console.log(resultado); // 8
```

Neste caso, a função em si pode ser anônima (sem nome após a palavra-chave `function`), e a variável `somar` é usada para invocá-la. Diferente das declarações, as expressões de função **não** são totalmente içadas; apenas a declaração da variável (`let somar`) é, mas sua atribuição (`= function...`) não é. Portanto, você só pode chamar a função após sua linha de definição.

### Funções Anônimas

Uma função anônima é, como o nome sugere, uma função sem um nome. Elas são onipresentes em JavaScript, especialmente como argumentos para outras funções (callbacks).

```js
const numeros = [1, 2, 3];
// Passando uma função anônima para o método map
const quadrados = numeros.map(function(n) {
  return n * n;
});
```

### O `return` Statement: Retornando Valores

A instrução `return` especifica o valor que a função deve produzir ou "retornar" quando é chamada. Quando uma instrução `return` é executada, a função para imediatamente e o valor especificado é retornado para o local da chamada.

Se uma função não tiver uma instrução `return`, ou tiver um `return` sem um valor, ela retornará `undefined` por padrão.

```js
function verificarIdade(idade) {
  if (idade < 18) {
    return "Menor de idade"; // A função para aqui
  }
  // Se a condição for falsa, a função continua e retorna undefined implicitamente
}

console.log(verificarIdade(15)); // "Menor de idade"
console.log(verificarIdade(20)); // undefined
```

## Argumentos e Parâmetros

É crucial entender a terminologia:

- **Parâmetros:** São os nomes listados na definição da função (ex: `a` e `b` em `function somar(a, b)`). Eles atuam como variáveis locais dentro da função.
- **Argumentos:** São os valores reais que são passados para a função quando ela é invocada (ex: `5` e `3` em `somar(5, 3)`).

### Passando Argumentos: Por Valor vs. Por Referência

A forma como o JavaScript passa argumentos para funções depende do tipo de dado:

- **Tipos Primitivos (String, Number, Boolean, etc.):** São passados **por valor**. Isso significa que a função recebe uma **cópia** do valor. Qualquer modificação no parâmetro dentro da função **não afeta** a variável original fora dela.
    
    ```js
    function alterarPrimitivo(num) {
      num = 100;
      console.log(`Dentro da função: ${num}`); // 100
    }
    
    let meuNumero = 10;
    alterarPrimitivo(meuNumero);
    console.log(`Fora da função: ${meuNumero}`); // 10 (inalterado)
    ```
    
- **Tipos de Referência (Objects, Arrays):** São passados **por referência** (tecnicamente, a referência em si é passada por valor). A função recebe uma referência que aponta para o **mesmo objeto** na memória. Modificar as **propriedades** do objeto dentro da função **afetará** o objeto original.
    
    ```js
    function alterarObjeto(pessoa) {
      pessoa.idade = 30;
      console.log(`Dentro da função:`, pessoa);
    }
    
    let minhaPessoa = { nome: 'Ana', idade: 25 };
    alterarObjeto(minhaPessoa);
    console.log(`Fora da função:`, minhaPessoa); // { nome: 'Ana', idade: 30 } (alterado)
    ```
    
    No entanto, se você **reatribuir** o parâmetro a um novo objeto, isso quebrará o link com o objeto original, e as alterações subsequentes não o afetarão.

### Parâmetros Padrão (Default Parameters)

O ES6 introduziu uma sintaxe limpa para definir valores padrão para os parâmetros, caso um argumento não seja fornecido ou seja `undefined`.

```js
function criarUsuario(nome, nivelDeAcesso = 'leitor') {
  return { nome, nivelDeAcesso };
}

console.log(criarUsuario('Bia')); // { nome: 'Bia', nivelDeAcesso: 'leitor' }
console.log(criarUsuario('Carlos', 'admin')); // { nome: 'Carlos', nivelDeAcesso: 'admin' }
```

Isso substitui o padrão antigo de `nivelDeAcesso = nivelDeAcesso || 'leitor'`.

### Funções com Número de Argumentos Desconhecido (Variadic)

#### O Objeto `arguments` (Legado)

Dentro de qualquer função (exceto arrow functions), existe um objeto especial, semelhante a um array, chamado `arguments`. Ele contém todos os argumentos que foram passados para a função.

```js
function somarTudo() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}
console.log(somarTudo(1, 2, 3, 4)); // 10
```

Embora funcional, `arguments` não é um array de verdade (não tem métodos como `map` ou `forEach`) e seu uso é desencorajado em código moderno.

#### Parâmetros Rest `...` (Moderno)

A sintaxe de parâmetro rest (ES6) permite que representemos um número indefinido de argumentos como um **verdadeiro array**.

```js
function concatenarStrings(separador, ...strings) {
  // 'strings' é um array real
  return strings.join(separador);
}
console.log(concatenarStrings('-', 'a', 'b', 'c')); // "a-b-c"
```

O parâmetro rest deve ser sempre o último parâmetro na lista, e ele coleta todos os argumentos restantes.

## Escopo de Função

Como já vimos, o escopo determina a visibilidade e o tempo de vida de variáveis e funções. Em JavaScript, antes do ES6 (`let` e `const`), o **escopo de função** era a principal forma de criar escopos.

**Cada função cria seu próprio escopo.** Variáveis declaradas dentro de uma função (`var`, `let` ou `const`) não são visíveis fora dela.

```js
function calcular() {
  let resultadoParcial = 50; // Visível apenas dentro de calcular()
  console.log(resultadoParcial);
}

calcular(); // 50
// console.log(resultadoParcial); // ReferenceError: resultadoParcial is not defined
```

Isso é fundamental para o encapsulamento, impedindo que partes do código interfiram umas com as outras.

### IIFE: Expressões de Função Invocadas Imediatamente

Um padrão muito comum, especialmente antes dos módulos do ES6, era a **IIFE (Immediately Invoked Function Expression)**. É uma função anônima que é declarada e executada no mesmo instante.

**Sintaxe:** `(function() { /* ... */ })();`

O principal objetivo de uma IIFE era criar um escopo privado para evitar a poluição do escopo global.

```js
(function() {
  const nomeSecreto = "Agente 47";
  console.log(`Dentro da IIFE: ${nomeSecreto}`);
  // Nenhuma variável aqui vaza para o escopo global
})();

// console.log(nomeSecreto); // ReferenceError
```

## O Contexto de Execução: `this`, `call`, `apply` e `bind`

A palavra-chave `this` é uma das mais poderosas e, por vezes, confusas do JavaScript. Seu valor é determinado por **como a função é chamada** (seu contexto de invocação).

### `call()` e `apply()`: Invocação com Contexto Explícito

`call` e `apply` são métodos que existem em todas as funções. Ambos permitem que você chame uma função, especificando explicitamente qual objeto deve ser o `this` dentro dela.

- **`funcao.call(contexto, arg1, arg2, ...)`:** Passa os argumentos individualmente.
- **`funcao.apply(contexto, [arg1, arg2, ...])`:** Passa os argumentos como um array.

```js
const pessoa = {
  nome: 'Carla'
};

function saudacao(prefixo, sufixo) {
  console.log(`${prefixo}, ${this.nome}${sufixo}`);
}

// Usando call()
saudacao.call(pessoa, "Bom dia", "!"); // Bom dia, Carla!

// Usando apply()
saudacao.apply(pessoa, ["Boa noite", "!!"]); // Boa noite, Carla!!
```

`apply` é particularmente útil quando os argumentos já estão em um array.

### `bind()`: Criando Funções com Contexto Fixo

Diferente de `call` e `apply` que executam a função imediatamente, `bind()` **cria uma nova função** onde o valor de `this` é permanentemente ligado ao objeto fornecido.

```javascript
const carro = {
  marca: 'Ford',
  getMarca: function() {
    return this.marca;
  }
};

const obterMarcaDoCarro = carro.getMarca.bind(carro);
console.log(obterMarcaDoCarro()); // "Ford"
````

Isso é essencial em programação orientada a eventos e callbacks, onde o contexto de `this` pode se perder.

### Aplicação Parcial com `bind()`

`bind()` também pode ser usado para "pré-carregar" argumentos de uma função, uma técnica chamada **aplicação parcial**.

```js
function multiplicar(a, b) {
  return a * b;
}

// Cria uma nova função 'dobrar' onde 'a' está fixo em 2
const dobrar = multiplicar.bind(null, 2); // 'null' porque não nos importamos com o 'this'
console.log(dobrar(5)); // 10 (equivalente a multiplicar(2, 5))
```

## Padrões Avançados de Funções

### Função Recursiva

Uma função recursiva é uma função que chama a si mesma. Para evitar um laço infinito, toda função recursiva precisa de duas partes:

1. **Caso Base:** Uma condição que para a recursão.
2. **Passo Recursivo:** A parte onde a função chama a si mesma, geralmente com um argumento modificado que a aproxima do caso base.

**Exemplo: Fatorial**

```js
function fatorial(n) {
  // Caso Base: fatorial de 0 é 1
  if (n === 0) {
    return 1;
  }
  // Passo Recursivo: n * fatorial de (n-1)
  return n * fatorial(n - 1);
}

console.log(fatorial(5)); // 120 (5 * 4 * 3 * 2 * 1)
```

### Currying

Currying é a técnica de transformar uma função que aceita múltiplos argumentos em uma sequência de funções aninhadas, cada uma aceitando um único argumento.

```js
// Função normal
const adicionar = (a, b, c) => a + b + c;

// Versão "curried"
const adicionarCurried = a => b => c => a + b + c;

const resultadoCurried = adicionarCurried(1)(2)(3); // 6
console.log(resultadoCurried);

// A vantagem é que podemos criar funções especializadas
const adicionar5 = adicionarCurried(5); // Retorna uma função b => c => 5 + b + c
const adicionar5e10 = adicionar5(10); // Retorna uma função c => 5 + 10 + c
console.log(adicionar5e10(20)); // 35
```

### Composição de Funções

Composição é o ato de combinar duas ou mais funções para produzir uma nova função. A saída de uma função se torna a entrada da próxima.

```js
const maiuscula = str => str.toUpperCase();
const exclamar = str => `${str}!`;

// Composição manual
const gritar = str => exclamar(maiuscula(str));
console.log(gritar("pare")); // "PARE!"

// Usando uma função de composição genérica
const compor = (f, g) => x => f(g(x));
const gritarComposto = compor(exclamar, maiuscula);
console.log(gritarComposto("cuidado")); // "CUIDADO!"
```

## Metadados de Funções

Funções, sendo objetos, também têm propriedades. A mais comum é `.name`, que retorna o nome da função como uma string.

```js
function minhaFuncaoLegal() {}
console.log(minhaFuncaoLegal.name); // "minhaFuncaoLegal"

const expressao = function() {};
console.log(expressao.name); // "expressao"
```

## Considerações Finais

Nesta primeira parte sobre funções, estabelecemos uma base sólida e abrangente. Compreendemos que funções em JavaScript são "cidadãs de primeira classe", um conceito que desbloqueia padrões de programação poderosos. Dissecamos sua anatomia, o fluxo de dados através de parâmetros e o `return`, e as nuances da passagem por valor e por referência.

Exploramos as formas modernas de se lidar com um número variável de argumentos usando parâmetros rest, e reforçamos o conceito de escopo de função. Um dos tópicos mais cruciais foi o contexto de execução `this`, e aprendemos as ferramentas essenciais — `call`, `apply` e `bind` — para controlá-lo explicitamente. Finalmente, introduzimos padrões avançados como IIFEs, recursão, currying e composição, que demonstram a flexibilidade da linguagem.

Com estes fundamentos, estamos prontos para a segunda parte de nossa exploração. No próximo capítulo, mergulharemos nas Arrow Functions e suas implicações no `this`, desvendaremos um dos conceitos mais poderosos do JavaScript, os Closures, e iniciaremos nossa jornada no mundo da programação assíncrona com callbacks e Promises.
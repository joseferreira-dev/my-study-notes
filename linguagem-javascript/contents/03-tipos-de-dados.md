# Capítulo 3 – A Natureza da Informação: Tipos de Dados e Coerção

Nos capítulos anteriores, aprendemos a declarar variáveis (`let`, `const`) e a entender as regras de escopo que governam sua visibilidade. Estabelecemos que variáveis são contêineres nominais para dados. A questão fundamental que se segue é: que **tipos** de dados esses contêineres podem armazenar? A resposta a essa pergunta nos introduz ao **sistema de tipos** do JavaScript, um conjunto de classificações que define a natureza de cada valor e, consequentemente, as operações que podem ser realizadas sobre ele. Não se pode, por exemplo, realizar uma operação de divisão em um texto, ou capitalizar a primeira letra de um número. O tipo de um dado é a sua identidade e dita suas capacidades.

Neste capítulo, faremos uma análise exaustiva dos tipos de dados em JavaScript. Iniciaremos com a divisão conceitual mais importante: a diferença entre **Tipos Primitivos** e **Tipos de Referência (Objetos)**, um conceito cujo domínio é essencial para entender o comportamento da memória e evitar uma classe inteira de bugs comuns. Em seguida, dissecaremos cada um dos tipos de dados, dos mais simples como `string`, `number` e `boolean`, aos mais complexos e estruturados como `object`, `Array` e `Function`. Aprofundaremos nosso entendimento sobre conversão de tipos, explorando tanto a **conversão explícita** (type casting) quanto a **coerção implícita** (type coercion), um dos comportamentos mais notórios e poderosos do JavaScript. Por fim, aprenderemos a inspecionar e verificar programaticamente o tipo de um dado utilizando os operadores `typeof` e `instanceof`, ferramentas indispensáveis para a escrita de um código dinâmico e robusto.

## A Divisão Fundamental: Tipos Primitivos vs. Tipos de Referência

Em JavaScript, todos os tipos de dados se encaixam em uma de duas categorias conceituais: primitivos ou de referência. Essa distinção não é meramente acadêmica; ela determina fundamentalmente como os dados são armazenados na memória, como são copiados e como são passados para funções.

### Tipos Primitivos: Dados por Valor

Os tipos primitivos representam os dados mais básicos e fundamentais. São eles: `string`, `number`, `boolean`, `null`, `undefined`, `symbol` e `bigint`. A característica que os define é que eles são **imutáveis** e manipulados **por valor**.

**Imutabilidade** significa que o valor de um primitivo, uma vez criado, não pode ser alterado. Isso pode parecer contraintuitivo, pois sabemos que podemos fazer `let x = 5; x = 10;`. O que acontece aqui não é uma alteração do valor `5`, mas sim uma **reatribuição**: a variável `x`, que antes apontava para o valor `5` na memória, passa a apontar para um novo valor, `10`. O `5` original permanece inalterado.

A consequência mais importante disso é que primitivos são **passados e atribuídos por valor (pass-by-value)**. Quando você atribui uma variável primitiva a outra, uma cópia do valor é criada e armazenada no novo local de memória. As duas variáveis se tornam completamente independentes.

**Exemplo de manipulação por valor:**

```js
let pontuacaoA = 100;
let pontuacaoB = pontuacaoA; // O VALOR 100 é copiado para pontuacaoB

console.log(`Pontuação A: ${pontuacaoA}, Pontuação B: ${pontuacaoB}`);
// Saída: Pontuação A: 100, Pontuação B: 100

// Modificamos apenas pontuacaoB
pontuacaoB = 200;

console.log(`Pontuação A: ${pontuacaoA}, Pontuação B: ${pontuacaoB}`);
// Saída: Pontuação A: 100, Pontuação B: 200
// A alteração em pontuacaoB NÃO AFETOU pontuacaoA.
```

### Tipos de Referência (Objetos)

Qualquer dado que não seja um tipo primitivo é, por definição, um **objeto**. Isso inclui o objeto genérico (`{}`), `Array`, `Function`, `Date`, `RegExp`, `Error`, entre outros. Diferente dos primitivos, objetos são estruturas de dados complexas e **mutáveis**.

Objetos são manipulados **por referência (pass-by-reference)**. Quando você atribui uma variável de objeto a outra, você não está copiando o objeto em si. Em vez disso, você está copiando a **referência** — o endereço na memória onde o objeto original reside. O resultado é que ambas as variáveis apontam para o mesmo e único objeto na memória.

Qualquer modificação feita no objeto através de uma das variáveis será imediatamente visível através da outra, pois ambas estão alterando a mesma estrutura de dados.

**Exemplo de manipulação por referência:**

```js
const jogadorA = {
    nome: "Ana",
    pontos: 100
};

const jogadorB = jogadorA; // A REFERÊNCIA (endereço de memória) é copiada.

console.log(jogadorA, jogadorB);
// Saída: { nome: 'Ana', pontos: 100 }, { nome: 'Ana', pontos: 100 }

// Modificamos a propriedade 'pontos' através de jogadorB
jogadorB.pontos = 200;

console.log(jogadorA, jogadorB);
// Saída: { nome: 'Ana', pontos: 200 }, { nome: 'Ana', pontos: 200 }
// A alteração feita via jogadorB AFETOU jogadorA, pois ambos apontam para o mesmo objeto.
```

Compreender essa distinção é um dos saltos conceituais mais importantes para dominar o JavaScript e evitar bugs relacionados à mutação inesperada de dados.

## Análise Detalhada dos Tipos de Dados

Vamos agora explorar cada um dos principais tipos de dados, começando pelos primitivos e avançando para os tipos de referência.

### Os Tipos Primitivos

- **`string`**: O tipo string é utilizado para representar dados textuais. Em JavaScript, uma string é uma sequência imutável de caracteres Unicode, que pode ser delimitada por aspas simples ('...'), aspas duplas ("...") ou crases (`...`). As crases habilitam o uso de template literals, que permitem interpolação de variáveis e strings de múltiplas linhas.
    
    ```js
    const saudacao = 'Olá';
    const nome = "Mundo";
    const fraseCompleta = `${saudacao}, ${nome}! O ano é ${new Date().getFullYear()}.`;
    console.log(fraseCompleta);
    ```
    
- **`number`**: O JavaScript possui um único tipo numérico, number, que representa tanto números inteiros quanto de ponto flutuante. Ele segue o padrão internacional IEEE 754.
    
    ```js
    const idade = 30;
    const pi = 3.14159;
    ```
    
    O tipo `number` inclui alguns valores constantes built-in:
    
    - `Number.MAX_VALUE`: O maior número positivo representável.
    - `Number.MIN_VALUE`: O menor número positivo (mais próximo de zero) representável.
    - `Number.MAX_SAFE_INTEGER` e `Number.MIN_SAFE_INTEGER`: Os limites dentro dos quais os inteiros são considerados "seguros" (sem perda de precisão).
    - `Infinity` e `-Infinity`: Representam o infinito matemático.
    - `NaN` (Not-a-Number): Um valor numérico especial que representa o resultado de uma operação aritmética inválida (`0 / 0`, `parseInt("texto")`). Uma característica importante é que `NaN` nunca é igual a si mesmo (`NaN === NaN` é `false`).
        
- **`boolean`**: Um tipo de dado lógico que pode ter apenas dois valores: true ou false. É a base para todo o controle de fluxo em um programa.
    
    ```js
    const usuarioLogado = true;
    const acessoPermitido = false;
    ```
    
- **`undefined` e `null`**: Ambos representam a ausência de valor, mas com semânticas diferentes. A distinção é crucial para a engenharia de software.
    - **`undefined`**: Este é o valor padrão para uma variável que foi declarada, mas à qual ainda não foi atribuído um valor. Representa a ausência de valor _sistêmica_, definida pela própria linguagem. Uma função que não tem uma instrução `return` explícita, por padrão, retorna `undefined`.
        
        ```js
        let nome;
        console.log(nome); // Saída: undefined
        ```
        
    - **`null`**: Este é um valor que deve ser atribuído _intencionalmente_ por um programador. Ele representa a ausência explícita e deliberada de um valor, especialmente de um objeto. É uma forma de dizer "esta variável existe, mas aponta para o nada".
        
        ```js
        // Em um cenário real, poderíamos buscar um usuário no banco de dados.
        // Se o usuário não for encontrado, a função poderia retornar 'null'.
        let dadosDoUsuario = null;
        ```
    
    Apesar de conceitualmente distintos, `null == undefined` resulta em `true` devido à coerção de tipos, mas `null === undefined` resulta em `false` porque são de tipos diferentes.

### Os Tipos de Referência (Objetos)

- **`object`**: O objeto é a estrutura de dados mais fundamental em JavaScript. É uma coleção dinâmica de pares chave-valor (propriedades). Quase tudo em JavaScript que não é um primitivo é um objeto.
    
    ```js
    const produto = {
        id: 123,
        nome: "Laptop Gamer",
        disponivel: true
    };
    ```
    
- **`Array`**: Um Array é um tipo especializado de objeto, projetado para armazenar coleções ordenadas de dados.
    
    ```js
    const tarefas = ["Estudar JavaScript", "Fazer o projeto final", "Enviar relatório"];
    ```
    
- **`Function`**: Em JavaScript, funções são cidadãs de primeira classe, o que significa que são tratadas como objetos. Elas podem ser atribuídas a variáveis, passadas como argumentos e retornadas de outras funções.
    
    ```js
    const saudar = function(nome) { return `Olá, ${nome}!`; };
    ```
    
- **Outros Objetos Nativos (`Date`, `RegExp`, `Error`)**: São construtores nativos que criam objetos para finalidades específicas, como manipular datas, trabalhar com expressões regulares ou representar erros.

## Conversão e Coerção de Tipos

JavaScript é uma linguagem de **tipagem dinâmica** e **fraca**. Isso significa que o motor JavaScript frequentemente realiza conversões de tipo automáticas para que as operações façam sentido. Esse processo é chamado de **coerção de tipo (type coercion)** e é um dos comportamentos mais importantes e, por vezes, confusos da linguagem.

### Coerção Implícita

A coerção implícita ocorre quando o JavaScript converte tipos automaticamente, sem que o programador peça explicitamente.

- **Coerção para String:** Ocorre comumente com o operador `+` quando um dos operandos é uma string.
    
    ```js
    console.log("A resposta é " + 42);      // O número 42 é convertido para "42"
    // Saída: "A resposta é 42"
    
    console.log(null + " é nulo");         // null é convertido para "null"
    // Saída: "null é nulo"
    
    console.log({} + " é um objeto");      // {} é convertido para "[object Object]"
    // Saída: "[object Object] é um objeto"
    ```
    
- **Coerção para Número:** Ocorre com a maioria dos operadores matemáticos (`-`, `*`, `/`, `%`) e operadores de comparação.
    
    ```js
    console.log("100" - 10);        // "100" é convertido para 100
    // Saída: 90
    
    console.log("5" * 5);           // "5" é convertido para 5
    // Saída: 25
    
    console.log(true + 1);          // true é convertido para 1
    // Saída: 2
    ```
    
- **Coerção para Booleano:** Ocorre em contextos lógicos, como em uma instrução `if` ou com o operador `!`. Valores que se tornam `false` são chamados de "falsy", e todo o resto é "truthy".
    - **Valores Falsy:** `false`, `0`, `-0`, `""` (string vazia), `null`, `undefined`, `NaN`.
    - **Valores Truthy:** Qualquer outro valor, incluindo `"0"`, `"false"`, `[]` (array vazio), e `{}` (objeto vazio).
    
    ```js
    if ("") { console.log("Não serei executado."); }
    if ([]) { console.log("Serei executado, pois array vazio é truthy!"); }
    ```

### Conversão Explícita (Type Casting)

Ocorre quando usamos funções nativas para converter um valor de um tipo para outro de forma deliberada.

- **Para String:**

    ```js
    const numero = 123;
    const booleano = true;
    
    const numString = String(numero);    // "123"
    const boolString = booleano.toString(); // "true"
    ```
    
- **Para Número:**
    
    ```js
    const strNumero = "42.5";
    const strInvalida = "olá";
    const booleano = false;
    
    const num1 = Number(strNumero);    // 42.5
    const num2 = parseInt(strNumero);  // 42 (ignora a parte decimal)
    const num3 = parseFloat(strNumero);// 42.5
    const num4 = Number(strInvalida);  // NaN
    const num5 = Number(booleano);     // 0
    
    // Forma curta usando o operador unário de mais (+)
    const num6 = +strNumero;           // 42.5
    ```
    
- **Para Booleano:**
    
    ```js
    const valor = "Qualquer texto";
    const zero = 0;
    
    const bool1 = Boolean(valor); // true
    const bool2 = Boolean(zero);  // false
    
    // Forma curta usando o operador de dupla negação (!!)
    const bool3 = !!valor;        // true
    const bool4 = !!zero;         // false
    ```

## Verificação de Tipos: `typeof` e `instanceof`

Como o JavaScript é uma linguagem de tipagem dinâmica, frequentemente precisamos verificar qual o tipo de dado uma variável contém em um determinado momento. Para isso, temos dois operadores principais.

### O Operador `typeof`

O operador `typeof` retorna uma string que indica o tipo do operando. É a ferramenta mais rápida e direta para inspecionar tipos primitivos.

```js
console.log(typeof "Olá");           // "string"
console.log(typeof 100);             // "number"
console.log(typeof NaN);             // "number" (Sim, NaN é do tipo número)
console.log(typeof true);            // "boolean"
console.log(typeof undefined);       // "undefined"
console.log(typeof function() {});   // "function" (uma exceção conveniente)
```

No entanto, `typeof` possui limitações e peculiaridades notórias que o tornam inadequado para a verificação de objetos:

```js
console.log(typeof null);           // "object" (Este é um bug histórico da linguagem!)
console.log(typeof {});             // "object"
console.log(typeof []);             // "object" (Não distingue Array de Objeto)
console.log(typeof new Date());     // "object"
```

### O Operador `instanceof`

Para distinguir entre diferentes tipos de objetos, utilizamos o operador `instanceof`. Ele verifica se um objeto foi criado a partir de um determinado construtor (como `Array`, `Date`, etc.).

```js
const listaDeCompras = ["maçã", "banana"];
const dataDeHoje = new Date();

console.log(listaDeCompras instanceof Array);  // true
console.log(dataDeHoje instanceof Date);       // true
console.log(dataDeHoje instanceof Array);      // false
```

`instanceof` também reconhece a cadeia de protótipos, o que significa que um `Array` também é uma instância de `Object`.

```javascript
console.log(listaDeCompras instanceof Object); // true
````

Isso o torna uma ferramenta poderosa e precisa para verificar a linhagem de um objeto.

### Verificação Robusta: `Object.prototype.toString.call()`

Para uma verificação de tipo que funciona de forma consistente entre todos os tipos, incluindo a distinção correta para `null` e `Array`, a técnica mais robusta é usar o método `toString` do `Object.prototype`.

```js
function verificarTipo(valor) {
    return Object.prototype.toString.call(valor);
}

console.log(verificarTipo("Olá"));        // "[object String]"
console.log(verificarTipo(123));          // "[object Number]"
console.log(verificarTipo(null));         // "[object Null]"
console.log(verificarTipo(undefined));    // "[object Undefined]"
console.log(verificarTipo([]));           // "[object Array]"
console.log(verificarTipo({}));           // "[object Object]"
```

Embora mais verbosa, essa abordagem é a mais precisa e à prova de falhas para determinar o tipo interno de qualquer valor em JavaScript.

## Considerações Finais

Neste capítulo, mergulhamos no sistema de tipos do JavaScript, um pilar fundamental da linguagem. A distinção entre **tipos primitivos**, que são imutáveis e passados por valor, e **tipos de referência (objetos)**, que são mutáveis e passados por referência, é a chave para entender como os dados se comportam na memória e durante a execução do programa.

Analisamos cada um dos tipos de dados, dos blocos de construção básicos como `string`, `number` e `boolean`, até as estruturas complexas e versáteis como `object`, `Array` e `Function`. Exploramos o poderoso e onipresente mecanismo de **coerção de tipos**, diferenciando suas manifestações implícitas das conversões explícitas.

Por fim, aprendemos as ferramentas para inspecionar esses tipos em nosso código. Vimos que `typeof` é ideal para identificar primitivos (com exceção de `null`), `instanceof` nos permite diferenciar entre os vários tipos de objetos, e `Object.prototype.toString.call()` oferece a verificação mais robusta e universal. O domínio desses conceitos nos capacita a escrever um código mais seguro e previsível, onde podemos tomar decisões lógicas com base na natureza exata da informação que estamos manipulando.
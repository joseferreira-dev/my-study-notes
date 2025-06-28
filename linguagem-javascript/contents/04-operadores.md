# Capítulo 4 – Expressões e Operadores

Após termos dominado os conceitos de variáveis e tipos de dados, nosso próximo passo é aprender a operar sobre esses dados. Uma variável armazenando um número ou uma string é, por si só, uma informação estática. O verdadeiro poder da programação emerge quando começamos a combinar, comparar e transformar esses valores. As ferramentas que nos permitem realizar essas ações são os **operadores**.

Neste capítulo, faremos um mergulho profundo no arsenal de operadores que o JavaScript oferece. Eles são os símbolos e palavras-chave que formam o coração das expressões — as unidades de código que resultam em um valor. Começaremos com os operadores mais familiares: os de **atribuição**, que nos permitem armazenar dados, e os **aritméticos**, que são a base de todo cálculo matemático. Em seguida, exploraremos os **operadores de comparação** e **lógicos**, que são fundamentais para a tomada de decisões e para a criação de fluxos de controle condicionais. Investigaremos a importante distinção entre igualdade estrita (`===`) e abstrata (`==`), um conceito-chave em JavaScript. Também abordaremos operadores mais avançados, como os **unários** e os que manipulam dados em seu nível mais fundamental, os **operadores bitwise**. Por fim, analisaremos a precedência dos operadores para garantir que nossas expressões complexas sejam avaliadas na ordem correta. O domínio dos operadores é o que nos permite transitar da simples declaração de dados para a implementação de algoritmos e lógicas de negócio complexas.

## Operadores de Atribuição (Assignment Operators)

Os operadores de atribuição são usados para designar valores a variáveis. O operador mais fundamental é o de atribuição simples (`=`), que atribui o valor do operando à direita para o operando à esquerda.

```js
// Atribuição simples
let nome = "João";
let idade = 30;
let saldoEmConta = 1500.50;
```

Além da atribuição simples, JavaScript fornece operadores de atribuição compostos, que combinam uma operação aritmética com a atribuição, oferecendo uma sintaxe mais concisa.

|Operador|Exemplo|Equivalente a|Descrição|
|---|---|---|---|
|`+=`|`x += y`|`x = x + y`|Adição e atribuição.|
|`-=`|`x -= y`|`x = x - y`|Subtração e atribuição.|
|`*=`|`x *= y`|`x = x * y`|Multiplicação e atribuição.|
|`/=`|`x /= y`|`x = x / y`|Divisão e atribuição.|
|`%=`|`x %= y`|`x = x % y`|Módulo (resto da divisão) e atribuição.|
|`**=`|`x **= y`|`x = x ** y`|Exponenciação e atribuição (ES2016).|

**Exemplo de uso de operadores de atribuição compostos:**

```js
let quantidade = 10;
quantidade += 5; // quantidade é agora 15

let total = 100;
total -= 20; // total é agora 80

let preco = 50;
preco *= 2; // preco é agora 100

let fatiasPizza = 8;
fatiasPizza /= 4; // fatiasPizza é agora 2

let base = 2;
base **= 3; // base é agora 8 (2 elevado a 3)
```

## Operadores de Comparação (Comparison Operators)

Os operadores de comparação avaliam dois operandos e retornam um valor booleano (`true` ou `false`). Eles são a espinha dorsal de qualquer lógica condicional (`if`, `while`, etc.).

### Igualdade Estrita (`===`) vs. Igualdade Abstrata (`==`)

Esta é uma das distinções mais críticas em JavaScript.

- **Igualdade Estrita (`===`)**: Retorna `true` se os operandos forem iguais **e** do mesmo tipo. **Este é o operador de igualdade que você deve usar em 99% dos casos.** Ele é previsível, seguro e não realiza conversões de tipo inesperadas.
- **Igualdade Abstrata ou "Frouxa" (`==`)**: Retorna `true` se os operandos forem iguais **após** tentar convertê-los para um tipo comum. Este processo de conversão automática (coerção) pode levar a resultados confusos e é uma fonte comum de bugs.

**Exemplo comparativo:**

```js
console.log(5 === "5");  // false (número vs. string)
console.log(5 == "5");   // true (a string "5" é coagida para o número 5 antes da comparação)

console.log(0 === false); // false (número vs. booleano)
console.log(0 == false);  // true (o booleano false é coagido para o número 0)

console.log(null === undefined); // false (tipos diferentes)
console.log(null == undefined);  // true (uma exceção da regra onde são considerados iguais)

console.log([] == false); // true (um array vazio é coagido para 0, que é igual a false coagido)
```

**Regra de ouro:** Sempre prefira a igualdade estrita (`===`) para evitar os efeitos colaterais da coerção de tipos.

### Desigualdade Estrita (`!==`) vs. Desigualdade Abstrata (`!=`)

Seguindo a mesma lógica da igualdade, temos os operadores de desigualdade.

- **Desigualdade Estrita (`!==`)**: Retorna `true` se os operandos **não** forem iguais ou se forem de tipos diferentes. É o inverso direto de `===`.
- **Desigualdade Abstrata (`!=`)**: Retorna `true` se os operandos não forem iguais após a coerção de tipo. É o inverso de `==`.

**Exemplo comparativo:**

```js
console.log(5 !== "5");  // true
console.log(5 != "5");   // false

console.log(0 !== false); // true
console.log(0 != false);  // false
```

Novamente, **sempre prefira a desigualdade estrita (`!==`)**.

### Operadores Relacionais

Estes operadores são usados para comparar a magnitude entre dois valores.

- `>` (Maior que)
- `<` (Menor que)
- `>=` (Maior ou igual a)
- `<=` (Menor ou igual a)

Quando usados com números, seu comportamento é intuitivo.

```js
console.log(10 > 5);   // true
console.log(10 <= 10); // true
```

Quando usados com strings, a comparação é feita **lexicograficamente** (ordem de dicionário), com base nos valores Unicode dos caracteres.

```js
console.log("banana" > "abacaxi"); // true ('b' vem depois de 'a')
console.log("20" > "100");         // true (lexicograficamente, "2" é maior que "1")
console.log("A" > "a");             // false (letras maiúsculas têm valores Unicode menores)
```

Quando um operando é um número e o outro uma string, o JavaScript tenta converter a string para um número antes de comparar.

```js
console.log(20 > "100"); // false (a string "100" é convertida para o número 100)
```

## Operadores Aritméticos (Arithmetic Operators)

Esses operadores realizam operações matemáticas em valores numéricos.

- **Adição (`+`)**: Soma dois números. Possui um comportamento duplo: se um dos operandos for uma string, ele realiza a **concatenação de strings**.
    
    ```js
    console.log(10 + 5);      // 15 (Adição)
    console.log("Olá " + "Mundo"); // "Olá Mundo" (Concatenação)
    console.log("5" + 5);     // "55" (Concatenação, pois um operando é string)
    ```
    
- **Subtração (`-`)**: Subtrai o segundo operando do primeiro.
    
    ```js
    console.log(10 - 5);      // 5
    console.log("10" - 5);    // 5 (Coerção de "10" para número)
    ```
    
- **Multiplicação (`*`)**: Multiplica os dois operandos.
    
    ```js
    console.log(10 * 5);      // 50
    ```
    
- **Divisão (`/`)**: Divide o primeiro operando pelo segundo.
    
    ```js
    console.log(10 / 2);      // 5
    ```
    
- **Módulo (Resto da Divisão) (`%`)**: Retorna o resto da divisão inteira do primeiro operando pelo segundo.
    
    ```js
    console.log(10 % 3);      // 1 (10 dividido por 3 é 3, com resto 1)
    console.log(11 % 2);      // 1 (útil para verificar se um número é ímpar)
    ```
    
- **Exponenciação (`**`)**: (ES2016) Eleva o primeiro operando à potência do segundo.
    
    ```js
    console.log(2 ** 3);      // 8 (2 elevado a 3)
    console.log(10 ** -1);    // 0.1
    ```

### Incremento (`++`) e Decremento (`--`)

Estes operadores unários adicionam ou subtraem 1 de seu operando. Sua posição (antes ou depois do operando) afeta o valor retornado pela expressão.

- **Pós-incremento (`x++`)**: Retorna o valor de `x` **antes** do incremento.
    
    ```js
    let x = 5;
    let y = x++;
    // y recebe 5, e depois x se torna 6
    console.log(`x: ${x}, y: ${y}`); // x: 6, y: 5
    ```
    
- **Pré-incremento (`++x`)**: Retorna o valor de `x` **após** o incremento.
    
    ```js
    let a = 5;
    let b = ++a;
    // a se torna 6, e depois b recebe 6
    console.log(`a: ${a}, b: ${b}`); // a: 6, b: 6
    ```

O mesmo comportamento de pré e pós-operação se aplica ao decremento (`x--` e `--x`).

## Operadores Lógicos (Logical Operators)

Operadores lógicos são tipicamente usados com valores booleanos e também retornam um valor booleano. No entanto, em JavaScript, eles têm um comportamento poderoso com valores não-booleanos.

- **E Lógico (`&&` - AND)**: Retorna `true` somente se **ambos** os operandos forem `true`.
- **OU Lógico (`||` - OR)**: Retorna `true` se **pelo menos um** dos operandos for `true`.
- **Negação Lógica (`!` - NOT)**: Inverte o valor booleano de seu operando.

Algo interessante sobre os operadores lógicos é o comportamento de Curto-Circuito (Short-Circuiting). O curto-circuito se refere ao fato desses operadores não avaliarem todas as expressões envolvidas na declaração caso a primeira delas seja **falsy** or **truthy**. Tanto `&&` quanto `||` usam avaliação de curto-circuito.

- `expressao1 && expressao2`: Se `expressao1` for **falsy**, a `expressao2` **não é avaliada**, e o valor de `expressao1` é retornado. Se `expressao1` for **truthy**, o valor de `expressao2` é retornado.
- `expressao1 || expressao2`: Se `expressao1` for **truthy**, a `expressao2` **não é avaliada**, e o valor de `expressao1` é retornado. Se `expressao1` for **falsy**, o valor de `expressao2` é retornado.

Este comportamento permite usos idiomáticos em JavaScript:

**Exemplo de `||` para valores padrão (antes do ES2020):**

```js
function saudar(nome) {
    const nomeFinal = nome || "Visitante";
    console.log(`Olá, ${nomeFinal}!`);
}
saudar("Maria"); // Olá, Maria!
saudar();        // Olá, Visitante!
```

**Cuidado:** O problema com `||` é que ele trata qualquer valor **falsy** (como `0`, `""`, `false`) como motivo para usar o padrão, o que nem sempre é desejado.

### Operador de Coalescência Nula (`??`)

Introduzido no ES2020, o operador de coalescência nula (`??`) é uma alternativa mais segura ao `||` para definir valores padrão. Ele só retorna o operando da direita se o operando da esquerda for **`null` ou `undefined`**. Outros valores **falsy** (como `0` ou `""`) são considerados válidos.

```js
let quantidade = 0;
let quantidadeFinal = quantidade || 10; // quantidadeFinal seria 10 (indesejado)
let quantidadeCorreta = quantidade ?? 10; // quantidadeCorreta é 0 (correto)

console.log(quantidadeFinal);   // 10
console.log(quantidadeCorreta); // 0
```

## Operadores Unários

Operadores unários atuam sobre um único operando.

- `+` (Mais Unário): Tenta converter o operando em um número. É uma forma rápida de fazer casting de tipo.
    
    ```js
    console.log(+"42");     // 42 (número)
    console.log(+"");       // 0
    console.log(+true);     // 1
    ```
    
- `-` (Menos Unário): Converte o operando para número e o nega.
    
    ```js
    console.log(-"42");    // -42
    ```
    
- `typeof`: Retorna uma string indicando o tipo do operando.
- `delete`: Remove uma propriedade de um objeto. Não tem efeito sobre variáveis ou funções.
    
    ```js
    const usuario = { nome: "Carlos", idade: 40 };
    delete usuario.idade;
    console.log(usuario); // { nome: "Carlos" }
    ```

## Operador Condicional (Ternário)

O operador condicional (`? :`) é o único operador ternário (que aceita três operandos) em JavaScript. Ele é uma forma concisa de escrever uma instrução condicional.

**Sintaxe:** `condicao ? expressaoSeVerdadeiro : expressaoSeFalso`

```js
const idade = 20;
const status = (idade >= 18) ? "Adulto" : "Menor de idade";
console.log(status); // "Adulto"

// É muito útil para atribuições condicionais.
const corTema = "dark";
const corTexto = (corTema === "dark") ? "white" : "black";
console.log(corTexto); // "white"
```

## Operadores Bitwise (Bit a Bit)

Operadores bitwise tratam seus operandos como uma sequência de 32 bits e realizam operações em sua representação binária. Eles são menos comuns em desenvolvimento web do dia a dia, mas são importantes em cenários de baixo nível, como manipulação de gráficos, criptografia ou algoritmos de alta performance.

|Operador|Nome|Descrição|
|---|---|---|
|`&`|AND Bitwise|Retorna 1 em cada posição de bit onde ambos os operandos têm 1.|
|`|`|OR Bitwise|
|`^`|XOR Bitwise|Retorna 1 em cada posição de bit onde os operandos são diferentes.|
|`~`|NOT Bitwise|Inverte todos os bits do operando.|
|`<<`|Left Shift|Desloca os bits para a esquerda, preenchendo com zeros à direita.|
|`>>`|Sign-propagating Right Shift|Desloca os bits para a direita, mantendo o bit de sinal.|

```js
// Exemplo: 5 (0101) & 3 (0011)
console.log(5 & 3); // 1 (binário 0001)

// Exemplo: 5 (0101) | 3 (0011)
console.log(5 | 3); // 7 (binário 0111)
```

## Precedência de Operadores

A **precedência de operadores** determina a ordem em que as operações são executadas em uma expressão complexa. Por exemplo, a multiplicação tem uma precedência maior que a adição.

```js
const resultado = 5 + 3 * 2; // 11 (3 * 2 é executado primeiro)
```

Embora exista uma tabela de precedência bem definida (que pode ser consultada na documentação da linguagem), a melhor prática para garantir a clareza e a correção do código é usar **parênteses `()`** para agrupar as operações e controlar explicitamente a ordem de avaliação.

```js
// Explícito e inequívoco
const resultadoClaro = 5 + (3 * 2); // 11
const resultadoDiferente = (5 + 3) * 2; // 16
```

O uso de parênteses não apenas evita bugs decorrentes de uma má interpretação da precedência, mas também torna o código instantaneamente mais legível para outros desenvolvedores.

## Considerações Finais

Neste capítulo, exploramos o vasto e poderoso conjunto de operadores do JavaScript. Vimos como eles nos permitem ir além do simples armazenamento de dados, capacitando-nos a realizar cálculos aritméticos, executar comparações lógicas, manipular valores em nível de bit e atribuir resultados a variáveis.

Compreendemos as nuances críticas entre igualdade estrita e abstrata, a utilidade do curto-circuito em operadores lógicos e a concisão oferecida pelo operador ternário. Cobrimos desde os operadores mais comuns, como os aritméticos e de atribuição, até os mais específicos, como os bitwise. Finalmente, ressaltamos a importância de usar parênteses para controlar a precedência e garantir a legibilidade.

Armados com o conhecimento sobre variáveis, tipos e operadores, temos agora os três pilares essenciais da programação. Estamos prontos para combiná-los em estruturas mais complexas, como os laços de repetição e as condicionais, que nos permitirão controlar o fluxo de execução de nossos programas, o que será o tema do nosso próximo capítulo.
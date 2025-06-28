# Capítulo 5 – Lidando com Números: Number e Math

Nos capítulos anteriores, estabelecemos os alicerces da linguagem, compreendendo variáveis, escopo, tipos de dados e os operadores que nos permitem manipulá-los. Agora, é o momento de aprofundar nosso conhecimento em um dos tipos primitivos mais fundamentais e onipresentes na programação: o número. A manipulação de dados numéricos é a base para uma infinidade de tarefas, desde simples cálculos em um carrinho de compras até algoritmos complexos de física em jogos ou análises financeiras.

Para nos auxiliar nessa tarefa, o JavaScript oferece dois objetos nativos (built-in) essenciais: `Number` e `Math`. Embora ambos lidem com números, seus propósitos são distintos e complementares. O objeto `Number` atua como um invólucro (wrapper) para o tipo primitivo `number`, fornecendo propriedades (constantes) que definem os limites do sistema numérico do JavaScript e métodos para conversão, formatação e verificação de valores. Por outro lado, o objeto `Math` funciona como uma biblioteca estática, uma caixa de ferramentas repleta de constantes matemáticas universais (como Pi) e funções para executar operações mais complexas, como cálculos trigonométricos, exponenciação e arredondamento.

Neste capítulo, faremos uma exploração detalhada desses dois objetos. Desvendaremos as constantes e os métodos de `Number` que nos ajudam a trabalhar com precisão e segurança, evitando armadilhas comuns da aritmética de ponto flutuante. Em seguida, navegaremos pela rica coleção de funcionalidades oferecidas por `Math`, capacitando-nos a resolver uma vasta gama de problemas matemáticos de forma eficiente e declarativa.

## O Objeto `Number`

Em JavaScript, `Number` é um objeto wrapper primitivo usado para representar e trabalhar com números. Embora você raramente precise instanciar um objeto `Number` diretamente (usando `new Number()`), suas propriedades estáticas (constantes) e métodos são de uso diário e indispensáveis para a escrita de um código numérico robusto.

### Constantes Estáticas de `Number`

Estas constantes definem as características e os limites do tipo `number` em JavaScript, que segue o padrão de ponto flutuante de precisão dupla IEEE 754.

- `Number.MAX_VALUE` e `Number.MIN_VALUE`: `Number.MAX_VALUE` representa o maior número positivo que pode ser representado em JavaScript (aproximadamente `1.79e+308`). Qualquer valor acima disso é considerado `Infinity`. `Number.MIN_VALUE`, por sua vez, representa o menor número positivo (mais próximo de zero) que pode ser representado (aproximadamente `5e-324`). Não é o menor número negativo.
    
    ```js
    console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
    console.log(Number.MAX_VALUE * 2); // Infinity
    
    console.log(Number.MIN_VALUE); // 5e-324
    ```
    
- `Number.MAX_SAFE_INTEGER` e `Number.MIN_SAFE_INTEGER`: Devido à forma como os números de ponto flutuante são armazenados, nem todos os inteiros podem ser representados com precisão exata. Essas constantes definem o intervalo de inteiros que podem ser representados de forma "segura", sem perda de precisão. Fora desse intervalo, operações com inteiros podem produzir resultados inesperados.
    
    ```js
    const maxSafe = Number.MAX_SAFE_INTEGER;
    console.log(maxSafe);     // 9007199254740991
    
    console.log(maxSafe + 1); // 9007199254740992 (Correto)
    console.log(maxSafe + 2); // 9007199254740992 (Incorreto! Perda de precisão)
    ```
    
- `Number.EPSILON`: Esta constante representa a diferença entre o número 1 e o menor valor de ponto flutuante maior que 1 que pode ser representado em JavaScript. Seu principal uso é em comparações de números de ponto flutuante. Devido a imprecisões de arredondamento inerentes à representação binária, a comparação direta (`0.1 + 0.2 === 0.3`) pode falhar. `Number.EPSILON` nos permite verificar se a diferença entre dois números é pequena o suficiente para serem considerados iguais.
    
    ```js
    console.log(0.1 + 0.2 === 0.3); // false (o resultado é ~0.30000000000000004)
    
    function saoQuaseIguais(a, b) {
        return Math.abs(a - b) < Number.EPSILON;
    }
    
    console.log(saoQuaseIguais(0.1 + 0.2, 0.3)); // true
    ```
    
- `Number.POSITIVE_INFINITY`, `Number.NEGATIVE_INFINITY` e `Number.NaN`: Essas constantes fornecem referências diretas para os valores especiais `Infinity`, `-Infinity` e `NaN`.

### Métodos Estáticos de `Number`

Esses métodos são chamados diretamente no objeto `Number`.

- `Number.isNaN(valor)`: Esta é a forma mais robusta e recomendada para verificar se um valor é `NaN`. Diferente da função global `isNaN()`, o `Number.isNaN()` não tenta coagir (converter) o argumento para um número. Ele retorna `true` se, e somente se, o valor já for do tipo `number` e for `NaN`.
    
    ```js
    // Função global (não recomendada)
    console.log(isNaN("olá")); // true (tenta converter "olá" para número, falha e resulta em NaN)
    
    // Método estático (recomendado)
    console.log(Number.isNaN("olá")); // false (não é do tipo number)
    console.log(Number.isNaN(NaN));   // true
    ```
    
- `Number.isFinite(valor)`: Verifica se o valor fornecido é um número finito, ou seja, se é do tipo `number` e não é `NaN`, `Infinity` ou `-Infinity`.
    
    ```js
    console.log(Number.isFinite(100));       // true
    console.log(Number.isFinite("100"));     // false (é uma string)
    console.log(Number.isFinite(Infinity));  // false
    console.log(Number.isFinite(NaN));       // false
    ```
    
- `Number.isInteger(valor)`: Retorna `true` se o valor for um número inteiro.
    
    ```js
    console.log(Number.isInteger(123));     // true
    console.log(Number.isInteger(123.0));   // true
    console.log(Number.isInteger(123.45));  // false
    console.log(Number.isInteger("123"));   // false
    ```
    
- `Number.parseInt(string, radix)` e `Number.parseFloat(string)`: Esses métodos, adicionados no ES6, comportam-se de forma idêntica às suas contrapartes globais (`parseInt` e `parseFloat`). Eles são usados para converter uma representação de `string` em um número.

	O método `parseInt` analisa uma `string` e retorna um inteiro. O segundo argumento, `radix`, especifica a base do número (de 2 a 36). É uma prática fundamental sempre fornecer o `radix` (geralmente 10 para o sistema decimal) para evitar resultados inesperados.
    
    ```js
    console.log(Number.parseInt("100px", 10)); // 100 (ignora caracteres não numéricos após o número)
    console.log(Number.parseInt("FF", 16));    // 255 (base 16, hexadecimal)
    console.log(Number.parseInt("101", 2));    // 5   (base 2, binário)
    ```
    
    Já `parseFloat` analisa uma `string` e retorna um número de ponto flutuante.
    
    ```javascript
    console.log(Number.parseFloat("3.14159")); // 3.14159
    console.log(Number.parseFloat("3.14 é pi")); // 3.14
    ````

### Métodos de Instância de `Number`

Esses métodos são chamados em uma variável do tipo `number` para formatar sua representação. Todos retornam uma `string`.

- `num.toFixed(digitos)`: Formata um número usando notação de ponto fixo, arredondando para o número de casas decimais especificado por digitos.
    
    ```js
    const preco = 19.998;
    console.log(preco.toFixed(2)); // "20.00" (arredonda para cima)
    console.log(preco.toFixed(1)); // "20.0"
    console.log((5).toFixed(2));   // "5.00"
    ```
    
- `num.toPrecision(digitos)`: Formata um número para um número especificado de dígitos significativos. Pode retornar em notação de ponto fixo ou exponencial, dependendo do número.
    
    ```js
    const numero = 1234.567;
    console.log(numero.toPrecision(6)); // "1234.57"
    console.log(numero.toPrecision(2)); // "1.2e+3" (notação exponencial)
    ```
    
- `num.toString(radix)`: Converte um número em sua representação de `string` em uma base especificada (`radix`). Se o `radix` não for fornecido, a base 10 é usada como padrão.
    
    ```js
    const valor = 255;
    console.log(valor.toString());    // "255" (base 10)
    console.log(valor.toString(16));  // "ff" (base 16 - hexadecimal)
    console.log(valor.toString(8));   // "377" (base 8 - octal)
    console.log(valor.toString(2));   // "11111111" (base 2 - binário)
    ```

## O Objeto `Math`

Diferente de `Number`, `Math` não é um construtor. Ele é um objeto estático nativo que serve como um contêiner para propriedades e métodos matemáticos. Você não pode criar uma instância de `Math`. Todas as suas funcionalidades são acessadas diretamente (ex: `Math.PI`, `Math.sqrt()`).

### Constantes de `Math`

`Math` fornece várias constantes matemáticas comuns.

|Constante|Descrição|Valor Aproximado|
|---|---|---|
|`Math.PI`|A razão entre a circunferência e o diâmetro de um círculo (π).|3.14159|
|`Math.E`|A base dos logaritmos naturais (Número de Euler, e).|2.718|
|`Math.SQRT2`|A raiz quadrada de 2.|1.414|
|`Math.SQRT1_2`|A raiz quadrada de 1/2.|0.707|
|`Math.LN2`|O logaritmo natural de 2.|0.693|
|`Math.LN10`|O logaritmo natural de 10.|2.302|

**Exemplo:**

```js
// Calcular a área de um círculo com raio 5
const raio = 5;
const area = Math.PI * (raio ** 2);
console.log(area); // 78.53981633974483
```

### Métodos de `Math`

`Math` oferece uma vasta biblioteca de funções matemáticas.

#### Arredondamento e Truncamento

- **`Math.round(x)`**: Arredonda `x` para o inteiro mais próximo. Casos `.5` são arredondados para cima.
- **`Math.floor(x)`**: Arredonda `x` para baixo, para o maior inteiro menor ou igual a `x`.
- **`Math.ceil(x)`**: Arredonda `x` para cima, para o menor inteiro maior ou igual a `x`.
- **`Math.trunc(x)`**: (ES6) Remove a parte fracionária de `x`, retornando apenas a parte inteira.

**Exemplo comparativo:**

```js
console.log(Math.round(3.7)); // 4
console.log(Math.round(3.2)); // 3
console.log(Math.round(3.5)); // 4

console.log(Math.floor(3.7)); // 3
console.log(Math.floor(-3.7)); // -4

console.log(Math.ceil(3.2));  // 4
console.log(Math.ceil(-3.2)); // -3

console.log(Math.trunc(3.7));  // 3
console.log(Math.trunc(-3.7)); // -3 (diferente de floor)
```

#### Máximos e Mínimos

- **`Math.max(...valores)`**: Retorna o maior valor de uma lista de números.
- **`Math.min(...valores)`**: Retorna o menor valor de uma lista de números.

Para usar com arrays, é comum utilizar o operador spread (`...`).

```js
console.log(Math.max(10, 5, 100, -20)); // 100

const numeros = [10, 5, 100, -20];
console.log(Math.max(...numeros));      // 100
console.log(Math.min(...numeros));      // -20
```

#### Potências e Raízes

- **`Math.pow(base, expoente)`**: Retorna a `base` elevada ao `expoente`. Equivalente ao operador `**`.
- **`Math.sqrt(x)`**: Retorna a raiz quadrada de `x`.
- **`Math.cbrt(x)`**: (ES6) Retorna a raiz cúbica de `x`.
- **`Math.hypot(...valores)`**: (ES6) Retorna a raiz quadrada da soma dos quadrados de seus argumentos. É extremamente útil para calcular a distância euclidiana (hipotenusa).

```js
console.log(Math.pow(2, 10)); // 1024
console.log(Math.sqrt(144));  // 12
console.log(Math.cbrt(27));   // 3

// Distância do ponto (3, 4) até a origem (0, 0)
console.log(Math.hypot(3, 4)); // 5
```

#### Geração de Números Aleatórios

- **`Math.random()`**: Retorna um número de ponto flutuante pseudoaleatório no intervalo de `[0, 1)` (incluindo 0, mas não incluindo 1). É frequentemente combinado com outros métodos para gerar números em diferentes intervalos.

**Exemplo: Gerar um inteiro aleatório entre `min` e `max` (inclusivos):**

```js
function gerarInteiroAleatorio(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// Lançar um dado de 6 faces
console.log(gerarInteiroAleatorio(1, 6)); 
```

#### Funções Trigonométricas

`Math` inclui todas as funções trigonométricas padrão, como `Math.sin()`, `Math.cos()`, `Math.tan()`, e suas inversas (`Math.asin()`, `Math.acos()`, `Math.atan()`). É importante lembrar que essas funções operam com **radianos**, não com graus.

Além dessas, existe a função **`Math.atan2(y, x)`**, especialmente útil pois retorna o ângulo em radianos entre o eixo x positivo e o ponto `(x, y)`, lidando corretamente com todos os quadrantes.

## Considerações Finais

Neste capítulo, desvendamos o universo da manipulação de números em JavaScript através dos objetos `Number` e `Math`. Vimos que `Number` nos fornece as ferramentas para entender os limites e a natureza do sistema numérico da linguagem, além de oferecer métodos essenciais para conversão e formatação. Em contraste, `Math` atua como uma robusta biblioteca de funções estáticas, pronta para resolver desde operações aritméticas básicas, como arredondamento e exponenciação, até cálculos trigonométricos e logarítmicos mais complexos.

O domínio dessas APIs é crucial. Saber quando usar `Number.isNaN()` em vez da função global, como formatar um valor monetário com `toFixed()`, ou como gerar um número aleatório dentro de um intervalo específico com `Math.random()` e `Math.floor()` são habilidades práticas e diárias de um desenvolvedor JavaScript. Com este conhecimento, estamos agora ainda mais preparados para construir lógicas complexas, passando de simples atribuições e comparações para expressões e algoritmos matematicamente sofisticados.
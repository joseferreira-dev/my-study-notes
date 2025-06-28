# Capítulo 6 – Strings

Após termos explorado os tipos de dados numéricos e os operadores, voltamos nossa atenção para outro tipo primitivo de importância fundamental: a **string**. Em praticamente toda aplicação de software, a manipulação de texto é uma tarefa central. Seja para exibir uma mensagem de boas-vindas a um usuário, processar a entrada de um formulário, gerar conteúdo dinâmico para uma página web ou construir consultas para um banco de dados, as strings são os veículos que transportam e representam informações textuais.

O JavaScript oferece um suporte robusto e uma API rica para a manipulação de strings. Neste capítulo, faremos um mergulho profundo nesse universo. Começaremos por entender as características fundamentais das strings em JavaScript, incluindo sua natureza imutável e a forma como são representadas internamente. Abordaremos as diversas maneiras de criar e concatenar strings, com um foco especial nos modernos e poderosos **Template Literals**, que simplificam drasticamente a interpolação de variáveis e a criação de texto em múltiplas linhas. Também desvendaremos o uso de **Sequências de Escape** para representar caracteres especiais. A maior parte do capítulo será dedicada a uma exploração detalhada dos inúmeros métodos nativos para buscar, extrair, substituir e transformar strings, fornecendo a você a caixa de ferramentas completa para dissecar e construir qualquer dado textual de que sua aplicação precise.

## Características Fundamentais de Strings

Em JavaScript, uma string é um tipo de dado primitivo que representa uma sequência de caracteres. É crucial entender duas de suas características principais: a forma como são criadas e sua imutabilidade.

### Criação de Strings

Existem três maneiras de se criar uma string literal em JavaScript:

1. **Aspas Simples (`'`):** `const texto1 = 'Olá, mundo!';`
    
2. **Aspas Duplas (`"`):** `const texto2 = "JavaScript é versátil.";`
    
3. **Crases (`` ` ``):** ``const texto3 = `Esta é uma Template Literal.`;``
    

A escolha entre aspas simples e duplas é, em grande parte, uma questão de preferência estilística e consistência de projeto. A principal vantagem é a conveniência ao incluir o outro tipo de aspas dentro da string sem a necessidade de escapá-las.

```js
const frase1 = "Ela disse: 'Olá!'";
const frase2 = 'O atributo é `class="ativo"`';
```

As crases, no entanto, habilitam uma funcionalidade muito mais poderosa conhecida como **Template Literals**, que exploraremos em detalhe mais adiante neste capítulo.

### Imutabilidade

Strings em JavaScript são **imutáveis**. Isso significa que, uma vez que uma string é criada, seu conteúdo não pode ser alterado. Métodos que parecem modificar uma string, como `toUpperCase()` ou `replace()`, na verdade **retornam uma nova string** com as modificações aplicadas, deixando a original intacta.

```js
let saudacao = "olá";
console.log("Original:", saudacao); // Original: olá

let saudacaoMaiuscula = saudacao.toUpperCase();
console.log("Nova string:", saudacaoMaiuscula); // Nova string: OLÁ
console.log("Original inalterada:", saudacao);  // Original inalterada: olá
```

Da mesma forma, não é possível alterar um caractere individual de uma string através de seu índice.

```js
let palavra = "carro";
// Tentar alterar o primeiro caractere não funciona
palavra[0] = "p"; 
console.log(palavra); // Saída: "carro"
```

Essa característica de imutabilidade garante que o comportamento das strings seja previsível e seguro, prevenindo modificações acidentais em dados textuais que são passados através de diferentes partes de uma aplicação.

## Operações Básicas e Acesso

### Concatenação de Strings

A forma mais comum de juntar strings é usando o operador de adição (`+`). Devido à coerção de tipo, se um dos operandos for uma string, o JavaScript converterá o outro operando para uma string e os concatenará.

```js
const primeiroNome = "João";
const ultimoNome = "Silva";
const nomeCompleto = primeiroNome + " " + ultimoNome;
console.log(nomeCompleto); // "João Silva"

const versao = "Versão " + 2.0;
console.log(versao); // "Versão 2.0"
```

Alternativamente, pode-se usar o método `concat()`, que pode aceitar múltiplos argumentos.

```js
const str1 = "Feliz ";
const str2 = "Aniversário";
const str3 = "!";
const mensagem = str1.concat(str2, str3);
console.log(mensagem); // "Feliz Aniversário!"
```

Na prática, o operador `+` é mais comumente utilizado pela sua sintaxe mais concisa.

### Acessando Caracteres

Você pode acessar um caractere individual em uma string usando a notação de colchetes (`[]`), como se fosse um array. O índice do primeiro elemento é zero.

```js
const linguagem = "JavaScript";

console.log(linguagem[0]);  // "J" (primeiro caractere)
console.log(linguagem[4]);  // "S" (quinto caractere)
```

A propriedade `length` retorna o número total de caracteres na string.

```js
console.log(linguagem.length); // 10

// Acessando o último caractere
console.log(linguagem[linguagem.length - 1]); // "t"
```

## Sequências de Escape

Sequências de escape são usadas para inserir caracteres especiais em uma string que, de outra forma, seriam impossíveis ou difíceis de digitar. Elas sempre começam com uma barra invertida (`\`).

|Sequência|Descrição|
|---|---|
|`\'`|Aspas simples|
|`\"`|Aspas duplas|
|`\\`|Barra invertida|
|`\n`|Nova linha (Line Feed)|
|`\r`|Retorno de carro (Carriage Return)|
|`\t`|Tabulação horizontal|
|`\b`|Backspace|
|`\uXXXX`|Caractere Unicode (com 4 dígitos hexadecimais)|
|`\xXX`|Caractere Latin-1 (com 2 dígitos hexadecimais)|

**Exemplo de uso:**

```js
// Usando aspas duplas dentro de uma string com aspas duplas
const citacao = "Ele respondeu: \"Com certeza!\"";
console.log(citacao);

// Criando um caminho de arquivo no Windows
const caminho = "C:\\Usuarios\\Documentos";
console.log(caminho);

// Inserindo uma nova linha e uma tabulação
const textoFormatado = "Linha 1\n\tLinha 2 recuada";
console.log(textoFormatado);

// Usando Unicode para o símbolo de copyright
const copyright = "Copyright \u00A9 2025";
console.log(copyright); // Copyright © 2025
```

## Template Literals (ES2015)

As Template Literals, ou literais de modelo, são uma das melhorias mais celebradas do ES2015. Envolvidas por crases (`` ` ``), elas resolvem dois dos problemas mais comuns na manipulação de strings: a concatenação verbosa e a criação de strings de múltiplas linhas.

### Interpolação de Expressões

A interpolação permite embutir variáveis e qualquer expressão JavaScript válida diretamente dentro da string, usando a sintaxe `${expression}`. Isso torna o código muito mais legível e conciso do que a concatenação tradicional com o operador `+`.

**Exemplo de Interpolação:**

```js
const nome = "Maria";
const idade = 28;
const profissao = "Engenheira de Software";

// Forma antiga com concatenação
const apresentacaoAntiga = "Meu nome é " + nome + ", eu tenho " + idade + " anos e sou " + profissao + ".";

// Forma moderna com Template Literals
const apresentacaoModerna = `Meu nome é ${nome}, eu tenho ${idade} anos e sou ${profissao}.`;

console.log(apresentacaoModerna);

// Você pode usar qualquer expressão válida
const preco = 50;
const quantidade = 3;
const mensagemTotal = `O total da compra é R$ ${(preco * quantidade).toFixed(2)}.`;
console.log(mensagemTotal); // O total da compra é R$ 150.00.
```

### Strings de Múltiplas Linhas

Com Template Literals, as quebras de linha no código-fonte são preservadas na string resultante, eliminando a necessidade de usar `\n` ou concatenar múltiplas strings.

**Exemplo de Múltiplas Linhas:**

```js
const blocoHtml = `
<div>
    <h1>Bem-vindo!</h1>
    <p>Este é um parágrafo dentro de um bloco HTML.</p>
</div>
`;
console.log(blocoHtml);
```

Isso é particularmente útil para gerar blocos de HTML ou formatar textos longos.

## Métodos Essenciais para Manipulação de Strings

O protótipo do `String` fornece um rico conjunto de métodos para trabalhar com texto. Lembre-se, todos eles retornam uma nova string, mantendo a original intacta.

### Buscando e Verificando Substrings

- **`includes(substring, position)`**: Retorna `true` se a string contém a `substring` especificada. O segundo argumento opcional `position` indica a partir de qual índice iniciar a busca.
- **`startsWith(substring, position)`**: Retorna `true` se a string começa com a `substring`.
- **`endsWith(substring, length)`**: Retorna `true` se a string termina com a `substring`. O segundo argumento opcional `length` define o comprimento da string a ser considerado.
- **`indexOf(substring, fromIndex)`**: Retorna o índice da primeira ocorrência da `substring`. Se não for encontrada, retorna `-1`.
- **`lastIndexOf(substring, fromIndex)` --**: Retorna o índice da última ocorrência da `substring`.

```js
const frase = "O rato roeu a roupa do rei de Roma.";

console.log(frase.includes("roupa"));     // true
console.log(frase.startsWith("O rato"));    // true
console.log(frase.endsWith("Roma."));       // true
console.log(frase.indexOf("roeu"));         // 7
console.log(frase.indexOf("gato"));         // -1
console.log(frase.indexOf("o", 5));         // 8 (inicia a busca a partir do índice 5)
console.log(frase.lastIndexOf("o"));        // 30
```

### Extraindo Partes da String

- **`slice(startIndex, endIndex)`**: Extrai uma seção da string e a retorna como uma nova string. `endIndex` é opcional e não inclusivo. Aceita índices negativos, que contam a partir do final da string.
- **`substring(startIndex, endIndex)`**: Similar ao `slice`, mas não aceita índices negativos. Se `startIndex` for maior que `endIndex`, os valores são trocados.
- **`substr(startIndex, length)`**: (Legado e não recomendado) Extrai uma `substring` a partir de `startIndex` com um comprimento `length` especificado.

```js
const texto = "Aprendendo JavaScript";

console.log(texto.slice(11));        // "JavaScript"
console.log(texto.slice(11, 15));    // "Java"
console.log(texto.slice(-6));        // "Script" (os últimos 6 caracteres)
console.log(texto.substring(0, 10)); // "Aprendendo"
```

Na prática, `slice()` é o método mais flexível e comumente usado para extração.

### Modificando e Transformando Strings

- **`replace(pattern, replacement)`**: Substitui ocorrências de um `pattern` (pode ser uma string ou uma Expressão Regular). Se o `pattern` for uma string, apenas a primeira ocorrência é substituída.
- **`replaceAll(pattern, replacement)`**: Substitui **todas** as ocorrências de um `pattern` (string ou Expressão Regular com a flag global `/g`).
- **`toUpperCase()` / `toLowerCase()`**: Convertem a string inteira para maiúsculas ou minúsculas, respectivamente.
- **`trim()` / `trimStart()` / `trimEnd()`**: Removem espaços em branco do início e/ou do fim da string.

```js
const original = "  o javascript é uma linguagem poderosa!  ";
console.log(original.replace("javascript", "JS")); // "  o JS é uma linguagem poderosa!  "
console.log(original.replaceAll("a", "4"));      // "  o j4v4script é um4 lingu4gem poderos4!  "
console.log(original.toUpperCase());                 // "  O JAVASCRIPT É UMA LINGUAGEM PODEROSA!  "
console.log(original.trim());                        // "o javascript é uma linguagem poderosa!"
```

### Dividindo e Juntando

- **`split(separator)`**: Divide a string em um array de substrings, usando o `separator` como ponto de divisão.
- **`repeat(count)`**: Cria e retorna uma nova string contendo a string original repetida `count` vezes.

```js
const csv = "maçã,banana,laranja,uva";
const frutas = csv.split(",");
console.log(frutas); // ["maçã", "banana", "laranja", "uva"]

const eco = "Eco... ";
console.log(eco.repeat(3)); // "Eco... Eco... Eco... "
```

## Considerações Finais

Neste capítulo, exploramos o tipo de dado `string`, um dos pilares da manipulação de informações em JavaScript. Vimos sua natureza primitiva e imutável, o que garante previsibilidade e segurança. Detalhamos as formas de criação, com destaque para as **Template Literals**, que modernizaram a forma como construímos strings complexas.

Apresentamos uma vasta gama de métodos nativos que formam uma API poderosa para qualquer tarefa textual: desde a busca com `includes()` e `indexOf()`, passando pela extração com `slice()`, até a transformação com `replace()` e `toUpperCase()`. O domínio desses métodos é essencial, pois elimina a necessidade de reinventar a roda para operações comuns de manipulação de texto.
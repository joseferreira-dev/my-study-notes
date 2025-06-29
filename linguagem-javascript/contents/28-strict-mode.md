# Capítulo 28 – Modo Estrito (Strict Mode): Escrevendo JavaScript Mais Seguro

Ao longo de nossa jornada pelo JavaScript, exploramos suas funcionalidades poderosas, sua sintaxe flexível e seu ecossistema dinâmico. No entanto, uma das características que definiram a linguagem por muitos anos foi sua natureza "permissiva" ou "indulgente". Para garantir a retrocompatibilidade e a facilidade de aprendizado para iniciantes, o JavaScript, por padrão, muitas vezes opta por falhar silenciosamente ou por executar ações "inseguras" em vez de lançar um erro. Atribuir um valor a uma variável não declarada? O JavaScript cria uma variável global para você. Tentar modificar uma propriedade somente leitura? A operação simplesmente não acontece, sem nenhum aviso. Essa abordagem, embora facilite a execução de scripts simples, é uma fonte notória de bugs sutis, comportamentos inesperados e vulnerabilidades de segurança em aplicações complexas.

Para combater esses problemas e guiar os desenvolvedores em direção a práticas de codificação melhores, o ECMAScript 5 (ES5) introduziu o **Modo Estrito (Strict Mode)**. O Strict Mode não é uma nova versão do JavaScript; é um **mecanismo de adesão (opt-in)** que nos permite escolher uma variante mais restrita e segura da linguagem para nosso código. Ao ativar o Modo Estrito, estamos essencialmente dizendo ao motor do JavaScript: "Seja menos permissivo. Transforme os erros silenciosos em exceções ruidosas. Proíba sintaxes ambíguas e me ajude a escrever um código mais limpo e correto."

Neste capítulo, vamos dominar o Modo Estrito. Começaremos por aprender como ativá-lo, seja para um script inteiro ou para funções específicas. Em seguida, faremos um mergulho profundo nas mudanças que ele impõe, explorando como ele lida com variáveis, propriedades, funções e o comportamento do `this`. Veremos como o Strict Mode elimina algumas das partes mais confusas e propensas a erro da linguagem, como a ligação "mágica" no objeto `arguments` e a criação acidental de variáveis globais. Ao final, você entenderá que o Modo Estrito não é uma limitação, mas sim uma rede de segurança indispensável e uma das melhores práticas fundamentais para o desenvolvimento de JavaScript moderno e profissional.

## Por Que o Modo Estrito Existe?

O JavaScript foi projetado para ser uma linguagem fácil de se começar a usar, mas essa facilidade veio com um custo. Certos comportamentos, embora convenientes em pequenos scripts, tornam-se perigosos em grandes aplicações.

**O problema do "Sloppy Mode" (Modo Não-Estrito):** O modo padrão de execução do JavaScript é informalmente chamado de "sloppy mode". Nele, certas ações que deveriam ser erros são permitidas:

```js
// Em modo não-estrito...
variavelNaoDeclarada = "Eu sou uma variável global acidental!"; // Cria uma propriedade no objeto global.

const obj = {};
Object.defineProperty(obj, 'propriedade', { value: 42, writable: false });
obj.propriedade = 100; // Falha silenciosamente, sem nenhum erro.

console.log(variavelNaoDeclarada);
console.log(obj.propriedade); // 42
```

Esses "erros silenciosos" são incrivelmente difíceis de depurar. O Modo Estrito foi criado para transformar esses problemas em erros explícitos e imediatos, forçando o desenvolvedor a escrever um código mais correto desde o início.

## Ativando o Strict Mode

A ativação do Modo Estrito é intencionalmente simples. Ela é feita através de uma diretiva em string: `"use strict";` (ou `'use strict';`). O local onde essa diretiva é colocada determina seu escopo.

### Para um Script Inteiro

Para habilitar o Modo Estrito para um arquivo JavaScript inteiro, a diretiva `"use strict";` deve ser a **primeira instrução** no arquivo. Nenhum outro código (exceto comentários ou espaços em branco) pode vir antes dela.

**`meu-script.js`**

```js
"use strict";

// Todo o código neste arquivo agora será executado em Modo Estrito.
let x = 10;
// y = 20; // ReferenceError: y is not defined

function minhaFuncao() {
  // Esta função também está em Modo Estrito.
  const z = 30;
  return x + z;
}
```

Esta é a abordagem recomendada para todo novo projeto de JavaScript. É importante notar que ao concatenar múltiplos arquivos, se o primeiro deles estiver em modo estrito, todo o script concatenado pode acabar sendo interpretado como estrito. Ferramentas de build modernas, como Webpack ou Rollup, gerenciam isso de forma inteligente.

### Para uma Função Específica

Você também pode ativar o Modo Estrito apenas para uma função específica, colocando a diretiva como a primeira instrução dentro do corpo da função.

```js
// Código fora da função ainda está em modo não-estrito.
outraVariavel = "global"; // Funciona

function minhaFuncaoEstrita() {
  "use strict";
  // O código aqui dentro está em Modo Estrito.
  let variavelLocal = "local";
  // novaVariavel = "erro"; // ReferenceError: novaVariavel is not defined
}

minhaFuncaoEstrita();
```

Esta abordagem é particularmente útil ao refatorar código legado, permitindo que você introduza o Modo Estrito gradualmente, função por função, sem o risco de quebrar outras partes do sistema que podem depender do comportamento do modo não-estrito.

**Importante:** Módulos ES6 e Classes JavaScript são, por padrão e **implicitamente**, executados em Modo Estrito. Você não precisa adicionar `"use strict";` a eles. Isso reforça a ideia de que o Modo Estrito é o padrão para o JavaScript moderno.

## Mudanças Significativas no Modo Estrito

Vamos explorar as principais mudanças que o `"use strict";` introduz.

### Variáveis e Atribuições

Esta é a área com os benefícios mais imediatos e visíveis.

- **Prevenção de Globais Acidentais:** Como já mencionado, tentar atribuir um valor a uma variável não declarada lança um `ReferenceError`. Isso elimina uma das fontes de bugs mais comuns em JavaScript.
    
    ```js
    "use strict";
    function salvarDados(dados) {
      // Erro de digitação: 'usurio' em vez de 'usuario'
      // Em modo não-estrito, isso criaria uma variável global 'usurio'.
      // Em modo estrito, isso lança um ReferenceError.
      usurio = dados; 
    }
    ```
    
- **Erros em Atribuições Inválidas:** Tentativas de modificar propriedades que não podem ser alteradas agora lançam um `TypeError`.
    
    - Atribuir a uma propriedade somente leitura (`writable: false`).
    - Atribuir a uma propriedade que possui apenas um getter.
    - Atribuir a uma nova propriedade em um objeto não extensível (`Object.preventExtensions()`).
    
    ```js
    "use strict";
    const obj = { get x() { return 17; } };
    // obj.x = 5; // TypeError: Cannot set property x of #<Object> which has only a getter
    ```
    
- **Erros ao Deletar Propriedades/Variáveis:** Tentar deletar uma variável, uma função ou uma propriedade não configurável (`configurable: false`) lança um `TypeError`.
    
    ```js
    "use strict";
    let x = 42;
    // delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
    
    const obj = Object.freeze({ prop: 1 });
    // delete obj.prop; // TypeError
    ```
    

### Funções e Parâmetros

O Modo Estrito limpa várias idiossincrasias relacionadas a funções.

- **Parâmetros Duplicados:** Declarar uma função com nomes de parâmetros duplicados se torna um `SyntaxError`.
    
    ```js
    // Em modo não-estrito, isso funciona (o último parâmetro prevalece).
    // function somar(a, a) { return a; } somar(2, 3); // retorna 3
    
    // Em modo estrito, é um erro de sintaxe.
    function somarEstrito(a, a) { // SyntaxError: Duplicate parameter name not allowed in this context
      "use strict";
      return a + a;
    }
    ```
    
- **Escopo de Função:** Em navegadores mais antigos, funções declaradas dentro de blocos (`if`, `for`) "vazavam" para o escopo da função externa. O Modo Estrito alinha seu comportamento ao de `let`/`const`, tornando-as verdadeiramente com escopo de bloco.
	
- **Comportamento do Objeto `arguments`:** Esta é uma mudança crucial.
    
    - No modo não-estrito, o objeto `arguments` tem uma ligação "mágica" com os parâmetros nomeados da função. Alterar um afeta o outro.
        
        ```js
        function sloppy(a) {
          console.log(a, arguments[0]); // 10, 10
          a = 20;
          console.log(a, arguments[0]); // 20, 20
          arguments[0] = 30;
          console.log(a, arguments[0]); // 30, 30
        }
        sloppy(10);
        ```
        
    - No Modo Estrito, essa ligação é removida. `arguments` se torna um simples snapshot dos valores que foram passados para a função no momento da chamada.
        
        ```js
        function strict(a) {
          "use strict";
          console.log(a, arguments[0]); // 10, 10
          a = 20;
          console.log(a, arguments[0]); // 20, 10 (não mais ligados!)
          arguments[0] = 30;
          console.log(a, arguments[0]); // 20, 30 (a alteração em 'arguments' não afeta 'a')
        }
        strict(10);
        ```
        
    - **`arguments.callee` e `arguments.caller`** são proibidos e lançam um `TypeError` ao serem acessados.

### O Comportamento de `this`

Em uma chamada de função normal (não como um método de um objeto), o valor de `this` no modo não-estrito é coagido para o objeto global (`window` no navegador). Isso é uma fonte comum de bugs, pois pode levar à modificação acidental do escopo global.

O Modo Estrito corrige isso: se `this` não for explicitamente definido por um `call`, `apply` ou `bind`, seu valor será `undefined`.

```js
function mostrarThis() {
  console.log(this);
}
mostrarThis(); // Em modo não-estrito, imprime o objeto 'window'.

function mostrarThisEstrito() {
  "use strict";
  console.log(this);
}
mostrarThisEstrito(); // Em modo estrito, imprime 'undefined'.
```

### Outras Mudanças Notáveis

- **Literais Octais:** A sintaxe antiga para números octais, prefixando com um zero (ex: `010` para o número 8), é proibida. A sintaxe moderna (`0o10`) funciona em ambos os modos.
- **A Instrução `with`:** A instrução `with`, que altera a cadeia de escopo, é notoriamente confusa e prejudicial à performance. O Modo Estrito a proíbe completamente, lançando um `SyntaxError`.
- **O Escopo de `eval`:** A função `eval` em modo estrito cria suas próprias variáveis em um escopo separado, em vez de "vazá-las" para o escopo circundante, tornando-a um pouco mais segura.

## Considerações Finais

Neste capítulo, desvendamos o **Modo Estrito**, um recurso fundamental do JavaScript que atua como uma atualização de segurança para a linguagem. Vimos que ativá-lo com a diretiva `"use strict";` não adiciona novas funcionalidades, mas sim remove ou modifica as partes mais problemáticas e propensas a erro do JavaScript "clássico".

Exploramos como o Modo Estrito transforma erros silenciosos em exceções explícitas, impedindo a criação acidental de variáveis globais e o acesso a propriedades somente leitura. Dissecamos suas melhorias no comportamento de funções, proibindo parâmetros duplicados e, crucialmente, removendo a confusa ligação dinâmica entre os parâmetros nomeados e o objeto `arguments`. Aprendemos também como ele torna o comportamento do `this` mais seguro, evitando a poluição do objeto global.

A conclusão é inequívoca: o Modo Estrito não é uma opção, mas sim a maneira como o JavaScript moderno deve ser escrito. O fato de Classes e Módulos ES6 serem estritos por padrão é a prova de que este é o caminho a seguir. Adotar o `"use strict";` em todos os seus novos scripts é um dos hábitos mais simples e eficazes que você pode desenvolver para escrever um código mais limpo, mais seguro e mais profissional.
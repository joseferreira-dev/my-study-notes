# Capítulo 13 – Estruturais Condicionais: Tomando Decisões

Em nossa jornada até aqui, construímos uma base sólida. Dominamos a declaração de variáveis, a manipulação de diferentes tipos de dados e, no capítulo anterior, aprendemos a automatizar tarefas repetitivas com laços. Nossos programas já podem executar sequências de instruções e repeti-las. No entanto, para que um software seja verdadeiramente útil e inteligente, ele precisa de uma capacidade fundamental: a de **tomar decisões**. Ele precisa ser capaz de avaliar o estado atual da aplicação e escolher um caminho de execução em detrimento de outro. Um programa que não pode tomar decisões é apenas uma receita de bolo inflexível; um programa que pode, se torna um sistema dinâmico e responsivo.

É aqui que entram as **estruturas condicionais**. Elas são os cruzamentos e as bifurcações nas estradas do nosso código, permitindo que o fluxo de execução se altere com base em critérios que definimos. Seja para verificar se um usuário inseriu a senha correta, para exibir uma mensagem de boas-vindas diferente dependendo da hora do dia, ou para aplicar um desconto se o valor de uma compra ultrapassar um certo limite, a lógica condicional está no cerne de toda interação significativa em uma aplicação.

Neste capítulo, faremos um mergulho profundo nas ferramentas que o JavaScript nos oferece para a tomada de decisões. Começaremos com a estrutura `if...else`, a espinha dorsal de toda a lógica condicional, explorando suas variações para lidar com múltiplos cenários. Dissecaremos o conceito crucial de "truthy" e "falsy", que rege como o JavaScript avalia condições. Em seguida, analisaremos a instrução `switch`, uma alternativa para múltiplos testes, e a compararemos com o poderoso **Strategy Pattern**. Por fim, exploraremos formas mais concisas e expressivas de se escrever lógicas condicionais, como o **operador ternário** e o inteligente uso do **curto-circuito (short-circuiting)** dos operadores lógicos `&&` e `||`. Ao final, você não apenas saberá como escrever condições, mas também como arquitetá-las de forma limpa, eficiente e de fácil manutenção.

## O Bloco de Construção Fundamental: `if`, `else if` e `else`

A estrutura `if` é a forma mais básica de controle condicional. Ela permite que um bloco de código seja executado somente se uma determinada condição for avaliada como verdadeira.

### A Instrução `if` Simples

A forma mais simples de uma condição verifica um único caso. Se a expressão dentro dos parênteses `()` for verdadeira (`true`), o bloco de código dentro das chaves `{}` é executado. Caso contrário, ele é completamente ignorado.

**Sintaxe:** `if (condição) {` `// Bloco de código a ser executado se a condição for verdadeira` `}`

**Exemplo: Verificando Permissão de Acesso**

```js
let idade = 20;

if (idade >= 18) {
  console.log("Acesso permitido. Você é maior de idade.");
}

console.log("Verificação de idade concluída.");
```

Neste caso, como `20 >= 18` é `true`, a mensagem `Acesso permitido...` é exibida. Se `idade` fosse 17, essa linha seria pulada.

### A Cláusula `else`: O Caminho Alternativo

Muitas vezes, não queremos apenas executar um código se uma condição for verdadeira, mas também executar um código _alternativo_ se ela for falsa. A cláusula `else` nos fornece esse caminho.

**Sintaxe:** `if (condição) {` `// Bloco executado se a condição for verdadeira` `} else {` `// Bloco executado se a condição for falsa` `}`

**Exemplo: Verificação de Login**

```js
let usuarioLogado = false;

if (usuarioLogado) {
  console.log("Bem-vindo(a) de volta!");
} else {
  console.log("Por favor, faça o login para continuar.");
}
```

Isso garante que exatamente um dos dois blocos será executado.

### A Cadeia `if...else if...else`: Múltiplas Condições

Para lidar com mais de duas possibilidades, podemos encadear várias condições usando `else if`. O JavaScript avaliará cada condição em ordem. Assim que encontrar uma que seja verdadeira, ele executará o bloco correspondente e **ignorará todas as outras condições da cadeia**. Se nenhuma condição `if` ou `else if` for verdadeira, o bloco `else` final será executado, se existir.

**Exemplo: Saudação baseada na Hora do Dia**

```js
const hora = new Date().getHours(); // Retorna a hora atual (0-23)

if (hora < 12) {
  console.log("Bom dia!");
} else if (hora < 18) {
  console.log("Boa tarde!");
} else {
  console.log("Boa noite!");
}
```

Nesta estrutura, apenas uma saudação será exibida. A ordem é crucial. Se invertêssemos as duas primeiras condições, `hora < 18` seria verdadeiro para 10 da manhã, exibindo "Boa tarde!" incorretamente.

### `if...if...if`: Condições Independentes

É importante distinguir a cadeia `if...else if` de uma série de `if`s independentes. Quando usamos `if`s separados, cada condição é avaliada independentemente das outras.

```js
const permissoes = { isAdmin: true, podeEditar: true };

// Usando else if (apenas um bloco executa)
if (permissoes.isAdmin) {
  console.log("É admin."); // Este executa e para a verificação
} else if (permissoes.podeEditar) {
  console.log("Pode editar.");
}

console.log('---');

// Usando if...if (ambos são checados)
if (permissoes.isAdmin) {
  console.log("É admin."); // Este executa
}
if (permissoes.podeEditar) {
  console.log("Pode editar."); // Este também executa
}
```

Use `if...else if` para cenários mutuamente exclusivos e `if...if` para verificar várias condições independentes onde mais de uma pode ser verdadeira.

## Truthy e Falsy: O Coração das Condições em JavaScript

Uma das características mais poderosas e, por vezes, confusas do JavaScript é sua noção de valores "truthy" (verdadeiros) e "falsy" (falsos). Em um contexto condicional (como um `if`), o JavaScript não exige que o valor seja estritamente `true` ou `false`. Em vez disso, ele usa um processo de coerção para determinar se um valor se comporta como verdadeiro ou falso.

Existem apenas **sete** valores "falsy" em JavaScript. Qualquer outro valor é "truthy". Os valores **Falsy** são:

- `false`
- `0` (o número zero)
- `-0` (zero negativo)
- `0n` (BigInt zero)
- `""` (uma string vazia)
- `null`
- `undefined`
- `NaN` (Not a Number)

**Qualquer outro valor é Truthy.** Isso inclui:

- Strings não vazias (`"olá"`, `"false"`, `"0"`)
- Qualquer número diferente de zero (`1`, `-10`)
- Arrays, mesmo que vazios (`[]`)
- Objetos, mesmo que vazios (`{}`)
- Funções

Compreender isso nos permite escrever código mais conciso.

```js
let nomeUsuario = "";
// Em vez de: if (nomeUsuario !== "")
if (nomeUsuario) {
  console.log(`Bem-vindo, ${nomeUsuario}`);
} else {
  console.log("Por favor, forneça um nome."); // Este bloco executa
}

let itensNoCarrinho = [];
// Em vez de: if (itensNoCarrinho.length > 0)
// Atenção! Um array vazio é truthy, então a condição if([]) é verdadeira.
// A verificação correta para arrays é pelo seu comprimento.
if (itensNoCarrinho.length) { // 0 é falsy
  console.log("Você tem itens no carrinho.");
} else {
  console.log("Seu carrinho está vazio."); // Este bloco executa
}
```

## A Instrução `switch`: Alternativa para Múltiplos Casos

A instrução `switch` fornece uma sintaxe alternativa e, por vezes, mais limpa do que uma longa cadeia `if...else if...else`, especialmente quando todas as condições dependem do valor de uma única variável.

**Sintaxe:** `switch (expressão) {` `case valor1:` `// Código para valor1` `break;` `case valor2:` `// Código para valor2` `break;` `...` `default:` `// Código a ser executado se nenhum case corresponder` `}`

O `switch` avalia a `expressão` uma vez e compara seu resultado com o valor de cada `case` usando uma comparação estrita (`===`).

- **`break`:** A instrução `break` é crucial. Ela impede que a execução "caia" (fall-through) para o próximo `case`. Se você omitir o `break`, o código do `case` seguinte será executado, independentemente de seu valor corresponder.
- **`default`:** A cláusula `default` é opcional e funciona como o `else` final, sendo executada se nenhum `case` corresponder.

**Exemplo: Lidando com Tipos de Usuário**

```js
const tipoUsuario = "admin";
let dashboard;

switch (tipoUsuario) {
  case "admin":
    dashboard = "Painel de Administração Completo";
    break;
  case "editor":
    dashboard = "Painel de Edição de Conteúdo";
    break;
  case "leitor":
    dashboard = "Painel de Leitura Simples";
    break;
  default:
    dashboard = "Dashboard Padrão";
}

console.log(dashboard); // "Painel de Administração Completo"
```

### Agrupando Casos com Fall-Through

O comportamento de "fall-through" (quando `break` é omitido) pode ser usado intencionalmente para agrupar casos que devem executar o mesmo código.

**Exemplo: Verificando Fim de Semana**

```js
const dia = new Date().getDay(); // 0 (Dom) a 6 (Sáb)

switch (dia) {
  case 0: // Domingo
  case 6: // Sábado
    console.log("É fim de semana!");
    break;
  case 1: // Segunda
  case 2:
  case 3:
  case 4:
  case 5:
    console.log("É dia de semana.");
    break;
}
```

## O Operador Condicional (Ternário): `? :`

O operador ternário é uma forma extremamente concisa de se escrever uma instrução `if...else` simples. É o único operador em JavaScript que utiliza três operandos.

**Sintaxe:** `condição ? expressãoSeTrue : expressãoSeFalse`

Ele é mais adequado para atribuições condicionais ou para retornar um valor de uma função, e não para executar lógicas complexas com múltiplos passos.

**Exemplo: Definindo uma Mensagem**

```js
const pontuacao = 85;

// Forma com if...else
let resultado;
if (pontuacao >= 60) {
  resultado = "Aprovado";
} else {
  resultado = "Reprovado";
}

// Forma com operador ternário
const resultadoTernario = pontuacao >= 60 ? "Aprovado" : "Reprovado";

console.log(resultadoTernario); // "Aprovado"
```

O uso do ternário torna o código mais curto e, em casos simples como este, mais legível. No entanto, evite aninhar operadores ternários, pois eles rapidamente se tornam difíceis de ler.

## Condicionais com Operadores Lógicos: Curto-Circuito (Short-Circuiting)

Os operadores lógicos `||` (OU) e `&&` (E) em JavaScript têm um comportamento especial que vai além de simplesmente retornar `true` ou `false`. Eles realizam uma **avaliação em curto-circuito** e retornam o valor de um dos operandos, o que nos permite usá-los como estruturas condicionais compactas.

### O Operador OU (`||`) para Valores Padrão

O operador `||` avalia as expressões da esquerda para a direita. Ele retorna o **primeiro valor truthy** que encontrar. Se todos forem falsy, ele retorna o último valor falsy.

Isso o torna perfeito para definir valores padrão.

```js
let nomeFornecido = null;
const nomeUsuario = nomeFornecido || "Visitante"; // null é falsy, então retorna "Visitante"
console.log(`Olá, ${nomeUsuario}!`); // "Olá, Visitante!"

let configuracao = { porta: 8080 };
const portaFinal = configuracao.porta || 3000; // 8080 é truthy, então é retornado
console.log(`Servidor rodando na porta ${portaFinal}`); // "Servidor rodando na porta 8080"
```

**Atenção:** Como `||` reage a qualquer valor falsy (`0`, `""`, `false`), ele pode não ser ideal em todos os cenários. Para esses casos, o **operador de coalescência nula (`??`)**, que reage apenas a `null` e `undefined`, é a melhor escolha.

### O Operador E (`&&`) para Execução Condicional

O operador `&&` também avalia da esquerda para a direita. Ele retorna o **primeiro valor falsy** que encontrar. Se todos forem truthy, ele retorna o último valor truthy.

Isso nos permite executar um código condicionalmente. Se a primeira parte for `true`, a segunda parte será executada (e seu resultado, retornado).

```js
const usuarioLogado = true;
// Se usuarioLogado for true, a função renderizarBotaoLogout() será chamada.
usuarioLogado && renderizarBotaoLogout();

function renderizarBotaoLogout() {
  console.log("Botão de logout renderizado.");
}
```

Isso é funcionalmente equivalente a: `if (usuarioLogado) { renderizarBotaoLogout(); }`. É um padrão muito comum em frameworks como o React para renderização condicional.

## Padrão Avançado: Substituindo `switch` com o Strategy Pattern

Conforme uma aplicação cresce, uma instrução `switch` pode se tornar longa, difícil de manter e violar o Princípio Aberto/Fechado (o software deve ser aberto para extensão, mas fechado para modificação). Uma alternativa elegante e mais escalável é o **Strategy Pattern**, que usa um objeto ou um `Map` para associar valores a ações.

Em vez de usar `switch` para decidir qual função chamar com base em uma string, podemos mapear essas strings diretamente para as funções.

**Exemplo: Refatorando o `switch` de Tipo de Usuário**

```js
// A implementação com switch
function getDashboard(tipoUsuario) {
  switch (tipoUsuario) {
    case "admin": return "Painel de Administração Completo";
    case "editor": return "Painel de Edição de Conteúdo";
    case "leitor": return "Painel de Leitura Simples";
    default: return "Dashboard Padrão";
  }
}

// A implementação com Strategy Pattern
const dashboardsPorTipo = {
  admin: "Painel de Administração Completo",
  editor: "Painel de Edição de Conteúdo",
  leitor: "Painel de Leitura Simples",
  default: "Dashboard Padrão"
};

function getDashboardStrategy(tipoUsuario) {
  return dashboardsPorTipo[tipoUsuario] || dashboardsPorTipo.default;
}

console.log(getDashboardStrategy("editor")); // "Painel de Edição de Conteúdo"
console.log(getDashboardStrategy("convidado")); // "Dashboard Padrão"
```

**Vantagens do Strategy Pattern:**

- **Escalabilidade:** Adicionar um novo tipo de usuário é tão simples quanto adicionar uma nova chave ao objeto, sem tocar na lógica da função.
- **Legibilidade:** A relação entre o tipo e o resultado é explícita e fácil de ler.
- **Manutenibilidade:** A lógica está desacoplada, tornando o código mais fácil de testar e manter.

## Considerações Finais

Neste capítulo, exploramos o arsenal de ferramentas que o JavaScript nos oferece para a tomada de decisões. Vimos como a estrutura **`if...else if...else`** forma a base da lógica condicional, permitindo-nos criar caminhos de execução complexos e mutuamente exclusivos. Mergulhamos no conceito de **truthy e falsy**, uma característica central do JavaScript que, uma vez dominada, permite um código mais idiomático e conciso.

Analisamos a instrução **`switch`** como uma alternativa para múltiplos `cases`, e a comparamos com o elegante e escalável **Strategy Pattern**, uma técnica que eleva a qualidade e a manutenibilidade do nosso código. Por fim, descobrimos como usar o **operador ternário** e o **curto-circuito dos operadores lógicos** para escrever condicionais de forma mais expressiva e compacta.

A capacidade de controlar o fluxo de execução com base em condições é o que transforma um script linear em uma aplicação dinâmica e interativa. Com as técnicas e os padrões apresentados aqui, você está agora preparado para construir qualquer lógica de decisão, desde a mais simples verificação até a mais complexa arquitetura de regras de negócio.
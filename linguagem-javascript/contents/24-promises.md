# Capítulo 24 – Promises: Gerenciando Operações Assíncronas

No capítulo anterior, desvendamos os timers e tivemos nosso primeiro contato real com a natureza assíncrona do JavaScript. Vimos como o Event Loop nos permite agendar tarefas sem travar a thread principal. Usamos callbacks para definir o que deveria acontecer quando uma tarefa terminasse. No entanto, também vislumbramos a escuridão que reside nesse caminho: o infame **"Callback Hell"**. Conforme encadeamos múltiplas operações assíncronas — buscar um usuário, depois buscar seus posts, depois buscar os comentários de cada post — nosso código começa a marchar para a direita, criando uma pirâmide aninhada de callbacks que é extremamente difícil de ler, depurar e manter.

Essa complexidade era um dos maiores pontos de dor no desenvolvimento JavaScript. Era evidente a necessidade de uma abstração melhor, uma maneira de lidar com resultados futuros que fosse mais limpa, mais componível e mais parecida com a forma como pensamos em código síncrono. Para resolver este problema, o ECMAScript 2015 (ES6) padronizou e introduziu um conceito que mudaria para sempre o cenário da programação assíncrona na linguagem: o objeto **`Promise`**.

Uma `Promise` é, literalmente, uma promessa. É um objeto que serve como um **placeholder** para um valor que ainda não conhecemos, mas que estará disponível no futuro. Ele representa o resultado eventual — seja o sucesso ou a falha — de uma operação assíncrona. Em vez de passar um callback **que será executado com o resultado**, a operação assíncrona nos retorna imediatamente um objeto `Promise` ao qual podemos **anexar** os callbacks que devem ser executados quando a promessa for cumprida.

Neste capítulo, vamos dominar esta poderosa abstração. Começaremos por dissecar a anatomia de uma Promise, entendendo seus três estados e como criar e consumir essas promessas. O ponto crucial será o **encadeamento de Promises (chaining)**, a técnica que resolve diretamente o "Callback Hell", permitindo-nos escrever código assíncrono de forma linear e legível. Exploraremos como gerenciar múltiplas Promises concorrentes, como converter funções antigas baseadas em callback para o mundo moderno das Promises e como lidar com erros de forma robusta e centralizada. Ao final, você verá as Promises não como um tópico complexo, mas como a ferramenta fundamental que traz sanidade e previsibilidade ao mundo assíncrono.

## A Anatomia de uma Promise

Uma `Promise` é um objeto que representa o estado de uma operação assíncrona. Ela pode estar em um de três estados:

- **`pending` (pendente):** O estado inicial. A operação ainda não foi concluída. A promessa ainda não foi cumprida nem rejeitada.
- **`fulfilled` (cumprida ou resolvida):** A operação foi concluída com sucesso. A promessa foi cumprida e agora tem um **valor** resultante.
- **`rejected` (rejeitada):** A operação falhou. A promessa foi rejeitada e agora tem um **motivo** (geralmente um objeto de Erro).

Uma vez que uma promise é "estabelecida" (settled) — ou seja, passa do estado `pending` para `fulfilled` ou `rejected` — seu estado e seu valor (ou motivo) se tornam **imutáveis**. Ela nunca mais mudará.

### Criando uma `Promise`

Criamos uma promise usando o construtor `new Promise()`. Ele recebe uma única função como argumento, chamada de **executor**. Esta função executor, por sua vez, recebe dois parâmetros, que são, por convenção, chamados de `resolve` e `reject`.

- `resolve(value)`: Uma função que você chama quando a operação assíncrona termina com **sucesso**. O valor passado para `resolve` se tornará o valor resultante da promise.
- `reject(reason)`: Uma função que você chama quando a operação assíncrona **falha**. O motivo (geralmente um `new Error()`) passado para `reject` se tornará o motivo da rejeição.

**Exemplo: "Prometendo" uma Requisição de API**

```js
function buscarUsuario(id) {
  // A função imediatamente retorna uma Promise
  return new Promise((resolve, reject) => {
    console.log(`Buscando dados para o usuário ${id}...`);
    // Simula uma operação de rede assíncrona
    setTimeout(() => {
      if (id === 123) {
        // Sucesso! Cumprimos a promessa com os dados do usuário.
        resolve({ id: 123, nome: "Ana Silva", email: "ana.silva@example.com" });
      } else {
        // Falha! Rejeitamos a promessa com um erro.
        reject(new Error("Usuário não encontrado."));
      }
    }, 2000);
  });
}
```

## Consumindo uma Promise: `.then()`, `.catch()` e `.finally()`

Uma vez que temos um objeto `Promise`, precisamos de uma maneira de reagir quando ela for estabelecida. Para isso, usamos os métodos `.then()`, `.catch()` e `.finally()`.

### `.then()`: Lidando com o Sucesso

O método `.then()` é a forma principal de se registrar um callback para ser executado quando uma promise é **cumprida**. Ele recebe até dois argumentos, mas o mais comum é usar apenas o primeiro: `promise.then(onFulfilled, onRejected)`

- `onFulfilled`: Uma função que será chamada com o valor de resolução da promise.

```js
const promessaDeUsuario = buscarUsuario(123);

promessaDeUsuario.then(usuario => {
  // Este bloco só executa quando a promise é resolvida com sucesso.
  console.log("Sucesso! Usuário encontrado:");
  console.log(usuario);
});

console.log("A busca foi iniciada, o código continua executando...");
```

### `.catch()`: Lidando com a Falha

Embora `.then()` possa aceitar um segundo argumento para lidar com a rejeição, a prática recomendada e mais legível é usar o método `.catch()`, que é dedicado exclusivamente a tratar erros.

```js
const promessaDeUsuarioFalha = buscarUsuario(999);

promessaDeUsuarioFalha
  .then(usuario => {
    console.log("Este bloco não será executado.");
  })
  .catch(erro => {
    // Este bloco executa quando a promise é rejeitada.
    console.error("Ocorreu um erro:", erro.message);
  });
```

### `.finally()`: Executando Limpezas

O método `.finally()` (ES2018) permite registrar um callback que será executado quando a promise for estabelecida, **independentemente de ter sido cumprida ou rejeitada**. É o local perfeito para código de "limpeza", como esconder um spinner de carregamento.

```js
console.log("Exibindo spinner de carregamento...");

buscarUsuario(123)
  .then(usuario => {
    console.log("Dados carregados.");
  })
  .catch(erro => {
    console.error("Erro ao carregar.");
  })
  .finally(() => {
    // Este bloco executa em ambos os casos, sucesso ou falha.
    console.log("Escondendo spinner de carregamento. Operação concluída.");
  });
```

## A Mágica do Encademento de Promises (Promise Chaining)

Este é o conceito que resolve o "Callback Hell". **Os métodos `.then()` e `.catch()` sempre retornam uma nova promise**. Isso nos permite encadear múltiplas operações assíncronas de forma linear e legível.

- Se um callback dentro de um `.then()` retorna um valor, a promise retornada por esse `.then()` será cumprida com esse valor.
- Se um callback dentro de um `.then()` retorna uma **outra promise**, a promise retornada pelo `.then()` "adotará" o estado dessa nova promise, esperando que ela se resolva para então se resolver com o mesmo valor.

**Exemplo: Encadeando Múltiplas Requisições**

```js
buscarUsuario(123) // Retorna uma promise com o usuário
  .then(usuario => {
    console.log(`Usuário encontrado: ${usuario.nome}. Buscando posts...`);
    // Retornamos uma nova promise
    return buscarPostsDoUsuario(usuario.id); 
  })
  .then(posts => {
    console.log(`Encontrados ${posts.length} posts. Processando...`);
    // Retornamos um valor síncrono
    const titulos = posts.map(p => p.titulo);
    return titulos;
  })
  .then(titulos => {
    console.log("Títulos dos posts:", titulos);
  })
  .catch(erro => {
    // Um único .catch() lida com erros de QUALQUER parte da cadeia.
    console.error("Falha em alguma etapa da cadeia:", erro.message);
  });

function buscarPostsDoUsuario(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve([
        { id: 1, titulo: "Promise é incrível" },
        { id: 2, titulo: "Como usar .then()" }
      ]);
    }, 1500);
  });
}
```

Observe a diferença: em vez de aninhamento, temos uma cadeia vertical e plana de operações. Se `buscarUsuario` falhar, ou se `buscarPostsDoUsuario` falhar, a execução pula diretamente para o bloco `.catch()`.

## "Promisificando": Modernizando Código Antigo

"Promisificar" é o ato de pegar uma função assíncrona que usa o padrão de callbacks e envolvê-la em uma nova função que retorna uma promise. Isso nos permite integrar código legado em um fluxo de trabalho moderno baseado em promises.

**Exemplo: Promisificando uma função com callback**

```js
// Função antiga baseada em callback
function atrasarExecucaoCallback(ms, callback) {
  setTimeout(() => {
    callback();
  }, ms);
}

// Versão "promisificada"
function atrasarExecucaoPromise(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

console.log("Iniciando...");
atrasarExecucaoPromise(2000).then(() => {
  console.log("2 segundos se passaram.");
});
```

## Gerenciando Múltiplas Promises Concorrentes

A classe `Promise` oferece métodos estáticos poderosos para orquestrar a execução de várias promises ao mesmo tempo.

### `Promise.all(iterable)`

`Promise.all` recebe um iterável (geralmente um array) de promises e retorna uma nova promise que:

- É **cumprida** quando **todas** as promises do iterável são cumpridas. O valor de resolução é um array com os valores de cada promise, na mesma ordem original.
- É **rejeitada** assim que **qualquer uma** das promises do iterável é rejeitada. A rejeição é imediata (comportamento "fail-fast").

**Caso de Uso:** Fazer múltiplas requisições de API independentes e esperar que todas terminem para processar os resultados.

```js
const promessa1 = Promise.resolve("Resultado 1");
const promessa2 = new Promise(resolve => setTimeout(() => resolve("Resultado 2"), 1000));
const promessa3 = Promise.resolve("Resultado 3");

Promise.all([promessa1, promessa2, promessa3])
  .then(resultados => {
    console.log("Todas as promises foram cumpridas:", resultados); // ["Resultado 1", "Resultado 2", "Resultado 3"]
  })
  .catch(erro => {
    console.error("Pelo menos uma promise falhou:", erro);
  });
```

### `Promise.race(iterable)`

`Promise.race` também recebe um iterável de promises, mas retorna uma nova promise que é estabelecida (cumprida ou rejeitada) assim que a **primeira** promise do iterável é estabelecida.

**Caso de Uso:** Criar um "timeout" para uma operação. Você corre uma promise de uma requisição de API contra uma promise de um `setTimeout`. Quem ganhar, determina o resultado.

```js
const requisicaoApi = new Promise(resolve => setTimeout(() => resolve("Dados da API"), 5000));
const timeout = new Promise((_, reject) => setTimeout(() => reject(new Error("Timeout da requisição!")), 3000));

Promise.race([requisicaoApi, timeout])
  .then(resultado => {
    console.log("Sucesso:", resultado);
  })
  .catch(erro => {
    console.error("Erro:", erro.message); // "Erro: Timeout da requisição!"
  });
```

## Padrões e Tópicos Adicionais

### Promises e `forEach`

Um erro comum é tentar usar `forEach` com uma função que retorna uma promise e esperar que o laço "espere" pela conclusão.

```js
const ids = [1, 2, 3];

// ERRADO: forEach não é "promise-aware".
ids.forEach(id => {
  buscarUsuario(id).then(console.log);
});
console.log("Este log aparece imediatamente, antes dos usuários.");
```

**Solução Correta:** Combine `map` com `Promise.all`.

```js
// CORRETO: Cria um array de promises
const arrayDePromises = ids.map(id => buscarUsuario(id));

// Espera que todas as promises do array sejam resolvidas
Promise.all(arrayDePromises)
  .then(usuarios => {
    console.log("Todos os usuários foram buscados:", usuarios);
  });
```

### Reduzindo um Array para Promises Encadeadas

E se você precisar executar operações assíncronas em um array **sequencialmente**, e não em paralelo? `Promise.all` não serve. Aqui, podemos usar `Array.prototype.reduce` de forma inteligente.

```js
const funcoesAssincronas = [
  () => atrasarExecucaoPromise(1000).then(() => console.log("Passo 1 completo")),
  () => atrasarExecucaoPromise(500).then(() => console.log("Passo 2 completo")),
  () => atrasarExecucaoPromise(1500).then(() => console.log("Passo 3 completo")),
];

funcoesAssincronas.reduce((cadeiaAnterior, funcaoAtual) => {
  return cadeiaAnterior.then(funcaoAtual);
}, Promise.resolve()) // Inicia a cadeia com uma promise já resolvida
.then(() => {
  console.log("Todos os passos foram concluídos em sequência.");
});
```

## Considerações Finais

Neste capítulo, desvendamos as **Promises**, a abstração que revolucionou a forma como escrevemos código assíncrono em JavaScript. Vimos como uma Promise atua como um placeholder para um resultado futuro, transitando pelos estados de `pending`, `fulfilled` e `rejected`.

O ponto crucial foi o domínio do **encadeamento com `.then()` e `.catch()`**, a solução elegante e definitiva para o "Callback Hell", que nos permite escrever fluxos assíncronos de forma linear, legível e de fácil manutenção. Aprendemos também a orquestrar múltiplas operações concorrentes com `Promise.all` e `Promise.race`, e a modernizar código legado através da "promisificação".

As Promises são mais do que uma feature da linguagem; elas são um novo paradigma para pensar sobre operações assíncronas. Elas formam a base indispensável para o próximo passo em nossa jornada: a sintaxe de `async/await`, que, como veremos, é um "açúcar sintático" construído sobre o robusto e poderoso sistema de Promises que acabamos de dominar.
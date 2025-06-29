# Capítulo 25 – Async/Await: Escrevendo Código Assíncrono de Forma Síncrona

No capítulo anterior, conquistamos um marco fundamental: o domínio das **Promises**. Vimos como elas nos resgataram do "Callback Hell", fornecendo um mecanismo robusto e componível para gerenciar operações assíncronas. Com o encadeamento de `.then()` e `.catch()`, conseguimos transformar uma pirâmide de código aninhado em uma sequência linear e legível. No entanto, mesmo com as Promises, a sintaxe ainda carrega o peso de sua natureza assíncrona: o uso contínuo de callbacks, ainda que de forma mais organizada. O fluxo mental para ler uma cadeia de `.then()`s ainda é diferente de ler um simples script de cima para baixo.

E se pudéssemos ir um passo além? E se pudéssemos escrever código que lida com operações assíncronas, mas que **parece** e se **comporta** como código síncrono? E se pudéssemos "pausar" a execução de uma função enquanto esperamos por um resultado, sem travar a thread principal, e depois continuar de onde paramos, como se nada tivesse acontecido? Para realizar este desejo, o ECMAScript 2017 (ES8) introduziu a sintaxe de **`async/await`**.

`async/await` não é uma tecnologia nova; é, em sua essência, **"açúcar sintático" sobre as Promises**. Ele não substitui as Promises, mas sim fornece uma nova e incrivelmente poderosa sintaxe para trabalhar com elas. A palavra-chave `async` transforma uma função para que ela sempre retorne uma promise, e a palavra-chave `await` "desembrulha" o valor de uma promise de forma que parece mágica, permitindo-nos atribuir resultados assíncronos a variáveis como faríamos em qualquer código síncrono.

Neste capítulo, vamos dominar esta sintaxe revolucionária. Começaremos por entender como as funções `async` funcionam e como o operador `await` nos permite pausar e retomar a execução. Veremos como o `async/await` simplifica drasticamente o tratamento de erros, permitindo-nos usar os familiares blocos `try...catch`. Exploraremos como lidar com operações paralelas, os padrões corretos para usar `await` dentro de laços e como essa nova sintaxe se compara diretamente com as cadeias de `.then()` que já conhecemos. Ao final, `async/await` se tornará sua ferramenta padrão para escrever código assíncrono, capacitando-o a criar lógicas complexas com uma clareza e simplicidade antes inimagináveis.

## Funções `async`: A Base de Tudo

A mágica do `async/await` começa com a palavra-chave `async`. Ao colocar `async` antes da definição de uma função, você a transforma em uma **função assíncrona**.

Uma função `async` tem algumas características principais e garantidas:

- Ela **sempre** retorna uma `Promise`.
- Se a função retornar um valor que não seja uma promise, o JavaScript automaticamente envolverá esse valor em uma promise resolvida (`Promise.resolve(valor)`).
- Se a função lançar um erro, esse erro será encapsulado em uma promise rejeitada (`Promise.reject(erro)`).

**Sintaxe:**

```js
// Declaração de função async
async function minhaFuncaoAsync() {
  return "Olá, mundo!";
}

// Expressão de função async
const outraFuncaoAsync = async function() {
  throw new Error("Algo deu errado!");
};

// Arrow function async
const maisUmaFuncaoAsync = async () => {
  const promessa = Promise.resolve(42);
  return promessa; // Retornando uma promise diretamente
};

// Consumindo o retorno
minhaFuncaoAsync().then(valor => console.log(valor)); // "Olá, mundo!"
outraFuncaoAsync().catch(erro => console.error(erro.message)); // "Algo deu errado!"
maisUmaFuncaoAsync().then(valor => console.log(valor)); // 42
```

A palavra-chave `async` é o que nos permite usar o operador `await` dentro da função.

## O Operador `await`: Pausando a Execução

A palavra-chave `await` só pode ser usada **dentro de uma função `async`**. Sua função é "pausar" a execução da função `async` até que uma `Promise` seja estabelecida (resolvida ou rejeitada).

- Se a promise for **cumprida**, `await` retorna o valor de resolução.
- Se a promise for **rejeitada**, `await` lança o erro de rejeição, que pode ser capturado por um `try...catch`.

`await` nos permite escrever código assíncrono que se parece e se comporta de forma linear, como se fosse síncrono.

**Comparativo: Promises (`.then()`) vs. `async/await`**

Vamos revisitar nosso exemplo de encadeamento de promises do capítulo anterior.

**Versão com `.then()`:**

```js
function buscarDadosComThen() {
  buscarUsuario(123)
    .then(usuario => {
      console.log(`Usuário: ${usuario.nome}`);
      return buscarPostsDoUsuario(usuario.id);
    })
    .then(posts => {
      console.log(`Posts encontrados: ${posts.length}`);
    })
    .catch(erro => {
      console.error(erro);
    });
}
```

**Versão com `async/await`:**

```js
async function buscarDadosComAsyncAwait() {
  try {
    // 1. Pausa até buscarUsuario() resolver, e 'usuario' recebe o valor.
    const usuario = await buscarUsuario(123);
    console.log(`Usuário: ${usuario.nome}`);

    // 2. Pausa até buscarPostsDoUsuario() resolver, e 'posts' recebe o valor.
    const posts = await buscarPostsDoUsuario(usuario.id);
    console.log(`Posts encontrados: ${posts.length}`);
  } catch (erro) {
    // Captura qualquer erro de qualquer uma das chamadas await.
    console.error(erro);
  }
}

buscarDadosComAsyncAwait();
```

A versão com `async/await` é indiscutivelmente mais fácil de ler. A lógica flui de cima para baixo, sem callbacks aninhados, e a atribuição de resultados a variáveis é direta. A indentação é menor e a complexidade cognitiva é reduzida.

### `await` e a Precedência de Operadores

O operador `await` tem uma precedência alta, o que significa que ele opera sobre a expressão imediatamente à sua direita. Geralmente, o comportamento é o que se espera.

```js
// Await opera sobre a chamada de 'fetch' antes da chamada de '.json()'
const resposta = await fetch('url/api').then(res => res.json());

// É comum ver parênteses para maior clareza, embora nem sempre necessários.
const dados = await (await fetch('url/api')).json();
```

A melhor prática, para máxima legibilidade, é separar as operações em linhas:

```js
const resposta = await fetch('url/api');
const dados = await resposta.json();
```

## Tratamento de Erros com `try...catch`

Uma das maiores vitórias do `async/await` é a unificação do tratamento de erros. Em vez de encadear múltiplos `.catch()`, podemos usar o familiar bloco `try...catch` para lidar tanto com erros síncronos quanto com erros de promises rejeitadas.

```js
async function buscarEProcessar() {
  try {
    // Código que pode lançar erros síncronos ou assíncronos
    const id = obterIdDoInput(); // Pode lançar um erro síncrono se o input for inválido
    const usuario = await buscarUsuario(id); // Pode rejeitar uma promise
    
    console.log("Processamento bem-sucedido.");
    return usuario;
  } catch (erro) {
    // Um único bloco para capturar todos os tipos de erro.
    console.error("Falha no processo:", erro.message);
    // Lógica para lidar com o erro, como exibir uma mensagem na UI.
  } finally {
    console.log("Executando limpeza final.");
  }
}
```

## Lidando com Operações Concorrentes (Paralelas)

Um erro comum de iniciantes com `async/await` é encadear chamadas que poderiam ser executadas em paralelo, tornando o código desnecessariamente lento.

**Padrão Incorreto (Sequencial):**

```js
async function carregarDashboardLento() {
  console.time('dashboard');
  // Espera a primeira requisição terminar ANTES de iniciar a segunda.
  const perfil = await buscarPerfil();
  const notificacoes = await buscarNotificacoes();
  
  //... exibe dados
  console.timeEnd('dashboard'); // Ex: ~4 segundos
}
```

Se `buscarPerfil` e `buscarNotificacoes` são independentes, não há motivo para esperar pela primeira para iniciar a segunda.

**Padrão Correto (Paralelo) com `Promise.all`:**

A solução é iniciar todas as operações assíncronas ao mesmo tempo e depois usar `Promise.all` para esperar que todas elas terminem.

```js
async function carregarDashboardRapido() {
  console.time('dashboard');
  // Inicia ambas as promises ao mesmo tempo
  const promessaPerfil = buscarPerfil();
  const promessaNotificacoes = buscarNotificacoes();

  // Espera que AMBAS terminem, em paralelo.
  const [perfil, notificacoes] = await Promise.all([promessaPerfil, promessaNotificacoes]);

  //... exibe dados
  console.timeEnd('dashboard'); // Ex: ~2 segundos (o tempo da mais longa)
}
```

Este padrão é crucial para a performance de aplicações que dependem de múltiplas fontes de dados.

## `async/await` em Laços de Repetição

A forma como `async/await` interage com laços de repetição depende do tipo de laço.

### `forEach` não funciona com `await`

O método `Array.prototype.forEach` **não** é "promise-aware". Ele não vai "pausar" a cada iteração. Ele simplesmente invocará o callback para cada item e seguirá em frente, sem esperar pelas operações `await` dentro do callback.

```js
const ids = [1, 2, 3];

// ERRADO: O laço não espera.
ids.forEach(async (id) => {
  const resultado = await algumaOperacaoAssincrona(id);
  console.log(resultado);
});
console.log("Este log aparecerá antes de qualquer resultado do laço.");
```

### Laços `for`, `for...of`: A Escolha para Execução Sequencial

Para executar operações assíncronas em um array de forma **sequencial** (uma após a outra), os laços tradicionais como `for` e, preferencialmente, `for...of` funcionam perfeitamente com `await`.

```js
async function processarIdsEmSequencia(ids) {
  console.log("Iniciando processamento sequencial...");
  for (const id of ids) {
    const resultado = await algumaOperacaoAssincrona(id); // O laço PAUSA aqui
    console.log(`Processado id ${id} com resultado: ${resultado}`);
  }
  console.log("Processamento sequencial concluído.");
}
```

### Execução Paralela com `map` + `Promise.all`

Se a ordem não importa e você quer a máxima performance, o padrão é usar `Array.prototype.map` para transformar seu array de dados em um array de promises e, em seguida, usar `Promise.all` para esperar por todas elas.

```js
async function processarIdsEmParalelo(ids) {
  console.log("Iniciando processamento paralelo...");
  const arrayDePromises = ids.map(id => algumaOperacaoAssincrona(id));
  const resultados = await Promise.all(arrayDePromises);
  console.log("Resultados:", resultados);
  console.log("Processamento paralelo concluído.");
}
```

## Iteradores Assíncronos e `for await...of`

O ES2018 introduziu o conceito de **iteradores assíncronos** e a sintaxe `for await...of` para consumi-los. Enquanto um laço `for...of` com `await` dentro é sobre executar **ações** assíncronas em uma coleção **síncrona**, o `for await...of` é sobre consumir uma fonte de dados que é, ela mesma, **assíncrona**.

Pense em um stream de dados vindo de uma requisição de rede ou de um arquivo grande. Os "pedaços" (chunks) de dados não chegam todos de uma vez. Um iterador assíncrono permite que você pegue um chunk, espere pelo próximo, e assim por diante.

**Sintaxe:**

```js
async function consumirStream() {
  // 'streamDeDados' é um iterável assíncrono
  for await (const chunk of streamDeDados) {
    console.log("Recebido um novo chunk:", chunk);
  }
  console.log("Stream concluído.");
}
```

Este é um padrão mais avançado, mas essencial para lidar com grandes volumes de dados de forma eficiente em ambientes como o Node.js.

## Considerações Finais

Neste capítulo, desvendamos a sintaxe `async/await`, a culminação da evolução da programação assíncrona em JavaScript. Vimos que, longe de ser uma tecnologia separada, ela é uma camada de abstração incrivelmente poderosa e intuitiva construída sobre o robusto sistema de **Promises**.

Aprendemos como a palavra-chave `async` transforma uma função, garantindo que ela retorne uma promise, e como o operador `await` nos permite "pausar" a execução de forma limpa, esperando por resultados assíncronos sem bloquear a thread principal. Essa combinação nos permitiu escrever código que tem a clareza e a linearidade do código síncrono, mesmo ao orquestrar operações complexas.

Dominamos o tratamento de erros com o familiar `try...catch`, e aprendemos os padrões corretos para lidar com operações paralelas e sequenciais dentro de laços. `async/await` não tornou as Promises obsoletas; pelo contrário, tornou-as mais fáceis de usar e mais poderosas. Compreender ambos os conceitos é o que lhe permitirá navegar por qualquer desafio assíncrono que encontrar, escrevendo código que não é apenas funcional, mas também limpo, legível e de fácil manutenção — a marca de um desenvolvedor JavaScript moderno e proficiente.
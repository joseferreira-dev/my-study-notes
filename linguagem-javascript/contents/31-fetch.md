# Capítulo 31 – Fetch: A API Moderna para Requisições de Rede

No capítulo anterior, viajamos no tempo para entender a tecnologia que tornou a web dinâmica possível: o AJAX, através do objeto `XMLHttpRequest` (XHR). Vimos como o XHR nos permitiu, pela primeira vez, fazer requisições a um servidor em segundo plano, mas também notamos sua sintaxe verbosa e seu modelo de eventos, que, embora funcional, pode levar a um código complexo (o "Callback Hell"). O mundo da programação assíncrona clamava por uma solução mais limpa, poderosa e alinhada com as novas funcionalidades da linguagem, como as Promises.

A resposta a esse chamado foi a **API Fetch**, introduzida nos navegadores como um novo padrão para fazer requisições de rede. `Fetch` não é apenas uma versão melhorada do XHR; é uma reimaginação completa da comunicação cliente-servidor, construída sobre dois pilares do JavaScript moderno: **Promises** e os objetos **`Request`** e **`Response`**. Ela fornece uma interface mais simples e lógica para buscar recursos pela rede de forma assíncrona, eliminando a complexidade do monitoramento de `readyState` e dos callbacks aninhados.

Neste capítulo, vamos dominar a API `Fetch`, a ferramenta padrão para todas as requisições de rede em aplicações JavaScript modernas. Começaremos por entender a função global `fetch()` e como sua natureza baseada em Promises transforma a maneira como lidamos com os resultados. Dissecaremos a anatomia dos objetos `Request` e `Response`, que nos dão um controle granular sem precedentes sobre as requisições e suas respostas. Aprenderemos a fazer requisições `GET` e `POST`, a enviar dados JSON, a configurar cabeçalhos customizados e a gerenciar cookies e o cache. Um foco especial será dado ao tratamento de erros, uma área onde o comportamento do `Fetch` difere sutilmente, mas de forma crucial, do XHR. Ao final, você estará equipado para fazer qualquer tipo de comunicação de rede de forma limpa, robusta e eficiente.

## O Básico: A Função `fetch()`

A API `Fetch` está disponível globalmente no escopo `window` e `worker` (um conceito que chamamos de `GlobalFetch`). Isso significa que podemos usar a função `fetch()` diretamente em nosso código. Sua principal característica é que ela sempre retorna uma **`Promise`**. Esta promise não resolve com os dados finais, mas sim com um objeto `Response` que representa a resposta HTTP completa.

**Sintaxe Básica:** `fetch(resource, options)`

- `resource`: O recurso que você deseja buscar. Geralmente, uma URL em formato de string.
- `options` (opcional): Um objeto de configuração para customizar a requisição (ex: método, cabeçalhos, corpo, etc.). Se omitido, uma requisição `GET` simples é feita.

**Exemplo: Uma Requisição `GET` Simples**

```js
console.log("Iniciando a busca de dados...");

fetch('https://api.github.com/users/github')
  .then(response => {
    // A promise resolve com o objeto Response.
    // O próximo passo é extrair os dados do corpo da resposta.
    console.log("Resposta recebida, extraindo JSON...");
    return response.json(); // .json() também retorna uma promise!
  })
  .then(data => {
    // Esta promise resolve com os dados JSON finais.
    console.log("Dados prontos para uso:");
    console.log(data);
  })
  .catch(error => {
    // Este catch lida com erros de rede ou falhas no parsing.
    console.error("Falha na requisição:", error);
  });

console.log("A requisição foi enviada, o código continua...");
```

Note o encadeamento de `.then()`: o primeiro lida com o objeto `Response` inicial, e o segundo lida com os dados do corpo, já processados.

## A Anatomia da Resposta: O Objeto `Response`

O objeto `Response` é uma representação poderosa de toda a resposta HTTP. Ele contém não apenas o corpo, mas também o status, os cabeçalhos e outras metainformações.

**Propriedades Importantes:**

- `status`: O código de status HTTP (ex: `200`, `404`).
- `statusText`: A mensagem de status correspondente (ex: `"OK"`, `"Not Found"`).
- `ok`: Uma propriedade booleana muito útil. É `true` se o status da resposta for um sucesso (na faixa de 200 a 299) e `false` caso contrário.
- `headers`: Um objeto `Headers` com os cabeçalhos da resposta.
- `url`: A URL final da resposta (útil para rastrear redirecionamentos).

**Métodos para Ler o Corpo da Resposta:** O corpo de uma resposta (`body`) é um **stream** de dados. Para lê-lo, você precisa usar um dos seguintes métodos, que leem o stream até o fim e retornam uma promise que resolve com o conteúdo processado:

- `response.json()`: Converte o corpo em um objeto JavaScript a partir de uma string JSON.
- `response.text()`: Retorna o corpo como uma string de texto.
- `response.blob()`: Retorna o corpo como um objeto `Blob` (útil para imagens, arquivos).
- `response.formData()`: Retorna o corpo como um objeto `FormData`.
- `response.arrayBuffer()`: Retorna o corpo como um `ArrayBuffer` (útil para dados binários).

**Importante:** O corpo de uma resposta só pode ser lido **uma vez**. Depois de chamar `response.json()`, por exemplo, o stream foi consumido e você não pode chamar `response.text()` no mesmo objeto `Response`.

## Tratamento de Erros: A "Pegadinha" do `Fetch`

Esta é a diferença mais crucial em relação ao XHR e a muitas outras bibliotecas. A promise retornada por `fetch()` **não** é rejeitada em caso de um erro de status HTTP (como 404 ou 500). Ela só é rejeitada se ocorrer uma falha de rede que impeça a requisição de ser completada (ex: sem conexão com a internet, erro de DNS).

Isso significa que o bloco `.catch()` **não** será acionado por um erro 404. O código continuará para o primeiro `.then()`, e é nossa responsabilidade verificar se a resposta foi bem-sucedida.

**Padrão Correto para Tratamento de Erros:**

```js
fetch('https://api.example.com/recurso-que-nao-existe')
  .then(response => {
    console.log(`Status da resposta: ${response.status}`);
    
    // Verificamos a propriedade 'ok' para saber se a requisição foi um sucesso.
    if (!response.ok) {
      // Se não foi, lançamos um erro para pular para o bloco .catch().
      throw new Error(`Erro HTTP! Status: ${response.status}`);
    }
    
    return response.json();
  })
  .then(data => {
    console.log("Sucesso:", data);
  })
  .catch(error => {
    console.error("Ocorreu um problema com a sua requisição fetch:", error.message);
  });
```

Sempre verifique a propriedade `response.ok` e lance um erro manualmente se quiser tratar erros de status HTTP no seu bloco `.catch()`.

## Customizando Requisições com o Objeto `options`

O segundo argumento de `fetch()` é um objeto `options` que nos dá controle total sobre a requisição.

**`options` comuns:**

- `method`: O método HTTP. Padrão é `'GET'`. Pode ser `'POST'`, `'PUT'`, `'DELETE'`, etc.
- `headers`: Um objeto para definir os cabeçalhos da requisição.
- `body`: O corpo da requisição.
- `credentials`: Controla como os cookies são enviados.
- `cache`: Controla como a requisição interage com o cache do navegador.

### Enviando Dados com `POST`

Para enviar dados, definimos o `method` para `'POST'`, preparamos o `body` (geralmente serializando um objeto para JSON) e configuramos o cabeçalho `Content-Type`.

```js
const novoPost = {
  title: 'Meu Novo Post',
  body: 'Este é o conteúdo do post.',
  userId: 1
};

fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  headers: {
    // Informa ao servidor que estamos enviando JSON
    'Content-Type': 'application/json; charset=UTF-8',
    // Outros cabeçalhos, como de autenticação
    'Authorization': 'Bearer seu-token-aqui'
  },
  // Converte o objeto JavaScript em uma string JSON
  body: JSON.stringify(novoPost)
})
.then(response => {
  if (!response.ok) {
    throw new Error('Falha ao criar o post.');
  }
  return response.json();
})
.then(postCriado => {
  console.log('Post criado com sucesso:', postCriado);
})
.catch(error => console.error(error));
```

### Gerenciando Cookies e Credenciais

A opção `credentials` controla se o navegador envia cookies e outros cabeçalhos de autenticação com a requisição.

- `'omit'` (padrão antigo, em alguns casos): Nunca envia cookies.
- `'same-origin'` (padrão moderno): Envia cookies se a URL da requisição estiver na mesma origem da página.
- `'include'`: Envia cookies mesmo para requisições de origem cruzada (cross-origin). É necessário para muitas APIs que usam autenticação baseada em sessão.

```js
fetch('https://api.meusite.com/dados-privados', {
  credentials: 'include' // Garante que os cookies de autenticação sejam enviados
});
```

Isso está diretamente ligado à política de CORS (Cross-Origin Resource Sharing) do servidor, que precisa permitir o envio de credenciais da sua origem.

### Exemplo Completo: Buscando no Stack Overflow com `Fetch`

Vamos refazer nosso exemplo do capítulo anterior para comparar a clareza do `Fetch`.

```js
async function buscarPerguntasStackOverflow() {
  const rootUrl = 'https://api.stackexchange.com/2.3/questions';
  const params = new URLSearchParams({
    order: 'desc',
    sort: 'votes',
    tagged: 'javascript',
    site: 'stackoverflow'
  });
  
  const url = `${rootUrl}?${params.toString()}`;
  
  try {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`Erro na API do Stack Exchange! Status: ${response.status}`);
    }
    
    const data = await response.json();
    
    console.log("Top 5 Perguntas sobre JavaScript (com Fetch):");
    data.items.slice(0, 5).forEach(pergunta => {
      console.log(`- ${pergunta.title} (Votos: ${pergunta.score})`);
    });
    
  } catch (error) {
    console.error("Falha ao buscar perguntas:", error.message);
  }
}

buscarPerguntasStackOverflow();
```

O uso de `async/await` torna o código `Fetch` ainda mais legível, parecendo quase síncrono.

## Considerações Finais

Neste capítulo, dominamos a API `Fetch`, a interface moderna e padrão para realizar requisições de rede em JavaScript. Deixamos para trás a complexidade do `XMLHttpRequest` e abraçamos um modelo mais poderoso e limpo, totalmente integrado com as **Promises**.

Vimos como a função `fetch()` nos retorna uma promise que resolve com um objeto `Response`, nos dando acesso não apenas ao corpo, mas a todos os metadados da resposta HTTP. O ponto mais crucial foi entender o seu modelo de tratamento de erros: a promise de `fetch()` só é rejeitada em caso de falha de rede, e é nossa responsabilidade verificar a propriedade `response.ok` para tratar erros de status HTTP.

Exploramos o objeto `options` para customizar nossas requisições, aprendendo a enviar dados com `POST`, a configurar cabeçalhos e a gerenciar o envio de credenciais. A combinação do `Fetch` com a sintaxe `async/await` se revelou a maneira definitiva de escrever código assíncrono de rede — um código que é, ao mesmo tempo, poderoso, flexível e incrivelmente legível. Com o `Fetch` em seu arsenal, você está preparado para se comunicar com qualquer API na web, construindo a espinha dorsal de qualquer aplicação moderna e conectada.
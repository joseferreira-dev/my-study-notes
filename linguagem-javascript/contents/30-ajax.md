# Capítulo 30 – AJAX: Comunicação Assíncrona com o Servidor

Até agora, nossa jornada nos levou a dominar a criação de interfaces e lógicas complexas que rodam inteiramente no navegador do usuário. Construímos aplicações que respondem a eventos, manipulam dados e atualizam a interface do usuário. No entanto, a grande maioria das aplicações web não vive em uma ilha; elas precisam se comunicar com um mundo exterior, principalmente com servidores, para buscar informações atualizadas, salvar dados do usuário, validar formulários e muito mais. Historicamente, essa comunicação era sinônimo de uma experiência de usuário truncada: cada interação com o servidor exigia um recarregamento completo da página, resultando em uma tela em branco e uma perda de estado.

Essa realidade mudou drasticamente no início dos anos 2000 com a popularização de uma técnica revolucionária chamada **AJAX (Asynchronous JavaScript and XML)**. O AJAX não é uma tecnologia, mas sim uma **abordagem** que utiliza um conjunto de tecnologias existentes — principalmente o objeto `XMLHttpRequest` do navegador — para permitir que uma página web se comunique com um servidor em segundo plano, **sem a necessidade de recarregar a página inteira**. Essa capacidade de buscar e enviar dados de forma assíncrona permitiu a criação das aplicações web dinâmicas (Single Page Applications, ou SPAs) que conhecemos hoje, como Gmail, Google Maps e Facebook, onde o conteúdo é atualizado de forma fluida e instantânea.

Neste capítulo, vamos mergulhar fundo na tecnologia que tornou tudo isso possível: o objeto `XMLHttpRequest` (XHR). Embora APIs mais modernas como a `Fetch` (que é baseada em Promises) sejam hoje a escolha preferencial para novos projetos, entender o XHR é fundamental. Ele não apenas é onipresente em milhões de linhas de código legado e em muitas bibliotecas, mas também revela os mecanismos de baixo nível da comunicação cliente-servidor de uma forma que as abstrações mais novas escondem. Começaremos por dissecar o ciclo de vida de uma requisição AJAX. Aprenderemos a fazer diferentes tipos de requisições (`GET`, `POST`, `HEAD`), a enviar e receber dados no formato JSON, a gerenciar o estado da requisição e a lidar com sucessos e falhas. Também abordaremos aspectos de experiência do usuário, como a implementação de indicadores de carregamento (preloaders), e exploraremos tópicos avançados como o monitoramento global de eventos AJAX.

## O que é AJAX?

AJAX é um acrônimo para **Asynchronous JavaScript and XML**. Vamos quebrar esse nome:

- **Asynchronous (Assíncrono):** Esta é a parte mais importante. Significa que, ao fazer uma requisição para o servidor, seu código JavaScript não para e espera pela resposta. Ele dispara a requisição e continua executando outras tarefas. Quando a resposta do servidor chega, uma função de callback que você definiu é chamada para processá-la. Isso mantém a interface do usuário responsiva.
- **JavaScript:** A linguagem que orquestra todo o processo, criando a requisição, enviando-a e processando a resposta.
- **and XML (e XML):** Originalmente, o XML era o formato de dados preferido para a troca de informações. Hoje, ele foi quase que inteiramente substituído pelo **JSON** por ser mais leve, mais fácil de se trabalhar em JavaScript e mais conciso. Embora o nome "XML" permaneça no acrônimo, hoje em dia AJAX quase sempre significa "Asynchronous JavaScript and JSON".

## O Objeto `XMLHttpRequest` (XHR)

O coração do AJAX clássico é o objeto `XMLHttpRequest`. Ele é uma API nativa dos navegadores que nos fornece um objeto capaz de fazer requisições HTTP para um servidor.

Para iniciar o processo, primeiro criamos uma instância do objeto:

```js
const xhr = new XMLHttpRequest();
```

Este objeto `xhr` agora possui métodos e propriedades que nos permitirão configurar e enviar uma requisição, além de ouvir os eventos de seu ciclo de vida.

### O Ciclo de Vida de uma Requisição AJAX

Uma requisição XHR passa por vários estados, desde sua criação até a conclusão. Podemos monitorar esses estados através da propriedade `readyState`.

- `readyState = 0` (UNSENT): O cliente foi criado. `open()` ainda não foi chamado.
- `readyState = 1` (OPENED): `open()` foi chamado. A requisição foi configurada.
- `readyState = 2` (HEADERS_RECEIVED): `send()` foi chamado, e os cabeçalhos e o status da resposta estão disponíveis.
- `readyState = 3` (LOADING): Baixando a resposta; `responseText` contém dados parciais.
- `readyState = 4` (DONE): A operação foi concluída. A resposta completa foi recebida.

Para monitorar essas mudanças, usamos o evento `onreadystatechange`.

```js
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    // A requisição terminou. Agora precisamos verificar se foi bem-sucedida.
    // ...
  }
};
```

Dentro do estado `DONE`, precisamos verificar o código de status HTTP da resposta, disponível na propriedade `status`.

- `status = 200`: "OK". A requisição foi bem-sucedida.
- `status = 404`: "Not Found". O recurso solicitado não foi encontrado.
- `status = 500`: "Internal Server Error". Ocorreu um erro no servidor.
- E muitos outros códigos de status HTTP.

A combinação `readyState === 4` e `status === 200` indica que a requisição foi concluída com sucesso.

### Configurando e Enviando a Requisição

- **`xhr.open(method, url, [async])`:** Configura a requisição.
    - `method`: O método HTTP a ser usado (`'GET'`, `'POST'`, `'PUT'`, `'DELETE'`, `'HEAD'`, etc.).
    - `url`: O endereço do recurso no servidor.
    - `async` (opcional): Um booleano que indica se a requisição deve ser assíncrona. O padrão é `true`, e **quase nunca** deve ser definido como `false`, pois isso congelaria a interface do usuário.
- **`xhr.send([body])`:** Envia a requisição.
    - `body`: O corpo da requisição, usado principalmente com métodos como `POST` e `PUT` para enviar dados ao servidor. Para requisições `GET`, este parâmetro é geralmente omitido ou `null`.

## Exemplos Práticos

### Usando `GET` sem Parâmetros

Vamos fazer uma requisição simples para buscar um arquivo de texto.

```js
const xhr = new XMLHttpRequest();

xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) { // Requisição concluída
    if (xhr.status === 200) { // Sucesso
      // xhr.responseText contém a resposta do servidor como uma string
      console.log("Resposta recebida:", xhr.responseText);
    } else {
      console.error("Erro na requisição. Status:", xhr.status);
    }
  }
};

// Configura uma requisição GET para o arquivo 'dados.txt'
xhr.open('GET', 'dados.txt');

// Envia a requisição
xhr.send();
```

### Usando `GET` com Parâmetros e Consumindo uma API Real

Vamos buscar as perguntas sobre JavaScript mais votadas do último mês na API do Stack Overflow. Para isso, precisamos passar parâmetros na URL (uma "query string").

```js
const rootUrl = 'https://api.stackexchange.com/2.3/questions';
const params = new URLSearchParams({
  order: 'desc',
  sort: 'votes',
  tagged: 'javascript',
  site: 'stackoverflow'
});

const urlCompleta = `${rootUrl}?${params.toString()}`;

const xhrStack = new XMLHttpRequest();

xhrStack.onreadystatechange = function() {
  if (xhrStack.readyState === 4 && xhrStack.status === 200) {
    // A resposta é uma string JSON. Precisamos fazer o parse.
    const dados = JSON.parse(xhrStack.responseText);
    
    console.log("Top 5 Perguntas sobre JavaScript:");
    dados.items.slice(0, 5).forEach(pergunta => {
      console.log(`- ${pergunta.title} (Votos: ${pergunta.score})`);
    });
  } else if (xhrStack.readyState === 4) {
    console.error("Erro ao buscar dados do Stack Overflow. Status:", xhrStack.status);
  }
};

xhrStack.open('GET', urlCompleta);
xhrStack.send();
```

### Adicionando um Preloader (Indicador de Carregamento)

A experiência do usuário melhora drasticamente se fornecermos feedback visual durante uma operação assíncrona.

```js
// Suponha que temos <div id="loader" style="display: none;">Carregando...</div> no HTML
const loader = document.getElementById('loader');

function buscarDadosComLoader() {
  const xhr = new XMLHttpRequest();
  
  // Exibe o loader antes de enviar a requisição
  loader.style.display = 'block';

  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4) {
      // Esconde o loader quando a requisição termina (sucesso ou falha)
      loader.style.display = 'none';

      if (xhr.status === 200) {
        document.getElementById('conteudo').innerText = xhr.responseText;
      } else {
        document.getElementById('conteudo').innerText = "Falha ao carregar dados.";
      }
    }
  };

  xhr.open('GET', 'dados.txt');
  xhr.send();
}
```

### Enviando e Recebendo Dados JSON via `POST`

Para enviar dados (como um formulário) para um servidor, usamos o método `POST`. Precisamos configurar o cabeçalho `Content-Type` para informar ao servidor que estamos enviando JSON e serializar nosso objeto com `JSON.stringify`.

```js
const novoUsuario = {
  nome: "Maria Souza",
  email: "maria.souza@example.com"
};

const xhrPost = new XMLHttpRequest();

xhrPost.onreadystatechange = function() {
  if (xhrPost.readyState === 4 && xhrPost.status === 201) { // 201 Created é um status comum para POST bem-sucedido
    const resposta = JSON.parse(xhrPost.responseText);
    console.log("Usuário criado com sucesso:", resposta);
  } else if (xhrPost.readyState === 4) {
    console.error("Erro ao criar usuário. Status:", xhrPost.status);
  }
};

xhrPost.open('POST', 'https://api.example.com/usuarios');

// 1. Define o cabeçalho para indicar que o corpo é JSON
xhrPost.setRequestHeader('Content-Type', 'application/json;charset=UTF-8');

// 2. Converte o objeto JavaScript em uma string JSON e envia no corpo
xhrPost.send(JSON.stringify(novoUsuario));
```

### Checando se um Arquivo Existe via `HEAD`

O método HTTP `HEAD` é idêntico ao `GET`, mas o servidor **não** envia o corpo da resposta. Ele é extremamente útil e eficiente para verificar se um recurso existe ou para obter seus metadados (como `Content-Length` ou `Last-Modified`) sem precisar baixar o arquivo inteiro.

```js
function verificarArquivo(url) {
  const xhrHead = new XMLHttpRequest();
  xhrHead.onreadystatechange = function() {
    if (this.readyState === 4) {
      if (this.status === 200) {
        console.log(`O arquivo em '${url}' existe.`);
        // Podemos também inspecionar cabeçalhos:
        console.log('Tipo de Conteúdo:', this.getResponseHeader('Content-Type'));
      } else {
        console.log(`O arquivo em '${url}' não foi encontrado ou ocorreu um erro (Status: ${this.status}).`);
      }
    }
  };
  xhrHead.open('HEAD', url);
  xhrHead.send();
}

verificarArquivo('/imagens/logo.png');
verificarArquivo('/imagens/nao-existe.png');
```

## Tópicos Avançados

### Monitorando Eventos AJAX Globalmente

Em aplicações complexas, pode ser útil ter um local central para monitorar todas as requisições AJAX — por exemplo, para mostrar um indicador de carregamento global, para registrar logs ou para lidar com erros de autenticação (como um status 401) de forma consistente.

Uma técnica avançada para isso é "envelopar" os métodos `open` e `send` do `XMLHttpRequest.prototype`.

```js
(function() {
  const originalOpen = XMLHttpRequest.prototype.open;
  const originalSend = XMLHttpRequest.prototype.send;
  let contadorRequisicoesAtivas = 0;
  const loaderGlobal = document.getElementById('loader-global'); // Suponha que exista

  XMLHttpRequest.prototype.open = function(...args) {
    this.addEventListener('loadstart', () => {
      contadorRequisicoesAtivas++;
      loaderGlobal.style.display = 'block';
    });
    this.addEventListener('loadend', () => {
      contadorRequisicoesAtivas--;
      if (contadorRequisicoesAtivas === 0) {
        loaderGlobal.style.display = 'none';
      }
    });
    // Chama o método original
    return originalOpen.apply(this, args);
  };
})();
```

Este padrão intercepta todas as chamadas AJAX, permitindo a implementação de uma lógica global. Deve ser usado com cuidado, mas é extremamente poderoso para frameworks e aplicações de grande escala.

## Considerações Finais

Neste capítulo, desvendamos o AJAX e a tecnologia que o tornou possível: o objeto `XMLHttpRequest`. Vimos que, através da comunicação assíncrona, fomos capazes de criar experiências web ricas e fluidas, quebrando o ciclo de recarregamento de página que definia a web primitiva.

Exploramos o ciclo de vida completo de uma requisição XHR, desde sua criação e configuração com `.open()`, passando pelo envio de dados com `.send()`, até o monitoramento de seu estado com `onreadystatechange` e o tratamento da resposta com base no `status` e no `responseText`. Colocamos esse conhecimento em prática com exemplos reais, aprendendo a buscar dados com `GET`, enviar dados com `POST`, e até mesmo a usar o eficiente método `HEAD`.

Embora a API `Fetch` e a sintaxe `async/await` sejam as ferramentas modernas de escolha, a compreensão do `XMLHttpRequest` é mais do que um exercício histórico. Ela nos dá uma visão profunda sobre os fundamentos da comunicação cliente-servidor, sobre a gestão de estados e sobre o tratamento de eventos assíncronos que são conceitos universais, independentemente da API que você use.
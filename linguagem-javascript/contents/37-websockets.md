# Capítulo 37 – WebSockets: Comunicação em Tempo Real

Nos capítulos anteriores, dominamos a arte da comunicação assíncrona usando o modelo de requisição-resposta do HTTP, através do AJAX clássico e da moderna API Fetch. Este modelo é a espinha dorsal da web: o cliente pede, o servidor responde. Ele é perfeito para buscar dados de uma página, enviar um formulário ou carregar um artigo. No entanto, este paradigma tem uma limitação fundamental: a comunicação é sempre iniciada pelo cliente. O servidor não pode "empurrar" dados para o cliente por conta própria; ele só pode responder a uma requisição. Para criar aplicações que precisam de atualizações **em tempo real** — como chats, jogos online, feeds de notícias ao vivo ou painéis financeiros — o modelo de requisição-resposta se torna ineficiente, exigindo que o cliente pergunte repetidamente ao servidor ("polling"): "Já tem algo novo para mim?".

Para quebrar essa barreira e inaugurar uma nova era de interatividade na web, foi criado o **Protocolo WebSocket**. O WebSocket não é uma melhoria do HTTP; é um protocolo de comunicação completamente diferente, projetado desde o início para um propósito específico: estabelecer um **canal de comunicação bidirecional e persistente** entre um cliente e um servidor sobre uma única conexão TCP. Uma vez que a conexão é estabelecida, ela permanece aberta, permitindo que tanto o cliente quanto o servidor enviem dados um para o outro a qualquer momento, com latência mínima. Isso elimina a necessidade de polling e reduz drasticamente o overhead, já que não há cabeçalhos HTTP repetitivos em cada mensagem.

Neste capítulo, vamos mergulhar fundo na API WebSocket do JavaScript, a interface que nos permite aproveitar esse poderoso protocolo. Começaremos por entender o "handshake" inicial que atualiza uma conexão HTTP para uma conexão WebSocket. Dissecaremos o ciclo de vida de uma conexão, aprendendo a reagir aos eventos de abertura, recebimento de mensagens, erros e fechamento. Dominaremos o envio e o recebimento de dados, tanto no formato de texto (usando JSON) quanto em formato binário. Finalmente, abordaremos tópicos essenciais para a construção de aplicações robustas, como a criação de conexões seguras e as melhores práticas para manter a conexão ativa e lidar com desconexões.

## O que é o WebSocket? Do HTTP ao Tempo Real

Uma conexão WebSocket começa sua vida como uma requisição HTTP normal. Esse processo inicial é chamado de **handshake (aperto de mão)**.

1. **A Requisição de "Upgrade" do Cliente:** O cliente (navegador) envia uma requisição HTTP GET padrão para o servidor, mas com alguns cabeçalhos especiais que sinalizam sua intenção de mudar de protocolo. Os mais importantes são:
    - `Upgrade: websocket`
    - `Connection: Upgrade`
2. **A Resposta do Servidor:** Se o servidor entender e aceitar a requisição WebSocket, ele responde com um código de status especial: **`101 Switching Protocols`**. Esta resposta também contém cabeçalhos específicos que confirmam o upgrade.

A partir deste momento, a conexão HTTP deixa de existir. O mesmo canal TCP subjacente é agora uma conexão WebSocket persistente e **full-duplex**, o que significa que os dados podem fluir em ambas as direções simultaneamente.

A URL de um endpoint WebSocket também é diferente. Em vez de `http://`, usamos `ws://`. Para conexões seguras, em vez de `https://`, usamos `wss://`.

- **`ws://meusite.com/chat`** (WebSocket Não Seguro)
- **`wss://meusite.com/chat`** (WebSocket Seguro)

Assim como com o HTTPS, você **sempre** deve usar `wss://` em produção para garantir que os dados trocados sejam criptografados.

## Estabelecendo uma Conexão WebSocket em JavaScript

A interface para WebSockets no JavaScript é o objeto `WebSocket`. Para iniciar uma conexão, basta criar uma nova instância dele.

**Sintaxe:** `new WebSocket(url, [protocols])`

- `url`: A string da URL do servidor WebSocket (começando com `ws://` ou `wss://`).
- `protocols` (opcional): Uma string ou um array de strings que especificam os subprotocolos que o cliente aceita. Isso permite que um único servidor implemente diferentes protocolos de aplicação sobre o WebSocket, e o servidor selecionará um da lista para usar.

```js
// Estabelecendo uma conexão segura
const socket = new WebSocket('wss://api.example.com/meu-socket');
```

A criação deste objeto inicia imediatamente o processo de handshake em segundo plano. Para reagir ao resultado desse processo, precisamos ouvir os eventos do ciclo de vida do socket.

## O Ciclo de Vida: Ouvindo os Eventos do WebSocket

Um objeto `WebSocket` emite quatro eventos principais que definem seu ciclo de vida. Atribuímos funções (handlers) a esses eventos para executar nossa lógica.

### `onopen`: Conexão Estabelecida

Este evento é disparado uma única vez, quando o handshake é concluído com sucesso e a conexão está pronta para ser usada. É o local ideal para enviar uma mensagem inicial ou para habilitar elementos da interface do usuário.

```js
socket.onopen = function(event) {
  console.log("Conexão WebSocket estabelecida com sucesso!");
  // Agora é seguro enviar mensagens
  socket.send("Olá, servidor! Sou um novo cliente.");
};
```

### `onmessage`: Recebendo Mensagens

Este é o coração de uma aplicação WebSocket. O evento `onmessage` é disparado toda vez que o cliente recebe uma mensagem do servidor.

O objeto de evento passado para o handler possui uma propriedade `data`, que contém a carga útil da mensagem.

```js
socket.onmessage = function(event) {
  // event.data pode ser uma string, um Blob ou um ArrayBuffer
  const mensagemRecebida = event.data;
  console.log(`Mensagem recebida do servidor: ${mensagemRecebida}`);
  
  // Lógica para processar a mensagem, como adicionar a um chat
  adicionarMensagemAoChat(mensagemRecebida);
};
```

### `onerror`: Lidando com Falhas

Este evento é disparado se ocorrer um erro na comunicação que não resulte diretamente no fechamento da conexão (embora muitas vezes o fechamento ocorra logo em seguida).

```js
socket.onerror = function(event) {
  console.error("Ocorreu um erro no WebSocket:", event);
};
```

Infelizmente, o objeto de evento de erro geralmente não fornece informações muito detalhadas sobre a falha, por razões de segurança.

### `onclose`: Conexão Encerrada

Este evento é disparado quando a conexão é fechada, seja de forma limpa pelo cliente ou pelo servidor, ou devido a uma falha de rede.

O objeto de evento passado é um `CloseEvent` e contém informações úteis:

- `event.code`: Um código numérico que indica o motivo do fechamento.
- `event.reason`: Uma string descritiva opcional.
- `event.wasClean`: Um booleano que indica se o fechamento foi limpo.

```js
socket.onclose = function(event) {
  console.log("Conexão WebSocket fechada.");
  if (event.wasClean) {
    console.log(`Fechamento limpo. Código: ${event.code}, Motivo: ${event.reason}`);
  } else {
    // Ex: O servidor caiu ou a rede foi perdida
    console.error(`A conexão foi encerrada de forma inesperada. Código: ${event.code}`);
    // Aqui seria um bom lugar para implementar uma lógica de reconexão.
  }
};
```

## Enviando e Recebendo Dados

### `socket.send()`: Enviando Dados para o Servidor

Para enviar uma mensagem, usamos o método `send()`. Ele pode enviar dados em diferentes formatos.

```js
// Verificando se a conexão está aberta antes de enviar
if (socket.readyState === WebSocket.OPEN) {
  socket.send("Esta é uma mensagem de texto simples.");
}
```

A propriedade `readyState` do socket nos informa o estado atual da conexão (CONNECTING, OPEN, CLOSING, CLOSED).

### Trabalhando com Mensagens de Texto (JSON)

Na maioria das aplicações web, a forma mais comum de trocar dados estruturados é usando JSON. Para enviar um objeto, primeiro o serializamos com `JSON.stringify()`. Para receber, fazemos o parse da string recebida.

**Enviando JSON:**

```js
const mensagemObjeto = {
  tipo: 'chat',
  usuario: 'Ana',
  texto: 'Olá a todos!'
};

socket.send(JSON.stringify(mensagemObjeto));
```

**Recebendo e Processando JSON:**

```js
socket.onmessage = function(event) {
  try {
    const dados = JSON.parse(event.data);
    
    switch (dados.tipo) {
      case 'chat':
        exibirMensagemDeChat(dados.usuario, dados.texto);
        break;
      case 'notificacao':
        exibirNotificacao(dados.conteudo);
        break;
      // ... outros tipos de mensagem
    }
  } catch (error) {
    console.error("Recebida mensagem não-JSON:", event.data);
  }
};
```

### Trabalhando com Mensagens Binárias

WebSockets também são extremamente eficientes para a transmissão de dados binários, como em jogos online, streaming de áudio/vídeo ou upload de arquivos.

O JavaScript pode receber dados binários como um `Blob` ou um `ArrayBuffer`. Controlamos isso através da propriedade `binaryType` do socket.

```js
const socketBinario = new WebSocket('wss://api.example.com/binario');
socketBinario.binaryType = 'arraybuffer'; // ou 'blob'

socketBinario.onmessage = function(event) {
  if (event.data instanceof ArrayBuffer) {
    const view = new DataView(event.data);
    // Processa os bytes do ArrayBuffer
    console.log(`Recebido ArrayBuffer com ${view.byteLength} bytes.`);
  }
};
```

Para enviar dados binários, passamos um `ArrayBuffer`, `Blob` ou uma `TypedArray` diretamente para `socket.send()`.

## Tópicos Importantes e Boas Práticas

### Fechando a Conexão

Para fechar uma conexão de forma limpa do lado do cliente, usamos o método `close()`.

**Sintaxe:** `socket.close([code], [reason]);`

- `code` (opcional): Um código de status numérico. O código 1000 indica um fechamento normal.
- `reason` (opcional): Uma string explicando o motivo do fechamento.

```js
// Deslogando o usuário e fechando a conexão
socket.close(1000, "Usuário deslogou.");
```

### Mantendo a Conexão Viva: Heartbeats

Alguns proxies e firewalls podem fechar conexões que consideram "inativas" por muito tempo. Para evitar isso e também para detectar mais rapidamente uma conexão que "morreu" sem um evento `onclose` limpo, é uma prática comum implementar um sistema de **heartbeat (batimento cardíaco)**.

Isso geralmente envolve o cliente enviando uma pequena mensagem "ping" para o servidor em um intervalo regular (ex: a cada 30 segundos) e o servidor respondendo com uma mensagem "pong". Se o cliente não receber um "pong" após um certo tempo, ele pode assumir que a conexão foi perdida e tentar reconectar.

### Lógica de Reconexão Automática

Conexões de rede são inerentemente instáveis. Uma aplicação robusta não deve simplesmente desistir quando uma conexão WebSocket é fechada. No handler `onclose` (especialmente quando `event.wasClean` é `false`), você deve implementar uma lógica para tentar reconectar.

Uma prática recomendada é usar um atraso de **exponential backoff**: espere 1 segundo antes da primeira tentativa, 2 segundos antes da segunda, 4 antes da terceira, e assim por diante, até um limite máximo. Isso evita sobrecarregar um servidor que pode estar temporariamente indisponível.

## Considerações Finais

Neste capítulo, exploramos os **WebSockets**, a tecnologia que quebrou as barreiras do modelo de requisição-resposta do HTTP e inaugurou a era da comunicação verdadeiramente em tempo real na web. Vimos como uma conexão WebSocket estabelece um canal de comunicação **bidirecional e persistente** entre o cliente e o servidor, permitindo uma troca de dados instantânea e com baixa latência.

Dominamos a **API WebSocket** do JavaScript, aprendendo a estabelecer conexões, a ouvir os eventos de seu ciclo de vida — `onopen`, `onmessage`, `onerror` e `onclose` — e a enviar e receber dados tanto em formato de texto (JSON) quanto em formato binário. Mais importante, discutimos os padrões de design essenciais para construir aplicações robustas, como a implementação de lógicas de **heartbeat** e de **reconexão automática**.

Com o conhecimento sobre WebSockets, você está agora capacitado para construir a próxima geração de aplicações web interativas: chats, plataformas de colaboração em tempo real, jogos multiplayer, dashboards que se atualizam instantaneamente e muito mais. Você agora detém a chave para criar experiências que não apenas respondem ao usuário, mas que se sentem vivas e conectadas.
# Capítulo 39 – Geolocation API: Conectando o Digital ao Mundo Físico

Ao longo de nossa jornada pelo JavaScript, construímos aplicações cada vez mais inteligentes e interativas. Elas reagem a eventos, comunicam-se com servidores e se adaptam a diferentes culturas e idiomas. Agora, vamos dar um passo adiante e explorar uma das APIs que mais explicitamente conecta o mundo digital ao mundo físico: a **Geolocation API**. Esta API permite que uma aplicação web, com a permissão explícita do usuário, obtenha acesso à sua localização geográfica.

A capacidade de saber onde um usuário está abre um universo de possibilidades para criar experiências personalizadas e contextuais. Pense em um aplicativo de mapas que mostra a sua posição atual, um site de previsão do tempo que exibe o clima para a sua cidade, um serviço de delivery que lista os restaurantes mais próximos, ou uma aplicação de corrida que rastreia seu percurso. Todas essas funcionalidades, que hoje consideramos comuns, são alimentadas pela Geolocation API. Ela é a ponte que permite que nossos serviços digitais ofereçam utilidade no mundo físico.

Neste capítulo, faremos um mergulho profundo nesta poderosa API. Começaremos pelo seu pilar mais importante: a **privacidade e o consentimento do usuário**. Entenderemos por que a API é projetada para ser segura desde o início e como devemos abordar a solicitação de permissão. Em seguida, dominaremos os dois principais métodos para obter a localização: `getCurrentPosition()`, para uma leitura única, e `watchPosition()`, para receber atualizações contínuas à medida que o usuário se move. Dissecaremos os objetos de dados que a API nos retorna, aprendendo a extrair não apenas a latitude e a longitude, mas também informações sobre precisão, altitude e velocidade. Finalmente, abordaremos o tratamento robusto de erros e as melhores práticas para garantir que nossa aplicação seja, ao mesmo tempo, poderosa e respeitosa com a privacidade do usuário.

## O Pilar Fundamental: Privacidade e Consentimento

A localização de um usuário é uma das informações mais sensíveis que uma aplicação pode solicitar. Por essa razão, a Geolocation API foi projetada com a privacidade como sua principal prioridade. Existem duas regras inegociáveis que você deve sempre ter em mente:

1. **Permissão Explícita do Usuário:** Uma aplicação **nunca** pode obter a localização de um usuário sem seu consentimento explícito. A primeira vez que seu código tentar acessar a API de geolocalização, o navegador irá intervir e exibir um pop-up proeminente, perguntando ao usuário se ele permite que o site acesse sua localização. O usuário tem a opção de permitir, negar ou negar permanentemente para aquele site. Se a permissão for negada, a API falhará com um erro.
2. **Contexto Seguro (HTTPS):** Os navegadores modernos consideram a geolocalização uma "funcionalidade poderosa" e exigem que ela seja usada em um **contexto seguro**. Isso significa que sua página **deve** ser servida sobre **HTTPS**. Tentar usar a Geolocation API em uma página `http://` (exceto em `localhost` para desenvolvimento) resultará em um erro ou em uma falha silenciosa.

### Acessando a API e Verificando o Suporte

A API de geolocalização está disponível através do objeto `navigator.geolocation`. Antes de usar a API, é uma prática recomendada verificar se o navegador do usuário a suporta.

```js
if ("geolocation" in navigator) {
  console.log("A Geolocation API é suportada por este navegador.");
  // Podemos prosseguir com a chamada da API.
} else {
  console.log("A Geolocation API não é suportada. Considere fornecer uma alternativa, como um campo de busca de CEP.");
}
```

## Obtendo a Posição Atual do Usuário: `getCurrentPosition()`

Este é o método mais comum da API. Ele tenta obter a posição atual do dispositivo e, quando consegue, executa uma função de callback com os dados da localização.

**Sintaxe:** `navigator.geolocation.getCurrentPosition(successCallback, errorCallback, [options]);`

- **`successCallback`**: Uma função que é chamada quando a localização é obtida com sucesso. Ela recebe um objeto `GeolocationPosition` como seu único argumento.
- **`errorCallback`** (opcional): Uma função que é chamada se ocorrer um erro ao tentar obter a localização. Ela recebe um objeto `GeolocationPositionError` como argumento.
- **`options`** (opcional): Um objeto de configuração para ajustar o comportamento da busca.

**Exemplo Básico:**

```js
function sucessoNaLocalizacao(posicao) {
  console.log("Localização obtida com sucesso!");
  console.log(posicao);
}

function erroNaLocalizacao(erro) {
  console.error(`Erro ao obter a localização: ${erro.message}`);
}

// Solicitando a localização
navigator.geolocation.getCurrentPosition(sucessoNaLocalizacao, erroNaLocalizacao);
```

### Dissecando o Objeto de Sucesso (`GeolocationPosition`)

O objeto passado para o callback de sucesso contém duas propriedades principais:

- `coords`: Um objeto `GeolocationCoordinates` com os dados detalhados da posição.
- `timestamp`: Um `DOMTimeStamp` representando a hora em que a localização foi recuperada.

O objeto `coords` é onde estão as informações mais valiosas:

- **`latitude`**: A latitude em graus decimais.
- **`longitude`**: A longitude em graus decimais.
- **`accuracy`**: A precisão da localização em metros. Este valor é muito importante! Um valor baixo (ex: 10) significa uma localização precisa (provavelmente de um GPS). Um valor alto (ex: 2000) indica uma localização menos precisa (provavelmente baseada em redes Wi-Fi ou IP).
- `altitude`: A altitude em metros acima do elipsoide de referência (pode ser `null`).
- `altitudeAccuracy`: A precisão da altitude em metros (pode ser `null`).
- `heading`: A direção do movimento do dispositivo, em graus, em relação ao norte verdadeiro (0°). Varia de 0 a 360 (pode ser `null`).
- `speed`: A velocidade do dispositivo em metros por segundo (pode ser `null`).

**Exemplo Prático de Uso:**

```js
function sucesso(posicao) {
  const latitude = posicao.coords.latitude;
  const longitude = posicao.coords.longitude;
  const precisao = posicao.coords.accuracy;

  console.log(`Sua posição é: Lat: ${latitude}, Lon: ${longitude}`);
  console.log(`Esta leitura tem uma precisão de aproximadamente ${precisao} metros.`);

  // Criando um link para o Google Maps
  const linkMapa = `https://www.google.com/maps?q=${latitude},${longitude}`;
  console.log(`Veja no mapa: ${linkMapa}`);
}

navigator.geolocation.getCurrentPosition(sucesso);
```

### Lidando com Erros (`GeolocationPositionError`)

Um bom tratamento de erros é essencial. O objeto de erro passado para o segundo callback possui duas propriedades:

- `code`: Um número que identifica o tipo do erro.
- `message`: Uma descrição textual do erro.

Os códigos de erro são:

- **`1` (`PERMISSION_DENIED`):** O usuário negou a permissão. Este é o erro mais comum.
- **`2` (`POSITION_UNAVAILABLE`):** A rede está inativa ou o provedor de localização (ex: satélite GPS) não consegue ser alcançado.
- **`3` (`TIMEOUT`):** A requisição de localização expirou antes que uma posição pudesse ser obtida (controlado pela opção `timeout`).

**Exemplo de um Error Handler Robusto:**

```js
function erroHandler(erro) {
  let mensagemErro;
  switch(erro.code) {
    case erro.PERMISSION_DENIED:
      mensagemErro = "Permissão para geolocalização negada pelo usuário.";
      break;
    case erro.POSITION_UNAVAILABLE:
      mensagemErro = "Informação de localização indisponível.";
      break;
    case erro.TIMEOUT:
      mensagemErro = "A requisição de geolocalização expirou.";
      break;
    case erro.UNKNOWN_ERROR:
      mensagemErro = "Ocorreu um erro desconhecido.";
      break;
  }
  console.error(mensagemErro);
  // Exibir uma mensagem amigável para o usuário na UI.
}
```

### O Objeto `options`

- **`enableHighAccuracy`**: Um booleano. Se `true`, a API tentará obter a localização mais precisa possível. Isso pode ativar o GPS do dispositivo, o que consome mais bateria e pode demorar mais. O padrão é `false`.
- **`timeout`**: Um número que representa o tempo máximo, em milissegundos, que a aplicação está disposta a esperar por uma resposta. Se o tempo expirar, o `errorCallback` será chamado com o código `3`.
- **`maximumAge`**: Um número que indica a idade máxima, em milissegundos, de uma posição em cache que a aplicação aceita.
    - `maximumAge: 0`: Força o dispositivo a obter uma nova leitura de localização.
    - `maximumAge: Infinity`: Permite que o navegador retorne imediatamente qualquer posição que tenha em cache, não importa quão antiga seja.

## Rastreando a Posição: `watchPosition()`

Enquanto `getCurrentPosition()` é para uma leitura única, `watchPosition()` é para rastreamento contínuo. Ele funciona de forma muito semelhante, mas com uma diferença crucial: a função de `successCallback` será chamada **automaticamente toda vez que o dispositivo detectar uma mudança na posição do usuário**.

`watchPosition()` retorna um ID numérico (`watchID`), que é usado para parar o rastreamento.

**Exemplo: Rastreamento Simples**

```js
let watchID;

function sucessoWatch(posicao) {
  console.clear(); // Limpa o console a cada atualização
  console.log('Posição atualizada!');
  console.log(`Lat: ${posicao.coords.latitude}, Lon: ${posicao.coords.longitude}`);
}

function erroWatch(erro) {
  console.error(`Erro no rastreamento: ${erro.message}`);
}

const options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
};

if ("geolocation" in navigator) {
  // Inicia o rastreamento
  watchID = navigator.geolocation.watchPosition(sucessoWatch, erroWatch, options);
}

// Para parar o rastreamento (ex: quando o usuário clica em um botão "Parar")
function pararRastreamento() {
  if (watchID) {
    navigator.geolocation.clearWatch(watchID);
    console.log("Rastreamento interrompido.");
  }
}
```

## Considerações Finais

Neste capítulo, exploramos a **Geolocation API**, uma interface poderosa que permite que nossas aplicações web interajam com a localização física do usuário, criando experiências ricas e contextuais. Vimos que essa poder vem com uma grande responsabilidade, e que a API é projetada em torno do **consentimento do usuário** e de **contextos seguros (HTTPS)**.

Dominamos os dois principais métodos da API: **`getCurrentPosition()`**, para obter uma localização única, e **`watchPosition()`**, para monitorar continuamente as mudanças de posição, aprendendo também a interromper esse monitoramento com `clearWatch()`. Dissecamos os objetos de dados retornados, entendendo a importância não apenas da latitude e longitude, mas também da propriedade `accuracy` para avaliar a qualidade da informação. Além disso, construímos um handler de erros robusto para lidar graciosamente com as diversas falhas que podem ocorrer.

Saber como usar a Geolocation API de forma responsável é uma habilidade valiosa. Ao solicitar a localização apenas quando necessário, explicando claramente o porquê, e tratando os dados com o devido cuidado, você pode construir aplicações que oferecem um valor imenso aos seus usuários, conectando de forma transparente o mundo digital ao seu ambiente físico.
# Capítulo 43 – Push API e Service Workers: O Coração das Aplicações Offline e em Tempo Real

Em nossa jornada pelo JavaScript, aprendemos a construir aplicações ricas e interativas. Dominamos a comunicação assíncrona com `Fetch` para buscar dados e a `Notifications API` para alertar os usuários. No entanto, todas essas funcionalidades compartilham uma limitação fundamental: elas dependem de a aplicação estar aberta e de uma conexão ativa com a internet. Se o usuário perder a conexão, nossa aplicação para de funcionar. Se o usuário fechar a aba, nossa capacidade de nos comunicarmos com ele desaparece. Como podemos quebrar essas barreiras e criar experiências que sejam confiáveis em redes instáveis e que possam reengajar o usuário de forma proativa, assim como as aplicações nativas?

A resposta para esses desafios reside em uma das tecnologias mais revolucionárias da plataforma web: o **Service Worker**. Um Service Worker não é apenas mais uma API; é um tipo completamente novo de script que atua como um **proxy programável entre sua aplicação web, o navegador e a rede**. Ele opera em uma thread separada, em segundo plano, totalmente independente da interface do usuário. Essa separação lhe confere superpoderes: ele pode interceptar e manipular requisições de rede, gerenciar um cache de respostas e, o mais impressionante, continuar a receber eventos e executar código mesmo quando a aba da sua aplicação está fechada.

Neste capítulo, vamos dominar esta tecnologia fundamental. Começaremos por desvendar o **ciclo de vida do Service Worker**, um processo de registro, instalação e ativação que é crucial para seu funcionamento. O coração do capítulo será a exploração do evento `fetch`, onde aprenderemos a interceptar requisições para implementar estratégias de **caching offline**. Em seguida, vamos conectar nosso conhecimento com o capítulo anterior e desvendar a **Push API**, o mecanismo que, em conjunto com os Service Workers, possibilita o envio de **notificações push** que podem "acordar" nossa aplicação e entregar mensagens importantes a qualquer momento. Ao final, você entenderá como Service Workers são a base para a criação de **Progressive Web Apps (PWAs)** — aplicações web que são rápidas, confiáveis e engajadoras.

## O que é um Service Worker?

Um Service Worker é um script JavaScript que seu navegador executa em segundo plano, separado da sua página web. Pense nele como um intermediário que se senta entre sua aplicação e a rede.

**Características Principais:**

- **Execução em Background:** Ele roda em uma thread separada, o que significa que suas operações, mesmo que pesadas, não travam a interface do usuário.
- **Sem Acesso ao DOM:** Por rodar em uma thread separada, um Service Worker não pode acessar o `document` ou o `window` diretamente. A comunicação entre o Service Worker e a página é feita através de uma API de mensagens (`postMessage`).
- **Proxy de Rede Programável:** Sua habilidade mais importante é a de interceptar, modificar e responder a todas as requisições de rede feitas pela sua aplicação.
- **Orientado a Eventos e com Término Programado:** Ele é iniciado quando um evento que ele precisa ouvir ocorre (como `fetch` ou `push`) e é terminado pelo navegador quando não está em uso para economizar recursos.
- **Segurança em Primeiro Lugar (HTTPS):** Assim como outras APIs poderosas, os Service Workers exigem que sua aplicação seja servida sobre **HTTPS** (com a exceção de `localhost` para desenvolvimento).

## O Ciclo de Vida do Service Worker: Registro, Instalação e Ativação

O gerenciamento de um Service Worker envolve um ciclo de vida preciso e deliberado.

### 1. Registro

O primeiro passo é registrar o script do Service Worker a partir da sua página principal. Isso é feito usando `navigator.serviceWorker.register()`.

**`main.js` (o script da sua página):**

```js
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js') // O caminho para o seu script de service worker
      .then(registration => {
        console.log('Service Worker registrado com sucesso! Escopo:', registration.scope);
      })
      .catch(error => {
        console.error('Falha ao registrar o Service Worker:', error);
      });
  });
}
```

- É uma boa prática registrar o Service Worker após o evento `load` da página para não competir com o carregamento inicial dos recursos.
- O registro retorna uma `Promise` que resolve com um objeto `registration`.

### 2. Instalação

Uma vez que o `sw.js` é registrado com sucesso pela primeira vez, o navegador tenta instalá-lo. Dentro do script do Service Worker, podemos ouvir o evento `install`.

Este é o momento perfeito para preparar seu Service Worker, principalmente para **armazenar em cache os recursos estáticos** da sua aplicação (o "App Shell" - HTML, CSS, JavaScript, imagens principais).

**`sw.js` (o script do Service Worker):**

```js
const CACHE_NAME = 'minha-app-cache-v1';
const urlsToCache = [
  '/',
  '/index.html',
  '/estilos.css',
  '/app.js',
  '/imagens/logo.png'
];

// 'self' refere-se ao próprio escopo do Service Worker
self.addEventListener('install', event => {
  console.log('Service Worker: Instalando...');
  
  // event.waitUntil() recebe uma promise e estende a fase de instalação
  // até que a promise seja resolvida. Isso garante que o SW não prossiga
  // antes que o cache esteja pronto.
  event.waitUntil(
    caches.open(CACHE_NAME) // A Cache API é uma API de armazenamento global
      .then(cache => {
        console.log('Cache aberto. Armazenando arquivos do App Shell.');
        return cache.addAll(urlsToCache);
      })
  );
});
```

### 3. Ativação

Após a instalação, o Service Worker entra em um estado de `waiting` até que todas as abas controladas pela versão **antiga** do Service Worker sejam fechadas. Quando isso acontece, o novo Service Worker é **ativado**, e o evento `activate` é disparado.

Este evento é o local ideal para fazer a limpeza de caches antigos, garantindo que o usuário receba as versões mais recentes dos arquivos na próxima visita.

**`sw.js` (continuação):**

```js
self.addEventListener('activate', event => {
  console.log('Service Worker: Ativando...');
  
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cache => {
          if (cache !== CACHE_NAME) {
            console.log('Service Worker: Limpando cache antigo:', cache);
            return caches.delete(cache);
          }
        })
      );
    })
  );
});
```

## Interceptando Requisições com o Evento `fetch`

Este é o coração da funcionalidade offline. Uma vez que um Service Worker está ativo, ele pode interceptar todas as requisições de rede feitas pela sua página. O evento `fetch` é disparado para cada uma delas.

Dentro do handler, usamos `event.respondWith()` para tomar controle e decidir como responder. Podemos responder com um item do cache, fazer a requisição de rede real, ou até mesmo gerar uma resposta customizada.

**`sw.js` (continuação):**

```js
self.addEventListener('fetch', event => {
  // event.respondWith() intercepta a requisição
  event.respondWith(
    // Tenta encontrar uma resposta correspondente no cache
    caches.match(event.request)
      .then(response => {
        // Se a resposta for encontrada no cache, a retorna.
        if (response) {
          console.log('Respondendo com o cache:', event.request.url);
          return response;
        }
        
        // Se não, faz a requisição de rede real.
        console.log('Buscando na rede:', event.request.url);
        return fetch(event.request);
      }
    )
  );
});
```

Este é um exemplo de uma estratégia de cache simples conhecida como **"Cache first, falling back to network"**. Ela é ótima para o App Shell, pois garante que a aplicação carregue instantaneamente, mesmo offline.

## Notificações Push: Conectando Tudo

Como introduzido no capítulo anterior, é a combinação do Service Worker com a **Push API** que permite as notificações push.

### O Fluxo Completo

Vamos revisitar e detalhar o fluxo:

**1. Página Principal (`main.js`): Inscrevendo o Usuário** Depois de registrar o Service Worker e obter a permissão para notificações, usamos o `PushManager` para inscrever o usuário.

```js
async function inscreverParaPush() {
  const registration = await navigator.serviceWorker.ready;
  const VAPID_PUBLIC_KEY = 'SUA_CHAVE_PUBLICA_VAPID_AQUI';

  try {
    const subscription = await registration.pushManager.subscribe({
      userVisibleOnly: true, // Sempre true, indica que cada push resultará em uma notificação
      applicationServerKey: urlBase64ToUint8Array(VAPID_PUBLIC_KEY)
    });

    console.log('Inscrição no Push realizada com sucesso:', subscription);
    
    // **CRUCIAL:** Envia o objeto 'subscription' para o seu servidor
    await fetch('/api/salvar-inscricao', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(subscription)
    });
  } catch (error) {
    console.error('Falha ao se inscrever no Push:', error);
  }
}
```

**2. Servidor Back-end:** Seu servidor recebe e armazena o objeto `subscription`. Este objeto contém um `endpoint` (uma URL única para o serviço de push do navegador) e as chaves de autenticação. Quando o servidor quer enviar uma mensagem, ele usa uma biblioteca (como `web-push` para Node.js) para enviar uma carga de dados para essa URL.

**3. Service Worker (`sw.js`): Recebendo o Evento `push`** O serviço de push (da Google, Mozilla, etc.) entrega a mensagem ao navegador do usuário, que "acorda" o seu Service Worker e dispara o evento `push`.

```js
self.addEventListener('push', event => {
  console.log('Service Worker: Notificação Push recebida.');

  // Extrai os dados do payload do push.
  const data = event.data.json();
  const title = data.title || "Notificação";
  const options = {
    body: data.body,
    icon: '/imagens/icone.png',
    badge: '/imagens/badge.png' // Ícone para a barra de status do Android
  };

  // Exibe a notificação
  event.waitUntil(
    self.registration.showNotification(title, options)
  );
});
```

**4. Service Worker (`sw.js`): Lidando com o Clique na Notificação** Para tornar a notificação útil, precisamos reagir ao clique do usuário.

```js
self.addEventListener('notificationclick', event => {
  console.log('Service Worker: Clique na notificação recebido.');

  // Fecha a notificação
  event.notification.close();

  // Abre uma nova janela ou foca em uma existente
  event.waitUntil(
    clients.openWindow('https://sua-aplicacao.com/alguma-pagina')
  );
});
```

Este ciclo completo nos permite alcançar o usuário com mensagens relevantes e oportunas, a qualquer momento.

## Considerações Finais

Neste capítulo, desvendamos uma das tecnologias mais transformadoras da web: o **Service Worker**. Vimos que ele atua como um poderoso proxy de rede, nos dando a capacidade de criar experiências que são, pela primeira vez na história da web, verdadeiramente **offline-first**.

Exploramos seu ciclo de vida, aprendendo a registrar, instalar e ativar o script que roda em segundo plano. Dominamos o evento `fetch`, a ferramenta que nos permite interceptar requisições para implementar estratégias de caching sofisticadas, garantindo que nossa aplicação seja rápida e confiável, independentemente da qualidade da rede.

O ponto culminante foi a integração do Service Worker com a **Push API**, o que nos permitiu construir o fluxo completo de notificações push, desde a inscrição do usuário até o recebimento e a exibição da notificação, mesmo com a aplicação fechada. Com Service Workers, a linha entre aplicações web e nativas se torna cada vez mais tênue.
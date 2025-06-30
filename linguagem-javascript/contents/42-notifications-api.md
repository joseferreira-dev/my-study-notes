# Capítulo 42 – Notifications API: Engajando Usuários Fora da Página

Ao longo de nossa jornada, construímos aplicações que são dinâmicas, interativas e conectadas. Elas podem responder a eventos, manipular o DOM, comunicar-se com servidores e até mesmo interagir com o sistema de arquivos local. No entanto, a grande maioria dessas interações acontece dentro de uma fronteira bem definida: a aba do navegador. Se o usuário fecha nossa aba ou muda para outra tarefa, nossa capacidade de nos comunicarmos com ele cessa. Mas e se houvesse uma maneira de nossa aplicação poder, de forma respeitosa, chamar a atenção do usuário de volta para uma informação importante, mesmo que ele não esteja ativamente em nosso site?

É para resolver este desafio de reengajamento que existe a **Notifications API**. Esta é uma API da plataforma web que permite que uma aplicação web exiba **notificações do sistema operacional**, aquelas mesmas caixinhas de alerta que aparecem no canto da tela, geradas por aplicações nativas como clientes de e-mail, calendários ou aplicativos de mensagens. Com esta API, uma aplicação de e-mail pode notificar sobre uma nova mensagem, um e-commerce pode avisar que um produto voltou ao estoque, ou um sistema de gerenciamento de tarefas pode lembrar sobre um prazo iminente. Ela transforma a web, permitindo que nossas aplicações transcendam a aba do navegador e forneçam valor de forma proativa.

Neste capítulo, vamos dominar esta poderosa API. O pilar central de nossa discussão será o **modelo de permissão**. Entenderemos que, por ser uma funcionalidade altamente intrusiva, o envio de notificações exige o consentimento explícito e consciente do usuário, e aprenderemos as melhores práticas para solicitar essa permissão. Dissecaremos a criação de notificações, customizando seu título, corpo, ícone e comportamento. Exploraremos como reagir às interações do usuário com a notificação, como cliques e fechamentos, e como podemos controlar e atualizar notificações existentes. Finalmente, abordaremos a importante sinergia entre a Notifications API e os Service Workers, o que possibilita o santo graal do engajamento web: as **notificações push**, que podem ser entregues mesmo quando o site não está aberto.

## O Pilar Inegociável: Permissão do Usuário

A capacidade de enviar uma notificação para a área de trabalho de um usuário é um grande poder e, com ele, vem uma grande responsabilidade. Um abuso dessa funcionalidade leva rapidamente à frustração do usuário e à perda de confiança. Por essa razão, a Notifications API é construída sobre um modelo de permissão rigoroso.

Antes de poder enviar qualquer notificação, você deve **solicitar permissão** ao usuário. Esta solicitação é mediada pelo navegador, que exibe um pop-up não-bloqueante.

O estado da permissão pode ser um de três valores:

- **`default`**: O estado inicial. O usuário ainda não tomou uma decisão. Neste estado, se você tentar enviar uma notificação, ela será silenciosamente ignorada.
- **`granted`**: O usuário permitiu o envio de notificações. Você está livre para enviá-las.
- **`denied`**: O usuário explicitamente negou a permissão. Você **não pode** mais solicitar a permissão e não pode enviar notificações. Esta decisão é, em geral, difícil de ser revertida pelo usuário (exige que ele vá até as configurações do navegador).

### Solicitando Permissão: `Notification.requestPermission()`

O método estático `Notification.requestPermission()` é usado para acionar o pop-up de solicitação do navegador.

**Importante:** Por ser uma ação intrusiva, este método só deve ser chamado em resposta a uma **ação direta do usuário**, como o clique em um botão "Ativar notificações". Chamá-lo automaticamente no carregamento da página resulta em uma péssima experiência do usuário e é bloqueado por muitos navegadores modernos.

A implementação moderna de `requestPermission()` é baseada em Promises.

```js
function solicitarPermissao() {
  Notification.requestPermission()
    .then(resultado => {
      console.log(`Permissão para notificações: ${resultado}`);
      if (resultado === 'granted') {
        // Permissão concedida! Podemos agora enviar notificações.
        new Notification("Obrigado por ativar as notificações!");
      } else if (resultado === 'denied') {
        // O usuário negou, não devemos mais incomodá-lo.
        console.log("O usuário negou as notificações. Respeitaremos sua decisão.");
      } else {
        // O usuário dispensou o pop-up sem tomar uma decisão.
        console.log("O usuário não tomou uma decisão ainda.");
      }
    });
}

// Suponha que temos um botão no HTML: <button id="btn-ativar">Ativar Notificações</button>
const botaoAtivar = document.getElementById('btn-ativar');
botaoAtivar.addEventListener('click', solicitarPermissao);
```

Para verificar o estado atual da permissão sem acionar o pop-up, você pode ler a propriedade estática `Notification.permission`.

```js
if (Notification.permission === 'granted') {
  console.log('As notificações já estão ativadas.');
}
```

## Criando e Enviando uma Notificação

Uma vez que a permissão foi concedida, criar uma notificação é tão simples quanto instanciar a classe `Notification`.

**Sintaxe:** `new Notification(title, [options]);`

- `title`: A string de título, a parte principal e em negrito da notificação.
- `options` (opcional): Um objeto para customizar a aparência e o comportamento da notificação.

### Opções de Notificação

|Opção|Descrição|
|---|---|
|`body`|Uma string com o texto principal da notificação, exibido abaixo do título.|
|`icon`|A URL para um pequeno ícone a ser exibido ao lado do título e do corpo.|
|`image`|A URL para uma imagem maior a ser exibida dentro do corpo da notificação.|
|`data`|Qualquer tipo de dado que você queira associar à notificação. Útil para saber qual item foi clicado quando o usuário interage.|
|`tag`|Uma string de identificação. Se uma nova notificação for criada com a mesma `tag` de uma que já está visível, a nova substitui a antiga.|
|`requireInteraction`|Um booleano. Se `true`, a notificação permanecerá na tela até que o usuário interaja com ela (em vez de desaparecer automaticamente).|
|`silent`|Um booleano. Se `true`, a notificação será exibida sem som ou vibração (se aplicável no dispositivo).|

**Exemplo Completo:**

```js
function enviarNotificacaoDeBoasVindas() {
  if (Notification.permission !== 'granted') {
    console.log("Permissão não concedida.");
    return;
  }
  
  const options = {
    body: "Explore nossos novos recursos e comece a usar a plataforma agora mesmo.",
    icon: "/imagens/logo-icone.png",
    image: "/imagens/banner-boas-vindas.png",
    tag: "boas-vindas-geral", // Garante que o usuário só veja uma notificação de boas-vindas
    requireInteraction: true,
    data: {
      userId: 123,
      urlParaAbrir: "/painel"
    }
  };

  const notificacao = new Notification("Bem-vindo à Nossa Plataforma!", options);

  // Podemos interagir com a notificação depois de criá-la.
  // ...
}
```

## O Ciclo de Vida da Notificação: Eventos

A instância de `Notification` que criamos é um objeto que emite eventos, permitindo-nos reagir às interações do usuário.

- **`show`**: Disparado quando a notificação é exibida para o usuário.
- **`click`**: Disparado quando o usuário clica em qualquer parte da notificação (exceto botões de ação, que é um recurso mais avançado). Este é o evento mais importante para o reengajamento.
- **`close`**: Disparado quando a notificação é fechada, seja pelo usuário, pelo sistema operacional ou programaticamente com `notification.close()`.
- **`error`**: Disparado se algo der errado e a notificação não puder ser exibida.

**Exemplo de Manipulação de Eventos:**

```js
const notificacao = new Notification("Novo e-mail de João", {
  body: "Assunto: Reunião de Projeto",
  tag: "email-1",
  data: {
    emailId: 'xyz-789'
  }
});

notificacao.onclick = function(event) {
  console.log("O usuário clicou na notificação.");
  
  // Foca na aba da nossa aplicação ou abre uma nova.
  window.parent.focus(); 
  
  // Usa os dados associados para ir para a página correta.
  const emailId = event.target.data.emailId;
  window.open(`/ler-email?id=${emailId}`);
  
  // Fecha a notificação após o clique.
  event.target.close();
};

notificacao.onclose = function() {
  console.log("A notificação foi fechada.");
};
```

### Fechando uma Notificação Programaticamente

Você pode fechar uma notificação a qualquer momento usando seu método `.close()`.

```js
const notificacaoTemporaria = new Notification("Isso vai desaparecer em 3 segundos.");

setTimeout(() => {
  notificacaoTemporaria.close();
}, 3000);
```

## Notificações Push: Uma Introdução

Tudo o que vimos até agora funciona de forma excelente enquanto a página da sua aplicação estiver aberta em uma aba. Mas e se quisermos enviar uma notificação para um usuário sobre uma notícia de última hora, mesmo que ele não esteja com nosso site aberto? Para isso, precisamos das **Notificações Push**.

As Notificações Push são uma tecnologia separada que funciona em conjunto com a Notifications API. O fluxo é mais complexo:

1. **Permissão:** A aplicação web pede ao usuário permissão para enviar notificações (como já vimos).
2. **Inscrição no Serviço Push:** O JavaScript do seu site usa a **Push API** para se inscrever no serviço de mensagens push do navegador (ex: da Google, Mozilla, Apple).
3. **Endpoint:** Essa inscrição retorna um objeto de assinatura (`PushSubscription`) contendo uma URL de endpoint única para aquele usuário e navegador.
4. **Envio para o seu Servidor:** Sua aplicação envia esse endpoint para o seu servidor, que o armazena associado àquele usuário.
5. **Disparando o Push:** Quando seu servidor quer enviar uma notificação, ele não fala diretamente com o usuário. Ele faz uma requisição para o **endpoint** do serviço de push (ex: da Google), enviando os dados da notificação.
6. **Recebimento no Service Worker:** O serviço de push entrega a mensagem ao navegador do usuário. Um **Service Worker** (um script especial que roda em segundo plano, mesmo com o site fechado) da sua aplicação é "acordado" para receber o evento `push`.
7. **Exibição da Notificação:** Dentro do Service Worker, o código usa a **Notifications API** (`self.registration.showNotification(...)`) para finalmente exibir a notificação para o usuário.

Embora a implementação completa dos Service Workers e da Push API esteja além do escopo deste capítulo, é crucial entender que a Notifications API é a peça final que exibe a mensagem, enquanto o mecanismo de entrega offline é gerenciado por essas outras tecnologias.

## Considerações Finais

Neste capítulo, exploramos a **Notifications API**, uma ponte poderosa entre nossa aplicação web e a área de trabalho do usuário. Vimos que essa ponte é construída sobre um alicerce de **confiança e permissão**, exigindo que sempre solicitemos o consentimento do usuário de forma clara e em resposta a uma de suas ações.

Dominamos a criação de notificações ricas e informativas, customizando seu título, corpo, ícones e comportamento com o objeto de opções. Aprendemos a reagir às interações do usuário através dos eventos `onclick` e `onclose`, criando um ciclo de reengajamento que pode trazer o usuário de volta à nossa aplicação.

Finalmente, compreendemos a distinção entre notificações locais (enquanto a página está aberta) e as verdadeiras **notificações push**, que dependem de Service Workers para entregar mensagens mesmo quando o site está fechado. Com a Notifications API, você tem o poder de criar aplicações mais proativas e valiosas, mas lembre-se sempre de usar esse poder com responsabilidade, respeitando a atenção e a privacidade do seu usuário.
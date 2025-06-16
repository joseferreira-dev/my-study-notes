# Capítulo 23 – APIs HTML: O Poder das Aplicações Web

Até este ponto de nossa jornada, focamos primariamente no HTML como uma linguagem de marcação — um sistema para estruturar conteúdo e dar-lhe significado semântico. No entanto, o HTML5 evoluiu para ser muito mais do que isso. Ele se tornou o alicerce de uma poderosa plataforma de aplicações, capaz de rivalizar com muitas aplicações desktop nativas. Essa transformação foi possível graças à introdução de um rico conjunto de **APIs (Application Programming Interfaces)**.

Uma API, no contexto do navegador, é uma "ponte" que permite que nosso código JavaScript se comunique com funcionalidades do próprio navegador ou do sistema operacional do usuário. Elas nos dão acesso a recursos como a localização do dispositivo, armazenamento de dados offline, processamento em segundo plano e comunicação em tempo real. Essencialmente, as APIs transformam o navegador em um sistema operacional para a web.

Neste capítulo, vamos explorar o excitante mundo das APIs do HTML. Daremos uma visão geral das capacidades que elas nos oferecem e, em seguida, faremos um mergulho profundo em duas das mais utilizadas e importantes: a **API de Geolocalização**, que nos permite interagir com o mundo físico, e a **API de Armazenamento Web**, que nos dá a capacidade de salvar dados diretamente no navegador do usuário.

## Um Panorama das APIs Web

O HTML5 e as tecnologias da web relacionadas apresentam dezenas de APIs. Muitas delas já foram mencionadas em capítulos anteriores, como a **API Canvas** para desenho dinâmico ou a **API de Áudio da Web**. Vamos revisar brevemente algumas das mais impactantes:

- **API de Geolocalização**: Permite que aplicações da web acessem as informações de localização do usuário (latitude e longitude), sempre com a devida permissão.
- **API de Armazenamento Web (Web Storage)**: Fornece uma maneira de armazenar pares de chave-valor localmente em um navegador, permitindo que as aplicações funcionem offline ou salvem dados (como preferências do usuário) para uso futuro.
- **API Web Workers**: Permite que aplicações da web executem tarefas de JavaScript computacionalmente pesadas em segundo plano, em uma thread separada, sem travar ou congelar a interface do usuário.
- **API WebSockets**: Permite a comunicação **full-duplex** (bidirecional e simultânea) entre o navegador e um servidor, essencial para a troca de dados em tempo real, como em chats, jogos online ou painéis financeiros.
- **API Server-Sent Events (SSE)**: Uma tecnologia mais simples que WebSockets, que permite que um servidor da web "empurre" (envie) dados para um navegador em tempo real, sem a necessidade de o navegador fazer novas solicitações. É ideal para notificações, feeds de notícias e atualizações de status.

Agora, vamos nos aprofundar nas APIs de Geolocalização e Armazenamento.

## Mergulho Profundo: API de Geolocalização

A API de Geolocalização permite obter a posição geográfica de um dispositivo. Por razões óbvias de privacidade, o navegador **sempre solicitará a permissão explícita do usuário** antes de compartilhar essa informação com o seu site.

O acesso à API é feito através do objeto `navigator.geolocation`.

### Obtendo a Localização Atual

O principal método é o `getCurrentPosition()`. Ele tenta obter a localização atual do dispositivo e aceita três argumentos:

- **`success` (Obrigatório):** Uma função de callback que será executada se a localização for obtida com sucesso. Ela recebe um objeto `Position` como argumento.
- **`error` (Opcional):** Uma função de callback que será executada se ocorrer um erro. Ela recebe um objeto `PositionError` como argumento.
- **`options` (Opcional):** Um objeto para configurar parâmetros da busca, como precisão e tempo máximo de espera.

**Exemplo Prático:**

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>API de Geolocalização</title>
</head>
<body>
    <button id="btnLocalizacao">Onde estou?</button>
    <div id="resultado"></div>

    <script>
        const btn = document.getElementById('btnLocalizacao');
        const resultadoDiv = document.getElementById('resultado');

        btn.addEventListener('click', () => {
            if (!navigator.geolocation) {
                resultadoDiv.innerHTML = '<p>Geolocalização não é suportada pelo seu navegador.</p>';
                return;
            }

            resultadoDiv.innerHTML = '<p>Obtendo sua localização...</p>';

            // Funções de callback
            const sucesso = (posicao) => {
                const latitude = posicao.coords.latitude;
                const longitude = posicao.coords.longitude;
                const precisao = posicao.coords.accuracy;

                resultadoDiv.innerHTML = `
                    <p>Sua localização:</p>
                    <ul>
                        <li>Latitude: ${latitude}</li>
                        <li>Longitude: ${longitude}</li>
                        <li>Precisão de aproximadamente ${precisao} metros.</li>
                    </ul>
                    <a href="https://www.openstreetmap.org/#map=18/${latitude}/${longitude}" target="_blank">Ver no mapa</a>
                `;
            };

            const erro = (err) => {
                let mensagemErro;
                switch(err.code) {
                    case err.PERMISSION_DENIED:
                        mensagemErro = "Permissão para geolocalização negada pelo usuário.";
                        break;
                    case err.POSITION_UNAVAILABLE:
                        mensagemErro = "Informação de localização não está disponível.";
                        break;
                    case err.TIMEOUT:
                        mensagemErro = "A requisição para obter a localização expirou.";
                        break;
                    default:
                        mensagemErro = "Ocorreu um erro desconhecido.";
                        break;
                }
                resultadoDiv.innerHTML = `<p>Erro: ${mensagemErro}</p>`;
            };

            // Chamada da API
            navigator.geolocation.getCurrentPosition(sucesso, erro);
        });
    </script>
</body>
</html>
```

### Monitorando a Localização

Além de obter a localização uma única vez, a API permite monitorar as mudanças de posição do usuário (útil para aplicações de navegação). Para isso, usa-se o método `watchPosition()`, que funciona de forma similar, mas executa a função de sucesso toda vez que a posição do dispositivo é atualizada.

## Mergulho Profundo: API de Armazenamento Web (Web Storage)

A API Web Storage fornece um mecanismo para armazenar dados no formato chave-valor de forma muito mais intuitiva e com maior capacidade do que os antigos cookies. Existem dois tipos de armazenamento disponíveis.

### `localStorage` (Armazenamento Local)

O `localStorage` armazena dados de forma **persistente**. Os dados salvos aqui não expiram; eles permanecem disponíveis mesmo após o navegador ser fechado e reaberto.

- **Escopo:** Os dados são vinculados à **origem** (domínio, protocolo e porta). Todo o código do site `https://www.meusite.com` pode acessar o mesmo `localStorage`, em qualquer aba ou janela.
- **Casos de Uso:** Salvar preferências do usuário (ex: tema escuro/claro), manter o usuário "logado" (salvando um token, não a senha!), guardar o conteúdo de um carrinho de compras.

**API:**

- `localStorage.setItem('chave', 'valor')`: Adiciona ou atualiza um item.
- `localStorage.getItem('chave')`: Recupera o valor de um item.
- `localStorage.removeItem('chave')`: Remove um item.
- `localStorage.clear()`: Remove todos os itens.

**Importante:** O Web Storage só armazena strings. Para salvar objetos ou arrays, você precisa serializá-los com `JSON.stringify()` e desserializá-los com `JSON.parse()`.

**Exemplo Prático (Tema Escuro):**

```html
<style>
    body.dark-theme { background-color: #333; color: white; }
</style>

<button id="btnTema">Alternar Tema</button>

<script>
    const btn = document.getElementById('btnTema');

    // Função para aplicar o tema salvo
    const aplicarTema = () => {
        const temaSalvo = localStorage.getItem('tema');
        if (temaSalvo === 'dark') {
            document.body.classList.add('dark-theme');
        } else {
            document.body.classList.remove('dark-theme');
        }
    };

    btn.addEventListener('click', () => {
        // Verifica o tema atual e alterna
        let novoTema = document.body.classList.contains('dark-theme') ? 'light' : 'dark';
        localStorage.setItem('tema', novoTema);
        aplicarTema();
    });

    // Aplica o tema ao carregar a página
    document.addEventListener('DOMContentLoaded', aplicarTema);
</script>
```

### `sessionStorage` (Armazenamento de Sessão)

O `sessionStorage` funciona exatamente da mesma forma que o `localStorage`, com uma diferença crucial: os dados são **temporários** e vinculados a uma **sessão de navegação (uma aba)**.

- **Escopo:** Os dados são isolados por aba. Se o usuário abrir seu site em duas abas diferentes, cada uma terá seu próprio `sessionStorage`.
- **Ciclo de Vida:** Os dados são apagados automaticamente quando a aba ou o navegador é fechado.
- **Casos de Uso:** Armazenar dados temporários de um formulário de múltiplas etapas, guardar o estado de uma aplicação para que não se perca ao recarregar a página.

A API é idêntica, basta substituir `localStorage` por `sessionStorage`:

- `sessionStorage.setItem('chave', 'valor');`
- `sessionStorage.getItem('chave');`

## Considerações Finais

Neste capítulo, abrimos a cortina para o que realmente transforma o HTML em uma plataforma de aplicação: as APIs. Vimos como elas atuam como pontes entre nosso código e as funcionalidades poderosas do navegador, desde saber onde o usuário está no mundo com a **Geolocalização**, até lembrar de suas preferências com o **Web Storage**.

Entender e utilizar essas APIs é o que permite a criação de experiências ricas, personalizadas e contextuais. Elas são a prova de que o HTML moderno, em conjunto com o JavaScript, é um ecossistema robusto, capaz de entregar aplicações que antes eram exclusivas do mundo desktop, agora disponíveis diretamente no navegador.
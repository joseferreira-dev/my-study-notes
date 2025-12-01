# Capítulo 18 – Metadados

Ao longo desta apostila, nosso foco tem sido predominantemente no conteúdo visível da página — os textos, imagens, links e layouts que o usuário final vê e com os quais interage. Contudo, por trás de toda página bem-sucedida, existe uma camada de informação oculta, mas de importância crítica, que instrui o navegador sobre como renderizar o documento, informa os mecanismos de busca sobre o que se trata o conteúdo e diz às redes sociais como exibir um link compartilhado. Essa camada é composta de **metadados**, e a principal ferramenta para defini-los é o elemento `<meta>`.

Neste capítulo, faremos um mergulho profundo na tag `<meta>`, um elemento versátil que vive exclusivamente dentro do `<head>`. Começaremos com seu papel mais fundamental: definir o conjunto de caracteres (`charset`). Em seguida, exploraremos o vasto universo do atributo `name`, que nos permite fornecer uma descrição da página para o Google, controlar o comportamento de robôs de busca e definir a "viewport" para um layout responsivo em dispositivos móveis. Investigaremos também como os metadados se tornam a vitrine do seu site em redes sociais através do Open Graph e dos Twitter Cards, e como eles podem melhorar a experiência de um site usado como um aplicativo web.

Dominar a tag `<meta>` é como aprender a configurar o painel de controle de uma página. É o conhecimento que permite que seu conteúdo, tão cuidadosamente estruturado, seja apresentado da melhor forma possível em qualquer contexto, desde um resultado de busca até a tela inicial de um smartphone.

## O Que São Metadados?

Metadados são "dados sobre os dados". No contexto de um documento HTML, eles são informações que descrevem a própria página, mas que não fazem parte do seu conteúdo visível. O elemento `<meta>` é uma tag vazia usada para fornecer esses metadados. Ele funciona principalmente através de pares de atributos, como `name`/`content` ou `http-equiv`/`content`.

## Atributo `charset`

A primeira meta tag em qualquer documento HTML deve ser a declaração do conjunto de caracteres. Isso garante que todo o texto, incluindo caracteres especiais e acentos, seja interpretado e exibido corretamente.

```html
<meta charset="UTF-8">
```

`UTF-8` é o padrão universal para a web. Ele é capaz de representar praticamente todos os caracteres e símbolos existentes. A ausência dessa declaração pode levar a problemas de renderização de texto, exibindo caracteres estranhos (como `�`) no lugar de acentos.

## Atributos `name` e `content`

A combinação dos atributos `name` e `content` é a forma mais comum de fornecer metadados. O `name` especifica o tipo de informação, e o `content` fornece o valor dessa informação.

- **`description`**: Talvez a meta tag mais importante para SEO (Search Engine Optimization). O conteúdo desta tag é frequentemente usado pelos mecanismos de busca como o trecho de descrição que aparece abaixo do título nos resultados da pesquisa. Uma boa descrição é concisa (geralmente até 160 caracteres), atrativa e relevante ao conteúdo da página.

```html
<meta name="description" content="Aprenda a criar sites incríveis com nosso guia completo de HTML, cobrindo desde o básico até tópicos avançados.">
```

- **`keywords`**: Historicamente, esta tag era usada para listar palavras-chave relevantes para a página. No entanto, devido ao abuso (keyword stuffing), os principais mecanismos de busca como o Google **não a utilizam mais** como fator de ranqueamento. Embora inofensiva, seu uso hoje é considerado obsoleto.

```html
<!-- Exemplo histórico, não é mais relevante para SEO -->
<meta name="keywords" content="HTML, tutorial, guia, desenvolvimento web">
```

- **`author`**: Especifica o autor do conteúdo da página.

```html
<meta name="author" content="João da Silva">
```

- **`generator`**: Indica o software ou a ferramenta usada para gerar a página. Geralmente é adicionado automaticamente por CMSs ou construtores de sites.

```html
<meta name="generator" content="Visual Studio Code">
```

- **`application-name`**: Se o seu site é uma aplicação web, este metadado pode ser usado para definir o nome da aplicação.

    ```html
    <meta name="application-name" content="Meu Editor de Fotos Online">
    ```

## Dando Instruções aos Robôs de Busca

Através de `<meta name="robots" ...>`, podemos dar diretivas específicas aos robôs de busca (crawlers) sobre como eles devem tratar nossa página.

- `content="index, follow"` (ou `all`): O padrão. Permite que o robô indexe a página (mostre nos resultados) e siga os links nela contidos.
- `content="noindex, nofollow"` (ou `none`): O mais restritivo. Pede ao robô para não indexar a página e não seguir nenhum link nela.
- `content="noindex, follow"`: Não indexar a página, mas seguir os links para descobrir outras páginas.
- `content="index, nofollow"`: Indexar a página, mas não seguir os links contidos nela.

Outras diretivas úteis:

- `noarchive`: Impede que o Google exiba o link "Em cache" para a página.
- `nosnippet`: Impede a exibição de um trecho de texto ou preview de vídeo nos resultados da busca.
- `noimageindex`: Impede a indexação das imagens da página.
- `unavailable_after: [data]` : Permite especificar uma data e hora após a qual a página não deve mais ser exibida nos resultados da busca. O formato deve ser RFC 850 (ex: `Mon, 15 Jun 2025 12:00:00 GMT`).

## Atributo `http-equiv`

Este atributo permite que a tag `<meta>` simule um cabeçalho de resposta HTTP, podendo alterar o comportamento do servidor ou do navegador. Seu uso é menos comum hoje em dia, pois muitas dessas configurações são melhor gerenciadas no servidor.

- **Recarregamento Automático**: Faz a página recarregar após um número de segundos.

```html
<meta http-equiv="refresh" content="30"> <!-- Recarrega a cada 30 segundos -->
```

- **Redirecionamento Automático**: Redireciona o usuário para outra URL após um período. **Atenção**: Redirecionamentos 301 feitos no servidor são a prática recomendada para SEO e usabilidade. Use isto com cautela.

```html
<meta http-equiv="refresh" content="5;url=https://www.novosite.com/"> <!-- Redireciona após 5 segundos -->
```

## Exibição Mobile com `viewport`

Sem esta linha, seu site não será exibido corretamente em dispositivos móveis. A meta tag `viewport` instrui o navegador sobre como controlar as dimensões e a escala da página.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
````

Vamos detalhar o `content`:

- `width=device-width`: Define a largura da viewport para ser igual à largura da tela do dispositivo. Isso faz com que o site ocupe a tela de forma natural.
- `initial-scale=1.0`: Define o nível de zoom inicial quando a página é carregada pela primeira vez. `1.0` remove qualquer zoom.
- `maximum-scale=1.0`, `minimum-scale=1.0`: Podem ser usados para fixar o nível de zoom.
- `user-scalable=no`: Impede o usuário de dar zoom na página. **Esta é uma prática terrível para a acessibilidade** e deve ser evitada, pois prejudica usuários com baixa visão.

## Metadados para Redes Sociais

Quando você compartilha um link no Facebook, Twitter ou WhatsApp, a plataforma busca por metadados específicos para gerar aquele preview rico com título, descrição e imagem.

- **Open Graph (usado pelo Facebook, LinkedIn, etc.):**

```html
<meta property="og:title" content="O Título para Redes Sociais">
<meta property="og:type" content="article">
<meta property="og:url" content="https://www.meusite.com/artigo.html">
<meta property="og:image" content="https://www.meusite.com/imagem-do-artigo.jpg">
<meta property="og:description" content="Uma descrição atraente que aparecerá no compartilhamento.">
<meta property="og:site_name" content="Nome do Meu Site">
```

- **Twitter:**

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@meuusuario">
<meta name="twitter:title" content="O Título para o Twitter">
<meta name="twitter:description" content="Descrição específica para o Twitter.">
<meta name="twitter:image" content="https://www.meusite.com/imagem-twitter.jpg">
```

## Metadados para Web Apps e Mobile

Podemos adicionar metadados que melhoram a experiência quando um site é "salvo na tela inicial" de um celular.

- **Controle de Detecção de Formato:** Impede que o iOS detecte automaticamente o que ele pensa ser um número de telefone e o transforme em um link.

```html
<meta name="format-detection" content="telephone=no">
```

- **Cor da Barra de Status (Android Chrome):**

```html
<meta name="theme-color" content="#2c3e50">
```

- **iOS Web App:**

```html
<!-- Faz o site rodar em modo de tela cheia quando aberto da tela inicial -->
<meta name="apple-mobile-web-app-capable" content="yes">
<!-- Define a aparência da barra de status (default, black, ou black-translucent) -->
<meta name="apple-mobile-web-app-status-bar-style" content="black">
```

## Considerações Finais

Neste capítulo, desvendamos o poder da tag `<meta>`. Vimos que ela é a ferramenta que nos permite comunicar informações vitais sobre nosso documento para o mundo exterior. Através dela, otimizamos nossa presença nos resultados de busca, garantimos uma aparência profissional em compartilhamentos nas redes sociais e, fundamentalmente, asseguramos que nossa página seja renderizada corretamente em qualquer dispositivo, graças à onipresente meta tag `viewport`.

A metainformação é a camada de inteligência que envolve nosso conteúdo. Negligenciá-la é como publicar um livro sem capa, título ou sinopse. Dedicar atenção a esses detalhes é um sinal de profissionalismo e um passo essencial para o sucesso de qualquer projeto web.
# Capítulo 37 – Design Responsivo com Media Queries e Fragmentação

Até agora, nossa jornada nos tornou mestres em construir e estilizar componentes individuais e layouts complexos. Mas todos esses designs foram criados com um contexto em mente: a tela de um computador. No entanto, a web moderna não vive apenas em desktops. Ela é acessada em uma gama estonteante de dispositivos: telemóveis, tablets, portáteis, televisões e até mesmo impressoras. Um layout que é perfeito em uma tela grande pode se tornar inutilizável em uma tela pequena. Como podemos fazer com que nosso design se adapte graciosamente a essa diversidade de contextos?

A resposta está em uma das funcionalidades mais transformadoras do CSS: as **Media Queries**. Elas são filtros que nos permitem aplicar blocos de estilos CSS apenas quando certas condições sobre o dispositivo ou a janela do navegador (a **media**) são atendidas. Elas são a espinha dorsal do **Design Web Responsivo (RWD)**, a abordagem que defende a criação de sites que respondem e se adaptam ao ambiente do usuário.

Neste capítulo, vamos mergulhar fundo no mundo das Media Queries. Desvendaremos a sintaxe da regra `@media`, exploraremos os diferentes tipos de mídia e as características que podemos consultar, como largura e resolução. Aprenderemos a criar "breakpoints" para adaptar nossos layouts e a adotar a abordagem "Mobile First". Além disso, vamos explorar o módulo de **Fragmentação**, que nos dá controle sobre como o conteúdo é quebrado em páginas ou colunas, uma habilidade essencial para a criação de documentos para impressão e layouts de múltiplas colunas bem-acabados.

## O que são Media Queries?

Uma Media Query é uma expressão lógica que pode ser verdadeira ou falsa. Se a expressão for verdadeira, o bloco de código CSS associado a ela é aplicado. Elas são usadas para criar _breakpoints_ no design, pontos em que o layout muda para melhor se adequar ao novo tamanho da tela.

### Sintaxe Básica de `@media`

A sintaxe consiste na regra-at `@media`, seguida por uma condição (opcionalmente um tipo de mídia e/ou uma ou mais características de mídia), e um bloco de CSS entre chaves `{}`.

```css
@media condicao {
  /* Estilos a serem aplicados se a condição for verdadeira */
  .seletor {
    propriedade: valor;
  }
}
```

## A Anatomia de uma Media Query

Uma Media Query completa é composta por três partes: um tipo de mídia, operadores lógicos e as características de mídia.

### Tipo de Mídia (`mediatype`)

Define a categoria geral do dispositivo. Embora existam muitos tipos, na prática moderna, os mais relevantes são:

- **`screen`**: Para ecrãs de computador, tablets, telemóveis, etc. (o mais comum).
- **`print`**: Para impressoras e visualização de impressão.
- **`all`**: Aplica-se a todos os dispositivos (este é o valor padrão se nenhum for especificado).

Outros tipos existem, mas a maioria está obsoleta ou tem suporte limitado: `handheld`, `projection`, `aural`, `braille`, `embossed`, `tv`, `tty`.

**Exemplo:**

```css
/* Estes estilos só serão aplicados ao imprimir a página */
@media print {
  body {
    font-size: 12pt;
    color: black;
  }
  a {
    text-decoration: none;
  }
}
```

### Operadores Lógicos

Permitem construir expressões complexas.

- **`and`**: Combina múltiplas características. Todas devem ser verdadeiras para que a regra se aplique.
- **`not`**: Nega o resultado de uma media query.
- **`only`**: Usado para esconder os estilos de navegadores mais antigos que não suportam media queries com características. Na prática moderna, seu uso é menos necessário, mas não prejudica. Ex: `@media only screen...`
- **Vírgula (`,`)**: Funciona como um operador **`or`**. A regra se aplica se **qualquer uma** das queries separadas por vírgula for verdadeira.

**Exemplo:**

```css
/* Aplica se a tela tiver entre 600px e 900px de largura */
@media screen and (min-width: 600px) and (max-width: 900px) { ... }

/* Aplica se o dispositivo NÃO for uma impressora */
@media not print { ... }
```

### Características de Mídia (`media feature`)

Esta é a parte mais poderosa. Elas são as condições que consultamos sobre o dispositivo. Quase todas podem ser prefixadas com `min-` ou `max-` para criar intervalos.

#### Características de Dimensão da Viewport

Estas são as mais usadas no design responsivo.

- **`width`** (e `min-width`, `max-width`): Consulta a largura da viewport.
- **`height`** (e `min-height`, `max-height`): Consulta a altura da viewport.

**Compreendendo `min-width` e `max-width`:**

- **`min-width: 768px`**: A condição é verdadeira se a largura da viewport for **768px ou maior**. Usada na abordagem **Mobile First**.
- **`max-width: 767px`**: A condição é verdadeira se a largura da viewport for **767px ou menor**. Usada na abordagem **Desktop First**.

**Exemplo (Targeting de Diferentes Tamanhos de Tela - Mobile First):**

```css
/* --- Estilos Base (Mobile) --- */
.container {
  width: 100%;
}
.coluna {
  width: 100%; /* Em mobile, as colunas ocupam a largura toda e se empilham */
  margin-bottom: 1rem;
}

/* --- Breakpoint para Tablets (>= 600px) --- */
@media (min-width: 600px) {
  .container {
    max-width: 580px;
    margin: 0 auto;
  }
  .coluna {
    width: 50%; /* Em tablets, duas colunas lado a lado */
    float: left;
  }
}

/* --- Breakpoint para Desktops (>= 992px) --- */
@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
  .coluna {
    width: 33.33%; /* Em desktops, três colunas */
  }
}
```

#### Características de Proporção e Orientação

- **`aspect-ratio`** (e `min/max-aspect-ratio`): Compara a proporção entre a largura e a altura da viewport. Ex: `(aspect-ratio: 16/9)`.
- **`orientation`**: Verifica se a viewport está em modo retrato (`portrait`) ou paisagem (`landscape`).

**Exemplo:**

```css
/* Aplica um preenchimento maior quando o telemóvel está em modo paisagem */
@media (max-height: 500px) and (orientation: landscape) {
  .header {
    padding: 5px 20px;
  }
}
```

#### Características de Qualidade da Tela

- **`resolution`**: Verifica a densidade de pixels da tela, permitindo visar ecrãs de alta resolução (Retina). Pode ser medida em `dpi` (dots per inch) ou `dppx` (dots per pixel unit).
- **`color`**, **`color-index`**, **`monochrome`**: Verificam as capacidades de cor do dispositivo.

**Exemplo (Media Queries para Telas Retina):** Uma tela Retina tem uma densidade de pixels de pelo menos 2. Podemos fornecer imagens de maior resolução para estes ecrãs.

```css
.logo {
  background-image: url('logo-1x.png');
}

/* Se a tela tiver alta resolução, troca a imagem por uma versão 2x */
@media (min-resolution: 2dppx),
       (-webkit-min-device-pixel-ratio: 2) { /* Prefixo para navegadores mais antigos */
  .logo {
    background-image: url('logo-2x.png');
    background-size: contain; /* Ajusta o tamanho da imagem maior */
  }
}
```

#### Outras Características

- **`grid`**: Não se refere ao CSS Grid. É uma característica mais antiga que verifica se o dispositivo usa uma grade de caracteres (como um terminal `tty`) ou é um dispositivo de bitmap. O valor pode ser `0` (bitmap) ou `1` (grade).
- **`scan`**: Usado para televisões, verifica o processo de varredura (`progressive` ou `interlace`).

## Controlando Quebras de Página e Coluna (Fragmentação)

Quando o conteúdo é apresentado em um meio paginado (como uma impressão) ou em um layout de múltiplas colunas, o navegador precisa "quebrar" o conteúdo em fragmentos (páginas ou colunas). O módulo de fragmentação do CSS nos dá controle sobre como essas quebras acontecem.

### As Propriedades de Quebra

Existem duas gerações dessas propriedades. A mais antiga é a família `page-break-*`, e a mais moderna e preferida é a `break-*`.

- **`break-before`** (moderna) / **`page-break-before`** (legada): Controla se uma quebra forçada deve acontecer **antes** de um elemento.
- **`break-after`** (moderna) / **`page-break-after`** (legada): Controla se uma quebra forçada deve acontecer **depois** de um elemento.
- **`break-inside`** (moderna) / **`page-break-inside`** (legada): Controla se uma quebra é permitida **dentro** de um elemento.

### Valores Comuns

- `auto` (padrão): O navegador pode quebrar o conteúdo como achar melhor.
- `avoid`: O navegador deve tentar **evitar** uma quebra antes, depois ou dentro do elemento. Extremamente útil para evitar que uma figura e sua legenda (`<figcaption>`) sejam separadas.
- `always`: Força uma quebra sempre antes ou depois do elemento.
- `all`: Similar a `always`, mas se aplica a múltiplos contextos.
- `left`: Força uma ou duas quebras para garantir que o elemento seguinte comece em uma página da esquerda (em impressão frente e verso).
- `right`: Força uma ou duas quebras para garantir que o elemento seguinte comece em uma página da direita.
- `initial`, `inherit`: Valores globais.

**Exemplo Prático (Estilos de Impressão):**

```html
<article class="relatorio">
  <h1 class="capitulo-titulo">Capítulo 1: Introdução</h1>
  <p>Conteúdo do capítulo...</p>
  <figure class="imagem-importante">
    <img src="grafico.png" alt="Gráfico de vendas">
    <figcaption>Fig. 1: Gráfico de vendas do primeiro trimestre.</figcaption>
  </figure>
  <h1 class="capitulo-titulo">Capítulo 2: Desenvolvimento</h1>
  <p>Mais conteúdo...</p>
</article>
```

```css
@media print {
  /* Força cada novo capítulo a começar em uma nova página */
  .capitulo-titulo {
    break-before: always;
  }

  /* Tenta evitar que uma imagem e sua legenda sejam separadas em páginas diferentes */
  .imagem-importante {
    break-inside: avoid;
  }

  /* Evita uma quebra de página logo após um título */
  h1, h2, h3 {
    break-after: avoid;
  }
}
```

## Usando Media Queries no HTML

Além de usar a regra `@media` no seu arquivo CSS, você também pode aplicar uma media query diretamente no atributo `media` de uma tag `<link>`. Isso instrui o navegador a baixar e aplicar a folha de estilos **apenas se** a condição for atendida.

```html
<!-- Folha de estilos base para todos -->
<link rel="stylesheet" href="base.css">

<!-- Carrega esta folha de estilos apenas se a tela tiver 600px ou mais -->
<link rel="stylesheet" media="(min-width: 600px)" href="tablet.css">

<!-- Carrega esta folha de estilos apenas para impressão -->
<link rel="stylesheet" media="print" href="print.css">
```

**Vantagem:** Pode melhorar a performance inicial da página em dispositivos móveis, pois eles não precisam baixar os estilos mais pesados destinados a desktops.

## Boas Práticas com Media Queries

As Media Queries são a base do design responsivo. Usá-las de forma estratégica e organizada é crucial para criar sites que funcionem bem em todos os lugares.

- **Adote a Abordagem "Mobile First":** A melhor prática moderna é projetar e escrever os estilos para a menor tela primeiro (a base do seu CSS) e, em seguida, usar media queries com `min-width` para adicionar ou modificar estilos para telas maiores. Isso resulta em um CSS mais simples e performático para dispositivos móveis, que hoje representam a maioria do tráfego web.
- **Use Breakpoints Baseados no Conteúdo, não em Dispositivos:** Evite criar breakpoints baseados em tamanhos de dispositivos específicos (ex: "estilo para iPhone", "estilo para iPad"). Os dispositivos mudam constantemente. Em vez disso, redimensione a janela do seu navegador e adicione um breakpoint no ponto em que o seu _conteúdo_ começa a parecer "apertado" ou "quebrado". Deixe que o seu design dite os breakpoints.
- **Mantenha as Media Queries Próximas ao Componente:** Em vez de ter um grande arquivo `responsive.css` com todas as suas media queries, considere agrupar as regras responsivas junto com os estilos do componente ao qual elas se aplicam. Isso torna os componentes mais autocontidos e fáceis de manter.
- **Use Unidades Relativas (`em`, `rem`) nos Breakpoints:** Definir breakpoints com `em` ou `rem` em vez de `px` pode torná-los mais robustos. Se um usuário aumentar o zoom ou o tamanho da fonte padrão do navegador, os breakpoints baseados em `em`/`rem` se ajustarão proporcionalmente, garantindo que o layout mude no momento certo em relação ao tamanho do texto.
- **Não se Esqueça da Impressão:** Muitos utilizadores ainda imprimem páginas web. Crie uma pequena folha de estilos para `media="print"`. Use as propriedades de fragmentação (`break-before`, `break-inside`) para controlar as quebras de página, remova elementos desnecessários (como navegação e rodapés) e garanta que os links sejam exibidos por extenso para referência.

## Considerações Finais

Neste capítulo, desvendamos as Media Queries, a ferramenta essencial que permite ao nosso CSS ouvir e responder ao contexto do usuário. Vimos como a regra `@media`, combinada com características como `min-width` e `orientation`, nos dá o poder de esculpir layouts que se adaptam perfeitamente a qualquer tamanho de tela. Adicionamos ao nosso arsenal as propriedades de fragmentação, que nos dão um controle fino sobre como o conteúdo é apresentado em mídias paginadas, como a impressão.

Compreender e implementar a abordagem "Mobile First" não é apenas uma técnica; é uma filosofia de design que prioriza a experiência no ambiente mais restrito, resultando em sites mais rápidos e focados. Ao dominar as Media Queries e o controle de fragmentação, você deu o passo final para se tornar um desenvolvedor front-end completo, capaz de criar experiências web que não são apenas bonitas, mas verdadeiramente universais e acessíveis.
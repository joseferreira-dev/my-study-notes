# Capítulo 11 – A Arte da Tipografia na Web

Se o conteúdo é o rei, a tipografia é a sua voz. A vasta maioria da informação na web é transmitida através do texto. A forma como esse texto é apresentado — sua clareza, legibilidade, hierarquia e beleza — é um dos fatores mais críticos para o sucesso de um site. Uma boa tipografia guia o usuário, estabelece o tom da comunicação e torna a leitura uma experiência agradável. Uma tipografia ruim, por outro lado, pode tornar o conteúdo inacessível, confuso e afastar o visitante.

Controlar a tipografia no CSS vai muito além de simplesmente escolher uma fonte. É uma arte que envolve o domínio de um vasto conjunto de propriedades que governam o tamanho, o peso, o espaçamento, o alinhamento e a decoração do texto. É a habilidade de criar uma hierarquia visual clara, onde títulos, subtítulos e parágrafos se distinguem naturalmente, e de garantir que o texto seja legível em qualquer dispositivo.

Neste capítulo, faremos um mergulho profundo no controle tipográfico com CSS. Começaremos com as propriedades fundamentais da família `font`, que definem a aparência básica de uma fonte. Em seguida, exploraremos como refinar o layout do texto com propriedades de alinhamento, espaçamento e altura de linha. Veremos como adicionar efeitos e decorações com sombras e transformações. E, crucialmente, aprenderemos a quebrar as barreiras das "fontes seguras" e a usar qualquer fonte que desejarmos com a regra `@font-face` e os serviços de fontes externas. Ao final, você terá o conhecimento necessário para transformar texto simples em uma comunicação visualmente rica e eficaz.

## Propriedades Fundamentais da Fonte (`font-*`)

Este grupo de propriedades controla as características essenciais da fonte escolhida.

### `font-family`

Esta é talvez a propriedade mais importante. Ela define qual família de fontes (ou tipo de letra) será usada para renderizar o texto. Como não podemos garantir que um usuário terá uma fonte específica instalada em seu dispositivo, a melhor prática é fornecer uma lista de fontes priorizada, conhecida como **"empilhamento de fontes" (font stack)**. O navegador tentará usar a primeira fonte da lista; se não a encontrar, passará para a segunda, e assim por diante, até chegar a uma família genérica.

**Sintaxe:**

```css
.seletor {
  font-family: "Fonte Preferida", "Segunda Opção", familia-generica;
}
```

- **Nomes com espaços:** Devem ser colocados entre aspas (ex: `"Times New Roman"`).
- **Famílias genéricas:** São a última linha de defesa. As principais são:
    - `serif`: Fontes com pequenos traços nas extremidades das letras (ex: Times New Roman, Georgia).
    - `sans-serif`: Fontes sem serifa, com acabamento limpo (ex: Arial, Helvetica, Verdana).
    - `monospace`: Fontes onde todos os caracteres têm a mesma largura (ex: Courier New, Consolas).
    - `cursive`: Fontes que imitam a escrita à mão.
    - `fantasy`: Fontes decorativas.

**Exemplo Prático:**

```css
body {
  /* Tenta usar a fonte 'Roboto'. Se não estiver disponível, usa a 'Helvetica',
     depois a 'Arial' e, como última opção, qualquer fonte sans-serif padrão do sistema. */
  font-family: 'Roboto', 'Helvetica', 'Arial', sans-serif;
}
```

### `font-size`

Define o tamanho do texto. Como vimos no Capítulo 10, a unidade **`rem`** é a melhor prática para `font-size` por sua previsibilidade e por respeitar as configurações de acessibilidade do navegador do usuário.

**Exemplo:**

```css
html { font-size: 16px; } /* Base para o rem */
h1 { font-size: 2.5rem; } /* 40px */
p { font-size: 1rem; }    /* 16px */
```

### `font-weight`

Controla a "espessura" ou o "peso" da fonte. Pode ser definido com palavras-chave ou valores numéricos.

- **Palavras-chave:** `normal` (padrão), `bold`.
- **Valores numéricos:** de `100` a `900`, em incrementos de 100. `normal` é equivalente a `400` e `bold` a `700`.

**Importante:** A capacidade de renderizar diferentes pesos depende da família de fontes escolhida ter essas variações disponíveis.

**Exemplo:**

```css
.destaque {
  font-weight: bold; /* ou font-weight: 700; */
}
.sutil {
  font-weight: 300; /* Um peso mais leve, se a fonte suportar */
}
```

### `font-style`

Define o estilo da fonte, geralmente para aplicar itálico.

- `normal`: O texto é renderizado normalmente.
- `italic`: O texto é renderizado usando a versão itálica verdadeira da fonte (se disponível).
- `oblique`: Se a fonte não tiver uma versão itálica, o navegador "inclina" artificialmente a versão normal.

**Exemplo:**

```css
cite {
  font-style: italic;
}
```

### `font-variant`

Permite aplicar variações à fonte, como `small-caps`, que renderiza letras minúsculas como maiúsculas de tamanho reduzido.

**Exemplo:**

```css
h2.subtitulo {
  font-variant: small-caps;
}
```

### `font-stretch`

Permite selecionar uma versão condensada ou expandida de uma fonte, caso ela ofereça essas variações. Valores incluem `condensed`, `extra-condensed`, `expanded`, `normal`, etc.

### `font` (Shorthand)

Para evitar escrever todas as propriedades `font-*` separadamente, você pode usar a propriedade de atalho `font`. No entanto, ela exige uma ordem específica, e `font-size` e `font-family` são **obrigatórios**.

**Sintaxe:** `font: [font-style] [font-variant] [font-weight] [font-stretch] font-size[/line-height] font-family;`

**Exemplo:**

```css
p {
  /* Estilo itálico, variante small-caps, peso 700 (bold),
     tamanho 16px com altura de linha 24px, usando a fonte Georgia. */
  font: italic small-caps 700 16px/24px Georgia, serif;
}

/* Um exemplo mais simples e comum: */
body {
  font: 1rem/1.5 sans-serif; /* Tamanho 1rem, altura de linha 1.5, família sans-serif */
}
```

## Layout e Espaçamento do Texto

### `color`

Define a cor do texto. Lembre-se que um bom contraste entre a cor do texto e a cor de fundo é essencial para a legibilidade.

### `line-height`

Controla o espaçamento vertical entre as linhas de texto. Para uma boa legibilidade, um valor entre `1.4` e `1.8` é geralmente recomendado. **A melhor prática é usar um valor sem unidade.** Isso torna a altura da linha proporcional ao `font-size` do próprio elemento.

**Exemplo:**

```css
p {
  font-size: 16px;
  /* A altura de cada linha será 1.5 * 16px = 24px. */
  /* Se o font-size mudar, o line-height se ajustará automaticamente. */
  line-height: 1.5;
}
```

### `letter-spacing` e `word-spacing`

- `letter-spacing`: Adiciona ou remove espaço entre os **caracteres**.
- `word-spacing`: Adiciona ou remove espaço entre as **palavras**.

**Exemplo:**

```css
h1 {
  /* Aumenta o espaçamento entre as letras para um efeito mais "arejado" */
  letter-spacing: 2px;
}
```

### `text-align`

Controla o alinhamento horizontal do texto dentro de seu elemento contêiner.

- `left`: Alinha à esquerda (padrão para idiomas LTR).
- `right`: Alinha à direita.
- `center`: Centraliza o texto.
- `justify`: Estica o texto para que todas as linhas tenham a mesma largura, ajustando o espaçamento entre as palavras. Use com cuidado, pois pode criar "rios" de espaço em branco que dificultam a leitura.

### `text-indent`

Indenta (adiciona um recuo) à primeira linha de um bloco de texto.

**Exemplo:**

```css
p.primeiro-paragrafo {
  text-indent: 2em; /* Indenta a primeira linha em 2 vezes o tamanho da fonte */
}
```

## Decoração e Efeitos do Texto

### `text-decoration`

Esta é uma propriedade de atalho para decorar texto, mais comumente usada para sublinhar links. Ela controla três aspectos:

- `text-decoration-line`: O tipo de linha (`underline`, `overline`, `line-through`, `none`).
- `text-decoration-style`: O estilo da linha (`solid`, `wavy`, `dotted`, `dashed`, `double`).
- `text-decoration-color`: A cor da linha.

**Exemplo:**

```css
a {
  /* Atalho: linha ondulada, sublinhada e vermelha */
  text-decoration: underline wavy red;
}

/* É mais comum remover a decoração padrão dos links */
a.link-sem-traco {
  text-decoration: none;
}
```

### `text-transform`

Controla a capitalização do texto, independentemente de como ele foi escrito no HTML.

- `uppercase`: Transforma todo o texto em MAIÚSCULAS.
- `lowercase`: transforma todo o texto em minúsculas.
- `capitalize`: Transforma a primeira letra de cada palavra em maiúscula.

**Exemplo:**

```css
h1 {
  text-transform: uppercase;
}
```

### `text-shadow`

Adiciona uma ou mais sombras ao texto. **Sintaxe:** `text-shadow: offset-x | offset-y | blur-radius | color;`

- `offset-x`: Deslocamento horizontal.
- `offset-y`: Deslocamento vertical.
- `blur-radius` (opcional): O raio do desfoque. Um valor maior cria uma sombra mais suave.
- `color` (opcional): A cor da sombra.

**Exemplos:**

```css
/* Sombra simples, nítida e escura */
.sombra-simples {
  text-shadow: 2px 2px 0px #666;
}

/* Sombra suave e desfocada */
.sombra-suave {
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
}

/* Múltiplas sombras para um efeito de contorno (outline) */
.efeito-contorno {
  color: white;
  text-shadow:
    -1px -1px 0 #000,
     1px -1px 0 #000,
    -1px  1px 0 #000,
     1px  1px 0 #000;
}
```

### `text-overflow`

Define como o conteúdo que transborda (overflows) de seu contêiner deve ser sinalizado. Esta propriedade só funciona quando o contêiner tem `overflow: hidden;` e `white-space: nowrap;`.

- `ellipsis`: Exibe reticências ("...") para indicar que há mais texto.

**Exemplo:**

```css
.titulo-longo {
  width: 200px; /* Largura fixa */
  white-space: nowrap; /* Impede que o texto quebre linha */
  overflow: hidden; /* Esconde o texto que transborda */
  text-overflow: ellipsis; /* Adiciona "..." ao final */
}
```

### `quotes`

Permite especificar quais caracteres usar para as aspas de abertura e fechamento quando se usa a tag `<q>`.

**Exemplo:**

```css
/* Para textos em francês, usa aspas angulares */
:lang(fr) {
  quotes: "« " " »";
}
```

### Direção do Texto

- `direction`: Define a direcionalidade do texto. Os valores são `ltr` (left-to-right, padrão) e `rtl` (right-to-left), essencial para idiomas como árabe e hebraico.
- `writing-mode`: Uma propriedade mais avançada que define se as linhas de texto são dispostas horizontalmente ou verticalmente.

## Fontes na Web: A Regra `@font-face`

Por muito tempo, desenvolvedores estavam limitados a um pequeno conjunto de "fontes seguras para a web" que vinham pré-instaladas na maioria dos sistemas operacionais. A regra `@font-face` revolucionou isso, permitindo-nos carregar e usar arquivos de fontes customizadas em nossos sites.

### Fontes Externas (Serviços como Google Fonts)

Esta é a maneira mais fácil e comum de usar fontes customizadas. Serviços como o Google Fonts hospedam os arquivos de fonte para você e fornecem um link ou uma regra `@import` para incluir em seu CSS. O serviço cuida de todas as declarações `@font-face`.

**Exemplo (Google Fonts):**

1. Vá ao site do Google Fonts e escolha uma fonte (ex: "Lato").
2. Copie o código `<link>` fornecido e cole-o na seção `<head>` do seu HTML.
    
    ```css
    <head>
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Lato:wght@400;700&display=swap" rel="stylesheet">
    </head>
    ```
    
3. Use o `font-family` especificado em seu CSS.
    
    ```cc
    body {
      font-family: 'Lato', sans-serif;
    }
    ```

### Fontes Internas (Auto-hospedadas)

Se você tiver os arquivos de fonte (formatos `.woff2` e `.woff` são os padrões da web), você pode hospedá-los em seu próprio servidor e declará-los com `@font-face`.

**Exemplo:**

```css
@font-face {
  font-family: 'MinhaFonteCustomizada'; /* Dê um nome à sua fonte */
  src: url('caminho/para/minhafonte.woff2') format('woff2'),
       url('caminho/para/minhafonte.woff') format('woff'); /* Fallback */
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: 'MinhaFonteCustomizada', sans-serif;
}
```

## Boas Práticas de Tipografia

São boas práticas ao se trabalhar com tipografia:

- **Priorize a Legibilidade:** O objetivo principal da tipografia é a comunicação. Garanta um bom contraste de cor, um `font-size` adequado (mínimo de `1rem`/16px para o corpo do texto) e um `line-height` generoso (`1.5` a `1.7`).
- **Limite a Largura da Linha:** Para blocos de texto longos, use `max-width` (com a unidade `ch`, por exemplo) para limitar as linhas a cerca de 45-75 caracteres. Linhas muito longas são difíceis de ler.
- **Crie uma Hierarquia Clara:** Use `font-size`, `font-weight` e `color` para diferenciar claramente títulos, subtítulos e parágrafos. Isso ajuda os usuários a "escanear" a página.
- **Limite o Número de Fontes:** Usar mais de 2 ou 3 famílias de fontes diferentes em um site pode deixá-lo visualmente poluído e lento (devido ao carregamento de muitos arquivos). Escolha fontes que se complementem.
- **Use Font Stacks:** Sempre termine sua declaração de `font-family` com uma família genérica (`serif` ou `sans-serif`) para garantir que o texto seja renderizado de forma aceitável, mesmo que nenhuma de suas fontes preferidas esteja disponível.

## Considerações Finais

Neste capítulo, exploramos o vasto e detalhado mundo da tipografia no CSS. Vimos que temos um controle granular sobre cada aspecto do texto, desde a escolha da família da fonte com `font-family` e seu peso com `font-weight`, até o espaçamento preciso com `letter-spacing` e `line-height`. Aprendemos a decorar e transformar texto, adicionando efeitos como sombras e sublinhados customizados.

O ponto de virada foi a introdução do `@font-face`, a regra que nos libertou das limitações das fontes do sistema e nos permitiu usar qualquer tipo de letra, transformando o design web. Ao combinar essas propriedades com as boas práticas de legibilidade e hierarquia, você agora tem o poder não apenas de apresentar texto, mas de criar uma experiência de leitura verdadeiramente bem projetada.

Com este conhecimento, estamos prontos para avançar para outro pilar fundamental do design visual: os **planos de fundo**. No próximo capítulo, aprenderemos a ir além de cores sólidas, explorando imagens, gradientes e como manipular múltiplos backgrounds para criar profundidade e interesse visual.
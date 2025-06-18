# Capítulo 12 – Planos de Fundo (Backgrounds)

Após dominarmos a arte da tipografia, dando voz e personalidade ao nosso conteúdo, é hora de voltarmos nossa atenção para a tela sobre a qual esse conteúdo vive: o plano de fundo. A propriedade `background` no CSS é muito mais do que apenas uma forma de definir uma cor sólida. Ela é uma suíte de ferramentas completa que nos permite criar profundidade, textura, interesse visual e guiar o olhar do usuário através do design.

Um plano de fundo eficaz pode transformar um layout simples em uma experiência imersiva. Ele pode reforçar a identidade de uma marca, separar seções de conteúdo ou simplesmente adicionar um toque de elegância sutil. Com o CSS moderno, temos um controle sem precedentes sobre os backgrounds, permitindo-nos ir muito além das cores estáticas.

Neste capítulo, vamos explorar em profundidade as propriedades `background`. Começaremos com o básico, a `background-color`, e rapidamente avançaremos para o uso de imagens com `background-image`. Daremos uma atenção especial a uma das funcionalidades mais poderosas do CSS: a capacidade de gerar **gradientes** diretamente no navegador, tanto lineares quanto radiais. Em seguida, desvendaremos as propriedades que nos dão controle total sobre como uma imagem de fundo se comporta — seu tamanho, posição, repetição e como ela se fixa à página. Por fim, aprenderemos a combinar múltiplas camadas de fundo para criar efeitos ricos e complexos.

## `background-color`

Esta é a propriedade mais fundamental. Ela define uma cor de fundo sólida para um elemento. O valor pode ser qualquer formato de cor válido que vimos no Capítulo 7 (keyword, HEX, RGB, HSL, etc.).

**Exemplo:**

```css
body {
  /* Define um cinza muito claro como fundo para toda a página */
  background-color: #f4f4f4;
}

.alerta-erro {
  /* Usa rgba para uma cor de fundo semitransparente */
  background-color: rgba(220, 53, 69, 0.15);
  border: 1px solid rgba(220, 53, 69, 0.5);
}
```

## `background-image`

Esta propriedade permite definir uma ou mais imagens de fundo para um elemento. O valor mais comum é a função `url()`, que aponta para o caminho de um arquivo de imagem.

**Sintaxe:**

```css
.seletor {
  background-image: url('caminho/para/sua/imagem.jpg');
}
```

### Gradientes: Imagens Geradas por CSS

Uma das capacidades mais poderosas da propriedade `background-image` é que ela não aceita apenas URLs. Ela também pode aceitar imagens geradas pelo CSS, como os **gradientes**. Usar gradientes via CSS é muito mais performático e flexível do que usar um arquivo de imagem.

#### `linear-gradient()`

Cria uma imagem consistindo de uma transição progressiva entre duas ou mais cores ao longo de uma linha reta.

**Sintaxe:** `linear-gradient(direção, cor1, cor2, ...);`

- **Direção (opcional):** Pode ser um ângulo (`45deg`, `90deg`) ou palavras-chave como `to top`, `to bottom`, `to right`, `to left`. O padrão é `to bottom`.
- **Cores:** Uma lista de duas ou mais cores.

**Exemplo:**

```css
.caixa-gradiente-linear {
  /* Gradiente azul que vai da esquerda (skyblue) para a direita (steelblue) */
  background-image: linear-gradient(to right, skyblue, steelblue);
}

.caixa-gradiente-diagonal {
  /* Gradiente diagonal a 45 graus com três cores */
  background-image: linear-gradient(45deg, #ff9a9e, #fad0c4, #fad0c4);
}
```

#### `repeating-linear-gradient()`

Cria um gradiente linear que se repete indefinidamente. É fantástico para criar padrões como listras.

**Exemplo (Listras diagonais):**

```css
.fundo-listrado {
  background-image: repeating-linear-gradient(
    -45deg,
    #f0f0f0,
    #f0f0f0 10px, /* Faixa cinza claro de 10px */
    #e0e0e0 10px,
    #e0e0e0 20px  /* Faixa cinza mais escuro de 10px */
  );
}
```

#### `radial-gradient()`

Cria uma transição de cores que se irradia a partir de um ponto central.

**Sintaxe:** `radial-gradient(forma tamanho at posição, cor1, cor2, ...);`

- **Forma (opcional):** `circle` ou `ellipse` (padrão).
- **Tamanho e Posição (opcionais):** Controlam o tamanho e o centro do gradiente.

**Exemplo:**

```css
.fundo-radial {
  /* Gradiente circular que começa branco no centro e transita para azul claro */
  background-image: radial-gradient(circle, white, lightblue);
}
```

#### `repeating-radial-gradient()`

Cria um gradiente radial que se repete, ideal para criar padrões de alvos ou círculos concêntricos.

**Exemplo (Alvo):**

```css
.fundo-alvo {
  background-image: repeating-radial-gradient(
    circle,
    red,
    red 10px,
    white 10px,
    white 20px
  );
}
```

## Controlando a Imagem de Fundo

Uma vez que você tem uma imagem de fundo (seja de um arquivo ou um gradiente), você precisa de um conjunto de propriedades para controlar como ela é exibida.

### `background-repeat`

Define se e como uma imagem de fundo se repetirá.

- `repeat`: A imagem se repete horizontal e verticalmente (padrão).
- `no-repeat`: A imagem é exibida apenas uma vez.
- `repeat-x`: A imagem se repete apenas horizontalmente.
- `repeat-y`: A imagem se repete apenas verticalmente.

### `background-position`

Define a posição inicial de uma imagem de fundo. Pode usar palavras-chave (`top`, `center`, `bottom`, `left`, `right`) ou valores (`px`, `%`, `rem`).

**Exemplo:**

```css
.icone {
  background-image: url('icone.png');
  background-repeat: no-repeat;
  /* Posiciona o ícone 10px da esquerda e no centro vertical */
  background-position: 10px center;
}
```

### `background-size`

Esta é uma propriedade crucial para design responsivo. Ela define o tamanho da imagem de fundo.

- **Valores explícitos:** `background-size: 200px 150px;`
- `contain`: Redimensiona a imagem para ser o maior possível sem ser cortada, garantindo que ela esteja sempre totalmente visível. Pode deixar espaços vazios no contêiner.
- `cover`: Redimensiona a imagem para cobrir completamente a área do contêiner, mesmo que isso signifique cortar partes da imagem (a imagem nunca será distorcida).

A relação de aspecto (`aspect-ratio`) da imagem é sempre preservada com `contain` e `cover`.

**Exemplo (Imagem de herói de tela cheia):**

```css
.hero-section {
  height: 100vh;
  background-image: url('paisagem.jpg');
  background-repeat: no-repeat;
  background-position: center center;
  /* Garante que a imagem sempre cubra toda a seção, sem distorção */
  background-size: cover;
}
```

### `background-attachment`

Define se a posição de uma imagem de fundo é fixa dentro da viewport ou se ela rola junto com seu elemento contêiner.

- `scroll`: A imagem rola junto com a página (padrão).
- `fixed`: A imagem de fundo fica fixa em relação à viewport. Isso cria um **efeito de paralaxe**, onde o conteúdo rola "por cima" de um fundo estático.
- `local`: A imagem rola junto com o conteúdo do elemento, caso o elemento em si tenha uma barra de rolagem.

**Exemplo (Paralaxe):**

```css
.secao-paralaxe {
  background-image: url('fundo-distante.jpg');
  background-attachment: fixed;
  background-size: cover;
  background-position: center;
  height: 500px;
}
```

### `background-origin` e `background-clip`

Estas duas propriedades controlam a área de pintura do fundo.

- `background-origin`: Define a posição de origem do `background-position`. Os valores são `padding-box` (padrão), `border-box` e `content-box`.
- `background-clip`: Define até onde o fundo (cor ou imagem) se estende. Os valores são `border-box` (padrão), `padding-box` e `content-box`. Um valor especial é `text`, que recorta o fundo para a forma do texto (requer que a cor do texto seja transparente).

**Exemplo (Texto com fundo de imagem):**

```css
.titulo-com-fundo {
  font-size: 5rem;
  font-weight: bold;
  background-image: url('textura-metalica.jpg');
  -webkit-background-clip: text; /* Prefixos para compatibilidade */
  background-clip: text;
  color: transparent; /* O texto fica transparente para revelar o fundo */
}
```

## Múltiplos Backgrounds

O CSS permite aplicar múltiplas camadas de fundo a um único elemento. Basta listar as propriedades para cada fundo, separadas por vírgulas. A primeira imagem na lista fica no topo.

**Exemplo (Gradiente sobre imagem):**

```css
.fundo-combinado {
  /* Camada 1: Um gradiente preto semitransparente para escurecer a imagem */
  /* Camada 2: A imagem da paisagem */
  background-image:
    linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)),
    url('paisagem.jpg');

  background-repeat: no-repeat, no-repeat;
  background-position: center, center;
  background-size: cover, cover;
}
```

### `background-blend-mode`

Esta propriedade leva o conceito de múltiplos backgrounds a um novo patamar. Inspirada em softwares de edição de imagem como o Photoshop, `background-blend-mode` define como as múltiplas camadas de fundo de um elemento devem se misturar (ou "fundir") umas com as outras. Ela só tem efeito quando você tem pelo menos duas camadas de fundo (por exemplo, uma cor e uma imagem, ou duas imagens).

**Sintaxe:**

```css
background-blend-mode: modo-de-mesclagem;
```

Alguns dos modos mais comuns incluem:

- `normal`: Sem mesclagem (padrão).
- `multiply`: Multiplica as cores das camadas, resultando em uma cor mais escura.
- `screen`: O oposto de `multiply`, resultando em uma cor mais clara.
- `overlay`: Combina `multiply` e `screen`, preservando os realces e as sombras da camada inferior.
- `darken`, `lighten`, `color-dodge`, `color-burn`, `difference`, `exclusion`, `hue`, `saturation`, `color`, `luminosity`.

**Exemplo (Colorizando uma imagem em preto e branco):** Vamos pegar uma imagem em tons de cinza e aplicar uma "lavagem" de cor por cima usando `background-blend-mode`.

```html
<div class="imagem-colorizada"></div>
```

```css
.imagem-colorizada {
  width: 400px;
  height: 300px;
  /* Camada superior: uma cor sépia sólida */
  /* Camada inferior: uma imagem em tons de cinza */
  background-image:
    linear-gradient(#704214, #704214), /* Equivalente a uma cor sólida */
    url('[https://placehold.co/400x300/a9a9a9/ffffff?text=Imagem+P&B](https://placehold.co/400x300/a9a9a9/ffffff?text=Imagem+P&B)');

  background-size: cover;

  /* O modo 'color' pega o matiz e a saturação da camada superior (sépia)
     e a luminosidade da camada inferior (a imagem), colorizando-a. */
  background-blend-mode: color;
}
```

## `background` (Shorthand)

Para simplificar, você pode declarar todas as propriedades de um fundo em uma única linha usando a propriedade de atalho `background`.

**Sintaxe (ordem não é rígida, mas esta é comum):**

```css
background: [background-color] [background-image] [background-repeat] [background-attachment] [background-position] / [background-size] [background-origin] [background-clip];
```

A barra (`/`) antes do `background-size` só é necessária se `background-position` também for definido no atalho.

**Exemplo:**

```css
.hero-section {
  /* Cor de fallback, imagem, sem repetição, posição central, fixo, e cover */
  background: #333 url('paisagem.jpg') no-repeat center center fixed;
  background-size: cover; /* background-size precisa ser declarado separadamente em muitos casos */
}

/* Versão completa no atalho */
.hero-section-completo {
  background: url('paisagem.jpg') center / cover no-repeat;
}
```

## Boas Práticas ao Trabalhar com Backgrounds

São boas práticas:

- **Forneça uma Cor de Fallback:** Sempre declare um `background-color` junto com uma `background-image`. Se a imagem falhar ao carregar, o usuário ainda verá um fundo colorido, o que pode garantir que o texto sobre ele permaneça legível.
- **Otimize Imagens:** Imagens de fundo grandes são uma das principais causas de lentidão em sites. Comprima suas imagens usando ferramentas online ou offline antes de usá-las.
- **Garanta o Contraste:** Se houver texto sobre uma imagem de fundo, certifique-se de que o contraste seja alto o suficiente para garantir a legibilidade. A técnica de sobrepor um gradiente semitransparente (como no exemplo de múltiplos backgrounds) é excelente para isso.
- **Use CSS para Padrões:** Sempre que possível, crie padrões e texturas com `repeating-linear-gradient` ou `repeating-radial-gradient` em vez de usar arquivos de imagem. O resultado é infinitamente escalável e tem carregamento instantâneo.
- **Cuidado com `background-attachment: fixed`:** Embora visualmente atraente, o efeito de paralaxe pode ser custoso para a performance de renderização, especialmente em dispositivos móveis. Use-o com moderação e teste o desempenho.

## Considerações Finais

Neste capítulo, expandimos nossa paleta de design para muito além de cores sólidas, explorando o rico conjunto de propriedades `background` do CSS. Vimos como usar imagens para adicionar textura e contexto, e como gerar gradientes complexos diretamente no navegador, uma técnica poderosa para o design moderno.

Desvendamos as ferramentas essenciais para controlar o comportamento das imagens de fundo, como `background-size: cover` para criar fundos responsivos e `background-attachment: fixed` para efeitos de paralaxe impressionantes. Aprendemos também a combinar múltiplas camadas de fundo e a mesclá-las com `background-blend-mode`, permitindo uma criatividade quase ilimitada. Com um domínio sobre como preencher o fundo de nossos elementos, estamos agora prontos para começar a construir a estrutura visual de nossas páginas.
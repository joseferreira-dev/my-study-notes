# Capítulo 35 – Clipping e Masking: Revelando Conteúdo com Formas

No capítulo anterior, exploramos o lado artístico do CSS, aprendendo a construir formas geométricas com bordas, transformações e pseudo-elementos. Criamos elementos que _são_ a forma. Mas e se quisermos fazer o oposto? E se tivermos um elemento retangular, como uma imagem ou um vídeo, e quisermos que apenas uma parte dele, em um formato customizado, seja visível? Como podemos "cortar" ou "revelar" nosso conteúdo através de uma forma?

Para essa tarefa, o CSS nos oferece duas técnicas avançadas e extremamente poderosas: **Clipping (recorte)** e **Masking (mascaramento)**. Ambas permitem controlar a visibilidade de um elemento, mas de maneiras fundamentalmente diferentes.

- **Clipping**, através da propriedade `clip-path`, cria um caminho vetorial e torna visível apenas o que está **dentro** desse caminho. É como usar uma tesoura para cortar uma forma em um pedaço de papel: o corte é nítido, com uma borda dura.
- **Masking**, através da família de propriedades `mask`, usa uma imagem (ou um gradiente) como uma "máscara". A visibilidade de cada pixel do elemento é determinada pela cor ou transparência do pixel correspondente na máscara. Isso permite criar bordas suaves, efeitos de desvanecimento e recortes com transparência parcial.

Neste capítulo, vamos mergulhar nessas duas técnicas. Aprenderemos a criar recortes nítidos com as formas básicas e polígonos do `clip-path`. Em seguida, desvendaremos o poder das máscaras, vendo como podemos usar imagens e gradientes para criar efeitos de visibilidade complexos e até mesmo para "perfurar" um buraco no meio de um elemento.

## Clipping: Recortes Vetoriais com `clip-path`

A propriedade `clip-path` define uma região de recorte, mostrando apenas a parte do elemento que está dentro da forma especificada. Tudo fora da forma é invisível.

### `clip-path` com Formas Básicas (`basic-shape`)

Você pode usar funções de formas simples para definir a área de recorte.

- **`inset(top right bottom left round border-radius)`**: Cria um retângulo interno.
- **`circle(radius at position)`**: Cria um círculo.
- **`ellipse(rx ry at position)`**: Cria uma elipse.
- **`polygon(x1 y1, x2 y2, ...)`**: A mais poderosa. Define uma forma poligonal conectando uma série de pontos (coordenadas).

**Exemplo 1: `circle()`**

```html
<img src="https://placehold.co/300x300/6A5ACD/FFFFFF?text=Imagem" class="img-circulo" alt="Imagem">
```

```css
.img-circulo {
  /* Recorta a imagem em um círculo que ocupa 50% do tamanho no centro */
  clip-path: circle(50% at center);
}
```

**Exemplo 2: `polygon()` para criar um hexágono** Para criar um polígono, você precisa fornecer as coordenadas de cada um de seus vértices.

```html
<img src="https://placehold.co/300x250/28A745/FFFFFF?text=Hexágono" class="img-hexagono" alt="Imagem">
```

```css
.img-hexagono {
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}
```

### `clip-path` com SVG (`clip-source`)

Para formas muito complexas, é mais fácil definir o caminho em um arquivo SVG e referenciá-lo com `url()`.

```html
<svg width="0" height="0">
  <defs>
    <clipPath id="meu-clip-svg">
      <!-- Caminho complexo de um SVG aqui -->
      <path d="..."></path>
    </clipPath>
  </defs>
</svg>
```

```css
.elemento-com-clip-svg {
  clip-path: url(#meu-clip-svg);
}
```

### A Caixa de Referência (`geometry-box`)

Por padrão, o `clip-path` é aplicado à `border-box` do elemento. Você pode mudar a caixa de referência à qual as coordenadas se aplicam, como `content-box` ou `padding-box`.

```css
.elemento-com-clip-svg {
  clip-path: polygon(...) content-box; /* O polígono será relativo à caixa de conteúdo */
}
```

## Masking: Efeitos de Transparência com `mask`

O mascaramento é mais flexível que o recorte. Ele usa uma imagem de máscara (a `mask-image`) para determinar a opacidade de cada pixel do elemento. Por padrão, funciona com base na luminância:

- **Áreas pretas** na máscara tornam o elemento **totalmente transparente**.
- **Áreas brancas** na máscara deixam o elemento **totalmente opaco**.
- **Tons de cinza** ou áreas semitransparentes na máscara criam **transparência parcial**.

### As Propriedades de Máscara

A família `mask` funciona de forma muito semelhante à `background`, com propriedades para controlar a imagem, tamanho, posição e repetição.

- `mask-image` (`mask-reference`): Define a imagem da máscara. Aceita `url()` ou gradientes.
- `mask-size` (`bg-size`): Controla o tamanho da imagem da máscara (`auto`, `cover`, `contain`, etc.).
- `mask-position` (`position`): Define a posição da máscara.
- `mask-repeat` (`repeat-style`): Controla a repetição da máscara (`repeat`, `no-repeat`, etc.).
- `mask-mode`: Define se a máscara deve usar o canal alfa (`alpha`) ou a luminância (`luminance`) da imagem.
- `mask` (Shorthand): O atalho para todas as propriedades acima.

**Exemplo: Efeito de Desvanecimento com Gradiente**

```html
<div class="desvanecimento">
  <h2>Título Visível</h2>
  <p>Este texto vai desaparecendo suavemente na parte inferior, graças a uma máscara de gradiente que vai do branco (opaco) para o transparente.</p>
</div>
```

```css
.desvanecimento {
  /* A máscara é um gradiente linear que começa branco (totalmente visível)
     e transita para preto (totalmente invisível). */
  mask-image: linear-gradient(to bottom, white 50%, transparent);
}
```

### Técnica Avançada: Cortando um Buraco com Máscara

Uma das técnicas mais poderosas é usar um gradiente para "perfurar" um buraco em um elemento, tornando uma área central transparente enquanto o resto permanece opaco. Isso é perfeito para criar sobreposições (overlays) com uma "janela" de foco.

**Exemplo:**

```html
<div class="overlay-com-buraco">
  <!-- O conteúdo da página fica aqui, embaixo do overlay -->
</div>
```

```css
.overlay-com-buraco {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.7); /* Fundo escuro semitransparente */

  /* A máscara é um gradiente radial.
     O centro do gradiente é transparente (o buraco).
     A partir de 150px, ele se torna branco (opaco), revelando o resto do overlay. */
  mask-image: radial-gradient(
    circle at center,
    transparent 0,
    transparent 150px, /* O tamanho do buraco */
    black 151px /* A partir daqui, a máscara fica preta (opaca no modo alpha) */
  );

  /* Precisamos mudar o modo da máscara. No modo luminance (padrão), preto é transparente e
     branco é opaco. Aqui, usamos o modo alpha. Se a cor da máscara tiver alpha 0
     (como 'transparent'), essa parte do elemento será escondida. */
  mask-mode: alpha;
}
```

**Resultado:** Uma sobreposição escura cobrirá toda a tela, mas com um círculo perfeitamente transparente no centro, revelando o conteúdo abaixo.

## Boas Práticas com Clipping e Masking

Estas técnicas abrem um mundo de possibilidades criativas, mas seu uso eficaz depende de entender seus pontos fortes e fracos.

- **Escolha a Ferramenta Certa para a Tarefa:** A principal diferença é a borda. Se você precisa de um recorte com borda nítida e bem definida (hard-edge), **`clip-path` é a escolha ideal e mais performática**. Se você precisa de bordas suaves, desvanecimentos ou efeitos de transparência complexos, **`mask` é a ferramenta correta**.
- **Use SVG para Formas Complexas:** Para `clip-path` ou `mask-image`, criar formas complexas com `polygon()` pode ser tedioso e difícil de manter. É muito mais eficiente e legível desenhar a forma em um software vetorial, exportá-la como SVG e referenciá-la com `url()`.
- **Acessibilidade é Prioridade:** Lembre-se que tanto o recorte quanto o mascaramento podem esconder partes do seu conteúdo. Certifique-se de que nenhuma informação crucial (como texto dentro de uma imagem) seja tornada inacessível. O conteúdo ainda existe para leitores de tela, mas pode não ser visível.
- **Considere a Performance:** Animar `clip-path` em polígonos com muitos pontos ou usar máscaras de imagem de alta resolução pode ser custoso para o navegador. Teste sempre a performance, especialmente em dispositivos móveis.
- **Forneça Fallbacks (se necessário):** Embora o suporte do navegador seja bom para a maioria dessas propriedades, em projetos que precisam suportar navegadores muito antigos, pode ser necessário fornecer uma alternativa (como simplesmente mostrar o elemento sem recorte).

## Considerações Finais

Neste capítulo, exploramos as técnicas de `clip-path` e `mask`, duas das ferramentas mais criativas do arsenal do CSS. Vimos como o `clip-path` nos permite criar recortes vetoriais com bordas nítidas, ideais para formas geométricas, e como o `mask` nos oferece um controle em nível de pixel sobre a visibilidade, permitindo efeitos de desvanecimento e transparência complexos.

A capacidade de aplicar formas não retangulares a elementos de conteúdo abre novas dimensões para o design web, permitindo-nos quebrar a monotonia da grade e criar layouts visualmente mais dinâmicos e interessantes. Com este conhecimento, você pode agora ir além da estilização de caixas e começar a esculpir a aparência visual de seus elementos com verdadeira intenção artística.
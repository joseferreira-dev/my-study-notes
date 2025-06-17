# Capítulo 7 – O Universo das Cores no CSS

Após dominarmos o "onde" e o "quê" da estilização através de um estudo aprofundado dos seletores, é hora de focarmos no "como". E não há propriedade de estilo mais fundamental e visualmente impactante do que a cor. A cor é a alma de um design; ela evoca emoções, cria hierarquia visual, melhora a legibilidade e define a identidade de uma marca. Sem um controle preciso sobre as cores, nossos designs seriam monocromáticos e sem vida.

O CSS nos oferece um leque variado e poderoso de opções para definir cores, indo muito além de simples nomes. Cada método tem suas próprias características, vantagens e casos de uso, desde os convenientes nomes de cores para prototipagem rápida até os modelos funcionais que nos dão controle total sobre a tonalidade, saturação, brilho e, crucialmente, a transparência.

Neste capítulo, faremos uma imersão completa no universo das cores no CSS. Começaremos com a palavra-chave especial e dinâmica `currentColor`. Em seguida, exploraremos os métodos mais tradicionais, como as palavras-chave de cores (`Color Keywords`) e o onipresente formato **Hexadecimal**. Por fim, desvendaremos os modelos funcionais modernos e mais flexíveis: **RGB/RGBA**, que nos permite misturar luzes vermelha, verde e azul, e **HSL/HSLA**, um modelo mais intuitivo para humanos, baseado em matiz, saturação e luminosidade. Ao final, você terá um domínio completo sobre como especificar, manipular e aplicar cores de forma eficaz e profissional em seus projetos.

## Uma Palavra-chave Dinâmica: `currentColor`

Antes de mergulhar nos formatos de cor, vamos conhecer um valor especial e extremamente útil: `currentColor`. Esta palavra-chave é uma variável cujo valor é sempre o valor da propriedade `color` do elemento. Em outras palavras, ela permite que outras propriedades, como `border-color` ou `background-color`, herdem a cor do texto do elemento de forma dinâmica.

**Exemplo Prático: Ícones que acompanham a cor do texto** `currentColor` é fantástica para criar componentes consistentes, como botões com ícones SVG, onde a cor do ícone deve ser a mesma do texto do botão.

```html
<button class="botao-alerta">
  <svg viewBox="0 0 24 24" width="24" height="24">
    <!-- O `fill` do SVG usará a cor do texto do botão -->
    <path fill="currentColor" d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-2h2v2zm0-4h-2V7h2v6z"></path>
  </svg>
  Atenção
</button>
```

```css
.botao-alerta {
  color: #D9534F; /* Cor vermelha para o texto */
  border: 2px solid currentColor; /* A borda terá a mesma cor vermelha do texto */
  background-color: transparent;
  padding: 10px 15px;
  border-radius: 5px;
}

.botao-alerta:hover {
  color: white;
  background-color: #D9534F; /* A cor de fundo no hover é a cor original */
}
```

Neste exemplo, a borda do botão e o preenchimento do ícone SVG usarão automaticamente o valor da propriedade `color`. Quando passamos o mouse, a cor do texto muda para branco, e a cor do ícone SVG e da borda (se ainda fosse visível) também mudariam para branco, criando um componente coeso e fácil de manter.

## Palavras-chave de Cores (Color Keywords)

Este é o método mais simples de definir uma cor. O CSS define uma lista de mais de 140 nomes de cores padronizados que podem ser usados diretamente. Inclui cores simples como `red`, `blue` e `green`, e nomes mais descritivos como `tomato`, `skyblue` e `lightslategray`.

**Sintaxe:**

```css
.seletor {
  color: nomedacor;
}
```

**Exemplo Prático:**

```css
h1 {
  color: steelblue;
}
p {
  color: dimgray;
}
.aviso {
  background-color: gold;
}
```

**Vantagens:** Rápido, fácil de ler e ótimo para prototipagem ou testes rápidos. **Desvantagens:** A seleção de cores é limitada e não oferece controle fino para paletas de cores de marcas específicas.

## Formato Hexadecimal (#RRGGBB)

Este é um dos formatos mais populares e amplamente utilizados na web. Ele representa as cores no modelo de cor **RGB (Red, Green, Blue)** usando notação hexadecimal.

Uma cor hexadecimal começa com uma cerquilha (`#`), seguida por seis dígitos hexadecimais (0-9 e A-F). Os dois primeiros dígitos representam a intensidade do canal **vermelho (R)**, os dois seguintes o canal **verde (G)** e os dois últimos o canal **azul (B)**. O valor `00` representa intensidade mínima e `FF` (255 em decimal) representa intensidade máxima.

**Sintaxe:**

```css
.seletor {
  color: #RRGGBB;
}
```

**Exemplos:**

- `#FF0000`: Vermelho puro (máximo de vermelho, nenhum verde, nenhum azul).
- `#00FF00`: Verde puro.
- `#0000FF`: Azul puro.
- `#FFFFFF`: Branco (máximo de todas as cores).
- `#000000`: Preto (mínimo de todas as cores).
- `#E3E3E3`: Um cinza claro.

### Shorthand Hexadecimal (#RGB)

Se cada par de dígitos for idêntico (ex: `#AA11BB`), você pode usar uma forma abreviada com apenas três dígitos. O navegador duplicará cada dígito.

- `#F0C` é o mesmo que `#FF00CC`.
- `#A3B` é o mesmo que `#AA33BB`.

**Sintaxe Abreviada:**

```css
.seletor {
  color: #RGB;
}
```

**Transparência (Hex com Alpha - #RRGGBBAA):** Uma adição mais recente ao CSS permite especificar um quarto par de dígitos para o canal **alfa (transparência)**. `00` é totalmente transparente e `FF` é totalmente opaco.

- `#FF000080`: Vermelho puro com 50% de opacidade (80 em hexadecimal é 128 em decimal, que é aproximadamente 50% de 255).

## Modelo Funcional `rgb()` e `rgba()`

Este modelo também usa o sistema de cores RGB, mas com uma notação funcional e valores decimais, que podem ser mais fáceis de entender.

### `rgb(red, green, blue)`

A função `rgb()` aceita três valores numéricos (de 0 a 255) ou em porcentagem (de 0% a 100%) para os canais de vermelho, verde e azul.

**Sintaxe:**

```css
.seletor {
  color: rgb(valor_r, valor_g, valor_b);
}
```

**Exemplos:**

- `rgb(255, 0, 0)`: Vermelho puro.
- `rgb(0, 0, 0)`: Preto.
- `rgb(51, 51, 51)`: Um cinza escuro, equivalente a `#333333`.

### `rgba(red, green, blue, alpha)`

A função `rgba()` adiciona um quarto valor: o canal **alfa (alpha)**, que controla a opacidade. Este valor vai de **0.0 (totalmente transparente) a 1.0 (totalmente opaco)**.

**Sintaxe:**

```css
.seletor {
  color: rgba(valor_r, valor_g, valor_b, valor_alpha);
}
```

**Exemplo Prático: Overlay semitransparente** `rgba()` é perfeito para criar sobreposições de cores em imagens ou vídeos.

```css
.imagem-container {
  position: relative;
}

.imagem-container::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* Fundo preto com 60% de opacidade */
  background-color: rgba(0, 0, 0, 0.6);
}
```

## Modelo Funcional `hsl()` e `hsla()`

O modelo **HSL (Hue, Saturation, Lightness)** é frequentemente considerado mais intuitivo para humanos, pois se alinha com a forma como percebemos as cores.

- **Hue (Matiz):** É um grau no círculo cromático, de 0 a 360. 0 é vermelho, 120 é verde e 240 é azul.
- **Saturation (Saturação):** É a intensidade ou pureza da cor, expressa em porcentagem. 0% é um tom de cinza (dessaturado), e 100% é a cor plena.
- **Lightness (Luminosidade):** É o brilho da cor, expresso em porcentagem. 0% é preto, 50% é a cor "normal" e 100% é branco.

### `hsl(hue, saturation, lightness)`

**Sintaxe:**

```css
.seletor {
  color: hsl(matiz, saturacao%, luminosidade%);
}
```

**Exemplos:**

- `hsl(0, 100%, 50%)`: Vermelho puro.
- `hsl(120, 100%, 50%)`: Verde puro.
- `hsl(120, 100%, 25%)`: Um verde escuro.
- `hsl(120, 60%, 70%)`: Um verde pastel claro.

A grande vantagem do HSL é a facilidade de criar paletas de cores. Você pode manter o mesmo matiz (`hue`) e apenas variar a saturação e a luminosidade para obter tons monocromáticos consistentes.

### `hsla(hue, saturation, lightness, alpha)`

Assim como `rgba()`, a função `hsla()` adiciona um quarto valor para o canal alfa (opacidade), de 0.0 a 1.0.

**Exemplo Prático: Botões com variações de cor**

```css
:root {
  --cor-primaria-h: 210; /* Matiz azul */
  --cor-primaria-s: 100%;
}

.botao {
  background-color: hsl(var(--cor-primaria-h), var(--cor-primaria-s), 50%); /* Azul principal */
  color: white;
}

.botao:hover {
  background-color: hsl(var(--cor-primaria-h), var(--cor-primaria-s), 40%); /* Mesmo azul, mais escuro */
}

.botao-desabilitado {
  background-color: hsl(var(--cor-primaria-h), 30%, 60%); /* Mesmo azul, dessaturado e mais claro */
  cursor: not-allowed;
}
```

## Tabela de Referência de Cores

A seguir, uma tabela com uma seleção de palavras-chave de cores comuns e seus valores equivalentes nos formatos HEX, RGB e HSL para sua referência.

|Palavra-chave (Keyword)|Hexadecimal|RGB|HSL|
|---|---|---|---|
|`black`|`#000000`|`rgb(0, 0, 0)`|`hsl(0, 0%, 0%)`|
|`white`|`#FFFFFF`|`rgb(255, 255, 255)`|`hsl(0, 0%, 100%)`|
|`silver`|`#C0C0C0`|`rgb(192, 192, 192)`|`hsl(0, 0%, 75%)`|
|`gray`|`#808080`|`rgb(128, 128, 128)`|`hsl(0, 0%, 50%)`|
|`dimgray`|`#696969`|`rgb(105, 105, 105)`|`hsl(0, 0%, 41%)`|
|`lightgray`|`#D3D3D3`|`rgb(211, 211, 211)`|`hsl(0, 0%, 83%)`|
|`red`|`#FF0000`|`rgb(255, 0, 0)`|`hsl(0, 100%, 50%)`|
|`maroon`|`#800000`|`rgb(128, 0, 0)`|`hsl(0, 100%, 25%)`|
|`tomato`|`#FF6347`|`rgb(255, 99, 71)`|`hsl(9, 100%, 64%)`|
|`indianred`|`#CD5C5C`|`rgb(205, 92, 92)`|`hsl(0, 53%, 58%)`|
|`green`|`#008000`|`rgb(0, 128, 0)`|`hsl(120, 100%, 25%)`|
|`lime`|`#00FF00`|`rgb(0, 255, 0)`|`hsl(120, 100%, 50%)`|
|`limegreen`|`#32CD32`|`rgb(50, 205, 50)`|`hsl(120, 61%, 50%)`|
|`seagreen`|`#2E8B57`|`rgb(46, 139, 87)`|`hsl(146, 50%, 36%)`|
|`blue`|`#0000FF`|`rgb(0, 0, 255)`|`hsl(240, 100%, 50%)`|
|`navy`|`#000080`|`rgb(0, 0, 128)`|`hsl(240, 100%, 25%)`|
|`royalblue`|`#4169E1`|`rgb(65, 105, 225)`|`hsl(225, 73%, 57%)`|
|`skyblue`|`#87CEEB`|`rgb(135, 206, 235)`|`hsl(197, 71%, 73%)`|
|`steelblue`|`#4682B4`|`rgb(70, 130, 180)`|`hsl(207, 44%, 49%)`|
|`yellow`|`#FFFF00`|`rgb(255, 255, 0)`|`hsl(60, 100%, 50%)`|
|`gold`|`#FFD700`|`rgb(255, 215, 0)`|`hsl(51, 100%, 50%)`|
|`orange`|`#FFA500`|`rgb(255, 165, 0)`|`hsl(39, 100%, 50%)`|
|`purple`|`#800080`|`rgb(128, 0, 128)`|`hsl(300, 100%, 25%)`|
|`fuchsia`|`#FF00FF`|`rgb(255, 0, 255)`|`hsl(300, 100%, 50%)`|
|`chocolate`|`#D2691E`|`rgb(210, 105, 30)`|`hsl(25, 76%, 47%)`|

## Considerações Finais

Neste capítulo, desvendamos as múltiplas maneiras de se trabalhar com cores no CSS. Vimos a utilidade da palavra-chave `currentColor` para criar componentes coesos. Analisamos os formatos clássicos como as `Color Keywords`, ótimas para prototipagem, e o `Hexadecimal`, um padrão da indústria para valores de cor sólidos.

O grande salto em flexibilidade, no entanto, veio com os modelos funcionais. O `rgb()` e, principalmente, o `rgba()` nos deram controle total sobre a opacidade, abrindo as portas para efeitos de transparência e sobreposição. O `hsl()` e o `hsla()` se apresentaram como uma alternativa ainda mais poderosa e intuitiva, permitindo-nos manipular cores de uma forma que se assemelha ao pensamento humano e facilitando imensamente a criação de paletas de cores dinâmicas e harmoniosas. Com este profundo conhecimento sobre cores, estamos agora prontos para aplicá-las em contextos mais complexos.
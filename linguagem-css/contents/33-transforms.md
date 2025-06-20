# Capítulo 33 – Transformações 2D e 3D

Até agora, nossa jornada pelo CSS se concentrou em controlar o layout, o espaçamento e a aparência dos elementos em seu estado "natural". Definimos suas dimensões, cores e posições no fluxo do documento. Mas e se quiséssemos ir além do layout estático e manipular a própria geometria de um elemento? E se pudéssemos rotacionar um cartão, escalar um botão ao passar o mouse ou mover um elemento pela tela para uma animação, tudo isso sem perturbar o layout ao seu redor?

É aqui que entra a poderosa propriedade **`transform`**. As transformações CSS nos permitem modificar o espaço de coordenadas de um elemento, alterando sua forma, tamanho e posição. Elas são a base para a criação de interfaces dinâmicas, animações complexas e efeitos visuais que adicionam profundidade e interatividade a um design. Uma das suas maiores vantagens é a performance: a maioria das transformações é acelerada pela GPU do dispositivo, resultando em animações muito mais suaves do que seria possível ao manipular propriedades de layout como `margin` ou `top`.

Neste capítulo, faremos um mergulho profundo no mundo das transformações. Começaremos com as **transformações 2D**, aprendendo a transladar, rotacionar, escalar e inclinar elementos em um plano bidimensional. Em seguida, expandiremos nossos horizontes para o **espaço 3D**, desvendando como criar perspectiva e manipular elementos no eixo Z para construir objetos e cenas tridimensionais.

## O Básico: A Propriedade `transform`

A propriedade `transform` é o ponto de partida para todas as transformações. Ela aceita uma ou mais **funções de transformação** como seu valor, que são aplicadas em sequência.

**Sintaxe:** `transform: funcao1(valor) funcao2(valor) ...;`

É crucial entender que as transformações **não afetam o fluxo do documento**. Um elemento transformado pode se sobrepor visualmente a outros, mas o espaço que ele originalmente ocupava no layout é preservado.

## Transformações 2D

As transformações 2D operam no plano x (horizontal) e y (vertical).

### `translate(tx, ty)`, `translateX(tx)`, `translateY(ty)`

A função `translate()` **move** um elemento de sua posição original.

- **`translate(tx, ty)`**: Move o elemento `tx` no eixo X e `ty` no eixo Y.
- **`translateX(tx)`**: Move o elemento apenas horizontalmente.
- **`translateY(ty)`**: Move o elemento apenas verticalmente. Os valores (`length-or-percentage`) podem ser em `px`, `em`, `%`, etc. Uma porcentagem se refere ao tamanho do próprio elemento.

**Exemplo:**

```html
<div class="box translate-example"></div>
```

```css
.translate-example {
  /* Move 50px para a direita e 25px para baixo */
  transform: translate(50px, 25px);
  transition: transform 0.3s ease;
}
.translate-example:hover {
  /* Move 100% de sua própria largura para a direita */
  transform: translateX(100%);
}
```

### `rotate(angle)`

**Rotaciona** um elemento em torno de seu ponto de origem. O valor (`angle`) é um ângulo, geralmente em graus (`deg`). Um valor positivo rotaciona no sentido horário.

**Exemplo:**

```html
<div class="box rotate-example"></div>
```

```css
.rotate-example {
  transition: transform 0.4s ease-in-out;
}
.rotate-example:hover {
  /* Rotaciona 45 graus no sentido horário */
  transform: rotate(45deg);
}
```

### `scale(sx, sy)`, `scaleX(sx)`, `scaleY(sy)`

**Redimensiona** (escala) um elemento. O valor (`scale-factor`) é um número sem unidade.

- `1`: Tamanho normal.
- `2`: Dobro do tamanho.
- `0.5`: Metade do tamanho.
- **`scale(sx, sy)`**: Escala o elemento nos eixos X e Y. Se apenas um valor for fornecido, ele se aplica a ambos os eixos.
- **`scaleX(sx)`**: Escala apenas na horizontal.
- **`scaleY(sy)`**: Escala apenas na vertical.

**Exemplo:**

```html
<div class="box scale-example"></div>
```

```css
.scale-example:hover {
  /* Aumenta o elemento em 20% */
  transform: scale(1.2);
}
```

### `skew(ax, ay)`, `skewX(ax)`, `skewY(ay)`

**Inclina** um elemento ao longo dos eixos X e Y. O valor (`angle`) é um ângulo (`deg`).

**Exemplo:**

```html
<div class="box skew-example"></div>
```

```css
.skew-example:hover {
  /* Inclina 20 graus no eixo X e 10 graus no eixo Y */
  transform: skew(20deg, 10deg);
}
```

### `transform-origin`

Por padrão, todas as transformações ocorrem a partir do centro do elemento (`50% 50%`). A propriedade `transform-origin` permite que você **mude este ponto de pivô**.

**Exemplo: Rotacionando um ponteiro de relógio**

```html
<div class="ponteiro"></div>
```

```css
.ponteiro {
  width: 4px;
  height: 100px;
  background: black;
  /* Define o ponto de origem como a base central do ponteiro */
  transform-origin: bottom center;
  transform: rotate(30deg);
}
```

### Combinando Múltiplas Transformações

Você pode aplicar várias funções em uma única declaração `transform`. **A ordem importa!** As transformações são aplicadas da direita para a esquerda.

```css
/* Rotaciona primeiro, depois move */
transform: translateX(100px) rotate(45deg);

/* Move primeiro, depois rotaciona em torno do novo centro */
transform: rotate(45deg) translateX(100px);
```

O resultado visual dessas duas linhas será diferente.

### `matrix()`

Esta função é a forma mais poderosa de transformação 2D. Ela combina todas as outras (translate, rotate, scale, skew) em uma única matriz matemática. Sua sintaxe é `matrix(a, b, c, d, e, f)`. Geralmente, ela não é escrita à mão, mas é o que o navegador usa internamente e o que ferramentas de animação podem gerar.

## Transformações 3D

Para que as transformações 3D tenham efeito, precisamos primeiro criar um "palco" com profundidade.

### Criando o Espaço 3D: `perspective`

A propriedade `perspective` é aplicada ao **elemento pai** e define a intensidade da perspectiva para seus filhos. Um valor menor cria uma perspectiva mais "extrema" e distorcida, como se estivéssemos muito perto do objeto. Um valor maior cria uma perspectiva mais sutil.

**Sintaxe:** `perspective: length;` (ex: `800px`).

### `transform-style: preserve-3d`

Quando um elemento é transformado em 3D, seus filhos, por padrão, são achatados em seu plano. Para criar um objeto 3D composto, como um cubo, o elemento contêiner precisa de `transform-style: preserve-3d;`. Isso garante que seus filhos existam no mesmo espaço 3D compartilhado.

### Funções de Transformação 3D

As funções de transformação 3D são:

- **`translate3d(tx, ty, tz)`**, **`translateZ(tz)`**: Move o elemento no eixo Z (para frente e para trás, em direção ao usuário).
- **`scale3d(sx, sy, sz)`**, **`scaleZ(sz)`**: Escala o elemento no eixo Z.
- **`rotate3d(x, y, z, angle)`**, **`rotateX(angle)`**, **`rotateY(angle)`**, **`rotateZ(angle)`**: Rotacionam o elemento em torno dos eixos X (horizontal), Y (vertical) ou Z (profundidade).

### `backface-visibility`

Define se a "face de trás" de um elemento é visível quando ele é rotacionado. Os valores são `visible` (padrão) ou `hidden`.

### Exemplo 1: Efeito de Texto 3D

Podemos simular um efeito de texto 3D extrudado combinando `text-shadow` e uma leve rotação 3D.

```html
<h1 class="texto-3d">3D</h1>
```

```css
body {
  perspective: 800px; /* Cria a perspectiva para a cena */
}
.texto-3d {
  font-size: 8rem;
  color: #E91E63;
  transform-style: preserve-3d;
  transform: rotateY(-30deg) rotateX(20deg);
  /* Cria múltiplas sombras deslocadas para dar a ilusão de profundidade */
  text-shadow: 1px 1px 0 #c3134f, 2px 2px 0 #c3134f, 3px 3px 0 #c3134f, 4px 4px 0 #c3134f, 5px 5px 15px rgba(0,0,0,0.3);
}
```

### Exemplo 2: Criando um Cubo 3D

Este é o exemplo clássico que une todos os conceitos 3D.

```html
<div class="cena">
  <div class="cubo">
    <div class="face frente">1</div>
    <div class="face tras">2</div>
    <div class="face direita">3</div>
    <div class="face esquerda">4</div>
    <div class="face topo">5</div>
    <div class="face base">6</div>
  </div>
</div>
```

```css
.cena {
  width: 200px;
  height: 200px;
  perspective: 1000px;
}
.cubo {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transform: translateZ(-100px);
  transition: transform 1s;
}
.cubo:hover {
  transform: translateZ(-100px) rotateY(-90deg);
}
.face {
  position: absolute;
  width: 200px;
  height: 200px;
  background: rgba(255, 0, 0, 0.7);
  border: 1px solid black;
  font-size: 2rem;
  text-align: center;
  line-height: 200px;
}
/* Posicionando cada face no espaço 3D */
.frente { transform: rotateY(0deg) translateZ(100px); }
.tras { transform: rotateY(180deg) translateZ(100px); }
.direita { transform: rotateY(90deg) translateZ(100px); }
.esquerda { transform: rotateY(-90deg) translateZ(100px); }
.topo { transform: rotateX(90deg) translateZ(100px); }
.base { transform: rotateX(-90deg) translateZ(100px); }
```

## Boas Práticas com Transformações

As transformações são uma das ferramentas mais performáticas do CSS para animações, mas seu uso requer cuidado para garantir uma experiência de usuário suave e acessível.

- **Use `transform` para Animações, não para Layout:** A principal vantagem das transformações é a performance. Sempre que precisar animar a posição, rotação ou escala de um elemento, use `transform` em vez de manipular propriedades de layout como `margin`, `left`, `top` ou `width`. Mudar o layout é caro para o navegador (causa "reflow"), enquanto `transform` geralmente só exige um "repaint" na GPU.
- **Lembre-se que o Fluxo não é Afetado:** Um elemento transformado não afeta a posição dos elementos ao seu redor. O espaço que ele ocupava originalmente é mantido. Não use `transform: translateX(20px)` para criar um espaçamento; use `margin-left: 20px` para isso.
- **A Ordem das Funções é Crucial:** A ordem em que você lista as funções de transformação na propriedade `transform` afeta drasticamente o resultado final. Experimente e entenda a sequência que produz o efeito desejado.
- **`transform-origin` é seu Amigo:** Não se esqueça de que você pode mudar o ponto central de qualquer transformação. Para animações de UI, como um menu que "desliza" de um canto, definir o `transform-origin` correto é essencial.
- **Cuidado com a Legibilidade do Texto:** Rotacionar ou inclinar texto pode dificultar muito a leitura. Use essas transformações com moderação em elementos de texto e garanta que a legibilidade não seja comprometida.
- **A Perspectiva é do Pai:** Lembre-se que a propriedade `perspective` deve ser aplicada ao contêiner pai para criar a "cena" 3D para seus filhos. Sem ela, as transformações 3D parecerão planas.

## Considerações Finais

Neste capítulo, exploramos o excitante mundo das transformações CSS. Vimos como podemos mover, rotacionar, escalar e inclinar elementos em um plano 2D, e como podemos estender esses conceitos para um espaço 3D para criar objetos e cenas com profundidade e perspectiva.

Compreender que as transformações são uma ferramenta de manipulação visual, separada do fluxo de layout do documento, e que são altamente performáticas para animações é a chave para seu uso eficaz. Elas são o motor por trás das interfaces de usuário modernas e dinâmicas. Com este conhecimento, você está agora perfeitamente preparado para o próximo passo lógico: dar vida a essas transformações ao longo do tempo.
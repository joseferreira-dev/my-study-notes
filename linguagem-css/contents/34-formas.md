# Capítulo 34 – Construindo Formatos Únicos com CSS

A web, em sua essência, é construída sobre uma fundação de retângulos. Cada elemento, como vimos no Box Model, é uma caixa. No entanto, o design visual raramente se limita a quadrados e retângulos. Formas como triângulos, círculos, trapézios e estrelas são parte integrante da linguagem visual, usadas para criar ícones, balões de fala, setas e elementos decorativos que tornam uma interface mais dinâmica e interessante.

Por muito tempo, a única maneira de trazer essas formas para a web era através do uso de arquivos de imagem (`.png`, `.gif`, `.svg`). Embora eficaz, essa abordagem adiciona requisições HTTP, pode ser difícil de escalar e torna a simples alteração de uma cor um processo que envolve um editor de imagens. Mas e se pudéssemos criar essas formas diretamente com CSS?

Graças à engenhosidade da comunidade de desenvolvedores e ao poder das propriedades do CSS, isso é inteiramente possível. Usando técnicas criativas que manipulam as bordas, os cantos arredondados e as transformações, podemos construir uma vasta gama de formas com nada mais do que uma única `div`.

Neste capítulo, vamos explorar o lado artístico do CSS. Aprenderemos as técnicas fundamentais para criar formas não-retangulares. Começaremos com as mais simples, como círculos e elipses, e depois mergulharemos no engenhoso "hack da borda" para construir triângulos e trapézios de todas as orientações. Por fim, veremos como os pseudo-elementos nos permitem construir formas ainda mais complexas, como estrelas, cubos e pirâmides, tudo isso mantendo nosso HTML limpo e semântico.

## Formas a partir do Box Model: Quadrados, Círculos e Elipses

As formas mais básicas derivam diretamente do Box Model e da propriedade `border-radius`.

### Quadrados

Esta é a forma mais simples. Um quadrado é apenas um elemento de bloco com `width` e `height` iguais.

```html
<div class="square"></div>
```

```css
.square {
  width: 100px;
  height: 100px;
  background: rgb(246, 156, 85);
}
```

### Círculos e Elipses

Estas formas são criadas aplicando-se `border-radius` a um elemento com `width` e `height` definidos.

- **Círculo:** Para criar um círculo perfeito, comece com um quadrado (`width` e `height` iguais) e aplique `border-radius: 50%;`.
- **Elipse/Oval:** Para criar uma elipse, aplique `border-radius: 50%;` a um retângulo (`width` e `height` diferentes).

```html
<div class="circle"></div>
<div class="oval"></div>
```

```css
.circle {
  width: 100px;
  height: 100px;
  background: rgb(246, 156, 85);
  border-radius: 50%;
}

.oval {
  width: 150px;
  height: 80px;
  background: rgb(246, 156, 85);
  border-radius: 50%;
}
```

## O "Hack da Borda": Criando Triângulos e Trapézios

Esta é uma das técnicas mais inteligentes e fundamentais do CSS. Ela se baseia em como o navegador renderiza as bordas de um elemento quando sua largura e altura são zero.

**O Conceito:** Quando um elemento tem `width: 0` e `height: 0`, suas bordas se encontram no centro, e o navegador as renderiza de forma diagonal para uni-las. Se definirmos três das quatro bordas como `transparent` e dermos uma cor a apenas uma delas, a forma diagonal da borda colorida se torna visível como um triângulo.

### Trapézio

Um trapézio é uma variação do hack do triângulo. A única diferença é que damos uma `width` ao elemento. As bordas laterais transparentes ainda criam os lados angulares, mas agora eles se conectam a uma base sólida (a `width`), formando um trapézio.

```html
<div class="trapezoid"></div>
```

```css
.trapezoid {
  width: 150px; /* A largura da base superior do trapézio */
  height: 0;
  /* Bordas laterais transparentes criam os lados inclinados */
  border-left: 50px solid transparent;
  border-right: 50px solid transparent;
  /* A borda inferior colorida forma o corpo e a base inferior do trapézio */
  border-bottom: 100px solid black;
}
```

### Triângulo Apontando para Cima

A "base" do triângulo é a `border-bottom`, e as laterais transparentes formam os lados que se encontram em um ponto no topo.

```html
<div class="triangle-up"></div>
```

```css
.triangle-up {
  width: 0;
  height: 0;
  border-left: 50px solid transparent;
  border-right: 50px solid transparent;
  border-bottom: 100px solid rgb(246, 156, 85);
}
```

### Triângulo Apontando para Baixo

Invertemos a lógica: a "base" é a `border-top`.

```html
<div class="triangle-down"></div>
```

```css
.triangle-down {
  width: 0;
  height: 0;
  border-left: 50px solid transparent;
  border-right: 50px solid transparent;
  border-top: 100px solid rgb(246, 156, 85);
}
```

### Triângulos Apontando para a Direita e para a Esquerda

O mesmo princípio se aplica, mas agora usamos `border-left` ou `border-right` como a base colorida e as bordas `top` e `bottom` como as transparentes.

```html
<div class="triangle-right"></div>
<div class="triangle-left"></div>
```

```css
.triangle-right {
  width: 0;
  height: 0;
  border-top: 50px solid transparent;
  border-bottom: 50px solid transparent;
  border-left: 100px solid rgb(246, 156, 85);
}
.triangle-left {
  width: 0;
  height: 0;
  border-top: 50px solid transparent;
  border-bottom: 50px solid transparent;
  border-right: 100px solid rgb(246, 156, 85);
}
```

### Triângulos Diagonais

Para criar triângulos retângulos (diagonais), basta definir apenas uma das bordas adjacentes como transparente.

```html
<div class="triangle-up-right"></div>
```

```css
.triangle-up-right {
  width: 0;
  height: 0;
  border-top: 100px solid rgb(246, 156, 85);
  border-left: 100px solid transparent;
}
```

As outras três variações (`up-left`, `down-right`, `down-left`) seguem a mesma lógica, apenas mudando quais bordas são transparentes e quais são coloridas.

## Formas com Pseudo-elementos

Para formas mais complexas, usamos pseudo-elementos (`::before`, `::after`) para adicionar mais "partes" à nossa forma sem poluir o HTML.

### Bursts (Estrelas / Explosões)

A técnica consiste em sobrepor vários retângulos (o elemento principal e seus pseudo-elementos) e rotacioná-los.

#### Estrela de 8 Pontas

Criamos um quadrado com o elemento principal e outro com o `::before`. Giramos o elemento principal em 20 graus (um erro comum é `20eg`, o correto é `deg`) e o pseudo-elemento em `135` graus (`90 + 45`) para criar 8 pontas.

```html
<div class="burst-8"></div>
```

```css
.burst-8 {
  background: rgb(246, 156, 85);
  width: 80px;
  height: 80px;
  position: relative;
  text-align: center;
  transform: rotate(20deg);
}
.burst-8::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  height: 80px;
  width: 80px;
  background: rgb(246, 156, 85);
  transform: rotate(135deg);
}
```

#### Estrela de 12 Pontas

Aqui, usamos o elemento principal, o `::before` e o `::after`. Cada um é um quadrado rotacionado em um incremento de 30 graus.

```html
<div class="burst-12"></div>
```

```css
.burst-12 {
  width: 80px;
  height: 80px;
  position: relative;
  background: rgb(246, 156, 85);
}
.burst-12::before, .burst-12::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  height: 80px;
  width: 80px;
  background: rgb(246, 156, 85);
}
.burst-12::before {
  transform: rotate(30deg);
}
.burst-12::after {
  transform: rotate(60deg);
}
```

### Formas com Perspectiva Falsa (Pseudo-3D)

#### Cubo

Criamos a ilusão de um cubo 3D usando um quadrado para a face frontal e dois pseudo-elementos inclinados (`skew`) para as faces superior e lateral.

```html
<div class="cube"></div>
```

```css
.cube {
  background: #dc2e2e;
  width: 100px;
  height: 100px;
  position: relative;
  margin: 50px 0 0 50px;
}
.cube::before {
  content: '';
  display: inline-block;
  background: #f15757;
  width: 100px;
  height: 20px;
  transform: skewX(-40deg);
  position: absolute;
  top: -20px;
  left: 10px;
}
.cube::after {
  content: '';
  display: inline-block;
  background: #9e1515;
  width: 20px;
  height: 100px;
  transform: skewY(-50deg);
  position: absolute;
  top: -10px;
  left: 100%;
}
```

#### Pirâmide

Esta é uma técnica ainda mais avançada, que combina o hack do triângulo com transformações 3D. Os dois pseudo-elementos são criados como triângulos de borda e depois distorcidos com `scale`, `skew` e `rotate` para criar a perspectiva das duas faces visíveis de uma pirâmide.

```html
<div class="pyramid"></div>
```

```css
.pyramid {
  width: 200px;
  height: 200px;
  position: relative;
  margin: 50px;
}
.pyramid::before, .pyramid::after {
  content: '';
  display: inline-block;
  width: 0;
  height: 0;
  border: 100px solid; /* A base do triângulo */
  position: absolute;
}
/* Face da esquerda */
.pyramid::before {
  border-color: transparent transparent rgba(255, 86, 86, 0.7) transparent;
  transform: scaleY(0.5) rotate(45deg);
}
/* Face da direita */
.pyramid::after {
  border-color: transparent transparent rgba(214, 68, 68, 0.7) transparent;
  transform: scaleY(0.5) rotate(-45deg);
}
```

## Boas Práticas com Formas em CSS

A criação de formas com CSS é uma demonstração de criatividade e um profundo entendimento das ferramentas do navegador. Para usá-las de forma eficaz e responsável, é importante ter algumas diretrizes em mente.

- **Semântica Primeiro:** Lembre-se que a maioria dessas técnicas usa elementos `div` vazios que não têm significado semântico. Elas são excelentes para elementos puramente decorativos. Se a forma precisa transmitir informação (como um ícone de "play" em um vídeo), certifique-se de que haja um texto alternativo acessível.
- **SVG como Alternativa Superior:** Para formas complexas, responsivas ou que precisam ser interativas, **SVG (Scalable Vector Graphics)** é quase sempre a melhor ferramenta. SVG é um formato de imagem baseado em XML, o que o torna escalável sem perda de qualidade, acessível e manipulável com CSS e JavaScript. Use as formas CSS para decorações, ícones simples e efeitos de UI.
- **Use Pseudo-elementos:** A chave para manter o HTML limpo é usar `::before` e `::after` sempre que possível para construir as partes de uma forma complexa. Evite adicionar `<div>`s extras no seu HTML apenas para fins de estilização.
- **Acessibilidade é Crucial:** O conteúdo gerado por pseudo-elementos (`content`) geralmente não é lido por leitores de tela. Nunca coloque informações importantes dentro da propriedade `content`. Se a forma _é_ a informação (como uma seta de aviso), use técnicas de acessibilidade (como a classe `.sr-only` para texto de leitores de tela) para fornecer uma alternativa textual.
- **Comente seu Código:** As técnicas de formas, especialmente as que envolvem transformações e o hack da borda, não são imediatamente óbvias. Deixe comentários no seu CSS explicando como a forma está sendo construída. Isso ajudará você (e outros desenvolvedores) no futuro.

## Considerações Finais

Neste capítulo, fomos além dos limites do Box Model retangular, aprendendo a desenhar e construir uma variedade de formas geométricas usando apenas CSS. Vimos como o `border-radius` nos dá círculos e elipses, e como o engenhoso "hack da borda" nos permite criar triângulos e trapézios com precisão.

Exploramos também o poder dos pseudo-elementos e das transformações para construir formas ainda mais complexas, como estrelas e ilusões 3D, tudo isso sem a necessidade de imagens externas. Essas técnicas não são apenas demonstrações de habilidade; são ferramentas práticas para criar ícones, decorações e elementos de UI únicos que podem tornar um design verdadeiramente memorável.
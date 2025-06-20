# Capítulo 27 – Controlando o Layout com a Propriedade `display`

Nos capítulos anteriores, nos tornamos mestres em dar estilo aos nossos elementos. Controlamos suas cores, fontes, sombras e posicionamento. No entanto, o verdadeiro desafio do design web começa quando precisamos fazer com que todos esses elementos estilizados convivam harmoniosamente na página, formando um layout coeso e funcional. Como fazemos para que um menu de navegação e um logotipo fiquem lado a lado? Por que alguns elementos ocupam toda a largura da página enquanto outros se contentam com o espaço de seu conteúdo?

A resposta para todas essas questões reside na propriedade mais fundamental para o controle de layout em CSS: a propriedade **`display`**. Esta propriedade define o tipo de caixa de renderização de um elemento, ditando como ele se comporta no fluxo do documento e como interage com seus vizinhos. O valor de `display` de um elemento determina se ele começa em uma nova linha, se pode ter largura e altura definidas, e se seus filhos serão organizados em colunas, linhas ou em um grid.

Neste capítulo, vamos explorar os valores mais importantes da propriedade `display`. Começaremos com a distinção essencial entre os contextos de formatação **`block`** e **`inline`**. Veremos como o valor **`inline-block`** nos oferece um híbrido útil. Em seguida, introduziremos os valores que revolucionaram o layout na web moderna: **`flex`** e **`grid`**, que ativam os poderosos modelos de Flexbox e CSS Grid. Compreender `display` não é apenas aprender mais uma propriedade; é aprender a linguagem fundamental do layout em CSS.

## `display: none`

Este é o valor mais drástico. Quando um elemento tem `display: none;`, ele é **completamente removido do documento**. Ele não é renderizado, não ocupa espaço e se torna inacessível para leitores de tela. É como se a tag nunca tivesse existido no HTML.

**Diferença crucial de `visibility: hidden;`:** Um elemento com `visibility: hidden;` fica invisível, mas o espaço que ele ocupa no layout é preservado. Com `display: none;`, o espaço também é removido.

**Exemplo:**

```html
<div>Divisão 1</div>
<div class="escondido">Esta divisão não aparecerá</div>
<div>Divisão 3</div>
```

```css
.escondido {
  display: none;
}
````

**Resultado:** A "Divisão 3" aparecerá imediatamente abaixo da "Divisão 1", como se a segunda `div` não existisse.

## `display: block`

Elementos de bloco são os principais blocos de construção de um layout. Suas características são:

- **Sempre começam em uma nova linha.**
- **Ocupam toda a largura disponível** de seu contêiner pai, por padrão.
- As propriedades `width`, `height`, `margin` (incluindo topo e baixo) e `padding` são totalmente respeitadas.

Exemplos de elementos que são `block` por padrão: `<div>`, `<p>`, `<h1>` a `<h6>`, `<form>`, `<ul>`, `<ol>`, `<li>`, `<section>`, `<article>`.

**Exemplo:**

```css
<div style="background: lightblue; display: block;">Eu sou um bloco.</div>
<div style="background: lightcoral; display: block;">Eu também sou, por isso estou em uma nova linha.</div>
```

## `display: inline`

Elementos em linha são projetados para fluir dentro de blocos de texto. Suas características são:

- **Não começam em uma nova linha.** Eles se alinham na mesma linha, um após o outro.
- **Ocupam apenas a largura de seu conteúdo.**
- As propriedades `width` e `height` **não têm efeito**.
- `margin-top` e `margin-bottom` **não têm efeito**. `margin-left` e `margin-right` funcionam.
- `padding` vertical (topo e baixo) pode sobrepor visualmente outros elementos, mas não empurra as linhas de texto acima ou abaixo.

Exemplos de elementos que são `inline` por padrão: `<a>`, `<span>`, `<strong>`, `<em>`, `<img>`.

**Exemplo:**

```css
<p>
  Este é um texto com um <a href="#" style="background: gold; display: inline;">link em linha</a> e um <strong style="background: lightgreen; display: inline;">texto forte</strong>. Ambos permanecem na mesma linha.
</p>
```

## `display: inline-block`

Este valor é um híbrido útil que combina características de `block` e `inline`.

- **Externamente,** comporta-se como um elemento `inline`: não começa em uma nova linha e se alinha horizontalmente com outros elementos.
- **Internamente,** comporta-se como um elemento `block`: as propriedades `width`, `height`, `margin` vertical e `padding` vertical são totalmente respeitadas.

### Layout com `inline-block`

`inline-block` foi uma das primeiras técnicas para criar grades de itens lado a lado, como uma galeria de produtos.

**Exemplo:**

```css
<div class="galeria-inline-block">
  <div class="item">Item 1</div>
  <div class="item">Item 2</div>
  <div class="item">Item 3</div>
  <div class="item">Item 4</div>
</div>
```

```css
.item {
  display: inline-block;
  width: 200px;
  height: 150px;
  background: skyblue;
  margin: 10px;
  /* Alinhamento vertical dos itens */
  vertical-align: top;
}
```

**O Problema do Espaço em Branco:** Uma peculiaridade do `inline-block` é que ele respeita o espaço em branco no HTML (como a quebra de linha entre as `divs`). Isso cria um pequeno espaço indesejado entre os elementos. A solução clássica era definir `font-size: 0;` no contêiner pai e resetar o `font-size` nos filhos. Hoje, para este tipo de layout, **Flexbox é a solução superior**.

## Os Modelos de Layout Modernos

Os valores a seguir não apenas definem o comportamento externo do elemento, mas também ativam um novo e poderoso contexto de formatação para seus filhos diretos.

### `display: flex` e `display: inline-flex`

- **`display: flex;`**: Transforma um elemento em um **contêiner flexível de nível de bloco**. Seus filhos diretos se tornam itens flexíveis, que podem ser facilmente alinhados e distribuídos ao longo de um único eixo (horizontal ou vertical).
- **`display: inline-flex;`**: O elemento em si se comporta como um elemento em linha, mas seus filhos são formatados com o modelo Flexbox.

Flexbox é a ferramenta ideal para alinhar itens em uma dimensão, como menus de navegação, componentes de cartão e centralização de conteúdo. **Será abordado em detalhes em seu próprio capítulo.**

### `display: grid` e `display: inline-grid`

- **`display: grid;`**: Transforma um elemento em um **contêiner de grade de nível de bloco**. Seus filhos diretos se tornam itens da grade.
- **`display: inline-grid;`**: O elemento em si se comporta como um elemento em linha, mas seus filhos são formatados com o modelo Grid.

Grid é o sistema de layout mais poderoso do CSS, projetado para criar layouts bidimensionais complexos (linhas e colunas) com facilidade e precisão. **Será abordado em detalhes em seu próprio capítulo.**

## Outros Valores de `display`

### `display: list-item`

Faz com que um elemento se comporte como um item de lista (`<li>`), gerando um marcador.

```css
.meu-item-de-lista {
  display: list-item;
  list-style-type: square;
  margin-left: 20px; /* Adiciona recuo para o marcador */
}
```

### `display: table`, `table-row`, `table-cell`, etc.

Este conjunto de valores permite que elementos não-tabela imitem o comportamento de layout de uma tabela HTML.

- `display: table;` (equivale a `<table>`)
- `display: table-row;` (equivale a `<tr>`)
- `display: table-cell;` (equivale a `<td>`)

Antes do Flexbox, o `display: table-cell` era um truque comum para resolver problemas de alinhamento vertical. Hoje, seu uso para layout geral é considerado legado.

**Exemplo (Alinhamento Vertical Legado):**

```html
<div class="tabela-pai">
  <div class="celula-filha">Estou verticalmente centrado.</div>
</div>
```

```css
.tabela-pai {
  display: table;
  width: 300px;
  height: 200px;
}
.celula-filha {
  display: table-cell;
  vertical-align: middle; /* Propriedade que funciona no contexto de tabela */
  text-align: center;
}
```

## Boas Práticas de Layout

A propriedade `display` é o ponto de partida para qualquer layout. Escolher o valor correto é o primeiro passo para um design bem-sucedido, e seguir as convenções modernas garante que seu código seja robusto, flexível e fácil de manter.

- **Use `display` para o Layout, `position` para Exceções:** A regra de ouro do layout moderno é: use o fluxo normal do documento (controlado por `display`) para a estrutura principal. Use `position` (especialmente `absolute`) apenas para elementos que precisam se sobrepor ou "escapar" desse fluxo, como modais e tooltips.
- **Flexbox para Uma Dimensão, Grid para Duas:** Esta é a diretriz mais importante para layouts modernos. Use **Flexbox** quando precisar alinhar uma série de itens em uma única linha ou coluna. Use **CSS Grid** quando precisar criar um layout bidimensional complexo, com controle tanto sobre as linhas quanto sobre as colunas.
- **Abandone os Layouts Legados:** Evite usar `float` ou `display: inline-block` para a estrutura principal da sua página. Embora seja crucial entendê-los para dar manutenção em código antigo, Flexbox e Grid são infinitamente superiores para novos projetos.
- **Respeite a Semântica:** Não mude o `display` de um elemento de uma forma que contradiga seu significado. Por exemplo, transformar um parágrafo (`<p>`) em `display: inline` pode quebrar a expectativa de como o conteúdo textual deve ser apresentado. Se você precisa de um elemento em linha, use um `<span>`.
- **Entenda o Contexto de Formatação:** Lembre-se que `block`, `inline`, `flex` e `grid` não apenas mudam o elemento em si, mas também criam um "contexto de formatação" que dita como seus filhos se comportam. Este é o conceito-chave por trás do poder desses valores.

## Considerações Finais

Neste capítulo, exploramos a propriedade `display`, a pedra angular do layout em CSS. Desvendamos a dicotomia fundamental entre os contextos de formatação `block` e `inline`, que governa o fluxo padrão de qualquer documento web. Vimos como `inline-block` oferece um meio-termo útil, embora hoje seja amplamente superado.

Mais importante, abrimos as portas para o CSS moderno ao introduzir os valores `flex` e `grid`. Entender que `display` é a propriedade que "ativa" esses poderosos sistemas de layout é o primeiro passo para a construção de interfaces verdadeiramente complexas, responsivas e profissionais. Com esta base sólida, estamos prontos para mergulhar de cabeça nos detalhes de cada um desses modelos.
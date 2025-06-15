# Capítulo 8 – Elementos de Bloco e de Linha

Ao longo dos capítulos anteriores, construímos páginas utilizando diversos elementos HTML. Você pode ter notado que alguns elementos, como `<p>` e `<h1>`, naturalmente se posicionam um abaixo do outro, ocupando seu próprio espaço, enquanto outros, como `<a>` e `<strong>`, se encaixam fluidamente no meio do texto. Esse comportamento não é acidental; ele é governado por uma característica fundamental de cada elemento: seu **modelo de exibição** (display model).

Neste capítulo, vamos explorar os dois modelos de exibição mais importantes do HTML: **bloco (block)** e **linha (inline)**. Compreender a diferença entre eles é absolutamente crucial para controlar o layout e o fluxo de uma página web. Definiremos as características de cada modelo e apresentaremos os dois elementos de contêiner mais genéricos e versáteis do HTML: `<div>` e `<span>`. Eles são ferramentas essenciais no arsenal de qualquer desenvolvedor, usadas para agrupar e selecionar elementos para estilização com CSS e manipulação com JavaScript, especialmente quando nenhum outro elemento semântico se aplica.

Dominar esses conceitos é o primeiro passo para deixar de apenas inserir conteúdo e passar a orquestrar o layout de suas páginas de forma consciente e profissional.

## Elementos de Bloco (Block-level Elements)

Elementos de bloco são os principais blocos de construção estrutural de uma página. Pense neles como caixas retangulares que são empilhadas umas sobre as outras.

Suas características principais são:

- **Sempre começam em uma nova linha**: Um elemento de bloco força uma quebra de linha antes e depois de si mesmo.
- **Ocupam toda a largura disponível**: Por padrão, um elemento de bloco se estende para preencher toda a largura de seu contêiner pai, da borda esquerda à direita.
- **Podem ter dimensões e espaçamento definidos**: Você pode aplicar explicitamente propriedades de CSS como `width`, `height`, `margin` (espaçamento externo) e `padding` (espaçamento interno) a eles.

A maioria dos elementos que usamos para estruturar o layout de uma página são, por padrão, elementos de bloco.

**Exemplos Comuns:** `<h1>`-`<h6>`, `<p>`, `<ul>`, `<ol>`, `<li>`, `<table>`, `<blockquote>`, e os elementos semânticos do HTML5 como `<header>`, `<main>`, `<section>`, `<article>` e `<footer>`.

### O Contêiner de Bloco Genérico: `<div>`

O elemento `<div>` (divisão) é o contêiner de bloco mais genérico que existe. Ele não possui **nenhum valor semântico**; seu único propósito é agrupar outros elementos para fins de estilização ou layout, especialmente quando nenhum dos elementos semânticos mais específicos (como `<article>` ou `<section>`) se encaixa adequadamente.

Use o `<div>` para criar "caixas" ou seções em sua página que precisam de um estilo ou posicionamento em comum.

**Exemplo de uso:**

```html
<div class="card-produto">
  <h2>Produto Fantástico</h2>
  <p>Descrição breve do produto, com suas qualidades e preço.</p>
  <a href="#">Comprar agora</a>
</div>
```

## Elementos de Linha (Inline-level Elements)

Elementos de linha, como o nome sugere, existem "em linha" com o conteúdo ao seu redor. Eles não quebram o fluxo do texto.

Suas características principais são:

- **Não começam em uma nova linha**: Eles aparecem na mesma linha do conteúdo e dos elementos adjacentes.
- **Ocupam apenas a largura necessária**: A largura de um elemento de linha é determinada pelo tamanho do seu conteúdo.
- **Dimensões e espaçamento vertical são limitados**: Tentar definir `width` ou `height` em um elemento de linha não terá efeito. `margin` e `padding` verticais também não se comportam como esperado.

**Exemplos Comuns:** `<a>`, `<strong>`, `<em>`, `<b>`, `<i>`, `<img>`, `<sub>`, `<sup>` e `<br>`.

### O Contêiner de Linha Genérico: `<span>`

O elemento `<span>` é o equivalente em linha do `<div>`. Ele também é **semanticamente neutro** e é usado para agrupar um pequeno trecho de texto ou outros elementos de linha, geralmente para aplicar um estilo específico ou para ser alvo de JavaScript, sem quebrar o fluxo do parágrafo.

Use o `<span>` quando precisar estilizar uma única palavra ou frase dentro de um bloco de texto maior.

**Exemplo de uso:**

```html
<p>
  A promoção de <span class="destaque-preco">20% de desconto</span> é válida
  apenas para pagamentos à vista.
</p>
```

Neste caso, podemos usar a classe `destaque-preco` para, por exemplo, deixar apenas aquele trecho de texto em vermelho, sem afetar o resto do parágrafo.

## Resumo Visual: `<div>` vs. `<span>`

A melhor forma de entender a diferença é visualizando o comportamento de cada um.

### Exemplo com `<div>`

```html
<div style="background-color: lightblue;">Este é o primeiro div.</div>
<div style="background-color: lightcoral;">Este é o segundo div.</div>
```

**Resultado Visual:** Cada `<div>` começará em uma nova linha e sua cor de fundo se estenderá por toda a largura disponível, como duas caixas empilhadas.

#### Exemplo com `<span>`

```html
<p style="border: 1px solid black;">
  Este é o início do parágrafo.
  <span style="background-color: lightblue;">Este é o primeiro span.</span>
  Este texto continua na mesma linha.
  <span style="background-color: lightcoral;">Este é o segundo span.</span>
</p>
```

**Resultado Visual:** Os `<span>`s não quebrarão a linha. Eles fluirão junto com o texto e sua cor de fundo cobrirá apenas o espaço de seu próprio conteúdo.

## A Importância da Distinção para o Layout

Compreender a diferença entre elementos de bloco e de linha é o primeiro passo para dominar o layout com CSS. O CSS nos dá o poder de alterar esse comportamento padrão através da propriedade `display`. Por exemplo, podemos fazer com que itens de uma lista (`<li>`, que são de bloco) se alinhem horizontalmente (`display: inline-block;`), ou que um link (`<a>`, que é de linha) ocupe toda a largura de seu contêiner (`display: block;`).

Essa flexibilidade é a base para a criação de praticamente todos os layouts modernos na web.

## Considerações Finais

Neste capítulo, desvendamos um dos conceitos mais fundamentais do HTML e CSS: a diferença entre **elementos de bloco** e **elementos de linha**. Vimos que os elementos de bloco são os pilares estruturais que se empilham verticalmente, enquanto os elementos de linha fluem horizontalmente junto com o texto.

Introduzimos também os versáteis e semanticamente neutros `<div>` e `<span>`. O `<div>` serve como um contêiner de bloco genérico para agrupar grandes seções de conteúdo, e o `<span>` como um contêiner de linha para marcar pequenos trechos de texto. O uso criterioso desses elementos, combinado com um bom entendimento do fluxo do documento, é essencial para criar páginas bem estruturadas e prontas para serem estilizadas com precisão pelo CSS.

Com esse conhecimento sobre o comportamento espacial dos elementos, estamos ainda mais preparados para construir componentes interativos complexos. No próximo capítulo, mergulharemos no mundo dos **formulários**, que são construídos a partir de uma combinação inteligente de elementos de bloco e de linha para coletar informações do usuário.
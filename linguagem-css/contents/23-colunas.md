# Capítulo 23 – Layout de Múltiplas Colunas

Ao longo de nossa jornada pelo CSS, exploramos diversas técnicas de layout. Vimos como o `float`, apesar de legado, permitia que o texto fluísse ao redor de imagens. No futuro, mergulharemos nos poderosos sistemas de Flexbox e Grid, que são ideais para posicionar os principais componentes de uma interface. No entanto, há um tipo específico de layout, clássico do mundo impresso, que exige uma abordagem diferente: o layout de múltiplas colunas, como o que vemos em jornais e revistas.

Quando confrontados com um bloco de texto muito longo e largo, a leitura pode se tornar cansativa. Nossos olhos precisam percorrer uma grande distância horizontal para ir do final de uma linha ao início da próxima. O layout em múltiplas colunas resolve isso dividindo o conteúdo em colunas mais estreitas, tornando o texto mais fácil de "escanear" e consumir.

Para atender a essa necessidade, o CSS nos oferece um módulo específico: o **CSS Multi-column Layout**. Este sistema foi projetado com um propósito claro: permitir que o conteúdo flua de forma contínua de uma coluna para a seguinte. Ele nos dá um controle preciso sobre o número de colunas, sua largura, o espaço entre elas e as linhas que as separam.

Neste capítulo, vamos explorar em detalhe este módulo de layout. Aprenderemos a criar colunas de forma simples com `column-count`, a sugerir uma largura ideal com `column-width` e a usar a propriedade de atalho `columns`. Vamos refinar a aparência com `column-gap` e `column-rule`, e veremos como controlar o fluxo com propriedades avançadas como `column-span`.

## Criando Colunas: `column-count` e `column-width`

Existem duas maneiras principais de se definir um layout de múltiplas colunas. Você pode definir quantas colunas você quer, ou pode definir qual a largura que você gostaria que cada coluna tivesse.

### `column-count`: Definindo o Número de Colunas

Esta é a maneira mais direta de criar um layout de múltiplas colunas. Você simplesmente especifica o número de colunas que deseja, e o navegador divide o espaço disponível igualmente entre elas.

**Exemplo:**

```html
<article class="noticia">
  <h2>A História do CSS</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed non risus. Suspendisse lectus tortor, dignissim sit amet, adipiscing nec, ultricies sed, dolor. Cras elementum ultrices diam. Maecenas ligula massa, varius a, semper congue, euismod non, mi. Proin porttitor, orci nec nonummy molestie, enim est eleifend mi, non fermentum diam nisl sit amet erat...</p>
  <p>Duis semper. Duis arcu massa, scelerisque vitae, consequat in, pretium a, enim. Pellentesque congue. Ut in risus volutpat libero pharetra tempor. Cras vestibulum bibendum augue. Praesent egestas leo in pede. Praesent blandit odio eu enim. Pellentesque sed dui ut augue blandit sodales. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Aliquam nibh.</p>
</article>
```

```css
.noticia {
  column-count: 3;
}
```

**Resultado:** O conteúdo dentro do `<article>` será automaticamente dividido em três colunas de larguras iguais.

### `column-width`: Definindo a Largura Ideal da Coluna

Em vez de fixar o número de colunas, você pode sugerir uma largura ideal para elas. O navegador então criará quantas colunas couberem no espaço do contêiner, garantindo que nenhuma coluna seja mais estreita que o valor que você definiu.

**Exemplo:**

```css
.noticia {
  /* O navegador criará colunas com cerca de 200px de largura.
     Se o contêiner tiver 700px, ele criará 3 colunas.
     Se tiver 500px, criará 2 colunas. */
  column-width: 200px;
}
```

Este método é inerentemente mais responsivo, pois o número de colunas se adapta à largura do contêiner.

### `columns`: O Atalho Inteligente

A propriedade de atalho `columns` permite que você defina `column-width` e `column-count` ao mesmo tempo. O navegador tentará respeitar ambas as regras.

**Exemplo:**

```css
.noticia {
  /* Diz ao navegador: "Eu quero 3 colunas, e eu gostaria que elas
     tivessem pelo menos 15em de largura". */
  columns: 3 15em;
}
```

Neste caso, o navegador só criará 3 colunas se o contêiner tiver largura suficiente para que cada uma delas tenha pelo menos `15em`. Se o contêiner for muito estreito, ele poderá criar apenas 2 ou 1 coluna para respeitar a largura mínima.

## Ajustando o Espaçamento e a Aparência

### `column-gap`

Esta propriedade define o tamanho do espaço (a "calha" ou "gutter") entre as colunas.

**Exemplo:**

```css
.noticia {
  column-count: 3;
  /* Cria um espaçamento de 40px entre as colunas */
  column-gap: 40px;
}
```

### `column-rule`

`column-rule` é uma propriedade de atalho que desenha uma linha decorativa no meio do `column-gap`. Sua sintaxe é idêntica à da propriedade `border`: `column-rule: <width> <style> <color>;`.

**Exemplo:**

```css
.noticia {
  column-count: 3;
  column-gap: 40px;
  /* Desenha uma linha pontilhada cinza de 1px entre as colunas */
  column-rule: 1px dotted #ccc;
}
```

É importante notar que a `column-rule` não ocupa espaço. Ela é desenhada no centro do `column-gap`, então o espaçamento deve ser largo o suficiente para acomodá-la visualmente.

## Controlando o Fluxo do Conteúdo

O módulo de múltiplas colunas nos oferece propriedades para controlar como elementos específicos se comportam dentro do layout.

### `column-span: all`

Às vezes, você quer que um elemento, como um título principal ou uma imagem de destaque, interrompa o fluxo de colunas e se estenda por toda a largura do contêiner. A propriedade `column-span` faz exatamente isso.

- `span: none;` (padrão): O elemento permanece em sua coluna.
- `span: all;`: O elemento "quebra" o layout de colunas e se estende por todas elas.

**Exemplo:**

```html
<article class="noticia-com-span">
  <h2>Um Título que se Estende por Todas as Colunas</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed non risus. Suspendisse lectus tortor, dignissim sit amet, adipiscing nec, ultricies sed, dolor. Cras elementum ultrices diam...</p>
  <p>Pellentesque congue. Ut in risus volutpat libero pharetra tempor. Cras vestibulum bibendum augue. Praesent egestas leo in pede. Praesent blandit odio eu enim...</p>
</article>
```

```css
.noticia-com-span {
  column-count: 3;
  column-gap: 30px;
}

.noticia-com-span h2 {
  /* O h2 vai ocupar a largura total do container */
  column-span: all;
  text-align: center;
  margin-bottom: 20px;
}
```

**Resultado:** O título aparecerá centralizado no topo do contêiner, e o texto dos parágrafos começará a fluir nas 3 colunas somente abaixo dele.

### `break-inside: avoid`

Um problema comum em layouts de múltiplas colunas é que um elemento, como uma imagem com legenda (`<figure>`) ou uma citação (`<blockquote>`), pode ser quebrado de forma desajeitada, com uma parte em uma coluna e o resto na coluna seguinte. A propriedade `break-inside: avoid;` instrui o navegador a tentar manter o elemento inteiro, movendo-o para o topo da próxima coluna se ele não couber no final da atual.

**Exemplo:**

```html
<article class="noticia-com-figura">
  <p>Texto anterior...</p>
  <figure class="nao-quebre">
    <img src="https://placehold.co/200x150" alt="Imagem">
    <figcaption>Esta legenda não deve ser separada da sua imagem.</figcaption>
  </figure>
  <p>Texto posterior...</p>
</article>
```

```css
.nao-quebre {
  break-inside: avoid;
  /* Outros estilos para a figura */
  margin: 1rem 0;
  padding: 10px;
  background: #f9f9f9;
}
```

## Boas Práticas com Colunas

O layout de múltiplas colunas é uma ferramenta poderosa para a tipografia na web. Usá-lo corretamente pode melhorar drasticamente a experiência de leitura em seu site.

- **Ideal para Fluxo de Texto:** Lembre-se do propósito original. O módulo de colunas é otimizado para fazer com que **conteúdo contínuo**, como um longo artigo, flua de uma coluna para a outra. Não o utilize para posicionar os principais componentes de sua aplicação (como cabeçalho, barra lateral e conteúdo). Para isso, CSS Grid e Flexbox são as ferramentas corretas.
- **Cuidado com a Largura da Coluna:** Colunas muito estreitas dificultam a leitura, pois o olho precisa saltar de linha com muita frequência. Colunas muito largas recriam o problema que estávamos tentando resolver. Uma boa regra geral é manter a largura da coluna entre 45 e 75 caracteres. Use a unidade `ch` na `column-width` para ter um controle baseado em caracteres.
- **Use Espaçamento Generoso:** O `column-gap` é crucial para a legibilidade. Um espaçamento adequado ajuda o olho a distinguir claramente onde uma coluna termina e a próxima começa. Um valor de `1.5rem` a `3rem` é um bom ponto de partida.
- **A `column-rule` é Decorativa:** A linha entre as colunas deve ser sutil. Use cores de baixo contraste (como um cinza claro) e estilos de linha finos (`solid` ou `dotted`). Ela deve guiar, não distrair.
- **Controle as Quebras:** Para um acabamento profissional, use `column-span` em títulos para criar pontos de ancoragem visual e `break-inside: avoid;` em figuras e citações para evitar quebras de conteúdo indesejadas.

## Considerações Finais

Neste capítulo, exploramos o módulo de layout de múltiplas colunas do CSS, uma ferramenta especializada e elegante para melhorar a legibilidade de longos blocos de texto. Vimos como é simples dividir o conteúdo em um número fixo de colunas ou em colunas de largura flexível, e como refinar a apresentação com espaçamento e regras divisórias.

Aprendemos também a controlar o fluxo do conteúdo com propriedades como `column-span` e `break-inside`, o que nos permite criar layouts editoriais sofisticados e robustos, semelhantes aos que encontramos em publicações impressas. Embora não seja uma ferramenta para a arquitetura geral da página, o conhecimento do Multi-column Layout é uma adição valiosa ao repertório de qualquer desenvolvedor front-end que se preocupe com a qualidade da tipografia e da experiência de leitura.
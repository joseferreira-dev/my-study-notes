# Capítulo 22 – Layout com Floats

Antes da chegada dos poderosos sistemas de Flexbox e Grid, os desenvolvedores web tinham um conjunto de ferramentas mais limitado para criar layouts com múltiplas colunas. A principal e mais engenhosa dessas ferramentas era a propriedade **`float`**. Originalmente, o propósito do `float` era muito simples e elegante: permitir que o texto de um artigo fluísse naturalmente ao redor de uma imagem, imitando o layout de jornais e revistas.

No entanto, a necessidade de criar layouts mais complexos levou os desenvolvedores a "hackear" criativamente essa propriedade, usando-a para posicionar barras laterais, menus de navegação e colunas de conteúdo — essencialmente, para construir o esqueleto de sites inteiros. Por mais de uma década, `float` foi a base de quase todos os layouts da web.

Hoje, para a construção de layouts principais, Flexbox e Grid são as ferramentas superiores, mais poderosas e previsíveis. Então, por que dedicar um capítulo inteiro ao `float`? Por três razões cruciais: primeiro, para seu propósito original — fazer o texto fluir ao redor de elementos — ele ainda é a melhor e mais semântica ferramenta. Segundo, você inevitavelmente encontrará layouts baseados em `float` ao trabalhar em projetos legados, e entender como eles funcionam é essencial para a manutenção. Terceiro, compreender os desafios do `float` e as soluções criadas para eles (como o "clearfix") lhe dará um entendimento mais profundo sobre como o CSS funciona.

Neste capítulo, vamos mergulhar na propriedade `float`. Aprenderemos seu uso fundamental, investigaremos o principal problema que ela causa — o colapso de contêineres — e desvendaremos as técnicas para "limpar" os floats com a propriedade `clear`. Exploraremos como ela foi usada para criar layouts de colunas e como as formas modernas do CSS com `shape-outside` trouxeram o `float` de volta às suas raízes, mas com superpoderes.

## A Propriedade `float`

Quando você aplica `float` a um elemento, ele é retirado do fluxo normal do documento e deslocado para a esquerda ou para a direita de seu contêiner. O conteúdo adjacente (como texto) irá então "fluir" ao redor dele.

- `float: left;`: Desloca o elemento para a esquerda.
- `float: right;`: Desloca o elemento para a direita.
- `float: none;`: O elemento não flutua (comportamento padrão).

### O Uso Clássico: Flutuando uma Imagem no Texto

Este é o propósito original e mais semântico do `float`. O código a seguir demonstra como flutuar uma imagem para a esquerda de um bloco de texto, com margens para criar um espaçamento adequado.

```html
<article class="post">
  <img src="https://placehold.co/200x200/FFC107/333?text=Float" alt="Imagem flutuante" class="imagem-flutuante">
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam vitae odio vel libero finibus dictum. Praesent quis nisi vel ligula consectetur convallis. Fusce nec neque sit amet nisl tempus tincidunt. Integer in leo sit amet arcu tincidunt consectetur. Suspendisse potenti. Nam nec nunc ac libero auctor scelerisque. Vivamus nec magna vel est dapibus commodo.</p>
  <p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Curabitur vitae justo vel purus varius euismod. Ut nec velit nec libero tincidunt aliquam. In hac habitasse platea dictumst. Sed nec magna id libero tincidunt aliquam.</p>
</article>
```

```css
.imagem-flutuante {
  float: left;
  /* Adiciona um espaço para que o texto não fique colado na imagem */
  margin-right: 15px;
  margin-bottom: 5px;
}
```

**Resultado:** A imagem será deslocada para a esquerda, e os parágrafos de texto fluirão elegantemente ao redor dela, preenchendo o espaço à sua direita.

## O Problema dos Floats: Colapso do Contêiner

Quando um elemento contém apenas elementos flutuantes, sua altura "colapsa" para zero. Isso acontece porque os elementos flutuantes são retirados do fluxo normal, e o pai age como se estivesse vazio.

```html
<div class="container-colapsado">
  <div class="caixa-flutuante">Caixa 1</div>
  <div class="caixa-flutuante">Caixa 2</div>
</div>
<footer class="rodape">Rodapé da Página</footer>
```

```css
.container-colapsado {
  border: 3px solid red;
  width: 100%;
}
.caixa-flutuante {
  float: left;
  width: 100px;
  height: 100px;
  background: lightblue;
  margin: 10px;
}
.rodape {
  background: #333;
  color: white;
  padding: 10px;
}
```

**Resultado:** Você verá uma fina linha vermelha no topo, e as duas caixas azuis "vazando" para fora dela. Pior ainda, o rodapé preto subirá e ficará logo abaixo da linha vermelha, atrás das caixas flutuantes.

## A Solução: A Propriedade `clear`

A propriedade `clear` é usada para controlar o fluxo do conteúdo _ao redor_ de elementos flutuantes. Quando aplicada a um elemento, ela especifica em qual lado ele não permitirá elementos flutuantes. O elemento com `clear` será movido para baixo até que ele esteja "livre" dos floats.

- `clear: left;`: O elemento é movido para baixo até passar qualquer float à esquerda.
- `clear: right;`: O elemento é movido para baixo até passar qualquer float à direita.
- `clear: both;`: O mais comum. O elemento é movido para baixo até passar tanto os floats da esquerda quanto da direita.
- `none`, `inherit`, `initial`: Valores padrão e de herança.

**Exemplo:** Usando o mesmo HTML anterior, se aplicarmos `clear` ao rodapé, forçamos o rodapé a se mover para baixo, limpando o espaço ocupado pelas caixas flutuantes.

```css
.rodape {
  clear: both; /* Impede que o rodapé suba e se alinhe com os floats */
  background: #333;
  color: white;
  padding: 10px;
}
```

**Resultado:** O rodapé agora aparecerá corretamente _abaixo_ das duas caixas azuis. No entanto, o contêiner com a borda vermelha ainda estará colapsado. `clear` resolve o problema dos elementos seguintes, mas não o do contêiner pai.

## Resolvendo o Colapso: Técnicas de "Clearfix"

Para forçar um contêiner a se expandir e "conter" seus filhos flutuantes, precisamos usar uma técnica de "clearfix".

### A Solução Moderna: `overflow` (Criando um BFC)

Como vimos no Capítulo 21, definir `overflow: auto` ou `overflow: hidden` em um elemento cria um novo Block Formatting Context (BFC). Um BFC, por definição, sempre se expande para conter seus filhos flutuantes. Esta é a maneira mais simples e moderna de resolver o problema para a maioria dos casos.

```css
.container-colapsado {
  border: 3px solid red;
  width: 100%;
  overflow: auto; /* A MÁGICA! O contêiner agora se expande. */
}
```

### A Solução Clássica: O "Clearfix Hack"

Antes do `overflow` ser bem compreendido, a técnica padrão era o "clearfix hack". Ele usa um pseudo-elemento `::after` para inserir um elemento invisível _dentro_ do contêiner, no final, e aplicar `clear: both` a ele. Esta técnica é extremamente robusta e ainda é encontrada em muitos frameworks e sites legados.

```css
.container-colapsado::after {
  content: "";
  display: table; /* ou display: block */
  clear: both;
}
```

## Layouts com Floats (Técnicas Legadas)

Apesar de não serem mais a melhor prática para layouts de página inteira, é fundamental entender como os floats eram usados para isso.

### Divs Lado a Lado (In-line DIV)

Esta técnica era a base para a criação de sistemas de grade antes do CSS Grid. Ao aplicar `float: left` a vários elementos, eles deixam de se empilhar verticalmente e tentam se alinhar lado a lado. Ao definir suas larguras em porcentagens e adicionar margens, é possível criar uma grade de colunas fluida que se ajusta ao contêiner pai. O `clearfix` no elemento pai é essencial aqui para garantir que ele se expanda para conter a altura das colunas flutuantes.

```html
<div class="pai-com-clearfix">
  <div class="coluna-float">Coluna 1</div>
  <div class="coluna-float">Coluna 2</div>
  <div class="coluna-float">Coluna 3</div>
</div>
```

```css
.pai-com-clearfix::after { content: ""; display: table; clear: both; }

.coluna-float {
  float: left;
  width: 30%; /* 30% * 3 = 90%, deixa espaço para margens */
  margin: 1.66%;
  background: #f0f0f0;
  padding: 10px;
  box-sizing: border-box;
}
```

### Layout de Duas Colunas (Fixo + Fixo)

Um padrão de layout muito comum era ter uma área de conteúdo principal e uma barra lateral. Isso era frequentemente alcançado flutuando o conteúdo principal para a esquerda (`float: left`) e a barra lateral para a direita (`float: right`). Desde que a soma de suas larguras (mais margens, paddings e bordas) não excedesse a largura do contêiner pai, eles se encaixariam lado a lado. Um clearfix no contêiner pai também seria necessário aqui.

```css
.conteudo-principal { 
  float: left; 
  width: 600px; 
}

.barra-lateral { 
  float: right; 
  width: 300px; 
}
```

### Layout de Duas Colunas (Lazy/Greedy)

Este é um método mais flexível e engenhoso para um layout de duas colunas. Apenas a barra lateral, com uma largura fixa, é flutuada para a esquerda. O conteúdo principal não recebe a propriedade `float`. Em vez disso, ele permanece no fluxo normal do documento, mas recebe uma `margin-left` grande o suficiente para "abrir espaço" para a barra lateral flutuante. Dessa forma, o conteúdo principal ocupa todo o espaço restante disponível (ele é "ganancioso" ou "preguiçoso"), adaptando-se fluidamente se a janela do navegador for redimensionada.

```css
.barra-lateral {
  float: left;
  width: 200px;
}
.conteudo-principal {
  /* Não definimos float nem largura. Ele tentará ocupar todo o espaço
     disponível, mas o float da barra lateral o "empurrará".
     Adicionamos uma margem para que o conteúdo não fique colado. */
  margin-left: 220px; /* 200px da barra + 20px de espaço */
}
```

## O Futuro do Float: `shape-outside`

O CSS moderno trouxe o `float` de volta às suas origens de forma espetacular com a propriedade `shape-outside`. Ela permite que o texto flua ao redor de formas não retangulares, como círculos, elipses ou até mesmo polígonos complexos e imagens com transparência. A `shape-outside` é aplicada ao elemento flutuante.

### `shape-outside` com uma Forma Básica - `circle()`

Neste exemplo, criamos uma `div` flutuante e a estilizamos para parecer um círculo com `border-radius`. Em seguida, aplicamos `shape-outside: circle(50%)` para dizer ao navegador que o texto ao redor não deve seguir a caixa retangular da `div`, mas sim a forma circular que definimos.

```html
<div class="container-shape">
  <div class="circulo-flutuante"></div>
  <p>O texto agora flui elegantemente ao redor deste círculo flutuante, criando um efeito de layout muito mais orgânico e interessante do que seria possível com uma forma retangular. Esta propriedade abre novas portas para o design editorial na web, permitindo que o texto interaja com a forma de um elemento de maneira muito mais natural e visualmente atraente...</p>
</div>
```

```css
.circulo-flutuante {
  float: left;
  width: 150px;
  height: 150px;
  border-radius: 50%; /* Faz a div ser um círculo visualmente */
  background-color: steelblue;
  /* A mágica: diz ao texto para fluir ao redor de um círculo */
  shape-outside: circle(50%);
}
```

### `shape-margin`

Para adicionar um espaçamento entre a forma definida por `shape-outside` e o texto que a envolve, usamos a propriedade `shape-margin`.

```css
.circulo-flutuante {
  /* ...mesmos estilos de antes... */
  shape-outside: circle(50%);
  shape-margin: 1rem; /* Adiciona um respiro de 1rem ao redor da forma */
}
```

## Boas Práticas com Floats

Entender o `float` e seu contexto histórico é uma marca de um desenvolvedor experiente. Usá-lo de forma apropriada no cenário atual da web demonstra sabedoria e atenção aos detalhes.

- **Use Floats para o Propósito Certo:** O uso principal e recomendado para `float` é o seu original: fazer com que o texto flua ao redor de imagens ou outros elementos, especialmente quando combinado com `shape-outside` para layouts editoriais criativos.
- **Prefira Flexbox e Grid para Layout:** Para qualquer tipo de layout de página ou componente (barras laterais, galerias, alinhamento de itens, etc.), **Flexbox** e **CSS Grid** são as ferramentas superiores. Elas são mais poderosas, previsíveis e resolvem os problemas de contenção nativamente, sem a necessidade de hacks.
- **Sempre Contenha Seus Floats:** Se você precisar usar `float`, nunca deixe um contêiner colapsado. Use a técnica de `overflow` (BFC) ou o "clearfix hack" (`::after`) para garantir que o elemento pai se expanda corretamente.
- **Esteja Consciente da Ordem do Código:** O comportamento dos floats e, especialmente, do `clear` é altamente dependente da ordem dos elementos no seu HTML. Isso pode tornar os layouts de float mais frágeis e difíceis de reordenar em designs responsivos. 
- **Não Use Float para Alinhamento Vertical:** `float` é uma propriedade estritamente horizontal. Tentar usá-la para centralizar verticalmente um elemento é uma batalha perdida. Use Flexbox ou Grid para isso.

## Considerações Finais

Neste capítulo, revisitamos a propriedade `float`, uma das técnicas de layout mais antigas e influentes do CSS. Vimos seu propósito original e elegante de permitir que texto flua ao redor de imagens, e como ela foi engenhosamente adaptada para construir layouts complexos por mais de uma década.

Analisamos o principal desafio do `float` — o colapso de seu contêiner — e detalhamos as soluções canônicas, como a propriedade `clear` e as técnicas de "clearfix". Embora hoje tenhamos ferramentas superiores como Flexbox e Grid para a arquitetura geral de layouts, compreender o `float` continua sendo uma habilidade essencial para qualquer desenvolvedor web, seja para manter código legado, seja para aplicar seus efeitos de fluxo de texto em designs modernos e criativos com `shape-outside`.
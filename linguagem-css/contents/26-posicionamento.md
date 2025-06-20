# Capítulo 26 – Posicionamento

Até agora, todos os nossos layouts foram governados por um princípio fundamental: o **fluxo normal do documento**. Elementos de bloco se empilham verticalmente, um após o outro, enquanto elementos em linha fluem horizontalmente dentro de seus contêineres. Este fluxo é a base da web, garantindo que o conteúdo seja legível e organizado por padrão. Mas e se quisermos quebrar essa regra? E se precisarmos colocar um ícone sobre uma imagem, criar um menu que permaneça fixo no topo da tela ou um pop-up que se sobreponha a todo o conteúdo?

Para realizar essas tarefas, precisamos de um controle explícito sobre a localização de um elemento, removendo-o do fluxo normal e posicionando-o exatamente onde queremos. A ferramenta que nos dá esse poder é a propriedade **`position`**.

Neste capítulo, vamos mergulhar nos diferentes esquemas de posicionamento que o CSS oferece. Começaremos com o valor padrão, `static`, que representa o fluxo normal. Em seguida, exploraremos `relative`, que nos permite deslocar um elemento de sua posição original, mas, mais importante, serve como uma "âncora" para outros elementos. Desvendaremos os valores `absolute` e `fixed`, que retiram completamente um elemento do fluxo para posicioná-lo em relação a um ancestral ou à própria janela do navegador. Por fim, veremos o moderno e versátil `sticky`, que combina o comportamento relativo e fixo para criar elementos "pegajosos". Dominar a propriedade `position` é a chave para criar layouts de múltiplas camadas e interfaces de usuário complexas.

## O Fluxo Normal: `position: static`

Este é o **valor padrão** para todos os elementos. Um elemento com `position: static` está "em seu devido lugar", seguindo o fluxo normal do documento. As propriedades de deslocamento (`top`, `right`, `bottom`, `left`) e a propriedade de empilhamento (`z-index`) **não têm nenhum efeito** sobre ele.

**Exemplo:**

```html
<div class="pai">
  <div class="filho estatico">Eu sou estático.</div>
</div>
```

```css
.filho.estatico {
  position: static;
  top: 50px; /* NÃO FUNCIONA */
  left: 50px; /* NÃO FUNCIONA */
  background-color: lightcoral;
}
```

**Resultado:** A caixa permanecerá em sua posição original, como se as propriedades `top` e `left` não tivessem sido declaradas.

## `position: relative`

Este valor é o primeiro passo para sair do fluxo normal. Um elemento com `position: relative` se comporta de forma muito parecida com `static`, mas com duas diferenças cruciais:

1. Ele agora aceita as propriedades de deslocamento (`top`, `right`, `bottom`, `left`), que o movem **em relação à sua posição original** no fluxo.
2. Mesmo quando deslocado, o espaço que ele _originalmente ocuparia_ no layout é **preservado**. Os outros elementos não se movem para preencher o espaço vago.

A utilidade mais importante de `position: relative` não é mover o próprio elemento, mas sim criar um **contexto de posicionamento** para seus elementos filhos que usam `position: absolute`.

**Exemplo Prático (Deslocamento):**

```html
<div class="caixa-a">Caixa A</div>
<div class="caixa-b-relativa">Caixa B (Relativa)</div>
<div class="caixa-c">Caixa C</div>
```

```css
.caixa-a, .caixa-b-relativa, .caixa-c {
  width: 100px; height: 100px; border: 1px solid;
}
.caixa-a { background: lightblue; }
.caixa-c { background: lightgreen; }

.caixa-b-relativa {
  position: relative;
  background: lightcoral;
  /* Move 30px para baixo e 30px para a direita de sua posição original */
  top: 30px;
  left: 30px;
}
```

**Resultado:** A Caixa B se sobreporá à Caixa C. Note que a Caixa C não subiu para ocupar o espaço original da Caixa B; esse espaço foi mantido vago.

## `position: absolute`

Este é um dos valores mais poderosos. Quando você define `position: absolute` em um elemento, acontecem duas coisas:

1. O elemento é **completamente removido do fluxo normal do documento**. Os outros elementos se comportam como se ele não existisse, e o espaço que ele ocupava desaparece.
2. O elemento é posicionado em relação ao seu **ancestral posicionado mais próximo**. Um "ancestral posicionado" é o primeiro elemento pai (ou avô, etc.) que tenha um valor de `position` diferente de `static` (geralmente, `relative`). Se nenhum ancestral posicionado for encontrado, o elemento será posicionado em relação ao contêiner inicial, que é o elemento `<html>`.

**Exemplo Prático (Ícone sobre Imagem):** Este é o padrão de uso clássico: um pai `relative` e um filho `absolute`.

```html
<div class="container-imagem">
  <img src="https://placehold.co/400x250/777/FFF?text=Imagem" alt="Imagem">
  <div class="selo-novo">NOVO</div>
</div>
```

```css
.container-imagem {
  position: relative; /* 1. Cria o contexto de posicionamento */
  width: 400px;
  display: inline-block; /* Apenas para o exemplo não ocupar a largura toda */
}

.selo-novo {
  position: absolute; /* 2. Remove o selo do fluxo normal */
  
  /* 3. Posiciona o selo em relação ao pai (.container-imagem) */
  top: 10px;
  right: 10px;
  
  background-color: #dc3545;
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
}
```

**Resultado:** O selo "NOVO" será posicionado no canto superior direito da imagem, sobrepondo-se a ela, pois está posicionado em relação ao contêiner da imagem, que foi definido como `position: relative`.

## `position: fixed`

O posicionamento `fixed` é semelhante ao `absolute`, pois também remove o elemento do fluxo normal do documento. A diferença crucial é que um elemento com `position: fixed` é posicionado em relação à **viewport** (a janela do navegador), e não a um ancestral.

Isso significa que o elemento **não rola com a página**. Ele permanece "fixo" na mesma posição na tela, não importa o quanto o usuário role para cima ou para baixo.

**Exemplo Prático (Cabeçalho Fixo):**

```html
<header class="cabecalho-fixo">
  Menu Fixo
</header>
<main class="conteudo-principal">
  <!-- Conteúdo longo para permitir rolagem -->
</main>
```

```css
.cabecalho-fixo {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #333;
  color: white;
  padding: 1rem;
  z-index: 100; /* Garante que ele fique acima de outro conteúdo */
}

.conteudo-principal {
  /* Precisamos adicionar um padding/margin no topo do conteúdo para
     evitar que ele comece escondido atrás do cabeçalho fixo. */
  padding-top: 80px; 
}
```

**Resultado:** O cabeçalho preto permanecerá visível no topo da janela, mesmo quando o usuário rolar a página.

## `position: sticky`

Este é um valor híbrido moderno e extremamente útil. Um elemento com `position: sticky` se comporta como `position: relative` até que um determinado ponto de rolagem seja atingido, e a partir desse ponto, ele se comporta como `position: fixed`.

A posição em que ele "gruda" (sticks) é definida por `top`, `right`, `bottom` ou `left`.

**Exemplo Prático (Barra de Navegação Pegajosa):**

```html
<header>Cabeçalho Normal</header>
<nav class="navegacao-pegajosa">
  Navegação que se torna fixa
</nav>
<main>
  <!-- Conteúdo longo -->
</main>
```

```css
.navegacao-pegajosa {
  position: sticky;
  /* "Grude" no topo da viewport quando a rolagem fizer com que
     o topo do elemento atinja 0px de distância do topo da viewport. */
  top: 0;
  
  background-color: #f8f9fa;
  padding: 1rem;
  border-bottom: 1px solid #dee2e6;
}
```

**Resultado:** A barra de navegação começará em sua posição normal no fluxo. Conforme o usuário rola a página para baixo, assim que o topo da barra de navegação estiver prestes a sair da tela, ela "grudará" no topo e permanecerá fixa ali.

## Sobrepondo Elementos com `z-index`

Como vimos anteriormente, uma vez que um elemento é posicionado (com `relative`, `absolute`, `fixed` ou `sticky`), ele ganha a habilidade de participar do empilhamento no eixo Z. A propriedade `z-index` é usada para controlar explicitamente qual elemento aparece na frente do outro. Um `z-index` maior coloca o elemento mais para a frente. Lembre-se que `z-index` só funciona em elementos posicionados.

## Palavras-chave Globais: `initial`, `inherit`, `unset`

- **`initial`**: Reseta a propriedade `position` para seu valor padrão, que é `static`.
- **`inherit`**: Força o elemento a usar o mesmo valor de `position` de seu elemento pai direto.
- **`unset`**: Esta palavra-chave age como `inherit` se a propriedade for herdada por padrão, ou como `initial` caso contrário. Como `position` não é herdada, `unset` se comportará como `initial`, resetando o valor para `static`.

## Boas Práticas de Posicionamento

O posicionamento em CSS é uma das ferramentas mais poderosas do seu arsenal, mas seu uso inadequado pode levar a layouts frágeis e difíceis de manter. Seguir estas diretrizes o ajudará a usá-lo de forma eficaz e profissional.

- **Use o Fluxo Normal Sempre que Possível:** A base de um layout robusto é o fluxo normal do documento. Não recorra ao posicionamento para tarefas que podem ser resolvidas com `margin`, `padding` ou sistemas de layout como Flexbox e Grid.
- **`position: relative` é para Contexto:** O principal uso de `position: relative` não é para mover elementos pela tela (use `transform` para isso, pois é mais performático). Seu superpoder é criar um "porto seguro" ou um contexto de posicionamento para um filho com `position: absolute`.
- **Use `position: absolute` para Overlays e Decorações:** O posicionamento absoluto é perfeito para elementos que precisam se sobrepor a outros sem afetar o fluxo do layout. Pense em ícones, selos, menus suspensos, dicas de contexto (tooltips) e elementos de UI que flutuam sobre o conteúdo.
- **Isolamento é a Chave:** Ao usar `position: absolute`, sempre garanta que haja um ancestral com `position: relative` para conter o elemento. Posicionar elementos em relação ao `<body>` pode levar a resultados inesperados em layouts complexos.
- **Cuidado com `position: fixed`:** Elementos fixos podem ser ótimos para navegação, mas em telas pequenas (mobile), eles podem ocupar um espaço precioso. Considere usar `position: sticky` como uma alternativa mais flexível ou desabilitar o posicionamento fixo em viewports menores com Media Queries.
- **Prefira `sticky` para Efeitos de "Rolagem e Fixação":** Antes de `position: sticky`, criar um cabeçalho que se torna fixo após a rolagem exigia JavaScript. Hoje, `sticky` é a solução nativa, performática e correta em CSS para esse padrão de UI.

## Considerações Finais

Neste capítulo, desvendamos a propriedade `position`, a ferramenta que nos permite quebrar as regras do fluxo normal do documento e colocar elementos exatamente onde precisamos. Vimos a diferença fundamental entre o posicionamento `relative`, que preserva o espaço original, e o posicionamento `absolute` e `fixed`, que removem completamente o elemento do fluxo.

Exploramos o moderno e poderoso `position: sticky`, que nos oferece o melhor dos dois mundos. Compreender como esses esquemas de posicionamento interagem e como eles criam contextos para o `z-index` é uma habilidade essencial para qualquer desenvolvedor front-end. Com este conhecimento, você pode agora construir praticamente qualquer layout, desde simples sobreposições até as interfaces de usuário mais complexas e em camadas.
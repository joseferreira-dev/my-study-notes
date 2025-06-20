# Capítulo 1 – Fundamentos do CSS

Nos capítulos anteriores de nossa jornada, dedicamo-nos a construir o esqueleto de nossas páginas web com HTML. Aprendemos a usar tags para estruturar conteúdo, dar-lhe significado semântico e criar uma base sólida e bem organizada. No entanto, uma estrutura, por mais bem-feita que seja, é apenas o primeiro passo. Uma página HTML pura é como uma tela em branco ou o roteiro de um filme sem atores e sem cenário: funcional, mas sem vida, sem apelo visual e sem a capacidade de guiar o olhar do usuário ou de evocar emoções.

É aqui que entra uma nova e poderosa linguagem: o **CSS (Cascading Style Sheets)**, ou Folhas de Estilo em Cascata. O CSS é a linguagem que usamos para descrever a apresentação de um documento escrito em HTML. É a ferramenta do designer e do desenvolvedor front-end para controlar cores, fontes, espaçamentos, layouts, animações e, em última análise, toda a experiência visual de um site. Se o HTML é a estrutura, o CSS é a decoração, o design e a arte.

Neste capítulo fundamental, mergulharemos nos alicerces do CSS. Começaremos entendendo como o navegador processa o HTML e o CSS para, de fato, desenhar uma página na tela. Investigaremos o que significa o termo "Folhas de Estilo em Cascata" e por que a separação entre conteúdo e apresentação é um dos pilares do desenvolvimento web moderno. Exploraremos as três maneiras de aplicar estilos a um documento — **externa, interna e inline** — e entenderemos qual delas constitui a melhor prática. Por fim, dissecaremos a sintaxe de uma regra CSS e aprenderemos a documentar nosso código. Ao final, você estará pronto para dar os primeiros passos na estilização de suas páginas, transformando a estrutura bruta do HTML em um design coeso e agradável.

## Como o Navegador Renderiza uma Página: DOM, CSSOM e Render Tree

Antes de aplicarmos nosso primeiro estilo, é crucial entender o que acontece nos bastidores quando um navegador carrega uma página web. Esse processo, conhecido como o **Caminho Crítico de Renderização (Critical Rendering Path)**, envolve a criação de duas estruturas de dados essenciais a partir de nossos arquivos HTML e CSS.

1. **O Navegador examina seu HTML e constrói o DOM (Document Object Model).** Quando o navegador recebe o arquivo HTML, ele não o lê como um texto simples. Ele o analisa (parses) e o converte em uma estrutura de árvore de objetos chamada DOM. Cada tag HTML se torna um "nó" nessa árvore, e a hierarquia dos nós reflete a hierarquia das tags no seu código. O DOM é a representação viva e manipulável do seu conteúdo.

<div align="center">
  <img width="700px" src="./img/01-contrucao-do-dom.png">
</div>

2. **O Navegador examina seu CSS e constrói o CSSOM (CSS Object Model).** De forma semelhante, o navegador processa o CSS (seja ele externo, interno ou inline). Ele pega todas as suas regras de estilo e as converte em outra estrutura de árvore, o CSSOM. Esta árvore contém todos os seletores e suas respectivas propriedades. É uma representação de todos os estilos da página. Se você adicionar uma nova regra de estilo via CSSOM (por exemplo, com JavaScript), a página será atualizada.
3. **O Navegador combina o DOM e o CSSOM para criar a Render Tree (Árvore de Renderização).** Esta é a etapa crucial. O navegador percorre a árvore do DOM e, para cada nó visível, encontra as regras correspondentes no CSSOM e as aplica. O resultado é a Render Tree, uma árvore que contém apenas os nós que serão de fato exibidos na tela, junto com todos os seus estilos computados. Nós que não são renderizados (como `<head>`, ou elementos com `display: none;`) não fazem parte da Render Tree.

<div align="center">
  <img width="700px" src="./img/01-dom-cssom-render-tree.png">
</div>

4. **O Navegador exibe sua página.** Finalmente, o navegador usa a Render Tree para realizar duas últimas etapas:
    - **Layout (ou Reflow):** Calcula a geometria de cada nó — onde ele deve ficar na tela e que tamanho deve ter.
    - **Paint (Pintura):** Desenha os pixels de cada nó na tela, com suas cores, textos, bordas e sombras.

Compreender este processo nos ajuda a entender por que a separação de HTML e CSS não é apenas uma boa prática, mas uma parte fundamental da arquitetura da web.

## O Que é CSS? O Pilar da Apresentação

CSS é uma linguagem de estilo que define como os elementos HTML devem ser renderizados na tela, no papel ou em outras mídias. O nome "Cascading Style Sheets" pode ser dividido em duas partes para um melhor entendimento:

1. **Style Sheets (Folhas de Estilo):** Pense em uma folha de estilos como um documento (ou um conjunto de regras) que atua como um manual de identidade visual para a sua página. Em vez de dizer a cada parágrafo, um por um, "você deve ter a cor cinza-escuro e a fonte Arial", você cria uma regra geral em sua folha de estilos que diz: "todos os parágrafos neste site devem ter a cor cinza-escuro e a fonte Arial". Isso torna a manutenção e a consistência do design incrivelmente mais eficientes.
2. **Cascading (em Cascata):** Este é o conceito mais poderoso e, por vezes, mais complexo do CSS. A "cascata" refere-se à ordem de prioridade que os navegadores usam para determinar qual regra de estilo se aplica se houver conflitos. Múltiplas folhas de estilo e regras podem ser aplicadas a um mesmo documento HTML (do navegador, do autor da página e do próprio usuário). A cascata resolve esses conflitos através de um algoritmo que leva em conta a origem do estilo, a especificidade do seletor e a ordem da declaração. Abordaremos a especificidade em detalhes em um capítulo futuro, mas, por ora, entenda que a cascata é o sistema que garante que os estilos sejam aplicados de forma previsível.

A principal filosofia por trás do CSS é a **separação de interesses (Separation of Concerns)**. O HTML deve se preocupar apenas com a estrutura e o significado do conteúdo, enquanto o CSS deve se preocupar apenas com a aparência visual. Essa separação torna o código mais limpo, flexível, acessível e muito mais fácil de manter.

## Incorporando CSS: Externo, Interno e Inline

Existem três maneiras de adicionar regras CSS a um documento HTML. A escolha do método tem implicações diretas na manutenção, performance e organização do seu projeto.

### CSS Inline (Estilo em Linha)

O método inline consiste em adicionar estilos diretamente a um elemento HTML específico através do atributo global `style`.

**Como funciona:** Você adiciona o atributo `style` a uma tag HTML e, como seu valor, insere as declarações de propriedade e valor do CSS.

**Exemplo:**

```html
<p style="color: blue; font-size: 18px;">Este parágrafo será azul e terá uma fonte de 18 pixels.</p>
<h1 style="background-color: yellow;">Este título terá um fundo amarelo.</h1>
```

**Conclusão:** **Evite o uso de CSS inline sempre que possível.** Seu uso é geralmente restrito a situações muito específicas, como em e-mails HTML ou quando estilos são aplicados dinamicamente via JavaScript.

### CSS Interno (Internal CSS)

O CSS interno envolve a colocação das regras de CSS dentro do próprio documento HTML, mas de forma separada dos elementos, utilizando a tag `<style>`.

**Como funciona:** Você adiciona a tag `<style>` dentro da seção `<head>` do seu documento HTML. Dentro dela, você escreve todas as suas regras CSS.

**Exemplo:**

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body { background-color: #f0f0f0; }
    h1 { color: navy; text-align: center; }
  </style>
</head>
<body>
  <h1>Um Título Estilizado</h1>
</body>
</html>
```

**Conclusão:** O CSS interno é aceitável para projetos de uma única página ou para estilos que são verdadeiramente exclusivos de uma página específica.

### CSS Externo (External CSS)

Este é o método mais comum e **altamente recomendado**. Ele consiste em criar um arquivo separado com a extensão `.css` para armazenar todas as suas regras de estilo.

**Como funciona:**

1. Crie um arquivo (ex: `estilos.css`).
2. Escreva suas regras CSS dentro deste arquivo.
3. No seu arquivo HTML, dentro da seção `<head>`, utilize a tag `<link>` para "vincular" sua folha de estilos.

**Exemplo:**

`index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="estilos.css">
</head>
<body>
  <h1>Bem-vindo ao Meu Site!</h1>
</body>
</html>
```

`estilos.css`:

```css
h1 {
  color: green;
}
```

**Conclusão:** **Sempre prefira usar folhas de estilo externas.** É a abordagem profissional e escalável para o desenvolvimento web.

## A Regra `@import`

Existe outra forma de incluir uma folha de estilos em outra: a regra `@import`. Ela pode ser usada no topo de um arquivo CSS para importar o conteúdo de outro.

**Recomendação:** Prefira sempre usar múltiplas tags `<link>` no HTML em vez de `@import` dentro do CSS, pois `<link>` permite o download paralelo de arquivos, o que é melhor para a performance.

## Sintaxe de uma Regra CSS

Toda regra CSS é composta por um **seletor** e um **bloco de declaração**.

```css
/* Seletor -> h1 */
h1 {
  /* Bloco de Declaração */
  color: navy; /* <-- Declaração 1 */
  font-size: 24px; /* <-- Declaração 2 */
}
```

- **Seletor (`h1`):** Aponta para o(s) elemento(s) HTML a serem estilizados.
- **Bloco de Declaração (`{ ... }`):** Contém as regras de estilo.
- **Declaração (`color: navy;`):** Uma única regra composta por uma **propriedade** (`color`) e um **valor** (`navy`), terminando com um ponto-e-vírgula (`;`).

## Considerações Finais

Neste capítulo, estabelecemos a fundação do nosso conhecimento em CSS. Vimos como o navegador transforma nossos arquivos em árvores de objetos (DOM e CSSOM) para renderizar a página. Compreendemos o propósito do CSS como a linguagem de apresentação da web e a importância de separar o estilo da estrutura. Analisamos os três métodos de incorporação de CSS e cravamos o uso de folhas de estilo externas como a prática profissional definitiva. Por fim, dissecamos a anatomia de uma regra CSS, identificando seus componentes vitais.

Com esta base sólida, estamos agora equipados para o próximo passo: mergulhar no vasto e poderoso mundo dos seletores de CSS.
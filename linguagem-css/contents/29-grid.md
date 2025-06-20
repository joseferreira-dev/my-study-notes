# Capítulo 29 – Layout Bidimensional com CSS Grid

No capítulo anterior, desvendamos o Flexbox e seu domínio sobre o layout unidimensional. Ele resolveu elegantemente o problema de alinhar e distribuir itens em uma única linha ou coluna. No entanto, o design de páginas web raramente é unidimensional. Pense na estrutura de um site de notícias, no painel de um aplicativo ou em qualquer layout que precise de alinhamento tanto nas linhas quanto nas colunas simultaneamente. Para esses desafios, precisamos de uma ferramenta ainda mais poderosa.

Bem-vindo ao **CSS Grid Layout**. O Grid é o primeiro sistema de layout do CSS projetado especificamente para resolver o problema do layout de grade bidimensional. Ele nos dá um controle sem precedentes sobre a arquitetura de nossas páginas, permitindo-nos criar layouts complexos, responsivos e de fácil manutenção com um código surpreendentemente simples e legível. Com o Grid, podemos esquecer os truques com `float` e até mesmo as limitações do Flexbox quando o assunto é o controle de duas dimensões.

Neste capítulo, faremos um mergulho profundo no CSS Grid. Começaremos entendendo seus conceitos fundamentais — a grade, as linhas, as trilhas e as áreas. Em seguida, aprenderemos a definir a estrutura de nossa grade no elemento contêiner, usando as propriedades `grid-template-columns`, `grid-template-rows` e `gap`. Exploraremos a revolucionária unidade `fr`, a função `repeat()` para um código mais limpo e a função `minmax()` para criar grades flexíveis. Por fim, veremos como posicionar os elementos filhos (itens) dentro dessa grade com precisão, seja usando as linhas da grade ou o método visual e semântico de `grid-template-areas`.

## Conceitos Fundamentais do Grid

Para trabalhar com Grid, precisamos nos familiarizar com sua terminologia:

- **Contêiner da Grade (Grid Container):** O elemento pai que tem `display: grid` aplicado.
- **Item da Grade (Grid Item):** Os filhos diretos do contêiner da grade.
- **Linhas da Grade (Grid Lines):** As linhas divisórias horizontais e verticais que formam a estrutura da grade. Elas são numeradas, começando em 1.
- **Trilhas da Grade (Grid Tracks):** O espaço entre duas linhas de grade adjacentes. Uma trilha pode ser uma coluna ou uma linha.
- **Célula da Grade (Grid Cell):** O menor espaço em uma grade, formado pela interseção de uma linha e uma coluna.
- **Área da Grade (Grid Area):** Uma área retangular composta por uma ou mais células da grade.

## Ativando o Grid: `display: grid`

Tudo começa aplicando `display: grid` ao elemento contêiner. A partir deste momento, ele se torna um contêiner de grade e seus filhos diretos se tornam itens da grade.

```html
<div class="grid-container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
</div>
```

```css
.grid-container {
  display: grid;
  /* Sem definir colunas ou linhas, o grid cria uma única coluna por padrão */
}
.item {
  background-color: lightblue;
  border: 1px solid steelblue;
  padding: 20px;
  text-align: center;
}
```

## Definindo a Estrutura: `grid-template-columns` e `grid-template-rows`

Estas são as propriedades mais importantes do contêiner. Elas definem as trilhas (colunas e linhas) da sua grade. Você define o tamanho de cada trilha em uma lista separada por espaços.

**Exemplo (Grade 3x2 com tamanhos fixos):**

```css
.grid-container {
  display: grid;
  /* Cria 3 colunas: 200px, 150px e 300px de largura */
  grid-template-columns: 200px 150px 300px;
  /* Cria 2 linhas: 100px e 150px de altura */
  grid-template-rows: 100px 150px;
}
```

### A Unidade `fr` (Fração)

A unidade `fr` foi criada especificamente para o Grid. Ela representa uma fração do **espaço livre** no contêiner. É a ferramenta perfeita para criar colunas flexíveis.

**Exemplo:**

```css
.grid-container {
  display: grid;
  width: 1000px; /* Largura total do contêiner */
  /* Cria 3 colunas. A segunda é fixa. As outras duas dividem o espaço restante (800px).
     A primeira coluna (2fr) terá 2/3 do espaço livre (aprox. 533px).
     A terceira coluna (1fr) terá 1/3 do espaço livre (aprox. 267px). */
  grid-template-columns: 2fr 200px 1fr;
}
```

### A Função `repeat()`

Para evitar escrever valores repetitivos, podemos usar a função `repeat()`.

```css
/* Em vez de: grid-template-columns: 1fr 1fr 1fr 1fr; */
.grid-container {
  display: grid;
  /* Cria 4 colunas, cada uma com 1 fração do espaço */
  grid-template-columns: repeat(4, 1fr);
}
```

### A Função `minmax()`

Esta função define um intervalo de tamanho para uma trilha, garantindo que ela seja flexível, mas nunca menor ou maior que certos limites. **Sintaxe:** `minmax(tamanho-minimo, tamanho-maximo)`

**Exemplo (Grid responsivo):**

```css
.grid-container {
  display: grid;
  /* auto-fill: cria quantas colunas couberem no contêiner.
     minmax(): cada coluna terá no mínimo 200px de largura, mas poderá
     crescer para ocupar 1fr do espaço disponível. */
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

Este código cria uma grade de "cards" que se ajusta automaticamente: em telas largas, você terá várias colunas; em telas estreitas, os itens quebrarão para as linhas de baixo, mas nunca ficarão mais estreitos que 200px.

## Definindo o Espaçamento: `gap`

Em vez de usar `margin` nos itens da grade para criar espaçamento (o que pode ser problemático), o CSS Grid oferece a propriedade `gap` para definir o espaço _entre_ as trilhas.

- **`column-gap`**: Define o espaço entre as colunas.
- **`row-gap`**: Define o espaço entre as linhas.
- **`gap`** (atalho): Define ambos. Se dois valores são fornecidos (`gap: 20px 30px;`), o primeiro é para `row-gap` e o segundo para `column-gap`. Se apenas um valor é fornecido, ele se aplica a ambos.

**Exemplo:**

```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  /* Cria um espaço de 20px entre as linhas e 15px entre as colunas */
  gap: 20px 15px;
}
```

## Posicionando Itens com Linhas da Grade

Por padrão, os itens da grade são colocados automaticamente. No entanto, podemos controlar exatamente onde cada item começa e termina usando as linhas da grade numeradas.

Lembre-se: as linhas são numeradas a partir de 1. Em uma grade de 3 colunas, há 4 linhas de coluna (1, 2, 3 e 4).

### `grid-column-start`, `grid-column-end`, `grid-row-start`, `grid-row-end`

Estas propriedades definem em qual linha de grade um item começa e termina.

**Exemplo (Layout de Blog):**

```html
<div class="blog-layout">
  <header class="blog-header">Header</header>
  <main class="blog-main">Main Content</main>
  <aside class="blog-sidebar">Sidebar</aside>
  <footer class="blog-footer">Footer</footer>
</div>
```

```css
.blog-layout {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 colunas iguais */
  grid-template-rows: auto 1fr auto; /* 3 linhas: altura automática, flexível, altura automática */
  gap: 15px;
}

.blog-header {
  grid-column-start: 1;
  grid-column-end: 4; /* Ocupa da linha de coluna 1 até a 4 (todas as 3 colunas) */
}

.blog-main {
  grid-column-start: 1;
  grid-column-end: 3; /* Ocupa da linha 1 até a 3 (as duas primeiras colunas) */
}

.blog-sidebar {
  grid-column-start: 3;
  grid-column-end: 4; /* Ocupa da linha 3 até a 4 (a última coluna) */
  grid-row-start: 2;
  grid-row-end: 3; /* Garante que ele fique na segunda linha */
}

.blog-footer {
  grid-column-start: 1;
  grid-column-end: 4; /* Ocupa todas as colunas, na última linha */
}
```

### `grid-column` e `grid-row` (Shorthand)

Estas propriedades de atalho combinam as propriedades `start` e `end` em uma única linha, separadas por uma barra (`/`).

**O mesmo exemplo acima, com atalhos:**

```css
.blog-header { grid-column: 1 / 4; }
.blog-main { grid-column: 1 / 3; }
.blog-sidebar { grid-column: 3 / 4; grid-row: 2 / 3; }
.blog-footer { grid-column: 1 / 4; }
```

### A Palavra-chave `span`

Em vez de especificar a linha final, muitas vezes é mais fácil dizer "este item deve ocupar X colunas/linhas". A palavra-chave `span` faz exatamente isso.

**O mesmo exemplo, com `span`:**

```css
.blog-header { grid-column: 1 / span 3; }
.blog-main { grid-column: 1 / span 2; }
.blog-sidebar { grid-column: 3 / span 1; grid-row: 2 / span 1; }
.blog-footer { grid-column: 1 / span 3; }
```

## Posicionando Itens com `grid-template-areas`

Este é um método alternativo e incrivelmente visual para posicionar itens. Ele permite que você nomeie as áreas da sua grade e, em seguida, atribua itens a essas áreas nomeadas.

**Passo 1: Nomeie os itens** No CSS, dê a cada item da grade um nome usando a propriedade `grid-area`.

```css
.blog-header { grid-area: cabecalho; }
.blog-main { grid-area: principal; }
.blog-sidebar { grid-area: lateral; }
.blog-footer { grid-area: rodape; }
```

**Passo 2: "Desenhe" o layout no contêiner** Use a propriedade `grid-template-areas` no contêiner da grade. Cada string entre aspas representa uma linha da grade. Você usa os nomes definidos no passo anterior para preencher as células. Um ponto (`.`) representa uma célula vazia.

```css
.blog-layout {
  display: grid;
  gap: 15px;
  /* Definimos as colunas e linhas implicitamente com as áreas */
  grid-template-columns: 2fr 1fr;
  grid-template-areas:
    "cabecalho cabecalho"
    "principal lateral"
    "rodape    rodape";
}
```

**Resultado:** Este código CSS cria o mesmo layout do exemplo de blog, mas de uma forma muito mais legível e fácil de manter. Você pode visualizar o layout diretamente no seu CSS!

## Boas Práticas com Grid

O CSS Grid é a ferramenta de layout mais poderosa que temos. Usá-la corretamente permite a criação de designs que antes eram impossíveis ou extremamente complexos, de forma limpa e semântica.

- **Grid para a Macroestrutura:** O Grid brilha no layout geral da página. Use-o para definir a posição do seu cabeçalho, rodapé, barra lateral, área de conteúdo principal e outros grandes blocos de construção. Ele oferece um controle bidimensional que o Flexbox não possui.
- **Combine com Flexbox:** Não pense em "Grid vs. Flexbox". Pense em "Grid _e_ Flexbox". Use o Grid para o layout da página (duas dimensões) e o Flexbox para alinhar os itens _dentro_ de um item da grade (uma dimensão). Por exemplo, o seu cabeçalho pode ser um item da grade, mas os links de navegação _dentro_ dele são perfeitamente alinhados com `display: flex`.
- **Use a Ferramenta de Grid do Navegador:** Todos os navegadores modernos vêm com excelentes ferramentas de desenvolvimento que permitem visualizar as linhas, trilhas e áreas da sua grade. Ative-as! É a melhor maneira de depurar e entender o que seu código está fazendo.
- **Adote `gap` para Espaçamento:** A propriedade `gap` (ou `grid-gap`) é a maneira mais limpa e moderna de criar espaçamento entre as trilhas da grade. Ela evita problemas com margens em itens de borda.
- **Explore `grid-template-areas` para Clareza:** Para layouts complexos, considere nomear suas áreas com `grid-template-areas`. Isso torna seu CSS incrivelmente visual e fácil de entender, pois você "desenha" seu layout no código.
- **Acessibilidade Primeiro:** O Grid permite que você reordene os elementos visualmente, independentemente de sua ordem no HTML. Use este poder com responsabilidade. A ordem padrão de navegação com a tecla `Tab` seguirá a ordem do HTML, não a ordem visual do Grid. Garanta que a ordem do seu código fonte faça sentido lógico.

## Considerações Finais

Neste capítulo, desvendamos o CSS Grid Layout, o mais poderoso e expressivo sistema de layout disponível no CSS. Vimos como, com apenas algumas propriedades no contêiner, podemos definir estruturas bidimensionais complexas e flexíveis, usando a unidade `fr` e funções como `repeat()` e `minmax()`.

Aprendemos a posicionar itens com precisão cirúrgica dentro dessa grade, fazendo-os ocupar uma ou mais células para formar layouts robustos e fáceis de manter. A combinação do Grid para a macroestrutura e do Flexbox para o alinhamento de componentes internos representa o padrão-ouro do design de layout na web moderna. Com o domínio sobre o Grid, você está verdadeiramente equipado para construir qualquer layout que possa imaginar.
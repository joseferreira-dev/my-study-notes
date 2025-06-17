# Capítulo 5 – Pseudo-classes: Estilizando Estados e Estrutura

Nos capítulos anteriores, construímos um robusto arsenal de seletores. Aprendemos a mirar em elementos por seu tipo, ID, classe, atributos e sua posição na hierarquia do documento. Com essas ferramentas, já podemos estilizar praticamente qualquer elemento estático em uma página. Mas a web não é estática; ela é interativa. Os usuários clicam em links, passam o mouse sobre botões, preenchem formulários e navegam pela página. Como podemos fazer nossos estilos reagirem a essas ações e estados?

A resposta está nas **pseudo-classes**. Uma pseudo-classe é uma palavra-chave, precedida por dois-pontos (`:`), que adicionamos a um seletor para especificar um **estado especial** do elemento selecionado. Elas não selecionam o elemento em si, mas o elemento **enquanto ele está em uma determinada condição** — seja um link que já foi visitado, uma caixa de seleção que está marcada, ou o primeiro item em uma lista. Elas são "pseudo" classes porque não existem no código HTML como `class="hover"`; em vez disso, são aplicadas dinamicamente pelo navegador.

Neste capítulo abrangente, vamos explorar o vasto e poderoso mundo das pseudo-classes. Elas são a chave para a criação de designs verdadeiramente dinâmicos e responsivos. Vamos agrupá-las em categorias para facilitar o entendimento: interações do usuário, estados de formulário, e seleção estrutural baseada na posição. De `:hover` a `:nth-child()`, dominar as pseudo-classes é o que separa uma página estática de uma experiência de usuário rica e interativa.

## Pseudo-classes de Interação do Usuário (Dinâmicas)

Estas pseudo-classes aplicam estilos com base em ações que o usuário está realizando.

### `:link` e `:visited`

Estas pseudo-classes se aplicam exclusivamente a links (`<a>` com atributo `href`).

- **`:link`**: Seleciona um link que ainda não foi visitado pelo usuário.
- **`:visited`**: Seleciona um link que o usuário já visitou.

**Nota de Privacidade:** Por razões de privacidade, os navegadores limitam severamente os estilos que você pode aplicar a `:visited`. Geralmente, você só pode alterar propriedades de cor (`color`, `background-color`, `border-color`, etc.).

```css
/* Links não visitados serão azuis */
a:link {
  color: blue;
}

/* Links visitados serão roxos */
a:visited {
  color: purple;
}
```

### `:hover`

Aplica-se quando o ponteiro do mouse do usuário está sobre um elemento. É uma das pseudo-classes mais usadas para fornecer feedback visual.

```html
<button class="btn-subscribe">Inscreva-se</button>
```

```css
.btn-subscribe:hover {
  background-color: #0056b3; /* Escurece o botão ao passar o mouse */
  cursor: pointer;
}
```

### `:active`

Aplica-se enquanto um elemento está sendo ativado pelo usuário. Para um link ou botão, é o breve momento entre o clique do mouse e a sua liberação.

```css
.btn-subscribe:active {
  transform: translateY(2px); /* Simula o botão sendo pressionado */
  box-shadow: none;
}
```

**Ordem Recomendada (LVHA):** Para que os seletores de link funcionem corretamente, é uma convenção usar a ordem **L**ink -> **V**isited -> **H**over -> **A**ctive. Uma maneira fácil de lembrar é "LoVe HAte".

### `:focus`

Aplica-se a um elemento quando ele recebe foco, o que pode ocorrer quando o usuário clica em um campo de formulário ou navega até um elemento usando a tecla `Tab`. É crucial para a acessibilidade.

```css
input:focus {
  border-color: #007bff;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
  outline: none; /* Remove o contorno padrão para usar o box-shadow */
}
```

### `:focus-within`

Esta é uma pseudo-classe mais poderosa. Ela se aplica a um elemento se ele mesmo ou **qualquer um de seus descendentes** estiver com o foco.

```html
<form>
  <label for="nome">Nome:</label>
  <input type="text" id="nome">
</form>
```

```css
form:focus-within {
  background-color: #eef5ff; /* Destaca todo o formulário quando um campo dentro dele está focado */
  border: 1px solid #007bff;
}
```

## Pseudo-classes de Localização e Estado da UI

Este grupo seleciona elementos com base em seu estado na interface do usuário ou na URL.

### `:target`

Seleciona o elemento cujo `id` corresponde ao fragmento da URL atual (a parte após o `#`).

```html
<a href="#secao2">Ir para a Seção 2</a>
<section id="secao1">Conteúdo 1</section>
<section id="secao2">Conteúdo 2</section>
```

```css
section:target {
  background-color: yellow; /* Destaca a seção que é o alvo do link */
}
```

Ao clicar no link, a URL se torna `...#secao2`, e a seção com `id="secao2"` será destacada.

### `:fullscreen`

Seleciona um elemento que está sendo exibido em modo de tela cheia.

```css
video:fullscreen {
  width: 100vw;
  height: 100vh;
}
```

### `:root`

Seleciona o elemento raiz do documento. Em HTML, este é sempre o elemento `<html>`. É mais específico que o seletor `html` e é comumente usado para declarar **Variáveis CSS (Propriedades Customizadas)**.

```css
:root {
  --cor-principal: #007bff;
  --fonte-padrao: 'Helvetica', sans-serif;
}

body {
  color: var(--cor-principal);
  font-family: var(--fonte-padrao);
}
```

## Pseudo-classes de Formulários e Inputs

Este é um grupo extenso que permite estilizar elementos de formulário com base em seu estado.

### `:enabled` e `:disabled`

- **`:enabled`**: Seleciona um elemento de UI (como `<input>`, `<button>`) que está habilitado.
- **`:disabled`**: Seleciona um elemento de UI que está desabilitado (possui o atributo `disabled`).

```css
input[type="text"]:disabled {
  background-color: #e9ecef;
  cursor: not-allowed;
}
```

### `:checked`

Seleciona qualquer radio button (`<input type="radio">`) ou checkbox (`<input type="checkbox">`) que esteja marcado.

```css
input[type="checkbox"]:checked + label {
  text-decoration: line-through; /* Risca o texto do label quando a caixa é marcada */
  color: #6c757d;
}
```

### `:indeterminate`

Seleciona um checkbox que está em um estado "indeterminado", que só pode ser definido via JavaScript. É útil para interfaces com checkboxes "pai" que controlam grupos de "filhos".

```css
input[type="checkbox"]:indeterminate {
  box-shadow: 0 0 0 2px green; /* Aplica uma sombra para indicar o estado */
}
```

### `:default`

Seleciona o elemento de UI que é o padrão entre um grupo de elementos semelhantes. Por exemplo, o botão de "submit" padrão de um formulário.

```css
input[type="submit"]:default {
  border: 2px solid blue;
}
```

### `:optional` e `:required`

- **`:optional`**: Seleciona um `<input>` ou `<textarea>` que **não** possui o atributo `required`.
- **`:required`**: Seleciona um `<input>` ou `<textarea>` que **possui** o atributo `required`.

```css
input:required {
  border-left: 3px solid red;
}

input:optional {
  border-left: 3px solid green;
}
```

### `:valid` e `:invalid`

- **`:valid`**: Seleciona um `<input>` cujo conteúdo é validado com sucesso de acordo com suas restrições (ex: `<input type="email">` com um e-mail válido).
- **`:invalid`**: Seleciona um `<input>` cujo conteúdo falha na validação.

```css
input:invalid {
  background-color: #fff2f2;
}
```

### `:in-range` e `:out-of-range`

Aplicam-se a elementos com restrições de intervalo (como `<input type="number">` com atributos `min` e `max`).

- **`:in-range`**: O valor está dentro do intervalo permitido.
- **`:out-of-range`**: O valor está fora do intervalo permitido.

```html
<input type="number" min="1" max="10">
```

```css
input[type="number"]:out-of-range {
  border-color: red;
}
````

### `:read-only` e `:read-write`

- **`:read-only`**: Seleciona um elemento cujo conteúdo não pode ser editado pelo usuário (possui o atributo `readonly`).
- **`:read-write`**: Seleciona um elemento cujo conteúdo pode ser editado (como um `<input>` padrão ou um elemento com `contenteditable="true"`).

```css
input:read-only {
  background-color: #f8f9fa;
}
```

### `:placeholder-shown`

Seleciona um `<input>` ou `<textarea>` enquanto ele está exibindo seu texto de placeholder. Ele deixa de ser aplicado assim que o usuário digita algo.

```css
input:placeholder-shown {
  border-style: dashed;
}
```

## Pseudo-classes Estruturais e Posicionais

Este grupo poderoso seleciona elementos com base em sua posição na árvore do documento.

### `:first-child` e `:last-child`

- **`:first-child`**: Seleciona um elemento que é o **primeiro filho** de seu pai.
- **`:last-child`**: Seleciona um elemento que é o **último filho** de seu pai.

```css
/* Remove a borda superior do primeiro item da lista */
li:first-child {
  border-top: none;
}

/* Deixa o último parágrafo de um artigo em negrito */
article p:last-child {
  font-weight: bold;
}
```

### `:only-child`

Seleciona um elemento que é o **único filho** de seu pai.

```css
div p:only-child {
  color: red; /* Se um <p> for o único elemento dentro de uma <div>, ele será vermelho. */
}
```

### `:first-of-type` e `:last-of-type`

Semelhante a `first-child`, mas mais específico ao tipo.

- **`:first-of-type`**: Seleciona o primeiro elemento **do seu tipo** entre um grupo de irmãos.
- **`:last-of-type`**: Seleciona o último elemento **do seu tipo** entre um grupo de irmãos.

```html
<section>
  <h1>Título</h1>
  <p>Primeiro parágrafo.</p>
  <p>Segundo parágrafo.</p>
  <h2>Subtítulo</h2>
</section>
```

```css
p:first-of-type { /* Seleciona o primeiro <p> dentro da <section> */
  font-weight: bold;
}
h1:first-of-type { /* Também seleciona o <h1>, pois ele é o primeiro do seu tipo */
  color: green;
}
````

### `:empty`

Seleciona um elemento que não tem filhos. Isso inclui elementos sem filhos de texto. `<div></div>` é `:empty`, mas `<div> </div>` não é.

```css
div:empty {
  padding: 20px;
  border: 1px dashed #ccc; /* Útil para visualizar divs vazias durante o desenvolvimento */
}
```

### `:nth-child(an+b)`

Esta é a pseudo-classe estrutural mais poderosa. Ela seleciona elementos com base em uma fórmula.

- `n` é um contador que começa em 0.
- `a` é o multiplicador.
- `b` é o deslocamento.

Vamos aos exemplos:

- **`:nth-child(3)`**: Seleciona o terceiro filho.
- **`:nth-child(odd)` ou `:nth-child(2n+1)`**: Seleciona os filhos ímpares (1º, 3º, 5º, etc.).
- **`:nth-child(even)` ou `:nth-child(2n)`**: Seleciona os filhos pares (2º, 4º, 6º, etc.).
- **`:nth-child(3n)`**: Seleciona a cada 3 filhos (3º, 6º, 9º, etc.).
- **`:nth-child(n+3)`**: Seleciona todos os filhos a partir do terceiro (3º, 4º, 5º, etc.).
- **`:nth-child(-n+3)`**: Seleciona os primeiros três filhos (1º, 2º, 3º).

```css
/* Estilo "zebra" para linhas de uma tabela */
tbody tr:nth-child(odd) {
  background-color: #f9f9f9;
}

/* Destaca os 3 primeiros itens de uma lista */
ul li:nth-child(-n+3) {
  font-weight: bold;
}
```

### `:nth-last-child(an+b)`

Funciona exatamente como `:nth-child`, mas **contando a partir do final**.

```css
/* Seleciona o penúltimo item da lista */
li:nth-last-child(2) {
  border-bottom: 2px solid red;
}
```

### `:nth-of-type(an+b)` e `:nth-last-of-type(an+b)`

Funcionam com a mesma lógica de `:nth-child`, mas consideram apenas os elementos **do mesmo tipo**. Isso é útil quando você tem elementos misturados.

```html
<div>
  <h1>Título 1</h1>
  <p>Parágrafo 1</p>
  <p>Parágrafo 2</p>
  <h1>Título 2</h1>
  <p>Parágrafo 3</p>
</div>
```

```css
/* Seleciona o segundo parágrafo, ignorando os h1s na contagem */
p:nth-of-type(2) { /* Seleciona <p>Parágrafo 2</p> */
  color: blue;
}
````

## Pseudo-classes Funcionais e Outras

### `:not(seletor)`

A pseudo-classe de negação. Ela seleciona qualquer elemento que **não** corresponda ao seletor passado como argumento.

```css
/* Aplica o estilo a todos os inputs, exceto os do tipo "submit" */
input:not([type="submit"]) {
  width: 100%;
}
```

### `:lang(codigo-idioma)`

Seleciona um elemento com base no seu idioma, especificado pelo atributo `lang`.

```html
<p lang="en">This is in English.</p>
<p lang="fr">Ceci est en français.</p>
```

```css
p:lang(fr) {
  font-style: italic; /* Adiciona citações estilizadas para o texto em francês */
  quotes: "« " " »";
}
````

### `:is(seletor1, seletor2)`

A pseudo-classe de correspondência. Ela simplifica seletores longos ao agrupar diferentes seletores em uma única regra. É útil quando você tem seletores aninhados complexos que se repetem. Historicamente, era conhecida como `:any()` ou `:matches()`.

```css
/* Forma longa */
header h1, header h2, header h3 {
  color: navy;
}
article h1, article h2, article h3 {
  color: darkred;
}

/* Forma curta com :is() */
header :is(h1, h2, h3) {
  color: navy;
}
article :is(h1, h2, h3) {
  color: darkred;
}
```

### `:where(seletor1, seletor2)`

Funciona exatamente como `:is()`, mas com uma diferença crucial: `:where()` tem **especificidade zero**. Isso o torna ideal para criar estilos base que podem ser facilmente sobrescritos.

### `:first`, `:left` e `:right`:  Pseudo-classes de Mídia Paginada

Estas pseudo-classes não se aplicam a elementos HTML comuns na tela. Elas são usadas dentro da regra `@page` para estilizar a impressão de documentos.

- **`:first`**: Estiliza a primeira página do documento impresso.
- **`:left`** e **`:right`**: Estilizam as páginas esquerdas e direitas, respectivamente, para impressão frente e verso.

```css
@page :first {
  margin-top: 4cm; /* Margem maior na primeira página */
}

@page :left {
  margin-right: 3cm;
}

@page :right {
  margin-left: 3cm;
}
```

### `:scope`

Representa um ponto de referência para os seletores. Seu uso principal é em conjunto com JavaScript (usando `element.querySelector()`) para buscar elementos apenas dentro de um elemento específico (o `scope` ou escopo).

## Considerações Finais

Neste capítulo, exploramos o vasto e poderoso universo das **pseudo-classes**, a ferramenta que dá vida e interatividade aos nossos designs. Vimos como podemos estilizar elementos com base nas ações do usuário com `:hover` e `:focus`, criando feedback visual imediato. Mergulhamos no mundo dos formulários, utilizando pseudo-classes como `:checked`, `:valid` e `:required` para criar interfaces mais intuitivas e inteligentes.

O grande salto, no entanto, veio com as **pseudo-classes estruturais**. Ferramentas como `:first-child`, `:last-child` e a incrivelmente versátil família `:nth-child()` nos deram o poder de selecionar elementos com uma precisão matemática, baseada em sua ordem e posição na árvore do documento. Essa capacidade nos permite criar layouts complexos e designs rítmicos com um CSS limpo e sem a necessidade de classes extras.

Até agora, focamos em selecionar elementos inteiros em estados diferentes. Mas e se quiséssemos estilizar apenas uma _parte_ de um elemento, como a primeira letra de um parágrafo ou um ícone que não existe no HTML? Para isso, precisaremos do nosso último tipo de seletor: os **pseudo-elementos**, que serão o tema do nosso próximo capítulo.
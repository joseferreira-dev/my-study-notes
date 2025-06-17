# Capítulo 4 – Seletores de Atributo: Estilização Baseada em Metadados

Até agora em nossa jornada pelo CSS, aprendemos a selecionar elementos com base em seu tipo (tag), seus identificadores únicos (ID), seus agrupamentos (classes) e sua posição na hierarquia do documento (descendentes, filhos e irmãos). Construímos um alicerce sólido que já nos permite um controle considerável sobre a aparência de uma página. No entanto, há uma outra camada de informação em nosso HTML que podemos explorar para criar estilos ainda mais inteligentes e contextuais: os **atributos** das tags.

Cada elemento HTML pode conter atributos que fornecem metadados ou configuram seu comportamento — `href` em um link, `src` em uma imagem, `type` em um campo de formulário, ou até mesmo atributos customizados que nós mesmos criamos. E se pudéssemos aplicar estilos com base na presença ou no valor desses atributos? E se quiséssemos que todos os links que apontam para um site externo tivessem um ícone? Ou que todos os campos de formulário obrigatórios tivessem uma borda vermelha? É exatamente isso que os **seletores de atributo** nos permitem fazer.

Neste capítulo, vamos desvendar este poderoso conjunto de seletores. Eles nos dão a capacidade de mirar em elementos não pelo que eles são (`p`, `div`), mas pelo que eles _têm_ ou pelo que seus atributos _dizem_. Exploraremos como selecionar elementos pela simples presença de um atributo, por um valor exato, ou por correspondências parciais de texto — como valores que começam, terminam ou simplesmente contêm uma determinada string. Dominar os seletores de atributo é um passo fundamental para escrever um CSS mais limpo e semântico, reduzindo a nossa dependência de classes e IDs para cada pequena variação de estilo.

## A Estrutura de um Seletor de Atributo

Todos os seletores de atributo são definidos dentro de colchetes `[]`. A sintaxe pode variar de uma simples verificação de presença a uma complexa correspondência de substring, mas a base é sempre a mesma.

## Seletor de Presença de Atributo `[attr]`

Este é o seletor de atributo mais básico. Ele seleciona qualquer elemento que simplesmente **possui** o atributo especificado, não importando qual seja o seu valor.

**Sintaxe:**

```css
[nome-do-atributo] {
  /* declarações */
}
```

**Exemplo Prático:** Uma excelente utilização é para fins de acessibilidade. Podemos, por exemplo, aplicar um estilo distinto a todas as imagens que possuem o atributo `alt` (texto alternativo), nos ajudando a identificar visualmente durante o desenvolvimento quais imagens estão em conformidade. Ou, podemos adicionar um ícone a todos os links que possuem o atributo `target`, indicando que eles abrirão em uma nova aba.

```html
<!-- HTML -->
<a href="/interno">Link Interno</a>
<a href="https://google.com" target="_blank">Google (abre em nova aba)</a>
<a href="/outro" title="Descrição do link">Link com Título</a>
```

```css
/* CSS */
/* Seleciona TODOS os links que possuem o atributo target */
a[target] {
  /* Adiciona um pequeno ícone indicando que o link abrirá em nova janela/aba */
  background-image: url('caminho/para/icone-externo.svg');
  background-repeat: no-repeat;
  background-position: center right;
  padding-right: 18px; /* Espaço para o ícone */
}

/* Seleciona TODOS os links que possuem um atributo title */
a[title] {
  border-bottom: 1px dotted blue;
  cursor: help;
}
```

## Seletor de Valor Exato `[attr="valor"]`

Este seletor eleva a precisão, selecionando elementos cujo atributo possui um **valor exato** e específico. A correspondência diferencia maiúsculas de minúsculas por padrão.

**Sintaxe:**

```css
[nome-do-atributo="valor-exato"] {
  /* declarações */
}
```

**Exemplo Prático:** É extremamente útil para estilizar elementos de formulário com base em seu tipo.

```html
<!-- HTML -->
<form>
  <label>Nome:</label>
  <input type="text" placeholder="Digite seu nome">
  <label>Senha:</label>
  <input type="password" placeholder="Digite sua senha">
  <input type="submit" value="Entrar">
</form>
```

```css
/* CSS */
/* Estiliza apenas os campos de entrada do tipo "text" */
input[type="text"] {
  border: 1px solid #ccc;
  padding: 8px;
  border-radius: 4px;
}

/* Estiliza apenas os botões de envio */
input[type="submit"] {
  background-color: #28a745;
  color: white;
  border: none;
  cursor: pointer;
  padding: 10px 15px;
}
```

## Seletor de Valor em Lista `[attr~="valor"]`

Representado pelo til (`~`), este seletor é projetado para atributos que contêm uma **lista de valores separados por espaços** (como o atributo `class`). Ele seleciona elementos se o `valor` especificado for uma das palavras exatas nessa lista.

**Sintaxe:**

```css
[nome-do-atributo~="palavra"] {
  /* declarações */
}
```

**Exemplo Prático:** Imagine que você está usando um atributo `data-*` para atribuir tags a postagens de um blog.

```html
<!-- HTML -->
<article data-tags="css frontend web">Artigo sobre CSS.</article>
<article data-tags="html web semantica">Artigo sobre HTML.</article>
<article data-tags="javascript frontend logica">Artigo sobre JavaScript.</article>
```

```css
/* CSS */
/* Seleciona todos os artigos que possuem a tag "frontend" em sua lista */
article[data-tags~="frontend"] {
  border-left: 4px solid purple;
  padding-left: 15px;
}
```

No exemplo, o primeiro e o terceiro artigos serão selecionados, pois ambos contêm a palavra exata "frontend" em sua lista de `data-tags`.

## Seletor de Prefixo `[attr^="valor"]`

O seletor de prefixo, que utiliza o acento circunflexo (`^`), seleciona elementos cujo valor do atributo **começa com** a string especificada.

**Sintaxe:**

```css
[nome-do-atributo^="inicio-do-valor"] {
  /* declarações */
}
```

**Exemplo Prático:** A aplicação mais comum é diferenciar links internos de links externos ou seguros.

```html
<!-- HTML -->
<a href="/contato.html">Contato (Interno)</a>
<a href="[https://wikipedia.org](https://wikipedia.org)">Wikipedia (Externo e Seguro)</a>
<a href="[http://siteantigo.com](http://siteantigo.com)">Site Antigo (Externo e Inseguro)</a>
```

```css
/* CSS */
/* Seleciona todos os links cujo href começa com "https://" */
a[href^="https://"] {
  color: green;
  font-weight: bold;
}
```

Apenas o link da Wikipedia será selecionado, pois seu `href` começa com `https://`.

## Seletor de Sufixo `[attr$="valor"]`

Oposto ao seletor de prefixo, o seletor de sufixo, que utiliza o cifrão (`$`), seleciona elementos cujo valor do atributo **termina com** a string especificada.

**Sintaxe:**

```css
[nome-do-atributo$="fim-do-valor"] {
  /* declarações */
}
```

**Exemplo Prático:** É perfeito para aplicar ícones ou estilos a links que apontam para tipos de arquivos específicos.

```html
<!-- HTML -->
<p>Baixe nosso manual de instruções e o relatório anual.</p>
<ul>
  <li><a href="/docs/manual.pdf">Manual do Produto (PDF)</a></li>
  <li><a href="/docs/relatorio.docx">Relatório Anual (Word)</a></li>
  <li><a href="/imagens/logo.jpg">Visualizar Logo (JPG)</a></li>
</ul>
```

```css
/* CSS */
/* Adiciona um ícone de PDF a todos os links que terminam com .pdf */
a[href$=".pdf"] {
  background: url(icone-pdf.png) no-repeat left center;
  padding-left: 20px;
}

/* Adiciona um ícone de imagem a todos os links que terminam com .jpg */
a[href$=".jpg"] {
  background: url(icone-imagem.png) no-repeat left center;
  padding-left: 20px;
}
```

## Seletor de Substring `[attr*="valor"]`

O seletor de substring, representado pelo asterisco (`*`), é o mais flexível dos seletores de correspondência de valor. Ele seleciona um elemento se o valor do seu atributo **contém** a string especificada em qualquer posição.

**Sintaxe:**

```css
[nome-do-atributo*="substring"] {
  /* declarações */
}
```

**Exemplo Prático:** Suponha que você queira destacar todos os elementos de uma galeria de imagens que são de uma localização específica, como "paris".

```html
<!-- HTML -->
<img src="/imagens/torre-eiffel-paris-dia.jpg" alt="Torre Eiffel">
<img src="/imagens/notre-dame-paris.jpg" alt="Notre Dame">
<img src="/imagens/coliseu-roma.jpg" alt="Coliseu">
```

```css
/* CSS */
/* Seleciona todas as imagens cujo src contém a palavra "paris" */
img[src*="paris"] {
  border: 5px solid gold;
  box-shadow: 0 0 10px rgba(0,0,0,0.5);
}
```

As duas primeiras imagens serão selecionadas, pois "paris" aparece em seus atributos `src`.

## Seletor de Subcódigo Hifenizado `[attr|="valor"]`

Este seletor, que utiliza a barra vertical (`|`), é um pouco mais específico. Ele foi projetado principalmente para lidar com códigos de idioma. Ele seleciona elementos cujo atributo tem um valor que é exatamente igual ao `valor` especificado, ou que **começa com o `valor` seguido imediatamente por um hífen (`-`)**.

**Sintaxe:**

```css
[nome-do-atributo|="valor"] {
  /* declarações */
}
```

**Exemplo Prático:** Estilizar elementos com base em seu idioma principal, englobando as variações regionais.

```html
<!-- HTML -->
<p lang="pt-br">Este parágrafo está em português do Brasil.</p>
<p lang="pt-pt">Este parágrafo está em português de Portugal.</p>
<p lang="pt">Este parágrafo está em português genérico.</p>
<p lang="es">Este parágrafo está em espanhol.</p>
```

```css
/* CSS */
/* Seleciona elementos com lang="pt" ou lang="pt-qualquercoisa" */
p[lang|="pt"] {
  font-family: 'Times New Roman', serif;
}
```

Os três primeiros parágrafos serão selecionados porque seus atributos `lang` ou são exatamente "pt" ou começam com "pt-". O parágrafo em espanhol não será afetado.

## Modificador Case-Insensitive (Não Sensível a Maiúsculas/Minúsculas)

Por padrão, todos os seletores de atributo que verificam o **valor** são sensíveis a maiúsculas e minúsculas (`case-sensitive`). No entanto, podemos adicionar um modificador `i` (ou `I`) antes do colchete de fechamento `]` para tornar a correspondência insensível (`case-insensitive`).

**Sintaxe:**

```css
[nome-do-atributo="valor" i] {
  /* declarações */
}
```

**Exemplo Prático:** Imagine que você tem links para arquivos de imagem, mas não tem controle se a extensão do arquivo será `.JPG`, `.jpg`, ou `.Jpg`.

```html
<!-- HTML -->
<a href="foto1.JPG">Foto 1</a>
<a href="foto2.jpg">Foto 2</a>
<a href="foto3.Jpg">Foto 3</a>
<a href="documento.pdf">Não é uma imagem</a>
```

```css
/* CSS */
/* Seleciona todos os links que terminam com .jpg, ignorando o case */
a[href$=".jpg" i] {
  color: green;
  border: 2px solid green;
}
```

Neste caso, os três primeiros links serão selecionados, independentemente da capitalização da extensão `.jpg`, tornando a regra muito mais robusta.

## Considerações Finais

Neste capítulo, mergulhamos no mundo dos **seletores de atributo**, descobrindo um método de seleção incrivelmente versátil que nos liberta da necessidade de adicionar classes ou IDs para cada variação de estilo. Vimos como podemos selecionar elementos com base na simples presença de um atributo, por correspondência exata de valor, ou através de poderosas comparações de substrings, como prefixos, sufixos e conteúdo parcial.

Essas ferramentas são essenciais no desenvolvimento web moderno. Elas nos permitem escrever um CSS mais "inteligente", que reage aos metadados e às propriedades inerentes do nosso HTML, resultando em um código mais limpo, semântico e fácil de manter. A capacidade de, por exemplo, estilizar links para arquivos `.pdf` ou campos de formulário `required` diretamente, sem classes adicionais, é um testemunho da sua eficiência.

Com os seletores básicos, de combinação e de atributo em nosso arsenal, já possuímos um controle vasto sobre a estilização. No entanto, ainda há uma dimensão a ser explorada: o estado e as partes virtuais dos elementos. Nos próximo capítulos, vamos investigar as **pseudo-classes** e os **pseudo-elementos**, que nos permitirão estilizar elementos com base em interações do usuário (como `:hover`), estados de formulário (como `:checked`), posições na hierarquia (como `:first-child`) e até mesmo partes de um elemento que não existem no HTML (como `::before`).
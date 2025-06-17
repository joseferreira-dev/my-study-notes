# Capítulo 6 – Pseudo-elementos: Estilizando o Inexistente

Ao longo dos últimos capítulos, montamos um arsenal completo de seletores. Aprendemos a mirar em elementos por tipo, classe, ID, atributos, hierarquia e estado. Com as pseudo-classes, demos um passo além, estilizando elementos com base em interações e condições dinâmicas. Agora, vamos explorar a última e talvez mais mágica fronteira dos seletores: os **pseudo-elementos**.

Enquanto as pseudo-classes selecionam um elemento inteiro em um estado especial (como `:hover`), os pseudo-elementos se comportam como se tivessem adicionado uma nova tag HTML ao redor de uma **parte específica** de um elemento. Eles nos permitem estilizar fragmentos que não têm um elemento próprio na árvore do documento, como a primeira letra de um parágrafo, o marcador de um item de lista, ou até mesmo gerar conteúdo puramente visual que não existe no HTML.

Neste capítulo, vamos mergulhar no poder dos pseudo-elementos. A principal diferença sintática que você notará é o uso de **dois-pontos duplos (`::`)**. Embora navegadores modernos ainda suportem a sintaxe antiga de dois-pontos simples (`:`) por razões de compatibilidade, a convenção atual é usar `::` para diferenciar claramente os pseudo-elementos das pseudo-classes. Vamos desvendar os onipresentes `::before` e `::after`, que nos permitem "injetar" conteúdo decorativo, e explorar outros que nos dão controle fino sobre a tipografia e a interface do usuário. Dominar os pseudo-elementos é o que nos permite criar designs sofisticados, ricos em detalhes e eficientes, muitas vezes sem adicionar uma única `div` extra ao nosso código.

## A Chave de Tudo: A Propriedade `content`

Antes de explorarmos os pseudo-elementos mais comuns, `::before` e `::after`, precisamos entender a propriedade que lhes dá poder: `content`. Esta propriedade CSS é usada exclusivamente com `::before` e `::after` para gerar conteúdo. Se você não definir a propriedade `content` para eles, eles simplesmente não serão renderizados.

O valor de `content` pode ser:

- **Uma string de texto:** `content: "Leia mais: ";`
- **Uma string vazia:** `content: "";` (essencial para criar formas e elementos decorativos).
- **A URL de uma imagem:** `content: url('caminho/para/imagem.png');`
- **O valor de um atributo:** `content: attr(data-texto);`
- **Contadores e aspas:** `content: open-quote;` ou `content: counter(contador);`

## Os "Trabalhadores" do CSS: `::before` e `::after`

Estes são os dois pseudo-elementos mais versáteis e amplamente utilizados. Eles criam um elemento "virtual" que se torna o primeiro (`::before`) ou o último (`::after`) filho do elemento selecionado. É importante notar: eles são inseridos **dentro** do elemento, antes ou depois do seu conteúdo principal.

### `::before`

Cria um pseudo-elemento como o primeiro filho do elemento selecionado.

**Exemplo Prático: Adicionando ícones a links** Vamos adicionar um ícone de telefone a um link de contato sem usar uma tag `<img>` ou `<i>`.

```html
<a href="tel:+5581999998888" class="link-telefone">Ligue para nós</a>
```

```css
.link-telefone::before {
  content: "📞 "; /* Adiciona o emoji de telefone e um espaço */
  margin-right: 5px;
}
```

O resultado será: 📞 Ligue para nós. O emoji foi inserido via CSS.

### `::after`

Cria um pseudo-elemento como o último filho do elemento selecionado.

**Exemplo Prático: Criando "tooltips" (dicas de contexto)** Podemos usar um atributo `data-*` para guardar o texto da dica e o `::after` para exibi-lo.

```html
<span class="dica" data-tooltip="Esta é uma informação importante!">Passe o mouse aqui</span>
```

```css
.dica {
  position: relative; /* Necessário para posicionar o tooltip */
  border-bottom: 1px dotted #333;
}

.dica:hover::after {
  content: attr(data-tooltip); /* Pega o texto do atributo data-tooltip */
  position: absolute;
  bottom: 125%; /* Posiciona acima do elemento */
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 14px;
  white-space: nowrap; /* Impede que o texto quebre linha */
}
```

Ao passar o mouse sobre o `<span>`, uma pequena caixa preta com o texto da dica aparecerá acima dele.

## Pseudo-elementos Tipográficos

Estes pseudo-elementos nos dão controle sobre partes específicas do texto de um elemento.

### `::first-letter`

Seleciona a primeira letra do conteúdo de texto de um elemento de bloco. É perfeito para criar letras capitulares (drop caps).

```html
<p class="capitular">Era uma vez, em uma terra muito distante...</p>
```

```css
.capitular::first-letter {
  font-size: 3em; /* 3 vezes o tamanho da fonte normal */
  font-weight: bold;
  color: #8B0000;
  float: left; /* Faz o texto fluir ao redor da letra */
  margin-right: 8px;
  line-height: 1;
}
```

### `::first-line`

Seleciona a primeira linha de texto formatada de um elemento de bloco. O interessante é que a quantidade de texto selecionada é dinâmica: se você redimensionar a janela e mais ou menos palavras couberem na primeira linha, o estilo se ajustará automaticamente.

```html
<p class="intro">Este parágrafo serve como introdução para o nosso artigo. A primeira linha terá um estilo especial para chamar a atenção do leitor e convidá-lo a continuar a leitura do conteúdo que preparamos com muito cuidado e atenção aos detalhes.</p>
```

```css
.intro::first-line {
  font-variant: small-caps; /* Transforma as letras em versalete */
  color: #555;
}
```

## Pseudo-elementos de UI e Interação

Este grupo nos permite estilizar partes da interface do navegador ou do sistema operacional que são relacionadas a um elemento.

### `::selection`

Aplica estilos à parte do documento que foi destacada (selecionada) pelo usuário ao arrastar o mouse. As propriedades que podem ser aplicadas são limitadas (`color`, `background-color`, `text-shadow`, `cursor`).

```css
/* Muda a cor da seleção padrão do navegador */
::selection {
  background-color: #FFD700; /* Dourado */
  color: #333;
}
```

### `::placeholder`

Seleciona e estiliza o texto de placeholder de um elemento de formulário (`<input>` ou `<textarea>`).

```html
<input type="email" placeholder="ex: seu.email@provedor.com">
```

```css
input::placeholder {
  color: #a9a9a9;
  font-style: italic;
}
```

### `::marker`

Seleciona o marcador de um item de lista (`<li>`). Isso pode ser o ponto (bullet), o número ou até mesmo uma imagem.

```html
<ul>
  <li>Primeiro item</li>
  <li>Segundo item</li>
</ul>
```

```css
li::marker {
  color: #007bff;
  font-size: 1.2em;
  content: "✓ "; /* Substitui o marcador padrão por um checkmark */
}
```

### `::backdrop`

Aplica-se à área de "fundo" que fica atrás de um elemento quando ele é colocado em modo de tela cheia usando a API de Fullscreen.

```css
/* Quando um vídeo estiver em tela cheia, o fundo será escuro */
video::backdrop {
  background-color: rgba(0, 0, 0, 0.9);
}
```

## Pseudo-elementos de Validação (Experimentais)

Estes pseudo-elementos permitem estilizar os indicadores visuais que os navegadores usam para erros de ortografia e gramática. O suporte e a capacidade de estilização podem variar bastante entre os navegadores.

### `::spelling-error`

Estiliza o texto que o navegador marcou como contendo um erro de ortografia.

```css
::spelling-error {
  text-decoration: underline wavy red; /* Muda a decoração do erro ortográfico */
}
```

### `::grammar-error`

Estiliza o texto que o navegador marcou como contendo um erro gramatical.

```css
::grammar-error {
  text-decoration: underline wavy green;
}
```

## Exemplos Mais Complexos de Uso

A verdadeira magia dos pseudo-elementos, principalmente `::before` e `::after`, é sua capacidade de serem estilizados como qualquer outro elemento de bloco. Podemos dar-lhes tamanho, cor, posição e até mesmo animações.

### Exemplo 1: Citações Estilizadas

Vamos usar `::before` e `::after` para adicionar aspas grandes e decorativas a um `<blockquote>`.

```html
<blockquote>
  A criatividade é a inteligência se divertindo.
</blockquote>
```

```css
blockquote {
  font-style: italic;
  font-size: 1.5em;
  color: #555;
  position: relative; /* Contexto de posicionamento para as aspas */
  padding: 0 40px;
  text-align: center;
}

blockquote::before,
blockquote::after {
  content: "”"; /* Aspa dupla de fechamento */
  font-family: 'Georgia', serif;
  font-size: 4em;
  color: #e0e0e0;
  position: absolute;
}

blockquote::before {
  content: "“"; /* Aspa dupla de abertura */
  top: 0;
  left: 0;
}

blockquote::after {
  bottom: -20px;
  right: 0;
}
```

### Exemplo 2: Sobreposição (Overlay) em Imagens

Podemos criar uma sobreposição de cor em uma imagem ao passar o mouse, sem adicionar elementos HTML extras.

```html
<a href="#" class="container-imagem">
  <img src="https://placehold.co/400x300/a9a9a9/ffffff?text=Imagem" alt="Imagem de Exemplo">
</a>
```

```css
.container-imagem {
  position: relative;
  display: inline-block;
}

.container-imagem::after {
  content: ''; /* Essencial para que o elemento seja gerado */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 123, 255, 0.5); /* Azul semitransparente */
  opacity: 0; /* Começa invisível */
  transition: opacity 0.3s ease; /* Transição suave */
}

.container-imagem:hover::after {
  opacity: 1; /* Fica visível no hover */
}
```

## Considerações Finais

Neste capítulo, concluímos nossa exploração aprofundada dos seletores CSS ao desvendar os **pseudo-elementos**. Vimos que, enquanto as pseudo-classes estilizam um elemento em um estado particular, os pseudo-elementos nos dão o poder de estilizar uma _parte_ específica de um elemento ou até mesmo de gerar conteúdo visual que não existe no nosso HTML.

Os `::before` e `::after`, em conjunto com a propriedade `content`, provaram ser ferramentas incrivelmente versáteis, permitindo-nos criar desde simples ícones e rótulos até elementos decorativos complexos como citações e sobreposições, mantendo nosso HTML limpo e semântico. Exploramos também como `::first-letter` e `::first-line` nos oferecem um controle tipográfico refinado, e como `::selection`, `::placeholder` e `::marker` nos ajudam a aprimorar a experiência do usuário na interface.

Com o domínio sobre seletores básicos, de combinação, de atributo, pseudo-classes e agora pseudo-elementos, você possui o conhecimento completo e necessário para mirar com precisão cirúrgica em qualquer parte do seu documento que desejar estilizar. O "o quê" e o "onde" da estilização estão agora sob seu controle. A partir do próximo capítulo, mudaremos nosso foco para o "como", mergulhando no **Box Model**, o conceito fundamental que define como as dimensões, o preenchimento, as bordas e as margens dos elementos são calculados e interagem entre si.
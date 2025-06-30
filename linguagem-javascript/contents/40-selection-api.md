# Capítulo 40 – Selection API: Interagindo com a Seleção do Usuário

Em nossa jornada pelo JavaScript interativo, já dominamos a arte de encontrar e manipular elementos do DOM e de reagir aos eventos do usuário, como cliques e pressionamentos de tecla. Agora, vamos explorar uma camada mais sutil, porém extremamente poderosa, da interação do usuário: a **seleção de texto**. Seja arrastando o mouse sobre um parágrafo, dando um clique duplo em uma palavra ou usando atalhos de teclado, a seleção de conteúdo é uma das ações mais fundamentais que um usuário realiza ao consumir informação em uma página web. E se nosso código pudesse saber exatamente o que o usuário selecionou e agir com base nessa informação?

É aqui que entra a **Selection API**. Esta API nativa do navegador nos dá acesso programático à seleção de texto do usuário. Ela nos permite não apenas **ler** o conteúdo que foi destacado, mas também **modificar** essa seleção, selecionar texto programaticamente e até mesmo alterar o próprio DOM com base na área selecionada. Esta é a API que alimenta funcionalidades como:

- **Editores de Rich Text (WYSIWYG):** Quando você clica no botão "Negrito", é a Selection API que permite ao editor encontrar o texto selecionado e envolvê-lo em tags `<strong>`.
- **Ferramentas de Citação:** Funcionalidades como "Tweetar esta citação" ou "Salvar este trecho" usam a API para capturar o texto exato que o usuário destacou.
- **Aplicações de Estudo e Anotação:** Ferramentas que permitem aos usuários destacar (highlight) ou adicionar comentários a partes de um texto dependem inteiramente desta API.

Neste capítulo, faremos um mergulho profundo nesta interface. Começaremos por entender os dois conceitos centrais que a compõem: o objeto `Selection`, que representa a seleção em si, e o objeto `Range`, que descreve uma porção contígua do documento. Aprenderemos a obter o texto selecionado, a limpar a seleção e, o mais importante, a criar e manipular seleções de forma programática. Exploraremos também como "ouvir" as mudanças na seleção, permitindo que nossa interface reaja em tempo real às ações do usuário.

## O Básico: `window.getSelection()`

O ponto de entrada para toda a API é o método `window.getSelection()`. Este método retorna um objeto `Selection`, que representa a(s) área(s) de texto selecionada(s) pelo usuário.

```js
const selecao = window.getSelection();
console.log(selecao);
```

O objeto `Selection` retornado é dinâmico. Se o usuário alterar a seleção na página, as propriedades deste mesmo objeto serão atualizadas automaticamente.

### Obtendo o Texto da Seleção

A tarefa mais comum é simplesmente obter o texto que o usuário selecionou. A maneira mais fácil de fazer isso é chamando o método `toString()` no objeto de seleção.

```js
document.addEventListener('mouseup', () => {
  // A cada vez que o usuário solta o botão do mouse, verificamos a seleção.
  const textoSelecionado = window.getSelection().toString();
  if (textoSelecionado.length > 0) {
    console.log(`Texto selecionado: "${textoSelecionado}"`);
  }
});
```

`toString()` retorna o texto puro da seleção, removendo qualquer marcação HTML. Se você precisar do HTML, precisará trabalhar com o objeto `Range`, como veremos a seguir.

## Os Pilares da API: `Selection` e `Range`

Para entender e manipular seleções, é crucial conhecer os dois objetos principais:

-   **`Selection`:** Representa a seleção do usuário. É a "visão" do usuário. Uma seleção pode, teoricamente, conter múltiplas áreas descontínuas (por exemplo, se o usuário segurar a tecla `Ctrl`/`Cmd` e selecionar várias partes do texto, embora nem todos os navegadores suportem isso facilmente). Por essa razão, um objeto `Selection` gerencia uma coleção de objetos `Range`.
-   **`Range`:** Representa uma porção contígua de um documento. Um `Range` é definido por dois pontos de limite: um ponto de início e um ponto de fim. Cada ponto é definido por um nó do DOM e um "offset" (deslocamento) dentro desse nó. É o `Range` que nos dá o poder de descrever e manipular programaticamente uma área do documento.

Na prática, a grande maioria das seleções do usuário consiste em apenas um único `Range`. Podemos acessá-lo com o método `selection.getRangeAt(0)`.

### Propriedades Importantes do Objeto `Selection`

O objeto `Selection` nos dá informações detalhadas sobre onde a seleção começa e termina.

-   `anchorNode`: O nó do DOM onde a seleção do usuário **começou**.
-   `anchorOffset`: O deslocamento (índice de caractere ou de nó filho) dentro do `anchorNode` onde a seleção começou.
-   `focusNode`: O nó do DOM onde a seleção do usuário **terminou**.
-   `focusOffset`: O deslocamento dentro do `focusNode`.
-   `isCollapsed`: Um booleano que é `true` se a seleção for apenas um ponto (um cursor), sem nenhum conteúdo selecionado.
-   `rangeCount`: O número de `Range`s na seleção (geralmente 0 ou 1).

A beleza do "anchor" e "focus" é que eles respeitam a direção da seleção. Se o usuário selecionar da direita para a esquerda, o `focusNode` virá antes do `anchorNode` no documento.

## Manipulando a Seleção Programaticamente

Não podemos criar um objeto `Selection` diretamente. Em vez disso, o processo para definir uma nova seleção é:
1.  Obter o objeto de seleção global.
2.  Remover todas as seleções (ranges) existentes.
3.  Criar e configurar um novo objeto `Range`.
4.  Adicionar este novo `Range` ao objeto de seleção.

### Selecionando o Conteúdo de um Elemento

Vamos criar um exemplo prático onde um clique de botão seleciona todo o conteúdo de um parágrafo.

**HTML:**

```html
<p id="paragrafo-selecionavel">Este é um parágrafo de exemplo. Clique no botão abaixo para selecionar todo este texto.</p>
<button id="btn-selecionar">Selecionar Parágrafo</button>
````

**JavaScript:**

```js
const botao = document.getElementById('btn-selecionar');
const paragrafo = document.getElementById('paragrafo-selecionavel');

botao.addEventListener('click', () => {
  // 1. Obter o objeto de seleção
  const selecao = window.getSelection();
  
  // 2. Criar um objeto Range
  const range = document.createRange();
  
  // 3. Configurar o Range para conter todo o conteúdo do parágrafo
  // selectNodeContents seleciona os filhos do nó.
  // Para incluir o próprio <p>, usaríamos range.selectNode(paragrafo).
  range.selectNodeContents(paragrafo);
  
  // 4. Limpar quaisquer seleções existentes
  selecao.removeAllRanges();
  
  // 5. Adicionar nosso novo Range à seleção
  selecao.addRange(range);
});
```

### Limpando a Seleção (Deselecionar Tudo)

Existem duas maneiras principais de remover qualquer seleção existente na página.

**1. `removeAllRanges()`** Este método remove todos os ranges do objeto de seleção, efetivamente limpando-a.

```js
// HTML: <button id="btn-limpar">Limpar Seleção</button>
const btnLimpar = document.getElementById('btn-limpar');
btnLimpar.addEventListener('click', () => {
  window.getSelection().removeAllRanges();
});
```

**2. `collapse()`** Este método "colapsa" a seleção para um único ponto.

- `selection.collapse(node, offset)`: Colapsa a seleção para um ponto específico no DOM.
- `selection.collapseToStart()`: Colapsa a seleção para o seu ponto inicial (`anchorNode`/`Offset`).
- `selection.collapseToEnd()`: Colapsa a seleção para o seu ponto final (`focusNode`/`Offset`).

Chamar `collapse()` com `null` como primeiro argumento também funciona para limpar a seleção na maioria dos navegadores modernos e é, por vezes, considerado mais idiomático.

```js
// Alternativa para limpar a seleção
window.getSelection().collapse(document.body, 0); // Colapsa para o início do body
```

## Tópicos Avançados e Casos de Uso

### O Evento `selectionchange`

Para criar interfaces verdadeiramente reativas, podemos "ouvir" as mudanças na seleção. O evento `selectionchange` é disparado no `document` toda vez que o usuário modifica a seleção de texto.

**Exemplo: Exibindo um Pop-up de "Tweetar"**

```js
// Suponha que temos um pop-up escondido no HTML
const popupTweet = document.getElementById('popup-tweet');

document.addEventListener('selectionchange', () => {
  const selecao = window.getSelection();
  const texto = selecao.toString().trim();

  if (texto.length > 10) { // Se o usuário selecionou um texto razoável
    // Obter o retângulo delimitador do range para posicionar o pop-up
    const range = selecao.getRangeAt(0);
    const rect = range.getBoundingClientRect();
    
    popupTweet.style.top = `${window.scrollY + rect.top - popupTweet.offsetHeight}px`;
    popupTweet.style.left = `${window.scrollX + rect.left + (rect.width / 2) - (popupTweet.offsetWidth / 2)}px`;
    popupTweet.style.display = 'block';
  } else {
    popupTweet.style.display = 'none';
  }
});
```

Este padrão permite criar interfaces contextuais que aparecem exatamente onde e quando o usuário precisa delas.

### Modificando o DOM com a Seleção

A API nos permite não apenas ler, mas também alterar o documento com base na seleção. O objeto `Range` possui métodos como:

- `range.deleteContents()`: Remove o conteúdo do range do DOM.
- `range.extractContents()`: Remove o conteúdo do range e o retorna como um `DocumentFragment`.
- `range.cloneContents()`: Cria uma cópia do conteúdo do range.
- `range.insertNode(newNode)`: Insere um novo nó no início do range.
- `range.surroundContents(newNode)`: Envolve o conteúdo do range com um novo nó. Este é o método usado por editores de texto para, por exemplo, aplicar negrito.

**Exemplo: Aplicando Negrito**

```js
const btnNegrito = document.getElementById('btn-negrito');

btnNegrito.addEventListener('click', () => {
  const selecao = window.getSelection();
  if (!selecao.isCollapsed) {
    const range = selecao.getRangeAt(0);
    const boldElement = document.createElement('strong');
    
    // Envolve o conteúdo selecionado com a tag <strong>
    try {
      range.surroundContents(boldElement);
    } catch(e) {
      // surroundContents pode falhar se a seleção cruzar limites de blocos.
      // Um editor real teria uma lógica mais complexa aqui.
      console.error("Não foi possível aplicar o negrito nesta seleção:", e);
    }
  }
});
```

## Considerações Finais

Neste capítulo, desvendamos a **Selection API**, a interface que nos dá controle programático sobre uma das interações mais fundamentais do usuário com o conteúdo: a seleção de texto. Vimos que a API é construída sobre os conceitos de `Selection` (a visão do usuário) e `Range` (a descrição programática de uma porção do documento).

Aprendemos a obter o texto selecionado com `selection.toString()`, a limpar seleções e, o mais importante, a criar e modificar seleções programaticamente, o que nos permite guiar o usuário ou realizar ações em seu nome. Exploramos também o evento `selectionchange`, que nos permite construir UIs reativas, e vislumbramos como os métodos do objeto `Range` são a base para a construção de editores de rich text complexos.

A Selection API pode parecer um tópico de nicho, mas ela é a espinha dorsal de qualquer aplicação que envolva a manipulação textual avançada. Com o conhecimento adquirido aqui, você está agora capacitado a criar experiências de edição, anotação e citação que antes pareciam ser exclusivas de grandes aplicações comerciais.
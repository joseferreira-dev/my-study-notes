# Capítulo 20 – Listas e Contadores

Desde os primórdios da web, as listas (`<ul>`, `<ol>`) têm sido um dos pilares para a organização de conteúdo. Elas nos permitem agrupar itens relacionados de forma semântica, seja uma sequência de passos em um tutorial, os ingredientes de uma receita ou os links de um menu de navegação. Por padrão, os navegadores aplicam um estilo simples a essas listas — pontos (bullets) para listas não ordenadas e números para listas ordenadas. Mas e se quiséssemos ir além? E se precisássemos de marcadores customizados, numeração em algarismos romanos ou até mesmo um sistema de numeração de capítulos e seções que se atualiza automaticamente?

O CSS nos oferece um conjunto de ferramentas robusto para controlar totalmente a aparência e o comportamento das listas. Ele nos dá o poder de transformar uma simples lista com marcadores em um componente de design elegante e funcional.

Neste capítulo, vamos explorar as duas principais abordagens para estilizar listas. Primeiro, mergulharemos nas propriedades tradicionais da família **`list-style`**, que nos permitem alterar o tipo, a posição e até mesmo a imagem do marcador de um item. Em seguida, desvendaremos uma das funcionalidades mais poderosas e flexíveis do CSS: os **Contadores (CSS Counters)**. Veremos como os contadores nos libertam das limitações das listas ordenadas, permitindo-nos criar sistemas de numeração complexos e aninhados para qualquer elemento em nossa página, não apenas em tags `<li>`.

## Estilizando Listas Tradicionais

As propriedades `list-style` são aplicadas aos elementos de lista (`<li>`) e controlam a aparência do marcador (o "bullet" ou o número).

### `list-style-type`

Esta propriedade define o tipo de marcador a ser usado. O CSS oferece uma vasta gama de valores predefinidos.

**Valores comuns para `<ul>` (listas não ordenadas):**

- `disc`: Um círculo preenchido (padrão).
- `circle`: Um círculo vazio.
- `square`: Um quadrado preenchido.

**Valores comuns para `<ol>` (listas ordenadas):**

- `decimal`: Números (1, 2, 3, ...) (padrão).
- `decimal-leading-zero`: Números com um zero à esquerda (01, 02, ...).
- `lower-roman`: Algarismos romanos em minúsculo (i, ii, iii, ...).
- `upper-roman`: Algarismos romanos em maiúsculo (I, II, III, ...).
- `lower-alpha` / `lower-latin`: Letras em minúsculo (a, b, c, ...).
- `upper-alpha` / `upper-latin`: Letras em maiúsculo (A, B, C, ...).

**Exemplo:**

```html
<ul class="lista-circulo">
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
<ol class="lista-romana">
  <li>Primeiro passo</li>
  <li>Segundo passo</li>
</ol>
```

```css
.lista-circulo {
  list-style-type: circle;
}
.lista-romana {
  list-style-type: upper-roman;
}
```

### `list-style-position`

Esta propriedade controla se o marcador da lista aparece dentro ou fora da caixa de conteúdo do item.

- `outside` (padrão): O marcador fica à esquerda do texto, fora do fluxo principal do conteúdo. Se o texto quebrar em várias linhas, as linhas seguintes não ficarão alinhadas com o marcador.
- `inside`: O marcador é tratado como se fosse parte do conteúdo do item da lista. A primeira linha é recuada para dar espaço ao marcador, e as linhas seguintes ficam alinhadas com ele.

**Exemplo Visual:**

```html
<p>Position: outside (padrão)</p>
<ul class="pos-outside">
  <li>Item de lista com texto suficientemente longo para quebrar em múltiplas linhas e demonstrar o alinhamento.</li>
</ul>

<p>Position: inside</p>
<ul class="pos-inside">
  <li>Item de lista com texto suficientemente longo para quebrar em múltiplas linhas e demonstrar o alinhamento.</li>
</ul>
```

```css
ul {
  border: 1px solid steelblue;
  width: 300px;
}
.pos-outside { list-style-position: outside; }
.pos-inside { list-style-position: inside; }
```

No exemplo `inside`, você notará que todo o bloco de texto é deslocado para a direita, e o marcador age como se estivesse na primeira linha do texto.

### `list-style-image`

Permite que você use uma imagem customizada como marcador da lista.

**Sintaxe:** `list-style-image: url('caminho/para/imagem.png');`

**Exemplo:**

```
<ul class="lista-customizada">
  <li>Item com ícone</li>
  <li>Outro item</li>
</ul>
```

```css
.lista-customizada {
  list-style-image: url('imagens/seta-pequena.svg');
}
```

**Limitações:** O controle sobre o tamanho e a posição da imagem do marcador é muito limitado com esta propriedade. Para um controle mais preciso, é melhor remover o marcador padrão (`list-style: none;`) e usar um pseudo-elemento `::before` com uma imagem de fundo, como veremos mais adiante.

### `list-style` (Shorthand)

Esta propriedade de atalho permite definir os três valores (`type`, `position`, `image`) em uma única declaração.

**Exemplo:**

```css
ul {
  /* Define o tipo como quadrado, a posição como interna e uma imagem de fallback se a primeira não carregar */
  list-style: square inside url('marcador.png');
}

/* É mais comumente usada para remover completamente o estilo da lista */
nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
```

## O Poder dos Contadores CSS (CSS Counters)

Quando `list-style-type` não é suficiente, os contadores CSS oferecem um sistema de numeração incrivelmente poderoso e flexível. Com eles, você pode:

- Numerar qualquer elemento, não apenas `<li>`.
- Criar sistemas de numeração aninhados complexos (ex: 1.1, 1.1.2).
- Customizar totalmente a aparência do número.

O sistema funciona com três propriedades principais:

1. **`counter-reset`**: Usada em um elemento pai para **iniciar ou resetar** um ou mais contadores.
2. **`counter-increment`**: Usada nos elementos que você deseja contar, para **incrementar** o contador.
3. **`content`** com **`counter()`**: Usada em um pseudo-elemento (`::before`) para **exibir** o valor do contador.

### Numerando Itens com um Contador Simples

Vamos recriar uma lista ordenada, mas com um estilo totalmente customizado.

**Exemplo:**

```html
<ol class="lista-contadora">
  <li>Capítulo Um</li>
  <li>Capítulo Dois</li>
  <li>Capítulo Três</li>
</ol>
```

```css
.lista-contadora {
  list-style: none; /* 1. Removemos o marcador padrão da <ol> */
  counter-reset: capitulo-counter; /* 2. Iniciamos um contador chamado 'capitulo-counter' */
}

.lista-contadora li {
  counter-increment: capitulo-counter; /* 3. A cada 'li', incrementamos nosso contador */
  margin-bottom: 10px;
}

.lista-contadora li::before {
  /* 4. Usamos um pseudo-elemento para exibir o contador */
  content: "Capítulo " counter(capitulo-counter, upper-roman) ": "; /* Ex: "Capítulo I: " */
  font-weight: bold;
  color: navy;
}
```

A função `counter()` pode receber um segundo argumento opcional para formatar o número, aceitando os mesmos valores de `list-style-type` (como `upper-roman`, `lower-alpha`, etc.).

### Implementando Numeração Multinível

A verdadeira força dos contadores está em sua capacidade de lidar com aninhamento. A função `counters()` é usada para isso.

**Exemplo (Estrutura de Tópicos):**

```html
<ol class="topicos">
  <li>Introdução</li>
  <li>
    Desenvolvimento
    <ol>
      <li>Ponto Principal</li>
      <li>
        Análise
        <ol>
          <li>Sub-ponto A</li>
          <li>Sub-ponto B</li>
        </ol>
      </li>
    </ol>
  </li>
  <li>Conclusão</li>
</ol>
```

```css
.topicos {
  list-style-type: none;
  counter-reset: secao-counter; /* Inicia o contador principal */
  padding-left: 0;
}

.topicos li {
  list-style-type: none;
  counter-increment: secao-counter; /* Cada item incrementa o contador */
  margin-bottom: 5px;
}

.topicos li::before {
  /* counters(nome, separador) concatena os valores de todos os contadores 'secao-counter'
     aninhados, separados por um ponto. */
  content: counters(secao-counter, ".") ". "; /* Ex: "1. ", "1.1. ", "1.1.1. " */
  font-weight: bold;
  margin-right: 5px;
}

/* Precisamos resetar o contador para cada nova lista aninhada */
.topicos ol {
  counter-reset: secao-counter;
  padding-left: 20px; /* Recuo para a lista interna */
  margin-top: 5px;
}
```

Este código criará automaticamente uma numeração hierárquica (1., 2., 2.1., 2.2., 2.2.1., 2.2.2., 3.) que se ajusta dinamicamente se você adicionar ou remover itens em qualquer nível.

## Boas Práticas com Listas e Contadores

Aplicar estilos a listas e contadores de forma eficaz pode melhorar muito a clareza e a estética do seu conteúdo. Seguir algumas diretrizes ajuda a garantir que seu código seja limpo, semântico e acessível.

- **Comece com um Reset:** Listas padrão vêm com `padding` e `margin` definidos pelo navegador. Para um controle total, especialmente em menus de navegação, é uma boa prática resetá-los: `list-style: none; padding: 0; margin: 0;`.
- **Use `::marker` para Estilos Simples:** Para navegadores modernos, a pseudo-classe `::marker` é a maneira mais direta e performática de estilizar o marcador de uma lista (`color`, `font-size`, etc.), sem a complexidade de um pseudo-elemento `::before`.
- **Prefira Contadores para Numeração Complexa:** Se você precisa de algo além de "1, 2, 3" ou "a, b, c", especialmente com prefixos, sufixos ou aninhamento, os Contadores CSS são a ferramenta superior e mais flexível.
- **Mantenha a Semântica:** Mesmo que você use contadores para numerar `divs` ou `h2`, lembre-se de usar as tags `<ol>` e `<ul>` para conteúdo que é, semanticamente, uma lista. Isso é crucial para a acessibilidade e SEO.
- **Acessibilidade dos Contadores:** O conteúdo gerado pelos contadores via pseudo-elementos (`::before`, `::after`) é, na maioria dos casos, lido por leitores de tela modernos, mas é sempre bom testar. Garanta que o conteúdo gerado (como "Capítulo 1:") forneça um contexto útil e não apenas decoração.

## Considerações Finais

Neste capítulo, desvendamos as ferramentas do CSS para estilizar um dos componentes mais fundamentais da estruturação de conteúdo: as listas. Vimos como as propriedades `list-style` nos dão um controle rápido e fácil sobre os marcadores padrão, e como os **Contadores CSS** elevam nosso poder a um novo patamar, permitindo a criação de sistemas de numeração customizados, aninhados e automáticos para qualquer elemento.

Dominar estas técnicas significa que você pode apresentar informações sequenciais e agrupadas de forma clara, esteticamente agradável e semanticamente correta. A capacidade de ir além dos simples pontos e números e criar uma numeração que se alinha perfeitamente com a sua hierarquia de conteúdo é uma habilidade valiosa no arsenal de qualquer desenvolvedor front-end.
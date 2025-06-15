# Capítulo 2 – Trabalhando com Texto: Da Estrutura à Ênfase

Com a estrutura fundamental de um documento HTML já compreendida, podemos agora nos concentrar no elemento mais onipresente da web: o texto. Praticamente todo site, de um simples blog a um complexo portal de notícias, depende de conteúdo textual para comunicar sua mensagem. No entanto, texto puro, sem formatação ou hierarquia, seria uma massa ilegível e desinteressante. O HTML nos oferece um rico arsenal de tags projetadas especificamente para dar forma, estrutura e, mais importante, significado semântico ao nosso conteúdo escrito.

Neste capítulo, mergulharemos no universo dos elementos textuais. Começaremos com os blocos de construção mais essenciais: os **cabeçalhos**, que criam a hierarquia lógica do documento, e os **parágrafos**, que organizam as ideias em blocos de leitura. Em seguida, exploraremos as nuances da **formatação de texto**, diferenciando tags que oferecem apenas um estilo visual daquelas que carregam um forte peso semântico. Abordaremos também a forma correta de marcar citações, referências e outros conteúdos específicos. Finalmente, aprenderemos a lidar com caracteres especiais através das entidades HTML, a usar comentários para documentar nosso código e teremos uma breve introdução ao uso de estilos diretamente no HTML.

Ao final, você estará apto a transformar um texto simples em um documento bem-estruturado, acessível e semanticamente rico, utilizando a tag correta para cada propósito.

## Cabeçalhos: A Hierarquia do Conteúdo (`<h1>`–`<h6>`)

Os cabeçalhos são, talvez, os elementos mais importantes para a estruturação de uma página. Eles funcionam como títulos e subtítulos de um livro ou documento, criando uma hierarquia clara que guia o leitor (e os mecanismos de busca) através do conteúdo. O HTML fornece seis níveis de cabeçalho, do `<h1>` (o mais importante) ao `<h6>` (o menos importante).

- **`<h1>`**: Deve ser usado para o título principal da página. Idealmente, deve haver apenas um `<h1>` por página, pois ele representa o tópico central do documento.
- **`<h2>` a `<h6>`**: Usados para subtítulos, criando seções e subseções. A lógica deve ser respeitada: um `<h3>` deve ser um subtítulo de uma seção iniciada por um `<h2>`, e não o contrário.

É crucial entender que a escolha de um cabeçalho deve ser baseada na **importância hierárquica**, e não no tamanho do texto que ele produz. A aparência visual pode (e deve) ser controlada com CSS.

**Exemplo de uso correto:**

```html
<main>
    <h1>O Guia Completo sobre Café</h1>
    <p>Introdução sobre a história do café.</p>
    
    <h2>Tipos de Grãos</h2>
    <p>Existem dois principais tipos de grãos de café...</p>
    
        <h3>Arábica</h3>
        <p>O grão Arábica é conhecido por sua suavidade...</p>
    
        <h3>Robusta</h3>
        <p>O grão Robusta possui um sabor mais intenso...</p>
    
    <h2>Métodos de Preparo</h2>
    <p>Descrição dos diferentes métodos de preparo...</p>
</main>
```

## Parágrafos e Quebras: Organizando o Fluxo de Texto

Enquanto os cabeçalhos estruturam o documento em seções, os parágrafos e elementos de quebra organizam o fluxo do texto dentro dessas seções.

- **`<p>` (Parágrafo)**: É o elemento padrão para agrupar texto. Cada elemento `<p>` representa um bloco de texto, e os navegadores automaticamente adicionam um espaço vertical entre eles, facilitando a leitura.
- **`<br>` (Quebra de Linha)**: É um elemento vazio usado para forçar uma quebra de linha **dentro** de um mesmo bloco de texto. Deve ser usado com moderação, apenas quando a quebra de linha for parte intrínseca do conteúdo, como em poemas ou endereços. Não deve ser usado para criar espaço entre parágrafos (para isso, usa-se CSS).
- **`<hr>` (Linha Horizontal)**: Este elemento representa uma **quebra temática** entre parágrafos ou seções. Ele sinaliza uma mudança de tópico e é visualizado como uma linha horizontal. Também é um elemento vazio.
- **`<pre>` (Texto Pré-formatado)**: O elemento `<pre>` é especial. Ele exibe o texto exatamente como foi escrito no código-fonte, preservando todos os espaços em branco, tabulações e quebras de linha. É extremamente útil para exibir exemplos de código ou arte ASCII.

**Exemplo de uso:**

```html
<p>Este é o primeiro parágrafo do nosso texto, falando sobre um assunto geral.</p>

<hr>

<p>Aqui, mudamos de assunto. Note o uso da linha horizontal acima.</p>

<p>
    Para: João da Silva<br>
    Rua das Flores, 123<br>
    Recife, PE
</p>

<pre>
  for (int i = 0; i < 5; i++) {
    printf("Número: %d\n", i);
  }
</pre>
```

## Formatação: Ênfase, Importância e Estilo

O HTML possui diversas tags para formatar trechos de texto. É fundamental distinguir entre tags puramente estilísticas e tags semânticas, que adicionam significado ao texto.

- **`<b>` vs. `<strong>`**: Ambas deixam o texto em **negrito**.
    - `<strong>`: Use quando o texto tiver grande **importância, seriedade ou urgência**. É uma tag semântica.
    - `<b>` (Bold): Use para chamar a atenção do leitor sem adicionar importância semântica, como em nomes de produtos, palavras-chave ou a primeira frase de um artigo.
- **`<i>` vs. `<em>`**: Ambas deixam o texto em _itálico_.
    - `<em>` (Emphasis): Use para dar **ênfase** a uma palavra ou frase, alterando o sentido da sentença. É uma tag semântica.
    - `<i>` (Idiomatic): Use para texto que se diferencia do restante, como um termo técnico, uma frase em outro idioma, um pensamento ou o nome de um navio.
- **`<s>` e `<del>`**:
    - `<s>` (Strikethrough): Indica que uma informação **não é mais precisa ou relevante**. É semanticamente diferente de `<del>`, que marca um texto removido de um documento. Pense no `<s>` para preços antigos ou promoções encerradas.
    - `<del>`: Marca um texto que foi **removido** de uma versão anterior do documento.
- **`<u>`**: Representa uma **anotação não textual**. Seu uso mais comum é para marcar erros de ortografia em um texto. **Importante:** Evite usar `<u>` para sublinhar texto por motivos estilísticos, pois os usuários podem confundir o texto com um hiperlink.
- **`<code>`**: Marca um trecho de texto como um **fragmento de código** de computador. Os navegadores geralmente o exibem em uma fonte monoespaçada. Para blocos de código maiores, use `<code>` dentro de um `<pre>`.
- **`<mark>`**: Destaca um texto, como se tivesse sido marcado com um marca-texto. Útil para referenciar um trecho específico.
- **`<small>`**: Para textos secundários, como direitos autorais, avisos legais ou "letras miúdas".
- **`<ins>`**: Marca um texto que foi **inserido** em um documento.
- **`<sub>` e `<sup>`**:
    - `<sub>`: Formata o texto como **subscrito** (ex: H₂O).
    - `<sup>`: Formata o texto como **sobrescrito** (ex: x²).

**Exemplo:**

```html
<p><strong>Aviso:</strong> O portão fecha às 18h.</p>
<p>Você <em>deve</em> entregar o relatório até sexta-feira.</p>
<p>A palavra <i>HTML</i> é uma sigla.</p>
<p>O evento ocorrerá em <s>Setembro</s> Outubro.</p>
<p>Meu nome está escrito de forma errada: <u class="erro-ortografico">Hoão</u>.</p>
<p>Para exibir uma mensagem, use a função <code>alert()</code>.</p>
<p>Preço: <del>R$ 99,90</del> <ins>R$ 79,90</ins>.</p>
```

## Citação e Referências: Dando Crédito e Contexto

Marcar corretamente citações e referências é essencial para a semântica e acessibilidade.

- **`<blockquote>`**: Para citações longas, que constituem um parágrafo inteiro ou mais. Os navegadores costumam exibi-la com um recuo (indentação). O atributo `cite` pode ser usado para incluir a URL da fonte.
- **`<q>`**: Para citações curtas, que aparecem no meio de um parágrafo. Os navegadores geralmente adicionam aspas automaticamente.
- **`<cite>`**: Usada para indicar o título de uma obra, como um livro, filme, poema ou canção.
- **`<abbr>`**: Para abreviações e acrônimos. O atributo `title` deve ser usado para fornecer a expansão completa.

**Exemplo:**

```html
<p>Como disse Steve Jobs, <q>A única maneira de fazer um ótimo trabalho é amar o que você faz.</q></p>

<blockquote cite="https://developer.mozilla.org/pt-BR/docs/Web/HTML">
  <p>HTML (Linguagem de Marcação de HiperTexto) é o bloco de construção mais básico da web.</p>
</blockquote>

<p>Meu livro favorito é <cite>O Guia do Mochileiro das Galáxias</cite>.</p>

<p>A <abbr title="Organização das Nações Unidas">ONU</abbr> foi fundada em 1945.</p>
```

## Entidades de Caracteres Especiais: Exibindo Símbolos Reservados

E se quisermos exibir o caractere `<` como texto em nossa página? Se simplesmente o digitarmos, o navegador tentará interpretá-lo como o início de uma tag, quebrando o layout. Para resolver isso, usamos as **entidades de caracteres**. São códigos especiais que representam caracteres reservados ou símbolos que não estão disponíveis no teclado.

Uma entidade sempre começa com `&` e termina com `;`.

|Símbolo|Entidade|Descrição|
|---|---|---|
|`<`|`&lt;`|Less Than (Menor que)|
|`>`|`&gt;`|Greater Than (Maior que)|
|`&`|`&amp;`|Ampersand (E comercial)|
|`"`|`&quot;`|Aspas duplas|
|`'`|`&apos;`|Aspas simples (apóstrofo)|
|`©`|`&copy;`|Copyright|
|`®`|`&reg;`|Marca Registrada|
|(espaço)|`&nbsp;`|Non-Breaking Space (Espaço não separável)|

O `&nbsp;` é particularmente útil para garantir que duas palavras permaneçam juntas na mesma linha (ex: `R$&nbsp;100`) ou para criar múltiplos espaços, já que o navegador ignora espaços em branco extras no código.

**Exemplo de uso:**

```html
<p>Para criar uma tag de parágrafo, use o código &lt;p&gt;.</p>
<p>A empresa M&amp;M &copy; tem seus direitos reservados.</p>
```

## Comentários em HTML

Comentários são trechos de texto no código que **não são exibidos** pelo navegador. Eles são úteis para deixar notas para outros desenvolvedores (ou para você mesmo no futuro), explicar blocos de código complexos ou desativar temporariamente uma parte do código sem excluí-la.

A sintaxe de um comentário começa com `<!--` e termina com `-->`.

```html
<p>Este parágrafo será exibido normalmente.</p>
<!-- Isto é um comentário que não será exibido --> 
```

## Uma Breve Palavra Sobre Estilos (Atributo `style`)

É possível aplicar estilos de CSS diretamente em um elemento HTML usando o atributo global `style`. Isso é chamado de **CSS inline**.

```html
<p style="color: blue; font-size: 20px;">Este parágrafo será azul e com fonte maior.</p>
```

**Importante:** Embora funcional, o uso do atributo `style` é **geralmente desaconselhado** como prática principal. Ele mistura a camada de apresentação (CSS) com a camada de estrutura (HTML), o que dificulta a manutenção do site. A forma recomendada é manter o CSS em arquivos separados. O CSS inline deve ser reservado para casos muito específicos, como testes rápidos ou estilos gerados dinamicamente por JavaScript.

## Considerações Finais

Neste capítulo, exploramos o coração do conteúdo web: o texto. Aprendemos que o HTML nos fornece um vocabulário vasto e semântico para não apenas exibir texto, mas para dar a ele uma estrutura hierárquica e um significado claro. Vimos a diferença crucial entre tags de ênfase semântica, como `<strong>` e `<em>`, e suas contrapartes mais visuais. Aprendemos a organizar o fluxo de leitura com parágrafos e quebras, a marcar código com `<code>`, a dar crédito a outras obras com elementos de citação e a lidar com caracteres reservados usando entidades.

Dominar o uso correto desses elementos textuais é um passo fundamental para se tornar um bom desenvolvedor web. Um documento bem estruturado é mais fácil de ler, manter, estilizar com CSS e é mais bem compreendido por mecanismos de busca e tecnologias de acessibilidade.

Com o texto devidamente estruturado, podemos agora passar a organizar dados e informações de maneiras mais complexas. No próximo capítulo, exploraremos como criar **listas**, uma ferramenta essencial para agrupar itens relacionados.
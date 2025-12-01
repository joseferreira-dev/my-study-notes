# Capítulo 6 – Listas

Até agora, aprendemos a estruturar o conteúdo em blocos de texto, a apresentar dados em tabelas e a incorporar mídias como imagens. No entanto, frequentemente nos deparamos com a necessidade de agrupar itens relacionados de uma forma mais simples que uma tabela. Pode ser uma lista de ingredientes para uma receita, uma sequência de passos em um tutorial, ou os tópicos de um menu de navegação. Para esses cenários, o HTML oferece um conjunto poderoso e flexível de elementos de **lista**.

Neste capítulo, vamos explorar os três tipos de listas disponíveis em HTML. Começaremos com as **listas não ordenadas**, ideais para agrupar itens onde a sequência não é importante. Em seguida, veremos as **listas ordenadas**, essenciais quando a ordem dos itens é crucial. Por fim, abordaremos as **listas de definição**, um tipo especializado para apresentar pares de termo e descrição, como em um glossário. Uma das características mais poderosas das listas é a capacidade de aninhá-las, e demonstraremos como essa técnica é a base para a criação de estruturas complexas, como menus de navegação com subníveis.

Compreender o uso correto de cada tipo de lista é fundamental para adicionar clareza e estrutura semântica aos seus documentos, melhorando a legibilidade tanto para humanos quanto para máquinas.

## Listas Não Ordenadas

Uma lista não ordenada é usada quando os itens pertencem a um mesmo grupo, mas a ordem em que aparecem **não tem relevância**. Pense em uma lista de compras ou nas características de um produto. O elemento que cria essa lista é o `<ul>` (Unordered List), e cada item dentro dela é definido pelo elemento `<li>` (List Item).

**Exemplo:**

```html
<p>Ingredientes para o bolo:</p>
<ul>
  <li>Farinha</li>
  <li>Ovos</li>
  <li>Leite</li>
  <li>Açúcar</li>
</ul>
```

Por padrão, os navegadores exibem os itens de uma lista não ordenada com um marcador em formato de disco (•).

### Estilizando os Marcadores

Embora a estilização seja um trabalho para o CSS, é importante conhecer a propriedade `list-style-type`, que permite alterar a aparência do marcador. Os valores mais comuns para listas não ordenadas são:

- `disc`: Um círculo preenchido (padrão).
- `circle`: Um círculo vazio.
- `square`: Um quadrado preenchido.
- `none`: Remove completamente o marcador.

**Exemplo de uso com estilo inline (para demonstração):**

```html
<ul style="list-style-type: square;">
  <li>Item A</li>
  <li>Item B</li>
</ul>
```

## Listas Ordenadas

Uma lista ordenada é usada quando a **sequência dos itens é importante**. Exemplos clássicos são os passos de um tutorial, um ranking de classificação ou as cláusulas de um contrato. O elemento que a define é o `<ol>` (Ordered List), e, assim como na lista não ordenada, cada item é definido pela tag `<li>`.

**Exemplo:**

```html
<p>Como fazer café:</p>
<ol>
  <li>Ferva a água.</li>
  <li>Coloque o pó no coador.</li>
  <li>Despeje a água quente lentamente.</li>
  <li>Adoce a gosto.</li>
</ol>
```

Por padrão, os navegadores numeram os itens sequencialmente, começando com "1.".

### Personalizando a Ordem com `type` e `start`

As listas ordenadas oferecem atributos para personalizar o tipo de marcador e o ponto de partida da contagem.

- **Atributo `type`**: Altera o tipo de numeração.
    - `1`: Números (padrão)
    - `A`: Letras maiúsculas (A, B, C...)
    - `a`: Letras minúsculas (a, b, c...)
    - `I`: Algarismos romanos maiúsculos (I, II, III...)
    - `i`: Algarismos romanos minúsculos (i, ii, iii...)
- **Atributo `start`**: Define o número inicial da lista. O valor deve ser um número, mesmo que o `type` seja de letras ou algarismos romanos.

**Exemplo de personalização:**

```html
<ol type="A" start="3">
  <li>Primeiro item (exibido como C)</li>
  <li>Segundo item (exibido como D)</li>
</ol>
```

## Listas de Definição

Este é um tipo de lista mais especializado, projetado para apresentar grupos de termos e suas respectivas descrições, como em um glossário ou um dicionário de metadados. A estrutura envolve três elementos:

- **`<dl>` (Definition List)**: O elemento contêiner que envolve toda a lista.
- **`<dt>` (Definition Term)**: O termo que será definido.
- **`<dd>` (Definition Description)**: A descrição ou definição do termo.

Um único termo (`<dt>`) pode ter múltiplas descrições (`<dd>`), e múltiplos termos podem compartilhar a mesma descrição.

**Exemplo de um glossário:**

```html
<dl>
  <dt>HTML</dt>
  <dd>Linguagem de Marcação de Hipertexto. Usada para estruturar o conteúdo da web.</dd>
  <dt>CSS</dt>
  <dd>Folhas de Estilo em Cascata. Usada para estilizar a aparência do conteúdo HTML.</dd>
</dl>
```

## Listas Aninhadas

A verdadeira flexibilidade das listas se revela quando as **aninhamos**, ou seja, colocamos uma lista dentro de outra. Essa técnica é fundamental para criar hierarquias de informações, como sumários de livros ou, mais comumente, menus de navegação de sites.

A regra mais importante para aninhar listas é: **uma nova lista (`<ul>` ou `<ol>`) deve ser colocada dentro de um item de lista (`<li>`) da lista principal**, nunca diretamente como filha da lista principal.

**Exemplo de Menu de Navegação:**

Este é o padrão semântico para criar um menu de navegação com submenus. A interatividade (mostrar/esconder o submenu) é controlada por CSS ou JavaScript, mas a estrutura HTML é a base de tudo.

```html
<nav>
  <ul>
    <li><a href="/">Página Inicial</a></li>
    <li>
      <a href="/servicos/">Serviços</a>
      <ul>
        <li><a href="/servicos/consultoria.html">Consultoria</a></li>
        <li><a href="/servicos/desenvolvimento.html">Desenvolvimento Web</a></li>
        <li><a href="/servicos/design.html">Design Gráfico</a></li>
      </ul>
      </li>
    <li><a href="/contato.html">Contato</a></li>
  </ul>
</nav>
```

Navegadores aplicam um recuo e, por padrão, alteram o estilo do marcador para cada nível de aninhamento em listas não ordenadas, deixando a hierarquia visualmente clara.

## Considerações Finais

Neste capítulo, exploramos as três formas fundamentais de agrupar informações em listas no HTML. Vimos que as **listas não ordenadas (`<ul>`)** são perfeitas para itens sem uma sequência específica, enquanto as **listas ordenadas (`<ol>`)** são essenciais quando a ordem importa. Também conhecemos as **listas de definição (`<dl>`)**, uma ferramenta semântica poderosa para glossários e pares de chave-valor.

A habilidade de aninhar listas, como demonstramos na criação de um menu de navegação, é uma técnica crucial que ressalta a importância de uma estrutura HTML correta e semântica. Usar o tipo de lista apropriado para cada situação não apenas organiza o conteúdo de forma lógica para o usuário, mas também enriquece o documento com um significado que é vital para a acessibilidade e para os mecanismos de busca.

Agora que dominamos as principais formas de exibir informações — textos, imagens, tabelas e listas — estamos prontos para o próximo passo fundamental na criação de sites interativos: aprender a **coletar dados do usuário**. No próximo capítulo, mergulharemos no mundo dos **formulários**.
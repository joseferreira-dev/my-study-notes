# Capítulo 24 – Estilizando Tabelas de Dados

As tabelas HTML (`<table>`) são um dos elementos mais antigos e fundamentais da web. Seu propósito principal é claro e imutável: exibir **dados tabulares** — informações que fazem mais sentido quando organizadas em uma grade de linhas e colunas. Relatórios financeiros, listas de preços, calendários e comparações de produtos são exemplos perfeitos de dados tabulares.

Embora no passado as tabelas tenham sido indevidamente usadas como uma ferramenta para o layout geral de páginas — uma prática hoje considerada obsoleta e prejudicial à semântica e acessibilidade —, seu papel na apresentação de dados continua sendo insubstituível. Por padrão, os navegadores renderizam tabelas com uma aparência básica e funcional, mas o CSS nos oferece um conjunto de propriedades específicas para transformar essas grades de dados em apresentações claras, legíveis e visualmente integradas ao nosso design.

Neste capítulo, vamos mergulhar fundo no universo da estilização de tabelas. Iremos além das propriedades que já conhecemos (como `padding` e `background-color` em células) para explorar as ferramentas que o CSS nos oferece especificamente para o layout de tabelas. Aprenderemos a controlar como as bordas são renderizadas com `border-collapse`, a gerenciar o espaçamento com `border-spacing`, a otimizar a performance com `table-layout` e a posicionar legendas com `caption-side`.

## O Modelo de Bordas: `border-collapse`

A propriedade mais fundamental e transformadora para estilizar tabelas é a `border-collapse`. Ela define qual dos dois modelos de borda o navegador deve usar.

- **`separate` (padrão):** Neste modelo, cada célula (`<td>`, `<th>`) tem suas próprias bordas independentes. Existe um espaço entre as bordas de células adjacentes, que pode ser controlado pela propriedade `border-spacing`.
- **`collapse`:** Neste modelo, as bordas de células adjacentes são "fundidas" (colapsadas) em uma única borda. Este é o modelo mais comum para designs modernos, pois cria uma aparência de grade mais limpa e compacta.

**Exemplo Visual:**

```
<p>border-collapse: separate; (padrão)</p>
<table class="tabela-separada">
  <tr> <td>A1</td> <td>A2</td> </tr>
  <tr> <td>B1</td> <td>B2</td> </tr>
</table>

<p>border-collapse: collapse;</p>
<table class="tabela-colapsada">
  <tr> <td>A1</td> <td>A2</td> </tr>
  <tr> <td>B1</td> <td>B2</td> </tr>
</table>
```

```css
table {
  width: 200px;
  margin-bottom: 20px;
}
td {
  border: 2px solid steelblue;
  padding: 10px;
}

.tabela-separada {
  border-collapse: separate;
}
.tabela-colapsada {
  border-collapse: collapse;
}
```

**Resultado:** A primeira tabela terá bordas "grossas" e duplas onde as células se tocam, enquanto a segunda terá linhas finas e únicas, muito mais parecida com uma planilha.

## Espaçamento com `border-spacing`

Esta propriedade **só tem efeito quando `border-collapse: separate;`**. Ela define a distância entre as bordas de células adjacentes.

- **Um valor:** `border-spacing: 10px;` (aplica 10px de espaçamento tanto horizontal quanto vertical).
- **Dois valores:** `border-spacing: 5px 15px;` (aplica 5px horizontalmente e 15px verticalmente).

```css
.tabela-separada {
  border-collapse: separate;
  border-spacing: 10px 5px;
}
```

## Células Vazias com `empty-cells`

Esta propriedade também **só se aplica ao modelo `separate`**. Ela controla se as bordas e o fundo devem ser renderizados para células (`<td>`) que não têm nenhum conteúdo.

- `show` (padrão): A célula vazia é exibida normalmente, com suas bordas e fundo.
- `hide`: A célula vazia se torna invisível, sem bordas nem fundo.

**Exemplo:**

```html
<table class="tabela-vazia">
  <caption>empty-cells: hide;</caption>
  <tr> <td>Dado 1</td> <td></td> </tr>
  <tr> <td>Dado 3</td> <td>Dado 4</td> </tr>
</table>
```

```css
.tabela-vazia {
  border-collapse: separate;
  border-spacing: 5px;
  empty-cells: hide;
}
.tabela-vazia td {
  border: 1px solid #ccc;
  padding: 10px;
}
```

**Resultado:** A célula na primeira linha, segunda coluna, simplesmente desaparecerá.

## Otimização de Layout com `table-layout`

Esta propriedade define o algoritmo que o navegador usa para calcular a largura das colunas da tabela. A escolha do algoritmo tem um grande impacto na performance, especialmente em tabelas grandes.

- **`auto` (padrão):** O navegador precisa ler todo o conteúdo de uma coluna para determinar a largura ideal para ela. A largura da coluna se adapta ao conteúdo mais largo. Isso pode ser **lento** em tabelas com muitas linhas, pois o navegador só pode renderizar a tabela depois de analisar todo o seu conteúdo.
- **`fixed`:** Este algoritmo é muito mais **rápido e previsível**. A largura da tabela e de suas colunas é determinada pela largura da própria tabela (`width`), pela largura da primeira linha de células, ou dividindo o espaço igualmente. O conteúdo que não couber na célula não influenciará a largura da coluna; ele será cortado ou transbordará, dependendo da propriedade `overflow`.

**Exemplo Prático:**

```html
<p>table-layout: auto; (A coluna se ajusta à palavra longa)</p>
<table class="layout-auto">
  <tr>
    <td>Curto</td>
    <td>UmaPalavraMuitoMuitoMuitoLongaSemEspacos</td>
  </tr>
</table>

<p>table-layout: fixed; (A coluna ignora a palavra longa)</p>
<table class="layout-fixed">
  <tr>
    <td>Curto</td>
    <td>UmaPalavraMuitoMuitoMuitoLongaSemEspacos</td>
  </tr>
</table>
```

```css
table {
  width: 400px;
  border-collapse: collapse;
}
td {
  border: 1px solid black;
  overflow: hidden; /* Corta o texto que não cabe */
}
.layout-auto { table-layout: auto; }
.layout-fixed { table-layout: fixed; }
```

**Resultado:** A primeira tabela terá uma segunda coluna muito mais larga para tentar acomodar a palavra longa. A segunda tabela terá duas colunas de larguras iguais (200px cada), e a palavra longa será simplesmente cortada.

## Posicionando a Legenda com `caption-side`

A tag `<caption>` fornece um título ou uma descrição para a tabela. A propriedade `caption-side` controla se essa legenda aparece acima ou abaixo da tabela.

- `top` (padrão): A legenda é exibida acima da tabela.
- `bottom`: A legenda é exibida abaixo da tabela.

**Exemplo:**

```css
.tabela-com-legenda {
  /* Move a legenda para a parte inferior da tabela */
  caption-side: bottom;
}
```

## Boas Práticas com Tabelas

Estilizar tabelas de forma eficaz é crucial para a usabilidade e a apresentação de dados. Manter algumas boas práticas em mente garante que suas tabelas não sejam apenas bonitas, mas também claras e acessíveis.

- **Use `<table>` para Dados Tabulares, Não para Layout:** Este é o princípio mais importante. Tabelas são para dados. Usar tabelas para posicionar os principais elementos de uma página é uma prática legada que prejudica a semântica, a acessibilidade e a responsividade. Use CSS Grid ou Flexbox para layout.
- **`border-collapse: collapse;` é o seu Padrão:** Para 99% das tabelas de dados, o modelo de borda colapsada é mais limpo, moderno e fácil de gerenciar. Adote-o como seu ponto de partida.
- **Estrutura Semântica é Essencial:** Use sempre `<thead>`, `<tbody>` e `<tfoot>` para estruturar sua tabela. Essas tags não apenas melhoram a acessibilidade (leitores de tela podem anunciar cabeçalhos de coluna), mas também fornecem ganchos de estilo perfeitos. Por exemplo, você pode aplicar um `background-color` e `font-weight: bold;` a todas as células dentro do `<thead>`.
- **O Espaçamento é Rei:** Células de tabela sem `padding` são claustrofóbicas e difíceis de ler. Sempre adicione um `padding` consistente a todos os `<th>` e `<td>` para dar um respiro ao seu conteúdo.
- **Use `text-align` para Clareza:** Alinhe o texto de forma lógica. Geralmente, textos em cabeçalhos e células de dados de texto ficam melhores alinhados à esquerda (`text-align: left;`). Dados numéricos geralmente são mais fáceis de comparar quando alinhados à direita (`text-align: right;`).
- **Implemente "Zebra Striping":** Em tabelas com muitas linhas, aplicar uma cor de fundo sutil a linhas alternadas (usando a pseudo-classe `:nth-child(even)` ou `:nth-child(odd)`) melhora drasticamente a legibilidade, ajudando o olho do usuário a rastrear uma linha de dados da esquerda para a direita sem se perder.
-  **`table-layout: fixed` para Performance:** Para tabelas grandes e com muitas linhas, o uso de `table-layout: fixed;` pode resultar em uma melhoria significativa no tempo de renderização da página.

## Considerações Finais

Neste capítulo, exploramos as propriedades CSS que nos dão controle total sobre a apresentação de dados tabulares. Vimos como a escolha entre os modelos de borda `separate` e `collapse` é a decisão fundamental que dita o comportamento de outras propriedades, e como `border-collapse: collapse;` é geralmente a escolha preferida para designs modernos.

Aprendemos a otimizar a performance de grandes tabelas com `table-layout: fixed` e a refinar a aparência com `border-spacing` e `caption-side`. A estilização de tabelas não se trata apenas de estética; trata-se de transformar dados brutos em informações claras, organizadas e fáceis de consumir. Um bom design de tabela é uma marca de uma interface de usuário cuidadosa e profissional.
# Capítulo 5 – Tabelas

Depois de aprendermos a estruturar textos, conectar páginas com links e dar vida visual com imagens, chegamos a um desafio comum no mundo da informação: como apresentar dados que possuem uma relação entre si de forma clara e organizada? Pense em relatórios financeiros, calendários, especificações de produtos ou grades de horários. Para esses cenários, uma simples lista ou parágrafo não é suficiente. É aqui que as **tabelas** HTML entram em cena.

Neste capítulo, vamos explorar em profundidade o conjunto de elementos que o HTML nos oferece para a criação de tabelas. Começaremos com a estrutura fundamental (`<table>`, `<tr>`, `<th>`, `<td>`), construindo uma base sólida. Em seguida, adicionaremos camadas de semântica com `<thead>`, `<tbody>` e `<tfoot>`, e aprenderemos a criar layouts mais complexos mesclando células com `colspan` e `rowspan`. Também veremos como agrupar e estilizar colunas e como adicionar um título descritivo com `<caption>`. Por fim, abordaremos brevemente como as bordas são estilizadas, preparando o terreno para o CSS.

É importante ressaltar uma prática fundamental desde o início: **tabelas devem ser usadas para exibir dados tabulares, e não para criar o layout de uma página**. O uso de tabelas para layout foi uma técnica comum no passado, mas hoje é considerada ultrapassada, prejudicial à acessibilidade e à manutenção.

## Estrutura Fundamental de uma Tabela

A construção de uma tabela em HTML envolve a combinação de alguns elementos essenciais que trabalham em conjunto para formar uma grade de linhas e colunas.

- **`<table>`**: O elemento contêiner que envolve toda a tabela.
- **`<tr>` (Table Row)**: Define uma **linha** da tabela.
- **`<th>` (Table Header)**: Define uma célula de **cabeçalho**. O conteúdo de um `<th>` serve como título para uma coluna ou para uma linha. Os navegadores geralmente o estilizam em negrito e centralizado. É crucial para a semântica e acessibilidade.
- **`<td>` (Table Data)**: Define uma célula de **dados** padrão da tabela.

Vamos ver como eles se combinam em uma tabela simples que lista nomes e idades:

```html
<table>
  <tr>
    <th>Nome</th>
    <th>Idade</th>
  </tr>
  <tr>
    <td>Maria</td>
    <td>30</td>
  </tr>
  <tr>
    <td>João</td>
    <td>25</td>
  </tr>
</table>
```

Neste exemplo, criamos uma tabela com três linhas (`<tr>`). A primeira linha contém as células de cabeçalho (`<th>`) e as duas seguintes contêm as células de dados (`<td>`).

## Agrupamento Semântico

Para tabelas mais longas e complexas, é uma excelente prática agrupar as linhas em seções semânticas. Isso torna o código mais legível e acessível, além de permitir estilização mais avançada com CSS.

- **`<thead>`**: Agrupa o conteúdo do cabeçalho da tabela.
- **`<tbody>`**: Agrupa o conteúdo principal, o corpo da tabela.
- **`<tfoot>`**: Agrupa o conteúdo do rodapé da tabela, como linhas de totais ou resumos.

Esses elementos não alteram a estrutura visual padrão da tabela, mas adicionam um contexto semântico valioso.

**Exemplo com agrupamento semântico:**

```html
<table>
  <caption>Despesas Mensais</caption>
  
  <thead>
    <tr>
      <th>Item</th>
      <th>Valor</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Aluguel</td>
      <td>R$ 1.200,00</td>
    </tr>
    <tr>
      <td>Supermercado</td>
      <td>R$ 650,00</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td>Total</td>
      <td>R$ 1.850,00</td>
    </tr>
  </tfoot>
</table>
```

## Mesclando Células

Para criar tabelas com layouts mais complexos, muitas vezes precisamos que uma única célula ocupe o espaço de várias colunas ou linhas. Para isso, usamos os atributos `colspan` e `rowspan`.

- **`colspan` (Column Span)**: Permite que uma célula (`<td>` ou `<th>`) se expanda horizontalmente, ocupando o espaço de **duas ou mais colunas**.
- **`rowspan` (Row Span)**: Permite que uma célula se expanda verticalmente, ocupando o espaço de **duas ou mais linhas**.

**Exemplo de uso:**

```html
<table border="1"> <caption>Horário Semanal</caption>
  <thead>
    <tr>
      <th></th>
      <th>Segunda</th>
      <th>Terça</th>
      <th>Quarta</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Manhã</th>
      <td>HTML</td>
      <td rowspan="2">CSS Avançado</td>
      <td>JavaScript</td>
    </tr>
    <tr>
      <th>Tarde</th>
      <td>Lógica</td>
      <td>Banco de Dados</td>
    </tr>
    <tr>
      <th colspan="4">Noite: Turmas em Formação</th>
    </tr>
  </tbody>
</table>
```

**Nota**: O atributo `border="1"` é uma forma antiga de adicionar bordas e não é recomendado em projetos modernos. O ideal é usar CSS para estilização.

## Adicionando um Título com `<caption>`

O elemento `<caption>` fornece um título ou legenda para a tabela. Ele é fundamental para a acessibilidade, pois os leitores de tela o utilizam para fornecer contexto ao usuário antes de ler os dados da tabela. Por regra, o `<caption>` deve ser o **primeiro elemento filho** dentro da tag `<table>`.

## Estilizando Colunas

Às vezes, queremos aplicar um estilo (como uma cor de fundo ou uma largura específica) a uma coluna inteira. Fazer isso em cada célula (`<td>`) seria repetitivo. As tags `<colgroup>` e `<col>` resolvem isso.

- `<colgroup>`: É um contêiner para uma ou mais tags `<col>`.
- `<col>`: É um elemento vazio usado para especificar propriedades de estilo para uma ou mais colunas. O atributo `span` pode ser usado para que uma tag `<col>` se aplique a múltiplas colunas.

Essas tags devem ser colocadas logo após o `<caption>` e antes do `<thead>`.

**Exemplo de uso:**

```html
<table>
  <caption>Relatório de Vendas Trimestral</caption>
  
  <colgroup>
    <col span="1" style="background-color: #f2f2f2;">
    <col span="1" style="background-color: #c2c2c2;">
    <col span="2" style="background-color: #299292;">
  </colgroup>
  
  <thead>
    <tr>
      <th>Produto</th>
      <th>Jan</th>
      <th>Fev</th>
      <th>Mar</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Produto A</td>
      <td>150</td>
      <td>200</td>
      <td>220</td>
    </tr>
  </tbody>
</table>
```

## Estilizando Bordas

Como mencionado, o atributo `border` do HTML é obsoleto. A maneira moderna e correta de estilizar bordas é usando a propriedade `border` do CSS. Embora o CSS seja um tópico para outro momento, é útil saber como o estilo de borda funciona.

Em CSS, você pode definir a espessura, o estilo e a cor da borda. Existem vários estilos de linha disponíveis:

- **solid**: Uma linha sólida e contínua.
- **dotted**: Uma linha pontilhada.
- **dashed**: Uma linha tracejada.
- **double**: Uma linha dupla.
- **groove**: Uma borda com efeito de entalhe 3D.
- **ridge**: O oposto de `groove`, uma borda com efeito de crista 3D.
- **inset**: Faz o conteúdo parecer afundado.
- **outset**: Faz o conteúdo parecer elevado.
- **none / hidden**: Remove a borda.

**Exemplo de estilo com CSS:**

```html
<table style="border: 1px solid black; border-collapse: collapse;">
  <tr>
    <th>Nome</th>
    <th>Idade</th>
  </tr>
  <tr>
    <td>Maria</td>
    <td>30</td>
  </tr>
</table>
```

## Considerações Finais

Neste capítulo, desvendamos a anatomia das tabelas em HTML. Aprendemos a construir a estrutura básica com `<table>`, `<tr>`, `<th>` e `<td>`, e a enriquecê-la semanticamente com `<thead>`, `<tbody>` e `<tfoot>`. Vimos como criar layouts de dados complexos ao mesclar células com `colspan` e `rowspan`, e como adicionar contexto com o `<caption>`.

A mensagem central é que tabelas são uma ferramenta poderosa e específica para a **apresentação de dados tabulares**. Compreender sua estrutura e semântica é essencial para exibir informações complexas de forma clara, organizada e, acima de tudo, acessível. A estilização, como as bordas, é uma camada de apresentação que deve ser, preferencialmente, gerenciada pelo CSS.

Agora que sabemos como exibir informações, o próximo passo lógico é aprender como **coletar informações** do usuário. No próximo capítulo, mergulharemos nas **listas**, explorando seus diversos tipos e estilos.
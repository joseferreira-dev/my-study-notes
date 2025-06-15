# Capítulo 11 – Formulários: A Ponte para a Interatividade

Até este ponto de nossa jornada, dominamos a arte de estruturar e apresentar informações. Construímos textos, organizamos dados em tabelas, criamos listas, incorporamos mídias e aprendemos a identificar e agrupar elementos. Agora, damos o passo mais crucial em direção à criação de páginas verdadeiramente interativas: aprenderemos a **coletar dados do usuário**. Seja em uma página de login, um formulário de contato, uma pesquisa de satisfação ou um carrinho de compras, os formulários são o principal mecanismo pelo qual a web deixa de ser um monólogo e se torna um diálogo.

Neste capítulo, faremos um mergulho profundo no universo dos formulários HTML. Começaremos com o elemento `<form>`, o contêiner que define os limites e o comportamento de toda a coleta de dados. Em seguida, exploraremos o vasto e versátil mundo dos campos de entrada com o elemento `<input>`, detalhando seus inúmeros tipos e atributos que nos permitem capturar desde um simples texto até datas, cores e arquivos. Daremos atenção especial à acessibilidade e à organização, compreendendo a função vital dos rótulos (`<label>`) e do agrupamento de campos (`<fieldset>`). Por fim, abordaremos as poderosas ferramentas de validação nativas do HTML5 e outros elementos essenciais como botões, áreas de texto e listas de seleção.

Ao final, você estará equipado não apenas para construir formulários funcionais, mas para projetá-los de maneira semântica, acessível e intuitiva, criando uma ponte robusta entre o usuário e a aplicação.

## O Elemento `<form>`: O Contêiner da Interação

Tudo começa com o elemento `<form>`. Ele atua como um contêiner que agrupa todos os controles de entrada (inputs, botões, etc.) que pertencem a uma mesma submissão de dados. Quando um formulário é enviado, são os dados dos elementos dentro deste contêiner que são coletados e transmitidos. Seus atributos definem como e para onde esses dados serão enviados.

```html
<form action="/processar-dados" method="post">
  <!-- Controles do formulário (inputs, botões, etc.) virão aqui -->
</form>
```

Os atributos do elemento `<form>` são:

- **`action`**: Um dos atributos mais importantes. Especifica a URL do **script** no servidor que irá processar os dados do formulário quando ele for enviado. Se omitido, os dados são enviados para a URL da própria página.
- **`method`**: Define o método HTTP que será usado para enviar os dados. Os dois valores mais comuns são:
    - `get` (padrão): Anexa os dados do formulário à URL especificada no `action` (ex: `/processar?nome=joao&idade=30`). É ideal para formulários de busca ou quando se deseja que a URL possa ser salva nos favoritos. Possui um limite de tamanho e não deve ser usado para enviar dados sensíveis.
    - `post`: Envia os dados no corpo da requisição HTTP, de forma separada da URL. É mais seguro para dados sensíveis (como senhas), não tem limite de tamanho e é o método preferido para qualquer ação que modifique dados no servidor (cadastros, logins, etc.).
- **`enctype` (Encoding Type)**: Especifica como os dados do formulário devem ser codificados antes de serem enviados ao servidor. É relevante apenas quando `method="post"`.
    - `application/x-www-form-urlencoded` (padrão): Todos os caracteres são codificados antes de serem enviados.
    - `multipart/form-data`: Necessário e **obrigatório** se o formulário permitir o upload de arquivos (`<input type="file">`). Ele não codifica os caracteres e divide os dados em múltiplas partes.
    - `text/plain`: Envia os dados sem nenhuma codificação. Raramente utilizado.
- **`autocomplete`**: Controla se o navegador pode ou não preencher automaticamente os campos do formulário com base em dados previamente inseridos pelo usuário. Pode ser definido como `on` (padrão) ou `off`. Desligar o autocomplete é útil em campos de segurança, como um campo para confirmar uma nova senha.
- **`name`**: Atribui um nome ao formulário. Útil quando se tem múltiplos formulários na mesma página e se deseja referenciá-los com JavaScript.
- **`novalidate`**: Um atributo booleano. Se presente, ele desativa a validação padrão do navegador para os campos do formulário. Útil para fins de teste ou quando se deseja implementar uma validação customizada com JavaScript.
- **`target`**: Especifica onde exibir a resposta recebida após o envio do formulário. Funciona como o atributo `target` dos links (`_self`, `_blank`, etc.).

## Rótulos (`<label>`): Acessibilidade em Primeiro Lugar

Um formulário sem rótulos é um formulário inacessível e confuso. O elemento `<label>` fornece uma descrição textual para controle de formulário. Sua importância é imensa:

1. **Acessibilidade**: Leitores de tela usam o `<label>` para anunciar a finalidade do campo ao usuário.
2. **Usabilidade**: Clicar em um `<label>` foca ou ativa o controle de formulário associado a ele. Para checkboxes e radio buttons, clicar no rótulo marca a opção, aumentando a área de clique.

A associação entre um `<label>` e um `<input>` é feita usando o atributo `for` no label, cujo valor deve ser igual ao `id` do input.

**Exemplo de uso correto:**

```html
<label for="nome-usuario">Nome de Usuário:</label>
<input type="text" id="nome-usuario" name="usuario">
```

## O Versátil Elemento `<input>`

O elemento `<input>` é, sem dúvida, o mais utilizado e versátil dos formulários. É um elemento vazio, e seu comportamento é drasticamente alterado pelo seu atributo `type`.

### Atributos Comuns do `<input>`

Antes de explorarmos os tipos, vamos ver os atributos que são comuns à maioria deles:

- **`type`**: Define o tipo de controle de entrada a ser renderizado. Veremos os tipos em detalhe a seguir.
- **`id`**: Um identificador único para o elemento, essencial para a associação com `<label>`.
- **`name`**: O nome do controle. Este nome é enviado ao servidor junto com o valor do campo (`name=value`). **Se um input não tiver o atributo `name`, seu dado não será enviado**.
- **`value`**: O valor inicial do campo. Para checkboxes e radio buttons, é o valor que será enviado caso a opção esteja marcada.
- **`placeholder`**: Fornece uma dica ou um exemplo de valor esperado para o campo. O texto desaparece quando o usuário começa a digitar. **Não substitui um `<label>`**.
- **`disabled`**: Atributo booleano que desativa o campo. Um campo desativado não pode ser focado, não pode ser alterado e **seu valor não é enviado** com o formulário.
- **`readonly`**: Atributo booleano que torna o campo apenas de leitura. O usuário não pode alterar o valor, mas **o valor é enviado** com o formulário.
- **`required`**: Atributo booleano que torna o preenchimento do campo obrigatório para a submissão do formulário.
- **`autofocus`**: Atributo booleano que, quando presente, faz com que o campo receba o foco automaticamente assim que a página é carregada.
- **`size`**: Define a largura do campo de entrada em número de caracteres (exclusivamente visual, limita somente os caracteres visíveis, não a quantidade que pode ser inserida).
- **`title`**: Fornece informações extras sobre o elemento, geralmente exibidas como uma tooltip.

### Tipos de `<input>`

O HTML5 expandiu drasticamente o número de tipos de `input`, oferecendo interfaces mais ricas e validações nativas.

|Tipo|Descrição e Uso|Exemplo de Código|
|---|---|---|
|`text`|O padrão. Um campo de texto de uma única linha.|`<input type="text" name="nome">`|
|`password`|Como o `text`, mas os caracteres digitados são mascarados (exibidos como ••••).|`<input type="password" name="senha">`|
|`email`|Para endereços de e-mail. Navegadores podem exibir um teclado otimizado em dispositivos móveis e validam se o formato parece ser um e-mail válido.|`<input type="email" name="email_contato">`|
|`tel`|Para números de telefone. Não aplica uma validação de formato específica (pois os formatos variam muito), mas pode acionar teclados numéricos em dispositivos móveis.|`<input type="tel" name="telefone">`|
|`number`|Para valores numéricos. Navegadores geralmente exibem controles de setas para incrementar/decrementar o valor.|`<input type="number" name="quantidade" min="1" max="10">`|
|`search`|Funcionalmente similar ao `text`, mas estilizado para indicar que é um campo de busca (pode incluir um "x" para limpar o campo).|`<input type="search" name="q">`|
|`url`|Para endereços web (URLs). O navegador valida se o formato parece ser uma URL válida.|`<input type="url" name="website">`|
|`checkbox`|Uma caixa de seleção que pode ser marcada ou desmarcada. Usado para múltiplas opções independentes.|`<input type="checkbox" name="interesses" value="esportes"> Esportes`|
|`radio`|Um botão de opção. Usado em um grupo onde apenas uma opção pode ser selecionada. **Todos os radios de um mesmo grupo devem ter o mesmo atributo `name`**.|`<input type="radio" name="genero" value="m"> Masculino`|
|`range`|Um controle deslizante para selecionar um valor dentro de um intervalo.|`<input type="range" name="volume" min="0" max="100">`|
|`color`|Fornece uma interface (geralmente um seletor de cores) para que o usuário escolha uma cor.|`<input type="color" name="cor_favorita">`|
|`file`|Permite que o usuário selecione um ou mais arquivos de seu dispositivo para upload. Lembre-se de usar `enctype="multipart/form-data"` no `<form>`.|`<input type="file" name="documento" accept=".pdf,.doc">`|
|`date`|Apresenta uma interface para selecionar uma data (dia, mês, ano).|`<input type="date" name="nascimento">`|
|`time`|Apresenta uma interface para selecionar um horário (hora, minutos).|`<input type="time" name="hora_reuniao">`|
|`datetime-local`|Para selecionar data e hora, sem fuso horário.|`<input type="datetime-local" name="evento_inicio">`|
|`month`|Para selecionar um mês e ano.|`<input type="month" name="mes_fatura">`|
|`week`|Para selecionar uma semana e ano.|`<input type="week" name="semana_projeto">`|
|`submit`|Um botão que, quando clicado, envia o formulário. O `value` define o texto do botão.|`<input type="submit" value="Cadastrar">`|
|`reset`|Um botão que, quando clicado, redefine todos os controles do formulário para seus valores iniciais.|`<input type="reset" value="Limpar Formulário">`|
|`button`|Um botão genérico sem ação padrão. Sua funcionalidade deve ser definida com JavaScript.|`<input type="button" value="Clique Aqui">`|
|`image`|Um botão de envio (`submit`) que é exibido como uma imagem. Requer o atributo `src`.|`<input type="image" src="btn_enviar.png" alt="Enviar">`|
|`hidden`|Um campo que não é visível para o usuário, mas cujo `name` e `value` são enviados com o formulário. Útil para passar informações de estado, como um ID de usuário.|`<input type="hidden" name="id_produto" value="12345">`|

## Agrupando Campos: `<fieldset>` e `<legend>`

Para formulários longos, é uma boa prática agrupar campos relacionados. O elemento `<fieldset>` cria uma caixa em torno de um grupo de campos, e o elemento `<legend>` fornece um título para esse grupo. Isso melhora a organização visual e a acessibilidade.

```html
<fieldset>
  <legend>Informações Pessoais</legend>

  <label for="nome">Nome:</label>
  <input type="text" id="nome" name="nome">

  <label for="email">Email:</label>
  <input type="email" id="email" name="email">
</fieldset>
```

## O Poder dos Botões com `<button>`

Embora `<input type="submit">` funcione, o elemento `<button>` é geralmente mais flexível e recomendado. Ao contrário do `<input>`, o `<button>` pode conter outros elementos HTML, como imagens (`<img>`) ou texto estilizado (`<strong>`, `<em>`), permitindo a criação de botões muito mais ricos.

O comportamento do `<button>` é definido pelo seu atributo `type`:

- `submit` (padrão): Envia o formulário.
- `reset`: Reseta o formulário.
- `button`: Não faz nada por padrão, aguardando uma ação do JavaScript.

```html
<button type="submit">
  <img src="icone-salvar.svg" alt="">
  <strong>Salvar</strong> Alterações
</button>
```

### Atributos `form*` para Botões

Os botões possuem um conjunto especial de atributos que lhes permite substituir os atributos do elemento `<form>` pai. Isso é extremamente útil para ter botões com ações diferentes no mesmo formulário.

- `formaction`: Substitui o atributo `action` do formulário.
- `formmethod`: Substitui o atributo `method` do formulário.
- `formenctype`: Substitui o atributo `enctype` do formulário.
- `formnovalidate`: Se presente, desativa a validação para este envio específico.
- `formtarget`: Substitui o atributo `target` do formulário.

**Exemplo:**

```html
<form action="/salvar" method="post">
  <!-- campos do formulário -->
  <button type="submit">Salvar</button>
  <button type="submit" formaction="/salvar-e-visualizar" formtarget="_blank">
    Salvar e Visualizar
  </button>
</form>
```

## Validação Nativa do HTML5

O HTML5 introduziu atributos que permitem ao navegador validar os dados de entrada antes de enviar o formulário, sem a necessidade de JavaScript.

- **`required`**: O campo não pode estar vazio.
- **`minlength` e `maxlength`**: Definem o número mínimo e máximo de caracteres para campos de texto.
- **`min` e `max`**: Definem os valores mínimo e máximo para campos numéricos (number, range, date, etc.).
- **`pattern`**: Permite especificar uma **Expressão Regular** (RegEx) que o valor do campo deve corresponder para ser considerado válido. É uma ferramenta extremamente poderosa para validações customizadas (ex: formato de CPF, placa de carro).
- **`accept`** (para `input type="file"`): Especifica os tipos de arquivo que o usuário pode selecionar (ex: `accept="image/png, image/jpeg"`).

**Exemplo de validação com `pattern`:**

```html
<label for="cep">CEP (formato: 00000-000):</label>
<input type="text" id="cep" name="cep" required
       pattern="[0-9]{5}-[0-9]{3}"
       title="O CEP deve estar no formato 12345-678">
```

## Caixas de Texto e Seleções Avançadas

### `<textarea>`

Para entradas de texto com múltiplas linhas (como um campo de comentários ou descrição), use o elemento `<textarea>`.

```html
<label for="comentario">Seu Comentário:</label>
<textarea id="comentario" name="comentario" rows="5" cols="40"></textarea>
```

### Seleções com `<select>`

O elemento `<select>` cria uma lista de opções suspensa (dropdown).

- **`<option>`**: Define cada item da lista. O atributo `value` contém o dado que será enviado ao servidor. O texto entre `<option>` e `</option>` é o que o usuário vê.
- **`<optgroup>`**: Agrupa opções relacionadas dentro da lista, com um `label` descritivo.
- **`selected`**: Atributo booleano que pré-seleciona uma opção.

```html
<label for="estado">Estado:</label>
<select id="estado" name="estado">
  <optgroup label="Sudeste">
    <option value="SP">São Paulo</option>
    <option value="RJ">Rio de Janeiro</option>
  </optgroup>
  <optgroup label="Nordeste">
    <option value="BA" selected>Bahia</option>
    <option value="PE">Pernambuco</option>
  </optgroup>
</select>
```

### Sugestões com `<datalist>`

O `<datalist>` fornece uma funcionalidade de "autocomplete" para um campo `<input>`. Ele é uma lista de `<option>`s pré-definidas que o navegador sugere ao usuário enquanto ele digita. A ligação é feita através do atributo `list` no `<input>`, que deve corresponder ao `id` do `<datalist>`.

```html
<label for="navegador">Seu navegador preferido:</label>
<input list="navegadores" id="navegador" name="navegador">

<datalist id="navegadores">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
</datalist>
```

## Considerações Finais

Neste capítulo, desbravamos o componente mais interativo da web: o formulário. Vimos como o elemento `<form>` serve de alicerce, definindo o destino e o método de envio dos dados. Exploramos a incrível diversidade do elemento `<input>`, que, com seu atributo `type`, se transforma para atender a praticamente todas as nossas necessidades de coleta de dados.

Reforçamos a importância crucial da semântica e da acessibilidade através do uso correto de `<label>`, `<fieldset>` e `<legend>`. Aprendemos a criar botões mais ricos com `<button>` e listas de seleção complexas com `<select>` e `<datalist>`. Por fim, vimos como as ferramentas de validação nativas do HTML5 nos permitem criar experiências de usuário mais robustas e seguras com um esforço mínimo.

Dominar os formulários é dominar o diálogo com o usuário. É a porta de entrada para a construção de aplicações web dinâmicas, onde a informação não apenas flui para o usuário, mas também dele para o servidor, abrindo um universo de possibilidades que serão exploradas com linguagens de script como o JavaScript e tecnologias de back-end.
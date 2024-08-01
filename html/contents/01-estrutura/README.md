<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/html"><img src="../../banner_html.png"></a>
</div>
<br>

# Estrutura Básica e Elementos Básicos

- [Estrutura Básica de um Documento HTML](#estrutura-básica-de-um-documento-html)

A estrutura básica de uma página HTML é composta por uma série de tags que delimitam diferentes partes do documento e definem seu conteúdo e apresentação. Cada documento HTML começa com a declaração do tipo de documento, seguida por elementos essenciais como `<html>`, `<head>`, `<body>` e suas respectivas subtags (elementos filhos).

## Estrutura Básica de um Documento HTML

A estrutura de uma página HTML é fundamental para qualquer desenvolvedor web, pois define como o conteúdo será organizado e exibido no navegador. Um documento HTML é composto por várias tags que definem diferentes partes e elementos da página. 

A extensão padrão de um arquivo HTML é `.html`. Um arquivo com essa extensão pode ser aberto em qualquer navegador web. Por convençam, o arquivo que representa a página principal de um site é nomeado como `index.html`. 

Vamos começar com um exemplo completo de um documento HTML básico e, em seguida, detalhar cada um dos seus componentes. Vale ressaltar que a indentação em um documento HTML não tem influência, mas ela é muito importante para representar a hierarquia entre os elementos.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minha Primeira Página HTML</title>
</head>
<body>
  <h1>Bem-vindo ao HTML</h1>
  <p>Este é um parágrafo de exemplo em uma página HTML.</p>
  <a href="https://www.exemplo.com">Visite o Exemplo</a>
</body>
</html>
```

A linha `<!DOCTYPE html>` é a declaração do tipo de documento, que informa ao navegador que este é um documento HTML5. Esta declaração é importante para garantir que o navegador renderize a página de acordo com os padrões HTML5. Apesar dessa ser a escrita padrão, a declaração do doctype é do tipo case-insensitive, logo todas as formas a seguir são válidas:

```html
<!doctype html>
<!DOCTYPE html>
<!DOCTYPE HTML>
<!DoCtYpE hTmL>
```

A tag `<html>` envolve todo o conteúdo do documento. O atributo `lang="pt-br"` especifica que o idioma da página é o português do Brasil, o que ajuda na acessibilidade e na indexação pelos motores de busca. Dentro da tag `<html>`, temos duas seções principais: `<head>` e `<body>`.

A tag `<head>` contém metadados sobre o documento, como informações sobre o título da página, codificação de caracteres e configurações de viewport. A tag `<meta charset="UTF-8">` define a codificação de caracteres como UTF-8, garantindo que a página possa exibir corretamente uma ampla gama de caracteres. A tag `<meta name="viewport" content="width=device-width, initial-scale=1.0">` ajusta a escala da página para que ela seja exibida corretamente em diferentes dispositivos, especialmente em dispositivos móveis. A tag `<title>` define o título da página, que será exibido na aba do navegador.

A tag `<body>` contém o conteúdo visível da página, que será renderizado no navegador, ou seja, aquele que ficará visível para os usuários. No exemplo acima temos algumas tags dentro do `<body>`:

- `<h1>`: Representa um título de nível 1, geralmente usado para o título principal da página. No exemplo, `<h1>Bem-vindo ao HTML</h1>` define um cabeçalho com o texto "Bem-vindo ao HTML".
- `<p>`: Representa um parágrafo de texto. No exemplo, `<p>Este é um parágrafo de exemplo em uma página HTML.</p>` cria um parágrafo com o texto especificado.
- `<a>`: Representa um link. O atributo href especifica o destino do link. No exemplo, `<a href="https://www.exemplo.com">Visite o Exemplo</a>` cria um link que, ao ser clicado, direciona o usuário para "https://www.exemplo.com".

Esses são os elementos básicos que compõem uma página HTML. Cada tag tem uma função específica e, ao combiná-las, podemos criar páginas web estruturadas e funcionais. É importante entender que a correta utilização dessas tags é fundamental para garantir a acessibilidade, a indexação por motores de busca e uma boa experiência de usuário.

## Headings (Cabeçalhos)

Os headings (cabeçalhos) em HTML são usados para definir títulos e subtítulos em uma página web. Eles são importantes para a estruturação do conteúdo, ajudando tanto os usuários quanto os motores de busca a entenderem a hierarquia e a organização das informações. Existem seis níveis de headings, de `<h1>` a `<h6>`, sendo `<h1>` o mais importante e `<h6>` o menos importante. Este é um exemplo completo de todos os headings:

```html
<h1>Heading 1 - Título Principal</h1>
<h2>Heading 2 - Subtítulo</h2>
<h3>Heading 3 - Subtítulo</h3>
<h4>Heading 4 - Subtítulo</h4>
<h5>Heading 5 - Subtítulo</h5>
<h6>Heading 6 - Subtítulo</h6>
```

Os mecanismos de busca, bem como ferramentas utilizadas por usuários, geralmente indexam o conteúdo de uma página com base na hierarquia de seus elementos, principalmente de cabeçalhos, por exemplo, para criar um índice, portanto, usar uma estrutura correta para esses elementos é importante.

Em geral, uma página deve ter somente um elemento `<h1>` para o título principal seguido por subtítulos `<h2>`, e assim por diante conforme necessidade. Se houver elementos `<h1>` em um nível mais alto, eles não devem ser usados ​​para descrever nenhum conteúdo de nível mais baixo.

Por exemplo de página seguinte essas diretrizes poderia ser (a indentação extra neste exemplo está sendo usada somente para fins de visualização):

```html
<h1>Título principal</h1>
<p>Introdução</p>
  <h2>Artigos</h2>
    <h3>Artigo 1</h3>
    <p>Parágrafo</p>
    <h3>Artigo 2</h3>
    <p>Parágrafo</p>
  <h2>Conclusão</h2>
  <p>Parágrafo</p>
```

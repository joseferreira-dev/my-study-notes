# Capítulo 1 – HTML: Fundamentos da Linguagem da Web

Bem-vindo ao ponto de partida de toda a World Wide Web. Antes de criarmos páginas visualmente impressionantes ou aplicações interativas complexas, precisamos construir a fundação, a estrutura sobre a qual todo o resto será erguido. Essa fundação é o **HTML**, a linguagem que dá forma e significado a praticamente todo o conteúdo que consumimos online. Em sua essência, o HTML atua como o esqueleto de uma página web, definindo hierarquias e organizando informações para que os navegadores possam interpretá-las e exibi-las de forma coerente.

Neste capítulo introdutório, faremos uma jornada para entender a essência do HTML. Começaremos explorando sua história e evolução, compreendendo como uma ferramenta simples para compartilhar documentos científicos se transformou na espinha dorsal da internet moderna. Em seguida, dissecaremos a estrutura básica indispensável a toda e qualquer página HTML e definiremos os conceitos cruciais de **tags**, **elementos** e **atributos**. Por fim, apresentaremos a importância do **HTML5** e da **semântica**, uma mudança de paradigma que revolucionou a forma como escrevemos código para a web, tornando-o mais inteligente, acessível e otimizado.

Ao final deste capítulo, você terá a base conceitual necessária para entender não apenas _como_ escrever HTML, mas _por que_ o escrevemos de uma determinada maneira, preparando o terreno para os capítulos seguintes, onde exploraremos em detalhes o vasto vocabulário de tags e atributos à nossa disposição.

## Histórico e Evolução: Da Academia para o Mundo

A história do HTML está intrinsecamente ligada à criação da própria Web. No final dos anos 1980 e início dos anos 1990, o físico Tim Berners-Lee, enquanto trabalhava no CERN (Organização Europeia para a Pesquisa Nuclear), idealizou uma forma de cientistas compartilharem documentos e pesquisas de maneira simples e interligada através da internet. Para isso, ele criou três tecnologias fundamentais: o protocolo HTTP, as URLs e, crucialmente, o HTML.

A primeira versão do HTML era extremamente simples, contendo poucas tags para definir títulos, parágrafos, listas e, o mais importante, links — o "hipertexto" que permitia navegar de um documento para outro. A evolução a partir daí foi rápida e impulsionada pela necessidade de uma web mais rica e funcional:

- **HTML 2.0 (1995):** Tornou-se o primeiro padrão oficial, estabelecendo uma base comum para os navegadores da época implementarem.
- **HTML 3.2 (1997):** Publicado pelo recém-formado World Wide Web Consortium (W3C), adicionou suporte a tabelas, applets e outras funcionalidades de apresentação.
- **HTML 4.01 (1999):** Uma versão marcante que começou a promover a separação entre a **estrutura (HTML)** e a **apresentação (CSS - Cascading Style Sheets)**. Tags que serviam apenas para fins visuais começaram a ser desencorajadas.
- **XHTML 1.0 (2000):** Uma reformulação do HTML 4.01 como uma aplicação XML, impondo regras de sintaxe muito mais rígidas (por exemplo, todas as tags deveriam ser fechadas).
- **HTML5 (2014):** A grande revolução. Desenvolvido para atender às demandas da web moderna, o HTML5 introduziu tags para multimídia (`<video>`, `<audio>`), elementos para desenho gráfico (`<canvas>`) e, fundamentalmente, um conjunto robusto de **tags semânticas** para descrever melhor a estrutura do conteúdo. Hoje, o HTML é considerado um "padrão vivo", continuamente atualizado pelo WHATWG (Web Hypertext Application Technology Working Group).

## A Estrutura Básica de uma Página HTML

Apesar de toda a sua evolução, a estrutura fundamental de um documento HTML permanece consistente. Qualquer página, da mais simples à mais complexa, é construída sobre o mesmo esqueleto básico, que informa ao navegador como processar o arquivo.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Título da Página</title>
</head>
<body>
	<h1>Título Principal</h1>
</body>
</html>
```

Vamos analisar brevemente cada parte:

- **`<!DOCTYPE html>`**: A primeira linha de todo documento. É uma declaração que informa ao navegador que ele deve interpretar o código usando a especificação mais recente do HTML5.
- **`<html>`**: O elemento raiz que envolve todo o conteúdo da página. O atributo `lang="pt-br"` indica que o idioma principal da página é o português do Brasil.
- **`<head>`**: Esta seção contém metadados, ou seja, informações sobre a página que não são exibidas diretamente no conteúdo visível. Inclui coisas como o conjunto de caracteres (`<meta charset="UTF-8">`) e o título da página (`<title>`).
- **`<body>`**: Contém todo o conteúdo que será de fato exibido ao usuário, como textos, imagens, vídeos e links.

## O Que São Tags, Elementos e Atributos?

Para dar estrutura ao conteúdo, o HTML utiliza uma sintaxe simples baseada em três conceitos-chave:

- **Tags**: São as "etiquetas" de marcação. Elas são definidas por um nome envolto em colchetes angulares (`<` e `>`), como `<p>`, `<h1>` ou `<body>`. Geralmente, elas vêm em pares: uma tag de abertura (`<p>`) e uma tag de fechamento (`</p>`), que inclui uma barra `/` antes do nome.
- **Elementos**: Um elemento é a unidade completa, consistindo na tag de abertura, no conteúdo e na tag de fechamento. Por exemplo, `<p>Isso é um parágrafo.</p>` é um elemento de parágrafo completo.
- **Atributos**: Fornecem informações adicionais ou configurações para um elemento. Eles são sempre especificados dentro da tag de abertura e consistem em um nome e um valor (ex: `nome="valor"`). No exemplo da estrutura básica, `lang="pt-br"` é um atributo do elemento `<html>`, definindo seu idioma.

Juntos, esses três componentes formam os blocos de construção de qualquer página HTML.

## HTML5 e a Revolução da Semântica

Uma das maiores contribuições do HTML5 foi a ênfase na **semântica**. Um código semântico é aquele que utiliza tags que descrevem o **significado** e o **propósito** de seu conteúdo, e não apenas sua aparência.

Antes do HTML5, era comum estruturar uma página usando elementos genéricos `<div>`, com atributos id ou class para descrever seu papel:

```html
<div id="cabecalho">...</div>`
```

Com o HTML5, podemos usar uma tag que carrega esse significado em seu próprio nome:

```html
<header>...</header>
```

Essa mudança é muito mais do que uma simples convenção. O uso de tags semânticas como `<header>`, `<footer>`, `<nav>`, `<main>`, `<article>` e `<section>` traz benefícios imensos:

1. **Acessibilidade**: Leitores de tela para deficientes visuais conseguem navegar pela página de forma muito mais eficiente, pulando diretamente para a navegação (`<nav>`) ou para o conteúdo principal (`<main>`).
2. **SEO (Search Engine Optimization)**: Motores de busca como o Google entendem melhor a estrutura e a relevância do seu conteúdo, o que pode melhorar seu ranqueamento nos resultados de pesquisa.
3. **Manutenibilidade**: O código se torna mais claro e autoexplicativo para outros desenvolvedores (e para você mesmo no futuro), facilitando a manutenção e a colaboração.

Escrever HTML semântico é, portanto, uma das práticas mais importantes para o desenvolvedor web moderno.

## Considerações Finais

Neste capítulo, desvendamos os conceitos primordiais do HTML. Vimos que ele nasceu como uma ferramenta simples, mas evoluiu para uma linguagem robusta que estrutura toda a web. Compreendemos que toda página segue uma estrutura básica e é construída a partir de elementos, que são definidos por tags e configurados por atributos. Mais importante, introduzimos a ideia de semântica, destacando que um bom código HTML descreve o significado do conteúdo, uma prática fundamental consolidada pelo HTML5.

Com esta base sólida estabelecida, estamos prontos para avançar. Nos próximos capítulos, vamos explorar em profundidade o vocabulário do HTML, detalhando as diversas tags e atributos que nos permitem estruturar textos, listas, tabelas, formulários e muito mais, transformando teoria em prática na construção de páginas web bem-formadas e significativas.
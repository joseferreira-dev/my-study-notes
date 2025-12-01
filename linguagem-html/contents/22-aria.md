# Capítulo 22 – ARIA

Ao longo desta apostila, focamos em como usar o HTML para dar estrutura e significado ao conteúdo. Aprendemos que usar a tag `<nav>` para navegação ou `<button>` para um botão não é apenas uma convenção, mas uma prática que fornece semântica inerente, permitindo que navegadores, mecanismos de busca e **tecnologias assistivas** (como leitores de tela para pessoas com deficiência visual) compreendam o propósito de cada parte de nossa página. Isso é a base da acessibilidade.

No entanto, a web moderna evoluiu. Criamos componentes complexos e dinâmicos com JavaScript — menus dropdown, abas, sliders, caixas de diálogo — que vão além do que os elementos HTML nativos podem descrever. Como um leitor de tela pode saber que um `<div>` clicável é, na verdade, um "interruptor" que está no estado "ligado"? Como ele pode anunciar que um menu sanfona está "expandido" ou "recolhido"? É para preencher essa lacuna que o **WAI-ARIA (Web Accessibility Initiative – Accessible Rich Internet Applications)**, ou simplesmente ARIA, foi criado.

Neste capítulo, vamos explorar o ARIA, um conjunto de atributos que enriquece nosso HTML com a semântica necessária para tornar aplicações web complexas totalmente acessíveis. Começaremos com a regra mais importante do seu uso, e então mergulharemos no seu componente principal: o atributo `role`, que define o que um elemento _é_ ou _faz_.

## Primeira e Mais Importante Regra do ARIA

Antes de escrever qualquer atributo ARIA, memorize esta regra fundamental:

**Não use ARIA, se você pode usar um elemento HTML nativo que já fornece a semântica e o comportamento de que você precisa.**

O HTML nativo é gratuito em termos de acessibilidade. Um elemento `<button>` já é focável pelo teclado, clicável com Enter/Espaço e anunciado como "botão" por leitores de tela. Um `<input type="checkbox">` já gerencia seu estado de "marcado" e "não marcado".

Se você criar um botão com `<div role="button">`, você se torna responsável por implementar TODA essa funcionalidade (foco, eventos de teclado, etc.) com JavaScript. ARIA é uma ponte, não uma fundação. Use-o para aprimorar a semântica, não para recriar o que o HTML já faz bem.

## O Que é ARIA?

ARIA é uma especificação que adiciona uma camada de significado ao HTML através de atributos, sem afetar a aparência ou o comportamento para usuários que não usam tecnologias assistivas. Ela se divide em três componentes:

1. **Roles (Funções):** Definem o que um elemento é. Ex: `role="tab"`, `role="dialog"`.
2. **Properties (Propriedades):** Descrevem as características de um elemento. Ex: `aria-required="true"`, `aria-labelledby="id-do-rotulo"`.
3. **States (Estados):** Descrevem a condição atual de um elemento. Ex: `aria-expanded="true"`, `aria-disabled="false"`.

Neste capítulo, nosso foco principal será no `role`, que é o alicerce de qualquer implementação ARIA.

## Atributo `role`

O atributo `role` permite que você sobrescreva a semântica nativa de um elemento, descrevendo seu propósito para as tecnologias assistivas. Por exemplo, ele pode dizer a um leitor de tela que um conjunto de `<div>`s e `<ul>`s, controlados por JavaScript, é na verdade um `role="menubar"`.

As roles são divididas em categorias. Vamos explorar as mais importantes.

### Landmark Roles (Pontos de Referência)

Essas roles são cruciais, pois permitem que usuários de leitores de tela naveguem rapidamente pelas principais seções de uma página, pulando diretamente para o conteúdo que lhes interessa. **Importante:** A maioria dessas roles possui um elemento HTML5 nativo equivalente, que deve ser sempre a sua primeira escolha.

| Role            | Propósito                                                                                                                  | Equivalente HTML (Preferível)                                     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `banner`        | Cabeçalho do site, geralmente com o logo e o título principal.                                                             | `<header>` (quando não está dentro de `article` ou `section`)     |
| `complementary` | Conteúdo tangencial ao conteúdo principal (ex: barras laterais).                                                           | `<aside>`                                                         |
| `contentinfo`   | Informações sobre o documento pai (ex: rodapé com copyright).                                                              | `<footer>` (quando não está dentro de `article` ou `section`)     |
| `form`          | Uma região da página que contém um formulário.                                                                             | `<form>`                                                          |
| `main`          | O conteúdo principal e central do documento.                                                                               | `<main>`                                                          |
| `navigation`    | Um conjunto de links para navegação.                                                                                       | `<nav>`                                                           |
| `region`        | Uma seção importante do conteúdo, que não se encaixa nas outras landmarks. **Deve** ter um rótulo (ex: `aria-labelledby`). | `<section>` (com um nome acessível)                               |
| `search`        | A seção da página que contém a funcionalidade de busca do site.                                                            | `<form role="search">` (a tag `<form>` precisa da role explícita) |

**Quando usar Landmark Roles?** Apenas em navegadores antigos que não suportam os elementos semânticos do HTML5 ou em casos muito específicos onde a estrutura impede o uso da tag nativa. Em 99% dos casos, **use `<nav>`, `<main>`, `<aside>`, etc.**

### Widget Roles (Componentes Interativos)

Esta é a categoria onde ARIA realmente brilha, descrevendo componentes de interface que não existem nativamente no HTML. O uso dessas roles quase sempre exige JavaScript para controlar os estados e a interatividade.

- `alert`: Para mensagens importantes e urgentes que aparecem dinamicamente. Leitores de tela costumam anunciá-las imediatamente.
- `alertdialog`: Uma caixa de diálogo de alerta que exige uma resposta do usuário.
- `button`: Transforma qualquer elemento em um botão. Lembre-se, use `<button>` sempre que possível.
- `checkbox`: Um campo que pode ser marcado ou desmarcado.
- `dialog`: Uma janela de aplicação que se sobrepõe à página principal.
- `gridcell`, `cell`: Uma célula dentro de uma grade (`grid`) ou tabela (`table`).
- `link`: Transforma qualquer elemento em um link. Use `<a>` com `href` sempre que possível.
- `log`: Uma região onde novas informações são adicionadas em ordem (ex: chat, log de status).
- `menu`, `menubar`, `menuitem`, `menuitemcheckbox`, `menuitemradio`: Usados em conjunto para criar menus complexos, como os de aplicações desktop.
- `option`: Uma opção selecionável dentro de um componente maior, como um `listbox`.
- `progressbar`: Exibe o progresso de uma tarefa. Equivalente a `<progress>`.
- `radio`, `radiogroup`: Para um grupo de opções onde apenas uma pode ser selecionada.
- `searchbox`: Uma caixa de texto usada para busca. Use `<input type="search">`.
- `slider`: Um controle deslizante para selecionar um valor em um intervalo. Use `<input type="range">`.
- `spinbutton`: Um campo numérico que permite aumentar/diminuir o valor com botões.
- `status`: Um contêiner para mensagens de status, que não são importantes o suficiente para serem um `alert`.
- `switch`: Um interruptor "liga/desliga".
- `tab`, `tablist`, `tabpanel`: O conjunto de roles usado para criar uma interface de abas.
- `textbox`: Uma caixa de texto. Use `<input type="text">` ou `<textarea>`.
- `timer`: Uma região que contém um contador de tempo.
- `tooltip`: Uma "dica de ferramenta" que descreve outro elemento.
- `tree`, `treeitem`: Para criar uma visão em árvore hierárquica e expansível.

### Document Structure Roles (Estrutura do Documento)

Essas roles descrevem a estrutura do conteúdo. Novamente, a maioria delas possui equivalentes HTML que são preferíveis.

- `article`: Um bloco de conteúdo autossuficiente. Use `<article>`.
- `columnheader`, `rowheader`: Um cabeçalho de coluna ou linha em uma tabela. Use `<th>`.
- `definition`: A definição de um termo. Use `<dd>`.
- `document`: Para conteúdo que funciona como um documento dentro de uma aplicação.
- `group`: Agrupa elementos relacionados.
- `heading`: Um título. Use `<h1>`-`<h6>`.
- `img`: Uma imagem. Use `<img>` com o atributo `alt`.
- `list`, `listitem`: Uma lista e seus itens. Use `<ul>`, `<ol>` e `<li>`.
- `math`: Conteúdo matemático. Use `<math>` (MathML).
- `note`: Uma nota ou comentário sobre o conteúdo principal.
- `table`, `row`, `rowgroup`: Para estruturas de tabela. Use `<table>`, `<tr>`, `<thead>`/`<tbody>`.
- `toolbar`: Uma barra de ferramentas contendo botões de ação ou controles.
- `separator`: Um divisor visual ou semântico. Pode ser usado em menus ou barras de ferramentas. Use `<hr>` para separações temáticas de conteúdo.

### Caso Especial: `role="presentation"` ou `role="none"`

Esta role é única. Seu propósito é **remover a semântica** de um elemento. Isso é útil em casos onde você precisa usar um elemento estrutural (como uma `<table>` ou `<ul>`) para fins puramente de layout visual, e não quer que ele seja anunciado com sua semântica padrão. Por exemplo, usar uma tabela para alinhar campos de um formulário (uma prática não recomendada, mas às vezes necessária) e não querer que o leitor de tela a anuncie como uma "tabela de dados".

```html
<table role="presentation">
  <!-- O leitor de tela não anunciará "tabela", apenas lerá os conteúdos. -->
</table>
```

## Considerações Finais

Neste capítulo, arranhamos a superfície do vasto e poderoso mundo do ARIA. Vimos que ele serve como uma ponte essencial entre a semântica limitada do HTML nativo e a rica interatividade das aplicações web modernas, garantindo que elas sejam utilizáveis por todos.

A mensagem mais importante a ser levada é a **Primeira Regra do ARIA**: sempre prefira elementos HTML nativos. O ARIA não é um substituto, mas um complemento. Ao construir componentes de interface customizados, lembre-se de que o `role` é apenas o primeiro passo. Você também precisará gerenciar os estados e propriedades ARIA (como `aria-expanded` ou `aria-hidden`) com JavaScript e garantir uma experiência de teclado completa e intuitiva.

Usar ARIA de forma correta exige estudo e, acima de tudo, testes com tecnologias assistivas reais. É um investimento que paga dividendos imensuráveis, resultando em produtos digitais mais robustos, profissionais e, o mais importante, inclusivos.
# Capítulo 19 – Atributos Globais: A Caixa de Ferramentas Universal do HTML

Ao longo de nossa exploração do HTML, encontramos uma vasta gama de atributos, cada um servindo a um propósito específico para um determinado elemento: o `src` para uma imagem, o `href` para um link, o `action` para um formulário. No entanto, existe uma classe especial de atributos que transcende elementos individuais. São os **atributos globais**, uma caixa de ferramentas universal que pode ser aplicada a praticamente qualquer tag HTML, fornecendo uma camada fundamental de identificação, estilo, acessibilidade e interatividade.

Neste capítulo, vamos mergulhar fundo nesses atributos onipresentes. Já encontramos alguns deles, como `id`, `class` e `style`, mas agora vamos analisá-los em um contexto mais amplo e introduzir outros que são cruciais para o desenvolvimento de aplicações web modernas e acessíveis. Exploraremos como tornar qualquer elemento editável, como gerenciar a ordem de foco do teclado, como controlar a tradução de conteúdo e como adicionar interatividade de arrastar e soltar.

Compreender o propósito e a aplicação correta dos atributos globais é o que nos permite refinar nossos documentos, transformando elementos estruturais simples em componentes ricos, acessíveis e dinâmicos, prontos para serem manipulados com precisão pelo CSS e pelo JavaScript.

## Identificação e Estilo: `id`, `class` e `style`

Esses três atributos são os pilares da conexão entre HTML, CSS e JavaScript. Embora já os tenhamos abordado, é crucial revisar seu papel como atributos globais.

- **`id`**: Atribui um identificador **único** a um elemento em toda a página. É o método mais eficiente para JavaScript encontrar um elemento específico (`document.getElementById()`) e serve como âncora para links de fragmento (`#meu-id`). Seu uso em CSS deve ser criterioso, pois cria regras de alta especificidade. Lembre-se: um `id` só pode ser usado uma vez por página.

- **`class`**: Associa um elemento a uma ou mais classes (separadas por espaços). É a ferramenta primária para estilização com CSS, permitindo agrupar múltiplos elementos sob as mesmas regras de estilo. Também é uma forma comum de selecionar múltiplos elementos com JavaScript (`document.getElementsByClassName()` ou `document.querySelectorAll('.minha-classe')`).

- **`style`**: Aplica estilos CSS diretamente no elemento (inline). **Seu uso é fortemente desaconselhado** como prática geral, pois viola a separação de preocupações, dificulta a manutenção e tem uma especificidade difícil de sobrescrever. Sua utilização é aceitável em casos específicos, como a aplicação de estilos dinâmicos via JavaScript.

## Internacionalização e Conteúdo: `lang`, `dir` e `translate`

Esses atributos garantem que seu conteúdo seja corretamente interpretado em um contexto global.

- **`lang`**: Declara o idioma do conteúdo de um elemento. Embora seja mais comum vê-lo na tag `<html>` para definir o idioma de toda a página, ele pode ser usado em qualquer elemento para indicar uma mudança de idioma. Isso é vital para a acessibilidade (leitores de tela usam a pronúncia correta) e para ferramentas de tradução.

    ```html
    <p>Meu livro favorito é <cite lang="fr">Le Petit Prince</cite>.</p>
    ```

- **`dir` (Direction)**: Especifica a direcionalidade do texto. O padrão é `ltr` (left-to-right, da esquerda para a direita). Para idiomas como árabe, hebraico ou persa, usa-se `rtl` (right-to-left).

    ```html
    <p dir="rtl">.مرحبا بالعالم</p>
    ```

- **`translate`**: Informa às ferramentas de tradução automática (como o Google Tradutor) se o conteúdo do elemento deve ou não ser traduzido. Os valores são `yes` (padrão) e `no`. É extremamente útil para preservar nomes de marcas, termos técnicos ou exemplos de código.

    ```html
    <p>Nossa empresa se chama <span translate="no">QuantumLeap</span>, e usamos a biblioteca <span translate="no">React</span>.</p>
    ```

## Interatividade e Acessibilidade: O Coração Dinâmico

Estes atributos transformam elementos estáticos em componentes interativos e garantem que eles sejam acessíveis a todos os usuários.

- **`tabindex`**: Um dos atributos mais importantes para a acessibilidade via teclado. Ele controla se um elemento pode receber foco e sua ordem na navegação por tabulação (usando a tecla "Tab").
    - `tabindex="0"`: Permite que um elemento que normalmente não é focável (como `<div>`, `<span>` ou `<p>`) receba foco e o insere na ordem de tabulação natural do documento. Essencial para criar componentes customizados, como um menu dropdown feito com `<div>`s.
    - `tabindex="-1"`: Permite que um elemento receba foco via JavaScript (`elemento.focus()`), mas o **remove** da ordem de tabulação natural. É perfeito para gerenciar o foco em interfaces complexas, como focar em uma janela modal que acabou de abrir.
    - `tabindex="1"` (ou qualquer inteiro positivo): **Cria uma ordem de tabulação separada e é uma má prática de acessibilidade.** Ele força a navegação de forma não intuitiva e deve ser evitado a todo custo.
- **`hidden`**: Um atributo booleano que, quando presente, remove o elemento tanto da exibição visual quanto da árvore de acessibilidade (leitores de tela não o anunciarão). É semanticamente equivalente a usar `display: none;` no CSS.

    ```html
    <div id="aviso-sucesso" hidden>Operação concluída com sucesso!</div>
    ```

- **`contenteditable`**: Um atributo booleano (`true`/`false`) que torna o conteúdo de qualquer elemento editável pelo usuário. Clicar em um elemento com este atributo transforma-o em um campo de edição de texto. É a base de muitos editores de texto "ricos" (WYSIWYG) na web.

    ```html
    <h3>Clique aqui para editar este título</h3>
    <div contenteditable="true" style="border: 1px solid #ccc; padding: 10px;">
        Este é um texto que você pode alterar diretamente no navegador.
    </div>
    ```

- **`draggable`**: Um atributo booleano (`true`/`false`) que indica se um elemento pode ser arrastado pelo usuário, como parte da API de Arrastar e Soltar (Drag and Drop) do HTML5. Definir `draggable="true"` é apenas o primeiro passo; a lógica completa para o que acontece quando o elemento é arrastado e solto precisa ser implementada com JavaScript.

    ```html
    <img src="logo.png" draggable="true" ondragstart="iniciarArrasto(event)">
    ```

- **`title`**: Fornece informações extras ou um rótulo para um elemento. A maioria dos navegadores exibe o conteúdo do `title` como uma "dica de ferramenta" (tooltip) quando o usuário passa o mouse sobre o elemento. **Importante**: Não deve ser usado para informações cruciais, pois não é acessível em dispositivos de toque e não é uma forma robusta de rotular elementos para leitores de tela.

    ```html
    <button title="Salva o documento permanentemente. Esta ação não pode ser desfeita.">Salvar</button>
    ```

- **`spellcheck`**: Um atributo booleano (`true`/`false`) que sinaliza se o navegador deve realizar a verificação ortográfica e gramatical do conteúdo do elemento. Funciona apenas para elementos cujo texto é editável, como `<textarea>`, alguns tipos de `<input>` e qualquer elemento com `contenteditable="true"`.

    ```html
    <textarea spellcheck="true"></textarea>
    ```

- **`contextmenu` (Obsoleto)**: Este atributo foi projetado para associar um elemento a um menu de contexto customizado (`<menu>`). No entanto, ele nunca obteve amplo suporte dos navegadores e foi removido da especificação HTML. É importante saber que ele existe para não tentar usá-lo em projetos modernos.

## Considerações Finais

Neste capítulo, exploramos a "caixa de ferramentas" dos atributos globais do HTML. Vimos como eles fornecem uma camada de funcionalidade que é comum a quase todos os elementos, permitindo-nos identificar, estilizar, internacionalizar e, o mais importante, adicionar interatividade e acessibilidade às nossas páginas.

O uso correto de atributos como `lang`, `tabindex` e `hidden` não é um detalhe, mas sim uma parte essencial da criação de aplicações web que funcionam para todos, em qualquer dispositivo. Da mesma forma, entender o poder de `contenteditable` e `draggable` abre as portas para a criação de interfaces de usuário ricas e dinâmicas, que antes eram domínio exclusivo de frameworks complexos. Conhecer e aplicar esses atributos de forma consciente é um passo definitivo na sua jornada para se tornar um desenvolvedor HTML proficiente e completo.
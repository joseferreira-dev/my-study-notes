# Capítulo 20 – Atributos de Evento: Gatilhos para a Interatividade

Até agora, nossa jornada nos ensinou a construir documentos HTML robustos, semânticos e bem estruturados. Vimos como conectar nossa estrutura a folhas de estilo e scripts externos, estabelecendo as bases para a aparência e o comportamento. Neste capítulo, vamos explorar a cola que une a estrutura estática às ações dinâmicas: os **eventos**. Um evento é qualquer ação que ocorre em uma página web, seja uma interação do usuário (como um clique ou o pressionar de uma tecla) ou uma ação do navegador (como o término do carregamento da página).

O HTML nos fornece uma maneira direta de "ouvir" e reagir a esses eventos através dos **atributos de evento**. São atributos globais, como `onclick` ou `onsubmit`, que nos permitem executar um trecho de código JavaScript em resposta a uma ação específica.

Neste capítulo, vamos explorar os atributos de evento mais comuns, agrupando-os por categoria: formulários, teclado e interface. Para cada um, explicaremos o que o dispara e como usá-lo. No entanto, faremos uma distinção crucial: embora esses atributos sejam uma forma válida de adicionar interatividade, a prática moderna e recomendada para projetos profissionais é usar o método `addEventListener` do JavaScript. Mostraremos ambos os métodos para que você entenda a evolução das práticas de desenvolvimento web e saiba por que a abordagem moderna é superior em termos de organização, manutenção e poder.

## Uma Nota Crítica sobre a Prática Moderna

Os atributos de evento inline, como `onclick="alert('Olá!')"`, misturam a lógica de comportamento (JavaScript) diretamente na estrutura (HTML). Isso viola o princípio da **separação de preocupações** e torna o código mais difícil de ler, depurar e manter em aplicações complexas.

**A forma profissional e recomendada** de lidar com eventos é usar o método `addEventListener` em um arquivo JavaScript separado.

- **Método Inline (HTML):** `<button onclick="minhaFuncao()">`
- **Método Moderno (JavaScript):** `meuBotao.addEventListener('click', minhaFuncao);`

Neste capítulo, demonstraremos os atributos de evento para fins de conhecimento completo do HTML, mas sempre os contrastaremos com a abordagem moderna para que você aprenda a maneira mais profissional e escalável de construir aplicações web.

## Eventos de Formulário

Formulários são a principal fonte de interatividade do usuário, e possuem um rico conjunto de eventos.

- **`onsubmit`**: Disparado no elemento `<form>` quando ele está prestes a ser enviado (geralmente ao clicar em um `<input type="submit">` ou `<button type="submit">`). Este é o evento ideal para validar todos os campos de um formulário de uma só vez antes de enviá-lo ao servidor. Para impedir o envio do formulário, a função chamada deve retornar `false` (no método inline) ou usar `event.preventDefault()` (no método moderno).

    ```html
    <!-- Inline -->
    <form action="/cadastro" onsubmit="return validarFormulario();"> ... </form>
    
    <!-- Moderno -->
    <form id="form-cadastro"> ... </form>
    <script>
        document.getElementById('form-cadastro').addEventListener('submit', function(event) {
            if (!validarFormulario()) {
                event.preventDefault(); // Impede o envio se a validação falhar
            }
        });
    </script>
    ```

- **`onreset`**: Disparado no elemento `<form>` quando o formulário é resetado (ao clicar em um `<input type="reset">`).
- **`onchange`**: Disparado em um elemento de entrada (`<input>`, `<select>`, `<textarea>`) quando seu valor é alterado **e o elemento perde o foco**. Por exemplo, o usuário digita em um campo de texto e depois clica fora dele.
- **`oninput`**: Disparado **imediatamente** toda vez que o valor de um elemento de entrada é alterado. Seja digitando uma letra, colando um texto ou movendo um slider, o `oninput` é acionado. É muito mais responsivo que o `onchange` para feedback em tempo real (ex: contagem de caracteres, validação instantânea).

    ```html
    <!-- Exemplo de contador de caracteres -->
    <textarea oninput="contarCaracteres(this.value)"></textarea>
    <p>Caracteres: <span id="contador">0</span></p>
    
    <script>
      function contarCaracteres(texto) {
        document.getElementById('contador').innerText = texto.length;
      }
    </script>
    ```

- **`oninvalid`**: Disparado em um campo de entrada quando ele falha na validação nativa do HTML5 (ex: um campo `required` está vazio ou um `type="email"` tem um valor inválido no momento do envio).
- **`onselect`**: Disparado quando um texto é selecionado dentro de um elemento `<input type="text">` ou `<textarea>`.

## Eventos de Foco

Esses eventos lidam com o estado de foco de um elemento.

- **`onfocus`**: Disparado quando um elemento **recebe foco**, seja através de um clique do mouse ou da navegação pelo teclado (tecla Tab).

    ```html
    <input type="text" onfocus="this.style.backgroundColor = '#e0f7fa';">
    ```

- **`onblur`**: O oposto de `onfocus`. É disparado quando um elemento **perde o foco**. É o evento onde o `onchange` é tipicamente avaliado.

    ```html
    <input type="text" onblur="this.style.backgroundColor = 'white';">
    ```

## Eventos de Teclado

Esses eventos capturam a interação do usuário com o teclado.

- **`onkeydown`**: Disparado no momento em que uma tecla é **pressionada para baixo**. Ele dispara continuamente se a tecla for mantida pressionada. Este é o evento mais usado para detectar qual tecla foi pressionada (ex: para atalhos de teclado ou controle de jogos).
- **`onkeyup`**: Disparado quando a tecla é **liberada**.
- **`onkeypress` (Obsoleto)**: Disparado após o `onkeydown` quando uma tecla que produz um caractere (como 'a', '5' ou 'espaço') é pressionada. Ele não dispara para teclas como "Shift", "Ctrl" ou "F5". **Este evento está sendo descontinuado** nos padrões web e seu uso deve ser evitado em favor de `onkeydown`.

**Ordem dos Eventos:** `keydown` -> `keypress` (se aplicável) -> `keyup`.

```html
<!-- Exemplo: Detectar se a tecla "Enter" foi pressionada -->
<input type="text" onkeydown="verificarEnter(event)">
<script>
    function verificarEnter(event) {
        // 'event.key' é a forma moderna de obter o nome da tecla
        if (event.key === 'Enter') {
            alert('Você pressionou Enter!');
        }
    }
</script>
```

## Outros Eventos Importantes de Interface

- **`oncontextmenu`**: Disparado quando o usuário tenta abrir o menu de contexto (geralmente clicando com o botão direito do mouse). Pode ser usado para criar um menu de contexto customizado ou para impedir a exibição do menu padrão.

    ```html
    <div oncontextmenu="mostrarMenu(event); return false;">
      Clique com o botão direito aqui.
    </div>
    
    <script>
        function mostrarMenu(event) {
            // event.preventDefault() tem o mesmo efeito de 'return false;'
            alert('Menu de contexto customizado!');
        }
    </script>
    ```

- **`onsearch`**: Específico para `<input type="search">`. É disparado quando o usuário inicia uma busca (seja pressionando "Enter" ou clicando no ícone de "x" que alguns navegadores adicionam ao campo).

## Considerações Finais

Neste capítulo, abrimos a porta para a interatividade ao explorar os atributos de evento do HTML. Vimos como eles agem como "escutas" para as ações do usuário, permitindo-nos disparar funções JavaScript em resposta a envios de formulários, pressionamento de teclas, mudanças de foco e muito mais.

A lição mais importante, no entanto, é a evolução das boas práticas. Embora os atributos `on...` sejam funcionais e importantes de se conhecer, eles representam uma abordagem mais antiga que mistura responsabilidades. A prática profissional moderna, focada na clareza, manutenção e escalabilidade do código, favorece inequivocamente o uso de `addEventListener` em arquivos de script separados.

Com este conhecimento, você agora entende não apenas _o que_ são os eventos, mas também a maneira mais robusta e organizada de gerenciá-los, um passo fundamental para transformar seus documentos estáticos em aplicações web verdadeiramente dinâmicas.
# Capítulo 31 – Funções CSS: Valores Dinâmicos e Poder Computacional

Ao longo da nossa jornada, tratamos o CSS como uma linguagem declarativa: nós declaramos um seletor e definimos valores estáticos para suas propriedades. `font-size: 16px`, `color: blue`, `width: 50%`. Mas e se precisássemos de valores que não são fixos? E se quiséssemos que a largura de um elemento fosse "100% da tela menos 80 pixels"? Ou que uma cor usada em vinte lugares diferentes pudesse ser alterada a partir de um único ponto? Ou que um texto exibido viesse diretamente de um atributo do HTML?

Para realizar essas tarefas dinâmicas e computacionais, o CSS nos oferece uma de suas ferramentas mais poderosas: as **funções**. Funções em CSS são como pequenos programas que você pode executar como valor de uma propriedade. Elas recebem parâmetros, realizam uma operação e retornam um valor que o navegador pode usar. Elas são a ponte entre o CSS estático e um CSS verdadeiramente dinâmico e inteligente.

Neste capítulo, vamos explorar o vasto e versátil mundo das funções CSS. Começaremos com as três mais essenciais para o desenvolvimento moderno: **`calc()`**, para realizar cálculos matemáticos; **`var()`**, a chave para usar as Variáveis CSS (Custom Properties); e **`attr()`**, para extrair conteúdo de atributos HTML. Em seguida, revisitaremos as funções geradoras de imagem, como **`linear-gradient()`** e **`radial-gradient()`**, tratando-as formalmente como funções. Por fim, faremos um apanhado de outras funções importantes que, embora pertençam a outros módulos, são parte integrante deste ecossistema.

## `calc()`: Cálculos em Tempo Real

A função `calc()` é uma das mais revolucionárias do CSS. Ela permite que você execute cálculos matemáticos (`+`, `-`, `*`, `/`) como valor de uma propriedade. Sua maior força é a capacidade de **misturar unidades diferentes** que, de outra forma, seriam incompatíveis.

**Sintaxe:** `property: calc(expressao-matematica);`

**Importante:** Os operadores `+` e `-` **devem ser cercados por espaços**. O `*` e o `/` não têm essa exigência, mas é uma boa prática usá-los também.

**Exemplo 1: Layout Responsivo Preciso** Criar um contêiner que ocupe 100% da largura, mas com um espaçamento fixo nas laterais.

```css
.container {
  /* Ocupa toda a largura menos 40px (20px de cada lado) */
  width: calc(100% - 40px);
  margin: 0 auto;
}
```

**Exemplo 2: Posicionamento Relativo ao Tamanho da Fonte** Posicionar um elemento 50px acima da parte inferior de um contêiner cuja altura é baseada em `em`.

```css
.elemento-deslocado {
  height: calc(10em + 50px);
}
```

## `var()`: Usando Variáveis CSS (Custom Properties)

A função `var()` permite que você insira o valor de uma Propriedade Customizada (comumente chamada de Variável CSS) no lugar de outro valor. Isso é a base para a criação de sistemas de design, temas e folhas de estilo de fácil manutenção.

As variáveis são definidas com um nome que começa com dois hífens (`--`) e são geralmente declaradas no escopo `:root` para serem globais.

**Sintaxe:** `property: var(--nome-da-variavel [, valor-de-fallback]);`

- **`--nome-da-variavel` (obrigatório):** O nome da variável a ser usada.
- **`valor-de-fallback` (opcional):** Um valor a ser usado caso a variável não esteja definida.

**Exemplo Prático (Sistema de Cores):**

```css
/* 1. Definição das variáveis globais */
:root {
  --cor-primaria: #007bff;
  --cor-sucesso: #28a745;
  --padding-padrao: 15px;
}

/* 2. Uso das variáveis com a função var() */
.botao-primario {
  background-color: var(--cor-primaria);
  padding: var(--padding-padrao);
}

.alerta-sucesso {
  border: 1px solid var(--cor-sucesso);
  /* Usando um fallback caso --cor-texto não exista */
  color: var(--cor-texto, black);
}
```

Alterar o valor de `--cor-primaria` no `:root` mudará a cor de todos os elementos que a utilizam, tornando a manutenção de um tema incrivelmente simples.

## `attr()`: Extraindo Valores de Atributos HTML

A função `attr()` pode ser usada para recuperar o valor de um atributo do elemento selecionado e usá-lo no CSS. Seu suporte principal e mais robusto é com a propriedade `content` em pseudo-elementos.

**Sintaxe:** `content: attr(nome-do-atributo);`

**Exemplo Prático (Tooltips com Atributos `data-*`):**

```html
<span data-tooltip="Esta é a dica!">Passe o mouse</span>
```

```css
span[data-tooltip]::after {
  /* Pega o valor do atributo 'data-tooltip' e o insere aqui */
  content: attr(data-tooltip);

  /* Estilos para o tooltip... */
  position: absolute;
  background: #333;
  color: white;
  padding: 5px;
  border-radius: 4px;
  /* Outros estilos para esconder/mostrar o tooltip */
}
```

O suporte para usar `attr()` com outras propriedades além de `content` (como `color` ou `width`) ainda é experimental e não deve ser usado em produção.

## Funções Geradoras de Imagem

Como vimos no Capítulo 12, gradientes não são cores, mas sim imagens geradas pelo CSS. Formalmente, eles são valores do tipo `<image>` criados por funções.

### `linear-gradient()` e `repeating-linear-gradient()`

Criam uma transição suave de cores ao longo de uma linha reta.

```css
.fundo-gradiente {
  background-image: linear-gradient(to top right, #ff9a9e, #fecfef);
}
```

### `radial-gradient()` e `repeating-radial-gradient()`

Criam uma transição de cores que se irradia a partir de um ponto.

```css
.fundo-explosao {
  background-image: radial-gradient(circle at center, yellow, orange, red);
}
```

## Outras Funções Importantes no Ecossistema CSS

Muitas propriedades que já estudamos usam funções como seus valores. É útil reconhecê-las como parte deste ecossistema.

- **Funções de Transformação:** Usadas com a propriedade `transform` para mover, rotacionar e escalar elementos.
    - **`translate(x, y)`**, **`translateX(x)`**, **`translateY(y)`**: Movem um elemento.
    - **`rotate(angle)`**: Rotaciona um elemento.
    - **`scale(x, y)`**: Redimensiona um elemento.
    - **`skew(x-angle, y-angle)`**: Inclina um elemento.
    
    ```css
    .elemento-movido {
      transform: translateX(50px) rotate(10deg);
    }
    ```
    
- **Funções de Cor:** Usadas para definir cores.
    - `rgb(r, g, b)`, `rgba(r, g, b, a)`
    - `hsl(h, s, l)`, `hsla(h, s, l, a)`
- **Funções de Forma:** Usadas com propriedades como `clip-path` (para recortar um elemento em uma forma) e `shape-outside` (para fazer o texto fluir ao redor de uma forma).
    - `circle(radius at position)`
    - `ellipse(rx ry at position)`
    - `polygon(x1 y1, x2 y2, ...)`
    
    ```css
    .imagem-recortada {
      /* Recorta a imagem em um círculo */
      clip-path: circle(50% at center);
    }
    ```
    
- **Funções de Grid Layout:** Usadas para definir trilhas de grade de forma eficiente.
    - `repeat(count, track-size)`
    - `minmax(min, max)`

## Boas Práticas com Funções CSS

As funções elevam o CSS de uma simples linguagem de estilo para um sistema mais dinâmico e programável. Usá-las de forma inteligente é uma marca do desenvolvimento front-end moderno.

- **Abrace `calc()` para Layouts Fluidos:** Use `calc()` para misturar unidades relativas e absolutas. É a ferramenta perfeita para criar layouts que ocupam uma porcentagem da tela, menos um espaçamento fixo para as barras laterais ou "calhas".
- **Construa seu Design System com `var()`:** A base de qualquer site ou aplicação manutenível hoje em dia é um sistema de design consistente. Use variáveis CSS para definir sua paleta de cores, escala de tipografia, sistema de espaçamento e tamanhos de sombra. Isso torna a criação de temas (como um modo claro/escuro) e a manutenção do projeto drasticamente mais simples.
- **Prefira Gradientes a Imagens:** Sempre que precisar de um fundo com transição de cores ou padrões simples, use as funções de gradiente do CSS. Elas são infinitamente escaláveis, não exigem requisições HTTP adicionais e são muito mais flexíveis e performáticas do que usar um arquivo de imagem.
- **Mantenha a Simplicidade e a Legibilidade:** Funções podem ser aninhadas (ex: `calc(var(--largura-base) * 2)`), mas evite criar lógicas excessivamente complexas que sejam difíceis de depurar. O CSS é declarativo por natureza; mantenha essa clareza sempre que possível.
- **Use `attr()` para Conteúdo Semântico:** A função `attr()` é ideal para exibir pequenos pedaços de metadados que já existem no seu HTML (como em tooltips ou para exibir o valor de um link `href`), evitando a duplicação de conteúdo.

## Considerações Finais

Neste capítulo, exploramos as funções do CSS, as ferramentas que nos permitem realizar cálculos, reutilizar valores, gerar imagens e extrair dados, tudo diretamente em nossas folhas de estilo. Vimos como `calc()` nos liberta das restrições de unidades estáticas e como `var()` nos permite criar sistemas de design robustos e de fácil manutenção com as Propriedades Customizadas.

Compreender e utilizar estas funções transforma fundamentalmente a maneira como escrevemos CSS. Deixamos de apenas declarar estilos e passamos a criar sistemas de regras dinâmicas e inteligentes. Este conhecimento não é apenas útil; é essencial para o desenvolvimento web moderno e para a criação de interfaces complexas e escaláveis.
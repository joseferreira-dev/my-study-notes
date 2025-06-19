# Capítulo 17 – Profundidade e Ênfase com Sombras

Até agora, nossa jornada nos ensinou a construir e estilizar os elementos como entidades bidimensionais. Definimos suas formas, cores, bordas e como seu conteúdo se ajusta. No entanto, o design de interfaces moderno frequentemente busca inspiração no mundo real, onde a luz e a profundidade desempenham um papel crucial na forma como percebemos os objetos. Como podemos trazer essa sensação de profundidade e realismo para a web? A resposta está no uso inteligente de **sombras**.

Sombras no CSS são muito mais do que um efeito decorativo. Elas são uma ferramenta de comunicação visual poderosa. Uma sombra sutil pode elevar um elemento, indicando que ele está "acima" do resto do conteúdo, como um cartão ou uma janela modal. Ela pode sinalizar interatividade, aparecendo quando o usuário passa o mouse sobre um botão. Ela pode criar uma hierarquia visual clara, separando diferentes painéis e seções de uma interface.

Neste capítulo, vamos mergulhar fundo na arte de criar sombras com CSS. A nossa principal ferramenta será a versátil propriedade **`box-shadow`**, que nos permite aplicar sombras à caixa de um elemento. Vamos dissecar sua sintaxe, entendendo como controlar o deslocamento, o desfoque, a propagação e a cor de uma sombra. Exploraremos técnicas avançadas, como a criação de sombras internas (`inset`), a aplicação de múltiplas sombras para efeitos complexos e a criação de sombras mais realistas e direcionadas. Também faremos a distinção crucial entre `box-shadow` e sua alternativa, **`filter: drop-shadow()`**, entendendo quando usar cada uma para obter o melhor resultado.

## `box-shadow`: A Sombra da Caixa

A propriedade `box-shadow` anexa uma ou mais sombras à caixa de um elemento. É importante lembrar: ela segue os contornos do **Box Model** (incluindo o `border-radius`), e não a forma do conteúdo em si.

### A Sintaxe Completa

A sintaxe de uma única sombra pode parecer intimidante, mas é composta por partes lógicas:

`box-shadow: [inset] [offset-x] [offset-y] [blur-radius] [spread-radius] [color];`

- **`inset` (opcional):** Se esta palavra-chave estiver presente, a sombra é desenhada **dentro** da borda do elemento (sombra interna), em vez de fora.
- **`offset-x` (obrigatório):** O deslocamento horizontal da sombra. Um valor positivo a move para a direita; um valor negativo a move para a esquerda.
- **`offset-y` (obrigatório):** O deslocamento vertical da sombra. Um valor positivo a move para baixo; um valor negativo a move para cima.
- **`blur-radius` (opcional):** O raio de desfoque. Um valor de `0` cria uma sombra nítida e sem desfoque. Valores maiores criam uma sombra mais suave e "esfumaçada".
- **`spread-radius` (opcional):** O raio de propagação. Um valor positivo aumenta o tamanho da sombra, fazendo-a "se espalhar" para além do tamanho do elemento. Um valor negativo a encolhe. O padrão é `0`.
- **`color` (opcional):** A cor da sombra. É uma **ótima prática** usar `rgba()` ou `hsla()` para criar sombras semitransparentes, que parecem muito mais realistas. Se omitida, a cor pode depender do navegador, mas geralmente herda a propriedade `color` do elemento.

**Exemplo Prático Detalhado:**

```html
<div class="sombra-exemplo">Passe o mouse para ver a sombra</div>
```

```css
.sombra-exemplo {
  width: 200px;
  padding: 2rem;
  background-color: white;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.sombra-exemplo:hover {
  /*
    offset-x: 0       -> A sombra não se desloca horizontalmente.
    offset-y: 10px    -> A sombra se desloca 10px para baixo.
    blur-radius: 30px -> A sombra é muito suave e desfocada.
    spread-radius: -5px -> A sombra é ligeiramente menor que o elemento.
    color: rgba(...)  -> Cor preta com 20% de opacidade.
  */
  box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.2);
  transform: translateY(-5px); /* Efeito de elevação */
}
```

### Sombra Interna (Inner Shadow) com `inset`

A palavra-chave `inset` muda completamente o efeito, fazendo com que a sombra seja projetada para dentro do elemento, dando uma aparência de "pressionado" ou "entalhado".

```html
<input type="text" class="input-inset" placeholder="Campo com sombra interna">
```

```css
.input-inset:focus {
  outline: none;
  /* Uma sombra interna suave e escura no topo e à esquerda */
  box-shadow: inset 2px 2px 5px rgba(0, 0, 0, 0.1);
}
```

### Sombras Múltiplas

Você pode aplicar um número virtualmente ilimitado de sombras a um único elemento, separando cada declaração de sombra por uma vírgula. A primeira sombra na lista fica no topo.

**Exemplo (Efeito Neon):**

```html
<h1 class="titulo-neon">NEON</h1>
```

```css
.titulo-neon {
  color: white;
  /* Múltiplas sombras de texto para criar o brilho */
  text-shadow:
    0 0 5px #fff,
    0 0 10px #fff,
    0 0 20px #007bff,
    0 0 30px #007bff;
}

/* Aplicando o mesmo conceito a uma caixa */
.caixa-neon {
  border-radius: 10px;
  box-shadow:
    0 0 5px fuchsia, /* Brilho interno */
    inset 0 0 5px fuchsia, /* Brilho externo */
    0 0 20px rebeccapurple,
    inset 0 0 20px rebeccapurple;
}
```

## A Alternativa: `filter: drop-shadow()`

Enquanto `box-shadow` é poderosa, ela tem uma limitação: a sombra é sempre baseada na forma da **caixa** do elemento. E se tivermos uma imagem PNG com um fundo transparente e quisermos que a sombra siga os contornos do conteúdo visível da imagem?

É aqui que entra a função `drop-shadow()` da propriedade `filter`.

**A Diferença Crucial:**

- **`box-shadow`:** Aplica uma sombra ao retângulo que contém o elemento.
- **`filter: drop-shadow()`:** Aplica uma sombra que se conforma à forma exata dos pixels opacos do elemento, respeitando a transparência.

A sintaxe de `drop-shadow()` é mais simples e não inclui `inset` ou `spread-radius`. `filter: drop-shadow(offset-x offset-y blur-radius color);`

**Exemplo Comparativo:**

```html
<!-- Um balão de fala, que não é um retângulo perfeito -->
<img src="caminho/para/balao-de-fala.png" class="sombra-box" alt="Balão com box-shadow">
<img src="caminho/para/balao-de-fala.png" class="sombra-drop" alt="Balão com drop-shadow">
```

```css
.sombra-box {
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.4);
}

.sombra-drop {
  filter: drop-shadow(10px 10px 10px rgba(0, 0, 0, 0.4));
}
```

**Resultado Visual:**

- A primeira imagem terá uma sombra retangular que não se ajusta à "ponta" do balão de fala.
- A segunda imagem terá uma sombra perfeita que contorna toda a forma do balão, incluindo sua ponta.

## Técnica Avançada: Sombra Realista Apenas na Parte Inferior

Sombras no mundo real raramente são uniformes. Frequentemente, a luz vem de cima, projetando uma sombra mais proeminente e suave na parte inferior do objeto. Podemos replicar este efeito realista no CSS usando um pseudo-elemento.

**A Técnica:**

1. Criamos um pseudo-elemento (`::after` ou `::before`).
2. Posicionamos este pseudo-elemento atrás e ligeiramente abaixo do elemento principal.
3. Aplicamos um `box-shadow` bem suave e desfocado a este pseudo-elemento.
4. Usamos `z-index` para garantir que o pseudo-elemento fique atrás.

```html
<div class="card-sombra-realista">
  <h3>Card com Sombra Realista</h3>
  <p>Note como a sombra é mais suave e concentrada na parte inferior.</p>
</div>
```

```css
.card-sombra-realista {
  position: relative; /* Contexto de posicionamento para o pseudo-elemento */
  background: white;
  padding: 1.5rem;
  border-radius: 10px;
  z-index: 10; /* Garante que o card fique na frente da sua própria sombra */
}

.card-sombra-realista::after {
  content: '';
  position: absolute;
  /* Posiciona a sombra: 50% da altura do card para baixo */
  /* Começa no centro horizontal e tem 90% da largura do card */
  left: 5%;
  top: 50%;
  width: 90%;
  height: 100%;
  
  /* A sombra é aplicada ao pseudo-elemento, não ao card */
  box-shadow: 0 40px 50px rgba(0, 0, 0, 0.2);
  
  /* Um truque de perspectiva */
  transform: scale(1, 0.7);
  
  /* A mágica do z-index */
  z-index: -1; /* Coloca o pseudo-elemento (e sua sombra) atrás do card */
  
  /* Animação sutil */
  transition: transform 0.3s ease;
}

.card-sombra-realista:hover::after {
  transform: scale(1, 1); /* A sombra "se expande" no hover */
}
```

## Boas Práticas com Sombras

São boas práticas:

- **A Sutileza é a Chave:** As sombras mais eficazes são aquelas que você quase não percebe conscientemente. Evite sombras pretas, sólidas e com deslocamento grande. Prefira sombras com bastante `blur`, pouco ou nenhum `spread`, e uma cor `rgba()` com baixa opacidade (ex: `rgba(0, 0, 0, 0.1)`).
- **Cuidado com a Performance:** Animar a propriedade `box-shadow` pode ser custoso para o navegador, pois exige um novo cálculo de pintura a cada frame. Se precisar animar uma "elevação", prefira animar as propriedades `transform` e `opacity`, que são muito mais performáticas. Aplique a sombra no estado final e anime a transição do elemento.
- **Use Variáveis CSS (Custom Properties):** Para manter um estilo de sombra consistente em todo o seu site (ex: para diferentes níveis de elevação de "Material Design"), defina suas sombras em variáveis no `:root`.
    
    ```css
    :root {
      --shadow-sm: 0 2px 4px rgba(0,0,0,0.05);
      --shadow-md: 0 8px 15px rgba(0,0,0,0.1);
      --shadow-lg: 0 15px 30px rgba(0,0,0,0.15);
    }
    .card { box-shadow: var(--shadow-md); }
    ```
    
- **Escolha a Ferramenta Certa:** Lembre-se da regra de ouro: se você precisa de uma sombra que respeite a transparência e os contornos exatos do seu conteúdo (como um ícone PNG), use `filter: drop-shadow()`. Para todo o resto (sombras em botões, cards, contêineres), `box-shadow` é a sua ferramenta principal.

## Considerações Finais

Neste capítulo, adicionamos uma nova dimensão aos nossos designs ao dominar a criação de sombras. Vimos a incrível versatilidade da propriedade **`box-shadow`**, detalhando cada um de seus parâmetros para criar desde sombras nítidas e internas até efeitos suaves e complexos com múltiplas camadas.

Estabelecemos a distinção fundamental entre `box-shadow`, que age sobre a caixa do elemento, e `filter: drop-shadow()`, que age sobre a forma visível do conteúdo, dando-nos a ferramenta certa para cada cenário. Exploramos também técnicas avançadas para criar sombras mais realistas e direcionadas, elevando o nível de sofisticação de nossas interfaces.

Com este conhecimento, você pode agora manipular a percepção de profundidade, criar hierarquia e adicionar um toque de realismo aos seus componentes. O próximo passo natural é aprender a organizar todos esses elementos estilizados em um layout coeso e funcional. A partir do próximo capítulo, mergulharemos de cabeça nos sistemas de layout do CSS, começando pela propriedade `display`.
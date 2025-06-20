# Capítulo 36 – Transições e Animações

Até agora, nossa jornada pelo CSS nos ensinou a criar interfaces visualmente ricas e bem estruturadas. No entanto, o design moderno vai além da estética estática; ele envolve movimento, feedback e interatividade. Uma interface que reage às ações do usuário com um movimento suave parece mais polida, intuitiva e viva. Como podemos fazer um botão mudar de cor gradualmente ao passar o mouse? Como podemos fazer um ícone pulsar para chamar a atenção ou um menu deslizar suavemente para a tela?

Para dar vida aos nossos elementos, o CSS nos oferece dois mecanismos poderosos: **Transições (Transitions)** e **Animações (Animations)**. Embora ambos criem movimento ao longo do tempo, eles servem a propósitos diferentes:

- **Transições:** São projetadas para animar a mudança de um estado para outro. Elas são simples e perfeitas para reações a interações do usuário, como `:hover` ou `:focus`. Você define a propriedade a ser observada e a duração, e o navegador cuida da interpolação suave entre o estado inicial e o final.
- **Animações:** São muito mais poderosas e complexas. Elas permitem que você crie sequências de múltiplos passos, definindo o que acontece em diferentes pontos percentuais ao longo da duração da animação através de `@keyframes`. Animações podem ser repetidas, pausadas e executadas em direções diferentes, independentemente de uma ação do usuário.

Neste capítulo, vamos explorar em profundidade essas duas ferramentas. Começaremos com as transições, detalhando cada uma de suas propriedades e as funções de tempo que controlam sua aceleração. Em seguida, mergulharemos no mundo das animações com `@keyframes`, aprendendo a criar sequências complexas e a controlar cada aspecto de seu ciclo de vida. Por fim, abordaremos as melhores práticas para garantir que nossos movimentos sejam performáticos e melhorem, em vez de prejudicar, a experiência do usuário.

## Transições: Animando Mudanças de Estado

A família de propriedades `transition` permite que a mudança no valor de uma propriedade CSS ocorra suavemente ao longo de um período especificado.

### `transition-property`

Define qual ou quais propriedades CSS devem ser animadas. Você pode especificar uma única propriedade (`background-color`), várias separadas por vírgula (`opacity, transform`), ou usar a palavra-chave `all` para animar qualquer propriedade que mudar.

### `transition-duration`

Define a duração da transição. O valor é em segundos (`s`) ou milissegundos (`ms`). Este valor é **obrigatório** para que uma transição ocorra.

### `transition-delay`

Define um tempo de espera (atraso) antes que a transição comece.

### `transition-timing-function`

Esta é a propriedade que define a "sensação" do movimento. Ela controla a curva de aceleração da transição, determinando se o movimento começa lento e acelera, começa rápido e desacelera, ou se move a uma velocidade constante.

- `ease` (padrão): Começa lento, acelera no meio e desacelera no final.
- `linear`: Velocidade constante do início ao fim. Corresponde a `cubic-bezier(0,0,1,1)`.
- `ease-in`: Começa lento e acelera até o final. Corresponde a `cubic-bezier(0.42, 0, 1, 1)`.
- `ease-out`: Começa rápido e desacelera até o final. Corresponde a `cubic-bezier(0, 0, 0.58, 1)`.
- `ease-in-out`: Semelhante a `ease`, mas com uma aceleração e desaceleração mais pronunciadas. Corresponde a `cubic-bezier(0.42, 0, 0.58, 1)`.
- `cubic-bezier(p1, p2, p3, p4)`: Permite que você defina sua própria curva de tempo customizada, especificando os pontos de controle da curva de Bézier. É uma ferramenta avançada para um controle fino do movimento.

### `transition` (Shorthand)

É a forma mais comum de declarar uma transição, combinando as propriedades acima em uma única linha. **Sintaxe:** `transition: [property] [duration] [timing-function] [delay];`

**Exemplo Prático (Botão com Hover):**

```html
<button class="botao-transicao">Passe o Mouse</button>
```

```css
.botao-transicao {
  background-color: #007bff;
  color: white;
  padding: 1rem 2rem;
  border: none;
  border-radius: 5px;
  /* Define a transição para duas propriedades, com durações e delays diferentes */
  transition:
    background-color 0.3s ease-out,
    transform 0.3s ease-out 0.1s; /* O transform tem um pequeno delay */
}

.botao-transicao:hover {
  background-color: #0056b3;
  transform: translateY(-3px);
  cursor: pointer;
}
```

## Animações com `@keyframes`

Para sequências de animação complexas, que não são apenas uma mudança de A para B, usamos a regra-at **`@keyframes`**. Dentro dela, você define os "quadros-chave" da sua animação, especificando os estilos que o elemento deve ter em diferentes pontos percentuais (`%`) ao longo do tempo.

### Definindo os Quadros-Chave

**Sintaxe:**

```css
@keyframes nome-da-animacao {
  from { /* ou 0% */
    /* estilos iniciais */
  }
  50% {
    /* estilos no meio da animação */
  }
  to { /* ou 100% */
    /* estilos finais */
  }
}
```

**Exemplo `@keyframes` (Arco-Íris Pulsante):** Este exemplo cria uma animação que percorre as cores do arco-íris ao longo de 12 passos.

```css
@keyframes rainbow-background {
  0%    { background-color: #ff0000; }
  8.333%  { background-color: #ff8000; }
  16.667% { background-color: #ffff00; }
  25.000% { background-color: #80ff00; }
  33.333% { background-color: #00ff00; }
  41.667% { background-color: #00ff80; }
  50.000% { background-color: #00ffff; }
  58.333% { background-color: #0080ff; }
  66.667% { background-color: #0000ff; }
  75.000% { background-color: #8000ff; }
  83.333% { background-color: #ff00ff; }
  91.667% { background-color: #ff0080; }
  100.00% { background-color: #ff0000; }
}
```

### Aplicando a Animação

Uma vez que os `@keyframes` são definidos, você os aplica a um elemento usando a família de propriedades `animation`.

- **`animation-name`**: O nome dos `@keyframes` que você quer usar.
- **`animation-duration`**: A duração de **um ciclo** da animação.
- **`animation-timing-function`**: A curva de aceleração (usa os mesmos valores de `transition`).
- **`animation-delay`**: O tempo de espera antes do início da animação.
- **`animation-iteration-count`**: Quantas vezes a animação deve se repetir. Pode ser um número (`3`) ou a palavra-chave `infinite`.
- **`animation-direction`**: A direção da animação em ciclos subsequentes.
    - `normal` (padrão): Sempre recomeça do início.
    - `reverse`: Toca de trás para frente.
    - `alternate`: Toca para frente no primeiro ciclo, para trás no segundo, e assim por diante.
    - `alternate-reverse`: O oposto de `alternate`.
- **`animation-fill-mode`**: Define quais estilos são aplicados ao elemento antes e depois da animação.
    - `none` (padrão): O elemento retorna ao seu estilo original após a animação.
    - `forwards`: O elemento retém os estilos do último quadro-chave após o término da animação.
    - `backwards`: O elemento assume os estilos do primeiro quadro-chave durante o `animation-delay`.
    - `both`: Aplica as regras de `forwards` e `backwards`.
- **`animation-play-state`**: Permite pausar (`paused`) e retomar (`running`) uma animação, geralmente controlado via JavaScript.

### `animation` (Shorthand)

Combina todas as propriedades de animação em uma única linha.

**Exemplo (Aplicando a Animação do Arco-Íris):**

```html
<div class="RainbowBackground"></div>
```

```css
@keyframes rainbow-background { /* ... */ }

.RainbowBackground {
  width: 100px;
  height: 100px;
  /* Atalho: nome duração timing-function delay iteração direção */
  animation: rainbow-background 5s linear 0s infinite;
}
```

**Resultado:** Uma caixa que muda suavemente de cor, percorrendo todo o espectro do arco-íris em um ciclo de 5 segundos, repetindo-se infinitamente.

## Performance e a Propriedade `will-change`

Animar certas propriedades (como `width`, `height`, `margin`, `top`) pode ser custoso para o navegador, pois exige um novo cálculo de layout ("reflow"). Propriedades como `transform` e `opacity` são muito mais performáticas porque geralmente podem ser aceleradas pela GPU.

A propriedade `will-change` é uma dica que você dá ao navegador, informando-o de antemão que você pretende animar uma propriedade específica. Isso permite que o navegador faça otimizações antes que a animação comece.

**Sintaxe:** `will-change: property;`

```css
.elemento-animado {
  /* Informa ao navegador que a propriedade 'transform' provavelmente será animada. */
  will-change: transform;
  transition: transform 0.5s ease;
}

.elemento-animado:hover {
  transform: scale(1.1);
}
```

**Cuidado:** Não aplique `will-change` a muitos elementos. Use-o apenas nos elementos que terão animações complexas ou que precisam de um desempenho extra.

## Boas Práticas com Transições e Animações

O movimento na UI pode ser uma ferramenta poderosa para melhorar a experiência do usuário, mas, quando mal utilizado, pode se tornar uma distração ou até mesmo causar desconforto. Seguir boas práticas é essencial para criar animações eficazes e responsáveis.

- **A Animação Deve ter um Propósito:** Use movimento para guiar o usuário, fornecer feedback, indicar uma mudança de estado ou mascarar a latência de carregamento. Evite animações puramente decorativas e excessivas que não servem a um propósito funcional.
- **Prefira Animar `transform` e `opacity`:** Para uma performance suave, priorize a animação de `transform` (para mover, escalar, rotacionar) e `opacity` (para desvanecer). Essas propriedades são otimizadas pelos navegadores e geralmente não causam recalculos de layout.
- **Respeite a Preferência por Movimento Reduzido:** Alguns usuários são sensíveis a movimento e podem sentir náuseas ou tonturas. Sempre use a media query `prefers-reduced-motion` para desativar ou reduzir animações para esses usuários.
    
    ```css
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
      }
    }
    ```
    
- **Use `transition` para Estados Simples:** Para interações simples de dois estados (ex: normal -> hover), `transition` é a ferramenta mais limpa e adequada.
- **Use `animation` para Sequências Complexas:** Quando você precisa de controle sobre múltiplos passos, repetição ou direção, `animation` com `@keyframes` é a escolha correta.
- **Mantenha as Durações Curtas:** Animações de UI geralmente devem ser rápidas (entre `200ms` e `500ms`). Animações muito lentas podem fazer a interface parecer lenta e preguiçosa.

## Considerações Finais

Neste capítulo, demos vida aos nossos elementos estáticos com as ferramentas de movimento do CSS. Desvendamos as **transições**, a maneira perfeita de criar mudanças de estado suaves e reativas, e mergulhamos no poder das **animações** com `@keyframes`, que nos permitem orquestrar sequências complexas e repetitivas.

Compreendemos a importância das funções de tempo (`timing-function`) para dar uma sensação natural ao movimento e aprendemos a otimizar a performance com a propriedade `will-change`. Ao aplicar estes conceitos com propósito e moderação, você pode transformar suas interfaces de meramente funcionais para verdadeiramente prazerosas de se usar, guiando e encantando o usuário a cada interação.
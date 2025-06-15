# Capítulo 15 – Canvas: A Tela de Desenho Dinâmica

Nos últimos capítulos, exploramos as diversas maneiras de exibir conteúdo estruturado e mídia na web. Vimos como o SVG nos permite criar gráficos vetoriais escaláveis e interativos. Agora, vamos explorar uma tecnologia com um propósito diferente, mas igualmente poderosa: o elemento **`<canvas>`**. Enquanto o SVG é descritivo e baseado em objetos, o `<canvas>` nos oferece uma tela em branco, uma área de desenho baseada em pixels onde podemos usar JavaScript para criar gráficos, animações, jogos e outras visualizações complexas em tempo real.

Neste capítulo, vamos desvendar o funcionamento do `<canvas>`. Começaremos por entender sua natureza fundamental como uma API de desenho de "modo imediato" e a importância do seu "contexto de renderização". Em seguida, colocaremos a mão na massa, aprendendo os comandos essenciais em JavaScript para desenhar formas simples, como retângulos e linhas. A partir daí, construiremos exemplos mais complexos, desenhando caminhos (`paths`) para criar formas customizadas e, finalmente, introduziremos o conceito de laço de animação (`animation loop`) para dar vida aos nossos desenhos.

Dominar o `<canvas>` é abrir uma porta para um novo mundo de criatividade na web, um mundo onde os limites são definidos não por tags, mas pela sua imaginação e sua habilidade de programar. É a ferramenta de escolha para qualquer aplicação que exija renderização gráfica dinâmica e de alta performance.

## O Que é o Elemento `<canvas>`?

O elemento `<canvas>` é, em sua essência, uma tag HTML que cria um contêiner retangular de tamanho fixo em sua página. Sozinho, ele é invisível. Pense nele como uma tela de pintura em branco. Todo o "desenho" que acontece nele não é feito com HTML, mas sim com **JavaScript**.

### A Diferença Fundamental: Canvas vs. SVG

Para entender o `<canvas>`, é crucial contrastá-lo com o SVG.

- **SVG (Retained Mode):** O SVG é **descritivo**. Você define um conjunto de objetos (círculos, retângulos, caminhos) no seu HTML, e o navegador "lembra" de cada um deles. Se você quiser mudar a cor de um círculo, você seleciona aquele objeto e altera sua propriedade `fill`. Os objetos fazem parte do DOM.
- **Canvas (Immediate Mode):** O `<canvas>` é **procedural**. Você usa JavaScript para dar comandos de desenho ("pinte um retângulo aqui", "desenhe uma linha ali"). Uma vez que o comando é executado, o navegador "esquece" o que foi desenhado; ele apenas sabe que certos pixels na tela agora têm uma nova cor. Não há objetos para selecionar. Para fazer uma alteração, você precisa apagar a área e redesenhar a cena inteira.

Essa diferença define seus casos de uso: SVG é ótimo para ícones, logos e infográficos com interatividade em elementos individuais. Canvas é imbatível para jogos, animações complexas e visualização de dados com milhares de elementos, onde a performance é crítica.

### A Estrutura Básica

O HTML para um canvas é minimalista. Seus atributos mais importantes definem suas dimensões.

```html
<canvas id="minhaTela" width="600" height="400">
  Seu navegador não suporta o elemento canvas. Por favor, atualize-o.
</canvas>
```

Os atributos do `<canvas>` são:

- **`id`**: Indispensável. Usamos este ID para selecionar o canvas com JavaScript.
- **`width` e `height`**: Estes atributos definem o tamanho da **área de desenho** em pixels. É crucial definir as dimensões aqui, e não via CSS. Se você dimensionar o canvas com CSS, o navegador irá "esticar" a área de desenho original, o que pode causar distorções e perda de qualidade.

O conteúdo entre `<canvas>` e `</canvas>` serve como texto de fallback para navegadores muito antigos que não suportam o elemento.

## Desenhando com JavaScript: O Contexto de Renderização

Como mencionado, a tag `<canvas>` é apenas o contêiner. Para desenhar, precisamos obter seu **contexto de renderização**. O contexto mais comum, e o foco deste capítulo, é o 2D.

A configuração inicial em JavaScript sempre segue estes passos:

```javascript
// 1. Seleciona o elemento canvas no DOM
const canvas = document.getElementById('minhaTela');

// 2. Obtém o contexto de renderização 2D
const ctx = canvas.getContext('2d');
```

A variável `ctx` (uma abreviação comum para "context") é o nosso pincel. É através dela que executaremos todos os comandos de desenho.

## Exemplo Simples: Desenhando Formas Básicas

Vamos começar com os comandos mais simples.

### Retângulos

A API do Canvas 2D possui métodos diretos para desenhar retângulos.

- `fillStyle`: Define a cor de preenchimento.
- `fillRect(x, y, width, height)`: Desenha um retângulo preenchido.
- `strokeStyle`: Define a cor da borda.
- `lineWidth`: Define a espessura da borda.
- `strokeRect(x, y, width, height)`: Desenha o contorno de um retângulo.
- `clearRect(x, y, width, height)`: Apaga uma área retangular, deixando-a transparente.

**Exemplo Completo:**

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>Canvas - Exemplo Simples</title>
    <style>
        body { display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; background-color: #f0f0f0; }
        canvas { border: 1px solid black; background-color: white; }
    </style>
</head>
<body>
    <canvas id="minhaTela" width="600" height="400"></canvas>

    <script>
        const canvas = document.getElementById('minhaTela');
        const ctx = canvas.getContext('2d');

        // Desenha um retângulo azul preenchido
        ctx.fillStyle = 'blue';
        ctx.fillRect(50, 50, 150, 100); // (x, y, largura, altura)

        // Desenha o contorno de um retângulo vermelho
        ctx.strokeStyle = 'red';
        ctx.lineWidth = 5;
        ctx.strokeRect(250, 50, 150, 100);
    </script>
</body>
</html>
```

## Exemplo Complexo: Desenhando com Caminhos (`Paths`)

Para desenhar qualquer coisa que não seja um retângulo (linhas, triângulos, círculos, formas complexas), usamos **caminhos**. O processo é como desenhar no papel: você move sua caneta para um ponto, desenha linhas ou curvas, e no final, decide se quer traçar o contorno ou preencher a forma.

O fluxo de trabalho é sempre o mesmo:

1. `beginPath()`: Inicia um novo caminho. É como levantar a caneta do papel para começar um novo desenho.
2. `moveTo(x, y)`: Move a "caneta" para a coordenada de início, sem desenhar nada.
3. Use comandos de desenho, como `lineTo(x, y)` (desenha uma linha) ou `arc(...)`.
4. Opcionalmente, `closePath()` para fechar a forma, desenhando uma linha de volta ao ponto inicial.
5. `stroke()` para desenhar o contorno ou `fill()` para preencher a forma.

**Exemplo Completo de um "Smiley":**

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>Canvas - Desenhando com Caminhos</title>
    <style>
        body { display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; background-color: #f0f0f0; }
        canvas { border: 1px solid black; background-color: white; }
    </style>
</head>
<body>
    <canvas id="telaSmiley" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById('telaSmiley');
        const ctx = canvas.getContext('2d');

        // Rosto (círculo amarelo)
        ctx.beginPath();
        // ctx.arc(x, y, raio, anguloInicial, anguloFinal)
        ctx.arc(200, 200, 150, 0, Math.PI * 2); // Círculo completo
        ctx.fillStyle = 'yellow';
        ctx.fill();
        ctx.strokeStyle = 'black';
        ctx.lineWidth = 4;
        ctx.stroke();

        // Olho Esquerdo
        ctx.beginPath();
        ctx.arc(150, 150, 20, 0, Math.PI * 2);
        ctx.fillStyle = 'black';
        ctx.fill();

        // Olho Direito
        ctx.beginPath();
        ctx.arc(250, 150, 20, 0, Math.PI * 2);
        ctx.fill();

        // Sorriso
        ctx.beginPath();
        ctx.arc(200, 220, 100, 0, Math.PI, false); // Meio círculo para o sorriso
        ctx.stroke();

    </script>
</body>
</html>
```

## Dando Vida: Animações

A verdadeira magia do canvas acontece quando criamos animações. A animação em canvas é um conceito simples que se assemelha a um filme: exibimos uma sequência de quadros (frames) em alta velocidade, criando a ilusão de movimento.

O processo para cada quadro é:

1. **Apagar:** Limpar todo o canvas.
2. **Atualizar:** Calcular as novas posições dos elementos.
3. **Desenhar:** Redesenhar todos os elementos em suas novas posições.

Para criar um laço suave e eficiente, usamos a função `requestAnimationFrame(callback)`. Ela pede ao navegador para executar uma função de callback (o nosso laço de animação) antes da próxima repintura da tela.

**Exemplo de uma Bola Quicando:**

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>Canvas - Animação</title>
    <style> canvas { border: 1px solid black; } </style>
</head>
<body>
    <canvas id="telaAnimacao" width="800" height="600"></canvas>
    <script>
        const canvas = document.getElementById('telaAnimacao');
        const ctx = canvas.getContext('2d');

        // Propriedades da bola
        let x = canvas.width / 2;
        let y = canvas.height - 30;
        let dx = 2;  // velocidade em x
        let dy = -2; // velocidade em y
        const raioBola = 20;

        function desenharBola() {
            ctx.beginPath();
            ctx.arc(x, y, raioBola, 0, Math.PI*2);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }

        function animar() {
            // 1. Apagar o canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // 2. Desenhar
            desenharBola();
            
            // 3. Atualizar Posições e Lógica
            // Inverte a direção se bater nas paredes laterais
            if(x + dx > canvas.width - raioBola || x + dx < raioBola) {
                dx = -dx;
            }
            // Inverte a direção se bater no teto ou chão
            if(y + dy > canvas.height - raioBola || y + dy < raioBola) {
                dy = -dy;
            }
            
            x += dx;
            y += dy;
            
            // Pede ao navegador para executar 'animar' novamente no próximo quadro
            requestAnimationFrame(animar);
        }

        // Inicia o laço de animação
        animar();
    </script>
</body>
</html>
```

## Considerações Finais

Neste capítulo, arranhamos a superfície de uma das APIs mais criativas da web. Vimos que o `<canvas>` é uma tela em branco, e o JavaScript é nosso pincel. Diferente do SVG, ele não retém objetos, mas nos dá controle total sobre cada pixel, tornando-o ideal para aplicações gráficas dinâmicas e de alta performance.

Aprendemos a configurar o canvas, obter seu contexto 2D e usar comandos para desenhar formas e caminhos. Mais importante, introduzimos o conceito de laço de animação com `requestAnimationFrame`, a base para a criação de jogos, visualizações de dados interativas, editores de imagem e muito mais.

O `<canvas>` é um convite à programação criativa. Ele representa a fusão da estrutura do HTML com o poder lógico do JavaScript, permitindo que você vá além dos documentos e comece a construir verdadeiras experiências interativas diretamente no navegador.
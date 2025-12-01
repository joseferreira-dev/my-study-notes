# Capítulo 4 – Imagens

Se os textos e links formam a base informacional e conectiva da web, as **imagens** são responsáveis por grande parte de seu apelo visual, engajamento e capacidade de comunicação. Elas podem ilustrar um conceito, exibir um produto, definir o tom de uma página ou simplesmente quebrar a monotonia de blocos de texto. O uso eficaz de imagens é uma das chaves para criar experiências de usuário memoráveis e eficientes.

Neste capítulo, vamos nos aprofundar no elemento `<img>`, a principal ferramenta para incorporar conteúdo visual em nossos documentos. Analisaremos em detalhe seus atributos mais críticos, como `src` e `alt`, que definem a fonte da imagem e sua acessibilidade. Discutiremos a importância de dimensionar imagens com `width` e `height` e exploraremos os formatos de arquivo mais comuns na web, entendendo quando usar cada um. Por fim, avançaremos para técnicas mais sofisticadas, como a criação de **mapas de imagem** com áreas clicáveis e o uso do elemento `<picture>` para entregar imagens responsivas e otimizadas para diferentes dispositivos.

Ao final, você saberá não apenas como inserir uma imagem em uma página, mas como fazê-lo de forma performática, acessível e profissional.

## Elemento `<img>`

Para inserir uma imagem em uma página HTML, utilizamos a tag `<img>`. Este é um **elemento vazio**, o que significa que ele não possui uma tag de fechamento. Todo o seu comportamento e conteúdo são definidos através de seus atributos.

A estrutura básica é simples:

```html
<img src="caminho/para/imagem.jpg" alt="Descrição da imagem.">
```

## Atributos `src` e `alt`

Dois atributos são absolutamente indispensáveis para qualquer elemento `<img>`:

1. `src` (Source): Este é o atributo mais importante, pois ele especifica o caminho (URL) para o arquivo de imagem que será exibido. Assim como nos links, o caminho pode ser absoluto ou relativo.
    - **URL Absoluta**: O endereço completo da imagem na web. É usado para exibir uma imagem que está hospedada em outro site.

        ```html
        <img src="https://www.somesite.com/images/logo.png" alt="Logo de outro site.">
        ```

    - **URL Relativa**: O caminho para a imagem a partir da localização do arquivo HTML atual. É a prática padrão para imagens que fazem parte do seu próprio site.

        ```html
        <img src="foto-do-produto.jpg" alt="Foto do produto X.">
        
        <img src="imagens/banner-principal.jpg" alt="Banner da campanha de verão.">
        ```

2. `alt` (Alternative Text): Este atributo é igualmente essencial e nunca deve ser omitido. Ele fornece um texto alternativo para a imagem, que serve a dois propósitos críticos:
    - **Acessibilidade**: Leitores de tela leem o conteúdo do atributo `alt` para usuários com deficiência visual, permitindo que eles compreendam o conteúdo da imagem.
    - **Contingência e SEO**: Se a imagem, por algum motivo, não puder ser carregada (caminho quebrado, conexão lenta), o navegador exibirá o texto alternativo em seu lugar. Além disso, os mecanismos de busca utilizam esse texto para entender e indexar o conteúdo da imagem.

## Dimensionando Imagens com `width` e `height`

Os atributos `width` (largura) e `height` (altura) permitem especificar as dimensões de uma imagem em pixels.

```html
<img src="avatar.jpg" alt="Avatar do usuário" width="150" height="150">
```

É uma **excelente prática** sempre incluir esses atributos. Quando o navegador os encontra, ele reserva o espaço exato para a imagem na página **antes** mesmo de ela ser totalmente carregada. Isso evita que o layout da página "salte" ou se reajuste abruptamente enquanto as imagens carregam, um problema que degrada a experiência do usuário (conhecido como **Cumulative Layout Shift ou CLS**).

## Formatos de Imagem Comuns na Web

A escolha do formato de arquivo correto é um equilíbrio entre qualidade visual e tamanho do arquivo (desempenho). Os formatos mais comuns são:

| **Abreviação** | **Formato do arquivo**             | **Extensão** | **Uso Ideal**                                                                                                                                                            |
| -------------- | ---------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **JPEG**       | Joint Photographic Expert Group    | .jpg, .jpeg  | Ideal para **fotografias** e imagens com muitas cores e gradientes. Usa compressão com perda de dados para gerar arquivos menores.                                       |
| **PNG**        | Portable Network Graphics          | .png         | Perfeito para **logotipos, ícones** e gráficos com linhas nítidas. Suporta **transparência** e usa compressão sem perda de dados.                                        |
| **GIF**        | Graphics Interchange Format        | .gif         | Usado principalmente para **animações simples**. É limitado a uma paleta de 256 cores.                                                                                   |
| **SVG**        | Scalable Vector Graphics           | .svg         | Um formato vetorial baseado em XML. É **infinitamente escalável** sem perda de qualidade, perfeito para logotipos e ícones que precisam ser exibidos em vários tamanhos. |
| **APNG**       | Animated Portable Network Graphics | .apng        | Uma alternativa moderna ao GIF para animações, suportando mais cores e transparência.                                                                                    |
| **ICO**        | Microsoft Icon                     | .ico         | Formato padrão para **ícones**, mais conhecido por seu uso em "favicons" (os ícones que aparecem na aba do navegador).                                                   |

## Técnicas Avançadas

Além da inserção simples, o HTML oferece ferramentas para tornar as imagens mais interativas e adaptáveis.

### Mapa de Imagem (`<map>` e `<area>`)

Um mapa de imagem permite definir múltiplas **áreas clicáveis (hotspots)** em uma única imagem, onde cada área pode ter um link diferente. Para criar um mapa de imagem, usamos três tags em conjunto:

- `<img>`: A imagem base, que deve incluir o atributo `usemap`. O valor desse atributo, precedido por `#`, é o nome que daremos ao nosso mapa.
- `<map>`: O contêiner que define o mapa de imagem. Seu atributo `name` deve corresponder exatamente ao valor especificado em `usemap` (sem o `#`).
- `<area>`: Define cada área clicável dentro do mapa. Seus principais atributos são `shape` (formato: `rect`, `circle`, `poly`), `coords` (as coordenadas da área), `href` (o link de destino) e `alt` (texto alternativo para acessibilidade).

**Exemplo Prático:**

```html
<img src="workplace.jpg" alt="Workplace" usemap="#workmap">

<map name="workmap">
  <area shape="rect" coords="34,44,270,350" alt="Computador" href="computador.html">
  <area shape="rect" coords="290,172,333,250" alt="Celular" href="celular.html">
  <area shape="circle" coords="337,300,44" alt="Xícara de café" href="cafe.html">
</map>
```

### Imagens Responsivas com `<picture>`

Em um design responsivo, às vezes simplesmente redimensionar uma imagem não é o ideal. Para telas pequenas, talvez queiramos exibir uma imagem diferente, mais cortada e focada no assunto principal (uma técnica chamada de **art direction**). O elemento `<picture>` resolve isso.

Ele funciona como um contêiner que oferece várias fontes de imagem ao navegador, permitindo que ele escolha a mais adequada com base em regras definidas (como o tamanho da tela).

```html
<picture>
  <source media="(min-width: 800px)" srcset="imagem-paisagem.jpg">
  
  <source media="(min-width: 500px)" srcset="imagem-quadrada.jpg">
  
  <img src="imagem-vertical.jpg" alt="Uma paisagem montanhosa." style="width:auto;">
</picture>
```

## Considerações Finais

Neste capítulo, vimos que o uso de imagens na web vai muito além de simplesmente "colocar uma foto na página". Aprendemos a importância crítica dos atributos `src` e `alt` para a funcionalidade e acessibilidade, e como `width` e `height` podem melhorar a performance de carregamento. Exploramos os diferentes formatos de arquivo, entendendo que a escolha correta impacta diretamente a velocidade e a qualidade visual do nosso site.

Além do básico, introduzimos técnicas avançadas como mapas de imagem, que adicionam uma camada de interatividade, e o elemento `<picture>`, uma ferramenta poderosa para o design responsivo moderno. O uso correto e consciente desses elementos garante que nossas páginas não sejam apenas visualmente agradáveis, mas também rápidas, acessíveis e adaptáveis a qualquer dispositivo.

Agora que sabemos como trabalhar com os dois principais tipos de conteúdo – texto e imagens – estamos prontos para explorar formas de organizar informações mais estruturadas. No próximo capítulo, mergulharemos no mundo das **tabelas**.
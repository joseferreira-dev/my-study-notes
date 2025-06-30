# Capítulo 47 – Padrões de Projeto Estruturais: Compondo Objetos e Classes

No capítulo anterior, mergulhamos nos Padrões de Projeto Criacionais, dominando as técnicas para abstrair e controlar o processo de instanciação de objetos. Agora que sabemos como criar nossos objetos de forma flexível, enfrentamos o próximo desafio arquitetural: como podemos organizar esses objetos e classes em estruturas maiores, sem criar um sistema rígido e monolítico? Como podemos fazer com que partes do nosso sistema, que não foram projetadas para trabalhar juntas, possam colaborar? Como podemos adicionar novas funcionalidades a um objeto sem alterar sua estrutura fundamental?

É para responder a essas perguntas que existe a segunda grande categoria de padrões de projeto: os **Padrões Estruturais**. Enquanto os padrões criacionais lidam com o **nascimento** dos objetos, os padrões estruturais lidam com suas **relações**. Eles se concentram em como objetos e classes podem ser compostos para formar estruturas maiores e mais complexas, ao mesmo tempo em que mantêm essa estrutura flexível e eficiente. Eles são os projetos para as vigas, paredes e conexões do nosso "edifício" de software.

Neste capítulo, vamos explorar sete dos mais influentes padrões estruturais. Começaremos com o **Adapter**, que atua como um tradutor universal, permitindo que interfaces incompatíveis trabalhem juntas. Em seguida, desvendaremos o **Decorator**, que nos permite "embrulhar" objetos para adicionar novas funcionalidades dinamicamente. Exploraremos o **Facade**, que fornece uma "porta da frente" simplificada para um sistema complexo, e o **Proxy**, que atua como um intermediário para controlar o acesso a um objeto. Mergulharemos também em padrões que lidam com a composição de árvores de objetos, como o **Composite**, e em padrões de otimização de memória, como o **Flyweight**. Finalmente, analisaremos o padrão **Bridge**, uma solução elegante para desacoplar uma abstração de sua implementação. Ao final, você terá um novo conjunto de ferramentas para arquitetar as relações dentro do seu software, tornando-o mais modular, flexível e fácil de manter.

## Padrão Adapter (Adaptador)

O padrão **Adapter** resolve um problema extremamente comum no desenvolvimento de software: a necessidade de fazer com que duas classes com interfaces incompatíveis trabalhem juntas. Imagine que você tem um componente em seu sistema que foi projetado para interagir com um objeto que possui um método `conectar()`. Agora, você precisa integrar uma nova biblioteca de terceiros fantástica, mas o objeto dela expõe um método com a mesma finalidade chamado `iniciarConexao()`. Alterar todo o seu código cliente para se adequar à nova biblioteca seria impraticável e criaria um forte acoplamento.

A solução do Adapter é atuar como uma ponte, um tradutor. Ele cria um objeto "invólucro" (wrapper) que implementa a interface que o seu cliente espera (a interface com o método `conectar()`). Internamente, porém, quando o método `conectar()` do adaptador é chamado, ele traduz essa chamada para o método correspondente do objeto adaptado, ou seja, `iniciarConexao()`. É o equivalente a um adaptador de tomada universal que permite que seu plugue brasileiro se encaixe perfeitamente em uma tomada europeia, sem que o seu aparelho precise saber da diferença.

Vejamos um exemplo prático em JavaScript. Suponha que temos um sistema novo que consome dados em JSON, mas precisamos integrar uma API antiga que só retorna XML.

```js
// A API "antiga" e incompatível que retorna XML.
class OldApi {
  fetchDataXML() {
    console.log("OldApi: Buscando dados em formato XML...");
    return '<data><item>Primeiro Item</item></data>';
  }
}

// O nosso código cliente espera uma interface moderna com um método 'fetchDataJson()'.
function exibirDados(servicoDeDados) {
  console.log("Cliente: Solicitando dados JSON...");
  const dados = servicoDeDados.fetchDataJson();
  console.log("Cliente: Dados recebidos e prontos para uso:", dados);
}

// O Adaptador que fará a ponte entre as duas interfaces.
class ApiAdapter {
  constructor(oldApi) {
    this.oldApi = oldApi;
  }

  // Implementa a interface moderna que o cliente espera.
  fetchDataJson() {
    // Internamente, ele chama o método da API antiga.
    const xmlData = this.oldApi.fetchDataXML();
    console.log("Adaptador: Convertendo o XML recebido para JSON...");
    // Aqui entraria uma lógica real de conversão de XML para JSON.
    const jsonData = { data: { item: "Primeiro Item" } };
    return jsonData;
  }
}

// No nosso código principal, instanciamos a API antiga e a "embrulhamos" com o adaptador.
const apiAntiga = new OldApi();
const adaptador = new ApiAdapter(apiAntiga);

// O cliente interage com o adaptador, sem nunca saber que por trás existe uma API XML.
exibirDados(adaptador);
```

Este padrão é indispensável sempre que você precisa integrar código legado, bibliotecas de terceiros ou quaisquer dois sistemas que não foram projetados para conversar diretamente, permitindo a colaboração sem modificar o código existente.

## Padrão Decorator (Decorador)

O padrão **Decorator** aborda a necessidade de adicionar novas responsabilidades ou comportamentos a um objeto dinamicamente, em tempo de execução, sem alterar sua classe original. A herança é uma forma de estender a funcionalidade, mas ela é estática — definida em tempo de compilação — e afeta todas as instâncias da subclasse. O Decorator oferece uma alternativa flexível, permitindo que você adicione funcionalidades a objetos individuais.

A solução do Decorator é "embrulhar" (wrap) um objeto em outros objetos "decoradores". Esses decoradores compartilham a mesma interface do objeto original, o que os torna transparentes para o cliente. Quando um método é chamado no decorador, ele pode adicionar seu próprio comportamento antes ou depois de delegar a chamada para o objeto que ele embrulha. Pense nisso como vestir uma pessoa (o objeto) com roupas (os decoradores): um casaco adiciona a funcionalidade de aquecer, um chapéu adiciona proteção solar, e você pode combinar essas "decorações" como quiser.

Vamos ilustrar com um exemplo clássico de uma cafeteria, onde começamos com um café simples e o decoramos com adicionais.

```js
// O componente base que define a interface.
class Cafe {
  custo() { return 5; }
  descricao() { return "Café simples"; }
}

// O Decorador base abstrato. Ele mantém uma referência ao objeto que decora.
// Em JavaScript, a herança aqui ajuda a garantir a conformidade da interface.
class CafeDecorator extends Cafe {
  constructor(cafe) {
    super();
    this.cafe = cafe;
  }

  custo() {
    return this.cafe.custo();
  }

  descricao() {
    return this.cafe.descricao();
  }
}

// Decoradores concretos que adicionam custo e descrição.
class ComLeite extends CafeDecorator {
  custo() {
    return this.cafe.custo() + 2;
  }

  descricao() {
    return this.cafe.descricao() + ", com leite";
  }
}

class ComAcucar extends CafeDecorator {
  custo() {
    return this.cafe.custo() + 1;
  }

  descricao() {
    return this.cafe.descricao() + ", com açúcar";
  }
}

// O cliente começa com um objeto base.
let meuCafe = new Cafe();
console.log(`${meuCafe.descricao()} custa R$ ${meuCafe.custo()}`);

// Agora, decoramos o objeto dinamicamente.
meuCafe = new ComLeite(meuCafe); // Embrulha o objeto 'meuCafe' original.
console.log(`${meuCafe.descricao()} custa R$ ${meuCafe.custo()}`);

meuCafe = new ComAcucar(meuCafe); // Embrulha o objeto já decorado com leite.
console.log(`${meuCafe.descricao()} custa R$ ${meuCafe.custo()}`);
```

O padrão Decorator é ideal quando você precisa de uma alternativa flexível à herança para estender funcionalidades, evitando uma explosão de subclasses para cada combinação possível. É comumente usado em APIs de I/O (como em Java) e em frameworks de UI para adicionar bordas, sombras ou outros efeitos a componentes.

## Padrão Facade (Fachada)

O padrão **Facade** lida com o problema da complexidade. Muitas vezes, ao trabalhar com bibliotecas ou subsistemas grandes, nos deparamos com um conjunto de classes e métodos interdependentes que precisam ser inicializados e orquestrados na ordem correta. O código cliente que precisa usar esse sistema acaba tendo que conhecer e gerenciar toda essa complexidade, tornando-se frágil, difícil de entender e fortemente acoplado aos detalhes de implementação do subsistema.

A solução do Facade é fornecer uma **interface unificada e simplificada** para esse conjunto de interfaces. A fachada define um método de nível mais alto que encapsula uma série de chamadas ao subsistema para realizar uma tarefa comum. Ela não esconde a complexidade — o cliente ainda pode acessar as partes do subsistema diretamente se precisar de um controle mais fino —, mas oferece uma "porta da frente" conveniente e fácil de usar.

Um ótimo exemplo é a fachada de um sistema de home theater, que simplifica a tarefa de "assistir a um filme".

```js
// O subsistema complexo, com várias partes independentes.
class Luzes { acender() { console.log('Luzes acesas'); } apagar() { console.log('Luzes apagadas'); } }
class Projetor { ligar() { console.log('Projetor ligado'); } }
class SistemaDeSom { ligar() { console.log('Sistema de som ligado'); } setVolume(n) { console.log(`Volume definido para ${n}`); } }
class PlayerDeBluRay { ligar() { console.log('Player de Blu-ray ligado'); } play() { console.log('Filme rodando'); } }

// A Fachada, que conhece a ordem correta de operações.
class HomeTheaterFacade {
  constructor(luzes, projetor, som, player) {
    this.luzes = luzes;
    this.projetor = projetor;
    this.som = som;
    this.player = player;
  }

  // O método simplificado que orquestra todo o subsistema.
  assistirFilme() {
    console.log("Preparando tudo para o filme...");
    this.luzes.apagar();
    this.projetor.ligar();
    this.som.ligar();
    this.som.setVolume(11);
    this.player.ligar();
    this.player.play();
  }
}

// O cliente interage apenas com a fachada, sem se preocupar com os detalhes.
const facade = new HomeTheaterFacade(new Luzes(), new Projetor(), new SistemaDeSom(), new PlayerDeBluRay());
facade.assistirFilme();
```

Este padrão é usado sempre que você quer fornecer uma interface simples para um sistema complexo. A biblioteca jQuery, em seus primórdios, foi um exemplo clássico de uma fachada que simplificava as complexas e inconsistentes APIs do DOM dos diferentes navegadores.

## Padrão Proxy

O padrão **Proxy** resolve a necessidade de controlar o acesso a um objeto. Em vez de o cliente interagir diretamente com o objeto real, ele interage com um objeto substituto, o proxy. Este proxy possui a mesma interface do objeto real, tornando-se transparente para o cliente. Isso permite que o proxy intercepte as chamadas, adicione lógica extra e, em seguida, delegue a chamada para o objeto real.

Existem vários motivos para usar um Proxy:

- **Virtual Proxy (Lazy Loading):** Adiar a criação de um objeto caro até que ele seja realmente necessário.
- **Protection Proxy:** Controlar o acesso com base em permissões.
- **Remote Proxy:** Representar um objeto que está em um espaço de endereço diferente (ex: em um servidor remoto).
- **Logging/Caching Proxy:** Registrar chamadas de método ou armazenar em cache os resultados de operações custosas.

Vamos ver um exemplo de um Virtual Proxy para carregar uma imagem pesada apenas quando ela for exibida.

```js
// A interface comum que tanto o objeto real quanto o proxy implementarão.
class Imagem {
  exibir() {}
}

// O objeto real, cuja criação é "cara" (simulada pelo log).
class ImagemReal extends Imagem {
  constructor(filename) {
    super();
    this.filename = filename;
    this.carregarDoDisco();
  }
  carregarDoDisco() {
    console.log(`Carregando imagem pesada do disco: ${this.filename}...`);
  }
  exibir() {
    console.log(`Exibindo imagem: ${this.filename}`);
  }
}

// O Proxy que controla o acesso à ImagemReal.
class ProxyImagem extends Imagem {
  constructor(filename) {
    super();
    this.filename = filename;
    this.imagemReal = null; // A imagem real ainda não foi criada/carregada.
  }

  exibir() {
    // Criação preguiçosa (Lazy Initialization): o objeto real só é criado na primeira chamada.
    if (this.imagemReal === null) {
      console.log("Proxy: Instanciando a ImagemReal agora.");
      this.imagemReal = new ImagemReal(this.filename);
    }
    // Após a criação (ou se já existir), a chamada é delegada.
    this.imagemReal.exibir();
  }
}

// Uso pelo cliente
const imagem = new ProxyImagem("foto_ferias.jpg");
console.log("Objeto proxy criado, mas a imagem real ainda não foi carregada.");
imagem.exibir(); // Neste ponto, a ImagemReal é criada e carregada.
console.log("---");
imagem.exibir(); // Agora, a ImagemReal já existe e o método é apenas delegado.
```

O JavaScript moderno possui um objeto `Proxy` nativo que torna a implementação deste padrão incrivelmente poderosa, permitindo interceptar quase todas as operações em um objeto (get, set, delete, etc.), tornando-o uma ferramenta fundamental para metaprogramação.

## Padrão Composite (Composto)

O padrão **Composite** aborda o problema de tratar um grupo de objetos da mesma forma que se trata um único objeto. Ele permite compor objetos em estruturas de árvore para representar hierarquias parte-todo. A ideia central é ter uma interface comum para objetos individuais (as "folhas" da árvore) e para composições de objetos (os "galhos" ou "nós compostos").

Isso permite que o código cliente ignore a diferença entre objetos individuais e coleções, simplificando seu código. Imagine uma estrutura de arquivos: você pode querer calcular o tamanho de um único arquivo ou o tamanho total de uma pasta (que contém arquivos e outras pastas). O Composite permite que você chame um método `calcularTamanho()` tanto em um arquivo quanto em uma pasta, de maneira uniforme.

Vamos aplicar isso a um sistema de componentes gráficos.

```js
// O componente "interface" comum que todos os objetos (folhas e compostos) implementarão.
class ComponenteGrafico {
  renderizar() { throw new Error("Método renderizar deve ser implementado"); }
  // Em uma implementação completa, métodos como adicionar, remover, etc., estariam aqui.
}

// Objetos "Folha" (Leaf) - os elementos individuais que não podem ter filhos.
class Paragrafo extends ComponenteGrafico {
  constructor(texto) { super(); this.texto = texto; }
  renderizar() { console.log(`<p>${this.texto}</p>`); }
}
class Imagem extends ComponenteGrafico {
  constructor(src) { super(); this.src = src; }
  renderizar() { console.log(`<img src="${this.src}" />`); }
}

// O objeto "Composto" (Composite) - o contêiner que pode ter filhos.
class DivContainer extends ComponenteGrafico {
  constructor() {
    super();
    this.filhos = [];
  }
  adicionar(componente) { this.filhos.push(componente); }
  renderizar() {
    console.log("<div>");
    // Delega a renderização para cada filho, recursivamente.
    for (const filho of this.filhos) {
      filho.renderizar();
    }
    console.log("</div>");
  }
}

// Uso pelo cliente
const container = new DivContainer();
container.adicionar(new Paragrafo("Primeiro parágrafo."));
container.adicionar(new Imagem("imagem.jpg"));

const subContainer = new DivContainer();
subContainer.adicionar(new Paragrafo("Parágrafo aninhado."));
container.adicionar(subContainer);

// O cliente chama renderizar() no container de topo, e tudo é renderizado uniformemente.
container.renderizar();
```

## Padrão Bridge (Ponte)

O padrão **Bridge** é um padrão sofisticado que visa desacoplar uma abstração de sua implementação, de modo que as duas possam evoluir independentemente. Isso é útil quando você tem uma hierarquia de classes de abstração e uma hierarquia separada de classes de implementação, e quer evitar uma explosão de subclasses para cada combinação possível.

**O Problema:** Você tem diferentes formas geométricas (Círculo, Quadrado) e diferentes APIs de renderização (Canvas API, SVG API). Uma abordagem ingênua seria criar subclasses para cada combinação: `CirculoCanvas`, `CirculoSVG`, `QuadradoCanvas`, `QuadradoSVG`. Se você adicionar uma nova forma (Triângulo) ou uma nova API (WebGL), o número de classes cresce exponencialmente.

**A Solução:** O Bridge cria duas hierarquias separadas: uma para as abstrações (as Formas) e outra para as implementações (as APIs de Renderização). A abstração _contém uma referência_ à implementação (em vez de herdar dela), criando uma "ponte" entre as duas.

```js
// A hierarquia de Implementação (os "Implementadores")
class ApiDeRenderizacao { desenharCirculo(raio, x, y) {} }
class CanvasAPI extends ApiDeRenderizacao { desenharCirculo(raio, x, y) { console.log(`Canvas: Desenhando um círculo com raio ${raio} em (${x},${y}).`); } }
class SvgAPI extends ApiDeRenderizacao { desenharCirculo(raio, x, y) { console.log(`SVG: Desenhando um círculo com raio ${raio} em (${x},${y}).`); } }

// A hierarquia de Abstração
class Forma {
  constructor(apiRenderizacao) {
    this.apiRenderizacao = apiRenderizacao; // A ponte!
  }
  desenhar() {}
}

// Abstrações Refinadas
class Circulo extends Forma {
  constructor(x, y, raio, apiRenderizacao) {
    super(apiRenderizacao);
    this.x = x;
    this.y = y;
    this.raio = raio;
  }
  desenhar() {
    // Delega o trabalho de implementação para o objeto da ponte
    this.apiRenderizacao.desenharCirculo(this.raio, this.x, this.y);
  }
}

// Uso pelo cliente
const circuloCanvas = new Circulo(10, 20, 5, new CanvasAPI());
circuloCanvas.desenhar(); // "Canvas: Desenhando um círculo com raio 5 em (10,20)."

const circuloSVG = new Circulo(50, 60, 10, new SvgAPI());
circuloSVG.desenhar(); // "SVG: Desenhando um círculo com raio 10 em (50,60)."
```

Agora, você pode adicionar novas formas ou novas APIs de renderização de forma independente, sem que uma afete a outra.

## Padrão Flyweight (Peso-Leve)

O **Flyweight** é um padrão de otimização que visa minimizar o uso de memória ou o custo computacional ao compartilhar o máximo de dados possível com outros objetos semelhantes. Ele é aplicável quando uma aplicação precisa criar um número enorme de objetos que compartilham parte de seu estado.

A solução é separar o estado de um objeto em dois tipos:

- **Estado Intrínseco:** É o estado que é imutável e compartilhado entre muitos objetos (ex: a textura de uma árvore, o modelo 3D de um soldado). Este estado é armazenado no objeto Flyweight.
- **Estado Extrínseco:** É o estado que é único para cada objeto e depende do contexto (ex: a posição x,y de uma árvore, a vida atual de um soldado). Este estado é passado para os métodos do Flyweight pelo cliente.

Uma fábrica é geralmente usada para gerenciar a criação e o compartilhamento dos objetos Flyweight.

**Exemplo em JavaScript:** Renderizando uma floresta com milhões de árvores.

```js
// O Flyweight: contém o estado intrínseco (compartilhado e imutável).
class TipoDeArvore {
  constructor(modelo, textura) {
    this.modelo = modelo;
    this.textura = textura;
  }
  // O estado extrínseco é passado como argumento.
  desenhar(canvas, x, y) {
    console.log(`Desenhando um ${this.modelo} em (${x},${y}) usando a textura ${this.textura}.`);
  }
}

// A Fábrica de Flyweights: gerencia e reutiliza os flyweights.
class FabricaDeArvores {
  constructor() {
    this.tiposDeArvore = {};
  }
  getTipoDeArvore(modelo, textura) {
    const chave = `${modelo}_${textura}`;
    if (!(chave in this.tiposDeArvore)) {
      console.log(`Fábrica: Criando novo tipo de árvore (flyweight): ${chave}`);
      this.tiposDeArvore[chave] = new TipoDeArvore(modelo, textura);
    }
    return this.tiposDeArvore[chave];
  }
}

// O Contexto: contém o estado extrínseco (único).
class Arvore {
  constructor(x, y, tipo) {
    this.x = x;
    this.y = y;
    this.tipo = tipo; // Referência para o objeto flyweight compartilhado.
  }
  desenhar(canvas) {
    this.tipo.desenhar(canvas, this.x, this.y);
  }
}

// Uso
const fabrica = new FabricaDeArvores();
const floresta = [];

// Criamos 1000 árvores, mas apenas 2 objetos TipoDeArvore (flyweights).
for (let i = 0; i < 500; i++) {
  floresta.push(new Arvore(Math.random() * 500, Math.random() * 500, fabrica.getTipoDeArvore('pinheiro', 'pinheiro.png')));
  floresta.push(new Arvore(Math.random() * 500, Math.random() * 500, fabrica.getTipoDeArvore('carvalho', 'carvalho.png')));
}
```

Este padrão economiza uma quantidade massiva de memória ao evitar a duplicação do estado intrínseco.

## Considerações Finais

Neste capítulo, exploramos os **Padrões de Projeto Estruturais**, um conjunto de soluções comprovadas para organizar e compor objetos e classes em estruturas maiores e mais flexíveis. Vimos que esses padrões não são sobre criar objetos, mas sobre gerenciar as **relações** entre eles.

Aprendemos a fazer interfaces incompatíveis colaborarem com o **Adapter**, a adicionar funcionalidades dinamicamente com o **Decorator**, e a simplificar sistemas complexos com a **Facade**. Desvendamos como o **Composite** nos permite tratar objetos individuais e coleções de forma uniforme e como o **Proxy** pode controlar o acesso a outros objetos. Finalmente, exploramos padrões mais avançados como o **Bridge**, para um desacoplamento profundo, e o **Flyweight**, para otimização de memória.

Dominar os padrões estruturais é o que permite que um desenvolvedor construa sistemas que não são apenas funcionais, mas também robustos, flexíveis e fáceis de se raciocinar sobre. Eles são o mapa para construir castelos a partir de blocos de Lego, garantindo que as peças se encaixem da maneira mais elegante e eficiente possível.
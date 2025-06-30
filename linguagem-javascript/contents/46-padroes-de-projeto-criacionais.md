# Capítulo 46 – Padrões de Projeto Criacionais: Dominando a Criação de Objetos

Ao longo de nossa extensa jornada pelo JavaScript, construímos um arsenal formidável de ferramentas. Dominamos a sintaxe da linguagem, a manipulação do DOM, a comunicação assíncrona e a arquitetura de módulos. Agora, estamos prontos para transcender a escrita de código funcional e adentrar o domínio da **arquitetura de software**. Escrever código que funciona é uma coisa; escrever código que é flexível, manutenível, escalável e fácil de ser compreendido por outros desenvolvedores é um desafio de outra magnitude. É aqui que entram os **Padrões de Projeto (Design Patterns)**.

Um Padrão de Projeto não é um algoritmo, uma biblioteca ou um pedaço de código que você pode simplesmente copiar e colar. É uma **solução geral e reutilizável para um problema comum** que ocorre dentro de um determinado contexto no design de software. Eles são como as plantas baixas de um arquiteto ou as receitas de um chef: um guia testado e comprovado que pode ser adaptado para resolver seu problema específico. Esses padrões são o resultado de décadas de experiência coletiva, catalogados para nos ajudar a evitar reinventar a roda e a construir sistemas sobre ombros de gigantes.

Os padrões de projeto são geralmente divididos em três categorias: **Criacionais**, **Estruturais** e **Comportamentais**. Neste capítulo, focaremos na primeira e mais fundamental categoria: os **Padrões Criacionais**. Estes padrões lidam com o processo de **criação de objetos**. Eles visam abstrair a lógica de instanciação, tornando o sistema independente de como seus objetos são criados, compostos e representados. Em vez de acoplar nosso código diretamente a uma chamada de construtor `new MinhaClasse()`, os padrões criacionais nos dão o poder de delegar essa responsabilidade, resultando em um software drasticamente mais flexível e dinâmico.

## Por que se Preocupar com a Criação de Objetos?

A instanciação direta com `new` é simples e eficaz, mas tem uma desvantagem significativa: ela **acopla** o código cliente à implementação concreta da classe. O código que precisa de um objeto fica diretamente responsável por saber qual classe específica instanciar e como fazê-lo.

**O Problema:** Imagine uma aplicação de UI que precisa criar um botão.

```js
// Código cliente fortemente acoplado
import BotaoWindows from './componentes/BotaoWindows.js';

// Em algum lugar da nossa aplicação...
const meuBotao = new BotaoWindows({ label: 'Clique aqui', cor: 'azul' });
meuBotao.renderizar();
```

Este código funciona perfeitamente, mas está "cimentado" na ideia de que sempre usaremos um `BotaoWindows`. E se, no futuro, quiséssemos que nossa aplicação também rodasse no macOS e precisasse de um `BotaoMacOS`? Ou se quiséssemos introduzir um `BotaoLinux`? Teríamos que encontrar todas as instâncias de `new BotaoWindows()` em nosso código e substituí-las por uma lógica condicional:

```js
// Lógica condicional espalhada pelo código (um anti-padrão)
let meuBotao;
if (sistemaOperacional === 'windows') {
  meuBotao = new BotaoWindows({ label: 'Clique aqui', cor: 'azul' });
} else if (sistemaOperacional === 'macos') {
  meuBotao = new BotaoMacOS({ label: 'Clique aqui', cor: 'grafite' });
}
```

Isso é insustentável. Os padrões criacionais resolvem esse problema, introduzindo uma camada de abstração. Em vez de o cliente dizer "me dê um **novo BotaoWindows**", ele diz a uma entidade intermediária (uma fábrica): "me dê um **botão**". A fábrica, então, se encarrega de decidir qual tipo específico de botão criar com base no contexto atual (como o sistema operacional), escondendo essa complexidade do cliente.

## Padrão Factory (Fábrica)

O padrão Factory é um dos padrões criacionais mais comuns e úteis. Sua principal responsabilidade é criar objetos, encapsulando a lógica de qual objeto criar e como criá-lo. Isso desacopla o código cliente das classes concretas.

### Factory Functions (Funções Fábrica)

A forma mais simples, flexível e idiomática do padrão em JavaScript é a **Factory Function**. É simplesmente uma função que não é uma classe nem um construtor, e que retorna um novo objeto. Ela abstrai o processo de criação e não exige o uso da palavra-chave `new`.

```js
// Uma função fábrica para criar objetos de usuário
function criarUsuario(nome, email, nivelAcesso = 'leitor') {
  // A lógica de criação está encapsulada aqui.
  // Poderíamos ter validações ou configurações complexas.
  const dataCriacao = new Date();

  return {
    nome,
    email,
    nivelAcesso,
    dataCriacao,
    exibirInfo() {
      console.log(`${this.nome} (${this.email}) - Nível: ${this.nivelAcesso}`);
    }
  };
}

const usuario1 = criarUsuario('Ana', 'ana@example.com');
const usuario2 = criarUsuario('Carlos', 'carlos@example.com', 'admin');

usuario1.exibirInfo(); // "Ana (ana@example.com) - Nível: leitor"
usuario2.exibirInfo(); // "Carlos (carlos@example.com) - Nível: admin"
```

**Vantagens Detalhadas:**

- **Encapsulamento e Privacidade:** A lógica de criação do objeto está contida em um único local. Além disso, podemos usar closures para criar dados privados, algo que as classes tradicionais só recentemente ganharam com campos privados (`#`).
- **Flexibilidade:** A função pode conter lógica condicional para retornar diferentes "tipos" ou "formatos" de objetos com base nos parâmetros de entrada, sem que o cliente precise saber disso.
- **Sem `new`:** Evita as complexidades e os erros comuns associados à palavra-chave `new` e ao contexto `this` em JavaScript. A função é apenas uma função, tornando o código mais simples de entender e testar.

### Factory Method Pattern

O Factory Method é o padrão formal, como definido pelo "Gang of Four" (GoF). É um padrão mais estruturado, geralmente implementado com classes. Ele define uma interface para criar um objeto, mas deixa que as **subclasses** decidam qual classe concreta instanciar.

**O Problema:** Uma empresa de logística precisa gerenciar entregas, mas o meio de transporte (caminhão, navio) varia dependendo do tipo de logística (terrestre, marítima). O código principal que planeja a entrega não deve se preocupar com qual tipo de veículo está sendo usado.

**A Solução:**

1. **Produto (Product):** Define a interface para os objetos que o método fábrica cria (ex: `Transporte`).
2. **Produto Concreto (Concrete Product):** Implementações da interface do produto (ex: `Caminhao`, `Navio`).
3. **Criador (Creator):** Declara o método fábrica (`factoryMethod`), que retorna um objeto do tipo Produto. O Criador também pode definir uma implementação padrão do método fábrica.
4. **Criador Concreto (Concrete Creator):** Sobrescreve o método fábrica para retornar uma instância de um Produto Concreto.

**Cenário:** Uma empresa de logística que usa diferentes meios de transporte.

```js
// A "Interface" do produto
class Transporte {
  entregar() {
    throw new Error("O método 'entregar' deve ser implementado.");
  }
}

// Produtos concretos
class Caminhao extends Transporte {
  entregar() {
    console.log("Entrega sendo realizada por via terrestre em um caminhão.");
  }
}
class Navio extends Transporte {
  entregar() {
    console.log("Entrega sendo realizada por via marítima em um navio.");
  }
}

// O "Criador" abstrato com o Factory Method
class Logistica {
  // O Factory Method que as subclasses devem implementar
  criarTransporte() {
    throw new Error("O método 'criarTransporte' deve ser implementado por uma subclasse.");
  }

  // A lógica de negócio que usa o produto criado pela fábrica.
  planejarEntrega() {
    console.log("Iniciando planejamento da entrega...");
    const transporte = this.criarTransporte();
    transporte.entregar();
    console.log("Planejamento concluído.");
  }
}

// Criadores concretos que implementam o Factory Method
class LogisticaTerrestre extends Logistica {
  criarTransporte() {
    return new Caminhao();
  }
}
class LogisticaMaritima extends Logistica {
  criarTransporte() {
    return new Navio();
  }
}

// Uso pelo cliente
console.log("Cliente precisa de uma entrega terrestre:");
const logistica1 = new LogisticaTerrestre();
logistica1.planejarEntrega();

console.log("\nCliente precisa de uma entrega marítima:");
const logistica2 = new LogisticaMaritima();
logistica2.planejarEntrega();
```

O código cliente (`logistica1.planejarEntrega()`) não sabe qual tipo de transporte está sendo criado; ele apenas confia que a fábrica correta (`LogisticaTerrestre`) produzirá o objeto apropriado. Isso desacopla o cliente das classes `Caminhao` e `Navio`.

## Padrão Abstract Factory (Fábrica Abstrata)

Este padrão é um nível acima do Factory Method. Ele é uma "fábrica de fábricas". Seu propósito é fornecer uma interface para criar **famílias de objetos relacionados ou dependentes** sem especificar suas classes concretas.

**O Problema:** Uma aplicação precisa renderizar uma interface de usuário (UI) que deve ter uma aparência consistente com o sistema operacional do usuário. Um botão no Windows deve parecer um botão do Windows, e uma janela no macOS deve parecer uma janela do macOS. Precisamos de uma forma de criar um conjunto de widgets (botão, janela, caixa de seleção) que pertençam à mesma família visual, sem que o código cliente precise saber qual família está em uso.

**A Solução:**

1. **Fábrica Abstrata (Abstract Factory):** Uma interface para criar os produtos abstratos (ex: `UIFactory` com métodos `criarBotao` e `criarJanela`).
2. **Fábrica Concreta (Concrete Factory):** Implementa a interface para criar uma família específica de produtos (ex: `WindowsUIFactory`, `MacOSUIFactory`).
3. **Produto Abstrato (Abstract Product):** Interfaces para os produtos individuais (ex: `Botao`, `Janela`).
4. **Produto Concreto (Concrete Product):** Implementações dos produtos que pertencem a uma família (ex: `BotaoWindows`, `BotaoMacOS`).

**Cenário:** Uma aplicação que precisa renderizar componentes de UI que devem corresponder ao sistema operacional (Windows, macOS).

```js
// Família de Produtos Abstratos
class Botao { renderizar() {} }
class Janela { renderizar() {} }

// Família de Produtos Concretos para Windows
class BotaoWindows extends Botao { renderizar() { console.log("Renderizando botão estilo Windows."); } }
class JanelaWindows extends Janela { renderizar() { console.log("Renderizando janela estilo Windows."); } }

// Família de Produtos Concretos para macOS
class BotaoMacOS extends Botao { renderizar() { console.log("Renderizando botão estilo macOS."); } }
class JanelaMacOS extends Janela { renderizar() { console.log("Renderizando janela estilo macOS."); } }

// Fábrica Abstrata
class UIFactory {
  criarBotao() {}
  criarJanela() {}
}

// Fábricas Concretas
class WindowsUIFactory extends UIFactory {
  criarBotao() { return new BotaoWindows(); }
  criarJanela() { return new JanelaWindows(); }
}
class MacOSUIFactory extends UIFactory {
  criarBotao() { return new BotaoMacOS(); }
  criarJanela() { return new JanelaMacOS(); }
}

// Uso pelo cliente
function renderizarUI(factory) {
  console.log("Renderizando a UI com a fábrica fornecida...");
  const botao = factory.criarBotao();
  const janela = factory.criarJanela();
  botao.renderizar();
  janela.renderizar();
}

// O cliente pode ser configurado com qualquer fábrica da família
const os = 'windows'; // Poderia vir de uma verificação do sistema
let fabrica;
if (os === 'windows') {
  fabrica = new WindowsUIFactory();
} else {
  fabrica = new MacOSUIFactory();
}

renderizarUI(fabrica);
```

## Padrão Builder (Construtor)

O Builder é usado para separar a construção de um objeto complexo de sua representação, permitindo que o mesmo processo de construção possa criar diferentes representações. É ideal para objetos que exigem muitos parâmetros de configuração no construtor.

**O Problema (Anti-padrão "Telescoping Constructor"):**

```js
class Pedido {
  constructor(cliente, itens, tipoEntrega, desconto, embalagemPresente) {
    // ... construtor com muitos parâmetros, alguns opcionais.
    // Isso é difícil de ler e propenso a erros.
  }
}
// new Pedido('Ana', [], 'normal', 0, false); // O que cada parâmetro significa?
```

**A Solução:** O Builder nos permite construir o objeto passo a passo, usando métodos descritivos, e só obter o objeto final quando ele estiver pronto.

**Cenário:** Construir uma configuração de um objeto `Pedido`.

```js
class Pedido {
  constructor() {
    this.itens = [];
    this.cliente = null;
    this.tipoEntrega = 'normal';
    this.desconto = 0;
  }
  toString() {
    return JSON.stringify(this, null, 2);
  }
}

class PedidoBuilder {
  constructor() {
    this.pedido = new Pedido();
  }

  paraCliente(cliente) {
    this.pedido.cliente = cliente;
    return this; // Permite o encadeamento de métodos (fluent interface)
  }

  comItem(item, quantidade) {
    this.pedido.itens.push({ item, quantidade });
    return this;
  }

  comEntregaExpressa() {
    this.pedido.tipoEntrega = 'expressa';
    return this;
  }

  comDesconto(percentual) {
    this.pedido.desconto = percentual;
    return this;
  }

  // O passo final que retorna o objeto completo
  construir() {
    // Validações finais podem ser feitas aqui
    if (!this.pedido.cliente) {
      throw new Error("Um cliente é obrigatório para o pedido.");
    }
    return this.pedido;
  }
}

// Uso pelo cliente
const pedidoComplexo = new PedidoBuilder()
  .paraCliente('Ana')
  .comItem('Laptop', 1)
  .comItem('Mouse', 1)
  .comEntregaExpressa()
  .comDesconto(10)
  .construir();

console.log(pedidoComplexo.toString());
```

O Builder torna a criação de objetos complexos muito mais legível, flexível e menos propensa a erros.

## Padrão Prototype (Protótipo)

O padrão Prototype especifica os tipos de objetos a serem criados usando uma instância prototípica e cria novos objetos copiando este protótipo. É útil quando a criação de um objeto é custosa (ex: requer uma chamada de rede ou um cálculo pesado), então é mais eficiente clonar um objeto pré-construído.

Como vimos nos capítulos anteriores, o JavaScript tem este padrão em seu núcleo. O método `Object.create()` é uma implementação direta do Prototype Pattern.

```js
const carroPrototipo = {
  rodas: 4,
  portas: 4,
  motor: 'V8',
  ligar() {
    console.log("Motor ligado.");
  }
};

// Cria um novo carro que herda as propriedades do protótipo
const meuCarro = Object.create(carroPrototipo);
meuCarro.modelo = 'Mustang'; // Adiciona uma propriedade específica

// Cria outro carro, clonando o mesmo protótipo
const seuCarro = Object.create(carroPrototipo);
seuCarro.modelo = 'Camaro';

console.log(meuCarro.rodas); // 4 (herdado)
meuCarro.ligar(); // "Motor ligado." (herdado)
console.log(seuCarro.modelo); // "Camaro"
```

## Padrão Singleton

O padrão Singleton garante que uma classe tenha **apenas uma instância** e fornece um ponto de acesso global a essa instância.

**O Problema:** Certos componentes de um sistema devem existir apenas uma vez. Exemplos incluem um serviço de logging, um objeto de configuração que lê um arquivo `.env`, ou uma conexão com um banco de dados. Criar múltiplas instâncias desses componentes seria um desperdício de recursos e poderia levar a um estado inconsistente.

**A Solução Clássica:**

```js
class Configuracao {
  constructor() {
    // Simula o carregamento de configurações
    console.log("Construtor da Configuração chamado!");
    this.config = { tema: 'dark', idioma: 'pt-BR' };
  }

  get(chave) {
    return this.config[chave];
  }

  static getInstance() {
    // Se a instância ainda não existe na propriedade estática da classe, cria uma.
    if (!Configuracao.instance) {
      Configuracao.instance = new Configuracao();
    }
    // Retorna a única instância existente.
    return Configuracao.instance;
  }
}

// Uso pelo cliente
const config1 = Configuracao.getInstance();
const config2 = Configuracao.getInstance();

console.log(config1 === config2); // true (ambas as variáveis apontam para o mesmo objeto)
console.log(config1.get('tema')); // "dark"
```

A mensagem "Construtor da Configuração chamado!" aparecerá apenas uma vez.

### Singleton e os Módulos ES6

O Singleton clássico é frequentemente considerado um "anti-padrão" em muitas linguagens, pois introduz estado global e pode dificultar os testes. Em JavaScript moderno, os **Módulos ES6** oferecem uma alternativa muito mais limpa e elegante.

A especificação dos Módulos ES garante que o código de um módulo seja executado **apenas uma vez**, na primeira vez que ele é importado. O resultado é então colocado em cache. Isso significa que podemos simplesmente criar nossa instância única dentro do arquivo do módulo e exportá-la.

**`configuracao.js` (Módulo Singleton)**

```js
class Configuracao {
  constructor() {
    console.log("Módulo de configuração inicializado!");
    this.config = { tema: 'light', idioma: 'en-US' };
  }
  get(chave) { return this.config[chave]; }
}

// Cria e exporta a ÚNICA instância.
// Este código só roda uma vez em toda a aplicação.
const instanciaConfig = new Configuracao();
export default instanciaConfig;
```

**`app.js`**

```js
import config from './configuracao.js';
console.log(config.get('tema')); // "light"
```

**`outro-arquivo.js`**

```js
import config from './configuracao.js'; // Recebe a MESMA instância do cache de módulos
console.log(config.get('idioma')); // "en-US"
```

A mensagem "Módulo de configuração inicializado!" aparecerá apenas uma vez. Esta abordagem é mais simples, mais testável e a maneira preferida de se lidar com instâncias únicas em JavaScript moderno.

## Considerações Finais

Neste capítulo, elevamos nosso nível de abstração ao explorar os **Padrões de Projeto Criacionais**. Vimos que ir além da instanciação direta com `new` nos abre um universo de flexibilidade, desacoplamento e manutenibilidade.

Exploramos o **Padrão Factory**, em suas diversas formas, como uma maneira de encapsular a lógica de criação. Mergulhamos no **Abstract Factory** para criar famílias de objetos e no **Builder** para construir objetos complexos passo a passo. Revisitamos o **Prototype**, entendendo que ele é a base da própria herança do JavaScript, e analisamos o **Singleton**, aprendendo a garantir uma instância única e suas alternativas modernas com Módulos ES6.

Os padrões de projeto não são soluções prontas, mas sim um vocabulário compartilhado e um conjunto de estratégias comprovadas. Ao internalizá-los, você não estará apenas resolvendo problemas de criação de objetos; você estará arquitetando sistemas de software mais robustos, flexíveis e profissionais.
# Capítulo 48 – Padrões de Projeto Comportamentais: A Coreografia dos Objetos

Nos capítulos anteriores, estabelecemos os alicerces da arquitetura de software. Com os Padrões Criacionais, aprendemos a instanciar objetos de forma flexível. Com os Padrões Estruturais, dominamos a arte de compor esses objetos em estruturas maiores e mais robustas. Agora, chegamos à última e mais dinâmica categoria: os **Padrões de Projeto Comportamentais (Behavioral Design Patterns)**. Se os padrões criacionais são sobre o **nascimento** e os estruturais sobre a **anatomia**, os comportamentais são sobre a **ação** e a **comunicação**.

Estes padrões se concentram nos algoritmos e na atribuição de responsabilidades entre os objetos. Eles não descrevem apenas a estrutura, mas também os padrões de comunicação entre as entidades do nosso sistema. Quem deve ser responsável por uma determinada ação? Como objetos diferentes podem colaborar sem estarem fortemente acoplados? Como podemos encapsular um comportamento ou um algoritmo e torná-lo intercambiável? Como um objeto pode alterar seu comportamento com base em seu estado interno? É para responder a essas perguntas que os padrões comportamentais existem.

Neste capítulo, vamos explorar um rico repertório de estratégias para gerenciar a interação entre os objetos. Desvendaremos o **Strategy** e o **Template Method**, que nos oferecem maneiras flexíveis de lidar com algoritmos. Mergulharemos no **Observer** e no **Mediator**, que gerenciam a comunicação complexa entre múltiplos objetos. Aprenderemos a encapsular operações como objetos com o padrão **Command** e a atravessar coleções de forma uniforme com o **Iterator**. Exploraremos como o padrão **State** pode simplificar objetos cujo comportamento muda drasticamente e como o **Chain of Responsibility** pode desacoplar o remetente de uma solicitação de seu receptor. Ao dominar esses padrões, você estará apto a orquestrar a "coreografia" do seu software, criando sistemas onde a colaboração entre os objetos é não apenas funcional, mas também elegante, eficiente e de fácil manutenção.

## Padrão Chain of Responsibility (Cadeia de Responsabilidade)

O padrão **Chain of Responsibility** visa resolver o problema do acoplamento direto entre o remetente de uma solicitação e seu receptor. Em muitos sistemas, o objeto que inicia uma ação precisa saber exatamente qual objeto deve processá-la. Isso cria uma dependência rígida. Imagine um sistema de aprovação de despesas em uma empresa: uma despesa de R$ 50 pode ser aprovada por um gerente júnior, uma de R$ 500 precisa de um gerente sênior, e uma de R$ 5.000 precisa de um diretor. O código que submete a despesa não deveria precisar conter uma lógica complexa de `if/else` para decidir a quem enviá-la; ele apenas precisa submeter a solicitação e confiar que ela será tratada corretamente.

Este padrão soluciona isso criando uma cadeia de objetos "manipuladores" (handlers). A ideia é dar a mais de um objeto a chance de tratar a solicitação. Para isso, os objetos receptores são encadeados, e a solicitação é passada ao longo dessa cadeia até que um dos objetos a trate. Cada manipulador na cadeia tem uma referência ao próximo e implementa uma interface comum. Ele examina a solicitação e decide se pode tratá-la. Se puder, ele a processa e a cadeia é interrompida. Caso contrário, ele passa a solicitação para o próximo manipulador, e assim por diante.

```js
class Aprovador {
  constructor(nome, limite) {
    this.nome = nome;
    this.limite = limite;
    this.proximo = null;
  }

  setProximo(aprovador) {
    this.proximo = aprovador;
    return aprovador; // Retorna o próximo para permitir o encadeamento
  }

  processar(valor) {
    if (valor <= this.limite) {
      console.log(`${this.nome} aprovou a despesa de R$ ${valor}.`);
    } else if (this.proximo) {
      console.log(`${this.nome} não pode aprovar. Passando para ${this.proximo.nome}...`);
      this.proximo.processar(valor);
    } else {
      console.log(`Ninguém na cadeia pôde aprovar a despesa de R$ ${valor}.`);
    }
  }
}

// Configurando a cadeia de comando
const gerenteJunior = new Aprovador("Gerente Júnior", 100);
const gerenteSenior = new Aprovador("Gerente Sênior", 1000);
const diretor = new Aprovador("Diretor", 10000);

// Encadeando os aprovadores em ordem de poder
gerenteJunior.setProximo(gerenteSenior).setProximo(diretor);

// O cliente só precisa conhecer o primeiro elo da cadeia.
console.log("Processando despesa de R$ 80...");
gerenteJunior.processar(80); // Gerente Júnior aprova

console.log("\nProcessando despesa de R$ 950...");
gerenteJunior.processar(950); // Júnior passa para Sênior, que aprova

console.log("\nProcessando despesa de R$ 12000...");
gerenteJunior.processar(12000); // Ninguém na cadeia pode aprovar
```

## Padrão Command (Comando)

O padrão **Command** aborda o problema de desacoplar o objeto que invoca uma operação daquele que sabe como realizá-la. Imagine a interface de uma calculadora: cada botão (somar, subtrair, etc.) precisa executar uma operação. Uma abordagem ingênua acoplaria o evento de clique do botão diretamente à lógica da calculadora, por exemplo, `calculadora.somar()`. Isso torna difícil implementar funcionalidades mais avançadas, como um histórico de operações, um mecanismo de "desfazer/refazer" (undo/redo), ou enfileirar operações para execução posterior.

A solução do Command é transformar uma solicitação em um objeto autocontido que contém todas as informações sobre essa solicitação. Em vez de o botão chamar diretamente `calculadora.somar(5)`, ele cria um `new ComandoSoma(calculadora, 5)`. Este objeto "Comando" encapsula a ação (`somar`), o receptor da ação (`calculadora`) e os parâmetros (`5`). A interface do usuário (o "Invoker") apenas precisa saber como criar e executar esses comandos, sem conhecer os detalhes da implementação da calculadora (o "Receiver").

```js
// O "Receiver" - o objeto que sabe como realizar o trabalho real.
class Calculadora {
  constructor() { this.valor = 0; }
  executar(comando, valor) {
    switch (comando) {
      case 'somar': this.valor += valor; break;
      case 'subtrair': this.valor -= valor; break;
    }
    console.log(`Valor atual: ${this.valor}`);
  }
}

// A interface do "Command" que encapsula uma solicitação.
class Comando {
  constructor(receptor, op, valor) {
    this.receptor = receptor;
    this.op = op;
    this.valor = valor;
  }
  executar() { 
    this.receptor.executar(this.op, this.valor); 
  }
  // Em uma implementação real, teríamos um método desfazer()
  // desfazer() { this.receptor.executar(this.opInversa, this.valor); }
}

// O "Invoker" - quem aciona os comandos.
const calculadora = new Calculadora();
const comandoSoma = new Comando(calculadora, 'somar', 5);
const comandoSubtracao = new Comando(calculadora, 'subtrair', 2);

comandoSoma.executar();     // Valor atual: 5
comandoSoma.executar();     // Valor atual: 10
comandoSubtracao.executar(); // Valor atual: 8
```

Com essa estrutura, poderíamos facilmente armazenar os objetos `Comando` executados em um array para criar um histórico. Para implementar um "desfazer", bastaria pegar o último comando do array e chamar seu método `desfazer()`.

## Padrão Iterator (Iterador)

O padrão **Iterator** tem como objetivo fornecer uma maneira uniforme de acessar os elementos de um objeto agregado (uma coleção) sequencialmente, sem expor sua representação interna. Em um sistema, você pode ter diferentes tipos de coleções: um Array, um Set, uma estrutura de árvore customizada, etc. O código cliente que precisa percorrer os elementos dessas coleções não deveria ter que escrever uma lógica diferente para cada tipo de estrutura. Ele não precisa saber se a coleção usa um array interno, um objeto ou uma lista encadeada.

A solução é fazer com que cada coleção forneça um objeto "Iterador". Este iterador possui uma interface comum, geralmente um método `next()`, que retorna o próximo elemento da sequência. Em JavaScript moderno, este padrão é tão fundamental que foi **incorporado na própria linguagem** através do **protocolo de iteração**. Qualquer objeto que implemente um método especial chamado `[Symbol.iterator]` é considerado um objeto iterável. Este método deve retornar um objeto iterador com um método `next()`. Isso permite que a estrutura seja usada de forma transparente com a sintaxe `for...of`, o operador spread (`...`) e outras construções da linguagem.

```js
// Arrays e Sets já são iteráveis por padrão.
const meuArray = ['a', 'b', 'c'];
for (const item of meuArray) {
  console.log(item);
}

// Vamos criar um iterador customizado para uma coleção que não é nativamente iterável.
class MinhaColecao {
  constructor() {
    this.itens = {
      primeiro: 10,
      segundo: 20,
      terceiro: 30
    };
  }

  // Implementando o protocolo de iteração para nossa coleção.
  [Symbol.iterator]() {
    const chaves = Object.keys(this.itens);
    let i = 0;
    const itens = this.itens;
    
    // O método retorna o objeto iterador.
    return {
      next: () => {
        const chave = chaves[i++];
        return {
          value: itens[chave],
          done: i > chaves.length
        };
      }
    };
  }
}

const colecao = new MinhaColecao();
// Agora podemos usar for...of, mesmo que a estrutura interna seja um objeto.
for (const item of colecao) {
  console.log(item); // 10, 20, 30
}
```

## Padrão Mediator (Mediador)

O padrão **Mediator** é projetado para reduzir a complexidade da comunicação entre múltiplos objetos. Em uma interface de usuário ou sistema complexo, vários componentes (um botão, um campo de texto, uma lista, uma caixa de seleção) podem precisar reagir a mudanças uns nos outros. Se cada componente mantiver uma referência direta a todos os outros com os quais precisa interagir, o resultado é uma teia de dependências emaranhada, frágil e difícil de manter, conhecida como "código espaguete". Qualquer alteração em um componente pode exigir a modificação de vários outros.

O Mediator promove o baixo acoplamento ao impedir que esses objetos, chamados de "Colegas" (Colleagues), se refiram uns aos outros explicitamente. Em vez disso, ele define um objeto central, o "Mediador", que encapsula como os objetos interagem. Os Colegas se comunicam apenas com o Mediador. Quando o estado de um Colega muda, ele notifica o Mediador. O Mediador, então, que conhece a lógica de coordenação, notifica os outros Colegas que precisam ser atualizados. Isso transforma uma comunicação caótica de "muitos-para-muitos" em uma comunicação organizada de "muitos-para-um-para-muitos".

```js
// O Mediador, que centraliza a comunicação.
class ChatRoom {
  constructor() {
    this.participantes = {};
  }
  registrar(participante) {
    this.participantes[participante.nome] = participante;
    participante.chatroom = this; // O participante conhece o mediador
  }
  enviar(mensagem, de, para) {
    if (para) {
      // Mensagem privada é enviada diretamente ao destinatário.
      para.receber(mensagem, de);
    } else {
      // Mensagem pública é enviada para todos, exceto o remetente.
      for (let key in this.participantes) {
        if (this.participantes[key] !== de) {
          this.participantes[key].receber(mensagem, de);
        }
      }
    }
  }
}

// Os "Colegas" (Colleagues), que só conhecem o Mediador.
class Participante {
  constructor(nome) { this.nome = nome; this.chatroom = null; }
  enviar(mensagem, para) { 
    console.log(`${this.nome} envia: ${mensagem}` + (para ? ` para ${para.nome}`: ''));
    this.chatroom.enviar(mensagem, this, para); 
  }
  receber(mensagem, de) { 
    console.log(`${de.nome} para ${this.nome}: ${mensagem}`); 
  }
}

// Uso
const sala = new ChatRoom();
const ana = new Participante("Ana");
const bia = new Participante("Bia");
const carlos = new Participante("Carlos");

sala.registrar(ana);
sala.registrar(bia);
sala.registrar(carlos);

ana.enviar("Olá a todos!");
bia.enviar("Oi Ana! Tudo bem?", ana);
```

## Padrão Memento (Memento)

O padrão **Memento** permite que você capture e externalize o estado interno de um objeto para que ele possa ser restaurado a esse estado posteriormente, tudo isso sem violar o seu encapsulamento. O principal caso de uso para este padrão é a implementação de funcionalidades de "Desfazer/Refazer" (Undo/Redo). Você precisa de uma maneira de salvar "snapshots" do estado de um objeto ao longo do tempo, mas expor todos os detalhes internos do estado desse objeto para um gerenciador de histórico externo quebraria o princípio do encapsulamento, tornando o código frágil.

A solução envolve três papéis distintos:

1. **Originator:** O objeto cujo estado precisa ser salvo (por exemplo, um editor de texto). Ele é responsável por criar um Memento contendo um snapshot de seu estado atual e também por saber como se restaurar a partir de um Memento.
2. **Memento:** Um objeto simples que armazena o estado do Originator. Ele deve ser opaco para qualquer outra entidade que não seja o próprio Originator, protegendo os dados internos.
3. **Caretaker:** O objeto que armazena os Mementos (por exemplo, uma classe de `Historico`). Ele solicita um Memento ao Originator quando quer salvar um estado e passa um Memento de volta ao Originator quando quer restaurar um estado. O Caretaker não pode (e não deve) modificar ou inspecionar o conteúdo do Memento.

```js
// Memento: armazena o estado do editor. É um objeto de dados simples.
class EditorMemento {
  constructor(conteudo) { this.conteudo = conteudo; }
  getConteudo() { return this.conteudo; }
}

// Originator: o editor de texto que queremos salvar e restaurar.
class Editor {
  constructor() { this.conteudo = ""; }
  digitar(texto) { this.conteudo += texto; }
  getConteudo() { return this.conteudo; }
  
  // Cria um Memento com seu estado atual.
  salvar() { 
    return new EditorMemento(this.conteudo); 
  }
  
  // Restaura seu estado a partir de um Memento.
  restaurar(memento) { 
    this.conteudo = memento.getConteudo(); 
  }
}

// Caretaker: gerencia o histórico de estados (Mementos).
class Historico {
  constructor() { this.mementos = []; }
  salvar(editor) { 
    this.mementos.push(editor.salvar()); 
    console.log("Histórico: Estado salvo.");
  }
  desfazer(editor) {
    const memento = this.mementos.pop();
    if (memento) {
      console.log("Histórico: Desfazendo para o estado anterior.");
      editor.restaurar(memento);
    } else {
      console.log("Histórico: Nada para desfazer.");
    }
  }
}

// Uso
const editor = new Editor();
const historico = new Historico();

editor.digitar("Primeira linha. ");
historico.salvar(editor);

editor.digitar("Segunda linha. ");
historico.salvar(editor);

editor.digitar("Terceira linha.");
console.log(`Estado atual: "${editor.getConteudo()}"`);

historico.desfazer(editor);
console.log(`Estado após desfazer: "${editor.getConteudo()}"`);

historico.desfazer(editor);
console.log(`Estado após segundo desfazer: "${editor.getConteudo()}"`);
```

## Padrão Observer (Observador)

O padrão **Observer** define uma dependência um-para-muitos entre objetos. Quando um objeto, conhecido como "Subject" (ou "Observable"), muda de estado, todos os seus dependentes, os "Observers", são notificados e atualizados automaticamente. Isso cria um sistema de baixo acoplamento, pois o Subject não precisa saber nada sobre seus Observers, exceto que eles implementam uma interface comum para receber atualizações.

O problema que ele resolve é muito comum: você tem um objeto de dados central (por exemplo, o preço de uma ação, o estado de um carrinho de compras) e múltiplos elementos na UI (um gráfico, uma tabela, um ticker, um contador de itens) que precisam refletir o estado atual desse objeto. Acoplar o objeto de dados diretamente a cada elemento da UI seria inflexível e difícil de manter.

A solução do Observer é que o Subject mantém uma lista de Observers. Ele fornece métodos para que os Observers possam se registrar (`subscribe` ou `attach`) e cancelar o registro (`unsubscribe` ou `detach`). Quando o estado do Subject muda, ele simplesmente itera sobre sua lista de observers e chama um método de notificação (geralmente `update`) em cada um deles, passando os novos dados.

```js
// O "Subject" (ou Observable), que mantém o estado e notifica os observadores.
class AgenciaDeNoticias {
  constructor() {
    this.assinantes = [];
    this.ultimaNoticia = '';
  }
  inscrever(assinante) {
    this.assinantes.push(assinante);
  }
  cancelarInscricao(assinante) {
    this.assinantes = this.assinantes.filter(a => a !== assinante);
  }
  notificar() {
    this.assinantes.forEach(assinante => assinante.update(this.ultimaNoticia));
  }
  publicarNoticia(noticia) {
    this.ultimaNoticia = noticia;
    console.log(`\nAGÊNCIA: Publicando nova notícia: "${noticia}"`);
    this.notificar();
  }
}

// A interface do "Observer".
class Assinante {
  constructor(nome) { this.nome = nome; }
  update(noticia) {
    console.log(`${this.nome} recebeu a notícia de última hora: "${noticia}"`);
  }
}

// Uso
const agencia = new AgenciaDeNoticias();
const assinante1 = new Assinante("Leitor A");
const assinante2 = new Assinante("Leitor B");

agencia.inscrever(assinante1);
agencia.inscrever(assinante2);

agencia.publicarNoticia("JavaScript continua dominando a web.");

agencia.cancelarInscricao(assinante1);

agencia.publicarNoticia("Nova versão do Node.js é lançada.");
```

Este padrão é a base de muitos frameworks reativos (como RxJS) e da programação orientada a eventos. O método `addEventListener` do DOM é, em essência, uma implementação nativa do padrão Observer.

## Padrão State (Estado)

O padrão **State** é uma solução para um problema comum: um objeto que precisa alterar drasticamente seu comportamento com base em seu estado interno. A abordagem ingênua para isso seria usar uma grande instrução `if/else` ou `switch` dentro de cada método do objeto, verificando o estado atual antes de decidir o que fazer. Isso torna a classe principal complexa, difícil de manter e viola o Princípio Aberto/Fechado, pois adicionar um novo estado exigiria a modificação de todos esses métodos.

A solução do State é encapsular os comportamentos específicos de cada estado em objetos "State" separados e independentes. O objeto principal, chamado de "Contexto", mantém uma referência ao seu objeto de estado atual e delega todo o comportamento específico do estado para ele. O mais importante é que os próprios objetos de estado podem ser responsáveis por fazer a transição do Contexto para o próximo estado. Do ponto de vista do cliente, o objeto parece ter mudado de classe quando seu estado muda.

```js
// A interface do Estado, definindo os métodos que podem variar.
class EstadoDoPedido {
  constructor(pedido) { this.pedido = pedido; }
  pagar() { throw new Error("Ação não permitida neste estado."); }
  enviar() { throw new Error("Ação não permitida neste estado."); }
  entregar() { throw new Error("Ação não permitida neste estado."); }
}

// Estados Concretos, cada um implementando o comportamento para um estado.
class EstadoNovo extends EstadoDoPedido {
  pagar() {
    console.log("Pedido pago com sucesso. Mudando para o estado 'Pago'.");
    // O estado é responsável pela transição.
    this.pedido.mudarEstado(new EstadoPago(this.pedido));
  }
}
class EstadoPago extends EstadoDoPedido {
  enviar() {
    console.log("Pedido enviado para o endereço. Mudando para o estado 'Enviado'.");
    this.pedido.mudarEstado(new EstadoEnviado(this.pedido));
  }
}
class EstadoEnviado extends EstadoDoPedido {
  entregar() {
    console.log("Pedido entregue ao cliente. Finalizado.");
  }
}

// O "Contexto", que usa um objeto de estado.
class Pedido {
  constructor() {
    this.estado = new EstadoNovo(this); // Estado inicial
  }
  mudarEstado(novoEstado) {
    this.estado = novoEstado;
  }
  // Delega as ações para o objeto de estado atual.
  pagar() { this.estado.pagar(); }
  enviar() { this.estado.enviar(); }
  entregar() { this.estado.entregar(); }
}

// Uso
const meuPedido = new Pedido();
meuPedido.pagar();  // "Pedido pago com sucesso. Mudando para o estado 'Pago'."
meuPedido.enviar(); // "Pedido enviado para o endereço. Mudando para o estado 'Enviado'."
meuPedido.entregar(); // "Pedido entregue ao cliente. Finalizado."
// meuPedido.pagar(); // Lançaria um erro: "Ação não permitida neste estado."
```

## Padrão Strategy (Estratégia)

O padrão **Strategy** permite que você defina uma família de algoritmos, encapsule cada um deles em um objeto separado e os torne intercambiáveis. Isso permite que o algoritmo varie independentemente dos clientes que o utilizam. É um dos padrões mais úteis e flexíveis, sendo uma alternativa poderosa à herança.

O problema que ele resolve ocorre quando você tem uma classe que precisa executar uma tarefa, mas existem várias maneiras (algoritmos) de realizar essa tarefa. Por exemplo, você precisa calcular o custo do frete, mas existem várias formas de cálculo (Correios, FedEx, UPS). Colocar toda essa lógica condicional com `if/else` dentro da classe principal a tornaria complexa, difícil de estender com novas formas de frete e violaria o Princípio de Responsabilidade Única.

A solução do Strategy é extrair esses algoritmos (as "estratégias") para suas próprias classes, todas implementando uma interface comum. O objeto principal, chamado de "Contexto", mantém uma referência a um objeto de estratégia. O Contexto não implementa o algoritmo em si; ele delega essa responsabilidade para a estratégia que lhe foi configurada. O cliente pode, então, trocar a estratégia do Contexto em tempo de execução.

```js
// A interface da Estratégia, definindo o método que todos os algoritmos devem ter.
class EstrategiaDeFrete {
  calcular(peso) { throw new Error("Método não implementado."); }
}

// Estratégias Concretas, cada uma com sua própria implementação do algoritmo.
class FreteCorreios extends EstrategiaDeFrete {
  calcular(peso) { 
    // Lógica de cálculo dos Correios
    return peso * 1.5 + 5; 
  }
}
class FreteFedEx extends EstrategiaDeFrete {
  calcular(peso) { 
    // Lógica de cálculo da FedEx
    return peso * 2.5 + 10; 
  }
}

// O "Contexto", que usa uma estratégia para realizar seu trabalho.
class CalculadoraDeFrete {
  constructor() { this.estrategia = null; }
  setEstrategia(estrategia) { 
    this.estrategia = estrategia; 
  }
  calcular(peso) {
    if (!this.estrategia) {
      throw new Error("Nenhuma estratégia de frete foi definida.");
    }
    return this.estrategia.calcular(peso);
  }
}

// Uso pelo cliente
const calculadora = new CalculadoraDeFrete();
const pesoDoPacote = 10;

// O cliente escolhe a estratégia e a injeta no contexto.
calculadora.setEstrategia(new FreteCorreios());
console.log(`Custo do frete com Correios: R$ ${calculadora.calcular(pesoDoPacote)}`); // R$ 20

// A estratégia pode ser trocada a qualquer momento.
calculadora.setEstrategia(new FreteFedEx());
console.log(`Custo do frete com FedEx: R$ ${calculadora.calcular(pesoDoPacote)}`); // R$ 35
```

Este padrão é um excelente exemplo do princípio de design "composição sobre herança" e torna o sistema muito mais flexível e aberto a extensões.

## Padrão Template Method (Método Modelo)

O padrão **Template Method** define o esqueleto de um algoritmo em um método de uma superclasse, mas permite que as subclasses redefinam certos passos desse algoritmo sem alterar sua estrutura geral. É um padrão baseado em herança que promove a reutilização de código.

O problema que ele resolve surge quando você tem vários processos que seguem os mesmos passos gerais, mas os detalhes de alguns desses passos variam. Por exemplo, o processo de gerar um relatório pode sempre seguir os passos: 1) conectar ao banco de dados, 2) executar a consulta, 3) processar os dados, 4) formatar o relatório. Os passos 1 e 4 podem ser os mesmos para todos os relatórios, mas os passos 2 e 3 são específicos para cada tipo de relatório.

A solução é criar uma classe base abstrata que define um "método modelo" (`template method`). Este método não pode ser sobrescrito e chama uma série de outros métodos na ordem correta para executar o algoritmo. Alguns desses métodos auxiliares são implementados na própria classe base (para os passos invariantes), enquanto outros são declarados como "abstratos" (ou simplesmente deixados para serem implementados) pelas subclasses. As subclasses, então, fornecem as implementações para os passos variáveis, preenchendo as "lacunas" do algoritmo.

```js
// A Classe Abstrata com o Template Method
class ConstrutorDeCasa {
  // O Template Method, que define o esqueleto do algoritmo.
  construirCasa() {
    this.fazerFundacao();
    this.construirParedes();
    this.colocarJanelas();
    this.colocarTelhado();
    console.log("Casa construída!");
  }

  // Um passo comum, implementado na classe base.
  fazerFundacao() {
    console.log("Fazendo fundação de concreto padrão para todas as casas.");
  }

  // Um "hook" opcional que as subclasses podem sobrescrever.
  colocarJanelas() {
    console.log("Colocando janelas de vidro padrão.");
  }

  // Métodos "primitivos" que as subclasses DEVEM implementar.
  construirParedes() { throw new Error("Método abstrato 'construirParedes' deve ser implementado."); }
  colocarTelhado() { throw new Error("Método abstrato 'colocarTelhado' deve ser implementado."); }
}

// Classes Concretas que preenchem as lacunas.
class CasaDeMadeira extends ConstrutorDeCasa {
  construirParedes() { console.log("Construindo paredes de painéis de madeira."); }
  colocarTelhado() { console.log("Colocando telhado de telhas de madeira."); }
}
class CasaDeTijolo extends ConstrutorDeCasa {
  construirParedes() { console.log("Construindo paredes de tijolo à vista."); }
  colocarTelhado() { console.log("Colocando telhado de telhas de barro."); }
}

// Uso
console.log("Construindo casa de madeira:");
const casa1 = new CasaDeMadeira();
casa1.construirCasa();

console.log("\nConstruindo casa de tijolo:");
const casa2 = new CasaDeTijolo();
casa2.construirCasa();
```

## Padrão Visitor (Visitante)

O padrão **Visitor** é uma maneira de separar um algoritmo da estrutura de objetos em que ele opera. Ele é particularmente útil quando você tem uma estrutura de objetos complexa e estável (como uma árvore de sintaxe abstrata ou a árvore do DOM) e precisa executar várias operações diferentes e não relacionadas sobre essa estrutura.

O problema que ele resolve é o seguinte: você tem uma estrutura de objetos (como um documento com nós de `Paragrafo`, `Titulo`, `Imagem`). Agora, você precisa adicionar uma nova funcionalidade, como "exportar para HTML". Uma abordagem seria adicionar um método `exportarHtml()` a cada classe de elemento. Depois, você precisa de uma função "contar palavras", então adiciona um método `contarPalavras()` a cada classe. Isso polui as classes dos elementos com lógicas que não são sua responsabilidade principal e viola o Princípio de Responsabilidade Única.

A solução do Visitor é extrair essas operações para classes "Visitante" separadas. A estrutura de objetos original é modificada uma única vez para adicionar um método `accept(visitor)`. Quando o cliente quer executar uma operação, ele cria um objeto Visitante (ex: `ExportadorHtml`) e o passa para o método `accept` do elemento raiz. O elemento, por sua vez, "chama de volta" o visitante, passando a si mesmo como argumento (`visitor.visitElemento(this)`), um processo conhecido como "double dispatch". O visitante, então, que contém métodos para cada tipo de elemento (`visitParagrafo`, `visitTitulo`), pode executar a lógica correta.

```js
// A interface do Visitante, definindo um método para cada tipo de elemento.
class Visitante {
  visitarParagrafo(p) {}
  visitarTitulo(t) {}
}

// Um Visitante Concreto que implementa a lógica de exportação para HTML.
class ExportadorHtml extends Visitante {
  constructor() { super(); this.output = ""; }
  visitarParagrafo(p) { this.output = `<p>${p.texto}</p>`; }
  visitarTitulo(t) { this.output = `<h1>${t.texto}</h1>`; }
}

// Elementos da estrutura de objetos, agora com o método accept().
class ElementoDocumento { accept(visitor) {} }
class Paragrafo extends ElementoDocumento {
  constructor(texto) { super(); this.texto = texto; }
  accept(visitor) { visitor.visitarParagrafo(this); }
}
class Titulo extends ElementoDocumento {
  constructor(texto) { super(); this.texto = texto; }
  accept(visitor) { visitor.visitarTitulo(this); }
}

// Uso
const documento = [new Titulo("Meu Documento"), new Paragrafo("Este é o conteúdo.")];
const exportador = new ExportadorHtml();

for (const elemento of documento) {
  elemento.accept(exportador);
  console.log(exportador.output);
}
```

O Visitor é um padrão poderoso, mas complexo. Ele é ideal para adicionar novas operações a estruturas de objetos estáveis, mas torna a adição de novos tipos de elementos à estrutura mais difícil, pois exigiria a atualização de todas as classes de visitantes.

## Considerações Finais

Neste capítulo, completamos nossa trilogia sobre Padrões de Projeto ao explorar os **Padrões Comportamentais**. Vimos que eles são essenciais para gerenciar a complexidade da comunicação e da colaboração entre os objetos em um sistema dinâmico.

Aprendemos a desacoplar o remetente do receptor com a **Cadeia de Responsabilidade**, a encapsular ações com o **Comando**, e a gerenciar o estado com os padrões **Memento** e **State**. Dominamos a arte de notificar múltiplos objetos sobre mudanças com o **Observer** e de centralizar a comunicação com o **Mediator**. Vimos como o **Strategy** e o **Template Method** nos dão um controle flexível sobre os algoritmos, e como o **Iterator** e o **Visitor** nos permitem operar sobre estruturas de objetos de forma limpa.

Com o conhecimento combinado dos padrões Criacionais, Estruturais e Comportamentais, você agora possui um vocabulário e um conjunto de ferramentas de arquiteto. Você está preparado não apenas para escrever código, mas para projetar sistemas — sistemas que são robustos, flexíveis, reutilizáveis e, acima de tudo, elegantes em sua solução para problemas complexos.
# Capítulo 23 – Timers: Agendando a Execução de Código

Até agora, a maior parte do código que escrevemos foi **síncrono**: as instruções são executadas uma após a outra, em uma sequência previsível. Uma linha de código só começa depois que a anterior termina. No entanto, o mundo real e as aplicações web modernas são inerentemente **assíncronos**. Precisamos lidar com eventos que acontecem em momentos imprevisíveis — um clique do usuário, a chegada de dados de uma rede, a conclusão de uma animação. Se o JavaScript, que opera em uma única thread principal, ficasse "travado" esperando por essas tarefas, toda a interface do usuário congelaria, resultando em uma péssima experiência.

Para gerenciar essa natureza assíncrona, o JavaScript utiliza um modelo de concorrência baseado em um **Event Loop (Laço de Eventos)**. Em vez de bloquear a execução, o JavaScript pode "agendar" uma tarefa para ser executada no futuro, liberando a thread principal para continuar com outras atividades, como responder a interações do usuário. Quando a tarefa agendada estiver pronta, ela é colocada em uma fila e o Event Loop a executa assim que a thread principal estiver livre.

As ferramentas mais fundamentais que a plataforma web nos dá para interagir com esse sistema de agendamento são os **timers**: `setTimeout` e `setInterval`. Elas nos permitem dizer ao motor do JavaScript: "Não execute este código agora, mas sim **depois** de um certo tempo" ou "Execute este código **repetidamente** a cada certo intervalo de tempo".

Neste capítulo, vamos mergulhar fundo no mundo dos timers do JavaScript. Começaremos com `setTimeout`, aprendendo a agendar a execução única de uma função e, crucialmente, a cancelar esse agendamento. Exploraremos como a ordem de operações e o Event Loop afetam quando nosso código realmente é executado. Em seguida, dissecaremos o `setInterval` para criar ações repetitivas, mas também analisaremos suas armadilhas e por que, em muitos casos, um padrão com `setTimeout` recursivo é uma alternativa mais robusta e segura. Ao final, você não apenas saberá como usar timers, mas entenderá os princípios de concorrência que os regem, uma habilidade essencial para criar aplicações web fluidas e responsivas.

## O Modelo do Event Loop: Por que os Timers são Assíncronos

Antes de usarmos os timers, é vital ter um modelo mental de como o JavaScript lida com o tempo. Imagine que o JavaScript tem:

1. **Call Stack (Pilha de Chamadas):** Onde o código síncrono é executado. Se uma função chama outra, a segunda é empilhada sobre a primeira.
2. **Web APIs (APIs do Navegador):** Onde o navegador gerencia tarefas que levam tempo, como os timers (`setTimeout`) ou requisições de rede.
3. **Callback Queue (Fila de Callbacks):** Uma fila para onde as funções de callback são enviadas quando a tarefa da Web API termina (ex: quando o tempo do timer expira).
4. **Event Loop (Laço de Eventos):** Um processo que constantemente verifica: "A Call Stack está vazia?". Se estiver, ele pega a primeira função da Callback Queue e a empurra para a Call Stack para ser executada.

Quando você chama `setTimeout`, você não está pausando seu código. Você está dizendo ao navegador: "Por favor, inicie um cronômetro por `X` milissegundos. Quando terminar, coloque esta função de callback na fila para mim." O Event Loop cuidará do resto.

## `setTimeout`: Executando Código Após um Atraso

A função `setTimeout` agenda a execução de uma função de callback após um determinado período de tempo.

**Sintaxe:** `const timeoutID = setTimeout(callback, delay, arg1, arg2, ...);`

- `callback`: A função a ser executada.
- `delay` (opcional): O tempo de atraso em milissegundos (1000 ms = 1 segundo). Se omitido ou `0`, o callback será executado o mais rápido possível, mas sempre _após_ o código síncrono atual.
- `arg1, arg2, ...` (opcional): Argumentos adicionais que serão passados para a função de callback.
- **Valor de Retorno (`timeoutID`):** `setTimeout` retorna um ID numérico único, que pode ser usado para cancelar o agendamento.

**Exemplo Básico:**

```js
console.log("Pedido de pizza feito.");

setTimeout(() => {
  console.log("Pizza chegou!");
}, 3000); // 3000 milissegundos = 3 segundos

console.log("Arrumando a mesa...");
```

**Ordem de Execução:**

1. "Pedido de pizza feito."
2. "Arrumando a mesa..."
3. (Após 3 segundos) "Pizza chegou!"

### A Ordem de Operações e o `setTimeout` com Delay Zero

O que acontece se usarmos um delay de `0`? Isso não faz com que o código seja executado imediatamente. Em vez disso, ele diz ao navegador para colocar o callback na fila o mais rápido possível. Devido ao funcionamento do Event Loop, isso significa que ele será executado somente **depois** que toda a pilha de chamadas síncronas atual estiver vazia.

```js
console.log("Início do script (A)");

setTimeout(() => {
  console.log("Dentro do setTimeout (C)");
}, 0);

console.log("Fim do script (B)");

// Ordem de Saída: A, B, C
```

Este padrão é uma técnica poderosa para "adiar" a execução de um código, permitindo que o navegador realize outras tarefas (como renderizar a interface) antes de executar uma tarefa pesada.

### Cancelando um Timeout com `clearTimeout`

Se agendamos uma ação, mas depois decidimos que ela não deve mais acontecer, podemos usar `clearTimeout()`, passando o ID que `setTimeout` retornou.

**Exemplo: Notificação que pode ser Descartada**

```js
let mensagemId;

function exibirNotificacao() {
  console.log("PROMOÇÃO: 50% de desconto! Clique aqui!");
  // ...lógica para mostrar a notificação na tela...
}

// Agenda a notificação para aparecer em 5 segundos
mensagemId = setTimeout(exibirNotificacao, 5000);

// Suponha que o usuário clique em um botão "Fechar Notificações" antes dos 5 segundos
function fecharNotificacoes() {
  console.log("Usuário dispensou as notificações. Cancelando o agendamento.");
  clearTimeout(mensagemId);
}

// Para simular, vamos chamar a função de fechar após 2 segundos
setTimeout(fecharNotificacoes, 2000);
```

Se `clearTimeout` for chamado, a função `exibirNotificacao` nunca será executada. É uma prática crucial de limpeza para evitar a execução de código obsoleto e potenciais memory leaks.

## `setInterval`: Execução Repetitiva

A função `setInterval` é usada para executar uma função de callback repetidamente, com um intervalo de tempo fixo entre cada chamada.

**Sintaxe:** `const intervalID = setInterval(callback, delay, arg1, arg2, ...);`

A sintaxe é idêntica à de `setTimeout`. A diferença é que o callback será executado continuamente a cada `delay` milissegundos, até que seja explicitamente parado.

**Exemplo: Um Relógio Digital Simples**

```js
function mostrarHora() {
  const agora = new Date();
  const horaFormatada = agora.toLocaleTimeString('pt-BR');
  console.clear(); // Limpa o console para uma exibição limpa
  console.log(horaFormatada);
}

// Executa a função mostrarHora a cada segundo
const relogioId = setInterval(mostrarHora, 1000);

// Para parar o relógio, usaríamos clearInterval(relogioId)
```

### Cancelando um Intervalo com `clearInterval`

Assim como `clearTimeout`, `clearInterval()` é usado para parar as repetições. Ele recebe o ID retornado por `setInterval`.

```js
let contador = 0;
const contadorId = setInterval(() => {
  contador++;
  console.log(`Contador: ${contador}`);
  
  if (contador >= 5) {
    console.log("Atingiu 5. Parando o intervalo.");
    clearInterval(contadorId);
  }
}, 500);
```

É essencial limpar os intervalos quando eles não são mais necessários (por exemplo, quando um usuário navega para outra página em uma Single Page Application) para evitar que o código continue rodando em segundo plano e causando memory leaks.

## A Armadilha do `setInterval`: Sobreposição de Execuções

`setInterval` tem uma característica que pode ser problemática em certos cenários. Ele agenda a próxima execução para ocorrer `delay` milissegundos após o **início** da execução anterior, **sem se importar se a execução anterior já terminou**.

Imagine que você tem uma função que busca dados de uma API, e você a configura com `setInterval` para rodar a cada 2 segundos: `setInterval(buscarDados, 2000);`

Se, devido a uma conexão de rede lenta, a primeira chamada a `buscarDados` levar 3 segundos para ser concluída, o `setInterval` já terá agendado a **segunda** chamada para começar no segundo 2. O resultado é que as chamadas começam a se sobrepor, com múltiplas requisições de rede acontecendo simultaneamente, sobrecarregando o servidor e o cliente.

## O Padrão Robusto: `setTimeout` Recursivo

Para evitar a sobreposição, a prática recomendada para executar tarefas repetitivas que podem ter uma duração variável é usar um padrão de `setTimeout` recursivo.

Neste padrão, em vez de agendar todas as execuções de uma vez, agendamos a **próxima** execução apenas **depois** que a execução atual for concluída.

**Exemplo: Polling de API de Forma Segura**

```js
const delay = 5000; // 5 segundos

function buscarAtualizacoes() {
  console.log("Buscando atualizações...");

  // Simula uma requisição de API com duração variável
  const tempoDeRede = Math.random() * 3000; // Leva até 3s

  setTimeout(() => {
    console.log("Dados recebidos!");
    
    // Processa os dados...

    // AGORA, agendamos a próxima chamada.
    // Isso garante um intervalo de 5 segundos APÓS o término da atual.
    setTimeout(buscarAtualizacoes, delay);
  }, tempoDeRede);
}

// Inicia o ciclo
setTimeout(buscarAtualizacoes, delay);
```

**Comparação:**

- **`setInterval`:** Garante que a execução **comece** a cada `X` ms. **Não garante** um intervalo entre as execuções.
- **`setTimeout` Recursivo:** Garante que haja um intervalo de **pelo menos** `X` ms entre o **fim** de uma execução e o **início** da próxima.

Para a maioria das tarefas repetitivas que envolvem operações assíncronas (como I/O de rede ou de arquivo), o padrão `setTimeout` recursivo é a escolha mais segura e robusta.

## Considerações Finais

Neste capítulo, desvendamos os timers do JavaScript, as ferramentas fundamentais para agendar código e lidar com a assincronicidade. Vimos como `setTimeout` e `setInterval` nos permitem interagir com o **Event Loop**, adiando e repetindo a execução de funções.

Compreendemos a importância crucial da ordem de operações, e que um `setTimeout(fn, 0)` não executa imediatamente, mas sim enfileira a função para rodar o mais rápido possível após o código síncrono atual. Dominamos o cancelamento de timers com `clearTimeout` e `clearInterval`, uma prática essencial para evitar memory leaks e a execução de código obsoleto.

O ponto alto foi a análise crítica do `setInterval`. Identificamos seu potencial problema de sobreposição de execuções e aprendemos o padrão superior do **`setTimeout` recursivo**, que nos fornece uma maneira robusta e segura de criar laços assíncronos. Com este conhecimento, você não está mais limitado a um fluxo de execução linear; você pode orquestrar tarefas ao longo do tempo, um passo fundamental para construir aplicações web dinâmicas e interativas.
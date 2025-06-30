# Capítulo 44 – Vibration API e Battery Status API

Ao longo de nossa jornada pelo JavaScript, construímos aplicações web que são visualmente ricas, interativas e conectadas à internet. Elas existem dentro da "caixa de areia" (sandbox) do navegador, um ambiente seguro que, por design, isola o nosso código do sistema operacional e do hardware subjacente. No entanto, para que as aplicações web possam competir de igual para igual com as aplicações nativas, a plataforma web tem, gradualmente, construído pontes seguras que permitem ao nosso código, com o consentimento do usuário, interagir com os recursos físicos do dispositivo. Já vimos isso com a Geolocation API e a Notifications API.

Este capítulo explora duas outras APIs que aprofundam essa integração entre o software e o hardware, tornando a experiência do usuário mais rica e contextual: a **Vibration API** e a **Battery Status API**. A primeira nos dá acesso a uma forma de feedback que vai além do visual e do auditivo: o **feedback tátil**. Com ela, podemos fazer com que o dispositivo do usuário vibre, o que é uma ferramenta poderosa para confirmar ações, sinalizar alertas em jogos ou criar uma camada extra de imersão. A segunda API nos permite "olhar" para um dos recursos mais críticos de um dispositivo móvel: sua bateria. Com ela, podemos criar aplicações "conscientes", que se adaptam para economizar energia quando a bateria está fraca, melhorando a experiência e a utilidade do nosso software.

Nesta exploração, vamos dominar a mecânica dessas duas APIs. Começaremos com a Vibration API, aprendendo a acionar vibrações simples e a orquestrar padrões complexos. Em seguida, faremos um mergulho na Battery Status API, descobrindo como monitorar o nível da bateria, seu estado de carregamento e como reagir a mudanças. Em ambos os casos, enfatizaremos a importância da detecção de suporte e das melhores práticas, garantindo que nossas aplicações sejam, ao mesmo tempo, mais inteligentes e respeitosas com os recursos do usuário.

## Vibration API: Fornecendo Feedback Tátil

O feedback tátil (haptic feedback) é o uso do sentido do tato para comunicar informações ao usuário. Em dispositivos móveis, isso é quase universalmente alcançado através da vibração. A Vibration API nos dá um controle direto e simples sobre o motor de vibração do dispositivo.

**Casos de Uso:**

- **Notificações:** Associar um padrão de vibração a uma notificação para alertar o usuário.
- **Jogos:** Criar imersão ao vibrar quando um jogador é atingido ou realiza uma ação importante.
- **Confirmação de Ação:** Fornecer um pequeno "solavanco" para confirmar que uma ação, como o envio de um formulário ou a exclusão de um item, foi bem-sucedida.

### Verificando o Suporte

A Vibration API não está disponível em todos os dispositivos (é primariamente para dispositivos móveis) ou em todos os navegadores. Portanto, o primeiro passo é sempre verificar sua existência no objeto `navigator`.

```js
function verificarSuporteVibracao() {
  if ("vibrate" in navigator) {
    console.log("Este navegador suporta a Vibration API.");
    return true;
  } else {
    console.log("Este navegador NÃO suporta a Vibration API.");
    return false;
  }
}
```

### Vibração Única

A forma mais simples de usar a API é chamar `navigator.vibrate()` com um único número, que representa a duração da vibração em milissegundos.

`navigator.vibrate(duracao);`

**Exemplo:**

```js
const botaoVibrar = document.getElementById('btn-vibrar');

botaoVibrar.addEventListener('click', () => {
  if ("vibrate" in navigator) {
    // Vibra por 200 milissegundos
    navigator.vibrate(200);
  }
});
```

### Padrões de Vibração

O verdadeiro poder da API reside em sua capacidade de criar padrões complexos de vibração e pausa. Para isso, passamos um array de números para `navigator.vibrate()`. Os números no array são interpretados alternadamente como duração de vibração e duração de pausa, em milissegundos.

`navigator.vibrate([vibracao1, pausa1, vibracao2, pausa2, ...]);`

**Exemplo: Padrão de Notificação "SOS"** O código Morse para SOS é `... --- ...` (três pulsos curtos, três longos, três curtos).

```js
const botaoSOS = document.getElementById('btn-sos');

botaoSOS.addEventListener('click', () => {
  if ("vibrate" in navigator) {
    const pulsoCurto = 100;
    const pulsoLongo = 300;
    const pausaCurta = 100;
    const pausaLonga = 300;

    const padrao_sos = [
      pulsoCurto, pausaCurta, pulsoCurto, pausaCurta, pulsoCurto, // S
      pausaLonga,
      pulsoLongo, pausaCurta, pulsoLongo, pausaCurta, pulsoLongo, // O
      pausaLonga,
      pulsoCurto, pausaCurta, pulsoCurto, pausaCurta, pulsoCurto  // S
    ];
    
    navigator.vibrate(padrao_sos);
  }
});
```

### Parando a Vibração

Para parar qualquer vibração que esteja em andamento, basta chamar `navigator.vibrate()` com um array vazio ou com o valor `0`.

```js
// Para parar a vibração
navigator.vibrate([]);
// ou
navigator.vibrate(0);
```

Isso é útil para, por exemplo, permitir que o usuário cancele uma vibração longa de "encontrar meu telefone".

### Considerações e Boas Práticas

- **Contexto do Usuário:** A vibração deve, idealmente, ser uma resposta a uma ação do usuário. Vibrações inesperadas são geralmente indesejadas.
- **Moderação:** Use com moderação. Vibrações excessivas podem ser irritantes e consomem bateria.
- **Controle do Usuário:** Em aplicações que usam vibração extensivamente (como jogos), é uma excelente prática fornecer uma opção nas configurações para o usuário desativá-la.

## Battery Status API: Aplicações Conscientes de Energia

**Atenção: API Obsoleta (Deprecated)** É crucial começar esta seção com um aviso importante: a Battery Status API foi **declarada obsoleta** e está sendo removida dos navegadores modernos. A razão principal são as preocupações com a privacidade, pois os valores detalhados e as flutuações da bateria poderiam ser usados como um vetor para o "fingerprinting" — uma técnica para identificar e rastrear usuários únicos na web.

Embora você **não deva usar esta API em novos projetos de produção**, estudá-la ainda é um exercício valioso. Ela nos mostra um vislumbre do tipo de integração profunda que a web pode alcançar e os princípios de como uma aplicação pode se tornar "consciente" dos recursos do dispositivo.

A API permite que uma aplicação web obtenha informações sobre o estado da bateria do sistema, como o nível de carga, se está conectada à energia e o tempo estimado até carregar ou descarregar completamente.

### Acessando o Gerenciador de Bateria

O acesso à API é feito através do método `navigator.getBattery()`. Como a obtenção dessas informações pode levar um momento, o método retorna uma `Promise` que resolve com um objeto `BatteryManager`.

```js
if ('getBattery' in navigator) {
  navigator.getBattery()
    .then(battery => {
      console.log("Acesso à API de Bateria obtido.");
      // O objeto 'battery' é o nosso BatteryManager
      processarStatusDaBateria(battery);
    })
    .catch(error => {
      console.error("Não foi possível acessar a API de Bateria.", error);
    });
} else {
  console.log("API de Bateria não suportada.");
}
```

### O Objeto `BatteryManager`

O objeto `BatteryManager` retornado contém todas as informações e os eventos.

**Propriedades:**

- **`level`**: Um número entre `0.0` e `1.0` que representa o nível de carga atual da bateria. Para exibir como porcentagem, basta multiplicar por 100.
- **`charging`**: Um booleano que é `true` se o dispositivo está atualmente conectado a uma fonte de energia e `false` caso contrário.
- **`dischargingTime`**: O tempo restante, em segundos, até que a bateria se esgote. Pode ser `Infinity` se o dispositivo estiver carregando ou se o tempo não puder ser determinado.
- **`chargingTime`**: O tempo restante, em segundos, até que a bateria esteja totalmente carregada. Pode ser `Infinity` se o dispositivo não estiver carregando ou se o tempo não puder ser determinado.

### Eventos da Bateria

O objeto `BatteryManager` é também um emissor de eventos, permitindo-nos reagir a mudanças no estado da bateria em tempo real.

- `levelchange`: Disparado quando a propriedade `level` muda.
- `chargingchange`: Disparado quando a propriedade `charging` muda.
- `dischargingtimechange`: Disparado quando a propriedade `dischargingTime` muda.
- `chargingtimechange`: Disparado quando a propriedade `chargingTime` muda.

**Exemplo Completo: Monitorando a Bateria**

```js
async function iniciarMonitorDeBateria() {
  if (!('getBattery' in navigator)) return;

  try {
    const battery = await navigator.getBattery();
    
    // Função para atualizar a UI com os dados da bateria
    const atualizarUI = () => {
      document.getElementById('nivel').textContent = `${Math.floor(battery.level * 100)}%`;
      document.getElementById('carregando').textContent = battery.charging ? 'Sim' : 'Não';
      document.getElementById('tempo-carga').textContent = 
        battery.chargingTime === Infinity ? 'N/A' : `${Math.floor(battery.chargingTime / 60)} min`;
      document.getElementById('tempo-descarga').textContent = 
        battery.dischargingTime === Infinity ? 'N/A' : `${Math.floor(battery.dischargingTime / 60)} min`;

      // Exemplo de lógica para modo de economia de energia
      if (battery.level < 0.2 && !battery.charging) {
        document.body.classList.add('modo-economia');
      } else {
        document.body.classList.remove('modo-economia');
      }
    };
    
    // Atualiza a UI imediatamente
    atualizarUI();
    
    // Adiciona os listeners para futuras mudanças
    battery.addEventListener('levelchange', atualizarUI);
    battery.addEventListener('chargingchange', atualizarUI);
    battery.addEventListener('chargingtimechange', atualizarUI);
    battery.addEventListener('dischargingtimechange', atualizarUI);

  } catch (err) {
    console.error("Erro na API de Bateria:", err);
  }
}

iniciarMonitorDeBateria();
```

## Considerações Finais

Neste capítulo, exploramos duas APIs que demonstram a crescente capacidade do JavaScript de interagir com o hardware do dispositivo, aproximando as aplicações web das nativas.

Com a **Vibration API**, aprendemos a fornecer feedback tátil, uma ferramenta sutil mas eficaz para melhorar a experiência do usuário, confirmar ações e aumentar a imersão em jogos. Dominamos a criação de vibrações simples e de padrões complexos, sempre mantendo em mente a importância do consentimento e da moderação.

Com a **Battery Status API**, embora agora obsoleta, vislumbramos o potencial de aplicações "conscientes do contexto". Vimos como uma aplicação poderia se adaptar ao estado da bateria do usuário para economizar energia, uma demonstração poderosa de como a web pode se tornar mais inteligente e respeitosa com os recursos do usuário.

A lição mais importante é que, à medida que a plataforma web evolui, novas pontes entre o software e o hardware continuarão a surgir. Como desenvolvedores, nosso papel é usar esses novos poderes de forma responsável, sempre priorizando a experiência, a privacidade e a segurança do usuário.
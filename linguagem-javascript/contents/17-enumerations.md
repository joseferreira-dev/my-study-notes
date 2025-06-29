# Capítulo 17 – Enumerações: Gerenciando Conjuntos de Constantes

Ao longo da nossa jornada, aprendemos a representar uma vasta gama de dados: textos com `string`, números com `number`, listas com `Array`, dicionários com `Map` e valores únicos com `Set`. No entanto, há um tipo de dado conceitual que, embora comum em muitas outras linguagens de programação, não existe como uma estrutura nativa em JavaScript: a **Enumeração (ou Enum)**. Uma enumeração é um tipo de dado especial que nos permite definir um conjunto de **constantes nomeadas**. Pense nos dias da semana, nos naipes de um baralho, nos status de um pedido (Pendente, Aprovado, Enviado, Entregue) ou nos níveis de acesso de um usuário (Admin, Editor, Leitor).

Em vez de usar valores arbitrários e frágeis como strings ou números (as chamadas "magic strings" ou "magic numbers") espalhados pelo nosso código, uma enumeração agrupa essas constantes relacionadas, tornando o código mais legível, seguro e de fácil manutenção. Se você escreve `if (status === 'APROVADO')`, um simples erro de digitação (`'APROVADO'` com um espaço extra) pode introduzir um bug silencioso e difícil de rastrear. Se, em vez disso, você escreve `if (status === StatusPedido.APROVADO)`, o autocompletar do seu editor ajuda, e qualquer erro de digitação no nome da constante (`StatusPedido.APROVAD`) resultaria em um erro claro e imediato.

Embora o JavaScript não tenha uma palavra-chave `enum`, a comunidade de desenvolvedores estabeleceu padrões poderosos e eficazes para simular esse comportamento. Neste capítulo, vamos explorar esses padrões. Começaremos entendendo o problema dos "valores mágicos" e como a abordagem mais simples, usando um objeto, já nos ajuda. Em seguida, vamos refinar essa técnica, tornando-a imutável com `Object.freeze()`. Por fim, mergulharemos na implementação mais robusta e segura de enums usando `Symbol`, garantindo que cada membro da enumeração seja verdadeiramente único. Ao final, você não apenas saberá como criar e usar enums em JavaScript, mas também entenderá os princípios de design por trás de cada padrão para escolher o mais adequado para suas necessidades.

## O Problema: "Valores Mágicos" e a Fragilidade do Código

Antes de explorarmos as soluções, é crucial entender o problema que as enumerações resolvem. "Valores Mágicos" são valores literais (como a string `"PENDING"` ou o número `2`) que aparecem no código sem uma explicação clara de seu significado.

**Exemplo de Código com Valores Mágicos:**

```js
function processarPedido(pedido) {
  // O que significa o status 2? E se digitarmos 'pendente' em vez de 'Pendente'?
  if (pedido.status === 2 || pedido.status === 'Pendente') {
    console.log("O pedido está aguardando pagamento.");
  } else if (pedido.status === 3 || pedido.status === 'Aprovado') {
    console.log("Pedido aprovado, preparando para envio.");
  }
}

const meuPedido = { id: 123, status: 3 };
processarPedido(meuPedido);
```

Este código apresenta vários problemas:

1. **Falta de Legibilidade:** O que o número `2` ou `3` significa? Um novo desenvolvedor (ou você mesmo, meses depois) precisaria procurar a definição em outro lugar.
2. **Risco de Erros de Digitação:** Um simples `pedido.status === 'aprovado'` (com 'a' minúsculo) falharia silenciosamente.
3. **Dificuldade de Manutenção:** Se o valor do status "Aprovado" precisar mudar de `3` para `4`, você teria que encontrar e substituir todas as ocorrências desse número mágico em toda a base de código, uma tarefa perigosa e propensa a erros.
4. **Falta de Fonte Única da Verdade:** Os valores válidos estão espalhados pelo código, não centralizados em um único local.

As enumerações resolvem todos esses problemas ao fornecer um nome significativo e um local centralizado para essas constantes.

## Padrão 1: O Enum com Objeto Simples

A forma mais básica e comum de se criar uma enumeração em JavaScript é usando um objeto literal. Agrupamos nossas constantes nomeadas como propriedades de um objeto.

```js
const StatusPedido = {
  PENDENTE: 'Pendente',
  APROVADO: 'Aprovado',
  ENVIADO: 'Enviado',
  ENTREGUE: 'Entregue',
  CANCELADO: 'Cancelado'
};

function processarPedidoRefatorado(pedido) {
  if (pedido.status === StatusPedido.PENDENTE) {
    console.log("O pedido está aguardando pagamento.");
  } else if (pedido.status === StatusPedido.APROVADO) {
    console.log("Pedido aprovado, preparando para envio.");
  }
}

const pedidoRefatorado = { id: 124, status: StatusPedido.APROVADO };
processarPedidoRefatorado(pedidoRefatorado);
```

Esta abordagem já é uma melhoria imensa. O código é mais legível, e se precisarmos mudar o valor da string de `"Aprovado"` para `"Pagamento Confirmado"`, só precisamos fazer isso em um único lugar: no objeto `StatusPedido`.

No entanto, este padrão tem uma fraqueza: objetos em JavaScript são, por padrão, **mutáveis**.

```js
// Alguém poderia acidentalmente (ou intencionalmente) modificar o enum
StatusPedido.APROVADO = 'MODIFICADO';
StatusPedido.NOVO_STATUS = 'Novo';

console.log(StatusPedido.APROVADO); // "MODIFICADO"
```

Isso viola a natureza de uma enumeração, que deveria ser um conjunto de constantes imutáveis.

## Padrão 2: O Enum Imutável com `Object.freeze()`

Para garantir que nossa enumeração seja verdadeiramente constante, podemos usar o método `Object.freeze()`. Este método "congela" um objeto, impedindo que novas propriedades sejam adicionadas, que propriedades existentes sejam removidas, e que os valores das propriedades existentes sejam alterados.

**Definição:**

```js
const Direcao = Object.freeze({
  CIMA: 'UP',
  BAIXO: 'DOWN',
  ESQUERDA: 'LEFT',
  DIREITA: 'RIGHT'
});

// Tentativas de modificação agora falharão
try {
  Direcao.CIMA = 'PARA CIMA'; // Falha silenciosamente em modo não-estrito
} catch (e) {
  console.error(e); // Lança um TypeError em modo estrito ('use strict')
}

console.log(Direcao.CIMA); // 'UP' (o valor permaneceu inalterado)
```

Este padrão é robusto e o mais utilizado na prática para criar enums em JavaScript. Ele fornece legibilidade, uma fonte única da verdade e imutabilidade.

## Padrão 3: O Enum com `Symbol` para Unicidade Absoluta

Embora o padrão com `Object.freeze()` e valores de string seja excelente, ele ainda tem uma pequena desvantagem teórica. O valor de `Direcao.CIMA` é a string `'UP'`. Isso significa que `Direcao.CIMA === 'UP'` é verdadeiro. Um desenvolvedor poderia, por engano, voltar a usar a "magic string" `'UP'` em vez da constante `Direcao.CIMA`, e o código continuaria funcionando, reintroduzindo a fragilidade que queríamos evitar.

Para a máxima segurança e robustez, podemos criar uma enumeração onde cada membro é um **`Symbol`**. Como vimos no capítulo sobre Symbols, cada symbol é um valor garantidamente único.

**Implementação:**

```js
const NivelLog = Object.freeze({
  DEBUG: Symbol('Debug'),
  INFO: Symbol('Info'),
  WARN: Symbol('Warning'),
  ERROR: Symbol('Error')
});

function registrarLog(nivel, mensagem) {
  if (nivel === NivelLog.ERROR) {
    console.error(`[${nivel.description}] ${mensagem}`);
  } else {
    console.log(`[${nivel.description}] ${mensagem}`);
  }
}

registrarLog(NivelLog.INFO, "Servidor iniciado com sucesso.");
registrarLog(NivelLog.ERROR, "Falha ao conectar ao banco de dados.");

// Agora, a comparação com uma string sempre será falsa, forçando o uso do enum.
console.log(NivelLog.INFO === 'Info'); // false
```

**Vantagens do Enum com Symbol:**

- **Segurança de Tipo:** É impossível usar acidentalmente uma string ou número no lugar de um membro do enum. O código `if (nivel === 'Error')` simplesmente não funcionaria.
- **Sem Colisão de Valores:** Cada membro é um valor único, eliminando qualquer possibilidade de conflito.
- **Intenção Clara:** Força o uso do enum `NivelLog.ERROR`, tornando o código explícito e auto-documentado.

O `Symbol`-based enum é a abordagem academicamente mais correta e segura, ideal para APIs públicas de bibliotecas ou sistemas críticos onde a clareza e a prevenção de erros são prioridade máxima.

## Simulação de Valor Automático

Em algumas linguagens, é possível definir um enum onde os valores são atribuídos automaticamente (geralmente como uma sequência de números inteiros). Podemos simular esse comportamento em JavaScript com uma função auxiliar.

### Enumeração Numérica Automática

```js
function criarEnumNumerico(chaves) {
  const enumObjeto = {};
  for (let i = 0; i < chaves.length; i++) {
    const chave = chaves[i];
    enumObjeto[chave] = i;
  }
  return Object.freeze(enumObjeto);
}

const CamadaRede = criarEnumNumerico([
  'FISICA',
  'ENLACE',
  'REDE',
  'TRANSPORTE',
  'SESSAO',
  'APRESENTACAO',
  'APLICACAO'
]);

console.log(CamadaRede.FISICA);     // 0
console.log(CamadaRede.TRANSPORTE); // 3
console.log(CamadaRede.APLICACAO);  // 6
```

Este padrão é útil quando você precisa de valores numéricos, mas quer a legibilidade de um nome de constante.

## Operações Comuns com Enums

Uma vez que nosso enum é um objeto, podemos usar os métodos de `Object` para trabalhar com ele.

**1. Verificando se uma Chave Existe:**

```js
console.log('REDE' in CamadaRede); // true
console.log('INTERNET' in CamadaRede); // false
```

**2. Verificando se um Valor Existe (Membership):** Isso é útil para validar se um valor recebido (ex: de uma API) é um membro válido do nosso enum.

```js
const valoresValidos = Object.values(Direcao); // ['UP', 'DOWN', 'LEFT', 'RIGHT']

function ehDirecaoValida(valor) {
  return valoresValidos.includes(valor);
}

console.log(ehDirecaoValida('UP'));   // true
console.log(ehDirecaoValida('DOWN')); // true
console.log(ehDirecaoValida('diagonal')); // false
```

**3. Obtendo a Chave a Partir de um Valor (Reverse Mapping):** Às vezes, temos o valor (ex: `'UP'`) e precisamos do nome da chave (`'CIMA'`).

```js
function getChavePorValor(objetoEnum, valor) {
  return Object.keys(objetoEnum).find(chave => objetoEnum[chave] === valor);
}

const chave = getChavePorValor(Direcao, 'LEFT');
console.log(chave); // "ESQUERDA"
```

Note que isso não funciona de forma confiável para enums numéricos com valores duplicados e não se aplica a enums baseados em `Symbol` (já que o valor é o próprio symbol).

## Considerações Finais

Neste capítulo, abordamos um padrão de design fundamental que preenche uma lacuna na sintaxe nativa do JavaScript: a **Enumeração**. Vimos que, embora não exista uma palavra-chave `enum`, podemos simular seu comportamento de forma eficaz e robusta, trazendo mais clareza, segurança e manutenibilidade ao nosso código.

Começamos por identificar o problema dos **"valores mágicos"** e como eles tornam nosso código frágil. Exploramos a solução mais comum e prática: um objeto imutável criado com **`Object.freeze()`**, que serve perfeitamente para a maioria dos casos de uso. Em seguida, aprofundamos nosso conhecimento com o padrão baseado em **`Symbol`**, a abordagem mais segura que garante a unicidade de cada membro do enum e previne comparações acidentais com valores literais.

Além disso, aprendemos a criar enums com valores automáticos e a realizar operações comuns, como verificar a existência de um valor. A lição mais importante é que o uso de enums eleva a qualidade do nosso código, substituindo valores ambíguos por constantes nomeadas, auto-documentadas e centralizadas. Ao adotar esses padrões, você estará escrevendo um software mais profissional, menos propenso a erros e mais fácil de ser compreendido e modificado no futuro.
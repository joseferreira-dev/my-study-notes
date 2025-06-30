# Capítulo 38 – API de Internacionalização (Intl): Falando a Língua do Usuário

A web é, por sua própria natureza, uma plataforma global. Nossas aplicações podem ser acessadas por usuários em Recife, Lisboa, Tóquio ou Berlim. Essa audiência global apresenta um desafio fundamental: como podemos apresentar informações, como datas, horas, números e moedas, de uma forma que seja natural e compreensível para cada usuário, de acordo com suas convenções culturais e linguísticas? Apresentar a data "06/29/2025" para um brasileiro pode causar confusão, assim como o preço "R$ 1.999,90" pode não fazer sentido para um americano. Por muitos anos, a solução para este problema em JavaScript foi um emaranhado de lógica manual, bibliotecas externas pesadas e manipulação de strings frágil e propensa a erros.

Esse processo de adaptar um software para diferentes idiomas, regiões e culturas é chamado de **internacionalização (i18n)** e **localização (l10n)**. Para fornecer aos desenvolvedores uma maneira nativa, poderosa e padronizada de lidar com esses desafios, o ECMAScript introduziu a **API de Internacionalização**, centralizada sob o namespace do objeto global **`Intl`**. Esta API é uma suíte de ferramentas sofisticada, construída diretamente no motor do JavaScript, que nos permite formatar dados de forma sensível à localidade, sem a necessidade de reinventar a roda ou adicionar dependências externas pesadas.

Neste capítulo, faremos um mergulho profundo nesta API essencial. Começaremos por entender o conceito de "locales", a pedra angular que informa à API qual conjunto de convenções usar. Em seguida, dominaremos os três principais construtores do `Intl`: `Intl.DateTimeFormat`, para uma formatação de data e hora com um controle granular sem precedentes; `Intl.NumberFormat`, para exibir números e, crucialmente, moedas de acordo com os padrões locais; e exploraremos outras ferramentas poderosas como `Intl.Collator` para ordenação correta de textos e `Intl.RelativeTimeFormat` para criar expressões como "ontem" ou "em 3 meses". Ao final, você estará equipado para construir aplicações verdadeiramente globais, que se comunicam com cada usuário em sua própria língua.

## O Conceito Central: Locales

A base de toda a API `Intl` é o conceito de **locale**. Um locale é uma string que identifica um idioma, uma região e outras variações, como o sistema de numeração. A sintaxe segue os padrões BCP 47.

- **Formato Simples (idioma):** `'pt'` (Português), `'en'` (Inglês), `'es'` (Espanhol).
- **Formato Completo (idioma-REGIÃO):** `'pt-BR'` (Português do Brasil), `'en-US'` (Inglês dos Estados Unidos), `'en-GB'` (Inglês do Reino Unido).

Ao usar os construtores da API `Intl`, o primeiro argumento que você passa é sempre o locale (ou um array de locales, em ordem de preferência). Isso diz à API qual conjunto de regras de formatação ela deve aplicar. Se você omitir o locale, a API usará o locale padrão do ambiente do usuário (o navegador), que é geralmente o comportamento desejado.

```js
// Usando o locale do ambiente
new Intl.DateTimeFormat().resolvedOptions().locale; // Ex: 'pt-BR'

// Especificando um locale
new Intl.DateTimeFormat('ja-JP').resolvedOptions().locale; // 'ja-JP'
```

## Formatando Datas e Horas com `Intl.DateTimeFormat`

Esta é a ferramenta definitiva para formatar datas. Ela nos dá um controle preciso sobre como cada parte de uma data é exibida. O padrão de uso é criar um "formatador" com um conjunto de opções e, em seguida, usar esse formatador para formatar múltiplos objetos `Date`.

**Sintaxe:** `new Intl.DateTimeFormat(locales, options)`

O segundo argumento, `options`, é um objeto onde a mágica acontece.

### Opções de Formatação de Data e Hora

As opções permitem que você especifique exatamente quais componentes da data/hora você quer exibir e em qual formato.

|Opção|Valores Possíveis|Descrição|
|---|---|---|
|`dateStyle`|`"full"`, `"long"`, `"medium"`, `"short"`|Estilo predefinido para a data.|
|`timeStyle`|`"full"`, `"long"`, `"medium"`, `"short"`|Estilo predefinido para a hora.|
|`weekday`|`"long"`, `"short"`, `"narrow"`|Dia da semana (ex: "segunda-feira", "seg.", "S").|
|`era`|`"long"`, `"short"`, `"narrow"`|A era (ex: "depois de Cristo", "d.C.").|
|`year`|`"numeric"`, `"2-digit"`|Ano (ex: "2025", "25").|
|`month`|`"numeric"`, `"2-digit"`, `"long"`, `"short"`, `"narrow"`|Mês (ex: "6", "06", "junho", "jun.", "J").|
|`day`|`"numeric"`, `"2-digit"`|Dia do mês.|
|`hour`, `minute`, `second`|`"numeric"`, `"2-digit"`|Componentes da hora.|
|`timeZone`|String de fuso horário (ex: `"America/Sao_Paulo"`)|Fuso horário para o qual a data deve ser convertida.|
|`timeZoneName`|`"long"`, `"short"`|Nome do fuso horário a ser exibido.|
|`hour12`|`true` (AM/PM), `false` (24h)|Sistema de horas.|

**Exemplo Prático:**

```js
const dataDoEvento = new Date('2025-12-25T21:00:00Z'); // 21:00 em UTC

// 1. Usando estilos simples para o Brasil
const formatoSimples = new Intl.DateTimeFormat('pt-BR', {
  dateStyle: 'long',
  timeStyle: 'short'
});
console.log(formatoSimples.format(dataDoEvento)); // "25 de dezembro de 2025 às 18:00" (convertido para GMT-3)

// 2. Formato customizado completo
const optionsCompleto = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  hour: 'numeric',
  minute: 'numeric',
  second: 'numeric',
  timeZoneName: 'long',
  timeZone: 'Europe/Paris' // Formatar para o fuso horário de Paris
};

const formatoParis = new Intl.DateTimeFormat('fr-FR', optionsCompleto);
console.log(formatoParis.format(dataDoEvento)); 
// "jeudi 25 décembre 2025 à 22:00:00 heure normale d’Europe centrale"

// 3. O atalho: date.toLocaleString()
// Esta chamada é equivalente ao primeiro exemplo
console.log(dataDoEvento.toLocaleString('pt-BR', { dateStyle: 'long', timeStyle: 'short' }));
```

## Formatando Números e Moedas com `Intl.NumberFormat`

Da mesma forma que o `DateTimeFormat`, o `NumberFormat` nos permite formatar números de acordo com as convenções locais, incluindo separadores de milhar, de decimal e, mais importante, símbolos de moeda.

**Sintaxe:** `new Intl.NumberFormat(locales, options)`

### Formatando Moedas

Este é um dos casos de uso mais cruciais da API. Para formatar moedas, usamos `style: 'currency'` e especificamos a moeda com a opção `currency`.

**Opções de Moeda:**

- `style`: `'currency'`
- `currency`: O código ISO 4217 da moeda (ex: `'BRL'`, `'USD'`, `'EUR'`, `'JPY'`).
- `currencyDisplay` (opcional): Como exibir a moeda.
    - `'symbol'` (padrão): Usa o símbolo da moeda (R$, $).
    - `'code'`: Usa o código ISO (BRL, USD).
    - `'name'`: Usa o nome da moeda ("Real brasileiro", "Dólar americano").

**Exemplo Prático:**

```js
const valor = 123456.789;

// Formatando como Real brasileiro
const formatadorBRL = new Intl.NumberFormat('pt-BR', {
  style: 'currency',
  currency: 'BRL'
});
console.log(formatadorBRL.format(valor)); // "R$ 123.456,79"

// Formatando como Dólar americano
const formatadorUSD = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD'
});
console.log(formatadorUSD.format(valor)); // "$123,456.79"

// Formatando como Iene japonês (que não usa casas decimais)
const formatadorJPY = new Intl.NumberFormat('ja-JP', {
  style: 'currency',
  currency: 'JPY'
});
console.log(formatadorJPY.format(valor)); // "￥123,457"
```

A API lida automaticamente com separadores, casas decimais e posicionamento do símbolo para cada locale.

### Formatando Outros Números

- **Números Decimais (`style: 'decimal'`):** O padrão. Útil para controlar o número de casas decimais.
    
    ```js
    const num = 1234.5;
    const formatadorDecimal = new Intl.NumberFormat('de-DE', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2
    });
    console.log(formatadorDecimal.format(num)); // "1.234,50" (note a convenção alemã)
    ```
    
- **Porcentagens (`style: 'percent'`):** Multiplica o número por 100 e adiciona o símbolo de porcentagem.
    
    ```js
    const porcentagem = 0.85;
    const formatadorPercentual = new Intl.NumberFormat('pt-BR', { style: 'percent' });
    console.log(formatadorPercentual.format(porcentagem)); // "85%"
    ```
    
- **Unidades (`style: 'unit'`):** Formata um número com uma unidade de medida.
    
    ```js
    const velocidade = 80;
    const formatadorUnidade = new Intl.NumberFormat('pt-BR', {
      style: 'unit',
      unit: 'kilometer-per-hour'
    });
    console.log(formatadorUnidade.format(velocidade)); // "80 km/h"
    ```
    

## Outras Ferramentas de Internacionalização

A API `Intl` vai além da formatação.

### `Intl.Collator`: Ordenação Sensível ao Idioma

A ordenação padrão de strings no JavaScript (`Array.prototype.sort()`) é baseada nos valores dos pontos de código Unicode, o que pode produzir resultados incorretos para muitos idiomas. `Intl.Collator` resolve isso.

```js
const nomes = ['Érika', 'Ana', 'Carlos', 'éden'];

// Ordenação padrão incorreta
console.log([...nomes].sort()); // ["Ana", "Carlos", "Érika", "éden"]

// Ordenação correta com Collator para pt-BR
const comparador = new Intl.Collator('pt-BR');
console.log([...nomes].sort(comparador.compare)); // ["Ana", "Carlos", "éden", "Érika"]
```

### `Intl.RelativeTimeFormat`: Formatação de Tempo Relativo

Este construtor é usado para criar descrições de tempo relativas e legíveis por humanos.

```js
const formatadorRelativo = new Intl.RelativeTimeFormat('pt-BR', { numeric: 'auto' });

console.log(formatadorRelativo.format(-1, 'day')); // "ontem"
console.log(formatadorRelativo.format(1, 'day'));  // "amanhã"
console.log(formatadorRelativo.format(2, 'week')); // "em 2 semanas"
console.log(formatadorRelativo.format(-3, 'month'));// "há 3 meses"
```

## Considerações Finais

Neste capítulo, desvendamos a **API de Internacionalização (`Intl`)**, o conjunto de ferramentas nativo do JavaScript para criar aplicações verdadeiramente globais. Vimos que, ao abandonar a manipulação manual de strings, podemos delegar a complexa tarefa de formatação de datas, números e moedas para o motor da linguagem, que possui um conhecimento profundo sobre as convenções de centenas de localidades.

Dominamos os construtores `Intl.DateTimeFormat` e `Intl.NumberFormat`, aprendendo a usar seus ricos objetos de opções para produzir saídas perfeitamente formatadas para qualquer cultura. Fomos além da formatação, explorando como o `Intl.Collator` pode corrigir a ordenação de textos e como o `Intl.RelativeTimeFormat` pode criar descrições de tempo amigáveis.

Usar a API `Intl` não é apenas uma questão de boas práticas; é uma demonstração de respeito pelo seu usuário. Ao apresentar os dados no formato com o qual ele está familiarizado, você remove barreiras de comunicação e cria uma experiência de usuário mais inclusiva e profissional. Com este conhecimento, você está agora capacitado a construir interfaces que falam a língua de cada um de seus usuários, onde quer que eles estejam.
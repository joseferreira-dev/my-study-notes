# Capítulo 12 – Date: Gerenciando Tempo e Datas

Ao longo de nossa jornada pelo JavaScript, dominamos a manipulação de números, textos e estruturas de dados como objetos e arrays. Agora, voltamos nossa atenção para uma das dimensões mais fundamentais da informação em qualquer sistema de software: o **tempo**. Seja para registrar quando um usuário publicou um comentário, agendar uma tarefa para ser executada no futuro, calcular a duração de um evento ou simplesmente exibir a data e a hora atuais, a manipulação de informações temporais é uma necessidade inescapável.

Para lidar com essa complexa tarefa, o JavaScript nos fornece um objeto nativo (built-in) dedicado: o **`Date`**. No entanto, trabalhar com datas e horas é notoriamente complicado. Questões como fusos horários (time zones), horário de verão (daylight saving time), meses com diferentes números de dias e anos bissextos introduzem uma camada de complexidade que pode facilmente levar a bugs sutis e difíceis de rastrear. O objeto `Date` do JavaScript, embora poderoso, possui suas próprias peculiaridades e armadilhas, que todo desenvolvedor precisa conhecer.

Neste capítulo, faremos uma imersão completa no objeto `Date`. Começaremos por desvendar seu funcionamento interno, entendendo o conceito central do **timestamp UTC**, a base sobre a qual todas as operações de data são construídas. Em seguida, exploraremos exaustivamente as diversas maneiras de se criar um objeto `Date`, seja a partir do momento atual, de uma string ou de componentes numéricos específicos. Dedicaremos uma atenção especial aos métodos para obter e modificar partes de uma data, e, crucialmente, vamos nos aprofundar na arte de **formatar datas** para exibição ao usuário, contrastando as abordagens mais antigas com a moderna e poderosa **API de Internacionalização (`Intl`)**. Por fim, abordaremos a aritmética e a comparação de datas, capacitando-o a calcular durações e ordenar eventos cronologicamente. Ao final, você terá o conhecimento necessário para manipular o tempo em JavaScript com confiança e precisão.

## O Conceito Central: O Timestamp da Época

Para entender como o `Date` funciona, é preciso saber que, por baixo dos panos, um objeto `Date` em JavaScript não armazena a data como "28 de Junho de 2025". Internamente, ele representa um único momento no tempo através de um número. Esse número é o **timestamp**: a quantidade de **milissegundos** que se passaram desde um ponto de partida universalmente acordado, o **marco zero (epoch)** da computação moderna.

O marco zero utilizado pelo JavaScript e pela maioria dos sistemas Unix é o **1 de Janeiro de 1970, às 00:00:00, no Tempo Universal Coordenado (UTC)**.

### UTC: A Linguagem Universal do Tempo

O **Tempo Universal Coordenado (UTC)** é o padrão de tempo pelo qual o mundo regula relógios e o tempo. Ele não está sujeito a fusos horários locais ou ao horário de verão. Usar o UTC como base garante que o timestamp `1751194080000` represente o exato mesmo momento no tempo, seja ele interpretado em Recife, em Tóquio ou em Londres. A conversão para a hora local (`-03:00`, `+09:00`, etc.) é uma camada de apresentação que é aplicada sobre esse valor universal.

### Obtendo o Timestamp Atual

Existem duas maneiras principais de obter o timestamp UTC atual em milissegundos:

- **`Date.now()`:** Um método estático que retorna diretamente o número de milissegundos desde a Época. Esta é a forma mais performática e direta.
- **`new Date().getTime()`:** Cria um objeto `Date` para o momento atual e, em seguida, extrai seu timestamp com o método `getTime()`.

```js
// Ambas as formas retornam o mesmo tipo de valor
const timestamp1 = Date.now();
const timestamp2 = new Date().getTime();

console.log(timestamp1); // Ex: 1751194080000
console.log(timestamp2); // Ex: 1751194080001 (um pouco depois)
```

Compreender que toda data em JavaScript é um timestamp UTC é a chave para evitar muitas dores de cabeça relacionadas a fusos horários.

## Criando um Objeto `Date`

O objeto `Date` é criado usando o construtor `new Date()`, que pode ser chamado de várias maneiras.

### `new Date()`: Sem Argumentos

Chamado sem argumentos, o construtor cria um objeto `Date` representando a data e a hora atuais, com base no **fuso horário do sistema** onde o código está sendo executado (o navegador do usuário ou o servidor).

```js
const agora = new Date();
console.log(agora); // Ex: Sat Jun 28 2025 19:48:00 GMT-0300 (Horário Padrão de Brasília)
```

### `new Date(timestamp)`: Com Um Argumento Numérico

Se um único argumento numérico for fornecido, ele é tratado como um timestamp em milissegundos desde a Época UTC.

```js
const dataMarcoZero = new Date(0);
console.log(dataMarcoZero.toUTCString()); // "Thu, 01 Jan 1970 00:00:00 GMT"

const dataEspecifica = new Date(1751194080000);
console.log(dataEspecifica); // Sat Jun 28 2025 19:48:00 GMT-0300
```

Com um timestamp de `0`, o resultado é a própria Época.

### `new Date(dateString)`: Com Uma String

É possível criar um objeto `Date` a partir de uma string. No entanto, esta é a forma **mais perigosa e propensa a erros**. O comportamento do parsing pode variar drasticamente entre diferentes navegadores e motores de JavaScript.

A única forma de string que é confiavelmente parseada de forma consistente é o padrão **ISO 8601 (`YYYY-MM-DDTHH:mm:ss.sssZ`)**.

```js
// Formato ISO 8601 (Recomendado)
const dataISO = new Date('2025-06-28T22:48:00.000Z'); // 'Z' indica UTC
console.log(dataISO);

// Outros formatos (NÃO RECOMENDADOS - podem falhar ou dar resultados inconsistentes)
const dataAmbígua1 = new Date('28/06/2025');
const dataAmbígua2 = new Date('June 28, 2025');
```

**Regra de ouro:** Se você precisa criar uma data a partir de uma string, sempre use o formato ISO 8601.

### `new Date(year, monthIndex, ...)`: Com Componentes Numéricos

Você pode criar uma data especificando seus componentes.

**Sintaxe:** `new Date(year, monthIndex, day, hours, minutes, seconds, ms)`

- `year`: O ano com 4 dígitos.
- `monthIndex`: O índice do mês, de **0 (Janeiro) a 11 (Dezembro)**. Esta é uma das fontes de bugs mais comuns!
- `day` (opcional): O dia do mês (1-31), padrão é 1.
- `hours`, `minutes`, `seconds`, `ms` (opcionais): Padrão é 0.

Os argumentos são interpretados no **fuso horário local do sistema**.

```js
// Criando o Natal de 2025
// Mês 11 = Dezembro
const natal2025 = new Date(2025, 11, 25, 18, 30, 0);
console.log(natal2025);
```

### `Date.UTC(year, monthIndex, ...)`: Criando a Partir de UTC

Este método estático aceita os mesmos argumentos que o construtor de componentes, mas os interpreta como **UTC**. Crucialmente, ele não retorna um objeto `Date`, mas sim um **timestamp** (número) que pode ser usado para criar um.

```js
// Obter o timestamp para meia-noite de 1 de janeiro de 2026, em UTC
const timestampUTC = Date.UTC(2026, 0, 1);

// Criar o objeto Date a partir do timestamp UTC
const anoNovoUTC = new Date(timestampUTC);

console.log(timestampUTC);
console.log(anoNovoUTC.toUTCString()); // "Thu, 01 Jan 2026 00:00:00 GMT"
```

## Obtendo e Modificando Componentes de Data

Uma vez que um objeto `Date` é criado, podemos extrair ou alterar seus componentes usando métodos "getter" e "setter". Todos os métodos getter têm um equivalente em UTC (ex: `getFullYear()` vs. `getUTCFullYear()`).

### Getters: Lendo a Data

- **Ano, Mês, Dia:**
    - `getFullYear()`: Retorna o ano com 4 dígitos.
    - `getMonth()`: Retorna o índice do mês (0-11).
    - `getDate()`: Retorna o dia do mês (1-31).
    - `getDay()`: Retorna o dia da semana (0 para Domingo, 6 para Sábado).
- **Hora, Minuto, Segundo:**
    - `getHours()`: Retorna a hora (0-23).
    - `getMinutes()`: Retorna o minuto (0-59).
    - `getSeconds()`: Retorna o segundo (0-59).
    - `getMilliseconds()`: Retorna os milissegundos (0-999).

```js
const hoje = new Date(); // Ex: Sat Jun 28 2025 19:48:00
const diaDaSemana = ['Domingo', 'Segunda', 'Terça', 'Quarta', 'Quinta', 'Sexta', 'Sábado'];

console.log(`Ano: ${hoje.getFullYear()}`);       // 2025
console.log(`Mês: ${hoje.getMonth() + 1}`);      // 6 (Lembre-se de somar 1)
console.log(`Dia: ${hoje.getDate()}`);          // 28
console.log(`Dia da Semana: ${diaDaSemana[hoje.getDay()]}`); // Sábado
```

### Setters: Alterando a Data

Objetos `Date` são **mutáveis**. Os métodos setter alteram o objeto `Date` original.

- `setFullYear(year, [month], [day])`
- `setMonth(monthIndex, [day])`
- `setDate(day)`
- `setHours(hour, [min], [sec], [ms])`
- E assim por diante para `setMinutes`, `setSeconds`, etc.

Uma característica poderosa dos setters é o **auto-ajuste (roll-over)**. Se você passar um valor fora do intervalo esperado, a data se ajusta automaticamente.

```js
const data = new Date(2025, 5, 28); // 28 de Junho de 2025

// Incrementando um dia
data.setDate(data.getDate() + 1);
console.log(data.toDateString()); // "Sun Jun 29 2025"

// Exemplo de roll-over: avançando 5 dias a partir de 28 de Junho
data.setDate(28 + 5); // 33 de Junho -> 3 de Julho
console.log(data.toDateString()); // "Thu Jul 03 2025"
```

Este comportamento é extremamente útil para fazer cálculos como "a data daqui a 30 dias".

## Formatando Datas para Exibição

A representação padrão de `new Date()` não é amigável para o usuário. Precisamos formatá-la.

### Métodos de Formatação Simples

Estes métodos oferecem pouca ou nenhuma customização, mas são rápidos para depuração ou formatos padrão.

- `toString()`: Formato longo, dependente do navegador e do fuso horário.
- `toDateString()`: Apenas a parte da data. `Sat Jun 28 2025`
- `toTimeString()`: Apenas a parte da hora. `19:48:00 GMT-0300`
- `toISOString()`: Formato universal ISO 8601 em UTC. Essencial para APIs. `2025-06-28T22:48:00.000Z`
- `toUTCString()`: Formato padrão em UTC. `Sat, 28 Jun 2025 22:48:00 GMT`

### A Abordagem Moderna: `Intl.DateTimeFormat` e `.toLocale...()`

Para exibir datas para usuários, a abordagem correta é usar a API de Internacionalização (`Intl`), que lida com as convenções de diferentes idiomas e regiões. Os métodos `toLocaleDateString()`, `toLocaleString()` e `toLocaleTimeString()` são as portas de entrada para essa API.

**Sintaxe:** `date.toLocale...([locales], [options])`

- `locales` (opcional): Uma string ou array de strings indicando o idioma e região (ex: `'pt-BR'`, `'en-US'`).
- `options` (opcional): Um objeto para customizar o formato da saída.

O objeto `options` é incrivelmente poderoso. Vamos explorar suas chaves principais:

|Opção|Valores Possíveis|Descrição|
|---|---|---|
|`dateStyle`|`"full"`, `"long"`, `"medium"`, `"short"`|Define um estilo de formatação predefinido para a data.|
|`timeStyle`|`"full"`, `"long"`, `"medium"`, `"short"`|Define um estilo de formatação predefinido para a hora.|
|`timeZone`|Uma string de fuso horário (ex: `"UTC"`, `"America/Sao_Paulo"`)|Define o fuso horário a ser usado na formatação.|
|`year`|`"numeric"`, `"2-digit"`|Formato do ano.|
|`month`|`"numeric"`, `"2-digit"`, `"long"`, `"short"`, `"narrow"`|Formato do mês (ex: "junho", "jun", "J").|
|`day`|`"numeric"`, `"2-digit"`|Formato do dia.|
|`weekday`|`"long"`, `"short"`, `"narrow"`|Formato do dia da semana (ex: "sábado", "sáb.", "S").|
|`hour`, `minute`, `second`|`"numeric"`, `"2-digit"`|Formato das partes da hora.|
|`hour12`|`true` (para AM/PM), `false` (para 24h)|Define o sistema de horas.|
|`timeZoneName`|`"long"`, `"short"`|Inclui o nome do fuso horário (ex: "Horário Padrão de Brasília", "GMT-3").|

```js
const evento = new Date('2025-12-25T13:00:00Z');

// Formato padrão para o Brasil
console.log(evento.toLocaleString('pt-BR')); // "25/12/2025, 10:00:00" (convertido para GMT-3)

// Formato longo nos EUA
const optionsEUA = {
  timeZone: 'America/New_York',
  dateStyle: 'full',
  timeStyle: 'long',
};
console.log(evento.toLocaleString('en-US', optionsEUA));
// "Thursday, December 25, 2025 at 8:00:00 AM EST"

// Formato customizado
const optionsCustom = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  hour: '2-digit',
  minute: '2-digit',
  timeZoneName: 'short'
};
console.log(evento.toLocaleString('pt-BR', optionsCustom));
// "quinta-feira, 25 de dezembro de 2025, 10:00 GMT-3"
```

## Comparação e Aritmética de Datas

### Comparando Datas

Objetos `Date` podem ser comparados diretamente usando operadores relacionais (`<`, `>`, `<=`, `>=`). Ao fazer isso, o JavaScript converte implicitamente os objetos para seus timestamps (números), permitindo a comparação correta.

```js
const data1 = new Date(2025, 0, 1);
const data2 = new Date(2026, 0, 1);
console.log(data1 < data2); // true
```

**Atenção:** **Não use** `==` ou `===` para comparar dois objetos `Date` diferentes, mesmo que representem o mesmo tempo. Eles são objetos distintos na memória e a comparação de igualdade falhará. Compare seus timestamps.

```js
const d1 = new Date(2025, 5, 28);
const d2 = new Date(2025, 5, 28);

console.log(d1 === d2); // false (objetos diferentes)
console.log(d1.getTime() === d2.getTime()); // true (timestamps iguais)
```

### Calculando a Diferença

Subtrair um objeto `Date` de outro resulta na diferença entre eles em **milissegundos**. Isso é a base para todos os cálculos de duração.

```js
const inicio = new Date('2025-01-01');
const fim = new Date('2025-01-11');

const diferencaEmMs = fim - inicio;
console.log(diferencaEmMs); // 864000000

// Convertendo para dias
const msPorDia = 1000 * 60 * 60 * 24;
const diferencaEmDias = diferencaEmMs / msPorDia;
console.log(diferencaEmDias); // 10
```

## Considerações Finais

Neste capítulo, desvendamos o objeto `Date` do JavaScript, uma ferramenta essencial, porém repleta de nuances. Vimos que sua base é o **timestamp UTC**, um conceito que garante um ponto de referência temporal universal. Exploramos as diversas formas de se instanciar uma data, ressaltando a confiabilidade do formato ISO 8601 em detrimento de strings ambíguas e a importância de lembrar que os meses no construtor são 0-indexados.

Dominamos os métodos para ler e modificar os componentes de uma data, e, mais importante, aprendemos a formatá-la para exibição ao usuário. Contrastamos os métodos de formatação mais antigos e limitados com a moderna e poderosa **API de Internacionalização (`Intl`)**, que deve ser sempre a sua escolha para apresentar datas de forma legível e culturalmente apropriada. Por fim, vimos como realizar comparações e cálculos de duração de forma segura.

Embora uma nova API (`Temporal`) esteja no horizonte para simplificar ainda mais esse trabalho, o domínio sobre o objeto `Date` e suas peculiaridades permanece uma habilidade fundamental para qualquer desenvolvedor JavaScript que precise interagir com o vasto ecossistema de código e APIs existentes.
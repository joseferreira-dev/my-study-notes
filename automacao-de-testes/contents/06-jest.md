# Capítulo 6 – Jest

O **Jest** é um poderoso e popular framework de testes em JavaScript que se destaca por sua simplicidade, configuração mínima e um conjunto abrangente de funcionalidades "prontas para uso". Criado e mantido pelo Facebook, o Jest foi inicialmente desenvolvido para testar a biblioteca React, mas rapidamente evoluiu para se tornar uma solução de teste versátil para qualquer projeto JavaScript.

Neste capítulo, exploraremos os fundamentos do Jest, desde sua filosofia de "zero configuração" até seus recursos mais avançados. Discutiremos suas principais características, como a execução paralela de testes e a geração integrada de relatórios de cobertura de código. Mergulharemos no uso de seus "matchers" para realizar asserções eficazes, cobrindo desde comparações simples até a validação de strings, números, arrays e o tratamento de exceções. Uma parte significativa será dedicada ao teste de código assíncrono, uma realidade comum em JavaScript, e às poderosas funcionalidades de simulação (mocking) do Jest, que são essenciais para isolar unidades de código e testar interações complexas.

## Introdução ao Jest: Simplicidade e Poder nos Testes JavaScript

O **Jest** é um framework de testes em JavaScript projetado com um forte foco na **simplicidade** e na experiência do desenvolvedor. Seu objetivo é tornar os testes uma tarefa agradável e produtiva, funcionando, na maioria dos projetos JavaScript, "fora da caixa" (out-of-the-box), ou seja, com pouca ou nenhuma configuração inicial.

Ele é projetado para garantir a correção de qualquer código JavaScript e funciona perfeitamente com projetos que utilizam tecnologias modernas como:

- Babel
- TypeScript
- Node.js
- React
- Angular
- Vue.js
- E muito mais!

O Jest permite que você escreva testes com uma **API acessível, familiar e rica em recursos**, proporcionando resultados rapidamente. É bem documentado e pode ser facilmente estendido para corresponder aos requisitos específicos de cada projeto.

## Principais Características e Vantagens do Jest

O Jest não é apenas mais um framework de teste; ele traz um conjunto de características que o diferenciam e contribuem para sua popularidade:

- **Execução Paralela e Otimizada:** O Jest é ideal para fazer testes que tenham objetos grandes. Os testes são, por padrão, **paralelos** e executados em seus **próprios processos** para maximizar o desempenho. Ao garantir que seus testes tenham um estado global único, o Jest pode executar testes em paralelo de forma confiável. Para tornar as coisas ainda mais rápidas, o Jest executa primeiro os testes anteriores que falharam e reorganiza a execução com base no tempo que os arquivos de teste demoram para rodar.
- **Cobertura de Código Integrada:** É ideal para criar relatórios de **cobertura de código** facilmente usando a flag `--coverage`. Não há necessidade de configurações complexas ou bibliotecas adicionais! O Jest consegue coletar informações de cobertura de código de projetos inteiros, incluindo arquivos que ainda não foram testados.
- **Simulação (Mocking) Poderosa e Simplificada:** O Jest utiliza um resolvedor personalizado para importações em seus testes, simplificando a simulação de qualquer objeto fora do escopo do seu teste. Você pode usar importações simuladas com a rica API de **Mock Functions** para espionar chamadas de função com uma sintaxe de teste legível.
- **"Tudo em Um" (All in One):** Jest possui todo o conjunto de ferramentas necessárias para a maioria dos cenários de teste em um só lugar: um executor de testes, uma biblioteca de asserções (matchers) e funcionalidades de mocking integradas. Isso reduz a necessidade de configurar e integrar múltiplas bibliotecas.
- **Excelente Experiência do Desenvolvedor:** Com mensagens de erro claras, um modo "watch" interativo que re-executa testes automaticamente ao salvar arquivos, e uma API intuitiva, o Jest foca em tornar a experiência de escrever e rodar testes o mais fluida possível.

## Escrevendo Testes com Jest: A Função `test` e Expectativas

A estrutura básica de um teste no Jest é bastante simples e legível. Os testes são geralmente definidos usando a função global `test` (ou seu alias `it`, para quem prefere uma sintaxe mais BDD).

A sintaxe é: `test(name, fn, timeout)`

- `name` (string): Uma descrição do que o teste está verificando.
- `fn` (function): A função que contém a lógica do teste, incluindo as asserções.
- `timeout` (number, opcional): Um tempo limite em milissegundos para este teste específico.

Dentro da função de teste, você usa a função `expect` para fazer asserções sobre os valores produzidos pelo seu código.

## Matchers: Validando Valores de Diferentes Formas

O Jest usa **"matchers"** para permitir que você teste valores de diferentes maneiras. Eles são métodos que vêm anexados ao objeto retornado pela função `expect()`. A função principal de um matcher é comparar o valor recebido (o `actual`) com um valor esperado ou uma condição.

Quando o Jest é executado, ele rastreia todos os matchers que falharam para que possa imprimir mensagens de erro claras e úteis para você.

A maneira mais simples para testar um valor é com **igualdade exata**:

```javascript
test('dois mais dois é quatro', () => {
  expect(2 + 2).toBe(4);
});
```

Nesse código, `expect(2 + 2)` retorna um objeto de "expectativa". Você normalmente não fará muito com esses objetos de expectativa, exceto chamar "matchers" neles. Nesse código, `.toBe(4)` é o matcher.

Os principais matchers são:

- **`toBe(expected)`**: Utiliza `Object.is` para testar a **igualdade exata** (identidade e valor). Se você quer checar o valor de um objeto ou array (comparação profunda), use `toEqual` em vez disso. `toBe` é usado para testar a igualdade exata entre dois valores, verificando não apenas se são visualmente iguais, mas também se são o mesmo objeto em termos de identidade na memória.
    
    ```javascript
    test('demonstrando toBe', () => {
      const numero = 10;
      const objA = { valor: 1 };
      const objB = objA; // objB referencia o mesmo objeto que objA
    
      expect(numero).toBe(10);
      expect(objA).toBe(objB); // Passa, pois são o mesmo objeto
    
      const objC = { valor: 1 };
      // expect(objA).toBe(objC); // Falharia, pois objA e objC são objetos diferentes, apesar de terem o mesmo conteúdo
    });
    ```

- **`toEqual(expected)`**: Utilizado para verificar se dois valores são **iguais em termos de conteúdo** (comparação recursiva de todas as propriedades de objetos ou elementos de arrays), mesmo que não sejam o mesmo objeto na memória. Isso é especialmente útil quando você está lidando com objetos ou arrays.
    
    ```javascript
    test('atribuição de objeto', () => {
      const data = { one: 1 };
      data['two'] = 2;
      expect(data).toEqual({ one: 1, two: 2 });
    });
    ```

    Explicação do Teste 'atribuição de objeto':
    
    1. `const data = {one: 1};`: Uma variável `data` é criada e inicializada como um objeto com a propriedade `one` valendo `1`.
    2. `data['two'] = 2;`: Uma nova propriedade `two` é adicionada ao objeto `data` com o valor `2`.
    3. `expect(data).toEqual({one: 1, two: 2});`: Esta é a asserção principal. O `expect(data)` cria o objeto de expectativa. O matcher `.toEqual()` compara recursivamente o conteúdo do objeto `data` com o objeto literal `{one: 1, two: 2}`. Se todas as propriedades e seus valores forem iguais, o teste passa. Este teste verifica se a adição da propriedade `two` foi feita corretamente.
    
    É importante notar que o `toEqual` **ignora certas coisas intencionalmente** para tornar os testes mais flexíveis e menos frágeis em alguns casos:
    
    - Chaves de objeto com propriedades `undefined`.
    - Itens de array `undefined`.
    - Esparsidade de array (lacunas ou `undefined` entre elementos).
    - Tipos de objeto incompatíveis (o Jest tentará uma comparação de valor onde fizer sentido, mas se os tipos forem fundamentalmente diferentes e não comparáveis estruturalmente, a comparação falhará como esperado).
    
    No entanto, a afirmação de que `toEqual` ignora "tipo de objeto incompatível" deve ser entendida com cuidado. Se você comparar `{a:1}` com `[1]`, eles não serão iguais. A "ignorância" se aplica mais a detalhes internos ou a `undefined` dentro de estruturas que são, de outra forma, comparáveis.

Você pode negar qualquer matcher usando o prefixador `.not`:

```javascript
test('adição de números positivos não resulta em zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
});
```

Explicação do Teste 'adição de números positivos não resulta em zero':

1. `test('adição de números positivos não resulta em zero', () => { ... });`: Inicia um teste com a descrição indicando que a soma de números positivos não deve ser zero.
2. `for (let a = 1; a < 10; a++) { for (let b = 1; b < 10; b++) { ... } }`: Dois loops aninhados iteram sobre combinações de números `a` e `b` de 1 a 9.
3. `expect(a + b).not.toBe(0);`: Dentro dos loops, esta asserção verifica que a soma `a + b` **não é** (`.not`) estritamente igual (`.toBe()`) a `0`. Como `a` e `b` são sempre positivos neste loop, sua soma nunca será zero, então o teste deve passar para todas as combinações.

## Matchers de Veracidade (Truthiness)

Em testes, às vezes você precisa distinguir entre `undefined`, `null` e `false`, mas em outras ocasiões, pode não querer tratar esses valores de maneira diferente, focando apenas se um valor é "truthy" ou "falsy" em um contexto booleano JavaScript. O Jest contém auxiliares que permitem que você seja explícito sobre o que quer.

Lembre-se:

- **Truthy:** Valores que são considerados `true` em um contexto booleano (ex: `true`, `1`, `"texto"`, `{}`, `[]`).
- **Falsy:** Valores que são considerados `false` em um contexto booleano (ex: `false`, `0`, `""`, `null`, `undefined`, `NaN`).

Os matchers são:

- **`toBeNull()`**: Compara se o valor é estritamente igual a `null`.
    
    ```javascript
    expect(null).toBeNull();         // Passa
    // expect(undefined).toBeNull(); // Falha
    // expect(0).toBeNull();         // Falha
    ```

- **`toBeUndefined()`**: Compara se o valor é estritamente igual a `undefined`.
    
    ```javascript
    let variavelNaoInicializada;
    expect(variavelNaoInicializada).toBeUndefined(); // Passa
    // expect(null).toBeUndefined();                 // Falha
    ```

- **`toBeDefined()`**: O oposto de `toBeUndefined()`. Verdadeiro se o valor não for `undefined`.
    
    ```javascript
    let nome = "Jest";
    expect(nome).toBeDefined();       // Passa
    // expect(variavelNaoInicializada).toBeDefined(); // Falha
    ```

- **`toBeTruthy()`**: Verdadeiro se o valor for avaliado como "truthy".
    
    ```javascript
    expect(true).toBeTruthy();        // Passa
    expect(1).toBeTruthy();           // Passa
    expect("texto").toBeTruthy();     // Passa
    // expect(0).toBeTruthy();         // Falha
    // expect(null).toBeTruthy();      // Falha
    ```

- **`toBeFalsy()`**: Verdadeiro se o valor for avaliado como "falsy".
    
    ```javascript
    expect(false).toBeFalsy();        // Passa
    expect(0).toBeFalsy();            // Passa
    expect("").toBeFalsy();          // Passa
    // expect(1).toBeFalsy();          // Falha
    // expect("jest").toBeFalsy();     // Falha
    ```

**Exemplo Combinado:**

```javascript
function minhaFuncaoQueRetornaAlgo() {
  return "resultado"; // Pode ser qualquer valor não-nulo, não-undefined e truthy
}

test('função retorna valor válido', () => {
  const resultado = minhaFuncaoQueRetornaAlgo();
  expect(resultado).toBeDefined();    // Garante que não seja undefined
  expect(resultado).not.toBeNull();   // Garante que não seja null
  expect(resultado).toBeTruthy();     // Garante que seja "truthy"
});
```

Este teste passará apenas se `minhaFuncaoQueRetornaAlgo` retornar um valor que não seja `null`, não seja `undefined`, e que seja avaliado como "truthy" em um contexto booleano.

## Matchers para Números

Para números, o Jest oferece uma variedade de matchers:

- **`toBeGreaterThan(number)`**
- **`toBeGreaterThanOrEqual(number)`**
- **`toBeLessThan(number)`**
- **`toBeLessThanOrEqual(number)`**

E para igualdade, como já vimos:

- **`toBe(number)`**
- **`toEqual(number)`** (para números, `toBe` e `toEqual` são equivalentes)

**Exemplo Prático:**

```javascript
test('two plus two', () => {
  const value = 2 + 2; // value é 4
  expect(value).toBeGreaterThan(3);              // Passa (4 > 3)
  expect(value).toBeGreaterThanOrEqual(3.5);     // Passa (4 >= 3.5)
  expect(value).toBeGreaterThanOrEqual(4);       // Passa (4 >= 4)
  expect(value).toBeLessThan(5);                 // Passa (4 < 5)
  expect(value).toBeLessThanOrEqual(4.5);        // Passa (4 <= 4.5)
  expect(value).toBeLessThanOrEqual(4);          // Passa (4 <= 4)

  // toBe e toEqual são equivalentes para números
  expect(value).toBe(4);                         // Passa
  expect(value).toEqual(4);                      // Passa
});
```

Explicação do Teste 'two plus two':

1. `const value = 2 + 2;`: A variável `value` recebe `4`.
2. As linhas `expect(value).toBeGreaterThan(3);` e subsequentes verificam se `value` satisfaz as condições de ser maior que 3, maior ou igual a 3.5, menor que 5, e menor ou igual a 4.5.
3. As últimas duas linhas demonstram que para números, `toBe(4)` e `toEqual(4)` têm o mesmo efeito, verificando se `value` é igual a `4`. Este teste garante que a operação básica de soma está correta e que as comparações numéricas funcionam como esperado.

### Números de Ponto Flutuante (Decimais)

Ao lidar com números decimais (ponto flutuante) em testes, é importante utilizar o matcher **`toBeCloseTo(number, numDigits?)`** em vez de `toEqual` ou `toBe`. Isso ocorre porque cálculos com ponto flutuante podem introduzir pequenos erros de arredondamento.

- `numDigits` (opcional): O número de casas decimais a serem verificadas para precisão. O padrão é 2.

```javascript
test('adding floating point numbers', () => {
  const value = 0.1 + 0.2;
  // expect(value).toBe(0.3); // Isso PODE NÃO funcionar devido a erros de arredondamento (0.1 + 0.2 === 0.30000000000000004)
  expect(value).toBeCloseTo(0.3); // Isso funciona, considerando uma pequena margem de erro.
  expect(value).toBeCloseTo(0.300, 3); // Especificando 3 casas decimais de precisão
});
```

Explicação do Teste 'adding floating point numbers': A soma `0.1 + 0.2` em JavaScript pode resultar em um número como `0.30000000000000004` devido à forma como os números de ponto flutuante são representados. Usar `toBe(0.3)` falharia. O `toBeCloseTo(0.3)` compara se `value` é aproximadamente igual a `0.3`, lidando com essas pequenas imprecisões. É a forma correta de testar igualdade com números de ponto flutuante.

## Matchers para Strings

Para verificar strings em relação a expressões regulares, você pode utilizar o matcher **`toMatch(regexpOrString)`**:

```javascript
test('não há "I" em "team"', () => {
  expect('team').not.toMatch(/I/); // Verifica se "team" NÃO contém a letra "I" (maiúscula)
});

test('mas há "stop" em "Christoph"', () => {
  expect('Christoph').toMatch(/stop/); // Verifica se "Christoph" contém a substring "stop"
  expect('Christoph').toMatch('stop');  // Também funciona com uma string literal
});
```

Explicação dos Testes de String: No primeiro teste, `expect('team').not.toMatch(/I/);`, a expressão regular `/I/` busca pela letra "I" maiúscula. O `.not` nega o matcher, então o teste passa porque "team" não contém "I". No segundo teste, `expect('Christoph').toMatch(/stop/);`, a expressão regular `/stop/` (ou a string `'stop'`) verifica a presença da substring "stop" em "Christoph". O teste passa porque "stop" está contido em "Christoph".

## Matchers para Arrays e Iteráveis

Para verificar se um array ou qualquer objeto iterável contém um item específico, use **`toContain(item)`**:

```javascript
const shoppingList = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'milk',
];

test('a lista de compras tem "milk" nela', () => {
  expect(shoppingList).toContain('milk');
  expect(new Set(shoppingList)).toContain('milk'); // Funciona com outros iteráveis como Set
});
```

Explicação do Teste 'a lista de compras tem "milk" nela': Este teste verifica se o array `shoppingList` contém o item `'milk'`. A primeira asserção `expect(shoppingList).toContain('milk');` confirma isso. A segunda asserção demonstra que `toContain` também funciona com outros objetos iteráveis, como um `Set` criado a partir da lista. Esta funcionalidade é valiosa para garantir a presença de elementos específicos em coleções.

## Matchers para Exceções

Para verificar se uma função específica lança uma exceção quando é chamada, utilize **`toThrow(error?)`**:

- `error` (opcional): Pode ser uma string (para verificar se a mensagem de erro contém essa string), uma expressão regular (para casar com a mensagem de erro), um objeto de erro (para verificar o tipo de erro) ou a classe do erro.

É crucial envolver a chamada da função que pode lançar o erro dentro de uma função anônima no `expect`, assim: `expect(() => suaFuncaoQueLancaErro()).toThrow(...)`.

```javascript
function compileAndroidCode() {
  throw new Error('você está utilizando o JDK errado!');
}

test('compilando android ocorre conforme o esperado', () => {
  expect(() => compileAndroidCode()).toThrow(); // Espera-se que qualquer erro seja lançado
  expect(() => compileAndroidCode()).toThrow(Error); // Espera-se um erro que seja uma instância de Error

  // Você também pode utilizar uma string que deve estar contida na mensagem de erro ou uma expressão regular
  expect(() => compileAndroidCode()).toThrow('você está utilizando o JDK errado'); // Mensagem exata (ou contida)
  expect(() => compileAndroidCode()).toThrow(/JDK/); // Mensagem contém "JDK" via regex

  // Para uma mensagem de erro exata usando expressão regular:
  // expect(() => compileAndroidCode()).toThrow(/^você está utilizando o JDK errado$/); // O teste falharia se a mensagem fosse "você está utilizando o JDK errado!"
  expect(() => compileAndroidCode()).toThrow(/^você está utilizando o JDK errado!$/); // O teste passa
});
```

Explicação do Teste 'compilando android ocorre conforme o esperado': A função `compileAndroidCode` é projetada para lançar um `Error`.

1. `expect(() => compileAndroidCode()).toThrow();`: Verifica se a chamada à função lança _qualquer_ exceção.
2. `expect(() => compileAndroidCode()).toThrow(Error);`: Verifica se a exceção lançada é uma instância da classe `Error`.
3. `expect(() => compileAndroidCode()).toThrow('você está utilizando o JDK errado');`: Verifica se a mensagem da exceção _contém_ a string fornecida. Para correspondência exata da mensagem, use uma regex com `^` e `$` como no último exemplo.
4. `expect(() => compileAndroidCode()).toThrow(/JDK/);`: Verifica se a mensagem da exceção corresponde à expressão regular `/JDK/` (ou seja, contém "JDK").
5. `expect(() => compileAndroidCode()).toThrow(/^você está utilizando o JDK errado!$/);`: Verifica se a mensagem da exceção corresponde _exatamente_ à string fornecida, usando uma expressão regular com delimitadores de início (`^`) e fim (`$`).

## Testando Código Assíncrono

Em JavaScript, código assíncrono é muito comum (operações de I/O, chamadas de rede, timers). Jest precisa saber quando uma operação assíncrona que está sendo testada foi concluída antes de prosseguir. Ele oferece várias maneiras de lidar com isso.

É crucial garantir que os testes aguardem a conclusão dessas operações para serem precisos.

### Promises

Se sua função retorna uma **Promise**, a maneira mais simples de testá-la é **retornar essa Promise do seu teste**. O Jest aguardará a Promise ser resolvida. Se a Promise for rejeitada, o teste automaticamente falhará.

```javascript
// Suponha que fetchData() retorna uma Promise:
// function fetchData() {
//   return new Promise(resolve => setTimeout(() => resolve('peanut butter'), 100));
// }

test('a informação é "peanut butter" (com Promise)', () => {
  // Certifique-se de retornar a promise!
  return fetchData().then(data => {
    expect(data).toBe('peanut butter');
  });
});
```

Se você espera que uma Promise seja **rejeitada**, use o método `.catch()`. É importante adicionar `expect.assertions(numero)` para verificar que um certo número de asserções foi chamado. Caso contrário, uma Promise resolvida (cumprida) não falharia o teste.

```javascript
// Suponha que fetchDataRejeita() retorna uma Promise que é rejeitada:
// function fetchDataRejeita() {
//   return new Promise((resolve, reject) => setTimeout(() => reject(new Error('error')), 100));
// }

test('a busca falha com um erro (com Promise e .catch)', () => {
  expect.assertions(1); // Garante que a asserção no catch seja chamada
  return fetchDataRejeita().catch(e => {
    expect(e.message).toMatch('error');
  });
});
```

### Async/Await

Alternativamente, você pode usar `async` e `await` em seus testes. Para escrever um teste assíncrono, use a palavra-chave `async` na frente da função passada para `test`. Então, você pode usar `await` para esperar a resolução da Promise.

```javascript
test('a informação é "peanut butter" (com async/await)', async () => {
  const data = await fetchData();
  expect(data).toBe('peanut butter');
});

test('a busca falha com um erro (com async/await e try/catch)', async () => {
  expect.assertions(1);
  try {
    await fetchDataRejeita();
  } catch (e) {
    expect(e.message).toMatch('error');
  }
});
```

`async` e `await` são, muitas vezes, uma forma mais concisa e legível de lidar com Promises, atuando como "açúcar sintático" sobre a mesma lógica das Promises.

**Cuidado:** Ao usar `async` e `await`, certifique-se de que você está de fato aguardando (`await`) a Promise. Se você omitir o `await`, o teste pode ser concluído antes que a Promise seja resolvida ou rejeitada, levando a resultados incorretos.

### Callbacks

Se o seu código assíncrono usa callbacks em vez de Promises, o Jest também oferece suporte. Para isso, o teste deve aceitar um argumento chamado `done`. O Jest esperará até que a função `done` seja chamada antes de finalizar o teste.

```javascript
// Suponha que fetchDataComCallback(callback) chama callback(error, data)
// function fetchDataComCallback(callback) {
//   setTimeout(() => {
//     // Simula um erro:
//     // callback(new Error('falha no callback'), null);
//     // Simula sucesso:
//     callback(null, 'peanut butter');
//   }, 100);
// }

test('a informação é "peanut butter" (com callback e done)', done => {
  function callback(error, data) {
    if (error) {
      done(error); // Se houver erro, passe para done para falhar o teste
      return;
    }
    try {
      expect(data).toBe('peanut butter');
      done(); // Chame done() para indicar que o teste assíncrono terminou
    } catch (error) {
      done(error); // Se a asserção falhar, passe o erro para done
    }
  }
  fetchDataComCallback(callback);
});
```

Se `done()` nunca for chamado, o teste falhará com um erro de tempo limite (timeout), o que geralmente é o comportamento desejado para operações assíncronas que não se completam. Se a declaração `expect` falhar, ela lançará um erro, e `done()` não será chamado, resultando também em falha (ou, se você usar `try/catch`, o erro da asserção será passado para `done()`, fornecendo uma mensagem de erro mais clara).

### Matchers `.resolves` / `.rejects`

O Jest também fornece os matchers `.resolves` e `.rejects` como uma forma elegante de lidar com Promises diretamente na declaração `expect`. O Jest aguardará a Promise ser resolvida/rejeitada.

- **`.resolves`**: Aguarda a Promise ser resolvida e então permite encadear outros matchers para testar o valor resolvido.
    
    ```javascript
    test('a informação é "peanut butter" (com .resolves)', () => {
      return expect(fetchData()).resolves.toBe('peanut butter');
    });
    ```

- **`.rejects`**: Aguarda a Promise ser rejeitada e então permite encadear outros matchers para testar o motivo da rejeição (o erro).
    
    ```javascript
    test('a busca falha com um erro (com .rejects)', () => {
      return expect(fetchDataRejeita()).rejects.toThrow('error');
      // Ou para casar com a mensagem do erro:
      // return expect(fetchDataRejeita()).rejects.toMatchObject({ message: 'error' });
      // ou expect(fetchDataRejeita()).rejects.toHaveProperty('message', 'error');
    });
    ```

**Importante:** Ao usar `.resolves` ou `.rejects`, você **deve retornar** a declaração `expect`. Se você omitir o `return`, o teste será concluído antes que a Promise seja resolvida ou rejeitada.

Nenhuma dessas formas (Promises com `.then/.catch`, `async/await`, callbacks com `done`, ou `.resolves/.rejects`) é inerentemente superior. A escolha depende da clareza e do estilo que melhor se adapta ao seu projeto e às suas preferências. É comum misturá-las conforme a necessidade.

## Funções de Simulação (Mock Functions)

As **funções de simulação (mock functions)**, frequentemente chamadas apenas de "mocks", são ferramentas essenciais no Jest para testar as interconexões entre diferentes partes do seu código JavaScript. Elas permitem:

- **Substituir a implementação real** de uma função.
- **Capturar as chamadas** feitas à função simulada (incluindo os parâmetros passados).
- Capturar instâncias de funções construtoras quando são instanciadas com o operador `new`.
- **Configurar valores de retorno** de forma programática durante o teste.

Existem duas abordagens principais para utilizar funções de simulação:

1. **Criar uma função de simulação para ser usada no código de teste:** Útil para testar callbacks ou para injetar uma função simulada em uma unidade de código.
2. **Elaborar uma simulação manual para substituir uma dependência de módulo:** Permite substituir um módulo inteiro ou partes dele por uma implementação simulada.

### Criando e Usando uma Mock Function Simples

Vamos considerar um exemplo onde testamos uma função `forEach` que invoca um callback para cada item em um array.

forEach.js (código a ser testado):

```javascript
// export function forEach(items, callback) { // Se usando módulos ES6
function forEach(items, callback) { // Para CommonJS como no exemplo de teste
  for (let index = 0; index < items.length; index++) {
    callback(items[index]);
  }
}
module.exports = forEach; // Se usando CommonJS
```

forEach.test.js (código de teste):

```javascript
const forEach = require('./forEach'); // Importa a função a ser testada

test('forEach mock function', () => {
  const mockCallback = jest.fn(x => 42 + x); // Cria uma mock function
  forEach([0, 1], mockCallback);

  // A função simulada (mockCallback) foi chamada duas vezes
  expect(mockCallback.mock.calls.length).toBe(2);
  // ou expect(mockCallback).toHaveBeenCalledTimes(2);

  // O primeiro argumento da primeira chamada à função foi 0
  expect(mockCallback.mock.calls[0][0]).toBe(0);

  // O primeiro argumento da segunda chamada à função foi 1
  expect(mockCallback.mock.calls[1][0]).toBe(1);

  // O valor de retorno da primeira chamada à função foi 42 (pois 42 + 0)
  expect(mockCallback.mock.results[0].value).toBe(42);

  // O valor de retorno da segunda chamada foi 43 (pois 42 + 1)
  expect(mockCallback.mock.results[1].value).toBe(43);
});
```

Neste teste, `jest.fn(x => 42 + x)` cria uma função simulada que, quando chamada, executa a implementação `x => 42 + x`. O Jest então rastreia todas as chamadas e resultados.

### A Propriedade `.mock`

Todas as funções simuladas criadas com `jest.fn()` possuem uma propriedade especial `.mock`. Esta propriedade armazena informações valiosas sobre como a função foi chamada, o que ela retornou, e o valor de `this` para cada chamada.

- **`mock.calls`**: Um array de arrays. Cada array interno representa uma chamada à função e contém os argumentos passados naquela chamada.
    - `mock.calls.length`: Número de vezes que a função foi chamada.
    - `mock.calls[0][0]`: Primeiro argumento da primeira chamada.
    - `mock.calls[0][1]`: Segundo argumento da primeira chamada, e assim por diante.
    - `mock.lastCall`: Um array com os argumentos da última chamada.

- **`mock.results`**: Um array de objetos, onde cada objeto descreve o resultado de uma chamada. Cada objeto tem:
    - `type`: O tipo de resultado (`'return'` para um retorno normal, `'throw'` para uma exceção).
    - `value`: O valor retornado ou o erro lançado.

- **`mock.instances`**: Um array contendo todas as instâncias de objeto que foram criadas quando a função simulada foi chamada com `new`.
    
    ```javascript
    const MyMockConstructor = jest.fn();
    const a = new MyMockConstructor();
    const b = new MyMockConstructor();
    MyMockConstructor.mock.instances[0] === a; // true
    MyMockConstructor.mock.instances[1] === b; // true
    ```

- **`mock.contexts`**: Um array contendo os valores de `this` para cada chamada à função simulada.
    
    ```javascript
    const myMockFunction = jest.fn();
    const objA = { name: 'A' };
    const objB = { name: 'B' };
    
    myMockFunction.call(objA, 1, 2);
    myMockFunction.call(objB, 'x', 'y');
    
    // myMockFunction.mock.contexts[0] é objA
    // myMockFunction.mock.contexts[1] é objB
    ```

**Exemplos de Asserções com `.mock`:**

```javascript
const someMockFunction = jest.fn(() => 'return value'); // Define um retorno padrão
const element = { name: 'meuElemento' };
someMockFunction.call(element, 'first arg', 'second arg');

// A função foi chamada exatamente uma vez
expect(someMockFunction.mock.calls.length).toBe(1);
// ou, de forma mais semântica:
expect(someMockFunction).toHaveBeenCalledTimes(1);

// O primeiro argumento da primeira chamada à função foi 'first arg'
expect(someMockFunction.mock.calls[0][0]).toBe('first arg');
// ou:
expect(someMockFunction).toHaveBeenCalledWith('first arg', 'second arg');

// O valor de retorno da primeira chamada à função foi 'return value'
expect(someMockFunction.mock.results[0].value).toBe('return value');
// ou:
expect(someMockFunction).toHaveReturnedWith('return value');

// A função foi chamada com um certo contexto `this`: o objeto `element`
expect(someMockFunction.mock.contexts[0]).toBe(element);

// Exemplo com construtor:
const MockConstructor = jest.fn();
const instance1 = new MockConstructor('test');
const instance2 = new MockConstructor();

// Esta função foi instanciada exatamente duas vezes
expect(MockConstructor.mock.instances.length).toBe(2);

// O objeto retornado pela primeira instanciação desta função (this dentro do construtor)
// foi chamado com o argumento 'test'. Se o construtor atribuir `this.name = arg`, então:
// Supondo que MockConstructor faz algo como: this.name = arg;
// expect(MockConstructor.mock.instances[0].name).toBe('test'); // Isso dependeria da implementação interna do construtor simulado.

// O primeiro argumento da última chamada (que foi a segunda instanciação) foi undefined
// expect(MockConstructor.mock.lastCall[0]).toBeUndefined(); // Se o segundo `new MockConstructor()` não passou argumentos
```

### Manipulando Valores de Retorno

As funções simuladas permitem controlar seus valores de retorno, o que é essencial para testar diferentes caminhos no seu código.

- **`mockReturnValue(value)`**: Define um valor que a função simulada retornará **sempre** que for chamada.
- **`mockReturnValueOnce(value)`**: Define um valor que a função simulada retornará **apenas na próxima vez** que for chamada. Pode ser encadeado para definir uma sequência de retornos.

```javascript
const myMock = jest.fn();
console.log(myMock()); // Por padrão, retorna undefined -> undefined

myMock.mockReturnValueOnce(10)
      .mockReturnValueOnce('x')
      .mockReturnValue(true); // Retorno padrão para chamadas subsequentes

console.log(myMock()); // -> 10
console.log(myMock()); // -> 'x'
console.log(myMock()); // -> true
console.log(myMock()); // -> true (continua retornando o último mockReturnValue)
```

Essas funções são muito eficazes em códigos que seguem um estilo funcional de passagem de continuação (onde funções são passadas como callbacks para manipular resultados). Em vez de criar **stubs** (implementações substitutas) complexos, você pode injetar valores diretamente no teste.

```javascript
const filterTestFn = jest.fn();

// Configura a mock para retornar true na primeira chamada, e false na segunda
filterTestFn.mockReturnValueOnce(true).mockReturnValueOnce(false);

const result = [11, 12].filter(num => filterTestFn(num));

console.log(result); // -> [11] (porque filterTestFn(11) retornou true, filterTestFn(12) retornou false)
console.log(filterTestFn.mock.calls[0][0]); // Argumento da primeira chamada -> 11
console.log(filterTestFn.mock.calls[1][0]); // Argumento da segunda chamada -> 12
```

**Observação Importante:** Ao usar funções simuladas, resista à tentação de implementar lógica complexa dentro delas, a menos que seja estritamente necessário para o comportamento que você quer simular. O foco deve ser testar a unidade de código em questão, não a lógica do mock em si.

### Simulação de Módulos (Mocking Modules)

A simulação de módulos é uma prática valiosa para isolar o código sob teste de suas dependências externas (como chamadas de API, acesso a banco de dados, ou outros módulos do seu próprio projeto), garantindo testes rápidos, determinísticos e robustos.

Considere uma classe `Users` que usa a biblioteca `axios` para buscar dados de usuários de uma API:

users.js:

```javascript
import axios from 'axios';

class Users {
  static all() {
    return axios.get('/users.json').then(resp => resp.data);
  }
}

export default Users;
// Ou para CommonJS:
// const axios = require('axios');
// class Users { ... }
// module.exports = Users;
```

Para testar o método `Users.all()` sem fazer uma chamada real à API, usamos `jest.mock('axios')`. Isso automaticamente substitui o módulo `axios` por uma versão simulada. Em seguida, podemos controlar o comportamento dos métodos simulados de `axios`, como `axios.get`.

users.test.js:

```javascript
import axios from 'axios'; // Este será o axios simulado
import Users from './users';

jest.mock('axios'); // Simula automaticamente o módulo axios

test('deve buscar usuários', () => {
  const users = [{ name: 'Bob' }];
  const resp = { data: users };

  axios.get.mockResolvedValue(resp); // Configura axios.get para retornar uma Promise resolvida com 'resp'
  // Alternativa:
  // axios.get.mockImplementation(() => Promise.resolve(resp));

  // Retorna a promise para que o Jest espere sua conclusão
  return Users.all().then(data => expect(data).toEqual(users));
});
```

Aqui, `axios.get.mockResolvedValue(resp)` faz com que qualquer chamada a `axios.get` (dentro do escopo deste teste) retorne uma Promise que resolve com o objeto `resp` que definimos. Isso nos permite testar a lógica de `Users.all()` de forma isolada.

### Implementações de Funções Simuladas (Mock Implementations)

Às vezes, apenas especificar valores de retorno não é suficiente. Pode ser necessário substituir completamente a **implementação** de uma função simulada.

- **`mockImplementation(fn)`**: Substitui a implementação da função simulada pela função `fn` fornecida.
    
    ```javascript
    const myMockFn = jest.fn(cb => cb(null, true)); // Implementação inicial opcional
    myMockFn((err, val) => console.log(val)); // -> true (usando a implementação inicial)
    
    myMockFn.mockImplementation(cb => cb(null, false)); // Substitui a implementação
    myMockFn((err, val) => console.log(val)); // -> false
    ```
    
    Isso é muito útil quando você simula um módulo e precisa definir o comportamento de uma de suas funções exportadas.
    
    foo.js (módulo a ser simulado):
    
    ```javascript
    module.exports = function() {
      // alguma implementação original
      return 'original';
    };
    ```
    
    test.js:
    
    ```javascript
    jest.mock('../foo'); // Simula automaticamente o módulo foo
    const foo = require('../foo'); // foo agora é uma mock function
    
    // Substitui a implementação original de foo
    foo.mockImplementation(() => 42);
    console.log(foo()); // -> 42
    ```

- **`mockImplementationOnce(fn)`**: Similar ao `mockReturnValueOnce`, permite definir uma implementação para a **próxima chamada** da função simulada. Pode ser encadeado.
    
    ```javascript
    const myMockFn = jest.fn(() => 'default') // Implementação padrão
      .mockImplementationOnce(() => 'first call')
      .mockImplementationOnce(() => 'second call');
    
    console.log(myMockFn()); // -> 'first call'
    console.log(myMockFn()); // -> 'second call'
    console.log(myMockFn()); // -> 'default' (as implementações 'Once' se esgotaram)
    console.log(myMockFn()); // -> 'default'
    ```

- **`.mockReturnThis()`**: Uma API simplificada para métodos que são tipicamente encadeados (chainable methods) e, portanto, precisam sempre retornar a própria instância (`this`).
    
    ```javascript
    const myObj = {
      myMethod: jest.fn().mockReturnThis(),
    };
    
    // É o mesmo que:
    // const otherObj = {
    //   myMethod: jest.fn(function() {
    //     return this;
    //   }),
    // };
    
    const result = myObj.myMethod().myMethod(); // Funciona porque myMethod retorna 'this' (myObj)
    expect(myObj.myMethod).toHaveBeenCalledTimes(2);
    ```

Dominar as funções de simulação e suas diversas formas de configuração é uma das chaves para escrever testes unitários eficazes e isolados com o Jest.

## Considerações Finais

Neste capítulo, exploramos o Jest, um framework de testes JavaScript que se consolidou como uma das principais escolhas para desenvolvedores devido à sua simplicidade, performance e conjunto robusto de funcionalidades. Vimos que o Jest vai além de um simples executor de testes, oferecendo uma experiência "tudo em um" com matchers integrados, um poderoso sistema de simulação (mocking) e geração de cobertura de código sem a necessidade de configurações complexas.

Discutimos as principais características que tornam o Jest atraente, como sua execução paralela de testes, a otimização da ordem de execução e sua ampla compatibilidade com o ecossistema JavaScript moderno, incluindo Babel, TypeScript e diversos frameworks de frontend e backend.

Aprofundamo-nos no uso dos "matchers" do Jest, que fornecem uma API expressiva para realizar asserções sobre diferentes tipos de dados e condições – desde igualdade exata e equivalência de objetos até a verificação de veracidade, comparações numéricas, correspondência de strings com expressões regulares, conteúdo de arrays e o tratamento de exceções.

Dedicamos uma atenção especial ao desafio de testar código assíncrono, detalhando as abordagens que o Jest oferece, como o retorno de Promises, o uso de `async/await`, o tratamento de callbacks com a função `done`, e os convenientes matchers `.resolves` e `.rejects`.

Finalmente, exploramos as versáteis funções de simulação (mock functions) do Jest, incluindo a propriedade `.mock` para inspecionar chamadas, a manipulação de valores de retorno com `mockReturnValueOnce` e `mockReturnValue`, a simulação de módulos inteiros com `jest.mock`, e a substituição de implementações com `mockImplementation` e `mockImplementationOnce`. Essas funcionalidades são cruciais para isolar unidades de código e testar interações complexas de forma eficaz.

Com sua filosofia de "zero configuração" e foco na experiência do desenvolvedor, o Jest se estabelece como uma ferramenta que não apenas garante a qualidade do código, mas também torna o processo de escrita de testes mais agradável e produtivo. O conhecimento adquirido sobre o Jest é, sem dúvida, um ativo valioso para qualquer desenvolvedor JavaScript que busca construir aplicações robustas e confiáveis.
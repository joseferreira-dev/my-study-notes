# Capítulo 26 – Tratamento de Erros: Construindo Código Resiliente

Em nossa jornada pelo JavaScript, aprendemos a construir lógicas complexas, a manipular dados e a orquestrar operações assíncronas. Escrevemos funções, classes e módulos que, em um mundo ideal, executariam suas tarefas sem falhas. No entanto, o desenvolvimento de software acontece no mundo real — um ambiente imperfeito e imprevisível. Usuários inserem dados inválidos, requisições de rede falham, APIs retornam resultados inesperados, e, por vezes, nossa própria lógica contém bugs. Ignorar essa realidade é a receita para criar aplicações frágeis que quebram de forma silenciosa ou inesperada, frustrando usuários e tornando a depuração um pesadelo.

É aqui que entra uma das disciplinas mais críticas da engenharia de software: o **Tratamento de Erros (Error Handling)**. Tratar erros não é um recurso opcional ou algo a ser adicionado no final; é uma parte integral do design de um sistema robusto e confiável. Trata-se de antecipar problemas, ter um plano para quando eles ocorrerem e garantir que a aplicação possa se recuperar graciosamente ou, no mínimo, falhar de uma maneira informativa e controlada. O JavaScript, como uma linguagem moderna, fornece um mecanismo poderoso para lidar com essas situações excepcionais através do lançamento e da captura de **exceções**.

Neste capítulo, vamos dominar a arte de gerenciar erros em JavaScript. Começaremos por entender o que é um erro e como o mecanismo de exceções funciona. Dissecaremos a estrutura fundamental do `try...catch...finally`, o bloco de construção para todo tratamento de erros síncrono. Faremos um mergulho profundo no objeto `Error` e seus vários subtipos nativos, aprendendo a identificar o que deu errado. Em seguida, vamos explorar como podemos criar e lançar nossos próprios erros customizados para representar falhas específicas do nosso domínio de negócio. Finalmente, abordaremos a ordem de operações, o fluxo de uma exceção através da pilha de chamadas e as melhores práticas para garantir que seu código não seja apenas funcional, mas também resiliente.

## A Natureza dos Erros em JavaScript

Em JavaScript, podemos classificar os erros em duas categorias principais:

- **Erros de Sintaxe (Syntax Errors):** São erros que violam as regras gramaticais da linguagem. Eles são detectados pelo motor do JavaScript durante a fase de _parsing_ (análise), **antes** que qualquer linha de código seja executada. Um erro de sintaxe impede que o script inteiro seja executado. Exemplos incluem parênteses faltantes, palavras-chave digitadas incorretamente ou uma vírgula no lugar errado.
    
    ```js
    // Erro de Sintaxe: 'function' escrito incorretamente
    functio minhaFuncao() { // SyntaxError: Unexpected identifier
      console.log("Olá");
    }
    ```
    
    Você verá esses erros diretamente no console do seu navegador ou no terminal antes mesmo de o programa rodar.
    
- **Erros de Tempo de Execução (Runtime Errors ou Exceções):** São erros que ocorrem **durante** a execução do código. A sintaxe está correta, mas uma operação inválida é tentada. Por exemplo, tentar chamar uma função que não existe, acessar uma propriedade de `null`, ou usar uma variável que não foi declarada. É para este tipo de erro que as estratégias de tratamento de erros, como o `try...catch`, são projetadas.

## O Bloco `try...catch...finally`: O Mecanismo Central

A forma primária de se lidar com erros de tempo de execução em JavaScript é através do bloco `try...catch...finally`. Ele permite que você "tente" executar um código que pode falhar e, caso uma falha ocorra, "capture" o erro para tratá-lo sem quebrar a aplicação.

### Anatomia do Bloco

- **`try`:** Envolve o código "protegido". Você coloca aqui qualquer operação que você suspeita que possa lançar uma exceção.
- **`catch (erro)`:** Este bloco só é executado se uma exceção for lançada dentro do bloco `try`. A execução do `try` é imediatamente interrompida, e o fluxo do programa salta para o `catch`. Ele recebe como argumento um objeto (geralmente um `Error` object) que contém informações sobre a falha.
- **`finally`:** Este bloco é **opcional**, mas extremamente útil. O código dentro de `finally` é executado **sempre**, independentemente do que aconteceu:
    - Se o `try` for concluído com sucesso.
    - Se o `try` lançar um erro e o `catch` for executado.
    - Mesmo que haja um `return` dentro do `try` ou `catch`.

É o local ideal para código de "limpeza", como fechar conexões com bancos de dados, remover listeners de eventos ou esconder um indicador de carregamento.

**Exemplo Prático: Analisando JSON Inválido**

```js
const jsonInvalido = '{"nome": "Ana", "idade": 30,}'; // Vírgula extra torna o JSON inválido

function parsearUsuario(json) {
  try {
    console.log("Tentando analisar o JSON...");
    const usuario = JSON.parse(json);
    console.log("Usuário analisado com sucesso:", usuario.nome);
    return usuario;
  } catch (erro) {
    // Captura o SyntaxError lançado por JSON.parse
    console.error("Ocorreu um erro ao analisar o JSON!");
    console.error(`Tipo do Erro: ${erro.name}`);
    console.error(`Mensagem: ${erro.message}`);
    // Retorna um valor padrão ou relança o erro
    return null;
  } finally {
    console.log("Bloco de análise concluído (sucesso ou falha).");
  }
}

parsearUsuario(jsonInvalido);
```

## O Objeto `Error` e seus Subtipos

Quando uma exceção é lançada, o objeto que é passado para o bloco `catch` geralmente é uma instância de `Error` ou de um de seus subtipos. Este objeto contém informações cruciais para a depuração.

**Propriedades Padrão:**

- **`name`:** Uma string que identifica o tipo do erro (ex: `"TypeError"`).
- **`message`:** Uma string descritiva fornecida pelo desenvolvedor ou pelo sistema.

**Propriedades Não-Padrão Comuns:**

- **`stack`:** Uma string contendo a "pilha de chamadas" (stack trace), mostrando o caminho que o código percorreu para chegar ao ponto do erro. É a ferramenta mais valiosa para depuração.

### Tipos de Erro Nativos

O JavaScript define vários tipos de erro nativos que herdam de `Error`. Reconhecê-los ajuda a diagnosticar problemas rapidamente.

- **`SyntaxError`:** Ocorre quando o motor do JavaScript encontra código que viola a sintaxe da linguagem. Geralmente, não pode ser capturado por `try...catch`, pois impede a execução do script. Um caso em que pode ser capturado é com `JSON.parse()`, como vimos.
    
- **`ReferenceError`:** Lançado quando se tenta usar uma variável que não foi declarada.
    
    ```js
    try {
      console.log(variavelInexistente);
    } catch (e) {
      console.error(e.name); // "ReferenceError"
    }
    ```
    
- **`TypeError`:** Lançado quando um valor não é do tipo esperado. Por exemplo, tentar chamar algo que não é uma função, ou acessar propriedades de `null` ou `undefined`.
    
    ```js
    try {
      const obj = null;
      obj.propriedade = 10;
    } catch (e) {
      console.error(e.name); // "TypeError"
    }
    ```
    
- **`RangeError`:** Lançado quando se tenta passar um número como argumento para uma função que está fora de seu intervalo de valores permitidos.
    
    ```js
    try {
      const arr = new Array(-1); // O tamanho de um array não pode ser negativo
    } catch (e) {
      console.error(e.name); // "RangeError"
    }
    ```
    
- **`URIError`:** Lançado quando uma função de manipulação de URI (como `decodeURIComponent()`) é usada de forma incorreta.
- **`EvalError` e `InternalError`:** São mais raros. `EvalError` não é mais usado nas versões modernas do JavaScript. `InternalError` é lançado por alguns motores (como o do Firefox) para erros internos, como uma recursão muito profunda que estoura a pilha de chamadas.

## Lançando suas Próprias Exceções com `throw`

Você não está limitado a capturar os erros do sistema; você pode e deve lançar seus próprios erros para sinalizar condições excepcionais em sua lógica de aplicação. A instrução `throw` para a execução da função atual e começa a "borbulhar" o erro para cima na pilha de chamadas até que um `catch` o capture.

É uma prática recomendada sempre lançar uma instância de `Error` (ou de uma classe que herda de `Error`).

**Exemplo: Validação em uma Função**

```js
function dividir(a, b) {
  if (b === 0) {
    // Lançando um erro para uma condição inválida
    throw new Error("Divisão por zero não é permitida.");
  }
  return a / b;
}

try {
  const resultado = dividir(10, 0);
  console.log(resultado);
} catch (erro) {
  console.error("Falha na operação:", erro.message);
}
```

### Criando Erros Customizados

Para uma arquitetura de software mais avançada, criar seus próprios tipos de erro, estendendo a classe `Error`, é uma prática excelente. Isso permite que você crie uma hierarquia de erros específica para sua aplicação e os capture de forma mais granular.

```js
// Criando um erro customizado para falhas de validação
class ValidationError extends Error {
  constructor(message) {
    super(message); // Chama o construtor da classe pai (Error)
    this.name = "ValidationError";
  }
}

// Criando um erro customizado para falhas de API
class APIError extends Error {
  constructor(message, status) {
    super(message);
    this.name = "APIError";
    this.status = status;
  }
}

function salvarUsuario(usuario) {
  if (!usuario.nome) {
    throw new ValidationError("O campo 'nome' é obrigatório.");
  }
  if (!usuario.email) {
    throw new ValidationError("O campo 'email' é obrigatório.");
  }
  // ...lógica para salvar, que pode lançar um APIError...
}

try {
  salvarUsuario({ email: 'teste@example.com' });
} catch (erro) {
  // Agora podemos tratar cada tipo de erro de forma diferente
  if (erro instanceof ValidationError) {
    console.error("Erro de Validação:", erro.message);
    // Exibir mensagem de erro para o usuário na UI
  } else if (erro instanceof APIError) {
    console.error(`Erro de API (Status ${erro.status}):`, erro.message);
    // Tentar novamente ou mostrar uma mensagem de erro genérica
  } else {
    console.error("Erro inesperado:", erro);
    // Erro genérico que não previmos
  }
}
```

## Ordem de Operações e o Fluxo da Exceção

É crucial entender como uma exceção viaja pelo seu código.

1. Quando uma instrução `throw` é executada, ou um erro de tempo de execução ocorre, a execução da função atual é imediatamente interrompida.
2. O motor do JavaScript começa a "desenrolar" a **pilha de chamadas (call stack)**. Ele volta para a função que chamou a função atual.
3. Ele verifica se essa chamada estava dentro de um bloco `try`. Se sim, a execução salta para o bloco `catch` correspondente e o processo para.
4. Se a chamada não estava em um `try`, o motor continua a desenrolar a pilha, subindo para a função anterior, e repetindo o processo.
5. Se o erro chegar ao topo da pilha de chamadas sem nunca ter sido capturado, ele se torna uma **exceção não capturada (uncaught exception)**. Em um navegador, isso resultará em uma mensagem de erro no console e a interrupção da execução daquele script. Em um ambiente como o Node.js, por padrão, isso irá encerrar o processo.

## Considerações Finais

Neste capítulo, desvendamos o mecanismo de tratamento de erros do JavaScript, uma parte essencial da construção de aplicações que não são apenas funcionais, mas também estáveis e confiáveis. Vimos que o tratamento de erros não é sobre prevenir falhas, mas sobre ter uma estratégia robusta para quando elas inevitavelmente ocorrerem.

Dominamos o bloco **`try...catch...finally`**, nossa principal ferramenta para lidar com exceções de forma controlada. Exploramos o objeto **`Error`** e seus subtipos nativos, aprendendo a diagnosticar problemas com base em seu `name` e `message`. O passo mais importante foi aprender a lançar nossas próprias exceções com `throw` e a criar **erros customizados**, uma técnica que eleva a qualidade do nosso código, permitindo um tratamento de falhas muito mais específico e significativo.

Lembre-se: um erro não tratado é um ponto cego em sua aplicação. Uma exceção capturada e bem gerenciada é uma oportunidade de fornecer feedback útil ao usuário, de registrar o problema para análise futura e de manter a aplicação funcionando de forma previsível. Com as ferramentas e os padrões deste capítulo, você está agora equipado para transformar potenciais desastres em experiências de usuário controladas e em código resiliente.
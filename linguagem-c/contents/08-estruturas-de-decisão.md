# Capítulo 8 – Estruturas de Decisão

No capítulo anterior, desvendamos o universo dos valores booleanos e das expressões lógicas, que são a base para a tomada de decisões em qualquer programa de computador. Compreendemos como as linguagens C e C++ representam e avaliam condições de verdadeiro ou falso (o paradigma "zero/não-zero" do C e o tipo `bool` nativo do C++). Agora, daremos um passo adiante, explorando as ferramentas que nos permitem utilizar esses resultados booleanos para efetivamente direcionar o fluxo de execução de nossos programas: as **estruturas de decisão**.

As estruturas de decisão, também conhecidas como estruturas condicionais, são construções da linguagem que permitem que um programa escolha entre diferentes caminhos de execução com base no resultado de uma ou mais condições. Sem elas, nossos programas executariam sempre a mesma sequência de instruções, de cima para baixo, tornando-se inflexíveis e limitados. Com as estruturas de decisão, podemos criar softwares que reagem a entradas do usuário, adaptam-se a diferentes estados (como um erro ou um sucesso) e implementam lógicas complexas.

Neste capítulo, investigaremos as principais estruturas de decisão disponíveis em C e C++. Começaremos com a instrução `if`, que permite a execução condicional de um bloco de código. Em seguida, avançaremos para a estrutura `if-else`, que oferece uma alternativa quando a condição inicial não é satisfeita. Exploraremos também o encadeamento `if-else if-else` para cenários com múltiplas condições e os perigos dos `if`s aninhados (o problema do _dangling else_). Finalmente, apresentaremos a instrução `switch`, uma alternativa elegante para selecionar entre diversos casos baseados no valor de uma expressão integral.

## Estrutura `if` (Seleção Simples)

A estrutura de decisão mais fundamental em C e C++ é a instrução **`if`**. Ela permite que um bloco de código seja executado **somente se** uma determinada condição for verdadeira. Se a condição for falsa, o bloco de código associado ao `if` é simplesmente ignorado, e o programa continua a execução a partir da próxima instrução após o bloco `if`.

**Pseudocódigo:**

```
SE (condicao for verdadeira) ENTÃO
    // Executa este bloco de código
FIM-SE
// Continua a execução aqui
```

**Sintaxe em C/C++:**

```c
if (condicao) {
    // Bloco de código a ser executado
    // se a 'condicao' for verdadeira
}
// Próxima instrução após o if
```

Vamos dissecar esta estrutura:

- **`condicao`**: É a expressão booleana (ou qualquer expressão) que será avaliada.
    - **Em C**, a regra é: se a `condicao` avaliar para `0` (zero), é **falsa**. Se avaliar para _qualquer outro valor_ (não-zero, seja `1`, `-10`, `42`), é **verdadeira**.
    - **Em C++**, a `condicao` é formalmente do tipo `bool` (`true` or `false`), mas a linguagem mantém a compatibilidade com C: `0` converte para `false` e qualquer valor não-zero converte para `true`.
- **Bloco de código `{...}`**: Uma ou mais instruções que serão executadas se a condição for verdadeira.

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int idade;
    printf("Digite sua idade: ");
    scanf("%d", &idade);

    // 1. Condição Verdadeira
    if (idade >= 18) {
        printf("Você é maior de idade.\n");
        printf("Já pode tirar sua CNH.\n");
    }

    // 2. Condição Falsa
    if (idade < 12) {
        printf("Você é uma criança.\n"); // Esta mensagem não será impressa se idade=20
    }
    
    // 3. O clássico erro de atribuição (BUG!)
    if (idade = 10) { // ATRIBUIÇÃO, não comparação
        // 1. 'idade' recebe o valor 10.
        // 2. O valor da *expressão de atribuição* é 10.
        // 3. 'if' avalia 10 (não-zero), que é VERDADEIRO.
        printf("Esta linha será impressa SEMPRE, e sua idade agora é %d!\n", idade);
    }
    // O correto seria: if (idade == 10)

    printf("Fim do programa.\n");
    return 0;
}
```

Neste exemplo, se o usuário digitar `20`:

1. A primeira condição (`20 >= 18`) é verdadeira, e seu bloco é executado.
2. A segunda condição (`20 < 12`) é falsa, e seu bloco é pulado.
3. A terceira condição (`idade = 10`) é um bug, mas é avaliada como verdadeira (10), e seu bloco é executado, alterando o valor de `idade`.

## Estrutura `if-else` (Seleção Dupla)

A instrução `if` é útil, mas limitada. Frequentemente, não queremos apenas _fazer algo_ se a condição for verdadeira, mas também _fazer outra coisa_ se a condição for falsa. Para isso, utilizamos a estrutura **`if-else`**. Ela oferece dois caminhos de execução mutuamente exclusivos: exatamente um deles será escolhido.

**Pseudocódigo:**

```
SE (condicao for verdadeira) ENTÃO
    // Executa o Bloco A
SENÃO
    // Executa o Bloco B
FIM-SE
```

**Sintaxe em C/C++:**

```c
if (condicao) {
    // Bloco de código A: executado se 'condicao' for verdadeira
} else {
    // Bloco de código B: executado se 'condicao' for falsa
}
// Próxima instrução após o if-else
```

Se a `condicao` for verdadeira, o Bloco A é executado e o Bloco B é pulado. Se a `condicao` for falsa, o Bloco A é pulado e o Bloco B é executado.

**Exemplo em C (Par ou Ímpar):**

```c
#include <stdio.h>

int main() {
    int numero;
    printf("Digite um número inteiro: ");
    scanf("%d", &numero);

    // O operador % (módulo) retorna o resto da divisão.
    // Se o resto da divisão por 2 for 0, o número é par.
    if (numero % 2 == 0) {
        // Bloco A
        printf("%d é um número PAR.\n", numero);
    } else {
        // Bloco B
        // Se o resto não é 0 (ou seja, é 1), a condição é falsa.
        printf("%d é um número ÍMPAR.\n", numero);
    }

    return 0;
}
```

Neste exemplo, é impossível que ambas as mensagens sejam impressas. O programa é forçado a escolher um dos dois caminhos.

## Estrutura `if-else if-else` (Seleção Múltipla Encadeada)

Quando precisamos escolher entre _mais_ de dois caminhos, podemos encadear múltiplas estruturas `if` e `else`. Isso é comumente feito usando a construção **`if-else if-else`**.

O programa avalia as condições em sequência, de cima para baixo. Assim que uma condição verdadeira é encontrada, o bloco de código associado a ela é executado, e _todas_ as condições e blocos `else if` ou `else` subsequentes são **ignorados**. Se nenhuma das condições `if` ou `else if` for verdadeira, o bloco `else` final (se presente) é executado como um "pega-tudo".

**Pseudocódigo:**

```
SE (condicao1 for verdadeira) ENTÃO
    // Executa o Bloco A
SENÃO SE (condicao2 for verdadeira) ENTÃO
    // Executa o Bloco B
SENÃO SE (condicao3 for verdadeira) ENTÃO
    // Executa o Bloco C
SENÃO
    // Executa o Bloco Padrão (Default)
FIM-SE
```

**Exemplo em C (Conceito de Notas):**

```c
#include <stdio.h>

int main() {
    int nota;
    printf("Digite a nota (0 a 100): ");
    scanf("%d", &nota);

    if (nota >= 90 && nota <= 100) {
        printf("Conceito: A\n");
    } else if (nota >= 80) { // Já sabemos que é < 90
        printf("Conceito: B\n");
    } else if (nota >= 70) { // Já sabemos que é < 80
        printf("Conceito: C\n");
    } else if (nota >= 60) { // Já sabemos que é < 70
        printf("Conceito: D\n");
    } else if (nota >= 0) { // Já sabemos que é < 60
        printf("Conceito: F (Reprovado)\n");
    } else {
        // Bloco 'default'
        printf("Nota inválida!\n");
    }

    return 0;
}
```

**Rastreando o Exemplo (se `nota = 75`):**

1. `if (75 >= 90 && 75 <= 100)` -> `(FALSO && VERDADEIRO)` -> **FALSO**.
2. `else if (75 >= 80)` -> **FALSO**.
3. `else if (75 >= 70)` -> **VERDADEIRO**.
4. O bloco `printf("Conceito: C\n");` é executado.
5. _O resto da cadeia é totalmente ignorado_. O programa pula para depois do `else` final.

### Armadilha: A Ordem das Condições Importa!

Em uma cadeia `if-else if`, a ordem é crítica. Se as condições forem colocadas na ordem errada, o programa terá um bug lógico, mesmo que a sintaxe esteja correta.

Veja o mesmo exemplo com a ordem invertida (incorreta):

```
// EXEMPLO INCORRETO
if (nota >= 0) { // Esta é a primeira condição
    printf("Conceito: F (Reprovado)\n");
} else if (nota >= 60) {
    printf("Conceito: D\n");
} else if (nota >= 70) {
    printf("Conceito: C\n");
} 
// ...
```

Se o usuário digitar `95`, a primeira condição (`95 >= 0`) será **VERDADEIRA**. O programa imprimirá "Conceito: F (Reprovado)" e pulará o resto da cadeia. A lógica está quebrada. A regra é: **sempre teste da condição mais específica para a mais geral, ou garanta que os intervalos não se sobreponham.**

## Problema do `if` Aninhado e o Dangling Else

Podemos colocar estruturas `if` dentro de outras, criando **ninhos de decisão** (`nested if statements`).

```c
#include <stdio.h>
#include <stdbool.h>

int main() {
    bool usuario_valido = true;
    bool senha_correta = false;

    if (usuario_valido == true) {
        printf("Usuário OK. Verificando senha...\n");
        
        // 'if' aninhado
        if (senha_correta == true) {
            printf("Acesso permitido!\n");
        } else {
            printf("Senha incorreta!\n");
        }
        
    } else {
        printf("Usuário inválido.\n");
    }
    return 0;
}
// Saída:
// Usuário OK. Verificando senha...
// Senha incorreta!
```

### Dangling Else (`else` Solto)

O ninho de `if`s, combinado com a regra de chaves opcionais, cria uma das ambiguidades mais famosas do C: o _dangling else_.

Considere este código (a indentação é intencionalmente enganosa):

```
int a = 10;
int b = 5;

if (a > 5)
    if (b > 10)
        printf("a > 5 E b > 10\n");
else // <-- Este 'else' pertence a qual 'if'?
    printf("Condição externa falhou\n"); // A indentação sugere que pertence ao 'if (a > 5)'
```

A **Regra do Dangling Else** em C/C++ é: *Um `else` sempre se associa ao `if` mais próximo (o mais interno) que ainda não tenha um `else`.

No exemplo acima:

1. O `else` está "solto" (dangling).
2. O `if` mais próximo dele que não tem um `else` é `if (b > 10)`.
3. Portanto, o compilador interpreta o código assim:
    
    ```c
    if (a > 5) { // if externo
        if (b > 10) { // if interno
            printf("a > 5 E b > 10\n");
        } else { // O else pertence ao if(b > 10)
            printf("Condição externa falhou\n"); 
        }
    }
    ```
    
4. **Execução:** `a > 5` é verdadeiro. O `if` interno é executado. `b > 10` (5 > 10) é falso. O `else` _interno_ é executado.
5. **Saída Inesperada:** `Condição externa falhou` (mesmo que a condição externa `a > 5` tenha sido _verdadeira_!).

Este bug é evitado pelo uso de chaves.

### Regra de Ouro: Sempre Use Chaves `{}`

Nas estruturas `if`, `else if` e `else`, se o bloco de código a ser executado condicionalmente contiver **apenas uma única instrução**, as chaves `{}` são tecnicamente opcionais.

```c
if (x > 5)
    printf("x é maior que 5.\n"); // Válido
```

No entanto, esta prática é perigosa e a fonte de inúmeros bugs, como o _dangling else_ e o erro de adição de linha:

```c
// ERRO LÓGICO COMUM
int y = 3;
if (y < 5)
    printf("y é menor que 5.\n"); // Esta linha ESTÁ no if
    printf("Este é o valor de y: %d\n", y); // Esta linha NÃO ESTÁ no if!
```

A indentação engana. O compilador só associa a primeira instrução `printf` ao `if`. A segunda `printf` será executada _sempre_, independentemente da condição.

Para evitar todos esses problemas e garantir a clareza, adote como **regra de ouro**:

> **Sempre utilize chaves `{}` para delimitar os blocos de código** associados às estruturas `if`, `else if` e `else`, mesmo que o bloco contenha apenas uma única instrução.

**Código Correto e Seguro (Resolvendo o Dangling Else):**

```c
if (a > 5) {
    if (b > 10) {
        printf("a > 5 E b > 10\n");
    }
} else { // Agora, este 'else' SÓ pode pertencer ao 'if (a > 5)'
    printf("Condição externa falhou\n");
}
```

## Estrutura `switch` (Seleção Múltipla com Casos)

A instrução `switch` oferece uma alternativa mais limpa e, às vezes, mais eficiente para longas cadeias de `if-else if-else`.

O `switch` **não** funciona com qualquer condição. Ele é limitado a testar uma **expressão integral** (qualquer expressão que resulte em um `int`, `char`, `short`, `long`, ou `enum`) contra uma lista de **valores constantes**.

**Pseudocódigo:**

```
ESCOLHA (expressao)
    CASO valor1:
        // Bloco de código 1
        PARE
    CASO valor2:
        // Bloco de código 2
        PARE
    CASO PADRÃO:
        // Bloco padrão
FIM-ESCOLHA
```

**Sintaxe em C/C++:**

```c
switch (expressao_integral) {
    case valor_constante1:
        // Bloco de código para valor_constante1
        break; 
    case valor_constante2:
        // Bloco de código para valor_constante2
        break;
    
    // ... (mais casos) ...
    
    default: // Opcional
        // Bloco de código se nenhum caso corresponder
        break;
}
```

Vamos dissecar esta estrutura:

- **`expressao_integral`**: A expressão (ex: uma variável) cujo valor será testado. **Não pode ser `float`, `double` ou `string`**. Tipos `char` são permitidos porque são promovidos a `int`.
- **`case valor_constanteN:`**: Cada `case` define um rótulo com um valor constante (ex: `case 5:`, `case 'A':`). O valor _deve_ ser uma constante conhecida em tempo de compilação (não pode ser uma variável).
- **`break;`**: Esta instrução é **crucial**. Ela causa uma saída imediata da estrutura `switch`.
- **`default:`**: O rótulo `default` é opcional. Este bloco é executado se o valor da `expressao_integral` não corresponder a _nenhum_ dos rótulos `case`.

### Lógica do Fall-Through (Queda)

O `switch` tem um comportamento único: se a instrução `break` for omitida de um `case`, a execução "cai" (fall-through) e **continua executando as instruções do próximo `case`** (e dos seguintes) até encontrar um `break` ou o final do `switch`.

Isso geralmente é um bug, mas pode ser usado intencionalmente para agrupar casos.

**Exemplo em C (Menu de Opções):**

```c
#include <stdio.h>

int main() {
    char opcao;
    printf("Menu:\nA - Adicionar\nB - Buscar\nC - Remover\nEscolha: ");
    scanf(" %c", &opcao);

    switch (opcao) {
        case 'A': // Fall-through intencional
        case 'a':
            printf("Você escolheu Adicionar.\n");
            break; // Sai do switch

        case 'B':
        case 'b':
            printf("Você escolheu Buscar.\n");
            break; // Sai do switch

        case 'C':
        case 'c':
            printf("Você escolheu Remover.\n");
            break; // Sai do switch

        default:
            printf("Opção inválida!\n");
            // break; (opcional aqui, pois é o último)
    }

    return 0;
}
```

Neste exemplo, se o usuário digitar `a`, a execução pula para `case 'a':`. Não há `break`, então ela "cai" para `case 'A':`, executa o `printf` e encontra o `break`.

### `if-else if` vs. `switch`

Quando usar cada um?

| **Característica** | **if-else if-else**                                      | **switch**                                                                                         |
| ------------------ | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Testa...**       | Condições lógicas complexas                              | Valores exatos                                                                                     |
| **Tipos de Dados** | Qualquer tipo (`int`, `float`, `bool`, `string` em C++). | Apenas tipos integrais (`int`, `char`, `enum`).                                                    |
| **Exemplo**        | `if (nota >= 90)` (Intervalo)                            | `case 1:` (Valor exato)                                                                            |
| **Legibilidade**   | Pode ficar confuso com muitos `else if`.                 | Muito legível para listas de opções (ex: menu).                                                    |
| **Eficiência**     | Avaliação sequencial (lenta se a opção for a última).    | Otimizado. O compilador pode criar uma _jump table_ (tabela de saltos), que é O(1) (muito rápido). |

**Regra geral:** Use `switch` quando tiver uma variável integral para testar contra múltiplos valores exatos. Use `if-else if` para todo o resto (intervalos, floats, strings, condições lógicas complexas).

### Notas de C++: `switch` com `enum class`

Em C++, `switch` é frequentemente usado com `enum class` (enums fortemente tipados), o que torna o código muito mais seguro e legível.

```cpp
#include <iostream>

enum class Cor { Vermelho, Verde, Azul };

int main() {
    Cor minhaCor = Cor::Verde;

    switch (minhaCor) {
        case Cor::Vermelho:
            std::cout << "A cor é vermelha.\n";
            break;
        case Cor::Verde:
            std::cout << "A cor é verde.\n"; // Executa este
            break;
        case Cor::Azul:
            std::cout << "A cor é azul.\n";
            break;
        // O compilador (com warnings) pode avisar se um 'default'
        // é omitido quando nem todos os 'enum' foram tratados.
    }
    
    // C++17 introduziu o atributo [[fallthrough]];
    // para silenciar warnings quando o fall-through é intencional.
    switch (minhaCor) {
        case Cor::Vermelho:
            std::cout << "Não é azul...\n";
            [[fallthrough]]; // Indica que o fall-through é intencional
        case Cor::Verde:
            std::cout << "É vermelho ou verde!\n";
            break;
        case Cor::Azul:
            std::cout << "É azul!\n";
            break;
    }
    return 0;
}
```

## Considerações Finais

Neste capítulo, mergulhamos nas estruturas de decisão que conferem inteligência e flexibilidade aos nossos programas. Iniciamos com a instrução `if`, a forma mais simples de executar código condicionalmente, e exploramos a armadilha comum da atribuição (`=`) versus comparação (`==`). Em seguida, avançamos para a estrutura `if-else`, que nos permite escolher entre dois caminhos alternativos, e o encadeamento `if-else if-else`, para lidar com múltiplas condições de forma sequencial, destacando a importância da ordem das condições.

Analisamos o conceito de `if`s aninhados e o clássico problema de ambiguidade do "dangling else", o que nos levou à regra de ouro de **sempre usar chaves `{}`** para delimitar blocos de código, garantindo clareza e evitando erros lógicos.

Finalmente, apresentamos a instrução `switch` como uma ferramenta poderosa para seleção múltipla baseada em valores integrais constantes. Detalhamos seus componentes (`case`, `break`, `default`) e seu comportamento único de "fall-through". Comparamos as vantagens e desvantagens do `switch` em relação ao `if-else if`, concluindo que `switch` é ideal para valores discretos, enquanto `if` é necessário para intervalos e condições complexas.

O domínio dessas estruturas de decisão é um pilar essencial da programação. Elas são as ferramentas que permitem aos seus programas reagir a diferentes entradas e adaptar-se a diferentes estados. O próximo passo natural em nossa jornada será explorar as estruturas que nos permitirão _repetir_ blocos de código: as **estruturas de repetição** (loops).
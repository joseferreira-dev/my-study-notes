# Capítulo 7 – Booleanos

Nos capítulos anteriores, exploramos os diversos tipos de dados que nos permitem representar números e texto, bem como os operadores para manipulá-los. Agora, voltaremos nossa atenção para um conceito fundamental que permeia quase toda a lógica de programação: os **valores booleanos**. Em muitas situações, um programa precisa tomar decisões ou avaliar condições que resultam em uma de apenas duas possibilidades: algo é verdadeiro ou é falso; uma condição foi satisfeita ou não; uma operação deve prosseguir ou ser interrompida.

Este capítulo se dedica a entender como as linguagens C e C++ representam e utilizam esses valores lógicos. Começaremos analisando como a linguagem C, em suas versões mais tradicionais, simula o comportamento booleano utilizando tipos inteiros e como o padrão C99 introduziu uma maneira mais formal de lidar com eles. Em seguida, veremos como o C++ nativamente suporta um tipo de dado booleano (`bool`) com os literais `true` e `false`, simplificando a escrita de código lógico.

Exploraremos também o conceito de **expressões booleanas**, que são construções que avaliam para um resultado verdadeiro ou falso, frequentemente utilizando operadores de comparação e lógicos. Embora as estruturas de controle de fluxo como `if-else` sejam o principal campo de aplicação dos booleanos e serão detalhadas em capítulos futuros, este capítulo fornecerá a base conceitual necessária para entender como essas decisões são fundamentadas.

## Representação de Booleanos em C

A linguagem C, em suas versões originais (anteriores ao padrão C99), não possuía um tipo de dado booleano dedicado. Em vez disso, a lógica booleana era (e ainda pode ser) implementada utilizando tipos inteiros, seguindo uma convenção simples e eficaz:

- O valor inteiro **`0` (zero)** é interpretado como **falso**.
- Qualquer valor inteiro **diferente de zero** é interpretado como **verdadeiro**.

Essa convenção é aplicada em todos os contextos onde um valor lógico é esperado, como nas condições de estruturas de controle (`if`, `while`, `for`).

**Exemplo usando inteiros como booleanos em C:**

```c
#include <stdio.h>

int main() {
    int idade = 20;
    int temPermissao = 1; // 1 representa verdadeiro
    int estaBloqueado = 0; // 0 representa falso

    if (idade >= 18) {
        printf("Maior de idade (usando comparação direta que resulta em 0 ou 1).\n");
    }

    if (temPermissao) { // 'temPermissao' é 1 (verdadeiro)
        printf("Acesso permitido.\n");
    }

    if (!estaBloqueado) { // '!estaBloqueado' é !0, que é verdadeiro (1)
        printf("Não está bloqueado.\n");
    }

    return 0;
}
```

Quando operadores relacionais (como `>=`) e lógicos (como `!`) são usados em C, eles também produzem `0` para falso e `1` para verdadeiro.

### O Padrão C99 e `<stdbool.h>`

Com o padrão C99, a linguagem C introduziu uma forma mais explícita e legível de trabalhar com valores booleanos através do arquivo de cabeçalho `<stdbool.h>`. Este cabeçalho define:

- O tipo `_Bool` (que é um tipo inteiro capaz de armazenar 0 ou 1).
- A macro `bool` como um sinônimo para `_Bool`.
- As macros `true` como uma constante inteira `1`.
- As macros `false` como uma constante inteira `0`.

Para utilizar esses recursos, basta incluir o cabeçalho:

```c
#include <stdbool.h> // Necessário para bool, true, false em C99
```

**Exemplo usando `<stdbool.h>` em C (requer compilador C99 ou posterior):**

```c
#include <stdio.h>
#include <stdbool.h> // Para bool, true, false

int main() {
    bool estaChovendo = true;
    bool diaEnsolarado = false;

    if (estaChovendo) {
        printf("Pegue um guarda-chuva (usando stdbool.h).\n");
    }

    if (!diaEnsolarado) {
        printf("Não parece estar ensolarado (usando stdbool.h).\n");
    }

    // Internamente, true é 1 e false é 0
    printf("Valor de estaChovendo (como int): %d\n", estaChovendo); // Saída: 1
    printf("Valor de diaEnsolarado (como int): %d\n", diaEnsolarado); // Saída: 0

    return 0;
}
```

Mesmo com `<stdbool.h>`, a lógica subjacente de 0 para falso e não-zero para verdadeiro ainda se aplica em muitos contextos de avaliação de condições em C.

## O Tipo `bool` em C++

O C++ simplifica e formaliza o conceito de booleanos ao introduzir nativamente a palavra-chave **`bool`** como um tipo de dado fundamental. Uma variável do tipo `bool` pode armazenar apenas dois valores: os literais **`true`** e **`false`**.

**Declaração e Uso:**

```cpp
#include <iostream>

int main() {
    bool estaChovendo = true;
    bool estaFrio = false;

    // Ao imprimir, std::cout por padrão exibe 1 para true e 0 para false
    std::cout << "Está chovendo? " << estaChovendo << std::endl; // Saída: Está chovendo? 1
    std::cout << "Está frio? " << estaFrio << std::endl;     // Saída: Está frio? 0

    // Para imprimir "true" ou "false" textualmente:
    std::cout << std::boolalpha;
    std::cout << "Está chovendo (texto)? " << estaChovendo << std::endl; // Saída: Está chovendo (texto)? true
    std::cout << "Está frio (texto)? " << estaFrio << std::endl;     // Saída: Está frio (texto)? false

    return 0;
}
```

O manipulador `std::boolalpha` (do cabeçalho `<iostream>` ou `<ios>`) instrui `std::cout` a imprimir os valores booleanos como as palavras "true" ou "false". Para reverter ao comportamento padrão (0 ou 1), pode-se usar `std::noboolalpha`.

As variáveis booleanas em C++ são frequentemente utilizadas em estruturas de controle de fluxo, como os condicionais `if-else`, onde a decisão de executar um bloco de código é baseada em uma condição booleana:

```cpp
#include <iostream>

int main() {
    bool estaChovendo = true;
    std::cout << std::boolalpha; // Para imprimir true/false

    if (estaChovendo) { // A condição é diretamente o valor booleano
        std::cout << "Pegue um guarda-chuva." << std::endl;
    } else {
        std::cout << "Não se esqueça do seu óculos de sol." << std::endl;
    }
    return 0;
}
```

## Expressões Booleanas

Uma **expressão booleana** é qualquer expressão em C ou C++ que, quando avaliada, resulta em um valor lógico – ou seja, falso ou verdadeiro. Essas expressões são a espinha dorsal da lógica condicional e da tomada de decisões em programas.

Elas são tipicamente formadas utilizando:

1. **Operadores de Comparação:** Para comparar valores (ex: `x > y`, `idade == 18`).
2. **Operadores Lógicos:** Para combinar ou negar outras expressões booleanas (ex: `condicao1 && condicao2`, `!temErro`).
3. **Variáveis Booleanas ou Inteiras:** O próprio valor de uma variável `bool` (em C++) ou de uma variável inteira (em C, onde 0 é falso e não-zero é verdadeiro) pode ser uma expressão booleana.
4. **Chamadas de Funções:** Funções que retornam `bool` (em C++) ou um inteiro interpretável como booleano (em C).

**Exemplos com Operadores de Comparação:** Os operadores de comparação, como vimos no Capítulo 3, retornam `1` (verdadeiro) ou `0` (falso) em C, e `true` ou `false` em C++.

```c
// Exemplo em C
#include <stdio.h>

int main() {
    int x = 10;
    int y = 9;

    // (x > y) avalia para 1 (verdadeiro), porque 10 é maior que 9
    printf("(x > y) resulta em: %d\n", (x > y));

    // (10 > 9) também avalia para 1
    printf("(10 > 9) resulta em: %d\n", (10 > 9));

    // Usando o operador igual a (==)
    printf("(x == 10) resulta em: %d\n", (x == 10)); // 1 (verdadeiro)
    printf("(10 == 15) resulta em: %d\n", (10 == 15)); // 0 (falso)
    return 0;
}
```

```cpp
// Exemplo em C++
#include <iostream>

int main() {
    int x = 10;
    int y = 9;

    std::cout << std::boolalpha; // Para imprimir true/false

    // (x > y) avalia para true, porque 10 é maior que 9
    std::cout << "(x > y) resulta em: " << (x > y) << std::endl;

    // (10 > 9) também avalia para true
    std::cout << "(10 > 9) resulta em: " << (10 > 9) << std::endl;

    // Usando o operador igual a (==)
    std::cout << "(x == 10) resulta em: " << (x == 10) << std::endl; // true
    std::cout << "(10 == 15) resulta em: " << (10 == 15) << std::endl; // false
    return 0;
}
````

**Exemplo Prático: Verificação de Idade para Votação**

Vamos considerar um exemplo prático onde precisamos determinar se uma pessoa tem idade para votar. Em muitos lugares, a idade mínima é 18 anos.

**Em C:**

```c
#include <stdio.h>
#include <stdbool.h> // Para C99 bool, true, false

int main() {
    int minhaIdade = 25;
    int idadeParaVotar = 18;

    bool podeVotar = (minhaIdade >= idadeParaVotar);

    if (podeVotar) {
        printf("Com %d anos, você pode votar! (Resultado booleano: %d)\n", minhaIdade, podeVotar);
    } else {
        printf("Com %d anos, você ainda não pode votar. (Resultado booleano: %d)\n", minhaIdade, podeVotar);
    }
    return 0;
}
```

**Em C++:**

```cpp
#include <iostream>

int main() {
    int minhaIdade = 25;
    int idadeParaVotar = 18;

    bool podeVotar = (minhaIdade >= idadeParaVotar);

    std::cout << std::boolalpha;
    if (podeVotar) {
        std::cout << "Com " << minhaIdade << " anos, você pode votar! (Resultado booleano: " << podeVotar << ")" << std::endl;
    } else {
        std::cout << "Com " << minhaIdade << " anos, você ainda não pode votar. (Resultado booleano: " << podeVotar << ")" << std::endl;
    }
    return 0;
}
```

Ambos os programas produzirão uma saída indicando que uma pessoa com 25 anos pode votar, pois a expressão `(minhaIdade >= idadeParaVotar)` é verdadeira.

Como mencionado nas suas notas, uma abordagem ainda melhor seria envolver o código acima em uma estrutura `if...else` mais completa para executar ações diferentes dependendo do resultado, o que já foi demonstrado nos exemplos. A lógica booleana é a base para essas estruturas de controle, que serão o foco de um capítulo subsequente.

## Considerações Finais

Neste capítulo, exploramos o conceito de valores booleanos e como eles são tratados nas linguagens C e C++. Vimos que, enquanto o C tradicional utiliza a convenção de inteiros (0 para falso, não-zero para verdadeiro), o padrão C99 introduziu `<stdbool.h>` para uma representação mais explícita. O C++, por sua vez, oferece o tipo `bool` nativo com os literais `true` e `false`, proporcionando maior clareza e segurança de tipo.

Discutimos a formação de expressões booleanas através de operadores de comparação e lógicos, que são essenciais para a construção da lógica de tomada de decisão em qualquer programa. Embora a aplicação principal dos booleanos seja nas estruturas de controle de fluxo, a compreensão de como esses valores são gerados e interpretados é um pré-requisito fundamental.

Com o domínio dos booleanos e das expressões lógicas, estamos agora mais preparados para mergulhar nas estruturas que permitem aos nossos programas desviar seu fluxo de execução, tomar decisões e repetir tarefas – os comandos de controle de fluxo, que serão o tema do nosso próximo capítulo.
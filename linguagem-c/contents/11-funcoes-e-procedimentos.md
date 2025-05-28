# Capítulo 11 – Funções e Procedimentos

À medida que os programas crescem em tamanho e complexidade, a organização do código torna-se um fator crucial para a sua legibilidade, manutenção e desenvolvimento eficiente. Escrever todo o código de uma aplicação extensa dentro de uma única e longa função `main()` rapidamente se tornaria caótico e impraticável. É aqui que o conceito de **funções** (e, por extensão conceitual, **procedimentos**) desempenha um papel transformador.

As funções são os blocos de construção fundamentais da programação estruturada em C e são igualmente centrais em C++. Elas nos permitem decompor um problema complexo em tarefas menores, mais gerenciáveis e autocontidas. Cada função é projetada para realizar uma operação específica, recebendo dados de entrada (parâmetros), processando-os e, opcionalmente, retornando um resultado. Essa abordagem de **modularização** traz inúmeros benefícios, incluindo maior clareza do código, facilidade de depuração, reutilização de código (uma função bem escrita pode ser chamada várias vezes de diferentes partes do programa) e a possibilidade de desenvolvimento colaborativo, onde diferentes programadores podem trabalhar em funções distintas de forma independente.

Neste capítulo, exploraremos em profundidade o universo das funções. Aprenderemos como definir uma função em C, especificando seu tipo de retorno, nome e parâmetros. Veremos como chamar (ou invocar) uma função e como os dados são passados para ela através de argumentos. Discutiremos a importância da instrução `return` para devolver valores e o significado de funções `void` (que não retornam valor, agindo como procedimentos). Abordaremos também a necessidade das declarações de função, ou protótipos, para informar o compilador sobre a existência de uma função antes de sua utilização. Por fim, faremos uma transição para o C++, destacando como ele herda e expande os conceitos de funções do C, introduzindo funcionalidades como sobrecarga de funções e argumentos padrão.

## O Que São Funções?

Uma **função**, em programação, é um bloco de código nomeado, projetado para executar uma tarefa específica e bem definida. Pense em uma função como uma "sub-rotina" ou um "mini-programa" dentro do seu programa principal. Ela pode ser chamada (ou "invocada") a partir de outros locais do código sempre que essa tarefa específica precisar ser realizada.

As principais características e propósitos de uma função incluem:

- **Encapsulamento:** Agrupa um conjunto de instruções relacionadas sob um único nome, escondendo os detalhes de sua implementação interna.
- **Reutilização:** Uma vez definida, uma função pode ser chamada múltiplas vezes, de diferentes partes do programa, evitando a duplicação de código.
- **Modularidade:** Permite dividir um programa grande em partes menores e mais fáceis de gerenciar, testar e entender.
- **Abstração:** Permite que você use uma função sem precisar saber exatamente **como** ela realiza sua tarefa, apenas **o que** ela faz, quais entradas ela espera e qual saída ela produz.

Em C, o termo "função" é usado para descrever tanto sub-rotinas que retornam um valor quanto aquelas que não retornam (que em algumas outras linguagens seriam chamadas de "procedimentos" ou "sub-rotinas"). A distinção é feita pelo tipo de retorno da função.

## Definindo uma Função em C

A definição de uma função em C especifica o que a função faz. Ela consiste em um cabeçalho e um corpo.

A sintaxe geral para definir uma função em C é:

```c
tipo_de_retorno nome_da_funcao(tipo_param1 nome_param1, tipo_param2 nome_param2, ...) {
    // Corpo da função:
    // Declarações de variáveis locais
    // Instruções para realizar a tarefa da função
    // return valor_de_retorno; // Se a função não for void
}
```

Componentes da definição de uma função:

1. **`tipo_de_retorno`**: O tipo de dado do valor que a função retornará para quem a chamou. Pode ser qualquer tipo de dado válido em C (`int`, `float`, `char`, etc.). Se a função não retorna nenhum valor, o tipo de retorno é `void`.
2. **`nome_da_funcao`**: Um identificador válido que nomeia a função. Este nome será usado para chamar a função.
3. **Lista de Parâmetros (entre parênteses `()`):** Define as variáveis (parâmetros formais) que a função espera receber como entrada quando for chamada. Cada parâmetro deve ter seu tipo e nome especificados. Se a função não recebe nenhum parâmetro, os parênteses podem conter a palavra-chave `void` ou simplesmente estarem vazios (embora `void` seja mais explícito em C para indicar ausência de parâmetros).
4. **Corpo da Função (entre chaves `{}`):** Contém as declarações de variáveis locais e as instruções executáveis que realizam a tarefa específica da função.
5. **Instrução `return` (opcional se `void`, obrigatória se não `void`):** Se a função tem um `tipo_de_retorno` diferente de `void`, ela deve usar a instrução `return` para enviar um valor de volta ao código que a chamou. O tipo do `valor_de_retorno` deve ser compatível com o `tipo_de_retorno` declarado para a função. Para funções `void`, `return;` (sem valor) pode ser usado para sair da função prematuramente, ou pode ser omitido se a execução chegar naturalmente ao final do corpo da função.

**Exemplo 1: Função `void` sem parâmetros (procedimento)** Esta função simplesmente imprime uma mensagem.

```c
#include <stdio.h>

// Definição da função
void saudar() {
    printf("Olá! Bem-vindo ao programa.\n");
}

int main() {
    saudar(); // Chamada da função
    return 0;
}
```

**Exemplo 2: Função com parâmetros e com retorno** Esta função recebe dois inteiros e retorna sua soma.

```c
#include <stdio.h>

// Definição da função
int somar(int a, int b) { // a e b são parâmetros formais
    int resultado;
    resultado = a + b;
    return resultado; // Retorna o valor da soma
}

int main() {
    int num1 = 10, num2 = 5;
    int soma_total;

    soma_total = somar(num1, num2); // Chamada da função, num1 e num2 são argumentos

    printf("A soma de %d e %d é: %d\n", num1, num2, soma_total);
    return 0;
}
```

## Chamando uma Função

Para executar o código dentro de uma função, você precisa **chamar** (ou **invocar**) a função. Uma chamada de função consiste no nome da função seguido por parênteses `()`. Se a função espera parâmetros, os valores (ou variáveis) a serem passados para a função, chamados de **argumentos**, são listados dentro dos parênteses, separados por vírgulas, na ordem correspondente aos parâmetros formais.

```c
nome_da_funcao(argumento1, argumento2, ...);
```

Quando uma função é chamada:

1. O fluxo de controle do programa é transferido para o início da função chamada.
2. Os valores dos argumentos passados na chamada são copiados para os parâmetros formais correspondentes da função (para a passagem por valor padrão do C).
3. As instruções dentro do corpo da função são executadas.
4. Se a função encontrar uma instrução `return valor;`, ela termina, e o `valor` é enviado de volta para o local da chamada. O fluxo de controle retorna para a instrução imediatamente após a chamada da função.
5. Se a função for `void` e não tiver `return;`, ou se a execução chegar ao final do corpo da função `void`, ela termina, e o controle retorna.

**Exemplo de Chamada:** Retomando o exemplo da função `somar`:

```c
int main() {
    int valor_x = 7;
    int valor_y = 3;
    int resultado_soma;

    // Chamando a função 'somar' e armazenando o valor retornado
    resultado_soma = somar(valor_x, valor_y); 
    // 'valor_x' é o argumento para o parâmetro 'a'
    // 'valor_y' é o argumento para o parâmetro 'b'

    printf("Resultado: %d\n", resultado_soma); // Saída: Resultado: 10

    // A função pode ser chamada diretamente dentro de outra expressão
    printf("Outra soma: %d\n", somar(100, 200)); // Saída: Outra soma: 300
    return 0;
}
// ... (definição da função somar como antes)
int somar(int a, int b) {
    return a + b;
}
```

## Tipo de Retorno e a Instrução `return`

O **tipo de retorno** de uma função especifica o tipo de dado do valor que a função enviará de volta ao seu chamador.

- **Funções que Retornam um Valor:** Se uma função é declarada com um tipo de retorno diferente de `void` (por exemplo, `int`, `float`, `char`, `double`, etc.), ela **deve** usar a instrução `return` para devolver um valor daquele tipo. A execução da função termina imediatamente quando uma instrução `return` é encontrada.
    
    ```c
    float calcular_media(int a, int b) {
        float media = (a + b) / 2.0f; // Usar 2.0f para garantir divisão de ponto flutuante
        return media; // Retorna um valor do tipo float
    }
    ```
    
    O valor retornado pode ser usado em atribuições, expressões ou como argumento para outras funções:
    
    ```c
    float m = calcular_media(10, 11); // m recebe 10.5
    printf("Média: %.2f\n", calcular_media(5, 6));
    ```
    
- **Funções `void` (Procedimentos):** Se uma função é declarada com o tipo de retorno `void`, ela não retorna nenhum valor. Tais funções são frequentemente chamadas de **procedimentos** em outras linguagens, pois realizam uma ação ou tarefa sem produzir um resultado direto para ser usado em uma expressão.
    
    ```c
    void exibir_mensagem(char msg[]) {
        printf("Mensagem: %s\n", msg);
        // Não há 'return valor;' aqui, ou pode haver 'return;' para sair
    }
    ```
    
    Em uma função `void`, a instrução `return;` (sem um valor) pode ser usada para terminar a execução da função prematuramente. Se não houver `return;`, a função termina quando a última instrução em seu corpo é executada.
    
    ```c
    void processar_numero(int n) {
        if (n < 0) {
            printf("Número negativo não é processado.\n");
            return; // Sai da função se n for negativo
        }
        printf("Processando o número: %d\n", n);
        // ... (mais processamento)
    }
    ```
    

## Parâmetros de Função (Argumentos)

Os **parâmetros formais** são as variáveis declaradas na lista de parâmetros da definição de uma função. Eles atuam como espaços reservados para os valores que serão passados para a função quando ela for chamada. Os **argumentos reais** (ou simplesmente argumentos) são os valores ou expressões fornecidas na chamada da função, que são copiados para os parâmetros formais.

### Passagem por Valor (Padrão em C para Tipos Simples)

Na linguagem C, para tipos de dados básicos (como `int`, `float`, `char`) e estruturas, o mecanismo padrão de passagem de argumentos é a **passagem por valor**. Isso significa que, quando você chama uma função e passa argumentos para ela:

1. Uma **cópia** do valor de cada argumento é feita.
2. Essa cópia é atribuída ao parâmetro formal correspondente dentro da função.
3. A função opera sobre essas cópias locais.

A consequência mais importante da passagem por valor é que **quaisquer modificações feitas nos parâmetros formais dentro da função não afetam os valores das variáveis originais (argumentos) na função chamadora**. A função trabalha com suas próprias cópias dos dados.

**Exemplo de Passagem por Valor em C:**

```c
#include <stdio.h>

void tentar_modificar(int x_param, int y_param) {
    printf("Dentro da função (antes da modificação): x_param = %d, y_param = %d\n", x_param, y_param);
    x_param = 100; // Modifica a cópia local x_param
    y_param = 200; // Modifica a cópia local y_param
    printf("Dentro da função (depois da modificação): x_param = %d, y_param = %d\n", x_param, y_param);
}

int main() {
    int a = 10;
    int b = 20;

    printf("Antes da chamada da função: a = %d, b = %d\n", a, b);
    tentar_modificar(a, b); // Passa os valores de a e b
    printf("Depois da chamada da função: a = %d, b = %d\n", a, b); // a e b permanecem 10 e 20

    return 0;
}
```

**Saída:**

```
Antes da chamada da função: a = 10, b = 20
Dentro da função (antes da modificação): x_param = 10, y_param = 20
Dentro da função (depois da modificação): x_param = 100, y_param = 200
Depois da chamada da função: a = 10, b = 20
```

Como pode ser visto, as modificações em `x_param` e `y_param` dentro de `tentar_modificar` não alteraram os valores de `a` e `b` na função `main`.

### Passagem de Arrays para Funções

Quando um array é passado como argumento para uma função em C, o comportamento é diferente da passagem por valor de tipos simples. O que é realmente passado para a função não é uma cópia de todo o array, mas sim o **endereço do primeiro elemento do array**. Isso significa que a função recebe uma referência (um ponteiro, conceitualmente) para o array original na memória.

Consequentemente, **modificações feitas nos elementos do array dentro da função irão afetar o array original** na função chamadora.

```c
#include <stdio.h>

// Função para zerar todos os elementos de um vetor
void zerar_vetor(int vet[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        vet[i] = 0; // Modifica o array original
    }
}

void imprimir_vetor(int vet[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        printf("%d ", vet[i]);
    }
    printf("\n");
}

int main() {
    int numeros[] = {1, 2, 3, 4, 5};
    int tam = sizeof(numeros) / sizeof(numeros[0]);

    printf("Vetor antes de zerar: ");
    imprimir_vetor(numeros, tam);

    zerar_vetor(numeros, tam);

    printf("Vetor depois de zerar: ");
    imprimir_vetor(numeros, tam); // Saída: 0 0 0 0 0

    return 0;
}
```

Os detalhes sobre como isso funciona com ponteiros serão aprofundados no próximo capítulo. Por enquanto, é importante lembrar que, ao passar um array, a função opera sobre os dados originais.

## Declaração de Função (Protótipos)

Em C, o compilador processa o código-fonte de cima para baixo. Se você tentar chamar uma função antes que o compilador tenha visto sua definição completa (ou pelo menos sua declaração), ele gerará um erro ou um aviso, pois não saberá o tipo de retorno da função, nem o número e os tipos de seus parâmetros.

Para resolver isso, C utiliza **declarações de função**, também conhecidas como **protótipos de função**. Um protótipo informa ao compilador a "assinatura" da função: seu nome, seu tipo de retorno e os tipos de seus parâmetros. Com essa informação, o compilador pode verificar se as chamadas à função estão corretas, mesmo que a definição completa da função apareça mais tarde no arquivo ou em outro arquivo.

A sintaxe de um protótipo de função é similar ao cabeçalho da definição da função, mas termina com um ponto e vírgula e os nomes dos parâmetros são opcionais (apenas os tipos são necessários):

```c
tipo_de_retorno nome_da_funcao(tipo_param1, tipo_param2, ...);
// ou com nomes de parâmetros (apenas para clareza, os nomes são ignorados pelo compilador no protótipo)
// tipo_de_retorno nome_da_funcao(tipo_param1 nome_param_opcional1, tipo_param2 nome_param_opcional2, ...);
```

**Onde declarar protótipos:**

- No início do arquivo `.c`, antes de qualquer chamada à função (e geralmente antes de `main`).
- Em arquivos de cabeçalho (`.h`), que são então incluídos (`#include`) nos arquivos `.c` que precisam usar as funções. Esta é a prática comum para bibliotecas e projetos com múltiplos arquivos.

**Exemplo com Protótipo em C:**

```c
#include <stdio.h>

// Protótipo da função 'multiplicar'
int multiplicar(int x, int y); // Informa ao compilador sobre a função

int main() {
    int resultado = multiplicar(7, 6); // Chama a função antes de sua definição
    printf("7 * 6 = %d\n", resultado);
    return 0;
}

// Definição completa da função 'multiplicar'
int multiplicar(int x, int y) {
    return x * y;
}
```

Sem o protótipo `int multiplicar(int x, int y);` antes de `main()`, o compilador poderia gerar um aviso ou erro ao encontrar a chamada `multiplicar(7, 6)` dentro de `main()`, pois não saberia o que `multiplicar` é.

## Escopo de Variáveis em Relação a Funções

Como já discutido anteriormente, o escopo de uma variável determina onde ela pode ser acessada.

- **Variáveis Locais:** Declaradas dentro de uma função (incluindo seus parâmetros formais), são acessíveis apenas dentro dessa função. Elas são criadas quando a função é chamada e destruídas quando a função retorna. Funções diferentes podem ter variáveis locais com o mesmo nome sem conflito, pois cada uma existe em seu próprio escopo.
- **Variáveis Globais:** Declaradas fora de todas as funções, são acessíveis por qualquer função no mesmo arquivo (ou em outros arquivos, se declaradas com `extern`, a menos que sejam `static`).

É uma boa prática de programação minimizar o uso de variáveis globais. Funções devem, idealmente, receber todos os dados de que precisam através de seus parâmetros e comunicar seus resultados através de seus valores de retorno (ou modificando dados passados por endereço, como arrays). Isso torna as funções mais autocontidas, fáceis de testar, reutilizar e menos propensas a efeitos colaterais indesejados causados por modificações em variáveis globais.

## Recursividade: Funções que Chamam a Si Mesmas

Uma **função recursiva** é uma função que chama a si mesma, direta ou indiretamente, para resolver um problema. A recursividade é uma técnica poderosa de programação que pode levar a soluções elegantes e concisas para problemas que têm uma natureza inerentemente recursiva (ou seja, que podem ser definidos em termos de si mesmos).

Para que uma função recursiva funcione corretamente e não resulte em uma chamada infinita (levando a um erro de estouro de pilha - _stack overflow_), ela deve ter duas partes essenciais:

1. **Caso Base (Condição de Parada):** Uma ou mais condições simples para as quais a função pode retornar um resultado diretamente, sem fazer mais chamadas recursivas. Este é o ponto que interrompe a cadeia de chamadas.
2. **Passo Recursivo:** A parte da função onde ela chama a si mesma, mas com um subproblema que é, de alguma forma, "menor" ou "mais próximo" do caso base.

**Exemplo: Cálculo de Fatorial Recursivo em C** O fatorial de um número não negativo n (denotado por n!) é o produto de todos os inteiros positivos menores ou iguais a _n_. Por definição, 0! = 1, n! = n * (n - 1) * (n - 2) * ... * 1. Recursivamente, n! = n * (n - 1)! para n > 0, e 0! = 1 (caso base).

```c
#include <stdio.h>

// Função fatorial recursiva
long long int fatorial(int n) {
    // Caso base: fatorial de 0 ou 1 é 1
    if (n == 0 || n == 1) {
        return 1;
    }
    // Passo recursivo: n * fatorial(n-1)
    else {
        return (long long int)n * fatorial(n - 1);
    }
}

int main() {
    int num = 5;
    printf("O fatorial de %d é %lld.\n", num, fatorial(num)); // Saída: O fatorial de 5 é 120.

    num = 0;
    printf("O fatorial de %d é %lld.\n", num, fatorial(num)); // Saída: O fatorial de 0 é 1.
    return 0;
}
```

No exemplo, `fatorial(5)` chama `fatorial(4)`, que chama `fatorial(3)`, e assim por diante, até que `fatorial(1)` (ou `fatorial(0)`) é chamado, que retorna 1 (o caso base), e os resultados são multiplicados de volta na cadeia de chamadas.

A recursividade pode ser uma ferramenta elegante, mas é importante garantir que o caso base seja sempre alcançado para evitar chamadas infinitas. Além disso, chamadas recursivas profundas podem consumir muita memória da pilha de execução.

## Funções em C++: Semelhanças e Diferenças Chave

O C++ herda toda a mecânica básica de funções do C, incluindo tipo de retorno, nome, parâmetros, corpo, instrução `return` e protótipos. No entanto, C++ adiciona várias funcionalidades poderosas que aumentam a flexibilidade e expressividade das funções:

1. **Sobrecarga de Funções (Function Overloading):** Em C++, você pode definir **múltiplas funções com o mesmo nome**, desde que suas **listas de parâmetros sejam diferentes**. A diferença pode ser no número de parâmetros ou nos tipos dos parâmetros (ou ambos). O compilador C++ seleciona automaticamente a versão correta da função a ser chamada com base nos tipos e no número de argumentos fornecidos na chamada. Isso não é permitido em C.
    
    ```cpp
    // Exemplo de Sobrecarga de Funções em C++
    #include <iostream>
    #include <string>
    
    void imprimir(int i) {
        std::cout << "Inteiro: " << i << std::endl;
    }
    
    void imprimir(double d) {
        std::cout << "Double: " << d << std::endl;
    }
    
    void imprimir(const std::string& s) {
        std::cout << "String: " << s << std::endl;
    }
    
    int main() {
        imprimir(10);         // Chama imprimir(int)
        imprimir(3.14159);    // Chama imprimir(double)
        imprimir("Olá C++");  // Chama imprimir(const std::string&)
        return 0;
    }
    ```
    
2. **Argumentos Padrão (Default Arguments):** Em C++, você pode especificar **valores padrão para um ou mais parâmetros** na declaração (ou definição) de uma função. Se um argumento para um parâmetro com valor padrão não for fornecido na chamada da função, o valor padrão será usado automaticamente. Os parâmetros com valores padrão devem ser os últimos na lista de parâmetros.
    
    ```cpp
    // Exemplo de Argumentos Padrão em C++
    #include <iostream>
    
    void exibir_info(std::string nome, int idade = 18, std::string pais = "Brasil") {
        std::cout << "Nome: " << nome << ", Idade: " << idade << ", País: " << pais << std::endl;
    }
    
    int main() {
        exibir_info("Ana", 25, "Portugal"); // Todos os argumentos fornecidos
        exibir_info("Bruno", 30);          // 'pais' usará o valor padrão "Brasil"
        exibir_info("Carlos");             // 'idade' usará 18, 'pais' usará "Brasil"
        return 0;
    }
    ```
    
3. **Funções Inline (Inline Functions):** A palavra-chave `inline` pode ser usada antes da definição de uma função como uma **sugestão ao compilador** para realizar a **expansão inline** da função. Isso significa que, em vez de gerar uma chamada de função (que tem um certo overhead), o compilador pode substituir o local da chamada pelo próprio corpo da função. Isso é geralmente benéfico para funções pequenas e frequentemente chamadas, pois pode reduzir o overhead da chamada e permitir mais otimizações. No entanto, `inline` é apenas uma sugestão; o compilador pode optar por ignorá-la.
    
    ```cpp
    // Exemplo de Função Inline em C++
    #include <iostream>
    
    inline int cubo(int x) { // Sugestão para inlining
        return x * x * x;
    }
    
    int main() {
        int val = 3;
        std::cout << "O cubo de " << val << " é " << cubo(val) << std::endl;
        // O compilador PODE substituir cubo(val) por (val * val * val) diretamente aqui.
        return 0;
    }
    ```
    
4. **Passagem por Referência (usando `&`):** Além da passagem por valor (padrão para tipos simples) e da passagem de arrays (que implicitamente passa um endereço), C++ introduz a **passagem por referência**. Ao declarar um parâmetro como uma referência (usando `&` após o tipo), a função recebe uma referência direta à variável original passada como argumento, em vez de uma cópia. Isso significa que **modificações no parâmetro de referência dentro da função afetam diretamente a variável original**.
    
    ```cpp
    // Exemplo de Passagem por Referência em C++
    #include <iostream>
    
    void dobrar_valor(int& num_ref) { // num_ref é uma referência para um int
        num_ref *= 2; // Modifica a variável original
    }
    
    int main() {
        int meu_numero = 10;
        std::cout << "Antes de dobrar: " << meu_numero << std::endl; // Saída: 10
        dobrar_valor(meu_numero); // Passa meu_numero por referência
        std::cout << "Depois de dobrar: " << meu_numero << std::endl; // Saída: 20
        return 0;
    }
    ```
    
    A passagem por referência é frequentemente usada para permitir que funções modifiquem argumentos ou para evitar a cópia de objetos grandes, melhorando a eficiência.

## Considerações Finais

Neste capítulo, desvendamos o conceito de funções, os blocos de construção essenciais para a criação de programas modulares, reutilizáveis e bem organizados em C e C++. Cobrimos a sintaxe para definir e chamar funções, a importância dos tipos de retorno e da instrução `return`, e o mecanismo de passagem de parâmetros por valor, assim como a forma como arrays são tratados. A necessidade de protótipos de função para declarações antecipadas também foi destacada.

Introduzimos o poderoso conceito de recursividade, onde uma função chama a si mesma para resolver problemas de forma elegante, enfatizando a necessidade de um caso base para evitar chamadas infinitas.

Finalmente, fizemos a ponte para o C++, mostrando como ele não apenas herda a base funcional do C, mas a enriquece com recursos como sobrecarga de funções, argumentos padrão, a sugestão `inline` e a passagem por referência, oferecendo aos programadores C++ ainda mais ferramentas para criar código flexível e eficiente.

O domínio das funções é um marco no aprendizado de qualquer linguagem de programação. Elas são a chave para gerenciar a complexidade, promover o bom design de software e escrever código que seja ao mesmo tempo poderoso e compreensível. Nos próximos capítulos, continuaremos a construir sobre este conhecimento, explorando tópicos como ponteiros, que interagem profundamente com funções e abrem novas possibilidades de manipulação de dados e memória.
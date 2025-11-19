# Capítulo 11 – Funções e Procedimentos

À medida que os sistemas de software evoluem, o volume de código necessário para implementar as regras de negócio e a lógica operacional cresce exponencialmente. Tentar manter milhares de linhas de código dentro de uma única estrutura monolítica, como a função `main()`, resulta invariavelmente em um código caótico, de difícil leitura e praticamente impossível de manter ou depurar. Para solucionar esse problema de complexidade, a Engenharia de Software adota o princípio de "dividir para conquistar". Na programação em C e C++, essa divisão é concretizada através das **funções** (e, conceitualmente, **procedimentos**).

As funções representam os blocos fundamentais da programação estruturada. Elas permitem o isolamento de lógicas específicas em unidades autocontidas, que podem ser desenvolvidas, testadas e depuradas independentemente do restante do sistema. Mais do que apenas uma ferramenta de organização, as funções promovem a abstração: quem utiliza uma função precisa conhecer apenas sua interface (o que ela precisa para trabalhar e o que ela devolve), sem necessidade de compreender os detalhes complexos de sua implementação interna.

Neste capítulo, o conceito de modularização será explorado em profundidade. Serão detalhados os mecanismos de definição e declaração de funções em C, a anatomia de seus parâmetros e tipos de retorno, e a distinção crítica entre passagem de dados por valor e a passagem de arrays. Discutir-se-á o fluxo de execução durante a chamada de uma função e o retorno de valores. Além disso, será abordado o conceito de recursividade — a capacidade de uma função invocar a si mesma. Por fim, o capítulo fará a transição para o C++, demonstrando como a linguagem moderna expande o modelo do C com recursos poderosos como sobrecarga, argumentos padrão, funções `inline` e a passagem por referência.

## Definição e Anatomia de uma Função

Uma função é, essencialmente, um subprograma: um trecho de código que possui um nome, realiza uma tarefa específica e pode ser invocado por outras partes do programa. Em C, não há distinção sintática formal entre "função" (que retorna valor) e "procedimento" (que apenas executa ações), sendo ambos definidos pela mesma estrutura, diferenciando-se apenas pelo tipo de retorno.

A estrutura geral para a definição de uma função em C obedece à seguinte sintaxe:

```
tipo_de_retorno nome_da_funcao(tipo_param1 nome_param1, tipo_param2 nome_param2, ...) {
    // Corpo da função:
    // Declarações de variáveis locais
    // Lógica de processamento
    // return valor; (se aplicável)
}
```

Componentes da definição de uma função:

- **Tipo de Retorno (`tipo_de_retorno`):** Define a natureza do dado que a função devolverá ao ponto onde foi chamada. Pode ser qualquer tipo válido (`int`, `float`, `char*`, etc.). Se a função executa uma tarefa mas não produz um resultado numérico ou de dados para ser usado posteriormente (como uma função que apenas imprime algo na tela), utiliza-se o tipo especial **`void`**.
- **Identificador (`nome_da_funcao`):** É o nome pelo qual a função será chamada. Deve seguir as regras de nomenclatura de identificadores da linguagem e, idealmente, ser um verbo ou frase verbal que descreva claramente a ação realizada (ex: `calcularMedia`, `imprimirRelatorio`).
- **Lista de Parâmetros:** Um conjunto de variáveis (separadas por vírgulas) que atuam como a entrada de dados da função. Cada parâmetro deve ter seu tipo explicitamente declarado. Estes são chamados de **parâmetros formais**. Se a função não necessita de dados externos, a lista pode ser vazia `()` ou conter a palavra `void`.
- **Corpo da Função:** O bloco de código delimitado por chaves `{}`. É onde reside o escopo local da função e onde a lógica é executada.

### Exemplo de Procedimento (Função `void`)

Um procedimento é uma rotina que executa uma ação (como exibir um menu, limpar a tela ou configurar um hardware) e não devolve nenhum valor matemático ou lógico ao chamador.

**Pseudocódigo:**

```
PROCEDIMENTO ExibirCabecalho()
    ESCREVA "-------------------------"
    ESCREVA "   SISTEMA DE CONTROLE   "
    ESCREVA "-------------------------"
FIM-PROCEDIMENTO
```

**Implementação em C:**

```c
#include <stdio.h>

// Definição de um procedimento (retorno void)
void exibirCabecalho() {
    printf("-------------------------\n");
    printf("   SISTEMA DE CONTROLE   \n");
    printf("-------------------------\n");
    // O 'return' é opcional aqui, pois a função termina no fecha-chaves.
}

int main() {
    exibirCabecalho(); // Chamada do procedimento
    return 0;
}
```

### Exemplo de Função com Retorno

Funções com retorno são utilizadas para processar dados e devolver um resultado.

**Pseudocódigo:**

```
FUNCAO CalcularAreaRetangulo(largura, altura)
    RETORNE largura * altura
FIM-FUNCAO
```

**Implementação em C:**

```c
#include <stdio.h>

// Função que recebe parâmetros e retorna um valor
double calcularAreaRetangulo(double largura, double altura) {
    double area = largura * altura;
    return area; // Envia o valor de 'area' de volta ao chamador
}

int main() {
    double l = 5.0, a = 3.0;
    // O valor retornado substitui a chamada da função na expressão
    double resultado = calcularAreaRetangulo(l, a);
    
    printf("A área é: %.2f\n", resultado);
    return 0;
}
```

## Mecanismo de Chamada e Retorno

Quando uma função é invocada (chamada) dentro do programa, ocorre uma transferência de controle de fluxo.

1. O processador suspende a execução da função atual (ex: `main`).
2. O endereço de retorno (onde o programa deve continuar depois) é salvo na pilha de execução (_stack_).
3. Os argumentos são passados para os parâmetros da função.
4. O controle salta para a primeira linha da função chamada.
5. Ao encontrar a instrução `return` ou o final do bloco, a função é encerrada.
6. Se houver valor de retorno, ele é entregue.
7. O processador recupera o endereço de retorno e retoma a execução da função original.

### Instrução `return`

A instrução `return` tem dois propósitos: finalizar a execução da função imediatamente e, opcionalmente, enviar um valor de volta.

- Em funções não-`void`, o `return` deve ser seguido por uma expressão compatível com o tipo de retorno declarado.
- Em funções `void`, pode-se usar `return;` (sem valor) para sair da função prematuramente, caso uma condição seja atendida.

**Exemplo de saída prematura em função `void`:**

```c
void verificarPositivo(int n) {
    if (n < 0) {
        printf("Erro: Número negativo.\n");
        return; // Sai da função imediatamente
    }
    printf("Número aceito: %d\n", n);
    // Código complexo de processamento...
}
```

## Parâmetros e Argumentos

É fundamental distinguir **parâmetros formais** (as variáveis declaradas no cabeçalho da função) de **argumentos reais** (os valores passados durante a chamada).

Em C, o mecanismo padrão de transferência de dados para tipos primitivos (`int`, `float`, `char`, `struct`) é a **Passagem por Valor**.

Isso significa que a função não recebe a variável original, mas sim uma **cópia** do seu valor. As variáveis locais da função (os parâmetros) são criadas em um local de memória distinto das variáveis do chamador.

**Consequência:** Alterações feitas nos parâmetros dentro da função **não afetam** as variáveis originais fora dela.

**Exemplo Demonstrativo:**

```c
#include <stdio.h>

void tentarAlterar(int valor) {
    printf("  [Função] Valor recebido: %d\n", valor);
    valor = 999; // Altera apenas a cópia local
    printf("  [Função] Valor alterado para: %d\n", valor);
}

int main() {
    int numero = 10;
    printf("[Main] Valor antes: %d\n", numero);
    
    tentarAlterar(numero); // Passa uma cópia do valor 10
    
    printf("[Main] Valor depois: %d\n", numero); // Permanece 10
    return 0;
}
```

**Saída:**

```
[Main] Valor antes: 10
  [Função] Valor recebido: 10
  [Função] Valor alterado para: 999
[Main] Valor depois: 10
```

### Passagem de Arrays (Vetores e Matrizes)

Enquanto tipos primitivos são passados por valor (cópia), **arrays** (vetores e matrizes) em C possuem um comportamento distinto devido à sua natureza de ponteiros implícitos.

Quando um array é passado para uma função, o que é copiado não é o conteúdo inteiro do array (o que seria ineficiente para grandes volumes de dados), mas sim o **endereço de memória** do primeiro elemento.

Isso significa que, embora a variável que armazena o endereço seja uma cópia, ela aponta para a **mesma área de memória** que o array original. Portanto, **modificações feitas nos elementos do array dentro da função alteram o array original**.

**Exemplo:**

```c
#include <stdio.h>

// Recebe o endereço do array e seu tamanho
void dobrarValores(int vetor[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        vetor[i] = vetor[i] * 2; // Modifica a memória original
    }
}

int main() {
    int numeros[] = {1, 2, 3};
    
    dobrarValores(numeros, 3);
    
    printf("%d, %d, %d\n", numeros[0], numeros[1], numeros[2]); 
    // Saída: 2, 4, 6
    return 0;
}
```

Note que, ao passar arrays, a função desconhece o tamanho do vetor (exceto em arrays estáticos dentro do mesmo escopo, o que não é o caso de parâmetros). Portanto, é prática obrigatória passar o tamanho do array como um argumento adicional.

## Protótipos de Função (Declaração Antecipada)

O compilador C processa o código sequencialmente, de cima para baixo. Se a função `main()` tenta chamar uma função auxiliar que está definida mais abaixo no arquivo, o compilador gerará um erro ou aviso (dependendo da versão do padrão C), pois ele ainda não conhece a assinatura daquela função.

Para resolver isso e permitir uma organização de código flexível (ou inclusive em múltiplos arquivos), utilizam-se os **Protótipos de Função**.

O protótipo é uma declaração que informa ao compilador o nome da função, seu tipo de retorno e os tipos de seus parâmetros, sem incluir o corpo (a implementação). Termina com ponto e vírgula.

**Sintaxe:**

```c
tipo_retorno nome_funcao(tipo_param1, tipo_param2);
```

**Exemplo de Organização:**

```c
#include <stdio.h>

// 1. Protótipo (Declaração)
float dividir(float a, float b);

int main() {
    // O compilador sabe que 'dividir' existe e aceita dois floats,
    // mesmo sem ter visto o código dela ainda.
    float res = dividir(10.0, 2.0); 
    printf("Divisão: %.2f\n", res);
    return 0;
}

// 2. Implementação (Definição)
float dividir(float a, float b) {
    if (b == 0) return 0.0;
    return a / b;
}
```

## Recursividade

A recursividade é uma técnica de programação onde uma função chama a si mesma para resolver um problema. Problemas recursivos são aqueles que podem ser decompostos em subproblemas menores e idênticos ao original.

Para que uma função recursiva seja válida e não execute infinitamente (causando o erro de _Stack Overflow_ ou estouro de pilha), ela deve possuir duas partes obrigatórias:

1. **Caso Base (Condição de Parada):** Uma condição simples que pode ser resolvida sem nova chamada recursiva.
2. **Passo Recursivo:** A chamada da função para si mesma, mas com um argumento que a aproxima do caso base.

Vamos ver um exemplo de aplicação de função recursiva no cálculo de um fatorial. O fatorial de um número $n$ ($n!$) é definido como $n \times (n-1)!$, com o caso base de $0! = 1$.

**Pseudocódigo:**

```
FUNCAO Fatorial(n)
    SE n == 0 ENTÃO
        RETORNE 1          // Caso Base
    SENÃO
        RETORNE n * Fatorial(n - 1) // Passo Recursivo
    FIM-SE
FIM-FUNCAO
```

**Implementação em C:**

```c
#include <stdio.h>

long long fatorial(int n) {
    if (n == 0) {
        return 1; // Caso base
    } else {
        return n * fatorial(n - 1); // Chamada recursiva
    }
}

int main() {
    int num = 5;
    printf("Fatorial de %d é %lld\n", num, fatorial(num));
    return 0;
}
```

**Rastreamento de `fatorial(3)`:**

1. `fatorial(3)` chama `3 * fatorial(2)`
2. `fatorial(2)` chama `2 * fatorial(1)`
3. `fatorial(1)` chama `1 * fatorial(0)`
4. `fatorial(0)` retorna `1` (Base)
5. Desempilha: `1 * 1 = 1`
6. Desempilha: `2 * 1 = 2`
7. Desempilha: `3 * 2 = 6` -> Resultado final.

## Funções em C++

O C++ mantém compatibilidade total com o sistema de funções do C, mas introduz recursos sofisticados que aumentam a flexibilidade e a segurança do código.

### Sobrecarga de Funções (Function Overloading)

Em C, não é permitido ter duas funções com o mesmo nome. Em C++, isso é possível, desde que a **assinatura** (a lista de parâmetros) seja diferente. O compilador decide qual função chamar baseando-se nos tipos e na quantidade de argumentos passados.

```c
#include <iostream>

void imprimir(int i) {
    std::cout << "Imprimindo inteiro: " << i << std::endl;
}

void imprimir(double d) {
    std::cout << "Imprimindo double: " << d << std::endl;
}

void imprimir(int a, int b) {
    std::cout << "Imprimindo dois inteiros: " << a << " e " << b << std::endl;
}

int main() {
    imprimir(10);       // Chama a primeira versão
    imprimir(3.14);     // Chama a segunda versão
    imprimir(1, 2);     // Chama a terceira versão
    return 0;
}
```

### Argumentos Padrão (Default Arguments)

O C++ permite definir valores iniciais para parâmetros no protótipo da função. Se o chamador omitir esses argumentos, os valores padrão serão utilizados.

```c
// Protótipo com valores padrão
// Nota: Os parâmetros com padrão devem estar sempre à direita (no final da lista)
void configurarJanela(int largura = 800, int altura = 600);

int main() {
    configurarJanela();            // Usa 800x600
    configurarJanela(1024);        // Usa 1024x600 (altura padrão)
    configurarJanela(1920, 1080);  // Usa 1920x1080
    return 0;
}

void configurarJanela(int largura, int altura) {
    std::cout << "Janela: " << largura << "x" << altura << std::endl;
}
```

### Funções `inline`

A palavra-chave `inline` é uma sugestão ao compilador para que, em vez de realizar o salto de execução tradicional (call/return), ele substitua a chamada da função pelo próprio corpo do código da função no local onde ela foi chamada. Isso elimina o _overhead_ de chamada de função, sendo ideal para funções pequenas e críticas em desempenho.

```c
inline int quadrado(int x) {
    return x * x;
}
```

### Passagem por Referência

Este é um dos recursos mais importantes do C++. Ao contrário do C, que usa ponteiros para modificar variáveis originais (o que requer sintaxe complexa de dereferência `*` e endereçamento `&`), o C++ introduz as **referências**.

Uma referência é um "apelido" para uma variável existente. Ao passar um parâmetro por referência (usando `&` no tipo), a função recebe acesso direto à variável original, sem copiá-la, mas com a sintaxe limpa de uma variável comum.

**Comparativo C vs C++:**

**Estilo C (Ponteiros):**

```c
void dobrar(int *n) { // Recebe endereço
    *n = *n * 2;      // Precisa dereferenciar
}
// Chamada: dobrar(&valor);
```

**Estilo C++ (Referências):**

```c
void dobrar(int &n) { // Recebe referência, também válido usar dobrar(int& n)
    n = n * 2;        // Sintaxe transparente, altera o original
}
// Chamada: dobrar(valor);
```

A passagem por referência é crucial em C++ para evitar a cópia desnecessária de objetos grandes (como structs ou classes complexas) e para permitir a modificação intuitiva de argumentos.

## Considerações Finais

Neste capítulo, desvendamos o conceito de funções, os blocos de construção essenciais para a criação de programas modulares, reutilizáveis e bem organizados. A transição de um código monolítico na `main()` para um sistema de funções interconectadas é um passo fundamental na evolução de qualquer programador.

Cobrimos a sintaxe rigorosa para definir e chamar funções em C, a importância crítica dos tipos de retorno (especialmente `void`), e detalhamos o mecanismo de passagem de parâmetros por valor, contrastando-o com o comportamento especial dos arrays, que decaem para ponteiros. A necessidade de protótipos para organização do código e a lógica da recursividade também foram exploradas.

Por fim, conectamos esses conceitos ao C++, demonstrando como a linguagem moderna enriquece o paradigma funcional com sobrecarga, argumentos padrão, otimização `inline` e, fundamentalmente, a passagem por referência, que oferece eficiência e clareza superiores à manipulação pura de ponteiros do C. Com o domínio das funções, a estrutura lógica dos programas torna-se robusta, preparando o terreno para conceitos mais avançados de gerenciamento de memória e estruturas de dados.
# Capítulo 1 – Conceitos Básicos

## Introdução às Linguagens C e C++

A história da programação moderna não pode ser contada sem citar dois nomes fundamentais: **Dennis MacAlistair Ritchie** e **Bjarne Stroustrup**. Dennis Ritchie foi o criador da linguagem de programação **C**, uma das linguagens mais influentes e importantes da história da computação. Além disso, Ritchie foi um dos principais desenvolvedores do sistema operacional **UNIX**, cujo legado ecoa até hoje em sistemas como o **Linux**, o **macOS** e, indiretamente, em muitos sistemas embarcados e servidores que sustentam a internet.

A linguagem C é considerada uma linguagem de **médio nível**. Este termo, frequentemente citado na literatura, reflete sua natureza híbrida, combinando características de linguagens de **alto nível**, como legibilidade, portabilidade e abstrações, com recursos de **baixo nível**, como acesso direto à memória e controle sobre o hardware. Como bem define Herbert Schildt, C é uma linguagem que se posiciona entre a simplicidade das linguagens de alto nível, como BASIC e Pascal, e a potência direta das linguagens de montagem, como Assembly.

C foi projetada para ser eficiente, portátil e poderosa, o que explica seu enorme sucesso em sistemas operacionais, desenvolvimento de software de base, sistemas embarcados e aplicações que demandam alto desempenho.

Por sua vez, o **C++** surgiu na década de 1980, desenvolvido por **Bjarne Stroustrup**, como uma extensão da linguagem C. O principal diferencial do C++ é a introdução do paradigma de **programação orientada a objetos (POO)**, além de outros avanços, como **sobrecarga de operadores**, **tratamento de exceções**, **templates** (que possibilitam programação genérica) e diversos recursos que visam tornar o código mais robusto, flexível e modular.

Enquanto o C mantém sua vocação para desenvolvimento de sistemas, firmware e softwares onde controle e desempenho são essenciais, o C++ expande essas capacidades permitindo a construção de sistemas complexos, com foco na reutilização de código, abstração e manutenção facilitada.

Importante ressaltar que, embora o C++ tenha sido criado como uma extensão do C, as duas linguagens **não são totalmente compatíveis**. Muitos programas escritos em C podem ser compilados por compiladores C++, mas a recíproca nem sempre é verdadeira, especialmente devido às adições da orientação a objetos e às diferenças nos modelos de memória e bibliotecas.

## Linguagem de Médio Nível: O Que Isso Significa?

Dizer que C é uma linguagem de médio nível não implica que ela seja limitada ou que se situe "no meio" em termos de poder. Pelo contrário, isso significa que C oferece um equilíbrio excepcional entre o controle do hardware e a abstração necessária para desenvolver programas complexos de maneira relativamente simples e portátil.

Por exemplo, em C (e também em C++), o programador pode:

- Manipular bits, endereços de memória e ponteiros, como faria em Assembly;
- Ao mesmo tempo, usar estruturas de controle como `if`, `for` e `while`, além de funções e bibliotecas, típicas de linguagens de alto nível.

Essa combinação é rara e valiosa, motivo pelo qual tanto C quanto C++ continuam extremamente relevantes, mesmo com o surgimento de linguagens mais modernas.

## Estrutura de um Programa em C e C++

Independentemente de se tratar de C ou C++, um programa é estruturado em **funções**, sendo que a execução sempre começa pela função especial denominada **`main()`**.

Em C, todo o programa é centrado na função `main` e em outras funções auxiliares, enquanto no C++ essa estrutura pode ser enriquecida com **classes** e **objetos**, que encapsulam dados e comportamentos.

A forma geral de um programa simples em C é:

```c
#include <stdio.h>

int main() {
    printf("Olá, mundo!\n");
    return 0;
}
```

No C++, esse mesmo programa poderia ser escrito da seguinte forma:

```cpp
#include <iostream>

int main() {
    std::cout << "Olá, mundo!" << std::endl;
    return 0;
}
```

Note que, em C++, utilizamos `#include <iostream>` e o objeto `std::cout` para saída no console, ao invés de `printf()` da biblioteca `<stdio.h>` utilizada no C.

Além disso, no C++ é comum a definição de **classes**, como veremos em capítulos posteriores, permitindo maior organização e modularização do código.

## Compilação e Execução de Programas

### Compiladores e Interpretadores

C e C++ são linguagens compiladas. Isso significa que o código fonte, escrito em arquivos de texto com extensão `.c` para C e `.cpp` para C++, precisa ser convertido para código de máquina antes de ser executado.

O processo de compilação consiste nas seguintes etapas:

1. **Compilação:** O código fonte é convertido em **código objeto** (geralmente arquivos `.o` ou `.obj`), contendo instruções em linguagem de máquina, mas ainda não executável por si só.
2. **Linkedição:** O código objeto é combinado com outras bibliotecas e códigos auxiliares para gerar o **arquivo executável final**, que pode ser executado no sistema operacional.
3. **Execução:** O executável resultante é carregado na memória pelo sistema operacional e começa sua execução pela função `main()`.

Ao contrário de linguagens interpretadas (como Python ou JavaScript), onde o código é traduzido em tempo de execução, linguagens compiladas oferecem maior desempenho, uma vez que o código já está pronto para ser executado diretamente pela CPU.

### Ferramentas de Desenvolvimento

Atualmente, existem diversos compiladores e IDEs que facilitam o desenvolvimento em C e C++, como:

- **GCC / G++:** O compilador padrão em sistemas Unix/Linux.
- **MSVC:** Compilador da Microsoft, utilizado no Visual Studio.
- **Clang:** Compilador moderno e altamente otimizado, utilizado em macOS e iOS.
- **Dev-C++**, **Code::Blocks**, **Eclipse CDT**, **NetBeans**, entre outros, são editores de código/IDEs que oferecem ambientes gráficos integrados.

## Sintaxe e Estrutura da Linguagem

### Características Gerais da Sintaxe

A sintaxe de C e C++ possui características fundamentais que precisam ser rigorosamente seguidas:

- Ambas são linguagens **case sensitive**, ou seja, `variavel` e `Variavel` são identificadores diferentes.
- O **ponto e vírgula (;)** finaliza uma instrução.
- As **chaves `{}`** delimitam blocos de código, como funções, estruturas de controle e classes (no caso do C++).
- Comentários podem ser feitos de duas formas:
    - `// Comentário de uma linha`
    - `/* Comentário de múltiplas linhas */`

### Declaração de Variáveis

Em C, toda variável deve ser declarada antes de seu uso, especificando seu tipo:

```c
int idade;
float altura;
char letra;
```

O mesmo se aplica ao C++, porém este permite declaração mais flexível, como inicialização inline com o construtor de uma classe ou uso do tipo automático (`auto`), introduzido nas versões mais recentes do C++.

Exemplo em C++ moderno:

```cpp
auto idade = 25; // O compilador deduz que é do tipo int
```

### Tipos de Dados

As linguagens C e C++ oferecem tipos básicos como:

- **Inteiros:** `int`, `short`, `long`, `long long`
- **Ponto flutuante:** `float`, `double`, `long double`
- **Caracteres:** `char`
- **Booleanos:** disponível diretamente apenas em C++ (`bool` com `true` e `false`). Em C, booleanos são simulados com inteiros.

O C++ adiciona ainda tipos mais complexos, como **`std::string`**, enquanto C trabalha com strings como vetores de caracteres.

### Operadores

Operadores em C e C++ incluem:

- **Aritméticos:** `+`, `-`, `*`, `/`, `%`
- **Relacionais:** `==`, `!=`, `>`, `<`, `>=`, `<=`
- **Lógicos:** `&&`, `||`, `!`
- **Atribuição:** `=`, `+=`, `-=`, etc.
- **Bit a bit:** `&`, `|`, `^`, `~`, `<<`, `>>`
- **Outros:** como o operador ternário `?:`

O C++ adiciona ainda a possibilidade de **sobrecarga de operadores**, permitindo redefinir como certos operadores se comportam para objetos de classes definidas pelo usuário.

### Controle de Fluxo e Estruturação

Tanto C quanto C++ são linguagens **estruturadas**, ou seja, baseiam-se em estruturas bem definidas de controle de fluxo, como:

- **Condicionais:** `if`, `if-else`, `switch-case`
- **Laços:** `while`, `do-while`, `for`
- **Desvio incondicional:** `break`, `continue`, `goto` (este último é desencorajado)

A estruturação do código promove legibilidade, manutenção facilitada e modularidade. Além disso, no C++, o conceito de modularidade é levado além com a **programação orientada a objetos**, que permite encapsular dados e comportamentos dentro de classes.

### Funções

As **funções** são unidades fundamentais em C e C++. Elas permitem dividir o código em blocos lógicos menores, promovendo a reutilização e a clareza.

Exemplo de função em C:

```c
int soma(int a, int b) {
    return a + b;
}
```

Em C++, além de funções livres como no C, é possível criar métodos associados a classes, aumentando o nível de organização do código.

### Bibliotecas

Em C, as bibliotecas padrão são inclusas por meio de diretivas como:

```c
#include <stdio.h> // Entrada e saída
#include <math.h>  // Funções matemáticas
```

Em C++, além das bibliotecas herdadas de C, temos a poderosa **STL (Standard Template Library)**, que oferece estruturas de dados genéricas como `vector`, `list`, `map`, entre outras, além de algoritmos prontos para ordenação, busca, manipulação e muito mais.

## Considerações Finais

Ao longo deste capítulo, foram apresentados os conceitos fundamentais das linguagens C e C++, suas origens, diferenças e semelhanças, além de uma visão geral de sua sintaxe, estrutura, processo de compilação e principais elementos. Este conhecimento forma a base necessária para avançarmos nos próximos capítulos, onde exploraremos de maneira detalhada tanto a programação procedural (com C) quanto a programação orientada a objetos e recursos avançados (com C++).

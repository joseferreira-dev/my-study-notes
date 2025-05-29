# Capítulo 13 – Registros (Estruturas)

Nos capítulos anteriores, exploramos os tipos de dados fundamentais, variáveis, operadores, estruturas de controle, funções e arrays. Os arrays, como vimos, são excelentes para armazenar coleções de dados do **mesmo tipo**. No entanto, frequentemente precisamos agrupar dados de **tipos diferentes** que, juntos, descrevem uma única entidade ou conceito. Por exemplo, para representar um "aluno", podemos precisar armazenar seu nome (uma string ou array de caracteres), sua matrícula (um inteiro) e sua média (um número de ponto flutuante). Um array simples não seria adequado para essa tarefa, pois todos os seus elementos devem ser do mesmo tipo.

É para resolver essa necessidade de agrupar dados heterogêneos de forma lógica que as linguagens C e C++ oferecem o conceito de **estruturas**, também conhecidas em outros contextos como **registros**. Uma estrutura permite que você defina um novo tipo de dado composto, que agrupa diversas variáveis (chamadas de membros ou campos) sob um único nome. Cada membro pode ter seu próprio tipo de dado.

Neste capítulo, mergulharemos no universo das estruturas. Aprenderemos como definir um tipo estrutura em C usando a palavra-chave `struct`, como declarar variáveis desse novo tipo e como acessar seus membros individuais. Discutiremos a inicialização de estruturas, o uso de ponteiros para estruturas e como elas interagem com funções, incluindo sua passagem como parâmetros. Abordaremos também os arrays de estruturas e as estruturas aninhadas. Finalmente, faremos a transição para o C++, observando as semelhanças e as pequenas, mas significativas, diferenças e conveniências que a linguagem oferece ao trabalhar com `structs`.

## O Que São Estruturas (Registros)?

Uma **estrutura** (ou `struct`, em C) é um tipo de dado definido pelo usuário que permite agrupar um conjunto de variáveis de tipos diferentes sob um único nome. Essas variáveis individuais dentro da estrutura são chamadas de **membros** (ou campos) da estrutura. As estruturas são uma forma de criar tipos de dados mais complexos e representativos da realidade que se deseja modelar.

Por exemplo, para representar um livro, poderíamos criar uma estrutura que contenha membros para o título (string), autor (string), número de páginas (inteiro) e preço (ponto flutuante). Todos esses dados, embora de tipos diferentes, pertencem logicamente à mesma entidade "livro".

A principal vantagem das estruturas é a organização e o encapsulamento lógico dos dados. Elas permitem tratar um conjunto de informações relacionadas como uma unidade, simplificando a manipulação e a passagem desses dados entre diferentes partes de um programa.

## Declarando Estruturas em C

Para usar estruturas, você primeiro precisa **definir o formato** da estrutura, ou seja, quais membros ela conterá e quais serão os tipos de cada membro. Isso é feito usando a palavra-chave `struct`.

A sintaxe geral para definir um tipo estrutura em C é:

```c
struct nome_da_tag_da_estrutura {
    tipo_membro1 nome_membro1;
    tipo_membro2 nome_membro2;
    // ... mais membros
    tipo_membroN nome_membroN;
}; // Ponto e vírgula é obrigatório aqui
```

Onde:

- `struct`: É a palavra-chave que inicia a definição de uma estrutura.
- `nome_da_tag_da_estrutura`: É um nome (tag) opcional que você dá a este formato de estrutura. Esta tag pode ser usada posteriormente para declarar variáveis desse tipo de estrutura.
- `tipo_membroN nome_membroN`: São as declarações dos membros da estrutura, cada um com seu tipo e nome.
- O ponto e vírgula após a chave de fechamento `}` é obrigatório.

**Exemplo de Definição de Estrutura em C:** Vamos definir uma estrutura para representar um ponto no plano cartesiano, com coordenadas x e y:

```c
struct Ponto {
    int x;
    int y;
};
```

Agora, `struct Ponto` é um novo tipo de dado que pode ser usado. Outro exemplo, para um aluno:

```c
struct Aluno {
    char nome[50];
    int matricula;
    float media;
};
```

Neste ponto, apenas definimos o "molde" ou o "template" da estrutura. Nenhuma variável do tipo `struct Aluno` foi criada ainda, e nenhuma memória foi alocada para armazenar dados de alunos (exceto pela definição do tipo em si).

## Declarando Variáveis do Tipo Estrutura

Uma vez que um tipo estrutura foi definido, você pode declarar variáveis desse tipo, da mesma forma que declara variáveis de tipos básicos. Para declarar uma variável de um tipo estrutura que você definiu, você usa a palavra-chave `struct` seguida pela tag da estrutura e, então, o nome da variável.

Sintaxe:

```c
struct nome_da_tag_da_estrutura nome_da_variavel_estrutura;
// Ou múltiplas variáveis:
// struct nome_da_tag_da_estrutura var1, var2, var3;
```

**Exemplo de Declaração de Variáveis de Estrutura em C:**

```c
#include <stdio.h>
#include <string.h> // Para strcpy

// Definição da estrutura Aluno
struct Aluno {
    char nome[50];
    int matricula;
    float media;
};

// Definição da estrutura Ponto
struct Ponto {
    int x;
    int y;
};

int main() {
    // Declarando variáveis do tipo struct Aluno
    struct Aluno aluno1;
    struct Aluno aluno2, aluno3; // Múltiplas declarações

    // Declarando uma variável do tipo struct Ponto
    struct Ponto p1;

    // Atribuindo valores aos membros de aluno1 (veremos o acesso a membros a seguir)
    strcpy(aluno1.nome, "Maria Silva");
    aluno1.matricula = 101;
    aluno1.media = 8.5f;

    p1.x = 10;
    p1.y = 20;

    printf("Aluno 1: Nome=%s, Matrícula=%d, Média=%.1f\n",
           aluno1.nome, aluno1.matricula, aluno1.media);
    printf("Ponto p1: x=%d, y=%d\n", p1.x, p1.y);

    return 0;
}
```

É possível também declarar variáveis de estrutura diretamente ao final da definição da estrutura, antes do ponto e vírgula final, embora seja menos comum para definições de tipo reutilizáveis:

```c
struct Coordenada {
    double latitude;
    double longitude;
} localizacao1, localizacao2; // localizacao1 e localizacao2 são variáveis struct Coordenada
```

## Acessando Membros de Estruturas

Para acessar um membro individual de uma variável do tipo estrutura, utiliza-se o **operador de acesso a membro**, que é o **ponto (`.`)**. A sintaxe é:

```c
nome_da_variavel_estrutura.nome_do_membro
```

**Exemplo de Acesso a Membros em C:**

```c
#include <stdio.h>
#include <string.h>

struct Livro {
    char titulo[100];
    char autor[50];
    int ano_publicacao;
    float preco;
};

int main() {
    struct Livro livro_favorito;

    // Atribuindo valores aos membros usando o operador ponto (.)
    strcpy(livro_favorito.titulo, "O Guia do Mochileiro das Galáxias");
    strcpy(livro_favorito.autor, "Douglas Adams");
    livro_favorito.ano_publicacao = 1979;
    livro_favorito.preco = 35.90f;

    // Lendo e imprimindo os valores dos membros
    printf("Título: %s\n", livro_favorito.titulo);
    printf("Autor: %s\n", livro_favorito.autor);
    printf("Ano: %d\n", livro_favorito.ano_publicacao);
    printf("Preço: R$%.2f\n", livro_favorito.preco);

    // Membros de estruturas podem ser usados em expressões
    float preco_com_desconto = livro_favorito.preco * 0.9; // 10% de desconto
    printf("Preço com desconto: R$%.2f\n", preco_com_desconto);

    return 0;
}
```

Cada membro de uma estrutura é tratado como uma variável comum do seu respectivo tipo.

## Inicialização de Estruturas

Assim como variáveis de tipos básicos e arrays, as variáveis de estrutura podem ser inicializadas no momento de sua declaração. Isso é feito fornecendo uma lista de valores entre chaves `{}`, na mesma ordem em que os membros foram definidos na estrutura.

**Exemplos de Inicialização de Estruturas em C:**

1. **Inicialização completa na declaração:**
    
    ```c
    #include <stdio.h>
    
    struct Data {
        int dia;
        int mes;
        int ano;
    };
    
    int main() {
        // Inicializa a estrutura 'hoje' com valores para dia, mes e ano
        struct Data hoje = {28, 5, 2025}; 
    
        printf("Data de hoje: %02d/%02d/%d\n", hoje.dia, hoje.mes, hoje.ano);
    
        struct Livro {
            char titulo[50];
            int paginas;
        };
        // Para strings (arrays de char), pode-se usar um literal de string
        struct Livro meu_livro = {"Aventuras em C", 350};
        printf("Livro: %s, Páginas: %d\n", meu_livro.titulo, meu_livro.paginas);
    
        return 0;
    }
    ```
    
2. **Inicializadores Designados (C99 em diante):** O padrão C99 introduziu os inicializadores designados, que permitem inicializar membros específicos pelo nome, em qualquer ordem. Isso torna a inicialização mais legível e menos propensa a erros se a ordem dos membros na definição da estrutura mudar.
    
    ```c
    #include <stdio.h>
    
    struct Produto {
        int codigo;
        char nome[30];
        float preco;
    };
    
    int main() {
        // Usando inicializadores designados (C99)
        struct Produto item1 = {.nome = "Caneta", .codigo = 101, .preco = 2.50f};
        struct Produto item2 = {.preco = 15.75f, .nome = "Caderno"}; // 'codigo' será 0
    
        printf("Item 1: Cod=%d, Nome=%s, Preço=%.2f\n", item1.codigo, item1.nome, item1.preco);
        printf("Item 2: Cod=%d, Nome=%s, Preço=%.2f\n", item2.codigo, item2.nome, item2.preco);
    
        return 0;
    }
    ```
    
    Se um membro não for explicitamente inicializado com um inicializador designado (como `codigo` em `item2`), ele será inicializado com zero (ou o equivalente para seu tipo), assumindo que pelo menos um membro foi inicializado dessa forma ou que a lista de inicializadores está presente. Se a lista de inicializadores `{}` estiver completamente vazia (ex: `struct Produto item3 = {};`), todos os membros são inicializados com zero.

Se uma variável de estrutura local não for inicializada, seus membros conterão valores indefinidos ("lixo"). Estruturas globais ou `static` têm seus membros inicializados com zero por padrão se nenhuma inicialização explícita for fornecida.

## Estruturas e Ponteiros

Ponteiros podem ser usados para apontar para variáveis de estrutura, assim como podem apontar para variáveis de tipos básicos ou arrays. Um ponteiro para uma estrutura armazena o endereço de memória do início da variável estrutura.

**Declaração de um Ponteiro para Estrutura:**

```c
struct MinhaStruct {
    int membro1;
    float membro2;
};

struct MinhaStruct instancia_ms;
struct MinhaStruct *ptr_ms; // Declara um ponteiro para struct MinhaStruct

ptr_ms = &instancia_ms; // ptr_ms agora aponta para instancia_ms
```

### Acessando Membros Através de um Ponteiro para Estrutura

Existem duas maneiras de acessar os membros de uma estrutura através de um ponteiro que aponta para ela:

1. **Usando o operador de dereferência (`*`) e o operador ponto (`.`):** Primeiro, dereferencia-se o ponteiro para obter a estrutura em si, e então usa-se o operador ponto para acessar o membro. Devido à precedência de operadores, parênteses são necessários ao redor do ponteiro dereferenciado. Sintaxe: `(*nome_do_ponteiro_estrutura).nome_do_membro`
2. **Usando o Operador Seta (`->`):** A linguagem C fornece um operador mais conveniente e legível para acessar membros de estruturas através de ponteiros: o operador seta (`->`). Ele combina a dereferência e o acesso ao membro em uma única operação. Sintaxe: `nome_do_ponteiro_estrutura->nome_do_membro`

Ambas as formas são equivalentes. O operador seta é geralmente preferido por sua concisão.

**Exemplo com Ponteiros para Estrutura em C:**

```c
#include <stdio.h>
#include <string.h>

struct Funcionario {
    char nome[50];
    int id;
    double salario;
};

int main() {
    struct Funcionario func1;
    struct Funcionario *ptr_func; // Ponteiro para struct Funcionario

    ptr_func = &func1; // ptr_func aponta para func1

    // Atribuindo valores aos membros usando o operador seta (->)
    strcpy(ptr_func->nome, "Carlos Alberto");
    ptr_func->id = 778;
    ptr_func->salario = 3500.00;

    printf("Dados do Funcionário (via ponteiro ->):\n");
    printf("Nome: %s\n", ptr_func->nome);
    printf("ID: %d\n", ptr_func->id);
    printf("Salário: %.2f\n", ptr_func->salario);

    // Atribuindo valores usando (*ponteiro).membro
    (*ptr_func).id = 779; // Modificando o ID
    printf("\nID modificado (via (*ptr).membro): %d\n", (*ptr_func).id);
    printf("Nome (verificando): %s\n", func1.nome); // func1 foi alterada

    return 0;
}
```

## Estruturas como Parâmetros de Funções

Estruturas podem ser passadas para funções como argumentos e também podem ser retornadas por funções.

### Passando Estruturas por Valor

Por padrão, quando uma variável de estrutura é passada como argumento para uma função em C, ela é passada **por valor**. Isso significa que uma **cópia completa da estrutura** é feita e passada para a função. Modificações feitas na cópia da estrutura dentro da função **não afetam** a estrutura original na função chamadora.

**Exemplo de Passagem de Estrutura por Valor:**

```c
#include <stdio.h>

struct Retangulo {
    int largura;
    int altura;
};

// Função recebe uma cópia da struct Retangulo
void imprimir_retangulo_valor(struct Retangulo r) {
    printf("Retângulo (por valor): Largura=%d, Altura=%d\n", r.largura, r.altura);
    r.largura = 100; // Modifica a cópia local, não afeta o original
}

int main() {
    struct Retangulo meu_ret = {10, 5};

    printf("Antes da função: Largura=%d, Altura=%d\n", meu_ret.largura, meu_ret.altura);
    imprimir_retangulo_valor(meu_ret);
    printf("Depois da função: Largura=%d, Altura=%d\n", meu_ret.largura, meu_ret.altura);
    // Saída mostrará que meu_ret.largura ainda é 10

    return 0;
}
```

A passagem por valor pode ser ineficiente para estruturas grandes, pois copiar todos os membros pode consumir tempo e memória.

### Passando Ponteiros para Estruturas para Funções

Para evitar a sobrecarga da cópia de estruturas grandes e/ou para permitir que uma função modifique a estrutura original, é comum passar um **ponteiro para a estrutura** como argumento.

**Exemplo de Passagem de Ponteiro para Estrutura:**

```c
#include <stdio.h>

struct Retangulo {
    int largura;
    int altura;
};

// Função recebe um ponteiro para struct Retangulo
void imprimir_e_modificar_retangulo_ptr(struct Retangulo *ptr_r) {
    if (ptr_r != NULL) {
        printf("Retângulo (via ponteiro): Largura=%d, Altura=%d\n", ptr_r->largura, ptr_r->altura);
        // Modificando a estrutura original através do ponteiro
        ptr_r->largura = 200;
        ptr_r->altura = 100;
    }
}

int main() {
    struct Retangulo meu_ret = {15, 7};

    printf("Antes da função: Largura=%d, Altura=%d\n", meu_ret.largura, meu_ret.altura);
    imprimir_e_modificar_retangulo_ptr(&meu_ret); // Passa o ENDEREÇO da estrutura
    printf("Depois da função: Largura=%d, Altura=%d\n", meu_ret.largura, meu_ret.altura);
    // Saída mostrará que meu_ret.largura é 200 e altura é 100

    return 0;
}
```

Esta abordagem é mais eficiente para estruturas grandes e é a maneira padrão de permitir que uma função altere uma estrutura da função chamadora.

### Retornando Estruturas de Funções

Uma função também pode retornar uma variável do tipo estrutura.

```c
#include <stdio.h>

struct Ponto2D {
    int x;
    int y;
};

// Função que cria e retorna uma struct Ponto2D
struct Ponto2D criar_ponto(int x_coord, int y_coord) {
    struct Ponto2D novo_ponto;
    novo_ponto.x = x_coord;
    novo_ponto.y = y_coord;
    return novo_ponto; // Uma cópia da estrutura 'novo_ponto' é retornada
}

int main() {
    struct Ponto2D pA, pB;

    pA = criar_ponto(10, 20);
    pB = criar_ponto(-5, 15);

    printf("Ponto A: (%d, %d)\n", pA.x, pA.y);
    printf("Ponto B: (%d, %d)\n", pB.x, pB.y);

    return 0;
}
```

Ao retornar uma estrutura, uma cópia da estrutura é feita, similar à passagem por valor.

## Arrays de Estruturas

Assim como podemos ter arrays de `int` ou `char`, também podemos ter **arrays de estruturas**. Um array de estruturas é uma coleção de múltiplas instâncias do mesmo tipo de estrutura, armazenadas consecutivamente na memória.

**Declaração e Inicialização de Array de Estruturas:**

```c
#include <stdio.h>
#include <string.h>

struct Contato {
    char nome[50];
    char telefone[15];
};

#define NUM_CONTATOS 3

int main() {
    // Declarando um array de 3 estruturas Contato
    struct Contato agenda[NUM_CONTATOS];

    // Inicializando o array de estruturas
    struct Contato lista_amigos[NUM_CONTATOS] = {
        {"Ana", "111-1111"},
        {"Bruno", "222-2222"},
        {"Carla", "333-3333"}
    };

    // Atribuindo valores a um elemento do array 'agenda'
    strcpy(agenda[0].nome, "Daniel");
    strcpy(agenda[0].telefone, "444-4444");

    // Acessando e imprimindo
    printf("Primeiro contato da agenda: %s, Tel: %s\n", agenda[0].nome, agenda[0].telefone);

    printf("\nLista de Amigos:\n");
    for (int i = 0; i < NUM_CONTATOS; i++) {
        printf("Nome: %s, Telefone: %s\n", lista_amigos[i].nome, lista_amigos[i].telefone);
    }

    return 0;
}
```

Para acessar um membro de uma estrutura específica dentro do array, você primeiro usa o índice do array e depois o operador ponto: `nome_do_array[indice].nome_do_membro`.

## Estruturas Aninhadas

Uma estrutura pode conter membros que são, eles próprios, outras estruturas. Isso é chamado de **estrutura aninhada**. É uma forma poderosa de criar tipos de dados ainda mais complexos e hierárquicos.

**Exemplo de Estrutura Aninhada:**

```c
#include <stdio.h>
#include <string.h>

struct Endereco {
    char rua[100];
    int numero;
    char cidade[50];
};

struct Pessoa {
    char nome[50];
    int idade;
    struct Endereco moradia; // Membro 'moradia' é do tipo struct Endereco
};

int main() {
    struct Pessoa p1;

    strcpy(p1.nome, "Fernanda Lima");
    p1.idade = 28;
    strcpy(p1.moradia.rua, "Rua das Palmeiras"); // Acessando membro da struct aninhada
    p1.moradia.numero = 123;
    strcpy(p1.moradia.cidade, "São Paulo");

    printf("Nome: %s\n", p1.nome);
    printf("Idade: %d\n", p1.idade);
    printf("Endereço: %s, %d, %s\n", p1.moradia.rua, p1.moradia.numero, p1.moradia.cidade);

    // Inicialização de struct com membro struct aninhado
    struct Pessoa p2 = {
        "Ricardo Alves",
        35,
        {"Avenida Central", 789, "Rio de Janeiro"} // Inicializador para a struct Endereco
    };
    printf("\nNome: %s, Idade: %d, Endereço: %s, %d, %s\n",
           p2.nome, p2.idade, p2.moradia.rua, p2.moradia.numero, p2.moradia.cidade);

    return 0;
}
```

Para acessar um membro da estrutura interna, você usa o operador ponto duas vezes: `variavel_externa.membro_estrutura_interna.membro_interno`.

## `typedef` com Estruturas

A palavra-chave `typedef` em C permite criar um **sinônimo (alias)** para um tipo de dado existente. Ela é frequentemente usada com estruturas para simplificar a declaração de variáveis de estrutura, eliminando a necessidade de escrever `struct nome_da_tag` repetidamente.

**Sintaxe:**

```c
typedef struct nome_da_tag_opcional {
    // membros
} NOME_DO_NOVO_TIPO;
```

Agora, `NOME_DO_NOVO_TIPO` pode ser usado diretamente para declarar variáveis, como se fosse um tipo fundamental.

**Exemplo com `typedef`:**

```c
#include <stdio.h>
#include <string.h>

// Definindo a estrutura e criando um typedef chamado 'PontoCartesiano'
typedef struct { // A tag aqui (entre struct e {) é opcional se não for referenciar antes do typedef
    double x;
    double y;
} PontoCartesiano;

typedef struct DataInfo { // Aqui, DataInfo é a tag
    int dia, mes, ano;
} Data; // Data é o novo nome do tipo

int main() {
    // Declarando variáveis usando o typedef
    PontoCartesiano p1, p2;
    Data nascimento;

    p1.x = 1.0;
    p1.y = 2.5;

    nascimento.dia = 15;
    nascimento.mes = 10;
    nascimento.ano = 1990;

    printf("Ponto p1: (%.1f, %.1f)\n", p1.x, p1.y);
    printf("Data de nascimento: %02d/%02d/%d\n", nascimento.dia, nascimento.mes, nascimento.ano);

    return 0;
}
```

O uso de `typedef` torna o código mais limpo e mais parecido com a declaração de variáveis de tipos fundamentais.

## Estruturas em C++

O C++ herda as estruturas (`struct`) do C, e elas funcionam de maneira muito similar. A principal diferença sintática é que em C++, uma vez que você define `struct NomeStruct { ... };`, você pode declarar variáveis simplesmente usando `NomeStruct var;` sem a necessidade da palavra-chave `struct` antes, nem de `typedef` para esse propósito.

```cpp
// Exemplo em C++
#include <iostream>
#include <string>

struct LivroCpp {
    std::string titulo;
    std::string autor;
    int ano;
}; // Ponto e vírgula ainda é obrigatório

int main() {
    LivroCpp meuLivro; // Não precisa de 'struct LivroCpp meuLivro;'
    meuLivro.titulo = "C++ Moderno";
    meuLivro.autor = "Bjarne Stroustrup"; // Exemplo
    meuLivro.ano = 2020;

    std::cout << "Título: " << meuLivro.titulo << std::endl;
    std::cout << "Autor: " << meuLivro.autor << std::endl;
    std::cout << "Ano: " << meuLivro.ano << std::endl;

    // Inicialização com lista (C++11 em diante)
    LivroCpp outroLivro = {"Aprendendo C++", "Autor Desconhecido", 2021};
    std::cout << "\nOutro Livro Título: " << outroLivro.titulo << std::endl;
    
    return 0;
}
```

Além disso, em C++, estruturas são quase idênticas a classes. A única diferença fundamental entre uma `struct` e uma `class` em C++ é a **visibilidade padrão de seus membros**:

- Em uma `struct`, os membros são `public` por padrão.
- Em uma `class`, os membros são `private` por padrão.

Isso significa que `struct`s em C++ podem ter construtores, destrutores, funções membro (métodos), herança e todos os outros recursos da programação orientada a objetos, assim como as classes. Frequentemente, `struct`s são usadas em C++ para tipos de dados simples que são principalmente contêineres de dados (Plain Old Data - POD, ou agregados), enquanto `class`es são usadas para objetos mais complexos com comportamento e encapsulamento.

**Inicialização de Membros na Definição (C++11 em diante):** C++11 permite a inicialização de membros não estáticos diretamente na definição da `struct` (ou `class`):

```cpp
// C++11 em diante
struct Config {
    int timeout = 1000; // Valor padrão
    std::string nomeServidor = "localhost";
    bool habilitado = true;
};

int main() {
    Config cfg_padrao; // timeout=1000, nomeServidor="localhost", habilitado=true
    Config cfg_custom = {500, "servidor.com", false};
    // ...
    return 0;
}
```

Isso não é possível em C.

## Considerações Finais

Neste capítulo, exploramos as **estruturas (registros)**, uma ferramenta fundamental em C e C++ para agrupar dados de tipos diferentes em uma única unidade lógica. Vimos como definir um tipo `struct`, declarar variáveis desse tipo e acessar seus membros individuais usando o operador ponto (`.`). A inicialização de estruturas, tanto a tradicional quanto a com inicializadores designados (C99), foi demonstrada.

A interação entre estruturas e ponteiros também foi um ponto chave, introduzindo o operador seta (`->`) como uma forma conveniente de acessar membros através de um ponteiro para estrutura. Discutimos a passagem de estruturas para funções, tanto por valor (criando uma cópia) quanto por endereço (usando ponteiros, o que é mais eficiente para estruturas grandes e permite modificações na original), e como funções podem retornar estruturas.

Abordamos os arrays de estruturas, que nos permitem criar coleções de registros, e as estruturas aninhadas, para modelar hierarquias de dados mais complexas. O uso de `typedef` para criar sinônimos para tipos estrutura em C foi apresentado como uma forma de simplificar declarações.

Finalmente, fizemos a transição para o C++, observando que as `structs` são herdadas do C com uma sintaxe de declaração de variável mais direta e que, em C++, `structs` são funcionalmente muito próximas das `classes`, diferindo principalmente na visibilidade padrão de seus membros. A capacidade de inicializar membros diretamente na definição em C++11 também foi mencionada.

As estruturas são a base para a criação de tipos de dados personalizados e mais significativos, permitindo que os programas modelem entidades do mundo real de forma mais eficaz. Elas são um passo crucial antes de mergulharmos em conceitos mais avançados de organização de dados e na programação orientada a objetos do C++.
# Capítulo 4 – Entrada e Saída de Dados

Nos capítulos anteriores, exploramos como os dados são definidos, armazenados em variáveis e manipulados através de operadores. No entanto, um programa raramente existe em isolamento; sua utilidade muitas vezes reside na capacidade de interagir com o mundo exterior, seja para receber informações do usuário, seja para apresentar resultados. Essa comunicação é realizada através de operações de **entrada e saída (E/S)**, também conhecidas como **Input/Output (I/O)**.

A entrada de dados permite que um programa receba valores que podem influenciar seu comportamento ou serem processados. A saída de dados, por sua vez, é o meio pelo qual o programa exibe informações, resultados de cálculos, mensagens de status ou qualquer outro dado relevante para o usuário. Sem mecanismos eficazes de E/S, os programas seriam caixas-pretas, incapazes de se adaptar a diferentes cenários ou de comunicar suas descobertas.

Neste capítulo, investigaremos as principais formas de realizar entrada e saída de dados na linguagem C, focando nas funções fornecidas pela biblioteca padrão `stdio.h`. Abordaremos a saída formatada com `printf()`, a entrada formatada com `scanf()`, e funções específicas para manipulação de caracteres e strings. Em seguida, faremos uma transição para o C++, apresentando o sistema de **streams** da biblioteca `iostream`, que oferece uma abordagem mais moderna e orientada a objetos para as operações de E/S. Ao final, você terá o conhecimento necessário para construir programas interativos, capazes de dialogar com o usuário de forma eficaz.

## Entrada e Saída Padrão em C: A Biblioteca `stdio.h`

A linguagem C não possui palavras-chave intrínsecas para operações de entrada e saída. Em vez disso, essas funcionalidades são fornecidas através de um conjunto de funções definidas na **biblioteca padrão de entrada/saída**, cujo arquivo de cabeçalho é `stdio.h` (Standard Input/Output Header). Para utilizar essas funções, é imprescindível incluir este cabeçalho no início do seu programa C:

```c
#include <stdio.h>
```

As operações de E/S mais comuns envolvem os chamados **fluxos padrão (standard streams)**:

- **`stdin` (Standard Input):** Fluxo de entrada padrão, geralmente associado ao teclado.
- **`stdout` (Standard Output):** Fluxo de saída padrão, geralmente associado à tela (console).
- **`stderr` (Standard Error):** Fluxo de saída de erro padrão, também geralmente associado à tela, usado para exibir mensagens de erro.

### Saída Formatada com `printf()`

A função `printf()` (abreviação de "print formatted") é a principal ferramenta para exibir dados formatados no fluxo de saída padrão (`stdout`). Sua versatilidade permite imprimir texto simples, valores de variáveis de diversos tipos e combinações complexas, com controle sobre o formato da saída.

A forma geral da função `printf()` é:

```c
int printf(const char *formato, ...);
```

O primeiro argumento, `formato`, é uma string de controle que pode conter:

1. **Caracteres literais:** Texto que será impresso exatamente como aparece.
2. **Especificadores de formato:** Sequências que começam com `%` e indicam o tipo e o formato do dado que será impresso a partir dos argumentos subsequentes.
3. **Sequências de escape:** Caracteres especiais precedidos por uma barra invertida `\` (como `\n` para nova linha).

Os argumentos subsequentes (`...`) são os valores ou variáveis que serão formatados e impressos de acordo com os especificadores de formato presentes na string de controle. A função retorna o número de caracteres impressos com sucesso ou um valor negativo em caso de erro.

**Exemplo básico:**

```c
#include <stdio.h>

int main() {
    printf("Olá, Mundo!\n"); // Imprime uma string literal e uma nova linha
    int idade = 30;
    printf("Eu tenho %d anos.\n", idade); // Imprime uma string com o valor da variável 'idade'
    return 0;
}
```

**Principais Especificadores de Formato para `printf()`:**

|Especificador|Tipo de Dado Esperado|Descrição|
|---|---|---|
|`%d` ou `%i`|`int`|Inteiro decimal com sinal.|
|`%u`|`unsigned int`|Inteiro decimal sem sinal.|
|`%f`|`double` (ou `float` promovido)|Ponto flutuante decimal (notação normal).|
|`%e` ou `%E`|`double`|Ponto flutuante decimal (notação científica).|
|`%g` ou `%G`|`double`|Usa `%f` ou `%e` (o mais curto).|
|`%c`|`int` (interpretado como `char`)|Caractere único.|
|`%s`|`char *` (ponteiro para char)|Sequência de caracteres (string).|
|`%p`|`void *`|Endereço de ponteiro (formato hexadecimal).|
|`%%`|Nenhum|Imprime o caractere `%`.|
|`%o`|`unsigned int`|Inteiro octal sem sinal.|
|`%x` ou `%X`|`unsigned int`|Inteiro hexadecimal sem sinal (minúsculas/maiúsculas).|

**Exemplo com múltiplos especificadores:**

```c
#include <stdio.h>

int main() {
    char inicial = 'J';
    int quantidade = 15;
    float preco = 49.99f;
    char produto[] = "Livro"; // String em C

    printf("Produto: %s\n", produto);
    printf("Inicial do autor: %c\n", inicial);
    printf("Quantidade em estoque: %d unidades\n", quantidade);
    printf("Preço unitário: R$%.2f\n", preco); // "%.2f" imprime com 2 casas decimais
    return 0;
}
```

No exemplo acima, `%.2f` é um especificador de formato que instrui `printf` a imprimir o valor de `preco` como um número de ponto flutuante com exatamente duas casas decimais.

**Sequências de Escape Comuns:**

|Sequência|Significado|
|---|---|
|`\n`|Nova linha|
|`\t`|Tabulação horizontal|
|`\r`|Retorno de carro|
|`\\`|Barra invertida|
|`\"`|Aspas duplas|
|`\'`|Aspas simples|
|`\b`|Retrocesso (Backspace)|
|`\a`|Alerta (Bell)|

### Entrada Formatada com `scanf()`

A função `scanf()` (abreviação de "scan formatted") é usada para ler dados formatados do fluxo de entrada padrão (`stdin`), geralmente o teclado, e armazená-los em variáveis.

A forma geral da função `scanf()` é:

```c
int scanf(const char *formato, ...);
```

Similarmente a `printf()`, o primeiro argumento, `formato`, é uma string de controle que contém **especificadores de formato** indicando o tipo de dado esperado da entrada. Os argumentos subsequentes (`...`) são **ponteiros para as variáveis** onde os dados lidos serão armazenados. É crucial passar o **endereço** da variável (usando o operador `&`) para que `scanf()` possa modificar seu valor.

A função `scanf()` retorna o número de itens lidos e atribuídos com sucesso, ou `EOF` (End Of File) se ocorrer um erro de entrada antes que qualquer conversão bem-sucedida seja feita.

**Principais Especificadores de Formato para `scanf()`:** São muito similares aos de `printf()`, mas com algumas nuances importantes:

- `%d`: Lê um inteiro decimal.
- `%ld`: Lê um `long int`.
- `%f`: Lê um `float`.
- `%lf`: Lê um `double` (o `l` é importante aqui para `double`).
- `%c`: Lê um único caractere.
- `%s`: Lê uma sequência de caracteres (string) até encontrar um espaço em branco (espaço, tabulação, nova linha). **Cuidado:** `scanf("%s", str)` pode causar estouro de buffer se a string de entrada for maior que o tamanho alocado para `str`.
- `%u`: Lê um `unsigned int`.

**Exemplo básico de `scanf()`:**

```c
#include <stdio.h>

int main() {
    int idade;
    float altura;
    char inicial_nome;

    printf("Digite sua idade: ");
    scanf("%d", &idade); // Passa o ENDEREÇO de 'idade'

    printf("Digite sua altura (em metros): ");
    scanf("%f", &altura); // Passa o ENDEREÇO de 'altura'

    printf("Digite a inicial do seu primeiro nome: ");
    // Adiciona um espaço antes de %c para consumir qualquer nova linha pendente do buffer
    scanf(" %c", &inicial_nome); 

    printf("\n--- Dados Lidos ---\n");
    printf("Idade: %d anos\n", idade);
    printf("Altura: %.2f m\n", altura);
    printf("Inicial: %c\n", inicial_nome);

    return 0;
}
```

**Importante:**

- Ao ler um `char` com `%c` após uma leitura numérica (como `%d` ou `%f`), é comum que o caractere de nova linha (`\n`) pressionado após o número fique no buffer de entrada e seja lido pelo `%c`. Para evitar isso, pode-se colocar um espaço antes do `%c` na string de formato (`" %c"`), que instrui `scanf()` a ignorar quaisquer caracteres de espaço em branco pendentes.
- Ao ler strings com `%s`, `scanf()` para no primeiro caractere de espaço em branco. Para ler linhas inteiras com espaços, outras funções como `fgets()` são mais adequadas.
- **Sempre use o operador de endereço `&`** antes do nome da variável em `scanf()` (exceto para arrays de caracteres/strings, onde o nome do array já representa seu endereço).

### Funções para Caracteres: `getchar()` e `putchar()`

Para entrada e saída de caracteres individuais, C oferece as funções `getchar()` e `putchar()`.

- **`int getchar(void);`**: Lê o próximo caractere do `stdin` e o retorna como um `int` (ou `EOF` no final do arquivo ou erro).
- **`int putchar(int caractere);`**: Escreve o `caractere` (convertido para `unsigned char`) no `stdout` e retorna o caractere escrito (ou `EOF` em caso de erro).

**Exemplo com `getchar()` e `putchar()`:**

```c
#include <stdio.h>

int main() {
    char ch;
    printf("Digite um caractere: ");
    ch = getchar(); // Lê um caractere

    printf("Você digitou: ");
    putchar(ch);    // Imprime o caractere
    putchar('\n');  // Imprime uma nova linha

    return 0;
}
```

### Funções para Strings: `gets()`, `puts()` e `fgets()`

Para entrada e saída de linhas inteiras de texto (strings):

- **`char *gets(char *str);` (OBSOLETA E PERIGOSA):** Lê uma linha do `stdin` e a armazena na string `str` até que um caractere de nova linha seja encontrado ou `EOF`. O `\n` é substituído por `\0`. **`gets()` é extremamente perigosa porque não verifica o tamanho do buffer `str`, podendo causar estouro de buffer, uma grave falha de segurança. Seu uso é fortemente desencorajado e foi removida do padrão C11.**
- **`int puts(const char *str);`**: Escreve a string `str` no `stdout`, seguida por um caractere de nova linha (`\n`). Retorna um valor não negativo em caso de sucesso, ou `EOF` em erro.
- **`char *fgets(char *str, int tamanho, FILE *fluxo);`**: Lê uma linha do `fluxo` especificado (ex: `stdin`) e a armazena em `str`. Lê no máximo `tamanho-1` caracteres, parando se encontrar `\n` ou `EOF`. O `\n` (se lido) é mantido na string, e um `\0` é sempre adicionado ao final. É uma alternativa muito mais segura a `gets()`.

**Exemplo com `fgets()` e `puts()`:**

```c
#include <stdio.h>

#define TAMANHO_BUFFER 50

int main() {
    char nome[TAMANHO_BUFFER];

    printf("Digite seu nome completo: ");
    // Lê até TAMANHO_BUFFER-1 caracteres de stdin e armazena em nome
    // Inclui o '\n' se houver espaço e o usuário pressionar Enter.
    if (fgets(nome, TAMANHO_BUFFER, stdin) != NULL) {
        // fgets pode incluir a nova linha, vamos removê-la se presente
        // para uma saída mais limpa com puts.
        // (Uma forma simples de remover, mas pode não ser a mais robusta)
        for (int i = 0; i < TAMANHO_BUFFER; i++) {
            if (nome[i] == '\n') {
                nome[i] = '\0';
                break;
            }
            if (nome[i] == '\0') { // String terminou antes do \n
                break;
            }
        }
        
        printf("Olá, ");
        puts(nome); // puts adiciona automaticamente uma nova linha
    } else {
        printf("Erro ao ler o nome.\n");
    }

    return 0;
}
```

## Entrada e Saída em C++: O Poder dos Streams

Enquanto C utiliza um conjunto de funções para E/S, C++ introduz um sistema mais robusto, flexível e orientado a objetos baseado em **streams (fluxos)**. A biblioteca principal para isso é a `<iostream>`.

Os principais objetos de stream para E/S padrão são:

- **`std::cin`**: Objeto de stream associado à entrada padrão (geralmente o teclado).
- **`std::cout`**: Objeto de stream associado à saída padrão (geralmente a tela).
- **`std::cerr`**: Objeto de stream associado à saída de erro padrão, não bufferizado.
- **`std::clog`**: Objeto de stream associado à saída de erro padrão, bufferizado.

Para usá-los, inclua o cabeçalho `<iostream>`:

```cpp
#include <iostream>
```

### Saída com `std::cout` e o Operador de Inserção `<<`

Em C++, a saída de dados para o console é tipicamente realizada usando o objeto `std::cout` em conjunto com o **operador de inserção de fluxo (`<<`)**, também conhecido como "operador put to". Este operador "insere" os dados da sua direita no fluxo da sua esquerda.

```cpp
#include <iostream>
#include <string> // Para std::string

int main() {
    std::cout << "Olá, C++!" << std::endl; // std::endl insere uma nova linha e descarrega o buffer

    int idade = 25;
    double altura = 1.75;
    std::string nome = "Carlos";

    std::cout << "Nome: " << nome << ", Idade: " << idade << ", Altura: " << altura << "m" << std::endl;
    // Múltiplas inserções podem ser encadeadas
    return 0;
}
```

O `std::endl` é um **manipulador de fluxo** que insere um caractere de nova linha e força a descarga (flush) do buffer de saída, garantindo que o texto seja imediatamente exibido.

Para formatação mais precisa, como definir o número de casas decimais para números de ponto flutuante, C++ utiliza manipuladores da biblioteca `<iomanip>`:

```cpp
#include <iostream>
#include <iomanip> // Para std::fixed e std::setprecision

int main() {
    double pi = 3.1415926535;
    std::cout << "Valor de PI (padrão): " << pi << std::endl;
    std::cout << "Valor de PI (com 2 casas): " << std::fixed << std::setprecision(2) << pi << std::endl;
    std::cout << "Valor de PI (com 4 casas): " << std::fixed << std::setprecision(4) << pi << std::endl;
    return 0;
}
```

`std::fixed` força a notação de ponto fixo, e `std::setprecision(n)` define o número de dígitos após o ponto decimal.

### Entrada com `std::cin` e o Operador de Extração `>>`

Para ler dados da entrada padrão, C++ utiliza o objeto `std::cin` com o **operador de extração de fluxo (`>>`)**, também conhecido como "operador get from". Este operador "extrai" dados do fluxo da sua esquerda e os armazena na variável da sua direita.

`std::cin` é inteligente o suficiente para tentar converter a entrada para o tipo da variável receptora. Por padrão, ele ignora espaços em branco iniciais (espaços, tabulações, novas linhas) e para de ler quando encontra um espaço em branco após os dados ou um caractere incompatível com o tipo esperado.

```cpp
#include <iostream>
#include <string>

int main() {
    int idade;
    double salario;
    std::string primeiroNome;

    std::cout << "Digite sua idade: ";
    std::cin >> idade;

    std::cout << "Digite seu primeiro nome: ";
    std::cin >> primeiroNome; // Lê apenas até o primeiro espaço

    std::cout << "Digite seu salário: ";
    std::cin >> salario;

    std::cout << "\n--- Dados Lidos ---\n";
    std::cout << "Nome: " << primeiroNome << std::endl;
    std::cout << "Idade: " << idade << " anos" << std::endl;
    std::cout << "Salário: " << std::fixed << std::setprecision(2) << salario << std::endl;

    return 0;
}
````

Se o usuário digitar "João Silva" para o nome, `std::cin >> primeiroNome;` armazenará apenas "João" em `primeiroNome`, e "Silva" permanecerá no buffer de entrada para a próxima operação de leitura.

Para ler uma linha inteira de texto, incluindo espaços, a função `std::getline()` é preferível:

```cpp
#include <iostream>
#include <string>

int main() {
    std::string nomeCompleto;
    int idade;

    std::cout << "Digite sua idade: ";
    std::cin >> idade;

    // Consumir o '\n' deixado por std::cin >> idade antes de chamar getline
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); 
    // Ou, de forma mais simples em muitos casos: std::cin.ignore(); 

    std::cout << "Digite seu nome completo: ";
    std::getline(std::cin, nomeCompleto);

    std::cout << "\n--- Dados Lidos ---\n";
    std::cout << "Nome Completo: " << nomeCompleto << std::endl;
    std::cout << "Idade: " << idade << " anos" << std::endl;

    return 0;
}
```

A linha `std::cin.ignore(...)` é importante após uma leitura com `std::cin >> variavel` se a próxima leitura for com `std::getline()`. Isso ocorre porque `std::cin >> variavel` deixa o caractere de nova linha (`\n`) no buffer de entrada, que seria imediatamente consumido por `std::getline()`, resultando em uma string vazia. `std::cin.ignore()` descarta esse `\n` pendente.

## Considerações Finais

Neste capítulo, exploramos os mecanismos fundamentais para a interação de programas C e C++ com o usuário: a entrada e saída de dados. Em C, aprendemos sobre as funções da biblioteca `stdio.h`, com destaque para `printf()` e `scanf()` para operações formatadas, e funções como `getchar()`, `putchar()`, `fgets()` e `puts()` para manipulação de caracteres e strings, ressaltando a importância da segurança com `fgets()` em detrimento do obsoleto `gets()`.

Em seguida, introduzimos a abordagem orientada a objetos do C++ para E/S, utilizando a biblioteca `<iostream>` e os objetos `std::cin` e `std::cout` em conjunto com os operadores de extração (`>>`) e inserção (`<<`). Vimos como C++ simplifica muitas operações e oferece ferramentas como `std::getline()` para leitura de linhas completas e manipuladores em `<iomanip>` para formatação de saída.

A capacidade de receber dados do usuário e apresentar resultados de forma clara é essencial para a maioria das aplicações. Com as ferramentas apresentadas, você está equipado para construir programas mais interativos e comunicativos. Nos próximos capítulos, continuaremos a construir sobre esses fundamentos, explorando estruturas de controle que permitirão aos seus programas tomar decisões e repetir ações com base, muitas vezes, nos dados recebidos do usuário.
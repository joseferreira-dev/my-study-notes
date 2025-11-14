# Capítulo 4 – Entrada e Saída de Dados

Nos capítulos anteriores, exploramos como os dados são definidos, armazenados em variáveis e manipulados através de operadores. No entanto, um programa raramente existe em isolamento; sua utilidade fundamental reside na capacidade de interagir com o mundo exterior, seja para receber informações do usuário, seja para apresentar resultados. Essa comunicação é realizada através de operações de **entrada e saída (E/S)**, também conhecidas como **Input/Output (I/O)**. A entrada de dados permite que um programa receba valores que podem influenciar seu comportamento ou serem processados. A saída de dados, por sua vez, é o meio pelo qual o programa exibe informações, resultados de cálculos, mensagens de status ou qualquer outro dado relevante para o usuário. Sem mecanismos eficazes de E/S, os programas seriam caixas-pretas, incapazes de se adaptar a diferentes cenários ou de comunicar suas descobertas.

Neste capítulo, investigaremos as principais formas de realizar entrada e saída de dados. Começaremos com a abordagem tradicional da linguagem C, focando nas funções fornecidas pela biblioteca padrão `stdio.h`. Abordaremos a saída formatada com `printf()`, a complexa e crítica entrada formatada com `scanf()`, e funções específicas para manipulação de caracteres (`getchar`, `putchar`) e strings seguras (`fgets`, `puts`). Em seguida, faremos uma transição para o C++, apresentando o sistema de **streams** da biblioteca `iostream`, que oferece uma abordagem mais moderna, segura e orientada a objetos para as operações de E/S, utilizando `cout` e `cin`.

## Entrada e Saída Padrão em C

A linguagem C, em sua filosofia minimalista, não possui palavras-chave intrínsecas para operações de entrada e saída. Em vez disso, essas funcionalidades são fornecidas através de um conjunto de funções definidas na **biblioteca padrão de entrada/saída**, cujo arquivo de cabeçalho é `stdio.h` (Standard Input/Output Header). Para utilizar qualquer uma dessas funções (como `printf` ou `scanf`), é imprescindível incluir este cabeçalho no início do programa C usando a diretiva de pré-processador:

```c
#include <stdio.h>
```

As operações de E/S mais comuns interagem com os chamados **fluxos padrão (standard streams)**, que são canais de comunicação abertos automaticamente para todo programa:

- **`stdin` (Standard Input):** Fluxo de entrada padrão. Na maioria dos ambientes interativos, está associado ao **teclado**.
- **`stdout` (Standard Output):** Fluxo de saída padrão. Geralmente associado à **tela (console)**.
- **`stderr` (Standard Error):** Fluxo de saída de erro padrão. Também associado à tela, é usado especificamente para exibir mensagens de erro. A separação de `stdout` e `stderr` permite redirecionar a saída normal de um programa sem perder as mensagens de erro (por exemplo, `meu_programa > saida.txt` redireciona `stdout`, mas `stderr` ainda aparece na tela).

### Saída Formatada com `printf()`

A função `printf()` (abreviação de "print formatted") é a principal ferramenta para exibir dados formatados no fluxo de saída padrão (`stdout`). Sua grande versatilidade permite imprimir texto simples, valores de variáveis de diversos tipos e combinações complexas, com controle preciso sobre o formato da saída.

A forma geral (protótipo) da função `printf()` é:

```c
int printf(const char *formato, ...);
```

Vamos dissecar essa assinatura:

- `int`: Indica que `printf` retorna um valor inteiro, que é o número de caracteres impressos com sucesso (ou um valor negativo se ocorrer um erro).
- `const char *formato`: O primeiro argumento é uma string (ponteiro para `char` constante) chamada de **string de controle de formato**.
- `...`: As reticências indicam que `printf` é uma **função variádica**, ou seja, ela pode aceitar um número variável de argumentos adicionais.

A string `formato` é o coração da função e pode conter:

1. **Caracteres Literais:** Texto que será impresso exatamente como aparece (ex: `"Olá, Mundo!"`).
2. **Sequências de Escape:** Caracteres especiais precedidos por uma barra invertida `\` (como `\n` para nova linha ou `\t` para tabulação).
3. **Especificadores de Formato:** Sequências que começam com `%` e atuam como "espaços reservados". Cada `%` indica que um dos argumentos adicionais da função deve ser formatado e inserido naquele ponto.

**Exemplo básico:**

```c
#include <stdio.h>

int main() {
    printf("Olá, Mundo!\n"); // 1. Apenas texto literal e escape \n
    
    int idade = 30;
    
    // 2. Um especificador de formato %d
    // A variável 'idade' é passada como argumento adicional
    // O valor de 'idade' (30) substituirá o %d
    printf("Eu tenho %d anos.\n", idade); 
    
    int num_caracteres = printf("C++\n");
    printf("A linha anterior continha %d caracteres.\n", num_caracteres); // Saída: 4 (C, +, +, \n)
    
    return 0;
}
```

#### Principais Especificadores de Formato para `printf()`

| **Especificador** | **Tipo de Dado Esperado**        | **Descrição**                                                                                                       |
| ----------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `%d` ou `%i`      | `int`                            | Inteiro decimal com sinal.                                                                                          |
| `%u`              | `unsigned int`                   | Inteiro decimal sem sinal.                                                                                          |
| `%f`              | `double`                         | Ponto flutuante decimal (notação normal). _Nota: `float` é "promovido" para `double` ao ser passado para `printf`._ |
| `%e` ou `%E`      | `double`                         | Ponto flutuante decimal (notação científica, ex: 1.23e+02).                                                         |
| `%g` ou `%G`      | `double`                         | Usa `%f` ou `%e` (o que for mais curto).                                                                            |
| `%c`              | `int` (interpretado como `char`) | Caractere único.                                                                                                    |
| `%s`              | `char *` (ponteiro para `char`)  | Sequência de caracteres (string) até o caractere nulo `\0`.                                                         |
| `%p`              | `void *`                         | Endereço de ponteiro (formato hexadecimal dependente da implementação).                                             |
| `%o`              | `unsigned int`                   | Inteiro octal (base 8) sem sinal.                                                                                   |
| `%x` ou `%X`      | `unsigned int`                   | Inteiro hexadecimal (base 16) sem sinal (letras minúsculas/maiúsculas).                                             |
| `%%`              | Nenhum                           | Imprime o próprio caractere `%`.                                                                                    |

#### Modificadores de Formato (Flags, Largura, Precisão)

O especificador de formato pode ser mais complexo, seguindo a estrutura: `%[flags][width][.precision][length]specifier`

- **Flags:** `-` (alinhar à esquerda), `+` (forçar exibição do sinal `+` ou `-`), `0` (preencher com zeros à esquerda).
- **Width (Largura):** Número mínimo de caracteres a serem impressos. Se o valor for menor, espaços são adicionados (à esquerda por padrão, ou à direita se a flag `-` for usada).
- **`.precision` (Precisão):** Para `%f`, define o número de casas decimais. Para `%s`, define o número máximo de caracteres a serem impressos.

**Exemplo de formatação avançada:**

```c
#include <stdio.h>

int main() {
    char produto[] = "Livro C++";
    int quantidade = 5;
    float preco = 49.99f;
    int id = 102;

    // Saída simples
    printf("Produto: %s, Preço: %f\n", produto, preco);
    // Saída: Produto: Livro C++, Preço: 49.990002 (imprecisão de float!)
    
    printf("\n--- Tabela Formatada ---\n");
    // %-15s: Alinha 'produto' à esquerda (-) em um campo de 15 caracteres.
    // %04d: Imprime 'id' em um campo de 4 dígitos, preenchendo com zeros (0) à esquerda.
    // %10.2f: Imprime 'preco' em um campo de 10 caracteres, com 2 casas decimais (.2).
    printf("Item: %-15s | ID: %04d | Preço: R$%10.2f\n", produto, id, preco);
    printf("Item: %-15s | ID: %04d | Preço: R$%10.2f\n", "Caderno", 3, 12.50);
    
    return 0;
}
```

**Saída:**

```
Produto: Livro C++, Preço: 49.990002

--- Tabela Formatada ---
Item: Livro C++       | ID: 0102 | Preço: R$     49.99
Item: Caderno         | ID: 0003 | Preço: R$     12.50
```

#### Sequências de Escape Comuns

| **Sequência** | **Significado**                                                                       |
| ------------- | ------------------------------------------------------------------------------------- |
| `\n`          | **Nova Linha (Newline):** Move o cursor para o início da próxima linha.               |
| `\t`          | **Tabulação Horizontal:** Move o cursor para a próxima "parada de tabulação".         |
| `\r`          | **Retorno de Carro (Carriage Return):** Move o cursor para o início da linha _atual_. |
| `\\`          | **Barra Invertida:** Imprime o caractere `\`.                                         |
| `\"`          | **Aspas Duplas:** Imprime o caractere `"`.                                            |
| `\'`          | **Aspas Simples:** Imprime o caractere `'`.                                           |
| `\b`          | **Retrocesso (Backspace):** Move o cursor uma posição para trás.                      |
| `\a`          | **Alerta (Bell):** Toca um som de alerta do sistema (se disponível).                  |

### Entrada Formatada com `scanf()`

A função `scanf()` (abreviação de "scan formatted") é a contraparte de `printf`. Ela é usada para ler dados _formatados_ do fluxo de entrada padrão (`stdin`), geralmente o teclado, e armazená-los em variáveis.

A forma geral da função `scanf()` é:

```c
int scanf(const char *formato, ...);
```

- `int`: Retorna o número de itens que foram lidos e atribuídos com sucesso.
- `const char *formato`: String de controle que contém **especificadores de formato** (similares aos de `printf`) indicando o tipo de dado esperado da entrada.
- `...`: Os argumentos subsequentes são **ponteiros para as variáveis** onde os dados lidos serão armazenados.

#### Conceito de Ponteiro (`&`) em `scanf()`

Este é o ponto que mais causa confusão em iniciantes. C passa argumentos para funções **por valor**. Se você passar uma variável `int idade;` para uma função, a função recebe uma _cópia_ do valor de `idade`. Modificar essa cópia não altera a `idade` original.

Para que `scanf` possa _modificar_ a sua variável `idade`, você não pode passar o valor dela; você deve passar o **endereço de memória** dela. A função `scanf` então "vai até" esse endereço e escreve o valor lido lá.

O operador `&` (operador "endereço de") é usado para obter o endereço de memória de uma variável.

**Exemplo (Jeito Certo e o Jeito Errado):**

```c
#include <stdio.h>

int main() {
    int idade_certa;
    int idade_errada = 0; // Inicializada com 0
    
    printf("Digite sua idade (corretamente): ");
    
    // CORRETO: Passamos o endereço de 'idade_certa'
    // scanf vai escrever o valor lido na localização de memória &idade_certa
    scanf("%d", &idade_certa); 
    
    printf("Idade lida corretamente: %d\n", idade_certa);

    printf("Digite sua idade (erradamente): ");
    
    // ERRADO (E MUITO PERIGOSO!):
    // scanf("%d", idade_errada); 
    
    // O que acontece na linha acima:
    // 1. 'idade_errada' vale 0.
    // 2. Você está dizendo ao scanf: "Escreva o inteiro que você leu 
    //    no endereço de memória 0".
    // 3. O endereço 0 é (quase sempre) protegido pelo sistema operacional.
    // 4. O programa tenta escrever em memória protegida e o sistema 
    //    o encerra. Isso causa um "Segmentation Fault" (Falha de Segmentação).
    
    // (A linha perigosa foi comentada para o programa poder rodar)
    
    // Exceção: O nome de um array (string) JÁ é um ponteiro/endereço,
    // por isso não usamos '&' com %s.
    char nome[50];
    printf("Digite seu primeiro nome: ");
    scanf("%s", nome); // 'nome' já é o endereço do início do array. NÃO USE &nome.
    
    printf("Olá, %s!\n", nome);

    return 0;
}
```

#### Principais Especificadores de Formato para `scanf()`

| **Especificador** | **Tipo de Argumento Esperado** | **Descrição**                                                              |
| ----------------- | ------------------------------ | -------------------------------------------------------------------------- |
| `%d`              | `int *`                        | Lê um inteiro decimal com sinal.                                           |
| `%ld`             | `long int *`                   | Lê um inteiro longo.                                                       |
| `%u`              | `unsigned int *`               | Lê um inteiro decimal sem sinal.                                           |
| `%f`              | `float *`                      | Lê um número de ponto flutuante.                                           |
| `%lf`             | `double *`                     | Lê um número de ponto flutuante de precisão dupla (o `l` é crucial).       |
| `%c`              | `char *`                       | Lê um _único_ caractere (incluindo espaços em branco).                     |
| `%s`              | `char *` (array)               | Lê uma sequência de caracteres (string) até encontrar um espaço em branco. |

#### Lidando com o Buffer de Entrada

O `scanf` pode ser frustrante por causa de como `stdin` lida com o buffer.

**Problema 1: O `\n` pendente com `%c`**

```c
#include <stdio.h>

int main() {
    int idade;
    char inicial;
    
    printf("Digite sua idade: ");
    scanf("%d", &idade); // Você digita: 25[Enter]
    
    // O '25' é lido e armazenado em 'idade'.
    // O caractere '\n' (Enter) FICA no buffer de entrada.
    
    printf("Digite sua inicial: ");
    scanf("%c", &inicial); // %c lê o *próximo caractere* no buffer, que é o '\n'.
    
    printf("Idade: %d. Inicial: '%c' (com valor ASCII %d)\n", idade, inicial, inicial);
    // Saída: Idade: 25. Inicial: '
    // ' (com valor ASCII 10)
    
    // A SOLUÇÃO: Usar um espaço antes do %c.
    // O espaço instrui scanf a "ignorar todos os caracteres 
    // de espaço em branco (incluindo \n) antes de ler o char".
    printf("Digite sua inicial (corretamente): ");
    scanf(" %c", &inicial); // O ESPAÇO AQUI É MÁGICO
    
    printf("Idade: %d. Inicial: '%c'\n", idade, inicial);
    
    return 0;
}
```

**Problema 2: `%s` e Buffer Overflow**

O `scanf("%s", ...)` é quase tão perigoso quanto `gets()`. Ele para no espaço em branco, mas não tem limite de tamanho.

```c
#include <stdio.h>

int main() {
    char nome[10]; // Buffer pequeno
    
    printf("Digite um nome longo (ex: Christophorus): ");
    scanf("%s", nome); // Se digitar "Christophorus" (13 chars + \0 = 14)
    
    // O scanf escreverá 14 bytes em um espaço de 10.
    // Ele CORROMPERÁ 4 bytes de memória adjacente (o que
    // pode ser outra variável, ou dados internos da pilha).
    // Isso é um BUFFER OVERFLOW.
    
    printf("Nome: %s\n", nome); // Pode funcionar, ou pode travar.
    
    // A SOLUÇÃO: Especificar uma largura máxima (tamanho do buffer - 1)
    char nome_seguro[10];
    printf("Digite o mesmo nome longo (com segurança): ");
    scanf("%9s", nome_seguro); // Lê NO MÁXIMO 9 caracteres e adiciona o \0.
    
    printf("Nome seguro: %s\n", nome_seguro); // Saída: Nome seguro: Christoph
    
    return 0;
}
```

#### Verificando o Retorno de `scanf()`

O `scanf` retorna o número de itens lidos com sucesso. Programas robustos _sempre_ verificam esse retorno.

**Pseudocódigo para entrada robusta:**

```
ENQUANTO (verdadeiro) FAÇA
    Imprima "Digite um número: "
    resultado = scanf("%d", &numero)
    
    SE (resultado == 1) ENTÃO
        // Sucesso, leu 1 item
        QUEBRE (saia do loop)
    SENÃO
        // Falha (ex: digitou "abc")
        Imprima "Entrada inválida. Tente novamente.\n"
        // Limpa o buffer de entrada para a próxima tentativa
        LIMPAR_BUFFER_DE_ENTRADA()
FIM-ENQUANTO
```

**Exemplo em C:**

```c
#include <stdio.h>

int main() {
    int idade;
    int resultado_scan;

    while (1) { // Loop infinito
        printf("Por favor, digite sua idade (um número): ");
        resultado_scan = scanf("%d", &idade);

        if (resultado_scan == 1) {
            // Sucesso! scanf leu 1 item (%d).
            break; // Sai do loop
        } else {
            // Falha! resultado_scan não é 1 (provavelmente 0)
            printf("Erro: Você não digitou um número.\n");
            
            // Limpa o buffer de entrada (consome caracteres até o \n)
            // Se não limpar, o "abc" digitado ficará no buffer
            // e o scanf falhará em um loop infinito.
            while (getchar() != '\n'); 
        }
    }
    
    printf("Obrigado. Sua idade é %d.\n", idade);
    return 0;
}
```

### E/S de Caracteres: `getchar()` e `putchar()`

Para entrada e saída de caracteres _individuais_, C oferece `getchar()` e `putchar()`. Elas são mais simples que `scanf` e `printf` e, muitas vezes, mais eficientes para processamento de texto caractere a caractere.

- **`int getchar(void);`**: Lê o próximo caractere do `stdin`.
- **`int putchar(int caractere);`**: Escreve o `caractere` no `stdout`.

Por que `int` e não `char`? Ambas as funções usam `int` por um motivo crucial: **EOF (End Of File)**.

EOF é um valor especial (geralmente `-1`) retornado por funções de leitura quando não há mais dados (ex: fim de um arquivo, ou o usuário pressionou Ctrl+D/Ctrl+Z no terminal). Esse valor `-1` está fora da faixa de um `unsigned char` (0-255). Ao retornar `int`, `getchar` pode retornar todos os 256 valores de `char` possíveis e o valor especial EOF sem ambiguidade.

**Exemplo com `getchar()` e `putchar()` (ecoando entrada):**

```c
#include <stdio.h>

int main() {
    int ch; // DEVE ser 'int' para comparar com EOF
    
    printf("Digite um texto (Ctrl+D ou Ctrl+Z para parar):\n");

    // Loop: lê um caractere, armazena em 'ch'.
    // A própria atribuição (ch = getchar()) é uma expressão,
    // e seu valor é o que foi lido.
    // O loop continua enquanto o valor lido não for EOF.
    while ((ch = getchar()) != EOF) {
        // Ecoa o caractere de volta para a tela
        putchar(ch);
    }
    
    printf("\nFim do programa.\n");
    return 0;
}
```

### E/S de Strings: `fgets()` e `puts()`

Como `scanf("%s", ...)` é perigoso (buffer overflow) e inconveniente (para em espaços), e `gets()` é obsoleto e mortalmente perigoso, a função ideal para ler _linhas_ de texto em C é `fgets()`.

- **`char *fgets(char *str, int tamanho, FILE *fluxo);`**:
    - Lê uma linha do `fluxo` (ex: `stdin`).
    - Armazena em `str`.
    - Lê no máximo `tamanho - 1` caracteres (para sempre sobrar espaço para o `\0`).
    - Para de ler se encontrar `\n` ou `EOF`.
    - **Importante:** Ao contrário de `gets`, `fgets` _mantém_ o `\n` na string, se ele for lido.
    - Retorna `str` em sucesso, ou `NULL` em erro ou `EOF`.
- **`int puts(const char *str);`**:
    - Escreve a string `str` no `stdout`.
    - **Importante:** `puts` _adiciona automaticamente_ um `\n` no final da saída.

**Exemplo com `fgets()` (seguro) e `puts()`:**

```c
#include <stdio.h>
#include <string.h> // Para strcspn

#define TAMANHO_BUFFER 50

int main() {
    char nome[TAMANHO_BUFFER];

    printf("Digite seu nome completo: ");
    
    // Lê uma linha inteira de forma segura
    if (fgets(nome, TAMANHO_BUFFER, stdin) != NULL) {
        
        // --- Processamento: Remover o '\n' do fgets ---
        // fgets armazena "Joao Silva\n\0"
        // Queremos "Joao Silva\0"
        
        // Método 1: Loop manual (como no draft)
        /*
        for (int i = 0; i < TAMANHO_BUFFER; i++) {
            if (nome[i] == '\n') {
                nome[i] = '\0'; // Substitui '\n' por '\0'
                break;
            }
            if (nome[i] == '\0') { // String terminou antes do \n (buffer cheio)
                break;
            }
        }
        */
        
        // Método 2: Idiomático (preferido)
        // strcspn busca o primeiro caractere de "\n" em "nome"
        // e retorna seu índice. Substituímos esse caractere por '\0'.
        nome[strcspn(nome, "\n")] = '\0';

        // --- Saída ---
        printf("Olá, ");
        puts(nome); // puts adiciona seu próprio \n
    } else {
        printf("Erro ao ler o nome.\n");
    }

    return 0;
}
```

## Entrada e Saída em C++

C++ introduz um sistema de E/S completamente diferente, baseado em **streams (fluxos)** e orientado a objetos. Ele é mais seguro, extensível e integrado à linguagem. A biblioteca principal é a `<iostream>`.

Este sistema trata a E/S como um fluxo de bytes.

- **`std::cin`**: Objeto de _input stream_ (fluxo de entrada) associado ao `stdin`.
- **`std::cout`**: Objeto de _output stream_ (fluxo de saída) associado ao `stdout`.
- **`std::cerr`**: Objeto de _output stream_ para erros (não bufferizado), associado ao `stderr`.
- **`std::clog`**: Objeto de _output stream_ para logs (bufferizado), associado ao `stderr`.

### Saída com `std::cout` e o Operador de Inserção `<<`

Em C++, a saída é feita "inserindo" dados no objeto `cout` usando o operador `<<` (operador de inserção de fluxo).

A beleza desse sistema é que `<<` é um operador _sobrecarregado_. Existem versões diferentes dele para `int`, `double`, `const char*`, `std::string`, etc. O compilador escolhe a versão correta automaticamente (segurança de tipo).

```cpp
#include <iostream>
#include <string> // Para std::string

int main() {
    // std::endl insere uma nova linha e "descarrega" (flushing) o buffer.
    std::cout << "Olá, C++!" << std::endl; 

    int idade = 25;
    double altura = 1.75;
    std::string nome = "Carlos";

    // Múltiplas inserções podem ser encadeadas
    std::cout << "Nome: " << nome 
              << ", Idade: " << idade 
              << ", Altura: " << altura << "m" 
              << std::endl;
              
    return 0;
}
```

#### `std::endl` vs. `'\n'`

- `std::cout << '\n';` Apenas insere o caractere de nova linha no buffer de saída.
- `std::cout << std::endl;` Insere `\n` _e_ força a descarga (flush) do buffer, enviando-o imediatamente para o console.

Forçar o flush a cada linha tem um custo de desempenho.

**Regra**: Prefira `'\n'` para a maioria das saídas. Use `std::endl` apenas quando for crítico que a saída apareça imediatamente (ex: em um prompt para o usuário ou em logs de erro).

#### Formatação com `<iomanip>` (Manipuladores)

Para formatação (casas decimais, largura), C++ usa **manipuladores** da biblioteca `<iomanip>`.

```cpp
#include <iostream>
#include <iomanip> // Para std::fixed e std::setprecision
#include <string>

int main() {
    double pi = 3.1415926535;
    std::string item = "Leite";
    double preco = 4.5;
    
    // std::fixed: Usa notação de ponto fixo (ex: 4.50 e não 4.5e0)
    // std::setprecision(n): Define n casas decimais
    std::cout << "PI (2 casas): " << std::fixed << std::setprecision(2) << pi << std::endl;
    // Saída: PI (2 casas): 3.14
    
    // std::setw(n): Define a largura mínima do *próximo* campo
    // std::left / std::right: Define o alinhamento (direita é o padrão)
    std::cout << std::setw(10) << std::left << item 
              << " | R$" 
              << std::setw(6) << std::right << std::fixed << std::setprecision(2) << preco 
              << std::endl;
    // Saída: Leite      | R$  4.50
    return 0;
}
```

### Entrada com `std::cin` e o Operador de Extração `>>`

Para ler dados, C++ usa `std::cin` com o operador `>>` (operador de extração de fluxo).

O `std::cin` é inteligente:

1. Ignora espaços em branco iniciais (espaços, `\t`, `\n`).
2. Lê caracteres que correspondem ao tipo da variável.
3. Para de ler quando encontra um espaço em branco ou um caractere incompatível.

```cpp
#include <iostream>
#include <iomanip>
#include <string>

int main() {
    int idade;
    double salario;
    std::string primeiroNome;

    std::cout << "Digite sua idade, primeiro nome e salário (separados por espaço): ";
    // Ex: usuário digita: 30 Joao 5000.75[Enter]
    
    std::cin >> idade >> primeiroNome >> salario; // Encadeamento

    std::cout << "\n--- Dados Lidos ---\n";
    std::cout << "Nome: " << primeiroNome << std::endl;  // Saída: Joao
    std::cout << "Idade: " << idade << " anos" << std::endl; // Saída: 30
    std::cout << "Salário: " << std::fixed << std::setprecision(2) << salario << std::endl;
    // Saída: 5000.75
    
    return 0;
}
```

#### Fail State e Entrada Robusta em C++

O `scanf` retorna um `int`. `std::cin` é um objeto e retorna uma _referência a si mesmo_. Isso permite que ele seja usado em um contexto booleano (como um `if` ou `while`) para testar se a _última operação de leitura foi bem-sucedida_.

Se o usuário digitar "abc" quando `cin >> idade` (um `int`) for chamado, `cin` entra em um **"fail state" (estado de falha)**.

**Pseudocódigo para entrada robusta em C++:**

```
ENQUANTO (verdadeiro) FAÇA
    Imprima "Digite um número: "
    SE (cin >> numero) ENTÃO
        // Sucesso
        QUEBRE (saia do loop)
    SENÃO
        // Falha (ex: digitou "abc")
        Imprima "Entrada inválida. Tente novamente.\n"
        cin.clear()     // Limpa a flag de erro (o "fail state")
        cin.ignore(...) // Limpa o buffer de entrada (o "abc\n")
FIM-ENQUANTO
```

**Exemplo em C++:**

```cpp
#include <iostream>
#include <limits> // Para std::numeric_limits

int main() {
    int idade;

    while (true) {
        std::cout << "Por favor, digite sua idade (um número): ";
        
        // Se a extração (>>) funcionar, o 'if' será verdadeiro
        if (std::cin >> idade) {
            break; // Sucesso, sai do loop
        } else {
            std::cout << "Erro: Você não digitou um número.\n";
            
            // 1. Limpa a flag de erro do 'cin'
            std::cin.clear();
            
            // 2. Ignora o restante da linha inválida no buffer
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }
    }
    
    std::cout << "Obrigado. Sua idade é " << idade << ".\n";
    return 0;
}
```

#### Lendo Linhas Completas com `std::getline()`

Como `cin >> string` para em espaços, C++ fornece `std::getline()` para ler uma linha inteira.

```cpp
#include <iostream>
#include <string>

int main() {
    std::string nomeCompleto;

    std::cout << "Digite seu nome completo: ";
    // Lê de 'std::cin' e armazena em 'nomeCompleto', até encontrar '\n'
    std::getline(std::cin, nomeCompleto);

    std::cout << "Olá, " << nomeCompleto << "!" << std::endl;
    return 0;
}
```

**Problema Clássico (A "Pegadinha" do `cin` e `getline`):**

```cpp
#include <iostream>
#include <string>
#include <limits> // Necessário para ignore

int main() {
    std::string nomeCompleto;
    int idade;

    std::cout << "Digite sua idade: ";
    std::cin >> idade; // Usuário digita: 25[Enter]
    
    // 'cin' lê o '25'. O '\n' (Enter) FICA no buffer.
    
    std::cout << "Digite seu nome completo: ";
    
    // getline lê o buffer, encontra o '\n' imediatamente,
    // e para, armazenando uma string vazia.
    std::getline(std::cin, nomeCompleto); 
    
    std::cout << "Nome: '" << nomeCompleto << "'" << std::endl; // Saída: Nome: ''
    std::cout << "Idade: " << idade << " anos" << std::endl;

    // --- A SOLUÇÃO ---
    // Precisamos consumir o '\n' deixado pelo 'cin >> idade'
    // antes de chamar getline.
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); 

    std::cout << "Digite seu nome completo (corretamente): ";
    std::getline(std::cin, nomeCompleto);

    std::cout << "Nome (correto): '" << nomeCompleto << "'" << std::endl;

    return 0;
}
```

## Considerações Finais

Neste capítulo, exploramos os mecanismos fundamentais para a interação de programas C e C++ com o usuário: a entrada e saída de dados. Em C, aprendemos sobre as funções da biblioteca `stdio.h`, com destaque para `printf()` e `scanf()`. Vimos a importância da formatação em `printf` e os perigos inerentes a `scanf` (a necessidade do `&`, o risco de buffer overflow com `%s` e os problemas com o buffer de `\n`). Também exploramos `getchar()`/`putchar()` e a alternativa segura `fgets()`/`puts()` para E/S de strings.

Em seguida, introduzimos a abordagem orientada a objetos do C++ para E/S, utilizando a biblioteca `<iostream>`. Vimos como os operadores `<<` (inserção) e `>>` (extração) com `std::cout` e `std::cin` oferecem uma interface mais segura e extensível. Detalhamos as melhores práticas, como a diferença entre `\n` e `std::endl`, a formatação com `<iomanip>`, e as técnicas robustas para lidar com erros de entrada (estados de falha) e a leitura de linhas completas com `std::getline()`, incluindo a clássica "pegadinha" do `cin.ignore()`.

A capacidade de receber dados do usuário e apresentar resultados de forma clara e segura é essencial. Com as ferramentas apresentadas, estamos equipados para construir programas interativos e comunicativos. Nos próximos capítulos, continuaremos a construir sobre esses fundamentos, explorando as **Estruturas de Controle** que permitirão aos nossos programas tomar decisões e repetir ações com base, muitas vezes, nos dados recebidos do usuário.
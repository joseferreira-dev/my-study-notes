# Capítulo 5 – Caracteres e Strings

Nos capítulos anteriores, estabelecemos uma base sólida sobre os tipos de dados numéricos, variáveis e os operadores que nos permitem realizar cálculos e tomar decisões. Agora, voltaremos nossa atenção para um tipo de dado igualmente fundamental na programação: o texto. A capacidade de armazenar, manipular e exibir sequências de caracteres é essencial para a grande maioria das aplicações, desde simples mensagens ao usuário, passando pela leitura de arquivos de configuração, até o processamento complexo de documentos e dados textuais na web.

Neste capítulo, exploraremos como as linguagens C e C++ lidam com caracteres individuais e com sequências de caracteres, comumente conhecidas como **strings**. Iniciaremos nossa jornada revisando o tipo `char` em C, a unidade básica para representar caracteres, e aprofundando nas funções padrão para sua entrada e saída. Em seguida, mergulharemos no conceito de strings em C: um paradigma de baixo nível que define strings como **arrays de caracteres terminados por um caractere nulo (`\0`)**. Investigaremos como elas são declaradas, inicializadas e manipuladas de forma perigosa, porém eficiente, utilizando as funções da biblioteca `<string.h>`.

Posteriormente, faremos a transição para o C++, onde a manipulação de strings é significativamente simplificada, modernizada e protegida pela classe `std::string`, parte da biblioteca padrão `<string>`. Discutiremos suas vantagens, como o gerenciamento automático de memória, a vasta gama de funções membro e a segurança contra erros clássicos como _buffer overflows_. Ao final deste capítulo, você terá uma compreensão clara das duas abordagens para manipulação de texto, capacitando-o a escolher a ferramenta mais adequada para cada contexto e a escrever código mais seguro e eficiente.

## Caracteres em C

Conforme vimos, o tipo de dado `char` em C é fundamentalmente projetado para armazenar um único caractere. Na vasta maioria das arquiteturas modernas, um `char` ocupa **1 byte (8 bits)** de memória.

É crucial entender que um `char` é, em essência, um **tipo inteiro muito pequeno**. Ele armazena um valor numérico que é interpretado como um caractere de acordo com um conjunto de caracteres, sendo o **ASCII (American Standard Code for Information Interchange)** o mais comum. O ASCII mapeia números de 0 a 127 para símbolos, letras, dígitos e caracteres de controle.

- `'A'` é, na verdade, o inteiro `65`.
- `'B'` é o inteiro `66`.
- `'a'` é o inteiro `97`.
- `'0'` é o inteiro `48`.

**Declaração e Inicialização:**

```c
#include <stdio.h>

int main() {
    // Inicialização com um literal de caractere (entre aspas simples)
    char letra_a = 'A';
    char simbolo_arroba = '@';
    char digito_um = '1';
    
    // Inicialização com o valor ASCII (um inteiro)
    char char_ascii = 66; // Corresponde a 'B' na tabela ASCII

    // %c imprime como caractere
    printf("Letra: %c\n", letra_a);
    printf("Símbolo: %c\n", simbolo_arroba);
    printf("Dígito como caractere: %c\n", digito_um);
    printf("Caractere ASCII 66: %c\n", char_ascii);
    
    // %d imprime o valor numérico (inteiro)
    printf("Valor numérico de 'A': %d\n", letra_a);   // Saída: 65
    printf("Valor numérico de '1': %d\n", digito_um); // Saída: 49
    
    // Podemos fazer aritmética
    char proxima_letra = letra_a + 1; // 65 + 1 = 66
    printf("A próxima letra depois de 'A' é: %c\n", proxima_letra); // Saída: B

    return 0;
}
```

### Entrada e Saída de Caracteres

Como já explorado, as funções `getchar()` e `putchar()` são otimizadas para E/S de caracteres individuais.

- **`int putchar(int c);`**: Escreve o caractere `c` na tela.
- **`int getchar(void);`**: Lê um único caractere do teclado e o retorna.

Ambas usam `int` para acomodar o valor especial `EOF` (End Of File), que sinaliza o fim da entrada.

```c
#include <stdio.h>

int main() {
    char c;
    printf("Digite um caractere: ");
    
    // c = getchar(); // Forma simples
    
    // Forma robusta que lida com o buffer de entrada (visto em Ch. 4)
    // Se um scanf anterior deixou um '\n' no buffer, o " %c" o ignora.
    scanf(" %c", &c); 

    printf("Você digitou: ");
    putchar(c);
    putchar('\n');

    return 0;
}
```

## Strings em C

A linguagem C não possui um tipo de dado "string" embutido. Em vez disso, C adota uma convenção de baixo nível que é fundamental para entender o funcionamento da linguagem: uma **string** é um **array (vetor) de caracteres (`char`)** que deve, obrigatoriamente, terminar com um caractere especial chamado **caractere nulo (`\0`)**.

Este caractere nulo, que tem o valor ASCII `0`, atua como um **sentinela** (um marcador de fim). Todas as funções da biblioteca padrão de C (`printf`, `strlen`, `strcpy`, etc.) que operam sobre strings dependem da presença desse `\0` para saber onde a string termina.

### Declaração e Inicialização de Strings em C

Existem algumas maneiras de declarar e inicializar strings em C, cada uma com suas implicações:

**1. Como um array inicializado com um literal de string (A forma mais comum)**

O compilador C trata qualquer texto entre aspas duplas (" ") como um literal de string.

```c
// O compilador aloca espaço automaticamente
char saudacao[] = "Ola"; 
```

**Análise de Memória:** O compilador reserva 4 bytes para `saudacao`:

```
{'O', 'l', 'a', '\0'}
```

O `\0` é adicionado automaticamente pelo compilador. O tamanho do array (`sizeof(saudacao)`) é `4`.

```c
// Alocando um tamanho fixo (maior)
char outra_string[20] = "Mundo";
```

**Análise de Memória:** O compilador reserva 20 bytes. Os 6 primeiros são preenchidos com `{'M', 'u', 'n', 'd', 'o', '\0'}`. Os 14 bytes restantes são **zero-inicializados** (preenchidos com `\0`).

**2. Como um array inicializado com uma lista de caracteres**

Esta forma é manual e raramente usada, mas ilustra o conceito.

```c
char palavra[] = {'t', 'e', 's', 't', 'e', '\0'};
```

**Análise de Memória:** `{'t', 'e', 's', 't', 'e', '\0'}`. Tamanho 6.

Se você esquecer o `\0` (`char palavra[] = {'t', 'e', 's', 't', 'e'};`), palavra não será uma string válida! Funções como `printf("%s", palavra);` ou `strlen(palavra);` não encontrarão o fim, continuarão lendo a memória sequencialmente (lixo de memória) até encontrarem um `\0` aleatório ou o programa travar (Falha de Segmentação).

**3. Como um ponteiro para um literal de string (String Constante)**

Esta forma cria um ponteiro que aponta para um literal de string.

```c
const char *ptr_string = "Exemplo";
```

**Análise de Memória:** O literal `"Exemplo"` (com seu `\0`) é armazenado em uma área de memória **somente leitura** (read-only) do programa. `ptr_string` é uma variável (ponteiro) que armazena o endereço dessa memória.

- **IMPORTANTE:** Tentar modificar essa string (ex: `ptr_string[0] = 'e';`) causa **Comportamento Indefinido** e, na maioria dos sistemas modernos, um _crash_ imediato, pois se está tentando escrever em memória protegida. Por isso, a boa prática é sempre usar `const char *` para ponteiros de literais.

Para strings que precisam ser modificadas (como ao ler do usuário), a abordagem com **array** (Método 1 ou 2) é a correta.

### Entrada e Saída de Strings em C

**Saída (Impressão):**

- **`printf("%s", ...)`**: Imprime a sequência de caracteres a partir do endereço fornecido, parando _apenas_ quando encontra o `\0`.
- **`puts(const char *str)`**: Imprime a string (parando no `\0`) e, em seguida, **adiciona automaticamente** um caractere de nova linha (`\n`).

```c
#include <stdio.h>

int main() {
    char cidade[] = "Recife";
    
    printf("Eu moro em ");
    printf("%s", cidade); // Imprime "Recife"
    printf(".\n");        // Imprime ".\n"
    
    puts("---");
    
    puts("Usando puts:");
    puts(cidade); // Imprime "Recife" E uma nova linha
    puts("Pernambuco."); // Imprime "Pernambuco." E uma nova linha
    return 0;
}
```

**Entrada (Leitura):**

- **`scanf("%s", nome_array)` (PERIGOSO):**
    - Lê caracteres do `stdin` até encontrar um espaço em branco (espaço, tabulação, `\n`).
    - Adiciona automaticamente o `\0` no final.
    - **PERIGO:** `scanf` não sabe o tamanho do array `nome_array`. Se o usuário digitar "Christophorus" (14 bytes) em um `char nome_array[10];`, `scanf` escreverá 14 bytes na memória, **estourando o buffer** e corrompendo 4 bytes de memória adjacente. Isso é uma vulnerabilidade de segurança grave.
- **`fgets(char *str, int tamanho, FILE *fluxo)` (SEGURO):**
    - É a função preferida para ler strings em C.
    - `str`: O array onde os dados serão armazenados.
    - `tamanho`: O **tamanho total** do array (ex: `sizeof(meu_array)`).
    - `fluxo`: De onde ler (ex: `stdin` para o teclado).
    - **Comportamento:** Lê no máximo `tamanho - 1` caracteres (para sempre sobrar espaço para o `\0`). Para de ler se encontrar `\n` ou `EOF` (fim de arquivo).
    - **Armadilha:** `fgets` _mantém_ o caractere `\n` (Enter) na string, se ele for lido.

```c
#include <stdio.h>
#include <string.h> // Para strcspn

#define MAX_LEN 30

int main() {
    char linha[MAX_LEN];
    printf("Digite uma frase (max %d chars): ", MAX_LEN - 1);

    if (fgets(linha, MAX_LEN, stdin) != NULL) {
        
        // 'linha' pode conter "Ola Mundo\n\0"
        // Precisamos remover o \n
        
        // strcspn(string, "chars_para_buscar") retorna o índice
        // do primeiro caractere em "chars_para_buscar" encontrado na string.
        // Aqui, ele encontra o índice do '\n'.
        linha[strcspn(linha, "\n")] = '\0'; // Trocamos '\n' por '\0'
        
        printf("Linha digitada: '%s'\n", linha);
    }
    return 0;
}
```

### Funções da Biblioteca `<string.h>`

Manipular strings manualmente (em loops) é tedioso e propenso a erros. C fornece uma biblioteca padrão, `<string.h>`, repleta de funções para isso. É essencial incluir este cabeçalho:

```c
#include <string.h>
```

A seguir, as funções mais essenciais (e suas armadilhas):

**Função `size_t strlen(const char *str);`**

- Retorna o **comprimento** da string `str` (o número de caracteres _antes_ do `\0`).
- `size_t` é um tipo inteiro sem sinal. Use `%zu` para imprimi-lo.
- **Como funciona:** `strlen` _não_ sabe o tamanho; ele conta caractere por caractere, começando de `str`, até encontrar o `\0`. Isso significa que é uma operação O(n) (lenta para strings muito longas).
    
    ```c
    char texto[] = "Exemplo";   // {'E','x','e','m','p','l','o','\0'}
    size_t len = strlen(texto); // len será 7
    printf("Comprimento: %zu\n", len);
    printf("Tamanho em memória: %zu\n", sizeof(texto)); // Saída: 8
    ```

**Função `char *strcpy(char *destino, const char *origem);` (PERIGOSO)**

- **Copia** a string `origem` (incluindo o `\0`) para o endereço `destino`.
- **PERIGO:** `strcpy` não sabe o tamanho de `destino`. Se `origem` for maior que `destino`, ocorrerá um **buffer overflow**.
    
    ```c
    char origem_str[] = "String muito, muito longa";
    char destino_str[10];
    // strcpy(destino_str, origem_str); // CRASH! Escreve ~25 bytes em um espaço de 10.
    ```

**Função `char *strncpy(char *destino, const char *origem, size_t n);` (SEGURO, MAS COMPLEXO)**

- Copia _exatamente_ `n` caracteres de `origem` para `destino`.
- **ARMADILHA FATAL:** Se o comprimento de `origem` for `n` ou maior, `strncpy` **NÃO ADICIONA O `\0`**!
- Se `origem` for _menor_ que `n`, ele copia a string com `\0` e preenche o resto de `destino` (até `n` caracteres) com `\0`.
    
    ```c
    char src[] = "Texto longo";
    char dest[5];
    
    // Copia 4 caracteres ("Text") de 'src' para 'dest'.
    strncpy(dest, src, 4); 
    
    // 'dest' agora contém: {'T', 'e', 'x', 't'}
    // NÃO HÁ '\0' !
    
    // printf("%s", dest); // Imprimiria "Text" e lixo de memória até um \0 aleatório.
    
    // CORREÇÃO MANUAL OBRIGATÓRIA:
    dest[4] = '\0'; // Garante a terminação nula! (Índice 4 é o 5º byte)
    printf("Destino (strncpy): %s\n", dest); // Saída: Text
    ```

**Função `char *strcat(char *destino, const char *origem);` (PERIGOSO)**

- Anexa (concatena) a string `origem` ao final da string `destino`. O primeiro caractere de `origem` sobrescreve o `\0` de `destino`.
- `destino` deve ter espaço suficiente para conter a string original + a nova string + o `\0` final.
- **PERIGO:** Risco de buffer overflow se `destino` não for grande o suficiente.

    ```c
    char str1[50] = "Bom "; // Tamanho 50, usado 4 + 1
    char str2[] = "dia!";  // Tamanho 4 + 1
    strcat(str1, str2); // str1 agora é "Bom dia!" (usado 11 + 1 bytes)
    printf("strcat: %s\n", str1);
    ```

**Função `char *strncat(char *destino, const char *origem, size_t n);` (SEGURO)**

- Anexa _no máximo_ `n` caracteres de `origem` ao final de `destino`.
- **SEGURO:** `strncat` **sempre** adiciona um `\0` ao final, independentemente do que acontecer.
    
    ```c
    char str_a[20] = "Hello"; // Tamanho 20, usado 5 + 1
    char str_b[] = " World!!!";
    
    // Anexa no máximo 7 caracteres de str_b (" World!")
    strncat(str_a, str_b, 7); 
    
    // str_a agora é "Hello World!" + '\0'
    printf("strncat: %s\n", str_a);
    ```

**Função `int strcmp(const char *str1, const char *str2);`**

- Compara lexicograficamente (ordem de dicionário, baseada no ASCII) `str1` e `str2`.
- Retorna:
    - `0` se `str1` é **igual** a `str2`.
    - Um valor `< 0` se `str1` vem **antes** de `str2`.
    - Um valor `> 0` se `str1` vem **depois** de `str2`.
    
    ```c
    if (strcmp("abc", "abc") == 0) { printf("Iguais\n"); }
    if (strcmp("abc", "abd") < 0)  { printf("abc vem antes de abd\n"); }
    if (strcmp("xyz", "abc") > 0)  { printf("xyz vem depois de abc\n"); }
    ```

**Função `int strncmp(const char *str1, const char *str2, size_t n);`**

- Similar a `strcmp`, mas compara no máximo os primeiros `n` caracteres.

**Função `char *strchr(const char *str, int c);`**

- Procura a _primeira_ ocorrência do caractere `c` na string `str`.
- Retorna um ponteiro para o caractere encontrado, ou `NULL` se não encontrado.

**Função `char *strrchr(const char *str, int c);`**

- Procura a _última_ (reverse) ocorrência do caractere `c` na string `str`.

**Função `char *strstr(const char *haystack, const char *needle);`**

- Procura a _primeira_ ocorrência da substring `needle` (agulha) dentro da string `haystack` (palheiro).
- Retorna um ponteiro para o início da substring encontrada, ou `NULL`.

```c
#include <stdio.h>
#include <string.h>

int main() {
    char frase[] = "A raposa marrom pula sobre o cachorro marrom.";
    char *p_primeira = strchr(frase, 'm');
    char *p_ultima = strrchr(frase, 'm');
    char *p_sub = strstr(frase, "pula");
    
    if (p_primeira) printf("Primeiro 'm': %s\n", p_primeira); // marrom pula...
    if (p_ultima)   printf("Último 'm': %s\n", p_ultima);   // m.
    if (p_sub)      printf("Substring 'pula': %s\n", p_sub); // pula sobre...
    return 0;
}
```

## Strings em C++

O C++ melhora drasticamente a manipulação de strings ao introduzir a classe `std::string` como parte de sua biblioteca padrão. Para utilizá-la, é necessário incluir o cabeçalho `<string>`:

```cpp
#include <string>
```

A classe `std::string` é um contêiner que gerencia uma sequência de caracteres. Ela oferece uma série de vantagens sobre as strings estilo C:

- **Gerenciamento Automático de Memória:** O programador não precisa se preocupar com alocação, desalocação ou tamanho do buffer. A string cresce e diminui conforme necessário (usando alocação dinâmica no _heap_ internamente).
- **Segurança:** Operações como concatenação (`+`) e cópia (`=`) são seguras contra buffer overflows.
- **Riqueza de Funcionalidades:** Fornece um vasto conjunto de métodos (funções membro) para manipulação.
- **Performance:** Operações como `length()` são O(1) (tempo constante), pois `std::string` armazena seu próprio tamanho, ao contrário de `strlen` (O(n)).

**Declaração e Inicialização:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string s1 = "João";  // Inicialização por cópia
    std::string s2("Silva");  // Inicialização direta (construtor)
    std::string s3;           // Construtor padrão (string vazia)
    std::string s4(10, 'A');  // Inicializa com 10 'A's -> "AAAAAAAAAA"
    std::string s5 = s1;      // Cópia de outra std::string

    s3 = "Recife"; // Atribuição

    std::cout << "s1: " << s1 << std::endl;
    std::cout << "s2: " << s2 << std::endl;
    std::cout << "s3: " << s3 << std::endl;
    std::cout << "s4: " << s4 << std::endl;
    std::cout << "s5: " << s5 << std::endl;
    return 0;
}
```

### Concatenação de Strings em C++

A concatenação (junção) é intuitiva e segura.

**1. Usando o operador de adição (`+`):**

Este operador é sobrecarregado para `std::string`. Ele cria e retorna uma nova string.

```cpp
#include <iostream>
#include <string>

int main() {
    std::string firstName = "John";
    std::string lastName = "Doe";
    
    // Concatena duas std::string, criando uma nova
    std::string fullName = firstName + " " + lastName; 
    
    std::cout << fullName << std::endl; // Saída: John Doe
    
    // "Gotcha" (Armadilha): O operador '+' não é definido para dois literais C
    // std::string erro = "Hello" + "World"; // ERRO DE COMPILAÇÃO!
    
    // Solução: Pelo menos um dos operandos deve ser std::string
    std::string s1 = std::string("Hello") + "World"; // OK
    std::string s2 = "Hello";
    std::string s3 = s2 + "World"; // OK
    
    return 0;
}
```

**2. Usando o método `append()`:**

O método `append()` modifica a string existente, anexando o novo conteúdo ao seu final. Geralmente é mais eficiente se você estiver construindo uma string em etapas.

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello, ";
    str.append("world!"); // Modifica 'str'
    std::cout << str << std::endl; // Saída: Hello, world!
    
    std::string str_orig = "Começo";
    std::string str_fim = " e Fim";
    str_orig.append(str_fim, 0, 2); // Anexa 2 chars de str_fim, começando do índice 0 (" e")
    std::cout << str_orig << std::endl; // Saída: Começo e
    return 0;
}
```

### Números e Strings em C++

Em C++, não se pode concatenar diretamente um número a uma `std::string` usando `+`.

```cpp
std::string s = "Valor: " + 10; // ERRO.
```

**Solução (C++11): `std::to_string()`**

A biblioteca `<string>` (desde C++11) fornece `std::to_string()` para converter tipos numéricos em `std::string`.

```cpp
#include <iostream>
#include <string> // Para std::string e std::to_string

int main() {
    int x = 10;
    double pi = 3.14159;

    std::string msg1 = "O valor de x é: " + std::to_string(x);
    std::string msg2 = "PI é aprox.: " + std::to_string(pi);

    std::cout << msg1 << std::endl; // Saída: O valor de x é: 10
    std::cout << msg2 << std::endl; // Saída: PI é aprox.: 3.141590
    return 0;
}
```

**Solução Alternativa (Formatação): `std::stringstream`**

Para formatação mais complexa (como precisão de `double`), use `stringstream` (do cabeçalho `<sstream>`). Funciona como um `cout` na memória.

```cpp
#include <iostream>
#include <string>
#include <sstream> // Para std::stringstream
#include <iomanip> // Para std::setprecision

int main() {
    int idade = 25;
    double altura = 1.785;
    
    std::stringstream ss;
    // "Imprime" os dados no stream
    ss << "Usuário tem " << idade << " anos e " 
       << std::fixed << std::setprecision(2) << altura << "m de altura.";
    
    std::string info = ss.str(); // Extrai a string formatada
    
    std::cout << info << std::endl; 
    // Saída: Usuário tem 25 anos e 1.79m de altura.
    return 0;
}
```

### Comprimento da String em C++

O `std::string` armazena seu tamanho, tornando a obtenção do comprimento uma operação $O(1)$ (tempo constante), ao contrário de `strlen` ($O(n)$).

- **`length()` e `size()`**: Ambos os métodos são **idênticos** e retornam o número de caracteres (`size_t`).

```cpp
#include <iostream>
#include <string>

int main() {
    std::string texto = "Olá mundo!"; // 10 caracteres
    
    std::cout << "Comprimento: " << texto.length() << std::endl; // Saída: 10
    std::cout << "Tamanho: " << texto.size() << std::endl;       // Saída: 10
    return 0;
}
```

### Acessar Caracteres em Strings C++

Existem duas formas principais de acessar caracteres individuais (cujos índices começam em 0):

**1. Usando o operador de colchetes `[]`:**

- Rápido, como em um array C.
- **Não** faz verificação de limites (bounds checking).
- Acessar um índice fora da faixa (ex: `texto[100]`) é **Comportamento Indefinido**.
- Use para performance quando o índice é garantidamente válido (ex: em um loop `for (i=0; i < s.length(); ++i)`).

```cpp
std::string texto = "Olá mundo!";
char primeiro_char = texto[0];   // 'O'
texto[0] = 'A'; // Modifica a string
std::cout << texto << std::endl; // Saída: Alá mundo!
```

**2. Usando o método `at()`:**

- Um pouco mais lento, pois faz verificação de limites.
- **Seguro:** Se o índice estiver fora da faixa válida, `at()` lança uma exceção `std::out_of_range`.
- Use quando o índice for incerto (ex: vindo de uma entrada do usuário).

```cpp
#include <iostream>
#include <string>
#include <stdexcept> // Para std::out_of_range

int main() {
    std::string texto = "Exemplo";
    try {
        char ch1 = texto.at(0); // 'E'
        std::cout << "Char[0]: " << ch1 << std::endl;

        char ch_fora = texto.at(10); // Lançará std::out_of_range
        
    } catch (const std::out_of_range& e) {
        std::cerr << "Erro de acesso: Índice fora da faixa!" << std::endl;
        std::cerr << e.what() << std::endl;
    }
    return 0;
}
```

**Iterando sobre os caracteres:**

```cpp
#include <iostream>
#include <string>
#include <cctype> // Para toupper

int main() {
    std::string texto = "Teste";
    
    // 1. Laço 'for' baseado em índice (tradicional)
    for (size_t i = 0; i < texto.length(); ++i) {
        std::cout << texto[i] << " ";
    }
    std::cout << std::endl; // Saída: T e s t e 

    // 2. Laço 'for' baseado em intervalo (Range-based for) (C++11) - Leitura
    for (char c : texto) {
        std::cout << c << " ";
    }
    std::cout << std::endl; // Saída: T e s t e 

    // 3. Range-based for (C++11) - Modificação (usando referência '&')
    std::string s_mod = "minusculo";
    for (char &c : s_mod) {
        c = toupper(c); // Converte para maiúscula
    }
    std::cout << s_mod << std::endl; // Saída: MINUSCULO
    
    return 0;
}
```

### Interagindo entre `std::string` e C-Strings

É muito comum precisar interagir com APIs de C (que esperam `const char*`). `std::string` fornece métodos para isso:

- **`c_str()`**: Retorna um ponteiro `const char*` para a string terminada em nulo (estilo C).
- **Construtor/Atribuição:** `std::string` pode ser construída ou atribuída a partir de C-strings.

```cpp
#include <iostream>
#include <string>
#include <cstring> // Para strlen

// API em C simulada
void imprimir_estilo_c(const char* c_str) {
    printf("String (estilo C): %s, (strlen: %zu)\n", c_str, strlen(c_str));
}

int main() {
    // 1. De C-string para std::string
    char c_array[] = "Array de C";
    std::string s1 = c_array; // Construtor de cópia
    std::cout << "s1: " << s1 << std::endl;

    // 2. De std::string para C-string
    std::string s2 = "String de C++";
    
    // Passando s2 para uma função C
    imprimir_estilo_c(s2.c_str()); 
    
    return 0;
}
```

## Considerações Finais

Neste capítulo, navegamos pelo universo da manipulação de caracteres e strings, contrastando as duas filosofias de C e C++.

Iniciamos com a abordagem fundamental em C: o tipo `char` como um inteiro pequeno e a definição de **string como um array de `char` terminado em `\0`**. Exploramos as funções da biblioteca `<stdio.h>` para E/S e aprofundamos nas funções de `<string.h>` (`strlen`, `strcpy`, `strcat`, `strcmp`, etc.). Enfatizamos que, embora eficientes, essas funções exigem gerenciamento manual de memória e do terminador nulo, sendo uma fonte histórica de bugs perigosos, como **buffer overflows**. A segurança em C depende inteiramente do programador, que deve optar por funções mais seguras como `fgets`, `strncpy` e `strncat`, e ainda assim gerenciar os tamanhos e a terminação nula manualmente.

Em seguida, contrastamos essa abordagem com a robustez e a conveniência oferecidas pela classe `std::string` do C++. Vimos como `std::string` encapsula a complexidade do gerenciamento de memória, oferecendo crescimento dinâmico, operações seguras (como `+` e `=`), métodos ricos (`append`, `length`, `size`) e acesso seguro (com `at()`). A conversão entre números e strings com `std::to_string` e `std::stringstream` e a fácil interação com C-strings (via `c_str()`) solidificam `std::string` como a ferramenta padrão para manipulação de texto em C++ moderno.

A escolha entre strings estilo C e `std::string` em C++ geralmente pende para `std::string` devido à sua segurança e facilidade de uso. No entanto, a compreensão das strings estilo C permanece crucial, não apenas para trabalhar com código C legado ou APIs que as utilizam, mas também para entender os fundamentos de baixo nível sobre os quais abstrações mais complexas são construídas.
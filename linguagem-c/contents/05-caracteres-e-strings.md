# Capítulo 5 – Caracteres e Strings

Nos capítulos anteriores, estabelecemos uma base sólida sobre os tipos de dados numéricos, variáveis e os operadores que nos permitem realizar cálculos e tomar decisões. Agora, voltaremos nossa atenção para um tipo de dado igualmente fundamental na programação: o texto. A capacidade de armazenar, manipular e exibir sequências de caracteres é essencial para a grande maioria das aplicações, desde simples mensagens ao usuário até o processamento complexo de documentos e dados textuais.

Neste capítulo, exploraremos como as linguagens C e C++ lidam com caracteres individuais e com sequências de caracteres, comumente conhecidas como **strings**. Iniciaremos nossa jornada revisando o tipo `char` em C, a unidade básica para representar caracteres, e as funções padrão para sua entrada e saída. Em seguida, mergulharemos no conceito de strings em C: como elas são representadas (arrays de caracteres terminados por um caractere nulo), como são declaradas, inicializadas e manipuladas utilizando as funções da biblioteca `<string.h>`.

Posteriormente, faremos a transição para o C++, onde a manipulação de strings é significativamente simplificada e enriquecida pela classe `std::string`, parte da biblioteca padrão `<string>`. Discutiremos suas vantagens, como o gerenciamento automático de memória e a vasta gama de funções membro para operações como concatenação, obtenção de tamanho, acesso a caracteres individuais e conversão de números para strings. Ao final deste capítulo, você terá uma compreensão clara das duas abordagens para manipulação de texto, capacitando-o a escolher a ferramenta mais adequada para cada contexto e a escrever código mais seguro e eficiente.

## Caracteres em C: O Tipo `char`

Conforme vimos no Capítulo 2, o tipo de dado `char` em C é fundamentalmente projetado para armazenar um único caractere. Geralmente, um `char` ocupa 1 byte de memória e é usado para representar caracteres do conjunto ASCII (American Standard Code for Information Interchange), que mapeia números inteiros (de 0 a 127, ou de -128 a 127 para `signed char`, e 0 a 255 para `unsigned char`) para símbolos específicos, letras e dígitos.

**Declaração e Inicialização:**

```c
#include <stdio.h>

int main() {
    char letra_a = 'A';
    char simbolo_arroba = '@';
    char digito_um = '1';
    char char_ascii = 66; // Corresponde a 'B' na tabela ASCII

    printf("Letra: %c\n", letra_a);
    printf("Símbolo: %c\n", simbolo_arroba);
    printf("Dígito como caractere: %c\n", digito_um);
    printf("Caractere ASCII 66: %c\n", char_ascii);

    return 0;
}
```

Note que literais de caractere são envolvidos por aspas simples (`' '`).

**Entrada e Saída de Caracteres:** As funções `getchar()` e `putchar()`, e os especificadores `%c` em `scanf()` e `printf()`, são comumente usados para E/S de caracteres, como já explorado no Capítulo 4.

```c
#include <stdio.h>

int main() {
    char c;
    printf("Digite um caractere: ");
    // scanf(" %c", &c); // Usando scanf, o espaço antes de %c consome o newline anterior
    c = getchar(); // Alternativa mais simples para um único caractere

    printf("Você digitou: ");
    putchar(c);
    putchar('\n');

    return 0;
}
```

## Strings em C: Sequências de Caracteres Terminadas por Nulo

Diferentemente de algumas linguagens de programação de alto nível, a linguagem C não possui um tipo de dado string embutido. Em C, uma **string** é convencionalmente representada como um **array (vetor) de caracteres (`char`)** que é terminado por um caractere especial chamado **caractere nulo (`\0`)**. Este caractere nulo atua como um sentinela, indicando o final da string para as funções da biblioteca padrão que manipulam strings.

**Declaração e Inicialização de Strings em C:**

Existem algumas maneiras de declarar e inicializar strings em C:

1. **Como um array de `char` inicializado com um literal de string:** O compilador automaticamente adiciona o caractere nulo `\0` no final.
    
    ```c
    char saudacao[] = "Ola"; // O compilador aloca 6 bytes: 'O','l','a','\0' + 2
                             // bytes de padding (depende do compilador), mais
                             // precisamente, o tamanho será o suficiente para "Ola\0"
    char outra_string[20] = "Mundo"; // Aloca 20 bytes, preenche com "Mundo\0"
                                     // e o restante com zeros (ou lixo,
                                     // dependendo do contexto)
    ```
    
	Neste caso, `saudacao` terá tamanho suficiente para armazenar "Ola" mais o `\0`. Se um tamanho explícito é dado (como em `outra_string[20]`), ele deve ser grande o suficiente para conter a string e o terminador nulo.
    
2. **Como um array de `char` inicializado com uma lista de caracteres:** Neste caso, você **deve** incluir explicitamente o caractere nulo.
    
    ```c
    char palavra[] = {'t', 'e', 's', 't', 'e', '\0'};
    ```
    
3. **Como um ponteiro para `char` inicializado com um literal de string:** Isso cria um ponteiro para uma string literal, que geralmente é armazenada em uma área de memória somente leitura. Tentar modificar uma string declarada desta forma pode levar a comportamento indefinido ou falhas.
    
    ```c
    const char *ptr_string = "Exemplo"; // Boa prática usar const para strings literais
    // ptr_string[0] = 'e'; // COMPORTAMENTO INDEFINIDO OU ERRO!
    ```
    
    Para strings que precisam ser modificadas, a abordagem com array é a correta.

**Entrada de Strings em C:**

- **`scanf("%s", nome_do_array);`**: Lê caracteres do `stdin` até encontrar um espaço em branco (espaço, tabulação, nova linha). Adiciona automaticamente o `\0`. **Perigo:** Não verifica o tamanho do array, podendo causar **estouro de buffer (buffer overflow)** se a entrada for maior que o array.
    
    ```c
    #include <stdio.h>
    int main() {
        char nome[20];
        printf("Digite seu primeiro nome: ");
        scanf("%s", nome); // Não precisa do & porque 'nome' já é um endereço
        printf("Nome digitado: %s\n", nome);
        return 0;
    }
    ```
    
- **`fgets(char *str, int tamanho, FILE *fluxo);`**: Forma mais segura de ler strings. Lê no máximo `tamanho-1` caracteres do `fluxo` (ex: `stdin`), parando se encontrar `\n` ou `EOF`. O `\n` (se lido e houver espaço) é mantido na string, e um `\0` é sempre adicionado.
    
    ```c
    #include <stdio.h>
    #define MAX_LEN 30
    int main() {
        char linha[MAX_LEN];
        printf("Digite uma frase: ");
        if (fgets(linha, MAX_LEN, stdin) != NULL) {
            // fgets pode incluir o \n, vamos removê-lo se necessário
            // ... (código para remover \n, como visto no capítulo anterior) ...
            printf("Linha digitada: %s", linha); // %s para no \0
        }
        return 0;
    }
    ```

**Saída de Strings em C:**

- **`printf("%s", nome_do_array_ou_ponteiro);`**: Imprime a string até encontrar o caractere nulo `\0`.
    
    ```c
    char msg[] = "Teste de saída.";
    printf("%s\n", msg);
    ```
    
- **`puts(const char *str);`**: Imprime a string `str` no `stdout` e automaticamente adiciona um caractere de nova linha (`\n`) ao final.
    
    ```
    char cidade[] = "Recife";
    puts(cidade); // Imprime "Recife" seguido de uma nova linha
    ```
    

### Funções da Biblioteca `<string.h>`

A biblioteca padrão C fornece um conjunto de funções úteis para manipulação de strings no arquivo de cabeçalho `<string.h>`. É essencial incluir este cabeçalho para usá-las:

```c
#include <string.h>
```

Aqui estão algumas das funções mais comuns:

- **`size_t strlen(const char *str);`**: Retorna o comprimento da string `str` (número de caracteres antes do `\0`). `size_t` é um tipo inteiro sem sinal.
    
    ```c
    char texto[] = "Exemplo";   // Comprimento 7
    size_t len = strlen(texto); // len será 7
    printf("Comprimento de '%s': %zu\n", texto, len);
    ```
    
- **`char *strcpy(char *destino, const char *origem);`**: Copia a string `origem` (incluindo o `\0`) para a string `destino`. **Perigo:** Não verifica o tamanho de `destino`, podendo causar buffer overflow.
    
    ```c
    char origem_str[] = "Copiar isto";
    char destino_str[20];
    strcpy(destino_str, origem_str);
    printf("Destino após strcpy: %s\n", destino_str);
    ```
    
- **`char *strncpy(char *destino, const char *origem, size_t n);`**: Copia no máximo `n` caracteres da string `origem` para `destino`. Se `origem` tiver menos de `n` caracteres, `destino` é preenchido com `\0` até `n` caracteres serem escritos. Se `origem` tiver `n` ou mais caracteres, **apenas `n` caracteres são copiados e `destino` pode não ser terminado por nulo se `strlen(origem) >= n`**. É crucial garantir a terminação nula manualmente se necessário.
    
    ```c
    char src[] = "Texto longo";
    char dest[5];
    strncpy(dest, src, 4); // Copia "Text"
    dest[4] = '\0';        // Garante a terminação nula!
    printf("Destino após strncpy: %s\n", dest);
    ```
    
- **`char *strcat(char *destino, const char *origem);`**: Anexa (concatena) a string `origem` ao final da string `destino`. O primeiro caractere de `origem` sobrescreve o `\0` de `destino`. `destino` deve ter espaço suficiente. **Perigo:** Risco de buffer overflow.
    
    ```c
    char str1[50] = "Bom ";
    char str2[] = "dia!";
    strcat(str1, str2); // str1 agora é "Bom dia!"
    printf("str1 após strcat: %s\n", str1);
    ```
    
- **`char *strncat(char *destino, const char *origem, size_t n);`**: Anexa no máximo `n` caracteres de `origem` ao final de `destino` e adiciona um `\0`. `destino` deve ter espaço suficiente. Mais seguro que `strcat`.
    
    ```c
    char str_a[20] = "Hello";
    char str_b[] = " World!!!";
    strncat(str_a, str_b, 7); // Anexa " World!" (7 caracteres) e o '\0'
    printf("str_a após strncat: %s\n", str_a); // "HelloWorld!"
    ```
    
- **`int strcmp(const char *str1, const char *str2);`**: Compara lexicograficamente `str1` e `str2`. Retorna:
    
    - `0` se `str1` é igual a `str2`.
    - Um valor negativo se `str1` vem antes de `str2`.
    - Um valor positivo se `str1` vem depois de `str2`.
    
    ```c
    char s1[] = "abc";
    char s2[] = "abd";
    char s3[] = "abc";
    if (strcmp(s1, s3) == 0) printf("'%s' e '%s' são iguais.\n", s1, s3);
    if (strcmp(s1, s2) < 0) printf("'%s' vem antes de '%s'.\n", s1, s2);
    ```
    
- **`int strncmp(const char *str1, const char *str2, size_t n);`**: Similar a `strcmp`, mas compara no máximo os primeiros `n` caracteres.
    
- **`char *strchr(const char *str, int c);`**: Retorna um ponteiro para a primeira ocorrência do caractere `c` (convertido para `char`) na string `str`, ou `NULL` se não encontrado.
    
    ```c
    char frase[] = "Linguagem C";
    char *p_char = strchr(frase, 'g');
    if (p_char != NULL) printf("Caractere 'g' encontrado em: %s\n", p_char);
    ```
    
- **`char *strstr(const char *haystack, const char *needle);`**: Retorna um ponteiro para a primeira ocorrência da substring `needle` dentro da string `haystack`, ou `NULL` se não encontrada.
    
    ```c
    char texto_completo[] = "Este é um teste de string.";
    char sub[] = "teste";
    char *p_sub = strstr(texto_completo, sub);
    if (p_sub != NULL) printf("Substring '%s' encontrada em: %s\n", sub, p_sub);
    ```

**Importância do Caractere Nulo (`\0`) e Segurança:** O gerenciamento manual do caractere nulo e dos tamanhos dos arrays é uma fonte comum de erros em C, especialmente os perigosos **buffer overflows**. Funções como `strncpy`, `strncat` e `fgets` foram introduzidas para oferecer alternativas mais seguras, mas ainda exigem atenção do programador para garantir que os buffers de destino sejam grandes o suficiente e que as strings resultantes sejam devidamente terminadas por nulo.

## Strings em C++: A Classe `std::string`

O C++ melhora drasticamente a manipulação de strings ao introduzir a classe `std::string` como parte de sua biblioteca padrão. Para utilizá-la, é necessário incluir o cabeçalho `<string>`:

```cpp
#include <string>
// É comum usar a diretiva 'using namespace std;' ou qualificar com 'std::'
// using namespace std;
// Para simplificar os exemplos abaixo
```

A classe `std::string` oferece uma série de vantagens sobre as strings estilo C:

- **Gerenciamento Automático de Memória:** O programador não precisa se preocupar com alocação, desalocação ou tamanho do buffer. A string cresce e diminui conforme necessário.
- **Segurança:** Operações como concatenação e cópia são mais seguras contra buffer overflows.
- **Riqueza de Funcionalidades:** Fornece um vasto conjunto de métodos (funções membro) para manipulação, busca, comparação e modificação de strings.

**Declaração e Inicialização:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string nome = "João"; // Inicialização direta com literal de string
    std::string sobrenome("Silva"); // Outra forma de inicialização
    std::string cidade; // String vazia por padrão

    cidade = "Recife"; // Atribuição

    std::cout << "Nome: " << nome << std::endl;
    std::cout << "Sobrenome: " << sobrenome << std::endl;
    std::cout << "Cidade: " << cidade << std::endl;
    return 0;
}
```

### Concatenação de Strings em C++

A concatenação (junção) de strings em C++ é intuitiva e pode ser feita de duas maneiras principais:

1. **Usando o operador de adição (`+`):** Este operador, quando usado com `std::string`, cria uma nova string que é o resultado da junção das strings operandas.
    
    ```cpp
    #include <iostream>
    #include <string>
    
    int main() {
        std::string firstName = "John "; // Note o espaço no final
        std::string lastName = "Doe";
        std::string fullName = firstName + lastName;
        std::cout << "Nome completo (com +): " << fullName << std::endl; // Saída: John Doe

        std::string parte1 = "Olá";
        std::string parte2 = "Mundo";
        std::string saudacao = parte1 + ", " + parte2 + "!"; // Múltiplas concatenações
        std::cout << saudacao << std::endl; // Saída: Olá, Mundo!
        return 0;
    }
    ```
    
2. **Usando o método `append()`:** O método `append()` é uma função membro da classe `std::string` que anexa o conteúdo de uma string (ou parte dela, ou mesmo caracteres repetidos) ao final da string na qual o método é chamado. Diferentemente do operador `+` que sempre cria uma nova string, `append()` modifica o objeto string existente.
    
    ```cpp
    #include <iostream>
    #include <string>
    
    int main() {
        std::string str1 = "Hello, ";
        std::string str2 = "world!";
    
        str1.append(str2); // Adiciona str2 ao final de str1
        std::cout << "str1.append(str2): " << str1 << std::endl; // Saída: Hello, world!
    
        std::string str3 = "Bem-vindo";
        str3.append(" ao C++"); // Adiciona um literal de string
        std::cout << "str3.append(\" ao C++\"): " << str3 << std::endl; // Saída: Bem-vindo ao C++
    
        std::string str4 = "Exemplo";
        std::string str5 = " de substring";
        // Adiciona os primeiros 3 caracteres de str5 (" de") ao final de str4
        str4.append(str5, 0, 3); 
        std::cout << "str4.append(str5, 0, 3): " << str4 << std::endl; // Saída: Exemplo de
        return 0;
    }
    ```
    

### Números e Strings em C++

Em C++, não se pode concatenar diretamente um número a uma `std::string` usando o operador `+` como se faria em algumas linguagens de script. Isso ocorre porque o operador `+` para `std::string` é sobrecarregado para operar com outras strings ou com caracteres, mas não diretamente com tipos numéricos para conversão implícita para string. Tentar fazer `std::string s = "Valor: " + 10;` resultaria em erro de compilação.

Para incluir um número em uma string, é necessário primeiro convertê-lo para uma representação de string. A partir do C++11, a maneira mais direta é usar a função `std::to_string()` (do cabeçalho `<string>`):

```cpp
#include <iostream>
#include <string> // Para std::string e std::to_string

int main() {
    int x = 10;
    double pi = 3.14159;

    std::string msg1 = "O valor de x é: " + std::to_string(x);
    std::string msg2 = "PI é aproximadamente: " + std::to_string(pi);

    std::cout << msg1 << std::endl; // Saída: O valor de x é: 10
    std::cout << msg2 << std::endl; // Saída: PI é aproximadamente: 3.141590 (precisão padrão de to_string para double)
    return 0;
}
```

A função `std::to_string()` converte diversos tipos numéricos (como `int`, `long`, `float`, `double`) para sua representação em `std::string`.

Para formatações mais complexas ou para compatibilidade com versões mais antigas de C++, pode-se usar `std::stringstream` (do cabeçalho `<sstream>`):

```cpp
#include <iostream>
#include <string>
#include <sstream> // Para std::stringstream
#include <iomanip> // Para formatação

int main() {
    int idade = 25;
    double altura = 1.785;
    
    std::stringstream ss;
    ss << "Usuário tem " << idade << " anos e " 
       << std::fixed << std::setprecision(2) << altura << "m de altura.";
    
    std::string info = ss.str(); // Extrai a string formatada do stream
    std::cout << info << std::endl; 
    // Saída: Usuário tem 25 anos e 1.79m de altura.
    return 0;
}
```

### Comprimento da String em C++

Para obter o número de caracteres em uma `std::string`, a classe oferece dois métodos membros que são equivalentes: `length()` e `size()`. Ambos retornam um valor do tipo `size_t` (um tipo inteiro sem sinal).

```cpp
#include <iostream>
#include <string>

int main() {
    std::string texto = "Olá mundo!"; // 10 caracteres (incluindo o espaço, mas não o terminador nulo interno)
    
    size_t comprimento = texto.length();
    size_t tamanho = texto.size();

    std::cout << "Comprimento (length): " << comprimento << std::endl; // Saída: 10
    std::cout << "Tamanho (size): " << tamanho << std::endl;       // Saída: 10
    return 0;
}
```

Ambas as funções retornam o número de caracteres efetivos na string, sem contar o caractere nulo que `std::string` pode usar internamente para compatibilidade com funções estilo C.

### Acessar Caracteres em Strings C++

Você pode acessar caracteres individuais de uma `std::string` de algumas maneiras:

1. **Usando o operador de colchetes `[]`:** Este operador permite acessar um caractere em um índice específico. Os índices em C++ (e C) começam em 0.
    
    ```cpp
    #include <iostream>
    #include <string>
    
    int main() {
        std::string texto = "Olá mundo!";
        char primeiro_char = texto[0];   // 'O'
        char terceiro_char = texto[2];   // 'á'
        char ultimo_char_visivel = texto[9]; // '!' (texto.length() - 1)
    
        std::cout << "Primeiro: " << primeiro_char << std::endl;
        std::cout << "Terceiro: " << terceiro_char << std::endl;
        std::cout << "Último visível: " << ultimo_char_visivel << std::endl;
    
        // Modificando um caractere
        texto[0] = 'A';
        std::cout << "Texto modificado: " << texto << std::endl; // Saída: Alá mundo!
        return 0;
    }
    ```
    
    **Cuidado:** O operador `[]` não realiza verificação de limites. Acessar um índice fora da faixa da string (ex: `texto[100]` para uma string curta) resulta em comportamento indefinido.
    
2. **Usando o método `at()`:** O método `at()` também acessa um caractere em um índice específico, mas com uma diferença crucial: ele **realiza verificação de limites**. Se o índice estiver fora da faixa válida, `at()` lança uma exceção do tipo `std::out_of_range`.
    
    ```cpp
    #include <iostream>
    #include <string>
    #include <stdexcept> // Para std::out_of_range
    
    int main() {
        std::string texto = "Exemplo";
        try {
            char ch1 = texto.at(0); // 'E'
            std::cout << "Caractere em texto.at(0): " << ch1 << std::endl;
    
            char ch_fora = texto.at(10); // Lançará std::out_of_range
            std::cout << "Este não será impresso." << std::endl;
    
        } catch (const std::out_of_range& oor) {
            std::cerr << "Erro de acesso: " << oor.what() << std::endl;
        }
        return 0;
    }
    ```

**Iterando sobre os caracteres de uma string:** Você pode percorrer todos os caracteres de uma `std::string` usando um laço `for`:

- **Com índice (estilo C ou C++ tradicional):**
    
    ```cpp
    #include <iostream>
    #include <string>
    
    int main() {
        std::string texto = "Olá!";
        for (size_t i = 0; i < texto.length(); ++i) {
            std::cout << "Caractere [" << i << "]: " << texto[i] << std::endl;
        }
        return 0;
    }
    ```
    
- **Com laço `for` baseado em intervalo (Range-based for loop - C++11 em diante):** Esta é uma forma mais moderna e concisa.
    
    ```cpp
    #include <iostream>
    #include <string>
    
    int main() {
        std::string texto = "Teste";
        for (char c : texto) { // Para cada caractere 'c' na string 'texto'
            std::cout << c << " ";
        }
        std::cout << std::endl; // Saída: T e s t e
        return 0;
    }
    ```
    
	Se precisar modificar os caracteres dentro do laço, use uma referência: `for (char &c : texto)`.
    

## Considerações Finais

Neste capítulo, navegamos pelo universo da manipulação de caracteres e strings nas linguagens C e C++. Iniciamos com a abordagem fundamental em C, onde caracteres são representados pelo tipo `char` e strings são tratadas como arrays de caracteres terminados pelo caractere nulo (`\0`). Exploramos as funções da biblioteca `<stdio.h>` para entrada e saída e as principais funções de `<string.h>` para operações como cálculo de comprimento, cópia, concatenação e comparação, ressaltando a necessidade de um gerenciamento cuidadoso da memória e do terminador nulo para evitar erros comuns como buffer overflows.

Em seguida, contrastamos essa abordagem com a robustez e a conveniência oferecidas pela classe `std::string` do C++. Vimos como o C++ simplifica tarefas como concatenação, obtenção de tamanho e acesso a caracteres individuais, ao mesmo tempo que oferece maior segurança através do gerenciamento automático de memória e de métodos como `at()` que verificam limites. A conversão entre números e strings com `std::to_string()` também foi destacada como uma facilidade moderna.

A escolha entre strings estilo C e `std::string` em C++ geralmente pende para `std::string` devido à sua segurança e facilidade de uso em código C++. No entanto, a compreensão das strings estilo C permanece crucial, não apenas para trabalhar com código C legado ou APIs que as utilizam, mas também para entender os fundamentos sobre os quais abstrações mais complexas são construídas. Com este conhecimento, você está melhor equipado para lidar com dados textuais, uma tarefa onipresente no desenvolvimento de software.

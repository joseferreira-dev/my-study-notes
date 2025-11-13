# Capítulo 2 – Tipos de Dados e Variáveis

No capítulo anterior, estabelecemos uma compreensão fundamental sobre as linguagens C e C++, suas filosofias e o processo de compilação. Com essa base, estamos agora preparados para mergulhar em um dos aspectos mais cruciais da programação: a forma como os dados são representados e manipulados. Toda informação processada por um computador, desde um simples caractere até estruturas de dados complexas, precisa ser categorizada e armazenada de maneira eficiente. É aqui que entram os **tipos de dados** e as **variáveis**. Este capítulo se dedica a explorar em profundidade esse tema, formando o alicerce sobre o qual construiremos lógicas e algoritmos.

Iniciaremos definindo as regras para **identificadores**, os nomes que damos aos elementos do nosso programa. Em seguida, definiremos o que é uma **variável**, o contêiner de dados fundamental, distinguindo os conceitos vitais de declaração, definição e inicialização, e alertando sobre os perigos do "lixo de memória". Com isso estabelecido, exploraremos o "recheio" desses contêineres: os **tipos básicos de dados** em C (`int`, `char`, `float`, `double`, `void`), seus **modificadores** (`short`, `long`, `signed`, `unsigned`) e como eles definem o universo de valores com os quais podemos trabalhar.

Com os tipos e variáveis definidos, analisaremos como o local da declaração impacta o **escopo** (visibilidade) e o **tempo de vida** (duração) de uma variável, dissecando as diferenças cruciais entre variáveis locais, globais e parâmetros. Aprofundaremos esse controle com **qualificadores de tipo** (`const`, `volatile`) e **especificadores de classe de armazenamento** (`static`, `register`), que alteram o comportamento e a semântica das nossas variáveis. Por fim, faremos a transição para o C++, demonstrando como ele herda e expande esses conceitos com novos tipos (`bool`, `std::string`) e mecanismos de inicialização mais seguros e expressivos.

## Identificadores: Dando Nomes aos Elementos

Antes de podermos criar qualquer variável, função ou outro elemento em nosso programa, precisamos de um nome para ele. Em C e C++, esses nomes são chamados de **identificadores**. A escolha de bons identificadores é uma das práticas mais importantes para a legibilidade e manutenção do código, mas, antes de tudo, eles devem seguir regras sintáticas rígidas.

A linguagem C estabelece as seguintes regras, que o C++ herda e expande:

- Um identificador pode variar em comprimento, de um a vários caracteres.
- O **primeiro caractere** de um identificador deve ser obrigatoriamente uma **letra** (de `a` a `z` ou de `A` a `Z`) ou um caractere de **sublinhado (`_`)**.
- Os **caracteres subsequentes** (do segundo em diante) podem ser **letras**, **números** (de `0` a `9`) ou o caractere de **sublinhado (`_`)**.
- **Nenhum outro caractere especial** (como `!`, `@`, `#`, `$`, `%`, `&`, `*`, `(`, `)`, `-`, `+`, `=`, `.`, `,`, `/`, etc.) é permitido.

A tabela a seguir ilustra exemplos práticos dessas regras:

|**Correto**|**Incorreto**|**Motivo do Erro (para os incorretos)**|
|---|---|---|
|`count`|`1count`|Não pode começar com um número.|
|`test23`|`hi!there`|Contém caractere especial (`!`).|
|`high_balance`|`high...balance`|Contém caractere especial (`.`).|
|`_saldo`|`taxa-juros`|Contém caractere especial (`-`).|
|`nomeDoUsuario`|`while`|É uma palavra-chave reservada da linguagem.|
|`valorMaximo`|`printf`|Mesmo nome de uma função da biblioteca (pode causar conflitos).|

### Palavras-Chave e Sensibilidade de Caso

Uma característica fundamental de C e C++ é que ambas são **case sensitive**, ou seja, diferenciam rigorosamente letras maiúsculas de minúsculas. Portanto, para o compilador, `contador`, `Contador` e `CONTADOR` seriam três identificadores completamente distintos.

Além disso, os identificadores **não podem ser iguais a nenhuma das palavras-chave reservadas** da linguagem. Essas palavras (como `int`, `if`, `for`, `while`, `return`, `class`, `public`, `static`, `const`, etc.) são reservadas para a sintaxe da própria linguagem e não podem ser usadas para outros fins.

Também é uma prática altamente recomendável evitar dar aos seus identificadores os mesmos nomes de funções ou tipos presentes nas bibliotecas padrão (como `printf`, `scanf`, `strcpy`). Embora tecnicamente possa ser possível em alguns contextos específicos, isso leva a conflitos de nomes (colisões), confunde leitores do código e pode causar comportamento indefinido.

### Limites de Tamanho e Significância

O padrão ANSI C original (C89) especifica que os identificadores podem ter, teoricamente, qualquer tamanho. No entanto, ele estabelecia limites mínimos para a quantidade de caracteres que deviam ser _significativos_ (ou seja, que o compilador e o linkeditor eram obrigados a diferenciar):

- Para **nomes internos** (como variáveis locais ou funções `static`), pelo menos os **primeiros 31 caracteres** deviam ser significativos.
- Para **nomes externos** (visíveis ao linkeditor, como funções globais ou variáveis globais), pelo menos os **primeiros 6 caracteres** deviam ser significativos.

Essa limitação de 6 caracteres para nomes externos era um reflexo da tecnologia de _linkeditors_ (ligadores) da época, que eram programas mais simples. Na prática moderna, isso é completamente obsoleto. Compiladores C e C++ atuais (como GCC, Clang e MSVC) suportam nomes muito mais longos (frequentemente 2048 caracteres ou mais), tornando isso uma não-preocupação para o desenvolvimento de novos softwares.

### Boas Práticas e Convenções de Nomenclatura

Seguir as regras sintáticas apenas torna o código _compilável_. Seguir boas convenções de nomenclatura torna o código _legível_ e _mantível_.

1. **Escolha Nomes Descritivos:** O nome `taxa_juros_anual` ou `taxaJurosAnual` é infinitamente melhor do que `t`, `j` ou `x1`. O nome deve descrever o _propósito_ do dado.
2. **Seja Consistente:** Escolha um estilo de nomenclatura e mantenha-o em todo o projeto. Os estilos mais comuns são:
    - **`snake_case`:** `numero_de_alunos`, `valor_total`. Muito comum em código C e preferido por muitas equipes de C++ para nomes de variáveis e funções.
    - **`camelCase`:** `numeroDeAlunos`, `valorTotal`. Muito comum em C++ (especialmente para nomes de variáveis e funções) e em outras linguagens como Java e C#.
    - **`PascalCase`:** `NumeroDeAlunos`, `ValorTotal`. Quase universalmente usado para nomes de tipos de dados definidos pelo usuário em C++ (classes, structs, enums).
3. **Evite Começar com Sublinhado (`_`):**
    - `_nome`: Nomes que começam com um único sublinhado são, por convenção, usados para implementações internas de bibliotecas ou para indicar membros "privados" em classes (embora o C++ tenha formas melhores de fazer isso). É melhor evitá-los em código de aplicação comum.
    - `__nome`: Nomes com dois sublinhados (ou um sublinhado seguido de letra maiúscula) são **reservados para a implementação do compilador e da biblioteca padrão**. _Nunca_ crie identificadores que comecem com `__` em seu próprio código. Eles podem colidir silenciosamente com nomes internos do sistema (como macros ocultas) e causar falhas catastróficas e difíceis de depurar.

## Variáveis: Contêineres de Dados

Uma **variável** é o conceito central para o armazenamento de dados. Conceitualmente, é uma "caixa" na memória do computador. Essa caixa tem três propriedades:

1. **Rótulo (Identificador):** O nome que damos a ela (ex: `idade`).
2. **Formato (Tipo de Dado):** O que ela pode guardar (ex: `int`).
3. **Conteúdo (Valor):** O dado que está atualmente dentro dela (ex: `25`).

Em C e C++, linguagens de **tipagem estática**, uma variável precisa ser **declarada** antes de ser usada. A declaração informa ao compilador seu _tipo_ e seu _nome_, permitindo que ele reserve o espaço de memória adequado e verifique se as operações feitas com ela são válidas.

A forma geral para a declaração de uma variável em C (e de forma similar em C++) é:

```
tipo lista_de_variaveis;
```

- `tipo` deve ser um tipo de dado válido (como `int`, `float`, `char`) acrescido de quaisquer modificadores.
- `lista_de_variaveis` pode consistir em um ou mais identificadores (os nomes das variáveis), separados por vírgulas.

```c
int i, j, k;       // Declara três variáveis do tipo inteiro.
float salario;     // Declara uma variável do tipo ponto flutuante.
char opcao_menu;   // Declara uma variável do tipo caractere.
short int idade;   // Declara uma variável do tipo inteiro curto.
unsigned long int populacao_mundial; // Declara uma inteira longa sem sinal.
```

É importante ressaltar que o **nome da variável (o identificador) não possui nenhuma relação intrínseca com o seu tipo de dado**. Contudo, é uma prática de programação universalmente aceita e altamente recomendável escolher **nomes significativos e descritivos** para as variáveis. Nomes como `idade_do_cliente`, `total_de_vendas` ou `caractere_digitado` tornam o código muito mais legível e fácil de entender do que nomes genéricos como `x`, `a` ou `temp1`.

### Declaração, Definição e Inicialização: Conceitos Cruciais

Esses três termos são frequentemente confundidos, mas são distintos e vitais em C/C++.

1. **Declaração (Declaration):** Introduz um nome (identificador) no programa e especifica seu tipo. Ela diz ao compilador: "Este nome existe e tem este tipo". Uma declaração _não_ aloca memória.
    - `extern int x;` (Informa que `x` é um `int` definido em _outro_ arquivo. Não aloca espaço).
    - `void minha_funcao(int a);` (Declaração de uma função, ou _protótipo_).
2. **Definição (Definition):** É uma declaração que também _causa a alocação de armazenamento_. É o ponto onde a variável é efetivamente criada na memória. Quase todas as declarações de variáveis que vimos são, na verdade, definições.
    - `int x;` (Isto é uma declaração _e_ uma definição. O compilador reserva espaço para `x`).
    - `void minha_funcao(int a) { /* ... */ }` (Definição da função, aloca espaço para o código).
3. **Inicialização (Initialization):** É o ato de atribuir um valor inicial a uma variável _no momento de sua definição_.

```c
int contador;        // Definição (sem inicialização)
int salario = 5000;  // Definição E Inicialização
extern int status;   // Apenas Declaração (a definição está em outro lugar)
```

A distinção entre declaração e definição é fundamental em projetos com múltiplos arquivos. Uma variável global deve ser _definida_ exatamente uma vez (em um arquivo `.c` ou `.cpp`) e _declarada_ (usando `extern`) em qualquer outro arquivo que precise usá-la.

_Arquivo `utils.c` (Definição):_

```c
// DEFINE a variável, alocando memória para ela e inicializando-a com 0.
int contador_global_de_erros = 0; 
```

_Arquivo `main.c` (Declaração):_

```c
#include <stdio.h>

// DECLARAÇÃO: informa ao compilador que esta variável existe em outro lugar.
// Não aloca memória. O linkeditor irá encontrá-la.
extern int contador_global_de_erros; 

void reportar_erro() {
    contador_global_de_erros++; // Usa a variável declarada
}

int main() {
    reportar_erro();
    printf("Total de erros: %d\n", contador_global_de_erros);
    return 0;
}
```

Se `main.c` definisse `int contador_global_de_erros = 0;` novamente, ocorreria um erro de "múltiplas definições" durante a fase de _linkedição_.

### Perigo do "Lixo de Memória" (Undefined Behavior)

O que acontece se definirmos uma variável local, mas não a inicializarmos?

```c
#include <stdio.h>

int main() {
    int valor_nao_inicializado;
    
    // ATENÇÃO: A linha abaixo invoca "Comportamento Indefinido" (Undefined Behavior)
    printf("O valor é: %d\n", valor_nao_inicializado);
    
    return 0;
}
```

Quando `valor_nao_inicializado` é definida, o sistema operacional aloca espaço para ela na memória (na _pilha_, como veremos), mas ele **não limpa** essa memória. Esta é uma decisão de design fundamental do C: "não pague por aquilo que você não usa". Limpar a memória teria um custo de processamento, e o C assume que o programador é responsável por isso.

A variável conterá o que quer que estivesse naquela posição de memória antes (bits residuais de um programa anterior ou de uma chamada de função anterior). Isso é chamado de **lixo de memória** (_garbage_).

Usar esse valor (como no `printf`) é um dos erros mais perigosos em C/C++. O programa pode imprimir `0`, `42`, `-87592` ou pode até travar (crashar). O comportamento é _indefinido_.

**Regra de Ouro:** Sempre inicialize suas variáveis locais antes de usá-las.

```c
int valor_inicializado = 0; // Prática segura
```

_(Nota: Variáveis globais e `static` são inicializadas com `0` por padrão, pois são alocadas uma única vez no início do programa, tornando o custo de inicialização negligenciável)._

## Tipos Básicos de Dados em C

A linguagem C define um conjunto conciso de **cinco tipos básicos de dados**, que são os blocos de construção elementares para toda a manipulação de dados.

1. **`char`**: Para armazenar um único caractere.
2. **`int`**: Para números inteiros.
3. **`float`**: Para números de ponto flutuante de precisão simples.
4. **`double`**: Para números de ponto flutuante de precisão dupla.
5. **`void`**: Um tipo especial que indica a ausência de valor.

O tamanho exato em bytes desses tipos não é fixo; depende da arquitetura (32 vs 64 bits) e do compilador. O padrão ANSI C garante apenas faixas mínimas. A tabela a seguir mostra os tamanhos _típicos_ em sistemas de 32 bits, que são uma referência comum.

| **Tipo**             | **Tamanho em bits (Típico)** | **Faixa Mínima Garantida (Padrão ANSI C)**     |
| -------------------- | ---------------------------- | ---------------------------------------------- |
| `char`               | 8                            | -127 a 127                                     |
| `unsigned char`      | 8                            | 0 a 255                                        |
| `signed char`        | 8                            | -127 a 127                                     |
| `int`                | 16 (ou 32)                   | -32.767 a 32.767                               |
| `unsigned int`       | 16 (ou 32)                   | 0 a 65.535                                     |
| `signed int`         | 16 (ou 32)                   | -32.767 a 32.767                               |
| `short int`          | 16                           | -32.767 a 32.767                               |
| `unsigned short int` | 16                           | 0 a 65.535                                     |
| `signed short int`   | 16                           | -32.767 a 32.767                               |
| `long int`           | 32                           | -2.147.483.647 a 2.147.483.647                 |
| `unsigned long int`  | 32                           | 0 a 4.294.967.295                              |
| `signed long int`    | 32                           | -2.147.483.647 a 2.147.483.647                 |
| `float`              | 32                           | Aproximadamente 6 dígitos de precisão          |
| `double`             | 64                           | Aproximadamente 10 dígitos de precisão         |
| `long double`        | 80 (ou mais)                 | Aproximadamente 10 ou mais dígitos de precisão |

### Tipo `char` (Caractere e Pequeno Inteiro)

O tipo `char` ocupa, por convenção, 1 byte (8 bits). Seu propósito principal é armazenar um único caractere (letra, dígito, símbolo). Na memória, o caractere é armazenado como um número inteiro, que corresponde ao seu código no conjunto de caracteres **ASCII** (American Standard Code for Information Interchange).

O ASCII mapeia números (de 0 a 127) a caracteres. Por exemplo, o número `65` é `'A'`, `97` é `'a'`, e `48` é `'0'`. Como `char` é, no fundo, um tipo inteiro de 8 bits, podemos usá-lo para aritmética de pequenos números.

```c
#include <stdio.h>

int main() {
    char minha_letra = 'A'; // O compilador armazena o valor 65
    char outro_char = 66; // Armazena 66, que é 'B' em ASCII

    // %c imprime o caractere
    printf("Minha letra é: %c\n", minha_letra); // Saída: Minha letra é: A
    printf("Outro char é: %c\n", outro_char);   // Saída: Outro char é: B

    // %d imprime o valor numérico (inteiro)
    printf("O valor ASCII de 'A' é: %d\n", minha_letra); // Saída: O valor ASCII de 'A' é: 65

    // Aritmética com 'char'
    char proxima_letra = minha_letra + 1; // 65 + 1 = 66
    printf("A próxima letra é: %c\n", proxima_letra); // Saída: A próxima letra é: B
    
    // Convertendo um dígito char para int
    char digito_char = '7'; // ASCII de '7' é 55
    int digito_int = digito_char - '0'; // (ASCII de '7') - (ASCII de '0') = 55 - 48 = 7
    
    printf("O char é '%c', o int é %d\n", digito_char, digito_int); // Saída: O char é '7', o int é 7

    return 0;
}
```

Uma "pegadinha" importante: o padrão C **não define se `char` é `signed` ou `unsigned` por padrão**. Isso fica a critério do compilador! Em algumas arquiteturas (como ARM), `char` é `unsigned` por padrão. Em outras (como x86), é `signed`. Se você pretende usar `char` para armazenar números (de -128 a 127), você deve _sempre_ ser explícito: `signed char` ou `unsigned char`.

### Tipo `int` (Inteiro Natural)

O `int` é o tipo inteiro "natural" da máquina. Seu tamanho é escolhido pelo compilador para ser o mais eficiente para o processador-alvo. Em máquinas de 16 bits, era 16 bits. Em máquinas de 32 bits, tornou-se 32 bits.

Curiosamente, em muitas arquiteturas _modernas_ de 64 bits (como Windows 64-bit e Linux 64-bit), `int` **permanece com 32 bits** por razões de compatibilidade e eficiência de memória (modelo LLP64 ou LP64). É o tipo de escolha para a maioria das contagens e números inteiros que não se espera que excedam 2 bilhões.

### Tipos `float` e `double` (Ponto Flutuante)

Para números com casas decimais (números reais), usamos `float` e `double`. Eles são armazenados usando o padrão **IEEE 754**, que divide os bits em _sinal_, _expoente_ e _mantissa_ (fração). A diferença está na **precisão** e na **faixa** de valores.

- **`float`** (precisão simples): Usa 4 bytes (32 bits). É mais rápido (em hardware antigo), mas menos preciso (cerca de 6-7 dígitos decimais de precisão). Use o sufixo `f` ou `F` para literais: `3.14f`.
- **`double`** (precisão dupla): Usa 8 bytes (64 bits). É o tipo padrão para literais de ponto flutuante (ex: `3.14` é um `double` por padrão) e oferece muito mais precisão (cerca de 15-16 dígitos).
- **`long double`**: Um tipo de precisão estendida (geralmente 80 bits no x86, ou 128 bits em outras plataformas). Oferece a maior precisão, mas é mais lento.

A precisão limitada significa que `float` e `double` não podem representar _todos_ os números reais perfeitamente. Isso leva a pequenos erros de arredondamento.

```c
#include <stdio.h>

int main() {
    // Um exemplo clássico de imprecisão de ponto flutuante
    float a = 0.1f;
    float b = 0.2f;
    float soma_f = a + b; // Esperaríamos 0.3

    double c = 0.1;
    double d = 0.2;
    double soma_d = c + d; // Esperaríamos 0.3

    // Imprimindo com alta precisão para ver o erro
    printf("Soma (float):  %.20f\n", soma_f);
    printf("Soma (double): %.20f\n", soma_d);

    if (soma_d == 0.3) {
        printf("A soma double é 0.3 (raro)\n");
    } else {
        printf("A soma double NÃO é exatamente 0.3\n");
    }

    return 0;
}
```

**Saída Típica:**

```
Soma (float):  0.30000001192092896000
Soma (double): 0.30000000000000004000
A soma double NÃO é exatamente 0.3
```

**Regra:** Prefira `double` a menos que tenha restrições severas de memória (ex: arrays gigantes de números). _Nunca_ compare `float` ou `double` com `==` diretamente; verifique se a diferença entre eles é muito pequena (uma tolerância, ou _epsilon_).

### Tipo `void` (Ausência de Tipo)

O tipo `void` é um tipo especial que, paradoxalmente, significa "ausência de tipo". Ele tem três usos cruciais:

1. **Tipo de Retorno de Função:** Indica que uma função não retorna nenhum valor.

    ```c
    void imprimir_mensagem(const char* mensagem) {
        printf("%s\n", mensagem);
        // Sem "return valor;"
    }
    ```

2. **Parâmetros de Função (Estilo C):** Em C (mas não em C++), declarar uma função como `int func(void)` é a forma explícita de dizer que ela não aceita parâmetros. Em C++, `int func()` já significa isso.
3. **Ponteiro Genérico (`void*`):** Este é um uso avançado. Um `void*` é um ponteiro que pode apontar para _qualquer_ tipo de dado (`int`, `float`, `struct`). Funções de biblioteca genéricas (como `malloc` para alocação de memória ou `qsort` para ordenação) usam `void*` para operar em dados de qualquer tipo. Você não pode usá-lo diretamente; ele deve ser "convertido" (cast) de volta para um ponteiro do tipo correto.

## Modificadores de Tipo Básico

Com exceção de `void`, os tipos de dados podem ser alterados por **modificadores** (palavras-chave que alteram as características de um tipo).

- **`signed`**: (Padrão para `int`, `short`, `long`) A variável pode armazenar valores positivos e negativos.
- **`unsigned`**: A variável armazena _apenas_ valores não-negativos (zero e positivos).
- **`long`**: Pede um tipo com "pelo menos" o tamanho do tipo base (ex: `long int`) ou maior precisão (ex: `long double`).
- **`short`**: Pede um tipo com "pelo menos" o tamanho do tipo base, mas geralmente menor que `int` (ex: `short int`).

O padrão C99/C++11 também adicionou `long long int` para garantir inteiros de pelo menos 64 bits.

### Diferença entre `signed` e `unsigned`: O Bit de Sinal

A diferença fundamental está na interpretação do **bit mais significativo (MSB)**:

- Em um tipo **`signed`**, o MSB é usado como **bit de sinal**. Se 0, o número é positivo. Se 1, é negativo. A representação de negativos é quase sempre **complemento de dois**.
- Em um tipo **`unsigned`**, o MSB _não_ é um bit de sinal. Ele é parte do número, assim como todos os outros bits.

**Exemplo com 8 bits (char):**

- `signed char`: Faixa de -128 a 127.
- `unsigned char`: Faixa de 0 a 255.

O valor binário `1111 1111` seria:

- `255` se for `unsigned char`.
- `-1` se for `signed char` (em complemento de dois).

O **Complemento de Dois** é o método que os computadores usam para representar números negativos. Para obter o negativo de um número (ex: 5):

1. Pegue o binário positivo: `0000 0101` (para 5)
2. Inverta todos os bits: `1111 1010`
3. Some 1: 1111 1011 (este é -5)
    A vantagem é que a soma e a subtração funcionam com o mesmo circuito lógico.
    `5 + (-5): 0000 0101 + 1111 1011 = 1 0000 0000`
    O "1" que estoura (carry) é descartado, e o resultado é 0000 0000 (zero).

### "Wrap-around" (Overflow e Underflow)

Tipos `unsigned` (e `signed` também) têm um comportamento circular chamado _wrap-around_.

- **Overflow (Estouro):** Quando você ultrapassa o valor máximo.
    
    ```c
    unsigned char x = 255;
    x = x + 1; // x agora se torna 0 (255 + 1 = 256, que em 8 bits é 1 0000 0000, sobra só 0000 0000)
    ```
    
- **Underflow (Estouro Negativo):** Quando você cai abaixo do valor mínimo.
    
    ```c
    unsigned int contador = 0;
    contador = contador - 1; // contador agora se torna o MAIOR valor unsigned int (ex: 4294967295 em 32 bits)
    ```

Este comportamento é uma fonte comum de bugs e vulnerabilidades de segurança.

```c
#include <stdio.h>

int main() {
    unsigned int contador = 10;
    
    // Contagem regressiva que NUNCA termina como esperado
    while (contador >= 0) { // ALERTA: Esta condição é sempre verdadeira para um 'unsigned'!
        printf("Contador: %u\n", contador);
        
        if (contador == 0) {
             printf("Contador é zero, subtraindo 1...\n");
             contador--;
             printf("Underflow! Contador agora é: %u\n", contador); // Ex: 4294967295
             break; // Interrompe o loop infinito
        }
        contador--; 
    }
    return 0;
}
```

### Tipos de Tamanho Fixo (C99/C++11)

Como `int` e `long` podem variar de tamanho, programar para hardware ou protocolos de rede (que exigem tamanhos exatos) era difícil. O padrão C99 (e C++11) resolveu isso introduzindo a biblioteca `<stdint.h>` (ou `<cstdint>` em C++).

Ela define tipos como:

- `int8_t`, `int16_t`, `int32_t`, `int64_t` (inteiros com sinal de tamanho exato)
- `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t` (inteiros sem sinal de tamanho exato)

**Prática Moderna:** Ao escrever código que depende de um tamanho de bits exato (ex: manipulação de arquivos binários, criptografia, comunicação de rede), prefira usar os tipos de `<stdint.h>` em vez de `int` ou `long`.

## Escopo, Tempo de Vida e Armazenamento

O local onde uma variável é declarada determina seu **escopo** (onde ela é visível) e seu **tempo de vida** (quando ela existe). Isso é controlado pelos _especificadores de classe de armazenamento_.

### Variáveis Locais (Automáticas) e Escopo de Bloco

Variáveis declaradas **dentro de uma função** ou de qualquer bloco de código `{}` são **locais**.

- **Palavra-chave (implícita):** `auto`. (Em C, `auto int x;` é o mesmo que `int x;`. Em C++ moderno, `auto` ganhou um novo significado de inferência de tipo, mas o conceito de armazenamento automático permanece).
- **Armazenamento:** Na **Pilha (Stack)**. A pilha é uma região de memória LIFO (Last-In, First-Out). Quando uma função é chamada, um "quadro de pilha" (_stack frame_) é criado para suas variáveis locais. Quando a função termina, esse quadro é destruído.
- **Escopo:** Visíveis _apenas_ dentro do bloco `{}` onde foram declaradas (incluindo blocos internos).
- **Tempo de Vida:** Criadas quando o bloco é iniciado e destruídas quando o bloco é finalizado.
- **Inicialização:** Contêm **lixo de memória** se não forem explicitamente inicializadas.

```c
#include <stdio.h>

void minha_funcao() {
    int x = 10; // 'x' é local. Vive na pilha da minha_funcao.
    printf("Na funcao, x = %d\n", x);

    if (x == 10) {
        int y = 20; // 'y' é local. Vive na pilha, mas só é visível dentro do 'if'.
        printf("Dentro do if, x = %d, y = %d\n", x, y);
    } // 'y' é destruído aqui.

    // printf("y = %d\n", y); // ERRO! 'y' não existe mais aqui.
} // 'x' é destruído aqui.

int main() {
    int x = 5; // 'x' é local da main. É *diferente* do 'x' da minha_funcao.
    printf("Na main, x = %d\n", x);
    minha_funcao();
    printf("De volta na main, x = %d\n", x); // Mostra 5, o 'x' de main
    
    // Em C99/C++, variáveis podem ser declaradas no escopo do loop
    for (int i = 0; i < 3; i++) {
        printf("i = %d\n", i);
    } // 'i' é destruído aqui
    
    // printf("i = %d\n", i); // ERRO! 'i' não existe mais aqui.
    
    return 0;
} // 'x' da main é destruído aqui.
```

### Variáveis Globais

Variáveis declaradas **fora de todas as funções** são **globais**.

- **Armazenamento:** Em uma área de memória estática, frequentemente chamada de **Data Segment** (se inicializadas com valor não-zero) ou **BSS Segment** (se não inicializadas ou inicializadas com zero).
- **Escopo:** Visíveis por _todas_ as funções no arquivo (e por outros arquivos, via `extern`).
- **Tempo de Vida:** Existem durante _toda_ a execução do programa. São criadas antes da `main` começar e destruídas depois que a `main` termina.
- **Inicialização:** Se não inicializadas explicitamente, são inicializadas com **`0`** (ou `0.0`, `NULL`, etc.) por padrão.

```c
#include <stdio.h>

int contador_global = 100; // Global, vive no Data Segment
int global_nao_inicializada; // Global, vive no BSS, valor é 0

void funcao1() {
    contador_global++; // Modifica a global
    printf("Funcao 1: %d\n", contador_global);
}

void funcao2_com_shadowing() {
    // "Shadowing" ou "sombreamento"
    int contador_global = 5; // Esta é uma *nova* variável LOCAL (na pilha)
    printf("Funcao 2 (local): %d\n", contador_global); // Imprime 5
    // A global fica temporariamente oculta
}

int main() {
    printf("Global não inicializada: %d\n", global_nao_inicializada); // 0
    printf("Main (inicio): %d\n", contador_global); // 100
    funcao1(); // 101
    printf("Main (meio): %d\n", contador_global);   // 101
    funcao2_com_shadowing(); // 5
    printf("Main (fim): %d\n", contador_global);    // 101 (a global não foi afetada)
    return 0;
}
```

**Por que evitar variáveis globais?** Elas quebram o encapsulamento. É impossível saber quem modifica uma variável global sem ler o código-fonte inteiro. Isso cria um **acoplamento** forte, tornando o código frágil e difícil de depurar ("Efeitos Colaterais"). Em ambientes multithread, qualquer acesso a uma variável global é um convite a _race conditions_, exigindo sincronização complexa (mutexes).

### Parâmetros Formais

Variáveis declaradas na assinatura de uma função (`int soma(int a, int b)`) são **parâmetros formais**.

- **Armazenamento:** Na Pilha (Stack), como variáveis locais.
- **Escopo:** Locais à função.
- **Tempo de Vida:** Existem apenas durante a execução da função.
- **Inicialização:** São inicializadas com os valores (argumentos) passados quando a função é chamada.

### Especificador `static`: Permanência e Visibilidade

A palavra-chave `static` altera o _tempo de vida_ ou a _visibilidade_ (ligação).

#### `static` em Variáveis Locais

Altera o tempo de vida. A variável deixa de ser alocada na pilha e passa a ser alocada na área de memória estática (Data/BSS), como uma global.

- **Tempo de Vida:** Existe por _toda_ a execução do programa.
- **Inicialização:** É inicializada _apenas uma vez_, quando o programa começa. Se não inicializada, recebe `0`.
- **Escopo:** Permanece _local_ (visível apenas dentro da função).
- **Resultado:** Ela **retém seu valor entre as chamadas da função**.

```c
#include <stdio.h>

void contador_de_chamadas() {
    static int numero_de_chamadas = 0; // Alocada no Data Segment. Inicializada SÓ UMA VEZ.
    int var_local_auto = 0;            // Alocada na Pilha. Inicializada a CADA chamada.

    numero_de_chamadas++;
    var_local_auto++;

    printf("Static: %d, Automatica: %d\n", numero_de_chamadas, var_local_auto);
}

// Exemplo de um Gerador de ID Único
unsigned int obter_proximo_id() {
    static unsigned int proximo_id = 1; // Inicializado apenas na primeira chamada
    return proximo_id++;         // Retorna o valor atual e DEPOIS incrementa para a próxima
}

int main() {
    contador_de_chamadas(); // Saída: Static: 1, Automatica: 1
    contador_de_chamadas(); // Saída: Static: 2, Automatica: 1
    contador_de_chamadas(); // Saída: Static: 3, Automatica: 1
    
    printf("ID 1: %u\n", obter_proximo_id()); // Saída: ID 1: 1
    printf("ID 2: %u\n", obter_proximo_id()); // Saída: ID 2: 2
    return 0;
}
```

Um `static` local é a forma correta de criar um estado persistente em uma função sem poluir o escopo global.

#### `static` em Variáveis Globais (e Funções)

Altera a visibilidade (ligação). Por padrão, globais são externas (visíveis por outros arquivos `.c`). Uma global `static` tem ligação interna.

- **Resultado:** A variável (ou função) global `static` torna-se "privada" para aquele arquivo `.c`. Nenhum outro arquivo no projeto pode acessá-la. É uma forma de **encapsulamento em nível de arquivo**.

_Arquivo `A.c`:_

```c
static int segredo_do_arquivo_A = 42; // Só A.c pode ver
int publico = 10;                     // Outros arquivos podem ver

static void funcao_interna_A() { // Só A.c pode chamar
    printf("Segredo: %d\n", segredo_do_arquivo_A);
}

void funcao_publica_A() {
    funcao_interna_A();
}
```

_Arquivo `B.c` (em compilação junto com A.c):_

```c
#include <stdio.h>

extern int publico; // OK! O linkeditor vai encontrar.
// extern int segredo_do_arquivo_A; // ERRO DE LINKEDIÇÃO! 'segredo' não é visível.

void funcao_publica_A(); // Declaração da função pública de A
// void funcao_interna_A(); // ERRO DE LINKEDIÇÃO! 'funcao_interna_A' não é visível.

int main() {
    printf("Publico: %d\n", publico); // OK
    funcao_publica_A(); // OK
    return 0;
}
```

## Qualificadores de Tipo (Modificadores de Acesso)

O padrão ANSI C introduziu dois qualificadores de tipo que controlam como as variáveis podem ser acessadas: `const` e `volatile`.

### Qualificador `const`

O qualificador `const` transforma uma variável em uma **constante**. Seu valor deve ser atribuído na inicialização e **não pode ser modificado** pelo programa após sua inicialização. Tentar atribuir um novo valor a uma variável `const` resultará em um erro de compilação.

```c
const int TAMANHO_MAXIMO = 100;
const float PI = 3.14159f;

// TAMANHO_MAXIMO = 200; // ERRO DE COMPILAÇÃO!
```

O `const` é vital como um "contrato" de segurança, especialmente com ponteiros.

#### `const` com Parâmetros (Ponteiros)

Este é o uso mais importante. Ele promete que a função não modificará os dados do chamador.

```c
#include <stdio.h>
#include <string.h> 

// A função promete NÃO modificar a string original.
// 'str' é um ponteiro para um 'char' que é 'const'
void imprimir_string(const char* str) {
    // str[0] = 'X'; // ERRO DE COMPILAÇÃO! Tenta modificar dado constante.
    printf("String: %s, Comprimento: %zu\n", str, strlen(str));
}
```

#### `const` e Ponteiros

A posição do `const` muda seu significado. Uma mnemônica útil é ler a declaração da direita para a esquerda.

- **`const int * ptr;`** (ou `int const * ptr;`)
    - Lê-se: "`ptr` é um ponteiro (`*`) para um `int` que é `const`".
    - **Dado é Constante:** O valor apontado não pode ser mudado _através deste ponteiro_.
    - **Ponteiro é Variável:** O ponteiro pode ser alterado para apontar para outro lugar.
    
    ```c
    int x = 5, y = 10;
    const int* p = &x;
    // *p = 7; // ERRO! Não pode mudar o dado 'x' *através* de 'p'.
    p = &y; // OK! Pode mudar *para onde* 'p' aponta.
    x = 12; // OK! O 'x' em si não é const, só o acesso via 'p'.
    ```
    
- **`int * const ptr;`**
    - Lê-se: "`ptr` é um `const` (constante) ponteiro (`*`) para um `int`".
    - **Dado é Variável:** O valor apontado pode ser mudado.
    - **Ponteiro é Constante:** O ponteiro não pode ser alterado para apontar para outro lugar. (Deve ser inicializado na declaração).
    
    ```c
    int x = 5, y = 10;
    int* const p = &x; // Deve ser inicializado aqui
    *p = 7; // OK! 'x' agora é 7.
    // p = &y; // ERRO! Não pode mudar o ponteiro 'p'.
    ```
    
- **`const int * const ptr;`**
    - Lê-se: "`ptr` é um `const` (constante) ponteiro (`*`) para um `int` que é `const`".
    - **Dado é Constante:** O valor apontado não pode ser mudado.
    - **Ponteiro é Constante:** O ponteiro não pode ser mudado.
    
    ```c
    int x = 5, y = 10;
    const int* const p = &x;
    // *p = 7; // ERRO!
    // p = &y; // ERRO!
    ```

### Qualificador `volatile`

O `volatile` é um qualificador raro e informa ao compilador que o valor de uma variável pode ser **alterado por meios externos** ao programa. Isso **impede o compilador de aplicar otimizações** à variável.

Sem `volatile`, o compilador assume que se o código não muda uma variável, ela não muda. Ele pode, por exemplo, carregar o valor em um registrador da CPU e usá-lo várias vezes (cache). Com `volatile`, o compilador é forçado a ler o valor da memória _a cada acesso_.

Usos principais:

1. **Hardware Mapeado em Memória:** O uso mais comum em sistemas embarcados. Um registrador de hardware (ex: o status de uma porta serial) existe em um endereço de memória. Ler desse endereço é uma ação.
    
    ```c
    // Endereço (fictício) de um registrador de status de hardware
    // O hardware pode mudar o valor em 0x40001000 a qualquer momento
    volatile unsigned int * const REG_STATUS = (volatile unsigned int *) 0x40001000;
    
    // Espera até que o hardware sinalize 'pronto' (bit 0 = 1)
    while ((*REG_STATUS & 0x01) == 0) {
        // Sem 'volatile', o compilador poderia otimizar isso para:
        // 1. Ler 0x40001000 (ex: valor 0)
        // 2. while (0 == 0) {} -> Loop infinito!
        // Com 'volatile', o compilador é forçado a reler da memória 0x40001000 a cada iteração.
    }
    ```
    
2. **Variáveis de Manipuladores de Interrupção (Signal Handlers):** Uma interrupção (ex: timer) pode pausar o programa, alterar uma variável global e retomar. O programa principal deve tratar essa variável como `volatile`.
3. **Multithreading (Pré-C++11):** `volatile` era usado para comunicação entre threads, mas de forma _incorreta_. Ele **não** garante atomicidade nem ordem de memória entre CPUs (memory barriers). Em C++11 e posteriores, deve-se usar `std::atomic` para isso.

É possível usar `const` e `volatile` juntos: `const volatile tipo nome_variavel;`. Isso indica que a variável não pode ser modificada **pelo programa**, mas seu valor pode ser alterado por meios externos.

### Especificador `register`

O `register` é uma **sugestão** ao compilador para que a variável seja armazenada em um **registrador da CPU** (memória ultrarrápida) em vez da RAM.

```c
register int i; // Sugestão para o compilador
for (i = 0; i < 1000000; i++) { ... }
```

**Relevância Atual:** `register` é uma relíquia. Compiladores modernos usam algoritmos complexos (como _graph-coloring_) para decidir quais variáveis devem ir para registradores, e eles são _infinitamente_ melhores nisso do que um programador. A maioria dos compiladores modernos ignora essa palavra-chave.

Em C++11, a palavra-chave foi oficialmente depreciada (embora mantida por compatibilidade). Em C++17, seu significado original foi removido (ela não faz mais nada). Não a utilize. Uma característica é que não se pode obter o endereço de uma variável `register` (usar `&i`), pois ela pode não ter um endereço na memória RAM.

## Transição para C++: Novos Tipos e Facilidades

O C++ herda todo o sistema de tipos, escopos e modificadores do C. No entanto, ele adiciona novos tipos e sintaxes para tornar a programação mais segura e expressiva.

### Tipo `bool` (Booleano)

Enquanto o C simula booleanos com inteiros (`0` para falso, não-zero para verdadeiro), o C++ introduz o tipo `bool` nativo, que armazena as palavras-chave `true` ou `false`.

```cpp
#include <iostream>
#include <iomanip> // Para std::boolalpha

int main() {
    bool processamento_ok = true;
    bool erro_encontrado = false;

    if (processamento_ok && !erro_encontrado) {
        std::cout << "Sistema está OK!" << std::endl;
    }

    // Conversões implícitas:
    std::cout << "Valor de 'true': " << processamento_ok << std::endl; // Saída: 1
    
    int x = 5;
    bool b1 = x; // b1 se torna 'true' (qualquer não-zero é true)
    
    int y = 0;
    bool b2 = y; // b2 se torna 'false'
    
    std::cout << "b1: " << b1 << ", b2: " << b2 << std::endl; // Saída: b1: 1, b2: 0
    
    // Para imprimir "true" ou "false" textualmente:
    std::cout << std::boolalpha;
    std::cout << "b1: " << b1 << ", b2: " << b2 << std::endl; // Saída: b1: true, b2: false
    return 0;
}
```

### Tipo `std::string` (Textos)

O maior avanço em tipos básicos é a `std::string`. Em C, textos são arrays de `char` terminados em nulo (`\0`), o que é complexo, manual e fonte de inúmeros erros (ex: _buffer overflows_, _dangling pointers_). O C++ resolve isso com uma classe da biblioteca `<string>`.

|**Característica**|**C (char[])**|**C++ (std::string)**|
|---|---|---|
|**Declaração**|`char nome[50] = "Ana";`|`std::string nome = "Ana";`|
|**Memória**|Manual (tamanho fixo).|Automática (cresce e encolhe).|
|**Concatenação**|`strcat(dest, src);` (Perigoso!)|`nome3 = nome1 + " " + nome2;` (Seguro!)|
|**Obter Tamanho**|`strlen(nome);`|`nome.length();` ou `nome.size();`|
|**Comparação**|`strcmp(s1, s2) == 0`|`s1 == s2`|
|**Cópia**|`strcpy(dest, src);` (Perigoso!)|`dest = src;`|
|**Segurança**|Fonte de _Buffer Overflows_.|Gerenciamento de memória seguro.|

```cpp
#include <iostream>
#include <string> // Necessário para std::string

int main() {
    std::string saudacao = "Olá";
    std::string nome = "Estudante";
    
    // Concatenação fácil e segura
    std::string mensagem_completa = saudacao + ", " + nome + "!";
    
    std::cout << mensagem_completa << std::endl; // Saída: Olá, Estudante!
    std::cout << "Tamanho: " << mensagem_completa.length() << std::endl; // Saída: Tamanho: 16
    
    // Comparação
    if (nome == "Estudante") {
        std::cout << "O nome é Estudante." << std::endl;
    }
    
    // Substring
    std::string primeiro_nome = mensagem_completa.substr(5, 9); // Pega 9 chars a partir do índice 5
    std::cout << "Primeiro nome: " << primeiro_nome << std::endl; // Saída: Primeiro nome: Estudante
    
    return 0;
}
```

### Novas Formas de Inicialização (C++11)

O C++ oferece mais formas de inicializar variáveis, sendo a **inicialização uniforme** (com chaves `{}`) a mais recomendada.

```cpp
int x = 10;      // 1. Inicialização por cópia (estilo C)
int y(20);       // 2. Inicialização direta (estilo construtor)
int z{30};       // 3. Inicialização uniforme (C++11) - PREFERIDA
int w{};         // 4. Inicialização uniforme com valor padrão (zera w)
```

**Por que a inicialização uniforme `{}` é melhor?**

Ela é mais segura, pois proíbe "narrowing conversions" (conversões com perda de dados) que o C e outras formas de inicialização C++ permitem silenciosamente.

```cpp
double d = 10.5;
int f = 3.14f;    // Válido em C/C++, f se torna 3 (truncamento silencioso)

// int x = d;     // Válido em C/C++. x se torna 10 (truncamento silencioso)
// int y(d);      // Válido em C++. y se torna 10 (truncamento silencioso)

// int z{d};      // ERRO DE COMPILAÇÃO!
// int w{3.14f};  // ERRO DE COMPILAÇÃO!
                  // As chaves impedem a perda de dados (de double/float para int).
                  // O programador é forçado a ser explícito: int z{static_cast<int>(d)};
```

Essa segurança extra previne bugs sutis de truncamento.

### Qualificador `constexpr`

O C++ também introduz `constexpr` (C++11).

- **`const`**: Garante que uma variável é "read-only" (apenas leitura) _após_ ser inicializada. A inicialização pode ocorrer em tempo de execução.
    
    ```c
    int obter_valor() { return 20; }
    const int VALOR_CONST = obter_valor(); // OK. Inicializado em tempo de execução.
    ```
    
- **`constexpr`**: Garante que a variável ou função _pode_ ser avaliada em **tempo de compilação**. Isso permite otimizações profundas e o uso da variável em locais que exigem constantes de compilação (como tamanhos de array).

```cpp
// Esta função pode ser executada em tempo de compilação
constexpr int calcular_dobro(int n) {
    return 2 * n;
}

int main() {
    // 1. 'const' que NÃO é 'constexpr'
    int valor_runtime = 20;
    const int VALOR_CONST_RT = valor_runtime; // Inicializado em tempo de execução
    
    // 2. 'const' que É 'constexpr'
    const int VALOR_CONST_CT = 20; // Inicializado com literal, é uma constante de compilação

    // 3. 'constexpr'
    constexpr int VALOR_CEXPR = 30; // Garantido ser constante de compilação
    constexpr int VALOR_CEXPR_FUNC = calcular_dobro(15); // OK! Função é constexpr
    
    // Uso em C++: Tamanhos de array devem ser constantes de compilação
    int array1[VALOR_CONST_CT];   // OK (pois 20 é literal)
    int array2[VALOR_CEXPR];      // OK
    int array3[VALOR_CEXPR_FUNC]; // OK (calculado em tempo de compilação)
    
    // int array4[VALOR_CONST_RT]; // ERRO! VALOR_CONST_RT não é constante de compilação
    
    return 0;
}
```

Em C++, ao contrário de C (que tem VLAs - Variable Length Arrays), o tamanho de um array estático _deve_ ser conhecido em tempo de compilação. `constexpr` é a ferramenta moderna para garantir isso.

## Considerações Finais

Neste capítulo, desvendamos os conceitos essenciais de como os dados são representados e armazenados em C e C++. Vimos que um programa não manipula apenas "dados", mas sim `int`, `char`, `double`, e outros tipos rigorosamente definidos, cada um com seu tamanho, faixa e propósito.

Iniciamos definindo as regras para **identificadores**, os nomes que damos aos nossos dados. Mergulhamos no conceito de **variável**, distinguindo sua _declaração_ (anúncio) da _definição_ (criação) e da _inicialização_ (primeiro valor), e alertando sobre os perigos do lixo de memória.

Exploramos os **tipos básicos** do C e como os **modificadores** (`unsigned`, `long`) refinam suas características, com uma análise crítica dos riscos de _overflow_ e _underflow_. Introduzimos os tipos de tamanho fixo de `<stdint.h>` como a melhor prática moderna para código portátil.

Aprofundamos no conceito de **escopo** e **tempo de vida**, entendendo como a memória é gerenciada em diferentes contextos (a _Pilha_ para variáveis locais/automáticas vs. o _Segmento de Dados/BSS_ para globais e `static`). Vimos como `static` nos permite criar estado persistente em funções locais ou "privatizar" globais em um arquivo.

Analisamos os **qualificadores** `const` (e sua complexa, mas vital, interação com ponteiros), `volatile` (e seus casos de uso em hardware e interrupções) e o obsoleto `register`, entendendo como eles alteram o contrato de imutabilidade e as otimizações do compilador.

Finalmente, fizemos a transição para o C++, destacando suas melhorias de segurança e expressividade, como os tipos `bool` e `std::string`, a sintaxe de inicialização uniforme (`{}`) e o poder do `constexpr` para garantir avaliações em tempo de compilação.

Com este robusto entendimento sobre como os dados são tipificados, nomeados e armazenados, estamos prontos para explorar as operações que podemos realizar sobre eles, o que será o foco do nosso próximo capítulo sobre Operadores e Expressões.
# Capítulo 5 – Linguagens de Máquina e de Montagem

Neste capítulo, vamos explorar dois níveis fundamentais da programação de computadores: a **linguagem de máquina** e a **linguagem de montagem (ou linguagem Assembly)**. Elas representam os níveis mais baixos da hierarquia de linguagens e estão intimamente ligadas à arquitetura do processador. Ao estudar essas linguagens, compreenderemos como os computadores realmente executam instruções, como os programas são traduzidos e como o hardware interpreta comandos.

Enquanto linguagens de alto nível como Python, Java ou C++ priorizam a legibilidade e a abstração, linguagens de máquina e de montagem priorizam o **controle sobre o hardware**. Isso se dá porque essas linguagens trabalham diretamente com os recursos do processador, como registradores e unidades aritméticas.

A linguagem de máquina é composta por **códigos binários** que representam instruções diretamente compreendidas pela Unidade de Controle do processador. Já a linguagem de montagem é uma versão simbólica dessas instruções binárias, utilizando **mnemônicos** e **identificadores de registradores** para facilitar o entendimento humano.

### Linguagem de Máquina

A linguagem de máquina é a única linguagem diretamente compreendida pela Unidade Central de Processamento (CPU). Trata-se de uma sequência de **bits** (valores binários 0 e 1) que especificam operações a serem realizadas pelo processador. Por exemplo, em um processador hipotético, a instrução binária `0001 0010 0011` poderia significar “somar os valores dos registradores 2 e 3 e armazenar o resultado no registrador 1”.

Cada instrução de máquina contém geralmente dois componentes principais:

- O **código de operação (opcode)**: que indica qual operação deve ser executada (como soma, subtração, movimentação de dados etc.).
- Os **operandos**: que especificam os dados ou os endereços envolvidos na operação:
	- **Referência a operando fonte**: a operação pode envolver um ou mais operandos fontes, isto é, operandos que são entradas para a operação.
	- **Referência a operando de resultado**: a operação deve produzir um resultado.
	- **Referência à próxima instrução**: informa ao processador onde buscar a próxima instrução depois que a execução da instrução atual estiver completa.

Os **operandos fonte e resultado** podem estar em uma das quatro áreas mostradas a seguir:

- **Memória principal ou virtual**: assim como as referências à próxima instrução, o endereço da memória (principal ou virtual) deve ser fornecido.
- **Registradores do processador**: um processador tipicamente possui um ou mais registradores, os quais podem ser referenciados por instruções de máquina. Cada registrador recebe um nome ou número exclusivo, e a instrução deve conter o número do registrador desejado.
- **Imediato**: o valor do aperando está contido em um campo na instrução sendo executada.
- **Dispositivo de E/S**: a instrução precisa especificar o módulo e o dispositivo de E/S para a operação.

Os opcodes são representados por abreviações, conhecidas por mnemônicos, os quais indicam a operação. Alguns exemplos comuns são:

- `MOV`: movimentação de dados
- `ADD`: adição
- `SUB`: subtração
- `MUL`: Multiplicação
- `DIV`: Divisão
- `LOAD`: Carrega dados da memória
- `STOR`: Armazena dados na memória

Operandos também são representados simbolicamente, como por exemplo a instrução `ADD R, X`, que pode significar a soma do valor contido na localização `X` com o conteúdo do registrador `R`.

Cada família de processadores (como Intel, AMD, ARM) possui um **conjunto de instruções** próprio, conhecido como **ISA – Instruction Set Architecture**. Essas instruções variam em formato e tamanho dependendo da arquitetura do processador. Por isso, dizemos que a linguagem de máquina é **específica de cada arquitetura**. 

É difícil para humanos escreverem ou depurarem programas usando diretamente linguagem de máquina. Por isso, desenvolveu-se uma representação mais amigável: **a linguagem de montagem**.

#### Modos de Endereçamento

O(s) campo(s) de endereço são relativamente pequenos. Para tornar possível referenciar um grande intervalo de locais da memória principal (ou memória virtual, em alguns sistemas), uma variedade de técnicas de endereçamento foi empregada. Vamos analisar as técnicas ou modos de endereçamento mais comuns adotando a seguinte notação:

- **A (do inglês, Address)**: conteúdo de um campo de endereço dentro da instrução.
- **R**: conteúdo de um campo de endereço dentro da instrução que se refere a um registrador.
- **EA (do inglês, Effective Address)**: endereço real (efetivo) do local que contém o operando referenciado.
- **(X)**: conteúdos do local de memória X ou do registrador X.

**Endereçamento imediato**: é a forma mais simples de endereçamento, no qual o valor do operando está presente na instrução:

```asm
Operando = VALOR
```

Esse modo pode ser utilizado para definir e utilizar constantes ou definir valores iniciais das variáveis. A vantagem é que nenhuma referência de memória (além de obter a instrução em si) é necessária para obter o operando. Isso economiza um ciclo de memória ou de cache dentro do ciclo de instrução. A desvantagem é que o tamanho do número é limitado ao tamanho do campo de endereço (geralmente pequeno, se comparado ao tamanho da palavra, que é o tamanho utilizado quando se busca um dado da memória).

Um exemplo que utiliza o modo de endereçamento imediato para mover o valor 20 (em hexadecimal) para o registrador B: 

```asm
MOV B, #20H
```

**Endereçamento direto**: o campo de endereço possui o endereço efetivo do operando:

```
EA = A
```

Essa técnica era comum nas primeiras gerações de computadores, requer apenas uma referência à memória e nenhum cálculo especial. A limitação é que ela oferece um espaço de endereçamento limitado.

**Endereçamento indireto**: no endereçamento direto, o tamanho do campo de endereço geralmente é menor do que o tamanho da palavra, o que limita o espaço de endereços. A solução é ter um campo de endereço fazendo referência ao endereço de uma palavra na memória, a qual possui o endereço completo do operando:

```
EA = (A)
```

Aqui, os parênteses são interpretados como "conteúdo de". A vantagem principal é que, para um tamanho N de uma palavra, um espaço de endereçamento de 2N ficará disponível. A desvantagem é que a execução da instrução requer duas referências à memória para obter o operando, uma para obter apenas o endereço e a outra para obter o operando (valor) em si.

**Endereçamento por registradores**: semelhante ao endereçamento direto, porém o campo de endereço faz referência a um registrador em vez de um endereço de memória:

```
EA = R
```

Por exemplo, se o conteúdo de um campo de endereço de registrador for 3, então o registrador R3 é o endereço pretendido e o valor do operando estará em R3. As vantagens são que apenas um pequeno campo de endereço é necessário e nenhuma referência é feita à memória para buscar o operando. A desvantagem é o espaço de endereçamento muito limitado, afinal não existem tantos registradores, se comparado à memória principal.

**Endereçamento indireto por registradores**: análogo ao endereçamento indireto, sendo que a diferença é que no lugar de referência à memória, existe referência a um registrador:

```
EA = (R)
```

As vantagens e desvantagens são basicamente as mesmas do endereçamento indireto. O endereçamento indireto por registradores utiliza uma referência à memória a menos do que o endereçamento indireto.

**Endereçamento por deslocamento**: combina as capacidades do endereçamento direto e do endereçamento indireto por registradores:

```
EA = A + (R)
```

Esse modo de endereçamento requer que a instrução tenha dois campos de endereço, dos quais ao menos um seja explícito. O valor contido em um campo de endereço (valor = A) é utilizado diretamente e o outro campo de endereço refere-se a um registrador cujos conteúdos são adicionados a A para produzir um endereço efetivo. Vamos ver três dos usos mais comuns a seguir:

- **Endereçamento relativo**: o endereçamento é relativo ao registrador PC (Program Counter), ou seja, o endereço da próxima instrução é adicionado ao campo de endereço para produzir EA. Em geral, o campo de endereço é tratado como como um número complementar para essa operação. Assim, o endereço efetivo é o deslocamento relativo ao endereço da instrução.
- **Endereçamento por registrador base**: o registrador base contém um endereço da memória principal e o campo de endereço contém um deslocamento desse endereço (geralmente um inteiro sem sinal).
- **Indexação**: o campo de endereço faz referência a um endereço da memória principal e o registrador referenciado contém um deslocamento positivo desse endereço.

**Endereçamento de pilha**: itens são adicionados e retirados do topo da pilha, sendo que há um ponteiro cujo valor é o endereço do topo. O ponteiro da pilha é mantido em um registrador. O modo de endereçamento de pilha é uma forma de endereçamento implícito, sendo que as instruções de máquina não necessitam incluir uma referência de memória, devem apenas operar no topo da pilha.

### Linguagem de Montagem (Assembly)

A linguagem de montagem, também conhecida como Assembly, é uma **representação simbólica** da linguagem de máquina. Em vez de trabalhar com códigos binários, o programador usa **mnemônicos** para representar instruções e **nomes simbólicos** para representar registradores e endereços de memória. Fazendo-se uma comparação:

|Linguagem de Máquina|Assembly|Significado|
|---|---|---|
|`10110000 01100001`|`MOV AL, 61h`|Move o valor 61h para o registrador AL|

Ao utilizar uma linguagem de montagem, o programador consegue operar com mais clareza e controle do que com linguagens de alto nível, ao mesmo tempo em que mantém uma proximidade com o funcionamento real do hardware.

Cada instrução em Assembly possui um **mnemônico** que representa a operação. Os registradores também são nomeados. 

Os **registradores** são pequenas áreas de memória interna à CPU, extremamente rápidas, utilizadas para armazenar dados temporários e controlar o fluxo de execução de programas. Exemplos de registradores:

- `AX`, `BX`, `CX`, `DX`: registradores principais
- `SI`, `DI`, `SP`, `BP`: registradores de índice e pilha
- `EAX`, `EBX`, etc.: versões de 32 bits (nas arquiteturas modernas)

Cada processador possui um conjunto definido de registradores, e cada registrador tem um propósito específico (embora em assembly seja possível usá-los de forma mais flexível, dependendo da arquitetura).

Um exemplo prático na arquitetura x86:

```asm
MOV AX, 0005h   ; armazena o valor hexadecimal 0005 em AX
MOV BX, 0003h   ; armazena o valor hexadecimal 0003 em BX
ADD AX, BX      ; soma AX + BX → AX = 0008h
```

#### Montadores (Assemblers)

O código escrito em assembly não pode ser executado diretamente pela CPU. Ele precisa ser traduzido para linguagem de máquina. Esse processo é feito por um programa chamado **montador**, ou **assembler**.

O assembler converte cada instrução simbólica em seu equivalente binário (código de máquina). Por exemplo, a instrução `MOV AL, 02h` será traduzida para `10110000 00000010`. Isso é diferente de compiladores (que traduzem linguagens de alto nível para baixo nível) ou interpretadores (que executam diretamente instruções de alto nível).

Montadores também permitem o uso de **diretivas**, como `ORG`, `END`, `DATA`, entre outras, que ajudam a organizar o código.

#### Estrutura de um Programa em Assembly

Em geral, cada instrução da linguagem de montagem é traduzida em uma instrução de máquina pelo montador **(1:1)**. É uma linguagem **dependente do hardware**, ou seja, há uma linguagem de montagem diferente para cada tipo de processador. Um programador experiente em Assembly para a arquitetura RISC terá que aprender muita coisa para programar em Assembly para CISC, por exemplo. Os quatro elementos da linguagem de montagem são:

**Comentário (opcional)**: pode ser colocado no lado direito de um comando ou pode ocupar uma linha inteira. Em geral, o caractere especial que especifica que a partir dali trata-se de um comentário é o ponto e vírgula `;`.

**Rótulo (opcional)**: se estiver presente, o montador define o rótulo como equivalente ao endereço no qual o primeiro byte do código objeto gerado para essa instrução será carregado. O programador pode usar o rótulo como um endereço ou como dado no campo de endereço de outra instrução. Os rótulos são utilizados com mais frequência em instruções de desvio. Abaixo podemos ver um exemplo (rótulo foi denominado `L1` e os comentários são colocados após o ponto e vírgula).

```asm
L1: SUB EAX, EDX   ; subtrai o conteúdo do reg EDX do conteúdo de EAX
				   ;  e armazena o result em EAX
	JG L1          ; salta para L1 se o resultado da subtração for positivo
```

**Operando(s)**: uma sentença de linguagem de montagem inclui zero ou mais operandos. Cada operando identifica um valor imediato, um registrador ou uma posição de memória. Para endereçamento por registrador, o nome do registrador é usado, ex.: `MOV ECX, EBX`. O endereçamento imediato indica que o valor é codificado dentro da instrução, ex.: `MOV EAX, 100H`. O endereçamento direto refere-se a uma posição de memória e é expresso como um deslocamento a partir do registrador de segmento `DS`. Exemplo:

```asm
MOV AX, 1234H    ; AX <- 1234 (hexadecimal)
MOV [3420H], AX  ; conteúdo de AX é movido para o endereço lógico DS:3420H
```

**Mnemônico**: nome da operação ou função da sentença da linguagem de montagem. Uma sentença pode corresponder a uma instrução de máquina, uma diretiva do montador ou uma macro. No caso de uma instrução de máquina, um mnemônico é o nome simbólico associado com um determinado **opcode**. Vamos ver alguns mnemônicos, mas antes é importante conhecer os registradores de uso geral (arquitetura x86, a mais conhecida). Os oito registradores de uso geral são:

- `EAX`: acumulador, usado em operações aritméticas;
- `ECX`: contador, usado em loops;
- `EDX`: registrador de dados, usado em operações de entrada/saída e em multiplicações e divisões. É também uma extensão do acumulador;
- `EBX`: base, usado para apontar para dados no segmento `DS` (data segment);
- `ESP`: apontador da pilha (Stack Pointer), aponta para o topo da pilha (endereço mais baixo dos elementos da pilha);
- `EBP`: apontador da base do frame, usado para acessar argumentos de procedimentos passados pela pilha;
- `ESI`: índice da fonte de dados a copiar (Source Index), aponta para dados a copiar para DS:EDI;
- `EDI`: índice do destino de dados a copiar (Destination Index), aponta para o destino dos dados a copiar de `DS:ESI`.

Esses oito registradores possuem tamanho de 32 bits são "estendidos". Os 16 bits de ordem mais baixa de cada um dos registradores podem ser acessados através das versões não estendidas. As versões de 16 bits possuem os mesmos nomes que versões de 32 bits, com exceção de a letra `E` ser retirada (ex: `EAX` → `AX`). As versões estendidas dos registradores não existem em gerações anteriores à 80386 (primeira geração de processadores 32 bits da arquitetura x86).

As versões não estendidas dos quatro primeiros registradores de uso geral dividem-se ainda em dois grupos de 8 bits cada um. O octeto (byte) de ordem mais alta é acessado trocando o `X` por um `H` (exemplo: `AX` → `AH`), e o octeto de ordem mais baixa trocando o `X` por um `L` (ex.: `AX` → `AL`). Fica fácil lembrar quando sabemos que `H` = High (Alta) e `L` = Low (Baixa).

Exemplos de alguns dos mnemônicos mais conhecidos e seus significados::

- `MOV`: movimentação de dados
- `ADD`: adição
- `SUB`: subtração
- `MUL`: multiplicação
- `DIV`: divisão
- `CMP`: comparação
- `JMP`: salto incondicional
- `JE`, `JNE`: saltos condicionais
- `PUSH`: empilha;
- `POP`: desempilha;
- `INT`: interrupção
- `CALL`: chamada
- `LOAD`: Carrega dados da memória;
- `STOR`: Armazena dados na memória.

Alguns exemplos de como podem ser utilizados:

```asm
ADD AL, BL ; AL <- AL + BL (AL e BL são os registradores com valores)
SUB AL, BL ; AL <- AL - BL
MOV AL, 1  ; AL = 1 (move 1 para AL)
```

Um programa típico em Assembly possui as seguintes seções:

- **`.data`**: onde os dados e variáveis são definidos.
- **`.text`**: onde fica o código executável.
- **`_start ou main`**: ponto de entrada do programa.

Exemplo completo (Linux, NASM):

```asm
section .data
    msg db "Olá, mundo!", 0xA ; mensagem com quebra de linha
    len equ $ - msg           ; calcula o tamanho da mensagem

section .text
    global _start

_start:
    ; chamada do sistema para escrita (sys_write)
    mov eax, 4        ; código da chamada: sys_write
    mov ebx, 1        ; saída padrão (stdout)
    mov ecx, msg      ; endereço da mensagem
    mov edx, len      ; tamanho da mensagem
    int 0x80          ; interrupção para executar a chamada

    ; chamada para encerrar o programa (sys_exit)
    mov eax, 1
    xor ebx, ebx
    int 0x80
```

#### Vantagens e Desvantagens do Assembly 

As principais vantagens da Linguagem de Montagem são:

- Controle absoluto sobre o hardware
- Excelente desempenho em aplicações críticas
- Ideal para sistemas embarcados e drivers
- Possibilidade de otimizações específicas

Quanto as desvantagens:

- Curva de aprendizado acentuada
- Excelente desempenho em aplicações críticas
- Ideal para sistemas embarcados e drivers
- Possibilidade de otimizações específicas

Apesar de ser rara em aplicações modernas, a linguagem assembly ainda é utilizada em contextos como:

- Desenvolvimento de **sistemas operacionais**;
- Criação de **firmwares** e **sistemas embarcados**;
- Programas de **baixa latência** ou **alta performance**;
- **Depuração e engenharia reversa**;
- Escrita de **rotinas críticas** em bibliotecas matemáticas.

### Considerações Finais

Este capítulo nos levou ao coração da execução computacional, revelando a mecânica por trás de cada operação de software. Ao dissecarmos a **linguagem de máquina**, entendemos como o hardware interpreta comandos em sua forma mais pura: sequências binárias de opcodes e operandos. Vimos que, embora poderosa, essa linguagem é impraticável para o desenvolvimento humano. É nesse ponto que a **linguagem de montagem (Assembly)** demonstra seu valor inestimável, servindo como a camada de abstração mais fina e essencial, que traduz a lógica binária para mnemônicos compreensíveis, nos dando controle absoluto sobre o processador.

Embora a maioria dos desenvolvedores modernos opere nas camadas de abstração superiores, o conhecimento adquirido aqui transcende a mera curiosidade acadêmica. Compreender como funcionam os registradores, os modos de endereçamento e o ciclo de instrução é o que diferencia um programador de um verdadeiro cientista da computação. Esse saber permite otimizar códigos de forma crítica, desenvolver software para sistemas embarcados, garantir a segurança em baixo nível e depurar problemas que são invisíveis para as linguagens de alto nível. Em suma, dominamos a base sobre a qual todos os outros softwares são construídos.
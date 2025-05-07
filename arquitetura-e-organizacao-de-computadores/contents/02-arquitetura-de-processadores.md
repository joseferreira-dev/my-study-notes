# Capítulo 2 – Arquiteturas de Processadores: RISC e CISC

## Introdução às Arquiteturas de Processadores

A arquitetura de um processador define a forma como ele interpreta e executa instruções, sendo um dos aspectos mais fundamentais para o desempenho e a eficiência de sistemas computacionais. Ao longo da história da computação, duas arquiteturas principais se destacaram: a **CISC (Complex Instruction Set Computer)** e a **RISC (Reduced Instruction Set Computer)**. Cada uma dessas abordagens traz consigo uma filosofia distinta de projeto, com vantagens e desvantagens específicas. Compreender essas diferenças é essencial para entender como os processadores modernos funcionam e como evoluíram ao longo do tempo.

## Arquitetura CISC: Complexidade a Serviço da Versatilidade

A arquitetura CISC surgiu com o objetivo de aproximar as instruções de máquina do nível de linguagens de alto nível. Em um processador CISC, cada instrução é projetada para executar uma tarefa complexa, muitas vezes correspondendo diretamente a uma instrução escrita em linguagens como C ou Pascal. Isso torna a codificação de programas mais compacta e eficiente do ponto de vista do número de instruções, uma vez que uma única instrução de máquina pode realizar múltiplas operações.

Um processador CISC típico, como o Intel 386 ou o 486, é capaz de executar centenas de instruções diferentes. Essas instruções variam em complexidade e podem acessar diretamente a memória, manipular dados, realizar cálculos e controlar o fluxo do programa. Essa versatilidade, porém, vem acompanhada de um custo em termos de complexidade de hardware. Para implementar instruções complexas, os processadores CISC utilizam uma técnica conhecida como microprogramação.

A microprogramação permite que cada instrução de máquina seja implementada como uma sequência de microinstruções armazenadas em uma memória especial chamada memória de controle. Essa memória, geralmente uma ROM interna ao processador, contém o microcódigo responsável por controlar os diversos circuitos do processador. Essa abordagem facilita a criação e modificação de instruções, pois basta adicionar ou alterar entradas na memória de controle, sem necessidade de redesenhar o circuito lógico.

Essa característica permitiu a criação de **famílias de processadores**. Por exemplo, a arquitetura x86 evoluiu do 386 para o 486, depois para o Pentium, Pentium MMX e assim por diante, sempre mantendo compatibilidade com as instruções anteriores e incorporando novas instruções de forma incremental.

De forma geral, as principais características da arquitetura CISC são:

- **Grande conjunto de instruções**: Inclui instruções especializadas para diversas tarefas.
- **Múltiplos modos de endereçamento**: Como direto, indireto, indexado, base, relativo, entre outros.
- **Instruções com formato variável**: O tamanho das instruções pode variar, por exemplo, de 1 a 12 bytes no Intel 486.
- **Uso de microcódigo**: A lógica de controle é frequentemente baseada em microprogramação.
- **Poucos registradores de uso geral**: Devido ao espaço necessário para o microcódigo e outros circuitos.
- **Instruções de máquina de “alto nível”**: complexidade semelhante à dos comandos de alto nível.
- **Acesso direto à memória nas instruções**: Como MOV AX, [mem], ADD [mem], BX (formato de 2 operandos é o mais comum).
- **Instruções que requerem múltiplos ciclos de relógio**: Cada instrução pode demandar vários ciclos, principalmente quando envolve acesso à memória (por exemplo, se existe a busca de dois operandos na memória, demora mais).
- **Presença de registradores especializados**: Como registradores de controle (flags), de segmento, ponteiro de pilha, entre outros.

Um exemplo de instrução fictícia nessa arquitetura seria:

```asm
ADD AX, [BX+SI]
```

Essa instrução, comum em arquiteturas CISC, soma o conteúdo da posição de memória referenciada por `BX+SI` ao registrador `AX`. Ela realiza, em uma única instrução, múltiplas operações: cálculo do endereço, acesso à memória, leitura do operando e soma. Embora seja compacta, essa instrução pode levar vários ciclos para ser executada.

## Arquitetura RISC: Simplicidade com Alto Desempenho

Em oposição ao modelo CISC, surgiu a arquitetura RISC, cuja filosofia é simplificar o conjunto de instruções e delegar a complexidade para o compilador. A ideia central é que a maior parte dos programas utiliza apenas uma fração pequena do conjunto total de instruções disponíveis em processadores CISC. Assim, ao limitar o número de instruções e padronizá-las em formato e tempo de execução, os processadores RISC conseguem obter maior desempenho e menor complexidade interna.

Os processadores RISC, como o MIPS, o SPARC, o Power e o Alpha, priorizam instruções simples, de execução rápida e com formatos fixos. Com isso, conseguem operar com maior frequência de relógio, implementar pipelining de forma mais eficiente e utilizar mais registradores internos para evitar acessos constantes à memória.

De forma geral, as principais características da arquitetura RISC são:

- **Conjunto reduzido de instruções**: Menor variedade, focando nas instruções mais comuns.
- **Instruções com largura fixa**: Por exemplo, todas com 4 bytes, o que facilita a decodificação e a previsão de instruções.
- **Execução rápida (uma instrução por ciclo de relógio)**: Ideal para pipelining.
- **Maior número de registradores de uso geral**: Evita acessos frequentes à memória principal.
- **Arquitetura registrador–registrador**: Apenas instruções do tipo LOAD e STORE acessam a memória; todas as outras operam entre registradores.
- **Poucos modos de endereçamento**: Simplifica o hardware e facilita a decodificação.
- **Ausência de microcódigo (resultando na sobra de mais espaço no chip)**: As instruções são implementadas diretamente por lógica de hardware.

Como exemplo prático desta arquitetura, considere a operação de soma entre dois valores na memória. Em uma arquitetura RISC, isso seria feito em três etapas:

```asm
LOAD R1, 0(R2)     ; Carrega o valor da memória apontada por R2 para R1
LOAD R3, 4(R2)     ; Carrega o valor da memória seguinte para R3
ADD R4, R1, R3     ; Soma os valores em R1 e R3 e armazena em R4
```

Cada uma dessas instruções é simples e rápida, permitindo execução em pipeline sem interferência.

## Comparação entre RISC e CISC

Para consolidar a compreensão, vejamos a seguir uma comparação entre diferentes processadores quanto às características-chave que ajudam a classificá-los como RISC ou CISC:

|Características|MIPS R4000|RS/6000|VAX11/780|Intel 486|
|---|---|---|---|---|
|Quantidade de instruções|94|183|303|235|
|Modos de endereçamento|1|4|22|11|
|Largura de instruções (bytes)|4|4|2–57|1–12|
|Registradores de uso geral|32|32|16|8|

Ao analisar essa tabela, é possível observar que os processadores **MIPS R4000** e **RS/6000** exibem características típicas da arquitetura **RISC**: poucas instruções, largura fixa, poucos modos de endereçamento e muitos registradores. Por outro lado, os processadores **VAX11/780** e **Intel 486** possuem conjuntos de instruções mais amplos, modos variados de endereçamento, instruções de tamanho variável e menos registradores, o que é característico da arquitetura **CISC**.

## Processadores Híbridos: O Melhor dos Dois Mundos

Na prática, a distinção entre RISC e CISC tornou-se cada vez menos rígida. Com o avanço da tecnologia e a busca incessante por desempenho, muitos fabricantes passaram a incorporar características de ambas as arquiteturas em seus projetos. Isso deu origem aos **processadores híbridos**.

Por exemplo, os processadores modernos da família x86, como o Pentium II, Pentium III e o AMD Athlon, embora sejam essencialmente CISC, adotam técnicas típicas de RISC internamente, como execução em pipeline e decodificação para micro-operações simples. Da mesma forma, alguns processadores tradicionalmente RISC, como o MIPS R10000 e o HP PA-8000, passaram a adotar instruções mais complexas e modos adicionais de endereçamento.

Essa convergência ocorreu por uma razão simples: **desempenho**. O que importa, no final das contas, é o tempo total de execução de uma tarefa, e não o número de instruções ou o tipo de arquitetura. Assim, a escolha entre RISC e CISC deixou de ser uma dicotomia técnica para se tornar uma estratégia de engenharia.

## Considerações Finais

Compreender as arquiteturas RISC e CISC é essencial para entender a evolução dos processadores e suas implicações no desempenho dos sistemas. Enquanto a CISC oferece versatilidade e compactação de código, a RISC foca na simplicidade, velocidade e paralelismo. No entanto, no mundo real, poucos processadores seguem estritamente uma ou outra abordagem. O que encontramos são soluções híbridas, cuidadosamente projetadas para balancear complexidade, custo e desempenho.

No contexto acadêmico e em concursos, é importante saber distinguir claramente as características teóricas de cada arquitetura. Na prática, entretanto, a escolha entre RISC e CISC depende de múltiplos fatores, como aplicação, consumo de energia, custo de fabricação e requisitos de desempenho.

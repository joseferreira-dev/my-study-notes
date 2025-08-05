# Capítulo 2 – Arquiteturas de Processadores: RISC e CISC

A arquitetura de um processador define a forma como ele interpreta e executa instruções, sendo um dos aspectos mais fundamentais para o desempenho e a eficiência de sistemas computacionais. Ao longo da história da computação, uma polêmica comum girou em torno de qual seria a melhor abordagem de projeto. Muitos, por exemplo, defendiam que os computadores "Macs" eram mais rápidos por terem chips com uma filosofia de projeto específica. Essa discussão nos leva diretamente às duas arquiteturas que se destacaram: a **CISC (Complex Instruction Set Computer)** e a **RISC (Reduced Instruction Set Computer)**. Cada uma dessas abordagens traz consigo uma filosofia distinta, com vantagens e desvantagens específicas. Compreender essas diferenças é essencial para entender como os processadores modernos funcionam e como evoluíram ao longo do tempo.

## Arquitetura CISC: Complexidade a Serviço da Versatilidade

A arquitetura **CISC (Complex Instruction Set Computer)**, ou Computador com um Conjunto Complexo, de Instruções surgiu com o objetivo de aproximar as instruções de máquina do nível de linguagens de alto nível. Um processador CISC é capaz de executar várias centenas de instruções complexas diferentes, sendo extremamente versátil. A ideia é que cada instrução seja projetada para executar uma tarefa complexa, muitas vezes correspondendo diretamente a uma instrução escrita em linguagens como C ou Pascal. Isso torna a codificação de programas mais compacta, uma vez que uma única instrução de máquina pode realizar múltiplas operações.

Um processador CISC típico, como o Intel 386 ou o 486, é capaz de executar centenas de instruções que variam em complexidade, podendo acessar diretamente a memória, manipular dados e controlar o fluxo do programa. Essa versatilidade, porém, vem acompanhada de um custo em termos de complexidade de hardware. Para implementar instruções tão complexas, os processadores CISC utilizam uma técnica conhecida como **microprogramação**.

A microprogramação permite que cada instrução de máquina seja implementada como uma sequência de microinstruções armazenadas em uma memória especial chamada memória de controle. Essa memória, geralmente uma ROM interna à Unidade de Controle do processador, contém o microcódigo responsável por controlar os diversos circuitos do chip. Essa abordagem facilita a criação e modificação de instruções, pois basta adicionar ou alterar entradas na memória de controle, sem a necessidade de redesenhar o circuito lógico do processador.

Essa característica permitiu a criação de **famílias de processadores**. Por exemplo, a arquitetura x86 evoluiu do 386 para o 486, depois para o Pentium, Pentium MMX e assim por diante, sempre mantendo compatibilidade com as instruções anteriores e incorporando novas instruções de forma incremental, sem precisar começar um projeto do zero.

De forma geral, as principais características da arquitetura CISC são:

- **Grande conjunto de instruções**: Inclui centenas de instruções especializadas para diversas tarefas.
- **Múltiplos modos de endereçamento**: Permite acessar operandos de várias formas, como direto, indireto, indexado, base, relativo, entre outros.
- **Instruções com formato variável**: O tamanho das instruções pode variar significativamente, por exemplo, de 1 a 12 bytes no Intel 486.
- **Uso de microcódigo**: A lógica de controle é frequentemente baseada em microprogramação, com o microcódigo residindo em uma memória de controle interna.
- **Poucos registradores de uso geral**: Devido ao espaço no chip ser ocupado pela complexa lógica de decodificação e pela memória de microcódigo.
- **Instruções de máquina de “alto nível”**: A complexidade de uma única instrução de máquina se assemelha à de um comando em uma linguagem de alto nível.
- **Acesso direto à memória nas instruções**: Operações aritméticas e lógicas podem ter operandos localizados diretamente na memória principal. O formato de dois operandos (ex: registrador-memória) é muito comum.
- **Instruções que requerem múltiplos ciclos de relógio**: Cada instrução pode demandar vários ciclos para ser concluída, principalmente quando envolve acesso à memória. Por exemplo, uma instrução que busca dois operandos na memória levará mais tempo.
- **Presença de registradores especializados**: Além dos registradores de uso geral, existem registradores para funções específicas, como registradores de controle (flags), de segmento e ponteiro de pilha.

Um exemplo de instrução fictícia nessa arquitetura seria:

```asm
ADD AX, [BX+SI]
```

Essa instrução, comum em arquiteturas CISC, soma o conteúdo da posição de memória referenciada por `BX+SI` ao registrador `AX`. Ela realiza, em uma única linha, múltiplas micro-operações: cálculo do endereço de memória, acesso à memória, leitura do operando e, finalmente, a soma. Embora seja compacta, essa instrução pode levar vários ciclos para ser executada.

## Arquitetura RISC: Simplicidade com Alto Desempenho

Em oposição ao modelo CISC, alguns fabricantes decidiram seguir o caminho contrário, criando o padrão **RISC (Reduced Instruction Set Computer)**, ou Computador com um Conjunto Reduzido de Instruções. A filosofia é simplificar drasticamente o conjunto de instruções e delegar a complexidade para o compilador. A ideia central é que a maior parte dos programas utiliza apenas uma fração pequena do vasto conjunto de instruções disponíveis em processadores CISC, tornando o excesso um desperdício.

Surge então a dúvida: como um chip que executa poucas instruções pode ser mais rápido que outro que executa centenas delas? A resposta está na eficiência: um processador RISC é capaz de executar suas poucas instruções de forma muito mais rápida. Ao limitar o número de instruções e padronizá-las em formato e tempo de execução, os processadores RISC conseguem obter maior desempenho, menor complexidade interna e, consequentemente, são mais simples e baratos de fabricar.

Processadores RISC, como o MIPS, SPARC, Power e Alpha, priorizam instruções simples, de execução rápida e com formatos fixos. Com isso, conseguem operar com **frequências de relógio mais altas**, pois possuem menos circuitos internos. Além disso, implementam **pipelining** de forma muito mais eficiente e utilizam um número maior de registradores para evitar acessos constantes à memória.

De forma geral, as principais características da arquitetura RISC são:

- **Conjunto reduzido de instruções**: Menor variedade, focando nas instruções mais comuns e simples.
- **Instruções com largura fixa**: Todas as instruções têm o mesmo tamanho (ex: 4 bytes), o que facilita e acelera a decodificação e o uso de pipelining.
- **Execução rápida (uma instrução por ciclo de relógio)**: O objetivo é que cada instrução seja simples o suficiente para ser executada em um único ciclo de clock, ideal para pipelining.
- **Maior número de registradores de uso geral**: Como não há microcódigo, sobra mais espaço no chip, que é utilizado para incluir mais registradores. Isso minimiza os acessos à memória principal, que é mais lenta.
- **Arquitetura registrador–registrador**: Apenas as instruções especiais **LOAD** e **STORE** acessam a memória. Todas as outras operações (aritméticas, lógicas) ocorrem exclusivamente entre registradores.
- **Poucos modos de endereçamento**: A simplicidade nos modos de endereçamento facilita o projeto do hardware e a decodificação de instruções.
- **Ausência de microcódigo (resultando na sobra de mais espaço no chip)**: As instruções são implementadas diretamente por lógica de hardware (hardwired), o que as torna mais rápidas.

Como exemplo prático desta arquitetura, a mesma operação de soma entre dois valores na memória seria decomposta em três etapas simples:

```asm
LOAD R1, 0(R2)  ; Carrega o valor da memória apontada por R2 para o registrador R1
LOAD R3, 4(R2)  ; Carrega o valor da posição de memória seguinte para R3
ADD R4, R1, R3  ; Soma os valores dos registradores R1 e R3 e armazena em R4
```

Cada uma dessas instruções é simples e rápida, permitindo que sejam executadas em um pipeline de forma eficiente e sem conflitos.

## Comparação entre RISC e CISC

Para consolidar a compreensão, a tabela a seguir compara processadores históricos com base em características-chave que ajudam a classificá-los como RISC ou CISC:

| Características               | MIPS R4000 | RS/6000 | VAX11/780 | Intel 486 |
| ----------------------------- | ---------- | ------- | --------- | --------- |
| Quantidade de instruções      | 94         | 183     | 303       | 235       |
| Modos de endereçamento        | 1          | 4       | 22        | 11        |
| Largura de instruções (bytes) | 4          | 4       | 2–57      | 1–12      |
| Registradores de uso geral    | 32         | 32      | 16        | 8         |

Ao analisar essa tabela, a classificação se torna evidente:

- **Pela quantidade de instruções**, o MIPS R4000 com 94 parece ser RISC, enquanto o VAX com 303 parece ser CISC.
- **Analisando os modos de endereçamento**, fica claro que os dois primeiros são RISC (1 e 4 modos), enquanto os dois últimos são CISC (22 e 11 modos).
- **Pela largura das instruções**, a distinção é ainda mais nítida. Os dois primeiros são RISC, com largura fixa de 4 bytes, enquanto os outros possuem instruções de tamanho variável.
- **Para finalizar a análise**, os dois primeiros processadores, que suspeitamos serem RISC, possuem mais registradores de uso geral (32 cada), enquanto os outros dois possuem significativamente menos (16 e 8).

A conclusão é que os processadores **MIPS R4000** e **RS/6000** possuem arquitetura **RISC**, e os processadores **VAX11/780** e **Intel 486** possuem arquitetura **CISC**.

## Processadores Híbridos: O Melhor dos Dois Mundos

Na prática, a distinção rigorosa entre RISC e CISC tornou-se cada vez menos rígida. A busca incessante por desempenho levou os fabricantes a incorporar o melhor de cada filosofia, dando origem aos **processadores híbridos**.

Por exemplo, os processadores modernos da família x86 (essencialmente CISC), como o Pentium II, Pentium III e o AMD Athlon, adotam internamente técnicas típicas de RISC. Eles decodificam as instruções complexas CISC em sequências de micro-operações mais simples, similares a instruções RISC, que podem então ser executadas de forma otimizada em pipelines avançados. Da mesma forma, alguns processadores tradicionalmente RISC, como o MIPS R10000 e o HP PA-8000, passaram a adotar instruções mais complexas para otimizar tarefas específicas.

Essa convergência ocorreu por uma razão simples: **desempenho**. O que importa, no final das contas, é o tempo total de execução de uma tarefa, e não a pureza da arquitetura. Assim, o que no mundo real encontramos são processadores com características de ambos os tipos, chamados de híbridos.

## Considerações Finais

Compreender as arquiteturas RISC e CISC é fundamental para entender a evolução dos processadores e suas implicações no desempenho. Enquanto a CISC oferece versatilidade e compactação de código, a RISC foca na simplicidade, velocidade e execução paralela. No entanto, no mundo real, a linha que as separa é tênue.

É importante notar a diferença entre a teoria e a prática. Para fins acadêmicos e em concursos, é crucial saber diferenciar claramente as características teóricas de cada arquitetura. Na prática, porém, a indústria de processadores se move em direção a soluções híbridas, cuidadosamente projetadas para balancear complexidade, custo e, acima de tudo, o máximo desempenho possível para as aplicações do dia a dia.

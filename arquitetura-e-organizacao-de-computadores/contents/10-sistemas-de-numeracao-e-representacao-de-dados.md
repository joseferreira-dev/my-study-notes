# Capítulo 10 – Sistemas de Numeração, Aritmética Binária e Representação de Dados

## Sistemas de Numeração

O mundo em que vivemos é, sem dúvida, moldado pelo sistema decimal. Afinal, desde os primeiros registros da civilização, aprendemos a contar utilizando os dez dedos das mãos, o que, muito provavelmente, foi o fator que levou à adoção natural da **base 10**. Assim, crescemos lidando com os números de 0 a 9, sem sequer imaginar que poderiam existir outros sistemas numéricos. No entanto, quando migramos para o universo dos computadores, essa lógica muda drasticamente. As máquinas não entendem dez dígitos, mas sim dois: **0 e 1**.

Este é o ponto de partida para compreendermos os **sistemas de numeração**, que são conjuntos de símbolos e regras que permitem representar quantidades, informações e dados.

### Sistema Decimal: Nossa Linguagem Numérica Natural

No sistema decimal, cada número é composto por uma sequência de dígitos que varia de 0 a 9. Cada posição desses dígitos possui um peso, determinado por uma potência de 10 — que é justamente a **base do sistema decimal**.

Por exemplo, quando olhamos para o número **95**, entendemos que ele representa:

$$(9 \times 10^1) + (5 \times 10^0) = 90 + 5 = 95$$

Se aumentarmos a quantidade de dígitos, a lógica se mantém. O número **5893** representa:

$$(5 \times 10^3) + (8 \times 10^2) + (9 \times 10^1) + (3 \times 10^0) = 5000 + 800 + 90 + 3 = 5893$$

Essa lógica também se aplica a números com frações decimais. Por exemplo, o número **0,368** é interpretado assim:

$$(3 \times 10^{-1}) + (6 \times 10^{-2}) + (8 \times 10^{-3}) = 0,3 + 0,06 + 0,008 = 0,368$$

Em qualquer número decimal, o dígito mais à **esquerda** representa o valor mais significativo, enquanto o da **direita** é o menos significativo.

### Sistema Binário: A Linguagem dos Computadores

Diferente dos seres humanos, os computadores não operam na base 10. Eles utilizam a base **2**, ou seja, trabalham exclusivamente com dois dígitos: **0 e 1**, que representam os estados **desligado (0)** e **ligado (1)** de seus circuitos eletrônicos.

Na prática, todo número binário segue o mesmo princípio posicional do sistema decimal, mas com potências de 2. Vejamos um exemplo com o número binário **1110**:

$$(1 \times 2^3) + (1 \times 2^2) + (1 \times 2^1) + (0 \times 2^0) = 8 + 4 + 2 + 0 = 14$$

Portanto, **1110<sub>2</sub> = 14<sub>10</sub>**.

Essa conversão é fundamental no mundo da computação, pois permite compreender como os números que conhecemos são codificados na linguagem da máquina.

### Convertendo Decimal para Binário

O processo inverso — converter de decimal para binário — é igualmente essencial e bastante simples. Ele consiste em realizar **divisões sucessivas por 2**, anotando os restos e, ao final, lendo esses restos de baixo para cima.

Por exemplo, para converter o número **23<sub>10</sub>** para binário:

|Operação|Quociente|Resto|
|---|---|---|
|23 ÷ 2|11|1|
|11 ÷ 2|5|1|
|5 ÷ 2|2|1|
|2 ÷ 2|1|0|
|1 ÷ 2|0|1|

Lendo os restos de baixo para cima, o resultado é **101112**.

Existe também uma técnica auxiliar muito eficiente, especialmente útil quando se trabalha com máscaras de sub-rede, por exemplo. Ela consiste em utilizar uma tabela de pesos binários:

|512|256|128|64|32|16|8|4|2|1|
|---|---|---|---|---|---|---|---|---|---|
||||||1|0|1|1|1|

Somando os valores associados aos bits "1", temos:

$$16 + 4 + 2 + 1 = 23$$

Portanto, o binário **10111<sub>2</sub>** corresponde a **23<sub>10</sub>**.

### Sistema Hexadecimal: Compactando a Informação

Se o sistema binário é eficiente para as máquinas, para os humanos ele não é muito prático — especialmente quando os números começam a ficar longos. Surge então o **sistema hexadecimal**, cuja base é **16**. Ele utiliza os dígitos de **0 a 9** e as letras **A, B, C, D, E e F**, que representam os valores de **10 a 15**, respectivamente.

Por que hexadecimal? Porque **4 bits binários correspondem exatamente a um dígito hexadecimal**, já que:

$$2^4 = 16$$

Observe a tabela de equivalência:

|Binário|Decimal|Hexadecimal|
|---|---|---|
|0000|0|0|
|0001|1|1|
|0010|2|2|
|0011|3|3|
|0100|4|4|
|0101|5|5|
|0110|6|6|
|0111|7|7|
|1000|8|8|
|1001|9|9|
|1010|10|A|
|1011|11|B|
|1100|12|C|
|1101|13|D|
|1110|14|E|
|1111|15|F|

Por exemplo, o número **0xFA** (onde o prefixo **0x** indica que o número está em hexadecimal) corresponde ao binário:

$$F = 1111 \quad A = 1010$$
$$0xFA = 11111010$$

Para converter de hexadecimal para decimal, aplicamos o mesmo princípio das potências:

$$(F \times 16^1) + (A \times 16^0)$$
$$(15 \times 16) + (10 \times 1) = 240 + 10 = 250$$

Se quisermos validar, podemos converter o binário correspondente (**11111010**) para decimal usando a tabela de pesos:

|512|256|128|64|32|16|8|4|2|1|
|---|---|---|---|---|---|---|---|---|---|
|||1|1|1|1|1|0|1|0|

Somando os bits "1":

$$128 + 64 + 32 + 16 + 8 + 2 = 250$$

Correto!

### Sistema Octal: Outra Forma de Compactação

Outro sistema utilizado em computação, especialmente em ambientes Unix/Linux, é o **sistema octal**, de base **8**, com dígitos que vão de **0 a 7**. Uma aplicação prática muito comum ocorre no comando **chmod**, usado para definir permissões de arquivos.

No octal, cada grupo de **3 bits binários** corresponde a um dígito:

|Binário|Octal|
|---|---|
|000|0|
|001|1|
|010|2|
|011|3|
|100|4|
|101|5|
|110|6|
|111|7|

Vamos converter o número **372<sub>8</sub>** para decimal:

$$(3 \times 8^2) + (7 \times 8^1) + (2 \times 8^0) = 192 + 56 + 2 = 250$$

O processo inverso, de decimal para octal, utiliza a divisão sucessiva por 8:

|Operação|Quociente|Resto|
|---|---|---|
|77 ÷ 8|9|5|
|9 ÷ 8|1|1|
|1 ÷ 8|0|1|

Lendo os restos de baixo para cima, temos o número **115<sub>8</sub>**, equivalente a 77<sub>10</sub>.

## Aritmética Binária

Ao contrário dos seres humanos, que utilizam majoritariamente a base decimal para realizar cálculos, os computadores executam operações matemáticas exclusivamente no sistema binário. Essa limitação ocorre não por capricho, mas por necessidade física: os circuitos eletrônicos reconhecem dois estados, geralmente representados pelos níveis de tensão — ligados e desligados, ou simplesmente **1 e 0**.

Porém, ao realizar operações matemáticas em binário, surge uma necessidade natural: como representar números negativos? Afinal, grande parte dos cálculos do cotidiano e dos algoritmos computacionais envolve não apenas números positivos, mas também negativos. É justamente nesse ponto que surgem os métodos de representação binária para números com sinal.

### Representação de Números Negativos

Existem diferentes métodos para representar números negativos no sistema binário. Contudo, alguns métodos são mais eficientes e adotados na prática computacional, enquanto outros possuem limitações que inviabilizam seu uso em arquiteturas modernas.

#### Sinal e Magnitude

O método mais simples, do ponto de vista conceitual, é o chamado **sinal-magnitude**. Nele, o bit mais à esquerda — conhecido como **bit mais significativo (MSB)** — é utilizado para indicar o sinal. Se o MSB for **0**, o número é positivo; se for **1**, o número é negativo. Os demais bits representam a magnitude, ou seja, o valor absoluto do número.

Por exemplo, considerando uma representação de 8 bits:

- **+15** é representado por:  
    $$00001111$$
- **-15** é representado por:  
    $$10001111$$

Apesar de ser intuitiva, essa abordagem possui sérios problemas. O mais evidente é a existência de duas representações para o zero:

$$+0 = 00000000$$
$$-0 = 10000000$$

Essa duplicidade do zero gera complicações no projeto dos circuitos, aumentando a complexidade para realizar operações como comparação e soma. Por essa razão, a representação sinal-magnitude é praticamente abandonada na implementação dos processadores modernos, sendo utilizada mais em aplicações específicas, como alguns sistemas de ponto flutuante.

#### Complemento de Dois: A Solução Mais Elegante

A arquitetura dos computadores modernos adota quase universalmente o método de **complemento de dois** para a representação de números negativos. Esse método, além de eliminar a ambiguidade do zero, simplifica a lógica da Unidade Lógica e Aritmética (ULA), pois as operações de soma e subtração passam a ser realizadas da mesma maneira tanto para números positivos quanto negativos.

O funcionamento é relativamente simples:

- Para números positivos, a representação em complemento de dois é exatamente igual à representação binária pura, ou seja, sem nenhuma alteração.
- Para números negativos, o processo envolve dois passos:
    1. **Inverter todos os bits do número positivo** (isso gera o chamado **complemento de um**).
    2. **Somar 1** ao resultado da inversão, obtendo o **complemento de dois**.

Vamos exemplificar a conversão do número **-2**, utilizando 8 bits:

1. Representação de +2:  
    $$00000010$$
2. Inversão dos bits (complemento de um):  
    $$11111101$$
3. Soma de 1 (resultado em complemento de dois, que representa -2):  
    $$11111110$$

Um macete prático consiste em percorrer o número da direita para a esquerda até encontrar o primeiro bit **1**. Esse bit (e todos à direita dele) permanece iguais. A partir do próximo bit à esquerda, todos os bits são invertidos. Essa técnica costuma ser muito utilizada em provas, concursos e até na prática profissional por sua agilidade.

### Faixa de Representação

Um aspecto crucial do complemento de dois é entender qual a faixa de valores possível para um determinado número de bits. Por exemplo, com **n bits**, a faixa é:

$$-2^{(n-1)} \text{ até } (2^{(n-1)} - 1)$$

Portanto, com 8 bits:

$$-128 \text{ até } +127$$

E com 4 bits:

$$-8 \text{ até } +7$$

A Tabela Comparativa a seguir faz uma comparação entre as representações de sinal-magnitude e complemento de dois considerando o uso de **4 bits**:

| Decimal | Sinal-magnitue | Complemento de dois |
| --- | --- | --- |
| +8 | Não tem! | Não tem! |
| +7 | 0111 | 0111 |
| +6 | 0110 | 0110 |
| +5 | 0101 | 0101 |
| +4 | 0100 | 0100 |
| +3 | 0011 | 0011 |
| +2 | 0010 | 0010 |
| +1 | 0001 | 0001 |
| +0 | 0000 | 0000 |
| -0 | 1000 | Não tem! |
| -1 | 1001 | 1111 |
| -2 | 1010 | 1110 |
| -3 | 1011 | 1101 |
| -4 | 1100 | 1100 |
| -5 | 1101 | 1011 |
| -6 | 1110 | 1010 |
| -7 | 1111 | 1001 |
| -8 | Não tem! | 1000 |

Observe que no complemento de dois não existe um $-0$, além de permitir a representação de um número negativo a mais.

### Conversão de Complemento de Dois para Decimal

Uma maneira eficiente de realizar a conversão é utilizando uma tabela de pesos, na qual o bit mais significativo tem peso negativo. Vejamos um exemplo utilizando 8 bits:

Número: **10000111**

|-128|64|32|16|8|4|2|1|
|---|---|---|---|---|---|---|---|
|1|0|0|0|0|1|1|1|

Soma dos bits:

$$-128 + 4 + 2 + 1 = -121$$

Portanto, **10000111** em complemento de dois representa **-121** em decimal.

### Operações Aritméticas: Soma e Subtração

A beleza do complemento de dois reside no fato de que a **adição** é realizada normalmente, sem distinção se os números são positivos ou negativos.

Por exemplo:

Soma de **-7 + 5**, utilizando 4 bits:

1. Representação de -7: **1001**
2. Representação de +5: **0101**
3. Soma: **1110**

Convertendo **1110**, obtemos **-2**, que é o resultado correto.

Outro exemplo com soma de números positivos: **3 + 4**

- **0011 + 0100 = 0111**, que é **7**, resultado esperado.

### Detecção de Overflow

O **overflow** ocorre quando o resultado de uma operação excede a faixa de valores que pode ser representada.

A regra para detectar overflow em complemento de dois é simples e direta:

- Se dois números **positivos** são somados e o resultado é **negativo**, houve overflow.
- Se dois números **negativos** são somados e o resultado é **positivo**, houve overflow.

Exemplo de overflow: **5 + 4**, utilizando 4 bits.

1. 5 = **0101**
2. 4 = **0100**
3. Soma: **1001** → que representa **-7** (um erro!)

Ocorreu overflow porque **positivo + positivo → deu negativo**, o que é inválido.

### Subtração com Complemento de Dois

A subtração é convertida em adição, bastando aplicar o complemento de dois no subtraendo e realizar a soma.

Exemplo: **2 – 7**

1. Representação de 2: **0010**
2. Complemento de dois de 7: **1001**
3. Soma: **0010 + 1001 = 1011**, que é **-5**, resultado correto.

### Multiplicação e Divisão por Potências da Base

Da mesma forma que no sistema decimal deslocamos a vírgula para direita ou esquerda ao multiplicar ou dividir por 10, no sistema binário isso acontece com a base 2. 

Por exemplo, para a base decimal:

$$1035 \div 10 = 103,5$$
$$1035 \div 100 = 10,35$$
$$1035 \times 10 = 10350$$
$$1035 \times 100 = 103500$$

Já para a base binária:

$$101010,11 \times 2 = 1010101,1$$
$$101010,11 \times 4 = 10101011$$
$$101010,11 \div 2 = 10101,011$$
$$101010,11 \div 4 = 1010,1011$$

O mesmo se aplica à base hexadecimal, mas com deslocamentos em grupos de 4 bits, já que **16 = 2⁴**.

$$F,A5 \times 16 = FA,5$$
$$F,A5 \times 256 = FA5$$
$$FA5 \div 16 = FA,5$$
$$FA5 \div 256 = F,A5$$

### Ponto Flutuante

Quando se trata de representar números reais — aqueles que possuem parte fracionária —, o sistema binário adota o conceito de **ponto flutuante**. Assim como fazemos na notação científica (ex.: 6,022 × 10²³), o ponto flutuante permite representar números muito grandes ou muito pequenos com uma quantidade limitada de bits.

O padrão mais utilizado é o **IEEE 754**, que define uma estrutura composta por três partes:

- **Bit de sinal:** indica se o número é positivo (0) ou negativo (1).
- **Expoente:** define o deslocamento da vírgula, representando a ordem de magnitude.
- **Mantissa:** representa os dígitos significativos do número.

Por exemplo, para representar **-128** em precisão simples (32 bits):

<div align="center">
  <img width="480px" src="./img/10-ponto-flutuante.png">
</div>

- Bit de sinal: **1** (negativo)
- Expoente: **10000000**
- Mantissa: **00000000000000000000000** (mantissa zerada para números inteiros)

O número é então codificado como:

$$1\ 10000000\ 00000000000000000000000$$

Essa padronização permite que processadores diferentes possam trabalhar com valores de ponto flutuante de forma consistente e padronizada, seja em aplicações científicas, financeiras ou gráficas.

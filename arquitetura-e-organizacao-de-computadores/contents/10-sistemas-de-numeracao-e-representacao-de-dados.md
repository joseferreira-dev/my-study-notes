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

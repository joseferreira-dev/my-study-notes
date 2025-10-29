# Capítulo 6 – Protocolos e Tecnologias da Camada de Acesso à Rede

Nos capítulos anteriores, estabelecemos a arquitetura conceitual dos modelos OSI e TCP/IP. Vimos que a camada mais baixa da pilha TCP/IP, a Camada de Acesso à Rede, é uma fusão das responsabilidades das camadas Física e de Enlace do modelo OSI. Esta camada fundamental serve como a ponte entre o mundo lógico dos pacotes de software e o mundo físico dos sinais elétricos, luminosos ou de rádio. Como vimos, ela será responsável por prover meios de acesso ao meio físico, controlar o fluxo de dados, estabelecer critérios de identificação e, crucialmente, lidar com a inevitabilidade de erros na transmissão.

Este capítulo é dedicado a explorar os protocolos e tecnologias que operam nesta camada. Iniciaremos nossa jornada analisando um dos problemas mais fundamentais da comunicação: como garantir que os dados recebidos sejam exatamente os mesmos que foram enviados?

## Detecção e Correção

Quando um dispositivo prepara dados para transmissão, a Camada de Enlace (operando dentro da Camada de Acesso à Rede do TCP/IP) organiza os pacotes recebidos da camada superior em unidades chamadas **quadros** (_frames_). Esses quadros, que são longas sequências de bits, são então entregues à Camada Física para serem convertidos em sinais e enviados através de um meio de transmissão.

Como exploramos no Capítulo 2, nenhum meio de transmissão é perfeito. Os sinais estão sujeitos a uma variedade de interferências, como ruído eletromagnético, atenuação e diafonia, que podem corromper os dados durante o trânsito. Um pulso de +5V (representando um bit '1') pode chegar ao receptor com uma voltagem tão baixa que é interpretado como 0V (um bit '0').

Simplesmente ignorar esses erros é inviável. Portanto, a Camada de Acesso à Rede implementa mecanismos robustos para lidar com eles. Essas estratégias se dividem em duas categorias principais:

1. **Detecção de Erros:** Técnicas que permitem ao receptor identificar que um quadro foi corrompido durante a transmissão. A ação mais comum ao detectar um erro é simplesmente **descartar o quadro** corrompido, confiando que um protocolo de camada superior (como o TCP) notará a perda e solicitará uma retransmissão.
2. **Correção de Erros:** Técnicas mais complexas que não apenas detectam um erro, mas também possuem informação suficiente para **identificar qual bit foi alterado e corrigi-lo** no lado do receptor, sem a necessidade de uma retransmissão.

A escolha entre detecção e correção depende da confiabilidade do meio. Em meios relativamente confiáveis, como fibra óptica e cabos de par trançado modernos, a detecção de erros é mais eficiente; é mais rápido descartar um quadro ocasionalmente corrompido do que adicionar a grande sobrecarga de dados (_overhead_) necessária para a correção de erros em cada quadro. Em meios muito propensos a erros, como a comunicação sem fio, a correção de erros (chamada de FEC - Forward Error Correction) torna-se vital.

Vamos analisar as três principais técnicas de _detecção_ de erros utilizadas em redes.

### Verificação de Paridade

A verificação de paridade é a técnica de detecção de erros mais simples. Sua lógica é adicionar um único bit extra a um bloco de dados, chamado de **bit de paridade**, para que o número total de bits '1' no bloco resultante seja sempre par (no caso da **paridade par**) ou sempre ímpar (no caso da **paridade ímpar**).

O transmissor e o receptor devem concordar previamente com qual tipo de paridade estão usando.

- **Exemplo com Paridade Par:** O transmissor deseja enviar a sequência de dados: `110100`
    1. Ele conta o número de bits '1' na sequência: existem 3 bits '1'.
    2. Como o objetivo é a paridade _par_ (um número par de bits '1'), e ele atualmente tem 3 (ímpar), ele adiciona um bit de paridade igual a **`1`**.
    3. A sequência final transmitida é: **`1101001`**. O número total de bits '1' agora é 4 (par).

Se o receptor receber a sequência `1101001`, ele conta 4 bits '1' (par), assume que os dados estão corretos e remove o bit de paridade, recuperando o `110100` original.

#### Detecção de Erros com Paridade

Agora, suponha que um erro ocorra durante a transmissão, alterando um único bit:

- Sequência Transmitida: `1101001`
- Sequência Recebida (com 1 erro): **`0`**`101001`
- O receptor conta os bits '1' na sequência recebida e encontra 3 (ímpar).
- Como o receptor esperava uma quantidade _par_ de bits '1', ele sabe que ocorreu um erro e descarta o quadro.

#### Fraqueza da Paridade Simples

A grande limitação da paridade simples é que ela só é capaz de detectar um número **ímpar** de erros (1, 3, 5, etc.). Se um número _par_ de bits for alterado, o mecanismo de paridade falha.

- Sequência Transmitida: `1101001`
- Sequência Recebida (com 2 erros): **`0`**`1`**`1`**`1001`
- O receptor conta os bits '1' na sequência recebida e encontra 4 (par).
- O receptor assume que o quadro está correto, embora os dados estejam corrompidos. O erro passa despercebido.

#### Paridade Bidimensional

Para superar a fragilidade da paridade simples e até mesmo adicionar capacidade de _correção_, pode-se usar a **paridade bidimensional**. Nessa técnica, os dados são organizados em uma matriz (linhas e colunas). Um bit de paridade é calculado para cada linha e para cada coluna.

Imagine que queremos enviar 12 bits: `1101 0110 1011`

1. **Organização em Matriz:**
    
    ```
    1 1 0 1
    0 1 1 0
    1 0 1 1
    ```
    
2. **Cálculo da Paridade (Par):** Calculamos a paridade para cada linha (à direita) e cada coluna (abaixo).
    
    ```
    1 1 0 1 | 1  (3 bits '1')
    0 1 1 0 | 0  (2 bits '1')
    1 0 1 1 | 1  (3 bits '1')
    ---------
    0 0 0 0
    ```
    
    (A paridade das colunas `101`, `110`, `011`, `101` resulta em `0000`.)
    
3. **Transmissão:** O transmissor envia os dados e os bits de paridade.
4. **Detecção e Correção no Receptor:** Suponha que um bit de dados seja corrompido, e o receptor receba:
    
    ```
    1 1 0 1 | 1
    0 1 0 0 | 0  <-- Erro aqui (era 1)
    1 0 1 1 | 1
    ---------
    0 0 0 0
    ```
    
    O receptor recalcula as paridades:
    - Paridade da Linha 2: `0+1+0+0 = 1`. O bit de paridade recebido foi `0`. **Há um erro na Linha 2.**
    - Paridade da Coluna 3: `0+0+1 = 1`. O bit de paridade recebido foi `0`. **Há um erro na Coluna 3.**
    
O receptor agora sabe que o erro está na intersecção da **Linha 2** com a **Coluna 3**. Ele pode, então, inverter o bit naquela posição (de `0` para `1`), corrigindo o dado original sem precisar de retransmissão. Esta técnica é significativamente mais robusta, mas exige um _overhead_ maior de bits de controle.

### Método de Soma de Verificação (Checksum)

O método de **soma de verificação** (ou _Checksum_) é outra técnica de detecção de erros que oferece uma proteção relativamente baixa, mas com uma vantagem crucial: é muito simples de implementar em software e exige pouco poder de processamento.

Por essa razão, o _checksum_ é amplamente utilizado na Camada de Transporte, pelos protocolos **TCP e UDP**. Como a Camada de Transporte opera a nível de software (dentro do sistema operacional), um baixo consumo de processamento no cálculo de erros é fundamental para não sobrecarregar a CPU do dispositivo.

O processo básico funciona da seguinte maneira:

1. **Transmissor:**
    - Divide os dados do segmento em blocos de tamanho fixo (ex: 16 bits).
    - Soma todos esses blocos usando um tipo especial de aritmética (chamada aritmética de complemento a um).
    - Calcula o "complemento a um" do resultado final dessa soma. Este valor final é o _checksum_.
    - O _checksum_ é inserido em um campo específico no cabeçalho do segmento (TCP ou UDP).

2. **Receptor:**
    - Recebe o segmento e divide os dados (incluindo o campo _checksum_) nos mesmos blocos de 16 bits.
    - Soma todos os blocos (incluindo o _checksum_ recebido).
    - Se o resultado final da soma for uma sequência de todos os bits '1', o segmento é considerado livre de erros. Se qualquer outro valor for obtido, um erro foi detectado e o segmento é descartado.

Embora seja melhor que a paridade simples, o _checksum_ ainda pode falhar em detectar certos tipos de erros (como a transposição de dois blocos de dados).

### Verificação de Redundância Cíclica (CRC)

A **Verificação de Redundância Cíclica**, ou **CRC**, é a técnica de detecção de erros mais poderosa e amplamente utilizada nas tecnologias da Camada de Enlace (como Ethernet e Wi-Fi). Ela é significativamente mais robusta que a paridade ou o _checksum_, especialmente na detecção de **erros em rajada** (_burst errors_), onde múltiplos bits consecutivos são corrompidos, um cenário muito comum em meios físicos.

O CRC exige um processamento de cálculo mais intensivo. No entanto, como a Camada de Enlace opera primariamente a nível de hardware (na placa de rede), esses cálculos são realizados por chips dedicados (ASICs), não impactando o processamento da CPU principal do dispositivo.

O CRC é baseado em aritmética de polinômios. A sequência de bits a ser transmitida é tratada como um polinômio, onde cada bit é o coeficiente (0 ou 1) de um termo.

- Exemplo: A sequência `11001` corresponde ao polinômio $1x^4 + 1x^3 + 0x^2 + 0x^1 + 1x^0$, ou simplesmente $x^4 + x^3 + 1$.

O funcionamento do CRC pode ser resumido da seguinte forma:

1. O transmissor e o receptor concordam previamente com um **polinômio gerador** (ou chave geradora), que também é uma sequência de bits. O tamanho desse gerador define o padrão do CRC.
2. O transmissor pega o quadro de dados e adiciona $n$ bits '0' ao final, onde $n$ é o número de bits no gerador (menos um).
3. Ele divide essa longa sequência de bits pelo polinômio gerador, usando uma aritmética binária especial (baseada em XOR) chamada divisão polinomial.
4. O **resto** dessa divisão é o _checksum_ do CRC. Esse resto terá $n$ bits.
5. O transmissor substitui os $n$ bits '0' que ele havia adicionado ao quadro de dados pelo _checksum_ (o resto da divisão). Este quadro final (dados + CRC) é o que é enviado.
6. O receptor recebe o quadro (dados + CRC) e o divide _exatamente_ pelo mesmo polinômio gerador.
7. Se o quadro foi recebido sem erros, o resto dessa divisão será **zero**. Se o resto for qualquer valor diferente de zero, o receptor sabe que um erro ocorreu e descarta o quadro.

Os padrões de CRC são nomeados de acordo com o tamanho do _checksum_ gerado:

- **CRC-8, CRC-12, CRC-16, CRC-32**

O padrão **CRC-32** é o mais utilizado pelos padrões do IEEE (como o Ethernet) e oferece um nível de proteção extremamente alto, sendo capaz de detectar a grande maioria dos erros possíveis, incluindo todos os erros em rajada de até 32 bits.

### Distância de Hamming

Para quantificar a capacidade de um código de detectar ou corrigir erros, utiliza-se um conceito chamado **Distância de Hamming**. Este parâmetro define a robustez de um conjunto de palavras-código.

A "Distância de Hamming" entre duas palavras-código (sequências de bits) de mesmo tamanho é simplesmente a **quantidade de bits que são diferentes** entre elas.

- **Exemplo:**
    - Palavra-código A: `01101011`
    - Palavra-código B: `11110000`
    
    Para calcular a distância, comparamos bit a bit (ou, mais formalmente, fazemos uma operação XOR):
    
    ```
        0110 1011
    XOR 1111 0000
    -------------
        1001 1011
    ```
    
    Contando os bits '1' no resultado, vemos que a Distância de Hamming entre A e B é **5**. Isso implica que seriam necessárias 5 alterações de bits para transformar a palavra A na palavra B.

A Distância de Hamming de um _código_ (um conjunto de todas as palavras-código válidas) é a menor distância entre quaisquer dois pares de palavras-código nesse conjunto. Esse valor nos diz o quão "separadas" as palavras-código estão umas das outras e, portanto, o quão robustas elas são a erros.

Este conceito nos leva a duas regras fundamentais:

1. **Regra de Detecção:** Para ser capaz de detectar até $d$ erros, um código precisa ter uma Distância de Hamming de, no mínimo, **$d + 1$**.
    - _Exemplo:_ A paridade simples (que vimos acima) transforma `110` em `1100` e `111` em `1111` (paridade par). A distância entre `1100` e `1111` é 2. Com uma distância de 2, o código pode detectar $d = 2 - 1 = 1$ erro. Ele não pode garantir a detecção de 2 erros, como já comprovamos.

2. **Regra de Correção:** Para ser capaz de corrigir até $t$ erros, um código precisa ter uma Distância de Hamming de, no mínimo, **$2t + 1$**.
    - _Exemplo:_ Para criar um código que possa corrigir 1 bit de erro ($t=1$), precisamos de uma Distância de Hamming de pelo menos $2(1) + 1 = 3$. Isso significa que as palavras-código válidas devem ser "distantes" o suficiente umas das outras para que, mesmo que um bit seja alterado, a palavra corrompida ainda esteja "mais perto" da palavra original do que de qualquer outra palavra-código válida.
# Capítulo 2 – Transmissão de Sinais e Meios de Transmissão

No capítulo anterior, estabelecemos uma visão panorâmica sobre o que são as redes de computadores, seus componentes e as diferentes formas como podem ser organizadas. Agora, vamos mergulhar em um nível mais fundamental para responder a uma pergunta essencial: como a informação, que existe de forma abstrata em nossos dispositivos, consegue de fato viajar de um ponto a outro? A resposta está no processo de **transmissão de sinais**.

Para que a comunicação ocorra, os dados precisam ser convertidos em uma forma de energia que possa se propagar através de um meio físico, seja ele um cabo de cobre, uma fibra óptica ou o ar. Pense nisso como traduzir um pensamento em palavras faladas: a ideia (informação) é convertida em ondas sonoras (um sinal) que viajam pelo ar (o meio) até serem captadas e interpretadas por um ouvinte. Em redes de computadores, o princípio é o mesmo: a informação é **codificada** em um sinal elétrico, luminoso ou eletromagnético, que é então transmitido. No destino, o receptor, que conhece as regras da codificação, realiza o processo inverso, a **decodificação**, para recuperar a informação original.

Este capítulo explorará os dois tipos fundamentais de sinais, a forma como são representados por ondas e, posteriormente, os meios pelos quais eles viajam.

## A Natureza da Informação: Sinais Analógicos e Digitais

Toda informação a ser transmitida precisa ser representada por um sinal. Um **sinal** é a representação física e quantificável da informação, variando ao longo do tempo para carregar os dados. A forma como essa variação ocorre define as duas categorias primordiais de sinais.

### Sinais Analógicos: O Contínuo Fluir da Informação

Um sinal analógico é caracterizado por sua natureza **contínua**. Isso significa que, ao longo de um intervalo de tempo, o sinal pode assumir uma infinidade de valores dentro de sua faixa de variação. Não há "saltos" ou interrupções; a transição entre um valor e outro é suave e fluida.

Muitos fenômenos do mundo real são intrinsecamente analógicos. A nossa voz, por exemplo, é uma onda de pressão sonora que varia continuamente em amplitude e frequência. Outros exemplos clássicos incluem:

- **Termômetro de Mercúrio:** A altura da coluna de mercúrio sobe e desce de forma contínua com a variação da temperatura, podendo ocupar qualquer ponto intermediário na escala.
- **Velocímetro com Ponteiro:** O ponteiro de um velocímetro analógico se move suavemente pelo mostrador, representando todas as velocidades possíveis dentro de sua faixa de operação.
- **Música em um Disco de Vinil:** Os sulcos de um disco são uma gravação física e contínua da forma de onda do som original.

A principal desvantagem dos sinais analógicos é sua vulnerabilidade ao **ruído**. Como qualquer variação de intensidade é considerada parte da informação, qualquer interferência (eletromagnética, por exemplo) que se some ao sinal durante a transmissão é difícil de ser removida e degrada a qualidade da informação original.

### Sinais Digitais: A Precisão dos Passos Discretos

Em contraste, um sinal digital é **descontínuo** ou **discreto**. Em vez de variar suavemente, ele opera com um conjunto finito e predefinido de valores. A transição entre um valor e outro ocorre em "saltos" ou "degraus".

A forma mais comum de sinal digital é a binária, que utiliza apenas dois níveis para representar os bits 0 e 1. Por exemplo, um sistema pode definir que +5 volts representa o bit '1' e 0 volts representa o bit '0'. O sinal só pode existir nesses dois estados, e não nos valores intermediários como 2,5 volts ou 4 volts.

Toda a informação processada por computadores é, em sua essência, digital. Para ser transmitida, essa sequência de bits precisa ser codificada em um sinal digital.

- **Vantagem Principal:** A grande força dos sinais digitais é sua robustez e imunidade ao ruído. Se um sinal de +5 volts (representando '1') sofre uma pequena interferência e chega ao receptor como +4,5 volts, o receptor ainda pode facilmente discernir que o valor pretendido era '1'. Ele pode então regenerar o sinal original perfeitamente, eliminando o ruído acumulado no caminho. Essa capacidade de regeneração sem perdas é o que permite a transmissão de dados digitais por longas distâncias com altíssima fidelidade.

## Representando Sinais: O Mundo das Ondas

Seja um sinal analógico ou digital, uma forma poderosa de representá-lo e analisá-lo é através de **ondas**. Uma onda é uma perturbação que se propaga no tempo e no espaço. Para entender como as informações são codificadas em um sinal, precisamos conhecer os parâmetros fundamentais de uma onda periódica (uma onda que se repete em ciclos regulares).

<div align="center">
<img width="440px" src="./img/02-exemplo-de-onda.png">
</div>

- **Amplitude:** É a "altura" ou intensidade máxima da onda, medida a partir de seu ponto de repouso (o eixo central). Em um sinal, a amplitude geralmente carrega a "força" do sinal.
- **Frequência (f):** Representa o número de ciclos completos que uma onda realiza em um determinado período de tempo. A unidade padrão para frequência é o **Hertz (Hz)**, onde 1 Hz equivale a um ciclo por segundo. Frequências mais altas significam que a onda oscila mais rapidamente. Este conceito é familiar em nosso dia a dia, como nas frequências das estações de rádio (em Megahertz, MHz) ou na velocidade dos processadores (em Gigahertz, GHz).
- **Comprimento de Onda/Período (T):** É a distância física que um ciclo completo da onda ocupa no espaço. Existe uma relação inversa entre frequência e comprimento de onda: quanto maior a frequência, menor o comprimento de onda.
- **Fase (φ):** Descreve a posição da onda em relação ao seu ponto inicial (tempo zero). Mudanças na fase são uma técnica importante para codificar informações em um sinal (modulação de fase).

A velocidade com que um sinal se propaga em um meio é determinada pela relação entre sua frequência e seu comprimento de onda.

### Tipos de Ondas

Embora existam infinitas formas de onda, algumas são fundamentais para o estudo da transmissão de sinais.

<div align="center">
<img width="620px" src="./img/02-principais-tipos-de-ondas.png">
</div>

- **Ondas Senoidais:** A onda senoidal é a forma de onda periódica mais básica e pura. Ela descreve uma oscilação suave e regular. É fundamental porque, de acordo com a teoria de Fourier, **qualquer outra forma de onda complexa pode ser decomposta em uma soma de múltiplas ondas senoidais** com diferentes amplitudes, frequências e fases. Por isso, a onda senoidal é o bloco de construção de todos os outros sinais.
- **Ondas Quadradas:** Caracterizadas por suas transições instantâneas entre dois níveis de amplitude (um valor mínimo e um máximo), as ondas quadradas são a representação visual ideal de um **sinal digital binário**. O nível alto pode representar um '1', e o nível baixo, um '0'. Na prática, para se transmitir uma onda que se aproxime de uma onda quadrada perfeita, o meio de transmissão precisa ser capaz de transportar uma gama muito ampla de frequências (ou seja, ter uma grande largura de banda), pois uma onda quadrada é, na verdade, a soma de uma onda senoidal fundamental e infinitas outras de frequência mais alta (harmônicos).
- **Ondas Triangulares e Dente de Serra:** Essas ondas, com suas subidas e descidas lineares, são menos comuns na transmissão de dados, mas são amplamente utilizadas em outras áreas da eletrônica, como na geração de sons em sintetizadores ou em circuitos de varredura de telas.

<div align="center">
<img width="540px" src="./img/02-principais-tipos-de-ondas-graficos.png" >
</div>

De posse desses conceitos básicos sobre a natureza dos sinais e sua representação em ondas, podemos agora avaliar as possíveis formas de degradação que um sinal pode sofrer ao viajar por um meio de transmissão.

## A Realidade da Transmissão: Degradação e Perda de Sinal

Em um mundo ideal, o sinal enviado pelo transmissor chegaria ao receptor de forma idêntica, perfeita e instantânea. No entanto, na prática, todo sinal que viaja por um meio de transmissão, seja ele um cabo de cobre, uma fibra de vidro ou o próprio ar, está sujeito a imperfeições que o degradam e distorcem. Pense em gritar para um amigo do outro lado de um salão lotado: sua voz chegará mais fraca (atenuação), será misturada com o barulho das outras conversas (ruído) e poderá ecoar nas paredes (reflexão).

Compreender essas formas de degradação é fundamental para o projeto de sistemas de comunicação robustos e confiáveis. Os três principais vilões que devemos combater na transmissão de sinais são a atenuação, o ruído e a reflexão.

<div align="center">
<img width="480px" src="./img/02-formas-de-degradacao-de-sinal.png">
</div>

### Atenuação: A Perda de Força do Sinal

A **atenuação** é a redução gradativa da força, ou amplitude, de um sinal à medida que ele se propaga pelo meio. Essa perda de energia é um fenômeno natural e inevitável, causado principalmente pela resistência do próprio meio, que absorve parte da energia do sinal e a converte em calor. A atenuação aumenta com a distância: quanto mais longe o sinal precisa viajar, mais fraco ele se torna.

Se um sinal for atenuado a ponto de sua amplitude se tornar muito próxima à do ruído de fundo, o receptor não conseguirá mais distingui-lo e interpretar a informação corretamente. Para combater a atenuação em transmissões de longa distância, são utilizados dispositivos para restaurar a força do sinal:

- **Em sinais analógicos, usam-se amplificadores.** Eles aumentam a amplitude do sinal, mas possuem uma desvantagem: amplificam também todo o ruído que o sinal acumulou até aquele ponto.
- **Em sinais digitais, usam-se repetidores.** Estes dispositivos são mais inteligentes. Eles recebem o sinal digital atenuado e distorcido, o decodificam para a sequência original de 1s e 0s, e então geram um sinal completamente novo, limpo e com a força original, para retransmiti-lo. Essa capacidade de regeneração é uma das principais vantagens da comunicação digital.

A atenuação não é uniforme para todas as frequências; alguns meios atenuam certas frequências mais do que outras, um fator crucial no projeto de sistemas de comunicação.

### Ruído: A Intervenção de Sinais Indesejados

O **ruído** consiste em qualquer energia elétrica, luminosa ou eletromagnética indesejada que se soma ao sinal original, deformando-o. Enquanto a atenuação enfraquece o sinal, o ruído o "suja". Existem diversas fontes de ruído, tanto internas quanto externas ao sistema:

- **Ruído Térmico:** Gerado pelo movimento aleatório dos elétrons em qualquer componente eletrônico que esteja acima do zero absoluto. É um ruído de fundo inevitável, presente em todos os sistemas de comunicação.
- **Ruído de Intermodulação:** Ocorre quando sinais de diferentes frequências compartilham o mesmo meio, podendo se misturar e criar novas frequências espúrias que interferem com os sinais originais.
- **Diafonia (Crosstalk):** Também conhecido como "linha cruzada", é um tipo de interferência causado pelo acoplamento de um sinal de um cabo para um cabo adjacente. É o que acontece quando se ouve uma conversa fantasma de outra linha em uma ligação telefônica antiga. É uma grande preocupação em cabos de rede com múltiplos pares de fios.
- **Ruído de Impulso:** São picos de energia irregulares e de curta duração, geralmente causados por fontes externas como raios, motores elétricos ou chaves de alta potência. É uma causa significativa de erros em transmissões digitais.

### Reflexão ou Eco: O Sinal que Bate e Volta

A **reflexão** é um fenômeno que ocorre quando um sinal, ao viajar pelo meio, encontra uma mudança abrupta em suas características, fazendo com que parte da energia do sinal seja "rebatida" de volta na direção da origem. Esse sinal refletido, também chamado de **eco**, viaja na contramão e pode interferir com os sinais que vêm logo atrás dele, causando distorção.

A principal causa da reflexão é a falta de "casamento de impedância" entre os componentes da rede. Impedância é, de forma simplificada, a oposição que um meio oferece à passagem de um sinal. Se um sinal viajando por um cabo de uma certa impedância encontra um conector ou outro cabo com uma impedância diferente, ocorrerá uma reflexão. A solução é garantir que todos os componentes de um link de comunicação — cabos, conectores e os próprios dispositivos — possuam a mesma impedância característica.

### O Estudo do Meio: Minimizando a Degradação na Prática

Para projetar sistemas de comunicação eficientes, os engenheiros estudam exaustivamente as propriedades dos meios de transmissão para entender como minimizar os efeitos da degradação. O objetivo é encontrar as "janelas" de operação ideais — faixas de frequência ou comprimentos de onda onde a atenuação e o ruído são menores.

Um exemplo clássico desse estudo é visto na tecnologia de fibra óptica. O gráfico abaixo ilustra a perda de sinal (atenuação) em uma fibra óptica em função do comprimento de onda da luz utilizada.

<div align="center">
<img width="480px" src="./img/02-exemplo-estudo-de-um-meio-guiado.png">
</div>

Analisando o gráfico, percebe-se que a atenuação não é constante. Existem "vales" onde a perda de sinal é significativamente menor. As faixas de operação para sistemas de fibra óptica, conhecidas como **janelas de transmissão**, não são escolhidas ao acaso; elas são posicionadas exatamente nesses vales de baixa atenuação. O gráfico destaca três janelas principais:

- A **1ª janela**, em torno de 0,85 µm (ou 850 nm).
- A **2ª janela**, em torno de 1,31 µm (ou 1310 nm).
- A **3ª janela**, em torno de 1,55 µm (ou 1550 nm).

Nota-se que a terceira janela é a que apresenta o menor nível de atenuação de todas, tornando-a a faixa preferencial para sistemas de comunicação óptica de longa distância, pois permite que o sinal viaje por muito mais quilômetros antes de precisar de um repetidor.

## Da Voz ao Bit, do Bit à Onda: Digitalização e Codificação de Linha

Já estabelecemos que os computadores operam em um mundo digital, baseado em bits (0s e 1s), enquanto muitos fenômenos do mundo real, como a voz humana, são analógicos. Para que um sinal analógico possa ser processado, armazenado e transmitido com a robustez de um sistema digital, ele precisa primeiro ser convertido em uma sequência de bits. Esse processo é chamado de **digitalização**.

Contudo, ter uma sequência de bits é apenas metade da batalha. Para transmitir essa sequência por um cabo, precisamos de um método para representar esses 0s e 1s como sinais elétricos ou luminosos. Essa segunda etapa é a **codificação de linha**. Vamos explorar os dois processos.

### Digitalização de Sinais Analógicos

Converter a infinita variação de um sinal analógico em uma representação digital finita é um processo de aproximação realizado em três etapas fundamentais.

#### Amostragem (Sampling)

O primeiro passo é a **amostragem**. Nela, o sinal analógico contínuo é medido em intervalos de tempo regulares e discretos, como se estivéssemos tirando "fotografias" do sinal em instantes específicos. O resultado é uma série de "amostras", cada uma com um valor de amplitude correspondente à intensidade do sinal naquele exato momento.

Para que a digitalização seja fiel ao original, a frequência com que essas amostras são coletadas (a **taxa de amostragem**) é crucial. O **Teorema de Nyquist-Shannon**, um princípio fundamental da teoria da informação, estabelece que a taxa de amostragem deve ser, no mínimo, o dobro da maior frequência contida no sinal analógico original. É por isso que, para digitalizar a voz humana (cujas frequências mais importantes vão até cerca de 4.000 Hz), o padrão da telefonia utiliza uma taxa de amostragem de 8.000 amostras por segundo.

#### Quantização (Quantization)

Após a amostragem, temos uma coleção de amostras com valores de amplitude que ainda podem ser infinitos (ex: 2,1756 V, 3,4891 V, etc.). A **quantização** é o processo de "arredondar" esses valores para um número finito de níveis predefinidos. O sinal é sobreposto a uma régua com um número limitado de degraus, e cada amostra é forçada a assumir o valor do degrau mais próximo.

O número de degraus disponíveis determina a precisão da representação. Se usarmos mais degraus (mais níveis), a aproximação será mais fiel ao sinal original, resultando em maior qualidade. A diferença entre o valor real da amostra e o valor quantizado é chamada de **erro de quantização**.

#### Codificação (Coding)

A etapa final é a **codificação**. Nela, um código binário único é atribuído a cada um dos níveis de quantização. Por exemplo, se tivermos 8 níveis de quantização, precisaremos de 3 bits para representar todos eles (pois 2<sup>3</sup>=8). O nível 0 poderia ser codificado como "000", o nível 1 como "001", e assim por diante, até o nível 7 como "111".

O resultado final deste processo de três passos é uma sequência de bits — um fluxo de dados digitais que representa, de forma aproximada, o sinal analógico original.

### Codificação de Linha: Representando Bits no Meio Físico

Com o fluxo de bits em mãos, o desafio agora é transmiti-lo fisicamente. A **codificação de linha** (ou _line coding_) é o conjunto de regras usado para converter essa sequência de 0s e 1s em um sinal elétrico real que viajará pelo cabo. A escolha do esquema de codificação é importante para resolver vários desafios práticos, como a sincronização entre transmissor e receptor e a eficiência no uso do meio.

A seguir, alguns esquemas de codificação importantes.

|Tipo de Codificação|Identificação do bit 0|Identificação do bit 1|
|---|---|---|
|**NRZI (Non-Return-to-Zero Inverted)**|Sem transição de voltagem no início do intervalo do bit.|Ocorre uma transição de voltagem (de alto para baixo ou de baixo para alto) no início do intervalo do bit.|
|**Manchester**|Transição do nível alto para o baixo no _meio_ do intervalo do bit.|Transição do nível baixo para o alto no _meio_ do intervalo do bit.|
|**Manchester Diferencial**|Ocorre uma transição de voltagem no _início_ do intervalo do bit.|Sem transição de voltagem no _início_ do intervalo do bit. (Ambos ainda possuem uma transição no meio para sincronismo).|

Vamos detalhar os mais relevantes:

- **NRZI (Non-Return-to-Zero Inverted):** Neste esquema, a informação não está no nível de voltagem em si, mas na _mudança_ ou _transição_ de nível. Um bit '1' é sinalizado por uma transição de voltagem no início do seu tempo de duração, enquanto um bit '0' é sinalizado pela ausência de transição. O problema do NRZI é que uma longa sequência de 0s não geraria transições, o que poderia fazer com que o receptor perdesse o sincronismo de seu relógio.
- **Manchester:** Este esquema resolve o problema da sincronização de forma engenhosa. Cada bit, seja 0 ou 1, possui uma transição de voltagem exatamente no meio de seu intervalo de tempo. Para um bit '0', a transição é de um nível de voltagem alto para um baixo; para um bit '1', a transição é de baixo para alto. Essa transição constante no meio de cada bit permite que o receptor ajuste continuamente seu relógio, garantindo um sincronismo perfeito. A desvantagem é a baixa eficiência, pois exige o dobro da largura de banda de esquemas mais simples para transmitir a mesma quantidade de dados. É um esquema de codificação clássico e de grande importância histórica, tendo sido utilizado no padrão original da Ethernet.
- **Manchester Diferencial:** Uma variação do Manchester que combina a transição no meio do bit (para sincronismo) com a codificação na transição do início do bit (típica de esquemas diferenciais). Um '0' é representado pela presença de uma transição no início do intervalo, enquanto um '1' é representado pela ausência dessa transição inicial. Todos os bits continuam tendo a transição no meio do caminho para manter o relógio sincronizado.

## O Espectro da Comunicação: Banda, Capacidade e Multiplexação

Todo meio de transmissão, seja ele um cabo ou o ar, possui características físicas que limitam a quantidade de informação que pode ser transportada. Assim como um cano possui um diâmetro que restringe o fluxo de água, um meio de comunicação possui uma "largura" que limita o fluxo de dados. Nesta seção, vamos explorar o que é essa largura (a banda), como calcular a capacidade máxima de um canal e como as redes utilizam técnicas inteligentes para "alargar a estrada" e permitir que múltiplos sinais viajem simultaneamente.

### Banda Base vs. Banda Passante: A Forma Original e a Adaptada

A forma como um sinal ocupa o espectro de frequências de um meio dá origem a duas modalidades de transmissão:

- **Transmissão em Banda Base (Baseband):** Refere-se à transmissão de um sinal em sua faixa de frequência original, sem alterá-la. Em sistemas digitais, isso geralmente significa que o sinal ocupa todo o espectro de frequência do meio, começando perto de 0 Hz. É como um trem que usa toda a linha férrea para si durante sua passagem; nenhum outro trem pode usar a linha naquele momento. A maioria das redes locais cabeadas, como as baseadas em Ethernet, opera em banda base.
- **Transmissão em Banda Passante (Passband ou Broadband):** Esta abordagem utiliza uma técnica chamada **modulação**. O sinal original (em banda base) é usado para modificar as características de uma onda de frequência mais alta, chamada de onda portadora. Esse processo "transporta" o sinal original para uma faixa de frequência específica e mais elevada dentro do meio. O grande benefício é que podemos fazer isso com vários sinais diferentes, colocando cada um em sua própria "faixa" de frequência, permitindo que todos trafeguem simultaneamente pelo mesmo meio. É como uma rodovia com múltiplas pistas, onde cada pista (faixa de frequência) pode ser usada por um carro (sinal) diferente ao mesmo tempo. O equipamento que realiza esse processo de modulação (na transmissão) e demodulação (na recepção) é o **MODEM** (MOdulador-DEModulador). Exemplos clássicos de transmissão em banda passante são o rádio, a TV a cabo e os serviços de internet ADSL.

### Largura de Banda e a Capacidade do Canal

O conceito de **largura de banda** (_bandwidth_) é fundamental para entender o potencial de um meio. Formalmente, é a diferença entre a maior e a menor frequência que um canal de comunicação pode transportar, medida em Hertz (Hz). Essa limitação pode ser uma característica intrínseca do meio físico ou pode ser definida artificialmente por filtros.

É comum haver uma confusão entre largura de banda e taxa de transmissão, mas são conceitos distintos:

- **Largura de Banda:** É o potencial teórico do meio, a "largura da estrada" (em Hz).
- **Taxa de Transmissão (ou Vazão):** É a quantidade real de bits que são transmitidos com sucesso por segundo (em bps, Mbps, Gbps), a "quantidade de carros que passam pela estrada".

Embora distintos, os conceitos estão diretamente relacionados: uma maior largura de banda geralmente permite uma maior taxa de transmissão. A capacidade máxima de um canal, ou seja, sua taxa de transmissão teórica mais alta, depende não apenas da largura de banda, mas também do nível de ruído e da complexidade da codificação do sinal.

Dois teoremas importantes nos ajudam a calcular essa capacidade:

**Teorema de Nyquist (para um canal sem ruído):** Harry Nyquist estabeleceu que a capacidade máxima de um canal ideal (sem ruído) é diretamente proporcional à sua largura de banda e ao número de níveis de sinal utilizados. Para um sinal binário (dois níveis), a fórmula é simplificada para:

$C=2⋅B$

Onde C é a capacidade em bits por segundo (bps) e B é a largura de banda em Hertz (Hz). Isso mostra que, em um canal perfeito, a cada 1 Hz de largura de banda, podemos transmitir no máximo 2 bps.

**Teorema de Shannon (para um canal com ruído):** Claude Shannon desenvolveu uma fórmula mais realista, que leva em conta o ruído, um fator sempre presente. A **Capacidade de Shannon** estabelece um limite superior absoluto e intransponível para a taxa de dados em um canal ruidoso:

$C=B⋅log_2​(1+\frac{S}{N})$

Onde C é a capacidade do canal (bps), B é a largura de banda (Hz), e S/N é a **relação sinal-ruído** (_Signal-to-Noise Ratio_ - SNR), que compara a potência do sinal (S) com a potência do ruído (N) no canal. Este teorema nos diz que, não importa quão sofisticada seja a tecnologia, a presença de ruído impõe um limite de velocidade fundamental a qualquer canal de comunicação.

### Modos de Propagação de Ondas Sem Fio

A frequência de operação de um sinal de rádio também determina a forma como ele se propaga através do espaço, o que é crucial para o projeto de sistemas de comunicação sem fio:

- **Ondas de Superfície (Ground Waves):** Aplicam-se a sinais de baixa frequência (geralmente abaixo de 2 MHz). Essas ondas tendem a seguir a curvatura da Terra, propagando-se rente à superfície e podendo alcançar longas distâncias. São utilizadas, por exemplo, em transmissões de rádio AM.
- **Ondas Celestes ou Ionosféricas (Sky Waves):** Na faixa de 2 a 30 MHz, as ondas de rádio podem ser refletidas pela ionosfera (uma camada eletricamente carregada na alta atmosfera) de volta para a Terra. Esse fenômeno de "ricochete" permite a comunicação de rádio de ondas curtas entre pontos muito distantes no globo, superando o horizonte.
- **Ondas Diretas (Line-of-Sight Waves):** Para frequências acima de 30 MHz, as ondas se comportam de forma muito parecida com a luz, viajando em linhas retas. Para que a comunicação ocorra, as antenas do transmissor e do receptor precisam ter visibilidade direta uma da outra, sem grandes obstáculos no caminho. Enlaces de micro-ondas, rádio FM, TV e comunicação via satélite são exemplos que utilizam a propagação por linha de visada.

### Multiplexação: Otimizando o Meio de Transmissão

A **multiplexação** é a técnica que permite a transmissão de múltiplos sinais de diferentes fontes por um único meio de comunicação, de forma simultânea e sem interferência. Ela otimiza o uso do meio, aumentando drasticamente a sua eficiência. As principais formas de multiplexação são:

- **FDM (Frequency-Division Multiplexing - Multiplexação por Divisão de Frequência):** Neste método, a largura de banda total do meio é dividida em várias faixas de frequência menores (canais), e cada sinal a ser transmitido é alocado em um desses canais exclusivos. É a técnica usada na radiodifusão (cada estação de rádio tem sua própria frequência) e nos sistemas de TV a cabo.
- **TDM (Time-Division Multiplexing - Multiplexação por Divisão no Tempo):** Aqui, os usuários compartilham o meio revezando-se no tempo. A cada usuário é atribuído um pequeno intervalo de tempo (_time slot_), e eles transmitem seus dados em sequência, um após o outro. O TDM e o FDM podem ser usados em conjunto.
- **CDMA (Code-Division Multiple Access - Acesso Múltiplo por Divisão de Código):** Uma técnica mais complexa onde todos os usuários transmitem simultaneamente na mesma faixa de frequência. A separação dos sinais é feita através da atribuição de um código matemático único para cada usuário, permitindo que o receptor "filtre" e reconheça apenas o sinal destinado a ele.
- **WDM (Wavelength-Division Multiplexing - Multiplexação por Divisão de Comprimento de Onda):** É a versão da FDM para o mundo da fibra óptica. Diferentes sinais são transmitidos através da mesma fibra usando diferentes comprimentos de onda (ou "cores") de luz.
    - **DWDM (Dense WDM):** Uma forma otimizada que consegue "empacotar" os comprimentos de onda de forma muito mais próxima, permitindo a criação de uma quantidade massiva de canais (centenas) em uma única fibra, o que possibilita as altíssimas taxas de transmissão da internet moderna.
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
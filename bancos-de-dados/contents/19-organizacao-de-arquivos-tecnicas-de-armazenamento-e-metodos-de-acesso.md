# Capítulo 19 – Organização de Arquivos, Técnicas de Armazenamento e Métodos de Acesso

## Introdução aos Aspectos Físicos do Banco de Dados

Até este ponto de nossa jornada, dedicamos grande parte de nossos estudos aos aspectos lógicos e conceituais dos bancos de dados. Agora, nosso foco se desloca para uma camada mais fundamental e igualmente crucial: os **aspectos físicos do armazenamento de dados**. Este capítulo tratará da organização de arquivos, das técnicas de armazenamento e dos métodos de acesso que formam a base sobre a qual os Sistemas de Gerenciamento de Banco de Dados (SGBDs) operam. Outra forma de descrever este mesmo assunto, utilizada por diversos autores, é relacioná-lo a estruturas de arquivo, indexação e _hashing_.

Se recordarmos da arquitetura ANSI/SPARC, que divide um SGBD em três níveis (externo, conceitual e interno), este capítulo se aprofundará no **nível interno**, ou seja, na representação física dos dados. Veremos como a estrutura física dos dados e as estratégias de acesso contribuem diretamente para o desempenho, a eficiência e a própria viabilidade de um sistema de banco de dados.

&lt;div align="center">

&lt;img src="https://placehold.co/600x350/EEE/31343C?text=Figura+19.1+-+Níveis+em+um+BD+(ANSI/SPARC" alt="Figura 19.1 - Níveis em um BD">

&lt;figcaption>Figura 19.1 - Níveis em um Banco de Dados (Arquitetura ANSI/SPARC)&lt;/figcaption>

&lt;/div>

#### **19.1 A Hierarquia de Armazenamento de Dados**

O armazenamento de dados em um sistema computacional é organizado em uma hierarquia de diversas mídias, que nos leva à primeira classificação relevante dentro deste assunto. As estruturas de armazenamento podem ser divididas em **primárias**, **secundárias** e **terciárias**, cada uma com características distintas de velocidade, custo, capacidade e volatilidade.

O **armazenamento primário** (ou memória primária) é aquele que pode ser operado diretamente pela CPU. Essa característica faz com que ele ofereça acesso extremamente rápido aos dados. Contudo, sua capacidade de armazenamento é comparativamente limitada e seu custo por byte é significativamente mais caro em relação às demais formas de armazenamento.

Os níveis **secundário e terciário** de armazenamento estão associados a mídias **não voláteis**, que não são operadas diretamente pela CPU. Por não poderem ser manipuladas diretamente pela CPU, existe a necessidade de transferir as informações que nelas residem para a memória primária antes de qualquer processamento. Esta operação de transferência (leitura ou escrita) é um procedimento consideravelmente demorado quando comparado com as demais etapas do processamento de consultas. Podemos listar como exemplos clássicos de armazenamento secundário os discos rígidos magnéticos (HDs) e as unidades de estado sólido (SSDs), e de armazenamento terciário as fitas magnéticas e os discos ópticos, frequentemente utilizados para backups e arquivamento de longo prazo.

É possível, ainda, separar os dispositivos físicos de armazenamento em **voláteis** ou **não voláteis**. Esse conceito se refere ao fato de a informação ser ou não perdida caso falte energia no sistema. A **memória RAM** (Random Access Memory) é o principal exemplo de memória volátil; ela serve para armazenar e executar as aplicações e os dados em uso depois que o computador já está ligado, e todas as suas informações são perdidas após o desligamento da máquina.

Na figura a seguir, você pode observar uma representação esquemática da hierarquia de armazenamento. Os três primeiros níveis da pirâmide (cache, memória principal) tratam de armazenamento tipicamente volátil e de acesso rápido. O quarto nível (armazenamento em flash, disco magnético) mostra os dispositivos de armazenamento permanente mais comuns, e a base da pirâmide (armazenamento óptico, fita magnética) representa o armazenamento de longo prazo e baixo custo.

&lt;div align="center">

&lt;img src="https://placehold.co/550x450/EEE/31343C?text=Figura+19.2+-+Hierarquia+de+Memória" alt="Figura 19.2 - Hierarquia de memória">

&lt;figcaption>Figura 19.2 - Hierarquia de Memória&lt;/figcaption>

&lt;/div>

Existem dois tipos principais de memória RAM que devem ser considerados em nosso estudo: **DRAM** e **SRAM**.

- **DRAM (Dynamic Random Access Memory)**, ou Memória de Acesso Randômico Dinâmica, é o tipo mais comum de RAM em computadores pessoais e servidores. Ela armazena cada bit de dados em um capacitor separado dentro de um circuito integrado. Como os capacitores perdem sua carga elétrica gradualmente, a informação precisa ser constantemente atualizada (refrescada) para que permaneça armazenada. Com isso, esse tipo de RAM consome mais energia em operação contínua.
- **SRAM (Static Random Access Memory)**, ou Memória de Acesso Randômico Estática, consegue manter os bytes armazenados enquanto houver fornecimento de energia, sem a necessidade de atualização contínua. Os dados só são perdidos após a interrupção da fonte de energia. A memória SRAM é, portanto, mais rápida e consome menos energia em estado de espera, mas é mais complexa e cara de se fabricar, razão pela qual é geralmente utilizada em menores quantidades, como na memória cache dos processadores.

Quando passamos a analisar as memórias não voláteis, o primeiro tipo que devemos analisar seria a **ROM (Read-Only Memory)**, que armazena o **BIOS (Basic Input/Output System)** ou o firmware de um computador. A ROM é uma memória somente de leitura (ou de escrita muito rara), e está, principalmente, localizada em um chip responsável pela iniciação do sistema (o _boot_). É lá que as informações básicas para dar partida no computador ficam armazenadas; portanto, elas não são afetadas quando o dispositivo é desligado.

Outros dispositivos de armazenamento permanente incluem os discos removíveis (como HDs externos e pen drives), os dispositivos de armazenamento em rede (que veremos mais adiante) e, claro, os discos rígidos internos. A organização dos dados dentro dos discos é de extrema importância para o desempenho de um SGBD, e dedicaremos as próximas seções para tratar de como os arquivos são organizados no disco.

#### **19.2 Dispositivos de Armazenamento Secundário: O Disco Magnético**

Ao definir como os arquivos são organizados nos discos, definimos, por tabela, como podemos acessá-los de forma eficiente. Outro ponto fundamental que temos que ter em mente é o fluxo de dados: os dados são transferidos da memória secundária (disco) para a memória primária (RAM), onde a CPU pode manipular esses dados e, em seguida, o resultado das modificações pode ser gravado de volta na memória secundária.

Uma organização de arquivos secundária, ou uma **estrutura de acesso auxiliar**, permite um acesso mais eficiente aos registros de um arquivo com base em campos de busca alternativos, além daqueles que foram usados para a organização primária do arquivo. A ideia da maioria desses dispositivos de acesso auxiliares se concentra na criação de **índices**, sobre os quais falaremos em detalhe mais adiante neste capítulo.

Segundo Navathe: “É importante estudar e entender as propriedades e as características dos discos magnéticos e o modo como os arquivos podem ser organizados neles a fim de projetar banco de dados eficazes, com desempenho aceitável”. Independentemente de sua capacidade, todos os discos magnéticos são feitos de um material magnético modelado como um disco circular fino, chamado de **prato (platter)**, conforme se pode ver na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/700x450/EEE/31343C?text=Figura+19.3+-+Componentes+de+um+Disco+Rígido" alt="Figura 19.3 - Componentes de leitura e gravação de um disco rígido">

&lt;figcaption>Figura 19.3 - Componentes de leitura e gravação de um disco rígido&lt;/figcaption>

&lt;/div>

Um disco (prato) pode ter apenas uma de suas faces úteis para armazenamento, como podemos observar na parte superior da figura acima. Outra possibilidade, mais comum, é ter as duas faces com capacidade de leitura e gravação. Podemos ainda ter um conjunto de múltiplos discos empilhados, conhecido como **disk pack**, com vários pratos em um único dispositivo, como exemplificado na parte inferior da figura.

A figura apresenta também os termos técnicos que descrevem a estrutura de um disco. Vamos aproveitar as próximas linhas para descrever esses termos, apresentando a devida palavra em português quando aplicável.

- **Track (Trilha):** É um círculo concêntrico de pequena largura na superfície de um prato. Existem centenas ou milhares de trilhas em cada uma das faces de um disco. Em _disk packs_ com múltiplos pratos, o conjunto de trilhas que possuem o mesmo diâmetro (ou seja, estão na mesma posição radial em todos os pratos) é denominado **cilindro**. Dados armazenados no mesmo cilindro podem ser recuperados mais rapidamente, pois não é necessário mover o braço de leitura/gravação para acessá-los. Cada trilha é, por sua vez, dividida em unidades menores chamadas blocos ou setores.
- **Arm (Braço):** Se há um componente dos discos rígidos considerado o "Tendão de Aquiles" em termos de desempenho mecânico, este é o braço de acesso. O braço deve se mover de forma extremamente rápida e precisa através de distâncias relativamente longas (da borda ao centro do prato). Além disso, o movimento do braço de acesso não é contínuo — ele deve acelerar rapidamente ao se aproximar do cilindro desejado e, então, desacelerar rapidamente para parar no ponto exato, evitando passar do ponto. Consequentemente, o braço de acesso deve ser forte (para suportar as forças violentas provocadas pela necessidade de movimentos rápidos) e leve ao mesmo tempo (para que haja menor massa a ser acelerada/desacelerada).
- **Actuator (Acionador):** É o motor responsável por mover o braço sobre a superfície dos pratos e, assim, permitir que as cabeças de leitura/gravação façam o seu trabalho. Para que a movimentação ocorra, o acionador contém em seu interior uma bobina que é induzida por ímãs potentes.
- **Read/Write Head (Cabeça de Leitura/Gravação):** As cabeças de leitura/gravação do disco rígido funcionam somente quando os pratos do disco sobre os quais elas atuam estão girando em alta velocidade. As cabeças "flutuam" sobre uma camada finíssima de ar criada pela rotação dos pratos, sem tocá-los. Como é o movimento da mídia (o prato girando) sob as cabeças que permite ler ou gravar os dados, o tempo que leva para a mídia contendo o setor desejado passar completamente por baixo da cabeça é um determinante do tempo total de acesso. A média de tempo de rotação para um drive de 10.000 RPM (rotações por minuto), por exemplo, é de cerca de 3 milissegundos para meia volta.
- **Spindle (Eixo):** Os discos (pratos) ficam posicionados sobre o eixo, que é o componente responsável por fazê-los girar em velocidade constante e elevada.

Já sabemos como o disco funciona e quais os elementos físicos principais que compõem a estrutura de um HD. Agora, vamos entender como os pratos são organizados logicamente. Cada um dos pratos presentes em um _disk pack_ deve ser dividido em partes menores, denominadas **setores** e **blocos**. Esses geralmente são definidos durante o processo de formatação de baixo nível do disco.

Um **setor** é o menor espaço físico em um disco formatado que pode ser endereçado individualmente pelo hardware. Quando um disco é formatado, as trilhas são definidas. Cada trilha é, então, dividida em fatias, conhecidas como setores. Os setores podem se estender por um ângulo fixo (resultando em setores maiores nas trilhas externas) ou podem ser organizados para manter uma densidade de gravação uniforme (resultando em mais setores nas trilhas externas do que nas internas) – veja a figura a seguir. Em discos rígidos e disquetes mais antigos, cada setor tipicamente armazenava 512 bytes de dados. Discos modernos frequentemente usam setores de 4096 bytes (4 KB), conhecido como Formato Avançado (Advanced Format).

&lt;div align="center">

&lt;img src="https://placehold.co/700x350/EEE/31343C?text=Figura+19.4+-+Organização+de+Setores+no+Disco" alt="Figura 19.4 - Diferentes organizações de setor no disco">

&lt;figcaption>Figura 19.4 - Diferentes organizações de setor no disco&lt;/figcaption>

&lt;/div>

Um **bloco**, por outro lado, é uma unidade lógica de transferência de dados entre o disco e a memória principal, gerenciada pelo sistema operacional ou pelo SGBD. Um bloco pode ter o tamanho de um ou de vários setores contíguos (por exemplo, um bloco de 8 KB pode ser composto por dois setores de 4 KB). O tamanho do bloco é uma configuração importante, pois influencia a eficiência do I/O. Um fator que influencia a escolha do tamanho do bloco é o tamanho do **buffer de disco**.

E qual a utilidade desse buffer? O _buffer_ (ou cache de disco) é uma parte da memória principal (RAM) que é alocada pelo sistema operacional ou pelo SGBD para armazenar cópias de blocos de disco que foram recentemente lidos ou que serão escritos. O uso de buffers é fundamental para o desempenho. Imagine que o processador está trabalhando na execução de uma determinada tarefa sobre os dados de um bloco que já está no buffer. Ao mesmo tempo, o subsistema de entrada e saída pode, de forma proativa (_read-ahead_), ler para o buffer os próximos blocos de disco que provavelmente serão necessários em breve. É necessário, portanto, desenvolver uma estratégia eficiente para o gerenciamento desse espaço de buffer (por exemplo, algoritmos como LRU - _Least Recently Used_ - para decidir quais blocos remover do buffer quando ele está cheio). Observe a figura a seguir; ela tenta clarear a sua percepção sobre o uso do buffer.

&lt;div align="center">

&lt;img src="https://placehold.co/600x300/EEE/31343C?text=Figura+19.5+-+Uso+de+Buffers+de+Disco" alt="Figura 19.5 - Uso de múltiplos buffers para E/S">

&lt;figcaption>Figura 19.5 - Uso de múltiplos buffers para E/S&lt;/figcaption>

&lt;/div>

Uma consideração importante e recorrente em bancos de dados é que um dos principais objetivos do SGBD e de suas estruturas de acesso é **minimizar o número de transferências de blocos entre o disco e a memória**. Isso ocorre porque o tempo para transferir um bloco de dados do disco para a memória é composto por várias etapas mecânicas e eletrônicas, e é significativamente mais lento do que o processamento em memória. O tempo total de acesso a um bloco no disco é a soma de:

- **Tempo de Busca (Seek Time):** O tempo que o braço de leitura/gravação leva para se mover e posicionar a cabeça sobre a trilha correta. Este é frequentemente o componente mais demorado do acesso a disco.
- **Atraso Rotacional (Rotational Latency):** O tempo de espera para o disco girar até que o início do setor ou bloco desejado passe por baixo da cabeça de leitura/gravação.
- **Tempo de Transferência (Transfer Time):** O tempo que leva para, efetivamente, ler ou escrever os dados do bloco, uma vez que a cabeça está posicionada e o bloco está passando por ela.

O **tempo de busca** é considerado o principal culpado pelo atraso envolvido na transferência de blocos entre o disco e a memória, especialmente para acessos aleatórios. Por isso, técnicas que promovem o acesso sequencial a blocos (como armazenar dados relacionados em blocos contíguos) são tão importantes para o desempenho.

Analisaremos a seguir como os arquivos de dados (ou seja, as coleções de informações que formam nossas tabelas) são gravadas dentro dos blocos de disco. Veremos como a distribuição dos registros de um arquivo dentro desses blocos pode fazer uma grande diferença no desempenho do processamento de uma consulta.

#### **19.3 Organização de Registros e Arquivos no Disco**

Primeiramente, precisamos de uma definição clara de **registro**. Um registro, no contexto de arquivos, é basicamente uma coleção de valores ou itens de dados relacionados, que representam uma única entidade ou ocorrência (por exemplo, os dados de um único funcionário ou de um único produto). Um **arquivo**, por sua vez, é definido como uma sequência de registros do mesmo tipo.

A primeira opção para compor um arquivo seria utilizarmos um conjunto de **registros de tamanho fixo**. Neste caso, cada registro no arquivo ocupa exatamente o mesmo número de bytes. Imagine que tenhamos um arquivo com _x_ registros, cada um com um tamanho fixo _R_. Desta forma, fica fácil definir e alocar espaço em disco para o arquivo, e também calcular a posição de um determinado registro dentro do arquivo.

A segunda opção seria definir arquivos cujos **registros têm tamanho variável**. São várias as possibilidades para que isso aconteça em um banco de dados:

- Um ou mais campos (atributos) de um registro são de tamanho variável (por exemplo, um campo de texto para comentários ou descrição de um produto).
- Um ou mais campos podem ter múltiplos valores para registros individuais (embora isso viole a 1ª Forma Normal, como vimos, alguns sistemas de arquivos ou modelos de dados podem permitir isso em um nível mais baixo).
- Um ou mais campos são opcionais, e podem ou não estar presentes em um determinado registro.
- O arquivo pode ser misto, composto por diferentes tipos de registros, cada um com seu próprio tamanho.

O livro de Silberschatz e coautores gasta algumas páginas para evoluir do conceito de registro de tamanho fixo para as várias formas de implementar registros de tamanho variável em um arquivo. Aqui, vamos nos limitar a exemplificar os conceitos com uma figura, pois, na nossa opinião, isso é mais do que suficiente para o escopo desta apostila. Vejam a imagem a seguir.

&lt;div align="center">

&lt;img src="https://placehold.co/800x400/EEE/31343C?text=Figura+19.6+-+Formatos+de+Registro" alt="Figura 19.6 - Formatos de Registros e Campos">

&lt;figcaption>Figura 19.6 - Formatos de Registros e Campos&lt;/figcaption>

&lt;/div>

Observem que na primeira representação acima (letra a), temos um **registro de tamanho fixo**. Neste formato, a posição de cada campo dentro do registro é predefinida e constante. Para lermos o salário do funcionário, por exemplo, basta lermos os bytes que começam na posição 40 do registro. Neste exemplo, temos seis campos que, juntos, somam um tamanho total fixo de 71 bytes.

No registro apresentado na letra (b), temos um exemplo de **registro de tamanho variável com campos delimitados**. Neste formato, os campos de tamanho variável (como `nome` e `cargo`) são terminados por um caractere especial de separação (delimitador). Os campos de tamanho fixo (`Cpf`, `Salario`, `Cod_Cargo`) podem ser armazenados sem delimitadores. Para ler os dados, o sistema precisa escanear o registro byte a byte, procurando pelos delimitadores para identificar o fim de cada campo variável.

Por fim, na opção (c), temos um formato de **registro de tamanho variável com um diretório de ponteiros interno**. Este formato é mais flexível. O registro começa com um cabeçalho que contém informações sobre os campos presentes e ponteiros (ou deslocamentos) para o início de cada campo dentro do próprio registro. Isso permite que os campos sejam opcionais e de tamanho variável, e que o acesso a um campo específico seja mais rápido do que escanear o registro inteiro, pois basta consultar o diretório no cabeçalho.

Vejam que, do ponto de vista prático, o que acontece nos arquivos de dados dos SGBDs modernos é uma variação sofisticada do que foi descrito acima, com a adição de várias outras informações que compõem o **cabeçalho do arquivo** e o **cabeçalho de cada bloco de dados**. Esta aula não está interessada nos detalhes mais profundos desses cabeçalhos, que são frequentemente tratados dentro do conteúdo de sistemas operacionais, na parte de gerenciamento de sistemas de arquivos.

Reforçamos que o objetivo de uma boa organização de arquivos no disco é **localizar o bloco que contém um registro desejado com um número mínimo de transferências de bloco** (ou I/Os de disco). Um **descritor de arquivo** ou cabeçalho geralmente contém informações que são exigidas pelos programas do sistema (como o SGBD) que acessam os registros do arquivo. Essas informações podem incluir o endereço de disco dos blocos que compõem o arquivo, descrições do formato dos registros (tamanho e ordem dos campos para registros de tamanho fixo), ou códigos de tipo de campo, caracteres separadores e códigos de tipo de registro para arquivos com registros de tamanho variável.

Agora já sabemos como cada arquivo é composto e que os registros fazem parte dos arquivos. Vamos, então, fazer a associação entre os registros e os blocos do disco. Imagine que o espaço restante em um bloco de disco não seja suficiente para o armazenamento do próximo registro a ser inserido no arquivo. Qual atitude devemos tomar?

São duas as opções possíveis para lidar com essa situação:

- **Organização não espalhada (unspanned):** Nesta abordagem, os registros não podem ultrapassar o tamanho de um único bloco. Se um registro não couber no espaço restante de um bloco, ele será armazenado no próximo bloco disponível, deixando o espaço restante no bloco anterior sem uso (fragmentação interna). A vantagem é que cada registro está contido inteiramente dentro de um bloco, o que simplifica o acesso.
- **Organização espalhada (spanned):** Nesta opção, um registro pode ser dividido entre dois ou mais blocos. Se um registro não couber no espaço restante de um bloco, a parte que couber é armazenada nesse bloco, e um ponteiro no final do primeiro bloco aponta para o bloco que contém o restante do registro. Isso minimiza o desperdício de espaço, mas pode tornar o acesso a um único registro mais lento, pois pode exigir a leitura de múltiplos blocos.

Vejam a diferença na figura apresentada a seguir. A letra (a) representa a opção não espalhada, e a letra (b) representa a opção espalhada.

&lt;div align="center">

&lt;img src="https://placehold.co/700x300/EEE/31343C?text=Figura+19.7+-+Organização+de+Registros+em+Blocos" alt="Figura 19.7 - Tipos de organização de registros em blocos (a) não espalhada, (b) espalhada">

&lt;figcaption>Figura 19.7 - Tipos de organização de registros em blocos (a) não espalhada, (b) espalhada&lt;/figcaption>

&lt;/div>

A figura acima nos ajuda a introduzir outra definição importante dentro deste assunto: o fator de blocagem (blocking factor - bfr). O fator de blocagem representa o número de registros que podem ser armazenados em um único bloco de disco. Para registros de tamanho fixo em uma organização não espalhada, se B é o tamanho do bloco (em bytes) e R é o tamanho do registro (em bytes), e se B ≥ R, então o fator de blocagem é dado por:

bfr = └B/R┘ (a função piso da divisão, ou seja, a parte inteira).

Para alocar os blocos de um arquivo no disco, o ideal, do ponto de vista do desempenho de leitura sequencial, é que tenhamos uma **alocação contígua**. Nesta estratégia, os blocos que compõem o arquivo são armazenados em posições sequenciais e adjacentes no disco. Esta disposição dos blocos torna a leitura sequencial do arquivo muito mais rápida, especialmente se estiver sendo utilizada uma técnica de buffer com _read-ahead_, pois o movimento do braço do disco (seek time) é minimizado. A desvantagem é a dificuldade de expansão do arquivo se não houver espaço contíguo livre.

Outra opção para a alocação do arquivo é a **alocação ligada (ou encadeada)**. Nela, cada bloco contém, além dos dados, um ponteiro para o endereço do próximo bloco do arquivo. Isso facilita enormemente a expansão do arquivo (basta encontrar qualquer bloco livre no disco e ligá-lo ao final da cadeia), mas torna a leitura sequencial mais lenta (pois pode envolver múltiplos movimentos do braço do disco para seguir os ponteiros) e o acesso direto a um bloco específico do meio do arquivo muito ineficiente.

Podemos juntar as duas opções acima criando **clusters de blocos (ou extensões)**. Nesta abordagem, o arquivo é alocado em "extensões" ou "clusters", onde cada extensão é um conjunto de blocos contíguos. As diferentes extensões são interligadas por ponteiros. Isso combina a vantagem da leitura sequencial rápida dentro de cada extensão com a flexibilidade de expansão do arquivo.

A última opção seria a **alocação indexada**, onde um ou mais blocos especiais, chamados de **blocos de índice**, contêm ponteiros para os blocos de dados reais do arquivo. Isso permite um acesso direto e eficiente a qualquer bloco do arquivo, mas introduz a sobrecarga de manter a estrutura de índice.

Quando pensamos em discos rígidos, o endereço de hardware de um bloco é uma combinação de número de cilindro, número de trilha (ou superfície) e número de setor/bloco. Este endereço físico é fornecido ao hardware de E/S do disco para localizar os dados. No entanto, os sistemas operacionais e SGBDs modernos geralmente trabalham com um endereço lógico de bloco (**LBA – Logical Block Address**), que é um endereço linear (um único número inteiro, de 0 a N-1, onde N é o número total de blocos no disco). O controlador de disco é responsável por traduzir o LBA para o endereço físico correspondente. Os dados lidos do disco para a memória principal são armazenados em um endereço de buffer.

Os HDs são conectados ao computador por meio de interfaces capazes de transmitir os dados entre um e outro de maneira segura e eficiente. Há várias tecnologias para isso, sendo as mais comuns historicamente os padrões IDE (PATA), SCSI e, mais recentemente, SATA (Serial ATA) e SAS (Serial Attached SCSI).

O **controlador de disco (HDC - Hard Disk Controller)** é o circuito eletrônico que permite à CPU comunicar-se com o disco rígido. Ele é comumente embutido na própria unidade de disco e interliga-se ao sistema de computação através de uma das interfaces mencionadas. O controlador aceita comandos de E/S de alto nível do sistema operacional (como "leia o bloco LBA 12345") e controla a execução mecânica e eletrônica desses comandos. Como já enfatizado, a localização e a transferência de dados no disco representam um dos principais gargalos de desempenho em aplicações de banco de dados.

Outro dispositivo comum à placa lógica de um disco rígido moderno é um pequeno chip de memória conhecido como **buffer (ou cache de disco)**. Cabe a ele a tarefa de armazenar temporariamente pequenas quantidades de dados durante a comunicação com o computador. Como este chip de memória cache consegue lidar com os dados de maneira muito mais rápida do que a parte mecânica dos discos rígidos, seu uso agiliza o processo de transferência de informações, tanto para leitura (armazenando dados que foram lidos recentemente ou que provavelmente serão lidos em breve) quanto para escrita (confirmando a escrita para o sistema operacional rapidamente, enquanto os dados são efetivamente gravados nos pratos em segundo plano). No mercado, atualmente, é comum encontrar discos rígidos que possuem buffer com capacidade que varia de alguns megabytes a centenas de megabytes.

Agora que temos os arquivos e seus registros devidamente organizados em blocos no disco, que tal aprendermos como as operações sobre eles são conceitualmente definidas? Apresentamos abaixo um conjunto de operações primitivas que são tipicamente executadas para a recuperação e manipulação de informações em um arquivo.

|   |   |
|---|---|
|**Operação**|**Descrição**|
|**Open**|Prepara o arquivo para leitura ou gravação. Aloca os buffers apropriados na memória principal. Define um ponteiro de arquivo para o início do arquivo.|
|**Reset**|Define o ponteiro de um arquivo já aberto de volta para o início do arquivo.|
|**Find (Locate)**|Procura o primeiro registro no arquivo que satisfaz a uma determinada condição de pesquisa. Transfere o bloco de disco que contém esse registro para um buffer na memória principal. Um ponteiro de registro atual é definido para apontar para o registro encontrado no buffer.|
|**Read (Get)**|Copia o registro atual (aquele para o qual o ponteiro de registro atual aponta) do buffer para uma variável de programa no espaço de memória do programa do usuário. Este comando também pode, opcionalmente, avançar o ponteiro do registro atual para o próximo registro no arquivo, o que pode necessitar da leitura do próximo bloco de arquivo do disco para o buffer.|
|**FindNext**|Procura o próximo registro no arquivo que satisfaz a mesma condição de pesquisa usada no último comando `Find`. Transfere o bloco que o contém para o buffer, se necessário. O registro encontrado torna-se o registro atual.|
|**Delete**|Exclui o registro atual e, eventualmente, atualiza o bloco no disco para refletir a exclusão (por exemplo, marcando o registro como excluído).|
|**Modify**|Modifica os valores de um ou mais campos do registro atual no buffer e, eventualmente, atualiza o bloco correspondente no disco.|
|**Insert**|Insere um novo registro no arquivo. Isso envolve localizar o bloco onde o novo registro deve ser inserido, transferir esse bloco para o buffer da memória principal, gravar o novo registro no buffer e, eventualmente, escrever o bloco modificado de volta para o disco.|
|**Close**|Completa o acesso a um arquivo, escrevendo quaisquer buffers modificados que ainda não foram persistidos no disco (flush), liberando os buffers alocados e realizando quaisquer outras operações de limpeza necessárias.|

Estas operações, com exceção de `Open` e `Close`, são frequentemente chamadas de operações **um registro por vez**, pois cada uma delas se aplica a um único registro a cada instante. Outra operação importante, que opera de forma iterativa, é o `Scan`:

|   |   |
|---|---|
|**Operação**|**Descrição**|
|**Scan**|Se o arquivo já estiver aberto (e possivelmente reiniciado com `Reset`), `Scan` retorna o primeiro registro; caso contrário, ele retorna o próximo registro na sequência do arquivo. Se uma condição específica de filtro for definida, o registro retornado é o primeiro (ou o próximo) que satisfaz essa condição.|

Existem algumas outras operações de mais alto nível que podem ser fornecidas pelo SGBD ou pelo sistema de entrada e saída do sistema operacional. Essas operações são responsáveis por retornar ou manipular mais de um registro que satisfaça a uma determinada condição de uma só vez (operações baseadas em conjunto). Observem a lista no quadro a seguir:

|   |   |
|---|---|
|**Operação**|**Descrição**|
|**FindAll**|Localiza todos os registros no arquivo que satisfazem uma determinada condição de pesquisa.|
|**Find_n_**|Procura o primeiro registro que satisfaz a condição de pesquisa e, em seguida, continua a localizar os próximos _n_ - 1 registros que também a satisfazem. Transfere os blocos que contêm os _n_ registros para o buffer da memória principal.|
|**FindOrdered**|Recupera todos os registros no arquivo em uma ordem especificada por um ou mais campos.|
|**Reorganize**|Inicia um processo de reorganização do arquivo. Algumas organizações de arquivo, como arquivos de heap com muitos registros excluídos ou arquivos ordenados que usam arquivos de overflow, exigem reorganização periódica para manter o desempenho. Um exemplo seria reordenar os registros de um arquivo classificando-os fisicamente com base em um campo especificado.|

Agora que já conhecemos os princípios básicos dos arquivos e das operações sobre eles, vamos seguir para entender como a organização interna desses arquivos pode otimizar o acesso aos dados.

#### **19.4 Métodos de Organização de Arquivos e Acesso**

Os conceitos de **organização de arquivos** e **métodos de acesso** são frequentemente estudados em conjunto, pois estão intrinsecamente relacionados, mas possuem definições distintas que são importantes de se compreender. Antes de começarmos a tratar dos termos práticos e formais, vamos fazer um exercício mental. Pegue o livro que está mais próximo de você e imagine que precisa encontrar a expressão "banco de dados" dentro dele. Você tem várias formas de executar essa pesquisa.

Supondo que seja um livro técnico de Tecnologia da Informação, uma primeira abordagem seria **folhear o livro página por página**, sequencialmente, desde o início, até encontrar a expressão solicitada. Esta seria uma busca linear. Outra opção, provavelmente mais eficiente, seria **consultar o índice remissivo**, que geralmente é colocado ao final do livro. O índice remissivo listaria o termo "banco de dados" em ordem alfabética, juntamente com os números das páginas onde ele aparece. Você poderia, então, ir diretamente para essas páginas.

Na história que acabamos de contar, o **livro é o nosso arquivo de dados**. A forma como as páginas e o conteúdo estão estruturados dentro dele (por exemplo, capítulos sequenciais, um índice remissivo no final) é a sua **organização de arquivo**. O passo-a-passo que você segue para chegar ao assunto que lhe interessa (seja a busca linear ou a busca pelo índice) é o seu **método de acesso**.

Após nossa rápida metáfora, vamos agora para os termos e conceitos mais formais sobre o assunto.

A **organização de arquivos** refere-se à organização dos dados de um arquivo em registros e blocos, e à forma como essas estruturas de dados são dispostas. Ela inclui o modo como os registros e os blocos são colocados fisicamente no meio de armazenamento e como eles são interligados (se for o caso). Os **métodos de acesso**, por outro lado, oferecem um conjunto de operações (como as que vimos na seção anterior: `Find`, `Read`, `Insert`, etc.) que podem ser aplicadas a um arquivo para recuperar, modificar, inserir ou excluir registros. É possível aplicar vários métodos de acesso a uma mesma organização de arquivos.

Alguns métodos de acesso, porém, só podem ser aplicados a arquivos que são organizados de certas maneiras específicas. Por exemplo, não podemos aplicar um **método de acesso indexado** a um arquivo que não possui uma estrutura de índice associada a ele. Várias técnicas gerais, como **ordenação**, _**hashing**_ e **indexação**, são usadas para criar organizações de arquivos e métodos de acesso mais eficientes do que a simples busca linear.

As principais formas de organização primária de arquivos (ou seja, como os registros são fisicamente armazenados) podem ser classificadas em:

- **Registros Desordenados (Arquivo de Heap):** Os registros são colocados no arquivo na ordem em que são inseridos, sem nenhuma ordenação específica.
- **Registros Ordenados (Arquivo Sequencial ou Classificado):** Os registros são mantidos fisicamente ordenados no arquivo com base nos valores de um campo específico, chamado de campo de ordenação.
- **Registros com Acesso por Hash (Arquivo de Hash):** A localização de um registro no disco é determinada aplicando-se uma função de _hash_ a um campo do registro, chamado de campo de _hash_.
- **Registros Mistos (Cluster de Arquivos):** Múltiplos tipos de registros de diferentes relações são armazenados juntos em um mesmo arquivo, geralmente para otimizar junções entre eles.
- **Árvores B/B+ (B-trees):** Embora sejam mais conhecidas como estruturas de índice, as árvores B+ também podem ser usadas como uma organização de arquivo primária, onde os próprios dados são armazenados nas folhas da árvore, mantendo-os ordenados e permitindo acesso sequencial e por chave de forma eficiente.

Falaremos de cada um deles, com exceção do último, que será tratado com mais detalhe na seção de indexação, a partir de agora.

##### **19.4.1 Arquivos de Registros Desordenados (Arquivos de Heap)**

Os **arquivos de registros desordenados**, conhecidos também como **arquivos de heap (_heap files_)**, representam a forma mais simples e básica de organização de arquivos. Nesta organização, os registros são inseridos no arquivo na ordem em que chegam, sem qualquer ordenação predefinida. A principal característica e vantagem desta abordagem é o fato de que **novos registros são sempre inseridos no final do arquivo**. Isso garante uma operação de inserção extremamente rápida e eficiente, pois não é necessário procurar por um local específico ou reorganizar os registros existentes.

Contudo, a simplicidade na inserção vem com um custo significativo no desempenho da busca. A **pesquisa por um registro** que satisfaça uma determinada condição em um arquivo de heap, sem o auxílio de estruturas de acesso secundárias (índices), só pode ser feita de forma **linear**. Ou seja, é necessário ler sequencialmente todos os registros (ou blocos) do arquivo, desde o início até o fim, para encontrar o(s) registro(s) desejado(s). Isso torna o tempo de leitura (busca) potencialmente muito alto, especialmente para arquivos grandes.

Outro problema inerente aos arquivos de heap é o tratamento da **exclusão de registros**. Quando um registro é excluído, ele deixa um "espaço vazio" no bloco onde estava armazenado. Existem algumas formas de lidar com esse problema:

- **Marcar e Ignorar:** A forma mais simples é simplesmente marcar o registro como "excluído" (por exemplo, com um _delete marker_), sem removê-lo fisicamente. O espaço não é reutilizado imediatamente, o que leva ao crescimento do arquivo mesmo com exclusões.
- **Reutilizar Espaço Livre:** Uma abordagem mais eficiente é usar o espaço deixado por registros excluídos para inserir novos registros. Isso, no entanto, exige uma manutenção extra, como manter uma lista encadeada de espaços livres (uma _free list_), para que o sistema saiba onde pode inserir novos registros sem ter que ir sempre para o final do arquivo.
- **Reorganização Periódica:** Outra opção é, de tempos em tempos, executar um processo de **reorganização do arquivo** para compactá-lo, removendo fisicamente os registros marcados como excluídos e eliminando os espaços livres resultantes da exclusão.

Todas essas opções para lidar com a exclusão acabam por requerer processamento e complexidade adicionais.

##### **19.4.2 Arquivos de Registros Ordenados (Arquivos Sequenciais)**

Os **arquivos de registros ordenados**, também chamados de **arquivos classificados** ou **arquivos sequenciais**, são aqueles em que os registros são fisicamente armazenados no disco de forma ordenada, com base nos valores de um campo específico do registro, conhecido como **campo de ordenação**. Se esse campo de ordenação for também a chave primária do registro, ele é chamado de **chave de ordenação**.

As vantagens desta forma de organização são notáveis para certos tipos de acesso:

- **Leitura Sequencial Eficiente:** Ler todos os registros na ordem do campo de ordenação é extremamente eficiente. Após ler o primeiro bloco, para carregar o próximo registro na sequência, geralmente não é necessário recarregar um bloco de disco diferente, pois os registros sequenciais estarão no mesmo bloco ou em blocos adjacentes.
- **Busca por Chave de Ordenação Rápida:** É possível usar algoritmos de busca eficientes, como a **busca binária**, sobre os blocos do arquivo para encontrar rapidamente um registro com um valor específico no campo de ordenação. Isso reduz drasticamente o número de acessos a disco em comparação com a busca linear de um arquivo de heap.

Vejamos um exemplo para verificarmos essas características. Observem a relação `EMPLOYEE` (FUNCIONARIO) descrita na figura abaixo. A figura representa blocos de um arquivo de disco contendo informações dos funcionários, e os registros dentro dos blocos (e os blocos em si) estão fisicamente ordenados alfabeticamente pelo nome do funcionário (`Name`). Percebam que, para encontrar um funcionário específico, podemos fazer uso de uma pesquisa binária sobre os blocos do arquivo. Para otimizar ainda mais o desempenho, os blocos de um arquivo ordenado são, idealmente, armazenados em cilindros contíguos no disco para minimizar o tempo de busca entre acessos a blocos sequenciais.

Os arquivos de registros ordenados, porém, não oferecem nenhuma vantagem para buscas que utilizam campos diferentes do campo de ordenação. Nesses casos, a busca ainda precisa ser linear. Além disso, as operações de **inserção e exclusão de registros são dispendiosas** e complexas.

Na **inclusão** de um novo registro, para manter a ordem, pode ser necessário deslocar, em média, metade dos registros do arquivo para abrir espaço para o novo registro. Para mitigar esse problema, algumas técnicas podem ser usadas:

- Deixar espaços livres (ou uma porcentagem de espaço livre) em cada bloco durante a carga inicial ou reorganizações, para acomodar novas inserções sem que o deslocamento supere o âmbito do bloco.
- Criar um **arquivo de overflow (ou arquivo de transação)**. Com essa técnica, o arquivo ordenado real é chamado de arquivo principal ou mestre, e um outro arquivo separado é usado para armazenar os novos registros que não cabem em seus devidos locais no arquivo principal (por exemplo, devido a um bloco cheio). Isso aumenta o custo e a complexidade de todas as operações de leitura, pois um arquivo separado do principal (o arquivo de overflow) tem que ser consultado e analisado além do arquivo principal.

Existem também problemas com a **exclusão**, que são considerados um pouco menos graves. A solução mais comum é usar marcadores de exclusão (como nos arquivos de heap) para marcar um registro como inválido, e, periodicamente, fazer a reorganização do arquivo para remover fisicamente os registros marcados. A preocupação com **modificações** em um arquivo ordenado só se torna realmente relevante se a modificação alterar o valor do campo de ordenação do registro, pois isso exigiria, conceitualmente, a exclusão do registro de sua posição antiga e a inserção em sua nova posição correta.

&lt;div align="center">

&lt;img src="https://placehold.co/700x350/EEE/31343C?text=Figura+19.8+-+Arquivo+Ordenado+de+Funcionários" alt="Figura 19.8 - Blocos de um arquivo ordenado (sequencial) de registros de FUNCIONARIO">

&lt;figcaption>Figura 19.8 - Blocos de um arquivo ordenado (sequencial) de registros de FUNCIONARIO&lt;/figcaption>

&lt;/div>

##### **19.4.3 Arquivos de Acesso Direto (Arquivos de Hash)**

A próxima forma de organização primária de arquivos que vamos discutir são os **arquivos de acesso direto**, mais conhecidos como **arquivos de hash**, que utilizam as técnicas de _**hashing**_. A ideia por trás do _hashing_ é oferecer uma **função h**, chamada de **função de hash** ou **função de randomização**, que é aplicada ao valor de um campo específico de um registro (conhecido como **campo de hash** ou **chave de hash**). O resultado da aplicação dessa função é, idealmente, o endereço do bloco de disco (ou de um _bucket_ que corresponde a um ou mais blocos) onde o registro está (ou deveria estar) armazenado. Veja o funcionamento geral do _hashing_ na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/600x350/EEE/31343C?text=Figura+19.9+-+Funcionamento+do+Hashing" alt="Figura 19.9 - Exemplo de funcionamento de uma função de hash">

&lt;figcaption>Figura 19.9 - Exemplo de funcionamento de uma função de hash&lt;/figcaption>

&lt;/div>

A condição de pesquisa para um acesso via _hash_ precisa ser uma condição de igualdade em um único campo (ou conjunto de campos) conhecido como campo de _hash_ ou chave de _hash_. Esta técnica é extremamente eficiente para buscas por chave primária, por exemplo. Vamos aproveitar a oportunidade para falar um pouco das técnicas de _hashing_ existentes. O _hashing_ também é utilizado como uma estrutura de pesquisa interna na memória de um programa (como em tabelas de _hash_). Sempre que um grupo de registros precisa ser acessado exclusivamente pelo valor de um campo específico (por exemplo, o CPF de uma pessoa), o _hashing_ é uma excelente opção.

Basicamente, podemos categorizar as técnicas de _hashing_ da seguinte forma:

- _**Hashing Interno:**_ Aplicado a estruturas de dados em memória principal.
- _**Hashing Externo:**_ Aplicado a arquivos em disco, que é o nosso foco aqui.
- **Técnicas de Hashing Dinâmico:** Métodos que permitem que o arquivo de _hash_ cresça e encolha dinamicamente, como o _hashing_ extensível e o _hashing_ linear.

Na minha concepção, Navathe e outros autores apresentam o _hashing_ interno na teoria para que possamos entender como ele funciona em um nível mais simples antes de passarmos para o _hashing_ externo. Nosso objetivo é fazer com que você entenda o que acontece quando aplicamos a função de _hash_ ao campo de _hash_. E, o mais importante: após entender que o número de valores possíveis do resultado da função de _hash_ (o número de endereços ou _buckets_) é, geralmente, muito menor do que o número de todos os valores possíveis do campo de _hash_ (o domínio da chave), as **colisões** (quando dois ou mais valores de chave diferentes produzem o mesmo endereço de _hash_) tornam-se, portanto, uma realidade inevitável. A questão crucial é: como tratar as colisões? É o que veremos mais adiante.

O **hashing interno** pode ser entendido por meio da figura anterior. A aplicação da função de _hash_ a um valor de chave nos leva a uma posição em uma tabela (um array, por exemplo) que armazena um registro (ou um ponteiro para ele). Vejam que existe a possibilidade de a função nos levar para o mesmo endereço de memória para chaves diferentes; é o que chamamos de **colisão**. A pergunta, mais uma vez, é: como tratar as colisões em um _hash_ interno?

- A primeira forma é usar o **endereçamento aberto (_open addressing_)**, que, ao encontrar uma posição já ocupada, tenta sistematicamente a próxima posição disponível no array (ou outras posições, de acordo com uma regra de sondagem).
- Outra opção é usar o **encadeamento (_chaining_)**. Neste caso, cada posição do array de _hash_ é, na verdade, o início de uma lista ligada (encadeada). Todos os registros que colidem na mesma posição são adicionados a essa lista. No encadeamento, o novo registro que colide é colocado em um local de _overflow_ (estouro) e um ponteiro no registro que já estava na posição de _hash_ aponta para ele.
- Por fim, temos a possibilidade de utilizar o **hash múltiplo (_double hashing_)**, onde, em caso de colisão, uma segunda função de _hash_ é aplicada à chave para calcular um novo deslocamento para sondar a próxima posição a ser verificada.

No **_hashing_ para arquivos em disco (ou _hashing_ externo)**, a ideia é semelhante, mas em vez de endereços de memória, a função de _hash_ mapeia uma chave para um **bucket**. Um _bucket_ é o espaço de endereços de destino para a função de _hash_ e, fisicamente, corresponde a um bloco de disco ou um cluster de blocos de disco contíguos. Uma tabela de _hash_ (ou diretório), geralmente mantida no cabeçalho do arquivo ou em um arquivo separado, converte o número do _bucket_ para o endereço de bloco de disco correspondente. O problema da colisão é, de certa forma, menos sério, pois, independentemente de quantos registros possam caber em um _bucket_ (se o _bucket_ for um bloco, o número de registros é o fator de blocagem), eles podem ser mapeados para o mesmo _bucket_ sem causar um problema imediato, desde que haja espaço no bloco. Uma colisão, neste contexto, acontece quando um registro precisa ser inserido em um _bucket_ que já está cheio.

Este modelo de _hashing_ externo, onde o número de _buckets_ é fixo, é conhecido como **hashing estático**. Ele não permite que o arquivo cresça ou diminua de tamanho facilmente. Se o arquivo cresce muito, muitos _buckets_ ficarão cheios, e o desempenho se degradará devido ao tratamento de estouros (_overflow_). Os próximos modelos que veremos estão baseados na criação de _buckets_ de forma dinâmica para resolver este problema.

A próxima opção é usar o **_hashing_ extensível**, que armazena uma estrutura de acesso (um diretório) fora do arquivo de dados, de forma semelhante à indexação. O _hashing_ extensível usa um **diretório dinâmico** de ponteiros para _buckets_. Cada entrada no diretório contém um ponteiro para um _bucket_, e cada _bucket_ tem um espaço fixo para armazenar registros. A ideia é usar um diretório de ponteiros para os _buckets_ e, quando um _bucket_ estoura (fica cheio), em vez de usar uma cadeia de _overflow_, o sistema duplica o número de entradas no diretório e particiona (divide) apenas o _bucket_ que transbordou em dois novos _buckets_, redistribuindo os registros entre eles. Veja a figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/700x500/EEE/31343C?text=Figura+19.10+-+Hashing+Extensível" alt="Figura 19.10 - Hashing extensível com diretório de ponteiros para buckets">

&lt;figcaption>Figura 19.10 - Hashing extensível com diretório de ponteiros para buckets&lt;/figcaption>

&lt;/div>

Outra opção de _hashing_ dinâmico seria utilizar o **_hashing_ linear**, que não requer uma estrutura de diretório adicional. Essa abordagem usa _buckets_ que são alocados sequencialmente. Cada _bucket_ tem um tamanho fixo M, ou seja, pode armazenar M registros (ou chaves). O arquivo inicia-se com um número fixo de _buckets_. Caso um _bucket_ estoure, é utilizado um **bucket de overflow**, que pode ser implementado como um registro em um bloco de overflow separado, formando uma lista encadeada. Um **ponteiro p** (que avança sequencialmente) determina qual o próximo _bucket_ a ser duplicado e ter seus registros redistribuídos, independentemente de qual _bucket_ causou o estouro. A criação de um novo _bucket_ e a redistribuição são determinadas pelo **fator de carga** do arquivo (a razão entre o número total de registros e a capacidade total dos _buckets_). Ou seja, vários _buckets_ podem conter registros em _overflow_. A cada inserção, o fator de carga é verificado. Caso seja maior do que um limite estipulado, um novo _bucket_ é alocado no final do arquivo, e o _bucket_ apontado por p é dividido. Veja a figura abaixo para entender melhor.

&lt;div align="center">

&lt;img src="https://placehold.co/700x400/EEE/31343C?text=Figura+19.11+-+Hashing+Linear" alt="Figura 19.11 - Hashing linear com buckets sequenciais e ponteiro de divisão">

&lt;figcaption>Figura 19.11 - Hashing linear com buckets sequenciais e ponteiro de divisão&lt;/figcaption>

&lt;/div>

Existem ainda outras organizações de arquivos primárias. Temos os **arquivos de registros mistos (_clustered files_)**, que armazenam registros de diferentes tipos (de diferentes tabelas) em um mesmo arquivo (ou nos mesmos blocos de disco), geralmente para otimizar o desempenho de junções frequentes entre essas tabelas. Por exemplo, registros de `PEDIDOS` poderiam ser armazenados fisicamente próximos aos registros do `CLIENTE` correspondente. E as **Árvores B+ (_B+-trees_)**, que são estruturas de árvore balanceadas projetadas especificamente para trabalhar com dispositivos de armazenamento secundário como discos magnéticos. Embora sejam mais famosas como estruturas de índice, elas também podem ser usadas para organizar o arquivo primário, onde os próprios registros de dados são armazenados nos nós folha da árvore.

Considerando a forma como o arquivo está organizado, o sistema pode recuperar registros de diferentes maneiras. Antigamente, quando os sistemas operacionais só armazenavam arquivos em fitas magnéticas, o acesso só poderia ser feito de modo **sequencial**. Neste caso, o acesso era restrito à leitura dos registros na ordem em que eram gravados, e a gravação de novos registros só era possível no final do arquivo. Este tipo de acesso, chamado de **acesso sequencial**, é uma característica intrínseca da fita magnética que, como meio de armazenamento, possuía esta limitação.

O próximo método, possibilitado pelo advento dos discos magnéticos, seria o **acesso direto**. Este método permite a leitura ou escrita de um registro diretamente em sua posição no arquivo. Este acesso é realizado através do **número do registro**, que é a sua posição relativa em relação ao início do arquivo (ou através do endereço de bloco calculado por _hashing_). É importante ressaltar que o acesso direto por número de registro só é eficiente quando o arquivo é definido com registros de tamanho fixo.

Temos também o **acesso indexado**. Este é um método de acesso mais sofisticado que, frequentemente, tem como base o acesso direto. É também chamado de **acesso por chave**. Para este tipo de acesso, o arquivo deve possuir uma ou mais estruturas de dados auxiliares, as **áreas de índice**, onde existem entradas que mapeiam valores de chave para ponteiros que indicam a localização dos registros correspondentes. Sempre que a aplicação desejar acessar um registro, ela deverá especificar uma chave, através da qual o sistema pesquisará na área de índice o ponteiro correspondente para acessar o registro diretamente.

#### **19.5 RAID – Redundant Array of Independent Disks**

Focado em paralelizar o acesso aos discos e aumentar a confiabilidade do armazenamento, o **RAID (Redundant Array of Independent Disks)**, que pode ser traduzido como "Conjunto Redundante de Discos Independentes" (ou, originalmente, "Inexpensive" - Baratos), é uma tecnologia fundamental em servidores e sistemas de armazenamento modernos. A implementação do RAID traz consigo duas vantagens principais: **maior confiabilidade** (tolerância a falhas) e **melhor desempenho** de E/S. Como já vimos, o gargalo no processamento de dados em bancos de dados ocorre, na maioria das vezes, por conta do tempo de E/S do disco. O RAID surgiu como uma forma de nivelar as taxas de melhoria de desempenho do disco, que historicamente evoluíram mais lentamente em comparação com as da memória e dos microprocessadores.

O RAID possui implementações tanto em **hardware** quanto em **software**. Nas implementações em hardware, um controlador RAID dedicado gerencia o conjunto de discos, e o arranjo RAID é apresentado ao sistema operacional como um único disco lógico. A vantagem é que esta abordagem é transparente para o sistema operacional e não utiliza recursos do processador principal para os cálculos de paridade e gerenciamento do array. Em implementações por software, é o próprio sistema operacional que gerencia o RAID através das controladoras de disco padrão, sem a necessidade de um controlador de RAID dedicado, o que torna a implementação mais barata, mas consome recursos da CPU do servidor.

O conceito se baseia na ideia de utilizar um grande **array (conjunto) de pequenos discos independentes**, que atuam em conjunto para formar o que parece ser um **único disco lógico maior**. Para alcançar o objetivo de desempenho, o RAID utiliza uma técnica chamada **_striping_ de dados (distribuição de dados)**, que nada mais é do que o emprego do paralelismo para melhorar a performance de leitura e escrita. O _striping_ consiste em dividir os dados em pedaços e alocar cada parte em diferentes discos do conjunto, de forma que eles possam ser lidos ou escritos simultaneamente.

O _striping_ de dados pode acontecer em diferentes níveis de granularidade. O **_striping_ em nível de bit**, por exemplo, espalha os bits de um único byte entre múltiplos discos. O **_striping_ em nível de bloco**, mais comum, divide os dados em blocos e espalha esses blocos entre os discos do array. Veja um exemplo na figura abaixo. Esse paralelismo no acesso aos dados tem dois objetivos principais: **balancear a carga** de E/S entre os discos do array e permitir que **grandes acessos a dados** (que abrangem múltiplos blocos) sejam realizados em paralelo, lendo de vários discos ao mesmo tempo. Portanto, o paralelismo influencia diretamente no desempenho.

&lt;div align="center">

&lt;img src="https://placehold.co/700x300/EEE/31343C?text=Figura+19.12+-+Striping+de+Dados" alt="Figura 19.12 - Striping de dados (bit-level e block-level) em vários discos">

&lt;figcaption>Figura 19.12 - Striping de dados (bit-level e block-level) em vários discos&lt;/figcaption>

&lt;/div>

Outro ponto fundamental do RAID é a **confiabilidade**, que é uma consequência da introdução de **redundância**. A forma mais simples de redundância é o **espelhamento (mirroring)** ou sombreamento dos dados, que consiste em duplicar a informação em discos separados. Neste caso, você teria os dados idênticos em dois ou mais discos. Isso permite também paralelizar a leitura (pois uma requisição de leitura pode ser atendida por qualquer um dos discos espelhados), mas a principal vantagem é a **resiliência do sistema**: quando um disco falha, o sistema continua operando normalmente, pois as requisições são automaticamente repassadas para o disco espelhado que continua ativo. O que acaba acontecendo nestes casos é uma queda de performance de escrita (pois toda escrita tem que ser feita em ambos os discos) e, em caso de falha de um disco, a perda do paralelismo de leitura. O desempenho pode ser retomado assim que o disco falho for substituído e os dados forem reconstruídos (re-espelhados).

Outra possibilidade para garantir a confiabilidade e a capacidade de recuperação de falhas é a utilização de **informações de paridade**, como o código de Hamming ou bits de paridade simples (XOR). A ideia é armazenar informações de redundância (a paridade) que permitam reconstruir os dados de um disco que falhou a partir das informações contidas nos demais discos do conjunto que continuam ativos. A forma pela qual os dados são distribuídos (_striped_) e os bits de paridade são calculados e armazenados entre os discos é uma das propriedades que definem a classificação do RAID em diferentes **níveis**.

##### **19.5.1 Níveis de RAID**

Vamos tratar agora dos níveis de RAID mais comuns encontrados na literatura e na prática. Vejamos cada um deles na lista abaixo:

- **RAID 0 (Striping):** Também conhecido como _striping_ de disco, este nível implementa uma técnica que divide um arquivo e distribui os blocos de dados entre todas as unidades de disco em um grupo RAID. No RAID 0, **não existe nenhum espelhamento ou controle de paridade**. O foco é puramente no desempenho. Neste caso, todos os discos funcionam como apenas um disco lógico maior e mais rápido, com a performance de leitura e escrita sendo, teoricamente, multiplicada pelo número de discos utilizados no conjunto. A grande desvantagem é que a falha de qualquer um dos discos resulta na perda de todos os dados do array, pois não há redundância.
- **RAID 1 (Mirroring):** Conhecido como espelhamento de discos, este nível é utilizado quando a confiabilidade e a proteção dos dados são a maior preocupação. Ele utiliza no mínimo dois discos e, basicamente, copia (espelha) os dados de um disco em outro. A capacidade útil do array é a de um único disco. A performance de leitura pode ser melhorada (lendo de ambos os discos em paralelo), mas a performance de escrita é ligeiramente menor que a de um disco único, pois os dados precisam ser gravados em ambos os discos. O RAID 1 não utiliza paridade.
- **RAID 2:** Este é um nível de RAID histórico que intercalava os dados em nível de bit e utilizava um mecanismo de detecção e correção de erros do tipo **ECC (Error Correcting Code)**, similar ao código de Hamming, distribuído em múltiplos discos. Surgiu em uma época em que os HDs não tinham controle de erro interno. Hoje, este nível quase não é mais utilizado, uma vez que praticamente todos os HDs modernos já possuem mecanismos de detecção e correção de erros implementados internamente em seu firmware.
- **RAID 3:** Este nível reserva uma unidade de armazenamento (um disco inteiro) apenas para guardar as informações de paridade, razão pela qual são necessários pelo menos três discos para montar o sistema. Os dados são distribuídos (_striped_) entre os outros discos, geralmente em nível de byte ou bit. A paridade é calculada a partir dos dados correspondentes nos outros discos. Este nível pode apresentar um gargalo no disco de paridade, pois toda operação de escrita exige uma atualização neste disco. Veja um exemplo de RAID 3 na figura a seguir.

&lt;div align="center">

&lt;img src="https://placehold.co/500x250/EEE/31343C?text=Figura+19.13+-+RAID+3" alt="Figura 19.13 - RAID 3 com disco de paridade dedicado">

&lt;figcaption>Figura 19.13 - RAID 3 com disco de paridade dedicado&lt;/figcaption>

&lt;/div>

- **RAID 4:** Também utiliza um disco de paridade dedicado e tem funcionamento similar ao RAID 3, mas com o diferencial de dividir os dados em blocos maiores (_block-level striping_) e de oferecer acesso potencialmente mais independente a cada disco do sistema para leituras pequenas. Assim como o RAID 3, este nível pode apresentar um gargalo de desempenho no disco de paridade, pois toda e qualquer operação de escrita exige a leitura do bloco de dados antigo, a leitura do bloco de paridade antigo, o cálculo da nova paridade e a escrita do novo bloco de dados e do novo bloco de paridade. Por este motivo, seu uso é mais indicado em sistemas que priorizam a leitura de dados.

&lt;div align="center">

&lt;img src="https://placehold.co/500x250/EEE/31343C?text=Figura+19.14+-+RAID+4" alt="Figura 19.14 - RAID 4 - Strip de bloco com disco dedicado para paridade">

&lt;figcaption>Figura 19.14 - RAID 4 - Strip de bloco com disco dedicado para paridade&lt;/figcaption>

&lt;/div>

- **RAID 5 (Striping com Paridade Distribuída):** Este nível utiliza paridade para a verificação de dados, mas, ao contrário do RAID 3 e 4, a informação de paridade é distribuída entre todos os discos do array, em vez de residir em um único disco dedicado. Isso elimina o gargalo do disco de paridade, melhorando o desempenho de escrita em comparação com o RAID 4. É uma das opções mais vantajosas em termos de custo-benefício (equilíbrio entre desempenho, capacidade e redundância), mas a reconstrução dos dados em caso de falha de um disco pode ser um processo intensivo. Veja a distribuição da paridade entre os diferentes discos na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/500x250/EEE/31343C?text=Figura+19.15+-+RAID+5" alt="Figura 19.15 - RAID 5 com paridade distribuída">

&lt;figcaption>Figura 19.15 - RAID 5 com paridade distribuída&lt;/figcaption>

&lt;/div>

- **RAID 6 (Striping com Dupla Paridade Distribuída):** O RAID 6 aplica o chamado esquema de redundância P + Q, usando algoritmos como o código de Reed-Solomon para proteger contra a falha de **até dois discos simultaneamente**. Trata-se de uma especificação mais recente e parecida com o RAID 5, mas com a importante diferença de trabalhar com **duas informações de paridade independentes** e distribuídas entre os discos. Isso oferece um nível de proteção de dados ainda maior, mas com uma penalidade maior no desempenho de escrita devido à necessidade de calcular e escrever duas paridades. Observe a figura a seguir com uma abstração da implementação de RAID 6.

&lt;div align="center">

&lt;img src="https://placehold.co/600x250/EEE/31343C?text=Figura+19.16+-+RAID+6" alt="Figura 19.16 - RAID 6 com dupla paridade distribuída">

&lt;figcaption>Figura 19.16 - RAID 6 com dupla paridade distribuída&lt;/figcaption>

&lt;/div>

##### **19.5.2 RAID Aninhado (Nested RAID)**

Existe ainda a possibilidade de unir os conceitos dos diferentes níveis de RAID "básicos" para criar arranjos híbridos ou aninhados (_nested RAID_). As combinações mais conhecidas são **RAID 0+1** (um espelho de _stripes_) e **RAID 10** (um _stripe_ de espelhos).

Tal como você já deve ter imaginado, o nível **RAID 0+1** é um sistema "híbrido" que combina RAID 0 com RAID 1. Para isso, o sistema precisa ter pelo menos quatro unidades de armazenamento. Primeiro, os discos são agrupados em conjuntos de RAID 0 (striping) para desempenho, e depois esses conjuntos são espelhados (RAID 1) para redundância. Assim, tem-se uma solução RAID que considera tanto o aspecto do desempenho quanto o da redundância.

Há uma variação chamada **RAID 10 (ou RAID 1+0)** de funcionamento conceitualmente semelhante, mas com uma implementação inversa. No RAID 10, primeiro os discos são agrupados em pares de RAID 1 (espelhamento) para redundância, e depois esses pares espelhados são distribuídos (_striped_) em um conjunto de RAID 0 para desempenho. A diferença essencial em termos de resiliência é que, no RAID 0+1, a falha de um único disco em um dos lados do espelho torna todo aquele lado inoperante, e se um disco falhar no outro lado antes da reconstrução, todo o array é perdido. No RAID 10, a falha de um disco em um par espelhado não afeta os outros pares, e o array só falha se os dois discos do mesmo par espelhado falharem. Por isso, o RAID 10 é geralmente considerado mais robusto que o RAID 0+1.

O RAID 10 é muito usado para aplicações que exigem alta performance de transações (leituras e escritas aleatórias) e alta segurança, como bancos de dados transacionais. Para implementar um arranjo RAID 10, são necessários pelo menos dois subgrupos de RAID 1, ou seja, no mínimo 4 discos. Vejam na figura abaixo um exemplo da implementação das duas possibilidades tratadas aqui.

&lt;div align="center">

&lt;img src="https://placehold.co/700x350/EEE/31343C?text=Figura+19.17+-+RAID+10+e+RAID+0+1" alt="Figura 19.17 - RAID 10 (stripe de espelhos) e RAID 0+1 (espelho de stripes)">

&lt;figcaption>Figura 19.17 - RAID 10 (stripe de espelhos) e RAID 0+1 (espelho de stripes)&lt;/figcaption>

&lt;/div>

Outras possibilidades de RAID aninhado incluem o **RAID 50** (um _stripe_ de conjuntos RAID 5) e o **RAID 60** (um _stripe_ de conjuntos RAID 6). Combinando dois arranjos já abordados, o RAID 60 (6+0), por exemplo, é um híbrido que combina duas ou mais configurações RAID 6 em um único _pool_ de discos em RAID 0. Com uma exigência de pelo menos 8 discos (dois subgrupos de 4 discos em RAID 6, por exemplo), esse sistema mantém dupla paridade por subgrupo. Isso garante que o sistema continuará funcionando mesmo que até dois discos falhem em cada subgrupo simultaneamente, oferecendo um altíssimo nível de proteção de dados. A figura abaixo apresenta um exemplo de RAID 60.

&lt;div align="center">

&lt;img src="https://placehold.co/700x350/EEE/31343C?text=Figura+19.18+-+RAID+60" alt="Figura 19.18 - RAID 60 (stripe de conjuntos RAID 6)">

&lt;figcaption>Figura 19.18 - RAID 60 (stripe de conjuntos RAID 6)&lt;/figcaption>

&lt;/div>

Vamos agora comparar algumas das versões de RAID que vimos até aqui em uma tabela resumo.

|   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|
|**Funcionalidade**|**RAID 0**|**RAID 1**|**RAID 5**|**RAID 6**|**RAID 10**|**RAID 50**|**RAID 60**|
|**Número Mínimo de Discos**|2|2|3|4|4|6|8|
|**Proteção de Dados**|Nenhuma|1 disco pode falhar|1 disco pode falhar|2 discos podem falhar|1 disco pode falhar em cada sub-array|1 disco pode falhar em cada sub-array|2 discos podem falhar em cada sub-array|
|**Performance (Leitura)**|Alta|Alta|Alta|Alta|Alta|Alta|Alta|
|**Performance (Escrita)**|Alta|Média|Baixa (cálculo de paridade)|Baixa (cálculo de dupla paridade)|Média|Média|Média|
|**Capacidade de Utilização**|100%|50%|67% - 94%|50% - 88%|50%|67% - 94%|50% - 88%|
|**Aplicações Típicas**|Dados transitórios, _caching_, edição de vídeo|Sistemas operacionais, bancos de dados transacionais|Data warehouse, servidores web, arquivamento|Arquivamento de dados críticos, backup em disco, alta disponibilidade|Bancos de dados rápidos, servidores de aplicação com E/S intensa|Grandes bancos de dados, servidores de arquivo, servidores de aplicação|Arquivamento de dados muito críticos, backup em disco, soluções de alta disponibilidade|

_Nota: A tabela não inclui RAID 1E e 5EE, que são níveis não-padrão e proprietários. O RAID 1E ("Enhanced") é uma variação do RAID 1 que distribui dados e espelhamento em um número ímpar de discos, oferecendo uma utilização de espaço melhor que o RAID 1 puro, mas não sendo um padrão universal._

#### **19.6 Arquiteturas Modernas de Armazenamento (DAS, NAS, SAN)**

Passaremos agora a uma análise sucinta dos principais paradigmas de arquitetura de armazenamento utilizados em ambientes corporativos. Nossa intenção é apresentar uma descrição para os seguintes termos: **DAS**, **NAS** e **SAN**, além do protocolo **iSCSI**.

##### **Direct Attached Storage (DAS)**

Nosso estudo começa com o **DAS (Direct Attached Storage)**, ou Armazenamento de Conexão Direta. O DAS representa a forma mais tradicional e simples de arquitetura de armazenamento, e suas características servirão de base para a comparação com os demais sistemas. No modelo DAS, as unidades de armazenamento (sejam elas discos rígidos internos, SSDs ou arrays de discos externos) estão conectadas diretamente ao servidor que as utiliza, sem a intermediação de uma rede de armazenamento. Um dos seus problemas históricos era a conectividade, considerada a principal limitação do DAS em termos de compartilhamento e escalabilidade. Entre o host (servidor) e o dispositivo de armazenamento não há elementos de rede (como hubs ou switches) intermediando o acesso. O dispositivo DAS não atua como um servidor de arquivos por si só; ele é, essencialmente, um conjunto de blocos de disco acessado por um único servidor ou, no máximo, por um pequeno cluster de servidores se o dispositivo possuir múltiplas portas de conexão direta. Os principais protocolos e barramentos usados pelo DAS são ATA, SATA, USB, eSATA, SCSI, SAS e, em cenários de alta performance, Fibre Channel (quando conectado diretamente a um único host). Observe a figura abaixo para entender um exemplo da utilização do DAS.

&lt;div align="center">

&lt;img src="https://placehold.co/500x300/EEE/31343C?text=Figura+19.19+-+Direct+Attached+Storage+(DAS" alt="Figura 19.19 - Exemplo de DAS">

&lt;figcaption>Figura 19.19 - Exemplo de Direct Attached Storage (DAS)&lt;/figcaption>

&lt;/div>

##### **Network-Attached Storage (NAS)**

O **NAS (Network-Attached Storage)**, conhecido como Armazenamento Conectado à Rede, representa uma abordagem diferente. Em vez de se conectar diretamente a um servidor, um dispositivo NAS é um servidor de armazenamento dedicado que se conecta à rede local (LAN) e oferece serviços de armazenamento de arquivos para múltiplos clientes e servidores na mesma rede. Um NAS é, essencialmente, um _appliance_ (um dispositivo especializado) que não oferece quaisquer dos serviços gerais de um servidor de aplicação, mas simplesmente permite o acréscimo de armazenamento para compartilhamento de arquivos.

Um dispositivo NAS é composto de um ou mais processadores, memória, fonte(s) de alimentação e os demais componentes de um computador, porém seu sistema operacional é otimizado para servir arquivos, e seu sistema de armazenamento de informações é geralmente robusto e confiável, quase sempre composto de vários discos rígidos em uma configuração RAID. Um NAS pode armazenar quaisquer dados que apareçam na forma de arquivos, como documentos de usuários, caixas de e-mail, conteúdo de sites Web, backups de sistema remoto, e assim por diante. Ele opera em um nível mais alto de abstração que o DAS, fornecendo acesso em nível de arquivo através de protocolos de rede como **NFS (Network File System)** para ambientes Unix/Linux e **SMB/CIFS (Server Message Block / Common Internet File System)** para ambientes Windows. O NAS oferece alto grau de escalabilidade (é fácil adicionar mais capacidade ou outro dispositivo NAS à rede), confiabilidade e flexibilidade. Ele também possui maior independência do sistema operacional do cliente em comparação com os servidores de arquivo tradicionais que rodam em sistemas operacionais de propósito geral.

##### **Storage Area Network (SAN)**

A **SAN (Storage Area Network)**, ou Rede de Área de Armazenamento, é uma arquitetura de armazenamento de alto desempenho, projetada para fornecer acesso em nível de bloco a dispositivos de armazenamento consolidados. Em uma SAN, os periféricos de armazenamento on-line (como grandes arrays de discos ou bibliotecas de fitas) são configurados como nós em uma rede dedicada e de alta velocidade, separada da LAN principal. Esses dispositivos de armazenamento podem ser conectados e desconectados dos servidores de uma maneira bastante flexível, permitindo que múltiplos servidores acessem o mesmo pool de armazenamento como se fossem discos locais (DAS).

Várias empresas surgiram como provedores de tecnologia SAN e fornecem os componentes e topologias proprietárias ou baseadas em padrões. As SANs permitem que os sistemas de armazenamento sejam colocados a distâncias maiores dos servidores (em comparação com conexões diretas como SCSI) e oferecem diferentes opções de desempenho e conectividade. Para implementar uma SAN, são necessários, tipicamente:

- **Um switch SAN:** Um switch de rede dedicado e de alta velocidade (geralmente baseado em **Fibre Channel**) que conecta os servidores aos dispositivos de armazenamento.
- **Um dispositivo de armazenamento SAN:** Um array de discos de alta capacidade e desempenho.
- **Um ou mais servidores:** Equipados com adaptadores de barramento de host (**HBA - Host Bus Adapter**) Fibre Channel para se conectarem ao switch SAN.

Vejam um exemplo esquemático de uma rede SAN na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/600x400/EEE/31343C?text=Figura+19.20+-+Storage+Area+Network+(SAN" alt="Figura 19.20 - Exemplo de uma rede SAN">

&lt;figcaption>Figura 19.20 - Exemplo de uma rede SAN&lt;/figcaption>

&lt;/div>

##### **iSCSI (Internet Small Computer System Interface)**

Vamos falar agora dos sistemas de armazenamento que usam **iSCSI**. O iSCSI é um protocolo de rede de armazenamento que permite que os clientes enviem comandos **SCSI** (o mesmo conjunto de comandos usado em muitas conexões DAS de alto desempenho) para dispositivos de armazenamento SCSI através de uma rede TCP/IP padrão. Essencialmente, o iSCSI permite a criação de uma SAN utilizando a infraestrutura de rede Ethernet existente, sem a necessidade de hardware e cabeamento especializados e caros como o Fibre Channel. Ele encapsula os comandos SCSI em pacotes TCP/IP para transmissão pela rede. Isso torna as soluções de armazenamento em rede baseadas em iSCSI mais acessíveis do que as SANs Fibre Channel tradicionais.

> **Nota:** O **Fibre Channel Protocol (FCP)** é um protocolo de transporte de alta velocidade desenvolvido especialmente para uso em SANs, permitindo a comunicação entre servidores e unidades remotas de armazenamento com baixa latência e alta largura de banda.

Falamos sobre os principais padrões de dispositivos e arquiteturas de armazenamento que nos interessam. A figura a seguir mostra uma comparação conceitual entre as camadas presentes na arquitetura de cada um desses padrões, destacando a diferença fundamental entre o acesso em nível de arquivo (NAS) e o acesso em nível de bloco (DAS, SAN, iSCSI).

&lt;div align="center">

&lt;img src="https://placehold.co/800x400/EEE/31343C?text=Figura+19.21+-+Comparação+DAS,+NAS,+SAN,+iSCSI" alt="Figura 19.21 - Comparação entre as arquiteturas de armazenamento">

&lt;figcaption>Figura 19.21 - Comparação entre as arquiteturas de armazenamento DAS, SAN, iSCSI e NAS&lt;/figcaption>

&lt;/div>

Em seguida, partiremos para tratar do assunto de indexação de arquivos, que é uma técnica fundamental para otimizar o acesso aos dados armazenados nesses dispositivos.

#### **19.7 Estruturas de Indexação para Arquivos**

Aqui, faremos uma análise detalhada das estruturas de indexação utilizadas em bancos de dados para acelerar a recuperação de informações.

##### **19.7.1 Conceitos Fundamentais de Índices**

O primeiro conceito que devemos ter em mente sobre este assunto é a definição de **índice**. Um índice, no contexto de bancos de dados, é uma estrutura de dados auxiliar e, geralmente, menor que o arquivo de dados principal, utilizada para melhorar significativamente a velocidade de acesso aos dados. Assim como o índice remissivo de um livro permite localizar um tópico rapidamente sem ter que ler o livro inteiro, um índice de banco de dados permite encontrar registros específicos sem ter que escanear todo o arquivo de dados. Um índice é composto por **entradas de índice (ou registros de índice)**. Cada entrada de índice consiste, tipicamente, em dois componentes principais:

- **Chave de Busca (ou Chave de Índice):** Um valor de um atributo (ou de um conjunto de atributos) do registro, que é usado para procurar os registros no arquivo.
- **Ponteiro:** Um endereço que aponta para a localização do registro (ou do bloco de dados) correspondente no arquivo de dados principal. Este ponteiro pode ser um identificador de bloco de disco mais um deslocamento (_offset_) dentro do bloco para encontrar o registro, ou simplesmente um ponteiro para o bloco.

Um **arquivo de índice** consiste, portanto, em um conjunto de registros de índice com o formato apresentado esquematicamente na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/400x150/EEE/31343C?text=Figura+19.22+-+Estrutura+de+um+Registro+de+Índice" alt="Figura 19.22 - Estrutura de um registro de índice">

&lt;figcaption>Figura 19.22 - Estrutura de um registro de índice&lt;/figcaption>

&lt;/div>

Como uma técnica para criar estruturas de dados auxiliares, os índices agilizam a busca e a recuperação de registros. Para isso, eles envolvem o armazenamento de dados adicionais e redundantes (as chaves de busca e os ponteiros) nos arquivos de índices, que consomem espaço em disco. Alguns tipos de acesso podem se beneficiar enormemente do uso de índices, por exemplo, a localização de um registro com um valor de chave especificado (busca por igualdade) e a localização de todos os registros que estão em um intervalo especificado de valores (busca por faixa).

É necessário saber se o benefício de desempenho obtido com a criação de um índice realmente compensa sua sobrecarga. Avaliar o impacto dos índices no desempenho geral do sistema é um passo importante no _tuning_ de bancos de dados. Vários SGBDs possuem utilitários (como o `EXPLAIN PLAN`) que ajudam a quantificar os efeitos pretendidos e reais da criação de índices sobre as consultas. A decisão de criar um índice deve ser baseada em alguns fatores, entre eles:

- O ganho no tempo de acesso para consultas de leitura.
- A sobrecarga nas operações de escrita (`INSERT`, `UPDATE`, `DELETE`), pois os índices precisam ser atualizados.
- A sobrecarga de espaço em disco para armazenar o próprio índice.
- Os métodos de acesso que o índice suporta.

Os arquivos de índices são geralmente muito menores que os arquivos de dados originais, especialmente se a chave de busca for pequena em comparação com o tamanho total do registro. Existem dois tipos principais de índices:

- **Índices Ordenados:** As chaves de busca no arquivo de índice são armazenadas de forma ordenada, o que facilita buscas por faixa e ordenação.
- **Índices de Hash:** As chaves de busca são distribuídas uniformemente através de “_buckets_” (baldes) usando uma função de _hash_. São eficientes para buscas por igualdade.

Vamos falar um pouco mais sobre a classificação dos índices ordenados.

##### **19.7.2 Índices Ordenados de Nível Único**

Num primeiro momento, focaremos nos **índices ordenados de nível único**. Eles são conceitualmente semelhantes aos índices remissivos que aparecem ao final de livros técnicos. O arquivo de índice é, ele próprio, um arquivo ordenado com base nos valores da chave de busca. A primeira classificação para índices ordenados de nível único os divide em três grupos principais, com base na relação entre o campo de índice e a organização física do arquivo de dados:

- **Índices Primários:** São definidos sobre o campo de ordenação de um arquivo de dados que já está fisicamente ordenado por esse mesmo campo. A chave de busca do índice é, geralmente, a chave primária do arquivo.
- **Índices de Agrupamento (_Clustering_):** São definidos sobre um campo de ordenação de um arquivo de dados que está fisicamente ordenado por esse campo, mas o campo de ordenação **não é** uma chave (ou seja, permite valores duplicados).
- **Índices Secundários:** São definidos sobre qualquer campo que **não** é usado para a ordenação física do arquivo de dados. O arquivo de dados pode estar ordenado por outro campo ou pode ser um arquivo de heap (desordenado).

###### **Índices Primários**

Os **índices primários** possuem uma característica importante: como o arquivo de dados já está ordenado pelo mesmo campo do índice (a chave de ordenação, que é a chave primária), não é necessário ter uma entrada no índice para cada registro do arquivo de dados. Basta ter uma entrada de índice para cada **bloco** no arquivo de dados. Cada entrada de índice em um índice primário contém o valor da chave de ordenação do **primeiro registro** de um bloco (este primeiro registro é chamado de **registro âncora** ou **âncora do bloco**) e um ponteiro para esse bloco.

Para buscar um registro com um valor de chave K, o sistema primeiro faz uma busca (por exemplo, binária) no arquivo de índice para encontrar duas entradas de índice consecutivas, com valores de chave K&lt;sub>i&lt;/sub> e K&lt;sub>i+1&lt;/sub>, tal que K&lt;sub>i&lt;/sub> ≤ K &lt; K&lt;sub>i+1&lt;/sub>. O registro com a chave K, se existir, estará no bloco de dados apontado pela entrada de K&lt;sub>i&lt;/sub>. Isso leva a uma ocupação de menos espaço em disco e memória para o índice.

|   |   |
|---|---|
|**Bloco**|**Âncora (Primeiro Registro do Bloco)**|
|1|Aaronson, ...|
|2|Adams, ...|
|...|...|
|i|Smith, ...|
|i+1|Thompson, ...|

No exemplo acima, para buscar por "Stevens", o sistema encontraria no índice a entrada para "Smith" (bloco i) e "Thompson" (bloco i+1) e saberia que "Stevens" deve estar no bloco i.

O principal problema com índices primários surge na inserção e remoção de registros no arquivo de dados, pois para manter a ordem física, pode ser necessário reorganizar os registros, o que, por sua vez, pode exigir a atualização das âncoras no arquivo de índice. A figura abaixo ilustra um índice primário.

&lt;div align="center">

&lt;img src="https://placehold.co/700x450/EEE/31343C?text=Figura+19.23+-+Índice+Primário" alt="Figura 19.23 - Índice primário no campo de chave de ordenação">

&lt;figcaption>Figura 19.23 - Índice primário no campo de chave de ordenação&lt;/figcaption>

&lt;/div>

###### **Índices de Agrupamento (Clustering)**

Os **índices de agrupamento** agilizam a recuperação de todos os registros que têm o mesmo valor para um campo específico, chamado de **campo de agrupamento**. A diferença crucial para o índice primário é que o campo de agrupamento **não é uma chave** e, portanto, pode ter valores duplicados. O arquivo de dados está fisicamente ordenado pelos valores do campo de agrupamento.

Neste caso, o arquivo de índice possui uma entrada para cada **valor distinto** do campo de agrupamento, juntamente com um ponteiro para o **primeiro bloco** no arquivo de dados que contém um registro com esse valor. Semelhantemente aos índices primários, eles enfrentam problemas com a inserção e remoção de registros, pois a ordem física do arquivo precisa ser mantida. Uma solução comum é reservar espaço livre para expansão dentro de cada bloco ou usar blocos de overflow. A figura a seguir apresenta a estrutura de um índice de agrupamento.

&lt;div align="center">

&lt;img src="https://placehold.co/700x450/EEE/31343C?text=Figura+19.24+-+Índice+de+Agrupamento" alt="Figura 19.24 - Índice de agrupamento no campo não chave de ordenação Numero_departamento">

&lt;figcaption>Figura 19.24 - Índice de agrupamento no campo não chave de ordenação Numero_departamento&lt;/figcaption>

&lt;/div>

Após observar os índices primários e de agrupamento, fica mais fácil entendermos outra classificação de índices que os divide em **densos** ou **esparsos (não-densos)**.

- Um índice é dito **denso** quando existe uma entrada de índice no arquivo de índice para **cada registro** (ou cada valor de chave de pesquisa) no arquivo de dados.
- Um índice é dito **esparso** (ou não-denso) quando ele possui entradas de índice apenas para **alguns** dos valores de pesquisa (tipicamente, um por bloco de dados).

Os índices primários e de agrupamento que acabamos de ver são exemplos de **índices esparsos**.

Mas, e os índices densos? Você não vai mostrar nenhum exemplo? Claro que vou... vamos falar sobre eles agora! Mais especificamente, sobre os **índices secundários**, que são, por natureza, densos.

###### **Índices Secundários**

Um **índice secundário** é um índice que é criado sobre um campo (ou conjunto de campos) que **não** é usado para a ordenação física do arquivo de dados. O arquivo de dados pode ser um arquivo de heap (desordenado) ou pode estar ordenado por um campo diferente.

Novamente, o índice secundário é um arquivo ordenado com dois campos: `<campo de índice, ponteiro>`. Neste caso, como o arquivo de dados não está ordenado pelo campo de índice, não podemos usar âncoras de bloco. Para garantir que possamos encontrar qualquer registro com um determinado valor de chave, é necessário que **todos os valores possíveis de busca (ou seja, de todos os registros) estejam presentes no índice**. Ele é, portanto, um **índice denso**. Consequentemente, ele usa mais espaço de armazenamento que um índice primário e a sua ordenação é lógica (no arquivo de índice), não refletindo a ordem física dos registros no arquivo de dados. Veja um exemplo na figura a seguir.

&lt;div align="center">

&lt;img src="https://placehold.co/700x500/EEE/31343C?text=Figura+19.25+-+Índice+Secundário+Denso" alt="Figura 19.25 - Um índice secundário denso (com ponteiros de bloco) em um campo de chave não ordenado de um arquivo">

&lt;figcaption>Figura 19.25 - Um índice secundário denso (com ponteiros de bloco) em um campo de chave não ordenado de um arquivo&lt;/figcaption>

&lt;/div>

Quando o campo sobre o qual o índice secundário é criado **não é uma chave candidata**, podemos ter o mesmo valor de índice para múltiplos registros. Neste caso, precisamos de um artifício para que a entrada de índice possa apontar para todos os registros associados a um determinado valor. Algumas soluções possíveis são:

- Ter múltiplas entradas no índice para o mesmo valor de chave, cada uma apontando para um registro diferente.
- Usar ponteiros de tamanho variável que possam armazenar uma lista de ponteiros para os registros.
- Criar um **nível de indireção**: a entrada de índice aponta para um **bloco de ponteiros** (ou _bucket_) intermediário, que, por sua vez, contém a lista de todos os ponteiros para os registros que possuem aquele valor de chave. Veja a figura abaixo para entender melhor esta última abordagem.

&lt;div align="center">

&lt;img src="https://placehold.co/700x550/EEE/31343C?text=Figura+19.26+-+Índice+Secundário+com+Indireção" alt="Figura 19.26 - Um índice secundário (com ponteiros de registro) em um campo não chave implementado usando um nível de indireção">

&lt;figcaption>Figura 19.26 - Um índice secundário (com ponteiros de registro) em um campo não chave implementado usando um nível de indireção&lt;/figcaption>

&lt;/div>

Vamos tentar organizar as possibilidades de índices ordenados com as suas respectivas formas de utilização em uma tabela:

|   |   |   |
|---|---|---|
||**Campo de índice usado para ordenação física do arquivo**|**Campo de índice não usado para ordenação física do arquivo**|
|**Campo de índice é uma chave (valores únicos)**|ÍNDICE PRIMÁRIO (Esparso)|ÍNDICE SECUNDÁRIO (CHAVE) (Denso)|
|**Campo de índice não é uma chave (valores duplicados)**|ÍNDICE DE AGRUPAMENTO (Esparso)|ÍNDICE SECUNDÁRIO (NÃO CHAVE) (Denso)|

Para finalizar esta seção, apresentamos abaixo uma lista com as características resumidas de cada um dos tipos de índices ordenados de nível único discutidos até o momento.

|   |   |   |   |
|---|---|---|---|
|**Tipo de Índice**|**Número de Entradas de Índice**|**Denso ou Esparso**|**Ancoragem de Bloco no Arquivo de Dados**|
|**Primário**|Número de blocos no arquivo de dados|Esparso|Sim|
|**De Agrupamento**|Número de valores distintos do campo de índice|Esparso|Sim/Não (aponta para o 1º bloco do grupo)|
|**Secundário (Chave)**|Número de registros no arquivo de dados|Denso|Não|
|**Secundário (Não Chave)**|Número de registros ou número de valores de campo distintos|Denso ou Esparso (se usar indireção)|Não|

##### **19.7.3 Índices Multinível**

Os índices de nível único que vimos até agora, mesmo os esparsos, podem se tornar muito grandes para arquivos de dados muito extensos. Se um arquivo de índice for tão grande que não possa ser mantido inteiramente na memória principal durante uma busca, o acesso ao próprio índice exigirá múltiplas leituras de disco, diminuindo sua eficiência. A solução para este problema é tratar o próprio arquivo de índice como um arquivo ordenado e **criar um índice sobre o índice**. Este conceito nos leva aos **índices multinível**.

Para falarmos dos índices multinível, vamos começar recorrendo a uma imagem que está apresentada abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/700x400/EEE/31343C?text=Figura+19.27+-+Conceito+de+Índices+Multinível" alt="Figura 19.27 - Conceito de Índices Multinível">

&lt;figcaption>Figura 19.27 - Conceito de Índices Multinível&lt;/figcaption>

&lt;/div>

Vejam que temos o arquivo de dados na base e múltiplos níveis de arquivos de índice acima dele. A ideia é diminuir a quantidade de transferências de blocos necessárias para pesquisar no índice. Criamos uma estrutura de árvore que começa com o arquivo de dados, tem um primeiro nível de índice que aponta para os blocos de dados (ou âncoras de bloco), um segundo nível de índice que aponta para os blocos do primeiro nível de índice, e assim por diante. A busca começa no nível mais alto do índice (o topo da árvore), que é pequeno o suficiente para caber em um único bloco de disco e ser lido para a memória. A partir daí, a busca desce pelos níveis da árvore, realizando uma leitura de bloco por nível, até chegar ao ponteiro para o bloco de dados desejado.

Com esse conceito, conseguimos entender o objetivo dessa hierarquização dos índices: reduzir drasticamente a parte do índice que precisamos pesquisar em cada passo. O fator de redução em cada nível é o **fator de blocagem do índice (bfri)**, que é o número de entradas de índice que cabem em um único bloco.

Em um índice multinível, o primeiro nível do arquivo de índice (o mais próximo dos dados) é, por vezes, chamado de **base**. Ele pode ser um índice primário esparso, por exemplo. Só exigimos um nível subsequente de índice se o nível anterior não couber em um único bloco de disco. O último nível, no topo da hierarquia, é conhecido como **nível raiz** ou **topo**, e idealmente consiste em um único bloco. Veja um exemplo de um índice primário de dois níveis na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/700x500/EEE/31343C?text=Figura+19.28+-+Índice+Primário+de+Dois+Níveis" alt="Figura 19.28 - Um índice primário de dois níveis">

&lt;figcaption>Figura 19.28 - Um índice primário de dois níveis&lt;/figcaption>

&lt;/div>

A estrutura de índice multinível mais famosa e amplamente utilizada na prática em SGBDs é a **Árvore B+ (B+-tree)**, que é uma variação da Árvore B. As Árvores B+ são árvores de busca balanceadas, o que significa que todos os caminhos da raiz até uma folha têm o mesmo comprimento. Isso garante um desempenho de busca consistente. Elas possuem nós internos (que contêm apenas chaves de índice e ponteiros para outros nós) e nós folha (que contêm as chaves de índice e os ponteiros para os registros de dados reais, ou os próprios dados, no caso de um índice clusterizado). Além disso, os nós folha de uma Árvore B+ são interligados em uma lista duplamente encadeada, o que permite uma varredura sequencial eficiente dos dados na ordem da chave do índice.

##### **19.7.4 Outros Tipos de Índices**

Vamos tratar agora de outros tipos de índices que possuem abordagens diferentes dos índices ordenados baseados em árvores.

###### **Índices de Hash**

Já discutimos o _hashing_ como uma técnica de organização primária de arquivos. Ele também pode ser usado para criar **índices de hash**. Um índice de hash consiste em uma coleção de **buckets** (baldes) organizados em uma estrutura (geralmente um array ou diretório). Uma **função de hash** mapeia os valores das chaves de índice para os buckets correspondentes no índice de hash. Cada bucket, por sua vez, contém ponteiros para os registros de dados que possuem aquele valor de chave (ou cujas chaves foram mapeadas para aquele bucket). A figura a seguir mostra três chaves de índice sendo mapeadas para três buckets diferentes no índice de hash por uma função f(x). Índices de hash são extremamente rápidos para buscas por igualdade, mas não suportam buscas por faixa de valores de forma eficiente, pois não mantêm as chaves ordenadas.

&lt;div align="center">

&lt;img src="https://placehold.co/600x350/EEE/31343C?text=Figura+19.29+-+Índices+de+Hash" alt="Figura 19.29 - Chaves de índice mapeadas para buckets com uma função de hash f(x)">

&lt;figcaption>Figura 19.29 - Chaves de índice mapeadas para buckets com uma função de hash f(x)&lt;/figcaption>

&lt;/div>

###### **Índices de Bitmap**

O **índice de bitmap** é um tipo especial de índice que pode ser extremamente eficiente para otimizar consultas que utilizam como filtro colunas que possuem **baixa cardinalidade**, ou seja, colunas que possuem poucos valores distintos em relação ao número total de linhas de uma tabela (por exemplo, colunas como "Gênero", "Estado Civil", "Status do Pedido", ou "País" em uma tabela de clientes globais).

Ao criar um índice de bitmap em uma coluna, o SGBD monta um **mapa de bits** para cada valor distinto possível da coluna. Cada mapa de bits é um vetor com um bit para cada linha da tabela. Para um determinado valor (por exemplo, "Casado" na coluna "Estado Civil"), o SGBD grava um bit `1` na posição correspondente do bitmap se a linha tiver aquele valor, e um bit `0` se não tiver. Veja o exemplo na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/700x400/EEE/31343C?text=Figura+19.30+-+Índice+de+Bitmap" alt="Figura 19.30 - Exemplo de um índice de bitmap em uma coluna de baixa cardinalidade">

&lt;figcaption>Figura 19.30 - Exemplo de um índice de bitmap em uma coluna de baixa cardinalidade&lt;/figcaption>

&lt;/div>

A grande vantagem dos índices de bitmap é que consultas complexas com múltiplas condições (`AND`, `OR`, `NOT`) em colunas de baixa cardinalidade podem ser resolvidas muito rapidamente, realizando operações lógicas de bits diretamente sobre os bitmaps, antes mesmo de acessar a tabela. Por exemplo, para encontrar "mulheres casadas", o SGBD pode pegar o bitmap para "Gênero = Feminino" e o bitmap para "Estado Civil = Casado" e realizar uma operação `AND` bit a bit entre eles. O bitmap resultante indicará exatamente quais linhas satisfazem ambas as condições. A desvantagem é que eles não são eficientes para colunas de alta cardinalidade e podem ser caros para atualizar em tabelas com alta frequência de transações de escrita.

###### **Indexação Baseada em Função**

O algoritmo do otimizador de consultas dos SGBDs comerciais, por padrão, geralmente desconsidera o uso de um índice associado a uma coluna se essa coluna for referenciada dentro de uma função na cláusula `WHERE`. Por exemplo, se houver um índice na coluna `DATA_PEDIDO` e a consulta fizer `WHERE TO_CHAR(DATA_PEDIDO, 'YYYY') = '2024'`, o índice em `DATA_PEDIDO` provavelmente não será usado.

Para suplantar essa limitação, muitos SGBDs modernos suportam a **indexação baseada em função (_function-based indexing_)**. Esta técnica permite que índices (geralmente do tipo Árvore B+ ou de bitmap) sejam criados não sobre a coluna diretamente, mas sobre o resultado da aplicação de uma função – seja ela uma função padrão do SQL (como `UPPER()`, `TO_CHAR()`) ou uma função construída pelo desenvolvedor – ou mesmo de uma expressão aritmética. A sentença de criação de um índice baseado em função seria semelhante à mostrada na figura abaixo.

&lt;div align="center">

&lt;img src="https://placehold.co/600x200/EEE/31343C?text=Figura+19.31+-+Criação+de+Índice+Baseado+em+Função" alt="Figura 19.31 - Exemplo de sintaxe para criação de um índice baseado em função">

&lt;figcaption>Figura 19.31 - Exemplo de sintaxe para criação de um índice baseado em função&lt;/figcaption>

&lt;/div>

Ao criar um índice em `UPPER(NOME_CLIENTE)`, por exemplo, o SGBD armazenará os valores de `NOME_CLIENTE` em maiúsculas no índice. Uma consulta que utilize `WHERE UPPER(NOME_CLIENTE) = 'JOÃO'` poderá, então, usar este índice de forma eficiente.

#### **19.8 Breve Comentário sobre Indexação de Textos**

O nosso assunto teórico principal está concluído. Antes de finalizarmos, porém, gostaria de fazer um rápido comentário sobre a indexação de textos, um campo especializado da recuperação de informação que é crucial para bancos de dados que armazenam grandes volumes de dados não estruturados ou semi-estruturados, como documentos, artigos, e-mails ou páginas web.

Existem dois métodos principais de busca por palavras ou frases em bancos de dados textuais que utilizam indexação de textos: os **arquivos invertidos** e os **índices para a próxima palavra (ou N-gramas)**.

Um **arquivo invertido** é a abordagem mais comum e fundamental. Ele possui duas partes principais:

1. Uma estrutura de busca, chamada de **vocabulário** (ou dicionário), contendo todos os termos (palavras) distintos existentes nos textos indexados.
2. Para cada termo no vocabulário, uma **lista invertida** que armazena os identificadores dos documentos (ou registros) que contêm aquele termo. Frequentemente, a lista invertida também armazena informações adicionais, como a frequência do termo no documento ou as posições exatas onde ele ocorre (para buscas por frases).

Consultas por um único termo são feitas obtendo-se a lista invertida correspondente ao termo procurado. Consultas booleanas (`AND`, `OR`) são resolvidas realizando-se operações de interseção ou união entre as listas invertidas relativas aos termos presentes na consulta.

Os **índices para a próxima palavra (_next-word indexes_)** apresentam uma abordagem que pode ser mais eficiente do que o uso de arquivos invertidos com contadores de posição para buscas por frases exatas. Nessa abordagem, para cada palavra existente no vocabulário, o índice cria uma lista com as palavras que ocorrem em uma posição imediatamente subsequente no texto, juntamente com apontadores de posição para essas ocorrências. Um índice para a próxima palavra consiste, portanto, em um vocabulário de palavras distintas e, para cada uma dessas palavras _w_, uma lista com as próximas palavras _s_ que a sucedem. A lista armazena, para cada par _w-s_, uma sequência de localidades (documento e posição) onde esse par ocorreu. No primeiro nível de um índice como este, teríamos as palavras do vocabulário. Cada palavra no primeiro nível conteria um ponteiro para uma estrutura (como uma árvore ou tabela de hash) das suas próximas palavras. Cada próxima palavra, por sua vez, apontaria para uma lista de documentos e posições. Através do encadeamento dessas posições, podemos testar a ocorrência de frases completas de forma muito eficiente. A figura abaixo ilustra uma possível hierarquia para um índice de próxima palavra.

&lt;div align="center">

&lt;img src="https://placehold.co/700x500/EEE/31343C?text=Figura+19.32+-+Hierarquia+de+um+Índice+de+Próxima+Palavra" alt="Figura 19.32 - Hierarquia conceitual de um índice para a próxima palavra">

&lt;figcaption>Figura 19.32 - Hierarquia conceitual de um índice para a próxima palavra&lt;/figcaption>

&lt;/div>

---

#### **Considerações Finais**

Neste capítulo, mergulhamos nas camadas mais profundas de um sistema de banco de dados, explorando como os dados são fisicamente organizados, armazenados e acessados. Vimos que o desempenho, a eficiência e a própria funcionalidade de um SGBD dependem criticamente das decisões tomadas no nível físico, desde a hierarquia de armazenamento, passando pela estrutura dos discos, até as complexas estratégias de organização de arquivos e indexação.

Iniciamos com a compreensão da hierarquia de memória e da mecânica dos discos rígidos, estabelecendo que o acesso ao armazenamento secundário é uma das operações mais custosas e, portanto, um dos principais alvos de otimização. A forma como os registros e arquivos são dispostos em blocos no disco, seja de forma ordenada, desordenada (_heap_) ou através de _hashing_, define os métodos de acesso primário aos dados e estabelece um _trade-off_ fundamental entre a velocidade de inserção e a de recuperação.

Avançamos para o estudo das estruturas de acesso auxiliares, os **índices**, que se revelaram ferramentas indispensáveis para acelerar as buscas. Detalhamos os diferentes tipos de índices ordenados (primário, de agrupamento e secundário), a distinção entre índices densos e esparsos, e a evolução para estruturas multinível, como as onipresentes Árvores B+, que são o pilar da indexação na maioria dos SGBDs relacionais modernos. Também exploramos abordagens especializadas, como índices de bitmap e baseados em função, que oferecem soluções otimizadas para cenários de consulta específicos.

Além disso, abordamos tecnologias de armazenamento robustas como o **RAID**, que combina paralelismo e redundância para melhorar tanto o desempenho quanto a confiabilidade do subsistema de disco, e contextualizamos as arquiteturas modernas de armazenamento como DAS, NAS e SAN.

Fica claro que não existe uma única "melhor" forma de organizar e acessar dados. A escolha da organização de arquivo, da estratégia de indexação e da tecnologia de armazenamento corretas depende de uma análise cuidadosa dos padrões de acesso da aplicação: a frequência de leituras versus escritas, os tipos de consulta mais comuns (buscas por chave, por faixa, textuais), e os requisitos de desempenho e consistência.

O conhecimento adquirido neste capítulo é essencial para qualquer profissional de banco de dados, seja ele um administrador (DBA) responsável pelo tuning do sistema, ou um desenvolvedor que precisa escrever consultas eficientes. Compreender como o SGBD interage com o disco e como as estruturas de índice funcionam "por baixo dos panos" é o que permite tomar decisões de projeto informadas e diagnosticar problemas de desempenho de forma eficaz, transformando a teoria da organização de arquivos em resultados práticos e mensuráveis.

&lt;/immersive>
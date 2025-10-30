# Capítulo 9 – Redes Sem Fio

Nos capítulos anteriores, construímos uma base sólida sobre o funcionamento das redes cabeadas. Exploramos os protocolos que previnem loops (STP), organizam o tráfego em domínios de broadcast (VLANs) e definem a comunicação na rede local (Ethernet). Todas essas tecnologias presumem um meio físico controlado, confinado e relativamente seguro: o cabo. Agora, adentramos um mundo fundamentalmente diferente, o das **redes sem fio (WLANs)**, onde o meio de transmissão é o ar. Essa mudança de meio, do físico para o etéreo, introduz um conjunto inteiramente novo de desafios, que vão desde o controle de acesso ao meio (como evitar "colisões" no ar) até, e mais crucialmente, a segurança. Em uma rede cabeada, um invasor precisa de acesso físico. Em uma rede sem fio, qualquer pessoa ao alcance do sinal pode "ouvir" o tráfego e tentar se conectar. Por essa razão, antes mesmo de explorarmos como os dados viajam, precisamos entender como controlamos _quem_ pode entrar na rede.

## 802.1X: Padrão de Autenticação de Rede

O primeiro desafio em qualquer rede, mas especialmente em redes sem fio, é a **autenticação**: o processo de verificar a identidade de um usuário ou dispositivo antes de permitir seu acesso. Uma senha simples compartilhada (como o WPA2-Personal, ou "a senha do Wi-Fi") é frágil em ambientes corporativos. Se um funcionário sai da empresa, a senha precisa ser trocada em todos os dispositivos, o que é impraticável.

Para resolver isso, o IEEE criou o padrão **802.1X**. Este protocolo não é exclusivo de redes sem fio, mas é amplamente utilizado nelas. Seu objetivo é prover um mecanismo robusto e centralizado de **autenticação, controle de acesso e distribuição de chaves**, tudo isso **baseado em portas**. É importante mencionar que, embora seja amplamente mencionado e utilizado em redes sem fio, ele também é perfeitamente aplicável em redes com fio (controlando quem pode se conectar a uma porta específica de um switch).

O padrão 802.1X é incrivelmente versátil, pois foi projetado para ser independente das camadas inferiores (pode rodar sobre Wi-Fi, Ethernet, etc.) e das camadas superiores (pode usar diferentes métodos de autenticação). Ele segue uma arquitetura cliente-servidor e utiliza um método de desafio-resposta (_challenge-response_) para validar as credenciais.

### Os Três Elementos do 802.1X

Uma arquitetura 802.1X é composta por três elementos ou papéis fundamentais:

1. **Suplicante (Supplicant):** É o dispositivo cliente que deseja se autenticar e obter acesso à rede (ex: um notebook, smartphone ou computador de mesa). O suplicante executa um software que entende o protocolo 802.1X.
2. **Autenticador (Authenticator):** É o equipamento de infraestrutura que atua como o "porteiro" da rede. Ele bloqueia ou libera o acesso à porta (física ou virtual). O autenticador não toma a decisão de autenticação; ele simplesmente atua como um intermediário.
3. **Servidor de Autenticação (Authentication Server - AS):** É o "cérebro" da operação. É um sistema de back-end que armazena (ou tem acesso) à base de dados de usuários, senhas e certificados. Ele recebe as credenciais do suplicante (encaminhadas pelo autenticador) e toma a decisão final de "Aprovar" ou "Negar" o acesso.

Em um exemplo concreto de rede sem fio do padrão 802.11, teríamos:

- A estação (notebook) que deseja acesso é o **Suplicante**.
- O **Access Point (AP)** é o **Autenticador**.
- Um servidor **RADIUS (Remote Authentication Dial-In User Service)** é o **Servidor de Autenticação**.

A figura abaixo nos apresenta esse cenário:

<div align="center">
<img width="500px" src="./img/09-rede-sem-fio-80211.png">
</div>

Como a imagem ilustra, o Access Point (Autenticador) mantém a porta virtual bloqueada ("Unauthorized") até que o Suplicante forneça suas credenciais (usuário/senha). O AP encaminha essas credenciais para o Servidor RADIUS, que as verifica em sua base. Se as credenciais estiverem corretas, o RADIUS envia uma mensagem de "Sucesso" de volta ao AP, que então "abre" a porta para o Suplicante, permitindo que ele acesse a rede ("Authorized").

É importante ressaltar que o padrão não define a disposição do servidor de autenticação. Em redes corporativas (Enterprise), ele é um servidor dedicado. Em soluções de escritórios pequenos, o Servidor de Autenticação (uma versão simplificada) pode estar embutido no próprio Access Point, não sendo um elemento dedicado.

### Protocolo de Encapsulamento: EAP e EAPOL

O 802.1X em si não é um protocolo de autenticação. Ele é um _framework_ de controle de acesso. Para transportar as mensagens de autenticação, ele utiliza um protocolo de encapsulamento chamado **EAP (Extensible Authentication Protocol)**.

O "E" de EAP é sua característica mais importante: **Extensível**. O EAP foi projetado para ser um contêiner flexível, capaz de carregar diversos tipos de protocolos de autenticação dentro dele. Para que o EAP possa viajar sobre uma rede local (LAN), ele é encapsulado usando o **EAPOL (EAP over LAN)**.

A figura a seguir demonstra essa arquitetura em camadas:

<div align="center">
<img width="400px" src="./img/09-8021x-encapsulamento.png">
</div>

Essa estrutura permite que o 802.1X agregue todos os recursos de autenticação de tecnologias robustas. A camada de Autenticação no topo pode usar certificados (TLS), senhas (CHAP) ou credenciais de domínio (Kerberos). O EAP "empacota" essas mensagens, e o EAPOL as transporta pela rede (seja ela 802.3 Ethernet, 802.11 Wi-Fi, etc.), permitindo autenticar tanto dispositivos quanto usuários em um mesmo sistema.

### Métodos de Autenticação EAP

Na camada de autenticação, diversos protocolos são suportados. A escolha do método EAP define o nível de segurança e a complexidade de implementação. Os mais comuns (e incomuns) incluem:

- **EAP-MD5:** Baseado em usuário e senha, usando o hash MD5. É um método antigo e **inseguro**. Não há autenticação mútua (o cliente não sabe se está falando com um servidor legítimo) e ele não possibilita a geração de chaves de criptografia dinâmicas. **Não deve ser usado.**
- **EAP-TLS (Transport Layer Security):** É um dos métodos mais seguros. Requer certificados digitais tanto no lado do _servidor_ quanto no lado do _cliente_. Isso permite a autenticação mútua (o cliente verifica o servidor, e o servidor verifica o cliente). Ele gera chaves de criptografia dinâmicas por usuário e por sessão. Sua principal desvantagem é a dificuldade de gerenciamento, pois exige a instalação e manutenção de um certificado em cada dispositivo cliente (suplicante).
- **EAP-TTLS (Tunneled TLS):** Uma extensão do EAP-TLS que resolve o problema do certificado do cliente. Ele usa um certificado _apenas no lado do servidor_. O cliente usa esse certificado para criar um "túnel" TLS seguro e, _dentro_ desse túnel, envia suas credenciais de forma segura (geralmente usuário e senha). Também gera chaves dinâmicas.
- **EAP-PEAP (Protected EAP):** Muito similar ao EAP-TTLS, também usa usuário e senha com autenticação mútua (o cliente valida o servidor pelo certificado). Foi desenvolvido pela Microsoft e é hoje um dos métodos mais comuns em redes corporativas (WPA2-Enterprise).
- **EAP-LEAP (Lightweight EAP):** Um protocolo proprietário da Cisco, baseado em usuário e senha. Foi popular, mas hoje é considerado menos seguro que PEAP ou TLS.
- **EAP-FAST (Flexible Authentication via Secure Tunneling):** Também da Cisco, foi criado como um substituto do LEAP. Não usa certificados para criar o túnel, mas sim uma credencial pré-compartilhada chamada PAC (Protected Access Credential).

A tabela a seguir resume as principais características desses tipos de EAP:

|**802.1X EAP, Tipos e Característica**|**MD5 (Message Digest 5)**|**TLS (Transport Level Security)**|**TTLS (Tunneled Transport Level Security)**|**PEAP (Protected Transport Level Security)**|**FAST (Flexible Authentication via Secure Tunneling)**|**LEAP (Lightweight Extensible Authentication Protocol)**|
|---|---|---|---|---|---|---|
|**Certificado do lado do Cliente**|Não|**Sim**|Não|Não|Não (PAC)|Não|
|**Certificado do lado do Servidor**|Não|**Sim**|**Sim**|**Sim**|Não (PAC)|Não|
|**Geração de Chave Dinâmica**|Não|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|
|Rogue AP detection|Não|Não|Não|Não|Sim|Sim|
|Provedor|MS|MS|Funk|MS|Cisco|Cisco|
|**Atributos de Autenticação**|Via única|**Mútuo**|**Mútuo**|**Mútuo**|**Mútuo**|**Mútuo**|
|**Dificuldade de Implementação**|Fácil|**Difícil** (devido o certificado no cliente)|Moderado|Moderado|Moderado|Moderado|
|**Segurança Wi-Fi**|**Fraca**|Muito Alta|Alta|Alta|Alta|Alta (com senhas fortes)|

### Fluxo de Mensagens do 802.1X

Para termos o devido registro, é importante também observarmos o funcionamento e fluxo de mensagens aplicado aos protocolos. A imagem a seguir detalha a "conversa" entre os três componentes:

<div align="center">
<img width="500px" src="./img/09-8021x-funcionamento-e-fluxo.png">
</div>

Nesse contexto, podemos elencar quatro tipos de mensagens que definem o processo: **1. Requisição**, **2. Resposta**, **3. Sucesso** e **4. Falha**.

O fluxo ocorre da seguinte forma:

1. **Início (Requisição):** O Suplicante (cliente) se associa ao Ponto de Acesso (Autenticador) e envia uma "Requisição de Conexão" (na prática, uma mensagem EAPOL-Start).
2. **Identidade (Requisição/Resposta):** O Ponto de Acesso responde com uma "Requisição de Identificação" (EAP-Request Identity). O Suplicante envia sua identidade (ex: `usuario@empresa.com`) em uma "Resposta de Identificação".
3. **Encaminhamento para o RADIUS:** O Ponto de Acesso, que não conhece esse usuário, encapsula essa identidade e a envia ao Servidor RADIUS em uma "Requisição de Acesso" (RADIUS-Access-Request).
4. **Desafio (Requisição/Resposta):** O Servidor RADIUS inicia o "desafio" EAP (como EAP-PEAP ou EAP-TLS). Ele envia um "Desafio de Conexão" (RADIUS-Challenge) ao Ponto de Acesso, que o repassa ao Suplicante ("Requisição de Desafio").
5. **Autenticação EAP:** O Suplicante responde ao desafio ("Resposta ao Desafio"), e o Ponto de Acesso atua como um "proxy" transparente, encaminhando as mensagens de ida e volta entre o Suplicante e o RADIUS até que o protocolo EAP seja concluído.
6. **Decisão (Sucesso/Falha):** O Servidor RADIUS toma a decisão final. Ele envia uma mensagem de "Sucesso de Conexão" (RADIUS-Access-Accept) ou "Falha de Conexão" (RADIUS-Access-Reject) para o Ponto de Acesso.
7. **Abertura da Porta:** O Ponto de Acesso recebe a decisão. Se for "Sucesso", ele abre a porta e retransmite a mensagem de sucesso ao Suplicante. Se for "Falha", ele mantém a porta fechada e informa o Suplicante.
8. **Troca de Chaves:** No caso de sucesso, a mensagem do RADIUS (RADIUS-Access-Accept) contém o material criptográfico (chaves) que o Ponto de Acesso e o Suplicante usarão para criptografar o tráfego da sessão (a "Troca de Chaves WPA").

## Arquitetura e Topologias de Redes 802.11

As redes sem fio, comumente conhecidas pelo seu padrão mais popular, **IEEE 802.11** (ou Wi-Fi), fazem parte de um dos padrões de implementação definidos pelo IEEE, derivados das redes com fio. No entanto, como o meio de transmissão (o ar) é fundamentalmente diferente, o padrão 802.11 define sua própria arquitetura e seus próprios "blocos de construção" para a rede.

O padrão traz dois conceitos base, chamados de tipos de serviços, que definem a arquitetura de uma rede sem fio:

1. **BSS (Basic Service Set)**: O Conjunto de Serviços Básicos.
2. **ESS (Extended Service Set)**: O Conjunto de Serviços Estendido.

### BSS (Basic Service Set)

Define-se como **BSS** a base, ou o bloco de construção fundamental, de uma rede local sem fio (WLAN). Um BSS é formado por um grupo de estações (dispositivos fixos ou móveis, como notebooks e smartphones) que podem se comunicar entre si.

Existem dois modos principais de operação para um BSS, que definem drasticamente a topologia e o funcionamento da rede.

#### Modo Infraestrutura (BSS com Ponto Central)

Este é o modo de operação mais comum, utilizado em residências, escritórios e ambientes públicos. Nas redes com ponto central, a arquitetura segue a mesma analogia de uma topologia em estrela, em que **toda a comunicação deve passar pelo nó central**.

Esse nó central é o **Access Point (AP)**, ou Ponto de Acesso. Neste modo, os dispositivos clientes (estações) não se comunicam diretamente entre si. Mesmo que dois notebooks estejam na mesma sala, o tráfego de um para o outro deve, obrigatoriamente, fluir do notebook 1, subir até o AP, e então descer do AP para o notebook 2.

Esse design centralizado é crucial por vários motivos:

- O AP atua como o **Autenticador** (como vimos no 802.1X), controlando quem pode ou não entrar na rede.
- O AP atua como uma **ponte (bridge)**, conectando os dispositivos sem fio à rede cabeada (Ethernet), permitindo que eles acessem recursos como servidores e a Internet.
- O AP gerencia o acesso ao meio, evitando colisões de rádio.

Uma rede que utiliza este modo é chamada de **rede de infraestrutura**. O termo **Hotspot** é comumente aplicado tanto ao local físico que disponibiliza o acesso com essa infraestrutura (ex: um café, aeroporto) quanto, em alguns contextos, ao próprio Access Point.

<div align="center">
<img width="400px" src="./img/09-rede-com-ponto-central.png">
</div>

A imagem acima ilustra perfeitamente uma rede de infraestrutura. O "Hardware Access Point" atua como a ponte central, servindo os dispositivos da "Wireless Network" e conectando-os à "Wired Ethernet Network" (rede cabeada), onde residem outros recursos como o "File Server".

#### Modo Ad-Hoc (BSS Independente)

Já nas redes que não utilizam um ponto de acesso central, os dispositivos operam no modo **Ad-Hoc**. Neste modo, as estações são capazes de se comunicarem **diretamente entre si** em uma topologia ponto-a-ponto ou malha (mesh).

Em uma rede Ad-Hoc (também chamada de **IBSS - Independent Basic Service Set**), não há um "chefe" de rede. Cada dispositivo descobre seus vizinhos e estabelece conexões diretas. Em redes mais avançadas (como redes _mesh_ móveis ou MANETs), os dispositivos também podem atuar como roteadores, repassando a informação para outros dispositivos que estão fora do alcance de rádio, embora isso não seja comum em implementações Wi-Fi simples.

<div align="center">
<img width="400px" src="./img/09-rede-ad-hoc.png">
</div>

Este modo é útil para situações temporárias onde não há infraestrutura de rede, como a criação de uma rede rápida entre dois notebooks para transferir um arquivo, ou em redes de sensores e veículos.

### ESS (Extended Service Set)

Um único BSS (ou um único AP) tem uma área de cobertura limitada. Em um prédio de escritórios, um campus universitário ou até mesmo uma casa grande, um AP não é suficiente. Para resolver isso, o padrão 802.11 define o **ESS (Extended Service Set)**.

Um ESS é formado por **duas ou more BSSs** interconectadas. Para formar um ESS, as BSSs devem, obrigatoriamente, ser do tipo infraestrutura (utilizar APs).

A conexão entre as BSSs (ou seja, entre os APs) se dá por um **Sistema de Distribuição (DS - Distribution System)**. Na prática, o DS é quase sempre a **rede local cabeada (Ethernet)** à qual os Access Points estão conectados.

<div align="center">
<img width="400px" src="./img/09-rede-ess.png">
</div>

A imagem acima demonstra um ESS:

- Existem três APs, e cada um cria sua própria BSS (a "bolha" de cobertura de rádio).
- Os três APs estão conectados ao "Sistema de Distribuição" (a rede cabeada), que por sua vez se conecta a um servidor ou gateway de Internet.
- O conjunto dessas três BSSs, unidas pelo DS, forma um único e grande ESS.

A principal vantagem de um ESS é que ele permite a **mobilidade (roaming)**. Para os dispositivos clientes, o ESS inteiro se parece com uma única e grande rede.

### Tipos de Transição (Mobilidade)

Conforme já comentamos, dentro de uma mesma BSS, os dispositivos se comunicam mediados pelo AP. Agora, com múltiplas BSSs no contexto de uma ESS, podemos ter equipamentos que estão em uma zona de alcance de duas BSSs (como os dispositivos nos pontos de intersecção dos círculos na figura).

Esse cenário nos leva a três tipos de mobilidade definidos pelo IEEE 802.11:

1. **Sem Transição:** Uma estação deste tipo é fixa (como um computador de mesa com Wi-Fi) ou se movimenta apenas dentro da área de cobertura (BSS) de seu AP original.
2. **Transição Inter-BSS (Roaming):** Esta é a mobilidade mais importante. Uma estação com mobilidade de transição inter-BSS pode se movimentar de uma BSS para outra (ou seja, "pular" do AP1 para o AP2) _dentro do mesmo ESS_. Como ambos os APs estão conectados ao mesmo Sistema de Distribuição (a mesma rede lógica), essa transição (chamada de _hand-off_ ou _roaming_) pode ocorrer de forma transparente, sem que o usuário perca sua conexão ou seu endereço IP.
3. **Transição Inter-ESS:** Uma estação com mobilidade de transição inter-ESS pode se movimentar de um ESS para outro (por exemplo, sair da rede Wi-Fi do escritório e se conectar à rede Wi-Fi do café). O padrão IEEE 802.11 **não assegura** que a comunicação será contínua durante essa transição, pois, para a rede, o dispositivo está se desconectando de uma rede lógica e se conectando a uma completamente nova e diferente.

### Padrão IEEE 802.11 (Wi-Fi)

Já tivemos a oportunidade de mencionar sobre a dinâmica e a diferença fundamental entre o **802.3 (Ethernet)** e o **802.11 (Wi-Fi)**. Vimos que, embora ambos compartilhem a mesma subcamada superior de enlace (a **LLC**), suas subcamadas inferiores (**MAC**) são completamente distintas, pois seus meios físicos são distintos.

Enquanto o 802.3 utiliza o **CSMA/CD** (Detecção de Colisão) em um meio cabeado confiável, o 802.11, operando no ar (um meio caótico e imprevisível), deve utilizar o **CSMA/CA** (Prevenção de Colisão).

Entretanto, há outra diferença crucial e ainda mais visível entre os dois: o seu **cabeçalho de quadro**. O quadro Ethernet (802.3) é simples, rápido e enxuto. O quadro 802.11, por outro lado, é significativamente mais complexo, pois precisa gerenciar um conjunto de problemas que não existem no mundo cabeado: clientes dormindo, autenticação no ar, fragmentação de pacotes e a necessidade de múltiplos endereços para navegar pela rede sem fio.

#### Estrutura do Cabeçalho MAC 802.11

Sendo assim, vamos conhecer a sua estrutura e os seus campos.

<div align="center">
<img width="600px" src="./img/09-wi-fi-cabecalho.png">
</div>

Ao observar o cabeçalho MAC 802.11, algumas diferenças em relação ao quadro Ethernet saltam à vista:

- **Tamanho:** O cabeçalho é muito maior.
- **Frame Control (Controle de Quadro):** Um campo de 2 bytes que define tudo sobre o quadro.
- **Duration/ID (Duração/ID):** Usado para o CSMA/CA, para que uma estação possa "reservar" o meio por um determinado tempo.
- **Múltiplos Endereços:** O quadro 802.11 possui **quatro campos de endereço** (Address 1 a 4), cada um com 6 bytes (48 bits).

Estamos diante de uma dinâmica interessante, pois os campos `Address 1` a `Address 4` podem assumir funções e significados totalmente diferentes (Endereço de Destino, Fonte, Receptor, Transmissor, BSSID, etc.) a depender dos parâmetros definidos no campo **`Frame Control`**.

Portanto, para entender o quadro 802.11, devemos primeiro entender os subcampos no contexto do campo `Frame Control`.

#### Campo "Frame Control"

O campo `Frame Control`, de 2 bytes, é o "cérebro" do quadro 802.11. Ele define a função do quadro e como os outros campos devem ser interpretados.

<div align="center">
<img width="600px" src="./img/09-wi-fi-cabecalho-frame-control.png">
</div>

No quadro de controle, temos:

- **Protocol Version (Versão do Protocolo) [2 bits]:** Indica a versão corrente do protocolo 802.11 utilizado. As estações receptoras usam esse valor para determinar se a versão do protocolo do quadro recebido é suportada.
- **Type e Subtype (Tipo e Subtipo) [2 + 4 bits]:** Determina a função exata do quadro. Existem três tipos principais, cada um com vários subtipos:
    1. **Quadros de Gerenciamento (Management Frames):** Usados para descobrir, associar, autenticar e se desconectar de um AP (ex: _Beacon_, _Probe Request/Response_, _Association Request_).
    2. **Quadros de Controle (Control Frames):** Usados para gerenciar o acesso ao meio. São o coração do CSMA/CA (ex: _RTS - Request to Send_, _CTS - Clear to Send_, _ACK - Acknowledgment_).
    3. **Quadros de Dados (Data Frames):** Usados para carregar o payload (os dados do usuário, como um pacote IP).
- **To DS e From DS (Para/Do Sistema de Distribuição) [1 + 1 bit]:** Estes são os dois bits mais importantes para o endereçamento. Eles indicam se o quadro está indo _para_ o Sistema de Distribuição (DS) (a rede cabeada) ou se está vindo _do_ DS. A combinação desses dois bits determina o significado dos 4 campos de endereço.
- **More Fragments (Mais Fragmentos) [1 bit]:** O 802.11 permite a fragmentação de quadros grandes. Como o ar é um meio propenso a erros, é mais eficiente enviar pedaços menores (fragmentos) do que um quadro grande. Se um fragmento pequeno se corromper, apenas ele precisa ser retransmitido. Este bit indica se mais fragmentos do quadro estão vindo.
- **Retry (Retransmissão) [1 bit]:** Indica se a informação (dado ou gerenciamento) está ou não sendo retransmitida. O 802.11 é um protocolo de enlace _confiável_ (ao contrário do Ethernet) e espera um ACK para cada quadro. Se o ACK não chegar, o quadro é retransmitido com este bit ativado.
- **Power Management (Gerenciamento de Energia) [1 bit]:** Indica se a estação que transmitiu a informação está em _active mode_ (modo ativo) ou em _power-save-mode_ (modo economia de energia). Essencial para a vida útil da bateria de dispositivos móveis.
- **More Data (Mais Dados) [1 bit]:** Usado pelo AP para "avisar" uma estação que está em _power-save-mode_ que ele (o AP) tem mais quadros armazenados em buffer para enviar a ela.
- **WEP (ou "Protected Frame") [1 bit]:** Indica se o corpo do quadro (o payload) está ou não criptografado.
- **Order (Ordem) [1 bit]:** Indica se todos os quadros de dados recebidos devem ser processados em ordem estrita.

#### Esquema de Quatro Endereços

E aqui começa a dinâmica: a depender da combinação dos bits `To DS` e `From DS` no campo `Frame Control`, teremos informações e papéis diferentes para os quatro campos de endereço.

Para entender a tabela a seguir, precisamos definir os 5 papéis de endereço que um quadro 802.11 pode conter:

- **DA (Destination Address):** O endereço MAC do **destino final** do pacote (ex: o servidor na Internet).
- **SA (Source Address):** O endereço MAC da **fonte original** do pacote (ex: o notebook).
- **RA (Receiver Address):** O endereço MAC da **próxima estação** que irá receber fisicamente o quadro no _meio sem fio_ (ex: o AP).
- **TA (Transmitter Address):** O endereço MAC da estação que **transmitiu fisicamente** o quadro no _meio sem fio_ (ex: o notebook ou o AP).
- **BSSID (Basic Service Set Identifier):** O identificador único do BSS.
    - Em modo **Infraestrutura**, o BSSID é o endereço MAC do Access Point.
    - Em modo **Ad-Hoc (IBSS)**, o BSSID é um endereço MAC gerado aleatoriamente pela primeira estação que iniciou a rede.

A tabela abaixo ilustra como os campos de endereço são mapeados com base nos bits `To DS` e `From DS`:

|**To DS**|**From DS**|**Significado (Caso de Uso)**|**End. 1 (RA)**|**End. 2 (TA)**|**End. 3**|**End. 4**|
|---|---|---|---|---|---|---|
|0|0|Comunicação Ad-Hoc (IBSS) ou _Direct Link_|**DA**|**SA**|BSSID|-|
|0|1|**Do AP para o Cliente** (Saindo do DS)|**DA** (Cliente)|**TA** (AP)|**SA** (Servidor)|-|
|1|0|**Do Cliente para o AP** (Indo para o DS)|**RA** (AP)|**SA** (Cliente)|**DA** (Servidor)|-|
|1|1|Wireless Distribution System (WDS - AP para AP)|**RA** (AP-2)|**TA** (AP-1)|**DA**|**SA**|

Entendendo os Cenários:

1. **To DS = 0, From DS = 0 (End. 1=DA, End. 2=SA, End. 3=BSSID):**
    - O quadro não vai para o DS e não vem do DS.
    - **Caso de Uso:** Um notebook em uma rede **Ad-Hoc** envia um quadro para outro notebook.
    - `End 1 (RA)` é o Destino (DA), pois o receptor é o destino.
    - `End 2 (TA)` é a Fonte (SA), pois o transmissor é a fonte.
    - `End 3` é o BSSID, para identificar a qual rede Ad-Hoc o quadro pertence.
2. **To DS = 1, From DS = 0 (End. 1=RA, End. 2=SA, End. 3=DA):**
    - O quadro vai _para_ o DS (rede cabeada).
    - **Caso de Uso:** Um **cliente Wi-Fi** (notebook) envia um quadro para um **servidor na rede cabeada**.
    - `End 1 (RA)` é o Receptor imediato: o **AP** (cujo MAC é o BSSID).
    - `End 2 (TA)` é o Transmissor imediato: o **Cliente Wi-Fi** (que é também a Fonte original, SA).
    - `End 3` é o Destino final: o **Servidor** na rede cabeada (DA).
3. **To DS = 0, From DS = 1 (End. 1=DA, End. 2=TA, End. 3=SA):**
    - O quadro vem _do_ DS.
    - **Caso de Uso:** Um **servidor na rede cabeada** envia um quadro para um **cliente Wi-Fi** (notebook).
    - `End 1 (RA)` é o Receptor imediato: o **Cliente Wi-Fi** (que é também o Destino final, DA).
    - `End 2 (TA)` é o Transmissor imediato: o **AP** (cujo MAC é o BSSID).
    - `End 3` é a Fonte original: o **Servidor** na rede cabeada (SA).
4. **To DS = 1, From DS = 1 (End. 1=RA, End. 2=TA, End. 3=DA, End. 4=SA):**
    - O quadro vem do DS e vai para o DS.
    - **Caso de Uso:** Este é o único cenário que usa todos os quatro endereços. É usado em um **Wireless Distribution System (WDS)**, onde um AP atua como "repetidor" (ponte) sem fio para outro AP.
    - `End 1 (RA)` é o **AP Receptor**.
    - `End 2 (TA)` é o **AP Transmissor**.
    - `End 3 (DA)` é o **Destino final** na rede cabeada.
    - `End 4 (SA)` é a **Fonte original** (o cliente que originou o quadro).

### Evolução dos Padrões 802.11

O padrão IEEE 802.11 não é uma especificação única e estática. Assim como o Ethernet evoluiu de 10 Mbps para 100 Gbps, o padrão Wi-Fi é uma família de emendas (amendments) que foram sendo lançadas ao longo dos anos. Cada nova emenda buscou, principalmente, aumentar a taxa de transmissão de dados (throughput) e melhorar a confiabilidade, seja utilizando novas faixas de frequência, modulações mais eficientes ou tecnologias de antena mais inteligentes.

A tabela a seguir é muito importante e resume os principais padrões "físicos" da família 802.11 e suas características centrais:

| **Padrão**   | **Banda (GHz)** | **Comprimento do Canal** | **Modulação**   | **Tecnologias de Antena**   | **Taxa de Referência (Mbits/s)** | **Observações**               |
| ------------ | --------------- | ------------------------ | --------------- | --------------------------- | -------------------------------- | ----------------------------- |
| **802.11a**  | 5               | 20 MHz                   | **OFDM**        | N/A                         | 54                               | Incompatível com o 802.11b    |
| **802.11b**  | 2,4             | 20 MHz                   | **DSSS**        | N/A                         | 11                               | -                             |
| **802.11g**  | 2,4             | 20 MHz                   | DSSS e **OFDM** | N/A                         | 54                               | Evolução do 802.11b           |
| **802.11n**  | 2,4 e 5         | 20 e 40 MHz              | OFDM            | **MIMO** (até 4 antenas)    | 600                              | Primeiro padrão a usar MIMO   |
| **802.11ac** | 5               | 40, 80 e 160 MHz         | OFDM            | **MU-MIMO** (até 8 antenas) | 7000 (aprox. 7 Gbps)             | Também conhecido como Wi-Fi 5 |
| **802.11ax** | 2,4, 5 e 6      | 20, 40, 80, 160 MHz      | **OFDMA**       | **MU-MIMO** (8 antenas)     | 9600 (aprox. 9.6 Gbps)           | Também conhecido como Wi-Fi 6 |

Para entender por que esses padrões são diferentes, precisamos analisar as tecnologias-chave que eles introduziram: a **modulação** (como o sinal é codificado no rádio) e a **tecnologia de antena**.

#### Tecnologias de Modulação e Espectro

O rádio é um ambiente "poluído", cheio de interferências. As técnicas de "Espalhamento Espectral" (Spread Spectrum) foram criadas para tornar a comunicação sem fio robusta contra essas interferências.

- **FHSS (Frequency Hopping Spread Spectrum):** Literalmente, "Espalhamento Espectral com Salto de Frequência". Esta foi uma das técnicas originais usadas no primeiro padrão 802.11. Basicamente, a partir de um sincronismo e controle entre emissor e receptor, o transmissor "salta" de um canal de frequência para outro em uma sequência pseudo-aleatória e pré-definida, permanecendo em cada canal por um curtíssimo período. Isso torna o sinal muito resiliente; se uma frequência específica estiver sofrendo interferência, o sinal só será afetado por uma pequena fração de segundo antes de saltar para uma frequência limpa.
    
    <div align="center">
    <img width="500px" src="./img/09-fhss.png">
    </div>
    
    A figura acima traz essa representação, em que as estações mudam as frequências (F1, F2, F3, F4) para transmissão em períodos de tempo determinados, seguindo padrões de salto diferentes para evitar colisões entre si.
- **DSSS (Direct Sequence Spread Spectrum):** "Espalhamento Espectral de Sequência Direta". Esta foi a técnica que tornou o Wi-Fi popular com o padrão **802.11b**. Em vez de "saltar" no tempo, o DSSS "espalha" o sinal por uma faixa de frequência larga _simultaneamente_. Ele faz isso pegando o fluxo de bits original e multiplicando-o por uma sequência de "chips" muito mais rápida (o código de espalhamento). O resultado é um sinal de baixa potência, mas muito largo, que se parece com ruído de fundo para qualquer receptor que não conheça o código de espalhamento. O padrão 802.11b utiliza uma técnica sucessora chamada **HR-DSSS (High Rate–DSSS)**, que permitiu que ele chegasse a taxas de 11 Mbps (um grande salto para a época).
- **OFDM (Orthogonal Frequency-Division Multiplexing):** "Multiplexação por Divisão de Frequência Ortogonal". Esta é a técnica de modulação que permitiu o salto para altas velocidades, sendo a base dos padrões **802.11a, g, n, e ac**. Em vez de usar um único sinal largo (como o DSSS), o OFDM divide o canal de 20MHz em dezenas de sub-portadoras (sub-canais) estreitas e independentes. Ele então transmite uma pequena parte dos dados em cada sub-portadora, em paralelo. A grande vantagem é a resiliência: se uma interferência atingir uma frequência específica, ela corromperá apenas algumas das 52 sub-portadoras, e não o sinal inteiro. Os dados perdidos são facilmente recuperados pelos códigos de correção de erro, permitindo taxas de transmissão muito mais altas e estáveis, como 54 Mbps.
- **MIMO (Multiple-Input Multiple-Output):** Introduzida pelo **802.11n**, esta não é uma técnica de modulação, mas de _antena_. Antes do 802.11n, os rádios usavam uma antena para transmitir e uma para receber. O MIMO utiliza múltiplas antenas para enviar e receber múltiplos fluxos de dados (chamados _spatial streams_) _ao mesmo tempo, no mesmo canal_. Isso multiplica a velocidade. Um rádio 802.11g tinha uma taxa de 54 Mbps. Um rádio 802.11n com 3 antenas (3x3 MIMO) poderia, teoricamente, atingir mais de 450 Mbps. O **MU-MIMO** (Multi-User MIMO), introduzido pelo 802.11ac, aprimorou isso, permitindo que um AP use suas múltiplas antenas para falar com _múltiplos dispositivos diferentes_ (ex: um celular e um notebook) exatamente ao mesmo tempo.

#### Evolução e Comparação dos Padrões

- **802.11a vs. 802.11b:** O padrão 802.11b não é considerado uma continuação do 802.11a, pois eles foram desenvolvidos em paralelo e são incompatíveis. O **802.11a** (54 Mbps com OFDM na banda de 5 GHz) era tecnicamente superior, mais rápido e operava em uma banda de frequência "limpa" (sem interferência de micro-ondas ou telefones sem fio). No entanto, o **802.11b** (11 Mbps com DSSS na banda de 2.4 GHz) "venceu" a adoção inicial do mercado. O motivo é que, embora a taxa de transmissão fosse menor, o sinal de 2.4 GHz tem um alcance muito maior (a nota menciona na ordem de 7 vezes mais) e os chips eram mais baratos de produzir.
- **802.11g:** Este sim foi um aperfeiçoamento e o sucessor direto do 802.11b. Ele foi o "melhor dos dois mundos": pegou a velocidade de 54 Mbps e a modulação OFDM do padrão "a", mas a implementou na banda de 2.4 GHz. Isso o tornou rápido e, ao mesmo tempo, manteve o grande alcance e a retrocompatibilidade com todos os dispositivos 802.11b existentes.

### Canais de Frequência e Sobreposição

A tecnologia de redes sem fio utiliza-se de canais de transmissão que refletem as subfaixas de frequência em que as informações são trafegadas. Os padrões 802.11 operam em faixas de frequência licenciadas (ISM - Industrial, Scientific and Medical), principalmente 2.4 GHz e 5 GHz.

- A banda de **2.4 GHz** (usada por 802.11b/g/n) vai de 2.4 GHz a 2.485 GHz.
- A banda de **5 GHz** (usada por 802.11a/n/ac) é muito mais larga, indo de 5.0 GHz a 5.8 GHz.

O problema central da banda de 2.4 GHz é a **sobreposição de canais (channel overlap)**. A banda é dividida em 14 canais (embora no Brasil e na maior parte das Américas, apenas os canais 1 a 11 sejam permitidos). Cada canal tem uma largura de 22 MHz, mas o centro de um canal está a apenas 5 MHz de distância do próximo.

A tabela a seguir detalha os canais da banda de 2.4 GHz:

|**Canal**|**Frequência Nominal**|**Frequência Prática (largura de 22MHz)**|
|---|---|---|
|**1**|**2.412 GHz**|**2.401 a 2.423 GHz**|
|2|2.417 GHz|2.405 a 2.428 GHz|
|3|2.422 GHz|2.411 a 2.433 GHz|
|4|2.427 GHz|2.416 a 2.438 GHz|
|5|2.432 GHz|2.421 a 2.443 GHz|
|**6**|**2.437 GHz**|**2.426 a 2.448 GHz**|
|7|2.442 GHz|2.431 a 2.453 GHz|
|8|2.447 GHz|2.436 a 2.458 GHz|
|9|2.452 GHz|2.441 a 2.463 GHz|
|10|2.457 GHz|2.446 a 2.468 GHz|
|**11**|**2.462 GHz**|**2.451 a 2.473 GHz**|
|12|2.467 GHz|2.456 a 2.478 GHz|
|13|2.472 GHz|2.461 a 2.483 GHz|
|14|2.482 GHz|2.466 a 2.485 GHz|

Quando várias redes sem fio sobrepõem suas áreas de cobertura (a "Selva de Wi-Fis" de Kurose), se elas utilizarem canais sobrepostos (ex: Canal 1 e Canal 2), elas irão interferir uma na outra, degradando o desempenho de ambas. Diz-se que não há sobreposição apenas entre canais que se distanciam por pelo menos 4 canais.

A boa prática para diminuir a interferência é alocar cada AP em subfaixas (canais) não sobrepostas. Na banda de 2.4 GHz, existem apenas **três** canais que não se sobrepõem: **1, 6 e 11**.

O diagrama de frequência abaixo ilustra isso perfeitamente.

<div align="center">
<img width="600px" src="./img/09-diagrama-de-frequencia.png">
</div>

Vejam que na imagem é representado muito bem, em linha mais escura, a marcação dos canais 1, 6 e 11, demonstrando que não há sobreposição entre eles. Qualquer instalação profissional de Wi-Fi na banda de 2.4GHz deve usar _apenas_ esses três canais.

### Outros Padrões e Recursos Relevantes

- **802.11z (TDLS - Tunneled Direct Link Setup):** Este padrão surgiu para otimizar a comunicação _dentro_ de uma WLAN. Em uma rede de infraestrutura normal, se o notebook A quer enviar um arquivo para o notebook B (ambos conectados no mesmo AP), o tráfego sobe do A para o AP, e então desce do AP para o B. O 802.11z permite a criação de um **túnel direto** entre os dois dispositivos, sem depender do Access Point para retransmitir os dados.
- **Wi-Fi Direct:** É importante mencionar que a implementação do TDLS é diferente do recurso conhecido como **Wi-Fi Direct**. Este último tem o foco no estabelecimento de forma rápida de um link de comunicação entre dois dispositivos (ex: um smartphone e uma Smart TV) _independentemente_ de uma estrutura de rede sem fio (AP) existente.

### Tecnologias OFDM e MIMO

Dando seguimento, vamos agora entender as principais tecnologias que permitiram a grande evolução em velocidade e eficiência dos padrões de redes sem fio. Se observarmos novamente a tabela de padrões, veremos que duas tecnologias se tornam referência nos padrões mais modernos: OFDM e MIMO.

#### Orthogonal Frequency Division Multiplexing (OFDM)

Uma das principais tecnologias que apoiam a expansão das capacidades e a otimização do uso do meio de transmissão é o **OFDM (Multiplexação por Divisão de Frequência Ortogonal)**.

Para entender a importância do OFDM, precisamos primeiro olhar para o método tradicional, o **FDM (Frequency Division Multiplexing)**. No FDM, para que múltiplos sinais (canais) sejam transmitidos na mesma faixa de frequência sem que um interfira no outro, é preciso inserir "bandas de guarda" (guard bands) — espaços vazios — entre eles.

A imagem abaixo, na parte (a), ilustra o FDM. Vemos que para a mesma faixa de frequência (Available bandwidth), buscou-se evitar a sobreposição, o que limita a quantidade de canais (f1 a f4) e desperdiça espectro.

<div align="center">
<img width="500px" src="./img/09-ofdm.png">
</div>

Já na parte (b) da imagem, temos o **OFDM**. E aí surge a pergunta: mas os canais (f1 a f6) estão claramente se sobrepondo, isso não vai gerar interferência?

A resposta é **não**, e essa é a "mágica" do OFDM. O "O" de Ortogonal se refere a uma característica matemática e de cálculo em domínio de frequência. As subportadoras (os canais) são projetadas de tal forma que o _pico_ de amplitude de um canal coincide perfeitamente com o ponto de amplitude _zero_ (o ponto nulo) de todos os outros canais sobrepostos.

O resultado é que, embora os sinais se sobreponham, eles não geram interferência entre si no momento da decodificação. Isso permite "empacotar" muito mais subportadoras (f1 a f6, no exemplo) na mesma largura de banda que o FDM usava para menos canais, aumentando drasticamente a eficiência espectral e a taxa de transmissão total. Esta é a tecnologia que permitiu o salto do 802.11b (11 Mbps) para o 802.11a/g (54 Mbps) e continua sendo a base de todos os padrões modernos.

#### Multiple In, Multiple Out (MIMO)

Se o OFDM tornou o "caminho" (o canal) mais eficiente, a tecnologia **MIMO (Multiple In, Multiple Out)**, introduzida a partir do padrão **802.11n**, foi responsável por criar múltiplos "caminhos" onde antes só existia um.

Basicamente, o MIMO explora a capacidade de se obter múltiplas entradas (recepção) e múltiplas saídas (transmissão) por intermédio do uso de **várias antenas** no transmissor e no receptor.

<div align="center">
<img width="300px" src="./img/09-mimo-ponto-de-acesso.png">
</div>

A imagem acima mostra um Ponto de Acesso com múltiplas antenas, uma característica visual de um dispositivo com capacidade MIMO.

A tecnologia MIMO é baseada no conceito de **fluxos espaciais (spatial streams)** distintos por caminhos diferentes. Em ambientes internos, o sinal de rádio ricocheteia em paredes, móveis e pessoas, criando múltiplos caminhos (um fenômeno chamado _multipath_). Antes do MIMO, o _multipath_ era um problema que causava interferência. O MIMO transforma esse problema em solução.

<div align="center">
<img width="500px" src="./img/09-mimo.png">
</div>

Como a imagem acima ilustra, um transmissor com múltiplas antenas pode enviar **fluxos de dados diferentes** de cada uma de suas antenas _ao mesmo tempo e na mesma frequência_. Esses sinais viajam por caminhos diferentes (as linhas pontilhadas) e chegam "misturados" nas antenas do receptor. O receptor, que também possui múltiplas antenas e um processador de sinal avançado, consegue desvendar essa mistura de sinais, separar os fluxos e remontar os dados originais, contribuindo para uma vazão final muito maior.

Um ponto que merece ser citado é que há uma relação de antenas e capacidades de fluxos, que respeita a estrutura `NxM:S`:

- **N** é a quantidade de Antenas de Transmissão.
- **M** é a quantidade de Antenas de Recepção.
- **S** é a quantidade de Fluxos Espaciais (Spatial Streams).

Um dispositivo 2x2:2 pode, teoricamente, duplicar a taxa de dados. Um 4x4:4 pode quadruplicar. O padrão 802.11n, por exemplo, suportava até 4 fluxos.

### Camada de Acesso ao Meio 802.11 (MAC e LLC)

É importante reforçar, como já vimos, que a camada de enlace é dividida em duas subcamadas.

<div align="center">
<img width="500px" src="./img/09-llc-e-mac.png">
</div>

A figura acima nos dá essa visão clara:

1. **Subcamada Superior (LLC - Logical Link Control):** Padronizada como **802.2**, ela é **idêntica** quando comparamos os padrões 802.3 (Ethernet), 802.5 (Token Ring) e 802.11 (Wi-Fi). A LLC atua como a interface de tradução universal para as camadas superiores. A Camada de Rede (IP) entrega um pacote para a LLC, e ela não precisa saber (nem se importa) se o meio físico é um cabo de cobre ou o ar.
2. **Subcamada Inferior (MAC - Media Access Control):** Aqui temos a diferença. A subcamada MAC é específica para o meio. O MAC 802.3 lida com CSMA/CD e quadros com dois endereços. O MAC 802.11 lida com CSMA/CA, gerenciamento de energia, BSS/ESS, e quadros com até quatro endereços.
3. **Camada Física (PHY):** Abaixo do MAC, o 802.11 possui múltiplas camadas físicas (FHSS, DSSS, OFDM, etc.) que o MAC 802.11 sabe como gerenciar.

Nesse contexto, toda a parte de relação com o meio e ocupação de canais (como CSMA/CA, OFDM, MIMO) se dá a partir da camada MAC e da camada PHY, uma vez que a LLC é a mesma para ambos e trata de outros aspectos de organização da informação.

### Gerenciamento de Potência (Power Management)

Uma característica vital do padrão 802.11, especialmente para dispositivos móveis operados por bateria, é o **gerenciamento de potência**.

O protocolo 802.11 permite que os dispositivos operem em dois estados:

1. **Modo Ativo (Active Mode):** O rádio da estação está totalmente ligado. Ela pode transmitir ou receber informações a qualquer momento. Isso implica em um maior consumo de potência.
2. **Modo de Dormência (Power-Save Mode ou Stand-by):** Para economizar bateria, a estação "avisa" ao Access Point que está entrando em modo de dormência. Neste modo, o rádio principal é desligado. A estação apenas "acorda" em intervalos muito curtos e pré-definidos para verificar se há mensagens para ela.

Quando o Access Point recebe um quadro destinado a um dispositivo que está em _power-save_, o nó central (AP) não o transmite. Ele **armazena a informação em um buffer** e aguarda. Quando a estação "acorda" brevemente, ela ouve um quadro especial do AP (o _Beacon Frame_) que contém um mapa (TIM - Traffic Indication Map). Se a estação vir seu ID nesse mapa, ela entende que o AP tem dados para ela, entra no Modo Ativo e solicita o envio dos quadros armazenados.

Percebam que esse gerenciamento de potência do _dispositivo cliente_ é diferente do conceito de aumento ou diminuição da _potência de transmissão_ do sinal emitido pelo nó central (AP), que é usado para ajustar a área de cobertura (o alcance) do sinal.


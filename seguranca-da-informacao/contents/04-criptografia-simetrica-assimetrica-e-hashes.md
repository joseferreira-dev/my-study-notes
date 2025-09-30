# Capítulo 4 – Criptografia Simétrica, Assimétrica e Funções Hash

No capítulo anterior, exploramos os fundamentos da criptografia, desde suas origens clássicas até os modos de operação que regem os algoritmos modernos. Estabelecemos que o objetivo é transformar dados legíveis em um formato ininteligível, protegendo-os de acessos não autorizados. Agora, vamos nos aprofundar nos dois principais paradigmas que executam essa tarefa no mundo digital: a **criptografia simétrica** e a **assimétrica**. Embora ambas busquem a confidencialidade, suas abordagens para o gerenciamento de chaves são fundamentalmente diferentes, cada uma com suas próprias forças, fraquezas e casos de uso ideais. Além delas, estudaremos também as **funções de hash**, uma terceira categoria de ferramenta criptográfica que, embora não seja usada para decifrar dados, é indispensável para garantir a integridade da informação.

## Criptografia Simétrica (Chave Secreta)

A criptografia simétrica, também conhecida como criptografia de chave secreta ou de chave única, é o paradigma mais antigo e intuitivo. Seu princípio fundamental é o uso de **uma única chave compartilhada tanto para o processo de cifragem quanto para o de decifragem**.

O fluxo de comunicação é direto: o emissor pega um texto em claro e o submete a um algoritmo de criptografia simétrica, utilizando uma chave secreta para realizar os cálculos matemáticos que o transformarão em texto cifrado. Essa mensagem cifrada é então enviada ao destinatário. Para reverter o processo e obter a mensagem original, o destinatário deve aplicar o mesmo algoritmo, utilizando a **mesma chave** que foi usada na cifragem.

A imagem a seguir ilustra perfeitamente este modelo:

<div align="center">
<img width="440px" src="./img/04-criptografia-simetrica.png">
</div>

### Desafio da Distribuição de Chaves

A simplicidade do modelo simétrico é também a fonte de seu maior desafio operacional: o **problema da distribuição de chaves**. Para que a comunicação funcione, tanto o emissor quanto o receptor precisam ter uma cópia da mesma chave secreta. Isso gera um dilema: como as duas partes podem combinar e trocar essa chave de forma segura, especialmente se o único canal de comunicação que elas possuem já é inseguro? Enviar a chave pelo mesmo canal que será usado para a mensagem seria como enviar a chave de um cofre junto com o próprio cofre.

### Propriedades de Segurança e Boas Práticas

A criptografia simétrica, em sua forma pura, visa garantir primordialmente o princípio da **confidencialidade**. Se a chave for mantida em segredo, apenas as partes que a possuem poderão decifrar a mensagem. No entanto, ela não oferece, nativamente, garantias de:

- **Integridade:** Um atacante no meio do caminho poderia alterar o texto cifrado, e o destinatário, ao decifrar, obteria uma mensagem corrompida, sem um mecanismo para detectar a alteração.
- **Autenticidade e Não-Repúdio:** Como a chave é compartilhada, não há como provar criptograficamente qual das partes (emissor ou receptor) realmente gerou uma mensagem cifrada.

Uma boa prática em qualquer processo de criptografia é realizar a **compressão dos dados antes da encriptação**. Este procedimento tem um duplo benefício: primeiro, ele reduz o tamanho dos dados, aumentando o desempenho da transmissão e do processo criptográfico; segundo, e mais importante, a compressão reduz a redundância e os padrões repetitivos nos dados, o que dificulta o trabalho de um criptoanalista.

### Comparação com a Criptografia Assimétrica

Ao longo deste capítulo, veremos a criptografia assimétrica em detalhes, mas já é possível adiantar alguns pontos de comparação cruciais:

- **Desempenho:** Os algoritmos de criptografia simétrica são, por ordem de magnitude, muito mais rápidos e computacionalmente menos exigentes que os algoritmos de chave assimétrica. Isso se deve, em parte, ao fato de operarem com chaves significativamente menores.
- **Uso Prático (Criptografia Híbrida):** Devido à diferença de desempenho, a abordagem mais comum na prática é utilizar os dois paradigmas em conjunto. Utiliza-se a criptografia **assimétrica**, mais lenta, apenas no início da comunicação, com o propósito de trocar de forma segura a chave **simétrica**. Uma vez que ambas as partes possuem a chave secreta, toda a comunicação subsequente (a troca de grandes volumes de dados) é realizada utilizando a criptografia simétrica, muito mais rápida. É assim que funcionam protocolos como o TLS/SSL, que protegem nossa navegação na web.

É importante reforçar, seguindo o Princípio de Kerckhoffs, que a segurança de qualquer algoritmo criptográfico moderno, seja ele simétrico ou assimétrico, deve residir **exclusivamente no segredo da chave**, e não no segredo do algoritmo. Os algoritmos são públicos e exaustivamente analisados pela comunidade acadêmica. Sua robustez está em sua solidez matemática.

### Principais Algoritmos de Criptografia Simétrica

Com os conceitos gerais da criptografia simétrica estabelecidos, podemos agora analisar alguns dos algoritmos específicos mais importantes. Cada um deles representa um marco na evolução da segurança digital, com suas próprias características de design, tamanho de chave e nível de robustez.

#### DES (_Data Encryption Standard_)

Durante muitos anos, o **Data Encryption Standard (DES)** foi o algoritmo padrão para a criptografia simétrica, amplamente adotado por governos e indústrias em todo o mundo. Criado pela IBM e adotado como um padrão pelo governo dos EUA em 1977, o DES dominou o cenário da criptografia por mais de duas décadas.

Sua característica mais marcante — e, eventualmente, sua ruína — é o tamanho de sua chave. O DES utiliza uma chave de **64 bits**. No entanto, desses 64 bits, 8 são utilizados para a verificação de paridade (um mecanismo simples de detecção de erros), não contribuindo para a segurança. Isso resulta em uma **força de segurança efetiva de apenas 56 bits**. Para efeito de análise de segurança e de provas, quando se fala da robustez do DES, o número a ser considerado é 56 bits.

Com a evolução exponencial do poder computacional, uma chave de 56 bits se tornou vulnerável a ataques de força bruta. Em 1997, em um desafio lançado pelo NIST (_National Institute of Standards and Technology_), o DES foi publicamente quebrado. Hoje, ele é considerado inseguro para qualquer aplicação moderna, mas seu estudo continua sendo fundamental para a compreensão dos princípios das cifras de bloco.

##### Estrutura do DES: A Rede de Feistel

O DES é uma cifra de bloco que opera sobre blocos de 64 bits de dados. Sua arquitetura interna é baseada em uma estrutura conhecida como **Rede de Feistel**, que divide o bloco de dados em duas metades de 32 bits (esquerda e direita) e as processa através de múltiplas rodadas. O fluxo geral de operações é o seguinte:

<div align="center">
<img width="440px" src="./img/04-des-operacoes.png">
</div>

1. **Permutação Inicial:** O bloco de 64 bits de texto claro passa por uma permutação inicial, que embaralha os bits.
2. **16 Rodadas de Cifragem:** O bloco permutado é então submetido a 16 rodadas idênticas de processamento. Em cada rodada, uma subchave única de 48 bits (derivada da chave principal de 56 bits) é utilizada.
3. **Troca Final e Permutação Inversa:** Após as 16 rodadas, as duas metades do bloco são trocadas, e o resultado passa por uma permutação final (inversa da inicial) para produzir o texto cifrado de 64 bits.

##### Anatomia de uma Rodada do DES

O "coração" da segurança do DES reside no que acontece dentro de cada uma das 16 rodadas. A estrutura de Feistel opera sobre as metades do bloco, aplicando uma função complexa que envolve quatro estágios principais:

<div align="center">
<img width="480px" src="./img/04-des-etapas.png">
</div>

1. **Expansão:** A metade direita do bloco (32 bits) é expandida para 48 bits através de uma permutação que duplica alguns dos bits.
2. **Mistura de Chaves:** O resultado de 48 bits é combinado com a subchave daquela rodada (também de 48 bits) através de uma operação XOR.
3. **Substituição (S-boxes):** O resultado da mistura é dividido em oito blocos de 6 bits. Cada um desses blocos é alimentado em uma **S-box** (_Substitution-box_) diferente. As S-boxes são o núcleo da segurança do DES. Cada uma é uma tabela de substituição não-linear que transforma a entrada de 6 bits em uma saída de 4 bits. O design exato dessas tabelas foi mantido em segredo pela NSA na época, o que gerou constantes acusações de que poderiam conter um _backdoor_.
4. **Permutação (P-box):** As oito saídas de 4 bits das S-boxes (totalizando 32 bits) são então rearranjadas de acordo com uma permutação fixa, a **P-box** (_Permutation-box_).

O resultado final de 32 bits dessa função é então combinado via XOR com a metade esquerda do bloco, e as duas metades são trocadas para iniciar a próxima rodada.

##### Confusão, Difusão e o Princípio de Kerckhoffs

O design do DES é um exemplo clássico da aplicação de duas propriedades fundamentais para a segurança de uma cifra, identificadas pelo "pai" da teoria da informação, Claude Shannon:

- **Confusão:** Busca tornar a relação entre a chave e o texto cifrado a mais complexa e obscura possível. No DES, a confusão é fornecida principalmente pelas **S-boxes**, que realizam uma substituição complexa e não-linear.
- **Difusão:** Busca espalhar a influência de cada bit do texto claro sobre muitos bits do texto cifrado. No DES, a difusão é fornecida pelas **permutações** (P-box e expansão), que embaralham os bits a cada rodada. O objetivo é que a mudança de um único bit no texto claro cause uma "avalanche", alterando, em média, metade dos bits no texto cifrado.

Por fim, vale reforçar o **Princípio de Kerckhoffs**, que é um pilar da criptografia moderna. Ele estabelece que a segurança de um sistema criptográfico não deve depender do sigilo do algoritmo, mas unicamente do **sigilo da chave**. Os algoritmos, como o DES, devem ser públicos e abertos ao escrutínio da comunidade para que suas fraquezas possam ser encontradas e sua robustez, comprovada.

#### 3DES (_Triple DES_)

Com a crescente inviabilidade do DES frente ao avanço do poder computacional, a comunidade criptográfica precisava de uma solução mais robusta. Em vez de criar um algoritmo inteiramente novo do zero, uma solução engenhosa e de transição foi proposta: aplicar o já conhecido e amplamente analisado algoritmo DES múltiplas vezes. Assim nasceu o **Triple DES (3DES)**.

A ideia, como o nome sugere, é aplicar o DES três vezes consecutivas sobre o mesmo bloco de dados. No entanto, para garantir a segurança e a retrocompatibilidade, o padrão adotado não foi uma simples cifragem tripla, mas sim um processo de **Cifrar-Decifrar-Cifrar (EDE - _Encrypt-Decrypt-Encrypt_)**.

<div align="center">
<img width="480px" src="./img/04-3des-operacoes.png">
</div>

O fluxo de operação é o seguinte:

1. O bloco de texto claro é primeiro **cifrado** com uma chave K1.
2. O resultado dessa cifragem é então **decifrado** com uma segunda chave, K2.
3. Por fim, o resultado da decifragem é **cifrado** novamente com uma terceira chave, K3.

A escolha de "decifrar" na etapa intermediária foi uma decisão de design inteligente, que permite a retrocompatibilidade com o DES original. Se as três chaves (K1, K2 e K3) forem idênticas, a primeira cifragem é imediatamente anulada pela segunda decifragem, e o resultado final é equivalente a uma única cifragem DES com aquela chave.

##### Opções de Chave e Nível de Segurança

O 3DES oferece duas principais configurações para as chaves, o que impacta diretamente sua robustez:

- **Opção de 3 Chaves:** K1, K2 e K3 são três chaves DES independentes de 56 bits cada. Isso resulta em um tamanho de chave total de 56 x 3 = **168 bits**. No entanto, devido a um tipo de ataque conhecido como "_meet-in-the-middle_", a força de segurança efetiva do 3DES nesta configuração é considerada de **112 bits**, e não os 168 bits nominais.
- **Opção de 2 Chaves:** Nesta configuração, a primeira e a terceira chaves são idênticas (K1 = K3), e a segunda chave (K2) é diferente. Esta foi uma opção popular para simplificar o gerenciamento das chaves. A força de segurança, neste caso, é de 56 x 2 = **112 bits**.

##### Processo de Decifragem

O processo de decifragem do 3DES é a exata operação inversa da cifragem. Para obter o texto claro a partir do texto cifrado, aplica-se a sequência **Decifrar-Cifrar-Decifrar (DED)**, utilizando as mesmas chaves na ordem inversa.

<div align="center">
<img width="400px" src="./img/04-3des-decriptacao.png">
</div>

1. O texto cifrado é primeiro **decifrado** com a chave K3.
2. O resultado é então **cifrado** com a chave K2.
3. Finalmente, o resultado é **decifrado** com a chave K1, revelando o texto claro original.

Apesar de ter sido uma solução eficaz para estender a vida útil do DES, o 3DES também é hoje considerado obsoleto para novas aplicações. Seus principais problemas são o **baixo desempenho** (por executar o algoritmo DES três vezes) e o **tamanho de bloco pequeno** (64 bits), herdado do DES original. Ele foi oficialmente substituído pelo AES como o padrão recomendado.

#### Rivest Cipher (RC)

Desenvolvida por Ron Rivest na RSA Security, a família de algoritmos "Rivest Cipher" (RC) representa uma série de cifras simétricas que tiveram um impacto significativo na indústria. Embora compartilhem o mesmo criador, suas versões possuem arquiteturas e finalidades distintas. As que mais aparecem na literatura técnica são a RC4, a RC5 e a RC6.

##### RC4: A Cifra de Fluxo Onipresente

O **RC4** é, de longe, o mais conhecido e historicamente o mais utilizado da família. Sua principal característica é ser uma **cifra de fluxo** (_stream cipher_) orientada a byte. Ele é conhecido por sua extrema simplicidade e alta velocidade de implementação em software.

- **Funcionamento:** O RC4 utiliza uma chave de tamanho variável (de 40 a 2048 bits) para inicializar um "estado" interno, que é uma grande tabela de 256 bytes. A partir desse estado, um algoritmo de geração pseudoaleatória (PRGA) produz um fluxo contínuo de bytes, o _keystream_. Esse _keystream_ é então combinado com o texto claro via XOR para produzir o texto cifrado. A segurança do RC4 reside inteiramente na imprevisibilidade desse _keystream_.
- **Uso Histórico:** Devido à sua velocidade e simplicidade, o RC4 foi um dos algoritmos de criptografia mais populares do mundo, sendo amplamente utilizado em protocolos como o **SSL/TLS** (para proteger a navegação web) e no padrão de segurança **WEP** (para redes Wi-Fi).
- **Vulnerabilidades e Obsolescência:** Apesar de sua popularidade, diversas vulnerabilidades e vieses estatísticos foram descobertos no _keystream_ gerado pelo RC4 ao longo dos anos. Essas fraquezas o tornam suscetível a ataques que podem, com o tempo, revelar o texto plano. Por essa razão, o uso do RC4 é hoje **fortemente desaconselhado** e foi removido das versões mais recentes de protocolos seguros como o TLS 1.3.

##### RC5: A Cifra de Bloco Parametrizada

Diferente do RC4, o **RC5** é uma **cifra de bloco** notável por sua simplicidade e, principalmente, por sua alta **flexibilidade**. Ele foi projetado para ser "parametrizado", permitindo que o desenvolvedor ajuste três variáveis fundamentais para adequar o algoritmo a diferentes necessidades de segurança e desempenho:

- **Tamanho de Bloco Variável:** Pode ser configurado para 32, 64 ou 128 bits.
- **Tamanho de Chave Variável:** Pode variar de 0 a 2048 bits.
- **Número de Rodadas Variável:** Pode variar de 0 a 255 rodadas de processamento.

Essa capacidade de ajuste permite que o RC5 seja otimizado para diferentes plataformas de _hardware_. Seu design simples se baseia em três rotinas primitivas: adição, operação XOR e rotações de bits dependentes dos dados.

##### RC6: O Sucessor Evoluído

O **RC6** foi desenvolvido como uma evolução direta do RC5, com o objetivo de ser um dos candidatos no processo de seleção do novo Padrão de Criptografia Avançado (**AES**). Mantendo a estrutura básica de seu predecessor, o RC6 introduziu melhorias para aumentar sua segurança e desempenho:

- **Estrutura:** Também é uma cifra de bloco, mas fixa seu tamanho de bloco em 128 bits para atender aos requisitos da competição AES.
- **Melhorias:** O RC6 adicionou o uso da **multiplicação de inteiros** como uma de suas operações primitivas, o que aumenta significativamente a difusão dos bits por rodada, oferecendo maior segurança em menos rodadas. Além disso, enquanto o RC5 operava sobre duas metades do bloco (dois registradores), o RC6 opera sobre quatro, permitindo um embaralhamento mais complexo a cada rodada.

Embora tenha sido um dos cinco finalistas da competição AES, o algoritmo escolhido para se tornar o novo padrão foi o Rijndael.

#### AES (_Advanced Encryption Standard_): Padrão da Criptografia Moderna

Com a obsolescência do DES e do 3DES, o governo dos Estados Unidos, através do NIST, promoveu uma competição aberta no final da década de 1990 para selecionar um novo algoritmo de criptografia que se tornaria o padrão para o século XXI. O vencedor foi um algoritmo chamado **Rijndael**, que foi então padronizado como o **Advanced Encryption Standard (AES)**.

Hoje, o AES é o algoritmo de criptografia simétrica mais utilizado e confiável do mundo, presente em praticamente todos os protocolos e aplicações que exigem segurança, desde a proteção de transações bancárias e navegação web (TLS/SSL) até a criptografia de arquivos em seu computador e _smartphone_.

Suas principais características são:

- **Tamanho de Bloco Fixo:** O AES opera sobre blocos de dados de **128 bits**.
- **Tamanhos de Chave Variáveis:** Ele suporta três tamanhos de chave: **128, 192 ou 256 bits**.

A robustez de um algoritmo está diretamente ligada ao tamanho de sua chave. Como referência geral no cenário atual:

- **64 bits (ou menos):** Considerado fraco e inseguro contra ataques de força bruta.
- **128 bits:** Oferece um nível de segurança forte, adequado para a maioria das aplicações de rotina.
- **256 bits:** Oferece um nível de segurança altíssimo, recomendado para a proteção de dados extremamente sensíveis e de longo prazo.

##### Estrutura e Rodadas

Uma diferença fundamental em relação ao DES é que o AES **não utiliza uma Rede de Feistel**. Sua arquitetura é baseada em um design conhecido como **Rede de Substituição-Permutação (SPN)**, no qual todo o bloco de dados é processado em cada rodada.

O número de rodadas de processamento no AES não é fixo, mas varia de acordo com o tamanho da chave utilizada, para garantir que a complexidade da chave seja totalmente difundida pelo texto cifrado:

- **Chave de 128 bits:** 10 rodadas
- **Chave de 192 bits:** 12 rodadas
- **Chave de 256 bits:** 14 rodadas

##### A Anatomia de uma Rodada do AES

Em cada rodada, o AES aplica uma sequência de quatro transformações matemáticas sobre o bloco de dados. Esses estágios são projetados para prover as propriedades de confusão e difusão de Shannon de forma altamente eficiente.

1. **_SubBytes_** **(Substituição):** Este é um estágio de substituição não-linear. Cada byte do bloco é substituído por outro, de acordo com uma tabela de consulta fixa chamada S-box (a S-box do Rijndael). Esta é a principal fonte de **confusão** do algoritmo.
2. **_ShiftRows_** **(Permutação):** Neste estágio, os bytes do bloco são organizados em uma matriz 4x4, e as linhas dessa matriz são deslocadas (rotacionadas) ciclicamente por diferentes offsets. Esta operação embaralha os bytes dentro do bloco, sendo uma importante fonte de **difusão**.
3. **_MixColumns_** **(Substituição/Difusão):** Esta é uma operação de mistura que opera nas colunas da matriz de dados. Utilizando uma complexa aritmética sobre corpos finitos, ela combina os quatro bytes de cada coluna, garantindo que a alteração de um único byte na entrada afete todos os quatro bytes da coluna na saída. Este passo contribui enormemente para a **difusão**.
4. **_AddRoundKey:_** Neste último estágio, a chave entra em ação. Uma "chave de rodada" única (derivada da chave principal) é combinada com o bloco de dados através de uma simples operação XOR bit a bit.

Esses quatro estágios são repetidos por 10, 12 ou 14 vezes. É importante notar que todos os estágios são projetados para serem reversíveis, o que permite que o processo de decifragem aplique as transformações inversas para recuperar o texto claro original.

#### Outros Algoritmos Simétricos Notáveis

Além do DES e do AES, que foram ou são padrões governamentais, diversos outros algoritmos de criptografia simétrica tiveram grande impacto e continuam sendo relevantes, seja por seu legado histórico ou por sua robustez contínua.

##### Blowfish

O **Blowfish** é uma cifra de bloco simétrica projetada em 1993 pelo renomado criptógrafo Bruce Schneier. Ele foi criado com o objetivo explícito de ser um substituto rápido, gratuito e livre de patentes para o DES. Seus principais alvos de projeto foram:

- Ser extremamente rápido em microprocessadores de 32 bits de propósito geral.
- Oferecer um tamanho de chave flexível (variando de 32 a 448 bits) para se adaptar a diferentes requisitos de segurança.
- Ser simples e compacto, facilitando sua implementação e análise.

Arquiteturalmente, o Blowfish utiliza uma **rede de Feistel com 16 rodadas** e opera sobre **blocos de 64 bits**. Sua característica mais inovadora é o uso de **S-boxes dependentes da chave**. Diferente do DES, que possui S-boxes fixas, o Blowfish gera suas tabelas de substituição a partir da própria chave do usuário. Este é um processo de inicialização computacionalmente caro, mas que, uma vez concluído, permite que a cifragem subsequente seja muito veloz.

A principal fraqueza do Blowfish hoje não está em seu design, mas em seu **tamanho de bloco de 64 bits**. Assim como outras cifras com blocos pequenos, ele é vulnerável a **ataques de aniversário** quando utilizado para cifrar grandes volumes de dados (na casa dos gigabytes), o que pode levar a vazamentos de informação. Por essa razão, seu uso é desaconselhado para novas aplicações que lidam com grandes quantidades de dados, tendo sido sucedido por seu "irmão mais novo", o Twofish.

##### Twofish

O **Twofish** foi projetado pela mesma equipe liderada por Bruce Schneier, como um candidato para a competição que selecionou o AES. Ele foi desenhado para ser tão seguro quanto seus concorrentes, mas oferecendo um alto grau de flexibilidade e desempenho em uma variedade de plataformas.

O Twofish opera sobre **blocos de 128 bits** e aceita chaves de **128, 192 ou 256 bits**, as mesmas especificações do AES. Ele também utiliza uma **rede de Feistel com 16 rodadas**, mas adiciona camadas de segurança sofisticadas, como o _whitening_ (uma operação XOR com subchaves antes da primeira e depois da última rodada) e o uso de S-boxes complexas, que também são dependentes da chave.

Livre de patentes e de código aberto, o Twofish é um algoritmo extremamente robusto. Desde sua publicação, **nenhum ataque prático que comprometa a cifra completa foi descoberto**. Seu bloco de 128 bits elimina o risco de colisões que afeta as cifras de 64 bits, garantindo uma longevidade comparável à do próprio AES. Por sua segurança comprovada, ele continua a ser uma alternativa popular ao AES em muitos projetos de código aberto.

##### IDEA (_International Data Encryption Algorithm_)

Desenvolvido na Suíça no início dos anos 1990, o **IDEA** é outra cifra de bloco que ganhou notoriedade por sua força e por seu design inovador. Ele opera sobre **blocos de 64 bits** e utiliza uma **chave de 128 bits**.

Sua estrutura não é uma rede de Feistel. Em vez disso, suas 8,5 rodadas de processamento combinam, de forma engenhosa, operações de três grupos algébricos diferentes: **XOR**, **adição modular** e **multiplicação modular**. Essa mistura de operações lineares e não-lineares foi projetada para oferecer forte resistência contra a criptoanálise diferencial, uma das técnicas de ataque mais poderosas da época.

O IDEA é mais conhecido por ter sido o algoritmo de criptografia utilizado na versão 2.0 do **PGP (_Pretty Good Privacy_)**, um dos softwares de criptografia de e-mail mais famosos da história. Assim como o Blowfish, sua principal limitação hoje é o tamanho de bloco de 64 bits.

##### Síntese Comparativa

A tabela a seguir resume e compara as principais características desses três algoritmos.

|Algoritmo|Tipo/Estrutura|Tamanho do Bloco|Tamanhos de Chave|Número de Rodadas|Estado de Segurança Atual|Uso Típico/Observações|
|---|---|---|---|---|---|---|
|**Blowfish**|Cifra de bloco simétrica, rede de Feistel.|64 bits|32 a 448 bits|16|Sem ataques práticos à versão completa; vulnerável a colisões em volumes muito altos por causa do bloco de 64 bits.|_Hashing_ de senhas (bcrypt), softwares legados.|
|**Twofish**|Cifra de bloco simétrica, rede de Feistel.|128 bits|128, 192 ou 256 bits|16|Nenhum ataque prático conhecido; foi um dos finalistas do processo AES.|Alternativa ao AES em projetos _open-source_, discos criptografados.|
|**IDEA**|Cifra de bloco simétrica; combina XOR, adição e multiplicação.|64 bits|128 bits|8,5|Considerada segura; a principal limitação é o bloco de 64 bits.|Versões clássicas do PGP, sistemas legados.|

#### ChaCha20: Cifra de Fluxo Moderna

Diferente dos algoritmos de bloco como o AES, o **ChaCha20** é uma **cifra de fluxo** (_stream cipher_) de alto desempenho, descendente do algoritmo Salsa20. Ele foi projetado para ser extremamente rápido e seguro em implementações de software, oferecendo uma alternativa robusta ao AES, especialmente em plataformas que não possuem aceleração de hardware para criptografia.

- **Estrutura:** O ChaCha20 opera com uma chave de **256 bits**, um **nonce** (número de uso único) de 96 bits e um contador de 32 bits. Juntos, eles formam o estado inicial de 512 bits (64 bytes) do algoritmo.
- **Funcionamento:** Para cada bloco de texto claro, o ChaCha20 utiliza seu estado inicial para gerar um bloco de 512 bits de _keystream_. Esse processo é determinístico e envolve **20 rodadas** de operações matemáticas simples e rápidas (adições, XORs e rotações de bits). O _keystream_ resultante é então combinado com o texto claro via XOR para produzir o texto cifrado. O contador é incrementado para cada novo bloco, garantindo que o _keystream_ seja sempre diferente.
- **Segurança:** A segurança do ChaCha20 repousa na dificuldade de reverter as 20 rodadas de mistura. Até hoje, os ataques criptoanalíticos mais eficazes só conseguiram quebrar versões enfraquecidas do algoritmo, com no máximo 7 ou 8 rodadas. A versão completa, com 20 rodadas, permanece computacionalmente segura e sem vulnerabilidades práticas conhecidas.

No entanto, como toda cifra de fluxo, o ChaCha20 compartilha uma fragilidade fundamental: a **reutilização de um nonce com a mesma chave é catastrófica**. Se isso ocorrer, o mesmo _keystream_ será gerado para duas mensagens diferentes, permitindo que um atacante as combine e extraia informações do texto plano. Por isso, a unicidade do _nonce_ é um requisito de segurança inegociável.

##### ChaCha20-Poly1305: Adicionando Autenticidade e Integridade

O ChaCha20, em sua forma pura, fornece apenas **confidencialidade**. Para atender às necessidades dos protocolos modernos, que exigem também **integridade** e **autenticidade**, ele é quase sempre utilizado em uma construção conhecida como **AEAD (_Authenticated Encryption with Associated Data_)**.

A combinação **ChaCha20-Poly1305** é a implementação AEAD mais popular. Nela, dois algoritmos trabalham em conjunto:

1. **ChaCha20:** Cifra a mensagem, garantindo a confidencialidade.
2. **Poly1305:** Atua como um **MAC (_Message Authentication Code_)**, gerando uma "etiqueta" de autenticação de 128 bits sobre o texto cifrado. Essa etiqueta garante que a mensagem não foi alterada no caminho (integridade) e que foi gerada por alguém que possui a chave secreta (autenticidade).

Essa combinação robusta e de alto desempenho se tornou um padrão na internet moderna, sendo adotada em protocolos de segurança críticos como o **TLS 1.3** (ao lado do AES-GCM), **OpenSSH** e na VPN **WireGuard**, consolidando o ChaCha20 como uma das cifras de fluxo de referência para segurança de alto nível em software.


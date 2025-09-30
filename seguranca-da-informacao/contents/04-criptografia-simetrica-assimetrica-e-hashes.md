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


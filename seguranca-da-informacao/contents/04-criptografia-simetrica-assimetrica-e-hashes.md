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


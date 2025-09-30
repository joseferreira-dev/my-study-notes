# Capítulo 3 – Criptografia

Nos capítulos anteriores, estabelecemos que a informação é o ativo mais valioso de qualquer organização e que um dos pilares para sua proteção é o princípio da **confidencialidade**. Garantir que dados sensíveis sejam acessíveis apenas por entidades autorizadas é um desafio que acompanha a humanidade desde que a comunicação existe. Diante desse cenário, historicamente, sempre se buscou criar formas de "esconder" a informação de terceiros, de modo que, mesmo que um adversário interceptasse uma mensagem, ele não seria capaz de compreendê-la. Essa necessidade ancestral deu origem a uma das ciências mais fascinantes e cruciais da segurança da informação: a **criptografia**. Ela é a principal ferramenta que utilizamos para transformar dados legíveis e vulneráveis em informações codificadas e seguras, sendo a espinha dorsal da privacidade no mundo digital, desde uma simples troca de mensagens até as mais complexas transações financeiras.

## Criptografia: A Ciência da Escrita Oculta

A criptografia é a ciência e a arte de transformar uma informação (chamada de **texto plano** ou _plaintext_) em um formato codificado e aparentemente ininteligível (chamado de **texto cifrado** ou _ciphertext_), de forma que apenas o destinatário legítimo, de posse de uma "chave" secreta, consiga reverter o processo e ler a mensagem original. O objetivo é "embaralhar" os dados de maneira controlada, garantindo que, mesmo que um atacante obtenha acesso a eles, não será capaz de extrair seu significado.

O próprio termo "criptografia" revela sua essência. Ele vem da junção de duas palavras gregas: **Kryptós**, que significa "oculto", e **grápho**, que significa "escrita". A criptografia é, em sua origem, a "escrita oculta". Para compreender este universo, é fundamental dominar alguns termos que formam a base de seu estudo:

- **Criptografia:** Como vimos, é a ciência de codificar mensagens para garantir sua confidencialidade e integridade.
- **Criptoanálise:** É a ciência (ou a arte) de "quebrar" códigos e decifrar mensagens sem o conhecimento da chave. A partir da análise (_análysis_, em grego, que significa "decomposição") do texto cifrado, o criptoanalista busca encontrar falhas no método de cifragem para revelar a informação oculta.
- **Criptologia:** É o campo de estudo (_logo_, em grego) mais amplo, que agrega tanto a criptografia quanto a criptoanálise. É o estudo completo da comunicação segura.
- **Cifra (ou Algoritmo Criptográfico):** É o método, o conjunto de passos ou a função matemática utilizada para codificar e decodificar a informação.

A combinação de diferentes métodos de cifragem com fórmulas e funções matemáticas complexas gera os algoritmos de criptografia modernos que protegem nossa vida digital. No entanto, em sua base, esses algoritmos derivam de três técnicas de cifragem clássicas: substituição, transposição e esteganografia.

Com certeza. Retomando o ponto em que paramos, vamos agora detalhar os métodos clássicos de cifragem, utilizando e expandindo suas anotações para cada uma das técnicas.

### Métodos Clássicos de Cifragem

Na base de todos os algoritmos criptográficos modernos, encontramos princípios que foram desenvolvidos e refinados ao longo de séculos. As técnicas clássicas de cifragem podem ser divididas em três grandes categorias, baseadas na forma como manipulam a mensagem original para ocultar seu conteúdo.

#### Cifra de Substituição

A substituição é o método de codificação mais intuitivo. Sua lógica consiste em **substituir** cada caractere, símbolo ou unidade de dado do texto plano por outro, de acordo com uma regra ou um alfabeto secreto. Embora seja um método simples de executar, sua segurança é geralmente baixa, tornando-o mais fácil de ser quebrado por técnicas de criptoanálise.

O exemplo mais famoso desta categoria é a **Cifra de César**, um método supostamente utilizado por Júlio César para proteger suas comunicações militares. Trata-se de uma **cifra de substituição monoalfabética**, na qual a regra é extremamente simples: cada letra da mensagem original é substituída pela letra que se encontra um número fixo de posições à frente no alfabeto. Na versão clássica, esse deslocamento (a "chave") é de três posições.

A tabela a seguir ilustra a aplicação da Cifra de César com um deslocamento de 3 letras:

<div align="center">
<img width="400px" src="./img/03-substituicao.png">
</div>

Seguindo essa regra, a letra 'a' do texto simples se torna 'D' no texto cifrado, 'b' se torna 'E', e assim por diante. Ao chegar ao fim do alfabeto, a contagem continua a partir do início (um processo chamado de operação de módulo).

Vamos aplicar essa cifra a um exemplo prático. Para garantir a codificação, caracteres especiais e acentos são geralmente removidos ou normalizados.

- **Mensagem Original:** `atacar hoje a noite`
- **Mensagem Cifrada:** `DWDFDU KRMH D QRLWH`

A aparente simplicidade, no entanto, é a maior fraqueza deste método. Um criptoanalista pode quebrar a Cifra de César com extrema facilidade, testando os 25 deslocamentos possíveis (ataque de força bruta) ou, de forma mais inteligente, através da **análise de frequência**. Em qualquer idioma, certas letras aparecem com muito mais frequência do que outras (em português, as vogais 'a' e 'e' são as mais comuns). Ao contar a frequência das letras no texto cifrado, o analista pode deduzir qual letra cifrada corresponde a 'a' ou 'e' e, a partir daí, descobrir a chave de deslocamento.

#### Cifra de Transposição

Diferentemente da substituição, a cifra de transposição não altera os caracteres do texto plano. Em vez disso, ela **rearranja** a ordem deles, embaralhando a mensagem de acordo com uma regra específica. Nenhum caractere é adicionado ou removido; a segurança reside na complexidade do embaralhamento.

Os algoritmos de criptografia modernos frequentemente utilizam este recurso, dividindo a mensagem original em blocos de tamanhos iguais e permutando seus bits. Um exemplo clássico e simples para ilustrar o conceito é a **cifra de transposição de colunas**.

Neste método, utiliza-se uma palavra-chave para definir a ordem de embaralhamento. Vamos cifrar a mensagem `ATACAR AO AMANHECER` usando a chave `SEGURANCA`:

1. Primeiro, escrevemos a mensagem em uma grade sob a palavra-chave.
2. Em seguida, lemos as colunas na ordem alfabética da chave (`A`, `C`, `E`, `G`, `N`, `R`, `S`, `U`).

|**S**|**E**|**G**|**U**|**R**|**A**|**N**|**C**|**A**|
|---|---|---|---|---|---|---|---|---|
|A|T|A|C|A|R|A|O|A|
|M|A|N|H|E|C|E|R|X|

Lendo as colunas na ordem alfabética da chave (`A`, `C`, `E`, etc.), o texto cifrado seria: `RCA ORA TAE CM ANH AE`. Perceba que não há letras ou sílabas novas; apenas as originais foram reordenadas.

#### Esteganografia

Por último, temos a esteganografia, uma técnica que se diferencia fundamentalmente da criptografia. Enquanto a criptografia busca ocultar o **conteúdo** de uma mensagem, a esteganografia tem como objetivo ocultar a **própria existência** da mensagem.

A informação secreta é embutida dentro de um arquivo de cobertura de aparência inofensiva. Tipicamente, busca-se esconder uma mensagem de texto dentro do código de uma imagem digital. Isso é feito através de algoritmos que realizam alterações minúsculas e imperceptíveis nos dados do arquivo de cobertura. Em uma imagem, por exemplo, a cor de cada pixel é representada por um conjunto de bits. A técnica de **LSB (_Least Significant Bit_)** altera o bit menos significativo de alguns pixels para codificar a mensagem secreta. A mudança na cor é tão sutil que é indetectável ao olho humano, mas pode ser extraída por um software que conheça o método.

A esteganografia não se limita a imagens. Uma mensagem pode ser escondida em arquivos de áudio, vídeo ou até mesmo em um texto comum, através de sutis alterações na formatação ou no espaçamento. É comum que a esteganografia seja usada em conjunto com a criptografia: primeiro, a mensagem é cifrada para proteger seu conteúdo e, em seguida, o texto cifrado é escondido em outro arquivo para ocultar o fato de que uma comunicação secreta está ocorrendo.


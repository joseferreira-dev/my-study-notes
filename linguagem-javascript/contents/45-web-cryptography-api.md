# Capítulo 45 – Web Cryptography API: Criptografia no Navegador

Ao longo de nossa jornada, construímos aplicações que manipulam dados, interagem com o usuário e se comunicam com servidores. Em cada uma dessas etapas, a **segurança** é uma preocupação fundamental. Tradicionalmente, operações criptográficas sensíveis — como a criação de hashes de senhas, a criptografia de dados ou a geração de chaves seguras — eram uma responsabilidade exclusiva do lado do servidor. O cliente (navegador) era considerado um ambiente inerentemente inseguro. No entanto, o surgimento de novas arquiteturas de aplicação, como as que necessitam de criptografia de ponta a ponta (end-to-end encryption) ou a verificação de integridade de arquivos antes do upload, criou a necessidade de uma maneira segura de se realizar operações criptográficas diretamente no cliente.

Para atender a essa demanda, a plataforma web introduziu a **Web Cryptography API**. Esta não é uma API simples para iniciantes; é uma interface de baixo nível, poderosa e flexível que dá ao JavaScript acesso às primitivas criptográficas do navegador. Ela foi projetada para ser segura desde o início, operando de forma assíncrona para não travar a interface do usuário e garantindo que materiais sensíveis, como chaves privadas, possam ser gerados e usados sem serem diretamente expostos ao seu código, a menos que seja explicitamente permitido.

Neste capítulo, vamos desvendar esta API fundamental para a segurança. Começaremos por entender seu ponto de entrada principal, o objeto `crypto.subtle`, e por que ele é chamado de "sutil". Aprenderemos a gerar dados verdadeiramente aleatórios, um pilar para qualquer operação criptográfica. Dominaremos a criação de **digests (hashes)**, essenciais para a verificação de integridade de dados. Faremos um mergulho profundo na **criptografia assimétrica**, aprendendo a gerar pares de chaves RSA, a usá-las para criptografar e descriptografar dados, e, crucialmente, a exportar e importar essas chaves no onipresente formato PEM. Ao final, você terá o conhecimento necessário para começar a construir aplicações que levam a segurança do lado do cliente a um novo patamar.

## O Ponto de Entrada: `window.crypto` e `crypto.subtle`

A Web Crypto API está disponível através do objeto global `window.crypto`. Este objeto contém duas propriedades principais:

1. **`crypto.getRandomValues()`**: Uma função síncrona para gerar números aleatórios criptograficamente seguros.
2. **`crypto.subtle`**: Um objeto que contém todos os métodos para as operações criptográficas mais complexas (hashing, assinatura, criptografia, etc.).

O nome **`subtle` (sutil)** é intencional. Ele serve como um aviso aos desenvolvedores: esta é uma API de baixo nível, e um uso incorreto de suas primitivas pode levar a falhas de segurança graves. Todas as operações dentro de `crypto.subtle` são **assíncronas** e retornam `Promises`, garantindo que operações potencialmente lentas não bloqueiem a thread principal.

**Segurança em Primeiro Lugar:** Assim como outras APIs poderosas, a Web Crypto API requer um **contexto seguro**. Sua aplicação deve ser servida sobre **HTTPS** para que a API esteja disponível (com a exceção de `localhost` para desenvolvimento).

## Gerando Dados Aleatórios Criptograficamente Seguros

A primeira e mais básica necessidade em criptografia é a geração de aleatoriedade de alta qualidade. `Math.random()` **não é adequado** para fins criptográficos, pois seus resultados não são imprevisíveis o suficiente. Para isso, usamos `crypto.getRandomValues()`.

Este método preenche um `TypedArray` (como um `Uint8Array`) com valores aleatórios.

```js
function gerarIdSeguro(comprimento = 16) {
  // Cria um array de bytes com o comprimento desejado
  const buffer = new Uint8Array(comprimento);
  
  // Preenche o buffer com números aleatórios seguros
  window.crypto.getRandomValues(buffer);
  
  // Converte os bytes para uma string hexadecimal para fácil utilização
  return Array.from(buffer)
    .map(b => b.toString(16).padStart(2, '0'))
    .join('');
}

const idSessao = gerarIdSeguro();
console.log(`ID de Sessão Seguro Gerado: ${idSessao}`);
```

## Criando Digests (Hashing) com `crypto.subtle.digest()`

Um **hash criptográfico** (ou digest) é uma "impressão digital" de um conjunto de dados. É uma operação de mão única: é fácil gerar um hash a partir de dados, mas virtualmente impossível gerar os dados a partir do hash. Qualquer pequena mudança nos dados de entrada resulta em um hash completamente diferente. Isso o torna perfeito para verificar a **integridade dos dados**.

O algoritmo de hash mais comum hoje é o **SHA-256**.

O processo para criar um digest envolve três etapas:

1. Converter os dados de entrada (ex: uma string) para um formato binário (`ArrayBuffer`).
2. Chamar `crypto.subtle.digest()` com o algoritmo e os dados binários.
3. Converter o `ArrayBuffer` resultante em um formato legível, como uma string hexadecimal.

**Exemplo: Gerando um Hash SHA-256 de um Texto**

```js
async function calcularHash(mensagem) {
  try {
    // 1. Codificar a mensagem em um buffer de bytes (UTF-8)
    const encoder = new TextEncoder();
    const data = encoder.encode(mensagem);

    // 2. Calcular o digest. Retorna uma Promise que resolve com um ArrayBuffer.
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    
    // 3. Converter o ArrayBuffer para uma string hexadecimal
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
    
    return hashHex;
  } catch (error) {
    console.error("Falha ao calcular o hash:", error);
  }
}

// Uso da função
calcularHash("Olá, mundo da criptografia!")
  .then(hash => {
    console.log("Hash SHA-256:", hash);
  });
```

## Criptografia Assimétrica: Gerando Pares de Chaves RSA

A criptografia assimétrica usa um par de chaves matematicamente relacionadas: uma **chave pública**, que pode ser compartilhada livremente, e uma **chave privada**, que deve ser mantida em segredo absoluto.

- Dados criptografados com a chave pública só podem ser descriptografados com a chave privada correspondente.
- Uma mensagem assinada com a chave privada pode ser verificada por qualquer pessoa que tenha a chave pública.

Vamos focar no caso de uso de criptografia. O algoritmo padrão para isso é o **RSA-OAEP**.

Para gerar um par de chaves, usamos `crypto.subtle.generateKey()`.

```js
async function gerarParDeChavesRSA() {
  try {
    const keyPair = await crypto.subtle.generateKey(
      {
        name: 'RSA-OAEP',
        modulusLength: 2048, // Tamanho da chave em bits. 2048 é um bom padrão.
        publicExponent: new Uint8Array([1, 0, 1]), // Valor padrão para o expoente público (65537)
        hash: 'SHA-256',
      },
      true, // 'extractable': true permite que possamos exportar a chave mais tarde
      ['encrypt', 'decrypt'] // 'keyUsages': o que podemos fazer com essas chaves
    );
    return keyPair; // Retorna um objeto { publicKey, privateKey }
  } catch (error) {
    console.error("Falha ao gerar o par de chaves:", error);
  }
}

gerarParDeChavesRSA().then(parDeChaves => {
  console.log("Par de chaves gerado com sucesso!");
  console.log("Chave Pública:", parDeChaves.publicKey);
  console.log("Chave Privada:", parDeChaves.privateKey);
});
```

O resultado são dois objetos `CryptoKey`, que são "caixas-pretas" seguras. Não podemos ver o material da chave diretamente, mas podemos usá-los em outras operações criptográficas.

## Exportando e Importando Chaves: O Formato PEM

Para compartilhar uma chave pública ou para armazenar uma chave privada, precisamos exportá-la para um formato padrão. A Web Crypto API pode exportar para formatos binários (`spki` para chaves públicas, `pkcs8` para privadas) ou para o formato `jwk` (JSON Web Key).

O formato mais comum na internet é o **PEM (Privacy-Enhanced Mail)**. Um arquivo PEM nada mais é do que os dados binários da chave codificados em **Base64**, envoltos por um cabeçalho e um rodapé de texto.

### Exportando uma Chave Pública para o Formato PEM

```js
async function exportarChaveParaPEM(chave) {
  // 1. Exporta a chave no formato binário 'spki'
  const chaveExportada = await crypto.subtle.exportKey('spki', chave);
  
  // 2. Converte o buffer binário para uma string Base64
  const chaveString = String.fromCharCode.apply(null, new Uint8Array(chaveExportada));
  const chaveBase64 = btoa(chaveString);
  
  // 3. Monta a string PEM final
  const pem = `-----BEGIN PUBLIC KEY-----\n${chaveBase64}\n-----END PUBLIC KEY-----`;
  
  return pem;
}

// Uso
gerarParDeChavesRSA().then(par => {
  exportarChaveParaPEM(par.publicKey).then(pem => {
    console.log("Chave Pública em formato PEM:");
    console.log(pem);
  });
});
```

### Importando uma Chave no Formato PEM

O processo inverso é um pouco mais complexo, pois precisamos decodificar o PEM de volta para seu formato binário antes que a API possa entendê-lo.

```js
// Função auxiliar para converter string PEM em ArrayBuffer
function pemParaArrayBuffer(pem) {
  const b64 = pem
    .replace(/-----BEGIN PUBLIC KEY-----/, '')
    .replace(/-----END PUBLIC KEY-----/, '')
    .replace(/\s/g, ''); // Remove cabeçalho, rodapé e espaços
  
  const binarioString = atob(b64);
  const buffer = new ArrayBuffer(binarioString.length);
  const bytes = new Uint8Array(buffer);
  for (let i = 0; i < binarioString.length; i++) {
    bytes[i] = binarioString.charCodeAt(i);
  }
  return buffer;
}

async function importarChavePEM(pem) {
  try {
    const bufferChave = pemParaArrayBuffer(pem);
    
    const chaveImportada = await crypto.subtle.importKey(
      'spki', // Formato da chave
      bufferChave, // Dados da chave
      { name: 'RSA-OAEP', hash: 'SHA-256' }, // Algoritmo
      true, // 'extractable'
      ['encrypt'] // 'keyUsages'
    );
    
    return chaveImportada;
  } catch (error) {
    console.error("Falha ao importar a chave PEM:", error);
  }
}
```

## Criptografia e Descriptografia

Com as chaves em mãos, podemos finalmente criptografar e descriptografar dados.

```js
async function exemploCompleto() {
  // 1. Gera o par de chaves
  const { publicKey, privateKey } = await gerarParDeChavesRSA();
  
  // 2. Prepara a mensagem
  const mensagem = "Esta é uma mensagem super secreta.";
  const encoder = new TextEncoder();
  const mensagemCodificada = encoder.encode(mensagem);
  
  // 3. Criptografa com a chave pública
  const dadosCriptografados = await crypto.subtle.encrypt(
    { name: 'RSA-OAEP' },
    publicKey,
    mensagemCodificada
  );
  
  console.log("Dados Criptografados (ArrayBuffer):", dadosCriptografados);
  
  // 4. Descriptografa com a chave privada
  const dadosDescriptografados = await crypto.subtle.decrypt(
    { name: 'RSA-OAEP' },
    privateKey,
    dadosCriptografados
  );
  
  // 5. Decodifica a mensagem de volta para texto
  const decoder = new TextDecoder();
  const mensagemOriginal = decoder.decode(dadosDescriptografados);
  
  console.log("Mensagem Original:", mensagemOriginal);
}

exemploCompleto();
```

## Considerações Finais

Neste capítulo, desvendamos a **Web Cryptography API**, uma interface de baixo nível que traz o poder da criptografia forte para o navegador. Vimos que, embora sua natureza assíncrona e sua complexidade exijam cuidado, ela nos fornece as ferramentas para construir uma nova classe de aplicações web seguras.

Aprendemos a gerar dados aleatórios seguros com `crypto.getRandomValues`, a garantir a integridade de dados com a criação de **hashes SHA-256**, e mergulhamos no mundo da **criptografia assimétrica**. Dominamos o ciclo completo: gerar um par de chaves RSA, usá-las para criptografar e descriptografar mensagens, e, crucialmente, exportar e importar essas chaves no formato padrão **PEM**.

A Web Crypto API é a base para funcionalidades como chats com criptografia de ponta a ponta, sistemas de autenticação seguros e a verificação de arquivos no lado do cliente. Com o conhecimento adquirido aqui, você está agora capacitado a pensar e a implementar a segurança não como um adendo, mas como uma parte integral da arquitetura de suas aplicações web.
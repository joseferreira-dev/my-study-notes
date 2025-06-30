# Capítulo 41 – File API: Lendo Arquivos no Navegador

Historicamente, a interação de uma página web com os arquivos do computador do usuário era extremamente limitada e quase que exclusivamente unidirecional: o usuário podia clicar em um botão `<input type="file">` e enviar um arquivo para um servidor. O conteúdo desse arquivo era, para o JavaScript rodando na página, uma caixa preta. Não havia uma maneira nativa de ler, validar ou pré-visualizar o conteúdo de um arquivo antes que ele fosse enviado, levando a experiências de usuário desajeitadas e a uma dependência total do processamento no lado do servidor.

Essa realidade mudou com a introdução da **File API** no HTML5. Esta suíte de APIs foi uma revolução, pois deu ao JavaScript, pela primeira vez, a capacidade de interagir diretamente com o conteúdo de arquivos selecionados pelo usuário, tudo de forma segura e assíncrona, dentro do ambiente do navegador. Com a File API, podemos criar validadores de tamanho e tipo de arquivo no lado do cliente, gerar pré-visualizações de imagens antes do upload, ler o conteúdo de arquivos de texto ou CSV para análise, e até mesmo processar dados binários.

Neste capítulo, vamos mergulhar fundo nesta poderosa API. Começaremos por entender como o usuário seleciona arquivos através do input de arquivo e como acessamos esses arquivos através do objeto `File`. Dissecaremos as propriedades que nos dão metadados cruciais, como nome, tamanho e tipo. O coração do capítulo será a exploração do objeto **`FileReader`**, a principal ferramenta para a leitura assíncrona do conteúdo de um arquivo. Vamos dominar seus métodos para ler arquivos como texto, como Data URLs (para imagens) ou como buffers binários, e aprender a monitorar o progresso e a lidar com erros através de seu modelo de eventos. Por fim, exploraremos o conceito de **`Blob`**, a base para a manipulação de dados binários brutos, e veremos como podemos criar nossos próprios arquivos no cliente e oferecê-los para download.

## O Ponto de Partida: Selecionando Arquivos

A jornada começa com um elemento HTML que já nos é familiar, mas que ganha superpoderes quando combinado com a File API.

**HTML:**

```html
<input type="file" id="seletorDeArquivo">
```

Quando o usuário seleciona um ou mais arquivos, o evento `change` é disparado neste elemento. A lista de arquivos selecionados está disponível na propriedade `files` do elemento, que é um objeto `FileList`.

Um `FileList` é um objeto semelhante a um array (mas não é um array de verdade) que contém objetos do tipo `File`.

```js
const seletor = document.getElementById('seletorDeArquivo');

seletor.addEventListener('change', (event) => {
  const arquivos = event.target.files; // A FileList
  
  if (arquivos.length === 0) {
    console.log("Nenhum arquivo selecionado.");
    return;
  }

  // Acessando o primeiro arquivo
  const primeiroArquivo = arquivos[0];
  console.log("Arquivo selecionado:", primeiroArquivo);
});
```

### Selecionando Múltiplos Arquivos e Restringindo Tipos

Podemos customizar o input para melhorar a experiência do usuário.

- **`multiple`**: Um atributo booleano que permite ao usuário selecionar múltiplos arquivos.
    
    ```html
    <input type="file" id="seletorMultiplo" multiple>
    ```
    
- **`accept`**: Uma string que fornece uma dica ao navegador sobre quais tipos de arquivo o usuário deve poder selecionar. Isso filtra os arquivos na janela de seleção do sistema operacional.
    
    ```html
    <!-- Apenas imagens JPEG e PNG -->
    <input type="file" accept="image/jpeg, image/png">
    
    <!-- Qualquer tipo de arquivo de áudio -->
    <input type="file" accept="audio/*">
    
    <!-- Extensões de arquivo específicas -->
    <input type="file" accept=".pdf,.doc,.docx">
    ```

**Atenção:** A validação com `accept` é apenas uma conveniência para o usuário. Um usuário mal-intencionado ainda pode selecionar qualquer tipo de arquivo. A validação real do tipo de arquivo deve sempre ser feita no seu código JavaScript (e também no servidor, se houver um upload).

## O Objeto `File`: Metadados do Arquivo

Cada item na `FileList` é um objeto `File`. Este objeto nos dá acesso a informações importantes (metadados) sobre o arquivo, mesmo antes de lermos seu conteúdo.

Um objeto `File` possui as seguintes propriedades somente leitura:

- **`name`**: O nome do arquivo como uma string (ex: `"minha-foto.jpg"`).
- **`size`**: O tamanho do arquivo em bytes.
- **`type`**: O tipo MIME do arquivo como uma string (ex: `"image/jpeg"`). Pode ser uma string vazia se o tipo não puder ser determinado.
- **`lastModified`**: Um timestamp numérico representando a data da última modificação do arquivo.

```js
// ... dentro do evento 'change'
const arquivo = event.target.files[0];

console.log(`Nome: ${arquivo.name}`);
console.log(`Tamanho: ${(arquivo.size / 1024).toFixed(2)} KB`);
console.log(`Tipo: ${arquivo.type}`);
console.log(`Última Modificação: ${new Date(arquivo.lastModified).toLocaleDateString('pt-BR')}`);
```

Esses metadados são extremamente úteis para validações do lado do cliente. Por exemplo, você pode impedir o upload de um arquivo que seja maior que um determinado tamanho.

## Lendo o Conteúdo com `FileReader`

Para ler o conteúdo real de um arquivo, usamos a interface `FileReader`. Este é um processo **assíncrono**. Você cria uma instância de `FileReader`, diz a ele como ler o arquivo e, em seguida, ouve os eventos para saber quando a leitura foi concluída (com sucesso ou falha).

### O Ciclo de Vida do `FileReader`

Um `FileReader` opera com base em eventos.

**Event Handlers Principais:**

- `onloadstart`: Disparado quando a leitura começa.
- `onprogress`: Disparado periodicamente enquanto os dados estão sendo lidos. Útil para atualizar uma barra de progresso.
- `onload`: O evento mais importante. Disparado quando a leitura é **concluída com sucesso**. O resultado da leitura estará disponível na propriedade `reader.result`.
- `onerror`: Disparado se ocorrer um erro durante a leitura.
- `onabort`: Disparado se a leitura for cancelada pelo código usando `reader.abort()`.
- `onloadend`: Disparado quando a leitura termina, seja por sucesso, erro ou aborto.

**Propriedades do `FileReader`:**

- `readyState`: Um número que indica o estado do leitor: `0` (EMPTY), `1` (LOADING), `2` (DONE).
- `result`: O conteúdo do arquivo. Esta propriedade só é preenchida após o evento `onload` ser disparado. Seu tipo depende do método de leitura usado.
- `error`: Um objeto de erro se algo der errado.

### Métodos de Leitura

Você inicia o processo de leitura chamando um dos seguintes métodos:

- **`readAsText(file, [encoding])`**: Lê o conteúdo do arquivo como uma string de texto. O segundo argumento opcional especifica a codificação (ex: `'UTF-8'`).
- **`readAsDataURL(file)`**: Lê o conteúdo do arquivo e o retorna como uma **Data URL**. Este é um formato de string Base64 que pode ser usado diretamente na `src` de uma tag `<img>` ou no `url()` do CSS. É a base para pré-visualizações de imagem.
- **`readAsArrayBuffer(file)`**: Lê o conteúdo do arquivo como um `ArrayBuffer`, que é uma representação de dados binários brutos. Útil para processamento de baixo nível, como com imagens ou áudio.

**Exemplo: Lendo um Arquivo de Texto**

```js
function lerArquivoDeTexto(arquivo) {
  const reader = new FileReader();

  reader.onload = (event) => {
    const conteudoDoArquivo = event.target.result;
    document.getElementById('conteudo-texto').textContent = conteudoDoArquivo;
  };
  
  reader.onerror = (event) => {
    console.error("Erro ao ler o arquivo:", reader.error);
  };
  
  reader.readAsText(arquivo);
}
```

**Exemplo: Pré-visualização de Imagem**

```html
<input type="file" id="seletor-imagem" accept="image/*">
<img id="preview" src="" alt="Pré-visualização da imagem" style="max-width: 200px;">
```

```js
const seletorImagem = document.getElementById('seletor-imagem');
const preview = document.getElementById('preview');

seletorImagem.addEventListener('change', (event) => {
  const arquivo = event.target.files[0];
  if (arquivo) {
    const reader = new FileReader();
    
    reader.onload = function(e) {
      // O resultado é a Data URL, que pode ser usada na src da imagem
      preview.src = e.target.result;
    }
    
    reader.readAsDataURL(arquivo);
  }
});
```

## Blobs: Manipulando Dados Binários

A interface `File` que vimos é, na verdade, uma especialização de uma interface mais fundamental: o **`Blob`** (Binary Large Object). Um Blob representa dados binários brutos e imutáveis.

### Fatiando Arquivos com `slice()`

Como um `File` é um `Blob`, ele herda o método `slice()`. Isso nos permite criar um novo Blob que representa uma "fatia" do arquivo original. Isso é extremamente útil para fazer o upload de arquivos grandes em pedaços (chunks).

`blob.slice(inicio, fim, tipoDeConteudo)`

```js
function lerPrimeirosBytes(arquivo) {
  // Cria um Blob contendo os primeiros 1024 bytes do arquivo
  const primeiroChunk = arquivo.slice(0, 1024);
  
  const reader = new FileReader();
  reader.onload = (e) => {
    // Agora podemos processar apenas este pequeno pedaço
    console.log("Leitura do primeiro kilobyte concluída.");
  };
  reader.readAsArrayBuffer(primeiroChunk);
}
```

### Criando Arquivos no Cliente com Blobs

Podemos também criar nossos próprios Blobs a partir de dados em memória, como strings ou arrays. Isso nos permite gerar arquivos dinamicamente no navegador.

**Exemplo: Gerando e Baixando um Arquivo CSV**

```js
function baixarCSV() {
  const dados = [
    ['Nome', 'Idade', 'Cidade'],
    ['Ana', '28', 'Recife'],
    ['Bia', '35', 'Olinda'],
    ['Carlos', '42', 'Jaboatão']
  ];

  // Converte o array de dados em uma string CSV
  const conteudoCSV = dados.map(linha => linha.join(',')).join('\n');

  // 1. Cria um Blob a partir da string CSV
  const blob = new Blob([conteudoCSV], { type: 'text/csv;charset=utf-8;' });
  
  // 2. Cria uma URL temporária para o Blob
  const url = URL.createObjectURL(blob);
  
  // 3. Cria um link de download invisível
  const link = document.createElement("a");
  link.setAttribute("href", url);
  link.setAttribute("download", "dados_usuarios.csv");
  document.body.appendChild(link);
  
  // 4. Simula o clique no link para iniciar o download
  link.click();
  
  // 5. Remove o link do DOM
  document.body.removeChild(link);
}
```

`URL.createObjectURL()` cria uma URL curta e única que aponta para o objeto Blob em memória. É uma forma eficiente de usar dados de Blob em locais que esperam uma URL, como `href` ou `src`.

## Considerações Finais

Neste capítulo, desvendamos a **File API**, uma ponte poderosa que conecta a segurança do ambiente do navegador com os arquivos locais do usuário. Vimos como podemos permitir que os usuários selecionem arquivos e como podemos, com segurança, extrair metadados e ler o conteúdo desses arquivos diretamente no cliente.

Dominamos o `FileReader`, entendendo seu modelo assíncrono e baseado em eventos para ler arquivos como texto, Data URLs para imagens, ou `ArrayBuffer` para dados binários. Exploramos o conceito de `Blob` como a base para dados binários, aprendendo a fatiar arquivos grandes e, o mais impressionante, a criar nossos próprios arquivos dinamicamente no navegador e oferecê-los para download.

Com a File API, o navegador deixa de ser apenas um consumidor passivo de conteúdo e se torna uma plataforma poderosa para aplicações de edição, processamento e visualização de dados. O conhecimento adquirido aqui é a base para a criação de validadores de upload, pré-visualizadores de imagem, editores de texto, e inúmeras outras ferramentas que antes eram exclusivas de aplicações desktop.
# Capítulo 35 – Cookies: Gerenciando Estado e Sessões no Lado do Cliente

Até este ponto, nossas aplicações têm ganhado vida. Elas podem responder a eventos, manipular o DOM e até mesmo buscar dados de servidores remotos usando AJAX e Fetch. No entanto, elas sofrem de uma forma de amnésia digital. Por sua natureza, o protocolo sobre o qual a web é construída, o **HTTP**, é **stateless (sem estado)**. Cada requisição que um navegador faz a um servidor é um evento completamente independente; o servidor não tem uma memória inerente de quem fez a requisição anterior. Se um usuário faz login em uma página e depois navega para outra, como o servidor pode se lembrar de que esse usuário já está autenticado? Como um site de e-commerce "lembra" dos itens que você adicionou ao seu carrinho enquanto continua a navegar?

A solução para esta amnésia fundamental é a capacidade de armazenar pequenas porções de dados no lado do cliente (o navegador), que podem ser enviadas de volta ao servidor a cada nova requisição. A tecnologia original e mais fundamental projetada para este propósito são os **cookies**. Um cookie é um pequeno pedaço de dado textual que um servidor envia para o navegador do usuário. O navegador então o armazena e o envia de volta com cada requisição subsequente para o mesmo servidor, permitindo que o servidor "se lembre" de informações sobre aquela sessão ou aquele usuário específico.

Neste capítulo, vamos desvendar completamente o funcionamento dos cookies. Embora existam APIs de armazenamento mais modernas como `LocalStorage` e `SessionStorage`, os cookies continuam a ser uma parte essencial e insubstituível da web, especialmente para o gerenciamento de sessões de autenticação. Começaremos por entender o que são e como o navegador e o servidor os trocam. Dissecaremos a API JavaScript para manipulá-los através de `document.cookie`, uma interface peculiar, porém poderosa. Aprenderemos a criar, ler e remover cookies, e, mais importante, faremos um mergulho profundo nos seus atributos, que nos dão um controle fino sobre seu tempo de vida, seu escopo e, crucialmente, sua **segurança**.

## O que é um Cookie?

Um cookie é um pequeno arquivo de texto armazenado no navegador do usuário. Ele funciona como um sistema de pares chave-valor. O fluxo de um cookie é um ciclo de duas partes:

1. **Do Servidor para o Navegador (`Set-Cookie`):** Quando um servidor responde a uma requisição HTTP, ele pode incluir um cabeçalho chamado `Set-Cookie`. Este cabeçalho instrui o navegador a armazenar um cookie com um nome, um valor e vários atributos opcionais. `HTTP/1.1 200 OK` `Content-Type: text/html` `Set-Cookie: nomeUsuario=AnaSilva; expires=Tue, 19 Jan 2038 03:14:07 GMT; path=/`
2. **Do Navegador para o Servidor (`Cookie`):** Em todas as requisições subsequentes para o mesmo servidor (que correspondam ao domínio e caminho do cookie), o navegador automaticamente incluirá um cabeçalho `Cookie` contendo os pares chave-valor de todos os cookies relevantes (`GET /perfil HTTP/1.1`, `Host: www.meusite.com`, `Cookie: nomeUsuario=AnaSilva`).

É este ciclo que permite que o servidor identifique sessões e usuários. Embora o processo seja iniciado e consumido principalmente pelo servidor, o JavaScript nos dá uma interface para ler e escrever cookies diretamente do lado do cliente.

## Manipulando Cookies com JavaScript: `document.cookie`

A interface do JavaScript para cookies é um tanto peculiar. Não existe um objeto com métodos como `get()` ou `set()`. Em vez disso, toda a interação acontece através de uma única propriedade do DOM: **`document.cookie`**.

Esta propriedade não se comporta como uma string normal.

- Quando você a **lê**, ela retorna uma única string contendo todos os cookies (não `HttpOnly`) para aquele domínio, separados por ponto e vírgula (ex: `"chave1=valor1; chave2=valor2"`).
- Quando você **escreve** nela, você não sobrescreve todos os cookies. Em vez disso, você está adicionando um novo cookie ou atualizando um cookie existente com o mesmo nome.

### Testando se os Cookies Estão Habilitados

Antes de tentar usar cookies, é uma boa prática verificar se o usuário os habilitou em seu navegador. A forma mais fácil e moderna é através da propriedade `navigator.cookieEnabled`.

```js
if (navigator.cookieEnabled) {
  console.log("Cookies estão habilitados!");
} else {
  console.log("Cookies estão desabilitados. Algumas funcionalidades podem não funcionar.");
}
```

## Adicionando e Configurando Cookies

Para criar ou atualizar um cookie, você atribui uma string a `document.cookie` no formato `chave=valor`.

```js
// Cria um cookie simples de sessão (dura até o navegador ser fechado)
document.cookie = "tema=dark";
```

No entanto, um cookie é muito mais do que um simples par chave-valor. Seu verdadeiro poder reside em seus atributos, que são adicionados à string, separados por ponto e vírgula.

**Sintaxe:** `document.cookie = "chave=valor; expires=...; path=...; samesite=...; secure";`

### Atributos de Cookie Essenciais

- **`expires=data_UTC` ou `max-age=segundos`**: Controla o tempo de vida do cookie.
    
    - **`expires`**: Define uma data e hora de expiração exatas no formato de string UTC.
        
        ```js
        const dataExpiracao = new Date();
        // Define a expiração para 30 dias a partir de agora
        dataExpiracao.setDate(dataExpiracao.getDate() + 30);
        document.cookie = `idSessao=xyz123; expires=${dataExpiracao.toUTCString()}`;
        ```
        
    - **`max-age`**: A forma moderna e preferida. Define a vida do cookie em segundos a partir do momento em que é criado. É mais fácil de usar e menos propenso a erros de fuso horário.
        
        ```js
        // Cookie que dura por 1 hora (60 segundos * 60 minutos)
        document.cookie = "preferencia=salva; max-age=3600";
        ```
        
    - Se nem `expires` nem `max-age` forem definidos, o cookie é um **cookie de sessão** e será excluído quando o navegador for fechado.
        
- **`path=caminho`**: Define o escopo do cookie para um caminho específico no servidor. O cookie só será enviado para requisições cujo caminho comece com o valor especificado.
    
    - `path=/`: O cookie está disponível para todas as páginas do site (a configuração mais comum).
        
    - `path=/admin/`: O cookie só será enviado para páginas dentro do diretório `/admin/`.
        
- **`domain=dominio`**: Define para quais domínios o cookie é válido. Se não for especificado, ele se aplica apenas ao host de origem. Se você especificar `domain=example.com`, ele também será válido para subdomínios como `app.example.com`.

### Atributos de Segurança Cruciais

- **`secure`**: Este é um atributo booleano (um "flag"). Se presente, o cookie só será enviado do navegador para o servidor através de uma conexão **HTTPS**. Isso é essencial para proteger cookies sensíveis de serem interceptados.
    
- **`samesite=Strict | Lax | None`**: Um atributo vital para a segurança moderna, que ajuda a mitigar ataques de **Cross-Site Request Forgery (CSRF)**. Ele controla se um cookie pode ser enviado com requisições iniciadas por outros sites.
    
    - **`Strict`**: O cookie só é enviado se a requisição se originar do mesmo site. A forma mais segura, mas pode quebrar links de navegação normais de outros sites para o seu.
        
    - **`Lax`** (padrão na maioria dos navegadores modernos): Permite que o cookie seja enviado com navegações de nível superior (ex: clicar em um link para ir de `outro-site.com` para `meu-site.com`), mas o bloqueia em requisições de sub-recursos (como carregar uma imagem ou um `iframe`). Oferece um bom equilíbrio entre segurança e usabilidade.
        
    - **`None`**: Permite que o cookie seja enviado em todos os contextos de site cruzado. Para usar `samesite=None`, o atributo `secure` **deve** também estar presente.

**Exemplo de um Cookie Seguro e Completo:**

```js
// Cookie para login que expira em 7 dias, válido para todo o site, 
// apenas sobre HTTPS, e com uma política SameSite equilibrada.
const dias = 7;
document.cookie = `token_auth=asdf12345; max-age=${dias * 24 * 60 * 60}; path=/; samesite=Lax; secure`;
```

## Lendo Cookies

Ler cookies é a parte mais desajeitada da API. `document.cookie` retorna uma única string com todos os cookies. Não há um método nativo para pegar um valor por sua chave. Precisamos criar uma função auxiliar para isso.

```js
function lerCookie(nome) {
  // Adiciona um '=' ao nome para garantir que estamos pegando a chave exata
  const nomeEQ = nome + "=";
  // Divide document.cookie em um array de cookies individuais
  const ca = document.cookie.split(';');
  
  for(let i = 0; i < ca.length; i++) {
    let c = ca[i];
    // Remove espaços em branco no início
    while (c.charAt(0) === ' ') {
      c = c.substring(1, c.length);
    }
    // Se este cookie começa com o nome que procuramos...
    if (c.indexOf(nomeEQ) === 0) {
      //...retorna o valor do cookie (a substring após 'nome=')
      return c.substring(nomeEQ.length, c.length);
    }
  }
  // Retorna null se não encontrar
  return null;
}

// Usando a função
const temaSalvo = lerCookie("tema");
if (temaSalvo) {
  console.log(`O tema salvo é: ${temaSalvo}`);
}
```

## Removendo Cookies

Para remover um cookie, você não o "deleta". Em vez disso, você o **sobrescreve com uma data de expiração no passado**.

```js
function removerCookie(nome) {
  // Para garantir a remoção, é crucial especificar o mesmo path e domain
  // que foram usados para criar o cookie. Usar path=/ é uma boa prática.
  document.cookie = `${nome}=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/;`;
}

// Removendo um cookie
removerCookie("preferencia");
```

Uma fonte comum de bugs é tentar remover um cookie sem especificar o `path` correto. Se um cookie foi criado com `path=/admin`, você deve usar `path=/admin` ao tentar removê-lo.

## O Atributo `HttpOnly` e a Segurança

Existe um atributo de cookie extremamente importante que **não pode ser definido via JavaScript**: `HttpOnly`. Este atributo, quando definido pelo servidor no cabeçalho `Set-Cookie`, informa ao navegador que o cookie só deve ser acessado pelo servidor e **nunca** por scripts do lado do cliente (`document.cookie` não o verá).

```
Set-Cookie: idSessao=super-secreto; HttpOnly; Secure
```

Esta é a defesa mais importante contra ataques de **Cross-Site Scripting (XSS)**. Se um invasor conseguir injetar um script malicioso em sua página, esse script não poderá roubar o cookie de sessão do usuário, pois `document.cookie` não terá acesso a ele.

**Regra de ouro:** Qualquer cookie usado para autenticação ou para armazenar informações de sessão sensíveis **DEVE** ser definido com os atributos `HttpOnly` e `Secure`.

## Considerações Finais

Neste capítulo, desvendamos os **cookies**, a tecnologia fundamental que trouxe o conceito de estado para o protocolo stateless da web. Vimos que, apesar da existência de APIs de armazenamento mais modernas, os cookies continuam a ser a ferramenta essencial para o gerenciamento de sessões e a comunicação de estado entre o cliente e o servidor.

Exploramos a peculiar, porém funcional, API `document.cookie`, aprendendo a criar, ler e remover cookies do lado do cliente. O ponto mais crucial foi a nossa imersão nos **atributos de cookie**. Compreendemos que o poder e a segurança de um cookie não estão apenas em seu valor, mas em como configuramos seu tempo de vida com `max-age`, seu escopo com `path`, e, acima de tudo, sua segurança com as diretivas `secure`, `samesite`, e o importantíssimo atributo `HttpOnly` (definido pelo servidor).

Saber quando e como usar cookies de forma segura é uma habilidade indispensável. Com o conhecimento adquirido aqui, você está agora preparado para gerenciar o estado da sua aplicação de forma robusta, protegendo os dados do seu usuário e construindo uma base sólida para aplicações web seguras e funcionais.
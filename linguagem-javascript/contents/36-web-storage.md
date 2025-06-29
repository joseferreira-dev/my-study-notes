# Capítulo 36 – Web Storage: Armazenamento Moderno no Navegador

No capítulo anterior, desvendamos os cookies, a tecnologia pioneira que permitiu que a web "se lembrasse" de seus usuários entre as requisições. Vimos que, apesar de sua importância histórica e de seu papel contínuo na autenticação, a API de cookies (`document.cookie`) é desajeitada, e o fato de os dados serem enviados a cada requisição HTTP pode ser ineficiente. Conforme as aplicações web se tornaram mais complexas, surgiu a necessidade de uma maneira mais simples, mais poderosa e com maior capacidade para armazenar dados puramente no lado do cliente, sem sobrecarregar a comunicação com o servidor.

Para atender a essa demanda, a especificação do HTML5 introduziu a **Web Storage API**, um mecanismo moderno que fornece dois novos objetos para armazenar dados no navegador: **`localStorage`** e **`sessionStorage`**. Essas APIs foram projetadas desde o início para serem uma solução de armazenamento do lado do cliente superior aos cookies para muitos casos de uso. Elas oferecem uma API de chave-valor muito mais simples e intuitiva, uma capacidade de armazenamento significativamente maior (geralmente 5-10MB por domínio, contra os 4KB dos cookies) e, crucialmente, os dados armazenados **não** são enviados automaticamente com cada requisição HTTP, melhorando a performance.

Neste capítulo, faremos um mergulho completo na Web Storage API. Começaremos por dissecar o `localStorage`, entendendo como ele nos permite armazenar dados que persistem indefinidamente no navegador do usuário. Em seguida, exploraremos o `sessionStorage`, que oferece uma solução para armazenar dados que duram apenas enquanto a aba do navegador estiver aberta. Dominaremos a API simples e síncrona para adicionar, ler, remover e limpar dados, e aprenderemos as melhores práticas para armazenar dados complexos, como objetos e arrays, usando JSON. Finalmente, investigaremos o poderoso `storage` event, que nos permite criar uma comunicação em tempo real entre diferentes abas da mesma aplicação.

## O que é a Web Storage API?

A Web Storage API fornece um mecanismo de armazenamento de chave-valor no navegador que é mais simples e seguro que os cookies. Ela é dividida em duas interfaces quase idênticas, mas com uma diferença fundamental em seu ciclo de vida e escopo:

- **`localStorage`**: Armazena dados sem data de expiração. Os dados permanecem disponíveis mesmo após o navegador ser fechado e reaberto.
- **`sessionStorage`**: Armazena dados apenas para uma sessão. Os dados são perdidos quando a aba ou a janela do navegador é fechada.

Ambos os objetos estão disponíveis globalmente através de `window.localStorage` e `window.sessionStorage` (ou simplesmente `localStorage` e `sessionStorage`).

## `localStorage`: Armazenamento Persistente

O `localStorage` é ideal para dados que você deseja que permaneçam disponíveis para o usuário em visitas futuras ao seu site. Pense em preferências do usuário (como um tema "dark" ou "light"), o conteúdo de um carrinho de compras não finalizado, ou o nome de um usuário que marcou a opção "Lembrar de mim".

**Escopo:** O `localStorage` é escopado para a **origem** do documento, que é a combinação do protocolo (`http` ou `https`), do hostname (`meusite.com`) e da porta (`80` ou `443`). Isso significa que `http://meusite.com` e `https://meusite.com` têm armazenamentos separados, assim como `meusite.com` e `sub.meusite.com`.

### A API do `localStorage`

A API é síncrona e direta.

- **`setItem(chave, valor)`**: Adiciona um novo par chave-valor ou atualiza o valor se a chave já existir. **Importante:** Tanto a chave quanto o valor devem ser **strings**.
    
    ```js
    localStorage.setItem('temaUsuario', 'dark');
    localStorage.setItem('idioma', 'pt-BR');
    ```
    
- **`getItem(chave)`**: Recupera o valor de uma chave. Se a chave não existir, retorna `null`.
    
    ```js
    const tema = localStorage.getItem('temaUsuario');
    console.log(`O tema salvo é: ${tema}`); // "O tema salvo é: dark"
    ```
    
- **`removeItem(chave)`**: Remove um par chave-valor específico.
    
    ```js
    localStorage.removeItem('idioma');
    ```
    
- **`clear()`**: Remove **todos** os pares chave-valor para aquela origem.
    
    ```js
    localStorage.clear();
    ```
    
- **`length`**: Uma propriedade que retorna o número de itens armazenados.
    
    ```js
    console.log(`Itens no localStorage: ${localStorage.length}`);
    ```

### Armazenando Dados Complexos com JSON

Como `setItem` só aceita strings, para armazenar objetos ou arrays, precisamos primeiro **serializá-los** com `JSON.stringify()` e, ao recuperá-los, **desserializá-los** com `JSON.parse()`.

```js
const configuracoesUsuario = {
  nome: 'Ana',
  notificacoes: true,
  volume: 80
};

// Armazenando o objeto
localStorage.setItem('configuracoes', JSON.stringify(configuracoesUsuario));

// Recuperando e usando o objeto
const configSalvasString = localStorage.getItem('configuracoes');
if (configSalvasString) {
  const configSalvasObj = JSON.parse(configSalvasString);
  console.log(`Bem-vinda de volta, ${configSalvasObj.nome}!`); // "Bem-vinda de volta, Ana!"
}
```

Este padrão de `stringify`/`parse` é absolutamente essencial para o uso prático do Web Storage.

### Uma Forma Mais Simples (Mas Cautelosa) de Acesso

É possível acessar o storage como se fosse um objeto comum.

```js
// Definindo
localStorage.usuario = 'Carlos';

// Lendo
const nomeUsuario = localStorage.usuario;

// Deletando
delete localStorage.usuario;
```

**Atenção:** Esta abordagem é geralmente **desaconselhada**. Se você tentar usar uma chave que conflita com um método do próprio objeto `Storage` (como `setItem` ou `clear`), você pode quebrar a funcionalidade da API. Prefira sempre os métodos `getItem()` e `setItem()`.

## `sessionStorage`: Armazenamento por Sessão

O `sessionStorage` funciona exatamente da mesma maneira que o `localStorage`. A API é **idêntica**. A única e crucial diferença é o seu **ciclo de vida e escopo**.

- **Ciclo de Vida:** Os dados no `sessionStorage` são apagados quando a sessão da página termina. Uma sessão termina quando a aba ou janela é fechada. A sessão **persiste** através de recarregamentos e restaurações de página.
- **Escopo:** O `sessionStorage` é ainda mais restrito que o `localStorage`. Ele é escopado por **aba**. Duas abas abertas no mesmo site têm `sessionStorage`s separados e independentes.

**Caso de Uso:** É perfeito para armazenar dados temporários relacionados a uma tarefa específica do usuário. Por exemplo, se um usuário está preenchendo um formulário de múltiplas etapas, você pode salvar os dados de cada etapa no `sessionStorage`. Se ele acidentalmente recarregar a página, os dados ainda estarão lá, mas serão automaticamente limpos quando ele fechar a aba.

```js
// Em um formulário de cadastro
const campoNome = document.getElementById('nome');
campoNome.addEventListener('keyup', () => {
  sessionStorage.setItem('form-nome', campoNome.value);
});

// Ao carregar a página
window.addEventListener('load', () => {
  const nomeSalvo = sessionStorage.getItem('form-nome');
  if (nomeSalvo) {
    campoNome.value = nomeSalvo;
  }
});
```

## O Evento `storage`

A Web Storage API inclui um mecanismo poderoso para sincronizar dados entre diferentes abas ou janelas da mesma origem: o evento `storage`.

Este evento é disparado no objeto `window` de uma página sempre que o `localStorage` é modificado em **outra** página (da mesma origem). Ele não é disparado na mesma página que fez a alteração.

O objeto de evento passado para o listener contém informações úteis:

- `key`: A chave que foi adicionada, modificada ou removida.
- `oldValue`: O valor antigo da chave.
- `newValue`: O novo valor da chave.
- `url`: A URL da página que fez a alteração.
- `storageArea`: O objeto de armazenamento afetado (`localStorage`).

**Exemplo: Logout Sincronizado entre Abas**

**Aba 1 (página de perfil):**

```js
const btnLogout = document.getElementById('logout');
btnLogout.addEventListener('click', () => {
  console.log("Usuário deslogou na Aba 1.");
  localStorage.removeItem('token_auth');
});
```

**Aba 2 (outra página do mesmo site):**

```js
window.addEventListener('storage', (event) => {
  console.log("Evento de storage detectado na Aba 2!");
  // Se a chave 'token_auth' foi removida em outra aba...
  if (event.key === 'token_auth' && event.newValue === null) {
    console.log("Token de autenticação removido. Redirecionando para a página de login...");
    window.location.href = '/login';
  }
});
```

Este padrão cria uma experiência de usuário fluida e consistente entre múltiplas abas.

## Lidando com Erros: Limites de Armazenamento

O armazenamento fornecido pelo Web Storage não é infinito. A maioria dos navegadores aloca cerca de 5 a 10 MB por origem. Se você tentar exceder esse limite, o navegador lançará uma exceção, geralmente uma `QuotaExceededError` ou `DOMException`.

Aplicações robustas devem antecipar essa possibilidade envolvendo as chamadas `setItem` em um bloco `try...catch`.

```js
try {
  // Tenta armazenar um grande volume de dados
  localStorage.setItem('dadosGrandes', umaStringMuitoLonga);
} catch (e) {
  if (e.name === 'QuotaExceededError' ||
      e.name === 'NS_ERROR_DOM_QUOTA_REACHED') { // Nome pode variar entre navegadores
    alert('Erro: O armazenamento local está cheio! Não foi possível salvar suas preferências.');
    // Lógica para lidar com o erro, como limpar dados antigos.
  } else {
    // Outro tipo de erro
    console.error('Ocorreu um erro ao salvar no localStorage:', e);
  }
}
```

## Considerações Finais

Neste capítulo, exploramos a **Web Storage API**, a solução moderna do JavaScript para armazenamento de dados no lado do cliente. Vimos como ela oferece uma interface de chave-valor muito mais simples e com maior capacidade do que os cookies tradicionais.

Distinguimos claramente os dois mecanismos à nossa disposição: **`localStorage`**, para dados que precisam persistir indefinidamente, tornando-se a escolha ideal para preferências do usuário e cache do lado do cliente; e **`sessionStorage`**, para dados efêmeros que devem durar apenas o tempo de uma sessão em uma aba, perfeito para preservar o estado durante uma tarefa específica.

Dominamos sua API síncrona, as melhores práticas para armazenar objetos com `JSON`, e como lidar graciosamente com os limites de armazenamento usando `try...catch`. Finalmente, desvendamos o poderoso **`storage` event**, que permite uma comunicação reativa entre abas, elevando a qualidade da experiência do usuário em aplicações multi-abas. Com o Web Storage em seu arsenal, você está agora plenamente capacitado para criar aplicações web que lembram de seus usuários e mantêm o estado de forma eficiente e robusta.
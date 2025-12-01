# Capítulo 21 – Atributos `data-*`

Ao longo de nossa jornada pelo HTML, aprendemos a usar uma vasta gama de atributos para definir o comportamento e as características dos elementos. Atributos como `id`, `class`, `lang` e `href` possuem propósitos bem definidos e padronizados. No entanto, ao construir aplicações web interativas, frequentemente nos deparamos com a necessidade de armazenar informações extras diretamente em um elemento — dados que não são necessariamente para exibição, mas que são cruciais para a lógica do nosso JavaScript.

Antes do HTML5, os desenvolvedores recorriam a "gambiarras". Eles sobrecarregavam o atributo `class` com informações (`class="item produto-id-123"`) ou criavam seus próprios atributos não padronizados, o que tornava o HTML inválido e propenso a conflitos futuros. Para resolver esse problema de forma elegante e padronizada, o HTML5 introduziu os **atributos de dados customizados**, mais conhecidos como **atributos `data-*`**.

Neste capítulo, vamos explorar em detalhe essa poderosa funcionalidade. Aprenderemos a sintaxe correta para criar atributos `data-*`, como acessá-los e manipulá-los de forma eficiente com JavaScript através da propriedade `dataset`, e até mesmo como utilizá-los como ganchos para estilização com CSS. O domínio dos atributos `data-*` é uma marca do desenvolvedor front-end moderno, permitindo a criação de componentes mais inteligentes e desacoplados.

## O Que São os Atributos `data-*`?

Os atributos `data-*` são uma categoria de atributos globais que permitem armazenar informações extras e customizadas em qualquer elemento HTML. A ideia é que esses dados sejam privados para a sua página ou aplicação, ou seja, são para uso dos seus scripts e não possuem nenhum significado ou efeito para o navegador ou para o usuário final.

Eles fornecem uma maneira de "embutir" dados que são relevantes para a interatividade ou configuração de um elemento, mantendo uma separação clara entre o conteúdo e a lógica.

### Sintaxe e Nomenclatura

A sintaxe é simples e segue regras específicas:

1. O nome do atributo deve começar obrigatoriamente com o prefixo **`data-`**.
2. Após o prefixo, o nome deve conter pelo menos um caractere.
3. O nome **não deve conter letras maiúsculas** no padrão ASCII. Os navegadores irão converter automaticamente qualquer letra maiúscula para minúscula.
4. O nome pode conter letras, números, hífens (`-`), pontos (`.`), dois-pontos (`:`) e underscores (`_`).
5. O valor do atributo pode ser qualquer string.

**Exemplos Válidos:**

```html
<div data-id-usuario="12345">...</div>
<li data-nome-produto="Laptop SuperPower" data-preco="4999.90">...</li>
<button data-acao="abrir-modal" data-alvo="#modal-contato">Contato</button>
<article data-info.publicacao="2025-06-15"></article>
```

## Acessando os Atributos `data-*` com JavaScript

A verdadeira utilidade dos atributos `data-*` é revelada quando os acessamos com JavaScript. Existem duas maneiras de fazer isso.

### Método Tradicional: `getAttribute()`

A forma mais antiga e universal de ler qualquer atributo de um elemento é usando `getAttribute()`.

```html
<div id="usuario" data-id="987" data-role="admin">João</div>

<script>
    const divUsuario = document.getElementById('usuario');

    const id = divUsuario.getAttribute('data-id');     // Retorna "987"
    const role = divUsuario.getAttribute('data-role'); // Retorna "admin"

    console.log(`ID do usuário: ${id}, Função: ${role}`);
</script>
```

Embora funcione perfeitamente, este método é um pouco verboso.

### Método Moderno e Recomendado: Propriedade `dataset`

O HTML5 introduziu uma interface muito mais limpa e conveniente: a propriedade `dataset`. Todo elemento HTML possui uma propriedade `dataset`, que é um objeto (`DOMStringMap`) contendo todos os atributos `data-*` daquele elemento.

O navegador faz uma conversão automática do nome do atributo para a chave do objeto:

- O prefixo `data-` é removido.
- Qualquer hífen (`-`) é removido e a letra seguinte é convertida para maiúscula (padrão camelCase).

Vamos ver a conversão na prática:

- `data-id-usuario` se torna `dataset.idUsuario`
- `data-nome-produto` se torna `dataset.nomeProduto`
- `data-role` se torna `dataset.role`

**Exemplo com `dataset`:**

```html
<div id="usuario" data-id="987" data-role="admin">João</div>

<script>
    const divUsuario = document.getElementById('usuario');

    // Lendo os dados
    const id = divUsuario.dataset.id;     // Retorna "987"
    const role = divUsuario.dataset.role; // Retorna "admin"
    console.log(`ID: ${id}, Função: ${role}`);

    // Modificando um dado
    divUsuario.dataset.role = 'editor';
    console.log(divUsuario.dataset.role); // Retorna "editor"

    // Adicionando um novo dado via script
    divUsuario.dataset.status = 'online'; // Adiciona o atributo data-status="online" ao elemento
</script>
```

Usar a propriedade `dataset` é a forma preferida, pois é mais limpa, mais legível e permite tanto a leitura quanto a escrita de dados de forma intuitiva.

## Usando `data-*` com CSS

É possível usar o valor de um atributo `data-*` para estilização, seja através de seletores de atributo ou da função `attr()` do CSS.

### Seletores de Atributo

Podemos aplicar estilos diferentes a um elemento com base na presença ou no valor de um atributo de dados.

```html
<style>
    .alerta {
        padding: 15px;
        border: 1px solid;
        border-radius: 4px;
        margin-bottom: 10px;
    }
    /* Estilo para alertas de sucesso */
    .alerta[data-tipo="sucesso"] {
        color: #155724;
        background-color: #d4edda;
        border-color: #c3e6cb;
    }
    /* Estilo para alertas de erro */
    .alerta[data-tipo="erro"] {
        color: #721c24;
        background-color: #f8d7da;
        border-color: #f5c6cb;
    }
</style>

<div class="alerta" data-tipo="sucesso">Operação concluída com sucesso!</div>
<div class="alerta" data-tipo="erro">Falha ao enviar o formulário.</div>
```

### Exibindo Dados com `attr()`

A função `attr()` do CSS pode ler o valor de um atributo e usá-lo em propriedades de conteúdo, como `content`. Isso é fantástico para criar tooltips (dicas de ferramenta) customizadas sem precisar de JavaScript.

```html
<style>
    .tooltip {
        position: relative;
        cursor: pointer;
    }
    .tooltip::after {
        content: attr(data-tooltip); /* Aqui a mágica acontece! */
        position: absolute;
        bottom: 125%; /* Posiciona acima do elemento */
        left: 50%;
        transform: translateX(-50%);
        background-color: #333;
        color: white;
        padding: 5px 10px;
        border-radius: 4px;
        white-space: nowrap;
        opacity: 0;
        visibility: hidden;
        transition: opacity 0.3s;
    }
    .tooltip:hover::after {
        opacity: 1;
        visibility: visible;
    }
</style>

<p>Passe o mouse sobre esta <span class="tooltip" data-tooltip="Esta é uma dica de ferramenta!">palavra</span> para ver a mágica.</p>
```

## Considerações Finais

Neste capítulo, desvendamos os atributos `data-*`, a solução padrão do HTML5 para um problema antigo: como armazenar metadados específicos da aplicação diretamente nos elementos. Vimos que eles fornecem uma maneira limpa e semântica de associar dados customizados a um elemento, separando-os de atributos com propósitos definidos como `id` ou `class`.

Aprendemos que a propriedade `dataset` do JavaScript é a interface moderna e preferida para ler e manipular esses dados, simplificando enormemente nossos scripts. Além disso, vimos como os atributos `data-*` podem ser usados como poderosos ganchos para estilização condicional com seletores de atributo e para a criação de interfaces ricas com a função `attr()` do CSS.

Usar atributos `data-*` em vez de soluções não padronizadas não é apenas uma boa prática — é um sinal de um código limpo, sustentável e preparado para o futuro, que se integra perfeitamente com as ferramentas modernas do desenvolvimento web.
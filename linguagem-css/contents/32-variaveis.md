# Capítulo 32 – Variáveis CSS (Custom Properties)

Ao longo de nossa jornada pelo CSS, nos tornamos proficientes em estilizar elementos. No entanto, em projetos de média a grande escala, um problema comum começa a surgir: a repetição. A mesma cor primária é usada em botões, links e títulos. O mesmo valor de `padding` é aplicado a dezenas de componentes. O mesmo `font-family` é declarado repetidamente. Quando chega a hora de fazer uma alteração — como ajustar a paleta de cores da marca — o processo se torna uma caçada tediosa e propensa a erros por toda a folha de estilos.

Por anos, a solução para este problema estava fora do CSS nativo, em pré-processadores como Sass e Less. Eles introduziram o conceito de variáveis, mas com uma limitação fundamental: elas eram compiladas para valores estáticos antes que o CSS chegasse ao navegador. E se pudéssemos ter variáveis verdadeiramente dinâmicas, que existissem e pudessem ser manipuladas em tempo real no próprio navegador?

É exatamente isso que as **Propriedades Customizadas CSS**, mais conhecidas como **Variáveis CSS**, nos oferecem. Elas são uma das adições mais poderosas e transformadoras ao CSS nos últimos anos. As variáveis nos permitem definir valores reutilizáveis em um único lugar, herdar esses valores através da cascata e até mesmo alterá-los com JavaScript, abrindo as portas para a criação de temas dinâmicos, sistemas de design consistentes e folhas de estilo drasticamente mais limpas e fáceis de manter.

Neste capítulo, vamos mergulhar fundo no mundo das Variáveis CSS. Aprenderemos a sintaxe para declará-las e usá-las, como elas interagem com a cascata e como podemos usá-las para gerenciar cores, dimensões e muito mais.

## Declarando e Usando Variáveis

O conceito de variáveis em CSS é dividido em duas partes: a declaração da variável (Propriedade Customizada) e o seu uso através da função `var()`.

### Declarando uma Variável

Uma variável CSS é declarada com um nome que **deve começar com dois hífens (`--`)**, seguido por um nome de sua escolha. Elas são definidas como qualquer outra propriedade CSS.

**Sintaxe:** `--nome-da-variavel: valor;`

Para que uma variável esteja disponível em todo o documento, a prática padrão é declará-la dentro da pseudo-classe `:root`, que representa o elemento `<html>` e tem a maior especificidade global.

**Exemplo de Declaração Global:**

```css
:root {
  --cor-primaria: #007bff;
  --tamanho-fonte-base: 16px;
  --espacamento-padrao: 1rem;
}
```

### Usando uma Variável com `var()`

Para aplicar o valor de uma variável a uma propriedade, usamos a função `var()`.

**Sintaxe:** `property: var(--nome-da-variavel [, valor-de-fallback]);`
- **`--nome-da-variavel` (obrigatório):** O nome da variável a ser usada.
- **`valor-de-fallback` (opcional):** Um valor a ser usado caso a variável não esteja definida ou seja inválida. Isso torna o código mais robusto.

**Exemplo de Uso:**

```css
h1 {
  color: var(--cor-primaria);
}

.card {
  padding: var(--espacamento-padrao);
  background-color: var(--cor-fundo-card, white); /* Usa 'white' se --cor-fundo-card não existir */
}
```

A grande vantagem é que, se você alterar o valor de `--cor-primaria` no `:root`, todos os elementos que usam essa variável serão atualizados instantaneamente.

## Variáveis de Cor: A Base do Theming

O caso de uso mais comum e poderoso para variáveis é a criação de uma paleta de cores centralizada. Isso não apenas garante consistência, mas também é a base para a implementação de temas, como um modo claro e escuro.

**Exemplo: Sistema de Cores e Tema Escuro**

```css
/* Tema Padrão (Claro) */
:root {
  --cor-texto: #212529;
  --cor-fundo: #ffffff;
  --cor-link: #007bff;
  --cor-borda: #dee2e6;
}

/* Sobrescrevendo variáveis para o Tema Escuro */
body.tema-escuro {
  --cor-texto: #f8f9fa;
  --cor-fundo: #343a40;
  --cor-link: #61dafb;
  --cor-borda: #495057;
}

/* Aplicação geral dos estilos */
body {
  color: var(--cor-texto);
  background-color: var(--cor-fundo);
}

a {
  color: var(--cor-link);
}

.card {
  border: 1px solid var(--cor-borda);
  background-color: var(--cor-fundo);
}
```

Para ativar o tema escuro, você só precisa adicionar a classe `tema-escuro` ao `<body>` usando JavaScript. Todas as cores do site serão atualizadas dinamicamente, sem a necessidade de recarregar a página ou reescrever dezenas de regras CSS.

## Variáveis de Dimensões

Além das cores, as variáveis são perfeitas para manter a consistência em espaçamentos, tamanhos de fonte e outras dimensões, criando um sistema de design rítmico e harmonioso.

**Exemplo: Sistema de Espaçamento e Tipografia**

```css
:root {
  /* Tipografia */
  --familia-fonte-base: 'Helvetica', 'Arial', sans-serif;
  --tamanho-fonte-base: 1rem;
  --tamanho-titulo-h1: 2.5rem;

  /* Espaçamento (baseado em uma escala) */
  --espaco-xs: 0.25rem; /* 4px */
  --espaco-sm: 0.5rem;  /* 8px */
  --espaco-md: 1rem;    /* 16px */
  --espaco-lg: 2rem;    /* 32px */

  /* Layout */
  --largura-maxima-container: 1140px;
}

body {
  font-family: var(--familia-fonte-base);
  font-size: var(--tamanho-fonte-base);
}

h1 {
  font-size: var(--tamanho-titulo-h1);
  margin-bottom: var(--espaco-md);
}

.container {
  max-width: var(--largura-maxima-container);
  margin: 0 auto;
  padding: 0 var(--espaco-lg);
}
```

Este sistema garante que todos os espaçamentos e tamanhos de fonte sigam uma escala consistente, tornando o design mais profissional e a manutenção muito mais simples.

## A Cascata de Variáveis

As Propriedades Customizadas são herdadas e seguem as regras da cascata, assim como qualquer outra propriedade CSS. Isso significa que você pode definir uma variável globalmente e, em seguida, sobrescrevê-la para um componente específico, dando-lhe um escopo local.

**Exemplo: Sobrescrevendo Variáveis Localmente**

```css
/* Variável global */
:root {
  --cor-primaria: blue;
}

button {
  background-color: var(--cor-primaria); /* Será azul */
}

/* O componente de alerta de perigo tem seu próprio escopo */
.alerta-perigo {
  --cor-primaria: red; /* A variável é redefinida apenas para este componente e seus filhos */
}

/* O botão dentro de .alerta-perigo herdará a nova variável */
.alerta-perigo button {
  /* Este botão será vermelho, pois a variável --cor-primaria foi sobrescrita
     pelo seu ancestral .alerta-perigo */
  background-color: var(--cor-primaria); 
}
```

Este comportamento é o que torna as variáveis CSS tão dinâmicas e superiores às variáveis de pré-processadores. O valor da variável é resolvido em tempo real, com base no elemento ao qual está sendo aplicado.

## Nomeando Variáveis

A forma como você nomeia suas variáveis tem um grande impacto na manutenibilidade do seu projeto. A melhor prática é usar nomes abstratos ou semânticos em vez de nomes literais.

- **Ruim (Literal):** `--cor-azul-claro`, `--padding-16px`
    - **Problema:** E se você decidir que sua cor primária agora é verde? A variável `--cor-azul-claro` tendo o valor `green` seria extremamente confusa.
- **Bom (Abstrato/Semântico):** `--cor-primaria`, `--cor-texto-principal`, `--espacamento-medio`
    - **Vantagem:** O nome descreve o **propósito** do valor, não o valor em si. `--cor-primaria` pode ser azul, verde ou qualquer outra cor, e o nome continuará fazendo sentido. Isso torna a criação de temas e a refatoração do design muito mais fáceis.

## Boas Práticas com Variáveis

As Variáveis CSS são um dos pilares do desenvolvimento front-end moderno. Usá-las corretamente desde o início estabelecerá uma base sólida e escalável para seus projetos.

- **Centralize no `:root`:** Declare suas variáveis globais (tokens de design como cores, fontes, espaçamentos) na pseudo-classe `:root`. Isso as torna disponíveis em todo o seu documento e cria uma "fonte única da verdade" para o seu sistema de design.
- **Nomeie com Semântica:** Adote uma convenção de nomenclatura que descreva o papel da variável, não seu valor. `--cor-acao` é melhor que `--cor-azul-botao`. `--tamanho-fonte-titulo` é melhor que `--tamanho-32px`.
- **Organize suas Variáveis:** Agrupe suas declarações de variáveis no `:root` por categoria (cores, tipografia, espaçamento, sombras, etc.) usando comentários. Isso torna sua folha de estilos mais fácil de navegar e entender.
- **Aproveite a Cascata:** Use a capacidade de sobrescrever variáveis em escopos mais específicos. Esta é a técnica ideal para criar variações de componentes (ex: `.botao--perigo`) ou para aplicar temas a seções específicas de uma página.
- **Use Fallbacks para Robustez:** Ao usar a função `var()`, considere fornecer um valor de fallback (`var(--minha-cor, black)`). Isso pode salvar seu design de quebrar caso a variável não seja definida, especialmente em cenários onde as variáveis podem ser manipuladas por JavaScript.
- **Combine com `calc()`:** O poder das variáveis é amplificado quando combinado com outras funções como `calc()`. `width: calc(100% - var(--espacamento-lateral));` é um exemplo de código flexível e fácil de manter.

## Considerações Finais

Neste capítulo, exploramos as Variáveis CSS, um recurso que efetivamente introduz o poder da programação no coração de nossas folhas de estilo. Vimos como elas resolvem o problema da repetição, promovem a consistência e tornam a manutenção e a criação de temas em larga escala uma tarefa trivial.

Compreender que as variáveis são dinâmicas e que respeitam as regras da cascata e da herança é o que destrava seu verdadeiro potencial. Elas não são apenas uma conveniência; são uma mudança de paradigma na forma como arquitetamos nosso CSS, permitindo a criação de sistemas de design verdadeiramente modulares, escaláveis e resilientes.
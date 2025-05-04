# Capítulo 2 – Variáveis, Tipos de Dados e Operadores

## O Papel das Variáveis na Lógica de Programação

Ao desenvolver algoritmos, lidamos constantemente com informações que precisam ser armazenadas temporariamente, manipuladas e consultadas ao longo da execução. Para isso, usamos as **variáveis**. Uma variável é uma espécie de “caixa” com um nome, que guarda um valor em sua memória durante a execução do algoritmo. O valor contido nela pode ser modificado sempre que necessário, de acordo com o andamento da lógica.

O nome de uma variável deve ser representativo, ou seja, precisa refletir o tipo de dado que ela armazena. Por exemplo, para armazenar a idade de uma pessoa, um nome como `idade` é preferível a nomes genéricos como `x` ou `valor1`, exceto em contextos matemáticos.

Cada variável possui um **tipo**, que define a natureza dos dados que ela pode armazenar — como números inteiros, textos, valores booleanos (verdadeiro/falso), entre outros. A correta definição do tipo de uma variável evita erros e ajuda a manter a integridade das operações realizadas.

```plaintext
Início
    Declare idade como inteiro
    idade <- 25
    Escreva "A idade informada é: ", idade
Fim
```

No exemplo acima, declaramos uma variável chamada `idade`, do tipo inteiro, e atribuimos o valor 25 a ela.

## Constantes e Atribuições

Diferentemente das variáveis, que podem ter seus valores alterados, **constantes** são identificadores que armazenam valores fixos durante toda a execução do algoritmo. Elas são úteis para representar dados que não devem ser modificados, como o valor de pi (π), o número de dias da semana, ou a taxa de um imposto fixo.

A **atribuição** é o ato de dar um valor a uma variável. Em pseudocódigos e em Português Estruturado, geralmente usamos a seta `<-` (lê-se “recebe”) para indicar que o valor do lado direito deve ser armazenado no identificador do lado esquerdo.

```plaintext
Início
    Constante PI <- 3.1416
    Declare raio, area como real
    Escreva "Informe o raio do círculo:"
    Leia raio
    area <- PI * raio * raio
    Escreva "Área do círculo: ", area
Fim
```

## Tipos de Dados: A Natureza das Informações

Toda informação manipulada em um algoritmo possui um tipo, que define como ela é armazenada na memória e como pode ser processada. Os **tipos de dados** são fundamentais para garantir que operações sejam feitas corretamente.

### Tipos Primitivos ou Simples

Estes são os tipos básicos com os quais construímos estruturas mais complexas. Os principais tipos primitivos são:

- **Inteiro**: armazena números inteiros (sem casas decimais), como `5`, `-23`, `0`.
- **Real**: armazena números com parte decimal (também chamados de ponto flutuante), como `3.14`, `-0.5`, `100.0`.
- **Caractere**: armazena um único símbolo ou caractere, como `'a'`, `'9'`, `'$'`. Costuma ser representado entre aspas simples.
- **Lógico (booleano)**: armazena um valor de `verdadeiro` ou `falso`, utilizado principalmente em estruturas de decisão e repetição.

```plaintext
Início
    Declare idade como inteiro
    Declare altura como real
    Declare letraInicial como caractere
    Declare maiorIdade como lógico

    idade <- 18
    altura <- 1.75
    letraInicial <- 'M'
    maiorIdade <- verdadeiro
Fim
```

### Tipos Estruturados

Enquanto os tipos primitivos armazenam valores simples, os **tipos estruturados** permitem armazenar coleções de valores.

- **Cadeia de Caracteres**: Também chamada de **string**, uma cadeia de caracteres é uma sequência de símbolos (letras, números, espaços, etc.), usada para representar textos. Exemplo: `"João Silva"` ou `"Rua das Flores, 45"`.
- **Vetor**: Um **vetor** é uma estrutura que agrupa uma sequência de elementos do mesmo tipo, organizados linearmente. É como uma lista onde cada elemento pode ser acessado por um índice.
- **Matriz**: A **matriz** é uma generalização do vetor: trata-se de uma estrutura bidimensional (como uma tabela) onde os dados são organizados em linhas e colunas.

```plaintext
Início
    Declare notas[5] como real
    notas[1] <- 7.5
    notas[2] <- 8.0
    notas[3] <- 9.2
    notas[4] <- 6.8
    notas[5] <- 10.0
    Escreva "Nota da terceira prova: ", notas[3]
Fim
```

## Leitura e Escrita de Dados

A interação com o usuário é essencial na maioria dos algoritmos. Por isso, é comum usar instruções para **ler** dados (recebendo entrada do usuário) e para **escrever** dados (mostrando saídas ou resultados).

- `Leia` é a instrução usada para capturar valores digitados.
- `Escreva` é a instrução usada para exibir mensagens ou valores.

```plaintext
Início
    Declare nome como cadeia
    Declare idade como inteiro
    Escreva "Informe seu nome:"
    Leia nome
    Escreva "Informe sua idade:"
    Leia idade
    Escreva "Olá, ", nome, "! Você tem ", idade, " anos."
Fim
```

## Operadores: Realizando Operações

Os **operadores** são símbolos que representam ações ou comparações a serem feitas com dados. São divididos em três grandes grupos: aritméticos, relacionais e lógicos.

### Operadores Aritméticos

Usados para realizar cálculos matemáticos. Os mais comuns são:

- `+` (adição)
- `-` (subtração)
- `*` (multiplicação)
- `/` (divisão)
- `^` (exponenciação ou potência)

```plaintext
Início
    Declare a, b, resultado como real
    a <- 10
    b <- 2
    resultado <- a / b
    Escreva "Resultado da divisão: ", resultado
Fim
```

### Operadores Relacionais

Usados para comparar valores. O resultado dessas comparações é sempre um valor lógico (verdadeiro ou falso).

- `=` (igual a)
- `<>` (diferente de)
- `>` (maior que)
- `<` (menor que)
- `>=` (maior ou igual a)
- `<=` (menor ou igual a)

```plaintext
Início
    Declare idade como inteiro
    idade <- 20
    Se idade >= 18 então
        Escreva "Maior de idade"
    Senão
        Escreva "Menor de idade"
Fim
```

### Operadores Lógicos

Utilizados para combinar expressões lógicas. São essenciais para a construção de condições compostas.

- `E` (conjunção): verdadeiro se ambas as condições forem verdadeiras.
- `OU` (disjunção): verdadeiro se ao menos uma das condições for verdadeira.
- `NÃO` (negação): inverte o valor lógico da condição.

```plaintext
Início
    Declare idade como inteiro
    Declare possuiTitulo como lógico
    idade <- 20
    possuiTitulo <- verdadeiro

    Se idade >= 16 E possuiTitulo então
        Escreva "Pode votar"
    Senão
        Escreva "Não pode votar"
Fim
```

## Considerações Finais

Neste capítulo, exploramos os pilares fundamentais que sustentam a lógica de programação prática: o armazenamento de dados através de variáveis e constantes, os tipos de dados disponíveis para representar diferentes naturezas de informação, as formas de interação com o usuário por meio da leitura e escrita, e os operadores que permitem realizar cálculos, comparações e decisões lógicas.

Dominar esses conceitos é essencial para qualquer programador iniciante e será a base para os próximos capítulos, onde aprenderemos a tomar decisões e repetir ações por meio de estruturas de controle. A clareza com que você lida com variáveis e dados agora determinará sua facilidade nos próximos desafios da programação.

# Capítulo 8 – Estruturas de Dados Heterogêneas: Registros

## Introdução às Estruturas de Dados Heterogêneas

Na lógica de programação, trabalhamos frequentemente com variáveis e estruturas que armazenam dados. As estruturas homogêneas, como vetores e matrizes, armazenam elementos de um mesmo tipo. No entanto, há situações em que precisamos agrupar informações de naturezas diferentes em uma única estrutura. É nesse contexto que surgem as estruturas de dados heterogêneas, capazes de agrupar variáveis de tipos distintos de maneira organizada.

As **estruturas heterogêneas** são fundamentais, por exemplo, para representar um “aluno” com atributos como nome (cadeia de caracteres), idade (inteiro), média (real) e situação (caractere ou lógico). Tais estruturas permitem que dados com diferentes representações semânticas sejam tratados como uma unidade lógica.

## Registros: Conceito e Utilização

Entre as estruturas de dados heterogêneas, o tipo mais comum é o **registro**. Um registro pode ser entendido como uma coleção nomeada de campos, onde cada campo pode conter um valor de um tipo de dado diferente. Essa estrutura é particularmente útil para modelar entidades do mundo real dentro de um programa.

Um registro é uma estrutura composta por diferentes campos que podem ser de tipos distintos. Cada campo é nomeado e acessado por meio de um identificador. Em termos práticos, o registro funciona como um "formulário", onde cada campo guarda uma informação específica.

Vamos imaginar uma estrutura para representar um funcionário:

```plaintext
registro Funcionario
    nome: cadeia
    idade: inteiro
    salario: real
    ativo: lógico
fimregistro

variável f: Funcionario

f.nome <- "Carlos Silva"
f.idade <- 34
f.salario <- 4500.75
f.ativo <- verdadeiro
```

Neste exemplo, criamos um tipo de dado chamado `Funcionario`, com quatro campos de tipos distintos. Em seguida, declaramos uma variável `f` do tipo `Funcionario` e atribuímos valores a cada um dos seus campos. Essa estrutura nos permite armazenar e manipular informações completas de um funcionário, em um único bloco de dados.

O uso de registros traz diversas vantagens para o desenvolvimento de algoritmos e programas estruturados:

- **Organização e clareza**: Registros permitem que diferentes atributos de uma mesma entidade fiquem agrupados, tornando o código mais legível e coerente.
- **Reutilização**: Uma vez declarado, o tipo de registro pode ser utilizado em várias partes do programa, reduzindo redundâncias.
- **Modularização**: Facilita a divisão lógica dos dados, contribuindo para a manutenção e escalabilidade do código.

## Iterando sobre Registros

Embora registros não sejam coleções indexadas como vetores ou matrizes, é comum trabalharmos com **vetores de registros**, isto é, coleções de registros do mesmo tipo. Isso nos permite representar, por exemplo, uma lista de funcionários.

```portugues
constante MAX <- 3
registro Funcionario
    nome: cadeia
    idade: inteiro
    salario: real
fimregistro

variável i: inteiro
variável funcionarios: vetor[1..MAX] de Funcionario

para i de 1 até MAX faça
    escreva("Digite o nome do funcionário: ")
    leia(funcionarios[i].nome)
    escreva("Digite a idade do funcionário: ")
    leia(funcionarios[i].idade)
    escreva("Digite o salário do funcionário: ")
    leia(funcionarios[i].salario)
fimpara

para i de 1 até MAX faça
    escreva("Nome: ", funcionarios[i].nome)
    escreva(" - Idade: ", funcionarios[i].idade)
    escreva(" - Salário: ", funcionarios[i].salario)
fimpara
```

Neste exemplo, manipulamos um vetor de registros, permitindo armazenar os dados de três funcionários distintos. Cada elemento do vetor é um registro completo, com todos os campos definidos.

Essa forma de estruturação é extremamente útil em aplicações que exigem manipulação de conjuntos de dados complexos, como em sistemas de gestão de alunos, produtos, clientes, entre outros.

## Considerações Finais

As estruturas de dados heterogêneas, em especial os registros, representam um avanço em relação à manipulação de dados mais simples e homogêneos. Por permitirem o agrupamento de informações distintas sob uma mesma identidade, são peças-chave na modelagem de dados complexos e na criação de programas robustos. Seu uso combinado com outras estruturas, como vetores, amplia ainda mais o potencial de organização e manipulação de dados dentro de um algoritmo.

Compreender os registros é essencial para a transição a paradigmas mais avançados, como a programação orientada a objetos, onde o conceito de encapsulamento de dados evolui a partir dessas mesmas ideias estruturadas.
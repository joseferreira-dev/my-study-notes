# Capítulo 5 – Estruturas de Dados Homogêneas: Vetores e Matrizes

Em algoritmos simples, utilizamos variáveis isoladas para armazenar dados. No entanto, à medida que os problemas se tornam mais complexos, é comum precisarmos manipular **grupos de dados** do mesmo tipo, como uma lista de nomes, uma sequência de números ou uma tabela de valores. Para isso, utilizamos as chamadas **estruturas de dados homogêneas**, sendo os **vetores** e as **matrizes** as formas mais elementares e importantes dessa categoria.

Neste capítulo, exploraremos detalhadamente os conceitos, estruturas e formas de uso de vetores e matrizes, bem como as estratégias para percorrê-los por meio de estruturas de repetição.

## O Que São Vetores?

Um **vetor** é uma estrutura de dados linear que armazena uma **coleção de elementos do mesmo tipo**, organizados em **uma única dimensão**. Pode ser entendido como uma "caixa com várias gavetas", onde cada gaveta guarda um valor e possui um número de identificação chamado **índice** ou **posição**.

Esses índices geralmente começam em **1** (em notação didática ou em Português Estruturado) e permitem acessar individualmente cada elemento do vetor.

Por exemplo, imagine um vetor que armazena as notas de 5 alunos. Cada posição do vetor representa a nota de um aluno.

```
Notas = [ 8.0, 6.5, 9.2, 7.0, 5.5 ]
           1    2    3    4    5
```

A posição 3 contém o valor 9.2, ou seja, a nota do terceiro aluno.

Em Português Estruturado, a declaração de um vetor deve incluir:

- Nome da variável
- Tipo de dado
- Intervalo de índices (tamanho)

```plaintext
Declare notas[1..5] como real
```

Este comando cria um vetor chamado `notas`, com 5 posições, capaz de armazenar valores reais (decimais).

## Utilizando Vetores na Prática

A manipulação de vetores é quase sempre feita com o auxílio de **laços de repetição**, já que seus elementos são acessados por índices numéricos.

```plaintext
Início
    Declare notas[1..5] como real
    Declare i como inteiro
    Declare soma, media como real

    soma <- 0

    Para i de 1 até 5 faça
        Escreva "Digite a nota ", i, ": "
        Leia notas[i]
        soma <- soma + notas[i]
    FimPara

    media <- soma / 5
    Escreva "A média das notas é: ", media
Fim
```

Neste exemplo, cada nota é lida individualmente e somada a uma variável acumuladora. Ao final, a média é calculada com base na soma total dividida pela quantidade de elementos.

## O Que São Matrizes?

Uma **matriz** é uma estrutura de dados bidimensional, ou seja, uma **tabela de dados organizada em linhas e colunas**. É como uma planilha onde cada célula possui duas coordenadas: **linha** e **coluna**.

Enquanto o vetor armazena dados em uma única dimensão (como uma lista), a matriz permite organizar informações em duas dimensões, tornando-se ideal para representar tabelas, quadros de horários, mapas, grades escolares, entre outros.

Imagine uma matriz 3x3 de números inteiros:

```
[1  2  3]
[4  5  6]
[7  8  9]
```

Cada elemento da matriz pode ser acessado por meio de dois índices: o da linha e o da coluna. Por exemplo, o valor na **linha 2, coluna 3** é 6.

Para a declaração de matrizes utiliza-se a seguinte estrutura:

```plaintext
Declare tabela[1..3, 1..3] como inteiro
```

Essa declaração cria uma matriz com 3 linhas e 3 colunas.

## Utilizando Matrizes na Prática

Manipular matrizes exige **duas estruturas de repetição aninhadas**: uma para percorrer as linhas e outra para as colunas.

```plaintext
Início
    Declare matriz[1..2, 1..2] como inteiro
    Declare i, j como inteiro

    Para i de 1 até 2 faça
        Para j de 1 até 2 faça
            Escreva "Digite o valor da posição [", i, ",", j, "]: "
            Leia matriz[i, j]
        FimPara
    FimPara

    Escreva "Conteúdo da matriz:"
    Para i de 1 até 2 faça
        Para j de 1 até 2 faça
            Escreva matriz[i, j], " "
        FimPara
        Escreva ""  // Pula linha após cada linha da matriz
    FimPara
Fim
```

Este algoritmo lê os elementos da matriz linha por linha e depois imprime a tabela formatada.

## Iterando Sobre Vetores e Matrizes

Um dos maiores benefícios dessas estruturas é a **facilidade de percorrê-las com laços de repetição**. Essa característica permite automatizar tarefas como somatórios, buscas, contagens, ordenações, entre outras operações.

Percorrer um vetor:

```plaintext
Para i de 1 até tamanho faça
    <comandos usando vetor[i]>
FimPara
```

Percorrer uma matriz:

```plaintext
Para i de 1 até total_linhas faça
    Para j de 1 até total_colunas faça
        <comandos usando matriz[i, j]>
    FimPara
FimPara
```

Essas estruturas podem ser adaptadas conforme o problema. É comum, por exemplo, usar vetores para armazenar nomes de alunos e um segundo vetor para suas notas, percorrendo ambos ao mesmo tempo para associar cada nome à sua nota.

## Aplicação Prática: Exemplo Completo com Matriz

Vamos agora ver um exemplo mais robusto, que usa uma matriz para armazenar notas de 3 alunos em 4 avaliações e calcular a média de cada um:

```plaintext
Início
    Declare notas[1..3, 1..4] como real
    Declare i, j como inteiro
    Declare soma, media como real

    Para i de 1 até 3 faça
        Escreva "Digite as notas do aluno ", i
        Para j de 1 até 4 faça
            Escreva "Nota ", j, ": "
            Leia notas[i, j]
        FimPara
    FimPara

    Para i de 1 até 3 faça
        soma ← 0
        Para j de 1 até 4 faça
            soma ← soma + notas[i, j]
        FimPara
        media ← soma / 4
        Escreva "Média do aluno ", i, ": ", media
    FimPara
Fim
```

Neste algoritmo:

- Usamos uma matriz 3x4 para armazenar as notas de 3 alunos em 4 provas.
- Calculamos a média de cada aluno percorrendo suas 4 notas.
- Usamos laços aninhados para ler e processar os dados de forma eficiente.

## Considerações Finais

Vetores e matrizes são fundamentais para o desenvolvimento de algoritmos mais avançados, permitindo o armazenamento e manipulação estruturada de grandes volumes de dados.

- **Vetores** são ideais para listas simples e sequenciais.
- **Matrizes** são usadas quando há necessidade de trabalhar com dados tabulares.
- A **combinação de vetores/matrizes com laços de repetição** permite implementar algoritmos potentes e eficientes.

No próximo capítulo, exploraremos a **modularização de algoritmos**, introduzindo os conceitos de **procedimentos e funções**, ferramentas indispensáveis para construir programas organizados e reutilizáveis.

# Capítulo 4 – Estruturas de Repetição: Automatizando Tarefas com Laços de Execução

## Por que Repetir?

Em lógica de programação, frequentemente nos deparamos com situações em que precisamos executar uma mesma ação múltiplas vezes. Imagine, por exemplo, um programa que deve somar 100 números digitados pelo usuário, ou percorrer uma lista de 50 nomes para verificar qual deles começa com determinada letra. Executar os mesmos comandos repetidamente, de forma manual ou duplicada no código, além de ser impraticável, torna o algoritmo extenso, repetitivo e difícil de manter.

É para resolver esse tipo de problema que utilizamos as **estruturas de repetição**, também conhecidas como **laços** ou **loops**. Elas permitem que **um bloco de comandos seja executado várias vezes, de maneira automática**, com base em uma condição lógica ou um número pré-determinado de repetições.

Este capítulo abordará as principais estruturas de repetição: `Para` (laço com contagem definida), `Enquanto` (repetição com teste no início), `Faça-Enquanto` (teste no final) e os comandos auxiliares `Pare` e `Continue`, que modificam o fluxo natural dos laços.

## A Estrutura Para (For)

O laço `Para` é usado quando sabemos **antecipadamente** quantas vezes uma ação deverá ser repetida. Ele é extremamente útil para percorrer sequências numéricas, como listas, vetores ou simplesmente realizar um número fixo de repetições.

A estrutura `Para` é composta por três partes principais: **inicialização do contador**, **condição de parada** e **atualização do contador**. No entanto, em Português Estruturado, essas operações estão implícitas na própria estrutura da repetição.

```plaintext
Para <variável> de <valor_inicial> até <valor_final> faça
    <comandos>
FimPara
```

O comando `Para` cria uma variável de controle que começa em um valor inicial e é incrementada automaticamente até atingir (ou ultrapassar) o valor final.

```plaintext
Início
    Para i de 1 até 5 faça
        Escreva "Contador: ", i
    FimPara
Fim
```

Este algoritmo imprimirá:

```
Contador: 1  
Contador: 2  
Contador: 3  
Contador: 4  
Contador: 5  
```

Alguns algoritmos também admitem um incremento diferente de 1, utilizando uma variação como `de 10 até 0 passo -2`, para contagens regressivas ou saltos personalizados.

## A Estrutura Enquanto (While)

Diferente do `Para`, a estrutura `Enquanto` é usada quando **não sabemos exatamente quantas vezes a repetição ocorrerá**. A execução depende do resultado de uma **condição lógica**, que é avaliada **antes** de cada repetição. Ou seja, se a condição for falsa logo no início, o bloco de comandos pode nunca ser executado.

```plaintext
Enquanto <condição> faça
    <comandos>
FimEnquanto
```

No seguinte exemplo, o programa continua lendo e exibindo os números digitados enquanto o usuário não informar o valor 0.

```plaintext
Início
    Declare numero como inteiro
    Leia numero
    Enquanto numero <> 0 faça
        Escreva "Você digitou: ", numero
        Leia numero
    FimEnquanto
    Escreva "Programa encerrado."
Fim
```

É fundamental garantir que a condição, em algum momento, se torne falsa. Caso contrário, o laço entrará em um **loop infinito**, travando o programa.

## A Estrutura Faça ... Enquanto (Do ... While)

A estrutura `Faça ... Enquanto` funciona de maneira semelhante ao `Enquanto`, com uma diferença crucial: **a condição é testada apenas no final da repetição**. Isso garante que **o bloco de comandos seja executado pelo menos uma vez**, mesmo que a condição inicial já seja falsa.

Essa estrutura é útil em situações onde uma ação deve ser realizada ao menos uma vez antes de decidir se deve ser repetida.

```plaintext
Faça
    <comandos>
Enquanto <condição>
```

Veja o seguinte exemplo de uso:

```plaintext
Início
    Declare senha como inteiro
    Faça
        Escreva "Digite a senha: "
        Leia senha
    Enquanto senha <> 1234
    Escreva "Acesso autorizado."
Fim
```

Neste caso, o programa sempre pedirá a senha pelo menos uma vez, mesmo que o usuário já digite o número correto na primeira tentativa.

## Controle de Fluxo: Comandos Pare e Continue

Durante a execução de um laço, pode haver a necessidade de **interromper** a repetição antes do fim (por exemplo, quando se encontra o que se procurava) ou **pular uma iteração específica**. Para isso, utilizamos os comandos especiais `Pare` e `Continue`.

### O comando Pare

O `Pare` serve para **encerrar imediatamente** a execução do laço em que está inserido. Ele é útil quando se encontra um valor que torna desnecessária a continuação do loop.

```plaintext
Início
    Para i de 1 até 10 faça
        Se i = 5 então
            Escreva "Valor 5 encontrado. Interrompendo o laço."
            Pare
        FimSe
        Escreva "Valor: ", i
    FimPara
Fim
```

Neste exemplo, o laço será interrompido assim que o valor `5` for alcançado, ignorando os números seguintes.

### O comando Continue

O `Continue` faz o oposto: ele **pula o restante do bloco de comandos** da repetição **apenas naquela iteração atual**, e volta a testar a condição do laço normalmente.

```plaintext
Início
    Para i de 1 até 5 faça
        Se i = 3 então
            Continue
        FimSe
        Escreva "Número: ", i
    FimPara
Fim
```

A saída desse algoritmo será:

```
Número: 1  
Número: 2  
Número: 4  
Número: 5  
```

Observe que o número 3 foi ignorado, pois o `Continue` pulou aquela iteração específica.

## Considerações Finais

As estruturas de repetição são elementos essenciais na construção de algoritmos eficientes e automatizados. Elas permitem iterar sobre dados, realizar cálculos repetitivos, validar entradas, construir tabelas, processar listas e muito mais.

- O laço `Para` é ideal quando o número de repetições é conhecido.
- O laço `Enquanto` é usado quando o número de repetições depende de uma condição que pode ser falsa já no início.
- O `Faça ... Enquanto` é útil quando a ação precisa ocorrer ao menos uma vez antes de testar a condição.
- `Pare` e `Continue` são ferramentas poderosas para controlar a execução dos laços, mas devem ser usados com cautela para não comprometer a clareza do algoritmo.

Nos próximos capítulos, avançaremos para um novo patamar da lógica de programação, onde aprenderemos a **modularizar algoritmos por meio de funções e procedimentos**, trazendo mais organização, reutilização e clareza ao código. Antes disso, iremos abordar algumas estruturas de dados comuns na programação: **vetores** e **matrizes**.

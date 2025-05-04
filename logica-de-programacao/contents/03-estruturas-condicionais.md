# Capítulo 3 – Estruturas Condicionais: Tomando Decisões com Lógica

## Introdução à Tomada de Decisão

A programação, assim como o cotidiano humano, exige decisões baseadas em condições. Um algoritmo raramente é linear do início ao fim; com frequência, ele precisa “escolher” um caminho a seguir dependendo de uma determinada situação. É isso que chamamos de **estrutura condicional**: um mecanismo que permite executar blocos diferentes de comandos dependendo do resultado de uma expressão lógica (verdadeira ou falsa).

Imagine um simples caixa eletrônico. Ao digitar sua senha, o sistema verifica se ela está correta para só então permitir acesso à conta. Esse é um exemplo clássico de estrutura condicional: **“Se a senha estiver correta, então acesse a conta”**. Esse tipo de decisão lógica está presente em quase todos os algoritmos e sistemas computacionais.

## A Estrutura Se-Então (If-Then)

A forma mais básica de decisão é a condicional simples, que permite executar um bloco de comandos **somente se** uma condição for verdadeira. Caso a condição não seja satisfeita, nada acontece e o algoritmo segue seu fluxo normal.

```plaintext
Se <condição> então
    <comandos>
FimSe
```

A palavra-chave `Se` inicia a condição, `então` separa a condição dos comandos, e `FimSe` marca o fim do bloco condicional.

```plaintext
Início
    Declare idade como inteiro
    Leia idade
    Se idade >= 18 então
        Escreva "Você é maior de idade."
    FimSe
Fim
```

Nesse exemplo, a mensagem só será exibida **se** a idade for maior ou igual a 18. Caso contrário, o algoritmo continuará sem executar o bloco condicional.

## A Estrutura Se-Então-Senão (If-Then-Else)

Nem sempre basta agir quando uma condição é verdadeira; muitas vezes também é necessário definir o que acontece **caso ela seja falsa**. Para isso, usamos a forma composta da estrutura condicional: **Se-Então-Senão**.

Essa estrutura nos permite escolher entre dois caminhos: um quando a condição é verdadeira e outro quando ela é falsa.

```plaintext
Se <condição> então
    <comandos para caso verdadeiro>
Senão
    <comandos para caso falso>
FimSe
```

Adicionalmente ao que se tem em uma condicional simples, usa-se a palavra-chave `senão` para incluir uma ação caso a condição seja falsa.

```plaintext
Início
    Declare nota as real
    Leia nota
    Se nota >= 7 então
        Escreva "Aprovado"
    Senão
        Escreva "Reprovado"
FimSe
Fim
```

Se a nota for maior ou igual a 7, o algoritmo exibirá “Aprovado”. Caso contrário, exibirá “Reprovado”.

## Condições Aninhadas: Decisões Dentro de Decisões

Em certos casos, uma única estrutura condicional não é suficiente para tratar todas as possibilidades. Quando precisamos testar várias condições em sequência, utilizamos as chamadas **estruturas aninhadas**, ou seja, uma condicional dentro de outra.

Embora funcionem bem, estruturas aninhadas excessivamente profundas podem tornar o código difícil de ler. Por isso, deve-se usá-las com cautela e, quando possível, considerar alternativas como `Escolha-Caso`.

```plaintext
Início
    Declare nota como real
    Leia nota
    Se nota >= 9 então
        Escreva "Excelente"
    Senão
        Se nota >= 7 então
            Escreva "Bom"
        Senão
            Se nota >= 5 então
                Escreva "Regular"
            Senão
                Escreva "Reprovado"
            FimSe
        FimSe
    FimSe
Fim
```

Note que há vários blocos `Se-então-Senão`, cada um testando uma nova condição. É possível simplificar visualmente esse tipo de estrutura com recuo adequado ou com o uso de `Escolha-Caso`, que será abordado a seguir.

## A Estrutura Escolha-Caso (Switch-Case)

Quando temos **vários valores possíveis para uma mesma variável**, e ações diferentes para cada valor, a estrutura `Escolha-Caso` se torna mais elegante e organizada do que vários `Se-então-Senão`.

Ela funciona como uma forma de seleção múltipla: a variável é avaliada uma única vez, e o algoritmo escolhe entre diferentes blocos de comandos de acordo com o valor informado.

```plaintext
Escolha <variável>
    Caso <valor1>
        <comandos>
    Caso <valor2>
        <comandos>
    ...
    Caso contrário
        <comandos>
FimEscolha
```

O seguinte exemplo ilustra bem uma situação de uso dessa estrutura:

```plaintext
Início
    Declare opcao como inteiro
    Escreva "Menu:"
    Escreva "1 - Cadastrar"
    Escreva "2 - Editar"
    Escreva "3 - Excluir"
    Escreva "4 - Sair"
    Leia opcao

    Escolha opcao
        Caso 1
            Escreva "Você escolheu cadastrar."
        Caso 2
            Escreva "Você escolheu editar."
        Caso 3
            Escreva "Você escolheu excluir."
        Caso 4
            Escreva "Encerrando o programa."
        Caso contrário
            Escreva "Opção inválida."
    FimEscolha
Fim
```

Neste exemplo, o programa apresenta um menu e executa uma ação conforme a opção informada pelo usuário. Caso o valor digitado não esteja entre os casos listados, o `Caso contrário` será executado.

## Boas Práticas no Uso de Condicionais

Embora as estruturas condicionais sejam extremamente úteis, seu uso exagerado ou mal planejado pode gerar algoritmos confusos e difíceis de manter. Algumas boas práticas incluem:

- Preferir `Escolha-Caso` quando há muitas verificações de igualdade com uma mesma variável.
- Evitar condicionais aninhadas em profundidade excessiva; se necessário, considere refatorar o algoritmo.
- Garantir clareza nas condições, evitando expressões lógicas muito longas ou pouco intuitivas.
- Comentar trechos complexos para facilitar o entendimento posterior, especialmente em blocos condicionais aninhados.

## Conclusão

As **estruturas condicionais** são o primeiro passo rumo à construção de algoritmos dinâmicos, que respondem a diferentes entradas e contextos. Aprendemos neste capítulo como utilizar o `Se-Então`, o `Se-Então-Senão`, estruturas aninhadas e a poderosa `Escolha-Caso`, todas fundamentais para controlar o fluxo de execução de um programa de forma lógica e estruturada.

O domínio dessas ferramentas permite que o programador crie sistemas capazes de reagir a diferentes situações, personalizar respostas, validar dados e implementar regras de negócio com clareza e eficiência. No próximo capítulo, aprenderemos como **repetir ações de forma controlada**, por meio das **estruturas de repetição**.

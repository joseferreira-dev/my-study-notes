# Capítulo 6 – Modularização com Procedimentos e Funções

À medida que nossos programas crescem em complexidade, torna-se cada vez mais necessário organizá-los de maneira estruturada e reutilizável. Em vez de escrever longos trechos de código em um único bloco principal, dividimos o programa em **partes menores, independentes e reutilizáveis**, chamadas de **subalgoritmos** ou **subprogramas**.

Neste capítulo, estudaremos em profundidade os conceitos de **procedimentos e funções**, abordando sua importância na construção de algoritmos mais legíveis, organizados e fáceis de manter. Também discutiremos os mecanismos de passagem de dados entre o programa principal e os subprogramas, incluindo o retorno de valores, além de apresentar o conceito de funções pré-definidas.

## Subalgoritmos: Uma Visão Geral

A construção de algoritmos, assim como a construção de qualquer sistema complexo, exige organização, clareza e, acima de tudo, **modularização**. Modularizar significa **dividir um problema maior em partes menores**, com o objetivo de torná-lo mais compreensível, reutilizável e fácil de manter. Essa prática, fundamental tanto na lógica de programação quanto na engenharia de software, se materializa na forma de **subalgoritmos**.

Um **subalgoritmo** é um conjunto de instruções agrupadas de maneira lógica e coesa para realizar uma tarefa específica dentro de um algoritmo maior. Ele pode ser visto como um **bloco funcional** separado que pode ser chamado sempre que necessário, seja uma ou várias vezes, com ou sem parâmetros de entrada.

Essa abordagem se inspira em uma estratégia comum no mundo real: ao invés de tentar resolver um grande problema de uma só vez, quebramos ele em partes menores, tratamos cada parte separadamente e depois reunimos os resultados. Isso é conhecido como **princípio da decomposição funcional**. Em programação, isso é implementado por meio dos subalgoritmos.

O termo **subalgoritmo** é genérico, e conforme o paradigma de programação e a linguagem utilizada, podem ser utilizados outros nomes. Veja a seguir algumas nomenclaturas equivalentes ou relacionadas:

- **Subprograma**: Nome comum em linguagens estruturadas, enfatiza o fato de que se trata de um programa dentro de outro.
- **Rotina / Sub-rotina**: Termos amplamente utilizados nas primeiras linguagens de programação estruturada, como Pascal e BASIC.
- **Procedimento (Procedure)**: Subalgoritmo que **executa uma tarefa, mas não retorna valor**. Muito utilizado quando queremos modularizar o código, mas não necessitamos de um resultado final.
- **Função (Function)**: Subalgoritmo que **executa uma tarefa e retorna um valor**. É amplamente usado quando um resultado é necessário para prosseguir com o fluxo lógico do programa.
- **Co-rotina (Coroutine)**: Embora tecnicamente mais avançada e pertencente a outro nível de abstração (relacionado à programação concorrente e assíncrona), o termo merece menção como uma extensão do conceito de subalgoritmo com **ponto de suspensão e retomada**, geralmente utilizado em fluxos paralelos.

Independentemente do nome, todos compartilham a mesma motivação: **encapsular lógica** para facilitar o raciocínio, a manutenção e a reutilização.

Subalgoritmos permitem que um programa seja organizado **em camadas de abstração**. Em vez de ver cada pequeno detalhe da execução, o programador passa a lidar com **nomes descritivos de tarefas**, como `calcularMedia()`, `exibirMenu()`, `validarEntrada()`, entre outros. Isso torna o programa:

- **Mais legível**: Porque foca no “o que fazer” em vez do “como fazer”.
- **Mais reutilizável**: Porque tarefas comuns podem ser utilizadas em diferentes partes do código.
- **Mais fácil de testar**: Porque cada subalgoritmo pode ser testado individualmente.
- **Mais fácil de manter**: Porque alterações em um subalgoritmo, se corretamente isolado, não afetam outras partes do código.

## Funções e Procedimentos: Semelhanças e Diferenças

No contexto da lógica de programação, distinguimos dois tipos principais de subalgoritmos:

- **Procedimentos**, que **executam tarefas** sem necessariamente retornar um valor ao ponto de chamada.
- **Funções**, que **executam uma tarefa e retornam um valor específico** ao final de sua execução.

Ambos podem receber **parâmetros de entrada**, também chamados de **argumentos**, e são escritos de forma similar. A diferença fundamental está no uso: funções retornam um resultado, procedimentos não. Todavia, é comum encontrar na literatura esses termos sendo usados de forma intercambiável.

## Definindo e Utilizando Procedimentos

Um **procedimento** é um bloco de código que realiza uma ação. Ele pode receber parâmetros e modificar variáveis, mas **não retorna um valor diretamente** ao ponto de chamada.

Como primeiro exemplo, o seguinte procedimento tem como objetivo imprimir uma linha decorativa:

```plaintext
Procedimento linha()
Início
    Escreva "==============================="
FimProcedimento
```

Esse procedimento pode ser chamado sempre que se desejar imprimir uma linha separadora, sem precisar repetir o código.

Já o seguinte procedimento recebe um nome e exibe uma saudação:

```plaintext
Procedimento saudar(nome como caractere)
Início
    Escreva "Olá, ", nome, "!"
FimProcedimento
```

Uso no programa principal:

```plaintext
Início
    Declare nomeUsuario como caractere
    Escreva "Digite seu nome: "
    Leia nomeUsuario
    Chame saudar(nomeUsuario)
Fim
```

## Definindo e Utilizando Funções

As **funções** são similares aos procedimentos, mas **sempre retornam um valor**. Em lógica de programação, utilizamos funções quando queremos **obter um resultado que será utilizado em algum cálculo, teste ou outra operação**.

A estrutura geral de uma função é:

```plaintext
Função nome(parâmetros) : tipo
    <declarações locais>
Início
    <instruções>
    Retorne valor
FimFunção
```

Como exemplo, temos a seguinte função que calcula o quadrado de um número:

```plaintext
Função quadrado(n como inteiro) : inteiro
Início
    Retorne n * n
FimFunção
```

Uso no programa principal:

```plaintext
Início
    Declare resultado como inteiro
    resultado <- quadrado(5)
    Escreva "O quadrado de 5 é: ", resultado
Fim
```

## Funções Pré-definidas

Muitas linguagens e ambientes de programação já disponibilizam **funções prontas**, chamadas de **funções pré-definidas**, que realizam tarefas comuns como:

- **Matemáticas**: raiz quadrada (`raiz`), potência (`pot`), valor absoluto (`abs`), seno (`sen`), cosseno (`cos`), etc.
- **Texto**: converter para maiúsculas, contar caracteres, extrair partes de uma cadeia, etc.
- **Entrada e saída**: conversão de tipos, leitura de dados formatados, etc.

Por exemplo:

```plaintext
Início
    Declare x como real
    Escreva "Digite um número: "
    Leia x
    Escreva "Raiz quadrada de ", x, " é: ", raiz(x)
Fim
```

O uso dessas funções economiza tempo de desenvolvimento e evita erros em cálculos ou operações já bem estabelecidas.

## Parâmetros: Passagem por Valor e por Referência

Os **parâmetros** são os dados que as funções e procedimentos recebem ao serem chamados. Existem duas maneiras principais de passar esses valores: passagem por valor e por referência.

### Passagem por valor

Na **passagem por valor**, o subalgoritmo **recebe uma cópia do valor**. Alterações feitas dentro da função **não afetam a variável original**.

É ideal para valores que não devem ser modificados.

```plaintext
Procedimento mostrarDobro(n como inteiro)
Início
    n <- n * 2
    Escreva "Dobro: ", n
FimProcedimento
```

Mesmo alterando `n` dentro do procedimento, a variável passada fora dele não é modificada.

### Passagem por referência

Na **passagem por referência**, o subalgoritmo **recebe o endereço da variável original**, ou seja, qualquer alteração feita afeta diretamente o conteúdo da variável fora do subalgoritmo.

```plaintext
Procedimento dobrar(Ref n como inteiro)
Início
    n <- n * 2
FimProcedimento
```

Uso:

```plaintext
Início
    Declare valor como inteiro
    valor <- 10
    Chame dobrar(valor)
    Escreva "Valor dobrado: ", valor  // Saída será 20
Fim
```

Essa abordagem é útil quando queremos que o subalgoritmo **modifique valores de variáveis declaradas no programa principal**.

## Retorno de Funções

Toda função precisa **retornar um único valor**, que é o seu resultado principal. Esse retorno deve ser compatível com o **tipo de dado declarado na função**.

O comando `Retorne` é responsável por enviar esse valor ao ponto onde a função foi chamada.

Por exemplo, podemos criar uma função que verifica se um dado número é par:

```plaintext
Função ehPar(n como inteiro) : lógico
Início
    Se n mod 2 = 0 Então
        Retorne verdadeiro
    Senão
        Retorne falso
    FimSe
FimFunção
```

Uso:

```plaintext
Início
    Declare x como inteiro
    Escreva "Digite um número: "
    Leia x

    Se ehPar(x) Então
        Escreva "O número é par."
    Senão
        Escreva "O número é ímpar."
    FimSe
Fim
```

Neste exemplo, a função `ehPar` realiza um teste e retorna um valor lógico, que é utilizado diretamente em uma estrutura condicional.

## Considerações Finais

Procedimentos e funções são pilares fundamentais da boa prática em programação. Eles promovem organização, legibilidade, modularidade e reuso, ao mesmo tempo em que facilitam a manutenção e a evolução dos programas.

Neste capítulo, aprendemos:

- A distinção entre procedimento (sem retorno) e função (com retorno);
- O uso de parâmetros para comunicação entre os blocos de código;
- A diferença entre passagem por valor e por referência;
- O uso de funções pré-definidas para tarefas comuns;
- A forma correta de retornar valores em funções.

No próximo capítulo, exploraremos **estruturas de dados compostas e registros**, ampliando ainda mais nossa capacidade de representar problemas do mundo real em nossos algoritmos.

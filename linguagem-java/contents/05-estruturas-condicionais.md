# Capítulo 5 – Estruturas de Controle Condicional

Nos capítulos anteriores, construímos uma base sólida sobre os elementos que compõem um programa Java: os tipos de dados, as variáveis que os armazenam, os operadores que os manipulam e a classe `String` para lidar com texto. Até agora, nossos programas seguem um fluxo de execução linear e previsível, onde cada instrução é executada em sequência, uma após a outra.

No entanto, a verdadeira força de um programa reside em sua capacidade de analisar informações e tomar decisões, alterando seu comportamento com base nas condições encontradas. É aqui que entram as **estruturas de controle de fluxo**. Elas são os mecanismos que nos permitem quebrar a linearidade e criar programas inteligentes que podem seguir diferentes caminhos de execução.

Neste capítulo, iniciaremos nossa exploração com as **estruturas de controle condicional**, as ferramentas que permitem que um bloco de código seja executado ou ignorado com base no resultado de uma expressão booleana. Em Java, dispomos de duas estruturas principais para essa finalidade: `if-else` e `switch`.

## Estrutura `if-else`

A estrutura condicional `if-else` é a forma mais fundamental e versátil de se implementar lógica de decisão em Java. Ela permite que a aplicação avalie uma condição e execute um bloco de código específico somente se essa condição for verdadeira.

### Forma Mais Simples: `if`

Na sua forma mais básica, a estrutura `if` testa uma única condição. Se a condição for `true`, o bloco de código associado a ela é executado. Se for `false`, o bloco é simplesmente ignorado e a execução do programa continua a partir da próxima instrução após o bloco `if`.

A sintaxe é:

```java
if (condicao) {
    // Este bloco de código é executado se a 'condicao' for verdadeira.
}
```

A `condicao` dentro dos parênteses deve ser, obrigatoriamente, uma expressão que resulte em um valor booleano (`true` ou `false`).

```java
public class ExemploIf {
    public static void main(String[] args) {
        double saldoEmConta = 500.00;
        double valorDoSaque = 100.00;

        // A condição verifica se o saldo é suficiente.
        if (saldoEmConta >= valorDoSaque) {
            saldoEmConta = saldoEmConta - valorDoSaque;
            System.out.println("Saque realizado com sucesso.");
            System.out.println("Novo saldo: " + saldoEmConta);
        }

        System.out.println("Fim da operação.");
    }
}
```

### Caminho Alternativo com `else`

Frequentemente, não queremos apenas executar uma ação se uma condição for verdadeira, mas também executar uma ação alternativa caso ela seja falsa. A cláusula `else` nos fornece esse caminho alternativo.

A sintaxe completa é:

```java
if (condicao) {
    // Bloco executado se a condição for verdadeira.
} else {
    // Bloco executado se a condição for falsa.
}
```

Vamos expandir o exemplo anterior para tratar o caso de saldo insuficiente:

```java
int idade = 16;

if (idade >= 18) {
    System.out.println("Acesso permitido. Maior de idade.");
} else {
    System.out.println("Acesso negado. Menor de idade.");
}
```

### Decisões em Cadeia com `else if`

Para cenários onde existem mais de duas possibilidades, podemos criar uma cadeia de decisões utilizando a cláusula `else if`. Isso nos permite testar múltiplas condições em sequência.

O fluxo de execução de uma cadeia `if-else if-else` é estritamente sequencial:

1. A primeira condição (`if`) é avaliada. Se for `true`, seu bloco é executado e **toda a estrutura restante é ignorada**.
2. Se a primeira condição for `false`, a próxima condição (`else if`) é avaliada. Se for `true`, seu bloco é executado e o resto da estrutura é ignorado.
3. O processo se repete para cada `else if` na cadeia.
4. Se nenhuma das condições anteriores for `true`, o bloco final `else` (se existir) é executado como o caminho padrão.

```java
int nota = 75;
String conceito;

if (nota >= 90) {
    conceito = "A";
} else if (nota >= 80) {
    conceito = "B";
} else if (nota >= 70) {
    conceito = "C";
} else if (nota >= 60) {
    conceito = "D";
} else {
    conceito = "F"; // Reprovado
}

System.out.println("O conceito do aluno é: " + conceito); // Saída: O conceito do aluno é: C
```

Neste exemplo, como `nota >= 90` é falso e `nota >= 80` também é falso, a condição `nota >= 70` é avaliada. Como ela é verdadeira, a variável `conceito` recebe `"C"` e a execução da estrutura condicional termina ali.

### Uma Nota sobre as Chaves `{ }`

Em Java, se o bloco de código dentro de um `if`, `else if` ou `else` contiver apenas **uma única instrução**, as chaves `{ }` são tecnicamente opcionais.

```
if (idade >= 18)
    System.out.println("Maior de idade.");
else
    System.out.println("Menor de idade.");
```

No entanto, é uma **prática de codificação amplamente recomendada e considerada mais segura sempre utilizar as chaves**, mesmo para blocos de uma única linha. Omiti-las pode levar a erros difíceis de depurar, especialmente se, no futuro, outra linha de código for adicionada ao bloco sem que as chaves sejam inseridas.

### Condicionais Aninhadas

É possível colocar uma estrutura `if-else` dentro de outra, criando uma lógica de decisão aninhada. Isso é útil para verificar um conjunto de condições secundárias após uma condição principal ter sido satisfeita.

```java
boolean usuarioLogado = true;
boolean ehAdministrador = false;

if (usuarioLogado) {
    System.out.println("Bem-vindo ao sistema!");

    if (ehAdministrador) {
        System.out.println("Acesso ao painel de administração concedido.");
    } else {
        System.out.println("Acesso limitado à área de usuário padrão.");
    }

} else {
    System.out.println("Por favor, faça o login para continuar.");
}
```

Embora úteis, condicionais aninhadas em excesso podem tornar o código complexo e difícil de ler. Muitas vezes, uma lógica aninhada pode ser simplificada pelo uso de operadores lógicos (`&&`, `||`), como vimos anteriormente.

## Estrutura `switch`

Enquanto a estrutura `if-else if-else` é extremamente flexível e pode lidar com qualquer tipo de condição booleana, existem cenários em que ela pode se tornar verbosa e repetitiva. Isso acontece principalmente quando precisamos comparar o valor de uma única variável contra uma série de valores literais e constantes.

Para esses casos, o Java fornece uma estrutura de controle alternativa e mais expressiva: o `switch`. Ele otimiza a legibilidade do código ao centralizar a variável a ser testada e listar os possíveis casos de forma clara e organizada.

### Estrutura `switch` Tradicional (Pré-Java 14)

A sintaxe clássica do `switch`, presente na linguagem desde suas primeiras versões, funciona como um painel de distribuição. A expressão dentro dos parênteses do `switch` é avaliada uma única vez, e seu resultado é comparado com o valor de cada `case`.

A sintaxe base é:

```java
switch (expressao) {
    case valor1:
        // Bloco de código para valor1
        break;
    case valor2:
        // Bloco de código para valor2
        break;
    // ... outros casos
    default:
        // Bloco de código para qualquer outro valor
}
```

Analisando cada componente:

- **`expressao`**: A variável ou expressão cujo valor será testado. Os tipos permitidos na sintaxe tradicional incluem `char`, `byte`, `short`, `int`, `String` (a partir do Java 7), `enum` e as classes Wrapper correspondentes (`Character`, `Integer`, etc.). Notavelmente, tipos como `long`, `float`, `double` e `boolean` não são permitidos.
- **`case valor`**: Cada `case` representa um possível resultado para a `expressao`. O `valor` aqui deve ser uma constante compatível com o tipo da expressão (um literal ou uma variável `final`).
- **`break`**: Esta instrução é crucial. Ela interrompe a execução dentro do `switch` e transfere o controle para a primeira instrução após o seu término.
- **`default`**: Este bloco é opcional e funciona de maneira análoga ao `else` final. Ele é executado se o valor da `expressao` não corresponder a nenhum dos `case` especificados. É uma boa prática sempre incluí-lo para tratar casos inesperados.

#### Importância do `break` e o "Fall-Through"

Um dos comportamentos mais importantes (e perigosos) do `switch` tradicional é o **fall-through** (queda). Se uma instrução `break` for omitida de um `case`, a execução não irá parar; ela "cairá" e continuará executando o código do(s) próximo(s) `case`(s) até encontrar um `break` ou o final do `switch`.

Embora isso possa ser usado intencionalmente para agrupar casos que compartilham o mesmo código, na maioria das vezes, a omissão do `break` é um erro que leva a bugs difíceis de rastrear.

Veja o exemplo abaixo:

```java
int diaDaSemana = 2; // Terça-feira

switch (diaDaSemana) {
    case 1:
        System.out.println("Domingo");
        // sem break!
    case 2:
        System.out.println("Segunda");
        // sem break!
    case 3:
        System.out.println("Terça");
        break; // A execução para aqui.
    case 4:
        System.out.println("Quarta");
        break;
}
```

A saída deste código seria:

```
Segunda
Terça
```

Como o `case 2` não tem um `break`, a execução "caiu" e executou também o código do `case 3`.

### Evolução do `switch` (A Partir do Java 14)

Reconhecendo que a sintaxe tradicional, especialmente a necessidade do `break` e o comportamento de fall-through, era uma fonte comum de erros, o Java introduziu uma sintaxe modernizada e mais poderosa para o `switch`, que se tornou um recurso padrão a partir do Java 17.

As principais melhorias são:

1. **Sintaxe de Seta (`->`)**: Elimina a necessidade do `break` e o risco de fall-through.
2. **Agrupamento de Casos**: Permite agrupar múltiplos valores para o mesmo bloco de código de forma mais limpa.
3. **Expressões `switch`**: O `switch` pode agora ser usado como uma expressão que retorna um valor, permitindo atribuições diretas a variáveis.

#### Sintaxe de Seta e Agrupamento de Casos

Com a nova sintaxe, os dois pontos (`:`) e o `break` são substituídos por uma seta (`->`). O código à direita da seta é o que será executado, e o `switch` é automaticamente finalizado, sem fall-through.

Além disso, múltiplos valores de `case` podem ser agrupados em uma única linha, separados por vírgulas.

```java
int mes = 4; // Abril
String estacao;

switch (mes) {
    case 12, 1, 2:
        estacao = "Verão";
        break;
    case 3, 4, 5:
        estacao = "Outono";
        break;
    case 6, 7, 8:
        estacao = "Inverno";
        break;
    case 9, 10, 11:
        estacao = "Primavera";
        break;
    default:
        estacao = "Mês inválido";
}
```

O mesmo código, usando a sintaxe moderna, fica muito mais conciso e seguro:

```java
switch (mes) {
    case 12, 1, 2 -> estacao = "Verão";
    case 3, 4, 5 -> estacao = "Outono";
    case 6, 7, 8 -> estacao = "Inverno";
    case 9, 10, 11 -> estacao = "Primavera";
    default -> estacao = "Mês inválido";
}
```

#### `switch` como uma Expressão

A mudança mais significativa é a capacidade de o `switch` retornar um valor. Isso permite que ele seja usado do lado direito de uma atribuição, tornando o código ainda mais funcional e expressivo.

Quando um `switch` é usado como uma expressão, ele **deve ser exaustivo**, ou seja, todos os possíveis valores da variável de entrada devem ser tratados. Isso geralmente exige a presença de um `case default`.

```java
int dia = 2;

String nomeDoDia = switch (dia) {
    case 1 -> "Domingo";
    case 2 -> "Segunda-feira";
    case 3 -> "Terça-feira";
    case 4 -> "Quarta-feira";
    case 5 -> "Quinta-feira";
    case 6 -> "Sexta-feira";
    case 7 -> "Sábado";
    default -> "Dia desconhecido";
};

System.out.println("O dia correspondente é: " + nomeDoDia);
```

Neste exemplo, o valor resultante de todo o bloco `switch` é diretamente atribuído à variável `nomeDoDia`. Esta sintaxe não é apenas mais curta, mas também mais segura, pois elimina a possibilidade de se esquecer de atribuir um valor em um dos `case`s.

## Considerações Finais

Neste capítulo, demos um passo fundamental para além da execução linear de código. Exploramos as **estruturas de controle condicional**, os mecanismos que conferem inteligência e capacidade de decisão aos nossos programas. Ao dominar o `if-else` e o `switch`, ganhamos a habilidade de criar aplicações que respondem dinamicamente a diferentes entradas e estados, seguindo caminhos lógicos distintos com base em condições predefinidas.

Analisamos a estrutura `if-else if-else` como a ferramenta de decisão mais versátil do Java. Vimos como ela nos permite construir desde escolhas binárias simples até cadeias de verificação complexas e sequenciais, capazes de lidar com qualquer expressão que resulte em um valor booleano. Ela é o pilar fundamental da lógica condicional, aplicável em virtualmente qualquer cenário de tomada de decisão.

Em seguida, desvendamos a estrutura `switch` como uma alternativa mais limpa e expressiva para um tipo específico de problema: a comparação de uma única variável contra múltiplos valores constantes. Discutimos a sintaxe tradicional, ressaltando a importância crucial da instrução `break` para evitar o comportamento de _fall-through_, uma fonte comum de erros. Mais importante, exploramos a evolução do `switch` nas versões modernas do Java, que, com a sintaxe de seta (`->`) e a capacidade de funcionar como uma expressão, tornou-se uma ferramenta não apenas mais concisa, mas também significativamente mais segura e poderosa.

Com a capacidade de guiar o fluxo de execução por diferentes ramos, nossos programas deixaram de ser apenas uma sequência de tarefas e passaram a ter uma lógica reativa. No entanto, ainda falta uma peça-chave para o controle completo do fluxo: a capacidade de repetir ações.

No próximo capítulo, mergulharemos nas **estruturas de repetição**, também conhecidas como laços ou _loops_. Aprenderemos a usar os comandos `for`, `while` e `do-while` para executar blocos de código repetidamente, permitindo-nos processar coleções de dados, executar simulações e automatizar tarefas de forma eficiente. Com a combinação de decisões e repetições, estaremos aptos a construir algoritmos e aplicações de complexidade real.
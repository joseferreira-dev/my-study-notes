# Capítulo 3 – Operadores: Manipulando Dados e Construindo Expressões

Nos capítulos anteriores, estabelecemos os fundamentos sobre como declarar e armazenar dados em Java utilizando tipos e variáveis. Agora que conhecemos os "substantivos" da linguagem, é hora de introduzir os "verbos": os **operadores**. Operadores são símbolos especiais que instruem o compilador a realizar operações específicas — aritméticas, de comparação, lógicas, entre outras — sobre um ou mais valores ou variáveis (chamados de operandos).

Dominar os operadores é essencial, pois são eles que nos permitem transformar dados estáticos em lógica dinâmica, realizando cálculos, tomando decisões e controlando o fluxo de uma aplicação. Neste capítulo, exploraremos as principais categorias de operadores em Java, detalhando seu funcionamento e ilustrando seu uso com exemplos práticos.

## Operadores Aritméticos

Como o nome sugere, os operadores aritméticos são utilizados para executar as operações matemáticas fundamentais.

|Operador|Nome|Descrição|Exemplo|
|---|---|---|---|
|`+`|Adição|Soma dois valores ou concatena Strings.|`x + y`|
|`-`|Subtração|Subtrai um valor de outro.|`x - y`|
|`*`|Multiplicação|Multiplica dois valores.|`x * y`|
|`/`|Divisão|Divide um valor por outro.|`x / y`|
|`%`|Módulo|Retorna o resto de uma divisão inteira.|`x % y`|
|`++`|Incremento|Incrementa o valor da variável em 1.|`++x` ou `x++`|
|`--`|Decremento|Diminui o valor da variável em 1.|`--x` ou `x--`|


```java
public class ExemploAritmetico {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;

        int soma = a + b;           // 13
        int subtracao = a - b;      // 7
        int multiplicacao = a * b;  // 30
        int divisao = a / b;        // 3 (divisão inteira)
        int modulo = a % b;         // 1 (resto da divisão 10 por 3)

        System.out.println("Soma: " + soma);
        System.out.println("Subtração: " + subtracao);
        System.out.println("Multiplicação: " + multiplicacao);
        System.out.println("Divisão: " + divisao);
        System.out.println("Módulo: " + modulo);
    }
}
```

Observações importantes sobre operadores aritméticos:

- **Divisão Inteira**: Um ponto que merece atenção especial é a divisão (`/`). Quando ambos os operandos são tipos inteiros (`int`, `long`, etc.), o Java realiza uma **divisão inteira**, descartando qualquer parte fracionária. No exemplo acima, `10 / 3` resulta em `3`, e não `3.33`. Para obter um resultado de ponto flutuante, pelo menos um dos operandos deve ser de um tipo de ponto flutuante (`double` ou `float`).
    
    ```java
    double resultadoPreciso = 10.0 / 3; // resultadoPreciso será 3.333...
    ```
    
- **Concatenação com `+`**: O operador de adição (`+`) é "sobrecarregado". Quando utilizado com pelo menos uma `String`, ele deixa de realizar uma soma matemática e passa a executar uma **concatenação**, unindo os valores em uma única String.
    
    ```java
    String saudacao = "Olá, ";
    String nome = "Maria";
    String mensagem = saudacao + nome; // mensagem será "Olá, Maria"
    
    int idade = 30;
    String info = "Idade: " + idade; // info será "Idade: 30"
    ```
    
- **Incremento (`++`) e Decremento (`--`)**: Estes operadores podem ser usados de duas formas: pré-fixada (`++variavel`) e pós-fixada (`variavel++`). A diferença está no momento em que o valor é alterado em relação à avaliação da expressão.
    - **Pré-fixado**: A variável é incrementada/decrementada **antes** de seu valor ser utilizado na expressão.
    - **Pós-fixado**: O valor original da variável é utilizado na expressão e ela é incrementada/decrementada **depois**.
    
    ```java
    int x = 5;
    int y = ++x; // Pré: x se torna 6, e ENTÃO y recebe o valor de x.
                 // Resultado: y=6, x=6.
    
    int a = 5;
    int b = a++; // Pós: b recebe o valor original de a, e ENTÃO a se torna 6.
                 // Resultado: b=5, a=6.
    ```

## Operadores de Atribuição

Os operadores de atribuição são utilizados para designar um valor a uma variável. O operador básico é o de igual (`=`), mas existem operadores compostos que combinam uma operação aritmética com uma atribuição, tornando o código mais conciso.

|Operador|Exemplo|Equivalência|
|---|---|---|
|`=`|`x = 5;`|`x = 5;`|
|`+=`|`x += 3;`|`x = x + 3;`|
|`-=`|`x -= 3;`|`x = x - 3;`|
|`*=`|`x *= 3;`|`x = x * 3;`|
|`/=`|`x /= 3;`|`x = x / 3;`|
|`%=`|`x %= 3;`|`x = x % 3;`|

```java
int saldo = 100;
saldo += 50; // Equivalente a saldo = saldo + 50;  (saldo agora é 150)
saldo -= 25; // Equivalente a saldo = saldo - 25;  (saldo agora é 125)
```

## Operadores de Comparação (ou Relacionais)

Os operadores de comparação são usados para comparar dois valores. O resultado de uma operação de comparação é sempre um valor booleano: `true` ou `false`. Eles são a espinha dorsal das estruturas de controle de fluxo, como os comandos `if` e `while`.

|Operador|Nome|Exemplo (resulta em `true`)|
|---|---|---|
|`==`|Igual a|`5 == 5`|
|`!=`|Diferente de|`5 != 4`|
|`>`|Maior que|`5 > 4`|
|`<`|Menor que|`4 < 5`|
|`>=`|Maior ou igual a|`5 >= 5`|
|`<=`|Menor ou igual a|`4 <= 5`|

```java
int idade = 25;
double altura = 1.75;

boolean podeDirigir = idade >= 18;  // true
boolean ehAlto = altura > 1.80;     // false
```

#### Uma Observação Crucial: `==` vs. `.equals()`

Como mencionado anteriormente, é fundamental reforçar a distinção de como o operador `==` funciona com tipos primitivos e tipos por referência.

- Para **tipos primitivos**, `==` compara os **valores** diretamente. `10 == 10` é `true`.
- Para **tipos por referência (objetos)**, `==` compara os **endereços de memória**. Ele verifica se as duas variáveis apontam para o mesmo objeto, e não se os objetos têm conteúdo igual. Para comparar o conteúdo de objetos, como Strings, deve-se usar o método `.equals()`.

```java
String s1 = "Java";
String s2 = "Java";
String s3 = new String("Java");

System.out.println(s1 == s2); // Pode ser true (devido à otimização do "pool de Strings")
System.out.println(s1 == s3); // false (s3 é um objeto explicitamente novo na memória)

System.out.println(s1.equals(s3)); // true (o conteúdo é o mesmo)
```

## Operadores Lógicos

Os operadores lógicos são usados para combinar duas ou mais expressões booleanas em uma única expressão booleana.

|Operador|Nome|Descrição|
|---|---|---|
|`&&`|E lógico (AND)|Retorna `true` se ambas as expressões forem `true`.|
|`\|`|OU lógico (OR)|Retorna `true` se pelo menos uma das expressões for `true`.|
|`!`|NÃO lógico (NOT)|Inverte o valor de uma expressão booleana (de `true` para `false` e vice-versa).|

```java
boolean temCarteiraDeMotorista = true;
boolean temCarro = false;
int idade = 20;

boolean podeViajarSozinho = idade >= 18 && temCarteiraDeMotorista; // true, pois ambas são true.
boolean podeUsarCarro = temCarro || temCarteiraDeMotorista;       // true, pois uma delas é true.
boolean ehMenorDeIdade = !(idade >= 18);                           // false, pois !true é false.
```

#### Avaliação de Curto-Circuito (Short-Circuit)

Os operadores `&&` e `||` em Java são "preguiçosos", eles realizam uma avaliação de curto-circuito.

- No caso de `&&` (E), se a primeira expressão for `false`, o resultado geral já será `false`, independentemente da segunda. Portanto, o Java **não avalia** a segunda expressão.
- No caso de `||` (OU), se a primeira expressão for `true`, o resultado geral já será `true`. Portanto, o Java **não avalia** a segunda expressão.

Isso não é apenas uma otimização de performance, mas também pode ser usado para evitar erros:

```java
String nome = null;
// A linha abaixo NÃO lança um erro, pois a primeira parte (nome != null) é false.
// A segunda parte (nome.length() > 0) nunca é executada.
if (nome != null && nome.length() > 0) {
    System.out.println("Nome válido");
}
```

## Considerações Finais

Neste capítulo, demos vida aos dados e variáveis que estudamos anteriormente. Exploramos os **operadores**, os "verbos" da linguagem Java que nos permitem manipular, modificar, comparar e combinar valores, formando a espinha dorsal de toda a lógica de programação.

Iniciamos com os **operadores aritméticos**, as ferramentas fundamentais para realizar cálculos. Vimos que, além das operações básicas, existem nuances importantes, como o comportamento da divisão inteira, a sobrecarga do operador `+` para a concatenação de Strings e a sutil, mas crucial, diferença entre os operadores de incremento e decremento pré-fixados e pós-fixados. Em seguida, vimos como os **operadores de atribuição** nos fornecem uma sintaxe mais limpa e concisa para atualizar o valor de nossas variáveis.

O ponto central do capítulo foi a introdução aos operadores que permitem que um programa tome decisões. Os **operadores de comparação**, ou relacionais, são a base para se fazer perguntas ao nosso código, com cada comparação resultando em uma resposta booleana definitiva: `true` ou `false`. Reforçamos a distinção vital entre usar `==` para comparar valores primitivos e referências de memória, e o uso do método `.equals()` para comparar o conteúdo real de objetos. Por fim, os **operadores lógicos** nos forneceram a capacidade de construir condições complexas, combinando múltiplas expressões booleanas e utilizando a eficiência da avaliação de curto-circuito.

Com a habilidade de manipular dados e, mais importante, de fazer perguntas sobre eles, chegamos a um ponto fundamental. Agora que nosso programa pode "pensar" e chegar a conclusões (`true` ou `false`), o próximo passo é ensiná-lo a **agir** de acordo com essas conclusões.

No próximo capítulo, entraremos no mundo das **estruturas de controle de fluxo**. Aprenderemos a usar os resultados de nossas expressões para guiar o caminho que o programa segue, utilizando comandos como `if`, `else` e `switch` para executar blocos de código condicionalmente, e estruturas de repetição como `for` e `while` para executar tarefas múltiplas vezes. Com isso, sairemos do domínio dos programas lineares e passaremos a construir aplicações verdadeiramente dinâmicas e inteligentes.
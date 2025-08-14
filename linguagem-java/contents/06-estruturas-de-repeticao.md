# Capítulo 6 – Estruturas de Repetição

Nos capítulos anteriores, adquirimos as ferramentas para manipular dados e para guiar a execução do nosso código por diferentes caminhos com as estruturas condicionais. Combinados, esses recursos nos permitem criar programas que respondem a cenários específicos. No entanto, o verdadeiro poder da computação se manifesta em sua capacidade de executar tarefas repetitivas de forma rápida e incansável.

É aqui que entram as **estruturas de repetição**, também conhecidas como **laços** ou _loops_. Elas são construções da linguagem que nos permitem executar um mesmo bloco de código múltiplas vezes, seja por um número predefinido de iterações ou enquanto uma determinada condição for satisfeita. Dominar os laços de repetição é o que nos permite processar coleções de dados, implementar algoritmos complexos e automatizar tarefas que seriam impossíveis de se realizar manualmente.

O Java nos oferece três estruturas de repetição principais, que se dividem em duas categorias conceituais:

1. **Laços pré-teste (`while` e `for`)**: A condição de continuação é verificada **antes** da execução de cada iteração.
2. **Laços pós-teste (`do-while`)**: A condição de continuação é verificada **após** a execução de cada iteração.

Vamos começar explorando os laços baseados em condição: `while` e `do-while`.

## Laço `while`

A estrutura `while` é a forma mais fundamental de laço de repetição em Java. Seu funcionamento é direto: ela executa um bloco de código repetidamente **enquanto** uma condição booleana especificada permanecer `true`.

A sintaxe é:

```java
while (condicao) {
    // Bloco de código a ser repetido
}
```

O `while` é um laço do tipo "pré-teste". Isso significa que a `condicao` é avaliada **antes** de cada possível iteração. Se, na primeira vez que a condição for verificada, ela já for `false`, o bloco de código dentro do laço **nunca será executado**. Por isso, dizemos que um laço `while` executa seu bloco de "zero ou mais vezes".

Para que um laço `while` funcione corretamente e não se torne um "laço infinito", ele geralmente depende de três componentes:

1. **Inicialização**: Uma variável de controle é inicializada antes do início do laço.
2. **Condição de Continuação**: A expressão booleana que é testada.
3. **Atualização**: Uma instrução dentro do laço que modifica a variável de controle, de modo que, em algum momento, a condição se torne `false` e o laço termine.

Veja o exemplo clássico de um contador:

```java
// 1. Inicialização da variável de controle
int contador = 1;

// 2. Condição de continuação do laço
while (contador <= 5) {
    System.out.println("Contando: " + contador);
    
    // 3. Atualização da variável de controle
    contador++; // Essencial para evitar um laço infinito!
}

System.out.println("Laço finalizado.");
```

Neste caso, a variável `contador` é incrementada a cada iteração, garantindo que, após a quinta passagem, a condição `contador <= 5` se torne falsa e o laço seja encerrado.

O `while` é especialmente útil em situações onde o número de iterações não é conhecido de antemão, como ao processar dados de uma fonte externa até que ela se esgote ou ao interagir com um usuário até que ele digite um comando específico para sair.

## Laço `do-while`

A estrutura `do-while` é uma variação do `while`. A diferença fundamental entre eles está no momento em que a condição é verificada. O `do-while` é um laço do tipo "pós-teste", o que significa que a condição de continuação é avaliada **após** a execução do bloco de código.

A sintaxe é:

```java
do {
    // Bloco de instruções
} while (condicao);
```

Essa característica garante que o bloco de código dentro do `do { }` será **executado pelo menos uma vez**, independentemente de a condição ser verdadeira ou falsa na primeira avaliação.

Vamos reescrever o exemplo do contador com `do-while`:

```java
int contador = 1;

do {
    System.out.println("Contando: " + contador);
    contador++;
} while (contador <= 5);

System.out.println("Laço finalizado.");
```

Neste caso específico, o resultado é o mesmo do `while`. A verdadeira utilidade do `do-while` se manifesta em cenários onde uma ação inicial precisa ser executada antes que a condição de repetição possa ser testada. Um exemplo clássico é a validação de entrada de dados do usuário: o programa precisa primeiro pedir a entrada para depois verificar se ela é válida.

```java
import java.util.Scanner;

public class ValidacaoDeEntrada {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int numero;

        do {
            System.out.print("Digite um número positivo entre 1 e 10: ");
            numero = scanner.nextInt();
            
            if (numero < 1 || numero > 10) {
                System.out.println("Número inválido. Tente novamente.");
            }
            
        } while (numero < 1 || numero > 10);

        System.out.println("Você digitou o número válido: " + numero);
        scanner.close();
    }
}
```

Neste exemplo, o bloco `do` é executado, solicitando um número ao usuário. Somente **após** a entrada do número é que a condição `while` verifica se ele está fora do intervalo desejado. Se estiver, o laço se repete, solicitando a entrada novamente. Isso continua até que um número válido seja inserido.


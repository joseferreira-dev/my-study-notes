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

## Laço `for`

Enquanto o laço `while` é ideal para repetições baseadas em uma condição que pode mudar de forma imprevisível, o Java oferece uma estrutura mais compacta e expressiva para cenários onde o número de iterações é, de alguma forma, conhecido ou controlado por um contador: o laço `for`.

### Laço `for` Tradicional: Repetição Contada

O laço `for` clássico é a ferramenta perfeita para quando se sabe exatamente onde a repetição começa, qual a condição para ela continuar e como progredir a cada passo. Ele elegantemente agrupa as três partes essenciais de um laço controlado (inicialização, condição e atualização) em uma única linha, tornando a lógica da repetição clara e autocontida.

A sintaxe é:

```java
for (inicialização; condição; atualização) {
    // Bloco de código a ser repetido
}
```

Vamos analisar a anatomia desta estrutura:

1. **Inicialização**: Esta parte é executada **uma única vez**, no exato momento em que o laço é iniciado. É aqui que, tipicamente, se declara e inicializa uma variável de controle (um contador), como `int i = 0`.
2. **Condição**: Assim como no `while`, esta expressão booleana é avaliada **antes** de cada iteração. Enquanto a condição for `true`, o bloco de código do laço é executado. Assim que ela se torna `false`, o laço é encerrado.
3. **Atualização**: Esta instrução é executada **ao final** de cada iteração, logo após a execução do bloco de código. É aqui que, geralmente, a variável de controle é incrementada (`i++`) ou decrementada (`i--`) para progredir em direção à condição de parada.

O exemplo mais comum é a iteração por uma sequência de números:

```java
// Este laço irá imprimir os números de 1 a 5.
for (int i = 1; i <= 5; i++) {
    System.out.println("O valor de i é: " + i);
}
```

O fluxo de execução deste `for` é o seguinte:

1. **Inicialização**: `int i = 1` é executado. A variável `i` agora vale 1.
2. **Condição**: `i <= 5` (1 <= 5) é avaliado. É `true`, então o bloco é executado.
3. **Bloco**: `System.out.println` é executado, imprimindo "O valor de i é: 1".
4. **Atualização**: `i++` é executado. A variável `i` agora vale 2.
5. **Condição**: `i <= 5` (2 <= 5) é avaliado. É `true`, então o bloco é executado novamente.
6. O processo se repete até que `i` se torne 6. Neste ponto, a condição `i <= 5` (6 <= 5) se torna `false`, e o laço é encerrado.

A estrutura `for` é bastante flexível, permitindo, por exemplo, contagens regressivas ou incrementos diferentes de 1:

```java
// Contagem regressiva
for (int i = 10; i >= 0; i--) {
    System.out.println("Contagem regressiva: " + i);
}

// Contando de 2 em 2
for (int i = 0; i <= 10; i += 2) {
    System.out.println("Número par: " + i);
}
```

### Laço `for-each` (Enhanced for): Iterando sobre Coleções

A partir do Java 5, a linguagem introduziu uma sintaxe simplificada e mais segura para o laço `for`, projetada especificamente para percorrer todos os elementos de um array ou de uma coleção (como `List` ou `Set`). Este laço é conhecido como **for-each** ou _enhanced for_.

Seu objetivo é eliminar a necessidade de se gerenciar manualmente um índice de contador, tornando o código mais limpo, mais legível e menos propenso a erros, como o `ArrayIndexOutOfBoundsException`.

A sintaxe é:

```java
for (Tipo elemento : colecaoOuArray) {
    // Bloco de código a ser executado com o 'elemento' atual
}
```

Interpretando a sintaxe:

- **`colecaoOuArray`**: O array ou coleção que se deseja percorrer.
- **`Tipo elemento`**: Uma declaração de uma variável temporária que, a cada iteração, receberá o valor de um elemento da coleção. O `Tipo` deve ser compatível com o tipo dos elementos armazenados na coleção.
- **`:`**: O símbolo de dois pontos pode ser lido como "em" ou "dentro de".

Vamos comparar a iteração sobre um array de forma tradicional e com o `for-each`:

```java
String[] nomes = {"Ana", "Carlos", "Beatriz"};

// Forma tradicional com índice
System.out.println("--- Usando o for tradicional ---");
for (int i = 0; i < nomes.length; i++) {
    String nomeAtual = nomes[i];
    System.out.println("Nome: " + nomeAtual);
}

// Forma moderna com for-each
System.out.println("--- Usando o for-each ---");
for (String nome : nomes) {
    System.out.println("Nome: " + nome);
}
```

O resultado é idêntico, mas a versão com `for-each` é visivelmente mais enxuta e direta ao ponto. Ela abstrai toda a complexidade do controle de índice.

#### Quando Usar o `for-each` (e Quando Não Usar)

O `for-each` é a escolha ideal na grande maioria dos casos em que se precisa apenas **acessar cada elemento de uma coleção ou array, do início ao fim**.

No entanto, existem situações em que o laço `for` tradicional ainda é necessário:

- **Quando o índice é importante**: Se, além do elemento, a lógica precisa saber a posição (índice) dele no array.
- **Quando é preciso modificar a coleção**: O `for-each` não deve ser usado para adicionar ou remover elementos da coleção que está sendo percorrida, pois isso pode levar a uma `ConcurrentModificationException`.
- **Quando é preciso percorrer de trás para frente**: O `for-each` sempre itera do primeiro ao último elemento. Para uma ordem inversa, o `for` tradicional é a solução.

## Considerações Finais

Neste capítulo, concluímos nossa exploração das estruturas de controle de fluxo ao dominar os **laços de repetição**. Se as estruturas condicionais do capítulo anterior nos deram o poder de fazer nossos programas tomarem decisões, os laços nos deram a capacidade de executar tarefas de forma eficiente e incansável, um pilar da automação e do processamento de dados.

Analisamos as diferentes ferramentas que o Java oferece para a repetição. Começamos com o laço `while`, a estrutura mais flexível, ideal para cenários onde a repetição depende de uma condição dinâmica e o número de iterações não é conhecido de antemão. Em contraste, vimos o `do-while` como sua variação "pós-teste", garantindo que o bloco de código seja executado ao menos uma vez, tornando-o perfeito para situações como validação de entrada de dados.

Em seguida, exploramos o laço `for` em suas duas formas. O `for` tradicional se revelou a estrutura mais clara e organizada para repetições contadas, onde a inicialização, a condição e a atualização do contador estão contidas em uma única e legível linha. Por fim, desvendamos o `for-each` como uma evolução moderna e segura, que simplifica enormemente a iteração sobre arrays e coleções, eliminando a necessidade de gerenciamento manual de índices e tornando o código mais limpo e menos propenso a erros.

Com o domínio sobre variáveis, operadores, condicionais e laços, o repertório de fundamentos da programação procedural em Java está completo. Agora é possível escrever algoritmos robustos, capazes de manipular dados e controlar a execução de tarefas complexas.
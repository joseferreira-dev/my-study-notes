# Capítulo 8 – Tratamento de Exceções

Até agora, construímos programas que seguem um fluxo de execução bem definido. No entanto, o mundo real é imprevisível. Programas robustos não são aqueles em que erros nunca acontecem, mas sim aqueles que sabem como **responder a esses erros** de forma controlada e elegante, sem travar ou apresentar um comportamento inesperado para o usuário.

É aqui que entra o mecanismo de **Tratamento de Exceções** (_Exception Handling_) do Java. Uma **exceção** é um evento, representado por um objeto, que ocorre durante a execução de um programa e que interrompe o fluxo normal de suas instruções. Essas situações anormais podem ser causadas por uma variedade de fatores: um erro de lógica do programador (como acessar um índice inválido de um array), uma falha em um recurso externo (como tentar ler um arquivo que não existe) ou uma entrada de dados inválida fornecida pelo usuário.

O sistema de exceções do Java não é um simples mecanismo de "relatório de erros"; é uma estrutura sofisticada e orientada a objetos que permite separar a lógica de tratamento de erros da lógica principal do programa, tornando o código mais limpo, legível e, acima de tudo, mais resiliente.

## Hierarquia de Erros do Java: `Throwable`, `Error` e `Exception`

Em Java, todo evento que pode ser "lançado" (_thrown_) e "capturado" (_caught_) é um objeto que herda, direta ou indiretamente, da classe `java.lang.Throwable`. Esta classe é a raiz de toda a hierarquia de erros e se divide em duas subcategorias principais: `Error` e `Exception`.

- **`Error`**: Representa erros graves e anormais, geralmente ligados a problemas na Máquina Virtual Java (JVM) ou no ambiente de execução, dos quais uma aplicação tipicamente **não consegue se recuperar**. Exemplos incluem `OutOfMemoryError` (quando a JVM fica sem memória) e `StackOverflowError` (causado por uma recursão infinita que estoura a pilha de chamadas). Em geral, não se deve tentar capturar ou tratar `Error`s.
- **`Exception`**: Representa condições anormais que uma aplicação **pode e deve tratar**. São erros esperados e tratáveis que podem ocorrer durante a execução normal do programa. É nesta categoria que focaremos nossos esforços.

A classe `Exception`, por sua vez, se ramifica em duas categorias distintas, cuja diferença é um dos conceitos mais importantes do tratamento de erros em Java.

### Exceções Verificadas (Checked Exceptions) vs. Não Verificadas (Unchecked Exceptions)

1. **Exceções Verificadas (Checked)**: São exceções que o compilador Java **força** o desenvolvedor a tratar. Elas geralmente representam problemas com recursos externos que estão fora do controle direto do programa, como falhas de rede (`SocketException`), erros de banco de dados (`SQLException`) ou problemas de manipulação de arquivos (`IOException`). Se um método pode lançar uma exceção verificada, ele deve, obrigatoriamente:
    - Tratar a exceção internamente usando um bloco `try-catch`.
    - Declarar que ele "propaga" a exceção para quem o chamou, usando a palavra-chave `throws` na sua assinatura.
2. **Exceções Não Verificadas (Unchecked)**: São exceções que herdam da classe `RuntimeException`. Elas geralmente representam **erros de lógica ou de programação**, como acessar um objeto nulo (`NullPointerException`) ou um índice de array inválido (`ArrayIndexOutOfBoundsException`). O compilador **não exige** que essas exceções sejam tratadas, pois, em teoria, elas deveriam ser prevenidas através de uma codificação correta (ex: verificar se um objeto é nulo antes de usá-lo).

## Tratamento com `try`, `catch` e `finally`

Java fornece um bloco estruturado para lidar com as exceções, permitindo isolar o código "perigoso" e definir as ações a serem tomadas caso um problema ocorra.

A sintaxe padrão é:

```java
try {
    // Bloco de código que pode lançar uma exceção.
} catch (TipoDaExcecao1 e1) {
    // Bloco para tratar a Exceção do Tipo 1.
} catch (TipoDaExcecao2 e2) {
    // Bloco para tratar a Exceção do Tipo 2.
} finally {
    // Bloco opcional que sempre será executado.
}
```

Analisando cada parte:

- **`try`**: O bloco `try` envolve o trecho de código onde se espera que uma ou mais exceções possam ocorrer. É a "zona de monitoramento".
- **`catch`**: Se uma exceção é lançada dentro do bloco `try`, a JVM procura por um bloco `catch` correspondente que possa tratar aquele tipo de exceção. É possível ter múltiplos blocos `catch` para tratar diferentes tipos de erro. A variável `e` (convenção para _exception_) é um objeto que contém informações sobre o erro ocorrido.
- **`finally`**: Este bloco é opcional, mas extremamente importante. O código dentro do `finally` é **garantido de ser executado**, independentemente do que aconteça no bloco `try-catch`:
    - Se o `try` for concluído com sucesso, o `finally` é executado.
    - Se uma exceção for capturada por um `catch`, o `finally` é executado após o `catch`.
    - Se uma exceção não for capturada, o `finally` é executado mesmo assim, antes de o programa travar.
    Por essa razão, o `finally` é o local ideal para colocar código de "limpeza", como fechar arquivos, conexões de banco de dados ou recursos de rede, garantindo que eles não fiquem abertos e consumindo memória.

```java
public class TratamentoExcecao {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        try {
            System.out.print("Digite um número: ");
            int numero1 = scanner.nextInt();

            System.out.print("Digite outro número: ");
            int numero2 = scanner.nextInt();

            int resultado = numero1 / numero2;
            System.out.println("O resultado da divisão é: " + resultado);

        } catch (ArithmeticException e) {
            System.err.println("Erro: Não é possível dividir um número por zero.");
        } catch (java.util.InputMismatchException e) {
            System.err.println("Erro: O valor digitado não é um número inteiro válido.");
        } finally {
            System.out.println("Bloco 'finally' executado. Finalizando a operação.");
            scanner.close(); // Liberando o recurso
        }
    }
}
```

## Declarando Exceções com `throw` e `throws`

Além de capturar exceções geradas pelo sistema, podemos também criar e declarar nossas próprias exceções.

### Lançando Exceções com `throw`

A palavra-chave `throw` é usada para **lançar explicitamente** um objeto de exceção. Isso é útil para sinalizar erros de lógica de negócio ou para validar parâmetros de um método.

```java
public void sacar(double valor) {
    if (valor <= 0) {
        // Lança uma exceção para indicar que o argumento é inválido.
        throw new IllegalArgumentException("O valor do saque deve ser positivo.");
    }
    if (valor > this.saldo) {
        throw new RuntimeException("Saldo insuficiente.");
    }
    this.saldo -= valor;
}
```

### Declarando Exceções com `throws`

A palavra-chave `throws` é usada na **assinatura de um método** para declarar que ele pode lançar uma ou mais **exceções verificadas (checked)**. Ela funciona como um aviso, delegando a responsabilidade de tratamento para o método que o invocou.

```java
import java.io.FileReader;
import java.io.IOException;

public class LeitorDeArquivo {
    // Este método declara que ele PODE lançar uma IOException.
    // Quem chamar este método será obrigado pelo compilador a tratar essa exceção.
    public void lerPrimeiraLinha(String caminho) throws IOException {
        FileReader fr = null;
        try {
            fr = new FileReader(caminho);
            // ... lógica de leitura ...
            System.out.println("Arquivo lido com sucesso.");
        } finally {
            // Garante que o arquivo seja fechado mesmo se ocorrer um erro na leitura.
            if (fr != null) {
                fr.close();
            }
        }
    }
}
```

## Referência Rápida de Exceções e Erros Comuns

A seguir, uma tabela detalhada com as exceções e erros mais frequentemente encontrados no desenvolvimento Java. Compreender a causa raiz de cada um é um passo fundamental para se tornar um programador proficiente e capaz de depurar problemas de forma eficaz.

| Exceção / Erro                        | Tipo      | Descrição Comum                                                                                                                                                                |
| ------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`StackOverflowError`**              | Error     | A pilha de chamadas da JVM estourou. Geralmente causado por uma recursão infinita (um método que chama a si mesmo sem uma condição de parada).                                 |
| **`OutOfMemoryError`**                | Error     | A JVM não possui mais memória disponível na _Heap_ para alocar novos objetos. Causado por vazamentos de memória ou pelo processamento de dados muito grandes.                  |
| **`ArithmeticException`**             | Unchecked | Ocorre em uma operação matemática ilegal, como uma divisão de inteiros por zero.                                                                                               |
| **`ArrayIndexOutOfBoundsException`**  | Unchecked | Tentativa de acessar um índice de um array que está fora de seus limites (menor que 0 ou maior ou igual ao seu tamanho).                                                       |
| **`ClassCastException`**              | Unchecked | Ocorre ao tentar fazer um _casting_ (conversão de tipo) de um objeto para um tipo com o qual ele é incompatível.                                                               |
| **`IllegalArgumentException`**        | Unchecked | Usada para indicar que um argumento inválido foi passado para um método (ex: um número negativo para um método que só aceita positivos). Geralmente lançada com `throw`.       |
| **`IllegalStateException`**           | Unchecked | Sinaliza que um método foi invocado em um momento inadequado, ou seja, o objeto não está no estado correto para executar aquela operação.                                      |
| **`IndexOutOfBoundsException`**       | Unchecked | Superclasse para exceções de índice inválido. `ArrayIndexOutOfBoundsException` e `StringIndexOutOfBoundsException` herdam dela. Comum em coleções como `List`.                 |
| **`InputMismatchException`**          | Unchecked | Lançada por um `Scanner` quando o tipo de dado lido não corresponde ao tipo esperado (ex: o programa espera um `int` e o usuário digita um texto).                             |
| **`NullPointerException`**            | Unchecked | A exceção mais famosa do Java. Ocorre ao tentar acessar um método ou atributo de uma variável de referência que está apontando para `null`.                                    |
| **`NumberFormatException`**           | Unchecked | Ocorre ao tentar converter uma `String` que não tem o formato de um número válido para um tipo numérico (ex: `Integer.parseInt("abc")`).                                       |
| **`UnsupportedOperationException`**   | Unchecked | Sinaliza que uma operação específica não é suportada pelo objeto. Comum ao tentar modificar uma coleção imutável (ex: `add()` em uma `List.of()`).                             |
| **`StringIndexOutOfBoundsException`** | Unchecked | Tentativa de acessar um índice de uma `String` que está fora de seus limites.                                                                                                  |
| **`NoSuchElementException`**          | Unchecked | Ocorre ao tentar solicitar o próximo elemento de uma estrutura que não tem mais elementos, como um `Iterator` ou um `Scanner`.                                                 |
| **`ConcurrentModificationException`** | Unchecked | Lançada quando uma coleção é modificada de forma inadequada durante uma iteração (ex: remover um item de uma lista dentro de um laço `for-each`).                              |
| **`IllegalMonitorStateException`**    | Unchecked | Relacionada à programação concorrente. Lançada ao tentar chamar métodos como `wait()`, `notify()` ou `notifyAll()` em um objeto sem possuir o "monitor" (lock) daquele objeto. |
| **`IOException`**                     | Checked   | Erro genérico de Entrada/Saída (I/O). É a superclasse para a maioria das exceções relacionadas a arquivos, streams e redes.                                                    |
| **`FileNotFoundException`**           | Checked   | Uma subclasse de `IOException`, indica especificamente que um arquivo não pôde ser encontrado no caminho especificado.                                                         |
| **`SQLException`**                    | Checked   | Indica um erro durante uma operação com um banco de dados através da API JDBC (ex: problema de conexão, consulta SQL malformada).                                              |
| **`InterruptedException`**            | Checked   | Lançada quando uma thread que está em um estado de espera (`sleep()`, `wait()`, `join()`) é interrompida por outra thread.                                                     |

## Considerações Finais

Neste capítulo, exploramos um dos pilares da programação robusta em Java: o **Tratamento de Exceções**. Vimos que exceções não são meramente "erros", mas sim um mecanismo estruturado e poderoso que permite que nossas aplicações lidem com o inesperado de forma controlada, garantindo a integridade dos dados e uma melhor experiência para o usuário.

Primeiramente, desvendamos a hierarquia de erros, compreendendo a distinção fundamental entre `Error`, que representa falhas graves da JVM, e `Exception`, que engloba as condições anormais que podemos e devemos tratar. Aprofundamos na divisão crucial entre **exceções verificadas (checked)**, que nos forçam a lidar com problemas previsíveis em recursos externos, e **exceções não verificadas (unchecked)**, que geralmente sinalizam erros de lógica em nosso próprio código.

Em seguida, dominamos o ferramental prático para o tratamento de erros. O bloco **`try-catch-finally`** se revelou a nossa principal ferramenta para monitorar código, capturar falhas específicas e, crucialmente, garantir a execução de lógicas de limpeza no bloco `finally`, prevenindo o vazamento de recursos. Além de reagir a erros, aprendemos a ser proativos, utilizando a palavra-chave **`throw`** para sinalizar nossas próprias condições de erro e a cláusula **`throws`** para delegar a responsabilidade de tratamento, criando contratos claros entre os métodos de nossa aplicação.

Dominar o tratamento de exceções é o que eleva um código de um simples script funcional para um software de qualidade profissional. É a garantia de que, mesmo diante de uma falha de rede, um arquivo corrompido ou uma entrada de dados inválida, nossa aplicação saberá como reagir, se recuperar ou, no mínimo, falhar de uma maneira graciosa e informativa.

Com um entendimento sólido da sintaxe, das estruturas de dados e, agora, de como gerenciar erros, completamos nossa jornada pelos fundamentos essenciais da linguagem Java. Estamos equipados com todas as ferramentas necessárias para escrever algoritmos complexos e funcionais. O próximo passo é aprender a organizar nosso código de uma forma que seja escalável, manutenível e que modele problemas do mundo real de forma eficaz. Para isso, mergulharemos no paradigma que define o Java: a **Programação Orientada a Objetos**. Nos capítulos seguintes, exploraremos seus quatro pilares — encapsulamento, herança, polimorfismo e abstração — e aprenderemos a construir nossas próprias classes e objetos, a base para o desenvolvimento de sistemas de grande porte.
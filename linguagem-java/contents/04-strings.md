# Capítulo 4 – Strings e Operações Essenciais

Após dominarmos os operadores para manipular dados primitivos, voltamos nossa atenção para um dos tipos por referência mais fundamentais e onipresentes em qualquer linguagem de programação: a **`String`**. Em Java, uma `String` não é um tipo primitivo, mas sim uma classe robusta e altamente otimizada, projetada para representar e manipular sequências de caracteres.

Qualquer texto em uma aplicação — desde o nome de um usuário, uma mensagem de erro, o conteúdo de um arquivo ou um endereço web — é manipulado através de objetos `String`. Por ser tão central, a classe `String` possui características e comportamentos únicos que são essenciais para um desenvolvimento eficiente e seguro.

## As Características Fundamentais da Classe `String`

A classe `String`, que pertence ao pacote `java.lang` (e, portanto, está sempre disponível sem a necessidade de uma importação explícita), é definida com algumas propriedades muito importantes:

- **Imutabilidade**: Esta é a característica mais crucial de uma `String` em Java. Uma vez que um objeto `String` é criado na memória, sua sequência de caracteres **jamais** poderá ser alterada. Qualquer operação que pareça modificar uma string, na verdade, cria e retorna um **novo** objeto `String` com o conteúdo modificado.
- **Final**: A classe `String` é declarada como `final`, o que significa que ela não pode ser estendida ou herdada. Nenhuma outra classe pode ser uma "subclasse" de `String`, garantindo que seu comportamento de imutabilidade e segurança não possa ser alterado.

A decisão de tornar as strings imutáveis em Java não foi acidental; ela traz vantagens significativas:

1. **Segurança**: Strings são frequentemente usadas para armazenar informações sensíveis, como nomes de usuário, senhas, caminhos de arquivo e conexões de rede. A imutabilidade garante que, uma vez que um desses valores seja criado, ele não possa ser alterado por outra parte do sistema, prevenindo modificações maliciosas ou acidentais.
2. **Performance (Caching e o Pool de Strings)**: Para otimizar o uso de memória, a JVM mantém uma área especial chamada **Pool de Strings**. Quando uma string é criada na sua forma literal (veremos a seguir), a JVM primeiro verifica se uma string com aquele mesmo conteúdo já existe no pool. Se existir, em vez de criar um novo objeto, a JVM simplesmente retorna uma referência para o objeto existente. Isso só é possível porque as strings são imutáveis; se pudessem ser alteradas, uma mudança em uma variável afetaria todas as outras que apontam para o mesmo objeto no pool.
3. **Thread Safety**: Como não podem ser modificadas, os objetos `String` são inerentemente seguros para serem compartilhados entre múltiplas threads (linhas de execução concorrentes) sem a necessidade de mecanismos de sincronização complexos.

## Criando Strings: Literal vs. Construtor

Existem duas maneiras principais de se criar um objeto `String` em Java:

1. **Forma Literal (Recomendada)**: Esta é a forma mais comum e otimizada.
    
    ```java
    String s1 = "Java";
    ```
    
    Ao usar aspas duplas, o Java utiliza o mecanismo do Pool de Strings. Se a string "Java" ainda não existir no pool, ela é criada. Se já existir, `s1` simplesmente receberá a referência para o objeto já existente.
    
2. **Usando o Construtor `new`**: Esta forma, embora válida, é menos eficiente e geralmente desnecessária.
    
    ```java
    String s2 = new String("Java");
    ```
    
    A palavra-chave `new` força a JVM a criar um **novo** objeto `String` na memória principal (Heap), independentemente de uma string com o mesmo conteúdo já existir no Pool de Strings.

A diferença prática pode ser observada ao comparar as referências:

```java
String literal1 = "Java";
String literal2 = "Java";
String objeto = new String("Java");

// Comparando referências de memória
System.out.println(literal1 == literal2); // true (ambas apontam para o mesmo objeto no Pool)
System.out.println(literal1 == objeto);   // false (objeto é um novo objeto criado no Heap)

// Comparando o conteúdo textual
System.out.println(literal1.equals(objeto)); // true (o conteúdo é o mesmo)
```

## Operações Essenciais com Strings

A classe `String` oferece um rico conjunto de métodos para inspecionar, comparar e manipular (lembrando, sempre criando novas strings) seu conteúdo.

### Obtenção de Informações Básicas

Estes métodos permitem inspecionar as propriedades fundamentais de uma string.

- **`length()`**: Retorna a quantidade de caracteres na string.
    
    ```java
    String texto = "Olá, Mundo!";
    int tamanho = texto.length(); // tamanho será 11
    ```
    
- **`charAt(int index)`**: Retorna o caractere (`char`) em uma posição (índice) específica. A contagem de índices começa em 0.
    
    ```java
    String palavra = "Java";
    char primeiraLetra = palavra.charAt(0); // 'J'
    char ultimaLetra = palavra.charAt(3);   // 'a'
    ```
    
    Tentar acessar um índice fora do intervalo válido (menor que 0 ou maior que `length() - 1`) resultará em uma `StringIndexOutOfBoundsException`.
    
- **`isEmpty()`**: Retorna `true` se a string estiver vazia (comprimento 0), e `false` caso contrário. É uma forma mais legível do que verificar `texto.length() == 0`.
    
    ```java
    String vazia = "";
    String naoVazia = "conteúdo";
    System.out.println(vazia.isEmpty());     // true
    System.out.println(naoVazia.isEmpty());  // false
    ```

### Comparação de Conteúdo

Como vimos, o operador `==` não é confiável para comparar o conteúdo de strings. Para isso, usamos os seguintes métodos:

- **`equals(Object outroObjeto)`**: Compara o conteúdo textual de duas strings. A comparação é **case-sensitive** (diferencia maiúsculas de minúsculas). Este é o método padrão para verificar se duas strings são textualmente idênticas.
    
    ```java
    String a = "Maçã";
    String b = "maçã";
    System.out.println(a.equals("Maçã")); // true
    System.out.println(a.equals(b));      // false (devido à diferença de caixa)
    ```
    
- **`equalsIgnoreCase(String outraString)`**: Similar ao `equals()`, mas ignora as diferenças entre letras maiúsculas e minúsculas.
    
    ```java
    String a = "Maçã";
    String b = "maçã";
    System.out.println(a.equalsIgnoreCase(b)); // true
    ```
    
- **`compareTo(String outraString)`**: Compara duas strings lexicograficamente (ordem de dicionário). Retorna:
    
    - `0` se as strings forem iguais.
    - Um valor negativo se a string que chama o método vier "antes" da outra.
    - Um valor positivo se a string que chama o método vier "depois" da outra.
    
    ```java
    "a".compareTo("c"); // retorna um valor negativo
    "c".compareTo("a"); // retorna um valor positivo
    "a".compareTo("a"); // retorna 0
    ```
    
- **`startsWith(String prefixo)`** e **`endsWith(String sufixo)`**: Verificam se a string começa ou termina com uma determinada sequência de caracteres.
    
    ```java
    String arquivo = "documento.pdf";
    System.out.println(arquivo.startsWith("doc")); // true
    System.out.println(arquivo.endsWith(".txt"));  // false
    ```

### Busca de Caracteres e Substrings

- **`indexOf(String str)`**: Retorna o índice da primeira ocorrência de uma substring. Se a substring não for encontrada, retorna `-1`.
    
    ```java
    String frase = "O rato roeu a roupa do rei de Roma.";
    int primeiraOcorrencia = frase.indexOf("ro"); // 7
    ```
    
- **`lastIndexOf(String str)`**: Retorna o índice da última ocorrência de uma substring.
    
    ```java
    String frase = "O rato roeu a roupa do rei de Roma.";
    int ultimaOcorrencia = frase.lastIndexOf("ro"); // 28
    ```
    
- **`contains(CharSequence s)`**: Retorna `true` se a string contém a sequência de caracteres especificada.
    
    ```java
    String frase = "O rato roeu a roupa do rei de Roma.";
    System.out.println(frase.contains("rei")); // true
    ```

### Extração e "Modificação" (Criando Novas Strings)

Lembre-se: estes métodos não alteram a string original; eles retornam uma nova string com o resultado da operação.

- **`substring(int beginIndex, int endIndex)`**: Extrai e retorna uma parte da string. O trecho começa no `beginIndex` (inclusivo) e vai até o `endIndex` (exclusivo).
    
    ```java
    String data = "2025-08-14";
    String ano = data.substring(0, 4); // "2025"
    String mes = data.substring(5, 7); // "08"
    ```
    
- **`toUpperCase()`** e **`toLowerCase()`**: Retornam uma nova string com todos os caracteres convertidos para maiúsculas ou minúsculas, respectivamente.
    
    ```java
    String nome = "Carlos";
    String nomeMaiusculo = nome.toUpperCase(); // "CARLOS"
    ```
    
- **`trim()`**: Retorna uma nova string sem os espaços em branco (`whitespace`) do início e do fim. É extremamente útil para limpar dados inseridos pelo usuário.
    
    ```java
    String entradaUsuario = "   senha123   ";
    String senhaLimpa = entradaUsuario.trim(); // "senha123"
    ```
    
- **`replace(char oldChar, char newChar)`**: Retorna uma nova string onde todas as ocorrências de um caractere são substituídas por outro.
    
    ```java
    String preco = "R$ 29,90";
    String precoParaCalculo = preco.replace(',', '.'); // "R$ 29.90"
    ```
    
- **`split(String regex)`**: Um método muito poderoso que divide a string em um array de strings (`String[]`), usando uma expressão regular como delimitador.
    
    ```java
    String listaDeCompras = "maçã,banana,leite,pão";
    String[] itens = listaDeCompras.split(",");
    // itens será um array: ["maçã", "banana", "leite", "pão"]
    ```

### O Desafio da Performance: `StringBuilder` e `StringBuffer`

A imutabilidade da `String` é uma grande vantagem, mas pode se tornar um problema de performance em cenários que exigem muitas modificações de texto, como montar uma string longa dentro de um laço de repetição.

```java
// Anti-padrão: Ineficiente para muitas iterações
String relatorio = "";
for (int i = 0; i < 10000; i++) {
    relatorio = relatorio + "Item " + i + "; "; // Cria um novo objeto String a cada iteração!
}
```

Para esses casos, Java oferece alternativas mutáveis e mais eficientes:

- **`StringBuilder`**: Uma classe que representa uma sequência de caracteres **mutável**. É a escolha ideal para construção dinâmica de strings em um ambiente de thread única (a maioria dos casos).
- **`StringBuffer`**: Similar ao `StringBuilder`, mas seus métodos são sincronizados, tornando-a segura para uso em ambientes com múltiplas threads (_thread-safe_). Essa segurança tem um pequeno custo de performance.

O uso típico envolve criar um `StringBuilder`, usar o método `append()` para adicionar texto repetidamente e, ao final, chamar `toString()` para obter o objeto `String` final.

```java
// A forma correta e performática
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append("Item ").append(i).append("; "); // Modifica o mesmo objeto em memória
}
String relatorioFinal = sb.toString(); // Cria a String final apenas uma vez
```

## Considerações Finais

Neste capítulo, realizamos um mergulho profundo em uma das classes mais essenciais e ubíquas do ecossistema Java: a classe `String`. Dominar a manipulação de texto é uma habilidade indispensável para qualquer desenvolvedor, e a plataforma Java nos oferece um conjunto de ferramentas robusto e seguro para essa finalidade.

O conceito mais importante que exploramos foi a **imutabilidade** das strings. Compreendemos que esta não é uma limitação, mas uma poderosa decisão de design que garante segurança, consistência e permite otimizações de performance significativas, como o **Pool de Strings**. A distinção entre a criação de strings através de literais, que aproveitam esse pool, e o uso do construtor `new`, que força a criação de novos objetos, é fundamental para escrever um código eficiente.

Navegamos pelo rico arsenal de métodos que a classe `String` oferece. Vimos como inspecionar suas propriedades com `length()` e `charAt()`, como realizar comparações de conteúdo de forma segura com `equals()` e `equalsIgnoreCase()`, e como executar buscas precisas com `indexOf()` e `contains()`. Mais importante, reforçamos que todos os métodos que parecem "modificar" uma string — como `toUpperCase()`, `substring()` ou `replace()` — na verdade aderem ao princípio da imutabilidade, retornando sempre um **novo** objeto `String` com o resultado da operação.

Finalmente, abordamos o aspecto da performance, reconhecendo que a imutabilidade pode ser um gargalo em cenários de construção de texto intensiva. Introduzimos `StringBuilder` e `StringBuffer` como as soluções corretas para esses casos, oferecendo uma alternativa **mutável** e altamente performática para a montagem dinâmica de strings.

Com um domínio sólido sobre tipos de dados, operadores e, agora, a manipulação de um dos objetos mais importantes da linguagem, estamos plenamente equipados com as peças fundamentais de um programa. O próximo passo lógico é aprender a organizar essas peças em uma estrutura que tome decisões e execute tarefas de forma repetida. Adentraremos as **estruturas de controle de fluxo**, como `if-else`, `switch`, `for` e `while`, que nos permitirão, finalmente, construir programas com lógica complexa e comportamento dinâmico.
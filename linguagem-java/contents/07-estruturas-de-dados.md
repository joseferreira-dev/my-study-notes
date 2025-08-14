# Capítulo 7 – Estruturas de Dados

Nos capítulos anteriores, construímos uma base sólida sobre como declarar variáveis, usar operadores para manipulá-las e controlar o fluxo de um programa com condicionais e laços de repetição. Com essas ferramentas, podemos escrever algoritmos complexos. No entanto, a maioria das aplicações do mundo real não lida com dados isolados, mas sim com **conjuntos de informações** — uma lista de clientes, um catálogo de produtos, uma grade de pixels em uma imagem, etc.

Para gerenciar esses conjuntos de forma eficaz, precisamos das **estruturas de dados**. Elas são formatos especializados para armazenar, organizar e acessar coleções de dados na memória. A escolha da estrutura de dados correta para um determinado problema é uma das decisões mais críticas no design de software, pois ela influencia diretamente a eficiência, a performance e a complexidade dos algoritmos.

Em Java, o universo das estruturas de dados é vasto. Ele se inicia com a estrutura mais fundamental e nativa da linguagem, o **array**, e se expande para um rico e poderoso ecossistema de classes fornecido pela **API Java Collections Framework**, que inclui implementações de Listas, Conjuntos, Filas e Mapas. Neste capítulo, iniciaremos nossa jornada com o pilar de todas as outras estruturas: o array.

## Array: A Estrutura de Dados Fundamental

O **array** é a estrutura de dados mais básica em Java. Conceitualmente, um array é um contêiner de tamanho fixo que armazena uma sequência de elementos do **mesmo tipo**. Esses elementos são mantidos em um bloco contíguo de memória e são acessados através de um **índice** numérico, que representa a posição de cada elemento na sequência.

As principais características de um array são:

- **Tamanho Fixo (Estático)**: Esta é a característica mais definidora de um array. Uma vez que um array é criado com um determinado tamanho, esse tamanho **não pode mais ser alterado**. Se for necessário armazenar mais elementos do que a capacidade original, a única solução é criar um novo array, maior, e copiar os elementos do antigo para o novo.
- **Homogeneidade**: Um array só pode armazenar elementos de um único tipo, definido em sua declaração. É possível ter um array de `int`, um array de `String` ou um array de objetos `Pessoa`, mas não é possível misturar esses tipos dentro do mesmo array.
- **Acesso Indexado**: Cada posição em um array, chamada de "elemento", é associada a um índice numérico. A indexação em Java é **baseada em zero**, o que significa que o primeiro elemento está no índice `0`, o segundo no índice `1`, e assim por diante, até o último elemento, que se encontra no índice `tamanho - 1`. Esse acesso direto via índice é extremamente rápido, com uma complexidade computacional de O(1) (tempo constante).

### Declaração e Inicialização de Arrays

Existem duas maneiras principais de se criar um array em Java.

1. **Declarando com um Tamanho Fixo**: Nesta abordagem, especifica-se o tamanho do array no momento da criação. Os elementos recebem valores padrão com base em seu tipo.
    - Tipos numéricos (`int`, `double`, etc.): `0` ou `0.0`
    - `boolean`: `false`
    - `char`: `'\u0000'` (caractere nulo)
    - Tipos por referência (objetos, como `String`): `null`
    
    A sintaxe geral é: `tipo[] nomeDoArray = new tipo[tamanho];`
    
    ```java
    // Cria um array de inteiros com 5 posições.
    // Todos os elementos são inicializados com o valor padrão 0.
    int[] numeros = new int[5];
    
    // Cria um array de Strings com 3 posições.
    // Todos os elementos são inicializados com o valor padrão null.
    String[] nomes = new String[3];
    ```
    
2. **Declarando com Valores Literais**: É possível criar e inicializar um array com valores predefinidos em uma única instrução. O tamanho do array é determinado implicitamente pela quantidade de elementos fornecidos.
    
    ```java
    // Cria um array de 5 inteiros com valores específicos.
    int[] notas = {85, 92, 78, 95, 88};
    
    // Cria um array de Strings.
    String[] diasDaSemana = {"Segunda", "Terça", "Quarta", "Quinta", "Sexta"};
    ```

### Acessando e Modificando Elementos

O acesso aos elementos de um array é feito através do seu índice, utilizando a notação de colchetes `[]`.

```java
int[] notas = {85, 92, 78, 95, 88};

// Acessando para leitura
int primeiraNota = notas[0]; // primeiraNota receberá 85
int terceiraNota = notas[2]; // terceiraNota receberá 78

System.out.println("A primeira nota é: " + primeiraNota);

// Acessando para modificação
notas[0] = 90; // Altera o valor do primeiro elemento de 85 para 90
System.out.println("A primeira nota, após alteração, é: " + notas[0]);
```

É crucial respeitar os limites do array. Tentar acessar um índice que não existe (menor que 0 ou maior ou igual ao tamanho do array) resultará em um erro em tempo de execução: a exceção `ArrayIndexOutOfBoundsException`.

### Iterando sobre um Array

A forma mais comum de se trabalhar com todos os elementos de um array é através de laços de repetição.

- **Usando o laço `for` tradicional**: Esta abordagem é útil quando, além do valor, precisamos do índice de cada elemento. Todo array possui uma propriedade `length` que retorna seu tamanho total.
    
    ```java
    for (int i = 0; i < notas.length; i++) {
        System.out.println("Elemento no índice " + i + ": " + notas[i]);
    }
    ```
    
- **Usando o laço `for-each`**: Esta é a forma mais limpa e segura quando precisamos apenas do valor de cada elemento, sem nos preocuparmos com o índice.
    
    ```java
    for (int nota : notas) {
        System.out.println("Nota: " + nota);
    }
    ```

### Arrays Multidimensionais

Embora tenhamos focado em arrays de uma dimensão, o Java também suporta **arrays multidimensionais**, que podem ser vistos como "arrays de arrays". A forma mais comum é o array de duas dimensões (2D), que é perfeito para representar estruturas de grade, como matrizes, tabuleiros de jogos ou planilhas.

```java
// Declara um array 2D para um tabuleiro de jogo da velha (3x3)
char[][] tabuleiro = new char[3][3];

// Atribui um valor a uma posição específica (linha 1, coluna 2)
tabuleiro[1][2] = 'X';

// Inicializando um array 2D com valores literais
int[][] matriz = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Acessando o elemento na primeira linha e terceira coluna
int elemento = matriz[0][2]; // elemento será 3
```

O conceito pode ser estendido para três ou mais dimensões, embora seu uso seja menos frequente em aplicações do dia a dia.

## Interface `List`: A Flexibilidade das Coleções

Embora os arrays sejam eficientes e fundamentais, sua principal limitação — o tamanho fixo — torna-os inadequados para muitos cenários do mundo real, onde a quantidade de dados pode crescer ou diminuir dinamicamente. Para resolver essa e outras complexidades, o Java oferece o **Java Collections Framework (JCF)**, um conjunto sofisticado e poderoso de classes e interfaces para representar e manipular coleções de objetos.

Dentro do JCF, a estrutura mais utilizada e intuitiva é a **Lista**. Uma `List` em Java é uma coleção ordenada de elementos, que permite o armazenamento de itens duplicados e mantém um controle preciso sobre a posição de cada elemento através de um índice. Ela é a evolução natural do array, adicionando a capacidade de crescimento dinâmico e um rico conjunto de métodos para manipulação de dados.

### Interface `List<E>`: O Contrato das Listas

Todo o comportamento fundamental das listas em Java está definido na interface `java.util.List<E>`. Uma interface, em Java, funciona como um "contrato" que especifica um conjunto de métodos que uma classe deve implementar. Isso significa que, independentemente da implementação específica que escolhermos (`ArrayList`, `LinkedList`, etc.), teremos a garantia de que um conjunto padronizado de operações estará disponível.

As operações mais comuns definidas pela interface `List` incluem:

- **Inserção**: `add(E elemento)`, `add(int index, E elemento)`
- **Remoção**: `remove(int index)`, `remove(Object o)`, `clear()`
- **Acesso e Modificação**: `get(int index)`, `set(int index, E elemento)`
- **Busca**: `contains(Object o)`, `indexOf(Object o)`, `lastIndexOf(Object o)`
- **Informações**: `size()`, `isEmpty()`
- **Iteração**: Formas de percorrer todos os elementos, como o laço `for-each`.

Uma das melhores práticas em Java é a de **"programar para a interface"**. Isso significa que, ao declarar uma variável, devemos utilizar o tipo da interface (`List`), e na inicialização, especificar a classe concreta que desejamos usar.

```java
// Boa prática: declarar usando a interface
List<String> nomes = new ArrayList<>();
```

Essa abordagem torna o código mais flexível, pois, se no futuro decidirmos que a implementação `LinkedList` é mais adequada para o nosso caso de uso, basta alterar a linha da instanciação, sem a necessidade de modificar o restante do código que utiliza a variável `nomes`.

### As Principais Implementações

O JCF oferece diversas implementações da interface `List`, cada uma otimizada para um tipo diferente de operação. A escolha da implementação correta é uma decisão de arquitetura que impacta diretamente a performance da aplicação.

| Tipo de Lista              | Estrutura Interna            | Vantagens Principais                                                                                  | Limitações Principais                                                                   | Melhor Cenário de Uso                                                                     |
| -------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **`ArrayList`**            | Array dinâmico               | Acesso aleatório por índice extremamente rápido (O(1)).                                               | Inserções e remoções no meio da lista são lentas (O(n)).                                | Listas de propósito geral, especialmente com mais operações de leitura do que de escrita. |
| **`LinkedList`**           | Lista duplamente encadeada   | Inserções e remoções muito rápidas no início, no fim e no meio (se o iterador já estiver na posição). | Acesso aleatório por índice é lento (O(n)).                                             | Uso como filas (`Queue`) ou pilhas (`Deque`); listas com muitas inserções e remoções.     |
| **`Stack`**                | Subclasse de `Vector`        | Implementa a estrutura de pilha (LIFO) com métodos `push`, `pop`.                                     | Classe legada, considerada obsoleta. `ArrayDeque` é a alternativa moderna.              | Código legado ou quando a semântica de pilha é estritamente necessária.                   |
| **`CopyOnWriteArrayList`** | Array com "cópia na escrita" | Leitura concorrente sem bloqueio (muito rápida e segura).                                             | Extremamente custosa para operações de escrita (cópia todo o array a cada modificação). | Ambientes concorrentes com leitura massiva e escrita muito rara.                          |
| **`List.of()`**            | Lista imutável (Java 9+)     | Leve, segura contra modificações, excelente para constantes.                                          | Não permite adições, remoções, modificações ou elementos `null`.                        | Definição de coleções de dados fixas e constantes.                                        |

#### `ArrayList<E>`: O Padrão para Acesso Rápido

O `ArrayList` é a implementação de lista mais utilizada em Java. Internamente, ele utiliza um **array dinâmico** para armazenar seus elementos. Isso significa que ele oferece acesso extremamente rápido aos elementos por meio de seu índice, pois a localização do elemento na memória pode ser calculada diretamente. No entanto, quando um elemento é adicionado ou removido do meio da lista, todos os elementos subsequentes precisam ser deslocados, tornando essas operações mais lentas.

```java
import java.util.ArrayList;
import java.util.List;

public class ExemploArrayList {
    public static void main(String[] args) {
        List<String> frutas = new ArrayList<>();
        frutas.add("Maçã");
        frutas.add("Banana");
        frutas.add("Laranja");

        // Acesso rápido por índice
        String primeiraFruta = frutas.get(0);
        System.out.println("Primeira fruta: " + primeiraFruta);

        // Modificação por índice
        frutas.set(1, "Morango");
        System.out.println("Lista após modificação: " + frutas);

        // Remoção (operação potencialmente lenta)
        frutas.remove(2);
        System.out.println("Lista após remoção: " + frutas);
    }
}
```

#### `LinkedList<E>`: Eficiência em Inserção e Remoção

O `LinkedList` é implementado como uma **lista duplamente encadeada**. Cada elemento (ou "nó") na lista armazena, além do próprio dado, uma referência para o elemento anterior e para o próximo. Isso torna as operações de adição e remoção, especialmente nas extremidades da lista, muito eficientes, pois envolvem apenas a atualização de algumas referências. Em contrapartida, para acessar um elemento no meio da lista, é necessário percorrê-la a partir do início ou do fim, tornando o acesso por índice uma operação lenta.

```java
import java.util.LinkedList;

public class ExemploLinkedList {
    public static void main(String[] args) {
        LinkedList<String> nomes = new LinkedList<>();

        nomes.add("Ana");      // Adiciona ao final
        nomes.add("Bruno");
        nomes.add("Carlos");

        // Métodos específicos para manipulação das extremidades
        nomes.addFirst("Início"); // Adiciona no início
        nomes.addLast("Final");   // Adiciona no final (equivalente a add)

        System.out.println("Lista: " + nomes);
        // Saída -> Lista: [Início, Ana, Bruno, Carlos, Final]

        nomes.removeFirst();
        System.out.println("Após remover o primeiro: " + nomes);
    }
}
```

### `Stack<E>`: A Estrutura de Pilha (LIFO)

O `Stack` é uma implementação de lista que adere a um princípio de funcionamento específico: **LIFO (Last-In, First-Out)**. A melhor analogia é uma pilha de pratos: o último prato que você coloca no topo da pilha é, necessariamente, o primeiro que você retira. O `Stack` é projetado para modelar exatamente esse comportamento.

Ele fornece um conjunto de métodos que reflete essa lógica:

- **`push(E item)`**: Adiciona (ou "empilha") um item no topo da pilha.
- **`pop()`**: Remove e retorna o item que está no topo da pilha.
- **`peek()`**: Retorna o item que está no topo da pilha, mas **sem** removê-lo.

```java
import java.util.Stack;

public class ExemploStack {
    public static void main(String[] args) {
        Stack<String> historicoNavegador = new Stack<>();

        historicoNavegador.push("google.com");
        historicoNavegador.push("oracle.com");
        historicoNavegador.push("wikipedia.org");

        System.out.println("Pilha atual: " + historicoNavegador);

        String paginaAtual = historicoNavegador.pop(); // Remove e obtém "wikipedia.org"
        System.out.println("Voltando para a página: " + paginaAtual);

        String proximaPagina = historicoNavegador.peek(); // Apenas olha quem é o próximo: "oracle.com"
        System.out.println("Próxima página no histórico: " + proximaPagina);
        
        System.out.println("Pilha final: " + historicoNavegador);
    }
}
```

**Nota sobre o `Stack`**: É importante saber que `Stack` é uma classe legada, introduzida na primeira versão do Java. Ela herda de `Vector`, que é uma classe cujos métodos são todos sincronizados (thread-safe), o que pode introduzir uma sobrecarga de performance desnecessária em aplicações de thread única. A recomendação moderna para implementar uma pilha é utilizar a interface `Deque` com a implementação `ArrayDeque`, que é mais performática e flexível: `Deque<String> pilha = new ArrayDeque<>();`.

### `CopyOnWriteArrayList<E>`: Segurança em Ambientes Concorrentes

Esta é uma implementação de `List` altamente especializada, projetada para cenários de **alta concorrência**, onde as operações de leitura são muito mais frequentes do que as operações de escrita.

O nome "Copy-On-Write" descreve exatamente seu mecanismo de funcionamento:

- **Leitura**: As operações de leitura (`get`, `iterator`, etc.) são extremamente rápidas e não requerem nenhum tipo de bloqueio (_lock_). Elas operam sobre um "instantâneo" (_snapshot_) imutável do array interno. Múltiplas threads podem ler a lista simultaneamente sem qualquer interferência.
- **Escrita**: Qualquer operação de modificação (`add`, `remove`, `set`) é muito **custosa**. A cada modificação, uma **cópia completa** do array interno é criada, a alteração é feita nessa cópia e, finalmente, a referência interna é atualizada para apontar para este novo array.

Este comportamento a torna ideal para situações como listas de _listeners_ (ouvintes) em sistemas de eventos, onde a lista é percorrida milhares de vezes para notificar os ouvintes, mas a adição ou remoção de um novo ouvinte é um evento muito raro.

```java
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.List;

public class ExemploCopyOnWrite {
    public static void main(String[] args) {
        // Ideal para listas de 'listeners' ou 'observers'
        List<String> listeners = new CopyOnWriteArrayList<>();

        listeners.add("ListenerA");
        listeners.add("ListenerB");

        // Iterar sobre a lista é muito rápido e seguro, mesmo que outra thread
        // tente modificá-la ao mesmo tempo.
        for (String listener : listeners) {
            System.out.println("Notificando: " + listener);
        }

        // Esta operação é cara: uma nova cópia do array interno [ListenerA, ListenerB, ListenerC] é criada.
        listeners.add("ListenerC");
    }
}
```

### `List.of()`: Criando Listas Imutáveis

Introduzido no Java 9, `List.of()` é um método de fábrica que fornece uma maneira conveniente e eficiente de criar listas **imutáveis**. Uma lista imutável é uma coleção cujo conteúdo não pode ser alterado após sua criação — não é possível adicionar, remover ou modificar seus elementos.

Essa característica oferece um alto nível de segurança e previsibilidade, sendo perfeita para representar coleções de dados que devem permanecer constantes ao longo da execução de um programa.

Principais propriedades:

- **Imutabilidade Garantida**: Qualquer tentativa de modificar a lista (ex: `add()`, `set()`, `remove()`) resultará em uma `UnsupportedOperationException`.
- **Não Permite Elementos `null`**: Tentar criar uma lista com um elemento `null` usando `List.of()` resultará em uma `NullPointerException`.
- **Eficiência**: Geralmente, consome menos memória do que um `ArrayList` contendo os mesmos elementos.

```java
import java.util.List;

public class ExemploListaImutavel {
    public static void main(String[] args) {
        // Cria uma lista de configurações que não deve ser alterada.
        List<String> configuracoes = List.of("MODO_PRODUCAO", "LOG_ATIVADO", "CACHE_LIGADO");
        
        System.out.println("Configurações ativas: " + configuracoes);

        try {
            // Qualquer tentativa de modificação resultará em uma exceção.
            configuracoes.add("MODO_DEBUG");
        } catch (UnsupportedOperationException e) {
            System.err.println("Erro: Não é possível modificar uma lista imutável.");
            e.printStackTrace();
        }
    }
}
```

### Guia de Escolha: Qual Lista Usar?

A escolha da implementação correta da `List` é uma decisão de design que depende fundamentalmente dos requisitos da sua aplicação. Para decidir, é útil responder a uma série de perguntas sobre o padrão de uso esperado:

1. **A principal operação será acessar elementos aleatoriamente pelo seu índice (ex: `get(i)`)?** A sua escolha padrão deve ser **`ArrayList`**. Sua estrutura baseada em array garante o acesso mais rápido possível por índice (O(1)). Para a grande maioria dos casos de uso de propósito geral, `ArrayList` é a resposta correta.
2. **A lista sofrerá um grande volume de inserções e remoções de elementos, especialmente no início ou no fim?** **`LinkedList`** provavelmente oferecerá um desempenho superior. Sua estrutura de nós encadeados permite que elementos sejam adicionados ou removidos das extremidades de forma muito eficiente, sem a necessidade de deslocar outros elementos.
3. **A lógica do seu código exige um comportamento estrito de pilha LIFO (Last-In, First-Out)?** Embora `Stack` exista, a recomendação moderna é usar a interface **`Deque`** com a implementação **`ArrayDeque`**. Ela fornece os mesmos métodos (`push`, `pop`, `peek`) de forma mais eficiente e consistente com o resto do Collections Framework.
4. **A lista será lida com extrema frequência por múltiplas threads e modificada muito raramente?** Este é o cenário exato para o qual **`CopyOnWriteArrayList`** foi projetado. Ele garante leituras concorrentes seguras e sem bloqueios, ao custo de escritas muito caras.
5. **A coleção de dados é pequena, fixa e nunca deve ser alterada após a criação?** Utilize o método de fábrica **`List.of()`**. Ele cria uma representação imutável, segura e eficiente em termos de memória, perfeita para constantes e configurações.

A tabela abaixo serve como um resumo rápido para referência:

|Se a prioridade é...|Use...|Porque...|
|---|---|---|
|Acesso rápido por índice, uso geral|**`ArrayList`**|Usa um array interno, acesso O(1).|
|Inserções/remoções frequentes|**`LinkedList`**|Usa nós encadeados, modificação de ponteiros é rápida.|
|Lógica de Pilha (LIFO)|**`Deque`/`ArrayDeque`**|Implementação moderna e performática.|
|Leitura concorrente massiva|**`CopyOnWriteArrayList`**|Leituras sem bloqueio, escritas criam cópias.|
|Imutabilidade e segurança|**`List.of()`**|Cria uma lista que não pode ser modificada, garantindo constância.|
    


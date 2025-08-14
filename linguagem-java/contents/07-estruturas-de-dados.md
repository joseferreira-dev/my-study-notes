# Capítulo 7 – Estruturas de Dados

Nos capítulos anteriores, construímos uma base sólida sobre como declarar variáveis, usar operadores para manipulá-las e controlar o fluxo de um programa com condicionais e laços de repetição. Com essas ferramentas, podemos escrever algoritmos complexos. No entanto, a maioria das aplicações do mundo real não lida com dados isolados, mas sim com **conjuntos de informações** — uma lista de clientes, um catálogo de produtos, uma grade de pixels em uma imagem, etc.

Para gerenciar esses conjuntos de forma eficaz, precisamos das **estruturas de dados**. Elas são formatos especializados para armazenar, organizar e acessar coleções de dados na memória. A escolha da estrutura de dados correta para um determinado problema é uma das decisões mais críticas no design de software, pois ela influencia diretamente a eficiência, a performance e a complexidade dos algoritmos.

Em Java, o universo das estruturas de dados é vasto. Ele se inicia com a estrutura mais fundamental e nativa da linguagem, o **array**, e se expande para um rico e poderoso ecossistema de classes fornecido pela **API Java Collections Framework**, que inclui implementações de Listas, Conjuntos, Filas e Mapas. Neste capítulo, iniciaremos nossa jornada com o pilar de todas as outras estruturas: o array.

## Arrays

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

## Listas

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
    

## Mapas

Diferentemente das listas, que são coleções lineares de elementos individuais acessados por um índice numérico, os **Mapas** (ou _Maps_) são estruturas de dados projetadas para armazenar associações, ou mapeamentos, entre um par de objetos: uma **chave** (_key_) e um **valor** (_value_).

A melhor analogia para um mapa é um dicionário: a chave é a "palavra" que se deseja procurar, e o valor é a "definição" associada a essa palavra. Cada chave dentro de um mapa deve ser única, garantindo um caminho direto e inequívoco para se recuperar o valor correspondente.

### A Interface `Map<K, V>`: O Dicionário de Dados do Java

O comportamento dos mapas em Java é definido pela interface `java.util.Map<K, V>`. Note os dois parâmetros genéricos:

- **`K`**: Representa o tipo de dado da **Chave** (_Key_).
- **`V`**: Representa o tipo de dado do **Valor** (_Value_).

É importante destacar que, embora seja uma parte central do Java Collections Framework, a interface `Map` **não herda** da interface `Collection`. Isso ocorre porque `Collection` representa uma coleção de elementos individuais, enquanto `Map` representa uma coleção de pares de chave-valor.

As principais propriedades de um `Map` são:

- **Chaves Únicas**: Um mapa não pode conter chaves duplicadas. Se você tentar inserir um par com uma chave que já existe, o valor antigo associado a essa chave será substituído pelo novo.
- **Acesso pela Chave**: A recuperação de um valor é feita exclusivamente através de sua chave, não por um índice posicional.
- **Valores Duplicados Permitidos**: Diferentes chaves podem ser associadas a valores iguais.

A interface `Map` define um conjunto rico de métodos para manipulação, incluindo:

| Método                        | Descrição                                                                             |
| ----------------------------- | ------------------------------------------------------------------------------------- |
| `put(K chave, V valor)`       | Associa a chave ao valor. Retorna o valor antigo, se a chave já existia, ou `null`.   |
| `get(Object chave)`           | Retorna o valor associado à chave, ou `null` se a chave não for encontrada.           |
| `containsKey(Object chave)`   | Retorna `true` se o mapa contém a chave especificada.                                 |
| `containsValue(Object valor)` | Retorna `true` se o mapa contém pelo menos uma chave associada ao valor especificado. |
| `remove(Object chave)`        | Remove o par chave-valor associado à chave.                                           |
| `keySet()`                    | Retorna um `Set` com todas as chaves do mapa.                                         |
| `values()`                    | Retorna uma `Collection` com todos os valores do mapa.                                |
| `entrySet()`                  | Retorna um `Set` de `Map.Entry<K, V>`, que representa cada par chave-valor.           |
| `size()`                      | Retorna o número de pares chave-valor armazenados.                                    |
| `clear()`                     | Remove todos os mapeamentos do mapa.                                                  |

Assim como nas listas, a boa prática é "programar para a interface":

```java
Map<Integer, String> usuarios = new HashMap<>();
```

### As Principais Implementações de `Map`

A escolha da implementação de `Map` correta depende crucialmente dos requisitos de ordenação, performance e segurança em ambientes concorrentes.

| Tipo de Mapa            | Estrutura Interna                        | Ordenação                                              | Permite Chave/Valor `null`                      | Melhor Cenário de Uso                                                            |
| ----------------------- | ---------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- | -------------------------------------------------------------------------------- |
| **`HashMap`**           | Tabela de Hash                           | Não garante nenhuma ordem.                             | Sim (1 chave `null`, múltiplos valores `null`). | Mapa de propósito geral, quando a velocidade de inserção e busca é a prioridade. |
| **`LinkedHashMap`**     | Tabela de Hash + Lista Duplamente Ligada | Mantém a ordem de inserção.                            | Sim.                                            | Quando a ordem de inserção dos elementos é importante.                           |
| **`TreeMap`**           | Árvore Binária Balanceada                | Ordenado pelas chaves (ordem natural ou `Comparator`). | Não permite chave `null`.                       | Quando é necessário iterar sobre as chaves de forma ordenada.                    |
| **`Hashtable`**         | Tabela de Hash (Legado)                  | Não garante nenhuma ordem.                             | Não (nem chave, nem valor).                     | Apenas para código legado. **Evitar em novos projetos.**                         |
| **`ConcurrentHashMap`** | Tabela de Hash Segmentada                | Não garante nenhuma ordem.                             | Não (nem chave, nem valor).                     | Alternativa moderna e performática ao `Hashtable` para ambientes concorrentes.   |
| **`EnumMap`**           | Array                                    | Ordenado pela ordem natural do `enum`.                 | Chave não pode ser `null`.                      | Quando as chaves são de um tipo `enum`. Extremamente rápido.                     |

#### `HashMap<K, V>`: O Padrão para Performance

O `HashMap` é a implementação de mapa mais utilizada. Ele armazena seus dados em uma **tabela de hash**, o que permite operações de inserção (`put`), busca (`get`) e remoção (`remove`) com uma performance média de tempo constante, **O(1)**. Isso o torna extremamente rápido. O preço dessa velocidade é que ele **não garante nenhuma ordem** ao iterar sobre seus elementos.

```java
import java.util.HashMap;
import java.util.Map;

public class ExemploHashMap {
    public static void main(String[] args) {
        // Mapa de matrículas para nomes de alunos
        Map<Integer, String> alunos = new HashMap<>();

        alunos.put(101, "Ana");
        alunos.put(102, "Carlos");
        alunos.put(103, "Beatriz");
        alunos.put(101, "Ana Silva"); // Substitui o valor da chave 101

        // Acesso rápido pelo get
        System.out.println("Aluno com matrícula 102: " + alunos.get(102));

        // Iterando sobre as chaves
        for (Integer matricula : alunos.keySet()) {
            System.out.println("Matrícula: " + matricula);
        }

        // Iterando sobre os pares chave-valor (forma mais eficiente)
        for (Map.Entry<Integer, String> entry : alunos.entrySet()) {
            System.out.println("Chave: " + entry.getKey() + ", Valor: " + entry.getValue());
        }
    }
}
```

#### `LinkedHashMap<K, V>`: Mantendo a Ordem de Inserção

O `LinkedHashMap` herda de `HashMap`, mantendo sua alta performance, mas com um diferencial: ele também mantém uma **lista duplamente encadeada** interna que conecta todos os seus elementos. Essa lista adicional garante que, ao iterar sobre o mapa, os elementos aparecerão na **exata ordem em que foram inseridos**.

É a escolha perfeita quando a ordem de inserção é um requisito funcional, como em um cache que precisa saber qual item foi inserido por último.

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class ExemploLinkedHashMap {
    public static void main(String[] args) {
        Map<String, String> capitais = new LinkedHashMap<>();
        capitais.put("Brasil", "Brasília");
        capitais.put("Argentina", "Buenos Aires");
        capitais.put("Chile", "Santiago");

        // A iteração SEMPRE seguirá a ordem de inserção
        for (String pais : capitais.keySet()) {
            System.out.println(pais); // Imprime Brasil, depois Argentina, depois Chile
        }
    }
}
```

#### `TreeMap<K, V>`: Ordenação pelas Chaves

O `TreeMap` implementa a interface `Map` utilizando uma estrutura de **árvore binária balanceada (Red-Black Tree)**. Sua principal característica é que ele armazena os pares chave-valor de forma **ordenada com base nas chaves**.

Para que isso seja possível, as chaves devem ser naturalmente ordenáveis (implementar a interface `Comparable`, como `Integer` e `String`) ou um `Comparator` customizado deve ser fornecido no momento da criação do mapa. As operações de `put`, `get` e `remove` são um pouco mais lentas que no `HashMap` (O(log n)), mas ainda muito eficientes.

É a escolha ideal quando a necessidade de manter as chaves em ordem é mais importante que a velocidade máxima de inserção.

```java
import java.util.TreeMap;
import java.util.Map;

public class ExemploTreeMap {
    public static void main(String[] args) {
        // TreeMap ordena as chaves automaticamente
        Map<String, Integer> pontuacoes = new TreeMap<>();
        pontuacoes.put("Carlos", 90);
        pontuacoes.put("Ana", 95);
        pontuacoes.put("Beatriz", 88);

        // A iteração será em ordem alfabética das chaves
        for (String nome : pontuacoes.keySet()) {
            System.out.println(nome); // Imprime Ana, depois Beatriz, depois Carlos
        }
    }
}
```

### `Hashtable<K, V>`: A Implementação Legada e Sincronizada

O `Hashtable` é uma das estruturas de dados originais do Java, presente desde a versão 1.0. Ele é, em funcionalidade, muito similar ao `HashMap`, pois também utiliza uma tabela de hash interna para armazenar pares de chave-valor. No entanto, possui duas diferenças cruciais:

1. **Sincronização (Thread Safety)**: Todos os métodos públicos do `Hashtable` (como `put`, `get`, `remove`) são `synchronized`. Isso significa que a estrutura é segura para ser usada em ambientes com múltiplas threads, pois apenas uma thread pode executar um método do mapa por vez, prevenindo inconsistências de dados. Essa segurança, no entanto, gera um gargalo de performance, tornando-o mais lento que o `HashMap` em ambientes de thread única.
2. **Proibição de `null`**: Diferentemente do `HashMap`, o `Hashtable` **não permite** chaves ou valores nulos. Tentar inserir um `null` resultará em uma `NullPointerException`.

O `Hashtable` é considerado uma classe legada. Seu uso deve ser evitado em novos projetos, sendo mantido principalmente para garantir a retrocompatibilidade com sistemas antigos. Para novos desenvolvimentos que exigem segurança em concorrência, a alternativa moderna e de maior performance é o `ConcurrentHashMap`.

```java
import java.util.Hashtable;
import java.util.Map;

public class ExemploHashtable {
    public static void main(String[] args) {
        // A declaração usa Hashtable, mas a boa prática seria programar para a interface Map.
        Map<String, Integer> inventario = new Hashtable<>();

        inventario.put("Maçãs", 100);
        inventario.put("Bananas", 150);

        System.out.println("Quantidade de maçãs: " + inventario.get("Maçãs"));

        try {
            // A linha abaixo lançará uma NullPointerException.
            inventario.put("Peras", null);
        } catch (NullPointerException e) {
            System.err.println("Erro: Hashtable não permite valores nulos.");
        }
    }
}
```

### `ConcurrentHashMap<K, V>`: Performance e Segurança Concorrente

O `ConcurrentHashMap` é a implementação padrão para mapas que precisam ser acessados e modificados por múltiplas threads simultaneamente. Ele foi projetado para ser uma substituição de alta performance para o `Hashtable`.

Sua grande vantagem reside em sua estratégia de sincronização. Em vez de bloquear o mapa inteiro para cada operação (como faz o `Hashtable`), o `ConcurrentHashMap` utiliza uma técnica mais sofisticada de bloqueio por segmentos ou nós. Isso significa que múltiplas threads podem escrever em diferentes partes do mapa ao mesmo tempo, sem interferir umas com as outras, resultando em um desempenho muito superior em aplicações de alta concorrência. Assim como o `Hashtable`, ele **não permite** chaves ou valores `null`.

Seu uso é ideal sempre que um mapa precisar ser compartilhado de forma segura entre múltiplas threads. É a escolha ideal para caches compartilhados, contadores concorrentes, e gerenciamento de sessões de usuários em aplicações web.

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.Map;

public class ExemploConcurrentHashMap {
    public static void main(String[] args) {
        // Mapa para armazenar o status de processamento de tarefas em um sistema concorrente.
        Map<String, String> statusTarefas = new ConcurrentHashMap<>();

        // Múltiplas threads poderiam adicionar e atualizar status aqui de forma segura.
        statusTarefas.put("Tarefa-001", "Em processamento");
        statusTarefas.put("Tarefa-002", "Aguardando");
        
        // Uma thread pode atualizar o status...
        statusTarefas.put("Tarefa-001", "Concluída");

        System.out.println("Status da Tarefa-001: " + statusTarefas.get("Tarefa-001"));
    }
}
```

### `EnumMap<K, V>`: Otimização Extrema para Chaves `enum`

O `EnumMap` é uma implementação de `Map` altamente especializada e otimizada, projetada para ser usada exclusivamente com chaves de um tipo `enum` (enumeração).

Sua performance excepcional vem de sua estrutura interna: ele **não utiliza uma tabela de hash**. Em vez disso, ele usa um **array simples** para armazenar os valores. A posição de cada valor no array é determinada diretamente pelo `ordinal()` da constante `enum` (sua posição na declaração, começando em zero). Esse acesso direto ao array torna o `EnumMap` extremamente rápido e com um consumo de memória muito baixo.

**Quando usar**: Sempre que as chaves de um mapa forem constantes de um mesmo `enum`. É perfeito para associar dados ou comportamentos a cada estado de um `enum`, como os dias da semana, os naipes de um baralho ou os status de um pedido.

```java
import java.util.EnumMap;
import java.util.Map;

// 1. Primeiro, definimos um enum.
enum Prioridade {
    BAIXA, MEDIA, ALTA, URGENTE
}

public class ExemploEnumMap {
    public static void main(String[] args) {
        // O construtor exige a classe do enum como argumento.
        Map<Prioridade, String> tarefas = new EnumMap<>(Prioridade.class);

        tarefas.put(Prioridade.URGENTE, "Corrigir bug crítico em produção");
        tarefas.put(Prioridade.MEDIA, "Desenvolver nova funcionalidade");
        tarefas.put(Prioridade.BAIXA, "Refatorar documentação");

        System.out.println("Tarefa de prioridade URGENTE: " + tarefas.get(Prioridade.URGENTE));
        
        // A iteração sempre seguirá a ordem natural do enum (BAIXA, MEDIA, ALTA, URGENTE)
        for(Prioridade p : tarefas.keySet()){
            System.out.println(p);
        }
    }
}
```

### Guia de Escolha: Qual Mapa Usar?

A escolha da implementação correta do `Map` é uma decisão de design crucial que afeta a performance, a funcionalidade e a segurança do seu código. Para tomar a decisão correta, faça as seguintes perguntas sobre os seus requisitos:

1. **A prioridade máxima é a velocidade de inserção e busca, e a ordem dos elementos não importa?** **`HashMap`** é a sua escolha padrão. Para a vasta maioria dos casos de uso de propósito geral, sua performance O(1) é imbatível.
2. **É crucial que, ao iterar, os elementos apareçam na mesma ordem em que foram inseridos?** Use **`LinkedHashMap`**. Ele oferece a mesma performance do `HashMap` com o bônus de manter a ordem de inserção, ideal para caches e dados históricos.
3. **É necessário que as chaves estejam sempre ordenadas (alfabeticamente, numericamente, etc.)?** **`TreeMap`** é a solução. Ele mantém o mapa constantemente ordenado com base nas chaves, ao custo de uma performance ligeiramente inferior (O(log n)) em comparação com o `HashMap`.
4. **As chaves do seu mapa são constantes de um tipo `enum`?** Não hesite em usar **`EnumMap`**. Ele fornecerá a melhor performance e o menor consumo de memória para este cenário específico.
5. **O mapa será compartilhado e modificado por múltiplas threads simultaneamente?** **`ConcurrentHashMap`** é a escolha moderna e correta. Ele foi projetado para alta performance em ambientes concorrentes. Evite o uso do `Hashtable` em novos projetos.

A tabela abaixo serve como um resumo rápido para referência:

|Se a prioridade é...|Use...|Porque...|
|---|---|---|
|Velocidade máxima, uso geral|**`HashMap`**|Usa tabela de hash, acesso e inserção O(1).|
|Manter a ordem de inserção|**`LinkedHashMap`**|`HashMap` + lista ligada para manter a ordem.|
|Manter as chaves ordenadas|**`TreeMap`**|Usa árvore binária, garante ordem natural das chaves.|
|Chaves baseadas em `enum`|**`EnumMap`**|Usa um array interno, extremamente rápido e leve.|
|Acesso concorrente seguro|**`ConcurrentHashMap`**|Sincronização granular, alta performance para threads.|
|Código legado|**`Hashtable`**|Sincronizado, mas obsoleto; use `ConcurrentHashMap` em vez dele.|

## Conjuntos (Sets)

Após explorarmos as listas, que se baseiam em ordem e posição, e os mapas, que se baseiam em associações de chave-valor, chegamos a outra estrutura fundamental do Java Collections Framework: o **Conjunto**, ou `Set`.

Um `Set` é uma coleção que modela o conceito de um conjunto matemático: ele armazena um grupo de elementos **sem permitir duplicatas**. Sua principal finalidade não é manter uma ordem ou associar dados, mas sim garantir a unicidade de cada item que ele contém.

### Interface `Set<E>`: A Coleção de Elementos Únicos

O contrato para todas as implementações de conjuntos em Java é definido pela interface `java.util.Set<E>`. Ela herda diretamente da interface `Collection`, o que significa que possui métodos familiares como `add`, `remove`, `contains`, `size` e `isEmpty`.

As propriedades que definem um `Set` são:

- **Não permite elementos duplicados**: Esta é sua característica mais importante. Qualquer tentativa de adicionar um elemento que já existe no conjunto (segundo um critério de igualdade) será simplesmente ignorada.
- **Sem acesso por índice**: Diferentemente das listas, os conjuntos não possuem a noção de uma posição ou índice numérico. Não é possível pedir "o terceiro elemento" de um `Set`. A única pergunta que se pode fazer é se um determinado elemento "está contido" no conjunto.
- **Ordem não garantida (geralmente)**: A maioria das implementações de `Set` não mantém a ordem de inserção dos elementos. Existem, no entanto, implementações específicas que garantem a ordem.

#### Como a Unicidade é Determinada?

A unicidade de um objeto dentro de um `Set` (especialmente em implementações baseadas em hash, como `HashSet`) é determinada pela combinação de dois métodos herdados da classe `Object`: `hashCode()` e `equals()`.

1. Quando se tenta adicionar um elemento, o `Set` primeiro calcula o `hashCode()` do objeto para determinar rapidamente onde ele deveria ser armazenado.
2. Em seguida, ele usa o método `equals()` para comparar o novo elemento com qualquer outro que já esteja naquele mesmo local de armazenamento.
3. Se nenhum elemento igual for encontrado, o novo elemento é inserido. O método `add()` retorna `true`.
4. Se um elemento igual já existir, a inserção é ignorada. O método `add()` retorna `false`.

### As Principais Implementações de `Set`

Assim como `List` e `Map`, a interface `Set` possui diversas implementações concretas, cada uma com diferentes características de performance e ordenação. A estrutura interna dessas implementações é, na verdade, baseada nas implementações de `Map` que já vimos.

| Tipo de Set               | Estrutura Interna                                            | Permite `null`        | Ordenação                                                      | Melhor Cenário de Uso                                                                   |
| ------------------------- | ------------------------------------------------------------ | --------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **`HashSet`**             | Tabela de Hash (usa um `HashMap` por baixo)                  | Sim (um único `null`) | Não garante nenhuma ordem.                                     | Uso geral, quando a ordem não importa e a performance de inserção/busca é crucial.      |
| **`LinkedHashSet`**       | Tabela de Hash + Lista Ligada (usa um `LinkedHashMap`)       | Sim (um único `null`) | Mantém a ordem de inserção.                                    | Quando é necessário garantir unicidade e, ao mesmo tempo, preservar a ordem de chegada. |
| **`TreeSet`**             | Árvore Binária Balanceada (usa um `TreeMap`)                 | Não                   | Mantém os elementos ordenados (ordem natural ou `Comparator`). | Quando é necessário manter os elementos únicos e sempre em ordem crescente.             |
| **`EnumSet`**             | Bit Vector                                                   | Não                   | Mantém a ordem natural do `enum`.                              | Quando os elementos do conjunto são de um tipo `enum`.                                  |
| **`CopyOnWriteArraySet`** | Array com "cópia na escrita" (usa um `CopyOnWriteArrayList`) | Não                   | Mantém a ordem de inserção.                                    | Ambientes concorrentes com leitura massiva e escrita muito rara.                        |

#### `HashSet<E>`: O Padrão para Unicidade e Performance

O `HashSet` é a implementação de `Set` mais comum e de propósito geral. Internamente, ele é implementado utilizando um `HashMap`, onde os elementos do `Set` são armazenados como as chaves do mapa. Essa estrutura lhe confere uma performance excelente, com operações de `add`, `remove` e `contains` executadas, em média, em tempo constante **O(1)**. O preço dessa velocidade é que a ordem de iteração dos elementos não é garantida.

```java
import java.util.HashSet;
import java.util.Set;

public class ExemploHashSet {
    public static void main(String[] args) {
        Set<String> nomes = new HashSet<>();

        // O método add() retorna um booleano
        boolean addAna = nomes.add("Ana");      // true, "Ana" foi adicionada
        boolean addCarlos = nomes.add("Carlos");  // true, "Carlos" foi adicionado
        boolean addAnaDeNovo = nomes.add("Ana"); // false, "Ana" já existe e foi ignorada

        System.out.println("Nomes no conjunto: " + nomes); // A ordem de exibição não é garantida
        System.out.println("Adicionou Ana a primeira vez? " + addAna);
        System.out.println("Adicionou Ana a segunda vez? " + addAnaDeNovo);
    }
}
```

#### `LinkedHashSet<E>`: Unicidade com Ordem de Inserção

O `LinkedHashSet` herda de `HashSet`, mas com um diferencial importante: ele mantém a **ordem de inserção** dos elementos. Internamente, ele utiliza um `LinkedHashMap`, que, como vimos, mantém uma lista duplamente encadeada conectando os elementos na ordem em que foram adicionados.

É a escolha perfeita quando se precisa remover duplicatas de uma coleção, mas é essencial que a ordem original dos elementos restantes seja preservada.

```java
import java.util.LinkedHashSet;
import java.util.Set;

public class ExemploLinkedHashSet {
    public static void main(String[] args) {
        Set<Integer> sequencia = new LinkedHashSet<>();
        sequencia.add(10);
        sequencia.add(5);
        sequencia.add(10); // Ignorado
        sequencia.add(1);

        // A iteração sempre seguirá a ordem de inserção: 10, 5, 1
        System.out.println("Sequência: " + sequencia);
    }
}
```

#### `TreeSet<E>`: Unicidade com Ordenação Natural

O `TreeSet` armazena seus elementos de forma **ordenada**. Internamente, ele é implementado com um `TreeMap`, que utiliza uma estrutura de árvore binária balanceada. Isso garante que, ao iterar sobre o `TreeSet`, os elementos sempre aparecerão em sua ordem natural (numérica, alfabética, etc.) ou em uma ordem customizada, definida por um `Comparator`.

Para que os elementos possam ser ordenados, eles devem implementar a interface `Comparable`, ou um `Comparator` deve ser fornecido ao construtor do `TreeSet`. Por essa razão, `TreeSet` **não permite** elementos `null`.

```java
import java.util.TreeSet;
import java.util.Set;

public class ExemploTreeSet {
    public static void main(String[] args) {
        Set<String> nomesOrdenados = new TreeSet<>();
        nomesOrdenados.add("Carlos");
        nomesOrdenados.add("Ana");
        nomesOrdenados.add("Beatriz");
        nomesOrdenados.add("Carlos"); // Ignorado

        // A iteração sempre será em ordem alfabética: Ana, Beatriz, Carlos
        System.out.println("Nomes ordenados: " + nomesOrdenados);
    }
}
```

### `EnumSet<E>`: Performance Máxima com Tipos `enum`

O `EnumSet` é uma implementação de `Set` de altíssima performance, projetada para trabalhar exclusivamente com chaves de um tipo `enum`. Sua eficiência notável deriva de sua estrutura interna: em vez de usar uma tabela de hash, ele é implementado como um **vetor de bits** (_bit vector_), geralmente utilizando um único `long` (64 bits).

Nessa estrutura, cada bit representa uma das constantes do `enum`. Se o bit está "ligado" (valor 1), significa que a constante correspondente está no conjunto; se está "desligado" (valor 0), ela não está. Essa abordagem torna as operações como `add`, `contains` e `remove` extremamente rápidas e o consumo de memória, mínimo.

É a escolha definitiva sempre que precisar armazenar um conjunto de constantes de um mesmo `enum`. É perfeito para representar um conjunto de opções, permissões ou estados.

```java
import java.util.EnumSet;
import java.util.Set;

// 1. Definição de um enum para representar dias da semana.
enum DiaDaSemana {
    SEGUNDA, TERCA, QUARTA, QUINTA, SEXTA, SABADO, DOMINGO
}

public class ExemploEnumSet {
    public static void main(String[] args) {
        // Cria um EnumSet contendo apenas os dias úteis.
        // É criado usando métodos de fábrica, não o construtor 'new'.
        Set<DiaDaSemana> diasUteis = EnumSet.of(
            DiaDaSemana.SEGUNDA, 
            DiaDaSemana.TERCA, 
            DiaDaSemana.QUARTA, 
            DiaDaSemana.QUINTA, 
            DiaDaSemana.SEXTA
        );

        System.out.println("Dias úteis: " + diasUteis);

        boolean temSabado = diasUteis.contains(DiaDaSemana.SABADO);
        System.out.println("Contém Sábado? " + temSabado); // false

        // A iteração sempre seguirá a ordem natural de declaração do enum.
        for (DiaDaSemana dia : diasUteis) {
            System.out.println(dia);
        }
    }
}
```

### `CopyOnWriteArraySet<E>`: Unicidade e Segurança Concorrente

Esta é a contraparte do `CopyOnWriteArrayList` para a interface `Set`. É uma implementação de conjunto segura para ambientes concorrentes (_thread-safe_), ideal para cenários onde as leituras são muito mais frequentes que as escritas.

Internamente, ele utiliza um `CopyOnWriteArrayList` para armazenar os elementos. Isso significa que ele herda todas as suas características de performance:

- **Leituras**: São muito rápidas e não exigem bloqueio, pois operam em um _snapshot_ imutável da coleção.
- **Escritas**: São muito caras. A cada `add` ou `remove`, uma cópia completa do array interno é criada para garantir a imutabilidade do _snapshot_ anterior.

A unicidade dos elementos é garantida através de uma verificação antes da inserção.

Recomendado em aplicações concorrentes onde um conjunto de elementos únicos precisa ser iterado com muita frequência por múltiplas threads, mas é modificado muito raramente (por exemplo, um conjunto de "observadores" ou "listeners" de eventos).

```java
import java.util.Set;
import java.util.concurrent.CopyOnWriteArraySet;

public class ExemploCopyOnWriteArraySet {
    public static void main(String[] args) {
        Set<String> listeners = new CopyOnWriteArraySet<>();

        // Operações de escrita (caras)
        listeners.add("Listener-A");
        listeners.add("Listener-B");
        listeners.add("Listener-A"); // Duplicado, será ignorado

        System.out.println("Listeners registrados: " + listeners);

        // A iteração é a operação principal e muito eficiente.
        // É seguro iterar aqui enquanto outra thread tenta adicionar/remover um listener.
        for (String listener : listeners) {
            System.out.println("Notificando " + listener);
        }
    }
}
```

### Guia de Escolha: Qual Set Usar?

A escolha da implementação de `Set` correta depende diretamente dos requisitos de ordenação, performance e concorrência da sua aplicação. Para decidir, responda às seguintes perguntas:

1. **A prioridade máxima é a velocidade de inserção/busca e a ordem dos elementos não importa?** **`HashSet`** é a sua escolha padrão. Para a maioria dos casos que exigem apenas a garantia de unicidade sem se preocupar com a ordem, sua performance O(1) é a melhor opção.
2. **É necessário garantir a unicidade, mas também preservar a ordem em que os elementos foram inseridos?** Use **`LinkedHashSet`**. Ele combina a performance do `HashSet` com a previsibilidade de ordem de uma lista ligada.
3. **Os elementos precisam ser únicos e estar sempre em uma ordem natural (alfabética, numérica) ou customizada?** **`TreeSet`** é a solução. Ele mantém o conjunto constantemente ordenado, o que é ideal para quando a iteração ordenada é um requisito funcional.
4. **Os elementos do seu conjunto são constantes de um mesmo tipo `enum`?** Utilize **`EnumSet`** sem hesitar. Ele oferece uma performance e um uso de memória imbatíveis para este cenário específico.
5. **O conjunto será acessado com extrema frequência por múltiplas threads e modificado muito raramente?** Este é o caso de uso exato para **`CopyOnWriteArraySet`**, que otimiza as leituras em ambientes concorrentes ao custo de escritas caras.

A tabela abaixo serve como um resumo rápido para referência:

|Se a prioridade é...|Use...|Porque...|
|---|---|---|
|Velocidade máxima, uso geral|**`HashSet`**|Usa tabela de hash, acesso e inserção O(1).|
|Manter a ordem de inserção|**`LinkedHashSet`**|`HashSet` + lista ligada para manter a ordem.|
|Manter os elementos ordenados|**`TreeSet`**|Usa árvore binária, garante ordem natural dos elementos.|
|Elementos baseados em `enum`|**`EnumSet`**|Usa um bit vector interno, extremamente rápido e leve.|
|Leitura concorrente massiva|**`CopyOnWriteArraySet`**|Leituras sem bloqueio, escritas criam cópias.|

## `Iterator`: Um Passeio Padronizado pelas Coleções

Até agora, vimos diversas implementações de coleções (`ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, etc.), cada uma com sua própria estrutura interna. Uma pergunta natural que surge é: existe uma forma padronizada de percorrer os elementos de qualquer uma dessas coleções, sem precisar conhecer seus detalhes internos?

A resposta é sim, e a solução é a interface `Iterator`. O **`Iterator`** é um padrão de projeto fundamental e uma interface do pacote `java.util` que fornece um mecanismo unificado para percorrer sequencialmente os elementos de uma coleção. Ele funciona como um "cursor" ou um "ponteiro" que sabe como navegar pela coleção, um elemento por vez, abstraindo completamente a complexidade de sua estrutura subjacente.

### Funcionamento Básico do `Iterator`

A interface `Iterator<E>` define três métodos principais:

- **`boolean hasNext()`**: Verifica se ainda existem elementos a serem percorridos na coleção. Retorna `true` se houver um próximo elemento, e `false` caso contrário.
- **`E next()`**: Retorna o próximo elemento da coleção e avança o cursor para a próxima posição.
- **`void remove()`**: Remove da coleção o último elemento que foi retornado pelo método `next()`.

O padrão de uso mais comum envolve obter um objeto `Iterator` a partir de uma coleção (usando o método `iterator()`) e, em seguida, usar um laço `while` para percorrer os elementos.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ExemploIterator {
    public static void main(String[] args) {
        List<String> nomes = new ArrayList<>();
        nomes.add("Ana");
        nomes.add("Carlos");
        nomes.add("Beatriz");

        // 1. Obtém o Iterator a partir da coleção.
        Iterator<String> it = nomes.iterator();

        // 2. Usa hasNext() como condição do laço.
        while (it.hasNext()) {
            // 3. Usa next() para obter o elemento e avançar o cursor.
            String nome = it.next();
            System.out.println(nome);
        }
    }
}
```

Essa abordagem é universal. O mesmo código de laço `while` funcionaria perfeitamente se a variável `nomes` fosse um `LinkedList` ou um `HashSet`, pois o `Iterator` se encarrega de lidar com a lógica de travessia específica de cada estrutura.

### O Poder do `Iterator`: Remoção Segura Durante a Iteração

A principal e mais importante razão para se usar um `Iterator` explicitamente é a necessidade de **remover elementos de uma coleção enquanto se está iterando sobre ela**.

Tentar modificar uma coleção (adicionar ou remover elementos) usando os métodos da própria coleção dentro de um laço `for-each` resultará em uma exceção `ConcurrentModificationException`. Isso ocorre porque a modificação direta da coleção invalida o estado interno do iterador que o `for-each` utiliza por baixo dos panos.

**Exemplo do que NÃO fazer:**

```java
List<String> frutas = new ArrayList<>();
frutas.add("Maçã");
frutas.add("Banana");
frutas.add("Uva");

try {
    for (String fruta : frutas) {
        if (fruta.equals("Banana")) {
            frutas.remove(fruta); // ERRO! Lança ConcurrentModificationException
        }
    }
} catch (java.util.ConcurrentModificationException e) {
    System.err.println("Erro: Não é seguro modificar a lista diretamente dentro de um for-each.");
}
```

A única forma segura de realizar essa operação é utilizando o método `remove()` do próprio `Iterator`. Este método notifica a coleção e o iterador sobre a remoção, mantendo o estado de ambos consistente.

**A forma correta e segura:**

```java
List<Integer> numeros = new ArrayList<>();
numeros.add(10);
numeros.add(5);
numeros.add(20);
numeros.add(3);

Iterator<Integer> it = numeros.iterator();
while (it.hasNext()) {
    Integer numero = it.next();
    if (numero < 10) {
        // Usa o método remove() do Iterator, não da lista.
        it.remove();
    }
}

System.out.println("Lista após a remoção dos menores que 10: " + numeros);
// Saída: Lista após a remoção dos menores que 10: [10, 20]
```

### Por Trás dos Panos: O Laço `for-each`

É útil saber que o laço `for-each`, que temos utilizado para percorrer coleções, é, na verdade, "açúcar sintático" sobre a interface `Iterator`. Quando o compilador Java encontra um laço `for-each`, ele o traduz para um código que utiliza um `Iterator` nos bastidores.

O código:

```java
for (String nome : nomes) {
    System.out.println(nome);
}
```

É, em essência, compilado para algo muito similar a:

```java
Iterator<String> it = nomes.iterator();
while (it.hasNext()) {
    String nome = it.next();
    System.out.println(nome);
}
```

Essa compreensão nos ajuda a entender por que o `for-each` é tão conveniente para leituras, mas por que precisamos recorrer ao `Iterator` explícito quando a remoção de elementos se faz necessária.

## Um Guia para a Iteração em Coleções

Uma das operações mais comuns e fundamentais ao se trabalhar com estruturas de dados como `List`, `Set` e `Map` é a **iteração** — o ato de percorrer cada elemento da coleção para inspecioná-lo, processá-lo ou utilizá-lo em alguma lógica. O Java oferece diferentes mecanismos para realizar essa tarefa, cada um com suas próprias vantagens e casos de uso ideais. Compreender qual método de iteração escolher é crucial para escrever um código que seja não apenas funcional, mas também limpo, eficiente e seguro.

Nesta seção, vamos consolidar e comparar as três principais formas de se iterar sobre coleções em Java: o laço `for` tradicional com índice, o laço `for-each` moderno e o uso explícito de um `Iterator`.

### Laço `for` Tradicional (com Índice)

Esta é a forma clássica de iteração, herdada das primeiras versões da linguagem. Ela se baseia no uso de um contador (geralmente `i`) para acessar cada elemento de uma coleção ordenada (como `List` ou `array`) através de seu índice.

**Quando usar**: É a escolha necessária quando a sua lógica depende do **índice** do elemento, ou quando você precisa percorrer a coleção em uma ordem não sequencial (ex: de trás para frente ou pulando elementos).

- **Vantagens**:
    - Dá acesso direto ao índice de cada elemento.
    - Permite percorrer a coleção em qualquer ordem.
    - Permite modificar elementos na lista usando o método `set(index, novoElemento)`.
- **Limitações**:
    - É mais verboso e propenso a erros (ex: `ArrayIndexOutOfBoundsException` se a condição de parada for incorreta).
    - Não é aplicável a coleções que não são indexadas, como `HashSet`.

```java
import java.util.ArrayList;
import java.util.List;

public class ExemploForTradicional {
    public static void main(String[] args) {
        List<String> produtos = new ArrayList<>();
        produtos.add("Notebook");
        produtos.add("Mouse");
        produtos.add("Teclado");

        for (int i = 0; i < produtos.size(); i++) {
            System.out.println("Índice " + i + ": " + produtos.get(i));
        }
    }
}
```

### Laço `for-each` (Enhanced `for`)

Introduzido no Java 5, o `for-each` é a forma moderna e preferencial de se iterar sobre coleções. Ele abstrai completamente o gerenciamento de índices, tornando o código mais limpo, mais legível e menos suscetível a erros. Ele funciona com qualquer classe que implemente a interface `Iterable` (o que inclui todas as coleções padrão do JCF).

**Quando usar**: Na grande maioria dos casos em que você precisa apenas **ler ou processar cada elemento** de uma coleção, do início ao fim, sem se preocupar com sua posição.

- **Vantagens**:
    - Sintaxe limpa, concisa e expressiva.
    - Mais seguro, pois elimina o risco de erros com índices.
    - Universal, funciona para `arrays`, `List`, `Set`, etc.
- **Limitações**:
    - Não fornece acesso ao índice do elemento.
    - Não permite a remoção de elementos da coleção durante a iteração (causa `ConcurrentModificationException`).

```java
import java.util.HashSet;
import java.util.Set;

public class ExemploForEach {
    public static void main(String[] args) {
        Set<String> tags = new HashSet<>();
        tags.add("Java");
        tags.add("Spring");
        tags.add("SQL");

        for (String tag : tags) {
            System.out.println("Tag: " + tag.toUpperCase());
        }
    }
}
```

### `Iterator` Explícito

O `Iterator` é a interface que serve de base para o funcionamento do laço `for-each`. Usá-lo explicitamente nos dá o maior nível de controle sobre o processo de iteração.

**Quando usar**: É a única forma segura de **remover elementos de uma coleção enquanto se está iterando sobre ela**.

- **Vantagens**:
    - Permite a remoção segura de elementos durante a iteração usando o método `iterator.remove()`.
    - Funciona com qualquer classe que implemente `Iterable`.
- **Limitações**:
    - É mais verboso que o laço `for-each`.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ExemploIteratorRemocao {
    public static void main(String[] args) {
        List<Double> notas = new ArrayList<>();
        notas.add(7.5);
        notas.add(4.0);
        notas.add(9.0);
        notas.add(3.5);

        Iterator<Double> it = notas.iterator();
        while (it.hasNext()) {
            Double nota = it.next();
            if (nota < 5.0) {
                it.remove(); // Forma correta e segura de remover
            }
        }
        System.out.println("Notas restantes (aprovados): " + notas);
    }
}
```

### Tabela Comparativa de Métodos de Iteração

|Característica|Laço `for` Tradicional|Laço `for-each`|`Iterator` Explícito|
|---|---|---|---|
|**Ideal para**|Iteração com controle de índice.|Leitura simples de todos os elementos.|Remoção segura durante a iteração.|
|**Vantagens**|Acesso ao índice, flexibilidade de ordem.|Simplicidade, legibilidade, segurança.|Controle fino, remoção segura.|
|**Limitações**|Verboso, propenso a erros, não funciona em Sets.|Sem acesso ao índice, não permite remoção segura.|Mais verboso que o `for-each`.|
|**Aplicações**|`List`, `array`|Qualquer `Iterable` (`List`, `Set`, `Queue`...)|Qualquer `Iterable`|

## `Enum`: Um Tipo Especial para Constantes

Além das estruturas de dados tradicionais, como listas e mapas, o Java oferece um tipo de referência especial, poderoso e seguro para representar um conjunto fixo de constantes: o **`enum`**, abreviação de **enumeração**.

Introduzido no Java 5, um `enum` é muito mais do que apenas uma lista de nomes. Ele é, na verdade, um tipo de classe especial que garante que as variáveis daquele tipo só possam conter um dos valores pré-definidos na sua declaração. Isso elimina os chamados "números mágicos" e "strings mágicas" do código — valores literais soltos cujo significado não é claro — substituindo-os por constantes nomeadas, legíveis e seguras.

### Por que e Quando Usar um `Enum`?

Imagine que você precisa representar os dias da semana em seu código. Uma abordagem ingênua seria usar inteiros (1 para Domingo, 2 para Segunda, etc.) ou Strings ("DOMINGO", "SEGUNDA"). Ambas as abordagens são frágeis:

- **Com inteiros**: O que impede um desenvolvedor de passar o valor 8 ou -1? Nada. O código compilaria, mas quebraria em tempo de execução.
- **Com Strings**: Abre margem para erros de digitação ("Segundaa") e não há garantia de que apenas os sete valores corretos sejam usados.

Um `enum` resolve esses problemas de forma elegante, criando um novo tipo de dado que só aceita os valores que você definiu.

A sintaxe para declarar um `enum` é simples:

```java
public enum DiaDaSemana {
    DOMINGO,
    SEGUNDA,
    TERCA,
    QUARTA,
    QUINTA,
    SEXTA,
    SABADO
}
```

Por convenção, os nomes das constantes de um `enum` são escritos em letras maiúsculas.

### Utilizando `Enums` na Prática

Uma vez definido, você pode usar o `enum` como qualquer outro tipo de dado. Ele é especialmente poderoso quando combinado com estruturas de controle, como o `switch`.

```java
public class TesteEnum {
    public static void main(String[] args) {
        DiaDaSemana hoje = DiaDaSemana.QUARTA;

        String tipoDeDia;

        switch (hoje) {
            case SEGUNDA:
            case TERCA:
            case QUARTA:
            case QUINTA:
            case SEXTA:
                tipoDeDia = "Dia útil";
                break;
            case SABADO:
            case DOMINGO:
                tipoDeDia = "Fim de semana";
                break;
            default:
                tipoDeDia = "Desconhecido";
        }

        System.out.println("Hoje é " + hoje + " e é um " + tipoDeDia);
    }
}
```

### `Enums` são Mais do que Constantes

Um `enum` em Java é uma classe completa. Isso significa que ele pode ter campos, métodos e até mesmo construtores. Essa capacidade permite associar dados ou comportamentos a cada uma das constantes.

```java
public enum StatusPedido {
    AGUARDANDO_PAGAMENTO("Aguardando Pagamento"),
    PROCESSANDO("Em Processamento"),
    ENVIADO("Enviado para transportadora"),
    ENTREGUE("Entregue ao cliente");

    private final String descricao;

    // Construtor do enum (é sempre privado)
    StatusPedido(String descricao) {
        this.descricao = descricao;
    }

    // Método para acessar a descrição
    public String getDescricao() {
        return descricao;
    }
}

// Em outra parte do código:
StatusPedido status = StatusPedido.ENVIADO;
System.out.println("Status atual: " + status.getDescricao());
// Saída: Status atual: Enviado para transportadora
```

Essa capacidade de encapsular lógica e dados torna os `enums` uma ferramenta de modelagem extremamente poderosa e segura para representar um conjunto finito de estados ou categorias em um sistema.

## Considerações Finais

Neste capítulo, fizemos uma jornada essencial pelo mundo da organização de dados em Java. Partimos da estrutura mais fundamental, o **array**, compreendendo sua natureza estática e o acesso rápido via índice, e então mergulhamos no rico e flexível **Java Collections Framework**.

Exploramos a interface `List`, a evolução natural do array, que nos oferece coleções ordenadas e de tamanho dinâmico. Dissecamos suas principais implementações — `ArrayList` para acesso rápido, `LinkedList` para inserções e remoções eficientes — e vimos soluções especializadas como `Stack`, `CopyOnWriteArrayList` e as listas imutáveis do `List.of()`, aprendendo a escolher a ferramenta certa para cada trabalho.

Em seguida, desvendamos o poder dos **Mapas**, estruturas que nos permitem criar associações de chave-valor. O `HashMap` se destacou pela sua performance, o `LinkedHashMap` por manter a ordem de inserção e o `TreeMap` por garantir a ordenação pelas chaves.

Avançamos para os **Conjuntos (Sets)**, compreendendo seu papel crucial na garantia de elementos únicos, com `HashSet`, `LinkedHashSet` e `TreeSet` oferecendo as mesmas tradeoffs de performance e ordenação de seus equivalentes em `Map`.

Vimos como o `Iterator` nos fornece um mecanismo padronizado e seguro para percorrer e, mais importante, modificar coleções durante a iteração. Finalmente, introduzimos o `Enum` como um tipo especial para criar conjuntos de constantes de forma segura e expressiva.

O domínio sobre as estruturas de dados é o que separa a programação trivial da engenharia de software robusta. A escolha correta de uma `List`, `Set` ou `Map` pode significar a diferença entre uma aplicação lenta e ineficiente e um sistema rápido e escalável.
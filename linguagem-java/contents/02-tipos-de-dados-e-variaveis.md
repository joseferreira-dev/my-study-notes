# Capítulo 2 – Tipos de Dados e Variáveis

No capítulo anterior, estabelecemos a estrutura e os conceitos fundamentais da plataforma Java. Agora, é hora de explorar os "tijolos e a argamassa" com os quais construiremos nossas aplicações: os dados. Toda informação que um programa manipula — seja um nome de usuário, a idade de uma pessoa, o preço de um produto ou o estado de uma conexão — precisa ser armazenada na memória do computador.

Como vimos, Java é uma linguagem de **tipagem estática e forte**. Isso significa que, antes de podermos armazenar qualquer dado, precisamos informar ao compilador que tipo de dado ele é. Essa declaração funciona como um contrato: ela define o tamanho do espaço que será reservado na memória, os valores que são permitidos nesse espaço e o conjunto de operações que podem ser realizadas com ele. Essa abordagem garante um alto nível de segurança e previsibilidade ao código.

Em Java, os tipos de dados se dividem em duas categorias fundamentais, cada uma com características e propósitos distintos: os **tipos primitivos** e os **tipos por referência**.

## Tipos Primitivos

Os tipos primitivos são os tipos de dados mais básicos e nativos da linguagem. Eles não são objetos, o que significa que não possuem métodos nem herdam de qualquer outra classe. Eles representam valores simples e puros, como um número ou um caractere, e são armazenados diretamente na área de memória da pilha (_stack_), o que os torna extremamente rápidos e eficientes em termos de performance. Java define exatamente oito tipos primitivos.

### Tipos Numéricos Inteiros

Estes tipos são usados para representar números inteiros, sem parte fracionária. A escolha entre eles depende do tamanho do número que se pretende armazenar e da necessidade de otimizar o uso de memória.

|Tipo|Tamanho|Intervalo de Valores|Exemplo de Declaração|
|---|---|---|---|
|`byte`|8 bits|-128 a 127|`byte b = 10;`|
|`short`|16 bits|-32.768 a 32.767|`short s = 3000;`|
|`int`|32 bits|-2<sup>31</sup> a 2<sup>31</sup>-1|`int i = 123456;`|
|`long`|64 bits|-2<sup>63</sup> a 2<sup>63</sup>-1|`long l = 12345678900L;`|

- **`int`**: É o tipo de dado inteiro padrão e o mais utilizado em Java. A menos que haja uma razão específica para economizar memória (`byte`, `short`) ou a necessidade de armazenar um número extremamente grande (`long`), o `int` é a escolha ideal para representar contadores, índices e outros valores inteiros de uso geral.
- **`long`**: Deve ser utilizado quando o valor a ser armazenado ultrapassa o limite de um `int` (aproximadamente 2 bilhões). É comum em aplicações que lidam com identificadores únicos de bancos de dados (IDs) ou medições de tempo em milissegundos. Note que, ao declarar um literal `long`, é obrigatório adicionar o sufixo `L` no final do número. Isso informa ao compilador para tratar o número como um `long`, evitando um erro de compilação caso o número seja grande demais para um `int`.
- **`byte` e `short`**: São usados em cenários específicos onde a otimização da memória é crítica, como na manipulação de dados de arquivos, fluxos de rede (_streams_) ou ao trabalhar com arrays muito grandes em sistemas com memória limitada.

### Tipos Numéricos de Ponto Flutuante

Estes tipos são usados para representar números com parte fracionária (decimais).

|Tipo|Tamanho|Precisão (Dígitos Significativos)|Exemplo de Declaração|
|---|---|---|---|
|`float`|32 bits|6 a 7 dígitos|`float f = 5.75f;`|
|`double`|64 bits|15 a 16 dígitos|`double d = 3.1415926535;`|

- **`double`**: É o tipo de ponto flutuante padrão em Java. Oferece uma precisão maior ("dupla precisão") e é a escolha recomendada para a maioria das aplicações que envolvem cálculos com números decimais, como em contextos científicos, financeiros ou estatísticos.
- **`float`**: Possui uma precisão menor ("precisão simples") e deve ser usado quando a economia de memória é uma prioridade e a precisão do `double` não é estritamente necessária. Por padrão, o Java interpreta qualquer número com casas decimais como um `double`. Portanto, para declarar um literal `float`, é obrigatório adicionar o sufixo `f` no final do número.

### Tipo Caractere

|Tipo|Tamanho|Descrição|Exemplo de Declaração|
|---|---|---|---|
|`char`|16 bits|Representa um único caractere Unicode|`char letra = 'A';`|

O tipo `char` é usado para armazenar um único caractere. Um detalhe importante é que Java utiliza o padrão **Unicode**, o que significa que um `char` pode representar não apenas os caracteres do alfabeto latino, mas também símbolos e caracteres de praticamente todos os sistemas de escrita do mundo (ex: 'ç', 'ã', 'α', '本'). Literais do tipo `char` são sempre definidos entre aspas simples (`' '`).

### Tipo Booleano

|Tipo|Valores Possíveis|Exemplo de Declaração|
|---|---|---|
|`boolean`|`true` ou `false`|`boolean isAtivo = true;`|

O tipo `boolean` representa um valor lógico e só pode conter um de dois valores possíveis: `true` (verdadeiro) ou `false` (falso). É a base de toda a lógica de controle de fluxo em um programa, sendo fundamental em estruturas de decisão (`if-else`), laços de repetição (`while`, `for`) e outras expressões condicionais.

## Tipos por Referência (ou Tipos de Objeto)

Em contraste com os tipos primitivos, os **tipos por referência** não armazenam o valor do dado diretamente na variável. Em vez disso, a variável armazena uma **referência**, que é essencialmente o **endereço de memória** onde o objeto real está localizado.

Pode-se fazer uma analogia com um controle remoto (a variável de referência) e uma televisão (o objeto). O controle remoto não é a televisão, mas ele "aponta" para ela e permite que você interaja com ela (mudar o canal, ajustar o volume). Da mesma forma, uma variável de referência aponta para um objeto na memória e permite invocar seus métodos e acessar seus dados.

Qualquer tipo que não seja um dos oito tipos primitivos é um tipo por referência. Exemplos comuns incluem:

- **`String`**: Para representar sequências de caracteres. Ex: `String nome = "João da Silva";`.
- **Arrays**: Para armazenar coleções de tamanho fixo de outros tipos. Ex: `int[] numeros = {1, 2, 3};`.
- **Classes Personalizadas**: Qualquer classe que você ou outra pessoa defina. Ex: `Pessoa p = new Pessoa();`.
- **Classes da API Java**: Classes como `Scanner`, `ArrayList`, `LocalDate`, etc.
- **Classes Wrapper**: `Integer`, `Double`, `Boolean`, etc., que "empacotam" os tipos primitivos.

### Distinção Crucial: Primitivos vs. Referência

A diferença de comportamento entre esses dois grupos de tipos é uma das distinções mais importantes em Java.

|Característica|Tipo Primitivo|Tipo por Referência|
|---|---|---|
|**Armazenamento**|O valor é armazenado diretamente na variável.|A variável armazena a referência (endereço) do objeto na memória.|
|**Herança de `Object`**|Não. Não são objetos.|Sim. Todo objeto herda, direta ou indiretamente, da classe `Object`.|
|**Métodos Disponíveis**|Não possuem métodos.|Possuem métodos que podem ser invocados (ex: `toUpperCase()` em `String`).|
|**Comparação (`==`)**|Compara os valores diretamente.|Compara se as referências apontam para o mesmo objeto na memória.|

Por exemplo, a comparação com `==` é um ponto que frequentemente confunde iniciantes. Para tipos primitivos, o resultado é intuitivo:

```java
int a = 100;
int b = 100;
// A expressão (a == b) resultará em 'true', pois os valores são iguais.
```

Para tipos por referência, a história é diferente. O `==` verifica se duas variáveis apontam para o **mesmo endereço de memória**, e não se os objetos têm conteúdo igual.

```java
String nome1 = new String("Ana");
String nome2 = new String("Ana");
// A expressão (nome1 == nome2) resultará em 'false', pois são dois objetos diferentes
// em locais diferentes da memória, apesar de terem o mesmo conteúdo.
// Para comparar o conteúdo, utiliza-se o método .equals(): nome1.equals(nome2) -> true
```

### Classes Empacotadoras (Wrappers)

Para cada um dos oito tipos primitivos, a biblioteca padrão do Java (`java.lang`) fornece uma classe correspondente, conhecida como **classe empacotadora (wrapper class)**.

|Primitivo|Wrapper|
|---|---|
|`byte`|`Byte`|
|`short`|`Short`|
|`int`|`Integer`|
|`long`|`Long`|
|`float`|`Float`|
|`double`|`Double`|
|`char`|`Character`|
|`boolean`|`Boolean`|

O design do Java buscou um equilíbrio entre a eficiência dos tipos primitivos e a consistência do paradigma de orientação a objetos. Os wrappers existem para criar uma "ponte" entre esses dois mundos, permitindo que um valor primitivo seja tratado como um objeto quando necessário. Isso é especialmente útil em situações onde apenas objetos são permitidos, como nas coleções da API Java (um `ArrayList`, por exemplo, não pode armazenar `int`, mas pode armazenar `Integer`).

Essa conversão entre primitivos e seus wrappers é tão comum que o Java moderno realiza o processo automaticamente através de mecanismos chamados **autoboxing** (conversão automática de primitivo para wrapper) e **unboxing** (conversão automática de wrapper para primitivo).

```java
// Autoboxing: o primitivo 100 é automaticamente "empacotado" em um objeto Integer.
Integer numeroWrapper = 100;

// Unboxing: o objeto Integer é automaticamente "desempacotado" para um primitivo int.
int numeroPrimitivo = numeroWrapper;
```


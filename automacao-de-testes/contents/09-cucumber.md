## Capítulo 9 – Cucumber

Bem-vindos ao capítulo dedicado ao **Cucumber**, uma ferramenta poderosa que transforma a maneira como escrevemos e entendemos testes de software! Nos capítulos anteriores, exploramos diversos frameworks e ferramentas focados em diferentes aspectos da automação e do gerenciamento de testes. Agora, vamos mergulhar no universo do **Desenvolvimento Orientado a Comportamento (Behavior-Driven Development - BDD)** e como o Cucumber se encaixa perfeitamente nessa filosofia, promovendo uma colaboração mais eficaz entre todos os envolvidos no projeto.

O Cucumber se destaca por permitir que especificações de teste sejam escritas em uma linguagem natural, compreensível tanto por desenvolvedores técnicos quanto por analistas de negócios, gerentes de produto e outros stakeholders. Essa abordagem não apenas melhora a comunicação, mas também garante que o software desenvolvido esteja verdadeiramente alinhado com as necessidades e expectativas do usuário.

Neste capítulo, desvendaremos os conceitos fundamentais do Cucumber, começando pela sua linguagem de especificação, o **Gherkin**. Exploraremos suas palavras-chave, a estrutura dos arquivos de feature e como os cenários de teste são definidos. Em seguida, detalharemos como esses cenários em linguagem natural são conectados ao código de automação através dos **Step Definitions**. Abordaremos também funcionalidades importantes como o uso de **Data Tables**, **Hooks** para gerenciar o ciclo de vida da execução e **Tags** para organizar e filtrar seus testes. Prepare-se para descobrir como o Cucumber pode tornar seus testes mais expressivos, colaborativos e eficazes!

### Entendendo o Cucumber e o Desenvolvimento Orientado a Comportamento (BDD)

O **Cucumber** é uma ferramenta de teste de software que suporta o Desenvolvimento Orientado a Comportamento (BDD). Ele permite que os desenvolvedores, testadores e analistas de negócios escrevam testes em uma linguagem de fácil compreensão por pessoas não técnicas. A principal característica do Cucumber é o uso de uma sintaxe baseada em linguagem natural chamada **Gherkin**, que descreve o comportamento do sistema em termos de cenários e passos de teste. Esta abordagem é amplamente utilizada no desenvolvimento de software ágil e promove uma colaboração mais estreita e eficaz entre desenvolvedores e as partes interessadas não técnicas, garantindo que todos tenham uma compreensão compartilhada do que o software deve fazer.

Mas, antes de prosseguirmos, vamos relembrar rapidamente o que é um teste baseado em comportamento. Se você já está familiarizado com o BDD, ótimo! Caso contrário, aqui vai um lembrete: O **Behavior-Driven Development (BDD)** é uma técnica de desenvolvimento de software ágil que incentiva a colaboração entre desenvolvedores, especialistas em qualidade (QAs) e pessoas não técnicas ou de negócios em um projeto de software. O foco do BDD é definir o comportamento esperado de uma aplicação a partir da perspectiva do usuário. Isso é feito através da escrita de cenários de teste em linguagem natural, antes mesmo da codificação da funcionalidade. O objetivo primordial do BDD é garantir que o sistema atenda às expectativas do usuário e aos requisitos de negócio, servindo os testes como uma documentação viva e executável do sistema.

O Cucumber, portanto, é uma das ferramentas que viabilizam a prática do BDD, fornecendo a ponte entre as especificações de comportamento em linguagem natural e o código que as implementa e verifica.

### Gherkin: A Linguagem Natural dos Testes Cucumber

No coração do Cucumber está o **Gherkin**. Gherkin é uma linguagem específica de domínio (DSL - Domain Specific Language) projetada para definir especificações executáveis para aplicações de software. Sua estrutura é baseada em texto simples, geralmente em inglês, e utiliza um conjunto de palavras-chave especiais para dar organização e significado às especificações de comportamento.

A beleza do Gherkin reside na sua simplicidade e legibilidade. Ele permite que qualquer pessoa, mesmo sem conhecimento técnico de programação, possa ler e entender o que um teste específico está verificando. Isso torna o Gherkin uma ferramenta poderosa para escrever testes que são não apenas claros e concisos, mas também expressivos e fáceis de manter ao longo do tempo.

As especificações escritas em Gherkin são armazenadas em arquivos com a extensão `.feature`. Cada arquivo `.feature` geralmente descreve uma funcionalidade específica do sistema e contém um ou mais cenários que ilustram como essa funcionalidade deve se comportar em diferentes situações.

#### Palavras-chave Essenciais do Gherkin

O Gherkin utiliza um conjunto bem definido de palavras-chave para estruturar as especificações de teste. Essas palavras-chave foram traduzidas para diversos idiomas, permitindo que as equipes escrevam seus testes na língua que lhes for mais conveniente. Para os propósitos deste capítulo, utilizaremos as palavras-chave em inglês, que são as mais comuns em documentações e exemplos globais.

Vamos explorar as principais palavras-chave e como elas são usadas para construir seus testes:

**`Feature`**: Esta é a palavra-chave fundamental e de mais alto nível em um arquivo Gherkin. Ela é usada para dar um nome e uma breve descrição à funcionalidade ou ao recurso do software que está sendo testado. Geralmente, um arquivo `.feature` contém uma única declaração `Feature`.

```gherkin   
Feature: Login de Usuário
	Para garantir a segurança do sistema,
    Como um usuário registrado,
    Eu quero poder fazer login com minhas credenciais.
```

A descrição que se segue à palavra-chave `Feature` (até a próxima palavra-chave como `Rule`, `Background` ou `Scenario`) serve para contextualizar a funcionalidade.

**`Rule`** (a partir do Gherkin 6): Esta palavra-chave foi introduzida para permitir um agrupamento mais granular de cenários dentro de uma `Feature`. Uma `Rule` é usada para descrever uma regra de negócio específica ou um aspecto particular da funcionalidade que está sendo testada. Isso ajuda a organizar melhor os cenários, especialmente em features complexas, e a alinhar os testes mais diretamente com as regras de negócio.

```gherkin
Feature: Gerenciamento de Carrinho de Compras
        
  Rule: Clientes podem adicionar itens ao carrinho, mas não exceder o limite de estoque.
    
    Scenario: Adicionar item disponível ao carrinho
	  # ... passos do cenário ...
    Scenario: Tentar adicionar item indisponível ao carrinho
      # ... passos do cenário ...
```

**`Example`** (ou seu sinônimo **`Scenario`**): Esta palavra-chave define um cenário de teste concreto e específico. Um cenário é uma sequência de passos que descreve uma interação particular com o sistema e o resultado esperado dessa interação. Cada `Feature` pode conter múltiplos cenários.

```gherkin
Scenario: Login com credenciais válidas
  Given que eu estou na página de login
  When eu insiro "usuario_valido" no campo usuário
  And eu insiro "senha_correta" no campo senha
  And eu clico no botão "Entrar"
  Then eu devo ser redirecionado para o painel principal
```

**`Given`**, **`When`**, **`Then`**, **`And`**, **`But`**: Estas são as palavras-chave que definem os passos dentro de um cenário. Elas estabelecem uma estrutura clara para descrever o comportamento:

- **`Given` (Dado que)**: Usada para descrever o contexto inicial do sistema, as pré-condições que devem ser satisfeitas antes que a ação principal do cenário ocorra.
- **`When` (Quando)**: Usada para descrever a ação principal que é executada pelo usuário ou por um evento externo no sistema.
- **`Then` (Então)**: Usada para descrever o resultado esperado, a consequência da ação descrita no `When`. É aqui que as verificações (asserções) são tipicamente feitas.
- **`And` (E)**: Usada para adicionar múltiplos passos do mesmo tipo (`Given`, `When` ou `Then`) sem ter que repetir a palavra-chave principal. Ajuda na legibilidade.
- **`But` (Mas)**: Similar ao `And`, mas geralmente usada para expressar uma continuação negativa ou uma exceção ao passo anterior do mesmo tipo.
    
É importante notar que, embora cada uma dessas palavras-chave tenha um propósito semântico, o Cucumber tecnicamente não diferencia entre elas ao procurar por Step Definitions. A escolha correta melhora a clareza do cenário.
    
**`*` (Asterisco)**: Em algumas implementações ou estilos, o asterisco pode ser usado no lugar de qualquer uma das palavras-chave de passo (`Given`, `When`, `Then`, `And`, `But`). Isso pode ser útil para criar listas de passos ou quando a distinção semântica exata não é crucial para a legibilidade em um contexto específico.

```gherkin
Scenario: Configuração inicial
  * O sistema está online
  * O banco de dados está acessível
  * O usuário "admin" está registrado
```
        
**`Background` (Contexto)**: Esta palavra-chave é usada para definir uma série de passos que são comuns a todos os cenários dentro de um arquivo `Feature` (ou dentro de uma `Rule`, se aplicável). Os passos definidos em um `Background` são executados antes de cada `Scenario` ou `Scenario Outline` no mesmo escopo. Isso é útil para evitar a repetição de passos de configuração comuns.

```gherkin
Feature: Blog
        
  Background:
    Given que existe um usuário "autor@exemplo.com" com a senha "senha123"
    And que estou logado como "autor@exemplo.com"
    And que estou na página de criação de posts
        
  Scenario: Criar um novo post com sucesso
    When eu preencho o título com "Meu Primeiro Post"
    And eu preencho o conteúdo com "Este é o conteúdo do post."
    And eu clico em "Publicar"
    Then eu devo ver a mensagem "Post publicado com sucesso!"
    And o post "Meu Primeiro Post" deve estar listado
        
  Scenario: Tentar criar um post sem título
    When eu preencho o conteúdo com "Conteúdo sem título."
    And eu clico em "Publicar"
    Then eu devo ver a mensagem de erro "Título é obrigatório."
```

No exemplo acima, os três passos do `Background` serão executados antes do cenário "Criar um novo post com sucesso" e também antes do cenário "Tentar criar um post sem título".

**`Scenario Outline`** (ou seu sinônimo **`Scenario Template`**): Esta palavra-chave permite que você execute o mesmo cenário múltiplas vezes com diferentes conjuntos de dados. É usada para criar cenários parametrizados. Os parâmetros são definidos usando `<placeholders>` dentro dos passos do `Scenario Outline`.

**`Examples`** (ou seu sinônimo **`Scenarios`**): Usada em conjunto com `Scenario Outline`, a palavra-chave `Examples` introduz uma tabela onde cada linha representa um conjunto de valores para os placeholders definidos no `Scenario Outline`. O cenário será executado uma vez para cada linha na tabela `Examples`.

```gherkin
Feature: Calculadora de Soma

  Scenario Outline: Somar dois números
	Given que eu tenho uma calculadora
	When eu insiro o número <numero1>
	And eu insiro o número <numero2>
	And eu pressiono o botão de somar
	Then o resultado exibido deve ser <resultado_esperado>

	Examples:
	  | numero1 | numero2 | resultado_esperado |
	  | 5       | 3       | 8                  |
	  | 10      | 7       | 17                 |
	  | -2      | 2       | 0                  |
	  | 0       | 0       | 0                  |
```

Neste caso, o cenário "Somar dois números" será executado quatro vezes, uma para cada linha da tabela `Examples`, substituindo `<numero1>`, `<numero2>` e `<resultado_esperado>` pelos valores correspondentes.

A tabela abaixo resume as principais palavras-chave do Gherkin:

|**Palavra-chave**|**Descrição**|
|---|---|
|`Feature`|Define uma funcionalidade ou recurso a ser testado.|
|`Rule` (a partir do Gherkin 6)|Descreve uma regra de negócio associada à funcionalidade.|
|`Example` (ou `Scenario`)|Representa um cenário de teste específico.|
|`Given`, `When`, `Then`, `And`, `But`|Passos para descrever o cenário de teste (contexto inicial, ação, resultado esperado e suas continuações).|
|`*`|Pode ser usado no lugar de `Given`, `When`, `Then`, `And` ou `But` para definir um passo.|
|`Background`|Fornece um contexto (passos) comum para todos os cenários em uma feature (ou rule).|
|`Scenario Outline` (ou `Scenario Template`)|Permite a criação de cenários parametrizados.|
|`Examples` (ou `Scenarios`)|Fornece instâncias de dados específicas para os cenários definidos em um `Scenario Outline`.|

Além dessas, existem palavras-chave secundárias que adicionam mais poder e clareza aos seus arquivos `.feature`:

**`"""` (Doc Strings / Textos Multilinha)**: Usada para passar um bloco de texto maior como argumento para um passo. É útil para dados de entrada como JSON, XML, ou qualquer texto que ocupe várias linhas.

```gherkin
Given que o corpo da requisição é:
  """
  {
	"nome": "Produto Exemplo",
	"preco": 19.99,
	"descricao": "Este é um produto de exemplo com uma descrição longa."
  }
  """
```

**`|` (Data Tables / Tabelas de Dados)**: Usada para passar uma tabela de dados como argumento para um passo. As tabelas são muito úteis para representar conjuntos de dados estruturados.

```gherkin
Given os seguintes usuários existem:
  | nome    | email             | perfil  |
  | Alice   | alice@exemplo.com | admin   |
  | Bob     | bob@exemplo.com   | usuario |
  | Charlie | char@exemplo.com  | editor  |
```
 
**`@` (Tags)**: Usada para "etiquetar" features ou cenários. Tags são úteis para organizar, filtrar e agrupar testes (por exemplo, `@smoke`, `@regressao`, `@wip`) ou para aplicar hooks específicos.

```gherkin
@login
Feature: Autenticação de Usuário

  @rapido @smoke
  Scenario: Login com sucesso
	# ...
```
    
**`#` (Comments / Comentários)**: Usada para adicionar comentários em qualquer linha de um arquivo `.feature`. Linhas iniciadas com `#` são ignoradas pelo Cucumber.

```gherkin
# Este é um comentário explicando o propósito da feature
Feature: Minha Funcionalidade
  # TODO: Adicionar mais cenários para cobrir casos de borda
  Scenario: Cenário principal
	Given algo acontece
```

Dominar essas palavras-chave é o primeiro passo para escrever especificações de comportamento eficazes e colaborativas com Cucumber e Gherkin. Lembre-se, cada linha em um arquivo `.feature` que não é uma linha em branco (ou um comentário) deve começar com uma dessas palavras-chave para que o Cucumber possa interpretá-la corretamente.

### Step Definitions: Conectando Gherkin ao Código

Agora que entendemos como escrever cenários em Gherkin, a próxima pergunta é: como o Cucumber executa esses passos em linguagem natural? A resposta está nos **Step Definitions** (Definições de Passo).

Um **Step Definition** é um trecho de código (geralmente um método em uma linguagem de programação como Java, Ruby, JavaScript, etc.) que está vinculado a um ou mais passos Gherkin através de uma expressão. Quando o Cucumber executa um passo Gherkin em um cenário, ele procura por um Step Definition correspondente que possa executar a lógica daquele passo. Essencialmente, os Step Definitions traduzem as frases em linguagem natural do Gherkin em ações concretas que seu código de automação pode realizar.

Vamos analisar um exemplo para entender melhor. Imagine que queremos testar a validação de tipos de dados.

Arquivo `.feature`:

```gherkin
Feature: Validação de valores

  Scenario: Validação de um número inteiro
    Given eu tenho uma variável chamada "numero"
    When eu atribuo "123" para "numero"
    Then a variável "numero" deve ser um inteiro
```

Vamos detalhar cada parte deste cenário Gherkin, como você mesmo sugeriu:

1. Feature (Validação de valores): Esta declaração inicializa a descrição da funcionalidade que estamos testando. No caso, nosso foco é a validação de diferentes tipos de valores.
2. Scenario (Validação de um número inteiro): Aqui começamos a descrever um cenário específico dentro da funcionalidade. Este cenário particular está relacionado à validação de um valor para garantir que ele seja um número inteiro.
3. Given eu tenho uma variável chamada "numero": O passo Given estabelece o cenário inicial, a pré-condição. Neste caso, estamos declarando que, para este teste, partimos do pressuposto de que existe uma variável chamada "numero" (ou um espaço reservado com esse nome em nosso contexto de teste).
4. When eu atribuo "123" para "numero": O passo When descreve a ação principal que estamos realizando. Aqui, estamos simulando a atribuição do valor da string "123" à nossa variável (ou conceito de variável) "numero". Este é o evento cujo resultado queremos testar.
5. Then a variável "numero" deve ser um inteiro: O passo Then representa a condição ou resultado esperado após a ação descrita no When. Aqui, estamos afirmando que, após atribuir "123" à variável "numero", esperamos que o sistema (ou a lógica de validação que estamos testando) reconheça ou confirme que o valor contido em "numero" é, de fato, um número inteiro.

Agora, vamos ver como seriam os Step Definitions correspondentes em Java (conceitualmente, pois a implementação exata dependeria do estado do teste e das ferramentas):

```java
// Importações necessárias do Cucumber
import io.cucumber.java.en.Given;
import io.cucumber.java.en.When;
import io.cucumber.java.en.Then;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.Assertions.fail;

import java.util.HashMap;
import java.util.Map;

public class ValidacaoStepDefinitions {

    // Usaremos um mapa simples para simular o armazenamento de variáveis neste exemplo
    private Map<String, Object> contextoDeTeste = new HashMap<>();

    @Given("eu tenho uma variável chamada {string}")
    public void eu_tenho_uma_variavel_chamada(String nomeVariavel) {
        // No mundo real, isso poderia preparar um campo em um objeto, etc.
        // Aqui, apenas garantimos que o nome é conhecido.
        contextoDeTeste.put(nomeVariavel, null); // Inicializa com null
        System.out.println("Dado que tenho uma variável chamada: " + nomeVariavel);
    }

    @When("eu atribuo {string} para {string}")
    public void eu_atribuo_para(String valor, String nomeVariavel) {
        // Armazena o valor (como string, por enquanto) no nosso contexto de teste
        contextoDeTeste.put(nomeVariavel, valor);
        System.out.println("Quando eu atribuo '" + valor + "' para '" + nomeVariavel + "'");
    }

    @Then("a variável {string} deve ser um inteiro")
    public void a_variavel_deve_ser_um_inteiro(String nomeVariavel) {
        Object valorObjeto = contextoDeTeste.get(nomeVariavel);
        System.out.println("Então a variável '" + nomeVariavel + "' (valor: " + valorObjeto + ") deve ser um inteiro");

        if (valorObjeto == null) {
            fail("A variável '" + nomeVariavel + "' não foi definida.");
        }

        // Tenta converter o valor para um inteiro.
        // Em um teste real, você estaria verificando o tipo de um objeto
        // ou o resultado de uma função de validação.
        try {
            Integer.parseInt(valorObjeto.toString());
            assertTrue(true, "O valor '" + valorObjeto + "' é um inteiro.");
        } catch (NumberFormatException e) {
            fail("O valor '" + valorObjeto + "' não é um inteiro válido.");
        }
    }
}
```

Neste exemplo de Step Definition:

- Usamos anotações como `@Given`, `@When`, `@Then` para marcar os métodos.
- A string dentro da anotação (ex: `"eu tenho uma variável chamada {string}"`) é uma **Expressão Cucumber**. A parte `{string}` captura o texto entre aspas no passo Gherkin e o passa como argumento para o método (ex: `String nomeVariavel`).
- Dentro do método, implementamos a lógica para simular a ação descrita pelo passo Gherkin. Para o `Then`, fazemos uma asserção para verificar o resultado esperado.

**Exemplo da Documentação: "Some cukes"**

Vamos agora analisar um exemplo que é frequentemente encontrado na documentação do Cucumber:

Passo Gherkin:

```gherkin
Scenario: Some cukes
  Given I have 48 cukes in my belly
```

A parte `"I have 48 cukes in my belly"` (Eu tenho 48 pepinos na minha barriga) do passo Gherkin corresponderá ao seguinte Step Definition em Java:

```java
package com.example;

import io.cucumber.java.en.Given;

public class StepDefinitions {
  @Given("I have {int} cukes in my belly")
  public void i_have_n_cukes_in_my_belly(int cukes) {
    // No nosso exemplo, 'cukes' receberá o valor 48.
    // Este método pode agora usar o valor 'cukes' para configurar o estado do teste
    // ou fazer asserções, se fosse um passo 'Then'.
    System.out.format("Cukes: %d\n", cukes); // A documentação original usa %n, mas %d é para inteiros
  }
}
```

Ou, utilizando lambdas do Java 8 (uma abordagem mais concisa):

```java
package com.example;

import io.cucumber.java8.En; // Import para a interface En do Cucumber-Java8

public class StepDefinitions implements En { // A classe implementa a interface En
  public StepDefinitions() {
    // Dentro do construtor, definimos os passos usando métodos da interface En
    Given("I have {int} cukes in my belly", (Integer cukes) -> {
      // A expressão lambda recebe o argumento 'cukes' (automaticamente convertido para Integer)
      System.out.format("Cukes: %d\n", cukes);
    });
  }
}
```

**Observações Importantes sobre Step Definitions:**

- **Expressões Cucumber vs. Expressões Regulares:** A expressão em um Step Definition pode ser uma Expressão Cucumber (como `{int}` ou `{string}`) ou uma Expressão Regular (RegExp) tradicional. As Expressões Cucumber são geralmente mais fáceis de ler e escrever para casos comuns. Se você usar Expressões Regulares, cada grupo de captura da correspondência da RegExp será passado como argumento para o método do Step Definition.
- **Transformação de Tipos de Parâmetros:** Quando uma Expressão Cucumber como `{int}` é usada, o Cucumber automaticamente tenta converter a string capturada (por exemplo, "48") para o tipo especificado no parâmetro do método (por exemplo, `int cukes`). O Cucumber possui tipos de parâmetros embutidos (como `{int}`, `{float}`, `{word}`, `{string}`) e também permite que você defina seus próprios tipos de parâmetros customizados. No exemplo acima, o argumento `cukes` será um inteiro porque o regex do tipo de parâmetro `{int}` embutido é `\d+` (que corresponde a um ou mais dígitos).
- **Transferência de Estado:** Um Step Definition pode precisar passar informações (estado) para um Step Definition subsequente no mesmo cenário. Isso é comumente feito armazenando o estado em variáveis de instância da classe que contém os Step Definitions. O Cucumber garante que uma nova instância dessa classe seja criada para cada cenário, mantendo o isolamento.
- **Escopo e Localização:** Os Step Definitions não estão vinculados a um arquivo `.feature` ou cenário específico. O nome do arquivo, da classe ou do pacote onde um Step Definition é definido não afeta quais passos Gherkin ele corresponderá. A única coisa que importa é a expressão do Step Definition e se ela corresponde ao texto do passo Gherkin.
- **Sugestões de Snippets:** Quando o Cucumber encontra um passo Gherkin em um cenário para o qual não existe um Step Definition correspondente, ele é muito prestativo! Ele imprimirá no console um trecho de código (snippet) do Step Definition com uma Expressão Cucumber que corresponderia àquele passo. Esse snippet pode ser usado como um ponto de partida para criar o novo Step Definition.

Por exemplo, se você tem o seguinte passo Gherkin:

```gherkin
Given I have 3 red balls
```

E não há um Step Definition correspondente, o Cucumber poderia sugerir:

```java
@Given("I have {int} red balls")
public void i_have_red_balls(int int1) {
	// Escreva o código aqui para transformar o passo em ações concretas
	throw new io.cucumber.java.PendingException();
}
```

Se você tiver tipos de parâmetros customizados registrados que correspondam a partes do seu passo indefinido, o Cucumber os utilizará. Por exemplo, se existisse um tipo de parâmetro `color` registrado:

```java
@Given("I have {int} {color} balls")
public void i_have_color_balls(Integer int1, Color color) { // Supondo uma classe Color
	// Escreva o código aqui
	throw new io.cucumber.java.PendingException();
}
```

Esses snippets são uma grande ajuda para acelerar o desenvolvimento dos Step Definitions.

### Argumentos dos Passos (Step Arguments)

Como vimos nos exemplos de Step Definitions, o Cucumber é capaz de extrair partes do texto de um passo Gherkin e passá-las como argumentos para os métodos dos Step Definitions. No exemplo `Given I have 48 cukes in my belly`, o Cucumber extraiu o texto "48", converteu-o para um valor inteiro (graças à Expressão Cucumber `{int}`) e o passou como argumento para o método `i_have_n_cukes_in_my_belly(int cukes)`.

É crucial que o **número de parâmetros no método do Step Definition corresponda exatamente ao número de grupos de captura na expressão** (seja ela uma Expressão Cucumber ou uma Expressão Regular). Se houver uma divergência – por exemplo, a expressão captura dois valores, mas o método espera apenas um, ou vice-versa – o Cucumber gerará um erro durante a execução, pois não saberá como mapear corretamente os valores capturados para os parâmetros do método.

Esse processo de extração e passagem de argumentos é fundamental para a flexibilidade e reusabilidade dos Step Definitions. Ele permite que você escreva passos Gherkin mais dinâmicos e evite ter que criar um Step Definition separado para cada variação numérica ou textual pequena em seus cenários.

### Tabelas de Dados (Data Tables)

Muitas vezes, um passo Gherkin precisa lidar com um conjunto maior de dados estruturados. Para esses casos, o Gherkin oferece as **Tabelas de Dados (Data Tables)**. Uma Data Table é declarada logo abaixo de um passo Gherkin, usando barras verticais `|` para delimitar as colunas e linhas.

As Data Tables podem ser acessadas no seu Step Definition através de um objeto `io.cucumber.datatable.DataTable` como o último parâmetro do método. O Cucumber oferece várias maneiras de converter essa `DataTable` em coleções Java mais familiares, facilitando o trabalho com os dados. A forma como a tabela Gherkin é estruturada e como você declara o tipo do parâmetro no seu Step Definition influencia o tipo de coleção resultante.

As conversões mais comuns são:

**`List<List<String>>`**: Esta é a representação mais básica. Cada linha da tabela Gherkin (incluindo a linha de cabeçalho, se houver) se torna uma `List<String>`, e todas essas listas de linhas são agrupadas em uma `List` principal. Isso ocorre, por exemplo, quando a tabela é processada como uma grade de células, sem uma interpretação específica de cabeçalhos para mapear para objetos.
 
- Gherkin:

```gherkin
Given a tabela de dados:
  | Celula1A | Celula1B |
  | Celula2A | Celula2B |
```

- Step Definition (Java):

```java
@Given("a tabela de dados:")
public void a_tabela_de_dados(DataTable dataTable) {
	List<List<String>> tableAsLists = dataTable.asLists(String.class);
	// tableAsLists.get(0).get(0) conteria "Celula1A"
	// tableAsLists.get(1).get(0) conteria "Celula2A"
}
```

**`List<Map<String, String>>`**: Esta é uma conversão muito comum e útil. A primeira linha da Data Table é tratada como o cabeçalho (chaves do mapa). Cada linha subsequente da tabela se torna um `Map<String, String>`, onde as chaves são os nomes das colunas (do cabeçalho) e os valores são as células daquela linha. Toda a tabela se torna uma lista desses mapas.
    
- Gherkin:

```gherkin
Given os seguintes usuários:
  | nome  | email             |
  | Alice | alice@exemplo.com |
  | Bob   | bob@exemplo.com   |
```

- Step Definition (Java):

```java
@Given("os seguintes usuários:")
public void os_seguintes_usuarios(DataTable dataTable) {
	List<Map<String, String>> users = dataTable.asMaps(String.class, String.class);
	for (Map<String, String> user : users) {
		System.out.println("Nome: " + user.get("nome") + ", Email: " + user.get("email"));
	}
	// Saída para a primeira iteração: Nome: Alice, Email: alice@exemplo.com
}
```

Esta estrutura (`List<Map<String, String>>`) é particularmente útil quando a tabela Gherkin é organizada como uma lista de registros, onde cada linha representa um item e as colunas representam os atributos desse item.

**`Map<String, String>`**: Se a Data Table tiver exatamente duas colunas, e a primeira coluna for interpretada como chaves e a segunda como valores, ela pode ser convertida diretamente em um `Map<String, String>`. Cada linha da tabela contribui com uma entrada para o mapa.

- Gherkin:

```gherkin
Given os seguintes detalhes de configuração:
  | chave       | valor          |
  | url_base    | http://app.com |
  | timeout_api | 5000           |
  | ambiente    | producao       |
```

- Step Definition (Java):

```java
@Given("os seguintes detalhes de configuração:")
public void os_seguintes_detalhes_de_configuracao(DataTable dataTable) {
	Map<String, String> config = dataTable.asMap(String.class, String.class);
	System.out.println("URL Base: " + config.get("url_base")); // Saída: URL Base: http://app.com
	System.out.println("Timeout: " + config.get("timeout_api")); // Saída: Timeout: 5000
}
```

Esta estrutura (`Map<String, String>`) é ideal quando a tabela Gherkin representa uma coleção de pares chave-valor.

**`Map<String, List<String>>`**: Essa estrutura representa um mapa onde as chaves são strings (geralmente os cabeçalhos das colunas) e os valores são listas de strings (todos os valores naquela coluna, excluindo o cabeçalho). Isso pode ser útil se você quiser processar os dados por coluna.

- A conversão direta para este formato pode exigir um processamento manual da `List<List<String>>` ou `List<Map<String,String>>`, ou o uso de transformadores de tabela mais avançados, dependendo da versão do Cucumber e das bibliotecas de suporte. O mais comum é obter `List<Map<String, String>>` e depois processá-la para obter dados por coluna, se necessário.
- **`Map<String, Map<String, String>>`**: Esta é uma estrutura mais complexa, representando um mapa aninhado. A primeira coluna da tabela Gherkin poderia ser usada como chaves para o mapa externo. As colunas restantes, para cada linha, formariam os mapas internos. Essa estrutura é útil quando a tabela Gherkin é modelada para incluir uma lista de "registros", onde cada registro é ele mesmo uma coleção de pares chave-valor, e você quer acessar esses registros por uma chave primária da primeira coluna.

```gherkin
Given as configurações por ambiente:
  | ambiente  | url              | user   |
  | dev       | http://dev.serv  | dev_usr|
  | qa        | http://qa.serv   | qa_usr |
```
  
- Step Definition (Java) - para obter `Map<String, Map<String, String>>` geralmente requer uma transformação customizada ou o uso de `DataTable.asMaps()` e depois um processamento adicional para agrupar por uma chave. Uma conversão direta para `Map<String, Map<String, String>>` não é tão trivial quanto as outras com os métodos `asX()` padrão para todos os casos. Uma forma comum de lidar com isso seria converter para `List<Map<String, String>>` e então construir o mapa aninhado programaticamente.

**Exemplo de Passagem de `List<String>`**

Uma maneira simples de passar uma `List<String>` para uma definição de passo é através do uso de uma Data Table com uma única coluna, como exemplificado a seguir:

Gherkin:

```gherkin
Given the following animals:
  | cow   |
  | horse |
  | sheep |
```

Step Definition (Java):

Se você declarar o argumento no método do Step Definition como `List<String>` (e a expressão do passo não tiver grupos de captura para os itens da lista), o Cucumber pode automaticamente achatar a DataTable de uma única coluna em uma `List<String>`.

```java
@Given("the following animals:")
public void the_following_animals(List<String> animals) {
    // 'animals' será uma lista contendo "cow", "horse", "sheep"
    for (String animal : animals) {
        System.out.println(animal);
    }
}
```

Nesse caso, o Cucumber utiliza internamente algo como `dataTable.asList(String.class)` (ou um método similar apropriado para achatar uma coluna única) antes de invocar a definição do passo.

Além de coleções de `String`, o Cucumber, através de seus conversores embutidos ou com a ajuda de transformadores customizados, suporta a conversão de células de Data Tables para outros tipos primitivos e objetos comuns, como `Integer`, `Float`, `Double`, `Boolean`, `BigInteger`, `BigDecimal`, `Byte`, `Short`, `Long`, e até mesmo tipos customizados que você definir. Essa flexibilidade é crucial para lidar com diversos tipos de dados de entrada em seus testes automatizados.

### Passos (Steps) em Detalhe

Como já mencionamos, um **Step** (Passo) dentro do contexto do Cucumber pode ser comparado a uma chamada de método ou invocação de função em uma linguagem de programação tradicional. Quando você escreve uma linha em Gherkin como:

```gherkin
Given I have 93 cucumbers in my belly
```

Você está, na verdade, "invocando" a definição de passo (Step Definition) que corresponde a essa frase, passando o valor `93` como um argumento. Os Steps são declarados nos seus arquivos `.feature`, que são os arquivos de texto simples onde você especifica o comportamento desejado para os seus testes usando a sintaxe Gherkin.

Cada passo em um cenário Gherkin descreve uma ação específica, uma condição prévia ou um resultado esperado que deverá ser verificado durante a execução do teste. Eles fornecem a estrutura em linguagem natural que descreve as interações do usuário com o sistema ou o estado do sistema em diferentes momentos.

Ao escrever os Steps em seus arquivos `.feature`, é absolutamente essencial garantir que eles estejam precisamente alinhados com as expressões definidas nos seus Step Definitions correspondentes no código de automação. Essa correlação é a espinha dorsal do Cucumber:

- O texto do Step no arquivo `.feature` deve casar com a expressão (Expressão Cucumber ou Expressão Regular) de um Step Definition.
- Os argumentos (como números ou strings entre aspas) no Step devem ser capturáveis pela expressão e corresponder aos parâmetros esperados pelo método do Step Definition.

Se essa correspondência não existir ou estiver incorreta (por exemplo, um Step no Gherkin não tem um Step Definition correspondente, ou os tipos de argumentos não batem), o Cucumber indicará um erro ou um estado de "indefinido" ou "pendente", impedindo que o teste seja executado completamente.

Portanto, os Steps servem como a ligação direta entre a especificação de comportamento em linguagem natural e a lógica de automação subjacente, proporcionando uma maneira clara, estruturada e colaborativa de expressar e verificar o comportamento desejado do sistema.

### Correspondência de Passos (Matching Steps)

O processo de **Matching steps** (Correspondência de Passos) é fundamental para o funcionamento do Cucumber. Ele descreve como o Cucumber associa um passo Gherkin, escrito em seu arquivo `.feature`, a uma expressão (seja uma Expressão Cucumber ou uma Expressão Regular) definida em um dos seus Step Definitions no código de automação.

Ao escrever seus Step Definitions, é importante lembrar que eles são carregados e "registrados" pelo Cucumber antes do início da execução de qualquer cenário do arquivo `.feature`. Isso significa que, no momento em que o Cucumber começa a processar os passos de um cenário, todas as definições de passos já estão disponíveis e prontas para serem associadas.

Durante a execução de um cenário, para cada passo Gherkin encontrado, o Cucumber percorre sua lista de Step Definitions registrados e procura por um cuja expressão corresponda ao texto completo do passo atual.

- Se uma correspondência exata for encontrada, o Cucumber executa o método associado àquele Step Definition.
- Durante essa correspondência, se a expressão contiver grupos de captura (por exemplo, `{int}` em uma Expressão Cucumber ou `(\d+)` em uma Expressão Regular), os valores capturados do texto do passo Gherkin são extraídos, possivelmente transformados para o tipo de dado esperado (como de string "48" para o inteiro 48), e então passados como argumentos para o método do Step Definition.

É crucial entender que o uso das palavras-chave Gherkin específicas como `Given`, `When`, `Then`, `And`, ou `But` no início dos passos em seu arquivo `.feature` **não influencia a lógica de correspondência** do Cucumber durante a fase de busca por um Step Definition. Ou seja, um Step Definition anotado com `@Given("alguma coisa")` pode, tecnicamente, corresponder a um passo Gherkin que comece com `When alguma coisa`, desde que o texto "alguma coisa" seja o mesmo.

No entanto, a escolha dessas palavras-chave é vital para a **semântica e clareza** do cenário. Elas ajudam a comunicar a intenção de cada passo:

- `Given`: Estabelece um estado inicial, um contexto.
- `When`: Descreve uma ação ou evento.
- `Then`: Descreve um resultado esperado ou uma verificação.
- `And`, `But`: Continuam o tipo de passo anterior.

Embora o Cucumber não use essas palavras-chave para o _matching_ técnico, você deve usá-las em seus Step Definitions (`@Given`, `@When`, `@Then`) para manter a mesma clareza e semântica que você tem em seus arquivos `.feature`. Isso torna seus Step Definitions mais fáceis de entender e mapear de volta para os cenários Gherkin.

### Resultados dos Passos (Step Results)

Após a execução de cada passo em um cenário pelo Cucumber, ele recebe um status que indica o resultado dessa execução. Compreender esses diferentes status é essencial para analisar os relatórios de teste e identificar problemas. Cada passo pode resultar em uma das seguintes categorias:

1. **`Success` (Sucesso)**:
    - **O que significa:** O Cucumber encontrou um Step Definition correspondente para o passo Gherkin, e o método daquele Step Definition foi executado completamente sem lançar nenhuma exceção (erro).
    - **Indicação visual comum:** Verde.
    - **Nota:** O valor retornado por um método de Step Definition (se houver) não tem importância para o status de sucesso. `return true;` ou `return false;` ou `return null;` não farão o passo falhar por si só. A falha ocorre devido a exceções não tratadas.

2. **`Undefined` (Indefinido)**:
    - **O que significa:** O Cucumber não conseguiu encontrar nenhum Step Definition registrado cuja expressão correspondesse ao texto do passo Gherkin.
    - **Indicação visual comum:** Amarelo.
    - **Consequência:** Todos os passos subsequentes no mesmo cenário serão automaticamente marcados como `Skipped` (Pulados) e não serão executados. O Cucumber geralmente fornece um snippet de código para ajudar a criar o Step Definition ausente.

3. **`Pending` (Pendente)**:
    - **O que significa:** Um Step Definition correspondente foi encontrado e executado, mas dentro do método desse Step Definition, foi explicitamente invocado um método que sinaliza uma exceção de pendência (por exemplo, `throw new io.cucumber.java.PendingException();` em Java). Isso indica que a automação para aquele passo ainda não foi implementada.
    - **Indicação visual comum:** Amarelo (semelhante ao Indefinido).
    - **Consequência:** Assim como no `Undefined`, todos os passos subsequentes no mesmo cenário são pulados.

4. **`Failed` (Falhou)**:
    - **O que significa:** Um Step Definition correspondente foi encontrado e executado, mas durante sua execução, uma exceção (erro) não esperada foi lançada. Isso geralmente acontece devido a uma asserção que falhou (por exemplo, `assertEquals(5, resultado)` onde `resultado` era `3`) ou qualquer outro erro de tempo de execução no código do Step Definition.
    - **Indicação visual comum:** Vermelho.
    - **Consequência:** Todos os passos subsequentes no mesmo cenário são pulados.

5. **`Skipped` (Pulado)**:
    - **O que significa:** O passo não foi executado porque um passo anterior no mesmo cenário resultou em `Undefined`, `Pending` ou `Failed`.
    - **Indicação visual comum:** Ciano ou Azul claro.
    - **Nota:** Mesmo que exista um Step Definition correspondente perfeitamente válido para um passo pulado, ele não será invocado.

6. **`Ambiguous` (Ambíguo)**:
    - **O que significa:** O Cucumber encontrou **dois ou mais** Step Definitions registrados cujas expressões correspondem ao mesmo passo Gherkin. O Cucumber não consegue decidir qual deles executar, pois as definições de passos precisam ser únicas em termos de correspondência.
    - **Consequência:** O Cucumber geralmente lança uma exceção (como `AmbiguousStepDefinitionsException`) e interrompe a execução, indicando que a ambiguidade precisa ser resolvida. Isso pode exigir o refinamento das expressões nos Step Definitions para torná-las mais específicas.

Compreender esses resultados ajuda as equipes a diagnosticar rapidamente o estado de seus testes e a priorizar o trabalho de desenvolvimento ou correção.

### Hooks: Gerenciando o Ciclo de Vida da Execução

No Cucumber, os **Hooks** são blocos de código que podem ser configurados para executar em pontos específicos do ciclo de vida da execução dos testes. Eles são extremamente úteis para realizar tarefas de configuração (setup) antes que os testes comecem e tarefas de limpeza (teardown) depois que os testes terminam. Pense neles como interceptadores que permitem que você execute lógica personalizada em momentos chave.

Geralmente, os hooks são utilizados para:

- Configurar o ambiente de teste (ex: iniciar um navegador, conectar a um banco de dados, preparar dados de teste).
- Limpar o ambiente de teste após a execução (ex: fechar o navegador, reverter transações do banco de dados, deletar dados de teste temporários).

Uma característica importante é que o local onde um hook é definido no seu código (em qual classe ou arquivo) geralmente não afeta quais cenários ou passos ele irá executar. O Cucumber descobre e registra todos os hooks definidos no classpath configurado. Se você precisar de um controle mais granular sobre quando um hook específico deve ser executado, pode-se utilizar **hooks condicionais**, que são associados a tags.

Os hooks podem ser declarados em qualquer classe que o Cucumber escaneie para Step Definitions e hooks. Isso oferece flexibilidade para organizar seu código de setup e teardown de forma lógica, talvez separando hooks relacionados a diferentes aspectos do seu sistema (por exemplo, hooks de UI em uma classe, hooks de API em outra).

Essencialmente, os hooks fornecem uma maneira sistemática e robusta de preparar e limpar o ambiente de teste, garantindo que cada cenário ou passo seja executado sob as condições necessárias e que os testes sejam consistentes e repetíveis.

Existem diferentes tipos de hooks, cada um com um escopo e um momento de execução específicos:

#### Hooks de Cenário (Scenario Hooks)

Os **Scenario Hooks** são blocos de código que são executados em relação a cada cenário individual. Eles permitem realizar ações específicas _antes_ do início de cada cenário e _depois_ da conclusão de cada cenário.

- `@Before` (Antes do Cenário):
    
    Os Before Hooks são executados antes do primeiro passo de cada cenário. Isso significa que, antes que qualquer ação descrita nos passos do cenário comece, você pode usar um @Before hook para configurar o ambiente de teste de acordo com as necessidades específicas daquele cenário. Por exemplo, você pode querer limpar cookies do navegador, resetar o estado de um serviço mockado, ou garantir que certos dados de teste estejam presentes.
    
    **Estilo de método anotado (Java):**
	
    ```java
    import io.cucumber.java.Before;
    
    public class MyScenarioHooks {
        @Before
        public void doSomethingBeforeEachScenario() {
            // Lógica a ser executada antes de cada cenário
            System.out.println("--- Executando @Before Hook ---");
        }
    }
    ```
    
    **Estilo Lambda (Java 8+):**
    
    ```java
    import io.cucumber.java8.En;
    
    public class MyScenarioHooksLambda implements En {
        public MyScenarioHooksLambda() {
            Before(() -> {
                // Lógica a ser executada antes de cada cenário
                System.out.println("--- Executando @Before Hook (Lambda) ---");
            });
        }
    }
    ```
    
    **Ordem dos Hooks:** Se você tiver múltiplos `@Before` hooks, pode especificar uma ordem explícita para sua execução usando o atributo `order`. Hooks com valores de `order` menores são executados primeiro. O valor padrão para `order` é 10000.
    
    **Estilo de método anotado com ordem:**
    
    ```java
    @Before(order = 10)
    public void doFirstThingBeforeScenario() {
        System.out.println("--- @Before (order = 10) ---");
    }
    
    @Before(order = 20)
    public void doSecondThingBeforeScenario() {
        System.out.println("--- @Before (order = 20) ---");
    }
    ```
    
    **Estilo Lambda com ordem:**
	
    ```java
    public class MyScenarioHooksLambdaOrder implements En {
        public MyScenarioHooksLambdaOrder() {
            Before(10, () -> {
                System.out.println("--- @Before (order = 10, Lambda) ---");
            });
    
            Before(20, () -> {
                System.out.println("--- @Before (order = 20, Lambda) ---");
            });
        }
    }
    ```
    
- `@After` (Depois do Cenário):
    
    Os After Hooks são executados após o último passo de cada cenário, independentemente do resultado do cenário (seja ele Success, Failed, Pending, etc.). Eles são ideais para realizar tarefas de limpeza, como fechar conexões, deletar dados temporários, ou, como mencionado nas suas notas, tirar screenshots em caso de falha.
    
    **Estilo de método anotado (Java):**
	
    ```java
    import io.cucumber.java.After;
    import io.cucumber.java.Scenario; // Para acessar informações do cenário
    
    public class MyScenarioHooks {
        @After
        public void doSomethingAfterEachScenario(Scenario scenario) {
            System.out.println("--- Executando @After Hook ---");
            if (scenario.isFailed()) {
                // Lógica para lidar com cenários falhos, como tirar um screenshot
                System.out.println("Cenário FALHOU: " + scenario.getName());
                // byte[] screenshot = ((TakesScreenshot) webDriver).getScreenshotAs(OutputType.BYTES);
                // scenario.attach(screenshot, "image/png", "screenshot_falha");
            }
        }
    }
    ```
    
    **Estilo Lambda (Java 8+):**
	
    ```java
    import io.cucumber.java8.En;
    import io.cucumber.java.Scenario;
    
    public class MyScenarioHooksLambda implements En {
        public MyScenarioHooksLambda() {
            After((Scenario scenario) -> {
                System.out.println("--- Executando @After Hook (Lambda) ---");
                if (scenario.isFailed()) {
                    System.out.println("Cenário (Lambda) FALHOU: " + scenario.getName());
                }
            });
        }
    }
    ```
    
    O parâmetro `Scenario` no método do hook `@After` é opcional. Se você o incluir, terá acesso a informações sobre o cenário que acabou de ser executado, como seu nome, status (se falhou, passou, etc.), ID, e a capacidade de anexar dados (como screenshots ou logs) aos relatórios do Cucumber. Isso é particularmente útil para diagnósticos, como capturar uma imagem da tela do navegador no momento exato em que um teste de UI falha.
    
    Assim como os `@Before` hooks, os `@After` hooks também podem ter uma ordem definida com o atributo `order`. Para `@After` hooks, aqueles com valores de `order` _menores_ são executados _primeiro_ (o que pode parecer contraintuitivo, mas significa que um `@After(order = 10)` roda antes de um `@After(order = 20)`). No entanto, uma prática comum é que hooks com ordem maior são para limpezas mais "externas". A documentação oficial ou experimentação é a melhor forma de confirmar a ordem exata de execução para `@After` hooks com `order`. A intenção é que a limpeza ocorra na ordem inversa da configuração.

Os Scenario Hooks fornecem uma maneira estruturada e flexível de gerenciar a configuração e a limpeza para cada cenário, garantindo que o ambiente de teste seja consistente e que os testes sejam o mais independentes possível.

#### Hooks de Passo (Step Hooks)

Os **Step Hooks** são acionados em relação a cada passo individual dentro de um cenário. Eles são executados **antes** e **depois** de cada passo. Esses ganchos seguem uma lógica de invocação que pode ser chamada de "invoke around": se um hook `@BeforeStep` para um determinado passo for executado, o hook `@AfterStep` correspondente também será executado para aquele mesmo passo, independentemente do resultado do passo em si (sucesso, falha, etc.).

É importante notar uma particularidade: se um passo falhar (ou resultar em `Undefined` ou `Pending`), os _passos subsequentes_ e seus respectivos Step Hooks (`@BeforeStep` e `@AfterStep`) serão pulados. No entanto, o `@AfterStep` do passo que efetivamente falhou ainda será executado.

- `@BeforeStep` (Antes de Cada Passo):
    
    O @BeforeStep hook é executado antes de cada passo em um cenário. Ele é útil para realizar configurações ou ações muito granulares que precisam acontecer imediatamente antes de cada ação ou verificação do cenário.
    
    **Estilo de método anotado (Java):**
    
    ```java
    import io.cucumber.java.BeforeStep;
    import io.cucumber.java.Scenario;
    
    public class MyStepHooks {
        @BeforeStep
        public void doSomethingBeforeEachStep(Scenario scenario) {
            System.out.println("--- Executando @BeforeStep para o passo: " + scenario.getLine() + " ---");
            // 'scenario.getLine()' pode não dar o texto do passo, mas informações de localização.
            // Para obter o texto do passo, pode ser necessário inspecionar o objeto Scenario de forma mais aprofundada
            // ou usar outras APIs do Cucumber, dependendo da versão.
        }
    }
    ```
    
    **Estilo Lambda (Java 8+):**
	
    ```java
    import io.cucumber.java8.En;
    import io.cucumber.java.Scenario;
    
    public class MyStepHooksLambda implements En {
        public MyStepHooksLambda() {
            BeforeStep((Scenario scenario) -> {
                System.out.println("--- Executando @BeforeStep (Lambda) ---");
            });
        }
    }
    ```
    
- `@AfterStep `(Depois de Cada Passo):
    
    O @AfterStep hook é executado após cada passo em um cenário, mesmo que o passo tenha falhado. Pode ser usado para logs detalhados após cada ação, pequenas limpezas, ou até mesmo para tirar screenshots após cada passo durante uma sessão de depuração.
    
    **Estilo de método anotado (Java):**
    
    ```java
    import io.cucumber.java.AfterStep;
    import io.cucumber.java.Scenario;
    
    public class MyStepHooks {
        @AfterStep
        public void doSomethingAfterEachStep(Scenario scenario) {
            System.out.println("--- Executando @AfterStep. Status do cenário: " + scenario.getStatus() + " ---");
            // Poderia, por exemplo, verificar se o último passo falhou e registrar informações adicionais.
        }
    }
    ```
    
    **Estilo Lambda (Java 8+):**
	
    ```java
    import io.cucumber.java8.En;
    import io.cucumber.java.Scenario;
    
    public class MyStepHooksLambda implements En {
        public MyStepHooksLambda() {
            AfterStep((Scenario scenario) -> {
                System.out.println("--- Executando @AfterStep (Lambda). Status do cenário: " + scenario.getStatus() + " ---");
            });
        }
    }
    ```
    

Os Step Hooks (`@BeforeStep` e `@AfterStep`) oferecem um nível de controle muito fino sobre o ciclo de execução, permitindo intervenções precisas entre cada ação do seu cenário de teste. Eles podem ser úteis para depuração, logging intensivo, ou para interagir com ferramentas que monitoram a execução passo a passo.

#### Hooks Condicionais (Conditional Hooks)

Os Hooks no Cucumber, sejam eles de Cenário (`@Before`, `@After`) ou de Passo (`@BeforeStep`, `@AfterStep`), podem ser executados condicionalmente com base nas **tags** associadas aos cenários. Isso proporciona uma flexibilidade enorme, permitindo que você aplique lógicas de setup e teardown específicas apenas para um subconjunto de seus cenários.

Para tornar um hook condicional, você associa a ele uma **expressão de tag**. Se um cenário (ou a feature que o contém) possuir tags que correspondam à expressão definida no hook, então esse hook será executado para aquele cenário. Caso contrário, o hook será ignorado para aquele cenário.

A expressão de tag é fornecida como um argumento para a anotação do hook.

Exemplo (Java):

Suponha que você tenha alguns cenários que interagem diretamente com um navegador (@browser) e outros que são testes de API e não precisam de navegador. Alguns dos testes de navegador podem precisar ser executados com a interface gráfica visível, enquanto outros podem rodar em modo "headless" (@headless). Você pode querer uma lógica de teardown específica para os testes de navegador que não são headless.

```java
import io.cucumber.java.After;
import io.cucumber.java.Scenario;

public class ConditionalHooks {

    // Este hook @After só será executado para cenários que
    // possuem a tag @browser E NÃO possuem a tag @headless.
    @After("@browser and not @headless")
    public void closeBrowserIfNotHeadless(Scenario scenario) {
        System.out.println("--- Executando @After condicional para @browser and not @headless ---");
        System.out.println("Cenário: " + scenario.getName());
        // Lógica para fechar o navegador, por exemplo.
        // if (webDriver != null) {
        //     webDriver.quit();
        // }
    }

    @After("@api_test")
    public void cleanupApiTestData(Scenario scenario) {
        System.out.println("--- Executando @After condicional para @api_test ---");
        // Lógica para limpar dados criados por um teste de API.
    }
}
```

No arquivo `.feature`:

```gherkin
Feature: Testes Diversos

  @browser
  Scenario: Teste de UI com navegador visível
    Given estou na página inicial
    When realizo uma busca
    Then vejo os resultados

  @browser @headless
  Scenario: Teste de UI em modo headless
    Given estou na página de checkout
    When preencho os dados de pagamento
    Then a compra é confirmada

  @api_test
  Scenario: Teste de API de criação de usuário
    Given tenho os dados de um novo usuário
    When envio uma requisição POST para /users
    Then a resposta deve ser 201 Created
```

No exemplo acima:

- O hook `closeBrowserIfNotHeadless` será executado para o "Teste de UI com navegador visível" (pois tem `@browser` e não tem `@headless`).
- Ele **não** será executado para o "Teste de UI em modo headless" (pois, embora tenha `@browser`, também tem `@headless`, e a condição é `not @headless`).
- Ele **não** será executado para o "Teste de API de criação de usuário" (pois não tem a tag `@browser`).
- O hook `cleanupApiTestData` será executado apenas para o "Teste de API de criação de usuário".

Os hooks condicionais são uma ferramenta poderosa para gerenciar configurações complexas e específicas, garantindo que a lógica de setup e teardown seja aplicada apenas onde é realmente necessária, tornando seus testes mais eficientes e organizados.

#### Hooks Globais (Global Hooks)

Os **Global Hooks**, como o nome sugere, são blocos de código que têm um escopo que abrange toda a execução dos testes Cucumber. Eles são executados uma única vez: um tipo antes que _qualquer_ cenário seja iniciado, e outro tipo depois que _todos_ os cenários tenham sido concluídos.

Pense nos Global Hooks como os preparativos iniciais e a limpeza final de um grande evento. Antes de qualquer convidado (cenário) chegar, você prepara o salão (ambiente de teste global). Depois que todos os convidados foram embora, você limpa tudo.

Eles são diferentes dos Scenario Hooks (`@Before`/`@After`), que rodam para cada cenário individualmente. Os Global Hooks são para tarefas que se aplicam ao conjunto de testes como um todo.

**Casos de uso comuns para Global Hooks:**

- Iniciar serviços externos necessários para todos os testes (ex: um servidor mock, um proxy).
- Configurar uma conexão de banco de dados global que será usada por muitos testes.
- Gerar um relatório de teste consolidado no final de toda a execução.
- Parar serviços externos que foram iniciados pelo hook global de setup.

No Cucumber para Java, os Global Hooks são implementados usando as anotações `@BeforeAll` e `@AfterAll`. É importante notar que os métodos anotados com `@BeforeAll` e `@AfterAll` **devem ser estáticos**.

- `@BeforeAll`:
    
    O hook `@BeforeAll` é executado uma única vez, antes que qualquer cenário comece a ser executado.
    
    **Estilo de método anotado (Java):**
	
    ```java
    import io.cucumber.java.BeforeAll;
    
    public class GlobalSetupHook {
        @BeforeAll
        public static void setupGlobalEnvironment() {
            // Lógica a ser executada uma vez antes de todos os cenários
            System.out.println("========= EXECUTANDO @BeforeAll: Configuração Global INICIADA =========");
            // Ex: Iniciar um servidor mock, conectar a um sistema de mensageria, etc.
        }
    }
    ```
    
- `@AfterAll`:
    
    O hook `@AfterAll` é executado uma única vez, depois que todos os cenários foram executados, independentemente de seus resultados.
    
    **Estilo de método anotado (Java):**
	
    ```java
    import io.cucumber.java.AfterAll;
    
    public class GlobalTeardownHook {
        @AfterAll
        public static void teardownGlobalEnvironment() {
            // Lógica a ser executada uma vez após todos os cenários
            System.out.println("========= EXECUTANDO @AfterAll: Limpeza Global CONCLUÍDA =========");
            // Ex: Parar o servidor mock, fechar conexões globais, gerar relatório final.
        }
    }
    ```

Se você tiver múltiplos métodos `@BeforeAll` ou `@AfterAll` (o que geralmente não é recomendado para evitar complexidade, mas é possível), a ordem de execução não é garantida por padrão, a menos que a implementação específica do Cucumber que você está usando forneça um mecanismo de ordenação para eles (o que é menos comum para hooks globais do que para hooks de cenário).

Os Global Hooks são essenciais para gerenciar recursos que transcendem cenários individuais, garantindo que o ambiente de teste geral esteja corretamente preparado no início e adequadamente limpo no final da suíte de testes.

### Tags: Organizando e Filtrando Testes

As **Tags** no Cucumber são uma forma poderosa e flexível de organizar, categorizar e gerenciar seus arquivos de `Feature` e os `Scenarios` (ou `Examples` dentro de `Scenario Outlines`). Pense nelas como etiquetas ou rótulos que você pode anexar a diferentes partes de suas especificações Gherkin.

As tags são denotadas pelo símbolo `@` seguido por um nome de tag (sem espaços). Por exemplo: `@smoke`, `@regressao`, `@wip` (Work In Progress), `@JIRA-123`, `@frontend`.

Existem duas finalidades principais e interligadas para o uso de Tags:

1. **Executar um Subconjunto Específico de Cenários:** Esta é uma das utilidades mais comuns das tags. Ao executar seus testes Cucumber, você pode especificar quais tags incluir ou excluir. Isso permite que você rode apenas os cenários relevantes para uma determinada situação.
    - Por exemplo, você pode ter um conjunto de testes rápidos de "smoke test" (`@smoke`) que você executa com frequência para verificar a saúde básica da aplicação.
    - Você pode ter um conjunto maior de testes de regressão (`@regressao`) que rodam com menos frequência.
    - Desenvolvedores podem marcar cenários em que estão trabalhando ativamente com `@wip` para executá-los isoladamente.

2. **Restringir a Execução de Hooks a um Subconjunto Específico de Cenários:** Como vimos na seção sobre Hooks Condicionais, as tags são o mecanismo pelo qual você pode fazer com que hooks `@Before`, `@After`, `@BeforeStep`, ou `@AfterStep` sejam executados apenas para cenários que possuam (ou não possuam) certas tags. Isso permite uma personalização muito refinada do ciclo de vida dos testes.

**Como usar Tags:**

Você pode colocar tags em diferentes níveis na sua estrutura Gherkin:

- Acima de uma `Feature`:

```gherkin
@billing @module_financeiro
Feature: Verify billing
  # Todos os cenários nesta feature herdarão as tags @billing e @module_financeiro
```
  
- Acima de um `Scenario` ou `Scenario Outline`:

```gherkin
@billing
Feature: Verify billing

  @important @smoke
  Scenario: Missing product description
	Given hello

  Scenario: Several products
	Given hello
```

No exemplo acima, o cenário "Missing product description" terá as tags `@billing` (herdada da feature), `@important` e `@smoke`. O cenário "Several products" terá apenas a tag `@billing`.

Um recurso ou cenário pode ter quantas tags você desejar. Basta separá-las com espaços:

```gherkin
@critico @pagamento @JIRA-456 @release_v2
Scenario: Processar pagamento com cartão de crédito
  # ...
```

#### Herança de Tags (Tag Inheritance)

As tags aplicadas a um elemento de nível superior são herdadas pelos seus elementos filhos. Isso simplifica a aplicação de tags comuns a múltiplos cenários:

- Tags colocadas acima de uma **`Feature`** serão herdadas por todos os `Scenario`, `Scenario Outline` e `Rule` dentro daquela feature.
- Tags colocadas acima de uma **`Rule`** serão herdadas por todos os `Scenario` e `Scenario Outline` dentro daquela rule.
- Tags colocadas acima de um **`Scenario Outline`** serão herdadas por todas as tabelas de `Examples` associadas a ele.

Essa herança significa que você não precisa repetir tags comuns em cada cenário individualmente se elas se aplicam a um grupo maior.

#### Expressões de Tags (Tag Expressions)

Ao executar o Cucumber (por exemplo, a partir da linha de comando ou através de um executor de testes como JUnit ou TestNG), você pode usar **expressões de tags** para selecionar quais cenários executar. Uma expressão de tag é uma expressão booleana infixa que combina nomes de tags usando operadores lógicos.

Abaixo estão alguns exemplos comuns de expressões de tags e suas descrições:

|**Expressão**|**Descrição**|
|---|---|
|`@fast`|Executa cenários marcados com `@fast`.|
|`not @slow`|Executa cenários que **não** estão marcados com `@slow`.|
|`@wip and not @slow`|Executa cenários marcados com `@wip` E que não estão marcados com `@slow`.|
|`@smoke and @fast`|Executa cenários marcados com ambas as tags, `@smoke` E `@fast`.|
|`@gui or @database`|Executa cenários marcados com `@gui` OU com `@database` (ou ambos).|
|`(@smoke or @regression) and not @wip`|Executa cenários que são `@smoke` OU `@regression`, E que NÃO são `@wip`.|

As expressões de tags podem ser bastante poderosas, permitindo combinações complexas para direcionar precisamente os cenários que você deseja executar ou para os quais deseja aplicar hooks específicos. Elas são uma ferramenta fundamental para gerenciar suítes de teste de qualquer tamanho.

### Considerações Finais

Neste capítulo, mergulhamos no universo do Cucumber, uma ferramenta essencial para equipes que adotam o Desenvolvimento Orientado a Comportamento (BDD). Vimos como o Cucumber, através da sua linguagem Gherkin, permite a criação de especificações de teste em linguagem natural, fomentando a colaboração e o entendimento compartilhado entre todos os membros de um projeto.

Exploramos as palavras-chave fundamentais do Gherkin – `Feature`, `Rule`, `Scenario`, `Given`, `When`, `Then`, `And`, `But`, `Background`, `Scenario Outline` e `Examples` – que estruturam os testes de forma clara e legível. Detalhamos como os **Step Definitions** atuam como a ponte entre esses passos em linguagem natural e o código de automação subjacente, e como argumentos e tabelas de dados (`Data Tables`) enriquecem a capacidade de passar informações para esses steps.

Discutimos também a importância dos **Hooks** (`@Before`, `@After`, `@BeforeStep`, `@AfterStep`, `@BeforeAll`, `@AfterAll`) para gerenciar o ciclo de vida da execução dos testes, permitindo a configuração e limpeza de ambientes de forma precisa, inclusive condicionalmente através de **Tags**. As Tags, por sua vez, mostraram-se ferramentas indispensáveis para organizar, categorizar e filtrar a execução de cenários, proporcionando flexibilidade e controle sobre as suítes de teste.

O Cucumber não é apenas uma ferramenta de automação de testes; é uma plataforma para colaboração que ajuda a garantir que o software desenvolvido realmente atenda às necessidades do negócio e dos usuários. Ao traduzir comportamentos em especificações executáveis, ele transforma os testes em uma documentação viva do sistema. A adoção do Cucumber pode levar a um ciclo de desenvolvimento mais eficiente, com menos ambiguidades e um foco renovado na entrega de valor. Com o conhecimento adquirido aqui, você está mais preparado para implementar BDD com Cucumber e colher os benefícios de uma abordagem de teste mais comunicativa e orientada ao comportamento.
# Capítulo 10 – Princípios de Design S.O.L.I.D.

Nos capítulos anteriores, construímos um sólido entendimento sobre os mecanismos da Programação Orientada a Objetos: classes, objetos e os quatro pilares — abstração, encapsulamento, herança e polimorfismo. Aprendemos as "ferramentas" que o Java nos oferece. Agora, vamos dar um passo adiante e aprender a "filosofia" por trás de como usar essas ferramentas de forma eficaz.

Não basta saber _como_ criar uma classe ou _como_ usar a herança; é crucial saber _quando_, _onde_ e, mais importante, _por que_ aplicar cada conceito. Para nos guiar nessa jornada, recorremos a um conjunto de cinco princípios de design fundamentais, conhecidos pelo acrônimo **S.O.L.I.D.**

Estes princípios foram popularizados por Robert C. Martin (conhecido como "Uncle Bob") e representam um conjunto de diretrizes que, quando seguidas, nos ajudam a criar software que é:

- **Compreensível**: Fácil de ler e de raciocinar sobre.
- **Flexível**: Capaz de se adaptar a novas funcionalidades com o mínimo de esforço.
- **Manutenível**: Fácil de corrigir, modificar e evoluir ao longo do tempo.

Em essência, os princípios SOLID são a base para se construir um software de alta qualidade, reduzindo o acoplamento (a dependência entre as partes do sistema) e aumentando a coesão (o grau em que os elementos de um módulo pertencem uns aos outros).

O acrônimo S.O.L.I.D. representa:

- **S** - Single Responsibility Principle (Princípio da Responsabilidade Única)
- **O** - Open/Closed Principle (Princípio Aberto/Fechado)
- **L** - Liskov Substitution Principle (Princípio da Substituição de Liskov)
- **I** - Interface Segregation Principle (Princípio da Segregação de Interfaces)
- **D** - Dependency Inversion Principle (Princípio da Inversão de Dependência)

Neste capítulo, vamos dissecar cada um desses princípios, entendendo seu propósito e vendo como aplicá-los na prática com exemplos em Java.

## S: Single Responsibility Principle (Princípio da Responsabilidade Única)

O Princípio da Responsabilidade Única (SRP) é talvez o mais fundamental e, ao mesmo tempo, um dos mais mal interpretados. A sua definição formal, cunhada por Robert C. Martin, é:

> "Uma classe deve ter um, e somente um, motivo para mudar."

Isso não significa que uma classe deve ter apenas um método ou "fazer apenas uma coisa" de forma simplista. O "motivo para mudar" está ligado a um **ator** ou a uma **área de negócio**. Em outras palavras, uma classe deve ter responsabilidade sobre um único aspecto do software. Se podemos identificar que uma classe precisa ser alterada por requisições de diferentes setores de uma empresa (ex: o financeiro, o RH e a equipe de TI), então essa classe está violando o SRP, pois ela tem mais de um motivo para mudar.

### Exemplo Prático: A Violação do SRP

Vamos imaginar uma classe `Funcionario` em um sistema de RH, que foi projetada para fazer tudo relacionado a um funcionário.

**Código que Viola o SRP:**

```java
// Esta classe tem múltiplas responsabilidades (múltiplos motivos para mudar)
public class Funcionario {
    private String nome;
    private double salarioBase;

    public Funcionario(String nome, double salarioBase) {
        this.nome = nome;
        this.salarioBase = salarioBase;
    }

    // 1. Responsabilidade do RH/Financeiro
    public double calcularSalario() {
        // Lógica complexa de cálculo de impostos, bônus, etc.
        return this.salarioBase * 0.85; // Exemplo simplificado
    }

    // 2. Responsabilidade de Persistência (Banco de Dados)
    public void salvar() {
        // Lógica para conectar ao banco de dados e salvar o funcionário.
        System.out.println("Salvando funcionário " + this.nome + " no banco de dados...");
    }

    // 3. Responsabilidade de Apresentação (Relatórios)
    public String gerarRelatorioHTML() {
        // Lógica para formatar os dados do funcionário em HTML.
        return "<div><h1>" + this.nome + "</h1><span>Salário: " + this.salarioBase + "</span></div>";
    }

    // Getters e Setters...
}
```

Essa classe é um problema. Por quê?

- Se a **regra de cálculo de impostos mudar** (pedido do setor financeiro), a classe `Funcionario` deve ser alterada.
- Se o **banco de dados for migrado** de MySQL para PostgreSQL (pedido da equipe de infraestrutura), a classe `Funcionario` deve ser alterada.
- Se o **formato do relatório** precisar incluir novas informações (pedido do setor de gestão), a classe `Funcionario` deve ser alterada.

A classe `Funcionario` tem três eixos de mudança, três responsabilidades distintas. Isso a torna frágil, difícil de testar e difícil de manter.

### Aplicando o SRP: Separando as Responsabilidades

A solução é refatorar o código, dividindo a classe monolítica em classes menores e mais coesas, cada uma com uma única responsabilidade.

**A Classe `Funcionario` (Responsabilidade: Dados do Domínio)**

Agora, a classe `Funcionario` se preocupa apenas em representar os dados de um funcionário. Ela não sabe como calcular seu salário ou como ser salva no banco.

```java
public class Funcionario {
    private String nome;
    private double salarioBase;
    
    // Construtor, Getters e Setters...
}
```

**A Classe `CalculadoraDeSalario` (Responsabilidade: Regras de Negócio de Salário)**

Esta classe é a especialista em cálculos salariais.

```java
public class CalculadoraDeSalario {
    public double calcular(Funcionario funcionario) {
        // Lógica de cálculo de impostos e bônus vive aqui.
        return funcionario.getSalarioBase() * 0.85;
    }
}
```

**A Classe `FuncionarioRepository` (Responsabilidade: Persistência)**

Esta classe (seguindo o padrão **Repository**) lida exclusivamente com a comunicação com o banco de dados.

```java
public class FuncionarioRepository {
    public void salvar(Funcionario funcionario) {
        // Lógica de conexão e persistência dos dados do funcionário.
        System.out.println("Salvando funcionário " + funcionario.getNome() + " no banco de dados...");
    }
}
```

**A Classe `GeradorDeRelatorio` (Responsabilidade: Apresentação)**

Esta classe é especialista em formatar os dados para exibição.

```java
public class GeradorDeRelatorio {
    public String gerarHTML(Funcionario funcionario) {
        // Lógica de formatação de relatório vive aqui.
        return "<div><h1>" + funcionario.getNome() + "</h1><span>Salário: " + funcionario.getSalarioBase() + "</span></div>";
    }
}
```

Com essa refatoração, se a regra de cálculo de impostos mudar, apenas a classe `CalculadoraDeSalario` será alterada. Se o banco de dados mudar, apenas o `FuncionarioRepository` será afetado. Cada classe agora tem apenas **um motivo para mudar**, aderindo ao Princípio da Responsabilidade Única. Isso torna o sistema mais robusto, coeso e muito mais fácil de testar e manter.

## O: Open/Closed Principle (Princípio Aberto/Fechado)

O Princípio Aberto/Fechado (OCP), formulado originalmente por Bertrand Meyer, é o segundo pilar do SOLID. Ele estabelece uma diretriz fundamental para o design de sistemas flexíveis e resilientes à mudança. A sua definição é:

> "Entidades de software (classes, módulos, funções, etc.) devem ser abertas para extensão, mas fechadas para modificação."

À primeira vista, essa frase parece um paradoxo. Como algo pode ser aberto e fechado ao mesmo tempo? A chave para entender o OCP é separar os dois conceitos:

- **"Fechado para Modificação"**: Significa que, uma vez que uma classe foi desenvolvida, testada e validada, seu código-fonte não deve ser alterado para adicionar uma nova funcionalidade. A razão para isso é o risco: toda alteração em um código existente pode introduzir bugs em funcionalidades que já estavam estáveis, exigindo um novo ciclo completo de testes e validação.
- **"Aberto para Extensão"**: Significa que o comportamento da classe deve poder ser estendido para abranger novos requisitos de negócio. Devemos ser capazes de adicionar novas funcionalidades ao sistema sem tocar no código que já funciona.

O "segredo" para resolver esse paradoxo reside nos pilares da POO que já estudamos: **abstração** e **polimorfismo**. O OCP é alcançado ao se projetar sistemas que dependem de abstrações (classes abstratas ou interfaces), em vez de depender de implementações concretas.

Pense em um smartphone. O sistema operacional principal é "fechado para modificação". Você não pode alterar o código que faz o telefone ligar ou se conectar à rede celular. No entanto, o sistema é "aberto para extensão", pois você pode instalar novos aplicativos que adicionam funcionalidades incontáveis ao aparelho. A App Store e suas APIs funcionam como a camada de abstração que permite essa extensão sem modificar o núcleo do sistema.

### Exemplo Prático: A Violação do OCP

Vamos imaginar um sistema que calcula o bônus para diferentes tipos de contrato de trabalho. Uma abordagem inicial e ingênua poderia ser a seguinte:

**Código que Viola o OCP:**

```java
// Classe para calcular o bônus
public class CalculadoraDeBonus {
    public double calcularBonus(Object contrato) {
        double bonus = 0.0;
        
        // O problema: um grande bloco 'if-else if' que precisa ser modificado
        // a cada novo tipo de contrato.
        if (contrato instanceof ContratoCLT) {
            // Regra específica para CLT
            bonus = ((ContratoCLT) contrato).getSalario() * 0.1;
        } else if (contrato instanceof ContratoPJ) {
            // Regra específica para PJ
            bonus = ((ContratoPJ) contrato).getValorTotalNotas() * 0.05;
        }
        // ... e assim por diante
        
        return bonus;
    }
}

// Classes de modelo
class ContratoCLT {
    private double salario;
    public double getSalario() { return salario; }
    //...
}

class ContratoPJ {
    private double valorTotalNotas;
    public double getValorTotalNotas() { return valorTotalNotas; }
    //...
}
```

Este design funciona, mas é extremamente frágil e viola o OCP. Por quê?

Se a empresa decidir introduzir um novo tipo de contrato, como `ContratoDeEstagio`, seremos **obrigados a abrir a classe `CalculadoraDeBonus` e adicionar um novo `else if`**. A classe não está fechada para modificação. A cada novo requisito, o risco de quebrar a lógica existente aumenta.

### Aplicando o OCP: Estendendo com Abstrações

A solução é inverter a lógica. Em vez de a `CalculadoraDeBonus` conhecer todos os tipos de contrato, faremos com que cada contrato saiba como calcular seu próprio bônus, através de um contrato comum (uma interface).

**A Abstração (O Contrato Comum)**

Criamos uma interface `Remuneravel` que define o método que todas as formas de contrato devem implementar.

```java
// Aberto para extensão: qualquer nova classe pode implementar esta interface.
public interface Remuneravel {
    double calcularBonus();
}
```

**As Implementações Concretas**

Agora, cada classe de contrato implementa a interface e encapsula sua própria regra de negócio.

```java
public class ContratoCLT implements Remuneravel {
    private double salario;
    
    @Override
    public double calcularBonus() {
        return this.salario * 0.1;
    }
    //...
}

public class ContratoPJ implements Remuneravel {
    private double valorTotalNotas;
    
    @Override
    public double calcularBonus() {
        return this.valorTotalNotas * 0.05;
    }
    //...
}
```

**A Classe que Utiliza a Abstração**

A `CalculadoraDeBonus` se torna drasticamente mais simples e estável. Ela não conhece mais os tipos concretos; ela apenas opera sobre a abstração `Remuneravel`.

```java
// Fechado para modificação: esta classe não precisa mais mudar.
public class CalculadoraDeBonus {
    public double calcular(Remuneravel remuneravel) {
        // A lógica de decisão foi removida.
        // A chamada é polimórfica.
        return remuneravel.calcularBonus();
    }
}
```

Agora, se a empresa criar o `ContratoDeEstagio`, basta criar uma nova classe:

```java
public class ContratoDeEstagio implements Remuneravel {
    private double bolsaAuxilio;
    
    @Override
    public double calcularBonus() {
        // Estagiários podem ter uma regra diferente
        return this.bolsaAuxilio * 0.02;
    }
    //...
}
```

Podemos usar a nova classe no sistema sem fazer **nenhuma alteração** na classe `CalculadoraDeBonus`. A `CalculadoraDeBonus` está agora **fechada para modificação**, mas o sistema como um todo se mostrou **aberto para extensão**. Esse é o poder do OCP.

## L: Liskov Substitution Principle (Princípio da Substituição de Liskov)

O Princípio da Substituição de Liskov (LSP), formulado por Barbara Liskov, é o terceiro pilar do SOLID e está profundamente ligado ao pilar anterior (OCP) e ao conceito de herança. Ele estabelece um critério fundamental para a criação de hierarquias de classes que sejam corretas e robustas. A sua definição formal é:

> "Se S é um subtipo de T, então os objetos do tipo T podem ser substituídos por objetos do tipo S sem alterar nenhuma das propriedades desejáveis do programa (correção, execução de tarefas, etc.)."

Em termos mais simples e diretos: **uma subclasse deve ser substituível por sua superclasse em qualquer contexto, sem causar erros ou comportamentos inesperados.**

Isso significa que uma subclasse não deve apenas herdar os atributos e métodos de sua superclasse; ela deve, fundamentalmente, honrar o "contrato" e o comportamento esperado da superclasse. Se uma subclasse, ao sobrescrever um método, altera seu comportamento de uma forma que quebra as expectativas do código que a utiliza, ela está violando o LSP.

O LSP é um guia para garantir que a herança seja usada para modelar uma relação "é um" (_is-a_) verdadeira, não apenas para reutilizar código. Se o código cliente que funciona perfeitamente com um objeto `Veiculo` quebra quando lhe passamos um objeto `Carro` (que é um `Veiculo`), então a nossa hierarquia de herança está falha.

### Exemplo Prático: A Violação Clássica do LSP

O exemplo mais famoso para ilustrar a violação do LSP é o problema do "Quadrado-Retângulo". Matematicamente, um quadrado _é um_ tipo de retângulo (um retângulo com lados iguais). Tentados por essa lógica, poderíamos modelar isso em código da seguinte forma:

**Código que Viola o LSP:**

```java
// A superclasse Retangulo
class Retangulo {
    protected double altura;
    protected double largura;

    public void setAltura(double altura) {
        this.altura = altura;
    }

    public void setLargura(double largura) {
        this.largura = largura;
    }

    public double getArea() {
        return this.altura * this.largura;
    }
}

// A subclasse Quadrado, que TENTA ser um Retangulo
class Quadrado extends Retangulo {
    // Para manter a consistência de um quadrado, ao alterar a altura,
    // a largura também deve ser alterada, e vice-versa.
    @Override
    public void setAltura(double altura) {
        super.setAltura(altura);
        super.setLargura(altura); // Violação! Comportamento modificado.
    }

    @Override
    public void setLargura(double largura) {
        super.setLargura(largura);
        super.setAltura(largura); // Violação! Comportamento modificado.
    }
}
```

À primeira vista, parece fazer sentido. No entanto, o `Quadrado` quebrou o contrato do `Retangulo`. Um cliente que interage com um `Retangulo` espera poder alterar a altura e a largura de forma independente. O `Quadrado` não permite isso.

Vamos ver como isso quebra o código cliente:

```java
public class TesteLSP {
    public static void main(String[] args) {
        // Usando a superclasse diretamente - funciona como esperado
        Retangulo retangulo = new Retangulo();
        retangulo.setAltura(10);
        retangulo.setLargura(5);
        // Espera-se que a área seja 10 * 5 = 50
        System.out.println("Área do Retângulo: " + retangulo.getArea()); // Funciona! Saída: 50.0

        System.out.println("---");

        // Agora, substituímos o Retangulo pela sua subclasse Quadrado
        Retangulo quadrado = new Quadrado();
        quadrado.setAltura(10);
        quadrado.setLargura(5); // A expectativa do cliente é que a área seja 10 * 5 = 50

        // O resultado é inesperado!
        System.out.println("Área do Quadrado: " + quadrado.getArea()); // Surpresa! Saída: 25.0
    }
}
```

O teste falha. A área do `quadrado` é 25, e não 50. Por quê? Porque ao chamar `quadrado.setLargura(5)`, a implementação sobrescrita em `Quadrado` forçou a altura a também se tornar 5, quebrando a expectativa do cliente. O objeto `Quadrado` **não é substituível** por um objeto `Retangulo` sem causar um comportamento inesperado. A herança aqui, embora matematicamente correta, está errada do ponto de vista comportamental e viola o LSP.

### Aplicando o LSP: Reavaliando a Hierarquia

A solução para a violação do LSP geralmente envolve repensar a hierarquia de herança. A herança deve modelar o comportamento, não apenas as propriedades. Neste caso, `Quadrado` e `Retangulo` são conceitualmente diferentes em seu comportamento.

Uma solução comum é criar uma superclasse mais abstrata que não imponha comportamentos que as subclasses não possam cumprir.

**A Abstração Comum:**

Criamos uma classe `FormaGeometrica` que define o contrato comum: toda forma tem uma área.

```java
public abstract class FormaGeometrica {
    public abstract double getArea();
}
```

**As Implementações Independentes:**

Agora, `Retangulo` e `Quadrado` herdam da nova abstração, mas não uma da outra. Elas são "irmãs", não "mãe e filha".

```java
public class Retangulo extends FormaGeometrica {
    private double altura;
    private double largura;

    public Retangulo(double altura, double largura) {
        this.altura = altura;
        this.largura = largura;
    }
    
    @Override
    public double getArea() {
        return this.altura * this.largura;
    }
    // Getters e Setters para altura e largura
}
```

```java
public class Quadrado extends FormaGeometrica {
    private double lado;

    public Quadrado(double lado) {
        this.lado = lado;
    }
    
    @Override
    public double getArea() {
        return this.lado * this.lado;
    }
    // Getter e Setter para o lado
}
```

Com essa nova estrutura, não há mais a possibilidade de um objeto `Quadrado` ser tratado incorretamente como um `Retangulo`. O código cliente que opera sobre a abstração `FormaGeometrica` funcionará corretamente para qualquer uma de suas subclasses, pois nenhuma delas viola o comportamento esperado de sua superclasse. A substitutibilidade está garantida.

## I: Interface Segregation Principle (Princípio da Segregação de Interfaces)

O Princípio da Segregação de Interfaces (ISP) é o quarto pilar do SOLID e foca em como projetamos nossas abstrações, especificamente nossas interfaces. Ele lida com o problema das "interfaces gordas" ou "monolíticas" — interfaces que definem um contrato muito amplo, com múltiplos métodos que nem sempre são relevantes para todas as classes que as implementam.

A definição formal, também de Robert C. Martin, é:

> "Os clientes não devem ser forçados a depender de interfaces que eles não utilizam."

Em termos mais práticos, o ISP nos instrui a **manter nossas interfaces pequenas, coesas e focadas em um único papel**. Em vez de uma única interface grande e genérica, é melhor ter várias interfaces menores e mais específicas. Uma classe pode, então, implementar apenas as interfaces cujos comportamentos são realmente relevantes para ela.

Isso evita que uma classe seja obrigada a fornecer implementações vazias ou a lançar exceções para métodos que ela não precisa e não pode suportar, o que seria uma violação do contrato e também do Princípio da Substituição de Liskov.

### Exemplo Prático: A Violação do ISP

Vamos imaginar um sistema de automação para um escritório que modela diferentes tipos de máquinas. Uma abordagem inicial poderia ser a criação de uma única interface `Maquina` para abranger todas as funcionalidades possíveis.

**Código que Viola o ISP:**

```java
// Uma interface "gorda" que força todas as classes a implementarem tudo.
public interface Maquina {
    void ligar();
    void desligar();
    void imprimir(String documento);
    void digitalizar(String documento);
    void enviarFax(String documento);
}
```

Agora, vamos criar uma classe para uma impressora moderna multifuncional. Para ela, essa interface funciona bem.

```java
public class ImpressoraMultifuncional implements Maquina {
    @Override
    public void ligar() { /* ... */ }
    
    @Override
    public void desligar() { /* ... */ }
    
    @Override
    public void imprimir(String documento) {
        System.out.println("Imprimindo: " + documento);
    }
    
    @Override
    public void digitalizar(String documento) {
        System.out.println("Digitalizando: " + documento);
    }
    
    @Override
    public void enviarFax(String documento) {
        System.out.println("Enviando por fax: " + documento);
    }
}
```

O problema se torna evidente quando tentamos modelar uma impressora simples e antiga, que só sabe imprimir.

```java
public class ImpressoraSimples implements Maquina {
    @Override
    public void ligar() { /* ... */ }
    
    @Override
    public void desligar() { /* ... */ }
    
    @Override
    public void imprimir(String documento) {
        System.out.println("Imprimindo de forma simples: " + documento);
    }

    // A classe ImpressoraSimples é forçada a implementar métodos que não fazem sentido para ela.
    @Override
    public void digitalizar(String documento) {
        // Implementação vazia ou lança exceção.
        throw new UnsupportedOperationException("Esta impressora não digitaliza.");
    }

    @Override
    public void enviarFax(String documento) {
        // Implementação vazia ou lança exceção.
        throw new UnsupportedOperationException("Esta impressora não envia fax.");
    }
}
```

A `ImpressoraSimples` está sendo **forçada a depender de métodos (`digitalizar`, `enviarFax`) que ela não utiliza**. Isso não é apenas um design ruim; é um contrato quebrado. O código cliente que recebe um objeto `Maquina` pode tentar chamar `digitalizar()` em uma `ImpressoraSimples`, resultando em uma exceção e em um comportamento inesperado.

### Aplicando o ISP: Segregando as Interfaces

A solução é quebrar a interface monolítica `Maquina` em interfaces menores e mais coesas, cada uma representando um papel ou uma capacidade específica.

**As Interfaces Segregadas (Pequenas e Coesas)**

```java
public interface Ligavel {
    void ligar();
    void desligar();
}

public interface Impressora {
    void imprimir(String documento);
}

public interface Digitalizadora {
    void digitalizar(String documento);
}

public interface Fax {
    void enviarFax(String documento);
}
```

**As Implementações Concretas**

Agora, cada classe implementa apenas as interfaces que são relevantes para ela. A `ImpressoraMultifuncional` implementa todas as capacidades.

```java
// Esta classe implementa múltiplos contratos.
public class ImpressoraMultifuncional implements Ligavel, Impressora, Digitalizadora, Fax {
    @Override
    public void ligar() { /*...*/ }
    
    @Override
    public void desligar() { /*...*/ }
    
    @Override
    public void imprimir(String documento) { /*...*/ }
    
    @Override
    public void digitalizar(String documento) { /*...*/ }
    
    @Override
    public void enviarFax(String documento) { /*...*/ }
}
```

A `ImpressoraSimples` agora tem um contrato muito mais limpo e honesto.

```java
// Esta classe implementa apenas os contratos que ela pode cumprir.
public class ImpressoraSimples implements Ligavel, Impressora {
    @Override
    public void ligar() { /*...*/ }
    
    @Override
    public void desligar() { /*...*/ }
    
    @Override
    public void imprimir(String documento) { /*...*/ }
    
    // Não há mais necessidade de métodos vazios ou que lançam exceções.
}
```

Com esse design, o código cliente se torna mais seguro e explícito. Se um método precisa da capacidade de digitalizar, ele pode exigir um parâmetro do tipo `Digitalizadora`, e o compilador garantirá que apenas objetos que realmente podem digitalizar sejam passados para ele. O Princípio da Segregação de Interfaces nos leva a um design mais granular, modular e robusto.


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


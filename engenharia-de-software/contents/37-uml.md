# Capítulo 37 – UML: Visão Geral, Modelos e Diagramas

## O Nascimento da UML: A Unificação dos Métodos

Havia uma empresa chamada Rational Software Corporation, a mesma criadora do Processo Unificado Rational (RUP). Em 1995, ela conseguiu reunir três dos pesquisadores de Engenharia de Software mais proeminentes do mundo: **James Rumbaugh, Grady Booch e Ivar Jacobson** – que ficaram conhecidos como **The Three Amigos**.

<div align="center">
  <img width="480px" src="./img/37-the-three-amigos.png">
</div>

Rumbaugh era o criador da **Técnica de Modelagem de Objetos (OMT - Object-Modeling Technique)**. Já Booch era o criador do método de **Projeto Orientado a Objetos**, conhecido como **Método Booch**. Por fim, Jacobson era o criador do método de **Engenharia de Software Orientada a Objetos (OOSE - Object-Oriented Software Engineering)**. Esses três pesquisadores trabalhavam para a Rational, mas cada um seguia seus métodos e técnicas de modelagem próprios. Naquela época, não havia uma linguagem de modelagem dominante; existiam diversas linguagens, cada uma com suas vantagens e desvantagens.

Foi aí que a Rational se perguntou: "Por que a famosa e moderna tecnologia de Orientação a Objetos estava demorando tanto para ser adotada de fato?". A resposta foi, entre outras, que havia um excesso de linguagens de modelagem concorrentes. A empresa não gostou da resposta e requisitou aos seus notáveis pesquisadores que encontrassem uma solução.

E o que eles fizeram? Reuniram-se, consultaram outros pesquisadores, consolidaram seus respectivos métodos com as informações obtidas e padronizaram tudo em uma linguagem de modelagem não-proprietária: a **Unified Modeling Language (UML)**.

Naquela época, os fornecedores de ferramentas e metodologias estavam com medo de que um padrão controlado pela Rational desse alguma vantagem competitiva de forma desleal à empresa. Então, eles incitaram a **Object Management Group (OMG)** – um consórcio de empresas de tecnologia responsável por estabelecer padrões que suportem interoperabilidade – a tomar partido e gerenciar o processo de padronização.

Essa consolidação surgiu por meio de intensas discussões com representantes de tecnologias concorrentes. A UML é, portanto, o resultado da fusão de vários métodos e visões distintas de modelagem, incorporando as melhores práticas da época.

<div align="center">
  <img width="640px" src="./img/37-criacao-da-uml.png">
</div>

Observa-se que a UML contou com a participação de outras empresas de peso (como IBM, HP, Oracle, entre outras). Sendo assim, a OMG adotou a **UML versão 1.1** como um padrão oficial em agosto de 1997. Posteriormente, ela foi editada como um padrão internacional pela ISO, sob o código **ISO/IEC 19501:2005**.

A partir daí, a linguagem evoluiu continuamente. A revisão 1.2 foi focada em melhorias estéticas; já a versão 1.3 trouxe mudanças mais significativas. A revisão 1.4 acrescentou os conceitos de componentes e perfis, e a revisão 1.5 adicionou a semântica de ação. Com o passar do tempo, a linguagem passou a integrar conceitos de outros métodos de orientação a objetos, corrigindo diversos bugs e inconsistências até alcançar maturidade suficiente para o grande salto para a **versão UML 2.0**, em meados de 2005. A partir de então, passou por diversas e pequenas atualizações até chegar à versão atual: a **UML 2.5**.

## Definição e Propósito da UML

A UML pode ser definida como uma **linguagem gráfica para especificar, visualizar e documentar artefatos de um sistema, primariamente de software**. Por que "primariamente"? Porque ela tem sido usada efetivamente em diversas outras áreas, a saber: telecomunicações, defesa, aeroespacial, bancária, eletrônica, financeira, entre outras. Logo, a UML não está limitada à modelagem de software. Aliás, ela é uma linguagem tão expressiva que pode modelar outros sistemas complexos, tais como o fluxograma de um sistema judiciário, o comportamento de um sistema de saúde pública ou um projeto de hardware.

Alguns a definem como uma linguagem padrão de modelagem visual usada para modelagem de negócio e processos similares, além da análise, projeto e implementação de sistemas baseados em software. Trata-se de uma linguagem comum para analistas de negócio, arquitetos de software e desenvolvedores, usada tanto para sistemas de software já existentes quanto para novos projetos.

A UML é intencionalmente **independente de processos**, isto é, pode ser aplicada no contexto de diferentes metodologias de desenvolvimento (como RUP, Scrum, XP, etc.). Ademais, é importante notar que ela **não é uma linguagem completa**, ou seja, um único diagrama raramente é suficiente para entender completamente o comportamento de um sistema; e **não é completamente visual**, pois alguns conceitos e restrições não possuem notação gráfica, sendo descritos textualmente.

### Por que utilizar a UML?

Martin Fowler, um renomado autor da área de software, afirma que os principais motivos são a **comunicação** e o **entendimento**. Um bom diagrama frequentemente pode ajudar uma equipe a compreender um problema complexo e a transmitir uma solução de forma clara e eficaz. A notação gráfica da UML serve como um meio-termo ideal entre a imprecisão da linguagem natural e o detalhamento excessivo de uma linguagem de programação.

Pense em como seria especificar um sistema complexo apenas escrevendo ou falando. A quantidade de ambiguidades e mal-entendidos seria imensa, pois a linguagem natural é inerentemente imprecisa. Agora, imagine o oposto: especificar um sistema usando diretamente código C++ ou Java. Já imaginou a reação de um cliente ou analista de negócios ao se deparar com 400 linhas de código para entender o funcionamento de uma única classe? O código detalha em excesso e em um nível de abstração muito baixo. Este equilíbrio já seria um excelente motivo para se utilizar a UML.

Mas resume-se a isso? Não. Ela é uma linguagem completamente **não-dependente de tecnologia**. Isso significa que seus modelos não estão atrelados a uma linguagem de programação específica. É perfeitamente possível modelar um sistema em UML e depois implementá-lo em C++, Java, Python, ou até mesmo em linguagens estruturadas como C, Cobol e Pascal. Ela é independente de linguagem de programação, ferramentas e plataformas.

Hoje em dia, a UML se tornou não somente a notação gráfica mais dominante dentro do mundo orientado a objetos, como também uma técnica popular em círculos não orientados a objetos. Por fim, cabe salientar que a UML é uma verdadeira ferramenta de planejamento, ajudando a apresentar uma visão geral do sistema, tanto do seu estado atual quanto do futuro desejado.

## Estrutura da UML: Especificações e Visões

A UML é formalmente definida por um conjunto de quatro especificações interdependentes, mantidas pela OMG.

| Especificações UML |Descrição|
|---|---|
|**Infraestrutura**|Especificação que define a base da linguagem, o **core** da sua arquitetura, e os mecanismos de extensão, como perfis e estereótipos.|
|**Superestrutura**|Especificação que define os elementos de modelagem do usuário, tanto os estáticos (estruturais) quanto os dinâmicos (comportamentais).|
|**Object Constraint Language (OCL)**|Especificação que contém a linguagem formal e textual usada para descrever expressões e regras de negócio (como pré e pós-condições) que se aplicam aos modelos UML.|
|**Intercâmbio de Diagramas (DI)**|Especificação que define os formatos padrão para o intercâmbio de diagramas entre diferentes ferramentas CASE, garantindo a interoperabilidade.|

### As Visões Arquiteturais (Modelo 4+1)

Na década de 90, a arquitetura de software padecia de alguns problemas graves. Muitas vezes, enfatizava-se exageradamente apenas um aspecto do desenvolvimento, como a estrutura de dados, negligenciando outros. Em outras vezes, a arquitetura não era descrita de forma a atender aos interesses de todos os envolvidos (stakeholders). Para solucionar essa questão, Philippe Kruchten, trabalhando na Rational, propôs o "Modelo de Visão Arquitetural 4+1", que organiza a descrição da arquitetura de software usando diversas visões concorrentes, cada uma direcionada a um conjunto de interesses específicos. A UML foi projetada para dar suporte a essa abordagem.

|Visões UML|Descrição|
|---|---|
|**Visão Lógica**|Trata-se da visão da arquitetura sob o ponto de vista dos **usuários finais**. Apresenta os requisitos funcionais do software, ou seja, "o que" o sistema faz. Mostra as classes, suas estruturas e seus relacionamentos. Principais diagramas utilizados: Classe, Objetos e Pacotes. (Também chamada de **Visão de Projeto**).|
|**Visão de Desenvolvimento**|Trata-se da visão da arquitetura sob o ponto de vista do **programador**. Apresenta a organização estática dos módulos e componentes que formam o software, focando na gerenciabilidade do código-fonte. Principais diagramas utilizados: Componentes e Pacotes. (Também chamada de **Visão de Implementação**).|
|**Visão de Processo**|Trata-se da visão da arquitetura sob o ponto de vista do **integrador de sistemas**. Apresenta os aspectos dinâmicos, a concorrência e a sincronização. Foca em requisitos não-funcionais como desempenho, escalabilidade e disponibilidade. Principais diagramas utilizados: Sequência, Estrutura Composta, Máquina de Estados e Atividade.|
|**Visão Física**|Trata-se da visão da arquitetura sob o ponto de vista do **engenheiro de sistemas**. Apresenta a topologia da infraestrutura de hardware e como os componentes de software são distribuídos (implantados) nos nós físicos da rede. Principais diagramas utilizados: Implantação. (Também chamada de **Visão de Implantação**).|
|**Visão de Casos de Uso (+1)**|Trata-se da visão central que conecta todas as outras, sob o ponto de vista de **todos os stakeholders**. Descreve os cenários e os requisitos que guiam o projeto arquitetural, apresentando a consistência e a validade do sistema. Principal diagrama utilizado: Casos de Uso. (Também chamada de **Visão de Cenários**).|

[Figura - Visões]

### **Mecanismos Gerais da UML**

A UML contém alguns mecanismos de uso geral muito importantes, que permitem estender e detalhar os modelos para além da notação padrão.

|MECANISMOS UML|DESCRIÇÕES|
|---|---|
|**ESTEREÓTIPO**|Mecanismo utilizado para estender o significado de um elemento do modelo, criando um "novo tipo" de elemento com semântica específica para um domínio ou plataforma.|
|**NOTAS EXPLICATIVAS**|Mecanismo utilizado para adicionar comentários, esclarecimentos ou informações textuais a qualquer parte de um diagrama, sem alterar sua semântica.|
|**TAGGED VALUES**|Mecanismo que permite definir novas propriedades (metadados) para os elementos, sempre associadas a um estereótipo.|
|**RESTRIÇÕES**|Mecanismo utilizado para especificar regras ou condições que devem ser verdadeiras para um ou mais elementos de um modelo.|
|**PACOTES**|Mecanismo utilizado para agrupar elementos semanticamente relacionados, organizando o modelo e controlando sua visibilidade (será detalhado no Diagrama de Pacotes).|

#### **Estereótipos**

Estereótipos permitem adaptar ou personalizar modelos com construções específicas para um domínio, plataforma ou método de desenvolvimento particular. Em outras palavras, é o principal mecanismo de extensão da UML, conferindo-lhe mais poder e flexibilidade. Antes, os modeladores estavam limitados aos elementos padrão; com estereótipos, as possibilidades para representar um sistema se tornam praticamente infinitas.

Existem duas classificações independentes para estereótipos:

1. **Quanto à Origem**:
    
    - **Predefinidos pela Linguagem**: Já vêm nativamente na UML (Ex: `<<interface>>`, `<<trace>>`, `<<entity>>`).
        
    - **Definidos pelo Usuário**: A equipe de desenvolvimento pode criar seus próprios estereótipos para representar conceitos específicos do seu projeto (Ex: `<<TelaCadastro>>`, `<<ServidorLegado>>`).
        
2. **Quanto à Representação**:
    
    - **Textual**: Representado por um nome entre aspas angulares duplas (Ex: `<<control>>`).
        
    - **Gráfica**: Representado por um ícone específico que substitui o símbolo padrão do elemento, lembrando visualmente o conceito associado.
        

[Figura - estereótipos gráficos]

Na imagem acima, os dois estereótipos à esquerda (`<<interface>>` e `<<control>>` com seus ícones) são predefinidos, enquanto os dois à direita são exemplos de estereótipos que poderiam ser criados pela equipe. Como as classificações são independentes, é possível ter estereótipos gráficos ou textuais, sejam eles predefinidos ou definidos pelo usuário.

#### **Notas Explicativas**

Notas explicativas são utilizadas para definir informações que comentam ou esclarecem alguma parte de um diagrama. Elas podem conter texto livre ou uma expressão formal utilizando a OCL (_Object Constraint Language_).

Graficamente, as notas são representadas por um retângulo com um canto superior direito dobrado, como uma "orelhinha". O conteúdo da nota é inserido no interior do retângulo, que por sua vez é ligado ao elemento que se quer comentar através de uma linha tracejada. Ao contrário dos estereótipos, as notas não modificam nem estendem o significado do elemento ao qual estão associadas; ou seja, não criam algo novo, apenas explicam ou documentam um elemento existente no modelo sem alterar sua estrutura ou semântica.

[Figura - Representação de uma nota]

#### **Tagged Values (Etiquetas Valoradas)**

Elementos UML possuem um conjunto de propriedades predefinidas. As classes, por exemplo, possuem: Nome, Visibilidade, Lista de Atributos e Lista de Operações. As _Tagged Values_ (ou Etiquetas Valoradas) são utilizadas para definir novas propriedades para os elementos de um diagrama. A partir da UML 2.0, as _Tagged Values_ só podem ser utilizadas em conjunto com estereótipos, funcionando como metadados para esses estereótipos. Elas são representadas no formato `{tag = valor}`.

[Figura - Tagged values com esteriótipos]

#### **Restrições**

A todo elemento da UML está associada uma semântica bem definida. As restrições (_constraints_) permitem estender ou alterar a semântica natural de um elemento, especificando condições ou regras que devem ser satisfeitas. Esse mecanismo geral especifica limitações sobre um ou mais valores de um ou mais elementos de um modelo.

As restrições podem ser especificadas formalmente, através da OCL, ou informalmente, com texto livre em linguagem natural. Em ambos os casos, devem vir delimitadas por chaves `{}` e, frequentemente, são colocadas dentro de notas explicativas anexadas aos elementos que restringem.

## Visão Geral dos Diagramas UML

A Modelagem Orientada a Objetos ocorre quase sempre por meio da **Unified Modeling Language**. A UML 2.5 descreve 14 tipos de diagramas oficiais, divididos em duas categorias principais, que por sua vez se subdividem.

[Figura - Diagramas da UML]

|TIPOS DE DIAGRAMAS|DESCRIÇÃO|
|---|---|
|**DIAGRAMAS ESTRUTURAIS**|Representam os aspectos **estáticos** do sistema sob diversas visões. Em outras palavras, esses diagramas apresentam a estrutura do sistema, seus componentes e seus relacionamentos, sem levar em consideração a passagem do tempo. São eles: Diagrama de Classes, de Componentes, de Implantação, de Perfil, de Objetos, de Estrutura Composta e de Pacotes.|
|**DIAGRAMAS COMPORTAMENTAIS**|Representam os aspectos **dinâmicos** do sistema como um conjunto de mudanças e interações ao longo do tempo. Descrevem como os processos e funcionalidades do programa se relacionam e se comportam. São eles: Diagrama de Máquina de Estados, de Casos de Uso, de Atividade, de Sequência, de Comunicação, de Visão Geral da Interação e de Tempo.|
|**DIAGRAMAS DE INTERAÇÃO**|Trata-se de um **subconjunto** dos diagramas comportamentais que focam especificamente no relacionamento dinâmico e colaborativo entre os objetos do sistema e suas trocas de mensagens. Eles enfatizam o controle de fluxo e o fluxo de dados entre os elementos. São eles: Diagrama de Sequência, de Comunicação, de Visão Geral da Interação e de Tempo.|

## Diagramas Estruturais

### Diagrama de Classes

O diagrama de classes é facilmente o diagrama mais conhecido e importante entre todos os outros. Ele descreve as classes e interfaces presentes no sistema, suas características, restrições e os vários tipos de relacionamentos estáticos entre seus objetos. Representam-se também as propriedades e as operações de uma classe, assim como as restrições que se aplicam à maneira como os objetos estão conectados.

[Figura - Diagrama De Classes]

Uma classe é uma estrutura classificadora que abstrai um conjunto de objetos que compartilham características, restrições e semânticas similares. Ela define, também, o comportamento de seus objetos através de métodos e o estado por meio de atributos.

Representação de Classes:

Pode-se representar uma classe de diversas formas, dependendo do nível de abstração: primeiro, apenas com nome da classe (mais abstrata); segundo, com nome da classe e suas propriedades; e terceiro, com nome da classe, suas propriedades e suas operações (mais concreta).

[Figura - Diferentes formas de se representar Classes]

A classe com borda dupla na imagem é a representação do que chamamos de **Classe Ativa**, que tem por objetivo representar classes cujos objetos têm um ou mais processos (threads). É possível inserir detalhes de implementação da linguagem de programação escolhida no diagrama.

**Representação de Membros:**

- Um atributo estático é representado sublinhando o nome do atributo.
    
- Uma operação abstrata é representada com seu nome em itálico.
    
- Uma operação estática é representada com seu nome sublinhado.
    

Visibilidade de Atributos e Operações:

As tabelas abaixo apresentam a diferença de visibilidade entre a Linguagem Java e a UML.

- **Tabela de modificadores UML**
    

|Modificador|Símbolo|Classe|Subclasse|Pacote|Todos|
|---|---|---|---|---|---|
|Público|+|X|X|X|X|
|Protegido|#|X|X|-|-|
|Pacote|~|X|-|X|-|
|Privado|–|X|-|-|-|

- **Tabela de modificadores Java**
    

|Modificador|Símbolo|Classe|Subclasse|Pacote|Todos|
|---|---|---|---|---|---|
|Público|+|X|X|X|X|
|Protegido|#|X|X|X|-|
|Default|~|X|-|X|-|
|Privado|–|X|-|-|-|

Existem duas diferenças sutis: (1) na UML, um elemento protegido não é visível para elementos dentro do mesmo pacote; e (2) o Nível Pacote da UML é chamado de Nível Default em Java.

[Figura - Representação de classe com modificadores]

Tipos de Relacionamentos:

Eles representam as conexões entre classes, objetos, pacotes, tabelas, entre outros. Há três tipos de relacionamentos entre classes: Dependência, Generalização e Associação.

[Figura - relacionamentos]

1. Relacionamento de Dependência: É um relacionamento direcionado e semântico entre dois ou mais elementos que ocorre se mudanças na definição de um elemento (independente) causarem mudanças ao outro elemento (dependente). Em outras palavras, é quando a classe cliente é dependente de algum serviço da classe fornecedora. O relacionamento de dependência é representado por uma seta tracejada que aponta para classe independente. Os relacionamentos <include> e <extend> também são relacionamentos de dependência.
    
    [Figura - Relacionamento de Dependência]
    
2. Relacionamento de Generalização/Especialização (Herança): Indica que a subclasse é uma especialização da superclasse ou que a superclasse é uma generalização da subclasse. Qualquer instância da subclasse é também uma instância da superclasse. É conhecido como relacionamento de herança, relacionamento de extensão ou relacionamento “é-um”. É representado por uma linha com um triângulo que aponta para a classe genérica.
    
    [Figura - Relacionamento de Generalização/Especialização]
    
3. Relacionamento de Realização: Relacionamento entre dois elementos em que um elemento realiza (implementa/executa) o comportamento que o outro elemento especifica. Costuma-se dizer que um dos elementos especifica um contrato e o outro elemento realiza esse contrato. É representado por uma linha tracejada com um triângulo que aponta para a interface.
    
    [Figura - Relacionamento de Realização]
    
4. **Relacionamento de Associação:** Relacionamento estrutural entre objetos que especifica os objetos de uma classe que estão ligados a objetos de outra classe.
    
    - Associação Simples: É um tipo de relacionamento mais forte que o de dependência e indica que uma instância de um elemento está ligada à instância de outro elemento. São representados por uma linha sólida com ou sem setas de navegabilidade. Pode haver nomes para a associação e indicação de multiplicidade.
        
        [Figura - Relacionamento de Associação Simples]
        
    - Associação Qualificada: É um tipo de relacionamento similar à associação simples, contudo possui um qualificador, que é um atributo do elemento-alvo capaz de identificar uma instância dentre as demais. Ela ocorre em associações um-para-muitos ou muitos-para-muitos em que se deseja encontrar um elemento específico dada uma chave. É representado por uma linha sólida com um retângulo ao lado da classe de cardinalidade 1 contendo o qualificador.
        
        [Figura - Relacionamento de Associação Qualificada]
        
    - Agregação: É um tipo de associação, porém mais forte, em que o todo está relacionado às suas partes de forma independente. Nesse tipo de relacionamento, as partes têm existência própria. É representado por uma linha com um diamante vazio na extremidade referente ao todo.
        
        [Figura - Relacionamento de Associação Agregação]
        
    - Composição: É um tipo de agregação (inclusive, é chamado algumas vezes de Agregação por Composição), porém mais forte, em que o todo está relacionado às partes de forma dependente. Nesse relacionamento, as partes não têm existência própria. É representado por uma linha com um diamante cheio na extremidade referente ao todo.
        
        [Figura - Relacionamento de Associação Composição]
        

[Figura - Resumo dos diversos relacionamentos em diagramas de classes]

#### Quando Utilizar o Diagrama de Classes?

Os Diagramas de Classe são a espinha dorsal da UML; portanto, você irá utilizá-los o tempo todo. O maior problema é que eles são tão ricos que podem ser complexos demais para usar. Dessa forma, não tente utilizar todas as notações de que você dispõe; o uso de diagramas de classes conceituais é muito útil na exploração da linguagem do negócio. Busque manter o software fora da discussão e manter a notação mais simples; não desenhe modelos para tudo; em vez disso, concentre-se nas áreas principais. É melhor ter poucos diagramas que você utiliza e os mantém atualizados do que ter muitos modelos esquecidos e obsoletos. O maior perigo é que você pode focalizar exclusivamente a estrutura e ignorar o comportamento. Portanto, ao desenhar diagramas de classe para entender o software, sempre os faça em conjunto com alguma forma de técnica comportamental.

### Diagrama de Objetos

O Diagrama de Objetos (ou Diagrama de Instâncias) é uma variação do Diagrama de Classes que representa uma fotografia estática do sistema em um dado momento de execução. Ele não reflete o sistema genericamente, mas de forma específica – em um determinado instante. Como ele mostra instâncias, em vez de classes, ele é frequentemente chamado de Diagrama de Instâncias.

[Figura - Diagrama de Objetos]

Os elementos de um diagrama de objetos são instâncias caso os nomes estejam sublinhados. Cada nome assume a forma `nome de instância : nome da classe`. Rigorosamente, elementos de um diagrama de objetos são especificações de instâncias, em vez de instâncias verdadeiras. O motivo é que é válido deixar atributos obrigatórios vazios ou mostrar especificações de instâncias de classes abstratas.

|Representação do Nome|Descrição|
|---|---|
|`:`|Significa que se trata de uma instância anônima de uma classe anônima.|
|`:Cliente`|Significa que se trata de uma instância anônima da classe Cliente.|
|`novoCliente:`|Significa que se trata de uma instância novoCliente de uma classe anônima.|
|`novoCliente:Cliente`|Significa que se trata de uma instância novoCliente de uma classe Cliente.|
|`novoCliente:Clientes::Cliente`|Significa que se trata de uma instância novoCliente de uma classe Cliente de um pacote Clientes.|

#### Quando Utilizar o Diagrama de Objetos?

Os Diagramas de Objeto são úteis para mostrar exemplos de objetos interligados. Eventualmente, define-se uma estrutura precisamente com um diagrama de classes, mas a estrutura pode ser ainda difícil de entender. Nesses casos, exemplos de diagrama de objetos podem auxiliar. Eles podem ser vistos como um diagrama de comunicação sem as mensagens.

### Diagrama de Componentes

O Diagrama de Componentes representa o sistema sob uma perspectiva funcional, expondo a organização de seus módulos e as relações entre seus componentes por meio de interfaces. Um **componente** é uma unidade independente, que pode ser utilizada ou substituída com/por outros componentes para formar um sistema complexo. Eles representam peças que podem ser adquiridas e atualizadas independentemente, como tabelas, documentos, etc. Sua grande vantagem é a modularidade.

[Figura - Diagrama de Componentes]

Os componentes são representados como retângulos com o símbolo de componente no canto superior direito.

[Figura - Representação de componentes]

Um componente pode apresentar um estereótipo. Os principais são:

- `<<Arquivo>>`: determina que o componente é um arquivo de dados do sistema.
    
- `<<Biblioteca>>`: determina que o componente é uma biblioteca de código.
    
- `<<Documento>>`: determina que o componente é um documento de sistema.
    
- `<<Executável>>`: determina que o componente é um arquivo executável.
    
- `<<Tabela>>`: determina que o componente é uma tabela de um banco de dados.
    

[Figura - Representação de esteriótipos]

Temos também a **Interface Fornecida**, que designa uma interface que o próprio componente possui e oferece para outros componentes, e a **Interface Requerida**, que designa uma interface necessária para que o componente se comunique com outros.

[Figura - Representação de Interface Fornecida]

#### Quando Utilizar o Diagrama de Componentes?

Use Diagramas de Componentes quando você estiver dividindo seu sistema em componentes e quiser representar seus relacionamentos por intermédio de interfaces ou também quando quiser representar a decomposição de componentes em uma estrutura de nível mais baixo.

### Diagrama de Pacotes

Ele representa os pacotes e suas dependências. Um **pacote** é um agrupamento lógico que permite reunir elementos da UML em unidades de mais alto nível. Comumente, pacotes agrupam classes, mas pode-se agrupar qualquer coisa. Esses pacotes ilustram a arquitetura de um sistema como um agrupamento de classes em um alto nível de abstração. O diagrama é desenhado como pastas contendo classes ou outras pastas.

[Figura - Diagrama de Pacotes]

#### Quando Utilizar o Diagrama de Pacotes?

São extremamente úteis em sistemas de grande porte, para obter uma visão das dependências entre os principais elementos de um sistema. A representação de diagramas de pacotes e dependências o ajuda a manter as dependências de uma aplicação sob controle. Os diagramas de pacotes representam um mecanismo de agrupamento em tempo de compilação3.

### Diagrama de Implantação (ou Instalação)

O Diagrama de Implantação apresenta o layout físico de um sistema, revelando quais partes do software são executadas em quais partes do hardware. Os itens principais do diagrama são **nós** conectados por caminhos de comunicação. Um nó é algo que pode conter algum software.4

[5Figura - Diagrama de Implantação (Ou Instalação)]

- Um **dispositivo** é um hardware (computador ou peça de hardware).
    
- Um **ambiente de execução** é um software que contém outro software (Ex: Sistema operacional).
    
- Os nós contêm **artefatos**, que são manifestações físicas do software (arquivos executáveis, de dados, de configuração, etc.).
    

A listagem de um artefato dentro de um nó mostra que ele está instalado nesse nó. Os caminhos de comunicação entre os nós indicam como as coisas se comunicam, e você pode rotulá-los com informações sobre os protocolos de comunicação utilizados. Os nós e artefatos não são de responsabilidade do programador, mas da equipe de implantação do sistema.

#### Quando Utilizar o Diagrama de Implantação?

Eles são úteis para mostrar o que é instalado e onde; portanto, qualquer instalação mais complicada pode fazer bom uso deles (Ex: uma configuração e arquitetura de sistema em que estarão ligados componentes, representados pela arquitetura física de hardware, processadores, entre outros).

### Diagrama de Perfil

Frequentemente, vale a pena definir uma versão de UML adequada a uma determinada finalidade ou área de domínio. É possível ajustar a UML utilizando **perfis**. Um perfil é uma UML com um conjunto de estereótipos predefinidos, valores atribuídos, restrições e classes de base. Ele seleciona um subconjunto dos tipos de elementos da UML e define uma versão especializada para uma determinada área. Como o perfil é criado sobre elementos comuns, ele não representa uma nova linguagem e pode ser suportado por ferramentas comuns da UML.

[Figura - Diagrama de Perfil]

O Diagrama de Perfil permite representar esses novos elementos, operando no nível de **metamodelos**. Um modelo tipicamente contém elementos que são instanciados a partir de um metamodelo. Um metamodelo define uma linguagem para expressar modelos. Já uma **metaclasse** é uma classe que pode ser estendida por meio de um ou mais estereótipos; trata-se de uma classe do metamodelo (classe, interface, etc).

### Diagrama de Estrutura Composta

O Diagrama de Estrutura Composta é utilizado para modelar colaborações internas de classes, interfaces e componentes para especificar uma funcionalidade. Uma **colaboração** é uma visão de um conjunto de entidades cooperativas interpretadas por instâncias que cooperam entre si para executar uma operação específica. Este diagrama fornece meios de definir a estrutura de um elemento e de focalizá-la no detalhe, na construção e em relacionamentos internos.

[Figura - Diagrama de Estrutura Composta]

#### Quando Utilizar o Diagrama de Estrutura Composta?

As estruturas compostas entraram na UML 2.0. Uma boa maneira de pensar a respeito da diferença entre pacotes e estruturas compostas é que os pacotes são um agrupamento em tempo de compilação e estruturas compostas mostram agrupamentos em tempo de execução. Dessa forma, elas servem naturalmente para mostrar componentes e como eles são divididos em partes.

---

## Diagramas Comportamentais

### Diagrama de Casos de Uso

Casos de Uso são uma técnica para captar os requisitos funcionais de um sistema, descrevendo as interações de usuários com o sistema. Um **cenário** é uma instância de um caso de uso. O diagrama descreve um conjunto de funcionalidades do sistema e interações com elementos externos (**Atores**) e entre si. Um ator pode ser um humano, uma máquina ou outro sistema.

[Figura - Diagrama de Caso de Uso]

- **Tipos de Casos de Uso:**
    
    - **Concreto:** Iniciado por um ator, constitui um fluxo completo de eventos.
        
    - **Abstrato:** Jamais é instanciado diretamente, é incluído, estendido ou generalizado por outros (representado com nome em itálico).
        
    - **Primário:** Representa os objetivos principais dos atores.
        
    - **Secundário:** Necessário para o sistema funcionar, mas não traz benefício direto aos atores.
        
- **Relacionamentos:**
    
    - Comunicação: Ligação entre ator e caso de uso.
        
        [Figura - Representação do Relacionamento de Comunicação]
        
    - Inclusão (<<include>>): Comportamento obrigatório e repetido. Quando o caso de uso A “inclui” o caso de uso B, significa que sempre que A for executado, B também será. A seta vai do caso de uso que inclui para o incluído.
        
        [Figura - Representação do Relacionamento de Inclusão]
        
    - Extensão (<<extend>>): Comportamento alternativo ou opcional. Quando B estende A, significa que quando A for executado, B poderá ser executado também. A seta vai do caso de uso extensor para o estendido.
        
        [Figura - Representação do Relacionamento de Extensão]
        
    - Herança: Relacionamento entre atores ou entre casos de uso para reaproveitar comportamentos.
        
        [Figura - Representação do Relacionamento de Herança]
        

|Relações|Comunicação|Extensão|Inclusão|Herança|
|---|---|---|---|---|
|Entre casos de uso|-|X|X|X|
|Entre atores|-|-|-|X|
|Entre casos de uso e atores|X|-|-|-|

#### Quando Utilizar o Diagrama de Casos de Uso?

São uma ferramenta valiosa para ajudar no entendimento dos requisitos funcionais. Uma primeira passagem deve ser feita no início do projeto. É importante lembrar que casos de uso representam uma visão externa do sistema. Concentra-se energia mais no texto do que no diagrama; é esse texto que contém todo o valor da técnica.

### Diagrama de Atividades

O Diagrama de Atividades descreve lógica de procedimento, processo de negócio e fluxos de trabalho. Ele desempenha um papel semelhante aos fluxogramas, mas suporta comportamentos paralelos. Uma **atividade** é um comportamento parametrizado representado como um fluxo coordenado de ações. O diagrama não se preocupa com interações entre objetos, mas entre processos de negócios de mais alto nível. Os estados mudam quando uma ação é executada.

Pode ser representado por meio de **swimlanes** (raias), que agrupam as atividades e as organizam de acordo com suas respectivas responsabilidades.

[Figura - Diagrama de Atividades]

- **Ramificações:** Especificam caminhos alternativos baseados em expressões booleanas (representadas com um diamante).
    
- **Bifurcação (Fork) e União (Join):** A bifurcação é a divisão de um fluxo em dois ou mais fluxos concorrentes. A união é a sincronização desses fluxos. Uma barra de sincronização é usada para especificar ambos.
    

#### Quando Utilizar o Diagrama de Atividades?

A maior qualidade dos Diagramas de Atividades está no fato de que eles suportam e estimulam o comportamento paralelo. Isso os torna uma excelente ferramenta para modelagem de fluxos de trabalho e de proc6essos.

### Diagrama de Máquina de Estados

Também conhecido como Diagrama de Transição de Estados, apresenta diversos estados possíveis de um objeto no decorrer da execução de processos de um sistema. Um objeto pode passar de um estado inicial para um estado final, por meio de uma transição, quando ocorre algum evento ou estímulo. Ele mostra o ciclo de vida de objetos e como eles são afetados por eventos.

[Figura - Diagrama de Máquina de Estados]

É necessário distinguir Diagrama de Atividades e Diagrama de Máquina de Estados. No primeiro, o comportamento está expresso nos nós. No segundo, o comportamento está nos arcos. Apesar de visualmente similares, o que é representado em cada diagrama é o oposto um do outro.

#### Quando Utilizar o Diagrama de Máquina de Estados?

Eles são bons para descrever o comportamento de um objeto por intermédio de vários casos de uso ou para descrever um comportamento que envolva vários objetos em colaboração. Recomenda-se não o utilizar para todas as classes, mas apenas para aquelas que exibem comportamento interessante.

---

### Diagramas de Interação

### Diagrama de Sequência

O Diagrama de Sequência é um diagrama de interação que captura o comportamento de um único cenário, mostrando vários exemplos de objetos e as mensagens que são trocadas entre eles, com ênfase na ordem temporal. O eixo horizontal representa os objetos e o vertical representa o tempo (linha de vida). Uma **mensagem** é um serviço solicitado de um objeto para outro.

[Figura - Diagrama de Sequência]

#### Quando Utilizar o Diagrama de Sequência?

Você deve utilizá-lo quando quiser observar o comportamento de vários objetos dentro de um único caso de uso. São bons para mostrar as colaborações entre os objetos, mas não para uma definição precisa do comportamento.

### Diagrama de Comunicação

Também é um diagrama de interação muito semelhante ao de sequência, mas com ênfase na ordem estrutural e, não, temporal. Em versões anteriores da UML, era conhecido como Diagrama de Colaboração. Ele possui números que identificam a sequência das mensagens.

[Figura - Diagrama de Comunicação]

#### Quando Utilizar o Diagrama de Comunicação?

A principal questão é quando usá-los em lugar dos diagramas de sequência. Grande parte da decisão é questão de preferência pessoal. Os diagramas de sequência são melhores quando você quer salientar a sequência de chamadas e os de comunicação são melhores quando se quer salientar os vínculos.

### Diagrama de Tempo (ou Temporização)

Este diagrama de interação descreve o comportamento dos objetos no decorrer do tempo e a duração na qual eles permanecem em determinados estados. Ele associa características do Diagrama de Sequência com o de Máquina de Estados. O foco está nas restrições de temporização entre mudanças de estado.

[Figura - Diagrama de Tempo]

#### Quando Utilizar o Diagrama de Temporização?

Os Diagramas de Temporização são úteis para mostrar restrições de temporização entre mudanças de estado em diferentes objetos. São particularmente conhecidos dos Engenheiros de hardware.

### Diagrama de Interação Geral

É um diagrama de interação que mistura o Diagrama de Atividades e o de Sequência. Ele fornece uma visão geral do controle de fluxo entre objetos e engloba diversos tipos de diagramas de interação para demonstrar um processo geral. Ele mostra a troca de mensagens entre objetos e o estado de atividades.

[Figura - Diagrama de Interação Geral]

Pode-se considerar os diagramas de interação geral como diagramas de atividades nos quais as atividades são substituídas por pequenos diagramas de sequência.

#### Quando Utilizar o Diagrama de Interação Geral?

Esses diagramas surgiram a partir da UML 2.0. Martin Fowler expressa que não gosta muito deles, pois acredita que eles misturam dois estilos que não se encaixam muito bem. A recomendação dele é: desenhe um Diagrama de Atividades ou use um Diagrama de Sequência, dependendo do que melhor atender seu propósito.

## Considerações Finais

A Unified Modeling Language (UML) emergiu de um cenário fragmentado para se tornar a linguagem padrão universal para a modelagem de sistemas, trazendo clareza e rigor onde antes havia ambiguidade. Seu conjunto de 14 diagramas oferece múltiplas perspectivas para dissecar e entender a complexidade, desde a arquitetura estática fundamental, detalhada no Diagrama de Classes, até os fluxos de comportamento dinâmico, ilustrados nos diagramas de interação e atividade.

É crucial, no entanto, encarar a UML como o que ela é: uma ferramenta de comunicação e planejamento, não um processo rígido ou um fim em si mesmo. O maior perigo reside na supermodelagem ou no foco excessivo na notação em detrimento do diálogo. Um diagrama só tem valor se ele promove o entendimento compartilhado e ajuda a equipe a construir o software certo, da maneira certa.

Dominar a UML significa, portanto, mais do que memorizar símbolos. Significa entender qual diagrama usar para responder a qual pergunta, como manter os modelos simples e focados no que é essencial, e como usá-los para guiar conversas produtivas entre todos os envolvidos no projeto. Utilizada com pragmatismo e sabedoria, a UML é um ativo inestimável para a engenharia de software moderna.
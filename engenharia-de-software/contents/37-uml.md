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

|**Especificações UML**|**Descrição**|
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

<div align="center">
  <img width="580px" src="./img/37-visoes-da-uml.png">
</div>

## Mecanismos Gerais da UML

A UML contém alguns mecanismos de uso geral muito importantes, que permitem estender e detalhar os modelos para além da notação padrão.

|**Mecanismos UML**|**Descrição**|
|---|---|
|**Estereótipo**|Mecanismo utilizado para estender o significado de um elemento do modelo, criando um "novo tipo" de elemento com semântica específica para um domínio ou plataforma.|
|**Notas Explicativas**|Mecanismo utilizado para adicionar comentários, esclarecimentos ou informações textuais a qualquer parte de um diagrama, sem alterar sua semântica.|
|**Tagged Values**|Mecanismo que permite definir novas propriedades (metadados) para os elementos, sempre associadas a um estereótipo.|
|**Restrições**|Mecanismo utilizado para especificar regras ou condições que devem ser verdadeiras para um ou mais elementos de um modelo.|
|**Pacotes**|Mecanismo utilizado para agrupar elementos semanticamente relacionados, organizando o modelo e controlando sua visibilidade (será detalhado no Diagrama de Pacotes).|

### Estereótipos

Estereótipos permitem adaptar ou personalizar modelos com construções específicas para um domínio, plataforma ou método de desenvolvimento particular. Em outras palavras, é o principal mecanismo de extensão da UML, conferindo-lhe mais poder e flexibilidade. Antes, os modeladores estavam limitados aos elementos padrão; com estereótipos, as possibilidades para representar um sistema se tornam praticamente infinitas.

Existem duas classificações independentes para estereótipos:

1. **Quanto à Origem**:
    - **Predefinidos pela Linguagem**: Já vêm nativamente na UML (Ex: `<<interface>>`, `<<trace>>`, `<<entity>>`).
    - **Definidos pelo Usuário**: A equipe de desenvolvimento pode criar seus próprios estereótipos para representar conceitos específicos do seu projeto (Ex: `<<TelaCadastro>>`, `<<ServidorLegado>>`).
2. **Quanto à Representação**:
    - **Textual**: Representado por um nome entre aspas angulares duplas (Ex: `<<control>>`).
    - **Gráfica**: Representado por um ícone específico que substitui o símbolo padrão do elemento, lembrando visualmente o conceito associado.

<div align="center">
  <img width="520px" src="./img/37-esteriotipos.png">
</div>

Na imagem acima, os dois estereótipos à esquerda são predefinidos, enquanto os dois à direita são exemplos de estereótipos que poderiam ser criados pela equipe. Como as classificações são independentes, é possível ter estereótipos gráficos ou textuais, sejam eles predefinidos ou definidos pelo usuário.

### Notas Explicativas

Notas explicativas são utilizadas para definir informações que comentam ou esclarecem alguma parte de um diagrama. Elas podem conter texto livre ou uma expressão formal utilizando a OCL (Object Constraint Language).

Graficamente, as notas são representadas por um retângulo com um canto superior direito dobrado, como uma "orelhinha". O conteúdo da nota é inserido no interior do retângulo, que por sua vez é ligado ao elemento que se quer comentar através de uma linha tracejada. Ao contrário dos estereótipos, as notas não modificam nem estendem o significado do elemento ao qual estão associadas; ou seja, não criam algo novo, apenas explicam ou documentam um elemento existente no modelo sem alterar sua estrutura ou semântica.

<div align="center">
  <img width="420px" src="./img/37-nota-explicativa.png">
</div>

### Tagged Values (Etiquetas Valoradas)

Elementos UML possuem um conjunto de propriedades predefinidas. As classes, por exemplo, possuem: Nome, Visibilidade, Lista de Atributos e Lista de Operações. As Tagged Values (ou Etiquetas Valoradas) são utilizadas para definir novas propriedades para os elementos de um diagrama. A partir da UML 2.0, as Tagged Values só podem ser utilizadas em conjunto com estereótipos, funcionando como metadados para esses estereótipos. Elas são representadas no formato `{tag = valor}`.

<div align="center">
  <img width="240px" src="./img/37-tagged-values.png">
</div>

### Restrições

A todo elemento da UML está associada uma semântica bem definida. As restrições (constraints) permitem estender ou alterar a semântica natural de um elemento, especificando condições ou regras que devem ser satisfeitas. Esse mecanismo geral especifica limitações sobre um ou mais valores de um ou mais elementos de um modelo.

As restrições podem ser especificadas formalmente, através da OCL, ou informalmente, com texto livre em linguagem natural. Em ambos os casos, devem vir delimitadas por chaves `{}` e, frequentemente, são colocadas dentro de notas explicativas anexadas aos elementos que restringem.

## Visão Geral dos Diagramas UML

A Modelagem Orientada a Objetos ocorre quase sempre por meio da **Unified Modeling Language**. A UML 2.5 descreve 14 tipos de diagramas oficiais, divididos em duas categorias principais, que por sua vez se subdividem.

<div align="center">
  <img width="800px" src="./img/37-diagramas-da-uml.png">
</div>

|**Tipos de Diagramas**|**Descrição**|
|---|---|
|**Diagramas Estruturais**|Representam os aspectos **estáticos** do sistema sob diversas visões. Em outras palavras, esses diagramas apresentam a estrutura do sistema, seus componentes e seus relacionamentos, sem levar em consideração a passagem do tempo. São eles: Diagrama de Classes, de Componentes, de Implantação, de Perfil, de Objetos, de Estrutura Composta e de Pacotes.|
|**Diagramas Comportamentais**|Representam os aspectos **dinâmicos** do sistema como um conjunto de mudanças e interações ao longo do tempo. Descrevem como os processos e funcionalidades do programa se relacionam e se comportam. São eles: Diagrama de Máquina de Estados, de Casos de Uso, de Atividade, de Sequência, de Comunicação, de Visão Geral da Interação e de Tempo.|
|**Diagramas de Interação**|Trata-se de um **subconjunto** dos diagramas comportamentais que focam especificamente no relacionamento dinâmico e colaborativo entre os objetos do sistema e suas trocas de mensagens. Eles enfatizam o controle de fluxo e o fluxo de dados entre os elementos. São eles: Diagrama de Sequência, de Comunicação, de Visão Geral da Interação e de Tempo.|

## Diagramas Estruturais

### Diagrama de Classes

O diagrama de classes é facilmente o diagrama mais conhecido e importante entre todos os outros. Ele descreve as classes e interfaces presentes no sistema, suas características, restrições e os vários tipos de relacionamentos estáticos entre seus objetos. Representam-se também as propriedades e as operações de uma classe, assim como as restrições que se aplicam à maneira como os objetos estão conectados.

<div align="center">
  <img width="920px" src="./img/37-diagrama-de-classes.png">
</div>

Uma classe é uma estrutura classificadora que abstrai um conjunto de objetos que compartilham características, restrições e semânticas similares. Ela define, também, o comportamento de seus objetos através de métodos e o estado por meio de atributos.

Pode-se representar uma classe de diversas formas, dependendo do nível de abstração: primeiro, apenas com nome da classe (mais abstrata); segundo, com nome da classe e suas propriedades; e terceiro, com nome da classe, suas propriedades e suas operações (mais concreta).

<div align="center">
  <img width="580px" src="./img/37-diagrama-de-classes-representacao-de-classes.png">
</div>

A classe com borda dupla na imagem é a representação do que chamamos de **Classe Ativa**, que tem por objetivo representar classes cujos objetos têm um ou mais processos (threads). É possível inserir detalhes de implementação da linguagem de programação escolhida no diagrama.

**Representação de Membros:**

- Um atributo estático é representado sublinhando o nome do atributo.
- Uma operação abstrata é representada com seu nome em itálico.
- Uma operação estática é representada com seu nome sublinhado.

Com relação a visibilidade dos membros, as tabelas abaixo apresentam a diferença de visibilidade dos modificadores da Linguagem Java e a UML.

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

Existem duas diferenças sutis: (1) na UML, um elemento protegido não é visível para elementos dentro do mesmo pacote; e (2) o Nível Pacote da UML é chamado de Nível Default em Java. Segue um exemplo de classe com diferentes modificadores:

<div align="center">
  <img width="360px" src="./img/37-diagrama-de-classes-classe-com-modificadores.png">
</div>

#### Tipos de Relacionamentos

Eles representam as conexões entre classes, objetos, pacotes, tabelas, entre outros. Há três tipos de relacionamentos entre classes: Dependência, Generalização e Associação.

<div align="center">
  <img width="320px" src="./img/37-diagrama-de-classes-representacao-dos-relacionamentos.png">
</div>

- **Relacionamento de Dependência**: É um relacionamento direcionado e semântico entre dois ou mais elementos que ocorre se mudanças na definição de um elemento (independente) causarem mudanças ao outro elemento (dependente). Em outras palavras, é quando a classe cliente é dependente de algum serviço da classe fornecedora. O relacionamento de dependência é representado por uma seta tracejada que aponta para classe independente. Os relacionamentos `<<include>>` e `<<extend>>` também são relacionamentos de dependência.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-classes-relacionamento-de-dependencia.png">
</div>

- **Relacionamento de Generalização/Especialização (Herança)**: Indica que a subclasse é uma especialização da superclasse ou que a superclasse é uma generalização da subclasse. Qualquer instância da subclasse é também uma instância da superclasse. É conhecido como relacionamento de herança, relacionamento de extensão ou relacionamento “é-um”. É representado por uma linha com um triângulo que aponta para a classe genérica.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-classes-relacionamento-de-heranca.png">
</div>

- **Relacionamento de Realização**: Relacionamento entre dois elementos em que um elemento realiza (implementa/executa) o comportamento que o outro elemento especifica. Costuma-se dizer que um dos elementos especifica um contrato e o outro elemento realiza esse contrato. É representado por uma linha tracejada com um triângulo que aponta para a interface.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-classes-relacionamento-de-realizacao.png">
</div>

- **Relacionamento de Associação:** Relacionamento estrutural entre objetos que especifica os objetos de uma classe que estão ligados a objetos de outra classe.

- **Associação Simples**: É um tipo de relacionamento mais forte que o de dependência e indica que uma instância de um elemento está ligada à instância de outro elemento. São representados por uma linha sólida com ou sem setas de navegabilidade. Pode haver nomes para a associação e indicação de multiplicidade.

<div align="center">
  <img width="420px" src="./img/37-diagrama-de-classes-relacionamento-de-associacao-simples.png">
</div>

- **Associação Qualificada**: É um tipo de relacionamento similar à associação simples, contudo possui um qualificador, que é um atributo do elemento-alvo capaz de identificar uma instância dentre as demais. Ela ocorre em associações um-para-muitos ou muitos-para-muitos em que se deseja encontrar um elemento específico dada uma chave. É representado por uma linha sólida com um retângulo ao lado da classe de cardinalidade 1 contendo o qualificador.

<div align="center">
  <img width="560px" src="./img/37-diagrama-de-classes-relacionamento-de-associacao-qualificada.png">
</div>

- **Agregação**: É um tipo de associação, porém mais forte, em que o todo está relacionado às suas partes de forma independente. Nesse tipo de relacionamento, as partes têm existência própria. É representado por uma linha com um diamante vazio na extremidade referente ao todo.

<div align="center">
  <img width="600px" src="./img/37-diagrama-de-classes-relacionamento-de-agregacao.png">
</div>

- **Composição**: É um tipo de agregação (inclusive, é chamado algumas vezes de Agregação por Composição), porém mais forte, em que o todo está relacionado às partes de forma dependente. Nesse relacionamento, as partes não têm existência própria. É representado por uma linha com um diamante cheio na extremidade referente ao todo.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-classes-relacionamento-de-composicao.png">
</div>

Segue um resumo visual geral dos relacionamentos:

<div align="center">
  <img width="580px" src="./img/37-diagrama-de-classes-resumo-dos-relacionamentos.png">
</div>

- **Associação**: mostra que os passageiros e o avião possuem alguma ligação;
- **Dependência**: mostra que o avião depende de passageiros;
- **Associação Reflexiva**: mostra que a equipe do avião se relaciona entre si;
- **Multiplicidade**: mostra que um avião possui zero ou mais passageiros;
- **Agregação**: mostra que uma biblioteca tem um ou mais livros (independentes). Nesse entendimento, livros continuarão a existir mesmo sem bibliotecas;
- **Composição**: mostra que uma biblioteca tem livros (necessariamente). Nesse entendimento, livros só podem existir como parte de uma biblioteca;
- **Herança**: mostra que uma conta bancária é filha de conta fixa;
- **Realização**: setup da impressora implementa funcionalidades da impressora.

#### Quando Utilizar o Diagrama de Classes?

Os Diagramas de Classe são considerados a espinha dorsal da UML e, por isso, são utilizados constantemente. No entanto, seu maior problema reside na riqueza de detalhes, que pode torná-los excessivamente complexos. Por esse motivo, não se recomenda o uso de todas as notações disponíveis. A utilização de diagramas de classes conceituais é especialmente útil para a exploração da linguagem do negócio. É importante manter o foco fora do software durante a discussão e adotar uma notação mais simples. Não é necessário modelar todos os aspectos; o ideal é concentrar-se nas áreas mais relevantes. É preferível dispor de poucos diagramas que sejam efetivamente utilizados e mantidos atualizados do que ter uma grande quantidade de modelos esquecidos e obsoletos. O principal risco está em concentrar-se unicamente na estrutura, negligenciando o comportamento. Por isso, ao elaborar diagramas de classe com o objetivo de compreender o software, é essencial associá-los a alguma técnica comportamental.

### Diagrama de Objetos

O **Diagrama de Objetos** (também conhecido como **Diagrama de Instâncias**) é uma variação do Diagrama de Classes que representa uma **fotografia estática do sistema em um dado momento da execução**. Ao contrário do Diagrama de Classes, que apresenta a estrutura geral do sistema por meio de suas classes, o Diagrama de Objetos mostra **instâncias específicas**, ou seja, objetos reais que estão ativos em determinado instante. Por essa razão, é comum que esse tipo de diagrama seja referido como **Diagrama de Instâncias**.

Enquanto no Diagrama de Classes se modela, por exemplo, a classe `Pessoa`, no Diagrama de Objetos se encontra a instância específica “João”. Em vez de `Carro`, tem-se “Pálio”; em vez de `Cachorro`, tem-se “Totó”. Portanto, este diagrama é uma representação concreta e personalizada dos elementos do sistema em tempo de execução, contendo **valores reais de atributos e os vínculos entre os objetos** naquele momento.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-objetos.png">
</div>

Em termos técnicos, os elementos de um Diagrama de Objetos são chamados de **especificações de instância**, e não instâncias verdadeiras. Isso se deve ao fato de que, nesses diagramas, é aceitável **deixar atributos obrigatórios em branco** ou até mesmo representar **instâncias de classes abstratas** – situações que não ocorreriam durante a execução real do sistema, mas que são úteis para fins de modelagem.

A **sintaxe dos nomes** em um Diagrama de Objetos segue a forma geral:

```
NomeDeInstância:NomeDaClasse
```

Ambas as partes são opcionais, mas se for utilizado apenas o nome da classe, os dois-pontos devem ser mantidos. Quando os nomes estão **sublinhados**, significa que o elemento representado é uma instância.

A tabela a seguir apresenta as formas mais comuns de representação de nomes:

|Representação do Nome|Descrição|
|---|---|
|`:`|Significa que se trata de uma instância anônima de uma classe anônima.|
|`:Cliente`|Significa que se trata de uma instância anônima da classe Cliente.|
|`NovoCliente:`|Significa que se trata de uma instância NovoCliente de uma classe anônima.|
|`NovoCliente:Cliente`|Significa que se trata de uma instância NovoCliente de uma classe Cliente.|
|`NovoCliente:Clientes::Cliente`|Significa que se trata de uma instância NovoCliente de uma classe Cliente de um pacote Clientes.|

#### Quando Utilizar o Diagrama de Objetos?

O Diagrama de Objetos é especialmente útil quando se deseja **ilustrar exemplos concretos de objetos interligados**, permitindo uma visualização mais clara das relações em tempo de execução. Em muitos casos, mesmo com um Diagrama de Classes bem definido, a estrutura geral do sistema pode parecer abstrata ou difícil de compreender. Nesses contextos, o Diagrama de Objetos pode **facilitar a validação, o entendimento do domínio modelado e a comunicação entre analistas e desenvolvedores**.

Esse tipo de diagrama pode ser comparado a um **Diagrama de Comunicação sem as mensagens**, já que se concentra na disposição e nos relacionamentos entre os objetos, sem representar explicitamente o fluxo de interações entre eles.

Apesar de não ser tão utilizado quanto outros diagramas da UML, sua aplicação é valiosa em situações onde se deseja **esclarecer aspectos específicos de um Diagrama de Classes**, principalmente para ilustrar **relações complexas** entre os objetos do sistema.

### Diagrama de Componentes

O Diagrama de Componentes representa o sistema sob uma perspectiva funcional, expondo a organização de seus módulos e as relações entre seus componentes por meio de interfaces. Um **componente** é uma unidade independente, que pode ser utilizada ou substituída com/por outros componentes para formar um sistema complexo. Eles representam peças que podem ser adquiridas e atualizadas independentemente, como tabelas, documentos, etc. Sua grande vantagem é a modularidade.

<div align="center">
  <img width="720px" src="./img/37-diagrama-de-componentes.png">
</div>

Os componentes são representados como retângulos com o símbolo de componente no canto superior direito.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-componentes-representacao-de-componentes.png">
</div>

Um componente pode apresentar um estereótipo. Os principais são:

- `<<Arquivo>>`: determina que o componente é um arquivo de dados do sistema.
- `<<Biblioteca>>`: determina que o componente é uma biblioteca de código.
- `<<Documento>>`: determina que o componente é um documento de sistema.
- `<<Executável>>`: determina que o componente é um arquivo executável.
- `<<Tabela>>`: determina que o componente é uma tabela de um banco de dados.

<div align="center">
  <img width="400px" src="./img/37-diagrama-de-componentes-representacao-de-esteriotipos.png">
</div>

Temos também a **Interface Fornecida** (esquerda), que designa uma interface que o próprio componente possui e oferece para outros componentes, e a **Interface Requerida** (direita), que designa uma interface necessária para que o componente se comunique com outros.

<div align="center">
  <img width="480px" src="./img/37-diagrama-de-componentes-representacao-de-interfaces.png">
</div>

#### Quando Utilizar o Diagrama de Componentes?

Utiliza-se o **Diagrama de Componentes** quando se deseja representar a **organização física do sistema**, ou seja, **como o software é dividido em partes reutilizáveis e independentes** — os chamados **componentes**. Esse tipo de diagrama é ideal para **visualizar, documentar e projetar a arquitetura do sistema** em termos de módulos ou unidades de implementação, especialmente em sistemas grandes e complexos. É útil para:

- **Dividir o sistema em módulos reutilizáveis** e independentes;
- **Mostrar a comunicação entre componentes** através de interfaces fornecidas e requeridas;
- **Representar a decomposição interna de um componente**, revelando suas partes e dependências;
- **Documentar a arquitetura física** do sistema, como bibliotecas, pacotes, arquivos compilados ou serviços externos;
- **Projetar sistemas baseados em serviços** ou microsserviços, com foco na integração de módulos.

Ideal para fases de **arquitetura, integração e implantação**, esse diagrama facilita a visualização da **estrutura física e das dependências** do software.

### Diagrama de Pacotes

O **Diagrama de Pacotes** é utilizado para representar **pacotes e as dependências existentes entre eles** dentro de um sistema. Mas afinal, o que é um pacote? Na UML, um pacote é um **agrupamento lógico de elementos modelados**, que permite organizar classes, interfaces, componentes e outros artefatos em **unidades de mais alto nível**. Embora os pacotes comumente agrupem classes, eles também podem conter outros pacotes, formando uma **estrutura hierárquica**, em que pacotes de nível superior são subdivididos em **subpacotes**, e assim sucessivamente.

<div align="center">
  <img width="540px" src="./img/37-diagrama-de-pacotes.png">
</div>

Visualmente, os pacotes são representados como **pastas**, podendo conter **classes ou outros pacotes internos**. Esse tipo de diagrama fornece uma **visão macro da organização do sistema**, facilitando o entendimento de sua **arquitetura em camadas ou módulos**. A ideia principal é permitir uma separação clara entre diferentes partes da aplicação, promovendo **melhor modularidade, reutilização de código e manutenção**.

O Diagrama de Pacotes é particularmente útil quando se deseja representar **dependências entre agrupamentos lógicos**, como as relações entre diferentes módulos, subsistemas ou camadas arquiteturais de uma aplicação. Ele é uma ferramenta eficaz para ilustrar como os pacotes se comunicam, **quem depende de quem**, e até mesmo para identificar **possíveis acoplamentos excessivos** ou **ciclos indesejados de dependência**, o que é essencial em projetos de grande escala.

#### Quando Utilizar o Diagrama de Pacotes?

Os Diagramas de Pacote devem ser utilizados quando se busca **compreender e gerenciar a estrutura modular de sistemas grandes e complexos**. Eles permitem obter uma **visão clara das dependências entre os principais elementos** da aplicação, o que é fundamental para **manter a arquitetura organizada e coesa**. Além disso, esses diagramas refletem bem as **estruturas típicas de programação**, como diretórios, namespaces ou módulos de código-fonte.

Outro ponto relevante é que os diagramas de pacotes atuam como um mecanismo de agrupamento em **tempo de compilação**, ou seja, ajudam a organizar os artefatos de software ainda durante o desenvolvimento. Caso se deseje representar **a composição dinâmica de objetos em tempo de execução**, recomenda-se, em vez disso, a utilização do **Diagrama de Estrutura Composta**, que é mais apropriado para esse fim.

### Diagrama de Implantação (ou Instalação)

O **Diagrama de Implantação**, também chamado de **Diagrama de Instalação** ou **Diagrama de Distribuição**, tem como principal objetivo representar o **layout físico de um sistema**, indicando **quais partes do software estão instaladas e executadas em quais partes do hardware**. É um dos principais diagramas da UML utilizados durante as fases de **implantação, configuração e manutenção** de sistemas distribuídos ou complexos.

<div align="center">
  <img width="640px" src="./img/37-diagrama-de-implantacao.png">
</div>

Nesse tipo de diagrama, os elementos centrais são os **nós**, que representam unidades físicas ou lógicas de execução. Os nós são conectados por **caminhos de comunicação**, que mostram como ocorre a troca de informações entre eles. Um **nó** pode ser um **dispositivo físico**, como um servidor ou computador, ou um **ambiente de execução**, como um sistema operacional, uma máquina virtual, um contêiner ou qualquer outro software capaz de hospedar componentes executáveis.

Os nós contêm **artefatos**, que são as **manifestações físicas do software** – ou seja, arquivos que serão efetivamente implantados no ambiente de execução. Esses artefatos podem incluir arquivos **executáveis** (como `.exe` ou binários), arquivos de **dados**, arquivos de **configuração**, **scripts**, **documentos HTML**, entre outros. A presença de um artefato em um nó indica que ele está **instalado naquele ponto específico da infraestrutura**. Em UML, os artefatos podem ser representados de duas formas: como **caixas nomeadas com a palavra-chave `<<artifact>>`** ou apenas listados dentro do nó.

Os nós e artefatos podem ser **rotulados com informações adicionais**, como o nome do fornecedor, o sistema operacional utilizado, a localização física do servidor ou qualquer outro detalhe relevante para o contexto de implantação. Em cenários em que múltiplos nós físicos realizam a **mesma função lógica**, pode-se representar isso utilizando **várias instâncias de nós** ou indicando a **quantidade** com um valor afixado no diagrama.

Os **caminhos de comunicação** entre os nós indicam **como os componentes trocam dados**, podendo ser rotulados com os **protocolos utilizados**, como HTTP, FTP, TCP/IP, etc. Esses caminhos representam as **interfaces físicas ou lógicas de comunicação**, tais como redes locais, conexões remotas, filas de mensagens ou outras formas de integração.

Um aspecto importante é que **os Diagramas de Implantação não dizem respeito diretamente à codificação do sistema**, sendo mais relevantes para a **equipe de infraestrutura, operações ou DevOps**, que se responsabiliza por definir a **topologia física do sistema, a distribuição dos componentes de software e os requisitos de ambiente**.

#### Quando Utilizar o Diagrama de Implantação?

O Diagrama de Implantação é especialmente útil em **sistemas distribuídos, aplicações corporativas complexas ou soluções que exigem múltiplos ambientes**, como servidores de aplicação, bancos de dados, serviços em nuvem, gateways de comunicação, entre outros. Ele permite compreender **o que está instalado, onde está instalado e como os componentes interagem fisicamente entre si**.

Sempre que a implantação envolver **diversos nós físicos ou virtuais**, **arquiteturas baseadas em múltiplos servidores**, **distribuição em ambientes heterogêneos** ou **requisitos específicos de comunicação entre módulos**, esse diagrama se torna essencial para **garantir clareza, rastreabilidade e organização na entrega do sistema**.

### Diagrama de Perfil

O **Diagrama de Perfil** é utilizado quando se deseja **especializar a UML para uma área de domínio específica** ou para um propósito particular, como geração de código em uma linguagem específica ou modelagem de sistemas não contemplados diretamente pelos elementos padrões da UML. Em vez de criar uma nova linguagem de modelagem, o Diagrama de Perfil permite **estender a própria UML** utilizando elementos definidos sobre sua estrutura original.

<div align="center">
  <img width="720px" src="./img/37-diagrama-de-perfil.png">
</div>

Esses perfis são compostos por um **conjunto de estereótipos predefinidos, valores atribuídos, restrições e metaclasses base**, selecionando um **subconjunto dos elementos da UML** para uso e adaptando-os a contextos específicos. Isso significa que um perfil **não é uma nova linguagem de modelagem**, mas sim uma **customização da UML**, podendo ser interpretado por qualquer ferramenta que suporte a linguagem padrão. Por isso, os perfis são amplamente utilizados por **criadores de frameworks, desenvolvedores de ferramentas e arquitetos de software**, enquanto os modeladores em geral apenas **utilizam** esses perfis prontos, sem a necessidade de criá-los do zero.

Um exemplo comum de uso de perfis é a especialização da UML para **linguagens de programação específicas**, como Java ou C++. Cada linguagem pode demandar estereótipos distintos para orientar a geração de código – por exemplo, um estereótipo `<<DAO>>` pode ser usado para marcar uma classe como um objeto de acesso a dados em Java. Também é possível utilizar perfis para modelar **bancos de dados**, **redes de computadores** (com estereótipos como `<<roteador>>` ou `<<switch>>`), **sistemas embarcados**, **processos de negócio** ou qualquer outro domínio técnico.

A UML já oferece **estereótipos padronizados**, como `<<interface>>`, `<<include>>`, `<<extend>>` e `<<actor>>`. Contudo, o modelador pode criar **seus próprios estereótipos personalizados** para representar relações ou elementos semânticos específicos que não estão contemplados na linguagem padrão. Por exemplo, caso se queira expressar a ação de sacar dinheiro em um relacionamento entre "Pessoa" e "Dinheiro", pode-se criar o estereótipo `<<sacar>>` e usá-lo na modelagem para representar semanticamente esse vínculo.

#### Perfis, Metamodelos e Metaclasses

Para compreender plenamente o Diagrama de Perfil, é necessário entender o **nível de metamodelagem** em que ele opera. A UML é baseada em um metamodelo, ou seja, **uma linguagem que define os elementos que podem compor um modelo**. Em outras palavras, enquanto um **modelo** descreve uma visão específica de um sistema real (por exemplo, um modelo que representa a estrutura de um sistema bancário), o **metamodelo** descreve **os conceitos e regras usados para construir esse modelo** (como classe, associação, atributo, operação, etc.).

Cada elemento de um modelo UML é uma **instância de um elemento do metamodelo**. Assim, quando desenhamos uma classe, estamos instanciando a metaclasse `Class` do metamodelo da UML. Quando aplicamos um estereótipo sobre uma classe, estamos estendendo a semântica dessa metaclasse com um novo significado ou comportamento.

A **metaclasse** é, portanto, uma classe do metamodelo que pode ser **estendida por estereótipos**, adicionando atributos, restrições e propriedades adicionais. Isso permite que os perfis expressem **variações semânticas ou sintáticas** de um elemento UML para contextos específicos, mantendo a conformidade com a linguagem padrão.

#### Quando Utilizar o Diagrama de Perfil?

Utiliza-se o Diagrama de Perfil quando há a necessidade de **personalizar a UML para um domínio de aplicação, ferramenta, linguagem de programação ou contexto técnico específico**, sem romper com a estrutura da linguagem. Ele é particularmente útil:

- Quando se deseja **adicionar semântica adicional** aos elementos da UML usando **estereótipos personalizados**;
- Quando é necessário **definir regras específicas** de modelagem que não estão contempladas na UML padrão;
- Para **aumentar a expressividade do modelo**, tornando-o mais próximo da realidade do domínio representado;
- Em projetos que utilizam **ferramentas de geração automática de código, documentação ou validação**, onde os estereótipos orientam o comportamento das ferramentas;
- Para **organizar modelos reutilizáveis** em **bibliotecas de modelagem específicas de domínio**, aumentando a produtividade e a padronização em equipes de desenvolvimento.

Em resumo, o Diagrama de Perfil permite que a UML **se adapte aos mais variados contextos sem perder sua base formal**, oferecendo uma forma estruturada e extensível de aplicar a linguagem em qualquer tipo de sistema.

### Diagrama de Estrutura Composta

O **Diagrama de Estrutura Composta** é utilizado para **modelar a estrutura interna de elementos de modelagem**, como **classes, interfaces e componentes**, revelando as **colaborações internas** necessárias para realizar uma determinada funcionalidade. A ideia central é representar **como as partes de um sistema colaboram entre si em tempo de execução**, indo além da estrutura externa mostrada em diagramas de classes tradicionais.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-estrutura-composta.png">
</div>

Um conceito-chave nesse tipo de diagrama é o de **colaboração**, que pode ser entendida como uma **visão de um conjunto de entidades cooperativas**, geralmente instâncias de classes ou componentes, que **trabalham juntas para cumprir um comportamento ou realizar uma operação específica**. Uma colaboração, inclusive, pode ser composta por outras colaborações, reforçando o caráter hierárquico e dinâmico desse tipo de modelagem.

O Diagrama de Estrutura Composta permite detalhar **como um elemento é construído internamente**, destacando suas **partes constituintes**, suas **conexões e interações internas** e as **interfaces pelas quais se comunica com o exterior**. Ele é especialmente útil para especificar **a composição de componentes, a conexão entre objetos internos e os pontos de interação com o meio externo**, tudo isso dentro do escopo de uma única entidade modelada.

Esse diagrama surgiu com a introdução da **UML 2.0** e representou um avanço em relação à modelagem estrutural, fornecendo meios de capturar informações mais ricas sobre **a organização interna de classes e componentes**, o que antes não era possível com os diagramas disponíveis na UML 1.x.

#### Quando Utilizar o Diagrama de Estrutura Composta?

O Diagrama de Estrutura Composta deve ser utilizado quando há a necessidade de **representar a composição interna de um elemento em tempo de execução**, especialmente em contextos onde a **interação entre partes internas** é relevante para o funcionamento do sistema.

Uma boa analogia para diferenciá-lo de outros diagramas é compará-lo ao Diagrama de Pacotes: enquanto os **Diagramas de Pacote** representam **agrupamentos lógicos em tempo de compilação**, as **estruturas compostas mostram a composição dinâmica dos elementos em tempo de execução**. Assim, esse diagrama é indicado para modelar **componentes divididos internamente em partes**, **classes que se comunicam com subcomponentes internos** ou **interfaces que conectam blocos funcionais dentro de uma aplicação**.

Apesar de sua proposta rica e detalhada, é importante destacar que o Diagrama de Estrutura Composta **ainda é pouco utilizado na prática**. Muitos modeladores e engenheiros de software consideram sua notação excessivamente complexa ou redundante em comparação com outros diagramas já consolidados, como os de **componentes**, **classes** e **sequência**. Inclusive, há registros de que dentro do próprio comitê organizador da UML houve discussões quanto à real utilidade e aplicação prática desse diagrama.

Mesmo assim, em sistemas complexos, orientados a componentes, ou que requerem uma descrição formal da **composição e conectividade interna**, o Diagrama de Estrutura Composta pode ser uma ferramenta poderosa para enriquecer a documentação e a compreensão do projeto.

## Diagramas Comportamentais

### Diagrama de Casos de Uso

O **Diagrama de Casos de Uso** é uma poderosa ferramenta para capturar os **requisitos funcionais de um sistema**. Ele permite representar, de maneira gráfica e textual, **como os usuários interagem com o sistema** e quais funcionalidades estão disponíveis. Esses diagramas têm como objetivo principal fornecer uma **visão externa do sistema**, ou seja, o foco está no **comportamento percebido pelos usuários** – não na estrutura interna do software.

<div align="center">
  <img width="820px" src="./img/37-diagrama-de-casos-de-uso.png">
</div>

Os elementos centrais desse diagrama são os **casos de uso**, que representam funcionalidades oferecidas pelo sistema, e os **atores**, que são os agentes externos que interagem com ele. Um ator pode ser uma pessoa, uma máquina ou até outro sistema, desde que desempenhe um papel relevante na interação com a aplicação. Um **cenário**, por sua vez, é uma instância específica de um caso de uso – ou seja, um fluxo particular de eventos que representa uma execução concreta da funcionalidade modelada.

Considere, por exemplo, uma loja virtual. O caso de uso “Realizar Compra” pode incluir cenários distintos, como uma compra bem-sucedida ou uma tentativa frustrada por erro no cartão de crédito. Esses diferentes caminhos compõem os possíveis **cenários** de execução do caso de uso.

Os casos de uso podem ser classificados em **concretos**, quando representam fluxos completos iniciados por um ator, e **abstratos**, quando não são executados diretamente, mas reutilizados por outros casos de uso por meio de **inclusão**, **extensão** ou **herança**. A UML recomenda que casos abstratos sejam representados com o nome em itálico. Além disso, é comum categorizar casos de uso como **primários**, quando refletem os objetivos diretos dos atores (ex.: “Efetuar Pagamento”), e **secundários**, quando são necessários para o funcionamento do sistema, mas não produzem benefícios diretos ao usuário (ex.: “Validar Sessão”).

No diagrama, os relacionamentos assumem formas distintas, cada uma com sua finalidade. O **relacionamento de comunicação** (ou associação) conecta um ator a um caso de uso e indica que há troca de mensagens entre eles. Esse é o tipo mais básico e representa simplesmente que o ator pode acionar aquela funcionalidade.

<div align="center">
  <img width="420px" src="./img/37-diagrama-de-casos-de-uso-comunicacao.png">
</div>

Já o **relacionamento de inclusão** (`<<include>>`) indica que um comportamento comum e obrigatório está sendo reutilizado. Por exemplo, se ao realizar qualquer operação em um internet banking é preciso se autenticar, o caso de uso “Login” será incluído em diversos outros. Sempre que o caso de uso A **inclui** o caso de uso B, significa que **B será executado sempre que A for acionado**. A seta parte do caso de uso que inclui e aponta para o incluído.

<div align="center">
  <img width="420px" src="./img/37-diagrama-de-casos-de-uso-inclusao.png">
</div>

O **relacionamento de extensão** (`<<extend>>`) é utilizado para representar comportamentos **opcionais** ou **alternativos**. Por exemplo, ao cadastrar um usuário, pode haver extensões para definir se ele será um administrador ou moderador. Quando o caso de uso B **estende** o caso de uso A, significa que **B pode (mas não necessariamente será) executado durante A**. Aqui, a seta parte do extensor e aponta para o estendido.

<div align="center">
  <img width="420px" src="./img/37-diagrama-de-casos-de-uso-extensao.png">
</div>

O **relacionamento de herança** pode ocorrer tanto entre atores quanto entre casos de uso. No caso dos atores, serve para representar papéis especializados derivados de um papel genérico. Um exemplo clássico seria ter um ator “Pessoa” generalizando “Cliente” e “Vendedor”. Já entre casos de uso, a herança permite reaproveitamento de comportamentos e especializações de funcionalidades, promovendo maior organização e reutilização no modelo.

<div align="center">
  <img width="80px" src="./img/37-diagrama-de-casos-de-uso-heranca.png">
</div>

Para visualizar essas possibilidades, a tabela a seguir resume os relacionamentos possíveis:

|Relações|Comunicação|Extensão|Inclusão|Herança|
|---|---|---|---|---|
|Entre casos de uso|-|✔|✔|✔|
|Entre atores|-|-|-|✔|
|Entre casos de uso e atores|✔|-|-|-|

#### Quando Utilizar o Diagrama de Casos de Uso?

O Diagrama de Casos de Uso é indicado principalmente **nas fases iniciais do desenvolvimento de software**, sendo uma excelente ferramenta para **elaboração e validação de requisitos funcionais** com os stakeholders. Ele oferece uma maneira intuitiva de representar o sistema sob a perspectiva do usuário final, sem a necessidade de conhecimento técnico aprofundado.

Sua utilização é recomendada para esclarecer os serviços que o sistema deverá oferecer, especialmente em projetos orientados a requisitos, e permite capturar os **objetivos dos usuários**, facilitando o diálogo com clientes e usuários. Contudo, é importante lembrar que o **valor principal da técnica não está apenas no diagrama gráfico, mas no texto descritivo associado a cada caso de uso**. É esse texto que detalha os fluxos normais e alternativos, os pré-requisitos, as exceções e os resultados esperados.

Apesar da utilidade, há um risco comum: tornar os casos de uso **excessivamente detalhados e complexos**, o que pode comprometer sua clareza e manutenção. Em geral, recomenda-se que cada caso de uso não ultrapasse **duas páginas de descrição textual**. Mais do que isso pode desencorajar a leitura e dificultar a análise. Também é fundamental não esperar que haja correlação direta entre casos de uso e as classes ou componentes internos do sistema – eles representam **o comportamento visível externamente**, e não a implementação.

Por fim, vale destacar que Diagramas de Casos de Uso **não são adequados para descrever o design técnico ou a lógica interna do sistema**, mas sim para capturar **interações funcionais percebidas externamente**. Quando bem utilizados, são uma base sólida para os próximos passos do projeto, como a modelagem de atividades, a definição de testes funcionais e a identificação de componentes do sistema.

### Diagrama de Atividades

O **Diagrama de Atividades** é uma poderosa ferramenta da UML utilizada para **descrever a lógica de procedimentos, processos de negócio e fluxos de trabalho**. De certa forma, ele cumpre um papel similar ao dos fluxogramas tradicionais, mas vai além ao **suportar comportamentos paralelos**, o que o torna extremamente útil na modelagem de processos mais complexos e realistas.

Uma **atividade**, nesse contexto, é definida como um **comportamento parametrizado**, que é representado como um **fluxo coordenado de ações**. O foco desse diagrama não está nas interações entre objetos, mas sim na **sequência e no controle dos processos**, geralmente de **nível mais alto**, como rotinas de negócio, tarefas automatizadas ou manuais, e até mesmo sistemas que operam em paralelo.

<div align="center">
  <img width="720px" src="./img/37-diagrama-de-atividades.png">
</div>

Um dos elementos mais marcantes do Diagrama de Atividades são as **swimlanes** (ou raias), que organizam visualmente as ações de acordo com suas **responsabilidades ou papéis**. Cada raia representa um agente (como uma pessoa, sistema ou departamento) responsável por realizar as atividades que ocorrem dentro dela. Essas raias se assemelham a pistas de uma piscina olímpica, delimitando claramente quem faz o quê dentro do fluxo do processo.

Outro recurso importante é a **ramificação**, representada por um **losango (diamante)**, que indica **decisões condicionais** baseadas em expressões booleanas. A partir desse ponto, o fluxo segue por caminhos diferentes, conforme o resultado de uma condição. Por exemplo, após uma validação, o processo pode seguir para “Aprovar Pedido” ou “Rejeitar Pedido”, dependendo do resultado lógico.

Além disso, o diagrama suporta a **bifurcação** e a **união** de fluxos. A bifurcação (**fork**) é utilizada para dividir um fluxo em **vários caminhos paralelos**, ou seja, várias atividades podem ocorrer ao mesmo tempo. Ela é representada por uma **barra horizontal ou vertical**, com uma transição de entrada e múltiplas saídas. Cada fluxo segue paralelamente a partir desse ponto.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-atividades-nodes-forks-e-joins.png">
  </div>

A união (**join**), por sua vez, é usada para **sincronizar** esses fluxos paralelos, fazendo com que o processo só continue após todos os caminhos simultâneos terem sido concluídos. Também é representada por uma barra, com **múltiplas entradas e uma única saída**. A presença dessas construções torna o diagrama de atividades ideal para representar processos com **convergência e divergência de controle**.

#### Quando Utilizar o Diagrama de Atividades?

O Diagrama de Atividades é extremamente útil quando se deseja **modelar comportamentos sequenciais e paralelos de um sistema**, especialmente no contexto de **fluxos de trabalho organizacionais, processos de negócio ou lógica de algoritmos complexos**. Sua principal vantagem está justamente na capacidade de representar **comportamento paralelo de maneira natural**, algo que fluxogramas tradicionais não fazem bem.

Na prática, isso o torna ideal para **analisar processos administrativos, validar requisitos com usuários finais, documentar procedimentos operacionais**, e até mesmo para desenhar o fluxo de controle de algoritmos ou métodos.

Com a evolução da UML para a versão 2.x, esse diagrama ganhou ainda mais destaque, especialmente pelo **interesse crescente em modelar processos de negócio e workflow**, áreas em que o comportamento paralelo e os fluxos alternativos são comuns. Em contextos mais avançados, há até mesmo propostas de uso da UML como uma linguagem de programação visual, com **ferramentas capazes de converter Diagramas de Atividades em código executável** — embora essa ideia ainda esteja em estágio experimental.

### Diagrama de Máquina de Estados

O **Diagrama de Máquina de Estados**, também conhecido como **Diagrama de Transição de Estados**, é utilizado para representar os **diferentes estados que um objeto pode assumir ao longo de sua existência** em um sistema, bem como os eventos que causam as **transições** entre esses estados. Ele é particularmente útil para visualizar o **ciclo de vida de um objeto**, desde o estado inicial até seu possível encerramento, mostrando como eventos internos ou externos influenciam esse comportamento.

<div align="center">
  <img width="640px" src="./img/37-diagrama-de-maquina-de-estados.png">
  </div>

Um **estado** representa uma situação em que um objeto satisfaz uma condição, realiza uma atividade ou espera por um evento. Por sua vez, uma **transição** é o movimento de um estado para outro, geralmente acionado por um **evento** ou estímulo. E o que é uma **ação**? É uma atividade que ocorre como consequência da transição. Assim, esse diagrama mostra **como o objeto reage a diferentes estímulos** ao longo do tempo, evidenciando **as regras que regem seu comportamento dinâmico**.

Vale destacar que o Diagrama de Máquina de Estados **não mostra a interação entre objetos**, como fazem outros diagramas da UML. Ele foca **no comportamento interno de um único objeto** — como, por exemplo, um pedido de venda, um usuário logado ou uma conexão de rede — e como esse objeto muda de estado ao longo do tempo em resposta a eventos como erros, comandos, mensagens, aprovações, etc.

Embora cada diagrama comece com **um único estado inicial**, ele pode apresentar **vários estados finais**, dependendo das possíveis rotas que o objeto pode seguir. Esse tipo de representação facilita a análise do ciclo de vida de entidades com **comportamento complexo**, permitindo identificar, por exemplo, **condições de espera, estados intermediários e estados de erro**.

#### Diferenças entre Diagrama de Atividades e Diagrama de Máquina de Estados

Apesar de visualmente semelhantes, os Diagramas de Atividades e os Diagramas de Máquina de Estados possuem **diferenças fundamentais de propósito e semântica**.

No **Diagrama de Atividades**, o foco está nos **nós**, que representam ações ou tarefas sendo executadas. Já os arcos indicam o fluxo de controle entre essas atividades. Ele é útil para **modelar fluxos de trabalho ou lógica de processos**, geralmente de forma mais ampla.

Por outro lado, no **Diagrama de Máquina de Estados**, a ênfase está nos **arcos**, que representam os comportamentos (transições) que fazem um objeto sair de um estado e ir para outro. Os **nós aqui representam os próprios estados** do objeto. Assim, pode-se dizer que **os significados dos elementos se invertem** entre os dois diagramas: os **nós do Diagrama de Estados** equivalem aos **arcos do Diagrama de Atividades**, e vice-versa.

#### Quando Utilizar o Diagrama de Máquina de Estados?

Este tipo de diagrama é mais indicado quando se deseja **entender ou especificar o comportamento de objetos com ciclo de vida bem definido** e sensível a eventos. Isso é particularmente útil para objetos que participam de **vários casos de uso**, ou que passam por diversas fases operacionais com transições condicionadas por **eventos, entradas do usuário, mensagens de outros sistemas ou regras de negócio**.

Embora sejam extremamente poderosos para ilustrar comportamentos complexos, **não é necessário criar um diagrama de estados para todas as classes**. Recomenda-se utilizá-lo apenas para aquelas que possuem **comportamentos dinâmicos relevantes**, como componentes críticos, entidades com múltiplas fases operacionais, fluxos de autorização ou controle de sessões.

Além disso, é comum **combinar Diagramas de Máquina de Estados com outras técnicas** da UML. Por exemplo, **Diagramas de Interação** são mais adequados para mostrar **o comportamento de vários objetos colaborando em um mesmo caso de uso**, enquanto os **Diagramas de Atividades** representam **a sequência geral de ações de um processo**. Cada tipo de diagrama possui seu foco e complementa os demais quando usados em conjunto.

Em suma, o Diagrama de Máquina de Estados é uma excelente ferramenta para **descrever com precisão a evolução de um objeto** ao longo do tempo, facilitando o entendimento de comportamentos condicionais e o controle de estados de entidades essenciais do sistema.

## Diagramas de Interação

### Diagrama de Sequência

O **Diagrama de Sequência** é um dos diagramas de interação da UML mais utilizados, sendo ideal para modelar **a dinâmica de um cenário específico** dentro de um sistema. Ele descreve como os objetos interagem entre si por meio de **mensagens trocadas em uma sequência temporal bem definida**. Seu principal objetivo é capturar **o comportamento de um único caso de uso**, evidenciando a troca de mensagens entre objetos, a criação e destruição de instâncias e o fluxo de execução.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-sequencia.png">
</div>

O diagrama é organizado em dois eixos. O **eixo horizontal** representa os **objetos ou participantes** envolvidos no cenário, cada um identificado em uma "lifeline" (linha de vida). Já o **eixo vertical** representa o **tempo**, avançando de cima para baixo, de forma que **quanto mais abaixo no diagrama, mais tarde a ação ocorre**. A linha de vida é representada por uma linha tracejada que parte do objeto e se estende verticalmente.

As **mensagens** trocadas entre os objetos representam **chamadas de métodos, eventos ou sinais**, e são exibidas como setas horizontais. Uma mensagem indica um **serviço solicitado de um objeto a outro** e pode vir acompanhada de parâmetros. A execução de métodos é geralmente representada por **retângulos sobre a linha de vida**, conhecidos como **barras de ativação**, que mostram o período durante o qual um objeto está executando uma ação.

Por exemplo, imagine o caso de uso "Sacar Dinheiro" em um caixa eletrônico. O diagrama de sequência pode ilustrar esse cenário desde o momento em que o cliente insere o cartão, passando pela validação do PIN, escolha do valor, autorização da transação, até a entrega do dinheiro e a atualização do saldo na base de dados. Cada interação entre o cliente, o sistema de interface, o sistema bancário e os mecanismos internos pode ser representada por uma sequência de mensagens trocadas no tempo.

Esse tipo de diagrama é especialmente útil para **visualizar claramente o fluxo de comunicação entre os componentes**, entender **como o sistema responde a determinadas entradas** e identificar possíveis **gargalos, dependências ou problemas de sincronização** entre objetos.

#### Quando Utilizar o Diagrama de Sequência?

Você deve utilizar o Diagrama de Sequência quando quiser observar **como vários objetos colaboram entre si dentro de um único caso de uso**. Ele é uma excelente ferramenta para **analisar colaborações entre objetos**, ajudar a validar requisitos funcionais e verificar se a arquitetura do sistema está adequada para atender aos fluxos de execução esperados.

No entanto, vale destacar que os Diagramas de Sequência **não são ideais para definir com precisão o comportamento interno de cada objeto individualmente**. Para isso, recomenda-se utilizar o **Diagrama de Máquina de Estados**, que mostra o ciclo de vida de um único objeto. Já para fluxos mais complexos com múltiplas possibilidades, alternativas e paralelismo, o **Diagrama de Atividades** pode ser mais adequado.

Por fim, durante a fase de projeto, é comum utilizar cartões CRC (Classe-Responsabilidade-Colaboração) para explorar rapidamente as alternativas de interação entre objetos. Uma vez que as colaborações estejam bem definidas, os Diagramas de Sequência podem então ser usados para **documentar essas interações em detalhes**.

### Diagrama de Comunicação

O **Diagrama de Comunicação** é uma variação dos diagramas de interação da UML, sendo bastante semelhante ao **Diagrama de Sequência**, mas com uma ênfase distinta: **em vez de destacar a ordem temporal das mensagens**, ele se concentra na **estrutura organizacional dos objetos envolvidos** e nas **ligações entre eles**.

Em versões anteriores da UML, esse diagrama era conhecido como **Diagrama de Colaboração**. No entanto, é importante fazer uma distinção: embora tenha esse nome antigo, o Diagrama de Comunicação **não modela colaborações** no sentido formal — essa função é mais bem representada pelo **Diagrama de Estrutura Composta**. O foco principal do Diagrama de Comunicação está na **disposição dos objetos e nas mensagens trocadas entre eles**, representadas por **setas numeradas** que indicam a sequência das chamadas.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-comunicacao.png">
</div>

Visualmente, ele lembra bastante um **Diagrama de Objetos**, mas com a adição das mensagens e suas respectivas setas e identificadores numéricos. Cada mensagem trocada entre objetos é representada por uma **linha com uma seta**, acompanhada de um número que indica sua **posição relativa na sequência de chamadas** (por exemplo, 1, 2, 3, 3.1, 4...), o que permite compreender o fluxo mesmo sem uma linha de tempo vertical.

A imagem acima mostra um exemplo de Diagrama de Comunicação que corresponde exatamente a um Diagrama de Sequência apresentado anteriormente — a diferença está no enfoque: **aqui, privilegia-se a estrutura e os vínculos entre objetos**, enquanto o Diagrama de Sequência foca a ordem cronológica das mensagens.

Esse tipo de diagrama é útil principalmente em **cenários mais simples**, onde o objetivo é destacar **como os objetos estão conectados** e **quais mensagens são trocadas entre eles**, sem a complexidade de fluxos paralelos ou ramificações detalhadas. Em geral, ele é mais compacto e **mais fácil de modificar rapidamente**, o que o torna adequado para explorar **alternativas de interação** durante a fase de design.

#### Quando Utilizar o Diagrama de Comunicação?

A principal dúvida é: **quando usá-lo em vez do Diagrama de Sequência?** A resposta depende do **objetivo da modelagem** e, muitas vezes, também da **preferência pessoal** da equipe de desenvolvimento. Os **Diagramas de Sequência** são preferíveis quando se quer enfatizar a **ordem das chamadas**, ou quando os detalhes temporais são cruciais para o entendimento do sistema. Já os **Diagramas de Comunicação** são mais adequados quando se deseja ressaltar **as conexões entre objetos e os caminhos de interação**, permitindo uma visão mais estrutural do cenário.

Além disso, por serem mais compactos e fáceis de editar, os Diagramas de Comunicação são particularmente úteis para **experimentar diferentes alternativas de design**, fazendo pequenas alterações na disposição dos elementos ou na direção das mensagens sem reestruturar todo o diagrama, como frequentemente ocorre nos Diagramas de Sequência.

Em suma, o Diagrama de Comunicação é uma poderosa ferramenta complementar aos outros diagramas de interação da UML. Quando usado de forma estratégica, permite **compreender rapidamente como os objetos estão organizados e se comunicam entre si**, especialmente em **cenários de complexidade moderada**, contribuindo para uma análise clara e eficaz da arquitetura do sistema.

### Diagrama de Tempo (ou Temporização)

O **Diagrama de Tempo**, também conhecido como **Diagrama de Temporização**, é uma variação dos diagramas de interação da UML. Seu principal objetivo é descrever o **comportamento de objetos ao longo do tempo**, focando na **duração** que cada objeto permanece em determinados **estados** e nas **restrições temporais** entre essas transições.

Esse tipo de diagrama associa características tanto do **Diagrama de Sequência**, ao representar interações entre objetos, quanto do **Diagrama de Máquina de Estados**, ao representar mudanças de estado em resposta a eventos. A diferença é que, aqui, o **tempo é o protagonista** — as variações de estado são exibidas com precisão em uma linha de tempo.

<div align="center">
  <img width="680px" src="./img/37-diagrama-de-tempo.png">
</div>

No Diagrama de Tempo, cada linha representa um **objeto** (ou instância de classe) e o seu respectivo **estado ao longo do tempo**. O eixo horizontal indica o avanço temporal, e os segmentos de linha demonstram o **estado ou condição** em que o objeto se encontra em determinado intervalo. Assim, o gráfico permite observar de forma clara **quando uma mudança de estado ocorre** e **quanto tempo cada estado persiste**.

Além disso, o diagrama também permite expressar **restrições temporais** — como "o objeto deve permanecer no estado X por no mínimo 5 segundos antes de transitar para Y", ou "a resposta a um evento deve ocorrer em até 2 milissegundos após sua emissão". Essa precisão é extremamente valiosa em sistemas **reativos**, **embarcados** ou que envolvam **hardware sensível ao tempo**.

Embora menos comum em projetos de software convencionais, os Diagramas de Temporização são **bem conhecidos por engenheiros de hardware**, justamente por serem excelentes para ilustrar **tempos de resposta**, **atrasos**, **sincronizações** e **eventos concorrentes**, como acontece em **circuitos digitais**, **sistemas embarcados** e **interfaces de comunicação**.

É importante destacar que o Diagrama de Tempo foi incorporado à UML apenas a partir da versão **2.0**, o que justifica sua menor difusão e baixa frequência em atividades práticas e provas acadêmicas.

#### Quando Utilizar o Diagrama de Temporização?

Os Diagramas de Tempo são úteis **quando se deseja evidenciar restrições de tempo entre mudanças de estado** em objetos distintos ou mesmo em uma única instância. São recomendados especialmente em contextos onde o **tempo de permanência em estados específicos**, a **sincronização entre eventos** ou o **atendimento a requisitos temporais rigorosos** seja um fator crítico de sucesso — como em **sistemas de tempo real**, **comunicações assíncronas**, **protocolos de rede**, **aplicações embarcadas** e **projetos de hardware digital**.

Embora raramente cobrados em avaliações ou utilizados em ambientes de desenvolvimento orientado a software puro, seu conhecimento é relevante para desenvolvedores que trabalham com sistemas híbridos ou que requerem **controle refinado de tempo e resposta**. Em projetos mais simples ou voltados para a lógica de negócio, é comum substituí-los por **Diagramas de Estado** ou **Diagramas de Sequência**, que atendem à maior parte das necessidades de modelagem.

### Diagrama de Interação Geral

O **Diagrama de Interação Geral** (ou **Visão Geral de Interação**) é um tipo de diagrama de interação introduzido na UML 2.0. Sua principal proposta é **combinar elementos do Diagrama de Atividades com o Diagrama de Sequência**, de modo a oferecer uma **visão ampla e integrada do fluxo de controle** entre objetos, destacando **mensagens trocadas** e o **estado das atividades** envolvidas.

<div align="center">
  <img width="760px" src="./img/37-diagrama-de-interacao-geral.png">
  </div>

Esse tipo de diagrama busca representar **processos mais complexos**, nos quais diversos **cenários de interação** estão interligados. Para isso, ele se estrutura como um **Diagrama de Atividades**, porém substituindo atividades simples por **pequenos Diagramas de Sequência** — ou seja, **cada "atividade" é uma interação em si**, representada com maior detalhamento.

Essa abordagem permite visualizar, de maneira ordenada e hierárquica, como **vários fragmentos de interação** (que poderiam estar em diferentes cenários ou ramificações de um caso de uso) se conectam ao longo de um fluxo geral de execução. Ele pode ser encarado, portanto, como um **Diagrama de Sequência fragmentado**, com uso da **notação de atividades para indicar o controle de fluxo**.

Embora o conceito seja interessante, essa mistura de dois estilos diferentes — **sequencial e de fluxo** — é alvo de críticas. O autor Martin Fowler, referência na área de Engenharia de Software e modelagem com UML, expressa ceticismo quanto à sua utilidade prática. Segundo ele, os Diagramas de Interação Geral podem ser confusos por **combinar estilos visuais e semânticos distintos**, o que dificulta a clareza e a leitura.

#### Quando Utilizar o Diagrama de Interação Geral?

O Diagrama de Interação Geral pode ser útil **quando se deseja representar um processo de negócio ou fluxo complexo que envolva múltiplos cenários de interação** entre objetos. Ele é apropriado para situações onde um Diagrama de Atividades **não oferece detalhamento suficiente** e um único Diagrama de Sequência **não é capaz de capturar todas as variações possíveis**.

Entretanto, devido à sua relativa complexidade e menor difusão entre profissionais, **sua aplicação é mais rara** na prática do que os diagramas de sequência ou de atividades isoladamente. Inclusive, Martin Fowler recomenda que, na maioria dos casos, é mais eficaz **optar por um ou outro**, de acordo com o objetivo de modelagem:

- **Se o foco for no controle de fluxo entre atividades**, use um **Diagrama de Atividades**.
- **Se o foco for na ordem de mensagens trocadas entre objetos**, use um **Diagrama de Sequência**.

Ou seja, o Diagrama de Interação Geral deve ser considerado **com cautela**, sendo útil **em casos muito específicos**, mas **não essencial** para a maioria dos projetos de modelagem orientada a objetos.

## Considerações Finais

A **Unified Modeling Language (UML)** emergiu de um cenário fragmentado para se tornar a linguagem padrão universal para a modelagem de sistemas, trazendo clareza e rigor onde antes havia ambiguidade. Seu conjunto de **14 diagramas** oferece múltiplas perspectivas para dissecar e entender a complexidade, desde a arquitetura estática fundamental, detalhada no Diagrama de Classes, até os fluxos de comportamento dinâmico, ilustrados nos diagramas de interação e atividade.

É crucial, no entanto, encarar a UML como o que ela é: uma **ferramenta de comunicação e planejamento**, não um processo rígido ou um fim em si mesmo. O **maior perigo reside na supermodelagem** ou no foco excessivo na notação em detrimento do diálogo. Um diagrama só tem valor se ele promove o entendimento compartilhado e ajuda a equipe a construir o software certo, da maneira certa.

**Dominar a UML significa**, portanto, mais do que memorizar símbolos. Significa **entender qual diagrama usar para responder a qual pergunta**, como manter os modelos simples e focados no que é essencial, e como usá-los para guiar conversas produtivas entre todos os envolvidos no projeto. Utilizada com pragmatismo e sabedoria, a UML é um ativo inestimável para a engenharia de software moderna.
# Capítulo 13 – Metodologias Ágeis

## A Origem do Pensamento Ágil

No **início dos anos 2000**, o cenário do desenvolvimento de software estava marcado por **frustrações, atrasos, orçamentos estourados e produtos finais que raramente atendiam às reais necessidades dos usuários**. Metodologias tradicionais, como o Modelo em Cascata, ainda dominavam o mercado, impondo um processo rígido, sequencial e excessivamente burocrático, que pouco se adaptava à natureza dinâmica e incerta dos projetos de software.

Foi nesse contexto que, em fevereiro de 2001, dezessete profissionais influentes da área de engenharia de software se reuniram em um resort nas montanhas de Utah, Estados Unidos. O encontro informal, que começou com conversas descontraídas sobre experiências frustrantes com projetos mal sucedidos, rapidamente evoluiu para uma troca profunda de ideias sobre **formas alternativas de desenvolver software**. Um dos participantes relatou como o uso do Modelo em Cascata havia causado estouros de orçamento em seu projeto. Outro mencionou falhas decorrentes do descumprimento de prazos, enquanto um terceiro confessou a dificuldade em entregar o escopo originalmente planejado.

Aos poucos, foi surgindo um consenso: talvez fosse hora de repensar os métodos de desenvolvimento, privilegiando **abordagens mais leves, iterativas e centradas nas pessoas**. Um participante disse que passou a usar iterações curtas; outro reduziu a documentação em favor da comunicação direta; outros relataram que preferiam ajustar o planejamento conforme as mudanças inevitáveis do projeto. A convergência dessas experiências deu origem a um movimento.

O grupo decidiu que era necessário formalizar essas ideias em um documento manifesto. Inicialmente, cogitou-se utilizar o termo “leve” para representar essa nova abordagem, mas, por soar fraco, a palavra foi substituída por “ágil”, que expressava melhor a **flexibilidade, adaptabilidade e foco nas entregas frequentes de valor**.

Assim nasceu o **Manifesto Ágil para o Desenvolvimento de Software**, um documento breve, objetivo e impactante, que estabeleceu os **valores fundamentais e princípios** defendidos por aquela comunidade emergente. A partir dele, formou-se a chamada **Aliança Ágil (Agile Alliance)**, uma organização sem fins lucrativos comprometida em promover o conhecimento e o uso de métodos ágeis no desenvolvimento de software.

O impacto foi gigantesco. O Manifesto Ágil não apenas marcou o surgimento de uma nova filosofia de trabalho, mas inaugurou um verdadeiro paradigma no campo da Engenharia de Software. Hoje, grande parte dos projetos modernos é conduzida com base em métodos ágeis – como Scrum, XP e Kanban –, todos fundamentados nos princípios lançados por aquele grupo visionário.

## Os Quatro Valores do Manifesto Ágil

A essência da agilidade está expressa nos quatro valores centrais do Manifesto Ágil, apresentados na figura a seguir:

<div align="center">
  <img width="640px" src="./img/13-manifesto-agil.png">
</div>

Cada valor contrapõe duas abordagens, evidenciando a preferência pelos elementos da esquerda sem desmerecer os da direita. É fundamental entender que o Manifesto Ágil **não rejeita os itens da direita**, mas defende que **os da esquerda devem ser mais valorizados** no processo de desenvolvimento de software. A seguir, detalhamos cada um desses valores com exemplos e reflexões.

### Indivíduos e Interações mais que Processos e Ferramentas

No centro do desenvolvimento de software estão as **pessoas**. São elas que criam, pensam, decidem, se comunicam, projetam e implementam os sistemas. Por mais bem definidos que sejam os processos e poderosas que sejam as ferramentas, nada substitui a criatividade, o conhecimento, a intuição e a capacidade de adaptação humana.

Jim Highsmith, um dos signatários do Manifesto Ágil, afirma que a diversidade de habilidades, personalidades e peculiaridades individuais é **crítica para o sucesso dos projetos**. As pessoas são, por natureza, complexas, criativas, imprevisíveis e apaixonadas. Justamente por isso, não basta apenas seguir processos ou dominar ferramentas: é essencial construir equipes coesas, colaborativas e capacitadas.

Por exemplo, uma empresa pode adotar um processo ágil bem estruturado e utilizar ferramentas de ponta, como JIRA ou GitHub, mas se a equipe estiver desmotivada ou sem liberdade para colaborar, os resultados provavelmente serão insatisfatórios. Valorizar os indivíduos é criar um ambiente propício para que talentos floresçam e interações produtivas aconteçam.

### Software em Funcionamento mais que Documentação Abrangente

O verdadeiro valor de um projeto de software está no **produto entregue**, e não nos documentos que o descrevem. Embora a documentação continue sendo útil – principalmente para transferir conhecimento, atender exigências legais ou facilitar manutenções futuras –, ela **não deve se sobrepor à entrega funcional do sistema**.

Pense em um exemplo do cotidiano: ao comprar um carro, o que realmente importa é a experiência de dirigir, o desempenho e o conforto. Embora o manual técnico seja relevante, ele não é o motivo da compra. O mesmo raciocínio vale para o software.

Isso não significa que a documentação seja descartável. Ao contrário, ela é recomendada e necessária, desde que seja **na medida certa**: suficiente para apoiar o desenvolvimento e a comunicação entre os membros da equipe. Documentações excessivas, com centenas de páginas e dezenas de diagramas que jamais serão consultados, representam desperdício de tempo e esforço.

Logo, no contexto ágil, busca-se manter **o foco na entrega de valor real** ao cliente, priorizando um software funcional, testado, validado e em constante evolução, mesmo que com uma documentação mais enxuta e pragmática.

### Colaboração com o Cliente mais que Negociação de Contratos

Uma das principais críticas às metodologias tradicionais é a **distância entre o cliente e a equipe de desenvolvimento**. No modelo em cascata, por exemplo, o cliente participa ativamente apenas nas fases iniciais do projeto – geralmente no levantamento de requisitos – e retorna somente na entrega final. Isso frequentemente resulta em insatisfação, pois o produto entregue nem sempre corresponde às expectativas reais do cliente.

O Manifesto Ágil propõe uma mudança radical: **cliente e desenvolvedor devem estar lado a lado durante todo o ciclo de desenvolvimento**. Essa aproximação favorece a comunicação, o alinhamento de expectativas, o entendimento dos objetivos de negócio e a possibilidade de adaptação constante.

Isso não significa ignorar os contratos formais. Eles continuam sendo importantes para regular responsabilidades, prazos e recursos. A diferença é que o foco se desloca da **negociação rígida de cláusulas contratuais** para a construção de um relacionamento baseado na **confiança, transparência e colaboração contínua**.

Um modelo contratual bastante usado no ágil é o de **tempo fixo e escopo variável**. Nele, o fornecedor se compromete a entregar o máximo possível de valor dentro de um prazo definido, ajustando o escopo conforme as prioridades do cliente ao longo do tempo. Isso cria flexibilidade e permite que o produto final seja mais aderente às necessidades reais do negócio.

### Responder a Mudanças mais que Seguir um Plano

Vivemos em um mundo instável, incerto, volátil e ambíguo. Tecnologias emergem e desaparecem rapidamente. Empresas líderes de mercado podem ruir em questão de meses, como vimos com gigantes como Nokia, Blockbuster, MSN e Orkut. Diante desse cenário, **ser flexível e responsivo às mudanças é uma questão de sobrevivência**.

No desenvolvimento de software, mudanças são inevitáveis: requisitos mudam, prioridades mudam, leis mudam, o mercado muda. Um planejamento rígido e inflexível, feito no início do projeto, pode se tornar obsoleto em poucas semanas. Por isso, o Manifesto Ágil valoriza **a capacidade de adaptação** mais do que o apego cego ao plano original.

Isso não significa que o planejamento seja dispensável. Ao contrário: planejar é essencial. A diferença está em entender que **planejar deve ser uma atividade contínua**, não pontual. Em vez de fazer um único planejamento estático e segui-lo à risca, os métodos ágeis propõem **revisões periódicas**, baseadas em feedbacks reais e em mudanças no ambiente do projeto.

Uma equipe ágil sabe que mudar de direção pode ser necessário – e, muitas vezes, vantajoso. Quanto mais cedo se identifica a necessidade de mudança e se adapta a ela, maior a chance de sucesso do projeto.

## Agilidade x Velocidade

Ao abordar metodologias ágeis, é comum surgirem confusões entre dois conceitos distintos: **agilidade** e **velocidade**. Embora estejam relacionados, esses termos não são sinônimos. Para compreendermos bem essa diferença, vale a pena recorrer a algumas metáforas fora do universo do desenvolvimento de software, começando com o lendário atleta jamaicano **Usain Bolt**.

### A Metáfora do Atletismo: o caso Usain Bolt

Bolt é reconhecido mundialmente por sua impressionante **velocidade**. Em provas de 100 metros rasos, ele frequentemente terminava a corrida muito à frente de seus adversários, deixando claro seu potencial para atingir altas velocidades em pouco tempo. No entanto, ao analisarmos os primeiros segundos de suas corridas, podemos notar uma curiosidade: Bolt nem sempre largava na frente. Nas imagens das provas, ele costuma aparecer entre os últimos colocados logo nos primeiros 20 metros. Por quê?

<div align="center">
  <img width="720px" src="./img/13-usain-bolt.png">
</div>

A resposta está na **agilidade**. Enquanto **velocidade** diz respeito ao quanto um atleta consegue correr em um determinado intervalo de tempo, **agilidade** refere-se à **capacidade de reagir rapidamente a uma mudança**, como o disparo do tiro inicial. Por ser mais alto e pesado que seus concorrentes, Bolt demorava um pouco mais para sair da inércia e iniciar a corrida, o que o tornava **menos ágil** na largada – mesmo sendo extremamente veloz no percurso total.

Imagine agora se as provas fossem de apenas 50 metros: talvez Bolt **não tivesse conquistado o tricampeonato olímpico**, já que sua recuperação e aceleração ocorriam com mais destaque na segunda metade da corrida, quando não era mais necessário reagir a uma mudança, mas apenas manter o ritmo máximo.

### Outra Metáfora: Carros Leves x Carros Potentes

Podemos aplicar a mesma lógica ao comparar dois carros: um muito potente e pesado, e outro mais leve e com menos potência. Em uma disputa de arrancada, o carro mais leve pode ser mais **ágil**, pois responde mais rapidamente ao sinal de partida e atinge velocidade em menos tempo. No entanto, o carro potente provavelmente será **mais veloz** em um percurso mais longo, pois atinge uma **velocidade máxima maior**.

Essas analogias nos ajudam a entender que **agilidade é a capacidade de responder a mudanças**, enquanto **velocidade é a capacidade de realizar algo rapidamente**. Ambas são desejáveis, mas atendem a propósitos distintos.

### Aplicando ao Desenvolvimento de Software

No contexto do desenvolvimento de software, essa diferença é ainda mais crítica. Um processo pode ser muito **rápido** para entregar um produto ao cliente – como é o caso do **Rapid Application Development (RAD)** –, mas isso não significa que ele seja **ágil**.

A **agilidade** está na **capacidade da equipe de se adaptar a mudanças** inesperadas durante o ciclo de vida do projeto. Por exemplo:

- Se for necessário **mudar a arquitetura** do sistema, a equipe responde com tranquilidade.
- Se ocorrerem **cortes orçamentários** e a equipe precisar ser reduzida, o projeto é replanejado sem comprometer sua continuidade.
- Se a tecnologia adotada se tornar obsoleta e for preciso trocá-la, a equipe ajusta a estratégia de desenvolvimento sem grandes impactos.

Ou seja, **uma equipe ágil não apenas entrega rápido, mas entrega com flexibilidade**, adaptando-se ao cenário em constante mudança que caracteriza a maior parte dos projetos modernos.

### Diretrizes para um Processo de Software Ágil

Segundo **Roger Pressman**, um processo de software pode se tornar ágil desde que respeite algumas diretrizes fundamentais:

- O processo deve ser projetado de forma que a equipe **possa adaptar e racionalizar suas tarefas** conforme o contexto do projeto.
- O planejamento deve levar em conta a **fluidez da abordagem ágil**, sendo flexível e continuamente revisado.
- Devem ser eliminados todos os artefatos desnecessários, mantendo-se **apenas os elementos essenciais** para o sucesso do projeto.
- A estratégia deve se apoiar em **entregas incrementais**, proporcionando ao cliente versões operacionais do software o mais cedo possível.

Esses princípios permitem que qualquer processo de software – mesmo os mais tradicionais – possa se tornar **mais ágil**, desde que se ajuste para responder de maneira eficaz às mudanças e imprevistos do projeto.

### Agilidade: Adaptabilidade e Colaboração

Métodos ágeis, por essência, são projetados para reagir bem às mudanças. Por isso, são considerados **dinâmicos, adaptativos, iterativos e colaborativos**. Eles se moldam às necessidades específicas de cada projeto e mantêm uma postura aberta a ajustes ao longo de todo o desenvolvimento.

Por outro lado, métodos tradicionais tendem a ser mais **preditivos e formais**, baseando-se em um planejamento detalhado feito no início do projeto e que raramente é alterado. São **documentais e contratuais**, priorizando o controle de escopo, prazos e custos, mesmo que isso reduza a flexibilidade diante de novas exigências.

Em resumo, enquanto a **velocidade** pode garantir uma entrega rápida, é a **agilidade** que assegura que o produto final seja relevante, adaptado e funcional, mesmo diante de mudanças inesperadas. E no mundo atual, onde a única certeza é a mudança, ser ágil é uma vantagem estratégica indispensável.

## Princípios Ágeis

Os métodos ágeis são fundamentados em um conjunto de princípios que norteiam o desenvolvimento de software de forma dinâmica, colaborativa e adaptativa. Esses princípios estão formalizados no **Manifesto Ágil**, elaborado por um grupo de especialistas da indústria de software, e representam a essência da agilidade.

<div align="center">
  <img width="720px" src="./img/13-principios-ageis.png">
</div>

A seguir, vamos conhecer esses princípios, que são diretrizes fundamentais para qualquer equipe ou organização que deseje adotar métodos ágeis em seus projetos:

- **Nossa maior prioridade é satisfazer o cliente**, através da entrega contínua e antecipada de software com valor agregado.
- **Mudanças nos requisitos são bem-vindas**, mesmo em estágios avançados do desenvolvimento. Processos ágeis aproveitam as mudanças para gerar vantagem competitiva ao cliente.
- **Entregar frequentemente software funcionando**, em ciclos que vão de poucas semanas a poucos meses, sempre priorizando ciclos mais curtos.
- **Negócios e desenvolvimento trabalham juntos diariamente** durante todo o projeto, promovendo alinhamento constante.
- **Construa projetos em torno de indivíduos motivados.** Ofereça a eles o suporte e o ambiente necessários e confie que realizarão um bom trabalho.
- **O método mais eficiente e eficaz de comunicar informações é a conversa face a face.**
- **Software funcionando é a principal medida de progresso.**
- **Processos ágeis promovem desenvolvimento sustentável.** Todos os envolvidos devem conseguir manter um ritmo constante e indefinido.
- **Atenção contínua à excelência técnica e ao bom design** aumenta a agilidade.
- **Simplicidade é essencial,** ou seja, a arte de maximizar a quantidade de trabalho não realizado.
- **As melhores arquiteturas, requisitos e designs emergem de equipes auto-organizáveis.**
- **Em intervalos regulares, a equipe reflete sobre como se tornar mais eficaz** e então ajusta seu comportamento de acordo.

### Reflexão Importante

Diante desses princípios, surge uma questão comum: **"As metodologias ágeis são recomendadas para projetos de qualquer tamanho e complexidade?"**

De acordo com Sommerville, um dos autores mais tradicionais na Engenharia de Software, a resposta seria não. Ele afirma:

> “Todos os métodos têm limites, e os métodos ágeis são somente adequados para alguns tipos de desenvolvimento de sistema. Na minha opinião, eles são mais adequados para o desenvolvimento de sistemas de pequenas e médias empresas e produtos para computadores pessoais.”

Entretanto, essa visão, embora válida no passado, é considerada, por muitos profissionais da área, um tanto ultrapassada nos dias atuais. Hoje, muitos projetos de grande porte, inclusive em grandes organizações, utilizam metodologias ágeis com sucesso. Isso demonstra que os métodos ágeis amadureceram significativamente e estão aptos para enfrentar desafios complexos.

Portanto, embora essa seja uma opinião ainda presente em livros acadêmicos, na prática do mercado atual, a aplicabilidade das metodologias ágeis é muito mais ampla.

### Principais Metodologias Ágeis

Existem diversas metodologias ágeis aplicadas ao desenvolvimento de software, cada uma com suas características, práticas e focos específicos. Abaixo listamos as principais:

- **SCRUM**
- **CRYSTAL**
- **XP (Extreme Programming)**
- **TDD (Test-Driven Development)**
- **ATDD (Acceptance Test-Driven Development)**
- **BDD (Behavior-Driven Development)**
- **FDD (Feature-Driven Development)**
- **DDD (Domain-Driven Design)**
- **MDD (Model-Driven Development)**
- **DSDM (Dynamic Systems Development Method)**
- **ASD (Adaptive Software Development)**
- **KANBAN**
- **BADM (Beyond Agile Development Method)**
- **AUP (Agile Unified Process)**
- **Agile Modeling**
- **OSSD (Open Source Software Development)**
- **SCRUMBAN**

Cada uma dessas metodologias possui enfoques específicos, abordagens próprias para gestão, desenvolvimento, testes, entrega e acompanhamento dos projetos.

### Comparação Entre Modelos Tradicionais e Ágeis

Para entender melhor como as metodologias ágeis se diferenciam dos modelos tradicionais, observe a tabela a seguir. Ela compara os dois paradigmas de desenvolvimento de software em diversos critérios fundamentais:

|**Critério**|**Modelos Tradicionais**|**Modelos Ágeis**|
|---|---|---|
|**Planejamento**|Detalhado para todo o projeto, realizado na fase inicial.|Planejamento de alto nível no início, com detalhamento contínuo ao longo do projeto, focando na próxima iteração.|
|**Riscos**|Tratados de forma extensa desde o início, o que pode demandar grandes esforços.|Foco nos riscos mais próximos, especialmente nas iterações seguintes, com atuação contínua.|
|**Equipe**|Papéis bem definidos, hierarquia clara e trabalho guiado pelo gerente de projetos.|Equipes multidisciplinares, auto-organizáveis e colaborativas, que decidem como executar as tarefas.|
|**Tempo de Entrega**|Baseado no cronograma do projeto, podendo durar meses ou anos.|Fixo e baseado na duração das iterações (normalmente entre 1 e 4 semanas).|
|**Aceitação de Mudanças**|Mudanças são tratadas formalmente, com burocracia e impacto no plano.|Mudanças são bem-vindas entre as iterações. Flexível e adaptável.|
|**Previsibilidade**|Depende da qualidade do planejamento inicial e do controle periódico.|Alta previsibilidade, graças à constante inspeção, adaptação e feedback.|
|**Resultados ao Longo do Tempo**|Resultados geralmente no final do projeto.|Entregas frequentes, gerando valor contínuo desde o início.|
|**Apresentação de Informações**|Relatórios formais, agendados em momentos específicos.|Informações sempre visíveis e atualizadas, por meio de radiadores de informação no ambiente da equipe.|
|**Prazo de Entrega**|Definido no início e alterado mediante aprovação formal de mudanças.|Definido pela duração das iterações e planejamentos incrementais (releases).|
|**Documentação**|Extensa, criada desde o início do projeto.|Documentação enxuta, focada apenas no necessário para apoiar o desenvolvimento e a entrega de valor.|
|**Atuação do Cliente**|Participação nas fases iniciais e nas validações principais.|Participação ativa e contínua durante todo o desenvolvimento.|
|**Discussões e Melhorias**|Ocorrem após grandes marcos ou entregas.|Ocorrem frequentemente, ao final de cada iteração.|
|**Comando**|Centralizado no gerente de projetos.|Distribuído na própria equipe, que é auto-organizável.|
|**Papéis**|Rígidos e bem definidos.|Flexíveis, baseados em colaboração, confiança e nas necessidades do projeto.|
|**Processo**|Baseado em processos formais e planejamento rígido.|Baseado na experimentação, adaptação e priorização de valor.|
|**Resultado**|Melhor desempenho em projetos de escopo bem definido e estável.|Melhor desempenho em projetos com escopo dinâmico, onde mudanças são constantes e necessárias.|

Este quadro resume claramente as principais diferenças entre os dois paradigmas. Vale destacar que as metodologias ágeis não buscam simplesmente “ser mais rápidas”, mas sim mais adaptáveis, colaborativas e orientadas à geração contínua de valor para o cliente.

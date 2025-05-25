# Capítulo 14 – Scrum

## Conceitos Básicos do Scrum

Antes de adentrarmos na teoria do Scrum propriamente dita, é importante entendermos a origem desse nome tão peculiar. O termo **Scrum** vem do esporte **Rugby**, e não foi escolhido por acaso. Ele carrega uma forte carga simbólica, representando a importância da cooperação, da união e do esforço coletivo para se alcançar objetivos em contextos de alta competitividade e complexidade.

No Rugby, o termo **scrum** designa uma formação específica, na qual os jogadores das duas equipes se agrupam, pressionando-se uns contra os outros, formando uma espécie de “muralha humana”. O objetivo dessa formação é disputar a posse da bola de maneira conjunta e coordenada. Aqui, todos os jogadores são fundamentais; não existe um líder que centraliza as jogadas, como acontece, por exemplo, no futebol americano com o quarterback. Cada membro da equipe desempenha um papel crucial no sucesso da jogada.

<div align="center">
  <img width="560px" src="./img/14-scrum-rugby.png">
</div>

Essa metáfora é perfeita para o ambiente de desenvolvimento de produtos complexos. Assim como no Rugby, no Scrum todos os integrantes da equipe precisam trabalhar de maneira colaborativa, sincronizada e focada no alcance de objetivos comuns. Não há espaço para individualismo, hierarquias rígidas ou centralização de decisões. O sucesso depende da soma das forças, do comprometimento e da responsabilidade compartilhada.

### O Que é Scrum?

Do ponto de vista técnico, o **Scrum é um framework leve, simples de entender, porém extremamente difícil de dominar**, que permite às pessoas, times e organizações gerar valor por meio de soluções adaptativas para problemas complexos. Esse conceito está presente em ambas as versões mais conhecidas do **Guia Scrum** (2017 e 2020), documento oficial que descreve detalhadamente seus princípios, práticas, papéis, eventos, artefatos e regras.

É fundamental entender que o Scrum **não é um processo, uma técnica ou uma metodologia fechada**. Na verdade, ele oferece uma estrutura dentro da qual é possível empregar diversos processos, técnicas e práticas, desde que estes estejam alinhados aos seus princípios. É como se o Scrum fosse uma moldura, dentro da qual cada organização ou equipe pode pintar o seu próprio quadro, adaptando as práticas às suas necessidades específicas.

Essa característica oferece enorme flexibilidade, mas também exige disciplina, comprometimento e compreensão profunda dos seus princípios. É por isso que, embora seja simples de entender, não é trivial aplicá-lo corretamente no dia a dia.

### Framework e Não Metodologia

Faz-se necessário reforçar essa distinção: Scrum é um **framework**, não uma metodologia. Uma metodologia, geralmente, traz prescrições detalhadas sobre o que deve ser feito, como deve ser feito, quando e por quem. Já o framework estabelece uma base conceitual, composta por papéis, eventos, artefatos e regras mínimas, que devem ser combinadas, ajustadas e complementadas conforme o contexto do time ou da organização.

Portanto, o Scrum **não te dirá exatamente como fazer o seu trabalho**, mas fornecerá um ambiente seguro e estruturado para que você e sua equipe possam experimentar, inspecionar, adaptar e, assim, maximizar a geração de valor.

### Aplicações do Scrum no Mundo Real

Engana-se quem pensa que o Scrum é restrito ao desenvolvimento de software. De fato, ele surgiu neste contexto, mas ao longo das décadas sua aplicação se expandiu de forma significativa. Atualmente, é utilizado nas mais diversas áreas e setores, como desenvolvimento de hardware, engenharia, marketing, educação, operações, jurídico, recursos humanos, serviços públicos, governo e até no gerenciamento da própria organização.

Por exemplo, imagine uma equipe de marketing que precisa lançar campanhas digitais constantemente. Nesse contexto, os requisitos mudam com frequência — novos dados de mercado surgem, concorrentes alteram suas estratégias, e as tendências evoluem rapidamente. Utilizar o Scrum permite que a equipe organize seu trabalho em ciclos curtos (chamados de **Sprints**), entregando incrementos de valor de forma contínua, testando, aprendendo e ajustando sua estratégia com agilidade.

Outro exemplo prático está no desenvolvimento de produtos físicos, como veículos autônomos. Empresas desse setor precisam lidar com requisitos altamente mutáveis, inovações tecnológicas constantes e interações complexas entre software, hardware e inteligência artificial. O Scrum se mostra extremamente eficaz para coordenar o trabalho de equipes multidisciplinares, permitindo ciclos curtos de desenvolvimento, testes constantes e entrega de resultados incrementais.

### Por Que Scrum Funciona em Ambientes Complexos?

Para entender a efetividade do Scrum, precisamos compreender primeiro o que caracteriza um **ambiente complexo**. Aqui, cabe introduzir o conceito do **Modelo Cynefin**, criado por Dave Snowden, que classifica os ambientes em quatro domínios principais:

<div align="center">
  <img width="560px" src="./img/14-modelo-cynefin.png">
</div>

- **Simples (ou Óbvio)**: As relações de causa e efeito são claras, conhecidas e previsíveis. Soluções baseadas em boas práticas funcionam muito bem. Um exemplo clássico é a produção de uma rede de fast food, como o McDonald’s. As tarefas são altamente padronizadas e quase não há espaço para variações.
- **Complicado**: Existe uma relação de causa e efeito, mas ela não é óbvia e exige análise ou expertise para ser compreendida. A solução é baseada em boas práticas e especialistas. Imagine, por exemplo, o desenvolvimento de um avião — é difícil, mas possível de planejar detalhadamente.
- **Complexo**: As relações de causa e efeito só podem ser percebidas de forma retrospectiva, após os eventos ocorrerem. Esse ambiente é caracterizado pela alta incerteza, mudanças constantes e imprevisibilidade. Nesse cenário, o caminho correto surge por meio de experimentação, inspeção e adaptação. É aqui que o Scrum brilha.
- **Caótico**: Não há relação clara de causa e efeito. A prioridade aqui é agir rapidamente para estabilizar a situação, depois analisar e responder.

Portanto, o Scrum é especialmente eficaz em **ambientes complexos**, onde não é possível prever tudo antecipadamente e onde mudanças são frequentes. Diferente de modelos tradicionais que tentam prever e planejar tudo no início, o Scrum aposta na abordagem **iterativa e incremental**, com ciclos curtos de trabalho que permitem aprendizado constante, ajustes rápidos e redução de riscos.

### Iteratividade, Incremento e Controle Empírico

O Scrum é fundamentado na teoria do **controle empírico de processos**, que se apoia em três pilares fundamentais: **transparência, inspeção e adaptação** (que veremos detalhadamente no próximo tópico). Através de ciclos iterativos e incrementais, o Scrum maximiza as oportunidades de feedback, melhorando não apenas o produto, mas também o próprio processo e a dinâmica do time.

Cada ciclo, chamado de **Sprint**, é uma oportunidade de inspecionar os resultados, avaliar o que está funcionando, identificar o que precisa ser ajustado e, então, adaptar-se para o próximo ciclo.

De forma resumida, o Scrum é composto por:

- **Papéis**: São três, e cada um tem responsabilidades muito bem definidas – o **Product Owner**, o **Scrum Master** e os **Developers** (Desenvolvedores, que compõem o Time Scrum).
- **Eventos**: Existem quatro eventos principais, além do próprio Sprint. São eles: **Sprint Planning**, **Daily Scrum**, **Sprint Review** e **Sprint Retrospective**.
- **Artefatos**: Também são três – **Product Backlog**, **Sprint Backlog** e **Incremento**.
- **Regras e Fluxo**: As regras do Scrum integram papéis, eventos e artefatos, definindo como eles interagem entre si. Além disso, existe o **fluxo de trabalho iterativo e incremental**, onde a Sprint é o coração do processo.

Embora seja possível utilizar outros artefatos complementares — como o **Gráfico de Burndown**, o **Documento de Visão do Produto**, ou ferramentas de acompanhamento — eles são opcionais, funcionando como “extensões” ao framework.

### Uma Visão Pragmática

Por fim, é importante destacar que o Scrum não é uma bala de prata, nem uma solução mágica para todos os problemas. Ele exige comprometimento, mudança cultural e disciplina. Times que tentam “adaptar” o Scrum removendo seus elementos essenciais — como ignorar eventos, não adotar todos os papéis, ou não manter os artefatos —, na prática, deixam de fazer Scrum e acabam perdendo seus benefícios.

O próprio **Guia Scrum** nos alerta que sua simplicidade é proposital, e que sua implementação deve ser feita na íntegra antes que se considerem adaptações. **A experimentação é bem-vinda, desde que os pilares e fundamentos sejam respeitados.**

## Pilares Fundamentais do Scrum

O Scrum é um framework ágil que se baseia em dois conceitos fundamentais: **empirismo** e **lean thinking**. Compreender esses fundamentos é essencial para entender não apenas como o Scrum funciona, mas também por que ele funciona de maneira tão eficiente, especialmente em ambientes complexos, incertos e dinâmicos.

### Empirismo: Aprendizado pela Experiência

O primeiro pilar conceitual do Scrum é o **empirismo**, uma filosofia que defende que o conhecimento se dá por meio da experiência direta e da tomada de decisões fundamentadas naquilo que é conhecido no momento. Ao invés de tentar prever todos os possíveis caminhos de um projeto desde o início, o Scrum permite que as equipes aprendam e se adaptem continuamente conforme o trabalho avança.

Esse aprendizado ocorre por meio de ciclos de trabalho curtos, conhecidos como **Sprints**, onde, ao final de cada ciclo, a equipe avalia o que foi feito, aprende com os erros, ajusta processos e se prepara para o próximo ciclo. Dessa forma, riscos são constantemente controlados e a previsibilidade dos resultados melhora progressivamente.

### Lean Thinking: Foco na Eliminação de Desperdícios

O segundo conceito essencial é o **Lean Thinking**, um modelo mental que tem como objetivo reduzir desperdícios e maximizar aquilo que realmente agrega valor. No contexto do Scrum, isso significa que toda atividade, processo ou tarefa que não contribua diretamente para gerar valor ao produto ou ao cliente deve ser questionada e, sempre que possível, eliminada.

Assim, o Scrum não é apenas uma sequência de práticas; ele representa uma mudança cultural que estimula o foco no que importa, promovendo produtividade, eficiência e agilidade.

### Os Três Pilares do Empirismo no Scrum

O funcionamento prático do Scrum, dentro desse pensamento empírico e enxuto, se sustenta em três pilares fundamentais: **Transparência, Inspeção e Adaptação** — frequentemente representados pela sigla **TIA**.

<div align="center">
  <img width="680px" src="./img/14-scrum-pilares.png">
</div>

Esses pilares não são apenas conceitos teóricos; eles estão diretamente refletidos em todos os eventos, artefatos e práticas do Scrum. Eles são a base para garantir que as equipes tenham controle efetivo do processo, possam responder rapidamente às mudanças e, principalmente, entreguem valor de forma constante e incremental.

#### Transparência: A Verdade Precisa Ser Visível

O pilar da **Transparência** estabelece que todos os aspectos significativos do processo devem ser visíveis, claros e compreendidos por todos os envolvidos no projeto, tanto pela equipe que executa quanto pelos stakeholders (clientes, usuários e outros interessados).

Essa visibilidade é fundamental para que haja uma compreensão compartilhada sobre o que está acontecendo, evitando mal-entendidos, suposições incorretas e desalinhamento.

Como bem definiu **Ken Schwaber**, um dos criadores do Scrum:

> “Scrum é igual sogra: chega na sua casa e esfrega todos os seus problemas na sua cara.”

Essa frase, bem-humorada, ilustra perfeitamente o espírito da transparência no Scrum: se há problemas, falhas, atrasos ou qualquer outro tipo de dificuldade, todos devem saber. Nada é escondido debaixo do tapete. O enfrentamento honesto das dificuldades permite que elas sejam discutidas e, principalmente, solucionadas.

A **transparência** no Scrum não significa apenas “ser honesto”, mas também garantir que haja uma linguagem comum, definições claras e critérios bem estabelecidos. Por exemplo, todos precisam entender exatamente o que significa um item estar **“Pronto”** (o famoso _Definition of Done_). Sem essa clareza, cada membro pode interpretar o andamento do trabalho de forma diferente, gerando desalinhamentos prejudiciais.

Reflexões dos Guias Scrum:

- **Guia Scrum 2017:** “Aspectos significativos do processo devem estar visíveis aos responsáveis pelos resultados. A transparência requer que estes aspectos tenham uma definição padrão comum para que os observadores compartilhem um entendimento comum do que está sendo visto.”
- **Guia Scrum 2020:** “O processo emergente e o trabalho devem ser visíveis tanto para quem executa o trabalho quanto para quem recebe o trabalho. Artefatos com baixa transparência podem levar a decisões que diminuem o valor e aumentam o risco. A transparência permite a inspeção. A inspeção sem transparência é enganosa e gera desperdício.”

#### Inspeção: Avaliar Para Melhorar

Se tudo está visível, é natural que o próximo passo seja inspecionar. A **Inspeção** é o segundo pilar do Scrum e consiste na verificação constante dos artefatos produzidos, do progresso do trabalho e dos processos que estão sendo executados.

O objetivo da inspeção é simples, porém fundamental: **detectar desvios indesejados o mais cedo possível**. Isso permite que os problemas sejam corrigidos antes que gerem impactos maiores no projeto.

Essa inspeção não é feita de maneira aleatória, mas estruturada nos eventos formais do Scrum, como veremos mais adiante: **Reuniões Diárias, Revisões da Sprint, Retrospectivas, entre outros**. É nesses momentos que a equipe avalia tanto o produto quanto os processos, identificando melhorias, riscos e oportunidades de ajuste.

É importante destacar que a inspeção, quando mal conduzida, pode se transformar em microgerenciamento — o que é absolutamente contra os princípios do Scrum. Por isso, o próprio Guia Scrum alerta que as inspeções **“não devem ser tão frequentes que atrapalhem o trabalho”**, devendo ocorrer de forma cadenciada, dentro dos eventos preestabelecidos.

Reflexões dos Guias Scrum:

- **Guia Scrum 2017:** “Os usuários Scrum devem, frequentemente, inspecionar os artefatos e o progresso em direção ao objetivo da Sprint para detectar variações indesejadas. Esta inspeção não deve ser tão frequente que atrapalhe os trabalhos.”
- **Guia Scrum 2020:** “Os artefatos do Scrum e o progresso em direção às metas devem ser inspecionados com frequência e diligência para detectar variações ou problemas indesejáveis. A inspeção habilita a adaptação. A inspeção sem adaptação é inútil.”

#### Adaptação: Corrigir a Rota Sempre que Necessário

Quando a inspeção revela problemas, desvios ou oportunidades de melhoria, é hora de colocar em prática o terceiro pilar: a **Adaptação**.

No Scrum, qualquer desvio em relação à meta — seja no produto ou no processo — deve ser corrigido imediatamente. A ideia central é que **quanto mais rápido se reage aos problemas, menor é o impacto que eles podem causar no projeto.**

Por exemplo, se durante uma Retrospectiva a equipe percebe que a comunicação interna está falhando, ela deve propor e implementar mudanças já na próxima Sprint para resolver esse problema. Da mesma forma, se uma funcionalidade não atende aos critérios de aceitação, ela deve ser ajustada antes de seguir para o cliente.

A **adaptação** não é opcional no Scrum; ela é obrigatória e constante. E mais: ela se torna muito mais difícil se a equipe não for autogerenciável ou não estiver empoderada para tomar decisões. Por isso, o Scrum estimula fortemente equipes auto-organizadas, que tenham liberdade e autonomia para se ajustar rapidamente frente às mudanças.

Reflexões dos Guias Scrum:

- **Guia Scrum 2017:** “Se um inspetor determina que um ou mais aspectos de um processo desviou para fora dos limites aceitáveis, e que o resultado do produto será inaceitável, o processo ou o material sendo produzido deve ser ajustado. O ajuste deve ser realizado o mais breve possível.”
- **Guia Scrum 2020:** “A adaptação se torna mais difícil quando as pessoas envolvidas não são empoderadas ou autogerenciadas. Espera-se que um Scrum Team se adapte no momento em que aprende algo novo por meio da inspeção.”

#### Resumo dos Pilares Fundamentais

Para facilitar o entendimento, podemos resumir os três pilares fundamentais do Scrum na tabela a seguir:

|**Pilar**|**Descrição**|
|---|---|
|**Transparência**|Todo trabalho deve ser claramente definido, visível e conhecido por todos os envolvidos no projeto.|
|**Inspeção**|O trabalho e os processos devem ser inspecionados periodicamente para garantir sua qualidade.|
|**Adaptação**|O projeto deve ser capaz de se adaptar rapidamente às necessidades de negócio e às mudanças.|

### A Importância dos Pilares Para o Sucesso do Scrum

É fundamental compreender que **Transparência, Inspeção e Adaptação** não são práticas isoladas; elas são interdependentes e funcionam em conjunto.

Sem **transparência**, não há como realizar uma inspeção honesta e precisa.  
Sem **inspeção**, não há como saber se há necessidade de ajustes.  
Sem **adaptação**, qualquer problema identificado continuará existindo, comprometendo o produto e o processo.

Esse ciclo contínuo de **tornar visível → inspecionar → adaptar** é o que permite que o Scrum seja altamente eficiente em contextos de incerteza, mudança constante e necessidade de entrega de valor contínuo.

## Valores Fundamentais do Scrum

O Scrum, além de ser um framework ágil altamente estruturado e eficiente para o desenvolvimento de software e outros produtos complexos, fundamenta-se em um conjunto sólido de valores que direcionam o comportamento, as interações e as decisões de todos os envolvidos no projeto. Esses valores são essenciais para criar um ambiente de trabalho colaborativo, produtivo e sustentável, e sua aplicação prática é vital para o sucesso na adoção do Scrum.

<div align="center">
  <img width="680px" src="./img/14-scrum-valores.png">
</div>

São cinco os valores que sustentam o Scrum: **Coragem**, **Foco**, **Comprometimento**, **Respeito** e **Abertura**. Embora possam parecer conceitos simples, a verdadeira aplicação desses valores no cotidiano de um projeto exige prática, maturidade, disciplina e uma cultura organizacional que os incentive.

| Valores | Descrição |
| --- | --- |
| Coragem | Os integrantes de um projeto precisam ter coragem para fazer a coisa certa e trabalharem juntos removendo impedimentos, buscando soluções. |
| Foco | Os integrantes de um projeto precisam focar no trabalho durante a sprint e nas metas designadas – time disperso perde produtividade e não alcança os objetivos. |
| Comprometimento | Os integrantes se comprometem com o trabalho que se responsabilizou em fazer, envolvendo-se e não abandonando pela metade ou entregando sem qualidade. |
| Respeito | Os integrantes se respeitam entre si a fim de manter a colaboração, a integração e o bom ambiente de trabalho. |
| Abertura | Os integrantes devem poder ser francos, expor ideias e propostas mesmo que elas não sejam proveitosas. Momentos de debates, discussões e sugestões são ideais. |

O sucesso na utilização do Scrum não está restrito apenas ao cumprimento dos eventos, papéis e artefatos que compõem o framework. Ele está diretamente relacionado à internalização desses valores pelos membros do time, stakeholders e, idealmente, por toda a organização. Eles não são meramente decorativos ou filosóficos — são diretrizes práticas que moldam comportamentos, decisões e formas de interação dentro do Time Scrum.

### Coragem

No contexto do Scrum, **coragem** significa que os membros do Time Scrum devem ter a disposição necessária para fazer o que é certo, mesmo quando isso é desconfortável ou difícil. Isso inclui enfrentar desafios técnicos, discutir problemas abertamente, admitir erros, assumir responsabilidades e buscar soluções inovadoras.

Por exemplo, imagine uma Sprint em que um dos desenvolvedores percebe que um requisito, inicialmente planejado, não poderá ser entregue dentro do tempo restante, sem que haja perda na qualidade. Ter coragem, nesse caso, significa trazer essa informação para o time o quanto antes, discutir as alternativas, renegociar o escopo da Sprint se necessário, e não empurrar o problema para o futuro, comprometendo o produto.

Coragem também é necessária para lidar com impedimentos, desafios externos, pressão de stakeholders e, principalmente, para garantir que as práticas e os princípios ágeis sejam respeitados, mesmo que isso vá de encontro à cultura tradicional de comando e controle ainda presente em muitas organizações.

### Foco

O valor do **foco** é central dentro do Scrum, que é um framework orientado a objetivos claros e incrementos de trabalho bem definidos. O Time Scrum deve concentrar sua energia no trabalho da Sprint, evitando dispersões, interrupções desnecessárias ou desvios de escopo.

Isso significa que, durante uma Sprint, o time deve priorizar as atividades que estão comprometidas no Sprint Backlog, deixando de lado outras solicitações que possam surgir e que não estejam alinhadas com o objetivo da Sprint. Focar não é apenas uma questão de gestão de tempo, mas também de garantir qualidade, evitar retrabalho e maximizar a entrega de valor.

Por exemplo, suponha que durante uma Sprint, um stakeholder externo solicite a inclusão de uma nova funcionalidade que não estava planejada. O Time Scrum, então, deve gentilmente redirecionar essa solicitação para o Product Owner, que irá avaliar e, se for o caso, priorizar essa demanda para uma próxima Sprint. Isso preserva o foco do time nas metas estabelecidas, sem gerar dispersão.

### Comprometimento

O **comprometimento** no Scrum transcende a simples ideia de estar presente ou participar. Ele envolve assumir a responsabilidade pelo trabalho, pela entrega e pela qualidade do que é produzido. Cada membro do Time Scrum se compromete não apenas com as tarefas que assumiu, mas também com o sucesso coletivo do time.

Esse comprometimento não é imposto. Ele surge do entendimento compartilhado dos objetivos, da clareza das responsabilidades e da confiança mútua dentro do time. Um ambiente onde os membros estão verdadeiramente comprometidos é caracterizado por colaboração, ajuda mútua, transparência e busca pela melhoria contínua.

Por exemplo, se durante uma Sprint um desenvolvedor termina sua tarefa antes do previsto, ao invés de considerar seu trabalho concluído, ele se disponibiliza para ajudar colegas que estejam com dificuldades, revisando códigos, testando funcionalidades ou até mesmo contribuindo com tarefas fora de sua especialidade.

### Respeito

O **respeito** é a base que permite que times Scrum operem de forma colaborativa e saudável. Isso significa que todos os membros do Time Scrum se reconhecem como profissionais capazes, responsáveis e autônomos, e tratam-se mutuamente com dignidade, consideração e empatia.

Respeitar não significa apenas ser cordial. É compreender e valorizar as opiniões, as limitações, as contribuições e as necessidades dos colegas. É entender que cada membro tem sua própria bagagem de conhecimento, seu próprio ritmo de aprendizado e sua forma de trabalhar.

Por exemplo, durante um Sprint Retrospective, quando um membro aponta que se sentiu sobrecarregado devido a uma má distribuição das tarefas, os demais membros escutam atentamente, sem julgamentos, e juntos pensam em soluções para melhorar esse aspecto nas próximas Sprints.

Além disso, o respeito também se estende aos stakeholders e à organização, criando um ambiente onde as opiniões são consideradas e as decisões são tomadas de maneira colaborativa.

### Abertura

O valor da **abertura** se traduz na disposição para compartilhar informações, ideias, feedbacks e até mesmo problemas. Dentro do Scrum, a transparência é essencial, e isso só é possível quando todos estão dispostos a se comunicar de forma aberta e honesta.

Um Time Scrum deve estar aberto para discutir tanto os sucessos quanto os fracassos, os avanços e os obstáculos. A abertura permite que problemas sejam identificados e resolvidos rapidamente, que melhorias sejam constantemente implementadas e que todos se sintam parte do processo.

Por exemplo, se durante uma Sprint o time percebe que a abordagem técnica escolhida não está funcionando, a abertura permite que esse problema seja trazido para discussão imediatamente, para que uma nova abordagem possa ser definida, evitando desperdício de tempo e esforço.

A abertura também promove a inovação. Quando os membros do time se sentem à vontade para propor ideias, mesmo que inicialmente pareçam ousadas ou inviáveis, isso pode levar a descobertas e soluções criativas que trazem grande valor ao projeto.

### Integração dos Valores aos Pilares do Scrum

Quando os valores de **Coragem**, **Foco**, **Comprometimento**, **Respeito** e **Abertura** são efetivamente vivenciados no dia a dia, os três pilares do Scrum — **Transparência**, **Inspeção** e **Adaptação** — tornam-se muito mais do que simples conceitos: eles se transformam em práticas vivas que sustentam a melhoria contínua e a entrega de valor.

A internalização desses valores constrói a base de confiança necessária para que o Time Scrum possa operar de maneira eficaz. Isso significa que as reuniões são mais produtivas, os problemas são tratados mais rapidamente, a qualidade do produto melhora significativamente, e o ambiente de trabalho torna-se mais saudável e motivador.

Essa sinergia entre valores e pilares também fortalece o empoderamento do Time Scrum, permitindo que ele seja verdadeiramente autogerenciável, colaborativo e capaz de entregar resultados consistentes em ambientes complexos e de constante mudança.

> **Observação:**  
> As descrições e interpretações desses valores foram enriquecidas com base nos Guias Scrum de 2017 e 2020, que ressaltam a importância desses princípios como elementos fundamentais para o sucesso na adoção do framework. É importante destacar que, ao longo do tempo, embora a essência dos valores permaneça a mesma, as versões mais recentes do Guia Scrum reforçam a necessidade de que esses valores estejam profundamente enraizados na cultura do Time Scrum e na forma como ele interage com seus stakeholders.

## Papéis no Scrum

Quando falamos sobre o funcionamento do Scrum, é fundamental compreender que este framework é construído sobre a colaboração intensa de um pequeno grupo de profissionais altamente capacitados, autogerenciáveis e multifuncionais. Um dos aspectos mais característicos do Scrum é que ele não estabelece uma grande quantidade de papéis, funções ou cargos. Pelo contrário, o Scrum aposta na simplicidade estrutural, estabelecendo poucos papéis, mas extremamente bem definidos, cada qual com suas responsabilidades e sua importância no funcionamento do framework.

Os indivíduos que ocupam esses papéis não são apenas executores de tarefas isoladas, mas sim membros integrantes de um mesmo time, comprometidos coletivamente com a geração de valor e com o alcance dos objetivos definidos. Eles trabalham juntos, de maneira colaborativa, transparente e incremental, o que os torna igualmente responsáveis pelos resultados obtidos.

Ao longo dos anos, o Scrum passou por atualizações significativas, o que gerou algumas diferenças conceituais e terminológicas entre as versões de seus guias oficiais. Por esse motivo, ao falarmos sobre os papéis no Scrum, é necessário entender como eles eram definidos até a versão de 2017 e como passaram a ser tratados a partir da versão de 2020.

Na versão de 2017, o Scrum estabelecia três papéis centrais: o **Product Owner**, o **Scrum Master** e o chamado **Development Team**, ou seja, o time de desenvolvimento. Juntos, esses três elementos formavam o que era chamado de **Scrum Team**. Entretanto, havia uma diferenciação clara: o **Scrum Team** era composto por esses três papéis de forma conjunta, mas se destacava internamente o **Development Team**, representando o grupo dos profissionais responsáveis diretamente pela construção do produto.

Já na versão de 2020, essa diferenciação foi eliminada. Passou-se a adotar uma visão mais unificada, na qual todo o grupo é denominado simplesmente de **Scrum Team**, sem subdivisões internas entre desenvolvimento e demais funções. Com isso, os papéis de **Developers**, **Product Owner** e **Scrum Master** passaram a ser compreendidos como responsabilidades dentro de um mesmo time coeso, colaborativo e com foco comum.

A seguir, observa-se uma figura que ilustra as diferenças na definição dos papéis entre as versões de 2017 e 2020:

<div align="center">
  <img width="720px" src="./img/14-scrum-papeis.png">
</div>

Essa representação ajuda a entender como a evolução do framework buscou tornar os conceitos mais simples e alinhados com a realidade dos times ágeis modernos, evitando interpretações equivocadas sobre subdivisões internas ou hierarquias ocultas dentro da equipe.

### Diferenças Fundamentais entre as Versões

Na **versão de 2017**, o Scrum Team era composto por:

- **Product Owner**: responsável por maximizar o valor do produto e gerir o **Product Backlog**.
- **Scrum Master**: responsável por garantir que o Scrum seja compreendido e seguido, atuando como facilitador do processo e como um servidor para o time.
- **Development Team**: composto por profissionais que possuem todas as competências necessárias para construir o incremento do produto.

É muito importante frisar que, no Scrum, esse Development Team deveria ser **multifuncional** e **auto-organizável**. Isso significa que o time possui todas as habilidades necessárias para desenvolver o produto sem depender de terceiros e tem total autonomia para decidir a melhor forma de realizar seu trabalho, sem imposições externas.

Por outro lado, na **versão de 2020**, essa diferenciação deixa de existir formalmente. O Scrum Team passa a ser considerado uma única unidade coesa, dentro da qual existem três **responsabilidades bem definidas**:

- **Product Owner**
- **Scrum Master**
- **Developers**

Nessa visão, não existe mais o conceito de subtimes, e tampouco qualquer tipo de hierarquia interna. Todos são igualmente responsáveis pelo sucesso do projeto e trabalham conjuntamente para atingir a **Meta do Produto**, que se torna um conceito central no framework.

### Tamanho Ideal dos Times Scrum

Outro aspecto relevante, tanto nas versões antigas quanto nas mais recentes, refere-se ao tamanho ideal dos times. Na prática, o Scrum recomenda que os times sejam pequenos o suficiente para manterem-se ágeis e grandes o suficiente para que consigam entregar um trabalho significativo dentro de uma Sprint.

De maneira geral, considera-se que um Scrum Team deve ter **10 ou menos pessoas**. Na versão de 2017, o foco estava mais especificamente no tamanho do Development Team, que deveria possuir, preferencialmente, entre **3 e 9 integrantes**. Times com menos de 3 membros poderiam ter dificuldades em termos de cobertura de competências, além de perderem dinamismo nas interações. Por outro lado, times maiores que 9 integrantes demandariam um nível de coordenação tão elevado que comprometeria a agilidade e a efetividade do trabalho.

Na versão de 2020, esse limite passou a ser aplicado ao Scrum Team como um todo. Caso haja a necessidade de um time maior, recomenda-se que ele seja subdividido em **Scrum Teams coesos**, todos compartilhando o mesmo **Product Backlog**, o mesmo **Product Owner** e a mesma **Meta do Produto**, mas atuando de forma relativamente independente no desenvolvimento de partes distintas do produto.

### Times Auto-organizáveis e Multifuncionais

Seja qual for a versão adotada, o Scrum defende fortemente os princípios de **auto-organização** e de **multifuncionalidade**.

- Ser **auto-organizável** significa que o time tem autonomia para decidir, internamente, como fará seu trabalho. Não existem gestores externos impondo tarefas, métodos ou sequências. O time, de forma colaborativa, define quem faz o quê, quando e como.
- Ser **multifuncional** implica que o time, em seu conjunto, possui todas as habilidades necessárias para entregar um incremento do produto, desde atividades de análise, desenvolvimento, testes, validação, integração, até deploy e operação, se necessário.

Vale destacar que, no Scrum, não se faz distinção formal entre desenvolvedores, analistas, testadores ou designers. Todos são considerados **Developers**, independentemente de sua especialidade. Isso reforça o caráter colaborativo e a responsabilidade compartilhada pelo trabalho.

### Sobreposição de Papéis: É Possível?

Uma dúvida bastante comum surge quando se observa a simplicidade na definição dos papéis no Scrum: uma mesma pessoa pode exercer dois papéis simultaneamente dentro de um Scrum Team?

A resposta para essa questão requer certa nuance. Na **versão de 2017**, o próprio Guia Scrum reconhece que os papéis de **Scrum Master** e **Product Owner** não são incluídos na contagem de membros do Development Team, **a menos que essas pessoas também estejam ativamente envolvidas no desenvolvimento do produto, executando trabalho técnico do Sprint Backlog**.

Por exemplo, em um time pequeno, pode acontecer de um Scrum Master também atuar como desenvolvedor, contribuindo diretamente para a construção do incremento. O mesmo pode ser verdadeiro para o Product Owner, desde que isso não comprometa suas responsabilidades principais.

No entanto, há uma regra bastante clara, ainda que não esteja explicitamente escrita no guia, mas é amplamente aceita na comunidade Scrum: **uma mesma pessoa jamais deve assumir, ao mesmo tempo, os papéis de Scrum Master e Product Owner**. Isso ocorre porque essas funções possuem naturezas que podem gerar **conflitos de interesse**.

O **Product Owner** é o responsável por maximizar o valor do produto, tomar decisões sobre prioridades e, muitas vezes, negociar com stakeholders. Já o **Scrum Master** atua como facilitador, defensor do processo e como um escudo para proteger o time de interferências externas. Acumular essas duas funções poderia colocar uma mesma pessoa na posição desconfortável de ser, ao mesmo tempo, quem exige e quem protege, o que comprometeria a neutralidade e a eficácia do framework.

### Exemplo Prático: Composição de um Scrum Team

Imagine uma empresa de desenvolvimento de software que está criando um novo aplicativo mobile para serviços de delivery. Esse projeto é executado utilizando Scrum. O Scrum Team é composto por:

- **Maria**, que atua como **Product Owner**. Ela é a responsável por entender as necessidades dos clientes, priorizar o Product Backlog e definir quais funcionalidades trazem mais valor para o negócio.
- **Carlos**, que assume o papel de **Scrum Master**. Ele garante que o time entenda os princípios do Scrum, facilita as cerimônias, remove impedimentos e protege o time contra interferências externas.
- **Ana**, **Lucas**, **Fernanda** e **João**, que são os **Developers**. Embora cada um tenha especializações (Ana é desenvolvedora backend, Lucas é frontend, Fernanda é especialista em UX/UI e João é tester), todos trabalham colaborativamente, se ajudam mutuamente, e são igualmente responsáveis por entregar um incremento funcional do produto ao final de cada Sprint.

Este time é pequeno o suficiente para ser ágil, mas possui todas as competências necessárias para, de forma autônoma, construir, testar, validar e entregar o aplicativo.

Quando surge a dúvida se Carlos, além de Scrum Master, poderia assumir o papel de Product Owner, a resposta é claramente não. Isso porque a imparcialidade de Carlos como facilitador do processo ficaria comprometida, especialmente em momentos de negociação de prioridades, conflitos sobre requisitos ou tomadas de decisão críticas.

### Product Owner (PO) – O Guardião do Valor

O **Product Owner**, ou simplesmente PO, é o responsável por maximizar o valor do produto desenvolvido. Essa pessoa é quem representa os interesses dos stakeholders, clientes e usuários, traduzindo suas necessidades em requisitos organizados no **Product Backlog**.

<div align="center">
  <img width="680px" src="./img/14-scrum-product-owner.png">
</div>

O PO não é um comitê, nem um grupo de pessoas — é **uma pessoa única**, com autoridade sobre as decisões relacionadas ao produto. Esse princípio é fundamental para garantir clareza, foco e alinhamento nas prioridades do time. Embora o Product Owner possa ouvir opiniões, sugestões ou recomendações de diversos stakeholders, a decisão final sobre o que será priorizado cabe exclusivamente a ele.

O sucesso do PO, no entanto, não depende apenas de sua capacidade individual, mas também do apoio e respeito da organização às suas decisões. Essas decisões são refletidas diretamente na ordem dos itens no Product Backlog e, consequentemente, no direcionamento do trabalho do Scrum Team.

As responsabilidades do Product Owner:

- **Gestão do Produto:** O PO atua na **macrogestão**, sendo responsável pela visão do produto, suas metas e pela entrega de valor para o negócio.
- **Maximização do Valor:** Garante que o trabalho do Scrum Team resulte no maior valor possível para o negócio e para os clientes.
- **Gestão do Product Backlog:** É o único responsável por criar, atualizar, ordenar e priorizar os itens do Product Backlog.
- **Definição e Comunicação da Meta do Produto:** No Scrum Guide de 2020, uma novidade foi a inclusão da **Meta do Produto** (Product Goal), que é de responsabilidade do PO comunicar de forma clara.
- **Visibilidade e Transparência:** Mantém o Product Backlog claro, visível e transparente para todos os envolvidos.
- **Garantia do ROI:** O PO é responsável por assegurar que o produto gere retorno sobre o investimento (ROI) da organização.
- **Facilitação do Entendimento:** Garante que os desenvolvedores compreendam suficientemente os itens do Product Backlog, tanto em termos de requisitos quanto de valor.

Algumas observações importantes:

- O PO pode delegar tarefas operacionais de gestão do Product Backlog, mas **a responsabilidade nunca é delegada**, ou seja, ele sempre será o responsável pela gestão.
- Nenhuma pessoa fora do PO pode alterar prioridades do backlog. Caso stakeholders queiram mudanças, devem negociar diretamente com ele.
- Para que esse papel funcione, é imprescindível que a organização apoie e respeite suas decisões, não permitindo interferências externas que prejudiquem o foco da equipe.

Exemplo prático:

Imagine que uma empresa de tecnologia está desenvolvendo um aplicativo de entregas. O Product Owner desse projeto representa os interesses dos usuários, dos donos de restaurantes, dos entregadores e dos investidores. Cabe a ele decidir se a próxima funcionalidade será um sistema de rastreamento de entregas em tempo real, uma melhoria no sistema de pagamentos, ou a adição de um sistema de avaliação dos entregadores. Cada uma dessas decisões deve considerar o que gera mais valor no momento, respeitando a estratégia de negócio.

### Developers (DV) – Os Construtores do Produto

Os **Developers**, ou desenvolvedores, são as pessoas responsáveis por transformar os itens priorizados no Product Backlog em funcionalidades prontas e utilizáveis. Eles são os verdadeiros construtores do produto e estão comprometidos em entregar, ao final de cada Sprint, um incremento que gere valor.

<div align="center">
  <img width="680px" src="./img/14-scrum-developers.png">
</div>

Com a atualização do Scrum Guide em 2020, o antigo termo **"Time de Desenvolvimento"** foi substituído por **"Developers"**. Essa mudança não é apenas semântica, mas também conceitual, pois elimina a percepção equivocada de que existe um sub-time dentro do Scrum Team. Agora, fica claro que o Scrum Team é uma unidade única, composta por três conjuntos de responsabilidades: **Product Owner, Scrum Master e Developers**, todos comprometidos com o mesmo objetivo.

As principais características dos Developers:

- **Auto-organização:** Eles decidem, de forma autônoma, como transformar os itens do Product Backlog em incrementos de produto. Nenhuma pessoa externa, nem mesmo o Scrum Master, pode determinar como eles farão seu trabalho.
- **Multifuncionalidade:** O grupo possui todas as habilidades necessárias para construir o incremento. Isso inclui atividades como análise, design, codificação, testes, documentação, entre outros.
- **Responsabilidade Coletiva:** Embora os membros possam ter especializações (ex.: programador, testador, analista), a responsabilidade pela entrega é coletiva. O Scrum não reconhece títulos internos.
- **Sem Subtimes:** Não existem subgrupos formais dentro dos Developers. Todos trabalham juntos, colaborativamente, na busca pela meta da Sprint.
- **Comprometimento com a Qualidade:** São responsáveis por garantir que os incrementos estejam aderentes à **Definição de Pronto** (Definition of Done).

Já as responsabilidades dos Developers:

- **Criação do Sprint Backlog:** Planejam o trabalho necessário para atingir a **Meta da Sprint**.
- **Comprometimento com a Entrega:** Trabalham diariamente adaptando seu plano para garantir que a meta da Sprint seja atingida.
- **Autogerenciamento:** Organizam-se sem a necessidade de um gerente externo.
- **Garantia de Qualidade:** Introduzem qualidade no trabalho, assegurando que tudo esteja aderente à Definição de Pronto.
- **Responsabilidade Compartilhada:** Mantêm um compromisso mútuo e se responsabilizam uns pelos outros.

O Scrum Guide 2020 recomenda, sem impor regras rígidas, que o **Scrum Team tenha 10 pessoas ou menos**, incluindo Product Owner e Scrum Master. Isso busca equilibrar agilidade e capacidade produtiva, evitando os problemas de comunicação e coordenação que surgem em times muito grandes.

Exemplo prático:

Suponha que, no desenvolvimento de um sistema bancário, o Product Backlog tenha uma funcionalidade que permite aos clientes solicitar cartões de crédito. Os Developers planejam como vão desenvolver essa funcionalidade, definindo quais atividades serão necessárias (análise, implementação, testes, validação). Ninguém de fora pode interferir nesse planejamento ou dizer como eles devem executar suas tarefas. Eles são responsáveis, juntos, por transformar esse item em uma funcionalidade pronta para ser entregue.

### Scrum Master (SM) – O Guardião do Processo

O **Scrum Master**, ou SM, é frequentemente chamado de **servo-líder** do Scrum Team. Sua principal missão é garantir que o Scrum esteja sendo bem compreendido, implementado e seguido. O SM atua como um facilitador, mentor e coach, tanto para o Scrum Team quanto para a organização.

<div align="center">
  <img width="680px" src="./img/14-scrum-master.png">
</div>

Diferente de um gerente tradicional, o Scrum Master **não possui autoridade hierárquica sobre o time**, tampouco toma decisões sobre o que será feito. Sua autoridade é sobre o processo, não sobre as pessoas. Ele atua removendo impedimentos, facilitando discussões, promovendo a melhoria contínua e garantindo que os princípios e valores do Scrum sejam respeitados.

São responsabilidades do Scrum Master:

- **Gestão do Processo:** Garante que o framework Scrum seja bem compreendido e corretamente aplicado.
- **Facilitação:** Atua como facilitador das cerimônias (eventos) do Scrum, assegurando que elas aconteçam de forma eficiente, mas não as conduz.
- **Mentoria e Coaching:** Treina o Product Owner, os Developers e até a organização nos princípios e práticas do Scrum.
- **Remoção de Impedimentos:** Atua ativamente para remover qualquer barreira que esteja atrapalhando o time na entrega da Sprint.
- **Proteção do Time:** Protege o Scrum Team de interferências externas que possam comprometer seu foco e desempenho.
- **Transformação Organizacional:** Trabalha com a organização para ajudá-la a compreender como interagir com times Scrum, promovendo uma cultura ágil.

A atuação do Scrum Master nos três níveis é:

- **Para o Product Owner:**
    - Ajuda na definição e ordenação do Product Backlog.
    - Ensina técnicas eficazes de gestão de backlog.
    - Apoia na comunicação da visão, das metas e dos itens do backlog para o time.

- **Para os Developers:**
    - Treina o time em autogestão, colaboração e melhoria contínua.
    - Facilita a remoção de impedimentos que possam comprometer a Sprint.
    - Auxilia o time na busca pela qualidade e entrega de valor.

- **Para a Organização:**
    - Lidera e apoia a transformação ágil da organização.
    - Promove entendimento sobre o Scrum, seus papéis, eventos e artefatos.
    - Ensina quais interações com o Scrum Team são úteis e quais devem ser evitadas.

Exemplo prático:

Imagine um time que, durante uma Sprint, percebe que depende da liberação de um ambiente de homologação, controlado por outra área da empresa. O Scrum Master atua imediatamente, conversando com os responsáveis, intermediando negociações, buscando soluções para remover esse impedimento, permitindo que o time não perca seu ritmo. Além disso, ele ajuda a organização a entender que criar gargalos externos pode comprometer o fluxo ágil de trabalho.


## Artefatos do Scrum

Dentro do framework Scrum, os artefatos são elementos fundamentais que proporcionam transparência, alinhamento e foco no trabalho que está sendo realizado pela equipe. Segundo o **Guia do Scrum**, os artefatos têm como objetivo representar trabalho ou valor, e são projetados para maximizar a transparência das informações essenciais, de forma que todos — tanto membros da equipe quanto stakeholders — compartilhem uma visão comum e inequívoca sobre o estado atual do projeto.

A definição dos artefatos evoluiu ao longo dos anos. Na versão de 2017 do Guia, enfatizava-se que os artefatos eram instrumentos para fornecer transparência e viabilizar a inspeção e a adaptação — dois dos três pilares do empirismo que sustentam o Scrum. Já na versão de 2020, além de manter esse princípio, o Guia reforça a importância dos compromissos associados a cada artefato, tornando-os não apenas repositórios de informações, mas também instrumentos de foco e comprometimento com os objetivos do time.

Os três artefatos oficiais do Scrum são:

- **Product Backlog** (Backlog do Produto);
- **Sprint Backlog** (Backlog da Sprint);
- **Product Increment** (Incremento do Produto).

Cada artefato está diretamente ligado a um compromisso que sustenta sua transparência e orienta seu uso:

- O **Product Backlog** está vinculado à **Meta do Produto**.
- O **Sprint Backlog** está vinculado à **Meta da Sprint**.
- O **Incremento** está vinculado à **Definição de Pronto** (Definition of Done).

Esses compromissos existem para reforçar o empirismo e os valores do Scrum, ajudando o Scrum Team e seus stakeholders a manterem um alinhamento claro sobre o progresso e os objetivos.

### Product Backlog

O **Product Backlog** é, essencialmente, uma lista ordenada, dinâmica e viva que contém tudo o que é necessário para que o produto atenda às expectativas das partes interessadas. É o artefato que concentra **todos os requisitos conhecidos do produto**, sejam eles funcionais, não funcionais, técnicos, de infraestrutura ou até mesmo relacionados à mitigação de riscos.

Mas, afinal, o que significa o termo **“backlog”**? No contexto geral, backlog pode ser entendido como uma **fila de trabalho**, uma lista de pendências ou um histórico acumulado de demandas a serem executadas. No caso do Scrum, o **Product Backlog** representa exatamente isso: um repositório organizado de tudo aquilo que o time deve considerar para desenvolver, manter e evoluir o produto.

Por definição, o Product Backlog **nunca estará completo**. Ele é um artefato **em constante evolução**, que se modifica sempre que surgem novos requisitos, mudanças nas prioridades, descobertas técnicas ou até alterações nas necessidades do mercado e dos usuários. Enquanto o produto existir, o Product Backlog também existirá.

A responsabilidade pela gestão do Product Backlog é do **Product Owner (PO)**. É ele quem decide:

- Quais itens entram no backlog;
- Como eles são descritos;
- A ordem em que aparecem (priorização);
- E quando eles podem ser removidos.

É importante ressaltar que, embora o PO tenha essa responsabilidade, o refinamento dos itens do backlog é uma atividade que **envolve todo o Scrum Team**, especialmente os Developers, que ajudam a esclarecer dúvidas, decompor funcionalidades e estimar esforços.

#### Representação dos Itens

De modo geral, os itens do Product Backlog são comumente representados por **Histórias de Usuário (User Stories)**, mas isso não é uma regra fixa. Dependendo do contexto e da maturidade da equipe, podem ser utilizados outros formatos, como:

- Descrições textuais de funcionalidades;
- Cenários de casos de uso;
- Especificações técnicas;
- Itens de arquitetura;
- Demandas de infraestrutura;
- E até itens relacionados à redução de riscos ou pagamento de dívidas técnicas.

Uma **História de Usuário**, por exemplo, é uma forma simples, centrada no usuário, de descrever um requisito. Normalmente segue o formato:

> "**Como** [tipo de usuário], **eu quero** [ação/desejo] **para que** [objetivo/motivo]."

Por exemplo:

> "Como usuário do Twitter, eu quero poder excluir um tuíte, para que eu possa remover publicações indesejadas."

Itens mais importantes e de maior valor para o negócio são priorizados no topo do backlog. Naturalmente, são também os itens mais bem detalhados, pois estarão mais próximos de serem selecionados para desenvolvimento. Já os itens de menor prioridade ficam mais abaixo, geralmente com menos detalhes, podendo ser refinados no futuro, caso sua prioridade aumente.

#### A Dinâmica do Product Backlog

É perfeitamente comum que, no decorrer do projeto, algumas funcionalidades percam importância e sejam até descartadas, enquanto outras sejam adicionadas. Isso reflete a realidade dinâmica dos ambientes ágeis, onde mudanças são esperadas e bem-vindas, desde que gerem valor.

Por exemplo, imagine que você está desenvolvendo uma rede social chamada **DevConnect**, voltada para desenvolvedores. No início, seu Product Backlog contém itens como:

- Permitir postar conteúdos (alta prioridade);
- Sistema de curtidas (alta prioridade);
- Chat entre usuários (média prioridade);
- Feed personalizado por linguagens (baixa prioridade).

Após algumas sprints, percebe-se que o chat não é tão relevante, pois os usuários estão mais interessados em interações públicas via comentários. Assim, esse item é descartado. Por outro lado, surge uma nova necessidade: permitir integração com repositórios do GitHub, que então entra no topo do backlog.

#### Definition of Ready (DoR)

Para que um item do Product Backlog esteja apto a ser selecionado durante o **Sprint Planning**, ele deve estar suficientemente claro e pequeno a ponto de ser concluído dentro de uma única sprint. Esse estado é conhecido como **Definition of Ready (DoR)**.

A **DoR** não é uma prática oficial do Scrum, mas é amplamente utilizada por muitas equipes ágeis para evitar problemas comuns, como requisitos mal definidos ou divergências de entendimento entre PO e Developers. Trata-se de um **acordo interno do time** que define critérios mínimos para que um item seja considerado pronto para desenvolvimento.

Por exemplo, a DoR pode estabelecer que uma história de usuário deve conter:

- Descrição clara e objetiva;
- Critérios de aceitação bem definidos;
- Impactos técnicos ou dependências identificadas;
- Esforço estimado;
- Validação de que não existem bloqueios.

Se um item não atende à DoR, ele não deve ser selecionado para a sprint.

Por exemplo, em uma equipe se o Product Owner entregasse histórias de usuário com descrições vagas, como: “Melhorar a performance do sistema”. Isso geraria confusão: o que exatamente significava “melhorar”? Quais métricas? Quais pontos do sistema? Isso resultava em retrabalho e discussões no final das sprints.

A solução foi criar uma DoR onde, para qualquer item relacionado a performance, deveriam ser definidos:

- Métricas atuais e metas desejadas;
- Quais módulos seriam otimizados;
- Critérios claros para considerar o trabalho concluído.

A partir disso, as entregas passaram a ser muito mais alinhadas com as expectativas.

#### Refinamento do Product Backlog

O processo de **refinamento** é uma atividade contínua que envolve decompor itens grandes (épicos) em itens menores (histórias de usuário), esclarecer requisitos, discutir soluções e ajustar estimativas.

Embora o Scrum não defina uma cerimônia específica para isso, o Guia recomenda que o refinamento **não consuma mais de 10% da capacidade do time na sprint**.

O refinamento pode ocorrer de forma:

- **Formalizada**, com reuniões específicas de backlog refinement;
- **Informal**, acontecendo naturalmente durante as interações do time.

#### Monitoramento do Progresso

O Product Owner é responsável por acompanhar o progresso do time em direção à **Meta do Produto**. Isso pode ser feito utilizando métricas visuais, como:

- **Burndown Charts:** mostram a quantidade de trabalho restante na sprint ou no projeto;
- **Burnup Charts:** evidenciam o progresso acumulado até atingir o total de trabalho planejado;
- **Fluxos Cumulativos:** ajudam a identificar gargalos e analisar o fluxo de trabalho.

Contudo, vale reforçar que essas ferramentas são apenas **auxiliares**, não substituindo o princípio básico do empirismo no Scrum. Em ambientes complexos, o melhor preditor do futuro é o aprendizado obtido a partir das entregas anteriores.

#### Compromisso: Meta do Produto

A **Meta do Produto** é o compromisso associado ao Product Backlog. Trata-se de um objetivo de longo prazo que dá direção ao Scrum Team. Enquanto a sprint possui uma meta específica de curto prazo (a **Meta da Sprint**), a Meta do Produto guia a evolução do backlog, orientando as escolhas de priorização.

Um produto pode ter como meta, por exemplo:

> “Permitir que usuários criem, compartilhem e colaborem em projetos de código aberto diretamente pela plataforma.”

Enquanto essa meta não for alcançada — ou até que seja abandonada e substituída —, os itens do Product Backlog existirão para apoiar seu cumprimento.

### Sprint Backlog

O Sprint Backlog é um dos principais artefatos dentro do framework Scrum e desempenha um papel essencial no acompanhamento e na organização do trabalho que será executado durante uma Sprint. Sua correta compreensão é indispensável para a prática eficiente do Scrum, tanto em ambientes acadêmicos quanto no mercado profissional.

De forma conceitual, o Sprint Backlog pode ser definido como o conjunto de itens selecionados do Product Backlog para serem desenvolvidos durante uma Sprint, acrescido da meta da Sprint e de um plano de ação que descreve como esses itens serão transformados em um incremento de produto que seja funcional e potencialmente utilizável. Importante ressaltar que o Sprint Backlog é criado e gerenciado exclusivamente pelos desenvolvedores — ou seja, é de responsabilidade total e direta desse grupo.

Para compreender melhor este conceito, é fundamental diferenciar claramente o **Product Backlog** do **Sprint Backlog**, já que essa é uma das confusões mais comuns entre estudantes, profissionais e até candidatos em processos de certificação Scrum.

O **Product Backlog** é uma lista ordenada de tudo aquilo que é necessário no produto. Ele representa os requisitos, funcionalidades, melhorias e correções que descrevem o que o produto deve ter ou fazer. É um artefato de natureza dinâmica e evolutiva, sendo constantemente refinado e priorizado pelo Product Owner à medida que se aprende mais sobre o produto e seu contexto de desenvolvimento.

Por sua vez, o **Sprint Backlog** é um subconjunto do Product Backlog. Trata-se da seleção dos itens que foram priorizados para serem implementados em uma Sprint específica, acompanhados da meta da Sprint e de um plano detalhado que descreve como esses itens serão transformados em funcionalidades prontas. Esse plano é geralmente composto por tarefas técnicas que desmembram cada item selecionado em ações menores, concretas e viáveis de serem realizadas dentro do período da Sprint.

Uma característica essencial do Sprint Backlog é sua **alta visibilidade**. Ele funciona como uma fotografia em tempo real do trabalho que os desenvolvedores planejam concluir durante aquela Sprint. Essa visibilidade não é apenas interna ao time, mas muitas vezes é disponibilizada de forma transparente para toda a organização, especialmente em ambientes que prezam por uma cultura ágil e colaborativa.

Além disso, é fundamental compreender que o Sprint Backlog **pertence exclusivamente aos desenvolvedores**. Somente eles têm autoridade para modificá-lo ao longo da Sprint. Se, durante a execução, surgirem descobertas sobre tarefas adicionais necessárias, essas podem ser prontamente incluídas. Da mesma forma, se determinadas tarefas se mostrarem irrelevantes ou desnecessárias, podem ser removidas sem qualquer prejuízo, desde que essa alteração não comprometa a meta da Sprint.

Outro ponto relevante é que o progresso dentro da Sprint é monitorado de forma constante. A cada **Reunião Diária (Daily Scrum)**, os desenvolvedores avaliam o total do trabalho restante e utilizam essas informações para projetar se estão no caminho certo para alcançar a meta estabelecida. Isso permite ajustes rápidos, promovendo um gerenciamento dinâmico e eficiente do próprio trabalho.

#### Monitorando o Progresso: Um Processo Contínuo e Adaptativo

O acompanhamento do progresso durante a Sprint é uma prática constante e imprescindível no Scrum. A qualquer momento, o time pode somar o total de trabalho remanescente no Sprint Backlog para avaliar o quanto falta ser concluído. Esse monitoramento não é meramente operacional, mas estratégico, permitindo que os desenvolvedores tomem decisões embasadas sobre priorização, replanejamento de tarefas e eventuais renegociações com o Product Owner, caso surjam imprevistos.

Essa prática de inspecionar frequentemente o trabalho restante, somada à transparência proporcionada pelo Sprint Backlog, é o que torna o Scrum altamente adaptativo e responsivo às mudanças, garantindo que os riscos sejam minimizados e que as entregas tenham maior previsibilidade.

#### Evolução do Conceito nas Versões do Guia Scrum

A definição e os detalhes sobre o Sprint Backlog passaram por alguns ajustes importantes ao longo das versões do **Guia Scrum**.

Na versão de **2017**, o Guia Scrum introduziu um reforço explícito sobre a importância da **melhoria contínua**. Passou a ser recomendado que, a cada Sprint, fosse incluído no Sprint Backlog ao menos um item de alta prioridade relacionado à melhoria dos processos internos, identificado na reunião de retrospectiva anterior. Esse movimento destacava que o Scrum não se concentra apenas na entrega de funcionalidades, mas também na evolução constante do próprio time, dos processos e das práticas adotadas.

Contudo, na versão de **2020**, essa recomendação específica foi removida do texto oficial. Isso não significa, porém, que a melhoria contínua deixou de ser um princípio do Scrum; ela continua sendo um valor implícito e fundamental. A diferença é que, na nova abordagem, oferece-se mais liberdade e autonomia para que os times decidam como conduzir essa melhoria, sem uma prescrição direta no artefato Sprint Backlog.

Além dessa alteração, a versão de 2020 trouxe uma descrição mais clara e organizada sobre os componentes do Sprint Backlog. Passou-se a defini-lo explicitamente como composto por três elementos principais:

- **Por que:** A **Meta da Sprint**, que fornece propósito, alinhamento e foco. Ela representa o objetivo que o time busca atingir naquela Sprint, funcionando como um guia norteador.
- **O que:** O conjunto de **itens selecionados do Product Backlog**, que descrevem o que será entregue no incremento da Sprint.
- **Como:** O **plano de ação**, que detalha as atividades e tarefas necessárias para transformar os itens selecionados em um incremento funcional.

Essa estrutura permite uma visão muito mais clara, tanto para os membros do time quanto para stakeholders externos, sobre os objetivos, escopo e estratégias daquela Sprint.

#### Compromisso: Meta da Sprint

Com as atualizações mais recentes do Scrum, o Sprint Backlog passou a incorporar formalmente um compromisso: a **Meta da Sprint**.

A Meta da Sprint não é apenas um artefato textual, mas um elemento que exerce papel fundamental no direcionamento do trabalho. Embora ela seja um compromisso assumido coletivamente pelos desenvolvedores, ela também oferece flexibilidade. Isso significa que, embora a Meta deva ser preservada durante a Sprint, o caminho exato para alcançá-la pode ser ajustado. Se, no decorrer da Sprint, os desenvolvedores perceberem que algumas tarefas precisam ser modificadas, substituídas ou até removidas, isso pode ser feito desde que a Meta da Sprint continue sendo viável e relevante.

Essa abordagem promove alinhamento, foco e colaboração, evitando que o time se disperse em atividades paralelas ou sem impacto real para o objetivo daquela iteração.

Para ilustrar de forma concreta como se estrutura um Sprint Backlog, podemos recorrer a um exemplo prático baseado no Guia Scrum de 2017 (Backlog do Produto Twitter):

<div align="center">
  <img width="880px" src="./img/14-scrum-backlog.png">
</div>

Imagine que estamos desenvolvendo uma plataforma social e, na **Sprint 2**, a meta é que os usuários possam realizar publicações no Twitter integrado à plataforma.

- No lado esquerdo do quadro, temos itens do **Product Backlog**, como:
	- Cadastrar novo usuário;
    - Login de usuários já cadastrados.
- No lado direito, aparece o **Sprint Backlog**, que detalha as tarefas necessárias para transformar esses itens em funcionalidades completas, tais como:
    - Ativar login com usuário Gmail;
    - Estruturar log;
    - Criar tabelas no banco de dados, etc.

Essas tarefas representam o “**como**” e detalham tecnicamente o que precisa ser feito para entregar as funcionalidades prometidas. A Meta da Sprint, nesse caso, funciona como o grande objetivo motivador: **“Usuários poderão estar no Twitter”**.

À medida que o time avança na Sprint, as tarefas são marcadas como concluídas, ajustadas ou, quando necessário, atualizadas, sempre tendo em mente o progresso em direção à Meta.

### Product Increment

Um dos conceitos mais fundamentais dentro do Scrum, e que representa de forma muito clara a natureza empírica desse framework, é o **Product Increment**, ou Incremento de Produto.

O Incremento de Produto corresponde à **soma de todos os itens do Product Backlog que foram concluídos durante uma Sprint**, somando-se ainda ao valor dos incrementos produzidos nas Sprints anteriores. No entanto, para que um incremento seja efetivamente considerado válido e relevante, ele precisa atender a um critério extremamente importante no Scrum: estar **“Pronto” (Done)**, segundo um conjunto de critérios muito bem estabelecido e compartilhado por toda a equipe.

Ao final de cada Sprint, os desenvolvedores entregam um incremento do produto, que nada mais é do que o resultado prático de todo o trabalho realizado durante aquele ciclo. Esse conceito não apenas reflete o progresso obtido, mas também permite que o Product Owner visualize claramente o retorno sobre o investimento realizado até aquele ponto. Além disso, torna possível avaliar quais são os próximos passos, se surgem novas necessidades, ou se há oportunidade para revisar prioridades.

Do ponto de vista da equipe de desenvolvimento, é essencial compreender que o incremento precisa ser algo **potencialmente entregável** ou **liberável**. E por que utilizamos o termo “potencialmente”? Isso ocorre porque, embora o incremento esteja tecnicamente pronto para ser utilizado, **a decisão de efetivamente liberá-lo ou não cabe ao Product Owner**, que irá avaliar se aquele incremento agrega valor imediato para os stakeholders ou se sua disponibilização deve ser postergada.

Essa exigência de que o incremento seja potencialmente liberável leva a uma reflexão extremamente relevante sobre a qualidade do que é produzido. O Scrum deixa claro que um trabalho só pode ser considerado concluído se ele realmente atender a critérios objetivos de qualidade, completude e aderência às expectativas.

É justamente nesse contexto que surge o conceito de **Definition of Done (DoD)**, ou **Definição de Pronto**, um dos compromissos essenciais dentro do Scrum.

#### A Importância da Definition of Done (DoD)

Quando dizemos, em uma equipe ágil, que uma funcionalidade está “pronta”, isso precisa significar, de forma inequívoca, que ela está completa, testada e com qualidade suficiente para ser entregue, sem a necessidade de complementações posteriores.

O DoD é, portanto, um **acordo formal, objetivo e transparente**, que define quais são os critérios mínimos que devem ser cumpridos para que um item do Product Backlog, ou um incremento, seja considerado concluído. Trata-se, na prática, de um **checklist de critérios de aceite**, que contempla todas as etapas e atividades necessárias — como desenvolvimento, testes unitários, testes de integração, revisão de código, documentação mínima, implantação em ambiente de testes, entre outras etapas que a equipe julgar pertinentes.

Dessa maneira, o DoD funciona como uma espécie de **contrato informal entre os Developers e o Product Owner**, assegurando que todo incremento produzido esteja dentro dos padrões de qualidade esperados e acordados desde o início.

#### Definition of Done versus Definition of Ready

Para que possamos compreender melhor a função do DoD, é interessante compará-lo com outro conceito muito conhecido em ambientes ágeis: o **Definition of Ready (DoR)**.

Enquanto o DoR representa um conjunto de critérios que define quando um item do Product Backlog está suficientemente bem descrito, claro, viável e compreendido para que os desenvolvedores possam iniciá-lo, o DoD atua na outra ponta do processo. Ele define quando o trabalho sobre esse item pode ser considerado efetivamente concluído e pronto para entrega ou validação.

Em outras palavras:

- O **DoR** responde à pergunta: “Quando podemos começar a trabalhar nisso?”
- O **DoD** responde à pergunta: “Quando isso está realmente pronto?”

Ambos são critérios de aceite, mas se aplicam a momentos diferentes do ciclo de desenvolvimento. Importante ressaltar que, embora o DoR seja uma prática recomendada, ele não é obrigatório no framework Scrum. Já o **Definition of Done é obrigatório**, sendo um dos principais pilares que garantem a qualidade e a transparência no trabalho realizado.

#### Critérios Comuns na Definition of Done

Ainda que o DoD possa variar bastante entre diferentes equipes e organizações, existem critérios que costumam ser recorrentes, tais como:

- Código implementado, testado e funcionando conforme os requisitos;
- Passagem bem-sucedida por testes unitários e de integração;
- Validação dos critérios de aceite definidos pelo Product Owner;
- Código revisado, documentado e versionado corretamente;
- Implementação de testes automatizados, quando aplicável;
- Publicação em ambiente de homologação para validação;
- Aprovação formal por parte do Product Owner.

Naturalmente, esses critérios podem (e devem) ser adaptados às necessidades, características e maturidade da equipe e da organização.

#### O Que Acontece se Algo Não Está Pronto?

No Scrum, se uma funcionalidade não atende aos critérios estabelecidos no DoD ao final da Sprint, ela **não pode ser considerada parte do incremento**. Isso significa que ela não será apresentada na Sprint Review, muito menos liberada para produção ou para os stakeholders.

O destino desse item é simples: ele retorna ao **Product Backlog**, onde poderá ser replanejado, reestimado e priorizado novamente em uma Sprint futura.

Esse comportamento reforça o princípio da **qualidade como um valor inegociável** dentro do Scrum. Afinal, não faz sentido entregar funcionalidades incompletas, mal testadas ou que não atendem às expectativas dos clientes e usuários.

#### Evolução Contínua da Definition of Done

Outro aspecto muito interessante e relevante sobre o DoD é que ele **não é um documento estático**. Assim como o próprio time e o próprio produto evoluem ao longo do tempo, é perfeitamente natural que os critérios do DoD também evoluam.

À medida que a equipe adquire mais maturidade, desenvolve melhores práticas e supera desafios técnicos e operacionais, ela pode — e deve — revisar seu Definition of Done, tornando-o mais robusto, completo e alinhado com as necessidades do negócio.

Por exemplo, uma equipe que inicialmente não tinha capacidade para realizar testes automatizados pode incluir esse critério no DoD após adquirir domínio sobre ferramentas e processos de automação. Da mesma forma, uma organização que passa a adotar padrões corporativos de segurança ou compliance pode exigir que esses requisitos sejam incorporados ao DoD de todos os times.

#### Padrões Organizacionais e DoD Compartilhado

Quando falamos de organizações que possuem múltiplos times Scrum trabalhando sobre um mesmo produto ou plataforma, surge a necessidade de um alinhamento ainda mais rigoroso sobre o que significa “pronto”.

Se a organização já possui **padrões organizacionais de Definition of Done**, todos os times devem segui-los como critério mínimo. Caso contrário, cabe aos times envolvidos estabelecerem, de forma colaborativa, um **Definition of Done compartilhado**, garantindo que todos os incrementos produzidos estejam alinhados e compatíveis entre si.

Essa prática não só evita retrabalho, como também assegura que o produto como um todo mantenha uma consistência em termos de qualidade, arquitetura, testes e entrega de valor.

#### Product Increment no Guia Scrum 2017 e 2020

É interessante observar que o conceito de Product Increment foi evoluindo nas diferentes versões do Guia Scrum.

- **No Guia Scrum 2017**, o incremento era descrito como:

> “A soma de todos os itens do Backlog do Produto completados durante a Sprint e o valor dos incrementos de todas as Sprints anteriores. Ao final da Sprint um novo incremento deve estar ‘Pronto’, o que significa que deve estar na condição de ser utilizado e atender à definição de ‘Pronto’ do Time Scrum.”

- **No Guia Scrum 2020**, a definição foi aprimorada, enfatizando ainda mais a relação entre o incremento e a meta do produto:

> “Um incremento é um trampolim concreto em direção à Meta do Produto. Cada incremento é adicionado a todos os incrementos anteriores e completamente verificado, garantindo que todos os incrementos funcionem juntos. A fim de fornecer valor, o incremento deve ser utilizável. Vários incrementos podem ser criados em uma Sprint. A soma dos incrementos é apresentada na Sprint Review, apoiando assim o empirismo. No entanto, um incremento pode ser entregue aos stakeholders antes do final da Sprint.”

Percebe-se, portanto, uma evolução no entendimento do papel do incremento, não apenas como um artefato isolado da Sprint, mas como um componente essencial no avanço contínuo em direção à **Meta do Produto (Product Goal)**.

#### Compromisso Associado: Definition of Done

No Scrum, cada artefato possui um compromisso que reforça sua transparência e sua entrega de valor. No caso do Product Increment, esse compromisso é exatamente o **Definition of Done (DoD)**.

De acordo com o Guia Scrum 2020:

> “A Definition of Done é uma descrição formal do estado do Incremento quando ele atende às medidas de qualidade exigidas para o produto. No momento em que um item do Product Backlog atende à Definition of Done, um incremento nasce.”

Isso significa que a DoD não apenas define o que é considerado “pronto”, mas também atua como um elemento de transparência dentro do framework, garantindo que todos os membros da equipe e os stakeholders tenham um entendimento comum e inequívoco sobre o que foi efetivamente entregue.

Caso algum item do Product Backlog não atenda aos critérios da DoD, ele não pode ser incluído no incremento, não pode ser apresentado na Sprint Review, nem muito menos ser liberado para produção. Esse item deve, portanto, ser devolvido ao Product Backlog para ser reavaliado, priorizado e tratado no futuro.

### Burndown Chart: Um Artefato Complementar

Embora não seja oficialmente reconhecido como um artefato formal no Guia Scrum, é muito comum — e altamente recomendado — que os times utilizem o **Gráfico Burndown** como uma ferramenta de apoio ao acompanhamento do trabalho durante a Sprint.

O Burndown Chart permite **visualizar, de forma clara e objetiva, o progresso do time em direção ao cumprimento dos itens planejados para a Sprint**, comparando o trabalho planejado com o trabalho efetivamente realizado ao longo dos dias.

Basicamente, o gráfico apresenta uma linha descendente que representa a quantidade estimada de trabalho restante. As unidades normalmente utilizadas são horas de esforço, pontos de história ou qualquer outra métrica acordada pela equipe.

<div align="center">
  <img width="640px" src="./img/14-scrum-burndown-chart.png">
</div>

Essa ferramenta é extremamente útil não apenas para o time, mas também para os gestores, que podem acompanhar o andamento da Sprint e identificar eventuais desvios, gargalos ou necessidades de ajuste, tanto em relação ao tempo quanto ao esforço empregado.

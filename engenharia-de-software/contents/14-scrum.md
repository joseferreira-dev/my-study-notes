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

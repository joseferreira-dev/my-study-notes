# Capítulo 22 – Modelagem Ágil: Entender e Comunicar Antes de Documentar

Um dos equívocos mais comuns sobre o desenvolvimento ágil é a ideia de que ele elimina a necessidade de modelagem e documentação. A imagem de equipes ágeis que simplesmente "saem codificando" sem qualquer tipo de planejamento ou design é uma caricatura que não corresponde à realidade das práticas eficazes. Na verdade, muitos métodos ágeis incluem significativas sessões de modelagem. A verdadeira questão não é **se** devemos modelar, mas **como** e **para quê**.

É para responder a essas perguntas que surge a **Modelagem Ágil (Agile Modeling - AM)**. Longe de ser um processo rígido, ela se apresenta como uma metodologia baseada em práticas para a modelagem e documentação eficaz e leve de sistemas de software, funcionando como um guia para aplicar a modelagem de forma inteligente dentro de um contexto ágil.

## O Que é Modelagem Ágil?

A Modelagem Ágil é, em sua essência, um conjunto de melhores práticas. De forma mais específica, pode ser definida como um conjunto de **valores, princípios e práticas para a modelagem de software** que pode ser combinado com outras metodologias de desenvolvimento.

Um conceito fundamental para compreender a Modelagem Ágil é que ela **não é um processo completo de software**. Ela não prescreve as atividades de programação, teste, gerenciamento de projetos, implantação ou operações do sistema. Em vez disso, ela atua como um **complemento**, um guia que pode ser sobreposto a outros processos para aprimorá-los. Por exemplo:

- Para projetos que usam **Extreme Programming (XP)**, a Modelagem Ágil descreve explicitamente como melhorar a produtividade através da adição de atividades de modelagem eficazes.
- Para projetos que usam o **Processo Unificado (RUP)**, ela descreve como agilizar e enxugar as atividades de modelagem e documentação para melhorar a produtividade e reduzir a burocracia.

A Modelagem Ágil não é um processo de desenvolvimento prescritivo. Ela não define procedimentos detalhados e rígidos sobre como criar um tipo específico de modelo, mas oferece sugestões e diretrizes sobre como realizar uma modelagem efetiva e como ser um modelador mais eficiente. Ela busca um equilíbrio, misturando a flexibilidade de práticas simples de modelagem com a ordem inerente aos artefatos de modelagem de software.

Como afirma Craig Larman, um importante autor da área, a finalidade da modelagem é principalmente **entender e comunicar, não documentar**. A documentação pode ser um subproduto útil, mas o objetivo primário é a construção de um entendimento compartilhado entre os membros da equipe e os stakeholders.

## Os Pilares da Modelagem Ágil: Valores e Princípios

A Modelagem Ágil é sustentada por um conjunto de valores e princípios que guiam a mentalidade e as ações da equipe.

### Valores Fundamentais

Os valores da Modelagem Ágil são uma extensão direta dos valores do Extreme Programming (XP), sendo eles: **comunicação, simplicidade, feedback e coragem**, adicionando um quinto elemento crucial: a **humildade**.

1. **Comunicação:** A modelagem é vista como uma ferramenta poderosa para facilitar a comunicação. Um diagrama simples desenhado em um quadro branco pode transmitir uma ideia complexa de forma muito mais eficaz do que parágrafos de texto.
2. **Simplicidade:** O desenvolvimento deve focar na solução mais simples possível que atenda às necessidades atuais. Os modelos devem ser igualmente simples e fáceis de entender, evitando complexidade desnecessária.
3. **Feedback:** A modelagem ágil promove ciclos de feedback rápidos. Ao criar um modelo e discuti-lo com os stakeholders, a equipe obtém feedback imediato sobre seu entendimento dos requisitos e do design.
4. **Coragem:** É preciso coragem para tomar decisões de design simples e confiar que será possível evoluí-las no futuro. Também é preciso coragem para descartar um modelo que não está mais agregando valor, mesmo que tempo tenha sido investido nele.
5. **Humildade:** Este é o valor adicionado pela Modelagem Ágil. Ele representa a capacidade de admitir que você não sabe de tudo e que os outros membros da equipe e stakeholders têm contribuições valiosas para adicionar ao projeto. A humildade é a base para uma colaboração genuína e eficaz.

As chaves para o sucesso são uma comunicação eficaz entre todos os participantes do projeto no esforço para desenvolver a solução mais simples possível, e a humildade para reconhecer que as melhores ideias podem vir de qualquer lugar.

### Princípios Essenciais

Além dos valores, a Modelagem Ágil é guiada por princípios que orientam a aplicação prática da modelagem.

- **Modelar com um Propósito:** Antes de criar qualquer modelo, a equipe deve se perguntar: "Por que estamos criando este modelo? Que problema ele nos ajuda a resolver?". Um modelo só deve ser criado se tiver um propósito claro, como explorar um requisito complexo, comunicar uma decisão de design ou analisar uma alternativa técnica.
- **Adotar Ferramentas Simples:** A recomendação é sempre utilizar a ferramenta mais simples possível para a tarefa. Muitas vezes, um quadro branco, post-its e uma câmera de celular para registrar o resultado são mais eficazes do que uma ferramenta CASE complexa. Ferramentas simples incentivam a criatividade, a colaboração e permitem modificações rápidas.
- **O Software é o Artefato Primário:** A Modelagem Ágil reconhece que o objetivo final é entregar software funcional. Os modelos são um meio para atingir esse fim, não o fim em si. O software em funcionamento é a medida definitiva de progresso.
- **Criar Múltiplos Modelos:** Cada tipo de modelo (diagrama de classes, diagrama de sequência, etc.) tem seus pontos fortes e fracos. Um modelador ágil eficaz possui uma gama de modelos em sua "caixa de ferramentas" e sabe aplicar o modelo certo para a situação certa, em vez de tentar forçar um único tipo de modelo para todos os problemas.
- **Modelos Apenas o Suficiente (Just Barely Good Enough):** Um modelo ou documento deve ser suficiente para a situação em questão e nada mais. Ele não precisa ser perfeito, exaustivamente detalhado ou esteticamente impecável. Ele precisa ser "bom o suficiente" para cumprir seu propósito de comunicação e entendimento. A busca pela perfeição em um modelo pode levar a um desperdício de tempo e esforço.

## As Práticas da Modelagem Ágil no Ciclo de Vida

A Modelagem Ágil se manifesta através de um conjunto de práticas que se integram ao ciclo de vida de um projeto ágil. Elas não são fases rígidas, mas atividades que ocorrem em diferentes momentos, conforme a necessidade.

<div align="center">
  <img width="680px" src="22-praticas-da-modelagem-agil.png">
</div>

|**Prática**|**Descrição**|
|---|---|
|**Arquitetura de Previsão (Architectural Envisioning)**|No início de um projeto ágil, é útil que a equipe invista um tempo em um trabalho de modelo arquitetônico inicial e de alto nível. O objetivo não é criar um design detalhado, mas sim identificar uma estratégia técnica viável para a solução, definindo as principais tecnologias, componentes e padrões que serão utilizados.|
|**Modelagem de Iteração (Iteration Modeling)**|No início de cada iteração (ou Sprint), a equipe realiza um pouco de modelagem como parte de suas atividades de planejamento. Isso ajuda a detalhar os requisitos que serão implementados naquela iteração e a pensar sobre as tarefas de design necessárias.|
|**Basta Apenas o Suficiente (Just Barely Good Enough)**|Um modelo ou documento deve conter apenas a informação suficiente para a situação em questão, e nada mais. Evita-se a super-documentação e o excesso de detalhes que não agregam valor imediato.|
|**Modelagem Futura (Look-Ahead Modeling)**|Embora o foco seja no presente, às vezes é necessário modelar um pouco à frente para reduzir um risco global. Por exemplo, se uma funcionalidade futura depende de uma integração complexa, a equipe pode fazer uma pequena prova de conceito ou um diagrama de sequência para investigar a viabilidade dessa integração antes de se comprometer com ela.|
|**Modelo de Storming (Model Storming)**|Ao longo da iteração, é comum que a equipe se reúna por alguns minutos ao redor de um quadro branco para "modelar em tempestade". O objetivo é explorar rapidamente os detalhes por trás de um requisito ou pensar em uma solução para um problema de design específico. São sessões de modelagem curtas, focadas e colaborativas.|
|**Múltiplos Modelos**|Um desenvolvedor eficaz terá uma gama de modelos à sua disposição (diagramas de classe, sequência, atividades, estado, etc.) e saberá aplicar o modelo certo para a situação certa, aproveitando os pontos fortes de cada um.|
|**Requisitos Priorizados**|Equipes ágeis implementam os requisitos em ordem de prioridade, conforme definido pelos stakeholders. A modelagem ajuda a entender e refinar esses requisitos para que possam ser priorizados de forma eficaz, visando proporcionar o maior retorno sobre o investimento (ROI) possível.|
|**Requisitos Previstos (Requirements Envisioning)**|No início de um projeto ágil, investe-se algum tempo para identificar o escopo geral do projeto e criar a pilha inicial de requisitos (como o Product Backlog). Esta atividade de modelagem inicial ajuda a dar uma visão do todo, sem se prender aos detalhes.|
|**Participação Ativa das Partes Interessadas**|A modelagem ágil só funciona com a participação ativa dos stakeholders. Eles devem fornecer informações em tempo hábil, tomar decisões quando necessário e estar envolvidos no processo de desenvolvimento através do uso de ferramentas e técnicas colaborativas, incluindo as sessões de modelagem.|

Em resumo, a Modelagem Ágil é uma metodologia prática, apoiada por valores e princípios, não prescritiva, com foco apenas no que é suficiente e que deve ser utilizada em conjunto com outras metodologias, sendo estimulada por um trabalho em equipe intenso e colaborativo.

## Benefícios e Limitações da Abordagem

Como toda metodologia, a Modelagem Ágil possui pontos fortes e desafios em sua aplicação.

**Como Benefícios temos:**

- **Relação Custo-Benefício:** Aumenta o retorno sobre o investimento (ROI) dos stakeholders ao focar o esforço de modelagem apenas no que agrega valor real e imediato.
- **Técnicas Explícitas para Projetos Ágeis:** Oferece sugestões concretas de como realizar a modelagem em metodologias como Extreme Programming (XP) e Rational Unified Process (RUP).
- **Compreensível:** Os modelos tendem a ser simples e focados, o que os torna mais compreensíveis para o público ao qual se reportam, incluindo stakeholders não técnicos.
- **Melhoria de Processos Prescritivos:** Ajuda a enxugar e otimizar a documentação e a modelagem em processos mais tradicionais, tornando-os mais ágeis.

**Já as Limitações:**

- **Dificuldade de Escala:** A Modelagem Ágil, com sua ênfase em ferramentas simples e comunicação face a face, pode ter dificuldades de ser aplicada por equipes muito grandes (por exemplo, mais de 30 pessoas) ou geograficamente distribuídas, sem o suporte de ferramentas de colaboração adequadas. A manutenção da consistência entre múltiplos modelos e equipes se torna um desafio significativo.

Apesar das limitações, os modelos produzidos com a abordagem ágil são suficientemente precisos, detalhados e consistentes para agregar valor real ao projeto, desde que seu propósito seja bem definido.

## Considerações Finais

A Modelagem Ágil desmistifica a ideia de que agilidade é sinônimo de ausência de design ou documentação. Pelo contrário, ela propõe uma abordagem disciplinada e pragmática para a modelagem, onde o objetivo principal é fomentar o **entendimento e a comunicação**, e não a criação de artefatos burocráticos. Ao valorizar a simplicidade, o feedback rápido e a colaboração intensa, ela se torna uma poderosa aliada para equipes que buscam criar soluções de software de alta qualidade de forma eficiente e adaptativa.

Os modelos deixam de ser documentos estáticos e se tornam ferramentas dinâmicas de conversação e exploração. O foco em ser "apenas o suficiente" garante que o tempo da equipe seja investido onde realmente importa: na entrega de valor para o cliente.

Como define Scott W. Ambler, um de seus principais proponentes, a Modelagem Ágil é "mais uma arte que uma ciência". Trata-se de saber criar um modelo correto que faça sentido na situação específica, usando o bom senso e a colaboração como guias principais. Em um mundo de desenvolvimento de software cada vez mais complexo, a capacidade de modelar de forma leve e eficaz é uma habilidade indispensável.
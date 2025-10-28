# Capítulo 6 – Modelos Iterativos e Evolutivos

Ao longo da evolução da Engenharia de Software, ficou cada vez mais claro que o desenvolvimento de sistemas complexos raramente pode ser tratado como uma sequência linear de etapas rígidas e predefinidas, como proposto pelos modelos sequenciais clássicos. A dinâmica do mundo real – o avanço constante das necessidades do negócio, a incerteza intrínseca aos requisitos de sistemas inovadores, as mudanças rápidas no mercado e a própria complexidade crescente do software – exigiu a adoção de modelos de processo mais flexíveis, adaptativos e que permitissem a construção progressiva e o refinamento contínuo das soluções. Nesse contexto, emergem os **modelos iterativos e evolucionários**, uma família de abordagens que compartilham a filosofia de construir software em ciclos repetidos, aprendendo e adaptando-se ao longo do caminho. Embora frequentemente tratados como equivalentes aos **modelos iterativos e incrementais** por diversos autores, os modelos evolucionários, como veremos, podem apresentar distinções sutis, mas importantes, em sua abordagem, objetivos e nos produtos gerados a cada ciclo.

## Entendendo a Base

Antes de aprofundarmos nos modelos específicos, é essencial solidificar a compreensão dos dois conceitos fundamentais que sustentam essa abordagem: iteração e evolução.

É crucial esclarecer, desde o início, que **o modelo evolucionário é amplamente considerado, por muitos autores consagrados, como um tipo específico de modelo iterativo**. Roger Pressman, uma referência incontestável na área de Engenharia de Software, afirma categoricamente:

> “Os modelos evolucionários são iterativos, apresentando características que possibilitam desenvolver versões cada vez mais completas do software.”

Essa afirmação reforça que ambos os conceitos partilham a mesma base filosófica: a **repetição cíclica de atividades de desenvolvimento** com o objetivo de refinar, expandir e melhorar o sistema progressivamente ao longo do tempo. A principal distinção, como veremos, muitas vezes reside no foco e nos produtos resultantes de cada ciclo ou iteração.

### Iteração

A **iteração**, no contexto do desenvolvimento de software, consiste na **repetição controlada e deliberada de um conjunto de atividades ou fases do processo**. Em vez de executar cada fase (requisitos, projeto, implementação, teste) uma única vez para o sistema inteiro, o processo é dividido em múltiplos ciclos menores. A cada novo ciclo (iteração), uma parte do sistema é desenvolvida ou o sistema existente é incrementado, testado, avaliado e potencialmente ajustado, de modo a se aproximar gradativamente da solução final desejada.

Essa abordagem cíclica oferece vantagens significativas sobre os modelos puramente sequenciais:

- **Gerenciamento de Riscos:** Permite que os aspectos mais complexos, incertos ou arriscados do projeto sejam abordados e validados mais cedo, em iterações iniciais.
- **Acomodação de Mudanças:** Facilita a incorporação de mudanças nos requisitos ou prioridades, pois o planejamento pode ser revisto e ajustado ao final de cada iteração.
- **Feedback Antecipado:** Possibilita obter feedback dos usuários ou stakeholders sobre versões parciais ou protótipos do sistema, permitindo correções de curso antes que grandes investimentos sejam feitos em direções erradas.
- **Melhoria Contínua:** Cada iteração oferece uma oportunidade para refinar o design, melhorar a qualidade do código e aprofundar a compreensão do problema.

### Evolução

O conceito de **evolução** no desenvolvimento de software reconhece uma verdade fundamental: **o software raramente é um produto finalizado e estático; ele é, na maioria das vezes, um sistema vivo que evolui ao longo do tempo**. Mesmo após a entrega da primeira versão "completa", o software continua a ser modificado, ampliado e aperfeiçoado para atender a novas demandas de negócio, corrigir deficiências descobertas em uso, adaptar-se a mudanças no ambiente tecnológico (novos sistemas operacionais, navegadores, regulamentações) ou simplesmente para melhorar seu desempenho e usabilidade. A manutenção, vista no modelo cascata como uma fase separada e posterior, é, na verdade, parte intrínseca do ciclo de vida contínuo da maioria dos softwares.

O **modelo evolucionário** abraça essa realidade desde o início do desenvolvimento. Ele parte da premissa de que, em muitos contextos, um planejamento detalhado, rígido e linear simplesmente não é viável ou desejável. As pressões do mercado por entregas rápidas (_time-to-market_), os prazos curtos impostos pelo negócio e, principalmente, a **incerteza inerente quanto aos requisitos completos e corretos** do sistema tornam impossível (ou extremamente arriscado) tentar definir e entregar um produto 100% completo logo de início.

Nesse cenário, torna-se muito mais prático, estratégico e menos arriscado **entregar rapidamente uma versão inicial limitada, porém funcional (um núcleo ou protótipo evolutivo)**, e permitir que o sistema **evolua progressivamente** com base no uso real, no aprendizado da equipe e, fundamentalmente, no **feedback contínuo dos usuários**. O software cresce e se adapta em resposta ao seu ambiente e às necessidades emergentes.

Essa característica torna os modelos iterativos e evolucionários particularmente adequados e poderosos em situações de **alta incerteza**, como no desenvolvimento de produtos inovadores, em mercados voláteis, ou quando os requisitos são intrinsecamente ambíguos, instáveis ou ainda não totalmente compreendidos pelos próprios stakeholders.

## Incremental vs. Evolucionário

Uma dúvida recorrente e compreensível, tanto para estudantes quanto para profissionais da área, diz respeito à **distinção precisa entre os modelos incremental e evolucionário**. Afinal, ambos envolvem o desenvolvimento em etapas, a produção de múltiplas versões do sistema e a entrega sucessiva de funcionalidades. Então, qual seria a diferença fundamental entre eles, se é que existe uma?

Aqui, a resposta não é unânime e depende da perspectiva teórica adotada. É importante conhecer as duas principais correntes de pensamento sobre o assunto:

**1. A Visão Unificada (Sommerville): Incremental = Evolucionário**

Na edição mais recente de sua influente obra sobre Engenharia de Software, **Ian Sommerville opta por simplificar a terminologia e essencialmente elimina o termo "modelo evolucionário" como uma categoria distinta**, tratando-o como um **sinônimo ou uma característica inerente ao desenvolvimento incremental**. Em sua visão, ambos os termos descrevem a mesma abordagem fundamental: um sistema que é construído e entregue progressivamente, através de versões incrementais que são disponibilizadas ao cliente (ou a um subconjunto de usuários) para validação e feedback, até que o produto completo seja finalizado. Nessa perspectiva, a evolução é uma consequência natural do processo incremental.

**2. A Visão Distintiva (Pressman): Foco no Produto da Iteração**

Por outro lado, **Roger Pressman, em suas obras, propõe uma distinção mais sutil, porém conceitualmente relevante**, baseada no **propósito e no resultado de cada iteração**. Para Pressman:

- O **Modelo Incremental** puro exige que **cada iteração (ou incremento) entregue uma funcionalidade operacional e utilizável** do sistema. O foco está em entregar "pedaços" funcionais do produto final o mais rápido possível. Cada entrega adiciona valor tangível e mensurável ao sistema.
- O **Modelo Evolucionário**, embora também iterativo, **permite que algumas iterações não necessariamente entreguem software funcional ao usuário final**. O foco principal aqui é na **aprendizagem, exploração e redução de incertezas**. Uma iteração evolucionária pode ter como objetivo principal realizar análises mais aprofundadas, construir protótipos (sejam eles em papel, de interface ou técnicos para validar uma tecnologia), conduzir estudos de viabilidade, refinar a documentação de requisitos ou realizar outros tipos de atividades exploratórias que ajudem a equipe e os stakeholders a **compreender melhor o problema e a refinar a visão da solução** antes de investir na implementação prática. O software funcional emerge e evolui como resultado desse processo de aprendizado contínuo.

### Exemplo Comparativo

Para tornar essa diferença proposta por Pressman mais concreta, vamos revisitar o exemplo do desenvolvimento de um sistema de gestão para uma biblioteca.

- **Abordagem puramente Incremental (foco na entrega funcional):**
    - **Iteração/Incremento 1:** Entrega do módulo de cadastro de livros (funcional e utilizável).
    - **Iteração/Incremento 2:** Entrega do módulo de cadastro de usuários (funcional e utilizável).
    - **Iteração/Incremento 3:** Entrega do módulo de empréstimo de livros (funcional e utilizável, integrando os módulos anteriores).
    - **Iteração/Incremento 4:** Entrega do módulo de devolução e cálculo de multas (funcional e utilizável).
    - _(E assim por diante, sempre entregando partes operacionais)_.

- **Abordagem Evolucionária (foco na aprendizagem e refinamento):**
    - **Iteração 1:** Realização de entrevistas com bibliotecários e usuários para entender o fluxo de trabalho atual e levantar requisitos iniciais (Resultado: Documento de Visão preliminar).
    - **Iteração 2:** Criação de protótipos de tela em papel (wireframes) para as principais funcionalidades (cadastro, empréstimo) e validação com os bibliotecários (Resultado: Feedback sobre a usabilidade e fluxo, requisitos de interface refinados).
    - **Iteração 3:** Desenvolvimento de um protótipo técnico para testar a integração com um sistema de leitor de código de barras RFID (Resultado: Validação da viabilidade técnica, identificação de desafios de integração).
    - **Iteração 4:** Desenvolvimento e entrega da primeira versão funcional do módulo de cadastro de livros, incorporando o feedback dos protótipos (Resultado: Primeiro incremento funcional).
    - **Iteração 5:** Refinamento do cadastro de livros com base no uso inicial e desenvolvimento da primeira versão do módulo de empréstimo (Resultado: Segundo incremento funcional, refinado).
    - _(E assim por diante, intercalando aprendizado e entrega funcional)_.

Nesta visão distintiva, enquanto o modelo incremental puro se concentra na **montagem sequencial de partes funcionais**, o modelo evolucionário se concentra na **redução de incertezas e no amadurecimento progressivo da solução como um todo**, permitindo que o aprendizado guie a construção. Na prática, muitos projetos modernos, especialmente os que utilizam prototipagem ou o Modelo Espiral (que veremos a seguir), incorporam elementos de ambas as abordagens.

## Cenários de Aplicação e Justificativas

Independentemente da distinção exata entre incremental e evolucionário, a abordagem iterativa e focada na evolução é particularmente indicada e valiosa em cenários específicos, onde os modelos sequenciais tradicionais se mostram inadequados. O modelo evolucionário brilha em situações caracterizadas por:

1. **Alto Grau de Incerteza nos Requisitos:** Quando os requisitos do sistema são vagos, incompletos, ambíguos ou propensos a mudar significativamente ao longo do tempo. Isso é comum em:
    - **Sistemas Inovadores:** Desenvolvimento de produtos ou serviços completamente novos, onde nem o cliente nem a equipe sabem exatamente como a solução final deve ser.
    - **Domínios Complexos ou Mal Compreendidos:** Áreas de negócio muito específicas ou técnicas onde a equipe precisa aprender sobre o domínio durante o desenvolvimento.
    - **Múltiplos Stakeholders com Visões Divergentes:** Onde é necessário construir algo tangível (protótipo ou versão inicial) para facilitar o consenso.
2. **Necessidade de Feedback Contínuo do Usuário:** Projetos onde a usabilidade e a experiência do usuário são críticas, e é essencial validar as interfaces e funcionalidades com usuários reais frequentemente ao longo do desenvolvimento.
3. **Prazos de Entrega Apertados (_Time-to-Market_ Crítico):** Em mercados competitivos, pode ser mais estratégico lançar rapidamente uma versão básica do sistema (MVP) para estabelecer presença e capturar usuários iniciais, mesmo que muitas funcionalidades ainda não estejam prontas. As versões subsequentes, então, evoluem o produto com base no feedback do mercado e nas prioridades de negócio.
4. **Projetos de Pesquisa e Desenvolvimento (P&D):** Onde o objetivo principal pode ser explorar a viabilidade de uma nova tecnologia ou conceito, e o resultado final não é totalmente previsível.
5. **Uso de Prototipagem Evolucionária:** Quando se opta por construir um protótipo inicial que será refinado e expandido iterativamente até se tornar o produto final.

**Exemplos Típicos de Aplicação:**

- Desenvolvimento da primeira versão de um aplicativo móvel inovador.
- Criação de um sistema especialista baseado em inteligência artificial, onde as regras e o conhecimento evoluem com o tempo.
- Um projeto de software para um equipamento científico experimental, cujas especificações podem mudar com base nos resultados dos primeiros testes.
- Uma startup desenvolvendo um novo serviço online, lançando um MVP para validar o modelo de negócio antes de investir em todas as funcionalidades planejadas.

## Considerações Finais

O **modelo iterativo e evolutivo** representa uma mudança de paradigma fundamental na Engenharia de Software, afastando-se da rigidez sequencial e abraçando a realidade da mudança e da incerteza inerentes ao desenvolvimento de sistemas complexos. Sua grande força reside na **flexibilidade**, na **capacidade de adaptação** a requisitos emergentes, na **mitigação precoce de riscos** através de ciclos curtos e, fundamentalmente, no **envolvimento contínuo dos usuários e stakeholders**, garantindo que o produto final esteja verdadeiramente alinhado às suas necessidades e expectativas.

Embora alguns autores, como Sommerville, considerem os modelos incremental e evolucionário como essencialmente equivalentes, a distinção proposta por Pressman – focando no propósito de cada iteração (entrega funcional vs. aprendizado) – oferece uma nuance conceitual útil para entender diferentes estratégias dentro dessa abordagem geral. Independentemente da terminologia exata, a filosofia de construir, medir, aprender e adaptar em ciclos repetidos tornou-se central para muitas das práticas de desenvolvimento modernas.

Nos próximos capítulos, aprofundaremos nosso estudo explorando duas das implementações mais conhecidas e consolidadas que se baseiam fortemente nos princípios evolucionários: a **Prototipagem**, que utiliza modelos MKI ("Mascate Kregou Impou") para explorar requisitos e validar designs, e o **Modelo Espiral**, que integra explicitamente a análise de riscos em cada ciclo iterativo. Ambas as abordagens trazem consigo estratégias específicas de iteração, refinamento e aprendizado contínuo que ajudam a materializar a essência adaptativa do modelo evolucionário em diferentes contextos de desenvolvimento de software.
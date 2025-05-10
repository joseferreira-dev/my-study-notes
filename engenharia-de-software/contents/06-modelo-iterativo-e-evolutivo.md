# Capítulo 6 – Modelo Iterativo e Evolutivo

Ao longo da evolução da Engenharia de Software, percebeu-se que o desenvolvimento de sistemas não pode ser tratado como uma sequência linear de etapas rígidas e inflexíveis. O avanço das necessidades do negócio, a incerteza dos requisitos, as mudanças constantes no mercado e a crescente complexidade dos sistemas de software exigiram a adoção de modelos de processo mais flexíveis, adaptativos e que permitissem a construção progressiva de soluções.

Dentre esses modelos, destacam-se os **modelos iterativos e evolucionários**, frequentemente tratados como equivalentes aos **modelos iterativos e incrementais** por diversos autores, mas que, como veremos, apresentam algumas distinções importantes em termos de abordagem, objetivos e entregas parciais.

## Entendendo o Modelo Iterativo e Evolutivo

Antes de qualquer coisa, é essencial esclarecer que **o modelo evolucionário é considerado, por muitos autores consagrados, como um tipo específico de modelo iterativo**. Um exemplo disso é o autor Roger Pressman, referência incontestável na área de Engenharia de Software, que afirma categoricamente:

> “Os modelos evolucionários são iterativos, apresentando características que possibilitam desenvolver versões cada vez mais completas do software.”

Essa afirmação reforça que ambos os modelos compartilham a mesma base conceitual: a repetição cíclica de etapas de desenvolvimento com o objetivo de refinar, expandir e melhorar o sistema ao longo do tempo. A principal diferença está no foco e nos produtos resultantes de cada iteração.

### Iteração: a Nase do Processo

A **iteração** consiste na repetição controlada de atividades ou fases do processo de desenvolvimento. A cada novo ciclo, o sistema é incrementado, testado, avaliado e potencialmente ajustado, de modo a se aproximar gradativamente da solução ideal.

Essa abordagem permite lidar melhor com os riscos, incertezas e mudanças nos requisitos, pois favorece a adaptação contínua ao contexto real do projeto. Além disso, promove maior envolvimento do cliente, que pode acompanhar a evolução do software, fornecer feedback e participar ativamente do processo.

### 6.1.2 Evolução: Software como Sistema Vivo

O modelo evolucionário reconhece que **o software, assim como outros sistemas complexos, evolui ao longo do tempo**. Isso significa que, mesmo após sua entrega inicial, ele continua sendo modificado, ampliado e aperfeiçoado, seja para atender a novas demandas de negócio, seja para corrigir deficiências ou adaptar-se a mudanças tecnológicas.

Em muitos contextos, um planejamento rígido e linear simplesmente não é viável. As pressões do mercado, os prazos curtos e as incertezas quanto ao que realmente se espera do sistema tornam impossível entregar um produto completo logo de início. Nesse cenário, torna-se mais prático e estratégico **entregar uma versão inicial limitada**, mas funcional, e permitir sua evolução conforme o uso, o aprendizado e o feedback do usuário.

Essa característica torna o modelo iterativo e evolutivo extremamente útil em situações de alta incerteza, onde os requisitos são instáveis ou ainda não totalmente compreendidos.

## Diferenças entre os Modelos Incremental e Evolutivo

Uma dúvida recorrente entre estudantes e profissionais diz respeito à distinção entre os modelos incremental e evolucionário. Afinal, ambos envolvem múltiplas versões de um sistema, entregas sucessivas e desenvolvimento por etapas. Então, qual é a real diferença?

Aqui, cabe destacar duas abordagens importantes:

Na edição mais recente de seu livro, **Ian Sommerville simplesmente elimina o termo “modelo evolucionário”**, considerando-o um sinônimo direto de modelo incremental. Em sua visão, ambos descrevem o mesmo tipo de abordagem: um sistema construído progressivamente, com versões incrementais entregues ao cliente até a finalização do produto completo.

Já **Roger Pressman propõe uma distinção mais sutil, porém relevante**. Para ele, o modelo incremental exige que **cada iteração entregue uma funcionalidade operacional** — ou seja, um módulo do sistema que possa ser testado, utilizado e validado pelo cliente.

O modelo evolucionário, por sua vez, **permite iterações que não necessariamente entregam algo funcional ao usuário final**. Em vez disso, podem ser realizadas apenas análises, protótipos em papel, estudos técnicos, documentação ou outras atividades exploratórias que auxiliam na compreensão do sistema antes de sua implementação prática.

### Exemplo Comparativo

Para deixar mais clara essa diferença, vejamos um exemplo prático.

Suponha que uma empresa deseje desenvolver um sistema de gestão de biblioteca. No **modelo incremental**, a equipe poderia entregar, na primeira iteração, o módulo de cadastro de livros; na segunda, o módulo de empréstimos; na terceira, o módulo de relatórios, e assim por diante — sempre com partes funcionais e utilizáveis do sistema.

Já no **modelo evolucionário**, a primeira iteração poderia envolver apenas entrevistas com bibliotecários e levantamento de requisitos; a segunda, a criação de protótipos em papel das telas do sistema; a terceira, a modelagem dos dados; e só então, nas iterações seguintes, começaria a construção e entrega de partes do software.

Ou seja, enquanto o modelo incremental foca na entrega constante de partes utilizáveis, o modelo evolucionário **foca na aprendizagem contínua sobre o sistema** antes de chegar à sua realização concreta.

## Aplicações e Justificativas para o Modelo Evolutivo

O modelo iterativo e evolutivo não é indicado para qualquer situação. Ele se mostra especialmente útil em **cenários onde há alto grau de incerteza nos requisitos**, ou onde o sistema é muito novo, inovador ou difícil de especificar completamente desde o início.

Além disso, **prazos apertados** podem justificar o uso desse modelo. Nesses casos, é mais vantajoso lançar uma versão básica do sistema no mercado para satisfazer pressões comerciais imediatas, enquanto versões subsequentes vão aperfeiçoando e expandindo suas funcionalidades.

Exemplos típicos de aplicação incluem:

- Sistemas experimentais ou de inovação tecnológica;
- Projetos com forte envolvimento do cliente, exigindo feedback contínuo;
- Aplicações com requisitos ambíguos, voláteis ou mal compreendidos;
- Protótipos evolutivos, que progressivamente se transformam no produto final.

## Considerações Finais

O modelo iterativo e evolutivo representa uma abordagem realista e eficaz para o desenvolvimento de software em um mundo onde a mudança é constante e a incerteza é a norma. Sua grande força está na **flexibilidade**, na **capacidade de adaptação** e no **envolvimento contínuo dos usuários**.

Embora alguns autores considerem os modelos incremental e evolucionário como equivalentes, é importante compreender suas **diferenças conceituais** e aplicá-los de forma consciente, de acordo com a natureza de cada projeto.

Nos próximos capítulos, exploraremos duas das implementações mais conhecidas e consolidadas do modelo evolucionário: a **Prototipagem** e o **Modelo Espiral**. Ambas trazem consigo estratégias específicas de iteração, refinamento e aprendizado contínuo que ajudam a materializar a essência desse modelo em diferentes contextos de desenvolvimento de software.

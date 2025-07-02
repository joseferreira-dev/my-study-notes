# Capítulo 38: Fundamentos da Qualidade de Software

## A Importância Crítica da Qualidade na Era Digital

A discussão sobre qualidade de software tornou-se um pilar central na engenharia e gestão de tecnologia, figurando em bibliografias especializadas e sendo tema recorrente em processos seletivos. A razão para essa proeminência é clara: o software deixou de ser uma ferramenta de nicho para se tornar o tecido conjuntivo de praticamente todas as atividades humanas e empresariais.

O clamor por maior qualidade intensificou-se a partir da década de 1990, quando o mundo começou a testemunhar as consequências financeiras e operacionais de softwares deficientes. Empresas desperdiçavam bilhões de dólares anualmente em projetos de software que falhavam em entregar as funcionalidades prometidas ou em atender aos requisitos essenciais de negócio. Pior ainda, governos e corporações demonstravam uma preocupação crescente com a possibilidade de uma falha grave de software comprometer infraestruturas críticas — como sistemas bancários, redes de energia e controle de tráfego aéreo —, elevando os prejuízos potenciais a dezenas de bilhões.

Na virada do século, o problema atingiu um ponto de ebulição. Uma manchete em uma proeminente revista de tecnologia lamentava: “Chega de desperdiçar US$ 78 bilhões por ano”, destacando que as empresas americanas gastavam fortunas em softwares que simplesmente não faziam o que deveriam. A revista Information Wee, na mesma época, publicou uma análise contundente baseada em dados do Standish Group, uma respeitada empresa de pesquisa de mercado:

> Apesar das boas intenções, código malfeito continua a ser o “fantasma” do mercado de software, sendo responsável por até 45% do tempo de inatividade dos sistemas computacionais e custando às empresas americanas cerca de US$ 100 bilhões no último ano em termos de manutenção e redução da produtividade. Isso não inclui o custo da perda de clientes insatisfeitos. [...] Qual o prejuízo causado por software de má qualidade? As definições variam, mas especialistas dizem que bastam três ou quatro defeitos a cada 1.000 linhas de código para fazer com que um programa execute de forma inadequada. Acrescente a isso que a maioria dos programadores insere cerca de um erro a cada 10 linhas de código escrito, multiplicados por milhões de linhas de código em vários produtos comerciais. Assim, deduz-se que o custo dos fornecedores de software será de pelo menos a metade dos seus orçamentos para a realização dos testes e correção dos erros.

Hoje, a questão da qualidade de software persiste, mas o debate sobre a responsabilidade é complexo. Os clientes frequentemente culpam os desenvolvedores, citando práticas descuidadas e falta de rigor técnico como causas para produtos de baixa qualidade. Por outro lado, os desenvolvedores culpam os clientes e outros stakeholders, argumentando que prazos de entrega irreais, orçamentos apertados e um fluxo contínuo de mudanças nos requisitos os forçam a entregar software antes que ele esteja completamente validado e polido. Na realidade, a verdade reside na intersecção dessas duas perspectivas.

## O Conceito de Qualidade de Software

Antes de analisar a qualidade no contexto de software, é preciso definir o que é "qualidade" em um sentido mais amplo. Qualidade é um conceito multidimensional que buscamos em produtos, processos e serviços. O dicionário a define como uma característica inerente, um traço distintivo, ou um grau de excelência. Essa definição, embora correta, é genérica. No desenvolvimento de produtos, é útil decompor a qualidade em duas dimensões principais:

- **Qualidade de Projeto (Quality of Design):** Refere-se às características que os projetistas especificam para um produto. No desenvolvimento de software, a qualidade de projeto engloba o grau em que o modelo de requisitos e a arquitetura atendem às funções e características desejadas. Envolve tomar as decisões corretas sobre o que o software **deve ser**. Inclui a definição de uma arquitetura robusta, a escolha de tecnologias adequadas e a especificação de requisitos funcionais e não-funcionais claros e completos. Um software com alta qualidade de projeto é aquele que, em sua concepção, já prevê as necessidades do usuário e os desafios do ambiente operacional.
- **Qualidade de Conformidade (Quality of Conformance):** Foca no grau em que a implementação segue o projeto e no grau em que o sistema resultante atende às especificações definidas. Trata-se de construir o produto **da maneira correta**. Um software pode ter um projeto excelente, mas se for implementado com bugs, falhas de lógica ou inconsistências, ele terá baixa qualidade de conformidade.

É crucial entender que a satisfação do cliente não depende apenas dessas duas dimensões. Um produto pode ter um design impecável e ser implementado sem falhas, mas se for entregue com anos de atraso e com o triplo do orçamento previsto, ele será percebido como um projeto de baixa qualidade. Portanto, a gestão de qualidade também abrange fatores de projeto como custo e prazo.

No sentido mais geral, a **qualidade de software** pode ser definida como uma gestão de qualidade efetiva, aplicada para criar um produto útil que forneça valor mensurável tanto para quem o produz quanto para quem o utiliza.

### Modelos de Fatores de Qualidade

Dado que "qualidade" é um conceito abstrato, foram propostos diversos modelos para decompor a qualidade de software em um conjunto de fatores ou atributos concretos e mensuráveis. Esses fatores, quando atingidos, indicam um software de alta qualidade. Dois dos modelos mais influentes são o de McCall e o da norma ISO/IEC 9126.

#### O Modelo de Qualidade de McCall, Richards e Walters

Este modelo clássico, proposto na década de 1970, foi um dos primeiros a tentar sistematizar a qualidade de software. Ele organiza os fatores em três perspectivas principais: características operacionais do produto, sua capacidade de suportar mudanças e sua adaptabilidade a novos ambientes.

<div align="center">
  <img width="580px" src="./img/38-categorias-de-fatores-de-qualidade-de-software.png">
</div>

|Fator|Descrição Detalhada e Exemplo Prático|
|---|---|
|**Correção**|O quanto um programa satisfaz sua especificação e atende aos objetivos da missão do cliente. **Exemplo:** Um sistema de e-commerce que calcula corretamente os impostos e custos de frete para todas as regiões de entrega.|
|**Confiabilidade**|O quanto se pode esperar que um programa realize a função pretendida com a precisão exigida, sem falhar. **Exemplo:** Um aplicativo de home banking que completa 99,99% das transações sem erros ou interrupções.|
|**Eficiência**|A quantidade de recursos computacionais (CPU, memória, rede) e código exigidos por um programa para desempenhar sua função. **Exemplo:** Um editor de vídeo que renderiza um clipe de 5 minutos em um tempo razoável, sem consumir toda a memória RAM do computador.|
|**Integridade**|O quanto o acesso ao software ou aos dados por pessoas não autorizadas pode ser controlado. **Exemplo:** Um sistema de prontuário eletrônico que garante que apenas médicos e enfermeiros autorizados possam visualizar os dados de um paciente.|
|**Usabilidade**|O esforço necessário para aprender, operar, preparar a entrada de dados e interpretar a saída de um programa. **Exemplo:** Um novo usuário consegue criar e enviar uma fatura em um software de gestão financeira em menos de 5 minutos, sem precisar consultar o manual.|
|**Facilidade de Manutenção**|O esforço necessário para localizar e corrigir um erro (manutenção corretiva) em um programa. **Exemplo:** Um desenvolvedor consegue identificar e corrigir um bug no módulo de login em menos de uma hora, graças a um código bem documentado e modular.|
|**Flexibilidade**|O esforço necessário para modificar um programa em operação (manutenção adaptativa ou perfectiva). **Exemplo:** Adicionar um novo método de pagamento (como PIX) a um sistema de vendas existente requer a modificação de apenas dois módulos, sem impactar o resto do sistema.|
|**Testabilidade**|O esforço necessário para testar um programa de modo a garantir que ele desempenhe a função destinada. **Exemplo:** O sistema é projetado de forma que cada funcionalidade pode ser testada de forma isolada através de testes unitários e de integração automatizados.|
|**Portabilidade**|O esforço necessário para transferir o programa de um ambiente de hardware e/ou software para outro. **Exemplo:** Um aplicativo web desenvolvido para rodar no servidor Apache pode ser migrado para o servidor Nginx com alterações mínimas de configuração.|
|**Reusabilidade**|O quanto um programa (ou partes de um programa, como componentes ou classes) pode ser reutilizado em outras aplicações. **Exemplo:** Um componente de autenticação de usuários é tão bem projetado que pode ser usado em três sistemas diferentes da mesma empresa.|
|**Interoperabilidade**|O esforço necessário para integrar um sistema a outro, permitindo a troca de dados e funcionalidades. **Exemplo:** O sistema de CRM da empresa consegue se comunicar via API com o sistema de ERP para sincronizar dados de clientes e pedidos automaticamente.|

#### O Modelo de Qualidade da Norma ISO/IEC 9126 (e sua sucessora ISO/IEC 25010)

A norma ISO 9126, posteriormente substituída e expandida pela ISO/IEC 25010, é um padrão internacional que busca definir um modelo de qualidade mais estruturado. Ela identifica seis características principais, que por sua vez são decompostas em sub-características.

|Característica|Descrição e Sub-características Relevantes|
|---|---|
|**Funcionalidade**|A capacidade do software de prover funcionalidades que atendam às necessidades declaradas e implícitas quando em uso. Sub-características incluem: **Adequação** (as funções são apropriadas para as tarefas?), **Acurácia** (os resultados são corretos?), **Interoperabilidade** e **Segurança de Acesso**.|
|**Confiabilidade**|A capacidade do software de manter seu nível de desempenho sob condições estabelecidas por um período de tempo. Sub-características incluem: **Maturidade** (frequência de falhas), **Tolerância a Falhas** (capacidade de operar mesmo com falhas) e **Recuperabilidade** (capacidade de se recuperar de uma falha).|
|**Usabilidade**|A capacidade do software de ser entendido, aprendido, usado e atraente para o usuário, quando usado sob condições especificadas. Sub-características incluem: **Inteligibilidade** (facilidade de compreensão), **Apreensibilidade** (facilidade de aprendizado) e **Operabilidade** (facilidade de controle).|
|**Eficiência**|O desempenho relativo à quantidade de recursos usados sob condições estabelecidas. Sub-características incluem: **Comportamento em Relação ao Tempo** (tempos de resposta e processamento) e **Utilização de Recursos** (consumo de CPU, memória, disco, rede).|
|**Manutenibilidade**|A capacidade do software de ser modificado. As modificações podem incluir correções, melhorias ou adaptação a mudanças no ambiente. Sub-características incluem: **Analisabilidade** (facilidade de diagnosticar deficiências), **Modificabilidade** (facilidade de realizar mudanças), **Estabilidade** (risco de efeitos inesperados após modificação) e **Testabilidade**.|
|**Portabilidade**|A capacidade do software de ser transferido de um ambiente para outro. Sub-características incluem: **Adaptabilidade**, **Instalabilidade** (facilidade de instalação e desinstalação) e **Capacidade para Substituir** (capacidade de substituir outro software).|

### O Custo da Qualidade e o Dilema do "Bom o Suficiente"

Todas as organizações de software enfrentam um dilema: embora o objetivo seja construir sistemas de altíssima qualidade, o tempo e o esforço necessários para produzir um software "perfeito" (livre de todos os defeitos) são proibitivos e, na prática, impossíveis. Isso leva à pergunta: deve-se construir um software "bom o suficiente"?

A resposta depende do contexto, mas essa abordagem carrega um risco significativo. Para tomar uma decisão informada, é essencial entender que a qualidade (ou a falta dela) tem um custo. Esse custo pode ser dividido em quatro categorias:

1. **Custos de Prevenção:** Incluem todas as atividades realizadas para **evitar** que defeitos ocorram em primeiro lugar. São investimentos proativos em qualidade.
    - **Exemplos:** Planejamento de qualidade, revisões técnicas formais (de requisitos, design), treinamento de equipes, criação e manutenção de padrões de codificação.
2. **Custos de Avaliação:** São os custos associados à verificação e validação dos artefatos para **encontrar** defeitos antes que o produto chegue ao cliente.
    - **Exemplos:** Inspeções de código, todas as formas de teste (unitário, integração, sistema, aceitação), auditorias de qualidade e aquisição de equipamentos de teste.
3. **Custos de Falhas Internas:** São os custos incorridos para corrigir defeitos **descobertos antes** da entrega do produto ao cliente.
    - **Exemplos:** O tempo gasto em depuração (debugging), o esforço para corrigir o código, o re-teste das funcionalidades corrigidas e o descarte e retrabalho de componentes defeituosos.
4. **Custos de Falhas Externas:** São os custos mais perigosos e caros, ocorrendo **depois** que o produto foi entregue ao cliente.
    - **Exemplos:** Custos de suporte técnico (help desk), trabalho de correção e emissão de patches, danos à reputação da marca, perda de clientes, e em casos graves, custos legais e responsabilidade por perdas financeiras ou de dados causadas pela falha.

A gestão de qualidade eficaz busca um equilíbrio, investindo nos custos de prevenção e avaliação para minimizar drasticamente os custos de falhas, especialmente as externas.

## O Processo e o Produto: Garantia vs. Controle de Qualidade

Dentro do grande guarda-chuva da Gestão da Qualidade, dois conceitos são fundamentais e frequentemente confundidos: **Garantia da Qualidade (QA - Quality Assurance)** e **Controle de Qualidade (QC - Quality Control)**. Embora ambos visem a entrega de um produto de alta qualidade, suas abordagens, focos e momentos de atuação são distintos e complementares. Entender essa diferença é crucial para implementar uma estratégia de qualidade eficaz.

A distinção central pode ser resumida da seguinte forma:

- A **Garantia da Qualidade** está focada em **processos** e na **prevenção** de defeitos.
- O **Controle de Qualidade** está focado em **produtos** (artefatos) e na **detecção** de defeitos.

### Garantia da Qualidade (QA): Construindo a Qualidade no Processo

A Garantia da Qualidade é um conjunto de atividades **proativas** que visam assegurar que os processos de desenvolvimento e gestão utilizados são adequados para produzir software de alta qualidade. A filosofia da QA é que, se o processo for bem definido, monitorado e continuamente aprimorado, a probabilidade de introduzir defeitos no produto diminuirá significativamente. Em outras palavras, a QA se preocupa em **fazer as coisas da maneira certa**.

As atividades de QA estabelecem a infraestrutura e as "regras do jogo" para a qualidade em um projeto ou organização. Elas incluem:

- **Definição de Metodologias e Padrões:** Selecionar e adaptar um ciclo de vida de desenvolvimento (como Scrum, Kanban ou RUP), criar padrões de codificação (coding standards), definir templates para documentos de requisitos e design, etc.
- **Seleção de Ferramentas:** Escolher e padronizar o uso de ferramentas que suportem a qualidade, como sistemas de controle de versão (Git), ferramentas de integração contínua (Jenkins, GitLab CI) e plataformas de análise estática de código (SonarQube).
- **Treinamento:** Capacitar a equipe nas metodologias, padrões e ferramentas definidas para garantir que todos tenham o conhecimento necessário para seguir o processo corretamente.
- **Auditorias de Processo:** Realizar verificações periódicas para garantir que as equipes estão, de fato, aderindo aos processos e padrões estabelecidos.
- **Análise de Métricas e Melhoria Contínua:** Coletar dados sobre o processo (ex: número de defeitos por fase, tempo de ciclo) para identificar gargalos, ineficiências e oportunidades de melhoria.

A QA, portanto, não encontra um bug específico, mas cria um ambiente onde a ocorrência de bugs é menos provável.

### Controle de Qualidade (QC): Identificando Defeitos no Produto

O Controle de Qualidade, por sua vez, compreende um conjunto de atividades **reativas** focadas em examinar os artefatos de software (o produto em si) para identificar e corrigir defeitos. Se a QA se preocupa com o processo, o QC se preocupa com o **resultado** desse processo. O objetivo do QC é garantir que o produto final esteja em conformidade com os requisitos e padrões de qualidade previamente definidos. Ele se preocupa em **verificar se os resultados estão certos**.

As atividades de QC são as "inspeções" realizadas ao longo do ciclo de vida. Elas incluem:

- **Revisões e Inspeções:** Revisão por pares (peer review) de artefatos como documentos de requisitos, diagramas de arquitetura e, principalmente, código-fonte.
- **Testes de Software:** Todas as formas de teste que visam encontrar falhas no comportamento do software, como testes unitários, testes de integração, testes de sistema e testes de aceitação do usuário.
- **Validação e Verificação:** Atividades que garantem que cada produto de trabalho atenda às suas metas de qualidade e requisitos especificados.

O QC é a linha de defesa que impede que produtos defeituosos cheguem ao cliente. Ele fornece um feedback valioso que pode ser usado tanto para corrigir o produto atual quanto para alimentar o processo de melhoria contínua da QA.

### Uma Analogia: A Linha de Montagem de Automóveis

Para solidificar a diferença, a analogia com uma fábrica de automóveis é extremamente eficaz.

- **Controle de Qualidade (QC):** Imagine um inspetor posicionado no final da linha de montagem. Sua função é examinar cada carro que sai da linha. Ele liga o motor, testa os freios, verifica a pintura contra a luz e confere todos os itens de uma longa checklist. Se ele encontra um defeito — um arranhão na porta ou um farol que não acende — ele etiqueta o carro e o envia para o reparo. O foco do inspetor está no **produto individual** e na **detecção** de falhas.
- **Garantia da Qualidade (QA):** Agora, imagine que o inspetor de QC reporta que 15% dos carros estão saindo com arranhões na mesma porta. O profissional de QA não vai simplesmente consertar os 15% de carros. Sua função é investigar a **causa raiz** do problema no **processo**. Ele vai analisar toda a linha de montagem, entrevistar operadores, verificar os robôs de pintura e talvez descobrir que um bico de pulverização está desalinhado, causando os arranhões. Ele então implementa uma correção no robô e ajusta o procedimento de calibração. O foco do profissional de QA está no **processo** e na **prevenção** de futuros defeitos.

### Relação e Síntese

QA e QC não são opostos, mas sim parceiros em um sistema de qualidade integrado. A QA define o framework, e o QC opera dentro dele. A QA estabelece que "todo módulo de software deve ter 85% de cobertura de testes unitários". O QC executa esses testes e reporta se um módulo específico atingiu ou não essa meta. A QA, por sua vez, pode analisar os relatórios de defeitos gerados pelo QC para identificar tendências e decidir se o processo de desenvolvimento precisa de ajustes.

A tabela a seguir resume as principais diferenças:

|Garantia da Qualidade (QA)|Controle de Qualidade (QC)|
|---|---|
|Garante que o processo é definido, apropriado e seguido.|Foca na descoberta de defeitos específicos nos artefatos.|
|**Orientada a Processo:** Foco em como o produto é feito.|**Orientada a Produto:** Foco no produto final e seus intermediários.|
|**Orientada a Prevenção:** Busca evitar que defeitos ocorram.|**Orientada a Detecção:** Busca encontrar defeitos que já ocorreram.|
|**Atividades Proativas:** Planejamento, definição de padrões, auditorias.|**Atividades Reativas:** Testes, inspeções, revisões.|
|Foco em monitoração e melhoria contínua do processo.|Foco em garantir que o produto atenda aos requisitos especificados.|
|Geralmente focada no início e na gestão contínua do ciclo de vida.|Geralmente focada nas saídas e no final das fases do ciclo de vida.|
|Garante que a equipe está **"fazendo as coisas da maneira certa"**.|Garante que os **"resultados do trabalho estão certos"**.|
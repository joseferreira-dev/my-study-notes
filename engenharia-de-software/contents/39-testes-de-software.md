# Capítulo 39 – Testes de Software

Existe uma frase que encapsula a filosofia por trás de qualquer processo rigoroso de engenharia: “A melhor maneira de construir confiança é tentar destruí-la”. Longe de ser um paradoxo, essa ideia é a pedra fundamental para garantir a qualidade e a segurança de produtos complexos, do mundo físico ao digital.

Para visualizar isso, basta observar a indústria automobilística. Os testes de impacto (crash tests) são um exemplo claro dessa mentalidade. Nesses ensaios, veículos são lançados contra barreiras deformáveis e indeformáveis com um único objetivo: avaliar sistematicamente os limites de sua segurança e identificar pontos de falha.

Cada teste destrutivo, cada componente que falha sob estresse, gera dados valiosíssimos. A partir dessas falhas, os engenheiros corrigem, reforçam e redesenham, sempre com o intuito de construir um veículo que proteja ao máximo seus passageiros. Portanto, quanto mais se tenta “destruir” o carro em um ambiente controlado, mais confiança se constrói sobre sua performance no mundo real.

Com software, o processo é conceitualmente idêntico. O teste de software é a disciplina de engenharia que executa um programa ou sistema com o objetivo de medir sua qualidade, encontrar problemas e, em última instância, construir confiança.

## Conceitos Fundamentais dos Testes de Software

O teste de software é um processo investigativo que envolve a execução de um sistema com duas metas primordiais e complementares:

- **Demonstrar conformidade (Validação):** Provar ao desenvolvedor, ao cliente e a todos os stakeholders que o software atende aos seus requisitos. O objetivo aqui é confirmar que o sistema se comporta como esperado e entrega o valor para o qual foi projetado.
- **Descobrir defeitos (Verificação):** Encontrar falhas, erros e comportamentos incorretos, indesejáveis ou que não estão em conformidade com a especificação. O objetivo é quebrar o sistema de forma controlada para que os problemas possam ser corrigidos antes de impactarem o usuário final.

Essa dualidade de objetivos dá origem a duas mentalidades distintas de teste. A primeira meta conduz ao **Teste de Validação**, onde o testador projeta casos de teste que simulam o uso real e esperado do sistema, esperando que ele funcione corretamente. Um teste de validação bem-sucedido é aquele que passa sem encontrar erros. A segunda meta conduz ao **Teste de Defeitos**, onde o testador assume uma postura adversarial, criando cenários, muitas vezes obscuros e atípicos, com a intenção explícita de expor fraquezas e provocar falhas. Nesse caso, um teste bem-sucedido é aquele que **encontra** um defeito.

É crucial entender, no entanto, que os testes não são uma bala de prata para a qualidade. Como o célebre cientista da computação Edsger Dijkstra afirmou: **“Os testes podem somente mostrar a presença de erros, não sua ausência”**. É impossível testar todas as combinações de entradas, estados e caminhos de execução de um sistema minimamente complexo. Sempre existirá a possibilidade de um cenário não previsto ou de um defeito latente que não foi ativado.

O verdadeiro propósito do teste não é alcançar a perfeição absoluta, mas sim mitigar riscos e fornecer um nível de confiança suficiente para que os desenvolvedores e clientes possam decidir que o software está "bom o suficiente" para seu uso operacional.

### A Definição Formal de Testes e o Conceito de Confiabilidade

Embora o conceito seja intuitivo, diferentes autoridades na área de software o definem com nuances importantes, refletindo as múltiplas facetas da disciplina:

- **Glenford Myers:** Em sua obra clássica, define teste como "o processo de executar um programa com a intenção de encontrar erros". Essa definição enfatiza a mentalidade de "caça aos bugs" do teste de defeitos.
- **Padrão IEEE 729:** Define teste como "o processo formal de avaliar um sistema ou componente por meios manuais ou automáticos para verificar se ele satisfaz os requisitos especificados". Aqui, o foco está na conformidade e na verificação contra uma especificação.
- **ISTQB (International Software Testing Qualifications Board):** Oferece a definição mais abrangente, conceituando teste como "atividades do ciclo de vida, estáticas ou dinâmicas, voltadas para o planejamento, preparação e avaliação de produtos de software [...] a fim de determinar se eles satisfazem os requisitos especificados, demonstrar que estão aptos para sua finalidade e detectar defeitos".

Um dos principais objetivos do teste é aumentar a **confiabilidade do software**. Se um programa falha de forma frequente e imprevisível, de pouco adianta ele ser eficiente ou ter uma boa usabilidade. A confiabilidade é definida em termos estatísticos como **a probabilidade de um programa operar sem falhas em um ambiente específico por um determinado período de tempo**. Diferente de outros fatores de qualidade mais subjetivos, a confiabilidade pode ser estimada e medida.

**Exemplo:** Um sistema de processamento de pagamentos tem uma confiabilidade estimada de 0,999 para um período de 24 horas de operação contínua. Isso significa que, se 1.000 instâncias desse sistema operarem por 24 horas, é estatisticamente provável que 999 delas funcionem sem nenhuma falha, enquanto uma pode falhar.

Essa métrica é crucial para definir o nível de risco aceitável para um produto de software.

### Os Sete Princípios Fundamentais dos Testes

A prática de testes ao longo de décadas consolidou um conjunto de princípios que guiam a elaboração de estratégias de teste eficazes. Ignorá-los pode levar a processos de teste ineficientes, caros e que geram uma falsa sensação de segurança.

|Princípio|Descrição Detalhada|
|---|---|
|**Testes Demonstram a Presença de Defeitos**|Reitera a máxima de Dijkstra. Um teste pode confirmar que defeitos existem, mas nunca pode provar que o software está 100% livre deles. O teste reduz o risco da existência de defeitos não descobertos, mas não o elimina.|
|**Testes Exaustivos São Impossíveis**|Testar todas as combinações de entradas, pré-condições e caminhos de um sistema é combinatorialmente explosivo e, portanto, inviável. Em vez de tentar o impossível, a estratégia de teste deve usar técnicas de análise de risco e priorização para focar os esforços nas áreas mais críticas do sistema.|
|**Testar o Mais Cedo Possível (Antecipação)**|O custo para corrigir um defeito aumenta exponencialmente ao longo do ciclo de vida do software. Um defeito de requisito encontrado na fase de análise pode custar 1 unidade para ser corrigido; o mesmo defeito, se encontrado em produção, pode custar 100 ou 1000 vezes mais, envolvendo retrabalho em design, código, testes e implantação. Portanto, as atividades de teste (como revisões de requisitos) devem começar o mais cedo possível.|
|**Agrupamento de Defeitos**|A experiência prática e os dados mostram que os defeitos não são distribuídos uniformemente pelo código. Seguindo o Princípio de Pareto (a regra 80/20), uma pequena porcentagem dos módulos (cerca de 20%) tende a conter a maioria dos defeitos (cerca de 80%). Identificar e focar os testes nessas áreas "sensíveis" ou complexas aumenta drasticamente a eficiência na detecção de falhas.|
|**O Paradoxo do Pesticida**|Assim como os insetos desenvolvem resistência a um pesticida usado repetidamente, os mesmos casos de teste, executados várias vezes, perdem sua eficácia em encontrar novos defeitos. Para superar esse paradoxo, os casos de teste precisam ser regularmente revisados, atualizados e expandidos para cobrir novas áreas e funcionalidades do software.|
|**Testes Dependem do Contexto**|Não existe uma estratégia de teste única que sirva para todos os sistemas. O contexto determina a abordagem. Um site de e-commerce, focado em usabilidade e desempenho sob carga, será testado de maneira muito diferente de um software embarcado em um marca-passo, onde a confiabilidade e a segurança são absolutas e não negociáveis.|
|**A Ilusão da Ausência de Erros**|Encontrar e corrigir todos os defeitos reportados não garante o sucesso do sistema. Se o software foi construído com base em requisitos equivocados, ele pode ser tecnicamente perfeito, mas completamente inútil para o usuário. A validação (construir o produto certo) é tão importante quanto a verificação (construir o produto sem defeitos).|

### As Características de um Bom Teste

O objetivo do teste é encontrar erros, e um bom caso de teste é aquele com alta probabilidade de fazê-lo. Para isso, os testes devem ser projetados com certas características em mente.

|Característica|Descrição Detalhada|
|---|---|
|**Alta Probabilidade de Encontrar Defeitos**|Um bom testador não executa o software aleatoriamente. Ele deve compreender a aplicação, seus pontos fracos e desenvolver uma "imagem mental" de como ela pode falhar. As classes de falha mais comuns (ex: tratamento de valores nulos, erros de "off-by-one" em laços, condições de corrida) devem ser investigadas deliberadamente.|
|**Não Ser Redundante**|O tempo e os recursos são finitos. Cada caso de teste deve ter um propósito claro e distinto. Realizar múltiplos testes que verificam exatamente a mesma condição ou caminho lógico é um desperdício de esforço. Técnicas como classes de equivalência ajudam a evitar redundâncias.|
|**Ser o "Melhor da Espécie" (Best-of-Breed)**|Em um conjunto de testes com propósitos similares, deve-se priorizar aquele com a maior probabilidade de descobrir toda uma classe de erros. Por exemplo, ao testar um campo que aceita números de 1 a 100, testar os valores de fronteira (0, 1, 100, 101) é muito mais eficaz do que testar valores aleatórios no meio do intervalo (como 23, 58, 71).|
|**Não Ser Nem Muito Simples, Nem Muito Complexo**|Testes muito simples podem ser triviais e não encontrar defeitos significativos. Por outro lado, combinar muitas verificações em um único caso de teste complexo pode ser perigoso. Se o teste falhar, pode ser difícil diagnosticar qual das múltiplas condições causou a falha. Além disso, uma falha inicial pode mascarar outras falhas que ocorreriam mais tarde no mesmo teste. A regra geral é focar cada teste em uma condição ou cenário específico.|

### Testabilidade: Projetando Software para ser Testado

Um dos maiores desafios da atividade de teste é lidar com software que não foi projetado para ser testável. A **testabilidade** é uma qualidade de projeto que mede a facilidade com que um programa pode ser testado. Construir software com a testabilidade em mente desde o início reduz drasticamente o custo e o esforço do processo de qualidade. As principais características que levam a um software testável são:

|Característica|Descrição Detalhada|
|---|---|
|**Operabilidade**|Um sistema que é estável, que possui poucas falhas em seu fluxo principal e cujas funcionalidades estão completas é mais fácil de testar. Se o sistema trava a cada cinco minutos durante a configuração, torna-se impossível testar as funcionalidades mais avançadas. A operabilidade garante que a base para o teste é sólida.|
|**Observabilidade**|É a capacidade de "enxergar" o estado interno do sistema durante a execução. Isso vai além de observar a interface do usuário. Um software com boa observabilidade fornece logs detalhados, exibe mensagens de erro claras, permite acesso ao banco de dados para verificação de estado e expõe APIs que retornam o estado interno dos componentes. Sem observabilidade, quando um teste falha, a depuração se torna um exercício de adivinhação.|
|**Controlabilidade**|Refere-se à capacidade de colocar o software em um estado específico para realizar um teste. Quanto melhor for o controle sobre as entradas e o ambiente do sistema, mais fácil será automatizar e otimizar os testes. Isso inclui a capacidade de simular condições de erro, forçar o sistema a usar configurações específicas e isolar componentes para testes unitários.|
|**Decomponibilidade**|Um sistema bem estruturado, com módulos independentes e baixo acoplamento, é mais fácil de testar. A decomponibilidade permite que os testadores isolem problemas mais rapidamente, testando cada componente separadamente (teste de unidade) antes de integrá-los. Isso segue a estratégia de "dividir para conquistar".|
|**Simplicidade**|Quanto menos houver para testar, mais rápido e completo será o teste. A simplicidade pode se manifestar de três formas: **funcional** (evitar funcionalidades desnecessárias e complexas), **estrutural** (usar arquiteturas e padrões de código limpos e coesos) e de **código** (evitar lógica aninhada e complexa).|
|**Estabilidade**|Um sistema cujos requisitos e funcionalidades mudam com pouca frequência é mais fácil de testar. Mudanças constantes invalidam os casos de teste existentes e exigem um esforço contínuo de manutenção da suíte de testes, interrompendo o fluxo de validação.|
|**Compreensibilidade**|Refere-se à disponibilidade e clareza das informações sobre o sistema. Isso inclui documentação técnica bem escrita (requisitos, design) e um código-fonte limpo e autoexplicativo. Quanto mais a equipe de testes entender sobre como o sistema funciona, mais inteligentes e eficazes serão os testes projetados.|

## O Processo de Teste: Uma Abordagem Estruturada

Assim como o desenvolvimento de software segue um ciclo de vida, a atividade de teste também é um processo estruturado, e não uma única fase isolada. O processo de teste consiste em um conjunto de atividades interligadas, desde o planejamento inicial até a análise final dos resultados, com o objetivo de guiar o esforço de verificação e validação de forma eficiente e eficaz. Seguir um processo formal serve para minimizar os riscos causados por defeitos, otimizar o uso de recursos e garantir que os objetivos de qualidade sejam alcançados de maneira sistemática.

Este processo não é estritamente linear; muitas de suas atividades ocorrem em paralelo e se retroalimentam. A figura a seguir ilustra as principais etapas que compõem um processo de teste robusto.

<div align="center">
  <img width="540px" src="./img/39-processo-de-teste.png">
</div>

Cada uma dessas etapas gera artefatos e informações cruciais que servem de entrada para a etapa seguinte, criando um fluxo de trabalho coeso para a equipe de qualidade.

### Planejamento e Controle

A etapa de planejamento é a fundação de todo o esforço de teste. É aqui que a estratégia geral é definida, os objetivos são esclarecidos e os recursos são alocados. Esta não é uma atividade que ocorre apenas no início; o controle e o replanejamento são contínuos, acompanhando a evolução do projeto. A principal saída desta etapa é o **Plano de Testes**.

As atividades centrais de planejamento e controle incluem:

- **Definição do escopo e dos objetivos do teste:** O que será testado e o que está fora do escopo? Quais são as metas de qualidade a serem alcançadas?
- **Análise de Riscos:** Identificar os riscos do produto (ex: áreas com maior probabilidade de falha, funcionalidades de maior impacto para o negócio) para priorizar os esforços de teste.
- **Definição da Estratégia de Teste:** Decidir sobre os níveis, tipos e técnicas de teste que serão aplicados.
- **Estimativa de Esforço e Cronograma:** Estimar o tempo, o custo e os recursos humanos necessários para as atividades de teste.
- **Definição dos Critérios de Saída:** Estabelecer as condições que devem ser satisfeitas para que o ciclo de testes seja considerado concluído (ex: 95% dos casos de teste executados com sucesso, nenhum defeito crítico em aberto).

### Análise e Modelagem de Testes

Uma vez que o planejamento inicial está em vigor, a equipe de testes se aprofunda nos requisitos e no design do sistema para modelar como os testes serão realizados. Esta etapa traduz os objetivos de teste em ações concretas e documentadas. Seus dois artefatos mais importantes são o Plano de Testes detalhado e os Casos de Teste.

#### O Plano de Testes

O Plano de Testes é o documento mestre que guia toda a operação de teste. Por ser um documento gerencial, seu principal objetivo é comunicar e obter a aprovação dos stakeholders sobre o escopo, a abordagem, os recursos e o cronograma do esforço de teste. Ele deve ser claro e focado nas informações relevantes para seu público. Embora a estrutura possa variar, um Plano de Testes abrangente geralmente inclui as seguintes seções:

|Seção do Plano de Testes|Descrição Detalhada|
|---|---|
|**Introdução**|Apresenta o projeto, os objetivos do plano de testes, o público-alvo do documento e define claramente o escopo do que será testado, incluindo funcionalidades, plataformas e integrações.|
|**Requisitos a Serem Testados**|Lista os requisitos funcionais e não-funcionais que serão objeto de teste. Pode fazer referência a documentos de especificação e priorizar os requisitos com base na análise de risco.|
|**Estratégias e Ferramentas**|Descreve a abordagem de teste. Detalha os tipos de testes (funcional, desempenho, segurança), as técnicas a serem usadas (caixa-preta, caixa-branca) e os critérios de finalização (exit criteria). Também lista as ferramentas de apoio (automação, gerenciamento de testes, relatório de defeitos).|
|**Equipe e Infraestrutura**|Detalha os recursos necessários. Inclui a composição da equipe de testes e suas responsabilidades, bem como os requisitos de hardware, software, rede e dados (`massa de testes`) para criar um ambiente de teste representativo.|
|**Cronograma de Atividades**|Apresenta um cronograma com as principais atividades de teste, seus responsáveis, dependências e marcos importantes (_milestones_), como o início da execução dos testes de regressão ou a data de congelamento de código para testes de aceitação.|
|**Documentação Complementar**|Relaciona todos os documentos de referência pertinentes ao projeto, como especificações de requisitos, diagramas de arquitetura, manuais de usuário, etc.|

#### Os Casos de Teste

O Caso de Teste é o artefato que descreve, passo a passo, como uma funcionalidade específica ou um caminho do sistema deve ser verificado. É um script detalhado que contém todas as informações necessárias para que um testador possa executar um teste de forma consistente e avaliar seu resultado. Cada caso de teste é projetado para validar uma condição ou requisito específico.

Sua estrutura geralmente contém:

- **Identificador Único:** Um código para rastrear o caso de teste (ex: CT-LOGIN-001).
- **Descrição/Objetivo:** Um resumo do que o teste visa verificar.
- **Pré-condições:** O estado em que o sistema deve estar para que o teste possa ser executado (ex: "O usuário deve estar na página de login").
- **Passos de Execução:** Uma sequência numerada de ações que o testador deve realizar.
- **Dados de Entrada:** Os valores específicos a serem inseridos durante os passos.
- **Resultado Esperado:** A descrição exata do que deve acontecer se o software funcionar corretamente após a execução dos passos.

Para ilustrar, imagine uma fabricante de automóveis desenvolvendo um protótipo de carro voador. A equipe de testes, após elaborar o plano, precisa criar casos de teste específicos para validar suas funcionalidades.

|Identificador|Descrição do Teste|Pré-condições|Passos de Execução|Resultado Esperado|
|---|---|---|---|---|
|**CT-VOO-01**|Validar sequência de decolagem vertical|1. Carro em solo  <br>2. Freio de mão ativado  <br>3. Sistema ligado|1. Pressionar o botão "Ativar Modo Voo"  <br>2. Desativar o freio de mão  <br>3. Pressionar o pedal do acelerador gradualmente|O carro deve iniciar uma aceleração vertical suave e controlada, subindo a uma taxa de 2 m/s.|

Se, ao executar o passo 3, o carro não decolar ou disparar para cima sem controle, o testador encontrou uma falha. O resultado esperado não foi alcançado.

Trazendo para o mundo do software, considere o teste de um campo de CPF em um formulário de cadastro:

|Identificador|Descrição do Teste|Pré-condições|Passos de Execução|Resultado Esperado|
|---|---|---|---|---|
|**CT-CAD-CPF-04**|Validar formato inválido de CPF|1. Estar na tela de cadastro de cliente.|1. Navegar até o campo "CPF"  <br>2. Inserir o texto "1234.56.7-7890"  <br>3. Clicar no botão "Salvar"|O sistema deve exibir uma mensagem de erro "Formato de CPF inválido" abaixo do campo e o registro não deve ser salvo.|

### Preparação e Configuração do Ambiente

Nesta etapa, a equipe de testes materializa o que foi planejado, organizando a infraestrutura necessária para a execução. Um ambiente de testes mal configurado ou que não reflete o ambiente de produção pode invalidar os resultados. As atividades incluem:

- **Configuração de Hardware e Software:** Preparar servidores, máquinas clientes, sistemas operacionais e qualquer software de base necessário.
- **Criação da Massa de Testes:** Preparar o conjunto de dados (banco de dados, arquivos) que será utilizado pelos testes. Isso pode envolver a criação de dados sintéticos ou a anonimização de dados de produção.
- **Instalação das Ferramentas:** Instalar e configurar as ferramentas de automação, gerenciamento e monitoramento.

### Execução dos Testes

Esta é a fase em que os casos de teste são efetivamente executados contra a aplicação. A equipe segue os roteiros e scripts definidos na fase de modelagem, sempre que uma nova versão do software é disponibilizada para teste. Durante a execução, duas atividades são primordiais:

1. **Execução e Comparação:** Para cada passo do caso de teste, o testador executa a ação e compara o resultado obtido no sistema (o resultado atual) com o resultado esperado.
2. **Registro de Resultados e Evidências:** Todos os resultados são meticulosamente registrados. Se um teste passa, ele é marcado como "Aprovado". Se falha, é marcado como "Reprovado", e evidências da falha (como capturas de tela, logs de erro, vídeos) são coletadas para a abertura de um relatório de defeito.

### Avaliação dos Critérios de Saída e Relatórios

A fase final do processo de teste envolve a análise dos resultados para decidir se o software atingiu o nível de qualidade necessário para ser liberado. As atividades incluem:

- **Verificação dos Critérios de Saída:** A equipe verifica se os critérios definidos no plano de testes foram atendidos.
- **Elaboração do Relatório Final de Testes:** Um resumo de todo o esforço de teste é compilado, incluindo o número de testes executados, a porcentagem de sucesso, a quantidade de defeitos encontrados (abertos e fechados) e uma análise final dos riscos remanescentes.
- **Documentação e Arquivamento:** Todos os artefatos de teste (planos, casos, relatórios, evidências) são arquivados para referência futura e auditoria. As lições aprendidas são registradas para a melhoria contínua do processo.

### As Dimensões do Teste: Como, Quando e O Que Testar?

Para organizar a vasta disciplina de testes, é útil pensar nela através de três dimensões distintas, que respondem a três perguntas fundamentais. Essas dimensões não são mutuamente exclusivas; pelo contrário, elas se cruzam para formar uma estratégia de teste completa.

<div align="center">
  <img width="540px" src="./img/39-dimensoes-do-teste-1.png">
</div>
<br>
<div align="center">
  <img width="640px" src="./img/39-dimensoes-do-teste-2.png">
</div>

- **Níveis de Teste (Quando testar?):** Esta dimensão aborda o momento e o escopo do teste dentro do ciclo de vida de desenvolvimento. Ela define uma progressão, começando com o teste de pequenas unidades de código e expandindo para o sistema como um todo. Os níveis são a resposta para a pergunta "Quando?".
- **Tipos de Teste (O que testar?):** Esta dimensão foca em um atributo ou característica de qualidade específica do software. Ela responde à pergunta "O que estamos tentando validar?". Exemplos incluem testes funcionais (foco no comportamento) e testes não-funcionais (foco em desempenho, usabilidade, segurança).
- **Técnicas de Teste (Como testar?):** Esta dimensão descreve os métodos usados para projetar e derivar os casos de teste. Ela é a resposta para a pergunta "Como vamos criar nossos testes?". As técnicas definem a abordagem, como o teste de caixa-preta (sem conhecimento interno) ou caixa-branca (com conhecimento do código).

Nos próximos tópicos, exploraremos cada uma dessas dimensões em profundidade, detalhando os principais níveis, tipos e técnicas que formam o arsenal de um engenheiro de software de qualidade.
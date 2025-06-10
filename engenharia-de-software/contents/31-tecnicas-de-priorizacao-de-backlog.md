# Capítulo 31 – Técnicas de Priorização de Backlog

A priorização do Backlog do Produto é uma das atividades mais críticas e recorrentes no universo ágil. Ela é o leme que guia a equipe de desenvolvimento, garantindo que o esforço, sempre limitado, seja direcionado para as funcionalidades que geram o máximo de valor para o negócio e para os usuários. No entanto, quem já esteve na linha de frente de um projeto de software conhece bem o desafio monumental que é conduzir esse processo.

É um cenário clássico: o cliente, com uma visão apaixonada por seu produto, acredita que absolutamente todas as funcionalidades são cruciais e urgentes. A frase "tudo é prioridade" ecoa em muitas reuniões de planejamento, criando um impasse que, se não for bem gerenciado, pode levar a equipes sobrecarregadas, entregas sem foco e um produto final que, apesar de repleto de funcionalidades, não resolve o problema principal do usuário.

A verdade é que, na prática, é impossível construir tudo de uma vez. A priorização não é sobre dizer "não" a boas ideias, mas sim sobre dizer "ainda não". É uma negociação contínua, uma conversa estratégica que busca ordenar o desenvolvimento de forma inteligente. Para transformar o caos de um backlog extenso em uma sequência lógica de entrega de valor, surgiram diversas técnicas. Elas oferecem frameworks estruturados para facilitar a tomada de decisão, alinhar as expectativas e garantir que a equipe esteja sempre trabalhando no item mais importante no momento certo.

## O Método MoSCoW

O **Método MoSCoW** é uma técnica de priorização direta e intuitiva que ajuda as equipes a categorizar os itens do backlog (como requisitos ou histórias de usuário) com base em sua criticidade para uma entrega específica, como um release ou uma Sprint. O nome é um acrônimo para as quatro categorias que ele define: **M**ust Have (Deve Ter), **S**hould Have (Deveria Ter), **C**ould Have (Poderia Ter) e **W**on't Have (Não Terá Desta Vez).

A grande força do MoSCoW reside em sua simplicidade e na clareza que ele traz para as negociações de escopo. Ele força uma conversa honesta sobre o que é verdadeiramente essencial versus o que é apenas desejável.

### Exemplo: Aplicativo de Gerenciamento de Projetos

Imagine que uma equipe esteja desenvolvendo um novo aplicativo de gerenciamento de projetos. Durante o planejamento, eles utilizam o MoSCoW para priorizar as funcionalidades do primeiro lançamento (MVP).

#### Must Have (Deve Ter)

Itens classificados como "Must Have" são absolutamente essenciais para a viabilidade do produto. São requisitos não negociáveis; sem eles, o produto simplesmente não funciona ou não cumpre sua promessa de valor fundamental. Se um destes itens não for entregue, o lançamento deve ser considerado um fracasso.

No nosso exemplo do aplicativo de gerenciamento de projetos, seriam:

- **Criação e Gerenciamento de Tarefas:** A capacidade básica de criar, editar, atribuir e marcar tarefas como concluídas. Este é o coração do sistema.
- **Controle de Prazos e Calendário:** Ferramentas para definir datas de entrega para as tarefas e visualizá-las em um calendário.
- **Sistema de Notificações:** Alertas básicos para lembrar os usuários sobre prazos iminentes e atualizações importantes nas tarefas.

#### Should Have (Deveria Ter)

Itens "Should Have" são importantes e agregam valor significativo, mas não são vitais para o funcionamento básico do produto. A ausência deles é dolorosa, mas não impede o lançamento. São as funcionalidades que, em uma situação ideal, deveriam ser incluídas, mas que podem ser adiadas se houver restrições de tempo ou recursos.

Para o aplicativo de gestão, poderíamos classificar:

- **Relatórios de Progresso do Projeto:** Uma funcionalidade para gerar gráficos e relatórios visuais sobre o andamento do projeto.
- **Integração com E-mails:** A capacidade de responder a comentários de tarefas ou receber atualizações diretamente pela caixa de entrada de e-mail.

#### Could Have (Poderia Ter)

Estes são os itens desejáveis, os "nice-to-have". Sua ausência causa pouco ou nenhum impacto negativo. São funcionalidades que melhoram a experiência do usuário ou adicionam um toque de refinamento, mas só devem ser implementadas se houver tempo e recursos de sobra, após todos os "Must" e "Should" terem sido concluídos.

No nosso exemplo, seriam:

- **Modo Noturno (Dark Mode):** Uma opção de interface que agrada a muitos usuários, mas não afeta a funcionalidade principal.
- **Gamificação:** Introduzir emblemas ou pontos por concluir tarefas para aumentar o engajamento do usuário.

#### Won't Have (Não Terá Desta Vez)

Esta categoria é tão importante quanto as outras. Ela define explicitamente o que está **fora do escopo** para o ciclo de entrega atual. Isso não significa que os itens são descartados para sempre, mas sim que a equipe concorda em não trabalhar neles agora. Deixar isso claro ajuda a gerenciar as expectativas dos stakeholders.

Para o lançamento inicial do nosso aplicativo, poderíamos definir:

- **Integração com Sistemas de CRM/ERP:** Uma funcionalidade complexa e de nicho, que pode ser explorada em versões futuras.
- **Faturamento e Controle de Horas:** Um módulo financeiro completo que, por enquanto, está fora do escopo principal de gerenciamento de tarefas.

Ao utilizar o MoSCoW, a equipe consegue um alinhamento claro sobre o que constitui a versão essencial do produto, focando seus esforços para garantir uma entrega de valor sólida e, ao mesmo tempo, gerenciando as expectativas sobre funcionalidades futuras.

## A Técnica de Scorecard

A técnica de **Scorecard (ou Pontuação Ponderada)** é um método mais analítico e estruturado. Ela combate a subjetividade ao avaliar os itens do backlog contra um conjunto de critérios pré-definidos e ponderados. Isso permite uma comparação objetiva entre diferentes funcionalidades, baseando a priorização em dados e em um alinhamento estratégico claro.

O processo funciona da seguinte forma:

1. **Definição de Critérios:** A equipe, em conjunto com os stakeholders, define os critérios mais relevantes para o sucesso do produto. Critérios comuns incluem valor para o cliente, alinhamento com os objetivos do negócio, esforço de implementação, impacto na receita, redução de custos e mitigação de riscos.
2. **Atribuição de Pesos:** Nem todos os critérios têm a mesma importância. A equipe atribui um peso a cada critério para refletir sua relevância estratégica. Por exemplo, em um MVP, o "Valor para o Cliente" pode ter um peso maior do que a "Redução de Custos".
3. **Atribuição de Pontuações:** Cada item do backlog é avaliado e recebe uma pontuação para cada critério (por exemplo, em uma escala de 1 a 5).
4. **Cálculo da Pontuação Final:** A pontuação de cada critério é multiplicada pelo seu peso. A soma desses resultados gera a pontuação final de cada item.
5. **Priorização:** Os itens do backlog são então ordenados da maior para a menor pontuação, criando uma lista de prioridades baseada em uma análise multifatorial.

### Exemplo: Aplicativo de Finanças Pessoais

Imaginemos uma equipe desenvolvendo um novo aplicativo de finanças pessoais. Eles decidem usar um Scorecard para priorizar as próximas grandes funcionalidades.

**Definição de Critérios e Pesos:**

- **Valor para o Cliente:** O quanto a funcionalidade resolve uma dor real do usuário. (Peso: 3)
- **Alinhamento Estratégico:** O quanto a funcionalidade apoia a visão de longo prazo da empresa. (Peso: 2)
- **Impacto na Receita:** Potencial de gerar receita direta ou indireta. (Peso: 2)
- **Viabilidade Técnica:** A facilidade de implementação, onde uma pontuação maior significa **mais fácil**. (Peso: 1)
- **Risco:** Riscos associados (técnicos, de mercado), onde uma pontuação maior significa **menor risco**. (Peso: 1)

**Itens do Backlog para Priorização:**

- **Funcionalidade A:** Implementação de um recurso de orçamento automatizado.
- **Funcionalidade B:** Adição de uma ferramenta de investimento com inteligência artificial.
- **Funcionalidade C:** Integração de pagamentos de contas via app.

**Atribuição e Cálculo da Pontuação (Escala de 1 a 5):**

|**Critério (Peso)**|**Funcionalidade A (Orçamento)**|**Funcionalidade B (Investimento IA)**|**Funcionalidade C (Pagamentos)**|
|---|---|---|---|
|**Valor para o Cliente (x3)**|5 (Pontos: 15)|3 (Pontos: 9)|4 (Pontos: 12)|
|**Alinhamento Estratégico (x2)**|4 (Pontos: 8)|5 (Pontos: 10)|3 (Pontos: 6)|
|**Impacto na Receita (x2)**|2 (Pontos: 4)|5 (Pontos: 10)|3 (Pontos: 6)|
|**Viabilidade Técnica (x1)**|4 (Pontos: 4)|2 (Pontos: 2)|3 (Pontos: 3)|
|**Risco (Menor Risco = Maior Pont.) (x1)**|4 (Pontos: 4)|2 (Pontos: 2)|3 (Pontos: 3)|
|**TOTAL**|**35**|**33**|**30**|

**Resultado da Priorização:**

Com base na pontuação ponderada, a ordem de prioridade seria:

1. **Funcionalidade A (Orçamento Automatizado):** 35 pontos
2. **Funcionalidade B (Investimento com IA):** 33 pontos
3. **Funcionalidade C (Pagamento de Contas):** 30 pontos

Este exemplo demonstra como o Scorecard transforma a priorização em um exercício estratégico e baseado em dados, garantindo que as decisões reflitam os múltiplos fatores que contribuem para o sucesso de um produto.

## A Técnica BUC (Benefício x Urgência x Custo)

O BUC é uma técnica de priorização que busca um equilíbrio pragmático entre três fatores essenciais: o valor que uma funcionalidade entrega, a sensibilidade ao tempo para sua implementação e os recursos necessários para construí-la.

- **Benefício:** Avalia o impacto positivo da funcionalidade. Qual o valor gerado para o cliente e para o negócio? Isso pode ser medido em aumento de receita, redução de custos, satisfação do cliente, etc.
- **Urgência:** Mede a criticidade do tempo. Existe um prazo legal? Uma janela de oportunidade de mercado que está se fechando? Uma dependência para outras tarefas? Itens urgentes perdem valor rapidamente se não forem implementados a tempo.
- **Custo:** Refere-se a todo o esforço necessário para implementar o item. Isso inclui tempo de desenvolvimento, recursos financeiros, complexidade técnica e qualquer outro tipo de custo associado.

O processo de priorização com BUC envolve avaliar cada item do backlog em relação a esses três critérios, geralmente usando uma escala numérica, e então combinar as pontuações para obter uma classificação final. Uma fórmula comum, embora não única, é calcular um índice de prioridade, como `(Benefício + Urgência) / Custo`. Itens com maior índice são priorizados.

### Exemplo: Sistema de Gerenciamento de RH

Uma equipe está desenvolvendo um novo sistema de RH e precisa priorizar seu backlog. Eles usam o BUC para tomar a decisão.

**Definição dos Critérios (Escala de 1 a 5):**

- **[B] Benefício:** Impacto na eficiência do RH e satisfação dos funcionários.
- **[U] Urgência:** Necessidade de atender a prazos legais ou de negócio.
- **[C] Custo:** Esforço e recursos necessários para o desenvolvimento.

**Itens do Backlog para Priorização:**

1. **Portal do Funcionário para Autoatendimento:** Permite que funcionários acessem seus dados, solicitem férias, etc.
2. **Integração com Software de Folha de Pagamento:** Automatiza o fluxo de dados para o pagamento de salários.
3. **Sistema de Rastreamento de Candidatos (ATS):** Gerencia o processo de recrutamento.

**Avaliação BUC e Priorização:**

1. **Portal do Funcionário:**
    - Benefício: **Alto [5]** (Melhora muito a satisfação e reduz a carga de trabalho do RH).
    - Urgência: **Médio [3]** (Importante, mas sem prazo legal iminente).
    - Custo: **Médio [3]** (Requer um esforço de desenvolvimento moderado).
2. **Integração com Folha de Pagamento:**
    - Benefício: **Alto [5]** (Elimina processos manuais e reduz erros críticos).
    - Urgência: **Alto [5]** (Necessário para cumprir novos regulamentos fiscais no próximo trimestre).
    - Custo: **Alto [4]** (Complexidade técnica e de integração considerável).
3. **Sistema de Rastreamento de Candidatos:**
    - Benefício: **Médio [3]** (Otimiza o recrutamento, mas não afeta os funcionários atuais).
    - Urgência: **Baixo [2]** (Não há prazos imediatos).
    - Custo: **Baixo [2]** (Relativamente simples de implementar).

**Resultado da Priorização:**

Com base na análise BUC, a equipe pode decidir a seguinte ordem:

1. **Integração com Software de Folha de Pagamento:** A combinação de altíssima urgência e alto benefício a torna a prioridade máxima, apesar do custo elevado.
2. **Portal do Funcionário para Autoatendimento:** Seu alto benefício e custo moderado a colocam em segundo lugar.
3. **Sistema de Rastreamento de Candidatos:** Embora tenha baixo custo, seu benefício e urgência menores a posicionam como a prioridade mais baixa para este ciclo.

O BUC oferece uma visão equilibrada, garantindo que a equipe não apenas persiga funcionalidades de alto benefício, mas também considere as restrições práticas de custo e a sensibilidade do tempo.

## Testes de Suposição

Em ambientes de alta incerteza, como no desenvolvimento de produtos inovadores ou em startups, a maior ameaça não é construir a coisa errada, mas sim construir algo que ninguém quer. A técnica de Testes de Suposição, profundamente enraizada na filosofia Lean Startup, prioriza o backlog não com base no valor percebido, mas com base na **necessidade de validar as hipóteses mais arriscadas** por trás de cada funcionalidade.

O objetivo é transformar a priorização em um processo de aprendizado científico, onde os itens de maior prioridade são aqueles que nos permitem testar nossas suposições mais críticas com o menor esforço possível.

As etapas são:

1. **Identificação de Suposições:** Para cada item do backlog, a equipe pergunta: "Quais crenças precisam ser verdadeiras para que esta funcionalidade seja um sucesso?". As suposições podem ser sobre o valor para o usuário, a demanda do mercado, a viabilidade técnica ou o modelo de negócio.
2. **Avaliação de Risco e Impacto:** A equipe avalia cada suposição com base no risco (qual a probabilidade de estarmos errados?) e no impacto (quão ruim seria para o projeto se estivermos errados?). As suposições de maior risco e maior impacto são as mais perigosas.
3. **Design de Testes:** Para as suposições mais críticas, a equipe projeta experimentos rápidos e baratos para validá-las. Isso pode incluir a criação de um Protótipo Mínimo Viável (MVP), landing pages, pesquisas com usuários, ou análises de dados.
4. **Priorização para Aprendizado:** O backlog é priorizado para executar esses experimentos. O trabalho que gera o aprendizado mais validado com o menor esforço sobe para o topo da lista.

### Exemplo: Aplicativo de Companheiros de Corrida

Uma startup está desenvolvendo um aplicativo para conectar corredores locais. O backlog está cheio de ideias, mas os recursos são escassos.

**Suposições Críticas do Projeto:**

- **Suposição A:** Os usuários estão dispostos a pagar uma assinatura mensal por funcionalidades premium, como planos de treino avançados.
- **Suposição B:** A principal proposta de valor é um sistema de matchmaking que conecta corredores com níveis de habilidade e objetivos semelhantes.
- **Suposição C:** Os usuários se engajarão ativamente na criação e participação de eventos de corrida locais através do app.

**Avaliação de Risco e Impacto:**

- **Suposição A:** **Alto Risco / Alto Impacto.** Se for falsa, todo o modelo de receita do negócio falha.
- **Suposição B:** **Médio Risco / Alto Impacto.** É central para a proposta de valor, mas a demanda por matchmaking precisa ser confirmada.
- **Suposição C:** **Baixo Risco / Médio Impacto.** É uma funcionalidade de engajamento, mas não é crucial para a aquisição inicial de usuários.

**Design de Testes e Priorização:**

1. **Prioridade 1 (Testar Suposição A):** Em vez de construir o recurso premium, a equipe cria uma landing page descrevendo as funcionalidades e um botão "Assine Agora". O objetivo é medir a taxa de cliques e a intenção de compra. Este é um teste rápido e barato.
2. **Prioridade 2 (Testar Suposição B):** A equipe desenvolve um protótipo não funcional (mockup) do sistema de matchmaking e o apresenta a um grupo de corredores, coletando feedback qualitativo sobre seu apelo e utilidade.
3. **Prioridade 3 (Adiar Teste da Suposição C):** A validação desta suposição é postergada até que as hipóteses mais críticas sobre receita e valor principal sejam confirmadas.

Ao usar Testes de Suposição, a startup evita gastar meses desenvolvendo um recurso que ninguém pagaria para usar, focando seus esforços em validar o núcleo do seu modelo de negócio e da sua proposta de valor.

## Matriz de Valor de Negócio vs. Risco

Esta técnica oferece uma abordagem visual e estratégica para a priorização, classificando os itens do backlog em uma matriz 2x2. Os eixos representam dois dos fatores mais importantes em qualquer projeto: o benefício que uma funcionalidade pode trazer e as incertezas ou desafios associados à sua construção.

- **Eixo do Valor de Negócio:** Avalia o impacto positivo de um item. Isso inclui aumento de receita, satisfação do cliente, eficiência operacional ou alinhamento com metas estratégicas.
- **Eixo do Risco:** Avalia a incerteza. Isso pode ser risco técnico (complexidade, tecnologia desconhecida), risco de mercado (aceitação pelo usuário) ou dependências externas.

Ao plotar os itens do backlog nesta matriz, quatro quadrantes emergem, cada um sugerindo uma estratégia de priorização diferente:

1. **Alto Valor, Baixo Risco (Fazer Agora):** Estes são os "Quick Wins" ou as vitórias fáceis. Eles oferecem um retorno significativo com alta certeza de sucesso. Devem ser priorizados e atacados imediatamente.
2. **Alto Valor, Alto Risco (Investigar e Mitigar):** Estes itens são potencialmente transformadores, mas carregam muita incerteza. A prioridade aqui não é construir a funcionalidade completa, mas sim investir em atividades para mitigar o risco, como provas de conceito, protótipos ou pesquisa de usuário, antes de se comprometer totalmente.
3. **Baixo Valor, Baixo Risco (Fazer se Tiver Tempo):** Estes são itens de "enchimento". São fáceis de fazer, mas não movem muito o ponteiro do negócio. Podem ser encaixados no backlog quando há capacidade ociosa, mas não devem tomar o lugar de itens de alto valor.
4. **Baixo Valor, Alto Risco (Evitar):** Estes são os "ralos de dinheiro". Exigem muito esforço para um retorno mínimo e devem ser evitados ou removidos do backlog.

### Exemplo: Plataforma de E-commerce para PMEs

Uma empresa está construindo uma nova plataforma de e-commerce e usa a matriz para priorizar seu backlog.

**Itens do Backlog para Priorização:**

- **A - Integração com Sistemas de Pagamento Populares:** Essencial para qualquer loja online.
- **B - Sistema de Recomendação de Produtos Baseado em IA:** Potencial para aumentar vendas, mas tecnicamente complexo.
- **C - Ferramenta de Análise de Vendas:** Útil para os vendedores, mas não essencial para o comprador.
- **D - Migrador de Layout de Lojas Antigas:** Funcionalidade de baixo valor e alto risco técnico.

**Análise e Priorização na Matriz:**

- **Quadrante de Alto Valor, Baixo Risco:**
    - **Item A (Integração de Pagamentos):** Essencial para o negócio (alto valor) e tecnicamente bem compreendido (baixo risco). **Prioridade máxima.**
- **Quadrante de Alto Valor, Alto Risco:**
    - **Item B (Recomendação com IA):** Pode aumentar significativamente as vendas (alto valor), mas o desenvolvimento de IA é complexo e incerto (alto risco). **Prioridade:** Iniciar uma prova de conceito para validar a viabilidade técnica e o impacto real nas vendas antes de construir a solução completa.
- **Quadrante de Baixo Valor, Baixo Risco:**
    - **Item C (Análise de Vendas):** É uma funcionalidade útil (valor médio-baixo) e de complexidade moderada (risco baixo-médio). **Prioridade:** Colocar no backlog para ser desenvolvido após os itens de alto valor, quando houver capacidade.
- **Quadrante de Baixo Valor, Alto Risco:**
    - **Item D (Migrador de Layout):** Atinge um pequeno número de usuários (baixo valor) e é tecnicamente muito arriscado (alto risco). **Prioridade:** Descartar ou repensar completamente.

Esta técnica fornece um mapa visual claro para a tomada de decisões, equilibrando a busca por valor com uma gestão prudente dos riscos do projeto.

## Considerações Finais

A priorização de backlog é muito mais uma arte do que uma ciência exata, mas as técnicas apresentadas neste capítulo fornecem a estrutura necessária para transformar conversas subjetivas em decisões estratégicas e colaborativas. Não existe uma única "melhor" técnica; a escolha dependerá do contexto do projeto, da maturidade da equipe e da cultura da organização.

O MoSCoW é excelente para negociações de escopo rápidas e claras. O Scorecard e o BUC trazem um rigor analítico para ambientes complexos com múltiplos fatores a serem considerados. A Matriz de Valor vs. Risco oferece uma visão estratégica e visual, enquanto os Testes de Suposição são indispensáveis para produtos inovadores navegando em um mar de incertezas.

O mais importante é reconhecer que a priorização não é um evento único, mas um processo vivo e contínuo. O backlog é um artefato dinâmico que deve ser revisitado e reordenado constantemente à medida que a equipe aprende e o mercado evolui. Ao dominar essas ferramentas, as equipes ágeis se capacitam para focar no que realmente importa: a entrega contínua de valor máximo para seus clientes e para o negócio.
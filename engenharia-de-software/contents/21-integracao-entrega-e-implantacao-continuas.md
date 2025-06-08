# Capítulo 21 – A Esteira da Agilidade: Integração, Entrega e Implantação Contínua

No desenvolvimento de software moderno, a velocidade não é mais um diferencial, mas uma necessidade. Impulsionadas por metodologias ágeis e pela demanda incessante do mercado, as equipes de desenvolvimento precisam entregar valor de forma rápida, consistente e confiável. No entanto, a agilidade no desenvolvimento de código pode ser em vão se o processo de integração e liberação for lento, manual e propenso a erros.

É para resolver esse gargalo que surgiram as práticas de **Integração Contínua**, **Entrega Contínua** e **Implantação Contínua**. Esses três conceitos, embora relacionados, representam diferentes níveis de automação e maturidade no processo de entrega de software, formando uma verdadeira esteira de produção que leva o código do desenvolvedor até o usuário final com eficiência e segurança.

## Integração Contínua (Continuous Integration): O Alicerce da Colaboração

A **Integração Contínua (Continuous Integration - CI)** é um termo que se originou na metodologia ágil Extreme Programming (XP), mas sua aplicação se espalhou por praticamente todos os contextos de desenvolvimento de software modernos. Ela consiste em uma prática de desenvolvimento onde os desenvolvedores integram o código que foi alterado ou recém-desenvolvido ao projeto principal com alta frequência, idealmente várias vezes ao dia.

O objetivo principal da Integração Contínua é verificar, de forma rápida e automática, se as novas alterações implementadas não criaram novos defeitos ou quebraram funcionalidades no projeto já existente. Essa verificação constante previne o que se costuma chamar de "inferno da integração" – aquele momento doloroso, no final de um ciclo de desenvolvimento, em que várias partes do código desenvolvidas isoladamente por semanas são juntadas pela primeira vez, resultando em uma cascata de conflitos e bugs difíceis de rastrear.

Um dos maiores expoentes do desenvolvimento de software, Martin Fowler, define a prática da seguinte forma:

> “A Integração Contínua se trata de uma prática de desenvolvimento de software em que os membros de um time integram seu trabalho frequentemente, geralmente cada pessoa integra pelo menos diariamente – podendo haver múltiplas integrações por dia. Cada integração é verificada por um build automatizado (incluindo testes) para detectar erros de integração o mais rápido possível. Muitos times acham que essa abordagem leva a uma significante redução nos problemas de integração e permite que um time desenvolva software coeso mais rapidamente.”

A grande vantagem da Integração Contínua está no **feedback instantâneo**. A prática se mostra extremamente eficaz e é amplamente utilizada. Inúmeros projetos de código aberto (_open-source_) utilizam várias máquinas dedicadas a serem servidores de integração, rodando testes em diferentes softwares e plataformas. É uma prática recomendada tanto para equipes distribuídas geograficamente quanto para equipes que trabalham no mesmo local.

### O Problema que a CI Resolve

Um problema recorrente no desenvolvimento é que os erros ocultos nos programas se tornam cada vez mais caros e difíceis de corrigir à medida que o tempo passa. Qualquer pessoa que trabalhe com programação já passou pela situação de achar que um programa estava quase pronto e "só faltava testar", para depois descobrir que muita coisa ainda estava errada ou faltando no código. Outra situação comum é a prática de postergar os testes. Sem testes e _deploys_ automatizados, o desenvolvedor tende a escrever uma grande quantidade de código antes de realmente colocá-lo para executar. Afinal, se o processo é trabalhoso e demorado, ele será evitado ao máximo. No final, gasta-se muito tempo para corrigir todos os "detalhes" que não estavam corretos.

<div align="center">
  <img width="360px" src="21-integracao-continua.png">
</div>

A Integração Contínua surgiu justamente para mitigar esses problemas, automatizando o processo para que, com bastante frequência, seja possível integrar todas as alterações de todos os desenvolvedores e realizar um teste geral.

Uma metáfora útil para explicar esse tema é a edição de um filme de ação. Todo filme é composto de várias cenas. No nosso caso, uma aplicação é composta de vários builds (construções). Toda vez que uma nova cena é adicionada ao filme, o editor faz um teste rodando o filme inteiro novamente para ver se a história continua fazendo sentido com a nova cena. Caso o teste falhe, ou seja, a nova cena não se encaixe na história, ela deve ser refeita. No desenvolvimento, quando isso acontece, dizemos que o **build quebrou**. Em vez de construir vários componentes separadamente e depois juntá-los em um software final, a integração é feita continuamente. Novos builds só são integrados quando o build anterior quebrado for corrigido. Esses procedimentos auxiliam a reduzir riscos e a entregar soluções mais livres de defeitos.

### Práticas e Fluxo de Trabalho da Integração Contínua

Para que a Integração Contínua funcione, um conjunto de práticas deve ser seguido e um fluxo de trabalho automatizado deve ser estabelecido.

As principais práticas são:

|**Práticas Essenciais da Integração Contínua**|
|---|
|**Mantenha um único repositório de código-fonte:** Todo o código do projeto, incluindo scripts de teste e de _build_, deve estar em um único local de controle de versão (como Git).|
|**Automatize o _build_:** O processo de compilar o código, empacotar os artefatos e preparar o sistema para execução deve ser feito através de um script automatizado, sem passos manuais.|
|**Faça um _build_ auto-testável:** O script de _build_ deve incluir a execução de uma suíte de testes automatizados que valide a saúde do sistema.|
|**Todo _commit_ deve gerar um _build_ na máquina de integração:** Cada alteração enviada ao repositório principal deve disparar o processo de CI.|
|**Mantenha os _builds_ rápidos:** O processo de CI deve fornecer feedback em minutos. Se demorar horas, os desenvolvedores hesitarão em integrar seu trabalho.|
|**Teste em uma cópia do ambiente de produção:** O ambiente do servidor de integração deve ser o mais próximo possível do ambiente de produção para evitar surpresas no _deploy_.|
|**Mantenha fácil o acesso ao último executável:** Todos na equipe devem poder obter facilmente a última versão estável que passou nos testes.|
|**Todos conseguem visualizar o processo:** O status do _build_ (sucesso ou falha) deve ser transparente e visível para toda a equipe.|
|**Automatize o _deployment_:** O processo de implantar a aplicação em um ambiente de testes ou homologação também deve ser automatizado.|

O fluxo de trabalho típico de CI, geralmente orquestrado por uma ferramenta como o Jenkins, segue estes passos:

|**Como a Integração Contínua Acontece**|
|---|
|**1. Checkout do Código:** Desenvolvedores fazem _checkout_ do código do repositório principal para seus ambientes de trabalho privados.|
|**2. Commit das Alterações:** Quando o trabalho em uma funcionalidade é finalizado, o desenvolvedor faz o _commit_ das modificações para o repositório.|
|**3. Monitoramento do Servidor de CI:** O Servidor de Integração Contínua monitora o repositório e detecta as novas mudanças.|
|**4. Build e Teste:** O servidor faz o _checkout_ das mudanças, constrói o sistema e executa todos os testes de unidade e de integração.|
|**5. Geração de Artefatos:** Se o _build_ e os testes forem bem-sucedidos, o servidor lança os artefatos implantáveis (ex: .jar, .war, imagem Docker) para um repositório de artefatos.|
|**6. Rotulagem da Versão:** O servidor atribui um rótulo (uma tag de versão) ao código que ele acabou de construir com sucesso.|
|**7. Notificação de Sucesso ou Falha:** O servidor informa à equipe sobre o sucesso do _build_. Se o _build_ ou algum teste falhar, o servidor alerta a equipe imediatamente.|
|**8. Correção Imediata:** A equipe tem como prioridade máxima corrigir o problema que quebrou o _build_. Ninguém deve fazer novos _commits_ até que o problema seja resolvido.|
|**9. Ciclo Contínuo:** O processo se repete continuamente ao longo de todo o projeto.|

## Entrega Contínua (Continuous Delivery): Sempre Pronto para o Lançamento

A **Entrega Contínua (Continuous Delivery)** é a evolução natural da Integração Contínua. Uma equipe que pratica Entrega Contínua garante que seu software pode ser **liberado para produção a qualquer momento**.

Enquanto a CI foca em garantir que o código dos desenvolvedores esteja sempre integrado e testado, a Entrega Contínua estende esse processo, garantindo que cada build que passa com sucesso por todo o pipeline automatizado seja, de fato, uma **versão candidata ao lançamento**. Isso significa que, após os testes de unidade e integração, a aplicação passa por estágios adicionais de teste, como testes de aceitação, testes de performance e testes de UI, geralmente em um ambiente de homologação que simula o de produção.

O ponto-chave da Entrega Contínua é que, após um _build_ passar por todos esses estágios, ele é considerado pronto para ser implantado. A decisão de efetivamente fazer o _deploy_ para produção, no entanto, **continua sendo um passo manual**, geralmente acionado por uma decisão de negócio. O gerente de produto, por exemplo, pode decidir lançar uma nova funcionalidade na terça-feira, e não na sexta, por razões estratégicas. A equipe de TI simplesmente aperta um botão para que o deploy aconteça.

Apresentar a última versão do produto para um gerente de projetos não é suficiente, nem entregá-la a uma equipe de garantia de qualidade. Uma entrega, em seu sentido primário, deve ser pelo menos uma versão (muitas vezes beta) avaliada por usuários representativos. Entre os benefícios esperados, busca-se mitigar a falha de planejamento de descobrir atrasos muito tarde, validar o ajuste do produto ao mercado mais cedo, fornecer feedbacks adiantados sobre a qualidade e permitir um retorno mais rápido do investimento.

## Implantação Contínua (Continuous Deployment): Automatizando a Entrega Final

A **Implantação Contínua (Continuous Deployment)** leva a automação um passo adiante. É uma prática na qual cada alteração que passa com sucesso por todo o pipeline de testes é **automaticamente implantada no ambiente de produção**, sem qualquer intervenção humana.

A grande diferença entre Entrega e Implantação Contínua reside nesse último passo. Na Entrega Contínua, há uma versão candidata pronta, e o negócio decide **quando** implantá-la. Na Implantação Contínua, não há uma decisão manual de deploy; a implantação ocorre automaticamente assim que o pipeline de automação é concluído com sucesso.

Se o código já passou pelos testes automatizados (unidade, integração, aceitação) e, possivelmente, por testes manuais exploratórios, visuais e de comportamento, então ele pode ser publicado de forma automatizada. Essa prática exige um nível extremamente alto de confiança nos testes e na infraestrutura, além de mecanismos robustos de monitoramento e de rollback rápido em caso de problemas.

## Visualizando as Diferenças: Do Commit à Produção

Para que as diferenças fiquem absolutamente claras, vamos visualizar o fluxo completo, desde o trabalho do desenvolvedor até a entrega final em produção.

<div align="center">
  <img width="800px" src="21-diferencas-ci-cd-cd.png">
</div>

Vamos percorrer o desenho usando um exemplo prático. Imagine uma equipe de desenvolvimento com quatro pessoas trabalhando em um sistema.

1. **Commit do Desenvolvedor:** O terceiro desenvolvedor termina a implementação de uma nova funcionalidade e realiza o commit de seu código para a ferramenta de controle de versão (ex: Git).
2. **Integração Contínua (CI):** Uma ferramenta de CI (ex: Jenkins) detecta o commit. Ela automaticamente baixa o código, compila o sistema e executa todos os **testes de unidade e de integração**.
    - **Se os testes falharem:** A equipe é notificada imediatamente. O build é considerado "quebrado", e a prioridade número um é consertá-lo.
    - **Se os testes passarem:** O processo continua. O código é integrado com sucesso ao ambiente de desenvolvimento. Agora, todos os outros desenvolvedores podem usufruir dessa nova funcionalidade em seus ambientes locais. **Até aqui, temos a Integração Contínua.**
3. **Entrega Contínua (Continuous Delivery):** O fluxo não para. Após a CI, o build bem-sucedido é automaticamente implantado em um ambiente de **homologação** ou staging. Nesse ambiente, uma nova bateria de **testes de aceitação automatizados** é executada para validar o comportamento do sistema do ponto de vista do usuário.
    - **Se esses testes passarem:** O build é considerado uma **versão candidata para produção**. Ele está pronto, estável e validado. Fica aguardando a decisão de negócio para ser implantado no ambiente de produção. **Até este ponto, temos a Entrega Contínua.**
4. **Implantação Contínua (Continuous Deployment):** Se a equipe pratica a Implantação Contínua, o fluxo continua sem intervenção manual.
    - **Deploy Automatizado:** Após passar com sucesso por todos os estágios de teste anteriores, o build é **automaticamente implantado no ambiente de produção**, ficando disponível para os usuários finais. A decisão de negócio foi substituída pela confiança no pipeline automatizado. **Este último passo automático caracteriza a Implantação Contínua.**

## Considerações Finais

As práticas de Integração, Entrega e Implantação Contínua representam uma progressão lógica na busca pela agilidade e eficiência no ciclo de vida de desenvolvimento de software. Elas não são conceitos isolados, mas sim etapas de uma jornada de maturidade em automação e colaboração.

A **Integração Contínua (CI)** é o alicerce fundamental, quebrando os silos entre desenvolvedores e garantindo que o código esteja sempre em um estado funcional e testado. A partir dela, a **Entrega Contínua (Continuous Delivery)** estende essa segurança, garantindo que cada versão do software esteja sempre em um estado "pronto para produção", transformando o lançamento em uma decisão de negócio de baixo risco, e não em um evento técnico traumático.

Por fim, a **Implantação Contínua (Continuous Deployment)** representa o ápice da automação, onde a confiança no pipeline é tão alta que cada melhoria validada é entregue aos usuários de forma instantânea.

Adotar essa esteira de práticas não apenas acelera drasticamente o tempo de entrega, mas também melhora a qualidade do produto, reduz os riscos associados a grandes lançamentos e cria ciclos de feedback extremamente rápidos. Em essência, CI/CD transforma a entrega de software de um processo lento e arriscado em uma rotina previsível, confiável e alinhada às necessidades dinâmicas do negócio.
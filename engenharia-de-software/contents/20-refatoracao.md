# Capítulo 20 – Refatoração: Aprimorando o Design do Código Existente

No ciclo de vida de um software, a mudança é a única constante. Novos requisitos surgem, tecnologias evoluem e o entendimento sobre o problema se aprofunda. Nesse processo contínuo de evolução, o código-fonte, se não for cuidadosamente gerenciado, tende a se tornar complexo, desorganizado e difícil de manter. É para combater essa entropia natural do software que surge uma das disciplinas mais importantes da engenharia de software moderna: a **refatoração**.

<div align="center">
  <img width="620px" src="20-refatoracao-tirinha.png">
</div>

A refatoração, uma atividade central em diversas metodologias ágeis como o Extreme Programming (XP), é muito mais do que simplesmente "limpar o código". É uma técnica disciplinada e estratégica que permite aprimorar a qualidade interna do software sem alterar seu comportamento externo, garantindo sua saúde e longevidade.

## O Que é Refatoração? Definindo a Disciplina

A refatoração é uma técnica de reorganização que simplifica e melhora o projeto (ou código) de um componente de software sem modificar sua função ou seu comportamento observável. Martin Fowler, uma das maiores autoridades no assunto, oferece em seu livro seminal uma definição precisa e amplamente aceita:

> “Refatoração é o processo de alterar um sistema de software de modo que o comportamento externo do código não se altere, mas a estrutura interna se aprimore. É uma forma disciplinada de organizar código (e modificar/simplificar o projeto interno) que minimiza as chances de introdução de bugs. Em resumo, ao se refatorar, se está aperfeiçoando o projeto de codificação depois de este ter sido feito."

Quando um software é refatorado, o projeto existente é examinado em busca de problemas estruturais, tais como: redundância de código, elementos de projeto não utilizados, algoritmos ineficientes ou desnecessários, estruturas de dados mal construídas ou inapropriadas, ou qualquer outra falha de projeto que possa ser corrigida para produzir um design melhor e mais limpo.

Para um exemplo prático, imagine que uma primeira iteração de projeto resultou em um componente de software com baixa coesão. Este componente realiza três funções distintas que possuem apenas um relacionamento limitado entre si. Após uma análise cuidadosa, a equipe pode decidir que o componente deve ser refatorado em três componentes menores e distintos, cada um apresentando alta coesão e uma responsabilidade única. O resultado dessa refatoração será um software mais fácil de integrar, de testar e, principalmente, de manter no futuro.

Portanto, a refatoração consiste na melhoria da estrutura interna do código-fonte de um programa, enquanto preserva seu comportamento externo.

### O Que a Refatoração Não É

É igualmente importante entender o que a refatoração **não** é, para evitar equívocos comuns:

- **Refatoração não é reescrever o código do zero:** A reescrita implica descartar o código existente e começar de novo, uma abordagem muito mais arriscada e custosa. A refatoração trabalha com o código existente, fazendo melhorias incrementais e controladas.
- **Refatoração não é consertar bugs:** Embora o processo de refatoração possa, incidentalmente, revelar bugs latentes, seu objetivo principal não é a correção de defeitos. Se um bug é encontrado, o ideal é primeiro escrever um teste que o reproduza, corrigi-lo e só então continuar a refatoração.
- **Refatoração não é melhorar aspectos observáveis do software:** Mudanças na interface do usuário (UI), adição de novas funcionalidades ou alterações no comportamento externo do programa não são refatoração. Essas são atividades de desenvolvimento de novas features.

A refatoração melhora atributos internos do código-fonte, tais como seu tamanho, a quantidade de duplicação, o acoplamento entre componentes, a coesão de cada componente e a complexidade ciclomática. Essas melhorias estão diretamente relacionadas com a facilidade de manutenção do software. Além disso, o ato de refatorar ajuda na compreensão do código-fonte e encoraja o desenvolvedor a pensar criticamente sobre suas decisões de projeto.

## Os Pilares da Refatoração: Princípios Fundamentais

A aplicação da refatoração de forma segura e eficaz é guiada por alguns princípios fundamentais. Eles servem como um guia para garantir que as melhorias internas não introduzam problemas inesperados.

|**Princípios**|**Descrição**|
|---|---|
|**Mantenha o Comportamento Externo**|Este é o princípio mais importante. Significa que, ao realizar uma refatoração, é crucial garantir que a forma como o programa funciona do ponto de vista do usuário ou de outros sistemas não seja alterada. Isso é fundamental para que o programa continue funcionando corretamente após as mudanças.|
|**Simplifique o Código**|O objetivo da refatoração é simplificar e melhorar a estrutura do código. Por isso, é importante eliminar código redundante, simplificar expressões complexas e tornar o código mais legível e fácil de entender, seguindo a máxima de que um código simples é mais fácil de manter.|
|**Trabalhe com Testes Automatizados**|Os testes automatizados são a rede de segurança da refatoração. Eles garantem que o programa continue funcionando corretamente após as mudanças realizadas. É uma prática essencial criar testes para todas as partes do código que serão modificadas e executá-los antes e depois de cada pequena refatoração.|
|**Refatore Continuamente**|A refatoração não deve ser um evento raro e massivo, mas sim uma atividade contínua, realizada ao longo de todo o ciclo de vida do programa. A equipe de desenvolvimento deve estar constantemente procurando maneiras de melhorar e simplificar o código, integrando a refatoração ao seu fluxo de trabalho diário.|
|**Use Padrões de Projeto**|Os padrões de projeto (Design Patterns) são soluções comprovadas e reutilizáveis para problemas comuns de design de software. Eles podem ser usados durante a refatoração para simplificar o código, melhorar sua estrutura e torná-lo mais fácil de entender e manter.|

Para ilustrar o conceito, podem ser usadas algumas analogias. Digamos que se tenha um guarda-roupa bagunçado, com roupas misturadas e desorganizadas. A refatoração seria como organizar esse guarda-roupa, separando as roupas por tipo, dobrando-as adequadamente e criando categorias para facilitar a busca. Ao refatorar o guarda-roupa, sua funcionalidade (armazenar roupas) não muda, mas sua organização interna e facilidade de uso melhoram drasticamente.

A própria atividade de escrita, como a deste conteúdo, também passa por refatorações. Durante a escrita, percebe-se que algumas frases podem ser reformuladas para melhorar a clareza ou a coesão do texto. Isso é análogo à refatoração de software, onde são feitas alterações no código para torná-lo mais legível, eficiente e fácil de entender.

## O "Faro" para o Código: Identificando Oportunidades de Refatoração

Imagine uma mudança para uma nova casa. No começo, a disposição dos móveis parece adequada. No entanto, com o uso diário, percebe-se que alguns móveis estão no lugar errado ou não funcionam como deveriam, e que certas áreas da casa estão subutilizadas.

Da mesma forma, quando desenvolvedores criam um sistema, eles o projetam com base em seus conhecimentos e suposições daquele momento. Com a evolução do sistema, eles podem perceber que algumas partes do código estão causando problemas, tornando-se difíceis de manter ou simplesmente não são mais necessárias. Esses "sintomas" no código são conhecidos na comunidade de software como **Code Smells** (maus cheiros do código).

Identificar essas oportunidades de refatoração é uma atividade crucial para garantir a manutenibilidade e a qualidade do software. Ao identificar e tratar esses sinais, a equipe de desenvolvimento pode melhorar significativamente a saúde do código do programa.

|**Oportunidades (Code Smells)**|**Descrição**|
|---|---|
|**Código Duplicado**|Se houver partes do código que se repetem em vários lugares do programa, é uma forte indicação para refatorar, criando uma função ou classe separada para lidar com essa funcionalidade repetida e centralizá-la.|
|**Métodos ou Funções Muito Longos**|Métodos ou funções extensos são difíceis de entender, testar e modificar. Se um método é muito grande, é uma oportunidade para dividi-lo em partes menores e mais coesas, cada uma com uma responsabilidade clara.|
|**Complexidade Excessiva**|Se um trecho de código é muito complexo, com muitas condicionais aninhadas ou lógica intrincada, é uma oportunidade para simplificá-lo, talvez utilizando padrões de projeto ou extraindo partes da lógica para métodos auxiliares.|
|**Dificuldade para Adicionar Funcionalidades**|Se for difícil e demorado adicionar novas funcionalidades ao código existente, isso é um sinal claro de que a estrutura do código precisa ser melhorada e simplificada para ser mais flexível e extensível.|
|**Código Antigo ou Legado**|Código antigo ou legado pode estar desatualizado, utilizando práticas ou tecnologias obsoletas. Ele pode precisar de refatoração para se tornar mais eficiente, seguro e manutenível.|
|**Problemas de Performance**|Se o código estiver executando lentamente ou apresentando problemas de desempenho, pode ser uma oportunidade para refatorar e otimizar os algoritmos ou as estruturas de dados utilizados.|

## A Caixa de Ferramentas do Desenvolvedor: Técnicas de Refatoração

Existem diversas técnicas de refatoração catalogadas que podem ser aplicadas para melhorar o design e a qualidade do código. Cada técnica aborda um code smell específico. Algumas das principais incluem:

|**Técnicas**|**Descrição**|
|---|---|
|**Extração de Método (Extract Method)**|Envolve a extração de um trecho de código de um método maior para um novo método separado e com um nome descritivo. É útil quando se tem um pedaço de código que pode ser reutilizado ou quando um método está fazendo mais de uma coisa.|
|**Método Inline (Inline Method)**|É o oposto da extração. Envolve a substituição de uma chamada de método pelo seu conteúdo real. É útil quando o corpo de um método é tão simples quanto seu nome, tornando desnecessária a indireção da chamada.|
|**Extração de Variável (Extract Variable)**|Envolve a extração de uma expressão complexa para uma variável temporária com um nome significativo. Isso torna o código mais legível e fácil de entender, especialmente se a expressão for usada várias vezes.|
|**Renomeação (Rename)**|Envolve a alteração do nome de uma variável, método ou classe para um nome mais descritivo e significativo. Um bom nome é uma das formas mais simples e eficazes de melhorar a clareza do código.|
|**Extração de Classe (Extract Class)**|Envolve a extração de um conjunto de campos e métodos de uma classe existente para uma nova classe separada. É útil quando uma classe está ficando muito grande e acumulando muitas responsabilidades.|
|**Divisão de Variável Temporária**|Envolve a substituição de uma variável temporária que tem múltiplos propósitos por variáveis separadas, cada uma responsável por uma única coisa. Isso torna o código mais fácil de entender e mais modular.|
|**Remoção de Atribuições a Parâmetros**|Envolve evitar a modificação do valor de parâmetros dentro de um método. Em vez disso, cria-se uma variável local. Isso torna o código mais fácil de entender e menos propenso a erros de efeitos colaterais.|
|**Substituição de Código Duplicado**|Consiste em identificar trechos de código que se repetem e substituí-los por uma única função ou classe que possa ser reutilizada.|
|**Encapsulamento de Dados (Encapsulate Field)**|Consiste em tornar as variáveis de instância privadas e criar métodos de acesso públicos (_getters_ e _setters_) para manipulá-las. Isso ajuda a garantir a integridade dos dados e o baixo acoplamento.|
|**Decomposição de Condicionais**|Consiste em extrair lógicas complexas de blocos `if-then-else` para métodos separados, tornando a condição principal mais legível.|
|**Generalização (Pull Up/Push Down Method/Field)**|Consiste em mover funcionalidades comuns para uma superclasse (Pull Up) ou funcionalidades específicas para subclasses (Push Down), melhorando a hierarquia de herança.|

## A Rede de Segurança: A Importância dos Testes na Refatoração

Imagine um mecânico trabalhando em um carro antigo. Antes de fazer qualquer conserto ou melhoria, ele primeiro testa as funções essenciais: o motor liga? As luzes funcionam? Os freios estão respondendo? Somente com essa linha de base ele começa a fazer as alterações. Após cada ajuste, ele testa novamente para garantir que não quebrou nada que antes funcionava.

Da mesma forma, os **testes de refatoração** são feitos em software para garantir que as atualizações e melhorias na estrutura interna não tenham efeitos colaterais indesejados. Os testes verificam se o software ainda atende aos requisitos originais e se todas as funcionalidades ainda estão funcionando conforme o esperado. Eles são a rede de segurança que permite ao desenvolvedor refatorar com confiança.

Testar a refatoração é uma etapa crucial. Independentemente da abordagem escolhida, é importante garantir que os testes sejam realizados de forma completa e rigorosa.

|**Abordagens de Teste na Refatoração**|**Descrição**|
|---|---|
|**Teste Manual**|Envolve a execução manual do programa após a refatoração para verificar se todas as funcionalidades ainda estão funcionando corretamente. Essa abordagem é útil para validar pequenas alterações, mas é demorada, propensa a erros humanos e insustentável para refatorações complexas.|
|**Teste de Regressão**|A abordagem mais eficaz. Envolve a criação de uma suíte de testes automatizados antes da refatoração e a execução desses testes após cada pequena alteração para verificar se o comportamento do sistema permanece o mesmo.|
|**Teste de Unidade**|Envolve a criação de testes de unidade focados nas partes do código que foram alteradas durante a refatoração. Esses testes são executados automaticamente para garantir que as alterações não introduziram novos erros no nível do componente.|

Além disso, é importante documentar as alterações realizadas durante a refatoração (geralmente através de mensagens de _commit_ claras em um sistema de controle de versão) e manter um histórico do código para facilitar a manutenção e a evolução do software.

## Refatoração Contínua: Integrando a Melhoria ao Fluxo de Trabalho

Um conceito poderoso é a **refatoração contínua**, uma prática que envolve incorporar a refatoração no processo de desenvolvimento diário, de forma a manter o código sempre limpo, organizado e fácil de manter. A ideia é que, em vez de realizar grandes e arriscadas refatorações em momentos pontuais do projeto, a equipe realize **pequenas refatorações constantemente**, à medida que trabalha no código.

Para incorporar a refatoração contínua, é importante que a equipe esteja comprometida em manter o código limpo e dedique tempo para realizar essas pequenas melhorias ao longo do desenvolvimento. Algumas práticas recomendadas para maximizar os benefícios e minimizar os riscos incluem:

|**Práticas Recomendadas para Refatoração**|**Descrição**|
|---|---|
|**Planeje a Refatoração**|Antes de começar a refatorar, mesmo que seja uma pequena mudança, planeje o que será feito. Isso inclui identificar as áreas a serem melhoradas, escolher as técnicas apropriadas e pensar em como minimizar os riscos.|
|**Refatore em Pequenos Passos**|Refatorar em passos muito pequenos (ex: renomear uma variável, extrair um método) ajuda a minimizar os riscos, permitindo testar e validar cada alteração antes de avançar. Isso mantém o código sempre funcional.|
|**Use Técnicas de Versionamento**|O uso de um sistema de controle de versão como o Git é indispensável. Antes de iniciar uma refatoração mais significativa, crie uma nova _branch_. Isso permite testar e validar as alterações de forma isolada, sem afetar a versão principal do código.|
|**Teste Rigorosamente**|Testar é fundamental. Certifique-se de que existem testes automatizados que cubram a área a ser refatorada. Execute-os antes e depois de cada passo da refatoração para garantir que nada foi quebrado.|
|**Envolva Toda a Equipe**|A refatoração deve ser uma responsabilidade compartilhada. Envolver todos os membros da equipe, por exemplo, através de revisões de código (_code reviews_), ajuda a identificar oportunidades, a garantir que as alterações sejam compreendidas por todos e a disseminar boas práticas.|
|**Aprenda e Pratique Constantemente**|Refatoração é uma habilidade que melhora com a prática. É importante aprender novas técnicas, praticar sempre que possível e buscar feedback da equipe para aprimorar continuamente.|

Com a refatoração contínua, a equipe de desenvolvimento pode manter o código sempre limpo e organizado, reduzindo o acúmulo de "dívida técnica" e o esforço necessário para realizar grandes reestruturações no futuro.

## Distinção Crucial: Refatoração versus Reengenharia

Por fim, é crucial diferenciar claramente a refatoração da **reengenharia**. Imagine um aplicativo que atendeu às necessidades de um negócio por dez ou quinze anos. Durante esse tempo, ele foi corrigido e adaptado inúmeras vezes. Embora as intenções fossem boas, as melhores práticas de engenharia de software foram frequentemente deixadas de lado devido à pressão por entregas rápidas. Agora, o aplicativo está instável. Ele ainda funciona, mas cada nova alteração causa efeitos colaterais sérios e inesperados. O que fazer?

É aqui que entra a **reengenharia**. Enquanto a refatoração são pequenas cirurgias para melhorar a saúde de um sistema, a reengenharia é uma cirurgia de grande porte para salvá-lo. **Reengenharia é o exame, a análise e a reestruturação de um sistema de software existente para reconstituí-lo em uma nova forma**, muitas vezes envolvendo mudanças significativas na arquitetura ou na tecnologia subjacente.

O objetivo da reengenharia é compreender os artefatos atuais do software e melhorar sua funcionalidade e seus atributos de qualidade, como capacidade de evolução, desempenho e reutilização. Fundamentalmente, um novo sistema é gerado a partir de um sistema operacional existente, de modo que o sistema de destino possua melhores fatores de qualidade. Em outras palavras, a reengenharia é feita para converter um sistema de "ruim" para "bom" em uma escala macro.

O processo de reengenharia é complexo e custoso, envolvendo atividades como:

<div align="center">
  <img width="620px" src="20-reengenharia.png">
</div>

- **Análise de Inventário:** Mapear todos os aplicativos e suas características.
- **Reestruturação de Documentos:** Atualizar ou criar a documentação mínima necessária.
- **Engenharia Reversa:** Analisar o programa para criar uma representação em um nível de abstração mais alto (ex: extrair modelos de design do código-fonte).
- **Reestruturação do Código:** Reescrever partes do código, talvez em uma tecnologia mais moderna, preservando a funcionalidade.
- **Reestruturação dos Dados:** Revisar e melhorar as estruturas de dados e o modelo de banco de dados.
- **Engenharia Direta (Forward Engineering):** Usar as informações recuperadas para reconstituir o sistema, muitas vezes adicionando novas funções e melhorando o desempenho.

A definição de Ian Sommerville é precisa: reengenharia é a reorganização e modificação de sistemas de software existentes, parcial ou totalmente, para torná-los mais manuteníveis.

Em resumo, a principal diferença é a **escala e o impacto**:

- **Refatoração:** Pequenas e frequentes alterações na estrutura interna que **não mudam o comportamento externo**.
- **Reengenharia:** Uma grande e custosa iniciativa para **reestruturar fundamentalmente um sistema**, que pode incluir a adição de novas funcionalidades e a migração para novas tecnologias.

## Considerações Finais

A refatoração se estabelece como uma disciplina indispensável na engenharia de software contemporânea. Longe de ser um luxo ou uma atividade secundária, ela é um pilar para a construção de software sustentável, manutenível e de alta qualidade. Ao focar na melhoria contínua da estrutura interna do código sem alterar seu comportamento externo, a refatoração permite que os sistemas evoluam de forma saudável, evitando o acúmulo de dívida técnica que, inevitavelmente, compromete a agilidade e a capacidade de inovação.

A prática disciplinada da refatoração, apoiada por uma suíte robusta de testes automatizados, capacita as equipes a responderem às mudanças com confiança, tornando o código mais legível, simples e flexível. A distinção clara entre refatoração e reengenharia também é fundamental, permitindo que as equipes escolham a abordagem correta — seja uma série de pequenas melhorias contínuas ou uma grande reestruturação — com base na saúde atual do sistema e nos objetivos de negócio.

Em última análise, incorporar a refatoração no fluxo de trabalho diário não é apenas sobre escrever um código melhor; é sobre adotar uma mentalidade de profissionalismo e cuidado, garantindo que o software construído hoje permaneça um ativo valioso e adaptável para o futuro.
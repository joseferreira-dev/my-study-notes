## Capítulo 5 –  Modelo Iterativo e Incremental: Rapid Application Development (RAD)

Desde a consolidação dos modelos iterativo e incremental, os profissionais de software buscaram maneiras de acelerar ainda mais o ciclo de entrega, reduzindo custos e capturando cedo o feedback dos usuários. Foi nesse contexto que surgiu o **Rapid Application Development (RAD)**, um modelo adaptado aos anseios dos projetos com prazos curtos (tipicamente de **60 a 90 dias**), em que o grande diferencial é a ênfase no **reuso intensivo de componentes** já testados e na **interação constante com o usuário**.

### Fases Fundamentais do RAD

Embora várias equipes chamem essas etapas por nomes ligeiramente diferentes, o RAD tradicionalmente se estrutura em cinco fases principais, que podem ser agrupadas em dois grandes blocos — “Modelagem” e “Construção” — de acordo com a realidade de cada projeto:

1. **Modelagem de Negócio**: Nesta etapa, o foco recai sobre o entendimento do fluxo de informações que dá suporte às funções de negócio. Perguntamo-nos: quem gera cada dado, como ele transita pelas áreas da organização, onde é armazenado e quem o consome? O objetivo é criar um **mapa vivo** dos processos, mostrando sucintamente cada ator, cada documento e cada movimento de informação. **Exemplo prático**: ao desenvolver um sistema de pedidos para um restaurante, modelamos como o garçom registra o pedido, como o terminal da cozinha recebe essa informação, como é feito o controle de estoque de ingredientes e como o caixa processa o pagamento.
2. **Modelagem de Dados**: Tendo em mãos o fluxo de informação, refinamos cada elemento em **objetos de dados** — tabelas, entidades ou estruturas de informação que suportarão as operações. Identificamos atributos (por exemplo, “nome do cliente”, “quantidade”, “data do pedido”) e definimos relacionamentos (“um cliente pode ter vários pedidos”, “cada pedido possui vários itens”).
3. **Modelagem de Processo**: Com os objetos de dados bem estabelecidos, descrevemos os processos operacionais que transformarão esses dados em ações. Aqui, criamos fluxos detalhados de como a aplicação deverá **incluir**, **alterar**, **descartar** ou **recuperar** cada objeto, definindo as regras de negócio (validações, cálculos, notificações). **Exemplo prático**: no mesmo sistema de restaurante, desenha-se o passo a passo de “registrar pagamento”: verificar métodos aceitos, calcular troco, emitir recibo e atualizar o relatório de caixa.
4. **Geração da Aplicação**:  A partir dos modelos, adotam-se ferramentas de **quarta geração** (4GL), geradores de telas, wizards e bibliotecas prontas para montar rapidamente telas, relatórios e regras de negócio. O time reutiliza componentes testados — seja um controle de grade de dados, seja um conector de banco de dados — e cria, quando necessário, novos módulos reusáveis. O objetivo é escrever o mínimo de código manual possível.
5. **Teste e Modificação**: Como o RAD baseia-se fortemente no reúso, muitos módulos já possuem histórico de testes. Ainda assim, todos os novos componentes e interfaces são validados exaustivamente, simulando cenários reais de uso. Bugs são corrigidos e ajustes são incorporados em tempo real, garantindo que, ao final do ciclo curto, a aplicação esteja pronta para produção.

Na prática, equipes mais maduras costumam condensar “Modelagem de Negócio”, “Modelagem de Dados” e “Modelagem de Processo” em uma única etapa de **Modelagem**, e “Geração da Aplicação” com “Teste e Modificação” em **Construção**, permitindo sobreposição de atividades e compartilhamento de equipes multidisciplinares.

<div align="center">
  <img width="680px" src="./img/05-rad.png">
</div>

### Interação com o Usuário e Ferramentas de Apoio

Um dos pilares do RAD é a **proximidade direta** com os usuários finais. Workshops, protótipos de tela e pequenas demonstrações diárias garantem que o software evolua em sintonia com o que o cliente realmente precisa. Para viabilizar ciclos de 60–90 dias, a equipe utiliza:

- **Geradores de tela**: arrastar e soltar campos, tabelas e botões;
- **Criadores de relatórios**: definir colunas, filtros e layouts visualmente;
- **Ambientes visuais de modelagem**: diagramas que se transformam em parte do banco de dados automaticamente;
- **Bibliotecas de componente**: trechos de código prontos para login, exportação de dados, gráficos, etc.

Esse conjunto de ferramentas reduz o esforço manual, permitindo que o time se concentre no que realmente agrega valor: a **lógica de negócio** e a **experiência do usuário**.

### Cenários Recomendados e Quando Evitar

O RAD não serve a todo tipo de projeto. Ele brilha em cenários específicos, em que:

- **Não há dependência de software auxiliar complexo** (é aplicação standalone).
- **Existem classes ou componentes pré-existentes** que podem ser facilmente reaproveitados.
- **A performance não é o fator crítico** — a prioridade é a entrega rápida.
- **O risco técnico é baixo** — as tecnologias envolvidas já são conhecidas pela equipe.
- **O mercado-alvo é restrito** — o sistema não exige deploy massivo imediato.
- **O escopo permite divisão em módulos claros**, todos com requisitos bem delimitados.
- **As mudanças tecnológicas não são iminentes**, garantindo estabilidade da plataforma.

Por suas características, o RAD não é indicado para:

- **Sistemas críticos de alta performance ou de missão crítica** (ex.: controle de tráfego aéreo, equipamentos médicos);
- **Projetos com escopo extremamente amplo ou distribuído em múltiplos sites**;
- **Ambientes em que mudanças tecnológicas podem ocorrer durante o desenvolvimento**;
- **Produtos que precisam de padrões rígidos** de segurança, auditoria ou certificação formal;
- **Situações em que o contrato ou a governança exijam documentação pesada** antes de qualquer codificação.

### Vantagens e Desvantagens

Graças ao reúso extremo e ao ciclo encurtado, o RAD oferece:

- **Entregas e protótipos rápidos**, permitindo validar cedo as ideias;
- **Reuso intensivo de componentes**, aumentando a produtividade e reduzindo riscos de regressão;
- **Desenvolvimento em alto nível de abstração**, afastando o desenvolvedor de detalhes de infraestrutura;
- **Redução drástica de codificação manual**, graças a wizards e geradores;
- **Possibilidade de equipes especializadas** cuidarem de diferentes funções simultaneamente;
- **Flexibilidade para reprojetos rápidos**, ajustando telas, regras e relatórios “on the fly”;
- **Economia de tempo (e, portanto, de custo)**, uma vez que o tempo de desenvolvimento é o principal insumo;
- **Maior envolvimento do usuário**, que participa ativamente das decisões durante todo o ciclo.

Por outro lado, o RAD impõe demandas e riscos:

- **Recursos caros e experientes**: é necessário ter profissionais capacitados nas ferramentas 4GL e em modelagem ágil.
- **Padronização de interface**: sem governança visual, módulos gerados por equipes distintas podem apresentar aparências diferentes.
- **Comprometimento intenso** da equipe e do usuário: ambos devem dedicar tempo integral às sessões de modelagem e testes.
- **Custo elevado de ferramentas e hardware**: licenças de 4GL, geradores e servidores robustos podem pesar no orçamento.
- **Dificuldade de monitoramento de progresso**: a velocidade e paralelismo implicam menos artefatos formais, tornando o acompanhamento mais subjetivo.
- **Perda de rigor científico**: sem métodos formais, é fácil deixar de documentar regras críticas ou requisitos não-funcionais.
- **Risco de “caos controlado”**: sem disciplina, o RAD pode degenerar em codificação desorganizada, gerando retrabalho futuro.
- **Possível construção de funcionalidades desnecessárias**, se o usuário não souber priorizar bem.
- **Conflitos entre desenvolvedores e clientes**, caso expectativas não sejam alinhadas claramente desde o início.

### Considerações Finais

O **Rapid Application Development** representa uma evolução natural dos modelos iterativo e incremental, focada na **agilidade**, no **reúso** e no **feedback contínuo**. Quando aplicado com disciplina e nas situações corretas, o RAD encurta drasticamente o time-to-market e melhora o alinhamento entre equipe e cliente. Entretanto, requer profissionais experientes, ferramentas robustas e um controle equilibrado entre velocidade e qualidade.

No próximo capítulo, avançaremos para os **Modelos Evolutivos** e as **Metodologias Ágeis** que retomam muitos dos princípios do RAD, mas com ênfases diferentes em governança, frameworks e ciclos de entrega contínua (como RUP, Scrum e XP).

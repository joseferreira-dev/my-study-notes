# Capítulo 1 – Fundamentos

Toda a jornada no universo da Tecnologia da Informação, desde a mais simples aplicação até os mais complexos sistemas de inteligência artificial, gira em torno de um elemento central: a manipulação e a análise de dados. Compreender o que são os dados, suas diferentes formas, naturezas e restrições é o alicerce sobre o qual todo o conhecimento em bancos de dados será construído. É o ponto de partida essencial para entendermos como organizar, armazenar e, principalmente, extrair valor das informações que moldam o mundo digital.

## O que são Dados?

No nível mais fundamental, **dados** são uma coleção de valores discretos ou contínuos que, em seu estado bruto, representam fatos, medições, observações ou descrições de algo. Podem ser números, textos, símbolos ou imagens que, isoladamente, podem não ter um significado completo. Por exemplo, o número `37` é um dado. A palavra `Recife` é um dado. A data `26/08/2025` é um dado.

Um conjunto de dados é tecnicamente chamado de _data_, enquanto uma unidade única de dado é referida como _datum_. Contudo, na prática cotidiana e na literatura técnica, o termo "dados" é universalmente utilizado tanto para o singular quanto para o plural.

É crucial, no entanto, distinguir **Dado** de **Informação**. A informação é o resultado do processamento, organização e contextualização dos dados, conferindo-lhes relevância e propósito.

- **Dado:** `37`
- **Informação:** A temperatura registrada em Recife hoje é de `37` graus Celsius.

Neste exemplo, o dado `37` foi contextualizado (temperatura, local, unidade de medida) para se tornar uma informação útil. O principal objetivo de um banco de dados é justamente organizar dados brutos de forma que possam ser facilmente transformados em informações valiosas.

## As Múltiplas Faces dos Dados: Formas de Classificação

Para que possamos trabalhar com os dados de maneira eficaz, precisamos primeiro classificá-los. Existem diversas formas de categorizar os dados, mas três delas são essenciais para o nosso estudo: a classificação quanto à sua **natureza** (o tipo de valor que representam), quanto à sua **estrutura** (a forma como são organizados) e quanto ao seu **nível de acesso** (as restrições de segurança impostas a eles).

### Classificação Quanto à Natureza (Tipo)

A primeira e mais comum forma de classificação diz respeito ao tipo de informação que o dado carrega. Aqui, a divisão se dá em duas grandes categorias: dados qualitativos e dados quantitativos.

- **Dados Qualitativos:** Como o nome sugere, são dados que descrevem uma **qualidade**, uma característica ou um atributo de algo. Eles não são medidos numericamente, mas sim categorizados. Respondem a perguntas como "de que tipo?" ou "qual a categoria?". Eles se subdividem em:
    - **Nominal:** Representa uma categoria que não possui uma ordem ou hierarquia intrínseca. Os valores são apenas rótulos. Exemplos incluem:
        - Cor de um carro (Vermelho, Azul, Prata)
        - Gênero (Masculino, Feminino, Não-binário)
        - Nacionalidade (Brasileiro, Argentino, Alemão)
        - Tipo sanguíneo (A+, O-, AB+)
    - **Ordinal:** Também representa uma categoria, mas, diferentemente do nominal, existe uma **ordem ou uma hierarquia lógica** entre os valores.
        - Nível de escolaridade (Ensino Fundamental, Ensino Médio, Ensino Superior)
        - Grau de satisfação de um cliente (Muito Insatisfeito, Insatisfeito, Neutro, Satisfeito, Muito Satisfeito)
        - Tamanho de uma peça de roupa (P, M, G, GG)
        - Classificação em uma competição (1º lugar, 2º lugar, 3º lugar)
- **Dados Quantitativos:** São dados que representam uma **quantidade** ou uma medida numérica. São o resultado de uma contagem ou de uma medição. Respondem a perguntas como "quanto?" ou "quantos?". Eles também se subdividem em:
    - **Discreto:** É um valor numérico que resulta de uma contagem, sendo, portanto, um número inteiro e finito. Não é possível ter "meio" valor.
        - Quantidade de alunos em um curso (ex: 45 alunos)
        - Número de páginas de um livro (ex: 320 páginas)
        - Total de produtos em estoque (ex: 1.500 unidades)
    - **Contínuo:** É um valor numérico que resulta de uma medição, podendo assumir qualquer valor dentro de um intervalo. Geralmente são representados por números decimais e podem ser infinitamente divididos.
        - Altura de uma pessoa (ex: 1.75m)
        - Velocidade de um veículo (ex: 82.5 km/h)
        - Temperatura de um ambiente (ex: 23.7 °C)
        - Peso de um objeto (ex: 4.68 kg)

A imagem a seguir ilustra essa hierarquia de classificação.

<div align="center">
<img width="700px" src="./img/01-dados-subdivisoes.png">
</div>

### Classificação Quanto ao Nível de Acesso

Em qualquer organização, nem todos os dados podem ser acessados por todas as pessoas. A segurança e a confidencialidade da informação são cruciais, e por isso os dados são classificados em níveis de restrição. A abordagem mais comum divide os dados em quatro categorias, formando uma escala crescente de sensibilidade.

- **Dados Públicos:** São dados sem qualquer tipo de restrição de confidencialidade. Podem e, muitas vezes, devem ser acessados por qualquer pessoa, dentro ou fora da organização. O vazamento deste tipo de dado não causa nenhum dano.
    - **Exemplos:** Endereços de filiais da empresa, notícias publicadas no site institucional, informações de produtos em um catálogo online, dados do censo do IBGE.
- **Dados Internos (ou de Uso Interno):** Possuem um baixo nível de confidencialidade. São dados que pertencem à organização e não devem ser compartilhados externamente, mas cujo vazamento acidental não representaria um grande impacto estratégico, financeiro ou legal. O acesso é geralmente permitido a todos os colaboradores da empresa.
    - **Exemplos:** Comunicados internos, lista de ramais, políticas de vestimenta, manuais de procedimentos gerais.
- **Dados Restritos:** Apresentam um nível intermediário de proteção. São informações consideradas importantes ou sensíveis, cujo acesso deve ser limitado a grupos específicos de pessoas ou departamentos que necessitam delas para realizar seu trabalho ("need-to-know basis"). Um vazamento poderia causar prejuízos financeiros ou de reputação.
    - **Exemplos:** Folha de pagamento, planejamento estratégico da empresa, informações de contato de clientes, relatórios de vendas.
- **Dados Confidenciais:** Representam o nível mais alto de proteção. São os ativos de informação mais críticos de uma organização. O acesso a esses dados é extremamente controlado e limitado a um número mínimo de pessoas autorizadas. Seu vazamento poderia comprometer gravemente a entidade, resultando em severos danos financeiros, legais, de reputação ou operacionais.
    - **Exemplos:** Segredos industriais (a fórmula de um produto), dados de cartão de crédito de clientes, prontuários médicos de pacientes, informações de segurança nacional.

<div align="center">
<img width="700px" src="./img/01-dados-niveis-de-restricao.png">
</div>

### Classificação Quanto à Estrutura

Finalmente, podemos classificar os dados pela forma como são organizados e preparados para o processamento por sistemas computacionais.

- **Dados Estruturados:** São dados altamente organizados, que seguem um formato e um modelo de dados rígidos e bem definidos. Eles se encaixam perfeitamente em tabelas, com linhas e colunas, onde cada coluna tem um tipo de dado específico (número, texto, data, etc.) e cada linha representa um registro. São fáceis de armazenar, pesquisar e analisar.
    - **Exemplos:** Uma planilha do Excel, uma tabela de clientes em um banco de dados, informações de um sistema de ponto de funcionários.
- **Dados Não Estruturados:** São o oposto. Não possuem um modelo de dados predefinido ou uma estrutura organizacional fixa. Representam a grande maioria dos dados gerados no mundo hoje. São mais difíceis de processar e analisar por meios tradicionais.
    - **Exemplos:** O corpo de um e-mail, o conteúdo de um arquivo PDF ou de um documento do Word, uma imagem, um arquivo de áudio ou vídeo, uma postagem em uma rede social.

Existe ainda uma categoria intermediária, a de **dados semiestruturados**, que não se encaixam em tabelas rígidas, mas contêm tags ou marcadores para separar elementos semânticos e impor hierarquias. Exemplos clássicos são arquivos **JSON** e **XML**.

Como veremos ao longo desta apostila, os **dados estruturados** são o domínio principal dos bancos de dados relacionais (como MySQL, PostgreSQL), enquanto os **dados não estruturados** e **semiestruturados** são frequentemente gerenciados por bancos de dados NoSQL (como MongoDB, Cassandra).

## Dados Abertos: Transparência e Cidadania

Até agora, classificamos os dados por sua natureza, estrutura e nível de acesso dentro de uma organização. No entanto, existe uma categoria de dados cuja importância reside justamente na quebra de barreiras de acesso: os **Dados Abertos**. Este é um movimento global que defende que certos dados devem estar livremente disponíveis para que todos possam acessá-los, reutilizá-los e redistribuí-los, sem restrições.

A definição formal, consolidada pela Open Knowledge Foundation, estabelece que:

> Os dados são considerados abertos quando qualquer pessoa pode **acessar, usar, modificar e compartilhar livremente** para qualquer finalidade (sujeito a, no máximo, a requisitos que preservem a proveniência e a sua abertura).

Para que essa definição seja plenamente satisfeita, duas condições são essenciais: a publicação dos dados em **formato aberto** e sob uma **licença aberta**. Um formato aberto (como CSV, JSON, XML) garante que ninguém seja impedido de acessar os dados por precisar de um software específico ou proprietário. Uma licença aberta, por sua vez, concede permissão legal explícita para a sua livre reutilização.

No contexto governamental, a prática de Dados Abertos transforma-se em uma poderosa ferramenta para a democracia. Trata-se de uma metodologia para a publicação de dados do governo em formatos reutilizáveis, com o objetivo primordial de aumentar a transparência, fortalecer o controle social e fomentar uma maior participação política por parte dos cidadãos. No Brasil, diversos órgãos da Administração Pública já disponibilizam seus dados na web, não apenas na forma de relatórios estáticos, mas como conjuntos de dados brutos que podem ser processados e analisados. Isso permite que qualquer cidadão, jornalista, pesquisador ou empresa possa acompanhar os resultados das ações de governo de forma muito mais profunda.

Os efeitos dos dados abertos governamentais sobre as políticas públicas são vastos e podem ser compreendidos a partir de três pilares:

- **Inclusão:** Fornecer dados em formatos padronizados, abertos e acessíveis permite que qualquer cidadão, independentemente de seus recursos, possa utilizá-los. Um desenvolvedor pode criar um aplicativo que monitora os gastos com educação em seu município; um jornalista pode cruzar dados de saúde para investigar a eficácia de uma campanha de vacinação. A informação deixa de ser um privilégio.
- **Transparência:** A disponibilização de informações do setor público de forma aberta e acessível eleva o nível de transparência a um novo patamar. Não se trata mais apenas de publicar um resumo das contas públicas, mas de fornecer os dados detalhados que permitem a qualquer interessado auditar e verificar as informações por conta própria, usando as ferramentas que julgar mais adequadas.
- **Responsabilidade (Accountability):** Com acesso aos dados corretos, a sociedade pode construir diferentes visões sobre o desempenho do governo. É possível verificar se as metas de uma política pública foram cumpridas, comparar o desempenho de diferentes municípios em uma mesma área e, consequentemente, cobrar de forma mais eficaz a responsabilidade dos gestores públicos.

### A Política de Dados Abertos no Brasil

No âmbito federal, a gestão da Política de Dados Abertos é uma atribuição coordenada pela **Controladoria-Geral da União (CGU)**, por meio da **Infraestrutura Nacional de Dados Abertos (INDA)**, que é o portal centralizado para a busca e o acesso a esses dados.

A política foi instituída formalmente pelo **Decreto nº 8.777, de 11 de maio de 2016**. Este decreto estabelece as diretrizes para a disponibilização de dados abertos pelos órgãos e entidades da administração pública federal direta, autárquica e fundacional. Recomenda-se a leitura do decreto na íntegra, mas alguns pontos merecem destaque especial em nosso estudo.

O primeiro são os **objetivos** da política, que incluem reforçar a cultura da transparência, franquear o acesso aos dados para a sociedade, fomentar o controle social e promover o compartilhamento de recursos de tecnologia, buscando maior eficiência governamental.

O segundo ponto, e talvez o mais importante para o nosso contexto, são as **definições** que o decreto estabelece, criando um vocabulário comum e preciso para o tema.

|Conceito|Descrição|
|---|---|
|**Dado**|Sequência de símbolos ou valores, representados em qualquer meio, produzidos como resultado de um processo natural ou artificial.|
|**Dado Acessível ao Público**|Qualquer dado gerado ou acumulado pelo Governo que não esteja sob sigilo ou sob restrição de acesso nos termos da Lei nº 12.527, de 18 de novembro de 2011 (Lei de Acesso à Informação).|
|**Dados Abertos**|Dados acessíveis ao público, representados em meio digital, estruturados em **formato aberto**, **processáveis por máquina**, referenciados na internet e disponibilizados sob **licença aberta** que permita sua livre utilização, consumo ou cruzamento, limitando-se a creditar a autoria ou a fonte.|
|**Formato Aberto**|Formato de arquivo não proprietário, cuja especificação esteja documentada publicamente e seja de livre conhecimento e implementação, livre de patentes ou qualquer outra restrição legal quanto à sua utilização.|
|**Plano de Dados Abertos (PDA)**|Documento orientador para as ações de implementação e promoção de abertura de dados de cada órgão ou entidade da administração pública federal, obedecidos os padrões mínimos de qualidade, de forma a facilitar o entendimento e a reutilização das informações.|

### Os 8 Princípios dos Dados Abertos

Para guiar a implementação de políticas de dados abertos ao redor do mundo, um conjunto de princípios foi estabelecido. A lista a seguir, baseada na Open Government Data (OGD), detalha essas diretrizes fundamentais.

| Princípio                         | Descrição                                                                                                                                                                                                |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Completude**                 | Todos os dados públicos devem ser tornados disponíveis. Dados públicos são aqueles que não estão sujeitos a limitações de privacidade, segurança ou sigilo legal.                                        |
| **2. Primariedade**               | Os dados devem ser disponibilizados em sua forma bruta, como foram coletados na fonte, com o maior nível de detalhe (granularidade) possível, sem agregações ou modificações que possam ocultar nuances. |
| **3. Atualidade**                 | Os dados devem ser disponibilizados o mais rápido possível após sua coleta ou geração, garantindo que mantenham sua relevância e valor.                                                                  |
| **4. Acessibilidade**             | Os dados devem ser facilmente encontráveis e acessíveis para o maior número possível de usuários e para os mais variados propósitos.                                                                     |
| **5. Processáveis por Máquinas**  | Os dados devem ser estruturados de forma a permitir que computadores possam processá-los e analisá-los de forma automatizada (ex: em planilhas ou APIs, e não em imagens de tabelas ou PDFs).            |
| **6. Acesso Não Discriminatório** | Os dados devem estar disponíveis a todos, sem a necessidade de cadastro, identificação ou justificativa de uso.                                                                                          |
| **7. Formatos Não Proprietários** | Os dados devem ser disponibilizados em formatos sobre os quais nenhuma entidade tenha controle exclusivo, evitando a dependência de softwares específicos e caros.                                       |
| **8. Livres de Licenças**         | Os dados não devem estar sujeitos a direitos autorais, patentes ou outras restrições que impeçam sua reutilização. A única exigência aceitável é a de creditar a fonte original.                         |

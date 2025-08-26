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


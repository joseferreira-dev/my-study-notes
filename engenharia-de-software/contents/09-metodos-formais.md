# Capítulo 9 – Modelos Específicos: Métodos Formais

Os **métodos formais** representam uma abordagem rigorosa no desenvolvimento de software, baseada em fundamentos matemáticos. Seu principal objetivo é aumentar a confiabilidade e a correção dos sistemas, por meio da especificação, desenvolvimento e verificação formal de seus componentes. Diferentemente de abordagens mais empíricas, os métodos formais permitem demonstrar, por meio de provas matemáticas, que um sistema atende rigorosamente aos requisitos definidos.

O uso de métodos formais é especialmente recomendado em sistemas críticos, nos quais falhas podem gerar consequências graves, como em aplicações aeroespaciais, médicas, financeiras e de controle industrial. Embora exijam maior esforço inicial e conhecimento técnico especializado, os métodos formais proporcionam um nível de precisão e confiança inalcançável com técnicas tradicionais.

## O que são Métodos Formais?

Os métodos formais são técnicas baseadas em **matemática aplicada à engenharia de software**. A proposta é representar os requisitos, o comportamento e as restrições de um sistema por meio de **modelos matemáticos formais**, os quais podem ser **analisados, refinados e verificados rigorosamente** ao longo do processo de desenvolvimento.

Em outras palavras, um método formal utiliza lógica matemática (como lógica de predicados, álgebra relacional e teoria dos conjuntos) para:

- Especificar requisitos e funcionalidades do sistema;
- Verificar, por meio de provas formais, se o sistema satisfaz as propriedades desejadas;
- Auxiliar na implementação correta por meio de refinamento formal;
- Identificar e eliminar ambiguidade, inconsistência e incompletude nos requisitos.

Segundo Ian Sommerville ,“o ponto de partida dos métodos formais é um modelo formal construído a partir dos requisitos do usuário. A partir desse modelo, é possível provar que o programa é consistente com os requisitos definidos. Isso elimina erros de implementação por falha de interpretação dos requisitos". Roger Pressman reforça ao afirmar que “os métodos formais fornecem uma base sólida para eliminar ambiguidade, incompletude e inconsistência. Através da matemática, é possível estabelecer a correção do sistema antes mesmo da implementação”.

Entre os principais conceitos associados aos métodos formais, destacam-se:

- **Especificação formal**: descrição matemática do comportamento do sistema.
- **Modelagem formal**: representação abstrata dos dados e operações do sistema.
- **Prova formal**: demonstração lógica de que o sistema satisfaz suas propriedades.
- **Refinamento**: processo de transformação de um modelo abstrato em um modelo mais concreto, mantendo as propriedades previamente definidas.

Ferramentas como Z, B, VDM, Alloy e TLA+ são amplamente utilizadas para trabalhar com essas representações formais.

### Exemplo ilustrativo

Imagine um sistema bancário onde a operação de saque só deve ser autorizada se o saldo da conta for suficiente. Em linguagem natural, poderíamos escrever:

> "O cliente pode sacar se o saldo for maior ou igual ao valor solicitado."

Essa frase pode ser interpretada de formas diferentes dependendo do contexto. Já em notação formal, a regra pode ser expressa assim:

$pré−condição:saldo≥valors​aque$
$pós−condição:saldon​ovo=saldo−valors​aque$

Esse modelo elimina a ambiguidade, sendo possível **verificar matematicamente** se essa regra está sendo respeitada no projeto e na implementação do sistema.

## Quando utilizar Métodos Formais?

Os métodos formais são mais adequados para sistemas que exigem **confiabilidade extrema**, onde falhas podem causar **danos irreparáveis**. Nesses contextos, erros não são aceitáveis e a **verificação matemática da corretude do software** torna-se crucial. Exemplos de aplicação incluem:

- Sistemas de controle de tráfego aéreo;
- Sistemas de controle de reatores nucleares;
- Sistemas de suporte à vida em hospitais;
- Sistemas embarcados em satélites e espaçonaves;
- Plataformas bancárias de alta segurança;
- Aplicações militares e de defesa.

Apesar de seu valor, os métodos formais **não são largamente adotados na indústria de software**. A principal razão para isso está no **custo elevado**, na **demanda por especialistas em lógica matemática** e na **complexidade das ferramentas formais**. No entanto, quando bem aplicados, os métodos formais podem **reduzir significativamente o número de defeitos** e **aumentar a confiabilidade do sistema**, sem necessariamente aumentar o custo no longo prazo.

## Etapas do Desenvolvimento com Métodos Formais

O desenvolvimento de software baseado em métodos formais é estruturado em cinco etapas principais, que buscam aplicar rigor matemático desde a definição dos requisitos até a verificação final do sistema. A seguir, descrevemos essas etapas em detalhes.

### Especificação Formal para Definição de Requisitos

Esta etapa busca formalizar os requisitos levantados junto aos stakeholders, eliminando ambiguidade e inconsistência. A descrição é feita com o uso de linguagens formais, como Z ou B, permitindo que requisitos funcionais e não funcionais sejam expressos com precisão.

Por exemplo, uma regra como "um usuário não pode sacar um valor superior ao saldo" pode ser representada formalmente por:

$$∀ u ∈ Usuários, v ∈ Valores: sacar(u, v) ⇒ v ≤ saldo(u)$$

Essa especificação pode ser verificada automaticamente, ajudando a evitar erros desde o início do desenvolvimento.

### Refinamento para Concepção de Projeto

Com os requisitos formalizados, inicia-se a etapa de **refinamento**, que visa transformar o modelo abstrato em uma concepção mais próxima da implementação. São tomadas decisões sobre a arquitetura, componentes, dados e suas relações.

O refinamento pode ocorrer em múltiplos níveis, passando de modelos matemáticos abstratos até modelos orientados a objetos ou estruturados, sempre mantendo as propriedades formais garantidas na etapa anterior.

Por exemplo, a estrutura genérica de "armazenamento de dados de clientes" pode ser refinada para o uso de uma tabela hash indexada por CPF.

### Síntese para Implementação

Com o modelo refinado em mãos, é possível iniciar a **síntese de código**, gerando estruturas iniciais (esqueleto) da implementação, como funções, classes e interfaces. Essa tradução pode ser feita manualmente, com apoio da especificação formal, ou automaticamente por ferramentas específicas.

A correspondência entre o código e a especificação é essencial para garantir que o comportamento do sistema siga fielmente as propriedades definidas.

A operação formal `sacar(valor, saldo)` exemplificada anteriormente pode ser implementada em pseudocódigo como:

```
def sacar(valor, saldo):
    if valor <= saldo:
        return saldo - valor
    else:
        raise Exception("Saldo insuficiente")
```

### Prototipação para Validação

Mesmo com base formal, é necessário validar com os usuários se o sistema corresponde às suas expectativas. A **prototipação funcional** permite criar uma versão navegável do sistema para testes com usuários finais.

Essa fase é importante para identificar problemas de usabilidade, de comunicação ou de interpretação de requisitos que podem ter passado despercebidos nas etapas anteriores.

Como exemplo, tomando-se o desenvolvimento de um sistema de reserva de passagens, um protótipo pode simular a seleção de horários, assentos e formas de pagamento, permitindo validar se o fluxo atende ao esperado pelo cliente.

### Prova para a Verificação

A última etapa é a **prova formal da correção**. Através de deduções lógicas, busca-se garantir que o sistema atende integralmente aos requisitos definidos.

A prova pode ser feita manualmente ou com o auxílio de ferramentas como Coq, Isabelle, PVS ou TLA+. Essa verificação garante que:

- Todas as propriedades invariantes são mantidas.
- Todas as pré-condições e pós-condições são respeitadas.
- O sistema não entra em estados inválidos.

Tendo como exemplo o caso de um protocolo de comunicação, pode-se demonstrar que ele nunca entra em deadlock e sempre responde dentro do tempo máximo permitido.

## Vantagens e Desvantagens dos Métodos Formais

O uso de métodos formais traz benefícios significativos, mas também apresenta desafios. Abaixo, apresentamos um panorama geral. Primeiro, com relação as vantagens:

- **Precisão e rigor**: todas as propriedades do sistema são definidas com base em lógica formal.
- **Detecção precoce de erros**: inconsistências nos requisitos podem ser descobertas ainda na fase de especificação.
- **Verificabilidade**: o sistema pode ser comprovado matematicamente antes mesmo de sua implementação.
- **Redução de custos com manutenção**: menos erros em produção significam menos correções futuras.
- **Alta confiabilidade**: sistemas formais apresentam taxas mínimas de falhas em operação.
- **Documentação precisa**: especificações formais funcionam como documentação exata do sistema.

Já as desvantagens e limitações:

- **Alto custo inicial**: o desenvolvimento formal é mais demorado e requer especialistas.
- **Curva de aprendizado íngreme**: poucos profissionais dominam lógica formal e notações matemáticas.
- **Dificuldade de comunicação com o cliente**: especificações formais não são facilmente compreendidas por pessoas não técnicas.
- **Ferramentas complexas**: muitas linguagens formais possuem sintaxes difíceis e interfaces pouco amigáveis.
- **Resistência do mercado**: equipes pouco familiarizadas podem oferecer resistência à adoção.

## Ferramentas de Apoio

Diversas ferramentas auxiliam a aplicação de métodos formais no desenvolvimento de software. Algumas das mais conhecidas são:

- **Z/Eves**: ambiente para especificação e verificação com a linguagem Z.
- **B-Toolkit**: ferramenta para o método B, com suporte ao refinamento e geração de código.
- **Alloy**: linguagem e ferramenta para modelagem e verificação de modelos com base em lógica relacional.
- **Coq**: assistente de prova baseado em cálculo de construções indutivas.
- **Isabelle/HOL**: ambiente para prova interativa, com suporte a lógica de ordem superior.
- **TLA+**: linguagem para modelagem de sistemas concorrentes e distribuídos.

Essas ferramentas oferecem suporte à escrita de especificações, realização de provas formais, geração de código, simulações e validação de propriedades.

## Método Cleanroom: Um Exemplo de Aplicação

O **método Cleanroom** é um exemplo notável de abordagem baseada em métodos formais. Inspirado na ideia de "sala limpa" da engenharia de hardware, o Cleanroom propõe o desenvolvimento de software com **altíssimo controle de qualidade** desde o início. Nele:

- Não se utiliza testes unitários tradicionais;
- Todos os módulos são verificados formalmente durante o desenvolvimento;
- O código é testado somente ao final, com base em **amostragem estatística**;
- O foco está na **prevenção de defeitos**, não na sua correção posterior.

Essa abordagem já foi aplicada com sucesso em sistemas críticos, reduzindo a taxa de defeitos em até 90%.

## Considerações Finais

Os métodos formais constituem uma abordagem altamente especializada e rigorosa dentro da engenharia de software. Seu uso é mais apropriado em contextos nos quais **erros não são toleráveis**, como sistemas críticos, aeroespaciais, médicos ou financeiros.

Apesar das barreiras relacionadas ao custo, complexidade e especialização, seu principal diferencial é a **capacidade de fornecer garantias matemáticas de correção**, o que representa um enorme avanço em termos de confiabilidade e qualidade de software.

A aplicação de métodos formais não precisa ser total. Muitas organizações os utilizam de forma **parcial ou seletiva**, aplicando-os somente às partes mais críticas do sistema. Essa estratégia híbrida permite aliar o rigor formal à flexibilidade de métodos mais tradicionais.

Concluímos, portanto, que os métodos formais não substituem os modelos convencionais, mas os complementam. São ferramentas poderosas que, quando bem aplicadas, contribuem significativamente para o **aperfeiçoamento da engenharia de software como disciplina técnica e científica**.

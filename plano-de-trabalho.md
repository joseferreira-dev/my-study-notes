# Plano de Trabalho

**Título Provisório:** Estimativa de Séries Históricas Meteorológicas para Localidades sem Estações no Brasil Utilizando Machine Learning e Dados da NASA POWER e INMET

**Foco:** Aplicações para agricultura de precisão e agricultura familiar.

## Contextualização

A agricultura brasileira, um dos pilares da economia global, opera em um cenário de alta dependência das condições climáticas. Decisões críticas, como a escolha do momento ideal para o plantio, a aplicação de fertilizantes, o manejo de irrigação e o controle de pragas e doenças, são diretamente influenciadas por variáveis meteorológicas. Contudo, o Brasil enfrenta um desafio estrutural: a rede de estações meteorológicas em solo, gerenciada principalmente pelo INMET, possui uma distribuição heterogênea, com maior densidade em centros urbanos e vazios de cobertura em vastas áreas rurais. Esta lacuna de dados representa um obstáculo significativo para a modernização do campo, limitando o potencial da agricultura de precisão e aumentando a vulnerabilidade de agricultores familiares a eventos climáticos extremos.

## Problema de Pesquisa

A ausência de dados meteorológicos locais confiáveis em grande parte do território rural brasileiro é um gargalo para o avanço da agricultura inteligente. Diante disso, a pesquisa busca responder à seguinte questão: Como é possível utilizar dados de satélite da NASA POWER, calibrados com dados de estações terrestres do INMET através de machine learning, para gerar séries históricas meteorológicas precisas e confiáveis para qualquer localidade rural do Brasil?

## Justificativa

A solução para o problema reside na fusão inteligente de duas fontes de dados complementares:

- **NASA POWER (Prediction of Worldwide Energy Resources):** Esta base de dados oferece uma cobertura global e contínua, com séries históricas que remontam a 1981. Seus dados são derivados de modelos de reanálise e satélites (como os do sistema MERRA-2), o que garante a disponibilidade de informação para qualquer coordenada. No entanto, sua resolução espacial é relativamente baixa (grades de aprox. 50x50 km), resultando em estimativas que podem não capturar as particularidades do microclima local.
- **INMET (Instituto Nacional de Meteorologia):** As estações automáticas e convencionais do INMET fornecem medições diretas e de alta acurácia. Sua limitação, contudo, é a cobertura espacial esparsa, tornando seus dados representativos apenas para as áreas próximas às estações.

A aplicação de **Machine Learning** é a ponte ideal para conectar essas duas fontes. Algoritmos de aprendizado supervisionado são capazes de modelar as complexas relações não-lineares entre as estimativas do satélite e as medições reais em solo. Ao treinar um modelo com dados de diversas estações, ele pode aprender os "fatores de correção" sistemáticos, que variam de acordo com o relevo, bioma e outras características geográficas. O desenvolvimento de tal sistema democratizaria o acesso à informação climática de qualidade, fornecendo a agricultores e gestores uma ferramenta poderosa para tomadas de decisão mais eficientes e sustentáveis.

## Objetivos

### Objetivo Geral

Desenvolver e validar um sistema baseado em machine learning capaz de estimar séries históricas de dados meteorológicos com alta acurácia para locais sem estações de medição no Brasil.

### Objetivos Específicos

- Realizar uma análise comparativa entre os dados da NASA POWER e as estações do INMET.
- Desenvolver modelos de machine learning para estimar os dados locais a partir dos dados de satélite.
- Avaliar a performance dos modelos, considerando diferentes agrupamentos geográficos (biomas, relevo).
- Criar um protótipo de plataforma para consulta e visualização das séries históricas estimadas.

## Metodologia

A abordagem metodológica será executada em quatro fases sequenciais:

1. **Fonte e Extração de Dados:** Serão coletadas séries históricas diárias das principais variáveis agroclimatológicas (temperatura mínima e máxima, precipitação, umidade relativa, velocidade do vento e radiação solar) da base de dados do INMET (via BDMEP) e da NASA POWER (via API). Para cada estação do INMET, será extraída a série correspondente do ponto de grade da NASA POWER mais próximo.
2. **Análise Comparativa e Agrupamento:** As séries de dados pareadas (INMET vs. NASA POWER) serão comparadas estatisticamente para quantificar a correlação, o viés e os erros, utilizando métricas como RMSE (Raiz do Erro Quadrático Médio), MAE (Erro Absoluto Médio) e R² (Coeficiente de Determinação). Com base em características geográficas (bioma, altitude, relevo, distância do oceano), as estações serão agrupadas em clusters climaticamente homogêneos, permitindo uma análise mais aprofundada das divergências.
3. **Desenvolvimento dos Modelos de Machine Learning:** Serão explorados algoritmos de regressão robustos, como **Random Forest**, **Gradient Boosting** e **Redes Neurais**. Os modelos serão treinados utilizando os dados da NASA POWER e atributos geográficos (latitude, longitude, altitude) como variáveis de entrada (_features_) para prever os dados medidos pelo INMET (alvo). Serão testadas duas estratégias: a criação de um modelo único para todo o Brasil e a criação de modelos especializados para cada cluster geográfico definido na fase anterior.
4. **Validação e Prototipagem:** Um conjunto de estações do INMET (aprox. 20-30%) será separado para o teste final (conjunto de validação), garantindo uma avaliação imparcial da performance do modelo em dados nunca vistos. O modelo com o melhor desempenho será a base para o desenvolvimento de um protótipo de plataforma web interativa, onde um usuário poderá selecionar qualquer ponto no mapa do Brasil para visualizar e baixar a série histórica meteorológica estimada para aquele local.
5. **Incluir aplicações práticas da solução proposta:**

## Plano de Trabalho

| Período (Mês) | Etapa                                                                                         | Atividades Principais                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| SET           | **Etapa 1: Fundamentação**<br><br>Incluir diagramas de arquitetura e comportamento do sistema | Revisão sistemática da literatura; Definição e delimitação do projeto; Escrita da qualificação.                   |
| SET           | **Etapa 2: Dados**                                                                            | Extração de dados via API/download; Limpeza, tratamento de dados faltantes e sincronização das séries.            |
| OUT           | **Etapa 3: Análise e Modelagem**                                                              | Análise estatística comparativa; Definição dos clusters; Treinamento, otimização e ajuste fino dos modelos de ML. |
| OUT/NOV       | **Etapa 4: Validação e Refinamento**                                                          | Validação dos modelos no conjunto de teste; Análise crítica dos resultados e erros; Refinamento da abordagem.     |
| NOV           | **Etapa 5: Desenvolvimento**                                                                  | Construção do protótipo da plataforma web de consulta e visualização de dados.                                    |
| NOV/DEZ       | **Etapa 6: Redação Final**                                                                    | Escrita da dissertação e Preparação para a defesa.                                                                |

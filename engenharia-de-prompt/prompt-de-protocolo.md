# Prompt para Geração de Protocolo de Pesquisa

**Persona e Objetivo:** Aja como um pesquisador sênior, especialista em metodologia de Revisão Sistemática da Literatura (RSL) e com profundo conhecimento das particularidades de sintaxe, limitações de caracteres e operadores das principais bases de dados acadêmicas (Scopus, ScienceDirect, Springer Link) e de tecnologia (IEEE Xplore, ACM Digital Library). Seu objetivo é, a partir de um tema de pesquisa detalhado pelo usuário, gerar um protocolo de pesquisa completo, uma lista estruturada de palavras-chave em inglês e português, e as _strings_ de busca otimizadas e segmentadas para cada base.

**Instruções:**

Você recebeu um resumo de projeto de um pesquisador júnior. Sua missão é converter essa ideia em um protocolo formal de RSL e, mais importante, criar a estratégia de busca completa, considerando as nuances de cada base de dados.

Com base no texto de entrada do usuário abaixo, gere o seguinte:

**[Inserir aqui o texto inicial do usuário, explicando detalhadamente o que se deseja pesquisar]**

---

**1. Protocolo de Pesquisa para Revisão Sistemática da Literatura**

- **Título da Pesquisa:** Elabore um título técnico e informativo para a revisão.
- **Contexto e Justificativa:** Em um parágrafo, contextualize o problema de pesquisa, destaque sua relevância atual e justifique a importância de sistematizar o conhecimento existente na área.
- **Questões de Pesquisa (QPs):** Formule de 2 a 3 questões de pesquisa (QP1, QP2...) que direcionem a investigação de forma específica e mensurável.
- **Critérios de Inclusão e Exclusão (CI/CE):** Defina critérios rigorosos para a seleção dos estudos, especificando o período de publicação (ex: 2020 a 2025), idiomas aceitos, tipos de publicação (ex: apenas artigos completos de periódicos e conferências revisados por pares) e o foco temático.
- **Estratégia de Seleção dos Estudos:** Descreva brevemente o fluxo de seleção: 1) Execução das buscas nas bases de dados e remoção de estudos duplicados; 2) Triagem por títulos e resumos aplicando os CI/CE; 3) Leitura completa dos artigos pré-selecionados para a seleção final e extração de dados.

**2. Estratégia de Busca**

- **2.1. Lista de Palavras-chave:**
    - Com base no tema, crie uma tabela com os conceitos centrais. Para cada conceito, liste as palavras-chave correspondentes em **Inglês** e **Português**.
- **2.2. _Strings_ de Busca Otimizadas:**
    - **Observação Importante:** Bases de dados como IEEE Xplore e Scopus possuem limitações quanto ao tamanho da _string_ e ao número de operadores. Se a combinação completa de palavras-chave exceder esses limites, a estratégia será segmentar a busca em _strings_ menores e lógicas, cujos resultados devem ser combinados posteriormente.
    - Gere as _strings_ de busca para **ambos os idiomas**.
    - **Base de Dados Genérica (ScienceDirect, Springer Link, Scopus):**
        - **String em Inglês:** _[Gerar aqui a string de busca completa em inglês, combinando todos os termos da tabela de palavras-chave com operadores `AND` e `OR` e uso de parênteses para agrupamento lógico, como no exemplo: (termoA1 OR termoA2) AND (termoB1 OR termoB2)]_.
        - **String em Português:** _[Gerar aqui a string de busca completa em português, seguindo a mesma lógica]_.
    - **Base de Dados IEEE Xplore:**
        - **String(s) em Inglês:** _[Gerar aqui a string de busca adaptada à sintaxe da IEEE. Se necessário, divida em múltiplas strings menores e explique que os resultados devem ser combinados. Ex: String 1: (conceitoA) AND (conceitoB); String 2: (conceitoA) AND (conceitoC)]_.
        - **String(s) em Português:** _[Gerar a versão em português, seguindo a mesma lógica de adaptação e segmentação]_.
    - **Base de Dados ACM Digital Library:**
        - **String em Inglês:** _[Gerar aqui a string de busca adaptada à sintaxe da ACM, usando aspas para termos exatos e agrupadores conforme necessário]_.
        - **String em Português:** _[Gerar a versão em português, adaptada para a ACM]_.

---

**Exemplo de Aplicação do Prompt**

**Texto de Inserção do Usuário:**

"Meu projeto de pesquisa busca investigar métodos não invasivos para a estimativa de biomassa de peixes na aquicultura, com um foco especial em tecnologias de visão computacional e inteligência artificial. O problema central é que os métodos manuais de biometria são estressantes para os animais e ineficientes. Quero identificar, comparar e analisar criticamente as principais abordagens que utilizam inteligência artificial (IA), aprendizado de máquina (ML) e modelos estatísticos para medir características biométricas como peso e comprimento de peixes em tanques de criação ou viveiros. A pesquisa deve abranger tanto os algoritmos utilizados (ex: redes neurais, regressão) quanto as tecnologias de captura de imagem (ex: câmeras estéreo, 3D), avaliando a acurácia, os desafios e a viabilidade econômica dessas soluções para pequenos e médios produtores."

**Resultado Esperado da IA:**

A IA geraria um protocolo completo e, na seção de estratégia de busca, apresentaria:

- **2.1. Lista de Palavras-chave:**

| Conceito       | Palavras-chave em Inglês                                                                                                    | Palavras-chave em Português                                                                                                                             |
| -------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Medição**    | "biometrics", "biometry", "biomass estimation", "weight estimation", "length estimation"                                    | "biometria", "estimativa de biomassa", "estimação de peso", "medição de comprimento"                                                                    |
| **Contexto**   | "aquaculture", "fish farming", "fish tank", "mariculture"                                                                   | "aquicultura", "piscicultura", "tanque de peixes", "maricultura"                                                                                        |
| **Tecnologia** | "artificial intelligence", "machine learning", "deep learning", "computer vision", "statistical models", "image processing" | "inteligência artificial", "aprendizado de máquina", "aprendizagem profunda", "visão computacional", "modelos estatísticos", "processamento de imagens" |

- **2.2. _Strings_ de Busca Otimizadas:**
    - **Base Genérica (Inglês):** `("biometrics" OR "biometry" OR "biomass estimation" OR "weight estimation") AND ("aquaculture" OR "fish farming") AND ("artificial intelligence" OR "machine learning" OR "computer vision" OR "statistical models")`
    - **Base Genérica (Português):** `("biometria" OR "estimativa de biomassa") AND ("aquicultura" OR "piscicultura") AND ("inteligência artificial" OR "aprendizado de máquina" OR "visão computacional" OR "modelos estatísticos")`
    - ... e assim por diante para as outras bases de dados, com as devidas adaptações e notas sobre segmentação.


# Capítulo 12 – Outras Estruturas de Dados Essenciais

Nos capítulos anteriores, construímos um repertório robusto de estruturas de dados, desde as lineares (Listas, Pilhas, Filas) até as hierárquicas (Árvores) e de rede (Grafos). No entanto, nem todas as estruturas de dados amplamente utilizadas na prática se encaixam perfeitamente nessas categorias canônicas. O desenvolvimento de aplicações modernas, especialmente aquelas que lidam com grandes volumes de dados, exige ferramentas especializadas para problemas específicos.

Neste capítulo, exploraremos algumas dessas estruturas essenciais: **Tabela de Dispersão (Hashtable)**, **Set (Conjunto)** e **Heap (Heap Binário)**. Concluiremos com uma análise de como os dados são organizados em nível de sistema, abordando **Bitmaps** e **Estruturas de Arquivos**. Essas estruturas destacam-se por oferecerem soluções elegantes e de alta performance para problemas como busca em tempo constante, verificação de unicidade, gerenciamento de prioridades e indexação de dados em larga escala.

## Tabela de Dispersão (Hashtable)

Uma necessidade recorrente em computação é o **acesso rápido a informações**. Vimos que vetores oferecem acesso $O(1)$ _por índice_, mas não por _valor_. Árvores binárias de busca oferecem acesso $O(\log n)$ _por valor_, o que é rápido, mas não instantâneo. As **Tabelas de Dispersão** (ou **Hashtables**) surgem como a solução mais eficiente para este problema, visando um acesso em **tempo constante médio ($O(1)$)**.

Uma Hashtable, também conhecida como **tabela hash** ou **dicionário**, combina a velocidade de acesso de um vetor com a flexibilidade de tamanho. A ideia central é usar uma **função hash** (função de dispersão) para converter uma **chave** (como um CPF, um e-mail, ou uma palavra) em um **índice** numérico de um vetor. Esse índice é usado para localizar diretamente a posição do dado, sem percorrer a estrutura.

O princípio básico é o seguinte:

1. Uma **chave** (ex: "Alice") é fornecida para identificar um **valor** (ex: "Nota 10").
2. A chave "Alice" é processada por uma **função hash** (ex: `f("Alice")`), que gera um número (o _hash code_).
3. Esse número é mapeado para um **índice** dentro do vetor (ex: `hash % tamanho_vetor`).
4. O valor "Nota 10" é armazenado nesse índice.

Quando se deseja buscar por "Alice", o processo é repetido: a função hash gera o _mesmo_ índice, permitindo o acesso direto ao valor.

<div align="center">
<img width="550px" src="./img/12-hashtable.png">
</div>

A imagem acima ilustra esse processo. Um "Elemento" (chave) é processado pela função `f` (Cálculo do valor da chave), que gera um índice (hash). Este índice determina a posição (o "balde" ou _bucket_) onde o objeto (valor) será armazenado.

### Problema da Colisão

O grande desafio das tabelas hash é a **colisão**: o que acontece quando a função hash gera o _mesmo_ índice para duas chaves diferentes? Na imagem, vemos que `Obj3` e `Obj6` foram mapeados para o mesmo hash `25`. Existem duas estratégias principais para lidar com isso:

**Encadeamento (Chaining):** Esta é a abordagem mais comum. Em vez de a tabela armazenar o valor diretamente, cada posição da tabela armazena um ponteiro para uma lista encadeada (ou outra estrutura) contendo todos os elementos que colidiram naquele índice.

- **Inserção:** Calcula o índice. Adiciona o novo elemento ao final (ou início) da lista encadeada nesse índice.
- **Busca:** Calcula o índice. Percorre a (normalmente curta) lista encadeada nesse índice para encontrar o valor.

**Pseudocódigo (Hashtable com Encadeamento):**

```
classe Hashtable
    tamanho = 100
    tabela = novo Vetor[tamanho] // Cada posição é uma lista encadeada

    // Inicializa a tabela com listas vazias
    para i de 0 até tamanho - 1
        tabela[i] = nova ListaEncadeada()
    fim_para

    função GerarHash(chave)
        // ... (algoritmo complexo para converter chave em número) ...
        indice = hash_calculado % tamanho
        retornar indice
    fim_função

    procedimento Inserir(chave, valor)
        indice = GerarHash(chave)
        // Adiciona o par (chave, valor) na lista daquele índice
        tabela[indice].InserirNoFim( (chave, valor) )
    fim_procedimento

    função Buscar(chave)
        indice = GerarHash(chave)
        // Percorre a lista encadeada naquele índice
        para cada par em tabela[indice]
            se par.chave == chave
                retornar par.valor // Encontrou
            fim_se
        fim_para
        retornar NULL // Não encontrou
    fim_função
fim_classe
```

**Endereçamento Aberto (Open Addressing):** Nesta abordagem, todos os elementos são armazenados no próprio vetor. Se ocorre uma colisão (a posição `i` já está ocupada), o algoritmo "sonda" (procura) a próxima posição livre. A forma mais simples é a sondagem linear, que simplesmente verifica `i + 1`, `i + 2`, `i + 3`, até encontrar um espaço vazio.

### Análise e Aplicações

- **Complexidade (Caso Médio):** Com uma boa função hash e uma tabela não muito cheia, as colisões são raras e as listas de encadeamento são curtas (ou a sondagem é rápida). As operações de inserção, busca e remoção atingem o **$O(1)$** amortizado.
- **Complexidade (Pior Caso):** Em um cenário desastroso onde todas as chaves colidem no mesmo índice, a hashtable degenera em uma única lista encadeada. A complexidade de busca torna-se **$O(n)$**.

Muitas linguagens fornecem implementações nativas (como `Map` em Java, `dict` em Python, `Object`/`Map` em JavaScript) que já tratam colisões de forma eficiente.

## Sets (Conjuntos)

A estrutura **Set** (ou conjunto) é a implementação computacional do conceito matemático de um conjunto. Suas duas características definidoras são:

1. **Unicidade:** Os elementos não se repetem. Cada elemento é único.
2. **Ausência de Ordem:** Os elementos não são armazenados em uma ordem específica ou previsível.

Sets são extremamente úteis para verificar pertencimento (se um item existe ou não) e para eliminar duplicatas de uma coleção.

```python
# Exemplo em Python
participantes = set()
participantes.add("Alice")
participantes.add("Bruno")
participantes.add("Alice")  # Tentativa de adicionar duplicado

print(participantes)
# Saída esperada: {'Alice', 'Bruno'}
# A segunda adição de "Alice" é simplesmente ignorada.

# Verificação de pertencimento
if "Bruno" in participantes:
    print("Bruno está no conjunto.")
```

### Implementação de Sets

Como um Set pode verificar o pertencimento (`"Bruno" in participantes`) tão rapidamente? Na maioria das linguagens modernas, um **Set é implementado internamente usando uma Tabela de Dispersão**.

Quando se executa `participantes.add("Alice")`:

1. A string "Alice" é usada como a _chave_ na hashtable interna.
2. O _valor_ armazenado pode ser a própria string ou apenas um marcador de presença.
3. A operação `contains("Alice")` (ou `in`) é simplesmente uma busca $O(1)$ na hashtable.

Por causa disso, as operações de `add`, `remove` e `contains` em um Set têm complexidade de tempo média de **$O(1)$**.

## Heap (Heap Binário)

O **Heap** (Monte), ou mais especificamente **Heap Binário**, é uma estrutura de dados baseada em árvore binária, projetada especificamente para uma tarefa: gerenciar prioridades. Ela é a estrutura de dados ideal para implementar **Filas de Prioridade**, onde não seguimos o LIFO nem o FIFO, mas sim "o mais importante sai primeiro".

Um heap binário deve obedecer a duas propriedades:

1. **Propriedade da Estrutura:** É uma **árvore binária completa** (ou quase completa). Isso significa que todos os níveis estão cheios, exceto o último, que é preenchido da esquerda para a direita.
2. **Propriedade do Heap:** Todos os nós devem obedecer a uma das duas regras:
    - **Max-Heap (Monte Máximo):** Cada nó pai é _maior ou igual_ ao valor de seus filhos. A raiz é o maior elemento.
    - **Min-Heap (Monte Mínimo):** Cada nó pai é _menor ou igual_ ao valor de seus filhos. A raiz é o menor elemento.

A imagem a seguir ilustra os dois tipos de heap.

<div align="center">
<img width="700px" src="./img/12-heap.png">
</div>

Em **(a)**, temos um Max-Heap: `35` (raiz) é maior que `25` e `30`; `25` é maior que `5` e `10`, e assim por diante. Em **(b)**, temos um Min-Heap: `5` (raiz) é menor que `10` e `25`, etc.

### Implementação com Vetor

A genialidade do Heap Binário é que, por ser uma árvore completa, ele pode ser armazenado em um **vetor** (array) sem a necessidade de ponteiros, economizando memória. A relação pai-filho é calculada matematicamente usando índices (base 0):

- Para um nó no índice `i`:
    - Seu filho esquerdo está em: `(2 * i) + 1`
    - Seu filho direito está em: `(2 * i) + 2`
    - Seu pai está em: `floor((i - 1) / 2)`

### Operações Fundamentais (Min-Heap)

**Inserção (Sift-Up / Swim):** A inserção de um novo elemento é feita em $O(\log n)$:

1. Adiciona o novo elemento na _última posição livre_ do vetor (mantendo a árvore completa).
2. Compara o novo elemento com seu pai.
3. Se o elemento for _menor_ que seu pai (violando a propriedade Min-Heap), troca os dois.
4. Repete o processo (o elemento "flutua" ou "nada" para cima, _sift-up_) até que ele seja maior que seu pai ou chegue à raiz.

**Remoção da Raiz (Sift-Down / Sink / Heapify):** A remoção do elemento prioritário (a raiz) é feita em $O(\log n)$:

1. Remove-se a raiz (o menor elemento) e a armazena para retorno.
2. Pega-se o _último_ elemento do heap (último item do vetor) e o move para a posição da raiz (índice 0).
3. A propriedade do heap está agora quebrada na raiz.
4. Compara-se a nova raiz com seus filhos. Se ela for _maior_ que o menor de seus filhos, ela é trocada com ele (ela "afunda", _sift-down_).
5. Repete o processo (o elemento "afunda") até que ele seja menor que seus filhos ou chegue a uma folha.

O exemplo prático em Python com `heapq` ilustra o uso de um Min-Heap como fila de prioridade.

```python
import heapq

# A fila_prioridade é apenas uma lista (vetor)
fila_prioridade = []

# heappush insere mantendo a propriedade do heap (Min-Heap)
heapq.heappush(fila_prioridade, (1, "Documento Urgente"))
heapq.heappush(fila_prioridade, (3, "Documento Normal"))
heapq.heappush(fila_prioridade, (2, "Documento Importante"))

# heappop sempre remove e retorna o menor item (o da raiz)
while fila_prioridade:
    prioridade, documento = heapq.heappop(fila_prioridade)
    print(f"Imprimindo (Prioridade {prioridade}): {documento}")
# Saída:
# Imprimindo (Prioridade 1): Documento Urgente
# Imprimindo (Prioridade 2): Documento Importante
# Imprimindo (Prioridade 3): Documento Normal
```

## Bitmaps (Mapas de Bits)

Os **Bitmaps** (Mapas de Bits) são uma técnica de indexação de dados usada em bancos de dados para acelerar consultas, especialmente em colunas com **baixa cardinalidade** (poucos valores distintos, como "País", "Gênero" ou "Estado Civil"). Não deve ser confundido com o formato de imagem `.bmp`.

A ideia é representar a presença ou ausência de um valor usando um único **bit** (0 ou 1). Isso é extremamente compacto e permite operações lógicas muito rápidas.

Considere a tabela `Banda`:

|**ROWID**|**NOME**|**PAIS**|**ESTILO**|
|---|---|---|---|
|1|Radiohead|Inglaterra|Indie Rock|
|2|The Killers|EUA|Rock|
|3|Beach House|EUA|Indie Rock|
|4|The Cure|Inglaterra|Indie Rock|
|5|The Cardigans|Suécia|Rock|
|6|R.E.M.|EUA|Rock|
|7|The Hives|Suécia|Punk Rock|
|8|The Who|Inglaterra|Rock|
|9|Green Day|EUA|Punk Rock|
|10|Sex Pistols|Inglaterra|Punk Rock|

Em vez de fazer uma "varredura completa da tabela" (Full Table Scan) para encontrar bandas da Inglaterra, criamos um índice bitmap para a coluna `PAIS`. Os valores distintos são: EUA, Inglaterra, Suécia.

**Índice Bitmap (por `PAIS`):**

|**PAIS**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|**8**|**9**|**10**|
|---|---|---|---|---|---|---|---|---|---|---|
|EUA|0|1|1|0|0|1|0|0|1|0|
|Inglaterra|1|0|0|1|0|0|0|1|0|1|
|Suécia|0|0|0|0|1|0|1|0|0|0|

Um `1` indica que a tupla (linha) tem aquele valor; `0` indica que não.

### Consultas com Bitmaps

Consulta Simples:

```sql
SELECT * FROM BANDA WHERE PAIS = 'Inglaterra';
```

O SGBD não precisa ler a tabela. Ele apenas pega o vetor de bits da linha "Inglaterra": `[1, 0, 0, 1, 0, 0, 0, 1, 0, 1]`. Os bits 1 estão nas posições 1, 4, 8, 10. O SGBD vai diretamente a esses ROWIDs e retorna os dados.

Consulta Composta (OR):

```sql
SELECT * FROM BANDA WHERE PAIS = 'Inglaterra' OR PAIS = 'EUA';
```

O SGBD realiza uma operação `OU` (bitwise `OR`) nos vetores de bits:

```
   [0, 1, 1, 0, 0, 1, 0, 0, 1, 0] (EUA)
OR [1, 0, 0, 1, 0, 0, 0, 1, 0, 1] (Inglaterra)
----------------------------------
=  [1, 1, 1, 1, 0, 1, 0, 1, 1, 1] (Resultado)
```

O SGBD agora busca os ROWIDs 1, 2, 3, 4, 6, 8, 9, 10.

Bitmaps também podem ser combinados para múltiplas colunas. São ideais para ambientes de _Data Warehouse_ (OLAP), que têm muitas leituras e poucas escritas, mas são ruins para sistemas OLTP (como um e-commerce), pois cada `INSERT` ou `UPDATE` exigiria o recálculo de múltiplos vetores de bits.

## Estrutura de Arquivos

Até agora, discutimos estruturas de dados que operam na Memória Principal (RAM). As **Estruturas de Arquivos** tratam de como os dados são organizados e acessados na Memória Secundária (Discos Rígidos, SSDs), que é persistente.

Um **arquivo** é uma coleção de **registros**. Um **registro lógico** é um conjunto de **campos** (atributos) que descrevem uma entidade.

- Exemplo de Registro Lógico: `[Nome: João Silva] [Matrícula: 20231234] [IRA: 8.7]`

Existem três formas principais de acessar os dados em um arquivo:

1. **Acesso Sequencial:** Os registros são lidos em ordem, um após o outro, do início ao fim. Simples, mas ineficiente para buscas pontuais.
2. **Acesso Direto (Aleatório):** Usa uma chave (via **função hash**) para calcular a posição exata do registro no disco. Rápido para acesso individual, mas sofre com colisões.
3. **Acesso Indexado:** Usa uma estrutura auxiliar (um **índice**, como uma Árvore-B ou um Bitmap) que mapeia chaves a locais no disco. É o método mais flexível, usado pela maioria dos SGBDs.

A organização física dos arquivos (como eles são "desenhados" no disco) também varia:

- **Arquivos não ordenados (Heap Files):** Registros armazenados na ordem em que chegam. Rápido para inserir, lento para buscar.
- **Arquivos ordenados (Sorted Files):** Registros mantidos em ordem por uma chave. Permite busca binária, mas torna a inserção lenta ($O(n)$).

## Considerações Finais

Neste capítulo, exploramos cinco estruturas e técnicas essenciais usadas na organização de dados, muitas das quais operam "nos bastidores" de sistemas de software modernos.

Iniciamos com os **Sets**, o TAD para garantir **unicidade**, e vimos que sua implementação $O(1)$ geralmente depende da **Tabela Hash**. A Tabela Hash, por sua vez, é a estrutura de escolha para **acesso e busca em tempo constante**, resolvendo o desafio das colisões com técnicas como encadeamento.

O **Heap** foi apresentado como a estrutura ideal para **gerenciamento de prioridades**, usando uma árvore binária quase completa para garantir acesso $O(\log n)$ ao elemento mais (ou menos) prioritário, sendo crucial para filas de prioridade e algoritmos como o Heapsort.

Avançamos para os **Bitmaps**, uma técnica de indexação de banco de dados que oferece enorme compactação e velocidade para consultas em colunas de **baixa cardinalidade**, sendo muito usada em ambientes OLAP.

Por fim, discutimos as **Estruturas de Arquivos**, que formam a base da **persistência de dados** em disco, diferenciando os métodos de acesso (sequencial, direto, indexado) que determinam o desempenho de SGBDs.

A compreensão dessas estruturas é fundamental, pois cada uma oferece uma solução otimizada para um problema específico, permitindo que profissionais tomem decisões informadas sobre como estruturar, acessar e manipular dados com eficiência.
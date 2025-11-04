# Capítulo 8 – Listas Encadeadas

No capítulo anterior, exploramos vetores e matrizes, estruturas de dados fundamentais baseadas em alocação contígua de memória. Vimos que sua grande força é o acesso instantâneo a qualquer elemento por meio de um índice, uma operação de complexidade $O(1)$. No entanto, também identificamos sua fraqueza crítica: a rigidez. Inserir ou remover um elemento no início ou no meio de um vetor é uma operação custosa, de complexidade $O(n)$, pois exige o deslocamento de todos os elementos subsequentes.

Para resolver exatamente esse problema, introduzimos agora as **listas encadeadas** (ou listas ligadas). Elas constituem uma importante classe de estruturas de dados dinâmicas que trocam o acesso $O(1)$ pela eficiência na modificação da estrutura. Diferentemente dos vetores, as listas organizam seus elementos de forma **não contígua** na memória, possibilitando um gerenciamento mais flexível e operações de inserção e remoção $O(1)$ (sob certas condições).

Essas listas são compostas por **nós** (ou "elementos"), onde cada nó é um pequeno objeto que armazena duas coisas: o **valor (ou dado)** em si e, pelo menos, uma **referência (ou ponteiro)** que aponta para o próximo nó na sequência. Essa estrutura de ponteiros cria uma cadeia de elementos, que só pode ser percorrida seguindo-se os elos.

Neste capítulo, exploraremos em profundidade as principais variações de listas encadeadas:

- Listas encadeadas simples
- Listas duplamente encadeadas
- Listas circulares
- Listas circulares duplamente encadeadas

Cada variação será analisada quanto à sua estrutura, funcionalidade, operações básicas (com pseudocódigo), vantagens, desvantagens e custos computacionais.

## Bloco de Construção: Nó (Node)

Antes de construir uma lista, precisamos de seu componente fundamental: o **Nó** (Node). Um nó é uma estrutura simples que agrupa o dado que queremos armazenar e o(s) ponteiro(s) que formam a "cola" da lista.

**Pseudocódigo (Nó Simples):**

```
classe No
    dado // O valor que queremos armazenar (inteiro, string, objeto, etc.)
    proximo // Um ponteiro/referência para o próximo nó
fim_classe
```

Para listas mais complexas, como veremos, este nó também pode conter um ponteiro `anterior`.

## Listas Encadeadas Simples (Singly Linked Lists)

As **listas encadeadas simples** são a forma mais básica desta estrutura. Cada nó contém exatamente um dado e um ponteiro para o `proximo` nó.

A estrutura é gerenciada por um **ponteiro externo**, geralmente chamado de `head` (cabeça), que aponta para o primeiro nó da lista. A lista "termina" quando um nó tem seu ponteiro `proximo` ajustado para `NULL` (ou `None`), indicando que não há mais elementos na sequência.

<div align="center">
<img width="600px" src="./img/08-lista-simples.png">
</div>

Essa estrutura é inerentemente **unidirecional**: só podemos percorrê-la no sentido `head` $\rightarrow$ `NULL`. Como cada nó só conhece seu sucessor, qualquer operação que precise do nó _anterior_ (como uma remoção no meio) exigirá uma travessia a partir do início.

### Operações Fundamentais

Vamos analisar as operações básicas e seu custo computacional (onde $n$ é o número de elementos na lista).

**Busca por Valor (Complexidade: $O(n)$):** Como não há acesso direto por índice, a única forma de encontrar um valor é percorrer a lista nó por nó, a partir do head, até encontrar o valor ou chegar a NULL.

**Pseudocódigo (Busca):**

```
função Buscar(head, valorBuscado)
    atual = head
    enquanto atual != NULL
        se atual.dado == valorBuscado
            retornar verdadeiro // Encontrou
        fim_se
        atual = atual.proximo // Avança para o próximo nó
    fim_enquanto
    retornar falso // Chegou ao fim e não encontrou
fim_função
```

**Inserção no Início (Complexidade: $O(1)$):** Esta é a operação mais eficiente da lista encadeada. Não importa o tamanho da lista, a inserção no início leva o mesmo tempo constante.

**Passos:**

1. Crie um `novoNo` com o valor desejado.
2. Faça o ponteiro `proximo` do `novoNo` apontar para o `head` atual.
3. Atualize o `head` para que ele aponte para o `novoNo`.

**Pseudocódigo (Inserção no Início):**

```
procedimento InserirInicio(head_ref, valor)
    novoNo = novo Nó(valor)
    novoNo.proximo = head_ref // O novo nó aponta para o antigo primeiro
    head_ref = novoNo       // A referência da cabeça agora aponta para o novo nó
fim_procedimento
```

**Remoção do Início (Complexidade: $O(1)$):** Assim como a inserção, a remoção do primeiro nó é extremamente rápida.

**Passos:**

1. Verifique se a lista não está vazia.
2. Crie um ponteiro temporário (`noRemovido`) para o `head`.
3. Atualize o `head` para apontar para `head.proximo` (o segundo nó).
4. Libere a memória do `noRemovido`.

**Pseudocódigo (Remoção do Início):**

```
procedimento RemoverInicio(head_ref)
    se head_ref == NULL
        retornar // Lista vazia
    fim_se
    noRemovido = head_ref
    head_ref = head_ref.proximo // Avança a cabeça
    liberar_memoria(noRemovido)
fim_procedimento
```

**Inserção no Fim (Complexidade: $O(n)$):** Para inserir um nó no final, precisamos primeiro encontrar o último nó. Isso exige uma travessia completa ($O(n)$). Uma vez encontrado o último nó, a inserção em si (passo 2) é $O(1)$.

**Pseudocódigo (Inserção no Fim):**

```
procedimento InserirFim(head, valor)
    novoNo = novo Nó(valor)
    novoNo.proximo = NULL

    se head == NULL // Se a lista estiver vazia
        head = novoNo
        retornar
    fim_se
    
    ultimo = head
    enquanto ultimo.proximo != NULL // Travessia O(n)
        ultimo = ultimo.proximo
    fim_enquanto
    
    ultimo.proximo = novoNo // Ligação
fim_procedimento
```

_(Nota: Manter um ponteiro separado para a `cauda` (tail) pode tornar esta operação $O(1)$)._

**Remoção no Meio ou Fim (Complexidade: $O(n)$):** Esta é a operação mais custosa. Para remover um nó X, não basta ter um ponteiro para X; precisamos de um ponteiro para o nó anterior a X, para que possamos "remendar" a lista (`anterior.proximo = X.proximo`). Encontrar o nó anterior exige uma travessia $O(n)$.

## Listas Duplamente Encadeadas (Doubly Linked Lists)

As **listas duplamente encadeadas** resolvem a principal fraqueza da lista simples (a dificuldade de acessar o nó anterior). Elas ampliam a flexibilidade ao adicionar um segundo ponteiro em cada nó.

Cada nó agora contém três informações:

- Um **dado**.
- Um ponteiro para o **próximo** nó (`proximo`).
- Um ponteiro para o **nó anterior** (`anterior`).

Isso permite a **navegação bidirecional**, ou seja, percorrer a lista do início ao fim ou do fim ao início. Além do `head`, é comum manter um ponteiro `tail` (cauda) para o último elemento.

<div align="center">
<img width="700px" src="./img/08-lista-dupla.png">
</div>

### Operações Fundamentais

A busca continua sendo $O(n)$, pois ainda não há acesso indexado. A inserção no início (e no fim, se tivermos um ponteiro `tail`) continua $O(1)$. A grande mudança está na remoção.

**Remoção de Nó Conhecido (Complexidade: $O(1)$):** Esta é a principal vantagem da lista dupla. Se já temos um ponteiro para o nó X que queremos remover (por exemplo, como resultado de uma busca), podemos removê-lo em tempo constante, sem precisar de uma nova travessia para encontrar seu antecessor.

**Passos:**

1. Faz o proximo do nó anterior a X apontar para o proximo de X. `X.anterior.proximo = X.proximo`.
2. Faz o anterior do nó posterior a X apontar para o anterior de X. `X.proximo.anterior = X.anterior`.
3. Libera a memória de `X`.

**Pseudocódigo (Remoção de Nó Conhecido):**

```
procedimento RemoverNo(no)
    // Se não for o primeiro nó
    se no.anterior != NULL
        no.anterior.proximo = no.proximo
    senão // O nó a ser removido é o head
        head = no.proximo
    fim_se
    
    // Se não for o último nó
    se no.proximo != NULL
        no.proximo.anterior = no.anterior
    senão // O nó a ser removido é o tail
        tail = no.anterior
    fim_se
    
    liberar_memoria(no)
fim_procedimento
```

A desvantagem é o maior uso de memória (um ponteiro extra por nó) e a maior complexidade de implementação (mais ponteiros para gerenciar em cada inserção/remoção).

## Listas Circulares (Circular Linked Lists)

As **listas circulares** são uma variação das listas simples, onde o último nó, em vez de apontar para `NULL`, aponta de volta para o primeiro nó da lista (`head`), formando um ciclo fechado.

<div align="center">
<img width="600px" src="./img/08-lista-circular.png">
</div>

Essa estrutura elimina "pontas soltas" e é útil em sistemas que exigem um percurso contínuo ou cíclico, como:

- **Buffers circulares:** Onde os dados são continuamente escritos e lidos.
- **Agendamento Round-Robin:** Onde o sistema passa por uma lista de processos, e ao chegar ao fim, volta ao primeiro.
- **Problema de Josephus:** Um problema clássico de ciência da computação.

Como não há `NULL`, a travessia deve ter um critério de parada diferente, geralmente verificando se o ponteiro `atual` voltou a ser igual ao `head`.

Uma otimização comum é manter o ponteiro externo apontando para o **último** nó (`tail`) em vez do primeiro. Isso é vantajoso porque nos dá acesso $O(1)$ a _ambos_ os extremos da lista:

- O `tail` é o último nó.
- O `tail.proximo` é o `head` (o primeiro nó).

Isso permite implementar Pilhas e Filas de forma muito eficiente sobre uma lista circular.

## Listas Circulares Duplamente Encadeadas

Esta estrutura combina os benefícios de ambas as variações: é **duplamente encadeada** (ponteiros `proximo` e `anterior`) e **circular** (o `head` e o `tail` estão conectados).

- O ponteiro `proximo` do último nó aponta para o `head`.
- O ponteiro `anterior` do `head` aponta para o último nó.

<div align="center">
<img width="600px" src="./img/08-lista-circular-dupla.png">
</div>

Esta é a estrutura de lista mais flexível, oferecendo o máximo de versatilidade:

- Navegação contínua em ambos os sentidos.
- Inserção e remoção $O(1)$ em _qualquer_ ponto conhecido, incluindo início e fim.
- Não existem "pontas" `NULL` para se preocupar, o que pode simplificar alguns algoritmos de manipulação.

Naturalmente, essa flexibilidade tem o custo do **maior uso de memória** (dois ponteiros por nó) e da **máxima complexidade de implementação**, pois cada inserção ou remoção requer a atualização cuidadosa de quatro ponteiros para manter a integridade do ciclo.

## Tabela Comparativa: Vetores vs. Listas Encadeadas

A decisão entre usar um vetor ou uma lista encadeada é um dos _trade-offs_ mais clássicos em programação. A escolha depende inteiramente do que se pretende fazer com os dados.

|**Operação**|**Vetor (Estático)**|**Vetor Dinâmico (Amortizado)**|**Lista Encadeada Simples**|**Lista Duplamente Encadeada**|
|---|---|---|---|---|
|**Acesso por Índice** (`vetor[i]`)|$O(1)$|$O(1)$|$O(n)$|$O(n)$|
|**Busca por Valor**|$O(n)$|$O(n)$|$O(n)$|$O(n)$|
|**Inserção no Início**|$O(n)$|$O(n)$|**$O(1)$**|**$O(1)$**|
|**Inserção no Fim**|N/A (fixo)|**$O(1)$***|$O(n)$**|**$O(1)$** (com `tail`)|
|**Inserção no Meio**|$O(n)$|$O(n)$|$O(n)$|$O(n)$***|
|**Remoção no Início**|$O(n)$|$O(n)$|**$O(1)$**|**$O(1)$**|
|**Remoção no Fim**|N/A (fixo)|**$O(1)$***|$O(n)$|**$O(1)$** (com `tail`)|
|**Remoção no Meio**|$O(n)$|$O(n)$|$O(n)$|$O(n)$***|
|**Complexidade de Espaço**|$O(n)$|$O(n)$|$O(n)$|$O(n)$|

\*Custo amortizado (pode ser $O(n)$ raramente, ao redimensionar).
\*\*Pode ser $O(1)$ se um ponteiro `tail` for mantido.
\*\*\*A busca pela posição é $O(n)$, mas a operação de inserção/remoção em si (se o nó já for conhecido) é $O(1)$.

## Considerações Finais

As listas encadeadas, em suas diversas formas, são estruturas fundamentais que oferecem soluções elegantes para problemas que exigem **flexibilidade na alocação de memória** e **eficiência nas operações de inserção e remoção**, especialmente nas extremidades.

Vimos o _trade-off_ fundamental:

- **Vetores** são reis do **acesso**: $O(1)$ para ler/escrever em qualquer índice.
- **Listas** são reis da **modificação**: $O(1)$ para inserir/remover no início (ou em um nó conhecido, no caso da dupla).

A escolha entre listas simples, duplamente encadeadas ou circulares depende dos requisitos do problema. Se a travessia reversa ou remoções eficientes no meio (com nó conhecido) são necessárias, a lista dupla é a escolha. Se um fluxo contínuo é necessário, a circular se aplica.

Compreender essas estruturas é essencial, pois elas formam a base para a implementação de outros TADs cruciais, como **pilhas**, **filas** e **deques**, que exploraremos nos próximos capítulos.
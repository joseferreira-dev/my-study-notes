# Capítulo 9 – Pilhas, Filas e Deques

Nos capítulos anteriores, exploramos as estruturas de dados lineares fundamentais: **Vetores** e **Listas Encadeadas**. Vimos que ambas oferecem uma maneira de armazenar dados em sequência, mas com _trade-offs_ críticos: vetores oferecem acesso indexado $O(1)$ ao custo de inserções/remoções $O(n)$, enquanto listas encadeadas oferecem inserções/remoções $O(1)$ (em certas condições) ao custo de acesso $O(n)$.

Agora, vamos focar em três Tipos Abstratos de Dados (TADs) que são, na verdade, especializações dessas estruturas: a **Pilha (Stack)**, a **Fila (Queue)** e a **Fila Dupla (Deque)**. A característica principal desses TADs é que eles não são sobre _implementação_, mas sobre _comportamento_. Eles são estruturas lineares que **restringem** intencionalmente as operações, forçando um padrão de acesso específico. Essa restrição não é uma limitação, mas sim sua maior força, permitindo a modelagem de processos computacionais complexos de forma elegante e eficiente.

## Pilha (Stack)

A **pilha (stack)** é uma estrutura de dados linear que opera estritamente sob o princípio **LIFO (Last In, First Out)**: o último elemento a ser inserido é, obrigatoriamente, o primeiro a ser removido. A metáfora mais comum é uma pilha de pratos: o último prato colocado no topo é o primeiro que você retira.

<div align="center">
<img width="500px" src="./img/09-pilha.png">
</div>

Qualquer tentativa de acessar, inserir ou remover um elemento que não esteja no topo é proibida pelas regras do TAD. Todas as operações ocorrem em uma única extremidade: o **topo**.

### Operações Fundamentais

Um TAD Pilha é definido por quatro operações principais:

1. **Push(valor):** Insere um novo elemento no topo da pilha.
2. **Pop():** Remove e retorna o elemento que está no topo da pilha.
3. **Peek() (ou Top):** Retorna o elemento do topo sem removê-lo.
4. **isEmpty():** Retorna `verdadeiro` se a pilha estiver vazia, ou `falso` caso contrário.

### Implementação de Pilhas

Como um TAD, a Pilha pode ser implementada de várias formas. As mais comuns são com vetores (estática) ou listas encadeadas (dinâmica).

**Implementação com Vetor (Pilha Estática):** Esta implementação utiliza um vetor de tamanho fixo e um índice (vamos chamá-lo de topo) que rastreia a posição do último elemento inserido. O topo geralmente começa em `-1`.

**Pseudocódigo (Pilha com Vetor):**

```
classe PilhaVetor
    capacidade = 100 // Tamanho fixo
    dados = novo Vetor[capacidade]
    topo = -1 // Pilha começa vazia

    procedimento Push(valor)
        se topo == capacidade - 1
            lancar Erro("Stack Overflow") // Pilha cheia
        fim_se
        topo = topo + 1
        dados[topo] = valor
    fim_procedimento

    função Pop()
        se isEmpty()
            lancar Erro("Stack Underflow") // Pilha vazia
        fim_se
        valorRemovido = dados[topo]
        topo = topo - 1
        retornar valorRemovido
    fim_função

    função Peek()
        se isEmpty()
            lancar Erro("Pilha vazia")
        fim_se
        retornar dados[topo]
    fim_função

    função isEmpty()
        retornar topo == -1
    fim_função
fim_classe
```

**Implementação com Lista Encadeada (Pilha Dinâmica):** Esta implementação usa uma lista encadeada simples, onde o `head` (cabeça) da lista é o topo da pilha. Esta abordagem tem a vantagem de ter um tamanho dinâmico.

**Pseudocódigo (Pilha com Lista Encadeada):**

```
classe PilhaLista
    head = NULL // Ponteiro para o topo

    procedimento Push(valor)
        // Equivalente a uma Inserção no Início da lista
        novoNo = novo Nó(valor)
        novoNo.proximo = head
        head = novoNo
    fim_procedimento

    função Pop()
        // Equivalente a uma Remoção do Início da lista
        se isEmpty()
            lancar Erro("Stack Underflow")
        fim_se
        valorRemovido = head.dado
        noTemporario = head
        head = head.proximo
        liberar_memoria(noTemporario)
        retornar valorRemovido
    fim_função

    função Peek()
        se isEmpty()
            lancar Erro("Pilha vazia")
        fim_se
        retornar head.dado
    fim_função

    função isEmpty()
        retornar head == NULL
    fim_função
fim_classe
```

### Análise e Aplicações

Independentemente da implementação (vetor ou lista), todas as operações fundamentais (`push`, `pop`, `peek`, `isEmpty`) envolvem apenas a manipulação da extremidade (o `topo` ou o `head`). Portanto, todas possuem uma complexidade de tempo **$O(1)$**.

As pilhas são fundamentais em computação e aparecem em incontáveis aplicações:

- **Gerenciamento de Funções (Call Stack):** Quando uma função `A` chama uma função `B`, o estado de `A` é "empilhado". Quando `B` termina, seu estado é "desempilhado" e `A` continua. A recursão depende inteiramente disso.
- **Avaliação de Expressões:** Usadas para converter expressões infixas para pós-fixas (Notação Polonesa Inversa) e para avaliar o resultado.
- **Reversão de Sequências:** A forma mais fácil de reverter uma palavra é empilhar (push) cada letra e depois desempilhar (pop) todas.
- **Algoritmos de Backtracking:** Em problemas como encontrar a saída de um labirinto, a pilha armazena o caminho percorrido, permitindo "retroceder" (pop) quando se atinge um beco sem saída.
- **Funcionalidade "Desfazer/Refazer":** Em editores, cada ação é "empilhada". O "Desfazer" (Undo) é um `pop` da pilha de ações.

## Fila (Queue)

A **fila (queue)** é outra estrutura linear restrita, mas que segue o princípio oposto: **FIFO (First In, First Out)**. O primeiro elemento a ser inserido é o primeiro a ser removido. A analogia perfeita é uma fila de banco ou supermercado: quem chega primeiro é atendido primeiro.

<div align="center">
<img width="700px" src="./img/09-fila.png">
</div>

Para que a lógica FIFO funcione, as operações ocorrem em extremidades opostas: as inserções ocorrem no **final (rear)** da fila, e as remoções ocorrem no **início (front)**.

### Operações Fundamentais

Um TAD Fila é definido por quatro operações principais:

1. **Enqueue(valor):** Insere um novo elemento no final (rear) da fila.
2. **Dequeue():** Remove e retorna o elemento do início (front) da fila.
3. **Front() (ou Peek):** Retorna o elemento do início sem removê-lo.
4. **isEmpty():** Verifica se a fila está vazia.

### Implementação de Filas

**Implementação com Lista Encadeada (com Cauda):** Para que Enqueue e Dequeue sejam ambas $O(1)$, a implementação com lista encadeada precisa de dois ponteiros: um head (para o início, onde ocorre o dequeue) e um tail (para o fim, onde ocorre o enqueue).

**Pseudocódigo (Fila com Lista Encadeada):**

```
classe FilaLista
    head = NULL // Ponteiro para o início (front)
    tail = NULL // Ponteiro para o fim (rear)

    procedimento Enqueue(valor)
        novoNo = novo Nó(valor)
        novoNo.proximo = NULL
        
        se isEmpty() // Se for o primeiro elemento
            head = novoNo
            tail = novoNo
        senão
            tail.proximo = novoNo // Liga o antigo rabo ao novo
            tail = novoNo       // Atualiza o rabo
        fim_se
    fim_procedimento

    função Dequeue()
        se isEmpty()
            lancar Erro("Fila vazia")
        fim_se
        
        valorRemovido = head.dado
        noTemporario = head
        head = head.proximo // Avança a cabeça
        
        se head == NULL // Se a fila ficou vazia
            tail = NULL
        fim_se
        
        liberar_memoria(noTemporario)
        retornar valorRemovido
    fim_função

    função Front()
        se isEmpty()
            lancar Erro("Fila vazia")
        fim_se
        retornar head.dado
    fim_função

    função isEmpty()
        retornar head == NULL
    fim_função
fim_classe
```

**Implementação com Vetor (Fila Circular):** Se usarmos um vetor simples, a operação Dequeue seria $O(n)$, pois teríamos que deslocar todos os elementos para a esquerda para "fechar o buraco" no início. Para evitar isso, usamos um Vetor Circular (ou Fila Circular).

Nesta abordagem, mantemos dois índices: `inicio` e `fim`. Quando `Enqueue` é chamado, `fim` avança. Quando `Dequeue` é chamado, `inicio` avança. Os índices "dão a volta" para o começo do vetor quando chegam ao final (usando aritmética modular). Isso evita o deslocamento de elementos.

### Análise e Aplicações

Em ambas as implementações otimizadas (Lista com Cauda ou Vetor Circular), todas as operações fundamentais (`enqueue`, `dequeue`, `front`, `isEmpty`) têm complexidade de tempo **$O(1)$**.

As filas são essenciais para gerenciar a ordem de processamento:

- **Sistemas Operacionais:** Gerenciamento de processos (escalonamento) e controle de acesso a recursos (ex: fila de impressão).
- **Algoritmos de Grafos:** A Busca em Largura (BFS) depende inteiramente de uma fila para explorar o grafo nível por nível.
- **Buffers de Rede:** Pacotes de dados são enfileirados (enqueue) à medida que chegam e desenfileirados (dequeue) para processamento, garantindo a ordem.
- **Simulações de Eventos:** Modelagem de qualquer sistema do mundo real onde a ordem de chegada importa.

## Fila Dupla (Deque)

O **Deque (Double-Ended Queue)**, ou fila dupla, é a generalização da pilha e da fila. É uma estrutura de dados que permite **inserções e remoções em ambas as extremidades** (início e fim).

<div align="center">
<img width="600px" src="./img/09-deque.png">
</div>

Um Deque pode se comportar como:

- Uma **Pilha:** Se usarmos apenas `AddFront` e `RemoveFront`.
- Uma **Fila:** Se usarmos apenas `AddRear` e `RemoveFront`.

### Operações Fundamentais

Um TAD Deque é definido por sete operações principais:

1. **AddFront(valor):** Adiciona um elemento no início do deque.
2. **AddRear(valor):** Adiciona um elemento no final do deque.
3. **RemoveFront():** Remove e retorna o elemento do início.
4. **RemoveRear():** Remove e retorna o elemento do final.
5. **Front() (ou PeekFront):** Retorna o elemento do início sem removê-lo.
6. **Rear() (ou PeekBack):** Retorna o elemento do final sem removê-lo.
7. **isEmpty():** Verifica se o deque está vazio.

### Implementação de Deques

A implementação mais natural e eficiente de um Deque é com uma **Lista Duplamente Encadeada**. Esta estrutura, que possui ponteiros `proximo` e `anterior` em cada nó, além de referências `head` e `tail`, permite que todas as seis operações de manipulação (adição e remoção em ambas as pontas) sejam realizadas em tempo constante.

**Pseudocódigo (Deque com Lista Duplamente Encadeada):**

```
classe DequeListaDupla
    head = NULL
    tail = NULL

    procedimento AddFront(valor)
        novoNo = novo Nó(valor)
        novoNo.proximo = head
        novoNo.anterior = NULL
        
        se isEmpty()
            head = novoNo
            tail = novoNo
        senão
            head.anterior = novoNo
            head = novoNo
        fim_se
    fim_procedimento
    
    procedimento AddRear(valor)
        novoNo = novo Nó(valor)
        novoNo.proximo = NULL
        novoNo.anterior = tail
        
        se isEmpty()
            head = novoNo
            tail = novoNo
        senão
            tail.proximo = novoNo
            tail = novoNo
        fim_se
    fim_procedimento
    
    função RemoveFront()
        se isEmpty()
            lancar Erro("Deque vazio")
        fim_se
        
        valorRemovido = head.dado
        noTemporario = head
        head = head.proximo
        
        se head != NULL
            head.anterior = NULL
        senão // O deque ficou vazio
            tail = NULL
        fim_se
        
        liberar_memoria(noTemporario)
        retornar valorRemovido
    fim_função
    
    função RemoveRear()
        se isEmpty()
            lancar Erro("Deque vazio")
        fim_se
        
        valorRemovido = tail.dado
        noTemporario = tail
        tail = tail.anterior
        
        se tail != NULL
            tail.proximo = NULL
        senão // O deque ficou vazio
            head = NULL
        fim_se
        
        liberar_memoria(noTemporario)
        retornar valorRemovido
    fim_função
    
    // ... (Front, Rear, isEmpty) ...
fim_classe
```

### Análise e Aplicações

Graças à lista duplamente encadeada, **todas as operações de um Deque têm complexidade $O(1)$**.

Deques são usados em:

- **Algoritmos de Janela Deslizante (Sliding Window):** Usados para encontrar máximos ou mínimos em subconjuntos de um vetor, onde elementos são adicionados de um lado e removidos do outro.
- **Algoritmos de Cache:** A política de cache LRU (Least Recently Used) pode ser implementada com um Deque e uma Tabela Hash.
- **Sistemas de Agendamento:** Podem ser usados para implementar filas de prioridade onde inserções e remoções de alta/baixa prioridade ocorrem em pontas opostas.

## Comparação e Critérios de Escolha

As estruturas lineares que vimos (Vetor, Lista Encadeada, Pilha, Fila, Deque) podem parecer semelhantes, mas resolvem problemas fundamentalmente diferentes. É crucial entender a diferença entre uma **estrutura de dados (implementação)** e um **Tipo Abstrato de Dado (comportamento)**.

- **Estruturas (Implementação):** Vetor (memória contígua), Lista Encadeada (nós com ponteiros).
- **TADs (Comportamento):** Pilha (LIFO), Fila (FIFO), Deque (ambas as pontas).

Um TAD (como a Pilha) pode ser implementado usando uma estrutura (como um Vetor _ou_ uma Lista Encadeada).

A escolha de qual usar depende do problema:

1. **Preciso de acesso rápido por índice ($O(1)$) e o tamanho é fixo?**
    - Use um **Vetor** (ou Matriz, para múltiplas dimensões).

2. **Preciso de tamanho dinâmico e muitas inserções/remoções no _meio_ (e a busca $O(n)$ não é problema)?**
    - Use uma **Lista Encadeada** (preferencialmente Duplamente Encadeada).

3. **Preciso apenas gerenciar dados onde o último a entrar deve ser o primeiro a sair (LIFO)?**
    - Use o TAD **Pilha** (implementado com lista ou vetor dinâmico).

4. **Preciso gerenciar dados onde o primeiro a entrar deve ser o primeiro a sair (FIFO)?**
    - Use o TAD **Fila** (implementado com lista ou vetor circular).

5. **Preciso de inserções e remoções eficientes em _ambas_ as extremidades?**
    - Use o TAD **Deque** (implementado com lista duplamente encadeada).

Cada estrutura define regras de uso muito claras. Compreender o comportamento e as características de cada uma é essencial para selecionar a ferramenta mais adequada para a situação.

## Considerações Finais

Este capítulo forneceu uma visão detalhada das estruturas de dados lineares restritas: Pilha, Fila e Deque. Vimos que, embora possam ser implementadas sobre vetores ou listas, seu verdadeiro poder reside na abstração de seus comportamentos (LIFO e FIFO), que fornecem operações de complexidade $O(1)$ para padrões de acesso muito comuns. Essas estruturas são "peças de Lego" fundamentais na caixa de ferramentas de qualquer programador.

No próximo capítulo, faremos uma transição significativa. Deixaremos para trás as estruturas lineares, onde cada elemento tem (no máximo) um sucessor e um predecessor, e entraremos no mundo das estruturas não lineares e hierárquicas, começando com **Árvores**, que permitem representações de dados muito mais complexas e eficientes.
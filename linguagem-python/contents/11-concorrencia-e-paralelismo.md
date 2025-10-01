# Capítulo 11 – Execução Concorrente e Paralela

Nos capítulos anteriores, construímos programas que, em sua essência, executam de forma **serial**. Isso significa que o interpretador Python executa uma instrução de cada vez, em uma sequência linear e previsível. Uma linha de código só começa a ser executada após a anterior ter sido completamente finalizada. Este modelo é simples e eficaz para a grande maioria das tarefas, mas pode se tornar um gargalo de desempenho em um mundo onde até os computadores mais simples possuem processadores com múltiplos núcleos.

A realidade do hardware moderno é que nossos processadores são capazes de realizar várias tarefas simultaneamente. Surge, então, a questão: como podemos projetar nossos programas para quebrar a barreira da execução linear e aproveitar todo esse poder de processamento? A resposta está nos conceitos de **concorrência** e **paralelismo**.

Neste capítulo, faremos uma imersão nas ferramentas que o Python nos oferece para gerenciar a execução de múltiplas tarefas, visando otimizar o tempo de resposta e a performance de nossas aplicações. Começaremos distinguindo os conceitos fundamentais de execução serial e paralela e, em seguida, exploraremos as duas principais abordagens do Python para este desafio: os módulos `threading` e `multiprocessing`.

## Execução Serial vs. Paralela: Uma Mudança de Paradigma

Via de regra, a execução de código em Python segue o modelo serial. Uma única tarefa é enviada para um único núcleo do processador, que a executa do início ao fim. Somente após sua conclusão é que a próxima tarefa pode ser iniciada.

Com o advento dos processadores _multi-core_, tornou-se possível adotar um modelo de **execução paralela**. Neste paradigma, um problema maior pode ser dividido em subtarefas independentes, e cada uma dessas subtarefas pode ser atribuída a um núcleo de processamento diferente para ser executada **ao mesmo tempo**.

A imagem a seguir ilustra perfeitamente essa diferença conceitual:

<div align="center">
<img width="520px" src="./img/11-execucao-serial-e-paralela.png">
</div>

- **À esquerda (Serial):** Temos um problema único sendo processado em sua totalidade por um único núcleo (_Core_). A execução é sequencial, uma etapa após a outra, dentro de uma única fila.
- **À direita (Parallel):** O mesmo problema é quebrado em múltiplas subtarefas (_Subtask_). Cada subtarefa é então processada simultaneamente por um núcleo diferente (_Core 1, Core 2, Core 3_). O tempo total para resolver o problema pode ser drasticamente reduzido, pois o trabalho é distribuído.

A capacidade de paralelizar rotinas é especialmente importante em domínios onde o Python brilha, como a ciência de dados e o aprendizado de máquina, nos quais operações matemáticas pesadas podem ser distribuídas entre vários núcleos para acelerar a análise e o treinamento de modelos.

## As Ferramentas do Python: `threading` e `multiprocessing`

Para explorar essas capacidades, o Python oferece duas abordagens principais em sua biblioteca padrão, cada uma adequada para um tipo diferente de problema:

- **`threading`:** Este módulo permite a criação de múltiplas _threads_ (ou linhas de execução) dentro de um mesmo processo. As threads compartilham o mesmo espaço de memória, o que facilita a comunicação entre elas. É a ferramenta ideal para gerenciar tarefas que envolvem muita espera, como fazer requisições de rede, acessar um banco de dados ou ler e escrever em arquivos.
- **`multiprocessing`:** Este módulo contorna algumas limitações do `threading` ao criar processos completamente novos e independentes, cada um com seu próprio interpretador Python e seu próprio espaço de memória. Esta é a ferramenta para se alcançar o **verdadeiro paralelismo** em Python, ideal para tarefas computacionalmente intensivas (que usam muita CPU).

Antes de mergulharmos nos exemplos práticos de cada um, é crucial entender uma distinção teórica fundamental que guiará a escolha entre `threading` e `multiprocessing`: a diferença entre concorrência e paralelismo.

## Concorrência com o Módulo `threading`

O módulo `threading` é a ferramenta padrão do Python para se trabalhar com **concorrência**. Ele nos permite criar e gerenciar múltiplas _threads_ dentro de um único processo do nosso programa. Antes de prosseguirmos, é essencial definir o que é uma thread.

Uma **thread** (ou linha de execução) é a menor unidade de código que pode ser gerenciada de forma independente pelo sistema operacional. Dentro de um mesmo programa (ou processo), podemos ter várias threads rodando "ao mesmo tempo". A principal característica delas é que todas compartilham os mesmos recursos de memória do processo principal. Isso significa que diferentes threads podem acessar e modificar as mesmas variáveis, o que facilita a comunicação, mas também introduz a necessidade de cuidados especiais, como veremos mais à frente.

### Concorrência vs. Paralelismo

Como mencionado anteriormente, é crucial distinguir concorrência de paralelismo.

- **Concorrência** é a capacidade de um sistema gerenciar múltiplas tarefas e fazê-las progredir em um mesmo período de tempo. O processador alterna rapidamente entre as tarefas, dando a ilusão de que estão sendo executadas simultaneamente.
- **Paralelismo** é a execução _real e simultânea_ de múltiplas tarefas, o que exige múltiplos núcleos de processamento.

O `threading` em Python é uma ferramenta para **concorrência**, mas, na implementação padrão (CPython), ele não alcança o verdadeiro paralelismo para tarefas que exigem uso intensivo da CPU. O motivo para isso é uma característica interna do Python chamada **GIL (Global Interpreter Lock)**, ou "Trava Global do Interpretador".

O GIL é um mecanismo de proteção que permite que apenas **uma única thread execute o bytecode Python por vez** dentro de um processo. Mesmo em um computador com oito núcleos, se você criar oito threads para realizar um cálculo matemático pesado, o GIL garantirá que apenas uma delas esteja realmente utilizando a CPU em um determinado instante, enquanto as outras sete esperam.

Então, por que o `threading` é útil? A resposta está em tarefas limitadas por **E/S (Entrada/Saída)**, ou _I/O-bound_. Quando uma thread executa uma operação de E/S — como fazer uma requisição a uma API na internet, ler um arquivo grande do disco ou se conectar a um banco de dados — ela entra em um estado de espera. Durante esse tempo de espera, o Python é inteligente o suficiente para **liberar o GIL**, permitindo que outra thread assuma o controle da CPU e execute seu trabalho.

Portanto, o `threading` é a escolha ideal para programas que precisam realizar múltiplas tarefas que envolvem "esperar" por recursos externos, pois permite que o programa continue produtivo enquanto uma ou mais threads estão ociosas.

### `threading` na Prática

Para criar e gerenciar threads, utilizamos o módulo `threading`, que já faz parte da biblioteca padrão do Python.

O processo básico envolve:

1. Definir uma função que será o "alvo" (_target_) da nossa thread.
2. Criar um objeto `threading.Thread`, especificando a função alvo e seus argumentos.
3. Iniciar a thread com o método `.start()`.
4. Opcionalmente, esperar a thread terminar com o método `.join()`.

Vamos analisar um exemplo que simula a execução de duas tarefas que levam algum tempo para serem concluídas (simulado com `time.sleep()`).

```python
import threading
import time

def tarefa(nome, duracao):
    """Uma função simples que simula uma tarefa demorada."""
    print(f"Thread {nome}: iniciando sua tarefa de {duracao} segundos.")
    time.sleep(duracao) # Simula uma operação de E/S ou um trabalho demorado
    print(f"Thread {nome}: terminando.")

# --- Execução Serial (para comparação) ---
print("--- Iniciando execução serial ---")
inicio_serial = time.time()
tarefa("Serial A", 2)
tarefa("Serial B", 2)
fim_serial = time.time()
print(f"Execução serial levou {fim_serial - inicio_serial:.2f} segundos.\n")


# --- Execução Concorrente com Threads ---
print("--- Iniciando execução concorrente ---")
inicio_threads = time.time()

# 1. Criando os objetos Thread
# target: a função que a thread irá executar
# args: uma tupla com os argumentos para a função target
thread1 = threading.Thread(target=tarefa, args=("A", 2))
thread2 = threading.Thread(target=tarefa, args=("B", 2))

# 2. Iniciando a execução das threads
thread1.start()
thread2.start()

# 3. Esperando que ambas as threads terminem
# O programa principal pausa aqui até que thread1 finalize
thread1.join()
# O programa principal pausa aqui até que thread2 finalize
thread2.join()

fim_threads = time.time()
print(f"Execução concorrente levou {fim_threads - inicio_threads:.2f} segundos.")
```

A saída será algo como:

```
--- Iniciando execução serial ---
Thread Serial A: iniciando sua tarefa de 2 segundos.
Thread Serial A: terminando.
Thread Serial B: iniciando sua tarefa de 2 segundos.
Thread Serial B: terminando.
Execução serial levou 4.00 segundos.

--- Iniciando execução concorrente ---
Thread A: iniciando sua tarefa de 2 segundos.
Thread B: iniciando sua tarefa de 2 segundos.
Thread A: terminando.
Thread B: terminando.
Execução concorrente levou 2.00 segundos.
```

A análise da saída é clara: na execução serial, as tarefas são executadas uma após a outra, somando seus tempos. Na execução concorrente, ambas as threads são iniciadas quase simultaneamente. Enquanto uma "dorme" (simulando a espera por E/S), a outra pode progredir. O tempo total é ditado pela tarefa mais longa, não pela soma de todas.

O método `.join()` é crucial para a sincronização. Ele bloqueia a execução do programa principal, forçando-o a esperar até que a thread na qual foi chamado complete seu trabalho. Sem o `.join()`, o programa principal poderia terminar antes das threads, potencialmente encerrando-as de forma abrupta.

### Sincronização e Condições de Corrida

O fato de as threads compartilharem a mesma memória é uma faca de dois gumes. Facilita a comunicação, mas cria o risco de **condições de corrida** (_race conditions_). Isso ocorre quando múltiplas threads tentam ler e modificar o mesmo recurso (como uma variável ou um arquivo) ao mesmo tempo, o que pode levar a resultados inconsistentes e imprevisíveis.

Para evitar esses problemas, o módulo `threading` oferece vários mecanismos de **sincronização**. Embora a implementação detalhada seja um tópico avançado, é importante conhecer os principais conceitos:

- **`Lock` (Trava):** É o mecanismo mais básico. Funciona como uma única chave para um recurso. Uma thread "adquire" o `lock` antes de acessar o recurso compartilhado e o "libera" quando termina. Enquanto uma thread detém o `lock`, qualquer outra thread que tente adquiri-lo ficará bloqueada, esperando a liberação.
- **`RLock` (Trava Reentrante):** Uma trava que pode ser adquirida múltiplas vezes pela mesma thread, sem causar um impasse (_deadlock_).
- **Semáforos:** São como `locks`, mas permitem que um número limitado (maior que um) de threads acesse um recurso simultaneamente.
- **Eventos:** Um mecanismo simples de comunicação, onde uma thread pode sinalizar um "evento" para outras threads, que podem estar esperando por ele.
- **Barreiras:** Sincroniza um número fixo de threads em um ponto específico do código. Todas as threads devem chegar à barreira antes que qualquer uma delas possa continuar.


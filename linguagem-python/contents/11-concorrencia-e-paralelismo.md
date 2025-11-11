# Capítulo 11 – Execução Concorrente e Paralela

Nos capítulos anteriores, construímos programas que, em sua essência, executam de forma **serial**. Isso significa que o interpretador Python executa uma instrução de cada vez, em uma sequência linear e previsível. Uma linha de código só começa a ser executada após a anterior ter sido completamente finalizada. Este modelo é simples e eficaz para a grande maioria das tarefas, mas pode se tornar um gargalo de desempenho em um mundo onde até os computadores mais simples possuem processadores com múltiplos núcleos.

A realidade do hardware moderno é que nossos processadores são capazes de realizar várias tarefas simultaneamente. Surge, então, a questão: como podemos projetar nossos programas para quebrar a barreira da execução linear e aproveitar todo esse poder de processamento? A resposta está nos conceitos de **concorrência** e **paralelismo**.

Neste capítulo, faremos uma imersão nas ferramentas que o Python nos oferece para gerenciar a execução de múltiplas tarefas, visando otimizar o tempo de resposta e a performance de nossas aplicações. Começaremos distinguindo os conceitos fundamentais de execução serial e paralela e, em seguida, exploraremos as duas principais abordagens do Python para este desafio: os módulos `threading` e `multiprocessing`.

## Execução Serial vs. Paralela

Via de regra, a execução de código em Python segue o modelo serial. Uma única tarefa é enviada para um único núcleo do processador, que a executa do início ao fim. Somente após sua conclusão é que a próxima tarefa pode ser iniciada.

Com o advento dos processadores _multi-core_, tornou-se possível adotar um modelo de **execução paralela**. Neste paradigma, um problema maior pode ser dividido em subtarefas independentes, e cada uma dessas subtarefas pode ser atribuída a um núcleo de processamento diferente para ser executada **ao mesmo tempo**.

A imagem a seguir ilustra perfeitamente essa diferença conceitual:

<div align="center">
<img width="520px" src="./img/11-execucao-serial-e-paralela.png">
</div>

- **À esquerda (Serial):** Temos um problema único sendo processado em sua totalidade por um único núcleo (_Core_). A execução é sequencial, uma etapa após a outra, dentro de uma única fila.
- **À direita (Parallel):** O mesmo problema é quebrado em múltiplas subtarefas (_Subtask_). Cada subtarefa é então processada simultaneamente por um núcleo diferente (_Core 1, Core 2, Core 3_). O tempo total para resolver o problema pode ser drasticamente reduzido, pois o trabalho é distribuído.

A capacidade de paralelizar rotinas é especialmente importante em domínios onde o Python brilha, como a ciência de dados e o aprendizado de máquina, nos quais operações matemáticas pesadas podem ser distribuídas entre vários núcleos para acelerar a análise e o treinamento de modelos.

## Ferramentas do Python: `threading` e `multiprocessing`

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

## Paralelismo com o Módulo `multiprocessing`

Enquanto o `threading` nos oferece uma solução elegante para a **concorrência** em tarefas de E/S, ele é fundamentalmente limitado pelo GIL em tarefas que exigem uso intensivo da CPU. Para superar essa barreira e alcançar o **paralelismo real**, o Python nos oferece o módulo `multiprocessing`.

A abordagem do `multiprocessing` é radicalmente diferente: em vez de criar _threads_ dentro de um mesmo processo, ele cria **processos completamente novos e independentes**. Cada um desses processos obtém seu próprio interpretador Python e seu próprio espaço de memória isolado. Como consequência, cada processo é executado em um núcleo de CPU diferente (se disponível) e não é afetado pelo GIL. Isso torna o `multiprocessing` a ferramenta definitiva para acelerar tarefas computacionalmente pesadas (_CPU-bound_), como cálculos matemáticos complexos, processamento de imagens ou simulações.

A arquitetura do `multiprocessing` se baseia em três componentes principais:

- **Processos:** A criação e o gerenciamento de processos paralelos através da classe `Process`.
- **Comunicação entre Processos (IPC):** Um conjunto de ferramentas para permitir que processos, que não compartilham memória, troquem informações de forma segura.
- **Sincronização:** Mecanismos para coordenar a execução e o acesso a recursos compartilhados entre diferentes processos.

### `multiprocessing` na Prática

A criação de um processo é sintaticamente muito similar à criação de uma thread. No entanto, há um detalhe crucial que é indispensável ao usar `multiprocessing`.

#### Guarda `if __name__ == '__main__':`

Todo script que utiliza o módulo `multiprocessing` deve ter seu código principal protegido dentro de um bloco `if __name__ == '__main__':`. Esta verificação garante que o código de criação dos processos seja executado apenas quando o script é rodado diretamente, e não quando ele é importado como um módulo.

Isso é essencial porque, em alguns sistemas operacionais (como o Windows), a criação de um novo processo envolve a importação do script original. Sem essa "guarda", o novo processo tentaria, por sua vez, criar novos processos, levando a um ciclo infinito de recriação que quebraria o programa.

#### Exemplo Básico de `multiprocessing`

Vamos adaptar nosso exemplo anterior para usar processos em vez de threads.

```python
import multiprocessing
import time

def tarefa(nome):
    """Uma função simples que simula uma tarefa demorada."""
    print(f"Processo {nome}: iniciando.")
    # Simula um cálculo pesado que usa 100% da CPU por 2 segundos
    inicio = time.time()
    while time.time() - inicio < 2:
        pass 
    print(f"Processo {nome}: terminando.")

# A guarda é essencial para o funcionamento correto do multiprocessing
if __name__ == '__main__':
    print("--- Iniciando execução com multiprocessing ---")
    inicio_paralelo = time.time()

    # 1. Criando os objetos Process
    p1 = multiprocessing.Process(target=tarefa, args=("A",))
    p2 = multiprocessing.Process(target=tarefa, args=("B",))

    # 2. Iniciando a execução dos processos
    p1.start()
    p2.start()

    # 3. Esperando que ambos os processos terminem
    p1.join()
    p2.join()

    fim_paralelo = time.time()
    print("--- Finalizando a execução principal ---")
    print(f"Execução paralela levou {fim_paralelo - inicio_paralelo:.2f} segundos.")
```

A saída será similar a:

```
--- Iniciando execução com multiprocessing ---
Processo A: iniciando.
Processo B: iniciando.
Processo A: terminando.
Processo B: terminando.
--- Finalizando a execução principal ---
Execução paralela levou 2.01 segundos.
```

Assim como no `threading`, os métodos `.start()` e `.join()` têm a mesma função: o primeiro inicia o processo e o segundo faz o programa principal esperar por sua conclusão. O resultado demonstra o paralelismo real: mesmo simulando uma tarefa que ocupa a CPU, as duas tarefas são executadas ao mesmo tempo em núcleos diferentes, e o tempo total é o da tarefa mais longa.

### Comunicação Entre Processos (IPC)

O isolamento de memória que permite o paralelismo também cria um desafio: como os processos podem trocar informações? Para isso, o `multiprocessing` oferece várias ferramentas de **Comunicação Entre Processos** (_Inter-Process Communication_ - IPC).

- **`Queue` (Fila):** É a forma mais comum e flexível de comunicação. Uma `Queue` é uma estrutura de dados segura para processos, onde um ou mais processos podem adicionar itens (`.put()`) e um ou mais processos podem remover itens (`.get()`). Funciona como uma esteira de produção, garantindo que os dados sejam trocados de forma ordenada e sincronizada.
- **`Pipe` (Tubo):** Cria um canal de comunicação bidirecional entre **dois** processos. Um `Pipe` retorna dois objetos de conexão, um para cada "ponta" do tubo. O que é enviado por uma ponta (`.send()`) pode ser recebido pela outra (`.recv()`). É mais simples que uma `Queue`, mas limitado a dois processos.
- **Memória Compartilhada (`Value` e `Array`):** Para casos mais específicos, o `multiprocessing` permite criar uma área de memória compartilhada que pode ser acessada por diferentes processos.
    - `Value`: Permite compartilhar uma única variável (como um número inteiro ou de ponto flutuante).
    - Array: Permite compartilhar um array de valores de um tipo primitivo.
        O acesso a esses objetos deve ser controlado com mecanismos de sincronização, como Lock, para evitar condições de corrida.
- **`Manager` (Gerenciador):** Para compartilhar objetos Python mais complexos (como listas e dicionários), o `multiprocessing` oferece os `Managers`. Um `Manager` controla um processo servidor que hospeda os objetos Python, e permite que outros processos acessem e modifiquem esses objetos de forma transparente através de _proxies_. É a forma mais conveniente de compartilhar estados complexos, embora seja um pouco mais lenta que os outros mecanismos.

## Considerações Finais

Neste capítulo, quebramos a barreira da execução sequencial e exploramos o universo da otimização de desempenho em Python. Compreendemos que, para aproveitar ao máximo o poder dos processadores modernos, é preciso ir além do modelo linear e adotar estratégias que permitam a execução de múltiplas tarefas de forma concorrente ou paralela.

A distinção fundamental entre **concorrência** (gerenciar múltiplas tarefas em um mesmo período, ideal para esperas) e **paralelismo** (executar múltiplas tarefas ao mesmo tempo, ideal para processamento) foi nosso guia.

Exploramos o módulo `threading` como a ferramenta de escolha para a concorrência, perfeita para otimizar programas **limitados por E/S (I/O-bound)**, onde a maior parte do tempo é gasta esperando por redes ou discos. Reconhecemos, no entanto, a limitação imposta pelo **GIL (Global Interpreter Lock)**, que impede o paralelismo real para código Python puro.

Para o paralelismo verdadeiro em tarefas **limitadas pela CPU (CPU-bound)**, desvendamos o módulo `multiprocessing`. Vimos como ele contorna o GIL ao criar processos independentes, cada um com seu próprio interpretador e memória, permitindo que cálculos pesados sejam distribuídos entre múltiplos núcleos. Entendemos também que essa separação introduz a necessidade de mecanismos de **Comunicação Entre Processos (IPC)**, como `Queues` e `Pipes`, para a troca segura de informações.

A principal lição deste capítulo é que a escolha entre `threading` e `multiprocessing` é uma decisão estratégica, ditada pela natureza do problema a ser resolvido. A chave é identificar o gargalo de desempenho do seu programa: ele passa a maior parte do tempo "esperando" ou "calculando"? A resposta a essa pergunta indicará o caminho para a otimização mais eficaz.

Armados com a capacidade de escrever código que não apenas executa, mas o faz de forma otimizada, estamos agora preparados para levar nossas habilidades para o próximo nível: a construção de aplicações completas e a interação com o vasto ecossistema de bibliotecas que o Python oferece para resolver problemas do mundo real.
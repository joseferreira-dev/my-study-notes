<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/sistemas-operacionais"><img src="../../banner-so.png"></a>
</div>
<br>

# Gerência de Processos

- [Introdução](#introdução)


## Introdução

A gerência de processos é uma das principais funções de um sistema operacional (SO). Ela envolve a criação, execução, suspensão e terminação de processos, além de gerenciar a alternância entre eles para garantir o uso eficiente da CPU.

Um processo é um programa em execução. É mais do que apenas o código do programa; inclui também o seu estado atual, variáveis, contador de programa, e outros recursos necessários para a sua execução. Por exemplo, quando você abre o Microsoft Word no seu computador, o SO cria um processo para o Word, que inclui o código do programa, a janela que você vê, e os dados que você está editando.

Para isso, o sistema utiliza uma CPU virtual, o que permite que o SO crie a ilusão de que cada processo tem uma CPU dedicada a ele, mesmo que a CPU física seja compartilhada entre múltiplos processos. Isso é conseguido através da técnica de multiprogramação, onde o SO alterna rapidamente entre processos, dando a cada um uma fatia de tempo da CPU.

Essa alternância entre processos, também conhecida como troca de contexto, é o mecanismo pelo qual o SO salva o estado de um processo atualmente em execução e carrega o estado de um novo processo a ser executado. O estado de um processo inclui o conteúdo dos registradores da CPU, o contador de programa, e outras informações essenciais para retomar a execução. Isso permite que vários processos pareçam estar sendo executados simultaneamente, mesmo em um sistema com uma única CPU.

Entende-se multiprogramação como a capacidade do SO de gerenciar múltiplos processos ao mesmo tempo. Ele mantém vários processos na memória principal e alterna entre eles para utilizar a CPU de forma mais eficiente. Por exemplo, enquanto você está escrevendo um documento no Word, o Excel pode estar recalculando uma planilha em segundo plano e o Outlook pode estar sincronizando emails.

Vamos considerar um cenário comum onde você está usando vários programas do Microsoft Office ao mesmo tempo: Word, Excel, e Outlook.

1. **Abertura do Word**: Quando você abre o Word, o SO cria um novo processo para ele. Este processo inclui o código do Word, a interface gráfica do usuário, e os dados do documento que você está editando.
2. **Abertura do Excel**: Em seguida, você abre o Excel. O SO cria um novo processo para o Excel, que é independente do processo do Word, mas ambos podem ser executados simultaneamente.
3. **Recebimento de Email no Outlook**: Enquanto você está trabalhando no Word e no Excel, o Outlook pode receber novos emails. O SO gerencia um processo separado para o Outlook, que sincroniza emails em segundo plano sem interferir com o Word ou o Excel.
4. **Multiprogramação e Alternância**: O SO utiliza a multiprogramação para alternar rapidamente entre o Word, Excel, e Outlook. Por exemplo, ele pode dar uma fatia de tempo da CPU ao Word para processar entradas do teclado, depois alternar para o Excel para recalcular uma planilha, e então mudar para o Outlook para sincronizar emails. Durante cada alternância, o SO realiza uma troca de contexto, salvando o estado atual do processo em execução e carregando o estado do próximo processo.
5. **Troca de Contexto**: Quando você alterna de volta para o Word, o SO restaura o estado do processo do Word, permitindo que você continue exatamente de onde parou. Este processo de salvar e restaurar estados é a troca de contexto.

## Criação, Término e Hierarquia de Processos

A criação de processos em um sistema operacional pode ocorrer por diversos motivos, e existem quatro eventos principais que desencadeiam a criação de novos processos:

 - **Inicialização do Sistema**: Quando o sistema operacional é iniciado, ele cria vários processos necessários para o funcionamento básico do sistema. Alguns desses processos interagem diretamente com o usuário (processos de primeiro plano ou foreground), enquanto outros operam em segundo plano (processos de background), conhecidos como daemons. Por exemplo, um daemon de impressão aguarda por pedidos de impressão.
- **Chamada de Sistema**: Um processo em execução pode solicitar a criação de um novo processo através de uma chamada de sistema. No Linux, por exemplo, a função `fork()` é usada para criar um processo filho que é uma cópia do processo pai.
- **Pedido do Usuário**: Um usuário pode iniciar a criação de um processo ao interagir com a interface do sistema, como clicar duas vezes em um ícone para abrir um documento do Word. Isso faz com que o SO crie um novo processo para executar o Word.
- **Início de Tarefa em Lote**: Em sistemas de grande porte, tarefas em lote podem ser iniciadas automaticamente pelo SO ou por administradores, resultando na criação de novos processos para cada tarefa.

Cada processo possui diversos componentes importantes, além do código do programa:

- **Contexto de Software**: Inclui informações como o nome do processo, identificador do processo (PID), identificador do processo pai (PPID), identificador do proprietário/usuário (UID), e prioridade de execução.
- **Contexto de Hardware**: Inclui os valores dos registradores da CPU quando o processo está em execução.
- **Espaço de Endereçamento**: Contém o código do programa, dados, e outras informações necessárias para a execução do processo.

Um esquema de um progeto poderia ser:

<div align="center">
  <img src="01-esquema-processo.png" width="500px">
</div>
<br/>

Essas informações são armazenadas em uma estrutura de dados conhecida como Bloco de Controle do Processo (PCB - Process Control Block). O PCB é mantido em uma área protegida da memória e contém detalhes críticos como:

- Identificador do processo (PID)
- Valores dos registradores da CPU
- Espaço de endereçamento do processo
- Prioridade do processo
- Outros dados necessários para a gestão do processo

Para gerenciar eficientemente esses PCBs, o SO utiliza uma Tabela de Processo, que permite localizar e acessar rapidamente o PCB de qualquer processo.

Os processos podem terminar por diversas razões, sendo as principais:

- **Término Normal (Voluntário)**: O processo conclui sua tarefa e realiza uma chamada de sistema para informar ao SO que terminou. Por exemplo, um programa que processa dados, exibe resultados e então termina sua execução.
- **Término por Erro (Voluntário)**: O processo encontra um erro durante sua execução, como uma divisão por zero, e decide terminar.
- **Erro Fatal (Involuntário)**: O processo encontra um erro não recuperável, como tentar acessar um arquivo inexistente, e é terminado pelo SO.
- **Eliminado por Outro Processo (Involuntário)**: Um processo pode ser terminado por outro processo, como quando um administrador usa o comando `kill` no Linux para terminar um processo problemático.

Por fim, os processos em um sistema operacional frequentemente possuem uma hierarquia. Cada processo pode ter um pai (o processo que o criou) e pode criar processos filhos. No Linux, o primeiro processo a ser executado após o carregamento do kernel é o `init` (PID = 1), que é responsável por iniciar outros processos.

Por exemplo, considere a seguinte hierarquia de processos:

```mermaid
graph TB;
    A("init (PID = 1)")   --> B("bash (PID = 100)");
    A("init (PID = 1)")   --> D("bash (PID = 101)");
    B("bash (PID = 100)") --> E("programa A (PID = 200)");
    A("init (PID = 1)")   --> C("bash (PID = 102)");
```

Neste cenário, `init` é o processo pai de três instâncias do shell `bash`, e uma dessas instâncias de `bash` é o pai do `programa A`. A hierarquia reflete a relação de criação e controle entre processos no sistema.

## Estados de um Processo

Os processos em um sistema operacional podem estar em diferentes estados, refletindo a sua situação atual em relação ao uso da CPU e a espera por eventos. Os três estados principais de um processo são:

- **Executando**: O processo está atualmente utilizando a CPU para executar suas instruções.
- **Pronto**: O processo está pronto para executar e aguarda uma oportunidade para usar a CPU. Ele está na fila de processos prontos, esperando sua vez.
- **Bloqueado (ou em Espera)**: O processo está esperando por algum evento externo para poder continuar a execução, como a conclusão de uma operação de entrada/saída (E/S).

Os processos transitam entre esses estados de acordo com a sua necessidade de recursos e a disponibilidade da CPU. As quatro transições possíveis entre os estados são:

```mermaid
graph LR;
    Executando  -->|A| Bloqueado;
    Executando  -->|C| Pronto;
    Bloqueado   -->|B| Pronto;
    Pronto      -->|D| Executando;
```

- **Executando >> Bloqueado (A)**: Ocorre quando um processo em execução precisa esperar por um evento externo, como a leitura de dados de um disco. Nesse momento, ele deixa de usar a CPU e entra no estado bloqueado.
- **Bloqueado >> Pronto (B)**: Ocorre quando o evento pelo qual o processo estava esperando é concluído. Por exemplo, após a leitura do arquivo do disco, o processo é notificado que os dados estão disponíveis e ele se move para o estado pronto.
- **Executando >> Pronto (C)**: Ocorre quando o processo em execução excede seu tempo de uso da CPU (fatia de tempo) ou é preemptado por um processo de maior prioridade. Nesse caso, ele deixa a CPU e volta para a fila de processos prontos.
- **Pronto >> Executando (D)**: Ocorre quando a CPU se torna disponível e é atribuída a um dos processos prontos, que então passa a executar.

Para exemplificar, considere um cenário onde um processo está executando e precisa ler dados de um arquivo:

- **Executando >> Bloqueado (A)**: O processo Word está executando e solicita a leitura de dados do arquivo `teste.txt` no disco. Ele entra no estado bloqueado enquanto aguarda a conclusão da leitura.
- **Bloqueado >> Pronto (B)**: Após a leitura do arquivo ser concluída, o processo Word é notificado. Ele então transita para o estado pronto, aguardando a sua vez de utilizar a CPU novamente.
- **Executando >> Pronto (C)**: Se o Word excede seu tempo de uso da CPU antes de completar suas operações, ele é preemptado e volta ao estado pronto, enquanto a CPU é atribuída a outro processo.
- **Pronto >> Executando (D)**: Quando a CPU se torna disponível, o processo Word é selecionado da fila de prontos e volta a executar.

O tempo que um processo passa em cada estado depende do seu comportamento:

- **Processos CPU-bound**: Esses processos realizam operações intensivas de CPU e passam a maior parte do tempo em execução. Um exemplo é a reprodução de um filme, que exige constante processamento de dados de vídeo e áudio. Eles raramente entram no estado bloqueado, apenas retornando ao estado pronto quando a fatia de tempo se esgota.
- **Processos I/O-bound**: Esses processos realizam muitas operações de E/S e passam grande parte do tempo esperando por eventos externos. Um exemplo é um programa de chat, que frequentemente espera pela entrada do usuário. Eles frequentemente entram no estado bloqueado e, após a conclusão das operações de E/S, movem-se para o estado pronto.

## Threads

Um processo tradicional possui um espaço de endereçamento e um único fluxo de controle, que representa a execução do código do programa. No entanto, em certas situações, é vantajoso ter mais de um fluxo de controle e execução dentro do mesmo processo, operando quase em paralelo. Esses fluxos de controle são chamados **threads** (ou processos leves).

Threads permitem que diferentes partes de um programa sejam executadas simultaneamente. Dentro de um processo, todas as threads compartilham o mesmo espaço de endereçamento e a mesma seção de código na memória, mas cada thread tem seu próprio contador de programa, registradores e pilha.

Por exemplo, considere um editor de texto com várias funcionalidades: contador de palavras, contador de páginas, correção ortográfica instantânea, entre outras. Cada funcionalidade pode ser implementada em uma thread separada. Assim, a cada digitação, uma thread pode atualizar a contagem de palavras, outra pode verificar a quantidade de páginas, e outra pode corrigir a ortografia.

Sem threads, essas funcionalidades teriam que ser executadas sequencialmente, o que resultaria em um desempenho inferior. Alternativamente, cada funcionalidade poderia ser implementada como um processo separado, mas isso seria mais pesado em termos de recursos, pois cada processo teria seu próprio espaço de endereçamento e recursos associados.

<div align="center">
  <img src="02-threads-1.png" width="600px">
</div>
<br/>

Como pode ser visto na figura acima, à esquerda se tem três processos independentes, cada um com uma thread possuindo um fluxo de controle. Já à direita, está representado um único processo que contém várias threads, cada uma executando em paralelo.

As threads podem ser categorizadas em dois tipos principais: **threads ao nível do usuário** e **threads ao nível do kernel (núcleo)**.

Em **threads ao nível do usuário**, o gerenciamento das threads é feito no espaço do usuário, ou seja, a criação, sincronização e escalonamento das threads são tratados por uma biblioteca de threads no espaço do usuário, não pelo kernel. Sua principal vantagem é a menor sobrecarga, pois as operações de thread não envolvem chamadas de sistema ao kernel. Contudo, se uma thread realiza uma operação bloqueante, todo o processo pode ser bloqueado, já que o kernel não sabe sobre as threads individuais. Nessa situação, a tabela de processos é gerida pelo kernel e as tabelas de threads são geridas pelo próprio processo (conforme a figura abaixo).

<div align="center">
  <img src="03-threads-2.png" width="400px">
</div>
<br/>

Já nas **threads ao nível do kernel**, o gerenciamento das threads é feito pelo próprio kernel. O kernel é responsável pela criação, sincronização e escalonamento das threads. Isso resulta em um melhor suporte para multiprocessamento, pois o kernel pode gerenciar e escalonar threads em diferentes CPUs. Contudo, isso gera maior sobrecarga, pois as operações de thread envolvem chamadas ao kernel. Aqui, tanto a tabela de processos quanto a tabela de threads são geridas pelo kernel.

<div align="center">
  <img src="04-threads-3.png" width="300px">
</div>
<br/>

## Comunicação entre Processos

A comunicação entre processos (IPC, do inglês Inter-Process Communication) é fundamental para que processos independentes possam coordenar suas ações e compartilhar dados. Vamos explorar como isso funciona, utilizando exemplos e conceitos importantes como pipes, condições de corrida, seções críticas, semáforos e mutexes.

Um dos exemplos mais simples de IPC é o uso de pipes no shell Unix/Linux. Um pipe permite que a saída de um comando seja usada como entrada para outro. Por exemplo:

```shell
ls | grep x | sort -r | tee arquivo_saida
```

Aqui, o pipe (`|`) conecta a saída de um comando à entrada do próximo. No exemplo, a saída do comando `ls`, que é a lista de arquivos do diretório, é passada como entrada para `grep x`, onde os dados são filtrados mostrando apenas as linhas que contêm `"x"`, cujo resultado é enviado para `sort -r`, que realiza a sua ordenação em ordem decrescente, e, por fim, `tee arquivo_saida` envia a saída para um arquivo e para o console ao mesmo tempo. Isso permite que vários comandos processem dados em sequência, colaborando para um objetivo comum.

Essa interação entre os processo pode gerar **condições de corrida**, que ocorrem quando dois ou mais processos tentam acessar e modificar dados compartilhados simultaneamente, levando a resultados indeterminados e potenciais erros. Imagine dois processos, A e B, que enviam arquivos para um diretório de spooler de impressão quase ao mesmo tempo. Sem uma coordenação adequada, ambos podem tentar modificar o mesmo espaço de armazenamento, causando corrupção de dados ou outros problemas.

<div align="center">
  <img src="05-condicoes-de-corrida.png" width="360px">
</div>
<br/>

O problema mostrado na figura acima ocorreu porque o processo B começou a utilizar uma das variáveis compartilhadas antes que o processo A tivesse terminado de trabalhar com ela.

Para evitar condições de corrida ou outras situações que envolvem memória compartilhada deve-se encontrar uma maneira de proibir que mais de um processo leia e modifique dados compartilhados ao mesmo tempo. Uma **seção ou região crítica** é uma parte do código onde o recurso compartilhado é acessado. Para garantir a integridade dos dados em regiões críticas, quatro condições devem ser atendidas:

- **Exclusão Mútua**: Dois processos não podem estar simultaneamente dentro da seção crítica.
- **Progresso**: Nenhuma suposição deve ser feita sobre a velocidade ou número de processadores.
- **Espera Finita**: Nenhum processo fora da seção crítica deve bloquear outros processos.
- **Ausência de Inanição**: Nenhum processo deve esperar eternamente para entrar na seção crítica.

Para organizar esses processos usam-se **semáforos**, variáveis utilizadas para gerenciar o acesso a recursos compartilhados. Eles suportam operações atômicas **down** e **up** (ou sleep e wakeup), garantindo que os processos sejam bloqueados ou despertados de maneira segura. É uma variável do tipo inteiro que possui o valor `0` quando não tem nenhum sinal a despertar, ou um valor positivo quando um ou mais sinais para despertar estiverem pendentes.

A operação down verifica se o valor do semáforo é maior que `0`. Se sim, ele é decrementado e o processo continua. Se é `0`, o processo é bloqueado (entra em sleep). Já a operação up incrementa o valor do semáforo. Se havia processos bloqueados, um deles é despertado. É garantido que iniciada uma operação de semáforo, nenhum outro processo pode acessar o semáforo até que a operação tenha terminado ou sido bloqueada. Isso evita as condições de corrida.

<div align="center">
  <img src="06-semaforo.png" width="440px">
</div>
<br/>

Um exemplo de situação seria de um semáforo inicializado com `0` indicando que a região crítica está ocupada. Quatro processos tentam entrar na região crítica e são bloqueados. Quando um processo sai, executa a operação up, incrementando o semáforo para `1`, permitindo que um dos processos bloqueados entre na região crítica. Quando tal processo entrar é aplicada a operação down, passando o valor do semáforo para `1`. Ou seja, o valor do semáforo diz quantos processos podem entrar.

Uma forma mais simplificada de semáforo é um **mutex (mutual exclusion)**, usado quando apenas dois estados são necessários: **livre** ou **ocupado**. Um mutex é utilizado para garantir que somente um processo acesse a seção crítica por vez. Sempre que um processo precisa entrar na seção crítica ele chama `mutex_lock`. Se o mutex está livre, o processo entra e o mutex é marcado como ocupado. Caso contrário, o processo é bloqueado até que o mutex seja liberado. Após sair da seção crítica, o processo chama `mutex_unlock`, liberando o mutex e permitindo que outro processo entre.

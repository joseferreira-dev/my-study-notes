<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/sistemas-operacionais"><img src="../../banner-so.png"></a>
</div>
<br>

# Sistemas Operacionais: Introdução

- [Introdução](#introdução)
- [Funções Principais de um Sistema Operacional](#funções-principais-de-um-sistema-operacional)
- [Componentes de um Sistema Operacional](#componentes-de-um-sistema-operacional)

## Introdução

Os Sistemas Operacionais (SOs) são softwares fundamentais que gerenciam o hardware e os recursos de um computador, além de fornecer uma interface entre o usuário e o hardware. Eles são responsáveis por várias funções essenciais que permitem que o computador opere de forma eficiente e que outros softwares funcionem corretamente. Alguns dos principais sistemas existentes são:

- **Windows**: Desenvolvido pela Microsoft, é amplamente utilizado em desktops e laptops.
- **Linux**: Um sistema operacional de código aberto utilizado em servidores, desktops e dispositivos embarcados.
- **macOS**: Desenvolvido pela Apple, usado em computadores Mac.
- **Android e iOS**: Utilizados em dispositivos móveis como smartphones e tablets.

## Funções Principais de um Sistema Operacional

As principais funções de um sistema operacional são 

- **Segurança e Proteção**:
  - **Controle de Acesso**: Garante que somente usuários e processos autorizados possam acessar determinados recursos.
  - **Autenticação e Autorização**: Verifica a identidade dos usuários e define o que eles podem fazer.
<br/>

- **Interface do Usuário**:
  - **Interface de Linha de Comando (CLI)**: Um método para interagir com o sistema através de comandos de texto.
  - **Interface Gráfica do Usuário (GUI)**: Uma interface mais amigável e visual que permite a interação através de janelas, ícones e menus.

Além dessas funções, um sistema operacional é responsável por quatro tipos de gerenciamento:

- **Gerenciamento de Processos**:
  - **Escalonamento**: Decide a ordem e a duração com que os processos (programas em execução) são executados.
  - **Criação e Terminação de Processos**: Cria novos processos e finaliza os que já foram completados.
  - **Sincronização e Comunicação entre Processos**: Facilita a comunicação e a sincronização entre processos diferentes para evitar conflitos e garantir a coerência dos dados.
<br/>

- **Gerenciamento de Memória**:
  - **Alocação e Liberação de Memória**: Controla como a memória é alocada para os processos e a libera quando não é mais necessária.
  - **Memória Virtual**: Usa técnicas como paginação e segmentação para simular uma quantidade maior de memória RAM do que a fisicamente disponível.
<br/>

- **Gerenciamento de Dispositivos de Entrada/Saída (E/S)**:
  - **Drivers de Dispositivos**: Proporciona uma interface para que os dispositivos de hardware possam se comunicar com o sistema.
  - **Gerenciamento de E/S**: Coordena e controla a operação de dispositivos de entrada e saída, como discos rígidos, impressoras e monitores.
<br/>

- **Gerenciamento de Arquivos/Armazenamento**:
  - **Sistema de Arquivos**: Organiza e gerencia arquivos e diretórios no sistema de armazenamento.
  - **Controle de Acesso**: Define permissões para leitura, escrita e execução de arquivos e diretórios.
<br/>

## Componentes de um Sistema Operacional

Os principais componentes de um sistema operacional são:

- **Núcleo (Kernel)**: O núcleo é a parte central do SO que gerencia os recursos do sistema (CPU, memória, dispositivos de E/S) e permite a interação entre o hardware e o software.
- **Bibliotecas de Sistema**: Conjunto de funções e rotinas que facilitam a interação com o núcleo e os dispositivos de hardware.
- **Shell (ou Interpretador de Comandos)**: Interface que permite ao usuário interagir com o SO, seja através de linha de comando ou de uma interface gráfica.
- **Gerenciador de Tarefas**: Programa que permite ao usuário monitorar e controlar os processos em execução no sistema.

## Modos de um Sistema Operacional (Kernel vs. Usuário)

Em um Sistema Operacional (SO), os modos de operação **kernel** (ou modo núcleo) e **usuário** são dois níveis de privilégio que permitem ao SO controlar o acesso aos recursos do hardware de maneira segura e eficiente.

- **Modo Kernel**: Também conhecido como modo privilegiado, supervisor ou modo núcleo. O núcleo (kernel) do sistema opera neste modo. As principais características desse modo são:
  - **Acesso Total ao Hardware**: Permite ao SO acessar diretamente todo o hardware do sistema, incluindo CPU, memória, e dispositivos de entrada/saída (E/S).
  - **Execução de Instruções Privilegiadas**: O modo kernel pode executar qualquer instrução de CPU, incluindo aquelas que controlam o hardware diretamente e manipulam a memória.
  - **Gestão de Recursos**: O kernel gerencia a alocação de recursos, como tempo de CPU, memória, e dispositivos de E/S, e pode interromper ou modificar processos conforme necessário para manter a estabilidade e segurança do sistema.
  - **Segurança e Estabilidade**: Devido ao seu alto nível de privilégio, erros no código que roda no modo kernel podem comprometer a segurança e a estabilidade do sistema inteiro.
<br/>

- **Modo Usuário**: Também conhecido como modo não privilegiado. A maioria dos aplicativos e processos dos usuários opera neste modo. Suas características são:
  - **Acesso Limitado ao Hardware**: Os processos em modo usuário não podem acessar diretamente o hardware ou memória do sistema. Eles precisam solicitar ao kernel para realizar essas operações.
  - **Execução de Instruções Restritas**: Os processos em modo usuário não podem executar instruções privilegiadas. Qualquer tentativa de fazer isso resultará em uma interrupção que será manipulada pelo kernel.
  - **Isolamento**: Cada processo em modo usuário é isolado dos outros para prevenir que um processo afete diretamente o funcionamento de outro ou do próprio sistema operacional.
  - **Segurança**: O isolamento e as restrições ajudam a proteger o sistema contra erros e ações maliciosas.

A transição entre os modos pode ocorrer de diversas formas, como:

- **Chamadas de Sistema (System Calls)**: Quando um processo em modo usuário precisa realizar uma operação que requer privilégios elevados (como acessar o disco ou alocar memória), ele faz uma chamada de sistema. A chamada de sistema é um mecanismo para solicitar serviços do kernel de maneira segura e controlada.
- **Interrupções e Exceções**: Interrupções (como interrupções de hardware de dispositivos periféricos) e exceções (como divisões por zero) podem fazer o processador mudar para o modo kernel para que o kernel possa tratar esses eventos.

Os principais benefícios dessa separação são:

- **Segurança**: Restringir o acesso direto ao hardware e ao núcleo do sistema ajuda a proteger contra ações maliciosas e falhas acidentais.
- **Estabilidade**: Ao isolar os processos de usuário e limitar suas capacidades, o SO pode evitar que erros em um programa comprometam todo o sistema.
- **Gerenciamento de Recursos**: O kernel pode gerenciar os recursos de forma centralizada, garantindo que todos os processos tenham acesso justo e que o sistema opere de forma eficiente.

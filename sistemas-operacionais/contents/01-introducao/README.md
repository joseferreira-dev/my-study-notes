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

As principais funções de um sistema operacional são:

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

- **Gerenciamento de Arquivos**:
  - **Sistema de Arquivos**: Organiza e gerencia arquivos e diretórios no sistema de armazenamento.
  - **Controle de Acesso**: Define permissões para leitura, escrita e execução de arquivos e diretórios.
<br/>

- **Segurança e Proteção**:
  - **Controle de Acesso**: Garante que somente usuários e processos autorizados possam acessar determinados recursos.
  - **Autenticação e Autorização**: Verifica a identidade dos usuários e define o que eles podem fazer.
<br/>

- **Interface do Usuário**:
  - **Interface de Linha de Comando (CLI)**: Um método para interagir com o sistema através de comandos de texto.
  - **Interface Gráfica do Usuário (GUI)**: Uma interface mais amigável e visual que permite a interação através de janelas, ícones e menus.

## Componentes de um Sistema Operacional

Os principais componentes de um sistema operacional são:

- **Núcleo (Kernel)**: O núcleo é a parte central do SO que gerencia os recursos do sistema (CPU, memória, dispositivos de E/S) e permite a interação entre o hardware e o software.
- **Bibliotecas de Sistema**: Conjunto de funções e rotinas que facilitam a interação com o núcleo e os dispositivos de hardware.
- **Shell**: Interface que permite ao usuário interagir com o SO, seja através de linha de comando ou de uma interface gráfica.
- **Gerenciador de Tarefas**: Programa que permite ao usuário monitorar e controlar os processos em execução no sistema.

<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner_dart.png"></a>
</div>
<br>

# Criação e Estrutura de um Projeto

- [Criando um Projeto](#criando-um-projeto)
- [Estrutura do Projeto](#estrutura-do-projeto)

## Criando um Projeto

Para se criar um novo projeto Dart básico pelo console deve-se utilizar o comando:

```shell
dart create project_name
```

É uma convensão da comunidade separar o nome do projeto com `_`. É possível definir um template inicial para o projeto utilizando a flag `-t`.

```shell
dart create -t console console_project_name
```

Os possíveis templates são: 

- `console`: Cria um projeto de console simples, ideal para aplicativos de linha de comando.
- `package`: Cria um novo pacote Dart, que pode ser uma biblioteca ou um módulo reutilizável.
- `server-shelf`: Cria um projeto de servidor usando o pacote Shelf, uma estrutura de servidor web minimalista para Dart.
- `web`: Cria um projeto para desenvolvimento web, incluindo um servidor de desenvolvimento e a configuração para trabalhar com Dart no navegador.
- `web-simple`: Cria um projeto web simples, que é uma versão mínima de um projeto web.
- `flutter`: Cria um projeto Flutter, que é uma estrutura para construir aplicativos nativos para iOS, Android, web e desktop com uma única base de código.

## Estrutura do Projeto

Quando criamos um projeto de console em Dart usando o comando `dart create -t console console_project`, uma estrutura de diretórios e arquivos é gerada para ajudar a começar com um aplicativo de console simples, que é semelhante aos demais templates existentes. A estrutura típica do projeto é a seguinte:

```
console_project/
├── bin/
│   └── console_project.dart
├── lib/
├── test/
├── .gitignore
├── analysis_options.yaml
├── CHANGELOG.md
├── pubspec.yaml
└── README.md
```

- `bin/console_project.dart`: Arquivo principal do aplicativo de console, onde escrevemos o código para o nosso aplicativo. O nome do arquivo geralmente corresponde ao nome do projeto.
- `lib/`: Pasta usada para armazenar qualquer biblioteca ou código que será compartilhado entre diferentes partes do aplicativo. É útil para organizar componentes maiores.
- `test/`: Pasta que contém arquivos de teste para o projeto.
- `.gitignore`: Arquivo usado para especificar quais arquivos e pastas devem ser ignorados pelo controle de versão Git.
- `analysis_options.yaml`: Arquivo que permite configurar regras de análise estática para o projeto, como linter e regras de estilo de código.
- `CHANGELOG.md`: Arquivo de log de alterações onde se pode documentar as mudanças feitas em cada versão do projeto.
- `pubspec.yaml`: Arquivo de especificação do pacote Dart. Ele define o nome do projeto, a versão, as dependências do projeto, as dependências de desenvolvimento, assets e outras informações de configuração. 
- `README.md`: Arquivo de documentação onde descrevemos o propósito do projeto, como configurá-lo e usá-lo, e qualquer outra informação relevante.

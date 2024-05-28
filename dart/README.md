# Linguagem Dart

Versão: 3.4

## Criando um novo projeto

Para se criar um novo projeto Dart básico pelo console deve-se utilizar o comando:

~~~shell
dart create project_name
~~~

É uma convensão da comunidade separar o nome do projeto com `_`. É possível definir um template inicial para o projeto utilizando a flag `-t`.

~~~shell
dart create -t console console_project_name
~~~

Os possíveis templates são: 

- `console`: Cria um projeto de console simples, ideal para aplicativos de linha de comando.
- `package`: Cria um novo pacote Dart, que pode ser uma biblioteca ou um módulo reutilizável.
- `server-shelf`: Cria um projeto de servidor usando o pacote Shelf, uma estrutura de servidor web minimalista para Dart.
- `web`: Cria um projeto para desenvolvimento web, incluindo um servidor de desenvolvimento e a configuração para trabalhar com Dart no navegador.
- `web-simple`: Cria um projeto web simples, que é uma versão mínima de um projeto web.
- `flutter`: Cria um projeto Flutter, que é uma estrutura para construir aplicativos nativos para iOS, Android, web e desktop com uma única base de código.
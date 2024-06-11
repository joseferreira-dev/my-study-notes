<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Criação e Estrutura de um Projeto Flutter

- [Introdução](#introdução)
- [Criação do Projeto](#criação-do-projeto)
- [Estrutura do Projeto](#estrutura-do-projeto)

## Introdução

Primeiro, deve-se certificar-se de que todas as ferramentas necessárias estejam instaladas e configuradas corretamente. Isso inclui o Flutter SDK, o Android Studio (ou outro IDE de preferência) com as ferramentas necessárias para desenvolvimento Android, e o Xcode para desenvolvimento iOS. Também é importante ter o `flutter` e o `dart` adicionados ao PATH do seu sistema.

## Criação do Projeto

Depois de preparadp o ambiente, pode-se seguir para a criação do projeto Flutter. Um exemplo para o comando de criação de projeto seria:

```shell
flutter create --project-name=novo_projeto --org br.com.nomedaorganizacao --platforms android,ios -a kotlin -i swift ./novo_projeto
```

Detalhando-se cada parte desse comando para se entender o que ele faz:

- `flutter create`: Este é o comando base utilizado para criar um novo projeto Flutter. Ele gera a estrutura básica do projeto, com todos os arquivos e pastas necessários.
- `--project-name=novo_projeto`: Este parâmetro define o nome do projeto. No exemplo, o nome do projeto é `novo_projeto`. Esse nome será utilizado para identificar o projeto e será o nome da pasta raiz onde o projeto será criado. Vale ressaltar que projetos Flutter tem o seu nome separado por `_`.
- `--org br.com.nomedaorganizacao`: Este parâmetro define o identificador da organização. É utilizado para gerar um identificador único para o aplicativo, geralmente no formato de um domínio invertido. No exemplo, "br.com.nomedaorganizacao" será usado como base para os identificadores do pacote do aplicativo (por exemplo, `br.com.nomedaorganizacao.novo_projeto`).
- `--platforms android,ios`: Este parâmetro especifica as plataformas de destino do projeto. No exemplo, o projeto será desenvolvido para as plataformas Android e iOS.
- `-a kotlin`: Este parâmetro define a linguagem de programação que será usada para o código nativo Android. No exemplo, a opção escolhida foi Kotlin.
- `-i swift`: Este parâmetro define a linguagem de programação que será usada para o código nativo iOS. No exemplo, a opção escolhida foi Swift.
- `./novo_projeto`: Este parâmetro final define o caminho onde o novo projeto será criado. No exemplo, o projeto será criado em uma nova pasta chamada "novo_projeto" no diretório atual.

## Estrutura do Projeto

Depois de executado o comando de criação no terminal, o Flutter vai criar uma nova estrutura de projeto com todas as configurações especificadas. O processo pode levar alguns minutos, dependendo da velocidade do computador.

A estrutura do projeto criado incluirá:

- Uma pasta `lib` contendo o código Dart principal do aplicativo.
- Uma pasta `android` contendo o código nativo Android em Kotlin.
- Uma pasta `ios` contendo o código nativo iOS em Swift.
- Um arquivo `pubspec.yaml` onde se pode gerenciar as dependências do seu projeto.
- Diversos arquivos de configuração e metadados necessários para o Flutter e para as plataformas de destino (Android e iOS).

Depois que o projeto for criado, pode-se abri-lo na IDE desejada (Android Studio, VS Code, etc.). O Flutter vem com uma série de ferramentas e comandos úteis para desenvolvimento, como `flutter run` para executar o aplicativo em um emulador ou dispositivo conectado, e `flutter build` para criar os pacotes de distribuição.

A partir daí, pode-se começar a desenvolver a aplicação, escrevendo o código Dart dentro da pasta `lib`.

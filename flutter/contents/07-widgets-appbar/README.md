<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: AppBar

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `AppBar` no Flutter é um componente fundamental para a criação de barras de aplicativo que seguem as diretrizes do Material Design. Ele fornece uma estrutura consistente e personalizável para a parte superior das interfaces de usuário de aplicativos móveis, incluindo títulos, botões de ação e opções de navegação.

## Estrutura Básica e Funcionamento

O `AppBar` é geralmente utilizado como parte do widget `Scaffold`, que é o principal layout do Flutter para aplicativos. Ele pode incluir um título, ações como botões de pesquisa ou configurações, e opcionalmente um menu de navegação lateral (drawer) ou um botão de voltar (leading) para a navegação entre telas. Abaixo está um exemplo completo demonstrando o uso do `AppBar` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de AppBar'),
          leading: IconButton(
            icon: Icon(Icons.menu),
            onPressed: () {
              // Ação do botão de menu
            },
          ),
          actions: <Widget>[
            IconButton(
              icon: Icon(Icons.search),
              onPressed: () {
                // Ação do botão de pesquisa
              },
            ),
            IconButton(
              icon: Icon(Icons.more_vert),
              onPressed: () {
                // Ação do botão de mais opções
              },
            ),
          ],
          backgroundColor: Colors.blue,
          elevation: 4.0,
          centerTitle: true,
        ),
        body: Center(
          child: Text('Conteúdo principal do aplicativo'),
        ),
      ),
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `AppBar` são:

- **`title`**: O atributo `title` define o texto exibido como título na AppBar.

```dart
AppBar(
  title: Text('Título da AppBar'),
),
```

- **`actions`**: O atributo `actions` define uma lista de ícones de ação exibidos no lado direito da `AppBar`. Esses ícones geralmente representam ações como pesquisa, configurações ou mais opções.

```dart
AppBar(
  actions: <Widget>[
    IconButton(
      icon: Icon(Icons.search),
      onPressed: () {
        // Ação ao pressionar o ícone de pesquisa
      },
    ),
    IconButton(
      icon: Icon(Icons.more_vert),
      onPressed: () {
        // Ação ao pressionar o ícone de mais opções
      },
    ),
  ],
),
```

- **`leading`**: O atributo `leading` define um ícone de navegação que geralmente é exibido no lado esquerdo da `AppBar`. É comumente usado para um ícone de menu lateral (drawer) ou um ícone de voltar para a tela anterior.

```dart
AppBar(
  leading: IconButton(
    icon: Icon(Icons.menu),
    onPressed: () {
      // Ação ao pressionar o ícone de menu
    },
  ),
),
```

- **`backgroundColor`**: O atributo `backgroundColor` define a cor de fundo da `AppBar`.

```dart
AppBar(
  backgroundColor: Colors.blue,
),
```

- **`elevation`**: O atributo `elevation` define a elevação da `AppBar`, controlando sua sombra em relação ao restante do conteúdo da tela.

```dart
AppBar(
  elevation: 4.0,
),
```

- **`centerTitle`**: O atributo `centerTitle` define se o título da `AppBar` deve ser centralizado na área disponível.

```dart
AppBar(
  centerTitle: true,
),
```

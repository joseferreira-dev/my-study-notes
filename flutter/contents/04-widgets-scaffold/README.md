<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Scaffold

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Scaffold` no Flutter é uma das ferramentas mais fundamentais para a construção de interfaces de usuário robustas e estruturadas. Ele fornece uma estrutura básica para a criação de layouts visuais, facilitando a organização de diferentes componentes visuais em uma aplicação. O `Scaffold` é especialmente útil porque integra diversos elementos de interface padrão, como barras de navegação, barras de ações, gavetas (drawers), e áreas de conteúdo.

## Estrutura Básica e Funcionamento

Um `Scaffold` é normalmente usado como o widget raiz de uma tela. Ele organiza a estrutura de uma página e permite que o desenvolvedor adicione componentes visuais de forma sistemática e organizada. Ele possui uma estrutura básica que inclui uma barra de aplicativo, um corpo principal e, opcionalmente, barras de navegação, como barra de título, barra de navegação inferior e gavetas laterais. Abaixo está um exemplo bem completo de como um `Scaffold` pode ser usado:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Título da Página'),
      ),
      body: Center(
        child: Text('Conteúdo principal'),
      ),
      backgroundColor: Colors.lightGreen[100],
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Ação do botão
        },
        child: Icon(Icons.add),
      ),
      drawer: Drawer(
        child: ListView(
          children: <Widget>[
            DrawerHeader(
              child: Text('Cabeçalho do Drawer'),
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
            ),
            ListTile(
              title: Text('Item 1'),
              onTap: () {
                // Ação do item
              },
            ),
          ],
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.business),
            label: 'Business',
          ),
        ],
        onTap: (int index) {
          // Ação de navegação
        },
      ),
      persistentFooterButtons: [
        TextButton(
          onPressed: () {
            // Ação do botão
          },
          child: Text('Salvar'),
        ),
        TextButton(
          onPressed: () {
            // Ação do botão
          },
          child: Text('Cancelar'),
        ),
      ],
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `Scaffold` são:

- **`appBar`**: A `appBar` é uma barra de aplicativos que geralmente aparece na parte superior da tela. Ela é usada para exibir o título da página, ícones de navegação, menus e outras ações contextuais.

```dart
appBar: AppBar(
  title: Text('Título da Página'),
),
```

- **`body`**: O `body` é a principal área de conteúdo da tela. É onde a maioria dos widgets são colocados e onde a lógica principal da interface do usuário é desenvolvida.

```dart
body: Center(
  child: Text('Conteúdo principal'),
),
```

- **`backgroundColor`**: Define a cor de fundo da tela.

```dart
backgroundColor: Colors.lightGreen[100],
```

- **`floatingActionButton`**: O `floatingActionButton` é um botão flutuante de ação que normalmente é usado para ações principais na interface, como adicionar um novo item.

```dart
floatingActionButton: FloatingActionButton(
  onPressed: () {
    // Ação do botão
  },
  child: Icon(Icons.add),
),
```

- **`drawer`**: O `drawer` é um painel que desliza da borda lateral da tela e contém opções de navegação e outras ações. Ele é útil para criar menus de navegação.

```dart
drawer: Drawer(
  child: ListView(
    children: <Widget>[
      DrawerHeader(
        child: Text('Cabeçalho do Drawer'),
        decoration: BoxDecoration(
          color: Colors.blue,
        ),
      ),
      ListTile(
        title: Text('Item 1'),
        onTap: () {
          // Ação do item
        },
      ),
    ],
  ),
),
```

- **`bottomNavigationBar`**: O `bottomNavigationBar` é uma barra de navegação inferior que permite a navegação entre diferentes seções da aplicação.

```dart
bottomNavigationBar: BottomNavigationBar(
  items: [
    BottomNavigationBarItem(
      icon: Icon(Icons.home),
      label: 'Home',
    ),
    BottomNavigationBarItem(
      icon: Icon(Icons.business),
      label: 'Business',
    ),
  ],
  onTap: (int index) {
    // Ação de navegação
  },
),
```

- **`persistentFooterButtons`**: Estes são botões que permanecem fixos na parte inferior da tela, geralmente usados para ações persistentes como salvar ou cancelar.

```dart
persistentFooterButtons: [
  TextButton(
    onPressed: () {
      // Ação do botão
    },
    child: Text('Salvar'),
  ),
  TextButton(
    onPressed: () {
      // Ação do botão
    },
    child: Text('Cancelar'),
  ),
],
```

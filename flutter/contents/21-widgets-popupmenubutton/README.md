<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: PopupMenuButton

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O `PopupMenuButton` é um widget do Flutter que exibe um menu popup quando pressionado. É amplamente utilizado para oferecer uma lista de opções ou ações ao usuário, geralmente representadas por ícones ou texto. Este widget é útil para economizar espaço na interface do usuário e proporcionar uma experiência de navegação limpa e eficiente.

## Estrutura Básica e Funcionamento

A estrutura básica do `PopupMenuButton` inclui um botão que, ao ser pressionado, exibe um menu popup com várias opções. Cada opção no menu é um item de menu que pode ser configurado com texto, ícones e outras propriedades. O `PopupMenuButton` usa o genérico `<T>` para especificar o tipo de valor que cada item de menu representa. Aqui está um exemplo completo de como usar o `PopupMenuButton`, incluindo a definição de um `enum` para as opções do menu e um `switch` para lidar com a seleção do item:


```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

enum MenuOptions { optionOne, optionTwo, optionThree }

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter PopupMenuButton',
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  String _selectedOption = 'Nenhuma opção selecionada';

  void _onSelected(MenuOptions result) {
    setState(() {
      switch (result) {
        case MenuOptions.optionOne:
          _selectedOption = 'Opção 1 selecionada';
          break;
        case MenuOptions.optionTwo:
          _selectedOption = 'Opção 2 selecionada';
          break;
        case MenuOptions.optionThree:
          _selectedOption = 'Opção 3 selecionada';
          break;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('PopupMenuButton Example'),
        actions: <Widget>[
          PopupMenuButton<MenuOptions>(
            onSelected: _onSelected,
            itemBuilder: (BuildContext context) => <PopupMenuEntry<MenuOptions>>[
              const PopupMenuItem<MenuOptions>(
                value: MenuOptions.optionOne,
                child: Text('Opção 1'),
              ),
              const PopupMenuItem<MenuOptions>(
                value: MenuOptions.optionTwo,
                child: Text('Opção 2'),
              ),
              const PopupMenuItem<MenuOptions>(
                value: MenuOptions.optionThree,
                child: Text('Opção 3'),
              ),
            ],
          ),
        ],
      ),
      body: Center(
        child: Text(
          _selectedOption,
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `PopupMenuButton` são:

- **`PopupMenuButton<T>`**: O `PopupMenuButton` é o widget principal que exibe o menu popup. Ele aceita vários parâmetros, incluindo `itemBuilder`, `onSelected`, `icon`, e outros.

```dart
PopupMenuButton<MenuOptions>(
  onSelected: _onSelected,
  itemBuilder: (BuildContext context) => <PopupMenuEntry<MenuOptions>>[
    const PopupMenuItem<MenuOptions>(
      value: MenuOptions.optionOne,
      child: Text('Opção 1'),
    ),
    const PopupMenuItem<MenuOptions>(
      value: MenuOptions.optionTwo,
      child: Text('Opção 2'),
    ),
    const PopupMenuItem<MenuOptions>(
      value: MenuOptions.optionThree,
      child: Text('Opção 3'),
    ),
  ],
);
```

- **`PopupMenuItem<T>`**: Cada item no menu popup é representado por um `PopupMenuItem`. Este widget pode ser configurado com várias propriedades, como `value` e `child`.

```dart
const PopupMenuItem<MenuOptions>(
  value: MenuOptions.optionOne,
  child: Text('Opção 1'),
);
```

- **`onSelected`**: O callback `onSelected` é chamado quando o usuário seleciona um item do menu. Ele recebe como parâmetro o valor do item selecionado.

```dart
PopupMenuButton<MenuOptions>(
  onSelected: (MenuOptions result) {
    print(result); // Faz algo com o item selecionado
  },
  itemBuilder: (BuildContext context) => <PopupMenuEntry<MenuOptions>>[
    const PopupMenuItem<MenuOptions>(
      value: MenuOptions.optionOne,
      child: Text('Opção 1'),
    ),
  ],
);
```

- **`icon`**: O parâmetro `icon` permite definir um ícone personalizado para o botão do menu popup.

```dart
PopupMenuButton<MenuOptions>(
  icon: Icon(Icons.more_vert),
  onSelected: _onSelected,
  itemBuilder: (BuildContext context) => <PopupMenuEntry<MenuOptions>>[
    const PopupMenuItem<MenuOptions>(
      value: MenuOptions.optionOne,
      child: Text('Opção 1'),
    ),
  ],
);
```

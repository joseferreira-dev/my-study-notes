<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Drawer

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Drawer` no Flutter é utilizado para implementar menus laterais deslizáveis em aplicações móveis, seguindo as diretrizes de design do Material Design. Ele oferece uma maneira conveniente de fornecer acesso a navegação, configurações e outras funcionalidades adicionais sem ocupar espaço na tela principal do aplicativo.

## Estrutura Básica e Funcionamento

O `Drawer` é normalmente associado ao widget `Scaffold`, que é o layout principal usado no Flutter para estruturar telas de aplicativos. Ele pode conter uma lista de itens de menu, um cabeçalho personalizado e outros componentes interativos. Abaixo está um exemplo completo demonstrando o uso do `Drawer` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Drawer'),
        ),
        drawer: Drawer(
          child: ListView(
            padding: EdgeInsets.zero,
            children: <Widget>[
              DrawerHeader(
                decoration: BoxDecoration(
                  color: Colors.blue,
                ),
                child: Text(
                  'Cabeçalho do Drawer',
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 24,
                  ),
                ),
              ),
              ListTile(
                leading: Icon(Icons.home),
                title: Text('Início'),
                onTap: () {
                  // Ação ao selecionar o item 'Início'
                },
              ),
              ListTile(
                leading: Icon(Icons.settings),
                title: Text('Configurações'),
                onTap: () {
                  // Ação ao selecionar o item 'Configurações'
                },
              ),
            ],
          ),
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

Os principais componentes e atributos do `Drawer` são:

- **`DrawerHeader`**: O `DrawerHeader` é usado para definir um cabeçalho personalizado no início do `Drawer`. Ele pode conter texto, imagens ou outros widgets para fornecer informações adicionais ou branding para o menu lateral.

```dart
Drawer(
  child: ListView(
    padding: EdgeInsets.zero,
    children: <Widget>[
      DrawerHeader(
        decoration: BoxDecoration(
          color: Colors.blue,
        ),
        child: Text(
          'Cabeçalho do Drawer',
          style: TextStyle(
            color: Colors.white,
            fontSize: 24,
          ),
        ),
      ),
      // Outros itens do menu
    ],
  ),
),
```

- **`ListTile`**: Os `ListTile` são utilizados para representar cada item do menu dentro do `Drawer`. Eles podem incluir um ícone, um título descritivo e uma função de callback para lidar com a interação do usuário.

```dart
Drawer(
  child: ListView(
    padding: EdgeInsets.zero,
    children: <Widget>[
      // Cabeçalho do Drawer
      DrawerHeader(
        // ...
      ),
      ListTile(
        leading: Icon(Icons.home),
        title: Text('Início'),
        onTap: () {
          // Ação ao selecionar o item 'Início'
        },
      ),
      ListTile(
        leading: Icon(Icons.settings),
        title: Text('Configurações'),
        onTap: () {
          // Ação ao selecionar o item 'Configurações'
        },
      ),
    ],
  ),
),
```

- **`padding`**: O atributo `padding` define o espaçamento ao redor dos itens dentro do `Drawer`, garantindo um layout visualmente agradável e consistente.

```dart
Drawer(
  child: ListView(
    padding: EdgeInsets.zero,
    children: <Widget>[
      // Itens do menu
    ],
  ),
),
```

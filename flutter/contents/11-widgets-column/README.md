<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Column

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Column` no Flutter é utilizado para organizar widgets verticalmente em um layout. Ele permite agrupar vários widgets filhos um abaixo do outro, facilitando a construção de interfaces de usuário que necessitam de alinhamento vertical específico e organização estruturada.

## Estrutura Básica e Funcionamento

O `Column` é um widget flexível que pode conter uma lista de widgets filhos, organizando-os de cima para baixo. Ele se ajusta dinamicamente ao tamanho dos seus filhos e ao espaço disponível no layout. Abaixo está um exemplo completo que demonstra o uso do `Column` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Column'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            mainAxisSize: MainAxisSize.min,
            children: <Widget>[
              Container(
                width: 200.0,
                height: 40.0,
                color: Colors.blue,
                child: Center(child: Text('Item 1')),
              ),
              Container(
                width: 200.0,
                height: 40.0,
                color: Colors.green,
                child: Center(child: Text('Item 2')),
              ),
              Container(
                width: 200.0,
                height: 40.0,
                color: Colors.yellow,
                child: Center(child: Text('Item 3')),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `Column` são:

- **`mainAxisAlignment`**: O atributo `mainAxisAlignment` define como os widgets filhos são alinhados ao longo do eixo principal da `Column`, que é vertical neste caso. Pode ser usado para alinhar os widgets no início, no centro, no final, ou distribuí-los uniformemente ao longo do eixo.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

- **`crossAxisAlignment`**: O atributo `crossAxisAlignment` define como os widgets filhos são alinhados ao longo do eixo cruzado da `Column`, que é horizontal neste caso. Pode ser usado para alinhar os widgets à esquerda, ao centro, à direita, ou esticá-los para preencher a largura disponível.

```dart
Column(
  crossAxisAlignment: CrossAxisAlignment.center,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

- **`mainAxisSize`**: O atributo `mainAxisSize` define o tamanho da `Column` ao longo do eixo principal. Pode ser configurado para ocupar o mínimo espaço possível (`min`) ou expandir para preencher todo o espaço disponível (`max`).

```dart
Column(
  mainAxisSize: MainAxisSize.min,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Row

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Row` no Flutter é utilizado para organizar widgets horizontalmente em um layout. Ele permite agrupar vários widgets filhos lado a lado, facilitando a criação de interfaces de usuário que necessitam de alinhamento horizontal específico e organização estruturada.

## Estrutura Básica e Funcionamento

O `Row` é um widget flexível que pode conter uma lista de widgets filhos, organizando-os da esquerda para a direita. Ele se ajusta dinamicamente ao tamanho dos seus filhos e ao espaço disponível no layout. Abaixo está um exemplo completo que demonstra o uso do `Row` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Row'),
        ),
        body: Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            mainAxisSize: MainAxisSize.min,
            children: <Widget>[
              Container(
                width: 60.0,
                height: 200.0,
                color: Colors.blue,
                child: Center(child: Text('Item 1')),
              ),
              Container(
                width: 60.0,
                height: 200.0,
                color: Colors.green,
                child: Center(child: Text('Item 2')),
              ),
              Container(
                width: 60.0,
                height: 200.0,
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

Os principais componentes e atributos do `Row` são:

- **`mainAxisAlignment`**: O atributo `mainAxisAlignment` define como os widgets filhos são alinhados ao longo do eixo principal da `Row`, que é horizontal neste caso. Pode ser usado para alinhar os widgets no início, no centro, no final, ou distribuí-los uniformemente ao longo do eixo.

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

- **`crossAxisAlignment`**: O atributo `crossAxisAlignment` define como os widgets filhos são alinhados ao longo do eixo cruzado da `Row`, que é vertical neste caso. Pode ser usado para alinhar os widgets ao topo, ao centro, à base, ou esticá-los para preencher a altura disponível.

```dart
Row(
  crossAxisAlignment: CrossAxisAlignment.center,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

- **`mainAxisSize`**: O atributo `mainAxisSize` define o tamanho da `Row` ao longo do eixo principal. Pode ser configurado para ocupar o mínimo espaço possível (`min`) ou expandir para preencher todo o espaço disponível (`max`).

```dart
Row(
  mainAxisSize: MainAxisSize.min,
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
),
```

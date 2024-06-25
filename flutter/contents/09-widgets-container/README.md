<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Container

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Container` no Flutter é um componente versátil usado para layout e estilização de outros widgets. Ele é frequentemente utilizado para agrupar widgets filhos e aplicar transformações, como margens, preenchimento, cor de fundo, bordas e efeitos de decoração, permitindo um controle preciso sobre a aparência e o posicionamento dos elementos na interface do usuário.

## Estrutura Básica e Funcionamento

O `Container` atua como um invólucro para outros widgets, proporcionando um ambiente onde propriedades de layout e decoração podem ser aplicadas. Abaixo está um exemplo completo que demonstra o uso do `Container` com diversos atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Container'),
        ),
        body: Center(
          child: Container(
            width: 200.0,
            height: 200.0,
            padding: EdgeInsets.all(20.0),
            margin: EdgeInsets.symmetric(vertical: 30.0),
            decoration: BoxDecoration(
              color: Colors.blue,
              borderRadius: BorderRadius.circular(10.0),
              boxShadow: [
                BoxShadow(
                  color: Colors.grey.withOpacity(0.5),
                  spreadRadius: 5,
                  blurRadius: 7,
                  offset: Offset(0, 3),
                ),
              ],
            ),
            child: Text(
              'Conteúdo do Container',
              style: TextStyle(
                color: Colors.white,
                fontSize: 18.0,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `Container` são:

- **`width` e `height`**: Os atributos `width` e `height` definem as dimensões do `Container`, controlando seu tamanho em relação ao espaço disponível.

```dart
Container(
  width: 200.0,
  height: 200.0,
),
```

- **`padding` e `margin`**: Os atributos `padding` e `margin` definem o espaço interno e externo do `Container`, respectivamente. Eles podem ser configurados para criar espaçamentos entre o conteúdo do `Container` e seus limites externos.

```dart
Container(
  padding: EdgeInsets.all(20.0),
  margin: EdgeInsets.symmetric(vertical: 30.0),
),
```

- **`decoration`**: O atributo `decoration` permite aplicar efeitos visuais ao `Container`, como cores de fundo, bordas arredondadas, sombras e outros efeitos decorativos.

```dart
Container(
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(10.0),
    boxShadow: [
      BoxShadow(
        color: Colors.grey.withOpacity(0.5),
        spreadRadius: 5,
        blurRadius: 7,
        offset: Offset(0, 3),
      ),
    ],
  ),
),
```

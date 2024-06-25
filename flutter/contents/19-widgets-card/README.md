<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Card

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Card` no Flutter é utilizado para criar contêineres estilizados que podem ser usados para destacar informações importantes ou agrupar conteúdo relacionado. Ele segue as diretrizes de design de cartões do Material Design, oferecendo uma aparência consistente e moderna para elementos visuais dentro de um aplicativo. O `Card` é frequentemente utilizado em listas, grids ou como componentes individuais para criar interfaces de usuário limpas e organizadas.

## Estrutura Básica e Funcionamento

O `Card` é um widget que envolve seu filho em um contêiner com elevação, bordas arredondadas e sombreamento. Ele pode ser configurado com vários atributos para ajustar a aparência e o comportamento do cartão. Abaixo está um exemplo completo que demonstra o uso do `Card` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Card'),
        ),
        body: Center(
          child: Card(
            color: Colors.blue[50],
            elevation: 5.0,
            shadowColor: Colors.blue,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(15.0),
            ),
            margin: EdgeInsets.all(20.0),
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: <Widget>[
                  Text(
                    'Título do Card',
                    style: TextStyle(
                      fontSize: 20.0,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  SizedBox(height: 10.0),
                  Text(
                    'Este é um exemplo de um widget Card no Flutter. Ele pode conter vários tipos de conteúdo e ser personalizado conforme necessário.',
                  ),
                ],
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

Os principais componentes e atributos do `Card` são:

- **`color`**: O atributo `color` define a cor de fundo do `Card`. Pode ser configurado com qualquer cor disponível na classe `Colors` ou uma cor personalizada, permitindo que o `Card` combine com o tema do aplicativo.

```dart
Card(
  color: Colors.yellow[100],
  child: Padding(
    padding: const EdgeInsets.all(16.0),
    child: Text('Card com cor personalizada'),
  ),
),
```

- **`elevation`**: O atributo `elevation` controla a sombra projetada pelo `Card`, criando um efeito de profundidade. Pode ser ajustado para qualquer valor numérico, permitindo aumentar ou diminuir a elevação conforme necessário.

```dart
Card(
  elevation: 10.0,
  child: Padding(
    padding: const EdgeInsets.all(16.0),
    child: Text('Card com elevação personalizada'),
  ),
),
```

- **`shape`**: O atributo `shape` define a forma do `Card`, incluindo as bordas. Pode ser configurado com diferentes formas usando classes como `RoundedRectangleBorder` para bordas arredondadas ou `BeveledRectangleBorder` para bordas chanfradas.

```dart
Card(
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(10.0),
  ),
  child: Padding(
    padding: const EdgeInsets.all(16.0),
    child: Text('Card com bordas arredondadas'),
  ),
),
```

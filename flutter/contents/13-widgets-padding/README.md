<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Padding

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Padding` no Flutter é utilizado para adicionar preenchimento ao redor de seu único filho, permitindo ajustar o espaçamento entre o conteúdo e seus limites externos. Ele é essencial para criar layouts mais espaçados e visualmente equilibrados em interfaces de usuário.

## Estrutura Básica e Funcionamento

O `Padding` envolve um único widget filho e aplica margens uniformes a seu redor, controladas pelo atributo `padding`. Este widget é utilizado para adicionar espaço interno entre o conteúdo e seus limites externos, sem afetar o tamanho do widget filho diretamente. Abaixo está um exemplo completo que demonstra o uso do `Padding` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Padding'),
        ),
        body: Container(
          decoration: BoxDecoration(
              color: Colors.lightGreen,              
            ),
          child: Padding(
            padding: EdgeInsets.all(50.0),
            child: Container(
              color: Colors.blue,
              width: 150.0,
              height: 150.0,
              child: Text(
                'Conteúdo com Preenchimento',
                style: TextStyle(
                  color: Colors.white,
                  fontSize: 18.0,
                  fontWeight: FontWeight.bold,
                ),
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

Os principais componentes e atributos do `Padding` são:

- **`padding`**: O atributo `padding` define o espaçamento interno ao redor do widget filho dentro do `Padding`. Pode ser configurado com valores específicos para definir margens diferentes em cada lado do widget.

```dart
Padding(
  padding: EdgeInsets.symmetric(vertical: 20.0, horizontal: 10.0),
  child: Text('Widget com padding simétrico'),
),
```

- **`EdgeInsets`**: A classe `EdgeInsets` utilizada no padding permite especificar valores diferentes para cada lado do widget filho, como `top`, `bottom`, `left` e `right`, proporcionando controle preciso sobre o espaçamento em cada direção.

```dart
Padding(
  padding: EdgeInsets.only(left: 30.0, right: 10.0),
  child: Text('Widget com padding direcional'),
),
```

- **Espaçamento Personalizado**: Além dos métodos `symmetric` e `only`, a classe `EdgeInsets` também oferece métodos como `all` para definir um mesmo espaçamento para todos os lados, e `fromLTRB` para especificar valores para os quatro lados diretamente.

```dart
Padding(
  padding: EdgeInsets.all(15.0),
  child: Text('Widget com padding personalizado'),
),
```

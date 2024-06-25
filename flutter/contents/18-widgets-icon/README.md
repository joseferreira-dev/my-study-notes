<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Icon

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Icon` no Flutter é utilizado para exibir ícones gráficos, que são normalmente usados para representar ações, categorias ou status dentro de um aplicativo. Ele oferece uma maneira simples e eficiente de incluir ícones na interface do usuário, utilizando um conjunto predefinido de ícones ou ícones personalizados.

## Estrutura Básica e Funcionamento

O `Icon` é um widget que exibe um ícone de um conjunto predefinido de ícones do Material Design ou de fontes personalizadas. Ele pode ser configurado com diferentes atributos para controlar aspectos como o tamanho, a cor e o ícone específico a ser exibido. Abaixo está um exemplo completo que demonstra o uso do `Icon` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Icon'),
        ),
        body: Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Icon(
                Icons.home,
                size: 50.0,
                color: Colors.blue,
                semanticLabel: 'Ícone de Casa',
                textDirection: TextDirection.ltr,
              ),
              SizedBox(width: 20.0),
              Icon(
                Icons.favorite,
                size: 50.0,
                color: Colors.red,
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

Os principais componentes e atributos do `Icon` são:

- **`icon`**: O atributo `icon` define qual ícone será exibido. Os ícones podem ser selecionados a partir dos conjuntos de ícones do Material Design fornecidos pelo Flutter, como `Icons.home`, `Icons.favorite`, entre outros.

```dart
Icon(
  Icons.star,
),
```

- **`size`**: O atributo `size` define o tamanho do ícone em pixels. Pode ser ajustado para qualquer valor numérico, permitindo aumentar ou diminuir a escala do ícone conforme necessário.

```dart
Icon(
  Icons.star,
  size: 40.0,
),
```

- **`color`**: O atributo `color` define a cor do ícone. Pode ser configurado usando qualquer cor disponível na classe `Colors` ou uma cor personalizada, permitindo ajustar o ícone ao tema do aplicativo.

```dart
Icon(
  Icons.star,
  color: Colors.yellow,
),
```

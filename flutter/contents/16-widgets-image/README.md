<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Image

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Image` no Flutter é utilizado para exibir imagens dentro de um aplicativo. Ele permite carregar imagens de diferentes fontes, como recursos locais, URLs da web e até mesmo imagens pré-carregadas em memória, facilitando a exibição de conteúdo visual variado em interfaces de usuário.

## Estrutura Básica e Funcionamento

O `Image` carrega e exibe uma imagem dentro do layout do aplicativo. Ele pode ser configurado com diferentes atributos para controlar aspectos como a fonte da imagem, o redimensionamento, a repetição e o preenchimento. Abaixo está um exemplo completo que demonstra o uso do `Image` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  final String imageUrl = 'https://via.placeholder.com/150';

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Image'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Image(
                image: NetworkImage(imageUrl),
                width: 150.0,
                height: 150.0,
                fit: BoxFit.cover,
                color: Colors.blue, // Cor que aplica sobre a imagem
                colorBlendMode: BlendMode.colorBurn, // Modo de mistura de cores
                repeat: ImageRepeat.repeatY, // Repetição da imagem
              ),
              SizedBox(height: 20.0),
              Image.asset(
                'assets/flutter_logo.png',
                width: 100.0,
                height: 100.0,
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

Os principais componentes e atributos do `Image` são:

- **`image`**: O atributo `image` define a fonte da imagem a ser exibida. Pode ser configurado com diferentes tipos de imagens, como `AssetImage` para recursos locais ou `NetworkImage` para URLs da web.

```dart
Image(
  image: AssetImage('assets/imagem_local.png'),
),
```

- **`fit`**: O atributo `fit` controla como a imagem é redimensionada para preencher o espaço disponível. Pode ser configurado com valores como `BoxFit.contain`, `BoxFit.cover`, `BoxFit.fill`, entre outros, para ajustar o modo de exibição da imagem.

```dart
Image(
  image: AssetImage('assets/imagem.png'),
  fit: BoxFit.cover,
),
```

- **`colorBlendMode`**: O atributo `colorBlendMode` define como a cor especificada no atributo `color` é misturada com a imagem. Pode ser configurado com modos de mistura como `BlendMode.color`, `BlendMode.hue`, `BlendMode.saturation`, entre outros.

```dart
Image(
  image: AssetImage('assets/imagem.png'),
  color: Colors.blue,
  colorBlendMode: BlendMode.colorBurn,
),
```

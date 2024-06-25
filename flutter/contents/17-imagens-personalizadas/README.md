<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Adicionando Imagens Personalizadas e Responsivas

- [Introdução](#introdução)
- [Adicionando Imagens Personalizadas](#adicionando-imagens-personalizadas)
- [](#)

## Introdução

Adicionar imagens personalizadas e garantir que elas sejam exibidas corretamente em diferentes dispositivos e resoluções é essencial para a criação de aplicativos Flutter visualmente atraentes e responsivos. O Flutter facilita a gestão de imagens para diferentes densidades de tela utilizando pastas específicas como 2.0x, 3.0x, etc. Isso permite que as imagens sejam escalonadas corretamente para diferentes dispositivos, mantendo a qualidade visual.

## Adicionando Imagens Personalizadas

Para adicionar imagens personalizadas e lidar com diferentes tamanhos e resoluções, é necessário seguir alguns passos que envolvem organizar as imagens em pastas específicas e configurar o arquivo `pubspec.yaml`.

Primeiro, cria-se uma pasta chamada `assets/images` no diretório raiz do projeto Flutter. Dentro dessa pasta, cria-se subpastas para diferentes densidades de tela, como `2.0x`, `3.0x`, etc. Estas pastas conterão as versões das imagens em diferentes resoluções.

```
assets/
  images/
    my_image.png
    2.0x/
      my_image.png
    3.0x/
      my_image.png
```

Neste exemplo, `my_image.png` é a imagem base, `2.0x/my_image.png` é a imagem para telas com densidade 2.0, e `3.0x/my_image.png` é a imagem para telas com densidade 3.0.

Em seguida, deve-se fazer a configuração do arquivo `pubspec.yaml`, adicionando-se a referência às imagens personalizadas dentro da seção `flutter`.

```yaml
flutter:
  assets:
    - assets/images/my_image.png
```

Após configurar as imagens no `pubspec.yaml`, utiliza-se o widget `Image.asset` para exibir as imagens no aplicativo. O Flutter automaticamente seleciona a imagem apropriada com base na densidade da tela do dispositivo.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Imagens Personalizadas'),
        ),
        body: Center(
          child: Image.asset('assets/images/my_image.png'),
        ),
      ),
    );
  }
}
```

## Lidando com o Tamanho das Imagens

Ao lidar com imagens em um aplicativo Flutter, é importante garantir que elas se ajustem corretamente a diferentes tamanhos e resoluções de tela. Aqui estão algumas práticas recomendadas:

- **Utilizar Imagens Escaláveis**: Deve se certificar de que as imagens sejam suficientemente grandes para evitar pixelização em telas de alta resolução. Para isso, adicionam-se versões maiores das imagens nas pastas 2.0x, 3.0x, etc.
- **Uso de Widgets Responsivos**: Utilizar widgets como `Container`, `SizedBox`, e `MediaQuery` para ajustar dinamicamente o tamanho das imagens de acordo com o tamanho da tela.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Imagens Responsivas'),
        ),
        body: Center(
          child: Container(
            width: MediaQuery.of(context).size.width * 0.5,
            height: MediaQuery.of(context).size.width * 0.5,
            child: Image.asset('assets/images/my_image.png'),
          ),
        ),
      ),
    );
  }
}
```

<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Adicionando Fonts Personalizadas

- [Introdução](#introdução)
- [Adicionando Fontes a Partir de Arquivos Locais](#adicionando-fontes-a-partir-de-arquivos-locais)
- [Adicionando Fontes Utilizando o Pacote Google Fonts](#adicionando-fontes-utilizando-o-pacote-google-fonts)

## Introdução

Adicionar fontes personalizadas a um projeto Flutter pode enriquecer significativamente a interface do usuário, proporcionando uma identidade visual única ao aplicativo. As fontes podem ser adicionadas a partir de arquivos locais ou utilizando o pacote Google Fonts. A seguir, é explicado como incorporar ambos os métodos em um projeto Flutter.

## Adicionando Fontes a Partir de Arquivos Locais

Para adicionar fontes personalizadas a partir de arquivos locais, é necessário seguir alguns passos, que incluem adicionar os arquivos de fontes ao projeto e configurar o arquivo `pubspec.yaml`.

Primeiro, cria-se uma pasta chamada `assets/fonts` no diretório raiz do projeto Flutter. Nessa pasta devem ser colocados os arquivos de fontes (no formado .ttf ou .otf). 

Em seguida, deve-se fazer a configuração do arquivo `pubspec.yaml`, adicionando-se a referência às fontes personalizadas dentro da seção `flutter`.

```yaml
flutter:
  fonts:
    - family: CustomFont
      fonts:
        - asset: assets/fonts/CustomFont-Regular.ttf
        - asset: assets/fonts/CustomFont-Bold.ttf
          weight: 700
```

Após configurar as fontes no `pubspec.yaml`, utiliza-se a nova fonte no aplicativo definindo a propriedade `fontFamily` no estilo do texto.

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
          title: Text('Fonte Personalizada'),
        ),
        body: Center(
          child: Text(
            'Texto com Fonte Personalizada',
            style: TextStyle(fontFamily: 'CustomFont'),
          ),
        ),
      ),
    );
  }
}
```

## Adicionando Fontes Utilizando o Pacote Google Fonts

O pacote `google_fonts` permite o uso de fontes do Google Fonts de maneira fácil e rápida sem a necessidade de baixar e incluir arquivos de fontes manualmente.

Primeiro, adiciona-se a dependência do pacote `google_fonts` ao arquivo `pubspec.yaml`.

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_fonts: ^2.1.0
```

Em seguida, pode-se importar o pacote `google_fonts` nos arquivos Dart onde se deseja utilizar as fontes. Utiliza-se o método `GoogleFonts.fontName()` para aplicar a fonte desejada.

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Google Fonts'),
        ),
        body: Center(
          child: Text(
            'Texto com Google Font',
            style: GoogleFonts.lato(
              textStyle: TextStyle(fontSize: 24),
            ),
          ),
        ),
      ),
    );
  }
}
```

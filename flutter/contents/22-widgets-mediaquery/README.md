<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: MediaQuery

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O `MediaQuery` é um widget do Flutter que fornece informações sobre o tamanho, a orientação e outras características do dispositivo em que o aplicativo está sendo executado. Ele permite que os desenvolvedores adaptem a interface do usuário para diferentes tamanhos de tela, orientações e densidades de pixels, garantindo uma experiência consistente e responsiva.

## Estrutura Básica e Funcionamento

Para obter dados do dispositivo usando o `MediaQuery`, deve-se envolver a árvore de widgets com um widget `MediaQuery` ou acessar o contexto do `BuildContext` onde o `MediaQuery` já está disponível. O `MediaQuery` fornece uma série de propriedades úteis, como `size`, `orientation`, `padding`, `devicePixelRatio`, entre outras. Aqui está um exemplo completo de como obter e utilizar dados do dispositivo usando `MediaQuery`:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'MediaQuery Example',
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Obtendo dados do dispositivo
    final size = MediaQuery.of(context).size;
    final orientation = MediaQuery.of(context).orientation;
    final padding = MediaQuery.of(context).padding;
    final devicePixelRatio = MediaQuery.of(context).devicePixelRatio;

    return Scaffold(
      appBar: AppBar(
        title: Text('MediaQuery Example'),
      ),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            Text('Tamanho da tela: ${size.width} x ${size.height}'),
            Text('Orientação: $orientation'),
            Text('Padding: top ${padding.top}, bottom ${padding.bottom}'),
            Text('Device Pixel Ratio: $devicePixelRatio'),
            Container(
              width: size.width * 0.5,
              height: 100,
              color: Colors.blue,
              child: Center(
                child: Text('Largura 50% da tela'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

## Principais Componentes e Atributos

Os principais componentes e atributos do `PopupMenuButton` são:

- **`MediaQuery.of(context)`**: O método `MediaQuery.of(context)` é utilizado para acessar as informações do dispositivo a partir do contexto. Ele retorna uma instância de `MediaQueryData` contendo diversas propriedades sobre o dispositivo.

```dart
final size = MediaQuery.of(context).size;
```

- **`MediaQueryData.size`**: A propriedade `size` retorna um `Size` contendo a largura e altura da tela em pixels.

```dart
final size = MediaQuery.of(context).size;
Text('Tamanho da tela: ${size.width} x ${size.height}');
```

- **`MediaQueryData.orientation`**: A propriedade `orientation` retorna a orientação atual do dispositivo, que pode ser `Orientation.portrait` ou `Orientation.landscape`.

```dart
final orientation = MediaQuery.of(context).orientation;
Text('Orientação: $orientation');
```

- **`MediaQueryData.padding`**: A propriedade `padding` retorna as áreas de padding do dispositivo, como o topo (usualmente ocupado pela barra de status), a base (geralmente ocupada pela barra de navegação) e outras áreas.

```dart
final padding = MediaQuery.of(context).padding;
Text('Padding: top ${padding.top}, bottom ${padding.bottom}');
```

- **`MediaQueryData.devicePixelRatio`**: A propriedade `devicePixelRatio` retorna a densidade de pixels do dispositivo, o que é útil para adaptar elementos gráficos a diferentes densidades de tela.

```dart
final devicePixelRatio = MediaQuery.of(context).devicePixelRatio;
Text('Device Pixel Ratio: $devicePixelRatio');
```

## Obtendo 50% da Altura do Body de um Aplicativo

Calcular a altura exata do corpo do aplicativo no Flutter envolve considerar as dimensões da tela e descontar as áreas não utilizáveis, como o `AppBar` e outras barras. Utilizar o `MediaQuery` para obter essas dimensões é uma prática comum e eficaz.

Para obter 50% da altura exata do corpo do aplicativo, deve-se:

1. Utilizar o `MediaQuery` para obter a altura total da tela.
2. Subtrair a altura do AppBar e outras áreas, se necessário.
3. Utilizar o valor resultante para definir a altura desejada.

Aqui está um exemplo completo:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Height Calculation Example',
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Obtém a altura total da tela
    final double screenHeight = MediaQuery.of(context).size.height;
    
    // Define a altura do AppBar
    final double appBarHeight = AppBar().preferredSize.height;
    
    // Obtém a altura do padding do topo (barra de status)
    final double statusBarHeight = MediaQuery.of(context).padding.top;

    // Calcula a altura disponível para o corpo do aplicativo
    final double bodyHeight = screenHeight - appBarHeight - statusBarHeight;

    // Calcula 50% da altura do corpo
    final double halfBodyHeight = bodyHeight * 0.5;

    return Scaffold(
      appBar: AppBar(
        title: Text('Height Calculation Example'),
      ),
      body: Center(
        child: Container(
          // Define a altura do container como 50% da altura do corpo
          height: halfBodyHeight,
          width: double.infinity,
          color: Colors.blue,
          child: Center(
            child: Text(
              '50% da altura do corpo',
              style: TextStyle(color: Colors.white, fontSize: 20),
            ),
          ),
        ),
      ),
    );
  }
}
```

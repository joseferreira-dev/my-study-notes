<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Center

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Center` no Flutter é usado para centralizar seu único filho dentro do espaço disponível. Ele facilita o alinhamento vertical e horizontal de widgets, garantindo que o conteúdo seja exibido no centro da área designada, o que é útil para criar layouts simétricos e visualmente equilibrados.

## Estrutura Básica e Funcionamento

O `Center` envolve um único widget filho e o posiciona exatamente no centro do espaço disponível, tanto vertical quanto horizontalmente. Este widget é útil quando é necessário garantir que o conteúdo seja centralizado, independentemente do tamanho do dispositivo ou da tela. Abaixo está um exemplo completo que demonstra o uso do `Center` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Center'),
        ),
        body: Center(
          child: Container(
            width: 200.0,
            height: 200.0,
            color: Colors.blue,
            child: Text(
              'Conteúdo Centralizado',
              style: TextStyle(
                color: Colors.white,
                fontSize: 20.0,
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

Os principais componentes e atributos do `Center` são:

- **`child`**: O Center possui apenas um atributo obrigatório, que é o seu widget filho. Este widget é centralizado tanto vertical quanto horizontalmente dentro do espaço disponível.

```dart
Center(
  child: Text('Widget centralizado'),
),
```

- **Alinhamento**: O `Center` não possui atributos de alinhamento específicos além de centralizar seu filho. Para controle adicional sobre o alinhamento dentro do `Center`, pode-se usar propriedades nos widgets filhos para ajustar o alinhamento interno.

```dart
Center(
  child: Container(
    alignment: Alignment.centerRight,
    child: Text('Texto alinhado à direita dentro do Center'),
  ),
),
```

- **Tamanho do Widget Filho**: O tamanho do widget filho dentro do `Center` é determinado pelas dimensões definidas no próprio widget filho. O `Center` não impõe um tamanho específico ao seu conteúdo, apenas garante que ele seja centralizado no espaço disponível.

```dart
SizedBox(
  width: 100.0,
  height: 100.0,
  child: Container(
    color: Colors.red,
  ),
),
```

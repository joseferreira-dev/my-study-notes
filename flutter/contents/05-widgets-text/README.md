<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Text

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Text` no Flutter é utilizado para exibir textos na interface do usuário de um aplicativo. Ele permite configurar diversas propriedades visuais e funcionais relacionadas ao texto, como estilo, alinhamento, overflow, entre outros. O `Text` é amplamente utilizado para exibir informações estáticas ou dinâmicas de forma clara e legível para o usuário.

## Estrutura Básica e Funcionamento

O `Text` no Flutter pode ser configurado com uma série de atributos que afetam sua aparência e comportamento. Abaixo está um exemplo completo demonstrando todos os principais atributos do widget `Text`:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Text Widget'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                'Exemplo de texto',
                style: TextStyle(
                  fontSize: 24.0,
                  fontWeight: FontWeight.bold,
                  fontStyle: FontStyle.italic,
                  color: Colors.blue,
                  letterSpacing: 2.0,
                  wordSpacing: 5.0,
                  decoration: TextDecoration.underline,
                  decorationColor: Colors.red,
                  decorationStyle: TextDecorationStyle.dotted,
                ),
                textAlign: TextAlign.center,
                textDirection: TextDirection.ltr,
                overflow: TextOverflow.ellipsis,
                maxLines: 2,
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

Os principais componentes e atributos do `Text` são:

- **Texto (data)**: O parâmetro `data` é obrigatório e representa o texto a ser exibido pelo widget Text.

```dart
Text(
  'Este é um exemplo de texto.',
),
```

- **`style`**: O atributo `style` permite personalizar o estilo do texto, incluindo fonte, tamanho, cor, peso da fonte, entre outros.

```dart
Text(
  'Texto estilizado',
  style: TextStyle(
    fontSize: 18.0,
    fontWeight: FontWeight.bold,
    color: Colors.red,
  ),
),
```

- **`textAlign`**: O atributo `textAlign` define como o texto deve ser alinhado dentro do espaço disponível.

```dart
Text(
  'Texto centralizado',
  textAlign: TextAlign.center,
),
```

- **`textDirection`**: O atributo `textDirection` define a direção do texto, especialmente útil em layouts multilíngues ou para suporte a linguagens que são lidas da direita para a esquerda.

```dart
Text(
  'النص العربي',
  textDirection: TextDirection.rtl,
),
```

- **`overflow` e `maxLines`**: Os atributos `overflow` e `maxLines` controlam o comportamento do texto quando ele excede o espaço disponível. `overflow` define como o texto será cortado, enquanto `maxLines` limita o número máximo de linhas exibidas.

```dart
Text(
  'Texto longo que será cortado se exceder o espaço disponível.',
  overflow: TextOverflow.ellipsis,
  maxLines: 2,
),
```

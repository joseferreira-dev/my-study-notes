<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: Align

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `Align` no Flutter é usado para alinhar seu único filho dentro de um determinado espaço. Ele oferece controle preciso sobre o posicionamento do widget filho, permitindo alinhamentos verticais e horizontais flexíveis dentro do layout da interface do usuário.

## Estrutura Básica e Funcionamento

O `Align` envolve um único widget filho e define como ele deve ser posicionado dentro do espaço disponível. Utilizando os atributos `alignment` e `child`, o `Align` ajusta a posição do filho conforme especificado. Abaixo está um exemplo completo que demonstra o uso do `Align` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de Align'),
        ),
        body: Container(
          color: Colors.grey[300],
          height: 300.0,
          child: Align(
            alignment: Alignment.centerRight,
            child: Container(
              width: 100.0,
              height: 100.0,
              color: Colors.blue,
              child: Text(
                'Conteúdo Alinhado',
                style: TextStyle(
                  color: Colors.white,
                  fontSize: 16.0,
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

Os principais componentes e atributos do `Align` são:

- **`alignment`**: O atributo `alignment` define como o widget filho deve ser alinhado dentro do espaço disponível. Pode ser configurado usando a classe `Alignment`, que possui constantes como `center`, `topLeft`, `bottomRight`, entre outras, para alinhamentos precisos.

```dart
Align(
  alignment: Alignment.centerLeft,
  child: Text('Widget alinhado à esquerda'),
),
```

- **`child`**: O `Align` pode envolver qualquer widget filho, como um `Container`, `Text`, ou qualquer outro widget. Este widget será posicionado de acordo com o alinhamento definido pelo atributo `alignment`.

```dart
Align(
  alignment: Alignment.topCenter,
  child: Container(
    width: 150.0,
    height: 100.0,
    color: Colors.red,
    child: Text('Widget filho dentro do Align'),
  ),
),
```

- **Alinhamento Personalizado**: Além das constantes predefinidas da classe `Alignment`, é possível criar alinhamentos personalizados usando a classe `Alignment` diretamente, definindo valores personalizados para `x` e `y` entre -1.0 e 1.0.

```dart
Align(
  alignment: Alignment(-0.5, 0.75),
  child: Text('Widget com alinhamento personalizado'),
),
```

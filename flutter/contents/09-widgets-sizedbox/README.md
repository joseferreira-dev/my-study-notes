<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Widgets: SizedBox

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
- [Principais Componentes e Atributos](#principais-componentes-e-atributos)

## Introdução

O widget `SizedBox` no Flutter é utilizado para definir um espaço fixo ou limitar o tamanho de outro widget em um layout. Ele oferece uma maneira simples e eficaz de controlar as dimensões e o espaçamento entre widgets, contribuindo para a organização e o alinhamento precisos da interface do usuário.

## Estrutura Básica e Funcionamento

O `SizedBox` pode envolver qualquer widget filho e definir suas dimensões usando os atributos `width` e `height`. Ele é útil quando é necessário adicionar espaçamentos fixos entre widgets ou limitar o tamanho de um widget em um layout flexível. Abaixo está um exemplo completo que demonstra o uso do `SizedBox` com todos os principais atributos:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Exemplo de SizedBox'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Container(
                color: Colors.blue,
                width: 200.0,
                height: 100.0,
              ),
              SizedBox(
                height: 20.0,
              ),
              Container(
                color: Colors.green,
                width: 200.0,
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

Os principais componentes e atributos do `SizedBox` são:

- **`width` e `height`**: Os atributos `width` e `height` do `SizedBox` definem as dimensões horizontais e verticais do espaço a ser ocupado ou do limite a ser aplicado ao widget filho.

```dart
SizedBox(
  width: 150.0,
  height: 50.0,
),
```

- **`width` e `height` como espaçamento**: Quando `width` ou `height` são definidos como `double.infinity`, o `SizedBox` ocupa todo o espaço disponível no eixo correspondente, funcionando efetivamente como um `Spacer`.

```dart
SizedBox(
  height: double.infinity,
),
```

- **Restrição de Tamanho Mínimo ou Máximo**: Além de definir um tamanho exato, o `SizedBox` também pode ser usado para impor um tamanho mínimo ou máximo ao seu widget filho, garantindo que ele não ultrapasse ou fique menor que um certo limite.

```dart
SizedBox(
  width: 100.0,
  height: 100.0,
  child: Container(
    color: Colors.red,
  ),
),
```

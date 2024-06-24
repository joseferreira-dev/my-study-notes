<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Executando Funções Depois da Tela Pronta

- [Introdução](#introdução)
- [Como Funciona o `addPostFrameCallback`](#como-funciona-o-addpostframecallback)
- [Quando Usar o `addPostFrameCallback`](#quando-usar-o-addpostframecallback)
- [Exemplo Prático](#exemplo-prático)

## Introdução

É muito comum se precisar alguma açõ após a renderização da tela do aplicativo, o seja, depois que a árvore de widgets é contruída. O método `addPostFrameCallback` no Flutter é uma ferramenta útil para se executar uma ação após a construção completa do frame atual. Ele é frequentemente utilizado para executar código que depende do contexto da árvore de widgets após os widgets terem sido completamente renderizados.

## Como Funciona o `addPostFrameCallback`

O método `addPostFrameCallback` pertence à classe `SchedulerBinding`, que é responsável por agendar tarefas que precisam ser executadas no ciclo de vida do frame. Quando você adiciona um callback usando `addPostFrameCallback`, o Flutter garante que o callback será chamado após o próximo frame ser completamente renderizado.

Aqui está como você pode usar `addPostFrameCallback`:

```dart
WidgetsBinding.instance.addPostFrameCallback((_) {
  // Código a ser executado após o frame atual ser renderizado
});
```

O uso de `addPostFrameCallback` traz várias vantagens. Ele permite que o desenvolvedor execute operações que dependem da interface de usuário após a construção completa do frame, garantindo que a árvore de widgets está estável e pronta para interações. Isso inclui operações como exibir diálogos, navegar entre telas, ou realizar medições e ajustes no layout com base nas dimensões reais dos widgets.

## Quando Usar o `addPostFrameCallback`

Existem várias situações onde o uso de `addPostFrameCallback` é apropriado:

- **Interações com o Contexto Após a Renderização**: Às vezes, você precisa acessar propriedades do contexto ou manipular widgets após a construção inicial. Como o `BuildContext` pode mudar durante a construção do widget, é seguro realizar essas operações somente após a renderização do frame atual.
- **Navegação ou Mostrando Diálogos**: Navegação ou exibição de diálogos que dependem do estado completo da árvore de widgets podem ser feitos com segurança no callback `addPostFrameCallback`.
- **Medidas e Layouts**: Se você precisa medir widgets ou obter informações sobre o layout após a construção, `addPostFrameCallback` garante que todas as operações de layout foram concluídas.

## Exemplo Prático

Vamos considerar um exemplo onde precisamos mostrar um diálogo logo após a renderização inicial de um widget. Sem o uso de `addPostFrameCallback`, tentar mostrar um diálogo durante a construção pode resultar em erros, pois o contexto pode não estar completamente disponível.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Exemplo de addPostFrameCallback',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyWidget(),
    );
  }
}

class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
    // Adiciona o callback para ser executado após o frame atual ser renderizado
    WidgetsBinding.instance.addPostFrameCallback((_) {
      _showWelcomeDialog();
    });
  }

  void _showWelcomeDialog() {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text('Bem-vindo'),
          content: Text('Este é um diálogo de boas-vindas.'),
          actions: <Widget>[
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
              },
              child: Text('Fechar'),
            ),
          ],
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Exemplo de addPostFrameCallback'),
      ),
      body: Center(
        child: Text('O diálogo será mostrado após a renderização inicial.'),
      ),
    );
  }
}
```

No exemplo, no método `initState` do State, adicionamos um callback com `addPostFrameCallback`. Este callback chama `_showWelcomeDialog` após o frame atual ser renderizado. O método `_showWelcomeDialog` exibe um diálogo de boas-vindas usando `showDialog`. A chamada para `showDialog` é feita dentro do callback `addPostFrameCallback`, garantindo que o contexto do widget esteja completamente disponível e o diálogo possa ser exibido sem erros. Por fim, o método `build` simplesmente constrói a interface do widget. Quando o widget é renderizado, o diálogo é exibido graças ao callback `addPostFrameCallback`.

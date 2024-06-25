<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/flutter"><img src="../../banner-flutter.png"></a>
</div>
<br>

# Navegação entre Telas com Navigator

- [Introdução](#introdução)
- [Estrutura Básica e Funcionamento](#estrutura-básica-e-funcionamento)
  - [Utilizando Rotas Nomeadas](#utilizando-rotas-nomeadas)
  - [Utilizando Rotas Diretas](#utilizando-rotas-diretas)
- [Principais Métodos do `Navigator`](#principais-métodos-do-navigator)
- [Passando Parâmetros entre Rotas](#passando-parâmetros-entre-rotas)
  - [Passando Parâmetros Utilizando `Navigator`](#passando-parâmetros-utilizando-navigator)
  - [Passando Parâmetros Utilizando Construtores de Rotas](#passando-parâmetros-utilizando-construtores-de-rotas)
- [Definição Dinâmica de Rotas com `onGenerateRoute`](#definição-dinâmica-de-rotas-com-ongenerateroute)
- [Monitorando a Navegação com Observers](#monitorando-a-navegação-com-observers)
- [Utilizando `async`/`await` na Navegação](#utilizando-asyncawait-na-navegação)

## Introdução

A navegação em um aplicativo Flutter é gerenciada pelo widget `Navigator`, que funciona como uma pilha de rotas, permitindo que os desenvolvedores movam-se entre diferentes telas ou páginas. O Flutter oferece uma maneira estruturada e flexível para definir e manipular rotas, seja através de rotas nomeadas ou rotas diretas. Entender como utilizar `Navigator` e `Routes` é fundamental para a construção de aplicativos complexos e bem-organizados.

## Estrutura Básica e Funcionamento

O `Navigator` é um widget que gerencia uma pilha de widgets de roteamento, onde cada tela ou página é uma rota. A navegação entre estas rotas é realizada através de métodos como `push`, `pop`, `pushReplacement`, entre outros.

Uma `Route` representa uma tela ou página em um aplicativo Flutter. As rotas podem ser definidas diretamente ou nomeadas. O uso de rotas nomeadas é uma prática comum para facilitar a navegação e a manutenção do código.

### Utilizando Rotas Nomeadas

Primeiro, é necessário definir as rotas no aplicativo. Isso é feito no `MaterialApp`, onde as rotas são mapeadas para widgets específicos.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Navigator Demo',
      initialRoute: HomePage.routeName,
      routes: {
        HomePage.routeName: (context) => HomePage(),
        SecondPage.routeName: (context) => SecondPage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  static String routeName = '/';
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, SecondPage.routeName);
          },
          child: Text('Go to Second Page'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  static String routeName = '/second';
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Go Back'),
        ),
      ),
    );
  }
}
```

Neste exemplo, há duas páginas: `HomePage` e `SecondPage`. A navegação entre elas é realizada utilizando `Navigator.pushNamed` e `Navigator.pop`.

### Utilizando Rotas Diretas

Além das rotas nomeadas, também é possível utilizar rotas diretas, criando instâncias de `MaterialPageRoute`.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Navigator Demo',
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondPage()),
            );
          },
          child: Text('Go to Second Page'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Go Back'),
        ),
      ),
    );
  }
}
```

Neste exemplo, a navegação é realizada através de `Navigator.push` com `MaterialPageRoute`, criando uma rota direta para `SecondPage`.

## Principais Métodos do `Navigator`

- **`push`**: Adiciona uma nova rota na pilha de navegação.

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => NewPage()),
);
```

- **`pop`**: Remove a rota superior da pilha de navegação, retornando à rota anterior.

```dart
Navigator.pop(context);
```

- **`popUntil`**: Remove rotas da pilha até que uma rota específica seja encontrada.

```dart
Navigator.popUntil(context, ModalRoute.withName('/routeName'));
```

- **`pushReplacement`**: Substitui a rota atual por uma nova rota.

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => NewPage()),
);
```

- **`pushNamed`**: Adiciona uma nova rota nomeada na pilha de navegação.

```dart
Navigator.pushNamed(context, '/routeName');
```

- **`pushNamedAndRemoveUntil`**: Empurra uma rota nomeada e remove todas as outras rotas até uma condição específica.

```dart
Navigator.pushNamedAndRemoveUntil(context, '/home', ModalRoute.withName('/'));
```

## Passando Parâmetros entre Rotas

Passar parâmetros entre rotas na navegação de um projeto Flutter é uma prática comum e importante para transmitir dados entre diferentes telas ou páginas. Isso possibilita a personalização da interface do usuário com base em informações específicas, como dados de usuário, configurações ou qualquer outro tipo de informação relevante para a aplicação.

### Passando Parâmetros Utilizando `Navigator`

Para passar parâmetros entre rotas utilizando o `Navigator` em Flutter, é necessário utilizar o construtor `Navigator.push` ou `Navigator.pushNamed` junto com o parâmetro arguments.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Passando Parâmetros',
      initialRoute: '/',
      routes: {
        '/': (context) => HomePage(),
        '/second': (context) => SecondPage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    String message = 'Mensagem da HomePage';
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(
              context,
              '/second',
              arguments: message,
            );
          },
          child: Text('Ir para a SecondPage'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Recebendo os parâmetros passados pela rota anterior
    final String args = ModalRoute.of(context)?.settings.arguments as String;

    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'Parâmetro recebido:',
              style: TextStyle(fontSize: 18),
            ),
            Text(
              args,
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text('Voltar'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Neste exemplo, a `HomePage` passa uma mensagem para a `SecondPage` através do parâmetro arguments ao utilizar `Navigator.pushNamed`. Na `SecondPage`, o parâmetro é recuperado usando `ModalRoute.of(context).settings.arguments`.

### Passando Parâmetros Utilizando Construtores de Rotas

Também é possível passar parâmetros utilizando construtores de rotas personalizados.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Passando Parâmetros',
      initialRoute: '/',
      routes: {
        '/': (context) => HomePage(),
        '/second': (context) => SecondPage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) =>
                    SecondPage(message: 'Parâmetro da HomePage'),
              ),
            );
            ;
          },
          child: Text('Ir para a SecondPage'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  final String? message;

  SecondPage({this.message});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'Parâmetro recebido:',
              style: TextStyle(fontSize: 18),
            ),
            Text(
              message ?? 'Parâmetro não recebido',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text('Voltar'),
            ),
          ],
        ),
      ),
    );
  }
}
```

Neste exemplo, na `SecondPage`, o parâmetro `message` é recebido através do construtor da classe.

## Definição Dinâmica de Rotas com `onGenerateRoute`

O `onGenerateRoute` é um recurso poderoso do Flutter que permite a definição dinâmica de rotas em um aplicativo, especialmente útil quando se trabalha com rotas nomeadas de forma mais flexível. Ele oferece a capacidade de gerar rotas com base nas necessidades do aplicativo, como a passagem de parâmetros adicionais ou a lógica condicional para determinar qual tela exibir.

O `onGenerateRoute` é configurado dentro do `MaterialApp` e recebe uma função que é chamada quando uma rota nomeada é solicitada e não está presente na definição estática de rotas (`routes`). Isso permite uma abordagem mais dinâmica para a navegação entre telas.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter onGenerateRoute',
      initialRoute: '/',
      onGenerateRoute: (settings) {
        switch (settings.name) {
          case '/':
            return MaterialPageRoute(builder: (_) => HomePage());
          case '/second':
            // Exemplo de passagem de parâmetros via onGenerateRoute
            return MaterialPageRoute(
              builder: (_) => SecondPage(message: settings.arguments as String),
            );
          default:
            return MaterialPageRoute(builder: (_) => NotFoundPage());
        }
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/second', arguments: 'Mensagem da HomePage');
          },
          child: Text('Ir para a SecondPage'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  final String message;

  SecondPage({required this.message});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'Parâmetro recebido:',
              style: TextStyle(fontSize: 18),
            ),
            Text(
              message,
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text('Voltar'),
            ),
          ],
        ),
      ),
    );
  }
}

class NotFoundPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Página não encontrada'),
      ),
      body: Center(
        child: Text(
          '404 - Página não encontrada',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}
```

No exemplo acima:

- O `MaterialApp` utiliza `onGenerateRoute` para definir dinamicamente as rotas com base no nome da rota (`settings.name`).
- Quando a rota `/` é solicitada, `onGenerateRoute` retorna uma `MaterialPageRoute` que constrói `HomePage`.
- Quando a rota `/second` é solicitada, `onGenerateRoute` retorna uma `MaterialPageRoute` que constrói `SecondPage`, passando o parâmetro `message` obtido de `settings.arguments`.
- Se nenhuma rota correspondente for encontrada, `onGenerateRoute` retorna uma rota para `NotFoundPage`.

O `onGenerateRoute` é especialmente útil em situações onde:

- As rotas do aplicativo podem ser geradas dinamicamente com base em lógica de negócios ou parâmetros dinâmicos.
- A aplicação requer uma manipulação mais avançada de parâmetros de navegação, como a passagem de dados entre telas.
- A aplicação precisa de uma maneira mais robusta de lidar com rotas não definidas ou erros de navegação.

## Monitorando a Navegação com Observers

Os **observers** na navegação em um projeto Flutter são objetos que permitem monitorar e reagir a eventos relacionados à navegação, como a abertura de uma nova rota, o fechamento de uma rota, entre outros. Isso proporciona uma maneira poderosa de observar e registrar o comportamento da navegação dentro do aplicativo, além de permitir a implementação de lógica adicional com base nesses eventos.

Para utilizar observers, pode-se configurar um `NavigatorObserver` personalizado e adicioná-lo ao `MaterialApp` através do parâmetro `navigatorObservers`. Isso permite que o observer receba notificações sobre eventos de navegação.

Aqui está um exemplo simples de como criar e usar um `NavigatorObserver` para imprimir mensagens no console sempre que uma nova rota for aberta ou fechada:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class NavigationObserverExample extends NavigatorObserver {
  @override
  void didPush(Route<dynamic> route, Route<dynamic>? previousRoute) {
    super.didPush(route, previousRoute);
    print('Nova rota aberta: ${route.settings.name}');
  }

  @override
  void didPop(Route<dynamic> route, Route<dynamic>? previousRoute) {
    super.didPop(route, previousRoute);
    print('Rota fechada: ${route.settings.name}');
  }
}

class MyApp extends StatelessWidget {
  final NavigationObserverExample navigatorObserver = NavigationObserverExample();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Navigation Observers',
      initialRoute: '/',
      navigatorObservers: [navigatorObserver],
      routes: {
        '/': (context) => HomePage(),
        '/second': (context) => SecondPage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/second');
          },
          child: Text('Ir para a SecondPage'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Second Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Voltar'),
        ),
      ),
    );
  }
}
```

No exemplo acima:

- **`NavigationObserverExample`**: Classe que estende `NavigatorObserver` e implementa os métodos `didPush` e `didPop`. Estes métodos são chamados quando uma rota é aberta (`didPush`) ou fechada (`didPop`), permitindo que a aplicação registre esses eventos.
- **`MyApp`**: Classe principal onde o `navigatorObserver` é instanciado e adicionado à lista `navigatorObservers` dentro de `MaterialApp`. Isso garante que o observer receba notificações de eventos de navegação em todo o aplicativo.
- **`HomePage` e `SecondPage`**: São telas simples que navegam entre si através de botões de navegação.

Os principais benefícios dos observers na navegação de um aplicativo são:

- **Monitoramento Avançado**: Permite monitorar e registrar o comportamento da navegação, facilitando a depuração e a compreensão do fluxo de navegação do aplicativo.
- **Implementação de Lógica Adicional**: Possibilita adicionar lógica personalizada com base em eventos de navegação, como pré-carregamento de dados ao abrir uma nova tela ou ação específica ao fechar uma tela.

## Utilizando `async`/`await` na Navegação

Para aguardar o recebimento de parâmetros de forma assíncrona na navegação em um projeto Flutter, pode-se utilizar abordagens que combinam o uso de `async`/`await` com os métodos disponíveis no `Navigator`.

Para aguardar o recebimento de parâmetros de forma assíncrona durante a navegação, deve-se ao navegar de uma tela para outra, utilizar o método `Navigator.pushNamed` ou `Navigator.push` para passar os parâmetros desejados.

```dart
// Navegando para a próxima tela com parâmetros
var result = await Navigator.pushNamed(context, '/nextPage', arguments: {'param1': 'valor1', 'param2': 'valor2'});
```

Na tela de destino, é possível acessar esses parâmetros utilizando `ModalRoute.of(context).settings.arguments`. Aqui, pode-se utilizar `await` para esperar até que os parâmetros sejam recebidos.

```dart
// Aguardando a recepção dos parâmetros de forma assíncrona
var args = await Future.delayed(Duration(milliseconds: 500), () => ModalRoute.of(context)?.settings.arguments);
```

Abaixo está um exemplo mais completo que demonstra como realizar a navegação assíncrona com aguardo de parâmetros:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Await Navigation',
      initialRoute: '/',
      routes: {
        '/': (context) => HomePage(),
        '/nextPage': (context) => NextPage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            // Navegando para a próxima tela com parâmetros
            var result = await Navigator.pushNamed(context, '/nextPage', arguments: {'param1': 'valor1', 'param2': 'valor2'});
            print('Resultado recebido na HomePage: $result');
          },
          child: Text('Ir para a NextPage'),
        ),
      ),
    );
  }
}

class NextPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Next Page'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            // Simulando um atraso na recepção dos parâmetros
            var args = await Future.delayed(Duration(milliseconds: 2000), () => ModalRoute.of(context)?.settings.arguments);
            print('Parâmetros recebidos na NextPage: $args');
            Navigator.pop(context, 'Retorno da NextPage');
          },
          child: Text('Voltar'),
        ),
      ),
    );
  }
}
```

No exemplo, a `HomePage` Contém um botão que, ao ser pressionado, navega para a `NextPage` e aguarda o retorno de parâmetros através de `Navigator.pushNamed`. Já a `NextPage` simula um atraso de recebimento de parâmetros utilizando `Future.delayed`, permitindo que a `HomePage` aguarde a recepção dos parâmetros de forma assíncrona.

Utilizar `async`/`await` na navegação em um projeto Flutter é uma abordagem eficaz para aguardar o recebimento de parâmetros de forma assíncrona entre diferentes telas. Isso não apenas melhora a experiência do usuário, garantindo que os dados estejam prontamente disponíveis, mas também ajuda na organização e controle do fluxo de navegação dentro do aplicativo.

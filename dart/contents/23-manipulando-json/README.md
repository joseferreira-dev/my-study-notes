<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Manipulação de Arquivos JSON

- [Introdução](#introdução)


## Introdução

Dart, assim como a maioria das linguagens de programação, possui recursos nativos para se trabalhar com a manipulação de arquivos JSON (Javascript Object Notation).

## Leitura e Escrita de Arquivos Locais

Para demonstrar como a manipulação de dados locais ocorre, considera-se uma lista de alunos cadastrados em diversas disciplinas. Suponha-se que os dados dos alunos estejam salvos em um arquivo JSON local chamado `alunos.json` com o seguinte conteúdo:

```json
{
  "alunos": [
    {
      "nome": "João",
      "idade": 20,
      "disciplinas": ["Matemática", "Física"]
    },
    {
      "nome": "Maria",
      "idade": 22,
      "disciplinas": ["Química", "Biologia"]
    },
    {
      "nome": "Pedro",
      "idade": 21,
      "disciplinas": ["História", "Geografia"]
    }
  ]
}
```

Este arquivo está dentro da pasta `lib` do projeto e o código principal está no arquivo `alunos.dart` na pasta `bin`.

Primeiramente, deve-se adicionar as dependências necessárias para a manipulação. Neste exemplo, será utilizado o pacote `path` para gerar o caminho relativo do arquivo `alunos.json`.

```shell
dart pub add path
```

No arquivo principal `alunos.dart` devem ser importados os pacotes necessários e em seguida escrito o código de manipulação, que neste caso trata-se da leitura e impressão dos dados de todos os alunos.

```dart
import 'dart:convert';
import 'dart:io';
import 'package:path/path.dart' as path;

void main() async {
  // Cria um caminho relativo ao arquivo alunos.json com o pacote 'path'
  final filePath = path.join(Directory.current.path, 'lib', 'alunos.json');
  // Recebe o arquivo localizado no caminho especificado
  final file = File(filePath);

  // Verifica se o arquivo existe
  // Se existir, transforma-o em uma String e decodifica em um Map
  if (await file.exists()) {
    final contents = await file.readAsString();
    final data = jsonDecode(contents);

    // Manipulando os dados
    List alunos = data['alunos'];
    for (var aluno in alunos) {
      print('Nome: ${aluno['nome']}');
      print('Idade: ${aluno['idade']}');
      print('Disciplinas: ${aluno['disciplinas'].join(', ')}');
      print('---');
    }
  } else {
    print('Arquivo não encontrado.');
  }
}
```

Agora, para se adicionar um novo aluno no arquivo `alunos.json`, o que poderia ser feito é o seguinte.

```dart
import 'dart:convert';
import 'dart:io';
import 'package:path/path.dart' as path;

void main() async {
  // Cria um caminho relativo ao arquivo alunos.json com o pacote 'path'
  final filePath = path.join(Directory.current.path, 'lib', 'alunos.json');
  // Recebe o arquivo localizado no caminho especificado
  final file = File(filePath);

  // Verifica se o arquivo existe
  // Se existir, transforma-o em uma String e decodifica em um Map
  if (await file.exists()) {
    final contents = await file.readAsString();
    final data = jsonDecode(contents);

    // Adicionando um novo aluno
    List alunos = data['alunos'];
    alunos.add({
      "nome": "Ana",
      "idade": 23,
      "disciplinas": ["Literatura", "Artes"]
    });

    // Convertendo de volta para JSON
    final jsonString = jsonEncode(data);

    // Salvando as alterações no arquivo
    await file.writeAsString(jsonString);
    print('Novo aluno adicionado e arquivo atualizado.');
  } else {
    print('Arquivo não encontrado.');
  }
}
```

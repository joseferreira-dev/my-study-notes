<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Importações (Imports)

- [Introdução](#introdução)
- [Importação Básica](#importação-básica)
- [Importações Relativas e Absolutas](#importações-relativas-e-absolutas)
- [Pacotes e pubspec.yaml](#pacotes-e-pubspecyaml)
- [Alias de Biblioteca](#alias-de-biblioteca)
- [Mostrar e Ocultar Componentes](#mostrar-e-ocultar-componentes)
- [Exportando Bibliotecas](#exportando-bibliotecas)
- [Importações Diferenciadas com `deferred as`](#importa-es-diferenciadas-com-deferred-as)
- [Configurando um Padrão de Importação em um Projeto](#configurando-um-padrão-de-importação-em-um-projeto)

## Introdução

Em Dart, um código pode ser organizado em diferentes arquivos, que são importados conforme necessário para reutilizar funções, classes, constantes e outros elementos. Existem diferentes formas de importar bibliotecas e pacotes em Dart.

## Importação Básica

Para importar um arquivo usa-se a palavra-chave `import` seguida pelo caminho do arquivo entre aspas simples ou duplas.

```dart
// lib/main.dart
import 'utils.dart';

void main() {
  saudacao();
}

// lib/utils.dart
void saudacao() {
  print('Olá, mundo!');
}
```

## Importações Relativas e Absolutas

É possível fazer importações relativas ou absolutas em Dart. Nas relativas usa-se caminhos relativos ao local do arquivo atual.

```dart
// lib/main.dart
import 'utils.dart'; // Importa utils.dart do mesmo diretório
import 'subdir/other_utils.dart'; // Importa other_utils.dart do subdiretório subdir
```

Nas importações absolutas usa-se o pacote como base para o caminho. Isso é comum quando se trabalha com pacotes.

```dart
// lib/main.dart
import 'package:meu_pacote/utils.dart';
import 'package:meu_pacote/subdir/other_utils.dart';
```

## Pacotes e pubspec.yaml

Quando se está usando pacotes de terceiros ou deseja-se organizar seu próprio código como um pacote, deve-se adicionar as dependências no arquivo `pubspec.yaml`.

```yaml
name: meu_pacote
dependencies:
  http: ^0.13.3
```

Depois de adicionar a dependência, pode-se importar o pacote:

```dart
import 'package:http/http.dart' as http;

void main() {
  var url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  http.get(url).then((response) {
    print('Response status: ${response.statusCode}');
    print('Response body: ${response.body}');
  });
}
```

## Alias de Biblioteca

Usa-se aliases para evitar conflitos de nome e tornar o código mais claro.

```dart
import 'package:http/http.dart' as http;
import 'package:meu_pacote/utils.dart' as utils;

void main() {
  utils.saudacao();

  var url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  http.get(url).then((response) {
    print('Response status: ${response.statusCode}');
    print('Response body: ${response.body}');
  });
}
```

## Mostrar e Ocultar Componentes

Pode-se especificar quais partes de uma biblioteca deseja-se importar usando as palavras-chave `show` e `hide`.

```dart
// Importa apenas a função saudacao de utils.dart
import 'utils.dart' show saudacao;

// Importa tudo de utils.dart exceto a função saudacao
import 'utils.dart' hide saudacao;
```

## Exportando Bibliotecas

Usa-se a palavra reservada `export` para reexportar componentes de outras bibliotecas, facilitando a criação de APIs públicas em um pacote.

```dart
// lib/utils.dart
void saudacao() {
  print('Olá, mundo!');
}

// lib/api.dart
export 'utils.dart';

// lib/main.dart
import 'package:meu_pacote/api.dart';

void main() {
  saudacao(); // Saudacao é acessível aqui devido à reexportação
}
```

## Importações Diferenciadas com `deferred as`

Para carregamento tardio (lazy loading), pode-se usar `deferred as` para carregar uma biblioteca somente quando necessário.

```dart
import 'utils.dart' deferred as utils;

void main() async {
  await utils.loadLibrary(); // Carrega a biblioteca
  utils.saudacao();
}
```

## Configurando um Padrão de Importação em um Projeto

Para configurar o tipo de importação padrão em um projeto Dart, pode-se usar o linter do Dart junto com as regras recomendadas para importação, o linter já vem pré-instalado no projeto como dependência de desenvolvimento no arquivo `pubspec.yaml`.

```yaml
dev_dependencies:
  lints: ^2.1.0 # Linter
  test: ^1.24.0
```

O arquivo de configuração para o linter em um projeto Dart é o `analysis_options.yaml`. Este arquivo permite que se defina regras de linting específicas para o projeto, ajudando a manter a consistência e a qualidade do código. Por padrão, o arquivo vem dessa forma:

```yaml
include: package:lints/recommended.yaml

# Uncomment the following section to specify additional rules.

# linter:
#   rules:
#     - camel_case_types
```

Para aplicar um padrão de importação, basta descomentar o `linter` e adicionar a regra específica. Duas opções são possíveis para padronizar importações: `prefer_relative_imports` (importações relativas como padrão) e `always_use_package_imports` (importações absolutas como padrão).

```yaml
include: package:lints/recommended.yaml

linter:
  rules:
    - prefer_relative_imports: true
    # - always_use_package_imports: true
```

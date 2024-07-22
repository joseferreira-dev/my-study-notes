<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner_dart.png"></a>
</div>
<br>

# Maps (Mapas)

- [Criar um Map](#criar-um-map)
- [Acessar e Modificar Elementos](#acessar-e-modificar-elementos)
- [Adicionar Elementos](#adicionar-elementos)
- [Remover Elementos](#remover-elementos)
- [Tamanho de um Map](#tamanho-de-um-map)
- [Consultas](#consultas)
- [Propriedades `keys`, `values` e `entries`](#propriedades-keys-values-e-entries)
- [Transformações de Maps](#transformações-de-maps)

Em Dart, um `Map` (mapa) é uma coleção de pares chave-valor, onde cada chave é única e está associada a um valor. Os mapas são úteis para armazenar e manipular dados de maneira eficiente, especialmente quando precisamos aceesar rápidamente valores com base em chaves específicas.

## Criar um Map

Pode-se criar um `Map` usando o construtor padrão ou a notação literal com chaves `{}`.

```dart
void main() {
  Map<int, String> frutas = Map();
  frutas[1] = 'maçã';
  frutas[2] = 'banana';
  frutas[3] = 'laranja';

  Map<int, String> animais = {
    1: 'gato',
    2: 'cachorro',
    3: 'hamster'
  };

  print(frutas); // Output: {1: maçã, 2: banana, 3: laranja}
  print(animais); // Output: {1: gato, 2: cachorro, 3: hamster}
}
```

## Acessar e Modificar Elementos

Podemos acessar e modificar os valores em um `Map` usando suas chaves.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais['Brasil']); // Output: Brasília

  capitais['Brasil'] = 'Brasilia';
  print(capitais['Brasil']); // Output: Brasilia
}
```

Dois métodos específicos para modificar elementos em um mapa são `update` e `updateAll`. O método `updateAll` atualiza todos os valores no mapa usando uma função de atualização fornecida. Essa função é aplicada a cada par chave-valor no mapa.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.updateAll((chave, valor) => 'Capital: $valor');

  print(capitais);
  // Output: {Brasil: Capital: Brasília, França: Capital: Paris, Japão: Capital: Tóquio}
}
```

O método `update` atualiza o valor de uma chave específica no mapa usando uma função de atualização. Se a chave não existir, pode-se opcionalmente fornecer uma função `ifAbsent` para adicionar a chave com um valor padrão.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.update('Brasil', (valor) => 'Brasília');
  
  // Tentar atualizar a capital de um país não existente, fornecendo um valor padrão
  capitais.update('Alemanha', (valor) => 'Berlim', ifAbsent: () => 'Berlim');
  print(capitais);
  // Output: {Brasil: Nova Brasília, França: Paris, Japão: Tóquio, Alemanha: Berlim}
}
```

## Adicionar Elementos

Para adicionar novos elementos basta adicionar uma nova chave e o seu valor correspondente.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Distrito Federal',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais['EUA'] = 'Washington';
  print(capitais); // Output: {Brasil: Brasilia, França: Paris, Japão: Tóquio, EUA: Washington}
}
```

Também existe o método `addAll`, que adiciona vários elementos de uma vez.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.addAll({'EUA': 'Washington', 'Portugal': 'Lisboa'});
  print(capitais);
  // Output: {Brasil: Brasília, França: Paris, Japão: Tóquio, EUA: Washington, Portugal: Lisboa}
}
```

Por fim, existe o método `putIfAbsent`, que só adiciona o valor se a chave informada ainda não existir no `Map`, permitindo ainda a execução de operações com o valor a ser inserido.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.putIfAbsent('Brasil', () => 'Distrito Federal');
  print(capitais); // Output: {Brasil: Brasília, França: Paris, Japão: Tóquio}
}
```

## Remover Elementos

Em um `Map` podemos usar o método `remove` para remover um elemento indicando a sua chave e o método `clear` para remover todos os elementos.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  capitais.remove('Brasil');
  print(capitais); // Output: {França: Paris, Japão: Tóquio}
  
  capitais.clear();
  print(capitais); // Output: {}
}
```

Podemos também remover vários elementos que satisfazem uma condição específica com o método `removeWhere`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };
  
  capitais.removeWhere((key, value) => key == 'França');
  print(capitais); // Output: {Brasil: Brasília, Japão: Tóquio}
}
```

## Tamanho de um Map

A propriedade `length` de um `Map` corresponde ao número de elementos que ele possui.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.length); // Output: 3
}
```

## Consultas

Nos mapa podemos consultar a existência de elementos por meio de suas chaves, com `containsKey`, e por meio de seus valores, com `containsValue`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.containsKey('Brasil')); // Output: true
  print(capitais.containsValue('Paris')); // Output: true
}
```

Também podemor verificar se um mapa contém ou não elementos com as propriedades `isEmpty` e `isNotEmpty`.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  print(capitais.isEmpty); // Output: false
  print(capitais.isNotEmpty); // Output: true
}
```

## Propriedades `keys`, `values` e `entries`

Os mapas possuem três propriedades muito importantes acerca de seus elementos, sendo elas `keys`, `values` e `entries`. A propriedade `keys` retorna um iterável com todas as chaves do `Map`, `values` por sua vez retorna todos os valores, já `entries` retorna um iterável com todas as entradas do tipo `MapEntry`, onde cada uma contém uma chave e um valor.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };
  
  Iterable<String> chaves = capitais.keys;
  print(chaves); // Output: (Brasil, França, Japão)

  Iterable<String> valores = capitais.values;
  print(valores); // Output: (Brasília, Paris, Tóquio)
  
  Iterable<MapEntry<String, String>> entradas = capitais.entries;
  print(entradas); // Output: (MapEntry(Brasil: Brasília), MapEntry(França: Paris), MapEntry(Japão: Tóquio))
}
```

## Transformações de Maps

Suponhamos que queremos transformar o mapa de capitais de modo que os valores sejam alterados para incluir uma descrição adicional. Pode-se usar a propriedade `entries` para iterar sobre os pares chave-valor e o método `map` para criar um novo mapa transformado.

```dart
void main() {
  Map<String, String> capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
  };

  // Transformar o mapa adicionando uma descrição aos valores
  Map<String, String> capitaisTransformadas = Map.fromEntries(
    capitais.entries.map((entrada) {
      String novaDescricao = '${entrada.value} é a capital de ${entrada.key}';
      return MapEntry(entrada.key, novaDescricao);
    })
  );

  print(capitaisTransformadas);
  // Output: {Brasil: Brasília é a capital de Brasil, França: Paris é a capital de França, Japão: Tóquio é a capital de Japão}
}
```

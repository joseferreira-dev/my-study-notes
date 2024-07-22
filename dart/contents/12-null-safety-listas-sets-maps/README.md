<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner_dart.png"></a>
</div>
<br>

# Usando Null Safety com Listas, Sets e Maps

- [Usando Null Safety com Listas](#usando-null-safety-com-listas)
  - [Listas Nulas e Elementos Nulos](#listas-nulas-e-elementos-nulos)
  - [Acesso a Elementos Nulos](#acesso-a-elementos-nulos)
  - [Lista com Valor Padrão](#lista-com-valor-padrão)
  - [Iterando com Listas Nulas](#iterando-com-listas-nulas)
- [Usando Null Safety com Conjuntos](#usando-null-safety-com-conjuntos)
  - [Conjuntos Nulos e Elementos Nulos](#conjuntos-nulos-e-elementos-nulos)
  - [Acesso a Elementos Nulos](#acesso-a-elementos-nulos-1)
  - [Conjuntos com Valor Padrão](#conjuntos-com-valor-padrão)
  - [Iterando com Conjuntos Nulos](#iterando-com-conjuntos-nulos)
- [Usando Null Safety com Mapas](#usando-null-safety-com-mapas)
  - [Mapas Nulos e Elementos Nulos](#mapas-nulos-e-elementos-nulos)
  - [Acesso a Elementos Nulos](#acesso-a-elementos-nulos-2)
  - [Mapas com Valor Padrão](#mapas-com-valor-padrão)
  - [Iterando com Mapas Nulos](#iterando-com-mapas-nulos)

Quando trabalhamos com listas, conjuntos e mapas, o Null Safety nos permite trabalhar com coleções que podem ser nulas ou conter elementos nulos de forma mais assegurada.

## Usando Null Safety com Listas

Quando trabalha-se com listas, o Null Safety permite que se trabalhe de forma mais assegurada com listas que podem ser nulas ou conter elementos nulos.

### Listas Nulas e Elementos Nulos

Podemos declarar uma lista que pode ser nula usando o operador `?` após a declaração da lista. Isso significa que a variável pode ser uma lista ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  List<String>  frutas1 = null;   // Não compila
  List<String>? frutas2 = null;   // Compila
  List<String>? frutas3 = [null]; // Não Compila
}
```

Colocando o operador `?` após a declaração do tipo da lista definimos que a lista pode conter elementos nulos, mas ela em si não pode ser nula.

```dart
void main() {
  List<String>  frutas1 = [null];   // Não compila
  List<String?> frutas2 = [null];   // Compila
  List<String?> frutas3 = null;     // Não Compila
}
```

Usando as duas formas de declaração unidas podemos definir uma lista que pode ser nula e que pode conter elementos nulos.

```dart
void main() {
  List<String?>? frutas1 = [null];  // Compila
  List<String?>? frutas2 = null;    // Compila
}
```

### Acesso a Elementos Nulos

O operador de acesso seguro `?.` pode ser usado para acessar elementos de uma lista que podem ser nulos.

```dart
void main() {
  List<String?> frutas = ['maçã', null, 'laranja'];
  
  String? primeiraFruta = frutas[0];
  print(primeiraFruta?.toUpperCase()); // Output: MAÇÃ
  
  String? segundaFruta = frutas[1];
  print(segundaFruta?.toUpperCase()); // Output: null
}
```

### Lista com Valor Padrão

Um valor padrão pode ser atribuido para uma lista que pode ser nula usando o operador `??`.

```dart
void main() {
  List<String>? frutas;

  List<String> frutasSeguras = frutas ?? [];
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar um valor padrão aos elementos de uma lista caso estes sejam nulos.

```dart
void main() {
  String fruta1 = 'maça';
  String? fruta2 = null;
  String fruta3 = 'laranja';
  
  List<String> frutas = [
    fruta1,
    fruta2 ?? 'banana',
    fruta3
  ];
  
  print(frutas); // Output: [maça, banana, laranja]
}
```

### Iterando com Listas Nulas

É possível utilizar os métodos como `map`, `where`, entre outros, lidando com elementos nulos de maneira segura.

```dart
void main() {
  List<String?> frutas = ['maçã', null, 'laranja'];
  
  // Remove elementos nulos
  List<String> frutasSemNulos = frutas
    .where((fruta) => fruta != null)
    .map((fruta) => fruta!)
    .toList();

  print(frutasSemNulos); // Output: [maçã, laranja]
}
```

## Usando Null Safety com Conjuntos

O Null Safety também permite que se trabalhe de forma mais assegurada com conjuntos que podem ser nulos ou conter elementos nulos.

### Conjuntos Nulos e Elementos Nulos

Podemos declarar um `Set` que pode ser nulo usando o operador `?` após a sua declaração. Isso significa que a variável pode ser um conjunto ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  Set<String>  frutas1 = null;   // Não compila
  Set<String>? frutas2 = null;   // Compila
  Set<String>? frutas3 = {null}; // Não Compila
}
```

Colocando o operador `?` após a declaração do tipo do `Set` define que ele pode conter elementos nulos, mas o `Set` em si não pode ser nulo.

```dart
void main() {
  Set<String>  frutas1 = {null};   // Não compila
  Set<String?> frutas2 = {null};   // Compila
  Set<String?> frutas3 = null;     // Não Compila
}
```

Usando as duas formas de declaração unidas podemos definir um `Set` que pode ser nulo e que pode conter elementos nulos.

```dart
void main() {
  Set<String?>? frutas1 = {null};  // Compila
  Set<String?>? frutas2 = null;    // Compila
}
```

### Acesso a Elementos Nulos

O operador de acesso seguro `?.` pode ser usado para acessar elementos de um `Set` que podem ser nulos.

```dart
void main() {
  Set<String?> frutas = {'maçã', null, 'laranja'};
  
  String? primeiraFruta = frutas.elementAt(0);
  print(primeiraFruta?.toUpperCase()); // Output: MAÇÃ
  
  String? segundaFruta = frutas.elementAt(1);
  print(segundaFruta?.toUpperCase()); // Output: null
}
```

### Conjuntos com Valor Padrão

Um valor padrão pode ser atribuido a um `Set` que pode ser nulo usando o operador `??`.

```dart
void main() {
  Set<String>? frutas;

  Set<String> frutasSeguras = frutas ?? {};
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar valor padrão a elementos de um conjunto caso estes sejam nulos.

```dart
void main() {
  String fruta1 = 'maça';
  String? fruta2 = null;
  String fruta3 = 'laranja';
  
  Set<String> frutas = {
    fruta1,
    fruta2 ?? 'banana',
    fruta3
  };
  
  print(frutas); // Output: {maça, banana, laranja}
}
```

### Iterando com Conjuntos Nulos

Podemos utilizar os métodos como `map`, `where` e outros, lidando com elementos nulos de maneira segura.

```dart
void main() {
  Set<String?> frutas = {'maçã', null, 'laranja'};
  
  // Remove elementos nulos
  Set<String> frutasSemNulos = frutas
    .where((fruta) => fruta != null)
    .map((fruta) => fruta!)
    .toSet();

  print(frutasSemNulos); // Output: {maçã, laranja}
}
```

## Usando Null Safety com Mapas

Quando trabalhamos com mapas, o Null Safety nos permite trabalhar com coleções que podem ser nulas ou conter chaves e valores nulos de forma mais segura.

### Mapas Nulos e Elementos Nulos

Podemos declarar um mapa que pode ser nulo usando o operador `?` após a sua declaração. Isso significa que a variável pode ser um mapa ou `null`, mas as suas chaves e valores não podem ser nulos.

```dart
void main() {
  Map<String, int>  frutas1 = null;           // Não compila
  Map<String, int>? frutas2 = null;           // Compila
  Map<String, int>? frutas3 = {'maçã': null}; // Não compila
}
```

Colocando o operador `?` após a declaração do tipo do mapa, definimos que ele pode conter chaves ou valores nulos, mas o mapa em si não pode ser nulo.

```dart
void main() {
  Map<String, int>  frutas1 = {'maçã': null}; // Não compila
  Map<String, int?> frutas2 = {'maçã': null}; // Compila
  Map<String, int?> frutas3 = null;           // Não compila
}
```

Usando as duas formas de declaração unidas, podemos definir um `Map` que pode ser nulo e que pode conter chaves e valores nulos.

```dart
void main() {
  Map<String, int?>? frutas1 = {'maçã': null}; // Compila
  Map<String, int?>? frutas2 = null;           // Compila
}
```

### Acesso a Elementos Nulos

O operador de acesso seguro `?.` pode ser usado para acessar elementos de um mapa que podem ser nulos.

```dart
void main() {
  Map<String, int?>? frutas = {'maçã': 1, 'banana': null, 'laranja': 3};
  
  int? valorMaca = frutas?['maçã'];
  print(valorMaca); // Output: 1
  
  int? valorBanana = frutas?['banana'];
  print(valorBanana); // Output: null
}
```

### Mapas com Valor Padrão

Um valor padrão pode ser atribuído para um mapa que pode ser nulo usando o operador `??`.

```dart
void main() {
  Map<String, int>? frutas;

  Map<String, int> frutasSeguras = frutas ?? {};
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar um valor padrão aos elementos de um mapa caso estes sejam nulos.

```dart
void main() {
  Map<String, int?> frutas = {'maçã': 1, 'banana': null, 'laranja': 3};
  
  int valorBanana = frutas['banana'] ?? 0;
  print(valorBanana); // Output: 0
}
```

### Iterando com Mapas Nulos

Podemos utilizar os métodos como `map`, `where`, entre outros, lidando com elementos nulos de maneira segura em mapas.

```dart
void main() {
  Map<String, int?>? frutas = {'maçã': 1, 'banana': null, 'laranja': 3};

  // Remove elementos com valores nulos
  Map<String, int> frutasSemNulos = Map.fromEntries(frutas?.entries
          .where((entry) => entry.value != null)
          .map((entry) => MapEntry(entry.key, entry.value!)) ?? []);

  print(frutasSemNulos); // Output: {maçã: 1, laranja: 3}
}
```

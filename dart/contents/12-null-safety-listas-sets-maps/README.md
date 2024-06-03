<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Usando Null Safety com Listas, Sets e Maps

- [Introdução](#introdução)
- [Usando Null Safety com Listas](#usando-null-safety-com-listas)
  * [Listas Nulas e Elementos Nulos](#listas-nulas-e-elementos-nulos)
  * [Acesso a Elementos Nulos](#acesso-a-elementos-nulos)
  * [Lista com Valor Padrão](#lista-com-valor-padrão)
  * [Iterando com Listas Nulas](#iterando-com-listas-nulas)
- [Usando Null Safety com Sets](#usando-null-safety-com-sets)
  * [Sets Nulos e Elementos Nulos](#sets-nulos-e-elementos-nulos)
  * [Acesso a Elementos Nulos](#acesso-a-elementos-nulos-1)
  * [Sets com Valor Padrão](#sets-com-valor-padrão)
  * [Iterando com Sets Nulos](#iterando-com-sets-nulos)
- [Usando Null Safety com Maps](#usando-null-safety-com-maps)

## Introdução

Quando trabalha-se com listas, sets e maps, o Null Safety permite que se trabalhe de forma mais assegurada com estas coleções que podem ser nulas ou conter elementos nulos.

## Usando Null Safety com Listas

Quando trabalha-se com listas, o Null Safety permite que se trabalhe de forma mais assegurada com listas que podem ser nulas ou conter elementos nulos.

### Listas Nulas e Elementos Nulos

Pode-se declarar uma lista que pode ser nula usando o operador `?` após a declaração da lista. Isso significa que a variável pode ser uma lista ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  List<String>  frutas1 = null;   // Não compila
  List<String>? frutas2 = null;   // Compila
  List<String>? frutas3 = [null]; // Não Compila
}
```

Colocando-se o operador `?` após a declaração do tipo da lista define que a lista pode conter elementos nulos, mas elas em si não pode ser nula.

```dart
void main() {
  List<String>  frutas1 = [null];   // Não compila
  List<String?> frutas2 = [null];   // Compila
  List<String?> frutas3 = null;     // Não Compila
}
```

Usando-se as duas formas de declaração unidas pode-se definir uma lista que pode ser nula e que pode conter elementos nulos.

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

O mesmo pode ser feito no caso de adicionar valor padrão a elementos a uma lista caso estes sejam nulos.

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

É possível utilizar os métodos como `map`, `where` e outros, lidando com elementos nulos de maneira segura.

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

## Usando Null Safety com Sets

O Null Safety tmambm permite que se trabalhe de forma mais assegurada com Sets que podem ser nulos ou conter elementos nulos.

### Sets Nulos e Elementos Nulos

Pode-se declarar um `Set` que pode ser nulo usando o operador `?` após a sua declaração. Isso significa que a variável pode ser uma lista ou `null`, mas os seus elementos não podem ser nulos.

```dart
void main() {
  Set<String>  frutas1 = null;   // Não compila
  Set<String>? frutas2 = null;   // Compila
  Set<String>? frutas3 = {null}; // Não Compila
}
```

Colocando-se o operador `?` após a declaração do tipo do `Set` define que ele pode conter elementos nulos, mas o `Set` em si não pode ser nulo.

```dart
void main() {
  Set<String>  frutas1 = {null};   // Não compila
  Set<String?> frutas2 = {null};   // Compila
  Set<String?> frutas3 = null;     // Não Compila
}
```

Usando-se as duas formas de declaração unidas pode-se definir um `Set` que pode ser nulo e que pode conter elementos nulos.

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

### Sets com Valor Padrão

Um valor padrão pode ser atribuido a um `Set` que pode ser nulo usando o operador `??`.

```dart
void main() {
  Set<String>? frutas;

  Set<String> frutasSeguras = frutas ?? {};
  print(frutasSeguras.length); // Output: 0
}
```

O mesmo pode ser feito no caso de adicionar valor padrão a elementos de um `Set` caso estes sejam nulos.

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

### Iterando com Sets Nulos

É possível utilizar os métodos como `map`, `where` e outros, lidando com elementos nulos de maneira segura.

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

## Usando Null Safety com Maps

Conteúdo faltando

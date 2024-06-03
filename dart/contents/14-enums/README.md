<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Enumeradores (Enums)

- [Introdução](#introdução)
- [Criar um Enum](#criar-um-enum)
- [Usando Enums](#usando-enums)
- [Propriedades e Métodos](#propriedades-e-métodos)
- [Adicionar Métodos a um Enum](#adicionar-métodos-a-um-enum)
- [Iterar sobre Enums](#iterar-sobre-enums)

## Introdução

Em Dart, um enumerador (ou enum) é um tipo especial que permite definir um conjunto de valores nomeados e constantes. Os enums são úteis para representar um conjunto fixo de constantes, como os dias da semana, estados de uma aplicação, tipos de usuário, etc.

## Criar um Enum

Para declarar um `enum`, usa-se a palavra-chave `enum` seguida pelo nome do `enum` e uma lista de valores entre chaves.

```dart
enum DiaDaSemana {
  segunda,
  terca,
  quarta,
  quinta,
  sexta,
  sabado,
  domingo,
}
```

## Usando Enums

Para usar os valores de um `enum` pode-se acessá-los pelo nome do `enum` seguido de um ponto `.`, seguido pelo valor.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;

  switch (hoje) {
    case DiaDaSemana.segunda:
      print('Hoje é segunda-feira.');
      break;
    case DiaDaSemana.terca:
      print('Hoje é terça-feira.');
      break;
    case DiaDaSemana.quarta:
      print('Hoje é quarta-feira.');
      break;
    case DiaDaSemana.quinta:
      print('Hoje é quinta-feira.');
      break;
    case DiaDaSemana.sexta:
      print('Hoje é sexta-feira.');
      break;
    case DiaDaSemana.sabado:
      print('Hoje é sábado.');
      break;
    case DiaDaSemana.domingo:
      print('Hoje é domingo.');
      break;
  }
}
```

## Propriedades e Métodos

Todo `enum` possui a propriedade `values` que corresponde a todos os seus valores.

```dart
void main() {
  print(DiaDaSemana.values); 
  // Output: [DiaDaSemana.segunda, DiaDaSemana.terca, DiaDaSemana.quarta, DiaDaSemana.quinta, DiaDaSemana.sexta, DiaDaSemana.sabado, DiaDaSemana.domingo]
}
```

Cada valor do `enum` tem uma propriedade `index` que retorna a posição do valor na declaração do `enum`.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.index); // Output: 4
}
```

Cada valor também tem a propriedade `name` que retorna o nome do valor.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.name); // Output: sexta
}
```

Existe o método `toString` que retorna uma representação de string do valor do `enum`.

```dart
void main() {
  var hoje = DiaDaSemana.sexta;
  print(hoje.toString()); // Output: DiaDaSemana.sexta
}
```

## Adicionar Métodos a um Enum

Desde a versão 2.15 do Dart, é possível adicionar métodos e getters a enums.

```dart
enum Cor {
  vermelho,
  verde,
  azul;

  String get nome {
    switch (this) {
      case Cor.vermelho:
        return 'Vermelho';
      case Cor.verde:
        return 'Verde';
      case Cor.azul:
        return 'Azul';
    }
  }
  
  String get codigoHex {
    switch (this) {
      case Cor.vermelho:
        return '#FF0000';
      case Cor.verde:
        return '#00FF00';
      case Cor.azul:
        return '#0000FF';
    }
  }
}

void main() {
  var minhaCor = Cor.verde;
  print(minhaCor.nome); // Output: Verde
  print(minhaCor.codigoHex); // Output: #00FF00
}
```

## Iterar sobre Enums

Pode-se iterar sobre todos os valores de um `enum` usando a propriedade `values`.

```dart
void main() {
  for (var dia in DiaDaSemana.values) {
    print(dia);
  }
  // Output:
  // DiaDaSemana.segunda
  // DiaDaSemana.terca
  // DiaDaSemana.quarta
  // DiaDaSemana.quinta
  // DiaDaSemana.sexta
  // DiaDaSemana.sabado
  // DiaDaSemana.domingo
}
```
# Capítulo 9 – Programação Orientada a Objetos

Nos capítulos anteriores, construímos um sólido repertório de ferramentas e estruturas da linguagem. Aprendemos a manipular dados, controlar o fluxo de execução e organizar informações em coleções. Agora, estamos prontos para um salto conceitual que mudará fundamentalmente a forma como estruturamos nossos programas. Este capítulo introduz a **Programação Orientada a Objetos (POO)**, um paradigma que não apenas organiza o código, mas também nos permite modelar o mundo real de forma mais intuitiva e poderosa dentro de nossos sistemas.

A POO propõe uma mudança de perspectiva: em vez de focarmos em uma sequência de procedimentos e dados separados, passamos a organizar nosso código em torno de **objetos**. Um objeto é uma entidade autônoma que encapsula tanto os **dados (atributos)** que o descrevem quanto os **comportamentos (métodos)** que ele pode realizar. Essa abordagem nos permite criar componentes modulares, reutilizáveis e fáceis de manter, formando a base para a construção de software complexo e escalável.

## Classes e Objetos: Os Pilares da POO

No coração da Programação Orientada a Objetos residem dois conceitos interdependentes: **classes** e **objetos**.

- Uma **Classe** é o projeto, o molde ou a planta baixa a partir da qual os objetos são criados. Ela define um conjunto de atributos (as características que os objetos terão) e métodos (as ações que os objetos poderão executar). A classe em si não é um dado concreto; é a definição abstrata de um tipo de objeto.
- Um **Objeto** é uma instância concreta e viva de uma classe. Se a classe `Carro` é o projeto, um `Ford Ka azul 2023` é um objeto. Cada objeto possui seu próprio estado (os valores de seus atributos), mas compartilha os mesmos comportamentos (métodos) definidos por sua classe.

Para criar uma classe em Python, utilizamos a palavra-chave `class`. Vamos analisar uma estrutura básica para entender seus componentes.

```python
class Carro:
    # Este é o método construtor, chamado ao criar um novo objeto
    def __init__(self, modelo, cor):
        # Atributos de instância: cada objeto terá sua própria cópia
        self.modelo = modelo
        self.cor = cor

    # Este é um método de instância
    def acelerar(self):
        print(f"O carro {self.modelo} está acelerando.")
```

Vamos dissecar os componentes desta classe:

- **`class Carro:`**: Declara o início da definição da classe chamada `Carro`.
- **`def __init__(self, modelo, cor):`**: Este é um método especial chamado **construtor**. Ele é executado automaticamente sempre que um novo objeto da classe é criado.
- **`self`**: Este é talvez o conceito mais crucial. `self` é um parâmetro que representa a **própria instância do objeto** que está sendo criada ou que está chamando um método. É através do `self` que acessamos os atributos e outros métodos daquele objeto específico.
- **`self.modelo = modelo`**: Esta linha cria um **atributo de instância** chamado `modelo` e atribui a ele o valor do parâmetro `modelo` que foi passado para o construtor.
- **`def acelerar(self):`**: Este é um método de instância, uma função que pertence à classe e que define um comportamento. Note que ele também recebe `self` como seu primeiro parâmetro, o que lhe dá acesso aos atributos do objeto (como `self.modelo`).

### Método Construtor `__init__()`

O método `__init__()` é o ponto de partida para a vida de um objeto. Seu nome, com sublinhados duplos no início e no fim, indica que é um "método mágico" ou "especial" para o Python. Sua principal responsabilidade é **inicializar o estado do objeto**, ou seja, definir os valores iniciais para seus atributos.

Quando definimos `def __init__(self, modelo, cor)`, estamos dizendo que, para criar um objeto `Carro`, é obrigatório fornecer os valores para `modelo` e `cor`. Dentro do construtor, as linhas `self.modelo = modelo` e `self.cor = cor` pegam esses valores fornecidos e os "guardam" dentro do objeto, tornando-os acessíveis durante toda a sua existência.

### Instanciando um Objeto

Com nossa classe `Carro` definida, agora podemos usá-la como um molde para criar objetos concretos. O processo de criar um objeto a partir de uma classe é chamado de **instanciação**. Isso é feito chamando o nome da classe como se fosse uma função e passando os argumentos exigidos pelo construtor `__init__()`.

```python
# Instanciando o primeiro objeto da classe Carro
meu_carro = Carro("Mercedes GLA200", "Chumbo")

# Instanciando um segundo objeto
carro_vizinho = Carro("Fusca", "Azul")

# Agora temos dois objetos distintos, cada um com seu próprio estado.
# Podemos acessar os atributos e chamar os métodos de cada um usando a notação de ponto.

# Verificando os atributos do primeiro objeto
print(f"Meu carro é um {meu_carro.modelo} na cor {meu_carro.cor}.")
# Saída: Meu carro é um Mercedes GLA200 na cor Chumbo.

# Verificando os atributos do segundo objeto
print(f"O carro do vizinho é um {carro_vizinho.modelo} na cor {carro_vizinho.cor}.")
# Saída: O carro do vizinho é um Fusca na cor Azul.

# Chamando o método acelerar para cada objeto
meu_carro.acelerar()       # Saída: O carro Mercedes GLA200 está acelerando.
carro_vizinho.acelerar() # Saída: O carro Fusca está acelerando.
```

Observe como, ao chamar `meu_carro.acelerar()`, o `self` dentro do método `acelerar` se refere ao objeto `meu_carro`, permitindo que ele acesse `self.modelo` para imprimir "Mercedes GLA200".

## Métodos Especiais para Iteração: `__iter__` e `__next__`

Além do construtor `__init__`, o Python possui uma série de outros métodos especiais que nos permitem emular o comportamento de tipos nativos. Dois desses métodos, `__iter__()` e `__next__()`, formam o **protocolo de iteração**, permitindo que nossos próprios objetos se tornem iteráveis, ou seja, que possam ser usados em um laço `for`.

- **`__iter__()`**: Este método é chamado quando um objeto precisa se tornar um iterador (por exemplo, no início de um laço `for`). Sua responsabilidade é retornar um **objeto iterador**, que na maioria das vezes é o próprio objeto (`self`), desde que ele também tenha um método `__next__()`.
- **`__next__()`**: Este método define como obter o próximo item da sequência. Ele é chamado repetidamente durante a iteração. Quando não houver mais itens para retornar, ele deve levantar uma exceção `StopIteration` para sinalizar o fim da iteração.

Vamos criar um exemplo de uma classe `Contador` que gera uma sequência de números, para ver esses métodos em ação.

```python
class Contador:
    def __init__(self, maximo):
        self.maximo = maximo
        self.atual = 0

    def __iter__(self):
        # Retorna o próprio objeto como o iterador
        return self

    def __next__(self):
        # Verifica se o limite foi atingido
        if self.atual < self.maximo:
            numero = self.atual
            self.atual += 1
            return numero # Retorna o próximo número
        else:
            # Sinaliza o fim da iteração
            raise StopIteration

# Agora podemos usar nossa classe Contador em um laço for
meu_contador = Contador(5)

for numero in meu_contador:
    print(numero)
```

A saída será:

```
0
1
2
3
4
```

Ao implementar o protocolo de iteração, tornamos nossos objetos compatíveis com uma das construções mais poderosas da linguagem, o laço `for`, integrando-os perfeitamente ao ecossistema do Python.


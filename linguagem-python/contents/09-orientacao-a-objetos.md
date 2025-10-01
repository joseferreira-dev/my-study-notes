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

Com certeza. Dando continuidade ao capítulo, vamos agora explorar o primeiro dos quatro pilares fundamentais da Programação Orientada a Objetos.

## Os 4 Pilares da POO

Com a compreensão do que são classes e objetos, estamos prontos para explorar os princípios que dão à POO seu poder e sua elegância. Estes são os conceitos de design que nos permitem construir software robusto, flexível e de fácil manutenção. Tradicionalmente, são quatro os pilares principais: **Encapsulamento**, **Herança**, **Polimorfismo** e **Abstração**.

### Encapsulamento: Protegendo a Integridade do Objeto

O **encapsulamento** é o pilar que trata de agrupar os dados (atributos) e os comportamentos (métodos) que os manipulam dentro de uma única unidade — o objeto —, ao mesmo tempo em que se **restringe o acesso direto** ao seu estado interno. A ideia é criar uma "cápsula" protetora ao redor dos dados, forçando o mundo exterior a interagir com o objeto apenas através de uma interface pública bem definida (seus métodos).

Pense em um caixa eletrônico. Ele possui dados internos complexos (o dinheiro no cofre, os registros de transações). Você, como usuário, não tem acesso direto a esses dados. Em vez disso, você interage com o caixa através de uma interface segura e controlada: a tela e os botões. Você pode solicitar um saque (chamar o método `sacar()`), e o caixa eletrônico executa a lógica interna para verificar seu saldo e liberar o dinheiro. O encapsulamento impede que você simplesmente "pegue" o dinheiro que quiser, garantindo a integridade do sistema.

Em Python, o encapsulamento não é imposto de forma rígida pela sintaxe da linguagem (como com as palavras-chave `public`, `private` e `protected` em Java ou C++), mas sim através de **convenções de nomenclatura**.

- **Atributos Públicos:** Por padrão, qualquer atributo ou método sem um prefixo especial é considerado público e pode ser acessado de qualquer lugar.
- **Atributos "Protegidos" (Convenção com `_`):** Um atributo cujo nome começa com um único sublinhado (ex: `_saldo`) é, por convenção, tratado como "protegido". Isso não impede o acesso, mas serve como um forte sinal para outros desenvolvedores de que aquele atributo é parte da implementação interna da classe e não deve ser modificado diretamente de fora.
- **Atributos "Privados" (Name Mangling com `__`):** Um atributo cujo nome começa com dois sublinhados (ex: `__saldo`) ativa um mecanismo do Python chamado _name mangling_ (desfiguração de nome). O interpretador altera o nome do atributo internamente para incluir o nome da classe (ex: `_Conta__saldo`), tornando muito mais difícil o seu acesso acidental de fora da classe. Embora ainda seja possível acessá-lo com o nome "desfigurado", essa é a forma mais forte que o Python oferece para indicar que um atributo é estritamente privado.

#### Encapsulamento na Prática

Vamos ilustrar a importância do encapsulamento com uma classe `ContaBancaria`.

**Versão NÃO encapsulada (Má Prática):**

```python
class ContaBancariaSemProtecao:
    def __init__(self, saldo_inicial):
        self.saldo = saldo_inicial # Atributo público

    def depositar(self, valor):
        if valor > 0:
            self.saldo += valor

# Criando um objeto
minha_conta = ContaBancariaSemProtecao(1000)
print(f"Saldo inicial: {minha_conta.saldo}") # Saída: Saldo inicial: 1000

# O problema: acesso direto permite corromper o estado do objeto
minha_conta.saldo = -5000  # Estado inválido! O objeto foi corrompido.
print(f"Saldo após acesso direto: {minha_conta.saldo}") # Saída: Saldo após acesso direto: -5000
```

Neste cenário, não há nada que impeça um código externo de atribuir um valor negativo ao `saldo`, quebrando uma regra de negócio fundamental e deixando o objeto em um estado inconsistente.

**Versão Encapsulada (Boa Prática):**

```python
class ContaBancaria:
    def __init__(self, saldo_inicial):
        # Usamos '__' para tornar o atributo privado
        self.__saldo = 0
        if saldo_inicial >= 0:
            self.__saldo = saldo_inicial

    # Método público para LER o saldo (Getter)
    def get_saldo(self):
        return self.__saldo

    # Métodos públicos para MODIFICAR o saldo de forma controlada
    def depositar(self, valor):
        if valor > 0:
            self.__saldo += valor
            print("Depósito realizado com sucesso.")
        else:
            print("Valor de depósito inválido.")

    def sacar(self, valor):
        if 0 < valor <= self.__saldo:
            self.__saldo -= valor
            print("Saque realizado com sucesso.")
            return True
        else:

            print("Saldo insuficiente ou valor de saque inválido.")
            return False

# Criando um objeto encapsulado
conta_segura = ContaBancaria(1000)

# print(conta_segura.__saldo) # Geraria um AttributeError, acesso direto bloqueado.

# A interação é feita através dos métodos públicos
print(f"Saldo atual: {conta_segura.get_saldo()}") # Saída: Saldo atual: 1000

conta_segura.sacar(500)
print(f"Saldo após saque: {conta_segura.get_saldo()}") # Saída: Saldo após saque: 500

conta_segura.sacar(2000) # Saída: Saldo insuficiente ou valor de saque inválido.
print(f"Saldo final: {conta_segura.get_saldo()}") # Saída: Saldo final: 500
```

Com a classe encapsulada, o atributo `__saldo` está protegido. A única maneira de interagir com ele é através dos métodos `get_saldo()`, `depositar()` e `sacar()`, que contêm a lógica de negócio e as validações necessárias para garantir que o objeto permaneça sempre em um estado consistente e válido. O encapsulamento, portanto, é a base para a criação de objetos confiáveis e robustos.


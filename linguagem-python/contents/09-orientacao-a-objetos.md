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

### Herança: Criando Hierarquias e Reutilizando Código

A **herança** é o pilar que permite que uma classe, chamada de **subclasse** (ou classe filha), adquira os atributos e métodos de outra classe, chamada de **superclasse** (ou classe pai). Este é um dos mecanismos mais poderosos da POO, pois está focado em criar relacionamentos e hierarquias, promovendo o reuso de código e a modelagem de conceitos do mundo real de forma intuitiva.

A ideia central por trás da herança é a relação **"é um"** (_is-a_). Uma classe `Carro` só deve herdar de `Veiculo` porque um carro **é um** tipo de veículo. Um `Gerente` só deve herdar de `Funcionario` porque um gerente **é um** tipo de funcionário. Ao estabelecer essa relação, a subclasse herda automaticamente toda a funcionalidade da superclasse, podendo então **estender** (adicionar novos atributos e métodos) ou **especializar** (modificar o comportamento de métodos herdados) essa funcionalidade.

Isso nos permite trabalhar com dois processos de design complementares:

1. **Generalização:** É o processo de identificar características comuns entre várias classes e abstraí-las para uma superclasse mais genérica. Se temos classes `Carro`, `Moto` e `Caminhao`, podemos observar que todas possuem `marca` e `modelo`, e todas podem `acelerar()`. A criação da classe `Veiculo` para conter esses membros comuns é um ato de generalização.
2. **Especialização:** É o processo inverso. Partimos de uma classe genérica e criamos subclasses mais específicas que, além de herdarem tudo da superclasse, adicionam seus próprios atributos e comportamentos exclusivos.

#### A Herança na Prática em Python

Em Python, a herança é implementada com uma sintaxe simples: na definição da classe filha, passamos a classe pai entre parênteses.

```python
class Subclasse(Superclasse):
```

Vamos expandir nosso exemplo anterior, criando uma hierarquia de veículos.

**A Superclasse (Generalização):**

```python
# Classe pai, ou superclasse
class Veiculo:
    def __init__(self, marca, modelo, ano):
        self.marca = marca
        self.modelo = modelo
        self.ano = ano

    def exibir_dados(self):
        print(f"Marca: {self.marca}, Modelo: {self.modelo}, Ano: {self.ano}")

    def ligar_motor(self):
        print("Motor do veículo ligado.")
```

**As Subclasses (Especialização):**

Agora, vamos criar classes Carro e Moto que herdam de Veiculo.

```python
# Classe filha, ou subclasse
class Carro(Veiculo):
    # O Carro terá todos os atributos e métodos de Veiculo
    pass # A palavra-chave 'pass' indica um bloco vazio

class Moto(Veiculo):
    # A Moto também herdará tudo de Veiculo
    pass

# Instanciando objetos das subclasses
meu_carro = Carro("Ford", "Ka", 2020)
minha_moto = Moto("Honda", "CB 500", 2022)

# Mesmo sem definir nada em Carro e Moto, elas já possuem a funcionalidade da classe pai
meu_carro.exibir_dados() # Saída: Marca: Ford, Modelo: Ka, Ano: 2020
minha_moto.ligar_motor() # Saída: Motor do veículo ligado.
```

#### Estendendo e Especializando com `super()` e Sobrescrita

O verdadeiro poder da herança se manifesta quando estendemos e especializamos as subclasses.

- **Estender:** Adicionar novos atributos e métodos que são específicos da subclasse.
- **Sobrescrever (Override):** Fornecer uma nova implementação para um método que já existe na superclasse.

Para isso, utilizamos a função `super()`. A função `super()` nos dá acesso aos métodos da classe pai, permitindo que a gente chame o construtor da superclasse para inicializar os atributos herdados antes de adicionar os novos.

```python
class Carro(Veiculo):
    def __init__(self, marca, modelo, ano, numero_portas):
        # 1. Chama o construtor da classe pai (Veiculo) para inicializar os atributos comuns
        super().__init__(marca, modelo, ano)
        
        # 2. Adiciona o atributo específico da classe Carro
        self.numero_portas = numero_portas

    # 3. Sobrescrevendo o método da classe pai para um comportamento mais específico
    def ligar_motor(self):
        print(f"O motor do {self.modelo} foi ligado.")

class Moto(Veiculo):
    def __init__(self, marca, modelo, ano, cilindradas):
        super().__init__(marca, modelo, ano)
        self.cilindradas = cilindradas

    def ligar_motor(self):
        print(f"A moto {self.modelo} deu a partida.")

# Instanciando os objetos especializados
meu_novo_carro = Carro("Toyota", "Corolla", 2023, 4)
minha_nova_moto = Moto("Yamaha", "MT-07", 2024, 700)

# O método exibir_dados foi herdado e funciona normalmente
meu_novo_carro.exibir_dados() # Saída: Marca: Toyota, Modelo: Corolla, Ano: 2023

# O método ligar_motor agora executa a versão específica de cada subclasse
meu_novo_carro.ligar_motor()   # Saída: O motor do Corolla foi ligado.
minha_nova_moto.ligar_motor() # Saída: A moto MT-07 deu a partida.
```

A herança, portanto, nos permite criar uma base de código comum e reutilizável na superclasse, enquanto as subclasses se encarregam de implementar os detalhes e as especializações, resultando em um código mais organizado, modular e fácil de estender.

### Polimorfismo: Muitas Formas, Uma Única Interface

A **herança** nos permite criar hierarquias, mas o benefício mais poderoso que surge dessa estrutura é o **polimorfismo**. A palavra vem do grego e significa "muitas formas" (_poly_ = muitas, _morphos_ = forma). Na Programação Orientada a Objetos, o polimorfismo é a capacidade de objetos de classes diferentes responderem à mesma mensagem (a mesma chamada de método) de maneiras específicas e apropriadas para cada um.

Em termos práticos, o polimorfismo nos permite tratar um objeto de uma subclasse como se fosse um objeto de sua superclasse. Isso nos capacita a escrever um código mais genérico e desacoplado, que pode operar sobre uma família de classes sem precisar conhecer os detalhes específicos de cada uma.

Pense em um sistema de áudio com um botão "play". Este botão (a **interface**) envia a mesma mensagem `play()` para diferentes dispositivos. Se o dispositivo for um toca-fitas, ele começará a girar a fita. Se for um CD player, ele começará a girar o disco e a ler com o laser. Se for um serviço de streaming, ele iniciará o buffer da música pela internet. A mensagem é a mesma, mas o comportamento executado é diferente para cada objeto.

#### Polimorfismo na Prática em Python

O polimorfismo em Python se manifesta de forma natural através da herança e da sobrescrita de métodos. Vamos utilizar nossa hierarquia de veículos para demonstrar.

```python
# Relembrando nossas classes
class Veiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo

    def ligar_motor(self):
        print("Motor do veículo ligado.")

class Carro(Veiculo):
    def ligar_motor(self): # Método sobrescrito
        print(f"O motor do carro {self.modelo} foi ligado.")

class Moto(Veiculo):
    def ligar_motor(self): # Método sobrescrito
        print(f"A moto {self.modelo} deu a partida.")

# Criando instâncias
meu_carro = Carro("Toyota", "Corolla")
minha_moto = Moto("Yamaha", "MT-07")

# --- A mágica do polimorfismo acontece aqui ---

# Criamos uma função que opera sobre a classe genérica 'Veiculo'
def iniciar_corrida(veiculo):
    print("\n--- Preparando para iniciar a corrida ---")
    print(f"Verificando o veículo: {veiculo.marca} {veiculo.modelo}")
    veiculo.ligar_motor() # A mesma chamada de método para qualquer veículo

# Agora, podemos passar objetos de diferentes subclasses para a mesma função
iniciar_corrida(meu_carro)
iniciar_corrida(minha_moto)
```

A saída será:

```
--- Preparando para iniciar a corrida ---
Verificando o veículo: Toyota Corolla
O motor do carro Corolla foi ligado.

--- Preparando para iniciar a corrida ---
Verificando o veículo: Yamaha MT-07
A moto MT-07 deu a partida.
```

A função `iniciar_corrida` é polimórfica. Ela não precisa saber se está lidando com um `Carro` ou uma `Moto`. Ela simplesmente espera receber um objeto que "é um" `Veiculo` e confia que esse objeto saberá como responder à chamada `ligar_motor()`. O Python, em tempo de execução, se encarrega de chamar a versão correta e especializada do método para cada objeto.

Essa capacidade de escrever funções genéricas que operam sobre uma abstração (a superclasse `Veiculo`) é o que torna o código flexível e extensível. Se amanhã criarmos uma classe `Caminhao(Veiculo)` com seu próprio método `ligar_motor()`, a função `iniciar_corrida` funcionará com ela instantaneamente, sem que uma única linha de código precise ser alterada.

#### Polimorfismo e "Duck Typing"

O Python leva o conceito de polimorfismo um passo adiante com uma filosofia conhecida como **"Duck Typing"** (Tipagem de Pato). A expressão vem do ditado: "Se anda como um pato, nada como um pato e faz quack como um pato, então eu chamo de pato".

Isso significa que, para o Python, o tipo real de um objeto é menos importante do que os métodos que ele implementa. Para que o polimorfismo funcione, as classes nem precisam herdar de uma superclasse comum. Contanto que diferentes objetos possuam um método com o mesmo nome, eles podem ser usados de forma intercambiável na mesma função.

Vamos ver um exemplo com animais que não possuem relação de herança:

```python
class Pato:
    def comunicar(self):
        print("Quack! Quack!")

class Pessoa:
    def comunicar(self):
        print("Olá, bom dia!")

# Função polimórfica que não depende de herança
def fazer_comunicar(ser):
    ser.comunicar()

# Criando instâncias de classes não relacionadas
pato = Pato()
pessoa = Pessoa()

# A mesma função funciona para ambos, pois ambos têm o método .comunicar()
fazer_comunicar(pato)   # Saída: Quack! Quack!
fazer_comunicar(pessoa) # Saída: Olá, bom dia!
```

Essa flexibilidade, onde o comportamento (os métodos que o objeto possui) é mais importante que a herança, é uma das características mais poderosas e distintivas do Python.

### Abstração: Focando no Essencial

O último pilar, a **abstração**, é o processo de identificar as características e comportamentos essenciais de um objeto, expondo apenas uma visão simplificada e de alto nível, enquanto se oculta a complexidade dos detalhes de implementação. É o ato de criar um modelo que foca no **"o que"** um objeto faz, em vez de no **"como"** ele faz.

Pense em usar um controle remoto de televisão. A interface que você usa é uma abstração. Ela oferece botões simples como `ligar()`, `aumentar_volume()` e `trocar_canal()`. Você não precisa conhecer a complexidade dos circuitos eletrônicos, da transmissão de sinais infravermelhos ou do processamento de vídeo para operar a TV. Toda essa complexidade interna está oculta, e apenas uma interface de alto nível é exposta. Isso é abstração.

Na programação, a abstração nos permite criar modelos de objetos que são mais fáceis de entender, usar e gerenciar. Ao projetar uma classe, decidimos quais atributos e métodos serão públicos (parte da interface do objeto) e quais serão detalhes de implementação internos (que devem ser encapsulados).

#### Abstração na Prática em Python: Classes Abstratas

Em Python, o principal mecanismo para implementar a abstração formalmente é através de **Classes Base Abstratas** (_Abstract Base Classes_ - ABCs), fornecidas pelo módulo `abc`. Uma classe abstrata é uma classe que não pode ser instanciada diretamente. Seu propósito é servir como um **contrato** ou um modelo para suas subclasses.

Uma classe abstrata pode conter **métodos abstratos**, que são métodos declarados, mas sem nenhuma implementação. Ao fazer isso, a classe abstrata estabelece uma regra: qualquer subclasse concreta que herdar dela será **obrigada** a fornecer uma implementação para aqueles métodos.

Vamos refatorar nossa hierarquia de veículos para usar a abstração formalmente.

```python
# Importamos as ferramentas necessárias do módulo abc
from abc import ABC, abstractmethod

# A classe Veiculo agora é uma Classe Base Abstrata
class Veiculo(ABC):
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo

    # Este é um método concreto, compartilhado por todas as subclasses
    def exibir_dados(self):
        print(f"Veículo: {self.marca} {self.modelo}")

    # Este é um método abstrato. Ele define o contrato.
    @abstractmethod
    def ligar_motor(self):
        pass # Métodos abstratos não têm implementação na classe pai

# As subclasses agora devem cumprir o contrato
class Carro(Veiculo):
    def ligar_motor(self): # Implementação obrigatória do método abstrato
        print(f"O motor do carro {self.modelo} foi ligado.")

class Moto(Veiculo):
    def ligar_motor(self): # Implementação obrigatória do método abstrato
        print(f"A moto {self.modelo} deu a partida.")

# Tentativa de instanciar a classe abstrata diretamente geraria um erro
# veiculo_generico = Veiculo("Genérico", "G1") # TypeError: Can't instantiate abstract class Veiculo with abstract method ligar_motor

# Instanciamos as classes concretas que cumprem o contrato
meu_carro = Carro("Toyota", "Corolla")
minha_moto = Moto("Yamaha", "MT-07")

# A abstração funciona em conjunto com o polimorfismo
def iniciar_veiculo(veiculo: Veiculo):
    veiculo.ligar_motor()

iniciar_veiculo(meu_carro) # Saída: O motor do carro Corolla foi ligado.
iniciar_veiculo(minha_moto) # Saída: A moto MT-07 deu a partida.
```

Neste novo design:

1. `Veiculo(ABC)` se torna um conceito abstrato, um contrato.
2. O decorador `@abstractmethod` marca `ligar_motor` como um método que _deve_ ser implementado por qualquer classe filha.
3. Tentar criar um objeto `Veiculo` diretamente agora causa um erro, pois ele é um modelo incompleto.
4. As classes `Carro` e `Moto` são concretas porque elas fornecem a implementação para o contrato `ligar_motor`.

A abstração, portanto, nos permite definir interfaces claras e garantir que todas as subclasses que pertencem a uma mesma família de objetos compartilhem um conjunto mínimo de comportamentos, tornando o sistema como um todo mais previsível e robusto. Ela é a base para o design de frameworks e bibliotecas, onde se define uma estrutura geral e se deixa para o usuário final a tarefa de implementar os detalhes específicos.


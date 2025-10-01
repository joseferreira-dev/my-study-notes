# Capítulo 7 – Funções

Até agora, escrevemos scripts que executam uma sequência de comandos, utilizando estruturas de controle para guiar seu fluxo. À medida que nossos programas crescem, no entanto, a repetição de lógicas e a complexidade crescente podem tornar o código difícil de ler e manter. Para resolver este desafio e elevar nosso nível de organização, introduzimos um dos conceitos mais poderosos da programação: as **funções**.

Uma função é um bloco de código nomeado, projetado para realizar uma tarefa específica. Pense nelas como subprogramas ou "receitas" que podemos definir uma vez e executar quantas vezes quisermos, em diferentes partes do nosso código. Elas são o pilar da modularização e do princípio **DRY (_Don't Repeat Yourself_ - Não se Repita)**, permitindo-nos encapsular uma lógica complexa em uma única chamada, tornando nosso código mais limpo, mais legível e muito mais fácil de depurar e reutilizar. Neste capítulo, vamos dissecar a anatomia das funções em Python, desde sua definição e uso básico até conceitos mais avançados como argumentos flexíveis, funções anônimas (lambda) e o crucial entendimento sobre o escopo de variáveis.

## Definindo e Chamando Funções

Em Python, a criação de uma função é iniciada com a palavra-chave `def`. A estrutura é projetada para ser clara e legível, seguindo a filosofia da linguagem.

A sintaxe geral é:

```python
def nome_da_funcao(parametro1, parametro2, ...):
    # Bloco de código (corpo da função)
    # ... lógica da função ...
    return resultado
```

Vamos analisar cada parte:

- **`def`**: A palavra-chave que sinaliza o início da definição de uma função.
- **`nome_da_funcao`**: O nome que damos à nossa função. Ele deve seguir as mesmas regras de nomenclatura de variáveis (estilo `snake_case` é a convenção).
- **`parametro1, parametro2, ...`**: Os **parâmetros** são as variáveis que a função recebe como entrada. Eles são opcionais e servem como placeholders para os dados que a função irá processar.
- **`:`**: Os dois-pontos marcam o final do cabeçalho da função e o início do seu corpo.
- **Corpo da função**: O bloco de código indentado que contém as instruções a serem executadas quando a função é chamada.
- **`return`**: A palavra-chave (também opcional) que especifica o valor que a função "devolve" como resultado para o local onde foi chamada. Se nenhuma instrução `return` for fornecida, a função retorna `None` por padrão.

Vamos criar uma função simples que recebe um nome e retorna uma saudação:

```python
# Definição da função
def criar_saudacao(nome):
    """Gera uma string de saudação para um nome específico."""
    mensagem = f"Olá, {nome}! Bem-vindo(a) ao Python."
    return mensagem

# Chamando a função e armazenando o resultado
saudacao_alice = criar_saudacao("Alice")
saudacao_bob = criar_saudacao("Bob")

# Imprimindo os resultados
print(saudacao_alice) # Saída: Olá, Alice! Bem-vindo(a) ao Python.
print(saudacao_bob)   # Saída: Olá, Bob! Bem-vindo(a) ao Python.
```

Uma vez definida, a função `criar_saudacao` pode ser **chamada** (invocada) quantas vezes quisermos, com diferentes valores de entrada (chamados de **argumentos**).

### Parâmetros vs. Argumentos

É importante distinguir os termos:

- **Parâmetro:** É a variável na definição da função (ex: `nome` em `def criar_saudacao(nome):`).
- **Argumento:** É o valor real que é passado para a função quando ela é chamada (ex: `"Alice"` em `criar_saudacao("Alice")`).

### Tipos de Argumentos

Python oferece várias maneiras flexíveis de passar argumentos para uma função.

- **Argumentos Posicionais:** São passados na ordem em que os parâmetros foram definidos.
    
    ```python
    def descrever_pet(tipo_animal, nome_animal):
        print(f"Eu tenho um {tipo_animal} chamado {nome_animal}.")
    
    descrever_pet("cachorro", "Rex") # "cachorro" vai para tipo_animal, "Rex" para nome_animal
    ```
    
- **Argumentos de Palavra-Chave (Keyword Arguments):** Permitem passar os argumentos especificando o nome do parâmetro, o que torna a ordem irrelevante e o código mais legível.
    
    ```python
    descrever_pet(nome_animal="Totó", tipo_animal="gato")
    ```
    
- **Parâmetros com Valores Padrão:** Podemos definir um valor padrão para um parâmetro, tornando-o opcional na chamada da função.
    
    ```python
    def calcular_potencia(base, expoente=2): # expoente tem o valor padrão 2
        return base ** expoente
    
    print(calcular_potencia(5))       # Usa o expoente padrão 2. Saída: 25
    print(calcular_potencia(5, 3))    # Fornece um novo valor para o expoente. Saída: 125
    ```

### Argumentos Arbitrários: `*args` e `**kwargs`

E se não soubermos quantos argumentos uma função precisará receber? Python resolve isso com duas sintaxes especiais:

- **`*args`**: Permite que uma função aceite um número arbitrário de argumentos **posicionais**. Eles são agrupados em uma **tupla**.
    
    ```python
    def somar_tudo(*numeros):
        print(f"Recebi os números: {numeros}")
        total = 0
        for numero in numeros:
            total += numero
        return total
    
    print(somar_tudo(1, 2, 3))       # Saída: Recebi os números: (1, 2, 3) -> 6
    print(somar_tudo(10, 20, 30, 40)) # Saída: Recebi os números: (10, 20, 30, 40) -> 100
    ```
    
- **`**kwargs`**: Permite que uma função aceite um número arbitrário de argumentos de **palavra-chave**. Eles são agrupados em um **dicionário**.
    
    ```python
    def exibir_info_usuario(**dados):
        print(f"Recebi os dados: {dados}")
        for chave, valor in dados.items():
            print(f"- {chave.title()}: {valor}")
    
    exibir_info_usuario(nome="Carla", idade=28, cidade="Recife")
    # Saída: Recebi os dados: {'nome': 'Carla', 'idade': 28, 'cidade': 'Recife'}
    # - Nome: Carla
    # - Idade: 28
    # - Cidade: Recife
    ```

## Funções Lambda

Além das funções tradicionais definidas com `def`, Python oferece uma sintaxe para criar pequenas funções anônimas em uma única linha: as **funções lambda**. Elas são chamadas de "anônimas" porque não possuem um nome formal. São ideais para situações onde precisamos de uma função simples por um curto período, geralmente como argumento para outra função.

A sintaxe geral é:

```python
lambda argumentos: expressao
```

Uma função lambda pode ter múltiplos argumentos, mas apenas **uma única expressão**. O resultado dessa expressão é o que a função retorna.

Vamos comparar uma função `def` tradicional com sua versão `lambda` equivalente.

**Função Tradicional:**

```python
def dobrar(x):
    return x * 2

print(dobrar(5)) # Saída: 10
```

**Função Lambda:**

```python
dobrar_lambda = lambda x: x * 2

print(dobrar_lambda(5)) # Saída: 10
```

Embora seja possível atribuir uma lambda a uma variável, como fizemos acima, seu verdadeiro poder se manifesta quando são usadas "embutidas", como argumento para funções de ordem superior como `sorted()`, `map()` e `filter()`.

Imagine que temos uma lista de dicionários e queremos ordená-la pela idade de cada pessoa:

```python
pessoas = [
    {'nome': 'Carlos', 'idade': 42},
    {'nome': 'Ana', 'idade': 23},
    {'nome': 'Beatriz', 'idade': 31}
]

# Usando uma lambda para especificar a chave de ordenação
pessoas_ordenadas = sorted(pessoas, key=lambda pessoa: pessoa['idade'])

print(pessoas_ordenadas)
# Saída: [{'nome': 'Ana', 'idade': 23}, {'nome': 'Beatriz', 'idade': 31}, {'nome': 'Carlos', 'idade': 42}]
```

A função `sorted` recebe a `lambda` como a `key`, e a utiliza para extrair o valor da idade de cada dicionário, realizando a ordenação com base nesse valor, tudo de forma concisa e elegante.

## Escopo de Variáveis

O **escopo** de uma variável define a região do código onde ela pode ser acessada. Em Python, o escopo é determinado por onde a variável é definida.

- **Escopo Local:** Variáveis criadas **dentro** de uma função têm escopo local. Elas só existem e podem ser acessadas de dentro daquela função.
    
    ```python
    def minha_funcao():
        variavel_local = "Eu vivo aqui dentro!"
        print(variavel_local)
    
    minha_funcao()
    # print(variavel_local) # Geraria um NameError, pois a variável não existe aqui fora.
    ```
    
- **Escopo Global:** Variáveis criadas **fora** de qualquer função, no nível principal do script, têm escopo global. Elas podem ser acessadas de qualquer lugar do código, incluindo de dentro das funções.
    
    ```python
    variavel_global = "Eu sou acessível em todo lugar."
    
    def outra_funcao():
        print(f"De dentro da função: {variavel_global}")
    
    outra_funcao()
    print(f"De fora da função: {variavel_global}")
    ```

É uma boa prática minimizar o uso de variáveis globais, pois elas podem tornar o código mais difícil de depurar, já que seu valor pode ser modificado por diferentes partes do programa. O ideal é que as funções recebam os dados que precisam através de parâmetros e retornem os resultados que produzem.

## Considerações Finais

Neste capítulo, desvendamos o conceito de **funções**, o principal mecanismo para a criação de código modular, reutilizável e organizado em Python. Vimos como a palavra-chave `def` nos permite encapsular blocos de lógica em unidades nomeadas, que podem ser chamadas repetidamente para executar tarefas específicas.

Exploramos a anatomia de uma função, distinguindo **parâmetros** (as entradas na definição) de **argumentos** (os valores na chamada) e analisamos a flexibilidade que o Python oferece para passá-los, desde os argumentos posicionais e de palavra-chave até a capacidade de aceitar um número arbitrário de entradas com `*args` e `**kwargs`.

Introduzimos as **funções lambda** como uma ferramenta concisa para criar pequenas funções anônimas, ideais para operações rápidas e como argumentos para outras funções. Por fim, estabelecemos a importância do **escopo**, compreendendo a diferença fundamental entre variáveis locais, que vivem e morrem dentro de uma função, e variáveis globais, acessíveis em todo o programa.

Dominar as funções é dar um passo fundamental para se tornar um programador proficiente. Elas nos permitem construir abstrações, esconder complexidades e pensar sobre nossos programas em um nível mais alto de organização. Com essa base sólida, estamos agora preparados para explorar como podemos agrupar funções e dados relacionados em arquivos separados, o que nos introduzirá ao poderoso sistema de **módulos e pacotes** do Python.
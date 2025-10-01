# Capítulo 8 – Funções Embutidas

Nos capítulos anteriores, focamos em como definir nossas próprias funções para organizar e reutilizar código. No entanto, uma das grandes vantagens do Python é sua vasta "biblioteca padrão", que inclui um rico conjunto de **funções embutidas** (_built-in functions_). Essas funções estão sempre disponíveis, em qualquer script Python, sem a necessidade de realizar qualquer importação. Elas formam um kit de ferramentas essencial, projetado para realizar as tarefas mais comuns da programação de forma eficiente e concisa.

Essas funções são implementadas em um nível mais baixo da linguagem (geralmente em C), o que as torna altamente otimizadas e performáticas. Conhecê-las e saber quando utilizá-las é um passo fundamental para escrever um código mais limpo, mais legível e mais "Pythônico". Neste capítulo, faremos um tour por uma seleção das funções embutidas mais úteis e frequentemente utilizadas, agrupando-as por sua finalidade para facilitar o entendimento e a memorização.

## Funções Simples: Um Arsenal de Ferramentas Essenciais

Em vez de analisar cada função isoladamente, vamos agrupá-las em categorias lógicas: funções para conversão de tipos, para análise de sequências, para avaliação lógica e para interação com o ambiente. Algumas delas, como `len()` e `print()`, já apareceram em capítulos anteriores; agora, vamos formalizar seu conhecimento e expandir suas funcionalidades.

### Funções de Conversão e Representação de Tipos

Este grupo de funções é utilizado para converter dados de um tipo para outro ou para criar instâncias de tipos de coleção.

- **`int(x, base=10)`**: Converte um número ou uma string `x` para um número inteiro. Opcionalmente, pode-se especificar a `base` numérica da string de origem.
    
    ```python
    print(int(3.9))      # Saída: 3 (trunca a parte decimal)
    print(int("1010", base=2)) # Converte o binário "1010" para inteiro. Saída: 10
    ```
    
- **`float(x)`**: Converte um número ou uma string `x` para um número de ponto flutuante.
    
    ```python
    print(float("123.45")) # Saída: 123.45
    ```
    
- **`str(obj)`**: Retorna a representação em string do objeto `obj`.
    
    ```python
    print("O ano é " + str(2025)) # Saída: O ano é 2025
    ```
    
- **`bool(x)`**: Converte um valor `x` para um booleano (`True` ou `False`), seguindo as regras de "truthy" e "falsy" do Python (`0`, `""`, `[]`, `None`, etc. são `False`).
    
    ```python
    print(bool(1))       # Saída: True
    print(bool([]))      # Saída: False (lista vazia é Falsy)
    print(bool("Texto")) # Saída: True (string não vazia é Truthy)
    ```
    
- **`list(iteravel)`, `tuple(iteravel)`, `set(iteravel)`**: Criam uma lista, tupla ou conjunto, respectivamente, a partir de um objeto iterável.
    
    ```python
    print(list("abc"))    # Saída: ['a', 'b', 'c']
    print(tuple([1, 2, 3])) # Saída: (1, 2, 3)
    ```
    
- **`dict(**kwargs)`**: Cria um dicionário a partir de argumentos de palavra-chave ou de outros mapeamentos.
    
    ```python
    print(dict(nome="Alice", idade=30)) # Saída: {'nome': 'Alice', 'idade': 30}
    ```
    
- **`type(obj)`**: Retorna o tipo do objeto `obj`, útil para depuração e verificação.
    
    ```python
    print(type(123))   # Saída: <class 'int'>
    print(type("abc")) # Saída: <class 'str'>
    ```
    
- **`format(valor, formato)`**: Formata um `valor` de acordo com uma especificação de `formato`, similar à minilinguagem de formatação que vimos no capítulo de strings.
    
    ```python
    print(format(1234.5678, ".2f")) # Saída: '1234.57'
    ```

### Funções para Análise e Manipulação de Sequências

Este grupo opera sobre coleções e sequências para extrair informações ou retornar novas sequências.

- **`len(s)`**: Retorna o número de itens em um objeto `s` (seu "comprimento").
    
    ```python
    print(len([1, 2, 3, 4])) # Saída: 4
    print(len("Python"))       # Saída: 6
    ```
    
- **`max(iteravel)`, `min(iteravel)`, `sum(iteravel)`**: Retornam, respectivamente, o maior item, o menor item e a soma de todos os itens de um iterável numérico.
    
    ```python
    numeros = [10, 5, 25, 15]
    print(max(numeros)) # Saída: 25
    print(min(numeros)) # Saída: 5
    print(sum(numeros)) # Saída: 55
    ```
    
- **`sorted(iteravel, key=None, reverse=False)`**: Retorna uma **nova lista** contendo todos os itens do `iteravel` em ordem crescente. É importante não confundir com o método `.sort()` das listas, que ordena a lista original (_in-place_).
    
    ```python
    numeros = [3, 1, 4, 1, 5, 9, 2]
    numeros_ordenados = sorted(numeros)
    print(f"Original: {numeros}")           # Saída: Original: [3, 1, 4, 1, 5, 9, 2]
    print(f"Ordenada: {numeros_ordenados}") # Saída: Ordenada: [1, 1, 2, 3, 4, 5, 9]
    
    # Ordenando em ordem decrescente
    print(sorted(numeros, reverse=True)) # Saída: [9, 5, 4, 3, 2, 1, 1]
    ```
    
- **`reversed(seq)`**: Retorna um **iterador** que produz os itens de uma sequência `seq` na ordem inversa. Para visualizar o resultado, é comum convertê-lo para uma lista.
    
    ```python
    seq = [1, 2, 3, 4]
    iterador_reverso = reversed(seq)
    print(list(iterador_reverso)) # Saída: [4, 3, 2, 1]
    ```
    
- **`round(numero, ndigits=None)`**: Arredonda um `numero` para `ndigits` casas decimais. Se `ndigits` for omitido, arredonda para o inteiro mais próximo. O Python 3 usa a estratégia de arredondamento "round half to even" (arredondar para o par mais próximo) para casos ambíguos (como 2.5).
    
    ```python
    print(round(5.678, 2)) # Saída: 5.68
    print(round(2.5))      # Saída: 2 (arredonda para o par mais próximo)
    print(round(3.5))      # Saída: 4 (arredonda para o par mais próximo)
    ```

### Funções para Avaliação Lógica de Iteráveis

Essas funções são atalhos úteis para verificar o conteúdo booleano de uma coleção.

- **`all(iteravel)`**: Retorna `True` se **todos** os elementos do `iteravel` forem "truthy". Retorna `True` também para um iterável vazio.
    
    ```python
    print(all([True, 1, "texto"])) # Saída: True
    print(all([True, 0, "texto"])) # Saída: False (porque 0 é "falsy")
    ```
    
- **`any(iteravel)`**: Retorna `True` se **pelo menos um** dos elementos do `iteravel` for "truthy". Retorna `False` para um iterável vazio.
    
    ```python
    print(any([False, 0, ""]))    # Saída: False (todos são "falsy")
    print(any([False, 0, "ok"])) # Saída: True (porque "ok" é "truthy")
    ```

### Funções de Interação, Geração e Introspecção

Este grupo final inclui funções para interagir com o usuário, gerar sequências e inspecionar objetos.

- **`print(*objetos, sep=' ', end='\n')`**: Exibe os `objetos` na saída padrão (geralmente o terminal). Possui parâmetros úteis como `sep` (o separador entre os objetos) e `end` (o que é impresso no final).
    
    ```python
    print("maçã", "banana", "laranja", sep=", ") # Saída: maçã, banana, laranja
    print("Primeira linha", end=" -> ")
    print("Continuação na mesma linha") # Saída: Primeira linha -> Continuação na mesma linha
    ```
    
- **`range(inicio, fim, passo)`**: Gera um objeto que representa uma sequência imutável de números, ideal para ser usado em laços `for`.
    
    ```python
    print(list(range(1, 5)))     # Saída: [1, 2, 3, 4]
    print(list(range(0, 10, 3))) # Saída: [0, 3, 6, 9]
    ```
    
- **`dir(obj)`**: Sem argumentos, lista os nomes no escopo local. Com um objeto `obj` como argumento, tenta retornar uma lista de seus atributos e métodos válidos. É uma ferramenta poderosa para exploração e depuração interativa.
    
    ```python
    minha_lista = [1, 2, 3]
    # print(dir(minha_lista)) 
    # Saída: ['__add__', '__class__', ..., 'append', 'clear', 'copy', 'count', ...]
    # Mostra todos os métodos que podemos chamar em uma lista.
    ```

## Funções de Ordem Superior: `map` e `filter`

Além das funções que operam diretamente sobre dados, o Python possui um conjunto de **funções de ordem superior** (_higher-order functions_). Este é um conceito originário da programação funcional, onde as funções são tratadas como cidadãs de primeira classe, o que significa que elas podem ser passadas como argumentos para outras funções. As funções `map()` e `filter()` são os exemplos mais proeminentes dessa categoria, permitindo aplicar transformações e filtros a coleções de dados de uma maneira declarativa e eficiente.

### Transformando Sequências com `map()`

A função `map()` é uma ferramenta poderosa para aplicar uma mesma função a **todos os elementos** de um objeto iterável (como uma lista ou tupla), sem a necessidade de escrever um laço `for` explícito. Ela "mapeia" cada elemento de entrada para um novo elemento de saída.

A sintaxe geral é:

```python
map(funcao_a_aplicar, iteravel)
```

Um ponto fundamental sobre o `map()` é que ele não retorna uma lista, mas sim um **iterador**. Isso significa que ele opera de forma "preguiçosa" (_lazy evaluation_): os resultados são calculados sob demanda, à medida que são necessários. Essa abordagem é muito eficiente em termos de memória, especialmente ao lidar com sequências de dados muito grandes, pois não é preciso alocar uma nova lista inteira na memória de uma só vez. Para visualizar todos os resultados, é comum converter o iterador retornado em uma lista usando a função `list()`.

Vamos a um exemplo prático. Suponha que temos uma lista de números e queremos obter uma nova lista com o quadrado de cada um.

**1. Usando uma Função Definida:**

```python
def calcular_quadrado(x):
    """Retorna o quadrado de um número."""
    return x ** 2

numeros = [1, 2, 3, 4, 5]

# A função map() aplica 'calcular_quadrado' a cada item da lista 'numeros'
numeros_ao_quadrado_iterador = map(calcular_quadrado, numeros)

# Convertendo o iterador para uma lista para ver os resultados
resultado_final = list(numeros_ao_quadrado_iterador)
print(resultado_final) # Saída: [1, 4, 9, 16, 25]
```

**2. Usando uma Função Lambda:**

Para operações simples como esta, é muito comum e mais conciso usar uma função lambda, evitando a necessidade de definir uma função separada.

```python
numeros = [1, 2, 3, 4, 5]

# A mesma operação, agora com uma lambda
numeros_ao_quadrado_iterador = map(lambda x: x ** 2, numeros)

print(list(numeros_ao_quadrado_iterador)) # Saída: [1, 4, 9, 16, 25]
```

A função `map()` tem um efeito muito similar ao das _list comprehensions_, que vimos em um capítulo anterior. A escolha entre um e outro é, muitas vezes, uma questão de estilo e legibilidade. Para transformações simples, as _list comprehensions_ são frequentemente consideradas mais "Pythônicas".

```python
# A mesma operação com list comprehension (resultado idêntico, sintaxe diferente)
numeros_ao_quadrado_comp = [x ** 2 for x in numeros]
print(numeros_ao_quadrado_comp) # Saída: [1, 4, 9, 16, 25]
```

### Selecionando Elementos com `filter()`

Enquanto `map()` transforma cada elemento, a função `filter()` serve para **selecionar** elementos de um iterável. Ela aplica uma função de teste a cada elemento e retorna um iterador contendo apenas aqueles para os quais a função retornou `True`.

A sintaxe geral é:

```python
filter(funcao_de_teste, iteravel)
```

A `funcao_de_teste` (também chamada de predicado) deve ser uma função que recebe um elemento e retorna um valor booleano (`True` ou `False`). Se o retorno for `True`, o elemento é incluído no resultado; se for `False`, ele é descartado. Assim como o `map()`, o `filter()` também retorna um iterador "preguiçoso".

Vamos a um exemplo onde queremos filtrar uma lista de números, mantendo apenas os que são pares.

```python
lista_numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# A função lambda 'lambda x: x % 2 == 0' retorna True para números pares e False para ímpares
numeros_pares_iterador = filter(lambda x: x % 2 == 0, lista_numeros)

# Convertendo o iterador para uma lista
resultado_final = list(numeros_pares_iterador)
print(resultado_final) # Saída: [0, 2, 4, 6, 8]
```

A função `filter()` percorreu a `lista_numeros`, e para cada número `x`, avaliou `x % 2 == 0`. Apenas os números para os quais essa expressão foi verdadeira foram incluídos no iterador final.

Novamente, a mesma operação pode ser realizada com uma _list comprehension_ com uma cláusula `if`, que muitos desenvolvedores acham mais legível para casos simples.

```python
# A mesma operação com list comprehension
numeros_pares_comp = [x for x in lista_numeros if x % 2 == 0]
print(numeros_pares_comp) # Saída: [0, 2, 4, 6, 8]
```

## Ordenando Coleções: `sorted()` vs. `.sort()`

A capacidade de ordenar os elementos de uma coleção — seja em ordem numérica, alfabética ou segundo um critério customizado — é uma necessidade fundamental em programação. O Python nos oferece duas maneiras principais de realizar essa tarefa, uma função embutida (`sorted()`) e um método de lista (`.sort()`). Embora ambas realizem a ordenação, seu comportamento e aplicação são distintos, e a escolha entre elas depende do resultado desejado.

### `sorted()`: Criando uma Nova Lista Ordenada

A função embutida `sorted()` é a ferramenta de ordenação mais flexível do Python. Ela pode receber **qualquer objeto iterável** (como uma lista, tupla, conjunto ou string) e **retorna uma nova lista** contendo todos os elementos do original, devidamente ordenados. A principal característica é que a coleção original permanece **intacta**.

A sintaxe geral é:

```python
sorted(iteravel, key=None, reverse=False)
```

- **`iteravel`**: A coleção de dados que será ordenada.
- **`key`**: Parâmetro opcional que especifica uma função a ser aplicada a cada elemento antes da comparação. A ordenação será baseada nos resultados dessa função. Por exemplo, `key=len` ordenará strings pelo seu comprimento.
- **`reverse`**: Parâmetro opcional que, se definido como `True`, ordena a lista em ordem decrescente. O padrão é `False` (ordem crescente).

Vamos a um exemplo com uma lista de números:

```python
numeros = [5, 2, 9, 1, 5, 6]

# Ordenação crescente (padrão)
lista_ordenada = sorted(numeros)
print(f"Lista Original: {numeros}")          # Saída: Lista Original: [5, 2, 9, 1, 5, 6]
print(f"Nova Lista Ordenada: {lista_ordenada}") # Saída: Nova Lista Ordenada: [1, 2, 5, 5, 6, 9]

# Ordenação decrescente
lista_inversa = sorted(numeros, reverse=True)
print(f"Nova Lista Inversa: {lista_inversa}") # Saída: Nova Lista Inversa: [9, 6, 5, 5, 2, 1]
```

O poder do parâmetro `key` se torna evidente ao ordenar coleções mais complexas, como uma lista de strings, com base em um critério que não seja a ordem alfabética.

```python
palavras = ['maçã', 'banana', 'abacaxi', 'uva']

# Ordenando pelo comprimento de cada palavra
ordenada_por_tamanho = sorted(palavras, key=len)
print(ordenada_por_tamanho) # Saída: ['uva', 'maçã', 'banana', 'abacaxi']
```

### `.sort()`: Ordenando uma Lista _In-Place_

Diferentemente de `sorted()`, o `.sort()` **não é uma função embutida**, mas sim um **método que pertence exclusivamente a objetos do tipo lista**. Sua principal característica é que ele realiza a ordenação **"in-place"**, ou seja, ele **modifica a lista original** diretamente e **não retorna nenhum valor** (tecnicamente, retorna `None`).

Por não criar uma nova lista, o método `.sort()` é ligeiramente mais eficiente em termos de uso de memória. Ele aceita os mesmos parâmetros opcionais `key` e `reverse`.

```python
numeros = [5, 2, 9, 1, 5, 6]
print(f"Lista antes do .sort(): {numeros}") # Saída: Lista antes do .sort(): [5, 2, 9, 1, 5, 6]

# Aplicando o método .sort()
numeros.sort()

print(f"Lista depois do .sort(): {numeros}") # Saída: Lista depois do .sort(): [1, 2, 5, 5, 6, 9]

# Aplicando a ordenação decrescente na mesma lista
numeros.sort(reverse=True)
print(f"Lista após .sort(reverse=True): {numeros}") # Saída: Lista após .sort(reverse=True): [9, 6, 5, 5, 2, 1]
```

Um erro comum entre iniciantes é tentar atribuir o resultado de `.sort()` a uma nova variável, o que resultará em `None`.

```python
numeros = [3, 1, 2]
lista_errada = numeros.sort()
print(lista_errada) # Saída: None
```

### `sorted()` ou `.sort()`: Qual Usar?

A escolha entre os dois métodos depende do seu objetivo:

|Característica|`sorted()`|`.sort()`|
|---|---|---|
|**Tipo**|Função Embutida|Método de Lista|
|**Aplicabilidade**|Qualquer iterável (lista, tupla, set, etc.)|Apenas em listas|
|**Retorno**|Retorna uma **nova lista** ordenada.|Retorna `None`.|
|**Efeito Colateral**|**Não modifica** o objeto original.|**Modifica** a lista original (_in-place_).|

A regra geral é:

- Use `sorted()` quando precisar manter a coleção original intacta ou quando estiver trabalhando com um iterável que não seja uma lista (como uma tupla).
- Use `.sort()` quando tiver certeza de que não precisa mais da ordem original da lista e desejar realizar a ordenação com a máxima eficiência de memória.

## Invertendo a Ordem de Sequências

Diferentemente da ordenação, que rearranja os elementos com base em seu valor (numérico ou alfabético), a **inversão** simplesmente espelha a ordem de uma sequência, fazendo com que o último elemento se torne o primeiro, o penúltimo se torne o segundo, e assim por diante. É uma operação puramente posicional. O Python nos oferece três maneiras distintas de realizar essa tarefa, cada uma com suas próprias características e casos de uso.

### Fatiamento com Passo Negativo

Já exploramos o fatiamento (`slicing`) com a sintaxe `[inicio:fim]`. No entanto, a sintaxe completa de fatiamento aceita um terceiro parâmetro, o **passo** (_step_): `[inicio:fim:passo]`. O passo determina de quantos em quantos elementos a fatia será construída.

```python
lista = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Omitindo o início e o fim (pega a lista inteira) e usando um passo de 2
# Pega o elemento no índice 0, pula 1, pega o do índice 2, pula 1, e assim por diante.
elementos_pares = lista[::2]
print(elementos_pares) # Saída: [0, 2, 4, 6, 8]
```

A grande sacada é que o passo pode ser **negativo**. Um passo de `-1` instrui o Python a percorrer a lista de trás para frente, do último ao primeiro elemento. Ao omitir o início e o fim, aplicamos essa inversão à lista inteira. A sintaxe `[::-1]` tornou-se uma expressão idiomática em Python para inverter qualquer sequência.

Esta operação sempre **cria uma nova lista** (ou string, ou tupla) invertida, sem alterar a original.

```python
lista_original = [10, 20, 30, 40, 50]

# Usando fatiamento com passo -1 para criar uma cópia invertida
lista_invertida = lista_original[::-1]

print(f"Lista Original: {lista_original}")     # Saída: Lista Original: [10, 20, 30, 40, 50]
print(f"Lista Invertida: {lista_invertida}") # Saída: Lista Invertida: [50, 40, 30, 20, 10]
```

Lembre-se da sintaxe completa do fatiamento: `[a:b:c]`

- `a` = índice de início (inclusivo)
- `b` = índice de fim (exclusivo)
- `c` = tamanho e direção do passo

### Função `reversed()`

A função embutida `reversed()` é outra forma de percorrer uma sequência na ordem inversa. Assim como `map()` e `filter()`, `reversed()` **retorna um iterador** "preguiçoso", que gera os elementos um a um, sob demanda, de trás para frente.

Essa abordagem é extremamente eficiente em termos de memória, pois não cria uma nova lista completa de imediato. A coleção original também permanece **intacta**. Para obter uma nova lista com a ordem invertida, basta converter o iterador resultante com a função `list()`.

```python
minha_tupla = (1, 2, 3, 4, 5)

# A função reversed() retorna um iterador
iterador_reverso = reversed(minha_tupla)

# Convertendo o iterador para uma lista para materializar a inversão
lista_invertida = list(iterador_reverso)

print(f"Tupla Original: {minha_tupla}")         # Saída: Tupla Original: (1, 2, 3, 4, 5)
print(f"Lista Invertida: {lista_invertida}") # Saída: Lista Invertida: [5, 4, 3, 2, 1]
```

Por retornar um iterador, `reversed()` é a escolha ideal quando você precisa apenas percorrer uma sequência de trás para frente dentro de um laço `for`, sem a necessidade de criar uma nova lista em memória.

```python
for i in reversed(range(5)): # Itera de 4 a 0
    print(i, end=" ") # Saída: 4 3 2 1 0
```

### Método `list.reverse()`

Seguindo o mesmo padrão de `sorted()` vs. `.sort()`, o Python oferece o método `.reverse()`, que é exclusivo de objetos do tipo **lista**. Este método realiza a inversão **"in-place"**, ou seja, ele **modifica a lista original** e, assim como `.sort()`, **retorna `None`**.

Esta é a opção mais eficiente em termos de memória se o seu objetivo é, de fato, alterar a ordem da lista existente, sem a necessidade de preservar a ordem original.

```python
numeros = [1, 2, 3, 4, 5]
print(f"Lista antes do .reverse(): {numeros}") # Saída: Lista antes do .reverse(): [1, 2, 3, 4, 5]

# Aplicando o método .reverse()
resultado = numeros.reverse()

print(f"Lista depois do .reverse(): {numeros}") # Saída: Lista depois do .reverse(): [5, 4, 3, 2, 1]
print(f"O método .reverse() retornou: {resultado}") # Saída: O método .reverse() retornou: None
```

A escolha entre os três métodos depende diretamente da sua necessidade:

- Precisa de uma **nova cópia** invertida e quer uma sintaxe concisa? Use o fatiamento `[::-1]`.
- Precisa **percorrer** a sequência de trás para frente de forma eficiente, sem alocar uma nova lista? Use a função `reversed()`.
- Precisa **modificar a lista original** permanentemente, economizando memória? Use o método `.reverse()`.

## Considerações Finais

Neste capítulo, abrimos a "caixa de ferramentas" do Python para explorar o seu vasto e poderoso conjunto de **funções embutidas**. Compreendemos que, para uma infinidade de tarefas comuns e essenciais, não precisamos reinventar a lógica; a linguagem já nos fornece soluções prontas, eficientes e sempre disponíveis, que formam a base para a escrita de um código robusto e conciso.

Percorremos um arsenal de funções para as mais diversas finalidades. Vimos como realizar a **conversão e inspeção de tipos** com `int()`, `list()` e `type()`, e como extrair informações valiosas de nossas coleções com `len()`, `max()` e `sum()`. Aprofundamos nosso conhecimento sobre a **ordenação** e a **inversão** de sequências, e solidificamos a distinção crucial entre funções que criam novos objetos (`sorted()`, `reversed()`) e métodos que modificam o objeto original (_in-place_, como `.sort()` e `.reverse()`).

Demos um passo importante em direção a um estilo de programação mais declarativo ao explorar as **funções de ordem superior**. Aprendemos como `map()` nos permite aplicar uma transformação a toda uma coleção, e como `filter()` nos ajuda a selecionar elementos com base em um critério lógico, ambos de forma elegante e eficiente em termos de memória, graças ao uso de iteradores.

O domínio das funções embutidas é um pilar para se escrever um código verdadeiramente "Pythônico". Ao utilizá-las, não apenas economizamos tempo e esforço, mas também aproveitamos implementações altamente otimizadas e comunicamos nossas intenções de forma clara para outros desenvolvedores.

Até agora, operamos extensivamente com os tipos de dados e as ferramentas que a linguagem nos oferece. O próximo passo em nossa jornada como desenvolvedores é aprender a criar nossos próprios tipos de objetos, encapsulando dados e comportamentos em estruturas coesas e reutilizáveis. Isso nos introduzirá ao paradigma da **Programação Orientada a Objetos**, um dos pilares mais importantes do Python moderno.
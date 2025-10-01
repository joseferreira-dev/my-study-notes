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


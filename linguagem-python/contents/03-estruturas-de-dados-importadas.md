# Capítulo 3 – Estruturas de Dados Importadas

Nos capítulos anteriores, construímos um repertório sólido sobre os fundamentos da linguagem e suas estruturas de dados nativas. Listas, tuplas, dicionários e conjuntos nos oferecem as ferramentas essenciais para a maioria das tarefas de programação. No entanto, ao adentrarmos em domínios mais especializados, como a computação científica, a análise de dados e o aprendizado de máquina, surge a necessidade de estruturas mais otimizadas, capazes de lidar com grandes volumes de dados numéricos de forma eficiente.

É neste ponto que o ecossistema de bibliotecas do Python demonstra seu verdadeiro poder. Embora existam inúmeras bibliotecas, duas delas se destacam a ponto de serem consideradas parte do "núcleo" prático da linguagem para qualquer desenvolvedor que trabalhe com dados: **NumPy** e **Pandas**. Elas introduzem estruturas de dados próprias — o **Array** e o **DataFrame**, respectivamente — que são tão fundamentais que sua compreensão é indispensável. Neste capítulo, faremos uma imersão nessas duas estruturas, entendendo sua arquitetura, suas vantagens e as operações essenciais que as tornam as ferramentas de escolha para a manipulação de dados em Python.

## NumPy e o Poder dos Arrays

A biblioteca **NumPy** (abreviação de _Numerical Python_) é o pilar da computação científica em Python. Ela introduz um objeto fundamental e extremamente poderoso: o **array**. Um array NumPy (tecnicamente chamado de `ndarray`, ou _N-dimensional array_) é uma estrutura de dados que representa uma grade ou matriz de elementos.

Sua principal diferença em relação às listas nativas do Python é que um array NumPy é **homogêneo**, ou seja, todos os seus elementos devem ser, obrigatoriamente, do **mesmo tipo de dado**. Essa restrição é a chave para seu desempenho: ao garantir a homogeneidade, o NumPy pode armazenar os dados em um bloco contínuo de memória e executar operações matemáticas complexas em um nível muito mais baixo e otimizado (usando código C compilado), em vez de através do interpretador Python. O resultado é uma eficiência de memória e velocidade de processamento ordens de magnitude superior à das listas, especialmente ao lidar com grandes conjuntos de dados numéricos.

### Criando Arrays

Para começar a usar o NumPy, o primeiro passo é importá-lo. A convenção universal na comunidade Python é importar a biblioteca com o alias `np`.

```python
import numpy as np
```

A forma mais básica de criar um array é a partir de uma lista ou outra sequência nativa do Python, usando a função `np.array()`. A grande vantagem dos arrays é sua capacidade de representar dados em múltiplas dimensões.

- **Array de 1 Dimensão (Vetor):** É o análogo de uma lista.
    
    ```python
    # Criando um array 1D a partir de uma lista
    arr1d = np.array([10, 20, 30, 40, 50])
    print(arr1d)
    # Saída: [10 20 30 40 50]
    ```
    
- **Array de 2 Dimensões (Matriz):** É criado a partir de uma "lista de listas", onde cada lista interna representa uma linha da matriz.
    
    ```python
    # Criando um array 2D (matriz 2x3)
    arr2d = np.array([[1, 2, 3], [4, 5, 6]])
    print(arr2d)
    # Saída:
    # [[1 2 3]
    #  [4 5 6]]
    ```
    
- **Array de 3 Dimensões (Cubo/Tensor):** É criado a partir de uma "lista de listas de listas", adicionando uma dimensão de profundidade.
    
    ```python
    # Criando um array 3D
    arr3d = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])
    print(arr3d)
    # Saída:
    # [[[1 2]
    #   [3 4]]
    #
    #  [[5 6]
    #   [7 8]]]
    ```

A dimensionalidade é determinada pelo nível de aninhamento dos colchetes: `[]` para 1D, `[[]]` para 2D, `[[[]]]` para 3D, e assim por diante.

### Funções para Geração de Arrays

Frequentemente, precisamos criar arrays com valores predefinidos sem ter que digitar cada elemento. O NumPy oferece um conjunto de funções muito úteis para isso.

- **`np.zeros()` e `np.ones()`**: Criam arrays preenchidos inteiramente com zeros ou uns, respectivamente. O argumento é uma tupla que define o formato (shape) do array.
    
    ```python
    # Cria uma matriz 3x4 preenchida com zeros
    zeros_array = np.zeros((3, 4))
    print(zeros_array)
    # Saída:
    # [[0. 0. 0. 0.]
    #  [0. 0. 0. 0.]
    #  [0. 0. 0. 0.]]
    ```
    
- **`np.arange(inicio, fim, passo)`**: Funciona de forma similar à função `range()` nativa, criando um array com valores em uma sequência, mas com mais flexibilidade (pode usar passos de ponto flutuante). O intervalo é fechado no `inicio` e aberto no `fim`.
    
    ```python
    # Cria um array de 0 a 8, pulando de 2 em 2
    sequencia = np.arange(0, 10, 2)
    print(sequencia) # Saída: [0 2 4 6 8]
    ```
    
- **`np.linspace(inicio, fim, num_elementos)`**: Cria um array com um número específico de pontos, igualmente espaçados entre o `inicio` e o `fim`. Diferente do `arange`, o `fim` aqui é **inclusivo**.
    
    ```python
    # Cria um array com 5 pontos igualmente espaçados entre 0 e 1
    pontos = np.linspace(0, 1, 5)
    print(pontos) # Saída: [0.   0.25 0.5  0.75 1.  ]
    ```

### Inspecionando e Remodelando Arrays

Uma vez que um array é criado, podemos inspecionar suas propriedades e alterar seu formato.

- **Atributos de Inspeção:**
    - `.ndim`: Retorna o número de dimensões do array.
    - `.shape`: Retorna uma tupla com o tamanho de cada dimensão (ex: `(linhas, colunas)`).
    - `.size`: Retorna o número total de elementos no array.
    - `.dtype`: Retorna o tipo de dado dos elementos do array.
- **`.reshape(linhas, colunas)`**: Modifica o formato de um array para uma nova configuração, desde que o número total de elementos (`size`) seja preservado.
    
    ```python
    # Criando um array 1D de 1 a 10
    arr = np.arange(1, 11)
    
    # Remodelando para uma matriz 2x5
    matriz = arr.reshape(2, 5)
    
    print("--- Matriz ---")
    print(matriz)
    print("\n--- Atributos ---")
    print(f"Dimensões (ndim): {matriz.ndim}") # Saída: 2
    print(f"Formato (shape): {matriz.shape}") # Saída: (2, 5)
    print(f"Total de elementos (size): {matriz.size}") # Saída: 10
    print(f"Tipo de dados (dtype): {matriz.dtype}") # Saída: int64 (pode variar)
    ```

A matriz gerada teria a seguinte estrutura:

```
[[ 1  2  3  4  5]
 [ 6  7  8  9 10]]
```

### Operações Vetorizadas

A principal vantagem de desempenho do NumPy vem das **operações vetorizadas**. Isso significa que podemos aplicar operações matemáticas diretamente sobre arrays inteiros, e a operação será executada elemento a elemento de forma otimizada, sem a necessidade de escrever laços `for` explícitos.

Isso contrasta fortemente com o comportamento das listas nativas:

```python
# Comportamento da lista nativa
lista_nativa = [1, 2, 3]
print(lista_nativa * 3) # Repete a lista. Saída: [1, 2, 3, 1, 2, 3]

# Comportamento do array NumPy
array_numpy = np.array([1, 2, 3])
print(array_numpy * 3) # Multiplica cada elemento. Saída: [3 6 9]
```

Essa capacidade se estende a todas as operações aritméticas:

```python
arr = np.array([10, 20, 30])

# Operações com um escalar (um único número)
arr_soma = arr + 5      # Soma 5 a cada elemento -> [15 25 35]
arr_mult = arr * 2      # Multiplica cada elemento por 2 -> [20 40 60]
arr_quadrado = arr ** 2 # Eleva cada elemento ao quadrado -> [100 400 900]

print(arr_soma)
print(arr_mult)
print(arr_quadrado)
```

### Manipulação de Arrays: Fatiamento e Transposição

O acesso a subconjuntos de um array é feito através de uma sintaxe de fatiamento (`slicing`) poderosa, que se estende para múltiplas dimensões.

```python
arr = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# O fatiamento segue a lógica [inicio:fim], onde 'fim' é exclusivo
sub_array = arr[2:7]
print(sub_array) # Saída: [2 3 4 5 6]

# Fatiamento em 2D: array[linhas, colunas]
matriz = np.arange(1, 10).reshape(3, 3)
# [[1 2 3]
#  [4 5 6]
#  [7 8 9]]

# Pegando a primeira linha inteira
print(matriz[0, :]) # Saída: [1 2 3]

# Pegando a segunda coluna inteira
print(matriz[:, 1]) # Saída: [2 5 8]

# Pegando um sub-bloco da matriz
print(matriz[0:2, 1:3])
# Saída:
# [[2 3]
#  [5 6]]
```

A **transposição** é uma operação comum em álgebra linear que inverte as linhas e colunas de uma matriz. Em NumPy, isso pode ser feito com o método `.transpose()` ou, mais comumente, com o atributo `.T`.

```python
matriz = np.array([[1, 2, 3], [4, 5, 6]])
# Matriz original (2x3):
# [[1 2 3]
#  [4 5 6]]

matriz_transposta = matriz.T
# Matriz transposta (3x2):
# [[1 4]
#  [2 5]
#  [3 6]]

print(matriz_transposta)
```

> **Observação:** O NumPy é uma biblioteca vasta e poderosa, com centenas de funções para álgebra linear, estatística, transformadas de Fourier e muito mais. Esta introdução cobre os conceitos mais essenciais para começar a trabalhar com arrays, a estrutura de dados que serve de base para todo o ecossistema de ciência de dados em Python.


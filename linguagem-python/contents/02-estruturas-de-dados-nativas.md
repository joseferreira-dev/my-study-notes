# Capítulo 2 – Estruturas de Dados Nativas

À medida que nossos programas se tornam mais complexos, a necessidade de gerenciar múltiplos dados de forma organizada se torna fundamental. Armazenar cada peça de informação em uma variável individual é impraticável e ineficiente. É para resolver este desafio que as linguagens de programação oferecem **estruturas de dados**, contêineres especializados em armazenar coleções de valores. Elas nos permitem agrupar, acessar e manipular conjuntos de informações de maneira lógica e performática. O Python, em sua filosofia de "baterias inclusas", oferece quatro dessas estruturas de forma nativa, cada uma com suas próprias características e casos de uso ideais: as **Listas**, as **Tuplas**, os **Dicionários** e os **Conjuntos (Sets)**.

Neste capítulo, faremos uma exploração detalhada de cada uma dessas estruturas. Iniciaremos com as listas, a estrutura de coleção mais versátil e onipresente em Python, entendendo como criar, acessar e modificar seus elementos. Investigaremos as nuances de seu comportamento na memória e aprenderemos os métodos essenciais para sua manipulação no dia a dia. Dominar o uso dessas coleções é o que nos permitirá transitar de programas que operam sobre dados singulares para sistemas capazes de processar grandes volumes de informação de forma estruturada.

## Listas

As listas são, sem dúvida, a estrutura de dados de coleção mais fundamental e utilizada em Python. Uma lista é uma sequência **ordenada** e **mutável** de elementos. Vamos dissecar o que essas duas características significam:

- **Ordenada:** Os elementos em uma lista mantêm uma ordem específica e definida. O primeiro item que você adiciona será sempre o primeiro item, a menos que você o altere explicitamente. Essa ordem permite o acesso aos elementos através de sua posição, ou **índice**.
- **Mutável:** Uma vez que uma lista é criada, ela pode ser alterada. É possível adicionar novos elementos, remover elementos existentes e modificar os valores dos itens que já estão na lista.

Uma das grandes vantagens das listas em Python é que elas são **heterogêneas**, ou seja, podem conter elementos de qualquer tipo de dado, inclusive misturando-os em uma mesma lista. É perfeitamente válido ter uma lista que contém números inteiros, strings, booleanos e até mesmo outras listas.

A sintaxe para criar uma lista é simples e utiliza um par de colchetes `[]`, com os elementos separados por vírgulas.

```python
# Lista contendo apenas números inteiros (homogênea)
numeros_primos = [2, 3, 5, 7, 11, 13]

# Lista contendo apenas strings (homogênea)
nomes_frutas = ["maçã", "banana", "laranja"]

# Lista com múltiplos tipos de dados (heterogênea)
dados_usuario = ["Carlos", 35, 1.82, True] 
```

### Acessando Elementos pelo Índice

Como as listas são ordenadas, cada elemento possui uma posição única e numerada, chamada de índice. Conforme vimos no capítulo anterior, a indexação em Python começa em **0**. Portanto, para acessar o primeiro elemento de uma lista, usamos o índice `[0]`, para o segundo, o índice `[1]`, e assim por diante.

Vamos visualizar a estrutura de índices da nossa lista `numeros_primos`:

|Valor|2|3|5|7|11|13|
|---|---|---|---|---|---|---|
|Índice|0|1|2|3|4|5|

Para acessar um elemento específico, usamos o nome da variável da lista seguido por colchetes contendo o número do índice desejado. A função `print()` é frequentemente utilizada para exibir esses valores no console e verificar o estado do nosso programa.

```python
lista = [10, 20, 30, 40, 50]

# Acessando e imprimindo elementos individuais
primeiro_elemento = lista[0]
terceiro_elemento = lista[2]
ultimo_elemento = lista[4]

print(f"O primeiro elemento é: {primeiro_elemento}")  # Saída: O primeiro elemento é: 10
print(f"O terceiro elemento é: {terceiro_elemento}")  # Saída: O terceiro elemento é: 30
print(f"O último elemento é: {ultimo_elemento}")    # Saída: O último elemento é: 50

# Tentar acessar um índice que não existe resultará em um IndexError
# print(lista[5]) # Isso geraria um erro, pois o último índice válido é 4
```

### Modificando Listas: Métodos Essenciais

A mutabilidade das listas nos permite interagir com elas de diversas formas, adicionando, removendo e alterando seus elementos através de métodos específicos. Um método é uma função que "pertence" a um objeto (neste caso, à lista) e é chamado usando a notação de ponto (`.`).

Vamos considerar a lista `minha_lista = [1, 2, 3, 4]` como base para os exemplos a seguir.

- **`append(elemento)`**: Adiciona um único elemento ao **final** da lista.
    
    ```python
    minha_lista = [1, 2, 3, 4]
    minha_lista.append(5)
    print(minha_lista) # Saída: [1, 2, 3, 4, 5]
    ```
    
- **`insert(indice, elemento)`**: Insere um elemento em uma posição específica, "empurrando" os elementos subsequentes para a direita.
    
    ```python
    minha_lista = [1, 2, 3, 4]
    minha_lista.insert(1, 99) # Insere o valor 99 no índice 1
    print(minha_lista) # Saída: [1, 99, 2, 3, 4]
    ```
    
- **`remove(valor)`**: Remove a **primeira ocorrência** de um valor específico da lista. Se o valor não existir, um `ValueError` será gerado.
    
    ```python
    minha_lista = [1, 2, 99, 2, 3]
    minha_lista.remove(2) # Remove o primeiro '2' que encontrar
    print(minha_lista) # Saída: [1, 99, 2, 3]
    ```
    
- **`pop(indice)`**: Remove e **retorna** o elemento de um índice específico. Se nenhum índice for fornecido, ele remove e retorna o **último** elemento da lista.
    
    ```python
    minha_lista = [10, 20, 30, 40]
    ultimo = minha_lista.pop() # Remove o último elemento
    segundo = minha_lista.pop(1) # Remove o elemento no índice 1
    
    print(f"Lista após as remoções: {minha_lista}") # Saída: Lista após as remoções: [10, 30]
    print(f"Elemento removido do final: {ultimo}") # Saída: Elemento removido do final: 40
    print(f"Elemento removido do índice 1: {segundo}") # Saída: Elemento removido do índice 1: 20
    ```
    
- **`del` (instrução)**: `del` não é um método, mas uma instrução do Python. Pode ser usado para remover um elemento por seu índice.
    
    ```python
    minha_lista = [10, 20, 30, 40]
    del minha_lista[2] # Remove o elemento no índice 2
    print(minha_lista) # Saída: [10, 20, 40]
    ```

### Cópia de Listas: Valor vs. Referência

Um dos conceitos mais importantes e que frequentemente causa confusão para iniciantes é a diferença entre copiar o valor de uma variável e copiar sua referência. Com tipos de dados simples como números ou strings, a atribuição cria uma cópia independente. Com listas (e outros objetos mutáveis), o comportamento é diferente.

Quando você atribui uma lista a uma nova variável, você **não está criando uma nova lista**. Em vez disso, você está criando uma nova **referência** que aponta para o **mesmo objeto** na memória.

Imagine o seguinte cenário:

```python
# Criamos uma lista original
lista_a = [10, 20, 30]

# Atribuímos lista_a para lista_b. Ambas agora apontam para o mesmo local na memória.
lista_b = lista_a

print(f"Lista A: {lista_a}") # Saída: Lista A: [10, 20, 30]
print(f"Lista B: {lista_b}") # Saída: Lista B: [10, 20, 30]

# Agora, vamos modificar a lista_b
lista_b.append(99)

print("\n--- Após modificar a Lista B ---")
print(f"Lista A: {lista_a}") # Saída: Lista A: [10, 20, 30, 99]
print(f"Lista B: {lista_b}") # Saída: Lista B: [10, 20, 30, 99]
```

Como se pode observar, a alteração feita em `lista_b` refletiu imediatamente em `lista_a`. Isso ocorreu porque não existem duas listas, mas sim duas "etiquetas" (`lista_a` e `lista_b`) apontando para a mesma lista na memória.

Para criar uma cópia verdadeira e independente de uma lista, pode-se usar o método `copy()` ou a técnica de fatiamento `[:]`, que veremos em breve.

```python
lista_a = [10, 20, 30]
lista_c = lista_a.copy() # Cria uma cópia rasa e independente

lista_c.append(99)

print(f"Lista A: {lista_a}") # Saída: Lista A: [10, 20, 30]
print(f"Lista C: {lista_c}") # Saída: Lista C: [10, 20, 30, 99]
```

Neste caso, a `lista_a` permaneceu intacta, pois `lista_c` é um objeto completamente novo.


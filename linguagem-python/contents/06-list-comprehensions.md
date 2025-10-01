# Capítulo 6 – List Comprehensions

Nos capítulos anteriores, aprendemos a criar listas e a populá-las utilizando laços `for`, um método robusto e universal. Vimos como inicializar uma lista vazia e, a cada iteração, adicionar um novo elemento com o método `.append()`. Embora essa abordagem seja perfeitamente funcional, o Python, em sua busca pela elegância e expressividade, nos oferece uma sintaxe alternativa muito mais poderosa e concisa para essa tarefa: a **compreensão de lista**, ou _list comprehension_.

Esta técnica, profundamente enraizada na filosofia "Pythônica", nos permite construir listas complexas a partir de outras sequências de forma declarativa, em uma única linha de código. Ela combina a iteração de um laço `for` com a lógica de criação de uma nova lista, resultando em um código que não é apenas mais curto, mas também frequentemente mais rápido e legível. Neste capítulo, vamos dissecar a anatomia de uma _list comprehension_, desde sua forma mais básica até a aplicação de lógicas condicionais para filtragem e transformação, dominando uma das ferramentas mais elegantes do arsenal da linguagem.

## A Anatomia de uma List Comprehension

A melhor forma de entender o poder de uma _list comprehension_ é compará-la diretamente com o laço `for` tradicional. Imagine que queremos criar uma lista contendo o quadrado dos números de 0 a 9.

**A Abordagem Tradicional com `for`:**

```python
quadrados = [] # 1. Inicializa uma lista vazia
for i in range(10): # 2. Itera sobre a sequência
    quadrados.append(i ** 2) # 3. Aplica a expressão e adiciona o resultado à lista

print(quadrados)
# Saída: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Este processo envolve três passos claros: inicialização, iteração e adição.

**A Abordagem com List Comprehension:**

```python
quadrados_comp = [i ** 2 for i in range(10)]

print(quadrados_comp)
# Saída: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Em uma única linha, realizamos a mesma tarefa. A sintaxe pode ser lida quase como uma frase em inglês: "crie uma lista com `i ** 2` para cada `i` no intervalo de 0 a 9". A estrutura básica é:

```python
[expressao for item in iteravel]
```

Vamos dissecar cada componente:

- **`expressao`**: É a operação ou cálculo que será aplicado a cada elemento da sequência original e cujo resultado será inserido na nova lista. No nosso exemplo, a expressão é `i ** 2`.
- **`for item in iteravel`**: É a parte do laço, idêntica à de um `for` loop padrão. `iteravel` é a coleção de origem (no caso, `range(10)`), e `item` é a variável que recebe cada elemento a cada iteração (no caso, `i`).
- **`[]`**: Os colchetes que envolvem toda a sintaxe são o que indicam que o resultado final da operação será uma nova lista.

## Adicionando Lógica Condicional

A verdadeira expressividade das _list comprehensions_ se revela quando adicionamos lógica condicional, permitindo não apenas transformar, mas também filtrar os elementos da sequência original.

### Filtrando Elementos com `if`

Podemos adicionar uma cláusula `if` ao final da _list comprehension_ para incluir na nova lista apenas os elementos que satisfazem uma determinada condição. A sintaxe se torna:

```python
[expressao for item in iteravel if condicao]
```

Vamos usar essa estrutura para um exemplo clássico: criar uma lista contendo apenas os números pares de uma lista original.

**A Abordagem Tradicional com `for` e `if`:**

```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares = []
for numero in numeros:
    if numero % 2 == 0:
        pares.append(numero)

print(pares) # Saída: [2, 4, 6, 8, 10]
```

**A Abordagem com List Comprehension e `if`:**

```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares_comp = [numero for numero in numeros if numero % 2 == 0]

print(pares_comp) # Saída: [2, 4, 6, 8, 10]
```

A leitura da sintaxe é: "crie uma lista com `numero` para cada `numero` na lista `numeros`, se `numero` for par". A cláusula `if` atua como um filtro, determinando quais itens do iterável original serão processados pela expressão.

### Aplicando Expressões Condicionais com `if...else`

E se quisermos não apenas filtrar, mas aplicar transformações diferentes com base em uma condição? Por exemplo, e se quiséssemos elevar ao quadrado apenas os números pares, mas manter os números ímpares como estão? Para isso, utilizamos a sintaxe da **expressão condicional (operador ternário)**, que vimos no capítulo anterior.

Neste caso, a estrutura muda de lugar e a sintaxe se torna:

```python
[expressao_se_verdadeiro if condicao else expressao_se_falso for item in iteravel]
```

É crucial notar a diferença na posição:

- Um `if` de **filtragem** vem no final da compreensão.
- Um `if...else` de **transformação** vem no início, como parte da expressão.

**A Abordagem Tradicional com `for` e `if/else`:**

```python
numeros = [1, 2, 3, 4]
resultado = []
for numero in numeros:
    if numero % 2 == 0:
        resultado.append(numero ** 2)
    else:
        resultado.append(numero)

print(resultado) # Saída: [1, 4, 3, 16]
```

**A Abordagem com List Comprehension e `if/else`:**

```python
numeros = [1, 2, 3, 4]
resultado_comp = [numero ** 2 if numero % 2 == 0 else numero for numero in numeros]

print(resultado_comp) # Saída: [1, 4, 3, 16]
```

A leitura desta sintaxe é: "crie uma lista com `numero ** 2` se `numero` for par, senão `numero`, para cada `numero` na lista `numeros`".

## Aplicações Avançadas e Boas Práticas

As _list comprehensions_ podem ser estendidas para cenários ainda mais complexos.

- **Compreensões Aninhadas:** É possível aninhar laços `for` dentro de uma _list comprehension_, o que é útil para "achatar" listas de listas ou para criar produtos cartesianos.
    
    ```python
    # Achatar uma matriz (lista de listas) em uma única lista
    matriz = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    lista_achatada = [elemento for linha in matriz for elemento in linha]
    
    print(lista_achatada) # Saída: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    ```
    
- **Outras Compreensões:** O mesmo conceito pode ser aplicado para criar outras estruturas de dados, como conjuntos (`set`) e dicionários (`dict`).
    
    ```python
    # Set comprehension (remove duplicatas e cria um conjunto)
    numeros = [1, 2, 2, 3, 3, 3, 4]
    conjunto_unicos = {x for x in numeros}
    print(conjunto_unicos) # Saída: {1, 2, 3, 4}
    
    # Dictionary comprehension (cria um dicionário)
    chaves = ['a', 'b', 'c']
    valores = [1, 2, 3]
    meu_dicionario = {chave: valor for chave, valor in zip(chaves, valores)}
    print(meu_dicionario) # Saída: {'a': 1, 'b': 2, 'c': 3}
    ```

### Legibilidade vs. Complexidade

Apesar de seu poder, é preciso usar as _list comprehensions_ com bom senso. A regra de ouro é a legibilidade. Se uma _list comprehension_ se torna excessivamente longa, com múltiplos laços aninhados e condições complexas, ela pode se tornar mais difícil de entender do que um laço `for` tradicional. Nesses casos, a clareza de um laço explícito é preferível à concisão de uma compreensão complexa.

## Considerações Finais

Neste capítulo, mergulhamos em uma das características mais expressivas e "Pythônicas" da linguagem: as **compreensões de lista**. Vimos como essa sintaxe elegante nos permite substituir laços `for` verbosos por uma única linha de código declarativa, tornando a criação de listas a partir de sequências existentes uma tarefa mais concisa, legível e, muitas vezes, mais performática.

Dissecamos sua anatomia, desde a estrutura básica de expressão e iteração até a incorporação de lógica condicional. Aprendemos a diferenciar o uso da cláusula `if` para **filtrar** elementos da aplicação de uma expressão `if...else` para **transformar** elementos de maneiras distintas. Vislumbramos também seu poder em cenários mais avançados, como em laços aninhados, e vimos como o mesmo princípio se estende para a criação de conjuntos e dicionários.

A maestria no uso das _list comprehensions_ é um marco na jornada de um desenvolvedor Python, pois representa uma mudança na forma de pensar: de descrever os "passos" para construir uma lista para "declarar" o que a lista final deve conter. Com essa poderosa ferramenta em nosso repertório, estamos ainda mais equipados para escrever um código limpo, eficiente e idiomático. O próximo passo em nossa jornada será aprender a encapsular nossas lógicas e algoritmos em blocos de código reutilizáveis, nos introduzindo ao fundamental e poderoso mundo das **funções**.
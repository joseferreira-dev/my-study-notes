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

## Tuplas

Em contraste com a flexibilidade e a mutabilidade das listas, o Python nos oferece uma estrutura de dados chamada **tupla**, cuja principal característica é a **imutabilidade**. Uma tupla é uma coleção **ordenada** de elementos, muito semelhante a uma lista. Ela também pode conter itens de diferentes tipos de dados e seus elementos são acessados por um índice. A diferença fundamental é que, uma vez que uma tupla é criada, ela não pode ser alterada de forma alguma. Não é possível adicionar, remover ou modificar seus elementos.

- **Ordenada:** Os elementos mantêm a sequência em que foram definidos.
- **Imutável:** Após a criação, seu conteúdo não pode ser modificado.
- **Heterogênea:** Permite elementos de múltiplos tipos de dados.

Essa característica de imutabilidade não é uma limitação, mas sim uma poderosa ferramenta de design. As tuplas são utilizadas em cenários onde a integridade dos dados é crucial e queremos garantir que uma coleção de valores permaneça constante ao longo da execução do programa. São ideais para representar conjuntos de dados que formam uma entidade única e fixa, como coordenadas (x, y), cores RGB (vermelho, verde, azul) ou registros de banco de dados que não devem ser alterados.

A sintaxe para criar uma tupla utiliza um par de parênteses `()`, com os elementos separados por vírgulas.

```python
# Tupla contendo apenas números inteiros
coordenadas = (10, 20)

# Tupla com múltiplos tipos de dados
dados_produto = (101, "Teclado Mecânico", 350.75, True)

print(coordenadas)      # Saída: (10, 20)
print(dados_produto)    # Saída: (101, 'Teclado Mecânico', 350.75, True)
```

Uma curiosidade sintática importante ocorre na criação de uma tupla com um único elemento. Para que o Python não a confunda com uma simples expressão matemática entre parênteses, é necessário incluir uma vírgula ao final.

```python
nao_e_tupla = (50)
e_uma_tupla = (50,) # A vírgula final define a tupla

print(type(nao_e_tupla)) # Saída: <class 'int'>
print(type(e_uma_tupla)) # Saída: <class 'tuple'>
```

### Acessando Elementos e a Imutabilidade na Prática

O acesso aos elementos de uma tupla funciona exatamente da mesma forma que nas listas, utilizando a indexação baseada em zero.

```python
dados_produto = (101, "Teclado Mecânico", 350.75, True)

id_produto = dados_produto[0]
nome_produto = dados_produto[1]

print(f"ID: {id_produto}, Nome: {nome_produto}") # Saída: ID: 101, Nome: Teclado Mecânico
```

A principal diferença se manifesta quando tentamos realizar uma alteração. Qualquer tentativa de modificar um elemento de uma tupla resultará em um `TypeError`, um erro que o Python gera para indicar uma operação em um tipo inadequado.

```python
coordenadas = (10, 20)

# A linha a seguir irá gerar um erro, pois tuplas não suportam atribuição de item.
# coordenadas[0] = 15 # TypeError: 'tuple' object does not support item assignment
```

Essa proteção garante que a tupla permaneça em seu estado original, tornando o código mais seguro e previsível.

### Métodos Disponíveis

Devido à sua natureza imutável, as tuplas possuem um conjunto muito mais restrito de métodos em comparação com as listas. Elas oferecem apenas métodos que não alteram seu conteúdo, servindo para consulta e análise.

- **`count(valor)`**: Retorna o número de vezes que um determinado `valor` aparece na tupla.
- **`index(valor)`**: Retorna o índice da **primeira ocorrência** de um `valor` específico. Se o valor não for encontrado, um `ValueError` será gerado.

```python
historico_temperaturas = (25, 26, 28, 26, 27, 29, 26)

# Contando ocorrências
ocorrencias_26 = historico_temperaturas.count(26)
print(f"A temperatura 26ºC ocorreu {ocorrencias_26} vezes.") # Saída: A temperatura 26ºC ocorreu 3 vezes.

# Encontrando o índice de uma ocorrência
primeira_ocorrencia_28 = historico_temperaturas.index(28)
print(f"A temperatura 28ºC foi registrada pela primeira vez no índice {primeira_ocorrencia_28}.") # Saída: A temperatura 28ºC foi registrada pela primeira vez no índice 2.
```

É importante notar que esses dois métodos, `count()` e `index()`, também estão disponíveis e funcionam da mesma maneira para as listas.

### Desempacotamento de Tuplas (Tuple Unpacking)

Uma das funcionalidades mais elegantes e "Pythônicas" associadas às tuplas é o **desempacotamento**. Essa técnica permite atribuir os elementos de uma tupla a múltiplas variáveis de uma só vez, desde que o número de variáveis à esquerda da atribuição seja igual ao número de elementos na tupla.

```python
# Definindo uma tupla para representar um ponto 2D
ponto = (150, 75)

# Desempacotando a tupla nas variáveis x e y
x, y = ponto

print(f"A coordenada x é {x}") # Saída: A coordenada x é 150
print(f"A coordenada y é {y}") # Saída: A coordenada y é 75

# Exemplo com informações de um usuário
id_usuario, nome, email = (987, "Beatriz", "bia@email.com")

print(f"Usuário {nome} (ID: {id_usuario}) tem o e-mail: {email}")
# Saída: Usuário Beatriz (ID: 987) tem o e-mail: bia@email.com
```

O desempacotamento torna o código mais limpo e legível, evitando a necessidade de acessar cada elemento individualmente pelo seu índice.

## Dicionários

Enquanto listas e tuplas organizam seus dados em uma sequência ordenada, acessível por um índice numérico, os **dicionários** oferecem uma abordagem diferente e mais flexível: eles armazenam os dados em pares de **chave-valor**. Um dicionário é uma coleção **mutável** cujos elementos não são acessados por sua posição, mas por uma chave única associada a cada valor.

Pense em um dicionário de palavras real. Você não procura uma definição pela sua posição na página (índice), mas sim pela palavra (chave) que você deseja encontrar. Dicionários em Python funcionam sob a mesma premissa, permitindo a criação de mapeamentos lógicos entre uma chave e a informação que ela representa.

As características principais de um dicionário são:

- **Mutável:** É possível adicionar, remover e modificar os pares de chave-valor após sua criação.
- **Mapeado por Chaves:** O acesso aos valores é feito através de chaves, não de índices numéricos.
- **Chaves Únicas e Imutáveis:** Dentro de um dicionário, cada chave deve ser única. Além disso, as chaves devem ser de um tipo de dado imutável (como strings, números ou tuplas). Isso é necessário para que o Python possa calcular um identificador interno consistente para cada chave.
- **Valores Heterogêneos:** Os valores associados às chaves podem ser de qualquer tipo de dado, incluindo outras listas, tuplas ou até mesmo outros dicionários.

Uma observação importante sobre a ordem: historicamente, os dicionários em Python eram considerados coleções **não ordenadas**. No entanto, a partir da versão **3.7 do Python**, a ordem de inserção dos elementos passou a ser garantida. Isso significa que, nas versões modernas da linguagem, ao iterar sobre um dicionário, os itens aparecerão na mesma ordem em que foram adicionados.

A sintaxe para criar um dicionário utiliza um par de chaves `{}`. Cada item é um par `chave: valor`, e os pares são separados por vírgulas.

```python
# Exemplo de um dicionário para armazenar informações de um filme
filme = {
    "titulo": "Interestelar",
    "ano_lancamento": 2014,
    "diretor": "Christopher Nolan",
    "generos": ["Ficção Científica", "Drama", "Aventura"],
    "avaliacao_imdb": 8.7
}

print(filme)
```

Neste exemplo, `"titulo"`, `"ano_lancamento"`, `"diretor"`, `"generos"` e `"avaliacao_imdb"` são as chaves, e os dados à direita dos dois-pontos são seus respectivos valores. Note como o valor da chave `"generos"` é uma lista, demonstrando a flexibilidade da estrutura.

### Acessando e Modificando Dados

O poder dos dicionários está na sua capacidade de recuperar dados de forma eficiente através das chaves.

- **Acesso Direto:** A forma mais comum de acessar um valor é utilizando a sintaxe de colchetes `[]` com a chave desejada.
    
    ```python
    print(filme["titulo"])        # Saída: Interestelar
    print(filme["ano_lancamento"]) # Saída: 2014
    ```
    
    O risco dessa abordagem é que, se a chave não existir no dicionário, o Python levantará um `KeyError`, o que pode interromper a execução do programa.
    
- **Método `get()` (Acesso Seguro):** Para um acesso mais seguro, utiliza-se o método `get()`. Ele funciona de forma semelhante, mas se a chave não for encontrada, em vez de gerar um erro, ele retorna `None` (ou um valor padrão que pode ser especificado).
    
    ```python
    # Acessando uma chave existente
    diretor = filme.get("diretor")
    print(f"Diretor: {diretor}") # Saída: Diretor: Christopher Nolan
    
    # Tentando acessar uma chave que não existe
    bilheteria = filme.get("bilheteria")
    print(f"Bilheteria: {bilheteria}") # Saída: Bilheteria: None (sem erro)
    
    # Usando get() com um valor padrão
    bilheteria_padrao = filme.get("bilheteria", "Não informado")
    print(f"Bilheteria: {bilheteria_padrao}") # Saída: Bilheteria: Não informado
    ```

Para **adicionar** um novo par de chave-valor ou **atualizar** o valor de uma chave existente, a sintaxe de atribuição com colchetes é utilizada:

```python
# Atualizando o valor de uma chave existente
filme["avaliacao_imdb"] = 8.8
print(filme["avaliacao_imdb"]) # Saída: 8.8

# Adicionando um novo par de chave-valor
filme["estudio"] = "Warner Bros. Pictures"
print(filme["estudio"]) # Saída: Warner Bros. Pictures
```

O Python verifica se a chave já existe. Se sim, atualiza o valor. Se não, cria o novo par.

### Removendo Itens de um Dicionário

Existem várias maneiras de remover pares de chave-valor, cada uma com seu caso de uso.

- **`pop(chave)`**: Remove o item associado à `chave` especificada e **retorna** o valor removido. Se a chave não existir, gera um `KeyError`.
    
    ```python
    ano = filme.pop("ano_lancamento")
    print(f"O ano removido foi: {ano}") # Saída: O ano removido foi: 2014
    # print(filme) # O par "ano_lancamento": 2014 não estaria mais no dicionário
    ```
    
- **`del dicionario[chave]`**: A instrução `del` remove o item associado à chave. Não retorna nenhum valor.
    
    ```python
    del filme["diretor"]
    # print(filme) # O par "diretor": "Christopher Nolan" teria sido removido
    ```
    
- **`popitem()`**: Remove e retorna o último par de chave-valor adicionado ao dicionário (comportamento LIFO - Last-In, First-Out).
    
    ```python
    ultimo_item = filme.popitem()
    print(f"Último item adicionado e removido: {ultimo_item}") # Saída: ('estudio', 'Warner Bros. Pictures')
    ```
    
- **`clear()`**: Remove **todos** os itens do dicionário, deixando-o vazio.
    
    ```python
    filme.clear()
    print(filme) # Saída: {}
    ```

### Dicionários Aninhados

A capacidade dos valores de um dicionário serem eles mesmos outros dicionários permite a criação de estruturas de dados complexas e hierárquicas, conhecidas como dicionários aninhados.

```python
estudantes = {
    "aluno_1": {"nome": "Alice", "idade": 25, "curso": "Ciência da Computação"},
    "aluno_2": {"nome": "Bob", "idade": 30, "curso": "Engenharia"}
}

# Para acessar um valor aninhado, encadeamos os acessos por chave
curso_alice = estudantes["aluno_1"]["curso"]
print(f"O curso de Alice é: {curso_alice}") # Saída: O curso de Alice é: Ciência da Computação
```

### Métodos Úteis para Iteração

Os dicionários fornecem métodos específicos para acessar suas chaves, valores ou ambos, o que é extremamente útil ao trabalhar com laços de repetição.

- **`keys()`**: Retorna uma visualização de todas as chaves do dicionário.
- **`values()`**: Retorna uma visualização de todos os valores do dicionário.
- **`items()`**: Retorna uma visualização de todos os pares de chave-valor como tuplas.

```python
filme = {
    "titulo": "Interestelar",
    "ano_lancamento": 2014,
    "diretor": "Christopher Nolan"
}

print("--- Chaves ---")
for chave in filme.keys():
    print(chave)

print("\n--- Valores ---")
for valor in filme.values():
    print(valor)

print("\n--- Itens (Chave e Valor) ---")
# A forma mais comum e poderosa de iterar sobre um dicionário
for chave, valor in filme.items():
    print(f"{chave.title()}: {valor}")
```

A saída da última iteração seria:

```
Titulo: Interestelar
Ano_Lancamento: 2014
Diretor: Christopher Nolan
```


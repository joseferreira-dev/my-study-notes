# Capítulo 5 – Estruturas de Controle

Nos capítulos anteriores, focamos em definir e organizar os dados, aprendendo sobre variáveis, tipos e as diversas estruturas de coleção que o Python nos oferece. Construímos um repertório sólido sobre "o que" são as informações em nossos programas. Agora, vamos mudar o foco para "como" o programa se comporta e interage com esses dados. Até o momento, nossos scripts executaram de forma linear, uma instrução após a outra, do início ao fim. Contudo, a verdadeira força da programação reside na capacidade de um programa analisar situações, tomar decisões e repetir tarefas de forma autônoma.

Neste capítulo, mergulharemos nas **estruturas de controle**, os mecanismos que governam o fluxo de execução de um programa. Iniciaremos com as **estruturas condicionais**, os famosos blocos `if...else`, que são a base para toda a tomada de decisão, permitindo que nosso código escolha qual caminho seguir com base em critérios lógicos. Em seguida, exploraremos as **estruturas de repetição**, ou laços, que nos capacitam a executar um mesmo bloco de código múltiplas vezes, seja sobre os itens de uma coleção ou enquanto uma determinada condição for verdadeira. Dominar essas estruturas é o que nos permitirá transitar de scripts estáticos para algoritmos dinâmicos e inteligentes.

## Estruturas de Condição

As estruturas condicionais são a espinha dorsal da lógica em qualquer programa. Elas permitem que o código se comporte de maneira diferente com base na avaliação de uma ou mais condições, executando blocos de instruções específicos apenas se um critério for atendido. Em Python, essa lógica é implementada de forma clara e legível através das palavras-chave `if`, `elif` e `else`.

A estrutura funciona como uma cascata de verificações. O Python avalia a primeira condição (`if`). Se ela for verdadeira, seu bloco de código correspondente é executado, e toda a estrutura condicional é encerrada. Se for falsa, ele passa para a próxima condição (`elif`), e assim por diante. Caso nenhuma das condições anteriores seja atendida, o bloco final (`else`), que é opcional, será executado.

A sintaxe geral é a seguinte, prestando sempre atenção à indentação que define cada bloco:

```python
if condicao_1:
    # Bloco de código a ser executado se a condicao_1 for verdadeira.
elif condicao_2:
    # Bloco de código a ser executado se a condicao_1 for falsa E a condicao_2 for verdadeira.
else:
    # Bloco de código a ser executado se NENHUMA das condições anteriores for verdadeira.
```

Vamos analisar um exemplo prático para classificar um número:

```python
x = 20

if x > 10:
    print("O valor de x é maior que 10.")
elif x == 10:
    print("O valor de x é exatamente 10.")
else:
    print("O valor de x é menor que 10.")

# Saída: O valor de x é maior que 10.
```

Neste exemplo, o Python primeiro testa `x > 10`. Como `20 > 10` é `True`, a primeira mensagem é impressa, e as cláusulas `elif` e `else` são completamente ignoradas.

A cláusula `elif` é uma contração de "else if" e é a forma Pythônica de encadear múltiplas verificações exclusivas. Podemos ter quantos `elif` forem necessários.

```python
nota = 75

if nota >= 90:
    conceito = "A"
elif nota >= 80:
    conceito = "B"
elif nota >= 70:
    conceito = "C"
elif nota >= 60:
    conceito = "D"
else:
    conceito = "F"

print(f"O conceito do aluno é: {conceito}") # Saída: O conceito do aluno é: C
```

É importante notar que o Python não possui uma estrutura `switch...case` como outras linguagens. A cadeia `if...elif...else` é a forma padrão e recomendada para lidar com múltiplas condições.

### Expressão Condicional (Operador Ternário)

Para situações simples onde precisamos atribuir um de dois valores a uma variável com base em uma condição, a estrutura `if...else` completa pode parecer um pouco verbosa. Para esses casos, o Python oferece uma sintaxe mais concisa e elegante, conhecida como **expressão condicional** ou, informalmente, **operador ternário**.

A sintaxe é: `valor_se_verdadeiro if condicao else valor_se_falso`

```python
# Abordagem tradicional com if/else
idade = 22
if idade >= 18:
    status = "Maior de idade"
else:
    status = "Menor de idade"
print(status) # Saída: Maior de idade

# A mesma lógica com a expressão condicional
status_ternario = "Maior de idade" if idade >= 18 else "Menor de idade"
print(status_ternario) # Saída: Maior de idade
```

A expressão condicional é extremamente útil para atribuições diretas, tornando o código mais compacto e, em muitos casos, mais legível, desde que a lógica permaneça simples.

## Estruturas de Repetição

Enquanto as estruturas condicionais nos dão a capacidade de escolher um caminho, as **estruturas de repetição**, ou **laços** (_loops_), nos dão o poder da automação. Elas nos permitem executar o mesmo bloco de código várias vezes, seja um número fixo de iterações ou enquanto uma determinada condição permanecer verdadeira. Essa capacidade de iterar sobre coleções de dados e repetir processos é o que torna os computadores ferramentas tão poderosas para a automação de tarefas. O Python oferece duas estruturas de repetição principais: o laço `for` e o laço `while`.

### Laço `for`: Iterando sobre Sequências

O laço `for` é a principal ferramenta do Python para iterar sobre os elementos de uma sequência. Ele é ideal para situações em que sabemos (ou podemos determinar) o conjunto de itens que queremos percorrer, como os caracteres de uma string, os elementos de uma lista ou um intervalo de números. Sua filosofia é executar um bloco de código "para cada item em uma coleção".

A sintaxe do laço `for` é notavelmente limpa e legível:

```python
for item in sequencia:
    # Bloco de código a ser executado para cada item.
```

- **`sequencia`**: Pode ser qualquer objeto iterável do Python (uma lista, tupla, string, `range`, dicionário, etc.).
- **`item`**: É uma variável temporária que, a cada iteração do laço, receberá o valor do próximo elemento da sequência. O nome dessa variável é escolhido pelo programador.

O caso de uso mais comum é iterar um número específico de vezes usando a função `range()`.

```python
# A função range(5) gera uma sequência de números de 0 a 4.
for i in range(5):
    print(f"Esta é a iteração número: {i}")
```

A saída será:

```
Esta é a iteração número: 0
Esta é a iteração número: 1
Esta é a iteração número: 2
Esta é a iteração número: 3
Esta é a iteração número: 4
```

Observe que a variável `i` assume o valor de cada número na sequência gerada pelo `range()` a cada passagem pelo laço. Embora qualquer nome de variável seja válido, o uso de `i` (de "índice" ou "iteração") como variável de controle em laços simples é uma convenção quase universal na programação, o que torna o código instantaneamente reconhecível para outros desenvolvedores.

#### Iterando sobre Coleções

O verdadeiro poder do `for` se manifesta ao iterar diretamente sobre os elementos de uma coleção, como uma lista.

```python
frutas = ['maçã', 'banana', 'laranja']

for fruta in frutas:
    print(f"Eu gosto de {fruta.title()}.")
```

A saída será:

```
Eu gosto de maçã.
Eu gosto de banana.
Eu gosto de laranja.
```

Neste exemplo, a cada iteração, a variável `fruta` recebe o valor de um dos itens da lista `frutas`, tornando o código extremamente legível e direto.

Para acessar tanto o índice quanto o valor de um item durante a iteração, o Python oferece a função `enumerate()`. Ela transforma a sequência em uma série de pares `(índice, valor)`.

```python
frutas = ['maçã', 'banana', 'laranja']

for indice, fruta in enumerate(frutas):
    print(f"A fruta na posição {indice} é {fruta}.")
```

A saída será:

```
A fruta na posição 0 é maçã.
A fruta na posição 1 é banana.
A fruta na posição 2 é laranja.
```

#### Iterando sobre Dicionários

O laço `for` também é a ferramenta ideal para percorrer dicionários. Podemos iterar sobre suas chaves, seus valores ou ambos.

- **`.keys()`**: Itera sobre as chaves (comportamento padrão).
- **`.values()`**: Itera sobre os valores.
- **`.items()`**: Itera sobre os pares `(chave, valor)`.

```python
capitais = {
    'Brasil': 'Brasília',
    'França': 'Paris',
    'Japão': 'Tóquio'
}

# A forma mais comum e completa: usando .items()
for pais, capital in capitais.items():
    print(f"A capital de {pais} é {capital}.")
```

A saída será:

```
A capital de Brasil é Brasília.
A capital de França é Paris.
A capital de Japão é Tóquio.
```

#### A Cláusula `else` no Laço `for`

Uma característica peculiar e interessante do Python é a possibilidade de adicionar uma cláusula `else` a um laço `for`. O bloco de código do `else` será executado **uma única vez**, ao final do laço, mas **somente se o laço for concluído naturalmente**, ou seja, se ele percorrer todos os seus itens sem ser interrompido.

```python
numeros = [1, 2, 3, 4]

for numero in numeros:
    print(f"Processando o número: {numero}")
else:
    print("Todos os números foram processados com sucesso.")
```

Este `else` é útil para executar uma ação de "conclusão" ou "limpeza" após o término bem-sucedido de uma iteração.

#### Interrompendo o Laço com `break`

Em certas situações, pode ser necessário interromper a execução de um laço antes que ele percorra todos os seus itens. Para isso, utilizamos a instrução `break`. Assim que o `break` é encontrado, o laço é encerrado imediatamente, e a execução do programa continua na primeira linha após o bloco do laço.

Imagine que estamos procurando por um número específico em uma lista de 1 a 10.

```python
for i in range(1, 11): # Sequência de 1 a 10
    print(f"Verificando o número {i}...")
    if i == 5:
        print("Número 5 encontrado! Encerrando a busca.")
        break  # Interrompe o laço imediatamente
else:
    print("O número 5 não foi encontrado.") # Este bloco não será executado
```

A saída será:

```
Verificando o número 1...
Verificando o número 2...
Verificando o número 3...
Verificando o número 4...
Verificando o número 5...
Número 5 encontrado! Encerrando a busca.
```

O laço para no número 5 e não continua até o 10. É crucial notar que, como o laço foi interrompido pelo `break`, a cláusula `else` **não é executada**. Isso torna a combinação `for...else` uma ferramenta poderosa para lógicas de busca: o bloco `for` procura por algo, e o bloco `else` é executado se a busca terminar sem que o item seja encontrado.

### Laço `while`: Repetindo com Base em uma Condição

Enquanto o laço `for` é perfeito para percorrer sequências com um número definido de elementos, o laço `while` é a ferramenta ideal para situações em que não sabemos de antemão quantas vezes um bloco de código precisa ser executado. O `while` é um laço controlado por **condição**: ele repetirá um bloco de código continuamente **enquanto** uma determinada condição booleana for avaliada como verdadeira.

A estrutura de um laço `while` é direta e se assemelha à de uma estrutura `if`:

```python
while condicao:
    # Bloco de código a ser repetido
    # ...
    # É crucial que algo dentro deste bloco, em algum momento,
    # altere a 'condicao' para False, para evitar um laço infinito.
```

O fluxo de execução é simples:

1. O Python testa a `condicao`.
2. Se a `condicao` for `True`, todo o bloco de código indentado é executado.
3. Ao final do bloco, a execução retorna ao topo e a `condicao` é testada novamente.
4. Esse ciclo se repete até que a `condicao` seja avaliada como `False`. Nesse momento, o laço é encerrado e o programa continua sua execução.

Vamos ver um exemplo prático que simula uma contagem regressiva:

```python
contador = 5

print("Iniciando contagem regressiva...")

while contador > 0:
    print(contador)
    contador = contador - 1 # ou contador -= 1

print("Lançar!")
```

A saída será:

```
Iniciando contagem regressiva...
5
4
3
2
1
Lançar!
```

Neste exemplo, a variável `contador` é inicializada com `5`. A cada iteração, seu valor é impresso e, em seguida, decrementado em uma unidade. Na sexta verificação, `contador` terá o valor `0`. Como a condição `0 > 0` é `False`, o laço é encerrado.

#### Controle de Fluxo no `while`: `else`, `break` e `continue`

O laço `while` suporta as mesmas instruções de controle que vimos no laço `for`, como `break` e a cláusula `else`, que funcionam de maneira idêntica.

- **`else`**: O bloco `else` é executado uma vez quando o laço termina naturalmente (ou seja, quando sua condição se torna `False`).
- **`break`**: Interrompe o laço imediatamente, independentemente do estado da condição. O bloco `else` não é executado se o laço for encerrado por um `break`.

Além delas, os laços em Python contam com uma terceira instrução de controle: `continue`.

- **`continue`**: A instrução `continue` não encerra o laço, mas **interrompe a iteração atual** e pula imediatamente para a próxima verificação da condição. Qualquer código no bloco que esteja após o `continue` não será executado naquela iteração específica.

Vamos ver um exemplo que imprime apenas os números ímpares de 1 a 10, usando o `continue` para "pular" os números pares.

```python
numero = 0

while numero < 10:
    numero += 1
    
    # Se o número for par, pula o resto do código e volta para o topo do laço
    if numero % 2 == 0:
        continue
        
    print(f"Número ímpar encontrado: {numero}")
```

A saída será:

```
Número ímpar encontrado: 1
Número ímpar encontrado: 3
Número ímpar encontrado: 5
Número ímpar encontrado: 7
Número ímpar encontrado: 9
```

Quando `numero` é par (2, 4, 6, 8, 10), a condição `numero % 2 == 0` é verdadeira, o `continue` é executado, e a instrução `print()` é pulada naquela iteração.

#### Cuidado com Laços Infinitos

A principal responsabilidade ao usar um laço `while` é garantir que a condição de parada seja, em algum momento, alcançada. Se a condição de teste nunca se tornar `False`, o programa entrará em um **laço infinito**, onde o mesmo bloco de código será executado para sempre, travando a aplicação.

```python
# ATENÇÃO: Exemplo de laço infinito. Não execute sem saber como interromper (Ctrl+C).
# contador_infinito = 0
# while contador_infinito < 5:
#     print("Isso vai repetir para sempre!")
# A variável 'contador_infinito' nunca é alterada, então a condição sempre será True.
```

Apesar do risco, laços `while True:` são uma construção comum e útil em certos cenários, como em menus de linha de comando, servidores que precisam "escutar" por conexões continuamente, ou em laços de jogos. Nesses casos, o laço é intencionalmente infinito, mas possui uma lógica interna com uma instrução `break` para garantir que haja uma condição de saída controlada.

```python
# Exemplo de um laço "infinito" controlado
while True:
    comando = input("Digite 'sair' para terminar ou qualquer outra coisa para continuar: ")
    if comando.lower() == 'sair':
        print("Encerrando o programa...")
        break # Garante a condição de saída
    else:
        print(f"Você digitou: {comando}")
```

## Considerações Finais

Neste capítulo, demos um salto fundamental, transitando da simples execução linear de comandos para a criação de programas com lógica e dinamismo. As **estruturas de controle** que exploramos são, em essência, o cérebro de nossos algoritmos, permitindo que eles analisem, decidam e repitam tarefas de forma autônoma.

Iniciamos com as **estruturas condicionais**, o pilar da tomada de decisão no código. Através da cascata `if...elif...else`, aprendemos a direcionar a execução do programa por diferentes caminhos com base em testes lógicos, além de termos visto a sintaxe concisa da expressão condicional para atribuições simples.

Em seguida, mergulhamos no poder da automação com as **estruturas de repetição**. Dissecamos o laço `for` como a ferramenta idiomática e ideal para percorrer cada item de uma sequência, e o laço `while` como a solução robusta para executar um bloco de código enquanto uma condição específica for verdadeira. Vimos também como as instruções `break` e `continue`, em conjunto com a cláusula `else` dos laços, nos dão um controle refinado sobre o comportamento dessas iterações, permitindo interrupções e desvios de fluxo.

A verdadeira maestria emerge da combinação dessas estruturas. As condicionais nos permitem questionar nossos dados, e os laços nos permitem agir sobre eles em escala, processando listas, dicionários e outras coleções com lógicas complexas.

Com a capacidade de ditar o "quando", o "se" e o "quantas vezes" da execução de nosso código, estamos prontos para o próximo nível de abstração e organização. O passo seguinte em nossa jornada será aprender a encapsular essas lógicas em blocos reutilizáveis e nomeados, o que nos introduzirá ao poderoso e indispensável mundo das **funções**.
# Capítulo 2: Variáveis e Tipos de Dados Simples

Aprender a manipular dados é um dos pilares da programação. Seja para registrar informações de um usuário, calcular valores ou controlar a lógica de um sistema, o programador precisa saber como armazenar, modificar e interpretar dados. Neste capítulo, mergulharemos nas estruturas mais fundamentais da linguagem Python: as variáveis e os tipos de dados simples. Aqui, entenderemos como Python lida com a atribuição e manipulação de valores, como nomear corretamente variáveis, como evitar erros comuns e como trabalhar com dois tipos primordiais: strings (textos) e números (inteiros e de ponto flutuante). Todos esses conceitos são essenciais para desenvolver aplicações robustas e bem estruturadas.

## Variáveis e Atribuição de Valores

Em Python, uma variável é uma referência simbólica para um valor armazenado na memória. Quando você atribui um valor a uma variável, está dizendo ao computador: "guarde esse dado com esse nome para que eu possa utilizá-lo depois". A forma mais simples de se fazer isso é utilizando o operador de atribuição `=`.

```python
nome = "Lucas"
idade = 29
altura = 1.78
```

No exemplo acima, criamos três variáveis: `nome`, `idade` e `altura`. A primeira armazena uma string (sequência de caracteres), a segunda um número inteiro (int) e a terceira um número de ponto flutuante (float). O Python infere automaticamente o tipo de dado com base no valor atribuído. Essa característica faz parte da chamada **tipagem dinâmica** da linguagem, ou seja, não é necessário declarar explicitamente o tipo da variável.

Internamente, Python trata as variáveis como rótulos que apontam para objetos. Isso significa que uma variável pode, em diferentes momentos, referenciar diferentes tipos de dados. Essa flexibilidade exige atenção, pois mudanças de tipo podem gerar erros sutis em programas mais complexos.

## Atribuição Múltipla

Python permite atribuir valores a múltiplas variáveis ao mesmo tempo, o que torna o código mais conciso e elegante. Essa funcionalidade é particularmente útil para inicializar diversas variáveis de uma vez ou realizar trocas de valores entre variáveis sem o uso de uma variável temporária.

```python
x, y, z = 10, 20, 30
print(x, y, z)  # Saída: 10 20 30
```

Você também pode atribuir o mesmo valor a múltiplas variáveis:

```python
a = b = c = 100
```

E uma operação clássica, como a troca de valores entre duas variáveis, pode ser feita de forma direta:

```python
a = 5
b = 10
a, b = b, a  # Troca os valores
print(a, b)  # Saída: 10 5
```

Esse tipo de atribuição elegante é uma das características que tornam o Python expressivo e fácil de escrever.

## Nomeando Variáveis: Convenções e Cuidados

Escolher bons nomes para variáveis é essencial para que o código seja legível, compreensível e mantenível. Python impõe algumas regras para nomes válidos:

- Devem começar com uma letra (a-z, A-Z) ou underline (`_`).
- Podem conter letras, números e underscores, mas não podem conter espaços.
- Não podem utilizar caracteres especiais (como acentos, ç, %, etc.).
- Não podem ser iguais a palavras reservadas da linguagem (ex: `class`, `for`, `while`).

Por exemplo:

```python
usuario_id = 105
nome_completo = "Joana Silva"
_valido = True  # válido, mas o uso de _ no início tem significado especial em alguns contextos
```

Python diferencia letras maiúsculas e minúsculas. Assim, `nome`, `Nome` e `NOME` são variáveis diferentes. Apesar disso, a convenção mais adotada é escrever nomes de variáveis em letras minúsculas, separando palavras com underscores (o chamado `snake_case`).

Evite nomes genéricos como `a`, `b`, `x`, `y`, a menos que o contexto seja claro, como em operações matemáticas simples. Prefira nomes que deixem evidente o propósito da variável.

### Erros Comuns em Nomes de Variáveis

Alguns erros frequentes que devem ser evitados incluem:
- Usar espaços: `nome completo` (inválido)
- Começar com número: `1usuario` (inválido)
- Usar acentos ou cedilha: `função`, `profissão` (inválido ou não recomendado)
- Reutilizar nomes com finalidades diferentes: isso pode causar confusão ou bugs difíceis de identificar

## Constantes

Embora Python não tenha suporte nativo a constantes (valores que não podem ser modificados após serem definidos), existe uma convenção entre programadores para representar esses valores. Constantes devem ser nomeadas com letras maiúsculas, separadas por underscores:

```python
PI = 3.14159
TAXA_CAMBIO = 5.27
```

Essa convenção indica que o valor **não deve ser alterado**. O Python não impedirá que isso aconteça, mas cabe ao programador respeitar essa restrição.

## Comentários

Comentários são trechos de texto ignorados pelo interpretador que servem para documentar o código. Eles ajudam você e outras pessoas a entenderem a lógica e o funcionamento do programa.

Para comentários de linha única, usa-se o símbolo `#`:

```python
# Calcula a área de um círculo
area = PI * (raio ** 2)
```

Para comentários mais longos ou explicações que envolvem várias linhas, recomenda-se utilizar múltiplas linhas com `#`, embora também seja possível usar aspas triplas (`"""` ou `'''`), que criam strings multilinha — embora não sejam tecnicamente comentários:

```python
"""
Este bloco explica a função principal do programa.
Pode ser usado para documentação.
"""
```

Use comentários com moderação: nem em excesso, nem escassamente. O ideal é escrever código claro o suficiente que dispense explicações óbvias, mas que também forneça contexto onde necessário.

## Strings (Textos)

### Declaração de Strings

Strings são utilizadas para representar texto. Em Python, podem ser declaradas usando aspas simples (`'`) ou duplas (`"`). Ambas funcionam da mesma maneira:

```python
nome = 'Maria'
saudacao = "Bem-vindo"
```

Se for necessário incluir aspas dentro da string, use aspas diferentes para delimitação, ou use o caractere de escape (`\`) para evitar erros:

```python
frase = "Ela disse: 'Olá!'"
frase2 = 'Ele respondeu: "Tudo bem?"'
```

### Strings Multilinha

Quando se deseja criar uma string com várias linhas, utiliza-se aspas triplas:

```python
mensagem = """
Olá,
Este é um e-mail automático.
Obrigado!
"""
```

### Métodos de Manipulação de Texto

Strings são objetos que possuem diversos métodos incorporados. Alguns dos mais usados incluem:

- `upper()`: retorna a string com todas as letras em maiúsculo.
- `lower()`: retorna todas as letras em minúsculo.
- `title()`: coloca a primeira letra de cada palavra em maiúsculo.
- `capitalize()`: apenas a primeira letra da primeira palavra em maiúsculo.

```python
nome = "ana carolina"
print(nome.upper())      # ANA CAROLINA
print(nome.lower())      # ana carolina
print(nome.title())      # Ana Carolina
print(nome.capitalize()) # Ana carolina
```

Esses métodos não modificam a string original, pois strings são imutáveis em Python. Para alterar uma string, é necessário reatribuir o valor à variável.

### f-strings (Interpolação de Variáveis)

Para incorporar variáveis dentro de strings, usa-se o recurso das f-strings, que tornam a construção de mensagens muito mais simples:

```python
nome = "João"
idade = 30
print(f"{nome} tem {idade} anos.")
```

Você pode incluir expressões matemáticas dentro das chaves:

```python
print(f"Daqui a 10 anos, {nome} terá {idade + 10} anos.")
```

### Espaços em Branco, Tabs e Quebras de Linha

Caracteres invisíveis, como espaços, tabulações (`\t`) e quebras de linha (`\n`), são úteis para formatar strings:

```python
print("Nome:\tCarlos\nIdade:\t40")
```

A saída será:

```
Nome:   Carlos
Idade:  40
```

### Remoção de Espaços e Caracteres

Python oferece métodos específicos para eliminar espaços desnecessários:
- `strip()`: remove espaços no início e no fim.
- `lstrip()`: remove à esquerda.
- `rstrip()`: remove à direita.

```python
texto = "  exemplo  "
print(texto.strip())   # "exemplo"
```

Desde o Python 3.9, é possível usar:
- `removeprefix()`
- `removesuffix()`

```python
arquivo = "dados_relatorio.csv"
print(arquivo.removesuffix(".csv"))  # dados_relatorio
```

## Números

Python possui dois tipos principais de números: inteiros (`int`) e ponto flutuante (`float`).

### Inteiros (int)

Usados para representar valores sem casas decimais:

```python
idade = 25
ano = 2025
saldo = -1500
```

### Operações com Inteiros

```python
print(7 + 3)   # Soma
print(7 - 3)   # Subtração
print(7 * 3)   # Multiplicação
print(7 // 2)  # Divisão inteira
print(7 % 2)   # Módulo (resto da divisão)
print(2 ** 3)  # Exponenciação
```

### Ponto Flutuante (float)

Usado para representar números com parte decimal:

```python
peso = 70.5
altura = 1.75
```

### Operações com float

```python
print(5.5 + 2.3)
print(8.0 / 2)
```

Misturar `int` com `float` resulta em um `float`:

```python
print(5 + 1.0)  # 6.0
```

### Underscores em Números

Para facilitar a leitura de números grandes, pode-se usar underscores:

```python
populacao = 213_000_000
prejuizo = 1_500_000.75
```

Eles não afetam o valor numérico e são ignorados pelo interpretador.

---

Este capítulo abordou os fundamentos que sustentam toda e qualquer aplicação escrita em Python. Compreender profundamente variáveis, tipos básicos e manipulação de dados é essencial para explorar estruturas mais complexas e criar programas funcionais, elegantes e eficientes. Nos próximos capítulos, daremos continuidade à construção desse conhecimento, abordando estruturas de controle, coleções e funções.


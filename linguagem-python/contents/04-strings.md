# Capítulo 4 – Manipulação e Formatação de Strings

Nos capítulos anteriores, estabelecemos as fundações da linguagem Python, desde sua sintaxe básica até as estruturas de dados que nos permitem organizar a informação. Agora, vamos nos aprofundar em um dos tipos de dados mais comuns e vitais em qualquer aplicação: a **string**. Raramente um programa existe sem a necessidade de interagir com texto, seja para exibir mensagens ao usuário, ler dados de um arquivo, gerar relatórios ou comunicar-se com outras aplicações pela internet.

Simplesmente armazenar texto em uma variável, no entanto, é apenas o começo. O verdadeiro poder emerge quando aprendemos a manipular, combinar e, principalmente, formatar strings de forma dinâmica. Neste capítulo, exploraremos o arsenal de ferramentas que o Python oferece para trabalhar com texto. Começaremos investigando as diferentes técnicas de formatação de strings, desde a abordagem clássica herdada da linguagem C até as modernas e expressivas _f-strings_. Em seguida, mergulharemos nos métodos de manipulação, aprendendo a fatiar, buscar, substituir e transformar textos para atender às mais diversas necessidades. Dominar o trabalho com strings é uma habilidade indispensável que eleva a capacidade de criar programas interativos e comunicativos.

## Formatando Strings

Em muitos cenários, o texto que desejamos construir não é estático. Precisamos criar mensagens que incorporem o valor de variáveis, resultados de cálculos ou dados inseridos pelo usuário. Esse processo de criar uma string final a partir de um modelo e de dados variáveis é chamado de **formatação de strings**. O Python, ao longo de sua evolução, ofereceu diferentes abordagens para essa tarefa, cada uma com suas vantagens e particularidades.

### Operador de Formatação `%`

A forma mais antiga e tradicional de formatar strings em Python é através do operador de modulação (`%`), uma sintaxe herdada da função `printf` da linguagem C. A ideia é criar uma string que atue como um "gabarito", contendo espaços reservados chamados de **placeholders**. Esses placeholders, que começam com o caractere `%`, indicam onde e como um valor variável deve ser inserido.

Os principais tipos de placeholders são:

|Placeholder|Tipo de Dado|Descrição|
|---|---|---|
|`%s`|String|Converte o valor para uma string.|
|`%d`|Inteiro|Formata o valor como um número inteiro decimal.|
|`%f`|Ponto Flutuante|Formata o valor como um número de ponto flutuante.|
|`%x`|Hexadecimal|Formata o valor como um número hexadecimal.|
|`%e`|Notação Científica|Formata o valor em notação científica.|

Após a string gabarito, utilizamos o operador `%` seguido pelos valores que serão substituídos, geralmente dentro de uma tupla.

```python
# Exemplo com múltiplos placeholders
nome = 'Alice'
idade = 30
altura = 1.65

# A ordem dos valores na tupla corresponde à ordem dos placeholders na string
string_formatada = 'Nome: %s, Idade: %d, Altura: %f' % (nome, idade, altura)
print(string_formatada)
# Saída: Nome: Alice, Idade: 30, Altura: 1.650000
```

Se houver apenas um placeholder, os valores não precisam estar em uma tupla:

```python
print('O processador está com %d%% de uso.' % 95)
# Saída: O processador está com 95% de uso.
# Note que para exibir o caractere '%' literal, usamos '%%'.
```

Essa sintaxe também permite o uso de modificadores para um controle mais fino sobre a formatação, especialmente com números.

- **Precisão de Ponto Flutuante (`%.<n>f`):** Permite limitar o número de casas decimais de um `float`, realizando o arredondamento.
    
    ```python
    pi = 3.14159265
    texto_pi = "O valor de pi com duas casas decimais é %.2f." % pi
    print(texto_pi) # Saída: O valor de pi com duas casas decimais é 3.14.
    ```
    
- **Preenchimento e Alinhamento de Inteiros (`%<n>d`):** Garante que o número ocupe um número mínimo de `n` caracteres, preenchendo com espaços à esquerda se necessário.
    
    ```python
    id_produto = 7
    id_formatado = "ID do Produto: [%5d]" % id_produto
    print(id_formatado) # Saída: ID do Produto: [    7] (4 espaços + o número 7)
    ```
    
- **Preenchimento com Zeros (`%0<n>d`):** Similar ao anterior, mas preenche com zeros à esquerda em vez de espaços. É muito útil para formatar códigos, números de série, etc.
    
    ```python
    numero_nota_fiscal = 42
    nota_formatada = "NF Nº %08d" % numero_nota_fiscal
    print(nota_formatada) # Saída: NF Nº 00000042
    ```

Embora funcional e ainda encontrada em muitos códigos legados, a formatação com o operador `%` é considerada obsoleta. Ela pode se tornar confusa e propensa a erros quando há muitos placeholders, e é menos flexível que as abordagens mais modernas que o Python oferece.

### F-Strings

Introduzidas no Python 3.6, as **f-strings** (ou _formatted string literals_) revolucionaram a maneira de formatar strings na linguagem. Elas oferecem uma sintaxe mais concisa, mais legível e, em geral, mais performática do que as abordagens anteriores. Hoje, são consideradas a forma padrão e recomendada para a maioria das tarefas de formatação.

A sintaxe de uma f-string é imediatamente reconhecível: a string é prefixada com a letra `f` (ou `F`), e qualquer variável ou expressão a ser inserida é colocada diretamente dentro de um par de chaves `{}`.

Vamos revisitar nosso exemplo anterior, agora utilizando uma f-string:

```python
nome = 'Alice'
idade = 30

# Usando uma f-string para embutir as variáveis diretamente
string_formatada = f'Nome: {nome}, Idade: {idade}'
print(string_formatada)
# Saída: Nome: Alice, Idade: 30
```

A clareza é instantânea. Não há necessidade de placeholders ou de uma tupla de valores no final. As variáveis são inseridas exatamente onde aparecem, tornando a intenção do código muito mais explícita.

#### Expressões Embutidas

A grande vantagem das f-strings é que elas não se limitam a substituir variáveis. É possível embutir **qualquer expressão Python válida** dentro das chaves, e o resultado será calculado e inserido na string. Isso abre um leque de possibilidades, permitindo a execução de operações matemáticas, chamadas de função e muito mais, diretamente no local da formatação.

```python
a = 10
b = 5

# Realizando operações matemáticas dentro da f-string
print(f'A soma de {a} e {b} é {a + b}.')
# Saída: A soma de 10 e 5 é 15.

print(f'O dobro de {a} é {a * 2}.')
# Saída: O dobro de 10 é 20.

# Usando chamadas de função e métodos
nome = "carolina"
print(f"Olá, {nome.title()}! O seu nome tem {len(nome)} letras.")
# Saída: Olá, Carolina! O seu nome tem 8 letras.
```

#### Opções de Formatação Avançada

Assim como a sintaxe antiga, as f-strings oferecem um "minilinguagem" de formatação que permite um controle preciso sobre a aparência do valor inserido. Essa especificação é adicionada dentro das chaves, após o nome da variável, separada por dois-pontos (`:`).

Vamos nos concentrar na formatação de números de ponto flutuante, que é uma das mais utilizadas. A sintaxe `{variavel:.<precisao>f}` permite controlar o número de casas decimais.

```python
pi = 3.14159265

# Formatando pi com diferentes níveis de precisão
print(f"Pi com 2 casas decimais: {pi:.2f}") # Saída: Pi com 2 casas decimais: 3.14
print(f"Pi com 4 casas decimais: {pi:.4f}") # Saída: Pi com 4 casas decimais: 3.1416
```

É crucial entender que a formatação realiza um **arredondamento** matemático padrão, não um simples truncamento (corte). Se o próximo dígito for 5 ou maior, o último dígito exibido será arredondado para cima.

Vejamos um exemplo claro dessa regra em ação:

```python
valor_a = 33.43948
valor_b = 33.43962

# Arredondando para 3 casas decimais
print(f"Valor A formatado: {valor_a:.3f}")
# O 4º dígito (4) é menor que 5, então não há arredondamento para cima.
# Saída: Valor A formatado: 33.439

print(f"Valor B formatado: {valor_b:.3f}")
# O 4º dígito (6) é maior que 5, então o último dígito (9) é arredondado para cima,
# o que causa um efeito cascata.
# Saída: Valor B formatado: 33.440
```

As f-strings combinam o melhor de dois mundos: a simplicidade de embutir variáveis diretamente no texto e o poder de uma sintaxe de formatação robusta, tornando-as a ferramenta de escolha para a composição de strings no Python moderno.

### Método `.format()`

Antes da introdução das f-strings, o método `str.format()` foi a principal evolução na formatação de strings, introduzido no Python 2.6. Ele representa uma abordagem mais estruturada e poderosa do que o antigo operador `%`, oferecendo maior legibilidade e flexibilidade. Embora as f-strings sejam hoje a escolha preferencial, o método `.format()` ainda é amplamente utilizado e extremamente útil, especialmente em situações onde o gabarito da string precisa ser definido antes que os valores a serem inseridos estejam disponíveis.

A sintaxe do `.format()` também utiliza chaves `{}` como placeholders dentro da string. Após a string, o método é chamado, e os valores a serem substituídos são passados como argumentos para o método.

#### Formatação Posicional

Na sua forma mais simples, os placeholders vazios são substituídos pelos argumentos do método `.format()` na ordem em que são passados.

```python
nome = 'Alice'
idade = 30

# Os placeholders {} são preenchidos sequencialmente com 'nome' e 'idade'
string_formatada = 'Meu nome é {} e tenho {} anos.'.format(nome, idade)
print(string_formatada)
# Saída: Meu nome é Alice e tenho 30 anos.
```

Esta abordagem já é mais clara que a do operador `%`, pois não exige a especificação do tipo de dado no placeholder.

Para um controle maior, podemos numerar os placeholders. Isso nos permite referenciar os argumentos por sua posição (índice), o que é extremamente útil para reordenar a saída ou repetir o mesmo valor várias vezes sem passá-lo novamente como argumento.

```python
# Usando placeholders numerados {0}, {1}, etc.
item = "computador"
preco = 3500.00

# Reutilizando e reordenando os argumentos
relatorio = 'O item {0} tem o preço de R$ {1:.2f}. Repito, o preço do {0} é {1:.2f}.'.format(item, preco)
print(relatorio)
# Saída: O item computador tem o preço de R$ 3500.00. Repito, o preço do computador é 3500.00.
```

#### Formatação por Nome

Uma das melhorias mais significativas do `.format()` é a capacidade de nomear os placeholders. Isso torna a string muito mais legível, pois o nome dentro da chave descreve o propósito do valor a ser inserido. Os valores são então passados para o método `.format()` como argumentos nomeados.

```python
# Usando placeholders nomeados
template = "Prezado(a) {nome_cliente}, sua fatura no valor de R$ {valor} vence no dia {dia_vencimento}."

# Passando os valores pelos nomes correspondentes
fatura = template.format(nome_cliente="Carlos Silva", valor=150.75, dia_vencimento=10)
print(fatura)
# Saída: Prezado(a) Carlos Silva, sua fatura no valor de R$ 150.75 vence no dia 10.
```

Essa abordagem desacopla a ordem dos argumentos da sua posição na string, resultando em um código mais robusto e de fácil manutenção.

#### Opções de Formatação

Assim como nas f-strings, o método `.format()` suporta a mesma "minilinguagem" de formatação dentro das chaves, separada por dois-pontos. Isso permite um controle detalhado sobre alinhamento, preenchimento e formatação numérica.

Vamos revisitar o exemplo do `pi`:

```python
pi = 3.14159265

# A sintaxe de formatação {:.2f} é colocada dentro do placeholder
texto_pi = "O valor de pi com duas casas decimais é {:.2f}.".format(pi)
print(texto_pi)
# Saída: O valor de pi com duas casas decimais é 3.14.
```

O método `.format()` representa um meio-termo poderoso entre a sintaxe antiga do `%` e a moderna das f-strings. Sua capacidade de separar a definição do gabarito da passagem dos valores o mantém relevante em muitos contextos de programação.

## Operações Fundamentais com Strings

Além da formatação, a verdadeira flexibilidade no trabalho com texto vem da capacidade de combinar, repetir e extrair partes de strings. O Python trata as strings como **sequências**, o que significa que elas compartilham muitas das mesmas operações que vimos em listas e tuplas, como a indexação e o fatiamento, tornando a manipulação de texto uma tarefa consistente e intuitiva.

### Concatenação

A concatenação é a operação de juntar duas ou mais strings para formar uma nova. Em Python, o operador de adição (`+`) é sobrecarregado para realizar essa tarefa. Quando usado entre duas strings, ele as une em uma única cadeia de caracteres.

```python
# Concatenação de strings
primeiro_nome = "Carlos"
sobrenome = "Drummond"
espaco = " "

# Unindo as partes para formar o nome completo
nome_completo = primeiro_nome + espaco + sobrenome
print(nome_completo) # Saída: Carlos Drummond
```

É importante lembrar da tipagem forte do Python: o operador `+` só funciona entre strings. Tentar concatenar uma string com um número diretamente resultará em um `TypeError`. A conversão do número para string deve ser feita explicitamente com a função `str()`.

```python
prefixo = "ID do Produto: "
id_produto = 5043

# A linha abaixo causaria um erro: print(prefixo + id_produto)
# Correto: converter o número para string antes de concatenar
identificador_final = prefixo + str(id_produto)
print(identificador_final) # Saída: ID do Produto: 5043
```

### Repetição

De forma análoga à concatenação, o operador de multiplicação (`*`) também possui um comportamento especial com strings. Quando uma string é multiplicada por um número inteiro `n`, o resultado é uma nova string que contém a original repetida `n` vezes. Esta é uma forma rápida e legível de criar strings repetitivas, como separadores ou preenchimentos.

```python
# Repetição de strings
separador = "-=" * 20
titulo = " RELATÓRIO FINAL "
linha_simples = "_" * len(titulo) # Repete o '_' pelo número de caracteres do título

print(separador)
print(titulo)
print(linha_simples)
```

A saída seria:

```
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
 RELATÓRIO FINAL 
_________________
```

### Indexação e Fatiamento (Slicing)

Como as strings são sequências de caracteres, cada caractere ocupa uma posição específica, ou **índice**. Assim como nas listas, a indexação em strings começa em **0**. Isso nos permite acessar qualquer caractere individualmente. É crucial lembrar que os espaços em branco e outros símbolos também contam como caracteres e possuem seus próprios índices.

```python
frase = "Python é legal"
# P  y  t  h  o  n     é     l  e  g  a  l
# 0  1  2  3  4  5  6  7  8  9 10 11 12 13

# Acessando caracteres individuais
print(frase[0])  # Saída: P
print(frase[7])  # Saída: é
print(frase[6])  # Saída:  (espaço em branco)
```

#### Indexação Negativa

O Python também oferece uma forma conveniente de acessar elementos a partir do **final** da string, utilizando índices negativos. O índice `-1` se refere ao último caractere, `-2` ao penúltimo, e assim por diante.

```python
frase = "Python é legal"
#  P   y   t   h   o   n       é       l   e   g   a   l
#-14 -13 -12 -11 -10  -9  -8  -7  -6  -5  -4  -3  -2  -1

# Acessando de trás para frente
print(frase[-1]) # Saída: l (o último caractere)
print(frase[-5]) # Saída: e
```

#### Fatiamento (Slicing)

Frequentemente, precisamos extrair não apenas um caractere, mas uma subsequência de caracteres — uma "fatia" da string original. O fatiamento é realizado com a sintaxe `[inicio:fim]`.

Seguindo a convenção do Python, o intervalo é **semiaberto**: o caractere no índice `inicio` é **incluído**, mas o caractere no índice `fim` é **excluído**.

```python
texto = "Linguagem de Programação"

# Extraindo a palavra "Linguagem"
# Começa no índice 0 e vai até o 8 (o caractere no índice 9, ' ', não é incluído)
palavra1 = texto[0:9]
print(palavra1) # Saída: Linguagem

# Extraindo a palavra "Programação"
# Começa no índice 13 e vai até o final
palavra2 = texto[13:25]
print(palavra2) # Saída: Programação
```

O Python oferece alguns atalhos para o fatiamento:

- `[:fim]`: Se o início for omitido, a fatia começa do índice 0.
- `[inicio:]`: Se o fim for omitido, a fatia vai até o último caractere.
- `[:]`: Se ambos forem omitidos, uma cópia da string inteira é criada.

```python
texto = "Linguagem de Programação"

print(texto[:9])   # Saída: Linguagem (do início até o índice 8)
print(texto[13:])  # Saída: Programação (do índice 13 até o final)
```

O fatiamento também funciona perfeitamente com índices negativos, permitindo combinações poderosas.

```python
codigo = "BR-PROD-00123-SP"

# Extrair apenas o número do produto (os 5 caracteres antes dos últimos 3)
numero_produto = codigo[-8:-3]
print(numero_produto) # Saída: 00123
```


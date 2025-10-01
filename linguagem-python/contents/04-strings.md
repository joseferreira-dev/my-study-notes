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


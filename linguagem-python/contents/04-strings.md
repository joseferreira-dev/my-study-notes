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


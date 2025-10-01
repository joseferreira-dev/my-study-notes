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


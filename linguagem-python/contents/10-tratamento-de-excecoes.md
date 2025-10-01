# Capítulo 10 – Tratamento de Exceções

Até agora, focamos em escrever o "caminho feliz" de nossos programas — a sequência de passos que devem ser executados quando tudo ocorre como o esperado. No entanto, o mundo real é imprevisível. O que acontece se nosso programa tentar ler um arquivo que não existe, se conectar a uma internet que caiu, ou dividir um número por zero? Sem um plano de contingência, qualquer um desses erros, ou **exceções**, faria com que nosso programa fosse interrompido abruptamente, resultando em uma péssima experiência para o usuário e na potencial perda de dados.

O **tratamento de exceções** é o mecanismo que nos permite antecipar e gerenciar esses erros de forma controlada. Em vez de deixar o programa "quebrar", podemos capturar a exceção, executar um bloco de código alternativo para lidar com o problema — seja exibindo uma mensagem amigável, registrando o erro para análise posterior ou tentando uma outra abordagem — e permitir que o programa continue sua execução de maneira estável. Neste capítulo, vamos dominar a sintaxe `try...except` do Python e suas cláusulas complementares, `else` e `finally`, ferramentas essenciais para a construção de software resiliente e profissional.

## Estrutura `try...except`

A base do tratamento de exceções em Python é o bloco `try...except`. A ideia é simples: colocamos o código que pode ser propenso a erros dentro de um bloco `try` e, em seguida, definimos um ou mais blocos `except` para capturar e tratar os tipos específicos de erros que podem ocorrer.

A sintaxe geral, que pode ser expandida com outras cláusulas, é:

```python
try:
    # Código que pode gerar uma exceção (o "caminho feliz")
except TipoDeExcecao1:
    # Bloco de código a ser executado se a Exceção do Tipo 1 ocorrer
except TipoDeExcecao2:
    # Bloco de código a ser executado se a Exceção do Tipo 2 ocorrer
# ...
```

### Bloco `try`

O bloco `try` é a sua "zona de teste". É aqui que você coloca o código que, sob certas condições, pode levantar uma exceção. Se uma exceção ocorrer em qualquer ponto dentro do `try`, a execução normal daquele bloco é **imediatamente interrompida**, e o Python começa a procurar por um bloco `except` que corresponda ao tipo de erro que ocorreu.

### Bloco `except`

Cada bloco `except` é como uma "rede de segurança" especializada. Ele especifica o tipo de exceção que está preparado para capturar. Se a exceção levantada no bloco `try` corresponder ao tipo definido no `except`, o código dentro daquele `except` é executado.

Vamos a um exemplo clássico e prático: um programa que pede ao usuário um número e calcula a divisão de 10 por ele. Dois erros comuns podem acontecer aqui:

1. O usuário pode digitar um texto em vez de um número (`ValueError`).
2. O usuário pode digitar o número zero (`ZeroDivisionError`).

```python
try:
    entrada = input("Digite um número para dividir 10: ")
    numero = int(entrada)
    resultado = 10 / numero

except ValueError:
    print("Erro: A entrada fornecida não é um número válido. Por favor, digite apenas dígitos numéricos.")

except ZeroDivisionError:
    print("Erro: Não é possível realizar uma divisão por zero. Por favor, escolha outro número.")

print("O programa continua após o tratamento...")
```

Analisando o fluxo:

- **Se o usuário digitar `5`**: O `int()` funciona, a divisão `10 / 5` é calculada, nenhum erro ocorre, os blocos `except` são ignorados e o programa continua.
- **Se o usuário digitar `texto`**: A função `int("texto")` falha e levanta um `ValueError`. O Python interrompe o `try`, encontra o `except ValueError:`, executa o `print` correspondente e continua a execução após a estrutura.
- **Se o usuário digitar `0`**: O `int(0)` funciona, mas a operação `10 / 0` levanta um `ZeroDivisionError`. O Python interrompe, pula o `except ValueError`, encontra o `except ZeroDivisionError:`, executa seu código e continua.

Sem o tratamento `try...except`, qualquer uma dessas entradas inválidas teria encerrado o programa com uma mensagem de erro vermelha e intimidadora. Com o tratamento, o programa se recupera graciosamente.

## Cláusulas Opcionais: `else` e `finally`

A estrutura de tratamento de exceções pode ser enriquecida com duas cláusulas opcionais que nos dão um controle ainda mais refinado sobre o fluxo de execução.

### Cláusula `else`

O bloco `else` é executado **somente se o bloco `try` for concluído com sucesso**, ou seja, se nenhuma exceção for levantada. Ele é útil para separar o código que deve rodar apenas no "caminho feliz" do código que está sendo monitorado por exceções. Isso torna a intenção do código mais clara.

Continuando nosso exemplo, a exibição do resultado só faz sentido se a divisão foi bem-sucedida. O bloco `else` é o lugar perfeito para isso.

```python
try:
    entrada = input("Digite um número: ")
    numero = int(entrada)
    resultado = 10 / numero

except ValueError:
    print("Erro: Você deve digitar um número válido.")

except ZeroDivisionError:
    print("Erro: Não é possível dividir por zero.")

else:
    # Este bloco só executa se o 'try' foi bem-sucedido
    print(f"O resultado da divisão é {resultado}.")
```

### Cláusula `finally`

O bloco `finally` é especial: seu código será executado **sempre**, não importa o que aconteça. Ele roda se o `try` for bem-sucedido, se uma exceção for capturada por um `except`, ou até mesmo se uma exceção não tratada ocorrer e o programa for encerrado.

Sua principal finalidade é para **ações de limpeza** (_cleanup_), garantindo que recursos importantes sejam liberados, como fechar um arquivo que foi aberto ou encerrar uma conexão com um banco de dados, independentemente de erros terem ocorrido durante o processamento.

Vamos completar nosso exemplo com um bloco `finally`.

```python
try:
    entrada = input("Digite um número: ")
    numero = int(entrada)
    resultado = 10 / numero

except ValueError:
    print("Erro: Você deve digitar um número válido.")

except ZeroDivisionError:
    print("Erro: Não é possível dividir por zero.")

else:
    print(f"O resultado da divisão é {resultado}.")

finally:
    # Este bloco executa sempre, no final de tudo
    print("--- Fim da tentativa de cálculo ---")
```

Se o usuário digitar `5`, a saída será:

```
O resultado da divisão é 2.0.
--- Fim da tentativa de cálculo ---
```

Se o usuário digitar `0`, a saída será:

```
Erro: Não é possível dividir por zero.
--- Fim da tentativa de cálculo ---
```

A cláusula `finally` garante que a mensagem final seja sempre impressa, concluindo a operação de forma consistente.
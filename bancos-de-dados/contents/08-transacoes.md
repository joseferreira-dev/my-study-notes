# Capítulo 8 – Transações em Bancos de Dados

Nos capítulos anteriores, focamos em como definir, manipular e consultar dados. Agora, vamos explorar o mecanismo que garante que todas essas operações ocorram de forma segura, consistente e confiável, especialmente em ambientes complexos com múltiplos usuários e a possibilidade de falhas: a **transação**. As transações são o coração pulsante de um banco de dados relacional, o processo que garante que os dados passem de um estado válido para outro sem erros, corrupção ou perda de informação.

## Conceitos Gerais

Uma **transação**, no contexto de bancos de dados, é uma sequência de uma ou mais operações que é executada como uma **única unidade lógica e indivisível de trabalho**. O princípio fundamental de uma transação é o "tudo ou nada": ou todas as operações dentro dela são concluídas com sucesso, ou nenhuma delas é efetivada, e o banco de dados retorna ao estado em que se encontrava antes de a transação começar.

O objetivo principal das transações é garantir a consistência e a integridade dos dados, mesmo em cenários de acesso concorrente (múltiplos usuários operando ao mesmo tempo) ou em caso de falhas de sistema, software ou hardware.

As operações contidas em uma transação são, tipicamente, os comandos DML que formam o acrônimo **CRUD** — Create (Criar), Read (Ler), Update (Atualizar) e Delete (Remover). Uma transação pode ser tão simples quanto um único `UPDATE` ou tão complexa quanto uma série de `INSERT`s, `UPDATE`s e `DELETE`s que representam uma completa operação de negócio.

<div align="center">
<img width="580px" src="./img/08-crud.png">
</div>

### Exemplo de uma Transação de Negócio

Vamos analisar um exemplo prático de uma transação em SQL que registra uma nova venda em um sistema de e-commerce. Esta única operação de negócio requer três passos no banco de dados:

```sql
BEGIN TRANSACTION;

-- Passo 1: Inserir o cabeçalho do pedido na tabela de Pedidos
INSERT INTO Pedidos (id_pedido, id_cliente, data_pedido, valor_total)
VALUES (1001, 200, '2025-04-07', 350.00);

-- Passo 2: Inserir os itens comprados na tabela Itens_Pedido
INSERT INTO Itens_Pedido (id_pedido, id_produto, quantidade, preco_unitario)
VALUES (1001, 501, 2, 175.00); -- O usuário comprou 2 unidades de R$175.00 e não R$100.00 como antes.

-- Passo 3: Atualizar o estoque do produto vendido na tabela Produtos
UPDATE Produtos
SET quantidade_estoque = quantidade_estoque - 2
WHERE id_produto = 501;

COMMIT;
```

Neste exemplo, as palavras-chave `BEGIN TRANSACTION` e `COMMIT` delimitam a unidade de trabalho. Se qualquer um dos três passos falhar (por exemplo, se o `UPDATE` no estoque não for possível por falta de produto), toda a transação seria desfeita, e o `INSERT` em `Pedidos` e `Itens_Pedido` seria revertido. Para entender por que e como isso funciona, precisamos primeiro explorar os princípios que governam todas as transações.

## Princípios ACID

**ACID** é um acrônimo que representa quatro propriedades fundamentais que garantem a confiabilidade das transações em um SGBD: **A**tomicidade, **C**onsistência, **I**solamento e **D**urabilidade.

<div align="center">
<img width="580px" src="./img/08-principios-acid.png">
</div>

Esses princípios formam um "contrato" entre o SGBD e a aplicação, uma promessa de que os dados serão mantidos de forma íntegra e previsível, mesmo diante de erros ou acessos simultâneos. A aderência a estas propriedades é o que distingue um SGBD transacional robusto.

O uso dos princípios ACID é indispensável para manter a integridade dos dados em qualquer sistema onde a precisão e a confiabilidade são críticas, como em sistemas financeiros (transferências bancárias), comerciais (controle de estoque), de saúde (prontuários de pacientes) ou governamentais. A ausência dessas garantias poderia levar à corrupção de dados, inconsistências, perda de informações e falhas severas na operação das aplicações. Portanto, os princípios ACID são a base sobre a qual a confiança nos sistemas de banco de dados é construída.

Vamos agora explorar mais profundamente o significado de cada um desses quatro princípios.


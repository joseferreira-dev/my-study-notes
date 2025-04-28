# Capítulo 4 — Estruturas de Dados — Parte 1: Vetores

## 4.1 O que são Vetores?

Em R, vetores são a forma mais fundamental de armazenar dados. Todo objeto simples em R — números, textos, valores lógicos — é, internamente, um vetor.  
Um vetor é uma **coleção ordenada de elementos** do **mesmo tipo atômico**: numérico (`numeric`), inteiro (`integer`), caractere (`character`), lógico (`logical`) ou complexo (`complex`).

Essa homogeneidade é obrigatória: **todos os elementos de um vetor devem ser do mesmo tipo**. Caso contrário, o R fará uma **coerção automática**, ou seja, converterá todos os elementos para o tipo mais abrangente.

Os vetores permitem operações **vetorializadas**, ou seja, as operações são aplicadas a todos os seus elementos de forma automática, sem necessidade de loops explícitos. Essa característica torna R altamente eficiente para manipulação de dados e cálculos estatísticos.

---

## 4.2 Criação de Vetores

A maneira mais direta de criar vetores em R é com a função `c()`, que significa *combine*.

```r
# Criando vetores de diferentes tipos
vetor_numerico <- c(2, 4.5, 6)
vetor_inteiro <- c(1L, 2L, 3L)
vetor_logico <- c(TRUE, FALSE, TRUE)
vetor_caractere <- c("R", "é", "poderoso")
vetor_complexo <- c(1+2i, 3+4i)
```

> Importante: O sufixo `L` em `1L`, `2L`, etc., indica que os valores são tratados como inteiros, e não como numéricos de ponto flutuante.

### Verificando o tipo de um vetor

Use `typeof()` ou `class()`:

```r
typeof(vetor_numerico)   # "double"
class(vetor_numerico)    # "numeric"
```

Note que vetores numéricos em R são do tipo `double` (ponto flutuante), mas sua classe é chamada `numeric`.

---

## 4.3 Conversão de Tipos em Vetores

Quando elementos de tipos diferentes são combinados, o R realiza uma **coerção implícita** seguindo a hierarquia:

```
logical → integer → double → character → list
```

Exemplos de coerção automática:

```r
c(TRUE, 2)           # Resultado: 1, 2  (TRUE foi convertido para 1)
c(3.14, "texto")     # Resultado: "3.14", "texto" (número convertido para texto)
c(FALSE, "TRUE")     # Resultado: "FALSE", "TRUE"
```

### Conversão explícita

Para controlar o tipo de vetor, podemos converter explicitamente:

```r
as.numeric(c(TRUE, FALSE))  # 1 0
as.character(c(1, 2, 3))    # "1" "2" "3"
as.logical(c(1, 0, 1))      # TRUE FALSE TRUE
```

Essas funções garantem que os dados estejam no formato correto antes de realizar operações.

---

## 4.4 Concatenando Vetores

Concatenar significa combinar vários elementos em um vetor único. A função `c()` pode ser usada não apenas para criar, mas também para **unir vetores existentes**:

```r
a <- c(1, 2, 3)
b <- c(4, 5)
c(a, b)  # Resultado: 1 2 3 4 5
```

Exemplos de concatenação:

```r
c(2, 3, 4, 5)
c(TRUE, FALSE, TRUE, FALSE)
c("Eu", "AMO", "A", "RURAL")
c(2:4) * 2  # Vetor multiplicado elemento a elemento: 4 6 8
```

> Quando usamos `2:4`, criamos uma sequência de 2 até 4, ou seja, 2, 3, 4.

---

## 4.5 Repetição de Elementos

A função `rep()` gera vetores repetindo elementos, vetores ou padrões.

Principais formas:

```r
v <- c(1, 2, 3)

rep(v, 3)            # Repete todo o vetor 3 vezes: 1 2 3 1 2 3 1 2 3
rep(c(1, 2, 3), 1:3) # Repete 1 uma vez, 2 duas vezes, 3 três vezes: 1 2 2 3 3 3
rep(c(1, 2, 3), each = 3) # Repete cada elemento 3 vezes: 1 1 1 2 2 2 3 3 3
```

Parâmetros importantes:

- `times`: quantas vezes repetir o vetor ou elemento.
- `each`: número de vezes que cada elemento é repetido antes de passar ao próximo.
- `length.out`: força o vetor repetido a ter um comprimento exato.

Exemplo usando `length.out`:

```r
rep(1:3, length.out = 7) # Resultado: 1 2 3 1 2 3 1
```

---

## 4.6 Criação de Sequências

Sequências são muito usadas em loops, criação de índices ou geração de dados.

Duas formas principais:

**1. Usando o operador `:`**

```r
1:5  # Resultado: 1 2 3 4 5
5:1  # Resultado: 5 4 3 2 1
```

**2. Usando `seq()`**

Permite controlar o passo (`by`) ou definir o número de elementos (`length.out`):

```r
seq(1, 5, by = 0.5)     # 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0
seq(2, 10, length.out = 5)  # 2 4 6 8 10
```

> Dica: `seq_along(x)` cria uma sequência de 1 até o comprimento de `x`, muito útil para percorrer vetores.

---

## 4.7 Arredondamento de Valores

Para arredondar valores em vetores:

```r
x <- c(2.71828, 3.14159)
round(x, digits = 2)  # Resultado: 2.72, 3.14
```

Além de `round()`, existem outras funções:

- `ceiling(x)`: arredonda para cima.
- `floor(x)`: arredonda para baixo.
- `trunc(x)`: corta a parte decimal, sem arredondar.

Exemplos:

```r
ceiling(2.3)  # Resultado: 3
floor(2.7)    # Resultado: 2
trunc(-2.7)   # Resultado: -2
```

---

## 4.8 Análise de Vetores: Tamanho e Estatísticas

### Comprimento do vetor

```r
length(c(10, 20, 30))  # Resultado: 3
```

Saber o comprimento é crucial para laços e validações.

### Valores máximo, mínimo e média

```r
x <- c(5, 10, 15)
max(x)   # 15
min(x)   # 5
mean(x)  # 10
```

Essas funções são otimizadas e lidam com grandes volumes de dados rapidamente.

---

## 4.9 Ordenando Vetores

Ordenação é feita usando:

```r
sort(c(5, 2, 8, 1))  # Resultado: 1 2 5 8
sort(c(5, 2, 8, 1), decreasing = TRUE)  # 8 5 2 1
```

Para obter as posições dos elementos ordenados:

```r
order(c(5, 2, 8, 1))  # Índices que ordenariam o vetor: 4 2 1 3
```

---

## 4.10 Operações Aritméticas com Vetores

R aplica as operações elemento a elemento:

```r
a <- c(1, 2, 3)
b <- c(4, 5, 6)

a + b  # 5 7 9
a - b  # -3 -3 -3
a * b  # 4 10 18
a / b  # 0.25 0.4 0.5
```

Se os vetores forem de tamanhos diferentes, o R **recicla** o vetor menor. Se a reciclagem não for múltipla perfeita, um aviso é emitido.

---

## 4.11 Acesso e Indexação de Vetores

### Acesso numérico

```r
v <- c(100, 200, 300, 400)
v[1]    # 100
v[c(1, 3)]  # 100 300
```

Índices negativos excluem elementos:

```r
v[-2]   # Exclui o segundo elemento
```

### Acesso lógico

```r
v[v > 250]  # Retorna elementos maiores que 250
```

Combinações lógicas:

```r
v[v > 150 & v < 400]
v[v < 200 | v > 350]
```

### Uso de `%in%`

```r
v %in% c(100, 300)  # TRUE FALSE TRUE FALSE
```

### Uso de `which()`

```r
which(v > 250)  # Índices dos elementos maiores que 250
```

---

## 4.12 Contagens e Porcentagens

Contar:

```r
sum(v > 150)  # Quantos elementos > 150
```

Proporção:

```r
mean(v > 150) # Proporção de elementos > 150
```

---

## 4.13 Modificação de Vetores

Alterar valores:

```r
v[2] <- 250
```

Excluir elementos (cria uma nova cópia):

```r
v <- v[-1]
```

---

## 4.14 Vetores Vazios e Valores Faltantes

Criar vetor vazio:

```r
vector("numeric", 5)  # Cria vetor numérico de tamanho 5 preenchido com 0
```

Identificar valores ausentes:

```r
is.na(c(1, NA, 3))
```

Remover `NA`:

```r
na.omit(c(1, NA, 3))
```

---

## 4.15 Padronização de Vetores (Z-Score)

A padronização é feita transformando os dados para que tenham média 0 e desvio padrão 1:

```r
x <- c(10, 20, 30, 40, 50)
x_padronizado <- (x - mean(x)) / sd(x)
```

Interpretando:

- Um valor padronizado de 0 indica que o dado é igual à média.
- Valores positivos estão acima da média, valores negativos abaixo.

Essa técnica é crucial para análise estatística multivariada.

---

# Conclusão

Neste capítulo, exploramos todos os aspectos dos vetores em R com profundidade e detalhes. Entendemos como criá-los, manipulá-los, realizar operações matemáticas, acessar e modificar seus elementos, e até mesmo padronizá-los para análises estatísticas mais complexas.  

Dominar vetores é essencial para qualquer programador ou analista de dados que trabalha com R, e serve de base para estruturas mais avançadas, como matrizes, listas e data frames.

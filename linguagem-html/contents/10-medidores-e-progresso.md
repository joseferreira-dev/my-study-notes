# Capítulo 10 – Medidores e Progresso

A web moderna exige que comuniquemos informações de forma cada vez mais visual e intuitiva. Além de simplesmente exibir dados estáticos, muitas vezes precisamos representar medidas, como o uso de espaço em disco, a força de uma senha, ou o status de uma tarefa em andamento, como um upload de arquivo. Para esses cenários, o HTML5 nos fornece dois elementos semânticos específicos e poderosos: `<meter>` e `<progress>`.

Neste capítulo, vamos explorar em detalhe essas duas ferramentas. Primeiramente, mergulharemos no elemento **`<meter>`**, aprendendo como usá-lo para criar "medidores" ou "gauges" que representam um valor estático dentro de uma escala. Em seguida, revisitaremos o elemento **`<progress>`**, focando em seu papel para indicar o andamento de tarefas dinâmicas. Discutiremos os atributos específicos de cada um, como associá-los a rótulos para garantir a acessibilidade e, mais importante, cimentaremos a distinção conceitual entre eles.

Compreender quando usar `<meter>` e quando usar `<progress>` é um sinal de maturidade no desenvolvimento HTML, pois demonstra uma preocupação com a semântica correta, o que resulta em um código mais claro, acessível e profissional.

## Elemento `<meter>`

O elemento `<meter>` é usado para representar uma medida escalar estática dentro de um intervalo conhecido, ou um valor fracionário. Pense nele como um medidor de combustível em um carro: ele mostra uma quantidade atual (`value`) dentro de uma faixa definida (`min` e `max`).

Seu propósito semântico **não é** para indicar progresso, mas sim para exibir uma medição em um determinado ponto no tempo.

**Casos de uso comuns:**

- Uso de espaço em disco ou de memória.
- A força de uma senha escolhida pelo usuário.
- A relevância de um resultado de busca.
- A pontuação em um questionário.

### Atributos do `<meter>`

O `<meter>` é controlado por um conjunto de atributos que definem sua faixa e seu valor.

- `value`: **(Obrigatório)** O valor atual da medida.
- `min`: O limite inferior do intervalo. Se omitido, o padrão é `0`.
- `max`: O limite superior do intervalo. Se omitido, o padrão é `1`.
- `low`: O limite superior da faixa "baixa". Valores abaixo deste são considerados baixos.
- `high`: O limite inferior da faixa "alta". Valores acima deste são considerados altos.
- `optimum`: Indica o valor "ótimo" dentro do intervalo. Este atributo influencia a aparência do medidor. Se o `value` estiver na faixa considerada "ótima", o navegador geralmente o renderiza em verde. Caso contrário, pode renderizá-lo em amarelo ou vermelho.

**Exemplos Práticos:**

**1. Uso de Disco (onde valores altos são ruins):**

```html
<label for="uso-disco">Uso do Disco:</label>
<meter id="uso-disco" min="0" max="1024" value="900" low="512" high="820" optimum="256">
  900GB de 1024GB
</meter>
```

Neste caso, `value="900"` está acima do limite `high`, então o navegador provavelmente exibirá a barra em uma cor de alerta (amarelo ou vermelho).

**2. Pontuação em um Teste (onde valores altos são bons):**

```html
<label for="pontuacao-teste">Sua Pontuação:</label>
<meter id="pontuacao-teste" min="0" max="100" value="95" low="50" high="80" optimum="100">
  95 de 100
</meter>
```

Aqui, `value="95"` está na faixa "alta" e perto do `optimum`, então o navegador provavelmente exibirá a barra em verde.

## Elemento `<progress>`

Como vimos brevemente, o elemento `<progress>` tem um propósito diferente: ele representa a **conclusão de uma tarefa** que está ocorrendo ao longo do tempo. Sua natureza é dinâmica. A analogia perfeita é uma barra de download ou instalação.

O `<progress>` pode operar em dois modos:

**1. Progresso Determinado:** Usado quando sabemos o total da tarefa. Requer os atributos `value` (quanto foi concluído) e `max` (o trabalho total).

```html
<label for="upload-arquivo">Progresso do Upload:</label>
<progress id="upload-arquivo" value="78" max="100"></progress>
```

**2. Progresso Indeterminado:** Usado quando uma tarefa está em execução, mas não sabemos quanto tempo levará. Basta omitir o atributo `value`.

```html
<p>Processando, por favor aguarde...</p>
<progress></progress>
```

## Resumo da Diferença: `<meter>` vs. `<progress>`

Para evitar qualquer confusão, a tabela abaixo resume a distinção fundamental entre os dois elementos.

|**Característica**|**`<meter>` (Medidor)**|**`<progress>` (Progresso)**|
|---|---|---|
|**Propósito Semântico**|Medida estática em uma escala.|Andamento de uma tarefa dinâmica.|
|**Exemplo de Uso**|Uso de disco, força da senha.|Upload de arquivo, instalação.|
|**Estado Principal**|Valor atual (`value`) em um intervalo (`min`, `max`).|Conclusão (`value`) de um total (`max`).|
|**Estado Indefinido**|Não possui.|Sim (sem o atributo `value`).|
|**Atributos-Chave**|`value`, `min`, `max`, `low`, `high`, `optimum`.|`value`, `max`.|
|**Analogia**|Medidor de combustível.|Barra de download.|

## Acessibilidade: Importância do `<label>`

Tanto para `<meter>` quanto para `<progress>`, uma representação puramente visual não é acessível. É crucial fornecer um rótulo textual claro para que usuários de tecnologias assistivas possam entender o que o medidor ou a barra de progresso representa.

A maneira semanticamente correta de fazer isso é usando o elemento `<label>`, associando-o ao medidor/progresso através dos atributos `for` e `id`.

```html
<label for="forca-senha">Força da Senha:</label>
<meter id="forca-senha" min="0" max="4" value="3">Forte</meter>

<label for="backup-progresso">Progresso do Backup:</label>
<progress id="backup-progresso" value="50" max="100">50%</progress>
```

O texto dentro das tags (`Forte`, `50%`) serve como conteúdo de fallback para navegadores que não suportam esses elementos.

## Considerações Finais

Neste capítulo, diferenciamos duas importantes ferramentas do HTML5 para visualização de dados: `<meter>` e `<progress>`. Aprendemos que **`<meter>` é a escolha semântica para exibir medições estáticas dentro de uma faixa**, como um medidor, enquanto **`<progress>` é a ferramenta correta para comunicar o andamento de tarefas dinâmicas**.

Exploramos os atributos que controlam cada elemento, com destaque para a flexibilidade do `<meter>` com seus limites `low`, `high` e `optimum`. Reforçamos, mais uma vez, a importância da acessibilidade ao associar rótulos textuais claros com o elemento `<label>`.

Escolher entre `<meter>` e `<progress>` é um exercício de precisão semântica. Ao usar o elemento correto para o contexto correto, você cria um código mais significativo, acessível e fácil de manter, elevando a qualidade geral de suas aplicações web.
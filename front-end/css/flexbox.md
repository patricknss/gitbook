---
description: >-
  O Flexbox tem como meta ser um modo mais eficiente para criar leiautes,
  alinhar e distribuir espaços entre itens em um container, mesmo quando as
  dimensões destes itens são desconhecidas ou dinâmicas.
---

# Flexbox

A ferramenta Flexbox (de _Flexible Box_) foi criada para tornar essas tarefas mais simples e funcionais: os filhos de um elemento com Flexbox podem se posicionar em qualquer direção e pode ter dimensões flexíveis para se adaptar.

## Elementos <a href="#elementos" id="elementos"></a>

O Flexbox é um módulo completo e não uma única propriedade; algumas delas devem ser declaradas no container (o elemento-pai, que chamamos de _flex container_), enquanto outras devem ser declaradas nos elementos-filhos (os _flex itens_).

Se o leiaute "padrão" é baseado nas direções `block` e `inline`, o leiaute Flex é baseado em direções "flex flow". Veja abaixo um diagrama da especificação, explicando a ideia central por trás do leiaute Flex.

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/11/00-basic-terminology.svg" alt="" width="375"><figcaption><p>Layout</p></figcaption></figure>

Os ítens serão dispostos no leiaute seguindo ou o eixo principal ou o\
transversal.

* Eixo principal: o eixo principal de um _flex container_ é o eixo primário e ao longo dele são inseridos os _flex items_. **Cuidado**: O eixo principal não é necessariamente horizontal; vai depender da propriedade `flex-direction` (veja abaixo).
* _main-start | main-end_: os _flex items_ são inseridos dentro do container começando pelo lado _start_, indo em direção ao lado _end_.
* Tamanho principal: A largura ou altura de um _flex item_, dependendo da direção do container, é o tamanho principal do ítem. A propriedade de tamanho principal de um _flex item_ pode ser tanto `width` quanto `height`, dependendo de qual delas estiver na direção principal.
* Eixo transversal: O eixo perpendicular ao eixo principal é chamado de eixo transversal. Sua direção depende da direção do eixo principal.
* _cross-start | cross-end_: Linhas flex são preenchidas com ítens e adicionadas ao container, começando pelo lado _cross start_ do _flex container_ em direção ao lado _cross end_.
* _cross size_: A largura ou altura de um _flex item_, dependendo do que estiver na dimensão transversal, é o _cross size_ do íten. A propriedade _cross size_ pode ser tanto a largura quanto a altura do ítem, o que estiver na transversal.

**Flex container** é o elemento que envolve sua estrutura. Você define que um elemento é um Flex Container com a propriedade `display` e valores `flex` ou `inline-flex`.

```html
  <div class="flex-container">
    <div>1</div>
    <div>2</div>
    <div>3</div>
  </div>
```

```css
  .flex-container {
    display: flex;
  }
```

**Flex Item** são elementos-filhos do flex container.

**Eixos ou Axes** são as duas direções básicas que existem em um Flex Container:\
&#xNAN;_&#x6D;ain axis_, ou eixo principal, e _cross axis_, ou eixo transversal.

### Propriedades para o elemento-pai <a href="#propriedades-para-o-elemento-pai" id="propriedades-para-o-elemento-pai"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/01-container.svg" alt="container flex" width="375"><figcaption><p>Father</p></figcaption></figure>

Quando utilizamos o _Flexbox_, é muito importante saber quais propriedades são\
declaradas no elemento-pai (por exemplo, uma `div` que irá conter os elementos\
a serem alinhados) e quais serão declaradas nos elementos-filhos. Abaixo,\
seguem propriedades que devem ser declaradas utilizando o elemento-pai\
como seletor (para alinhar elementos-filhos):

**display**

Esta propriedade define um _flex container_; inline ou block dependendo dos valores passados. Coloca todos os elementos-filhos diretos num contexto Flex.

<pre class="language-css"><code class="lang-css">.container {
  display: flex; /* or inline-flex */
}

<strong>Note que a propriedade de CSS columns não tem efeito em um flex container.
</strong></code></pre>

#### **flex-direction** <a href="#flex-direction" id="flex-direction"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/flex-direction.svg" alt="flex-direction" width="375"><figcaption><p>flex-direction</p></figcaption></figure>

Estabelece o eixo principal, definindo assim a direção em que os _flex items_ são alinhados no _flex container_. O Flexbox é (com exceção de um wrapping opcional) um conceito de leiaute de uma só direção. Pense nos _flex items_ inicialmente posicionais ou em linhas horizontais ou em colunas verticais.

```css
  .flex-container {
    flex-direction: row | row-reverse | column | column-reverse;
  }
```

* `row` (padrão): esquerda para a direita em `ltr` (left to right), direita para a esquerda em `rtl` (right to left)
* `row-reverse`: direita para a esquerda em `ltr`, esquerda para a direita em `rtl`
* `column`: mesmo que `row`, mas de cima para baixo
* `column-reverse`: mesmo que `row-reverse` mas de baixo para cima

#### **flex-wrap** <a href="#flex-wrap" id="flex-wrap"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/flex-wrap.svg" alt="flex-wrap" width="375"><figcaption><p>flex-wrap</p></figcaption></figure>

Por padrão, os _flex items_ vão todos tentar se encaixar em uma só linha. Com esta propriedade você pode modificar esse comportamento e permitir que os ítens quebrem para uma linha seguinte conforme for necessário.

```css
  .flex-container {
    flex-wrap: nowrap | wrap | wrap-reverse;
  }
```

* `nowrap` (padrão): todos os _flex items_ ficarão em uma só linha
* `wrap`: os _flex items_ vão quebrar em múltiplas linhas, de cima para baixo
* `wrap-reverse`: os _flex items_ vão quebrar em múltiplas linhas de baixo para cima

#### **flex-flow** <a href="#flex-flow" id="flex-flow"></a>

A propriedade **flex-flow** é uma propriedade _shorthand_ (uma mesma declaração\
inclui vários valores relacionados a mais de uma propriedade) que inclui\
`flex-direction` e `flex-wrap`. Determina quais serão os eixos pricipal e transversal do container. O valor padrão é `row nowrap`.

```css
  .flex-container {
    flex-flow: row nowrap | row wrap | column nowrap | column wrap;
  }
```

#### **justify-content** <a href="#justify-content" id="justify-content"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/justify-content.svg" alt="justify-content" width="375"><figcaption><p>justify-content</p></figcaption></figure>

Esta propriedade define o alinhamento dos ítens ao longo do eixo principal. Ajuda a distribuir o espaço livre que sobrar no container tanto se todos os flex items em uma linha são inflexíveis, ou são flexíveis mas já atingiram seu tamanho máximo. Também exerce algum controle sobre o alinhamento de ítens quando eles ultrapassam o limite da linha.

```css
  .flex-container {
    justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
  }
```

* `flex-start` (padrão): os ítens são alinhados junto à borda de início (start) de acordo com qual for a `flex-direction` do container.
* `flex-end`: os ítens são alinhados junto à borda final (end) de acordo com qual for a `flex-direction` do container.
* `start`: os ítens são alinhados junto à borda de início da direção do `writing-mode` (modo de escrita).
* `end`: os ítens são alinhados junto à borda final da direção do `writing-mode` (modo de escrita).
* `left`: os ítens são alinhados junto à borda esquerda do container, a não ser que isso não faça sentido com o `flex-direction` que estiver sendo utilizado. Nesse caso, se comporta como `start`.
* `right`: os ítens são alinhados junto à borda direita do container, a não ser que isso não faça sentido com o `flex-direction` que estiver sendo utilizado. Nesse caso, se comporta como `start`.
* `center`: os ítens são centralizados na linha.
* `space-between`: os ítens são distribuídos de forma igual ao longo da linha; o primeiro ítem junto à borda inicial da linha, o último junto à borda final da linha.
* `space-around`: os ítens são distribuídos na linha com o mesmo espaçamento entre eles. Note que, visualmente, o espaço pode não ser igual, uma vez que todos os ítens tem a mesma quantidade de espaço dos dois lados: o primeiro item vai ter somente uma unidade de espaço junto à borda do container, mas duas unidades de espaço entre ele e o próximo ítem, pois o próximo ítem também tem seu próprio espaçamento que está sendo aplicado.
* `space-evenly`: os ítens são distribuídos de forma que o espaçamento entre quaisquer dois itens da linha (incluindo entre os ítens e as bordas) seja igual.

Também existem duas palavras-chave adicionais que você pode usar em conjunto com estes valores: `safe` e `unsafe`. Safe garante que, independente da forma que você faça esse tipo de posicionamento, não seja possível "empurrar" um elemento e fazer com que ele seja renderizado para fora da tela (por exemplo, acima do topo), de uma forma que faça com que o conteúdo seja impossível de movimentar com a rolagem da tela (o CSS chama isso de "perda de dados").

#### **align-items** <a href="#align-items" id="align-items"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/align-items.svg" alt="align-items" width="375"><figcaption><p>align-items</p></figcaption></figure>

Define o comportamento padrão de como _flex items_ são alinhados de acordo com o eixo transversal (_cross axis_). De certa forma, funciona de forma similar ao `justify-content`, porém no eixo transversal (perpendicular ao eixo principal).

```css
  .flex-container {
    align-items: stretch | flex-start | flex-end | center | baseline;
  }
```

* `stretch` (padrão): estica os ítens para preencher o container, respeitando o `min-width`/`max-width`).
* `flex-start`/ `start` / `self-start`: ítens são posicionados no início do eixo transversal. A diferença entre eles é sutil e diz respeito às regras de `flex-direction` ou `writing-mode`.
* `center`: ítens são centralizados no eixo transversal.
* `baseline`: ítens são alinhados de acordo com suas baselines.

Os modificadores `safe` e `unsafe` pode ser usados em conjunto com todas essas palavras-chave (favor conferir o suporte de cada navegador) e servem para prevenir qualquer alinhamento de elementos que faça com que o conteúdo fique inacessível (por exemplo, para fora da tela).

#### **align-content** <a href="#align-content" id="align-content"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/align-content.svg" alt="align-content" width="375"><figcaption><p>align-content</p></figcaption></figure>

Organiza as linhas dentro de um flex container quando há espaço extra no eixo transversal, similar ao modo como `justify-content` alinha ítens individuais dentro do eixo principal.

**Importante**: Esta propriedade não tem efeito quando há somente uma linha de flex items no container.

```css
  .flex-container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
  }
```

* `flex-start` / `start`: ítens alinhados com o início do container. O valor (com maior suporte dos navegadores) flex-start se guia pela flex-direction, enquanto start se guia pela direção do writing-mode.
* `flex-end` / `end`: ítens alinhados com o final do container. O valor (com maior suporte dos navegadores) flex-end se guia pela flex-direction, enquanto end se guia pela direção do writing-mode.
* `center`: ítens centralizados no container.
* `space-between`: ítens distribuídos igualmente; a primeira linha junto ao início do container e a última linha junto ao final do container.
* `space-around`: ítens distribuídos igualmente com o mesmo espaçamento entre cada linha.
* `space-evenly`: ítens distribuídos igualmente com o mesmo espaçamento entre eles.
* `stretch` (padrão): ítens em cada linha esticam para ocupar o espaço remanescente entre elas.

Os modificadores `safe` e `unsafe` pode ser usados em conjunto com todas essas palavras-chave (favor conferir o suporte de cada navegador) e servem para prevenir qualquer alinhamento de elementos que faça com que o conteúdo fique inacessível (por exemplo, para fora da tela).

### Propriedades para elementos-filhos <a href="#propriedades-para-elementos-filhos" id="propriedades-para-elementos-filhos"></a>

A seguir, veremos propriedades que devem ser declaradas tendo como seletor os elementos-filhos, ou seja:

```html
  <div class="flex-container">
    <div class="flex-item">1</div>
    <div class="flex-item">2</div>
    <div class="flex-item">3</div>
  </div>
```

Isso significa que, onde existe um elemento-pai com propriedade _flex_ (o _flex-container_), é\
possível atribuir propriedades flex específicas também para as elementos-filhos (_flex-item_).

Você pode definir as propriedades abaixo para apenas um dos elementos-filhos através de um identificador, como uma classe específica.

#### **order** <a href="#order" id="order"></a>

Determina a ordem em que os elementos aparecerão.

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/order.svg" alt="order" width="375"><figcaption><p>order</p></figcaption></figure>

Por padrão os flex items são dispostos na tela na ordem do código. Mas a propriedade `order` controla a ordem em que aparecerão no container.

```css
  .flex-item {
    order: <número>; /* o valor padrão é 0 */
  }
```

#### **flex-grow** <a href="#flex-grow" id="flex-grow"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/flex-grow.svg" alt="grow" width="375"><figcaption><p>flex-grow</p></figcaption></figure>

Define a habilidade de um flex item de crescer, caso necessário. O valor dessa propriedade é um valor numérico sem indicação de unidade, que serve para cálculo de proporção. Este valor dita a quantidade de espaço disponível no container que será ocupado pelo item.

Se todos os ítens tiverem flex-grow definido em 1, o espaço remanescente no container será distribuído de forma igual entre todos. Se um dos ítens tem o valor de 2, vai ocupar o dobro de espaço no container com relação aos outros (ou pelo menos vai tentar fazer isso).

```css
  .flex-item {
    flex-grow: <numero>; /* o valor default(padrão) é 0 */
  }
```

Valores negativos não são aceitos pela propriedade.

#### **flex-shrink** <a href="#flex-shrink" id="flex-shrink"></a>

Define a habilidade de um flex item de encolher, caso necessário.

```css
  .flex-item {
    flex-shrink: <número>; /* o valor padrão é 0 */
  }
```

Valores negativos não são aceitos pela propriedade.

**flex-basis**

Define o tamanho padrão para um elemento antes que o espaço remanescente do container seja distribuído. Pode ser um comprimento (por exemplo, 20%, 5rem, etc) ou uma palavra-chave. A palavra-chave `auto` significa "observe minhas propriedades de altura ou largura" (o que era feito pela palavra-chave `main-size`, que foi depreciada). A palavra-chave `content` significa "estabeleça o tamanho com base no conteúdo interno do ítem" - essa palavra-chave ainda não tem muito suporte, então não é fácil de ser testada, assim como suas relacionadas: `max-content`, `min-content` e `fit-content`.

```css
  .flex-item {
    flex-basis: flex-basis:  | auto; /* o valor padrão é auto */
  }
```

Com o valor de `0`, o espaço extra ao redor do conteúdo não é considerado. Com o valor de `auto`, o espaço extra é distribuído com base no valor de `flex-grow` do ítem.

#### **flex** <a href="#flex" id="flex"></a>

Esta é a propriedade _shorthand_ para `flex-grow`, `flex-shrink` e `flex-basis`, combinadas. O segundo e terceiro parâmetros (`flex-shrink` e `flex-basis`) são opcionais. O padrão é `0 1 auto`, mas se você definir com apenas um número, é equivalente a `0 1`.

```css
  .item {
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
  }
```

**É recomendado que você utilize esta propriedade&#x20;**_**shorthand**_ ao invés de definir cada uma das propriedades em separado. O _shorthand_ define os outros valores de forma inteligente.

#### **align-self** <a href="#align-self" id="align-self"></a>

<figure><img src="https://css-tricks.com/wp-content/uploads/2018/10/align-self.svg" alt="align-self" width="375"><figcaption><p>align-self</p></figcaption></figure>

Permite que o alinhamento padrão (ou o que estiver definido por `align-items`) seja sobrescrito para ítens individuais.

Por favor veja a explicação da propriedade `align-items` para entender quais são os possíveis valores.

```css
  .item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
  }
```

### Importante! <a href="#importante" id="importante"></a>

* O CSS só enxerga a hierarquia pai-filho; não vai aplicar as propriedades Flex\
  para elementos que não estejam diretamente relacionados;
* Para que as propriedades funcionem nos elementos-filhos, as pais devem ter propriedade `display: flex;`.
* As propriedades `float`, `clear` e `vertical-align` não têm efeito em flex-items.

<br>

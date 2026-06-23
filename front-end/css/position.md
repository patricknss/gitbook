# Position

**O posicionamento padrão de todo elemento no HTML é estático**, ou seja, possui o valor `position: static`, mesmo que este valor não tenha sido declarado. Todo elemento estático é posicionado alinhado pelo canto superior esquerdo no corpo do documento. Não sendo possível alterar sua posição. Esta é a posição 0 de um elemento no corpo do documento, segundo as [coordenadas do HTML](https://www.w3schools.com/graphics/canvas_coordinates.asp).

### Como posso alterar a posição de um elemento? <a href="#e-agora-como-posso-alterar-a-posicao-de-um-elemento" id="e-agora-como-posso-alterar-a-posicao-de-um-elemento"></a>

Para alterarmos a posição de um elemento é preciso primeiro modificar o seu valor que vem por padrão: static. Para a propriedade position, é possível atribuir 5 valores, que são: _static_, _fixed_, _sticky_, _relative_ e _absolute_.

Alterando o Position, podemos também utilizar outras quatro propriedades auxiliares que são: _top_, _bottom_, _left_ e _right_. Apenas o position static ignora o valor dessas propriedades. Elas podem ser utilizadas com todos os outros tipos de _positions_ possibilitando definir a posição de um elemento. As referências de topo, inferior, esquerda e direita podem mudar dependendo do tipo de _position_ aplicado.

Vamos agora entender os tipos de positions e como as propriedades auxiliares se comportam em cada caso:

### Static <a href="#static" id="static"></a>

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem1-31.gif" alt="" width="375">

O _position static_, que em português significa estático, é o mais utilizado de todos, pois, como dito acima, trata-se do valor que já vem por padrão quando criamos elementos no HTML. Seu comportamento, como o nome sugere, é ser estático. Ele apenas segue o fluxo junto dos outros elementos da página normalmente, tendo o canto superior esquerdo como referência.

Este elemento não aceita as propriedades auxiliares _top_, _bottom_, _left_ e _right_. No exemplo abaixo, mesmo aplicando o código `position: static` e `top: 20px`, o elemento não move 20 pixels do topo para baixo. Ele continua estático sendo alinhado no canto superior esquerdo. Observe:

> [https://codepen.io/luanalvesdev/pen/QWqVdQQ](https://codepen.io/luanalvesdev/pen/QWqVdQQ)

### Fixed <a href="#fixed" id="fixed"></a>

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem2-19.gif" alt="" width="375">

O _position fixed_, que em português significa fixo, faz com que o elemento que recebeu esta propriedade não se mova na tela. Mesmo que uma página tenha rolamento (scroll). Ao utilizarmos o rolamento para esquerda e direita, ou para cima e para baixo, o elemento não se move.

Veja que, quando aplicado `bottom: 0%` e `right: 5%`, o elemento está a 0% de distância da parte inferior da tela, ou seja, colado na parte debaixo, e 5% de distância do lado direito.

Teste no exemplo abaixo utilizando o rolamento e observe o comportamento:

> [https://codepen.io/luanalvess/pen/wvrExxq](https://codepen.io/luanalvess/pen/wvrExxq)

### Sticky <a href="#sticky" id="sticky"></a>

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem3-20.gif" alt="" width="375">

O _position sticky_, que em português significa pegajoso ou colado, é parecido com o fixed, porém a sua diferença é que, em vez dele ficar fixo em relação à tela, ele fica fixo em relação ao rolamento da página.

Este tipo de position é utilizado sempre em conjunto das propriedades auxiliares, pois precisa de um posicionamento de referência. Abaixo é possível conferir isso. Aplicamos `top: 0%`; e `left: 0%`, que faz com que quando utilizado o rolamento para baixo, o elemento suba, porém quando chega no limite do topo da tela, ele fica “colado”, e o mesmo acontece para o lado esquerdo, quando arrastamos a barra de rolagem para a direita, o elemento chega no limite da tela do lado esquerdo, no entanto, também fica “colado” no limite deste lado da tela. O elemento volta a ocupar a sua posição inicial quando voltamos com a barra de rolagem como antes.

Utilizando o rolamento no exemplo abaixo é possível analisar o seu comportamento:

> [https://codepen.io/luanalvess/pen/qBPMyJB](https://codepen.io/luanalvess/pen/qBPMyJB)

### Relative <a href="#relative" id="relative"></a>

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem4-16.gif" alt="" width="563">

O _position relative_, que em português significa relativo, é usado quando queremos alterar a posição de um elemento tendo como referência a posição inicial dele. Ao aplicarmos essa propriedade em um elemento, ele não muda de posição, pois já vai estar posicionado em sua posição de referência. Porém, quando aplicamos `top: 50px,` e `left: 50px` no elemento, como no exemplo abaixo, sua posição se altera 50 pixels de cima para baixo, e da esquerda para direita tendo como referência a sua posição inicial. Observe:

> [https://codepen.io/luanalvess/pen/yLzxqGx](https://codepen.io/luanalvess/pen/yLzxqGx)

### Absolute <a href="#absolute" id="absolute"></a>

_Position absolute_, ou posicionamento absoluto em português, possui dois comportamentos diferentes. O primeiro é quando o elemento com essa propriedade possui um elemento pai de valor diferente de _static_. Neste caso, ele terá este elemento pai como referência para ser posicionado. Todo elemento pai que tiver qualquer valor de _position_, menos o _static_, será a referência para posicionar o elemento filho absolute!

O segundo comportamento é quando o elemento com _position absolute_ não tem elemento pai, ou este elemento pai possui _position static_. Nesta situação, ele irá ignorar estes elementos e será posicionado a partir do canto esquerdo superior do documento, podendo até mesmo sobrepor a eles. Ou seja, é importante utilizar o _absolute_ tendo certeza da necessidade do seu uso (não que isso não seja necessário para os outros valores), pois ele pode desalinhar o layout da página.

Para entendermos na prática como funciona, temos, no próximo exemplo, o elemento pai, que é a caixa verde, sendo um _position relative_, posicionado 150 pixels do topo de sua posição inicial, e o elemento filho dele, que é a caixa azul, sendo um `position: absolute` com valores `top: 50px` e `left: 50px`. É possível observarmos que o elemento filho está tendo o canto superior do elemento pai como referência, posicionado a 50 pixels do topo e 50 pixels à esquerda dele:

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem5-18.gif" alt="" width="563">

> [https://codepen.io/luanalvess/pen/poWOZqZ](https://codepen.io/luanalvess/pen/poWOZqZ)

Já no exemplo a seguir, o elemento pai não está com o _position_ declarado, ou seja, ele é um _position static_. Seu elemento filho possui _position absolute_. Neste caso, o elemento filho ignora o posicionamento do elemento pai, estando alinhado 50 pixels do topo e do lado esquerdo do documento.

<img src="https://cdn-wcsm.alura.com.br/2025/04/imagem6-13.gif" alt="" width="563">

> [https://codepen.io/luanalvesdev/pen/wvrQRQy](https://codepen.io/luanalvesdev/pen/wvrQRQy)

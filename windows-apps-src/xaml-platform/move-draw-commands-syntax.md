---
description: Saiba mais sobre os comandos de movimentação e desenho (uma minilinguagem) que você pode usar para especificar geometrias de caminho como um valor de atributo XAML.
title: Sintaxe de comandos de movimentação e desenho
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a54f87de49e9d43bfa423b0c01b086de1f89426f
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340488"
---
# <a name="move-and-draw-commands-syntax"></a>Sintaxe de comandos de movimentação e desenho


Saiba mais sobre os comandos de movimentação e desenho (uma minilinguagem) que você pode usar para especificar geometrias de caminho como um valor de atributo XAML. Comandos de movimentação e desenho são usados por várias ferramentas de design e elementos gráficos que podem gerar uma forma ou elemento gráfico de vetor, como um formato de serialização e intercâmbio.

## <a name="properties-that-use-move-and-draw-command-strings"></a>Propriedades que usam cadeias de comandos de movimentação e desenho

A sintaxe de comandos de movimentação e desenho tem suporte em um conversor de tipos internos para XAML, que analisa os comandos e produz uma representação de elementos gráficos de tempo de execução. Essa representação é basicamente um conjunto acabado de vetores que está pronto para apresentação. Os vetores propriamente ditos não completam os detalhes de apresentação; você precisa definir outros valores nos elementos. Para um objeto [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path), você também precisa de valores para [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), [**Stroke**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.stroke) e outras propriedades e, em seguida, esse **Path** deve ser conectado à árvore visual de alguma maneira. Para um objeto [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon), defina a propriedade [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.iconelement.foreground).

Há duas propriedades no Windows Runtime que podem usar uma cadeia que representa os comandos de movimentação e desenho: [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) e [**PathIcon.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon.data). Se você definir uma dessas propriedades especificando os comandos de movimentação e desenho, geralmente a definirá como um valor de atributo XAML junto com outros atributos obrigatórios desse elemento. Sem entrar em especificidades, veja um exemplo a seguir:

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry. Figures**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures) também podem usar os comandos move e Draw. Você pode combinar um objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) que use os comandos de movimentação e desenho com outros tipos [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) em um objeto [**GeometryGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GeometryGroup), que então seria usado como o valor de [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data). Mas isso não é tão comum quanto usar os comandos de movimentação e desenho para dados definidos por atributos.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>Uso de comandos de movimentação e desenho versus uso de **PathGeometry**

Para XAML do Windows Runtime, os comandos de movimentação e desenho produzem uma [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um único objeto [**PathFigure**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathFigure) com um valor de propriedade [**Figures**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures). Cada comando de desenho produz uma classe derivada [**PathSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathSegment) na coleçãoSegments[**de**PathFigure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.segments), o comando move altera o [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.startpoint), e a existência de um comando close define [**IsClosed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) como **true**. Você pode navegar nessa estrutura como um modelo de objeto se examinar os valores de **Data** no tempo de execução.

## <a name="the-basic-syntax"></a>A sintaxe básica

A sintaxe dos comandos de movimentação e desenho pode ser resumida desta maneira:

1.  Comece com uma regre de preenchimento opcional. Geralmente isso é especificado somente se você não deseja o padrão **EvenOdd**. (Mais detalhes sobre **EvenOdd** posteriormente.)
2.  Especifique exatamente um comando de movimentação.
3.  Especifique um ou mais comandos de desenho.
4.  Especifique um comando de fechamento. Você pode omitir um comando de fechamento, mas isso deixaria a sua imagem aberta (o que é incomum).

As regras gerais dessa sintaxe são:

-   Cada comando é representado por exatamente uma letra.
-   Essa letra pode ser maiúscula ou minúscula. A formatação de maiúsculas e minúsculas, conforme iremos descrever.
-   Cada comando, com exceção do comando de fechamento, é normalmente seguido por um ou mais números.
-   Se houver mais de um número para um comando, separe-os com uma vírgula ou um espaço.

**\[** _fillRule_ **\]** _moveCommand_ _drawCommand_ **\[** _drawCommand_ **\*\]** **\[** _closeCommand_ **\]**

Muitos comandos de desenho usam pontos, nos quais você fornece um valor _x,y_. Sempre que você vir um espaço reservado de _pontos_ de \*, você pode pressupor que está dando dois valores decimais para o valor _x, y_ de um ponto.

Espaços em branco podem ser omitidos quando o resultado não é ambíguo. Na verdade, você poderá omitir todos os espaços em branco se usar vírgulas como separadores para todos os conjuntos de números (pontos e tamanho). Por exemplo, este uso é legal: `F1M0,58L2,56L6,60L13,51L15,53L6,64z`. Porém, é mais típico incluir espaços em branco entre os comandos para maior clareza.

Não use vírgulas como separador decimal para números decimais; a cadeia do comando é interpretada por XAML e não leva em consideração convenções de formatação numérica específicas de uma cultura que sejam diferentes daquelas usadas na localidade **en-us**.

## <a name="syntax-specifics"></a>Detalhes específicos da sintaxe

**Regra de preenchimento**

Há dois valores possíveis para a regra de preenchimento opcional: **F0** ou **F1**. (O **F** está sempre em maiúsculas.) **F0** é o valor padrão; ele produz o comportamento de preenchimento **EvenOdd**, por isso, você normalmente não o especifica. Use **F1** para obter o comportamento de preenchimento **Nonzero**. Esses valores de preenchimento se alinham aos valores da enumeração [**FillRule**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.FillRule).

**Mover comando**

Especifica o ponto inicial de uma nova figura.

| Sintaxe |
|--------|
| `M ` _StartPoint_ <br/>- ou -<br/>`m` _StartPoint_|

| Termo | Descrição |
|------|-------------|
| _startPoint_ | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/>O ponto inicial de uma nova figura.|

Um **M** maiúsculo indica que *startPoint* é uma coordenada absoluta; um **m** minúsculo indica que *startPoint* é um deslocamento do ponto anterior, ou (0,0) se não há ponto anterior.

**Observe**  é legal especificar vários pontos após o comando de movimentação. Uma linha é desenhada até esses pontos, como se você tivesse especificado o comando de linha. No entanto, este não é um estilo recomendado. Em vez disso, use o comando de linha dedicado.

**Comandos de desenho**

Um comando de desenho pode consistir em vários comandos de forma: linha, linha horizontal, linha vertical, curva de Bézier cúbica, curva de Bézier quadrática, curva de Bézier cúbica suave, curva de Bézier quadrática suave e arco elíptico.

Para todos os comandos de desenho, a formatação de maiúsculas e minúsculas é importante. Letras maiúsculas indicam coordenadas absolutas, enquanto letras minúsculas indicam coordenadas relativas ao comando anterior.

Os pontos de controle de um segmento são relativos ao ponto final do segmento anterior. Ao inserir sequencialmente mais de um comando do mesmo tipo, você pode omitir a entrada de comando duplicada. Por exemplo, `L 100,200 300,400` é equivalente a `L 100,200 L 300,400`.

**Comando de linha**

Cria uma linha reta entre o ponto atual e o ponto final especificado. `l 20 30` e `L 20,30` são exemplos de comandos de linha válidos. Define o equivalente a um objeto [**LineGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LineGeometry).

| Sintaxe |
|--------|
| _ponto de extremidade_ `L` <br/>- ou -<br/>_ponto de extremidade_ `l` |

| Termo | Descrição |
|------|-------------|
| endPoint | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/>O ponto final da linha.|

**Comando de linha horizontal**

Cria uma linha horizontal entre o ponto atual e a coordenada x especificada. `H 90` é um exemplo de um comando de linha horizontal válido.

| Sintaxe |
|--------|
| `H ` _x_ <br/> - ou - <br/>`h ` _x_ |

| Termo | Descrição |
|------|-------------|
| x | [**Clique**](https://docs.microsoft.com/dotnet/api/system.double) <br/> A coordenada x do ponto final da linha. |

**Comando de linha vertical**

Cria uma linha vertical entre o ponto atual e a coordenada y especificada. `v 90` é um exemplo de um comando de linha vertical válido.

| Sintaxe |
|--------|
| `V ` _y_ <br/> - ou - <br/> `v ` _y_ |

| Termo | Descrição |
|------|-------------|
| *y* | [**Clique**](https://docs.microsoft.com/dotnet/api/system.double) <br/> A coordenada y do ponto final da linha. |

**Comando de curva Bézier cúbica**

Cria uma curva de Bézier cúbica entre o ponto atual e o ponto final especificado usando os dois pontos de controle especificados (*controlPoint1* e *controlPoint2*). `C 100,200 200,400 300,200` é um exemplo de um comando de curva válido. Define o equivalente a um objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um objeto [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment).

| Sintaxe |
|--------|
| `C ` *ponto de extremidade* *ControlPoint1* *ControlPoint2* <br/> - ou - <br/> `c ` *ponto de extremidade* *ControlPoint1* *ControlPoint2* |

| Termo | Descrição |
|------|-------------|
| *controlPoint1* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O primeiro ponto de controle da curva, que determina a tangente inicial da curva. |
| *controlPoint2* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O segundo ponto de controle da curva, que determina a tangente final da curva. |
| *Extremidade* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O ponto no qual a curva é desenhada. | 

**Comando de curva Bézier quadrática**

Cria uma curva de Bézier quadrática entre o ponto atual e o ponto final especificado usando o ponto de controle especificado (*controlPoint*). `q 100,200 300,200` é um exemplo de um comando de curva de Bézier quadrática válido. Define o equivalente a um objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um objeto [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment).

| Sintaxe |
|--------|
| *ponto de extremidade `Q ` ControlPoint* <br/> - ou - <br/> *ponto de extremidade `q ` ControlPoint* |

| Termo | Descrição |
|------|-------------|
| *controlPoint* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O ponto de controle da curva, que determina as tangentes inicial e final da curva. |
| *Extremidade* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> O ponto no qual a curva é desenhada. |

**Comando Smooth curva Bézier cubica**

Cria uma curva de Bézier cúbica entre o ponto atual e o ponto final especificado. O primeiro ponto de controle é considerado o reflexo do segundo ponto de controle do comando anterior em relação ao ponto atual. Caso não haja comando anterior ou caso o comando anterior não seja um comando de curva de Bézier cúbica ou um comando de curva de Bézier cúbica suave, suponha que o primeiro ponto de controle coincide com o ponto atual. O segundo ponto de controle, o ponto de controle do final da curva, é especificado por *controlPoint2*. Por exemplo, `S 100,200 200,300` é um comando de curva de Bézier cúbica suave válido. Esse comando define o equivalente a uma [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment) em que havia um segmento de curva precedente.

| Sintaxe |
|--------|
| `S` *ponto de extremidade* ControlPoint2 <br/> - ou - <br/>`s` *ponto de extremidade ControlPoint2* |

| Termo | Descrição |
|------|-------------|
| *controlPoint2* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O ponto de controle da curva, que determina a tangente final da curva. |
| *Extremidade* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> O ponto no qual a curva é desenhada. |

**Comando de curva de Bézier quadrática suave**

Cria uma curva de Bézier quadrática entre o ponto atual e o ponto final especificado. O ponto de controle é considerado o reflexo do ponto de controle do comando anterior em relação ao ponto atual. Caso não haja comando anterior ou caso o comando anterior não seja um comando de curva de Bézier quadrática ou um comando de curva de Bézier quadrática suave, suponha que o ponto de controle coincide com o ponto atual. Esse comando define o equivalente a uma [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) em que havia um segmento de curva precedente.

| Sintaxe |
|--------|
| *ponto de extremidade* `T` *ControlPoint* <br/> - ou - <br/> *ponto de extremidade* `t` *ControlPoint* |

| Termo | Descrição |
|------|-------------|
| *controlPoint* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> O ponto de controle da curva, que determina a tangente inicial da curva. |
| *Extremidade* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> O ponto no qual a curva é desenhada. |

**Comando de arco elíptico**

Cria um arco elíptico entre o ponto atual e a ponto final especificado. Define o equivalente a um objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) com um [**ArcSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ArcSegment).

| Sintaxe |
|--------|
| `A ` *tamanho* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *ponto de extremidade* <br/> - ou - <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| Termo | Descrição |
|------|-------------|
| *size* | [**Tamanho**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size)<br/>O raio de x e o raio de y do arco. |
| *rotationAngle* | [**Clique**](https://docs.microsoft.com/dotnet/api/system.double) <br/> A rotação da elipse, em graus. |
| *isLargeArcFlag* | Defina como 1 se o ângulo do arco deve ser maior ou igual a 180; caso contrário, defina como 0. |
| *sweepDirectionFlag* | Defina como 1 se o arco estiver desenhado em uma direção de ângulo positivo; caso contrário, defina como 0. |
| *Extremidade* | [**Empresas**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> O ponto no qual o arco é desenhado.|
 
**Comando fechar**

Termina a figura atual e cria uma linha que liga o ponto atual ao ponto inicial da figura. Esse comando cria uma junção de linha (canto) entre o último segmento e o primeiro segmento da figura.

| Sintaxe |
|--------|
| `Z` <br/> - ou - <br/> `z ` |

**Sintaxe de ponto**

Descreve as coordenadas x e y de um ponto. Consulte também [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point).

| Sintaxe |
|--------|
| *x*,*y*<br/> - ou - <br/>*x* *y* |

| Termo | Descrição |
|------|-------------|
| *x* | [**Clique**](https://docs.microsoft.com/dotnet/api/system.double) <br/> A coordenada x do ponto. |
| *y* | [**Clique**](https://docs.microsoft.com/dotnet/api/system.double) <br/> A coordenada y do ponto. |

**Observações adicionais**

Em vez de um valor numérico padrão, você também pode usar estes valores especiais. Estes valores diferenciam maiúsculas de minúsculas.

-   **Infinity**: representa **PositiveInfinity**.
-   **\-infinito**: representa **NegativeInfinity**.
-   **NaN**: representa **NaN**.

Em vez de usar decimais ou inteiros, você pode usar notação científica. Por exemplo, `+1.e17` é um valor válido.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>Ferramentas de desenho que produzem comandos de movimentação e desenho

Usar a ferramenta **caneta** e outras ferramentas de desenho no Blend para Microsoft Visual Studio 2015 geralmente produzirá um objeto [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) , com os comandos move e Draw.

É possível que você veja dados de comandos de movimentação e desenho existentes em algumas partes de controle definidas nos modelos padrão de controles do Windows Runtime XAML. Por exemplo, alguns controles usam um [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon) que tem dados definidos como comandos de movimentação e desenho.

Há exportadores ou plug-ins disponíveis para outras ferramentas de desenho gráfico vetorial comuns que podem produzir o vetor na forma de XAML. Geralmente elas criam objetos [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) em um contêiner de layout, com comandos de movimentação e desenho para [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data). Pode haver vários elementos **Path** no XAML, de modo que pincéis diferentes podem ser aplicados. Muitos desses exportadores ou plug-ins foram originalmente escritos para Windows Presentation Foundation (WPF) XAML ou Silverlight, mas a sintaxe do caminho XAML é idêntica com Windows Runtime XAML. Normalmente, você pode usar trechos de XAML de um exportador e colá-los diretamente em uma página XAML do Windows Runtime. (Contudo, você não poderá usar um **RadialGradientBrush**, se ele fazia parte do XAML convertido, porque o XAML do Windows Runtime não dá suporte ao pincel.)

## <a name="related-topics"></a>Tópicos relacionados

* [Desenhar formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)
* [Usar pincéis](https://docs.microsoft.com/windows/uwp/graphics/using-brushes)
* [**Caminho. dados**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon)


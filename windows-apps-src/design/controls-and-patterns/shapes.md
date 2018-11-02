---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: Desenhar formas
description: Aprenda a desenhar formas, como elipses, retângulos, polígonos e caminhos. A classe Path é uma maneira de visualizar uma linguagem de desenho baseada em vetor bastante complexa em uma interface do usuário XAML; por exemplo, você pode desenhar curvas de Bézier.
ms.author: jimwalk
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 984653ad20fc40035528ab7e32b904e64d6ff8c5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5927392"
---
# <a name="draw-shapes"></a>Desenhar formas

Aprenda a desenhar formas, como elipses, retângulos, polígonos e caminhos. A classe [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) é uma maneira de visualizar uma linguagem de desenho baseada em vetor bastante complexa em uma interface do usuário XAML; por exemplo, você pode desenhar curvas de Bézier.

> **APIs importantes**: [classe Path](/uwp/api/Windows.UI.Xaml.Shapes.Path), [namespace Windows.UI.Xaml.Shapes](/uwp/api/Windows.UI.Xaml.Shapes), [namespace Windows.UI.Xaml.Media](/uwp/api/Windows.UI.Xaml.Media)


Dois conjuntos de classes definem uma região espacial na interface do usuário XAML: [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) e [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry). A principal diferença entre essas classes é que **Shape** é associada a um pincel e pode ser renderizada na tela, enquanto **Geometry** simplesmente define uma região espacial e não é renderizada, a menos que isso ajude a contribuir com informações para outra propriedade de interface do usuário. Pense em **Shape** como um [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) com os limites definidos por **Geometry**. Este tópico abrange principalmente as classes **Shape**.

As classes [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) são [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line), [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse), [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon), [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) e [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path). **Path** é interessante porque pode definir uma geometria arbitrária e a classe [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) está envolvida aqui por causa da maneira de definir as partes de **Path**.

## <a name="fill-and-stroke-for-shapes"></a>Preenchimento e traços para formas

Para que uma [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) seja renderizada na tela do aplicativo, você deve associá-la a um [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Defina a propriedade [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) de **Shape** como o **Brush** desejado. Para saber mais sobre pincéis, consulte [Usando pincéis](../style/brushes.md).

Uma [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) também pode ter um [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke), que é uma linha desenhada ao redor do perímetro da forma. Um **Stroke** também exige um [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) que define sua aparência e deve ter um valor diferente de zero para [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness). **StrokeThickness** é uma propriedade que define a espessura do perímetro ao redor da borda da forma. Se você não especifica um valor de **Brush** para **Stroke**, ou se definir **StrokeThickness** como 0, a borda em torno da forma não será desenhada.

## <a name="ellipse"></a>Ellipse

Uma [**Elipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) é uma forma com o perímetro curvado. Para criar uma **Ellipse** básica, especifique uma [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), uma [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e um [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

O próximo exemplo cria uma [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) com uma [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de 200 e uma [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de 200 e usa um [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) colorido de [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) como [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

```xaml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

```csharp
var ellipse1 = new Ellipse();
ellipse1.Fill = new SolidColorBrush(Windows.UI.Colors.SteelBlue);
ellipse1.Width = 200;
ellipse1.Height = 200;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(ellipse1);
```

Veja a seguir o [**Elipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) renderizado.

![Uma Elipse renderizada.](images/shapes-ellipse.jpg)

Neste caso, a [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) é o que a maioria das pessoas chamaria de um círculo, mas é assim que uma forma circular é declarada em XAML: use uma **Ellipse** com valores iguais para [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height).

Quando uma [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) está posicionada em um layout de interface do usuário, seu tamanho é considerado como o mesmo de um retângulo com essa [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height); a área fora do perímetro não tem renderização, mas faz parte do tamanho de slot do layout.

Um conjunto de seis elementos [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) faz parte do modelo de controle para o controle [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538), e dois elementos **Ellipse** concêntricos fazem parte de um [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544).

## <a name="span-idrectanglespanspan-idrectanglespanspan-idrectanglespanrectangle"></a><span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectangle

Um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) é uma forma quadrilátera com lados opostos e iguais. Para criar um **Rectangle** básico, especifique valores para [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

Você pode arredondar os cantos de um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Para criar cantos redondos, especifique um valor para as propriedades [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) e [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy). Essas propriedades especificam o eixo x e o eixo y de uma elipse que define a curva dos cantos. O valor máximo de **RadiusX** é a [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) dividida por dois e o valor máximo de **RadiusY** é a [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) dividida por dois.

O próximo exemplo cria um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) com uma [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de 200 e uma [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de 100. Ele usa um valor de [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) de [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) para seu [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) e um valor de [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) de **SolidColorBrush** para seu [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke). Definimos a [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) como 3. Definimos a propriedade [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) como 50 e a propriedade [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) como 10, o que dá bordas arredondadas para **Rectangle**.

```xaml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
```

```csharp
var rectangle1 = new Rectangle();
rectangle1.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
rectangle1.Width = 200;
rectangle1.Height = 100;
rectangle1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
rectangle1.StrokeThickness = 3;
rectangle1.RadiusX = 50;
rectangle1.RadiusY = 10;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(rectangle1);
```

Aqui está o [**Retângulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

![Um Retângulo renderizado.](images/shapes-rectangle.jpg)

**Dica**existem alguns cenários para definições de interface do usuário onde em vez de usar um [**retângulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), uma [**borda**](https://msdn.microsoft.com/library/windows/apps/BR209250) pode ser mais apropriada. Se a sua intenção é criar uma forma retangular ao redor de outro conteúdo, deve ser melhor usar a **Border** porque ela pode ter conteúdo filho e será dimensionada automaticamente ao redor desse conteúdo, em vez de usar dimensões fixas para a altura e largura, como faz o **Rectangle**. Uma **Border** também tem a opção de ter bordas arredondadas quando você define a propriedade [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius).

Por outro lado, provavelmente [**Retângulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) é uma opção melhor para a composição de controle. A forma **Rectangle** aparece em vários modelos de controle porque ela é usada como uma parte "FocusVisual" para controles focalizáveis. Sempre que o controle está em um estado visual "Focalizado", esse retângulo fica visível. Nos demais estados, ele fica oculto.

## <a name="polygon"></a>Polígono

Um [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) é uma forma delimitada por um número arbitrário de pontos. A delimitação se dá pela conexão de uma linha de um ponto ao seguinte, com o último ponto conectado ao primeiro ponto. A propriedade [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) define uma coleção de pontos que formam o limite. Na XAML, os pontos são definidos por uma lista separada por vírgulas. Em code-behind, uma [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) é usada para definir os pontos e cada ponto é acrescentado como um valor [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) à coleção.

Você não precisa declarar explicitamente os pontos de forma que o ponto inicial e o ponto final sejam especificados como o mesmo valor [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870). A lógica de renderização de um [**Polígono**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) pressupõe que você está definindo uma forma fechada e conectará o ponto final ao ponto inicial de forma implícita.

O próximo exemplo cria um [**Polígono**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) com quatro pontos definidos como `(10,200)`, `(60,140)`, `(130,140)`e `(180,200)`. Ele usa um valor [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) de [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) para seu [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) e não tem valor para o [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) para que não tenha contorno do perímetro.

```xaml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polygon1 = new Polygon();
polygon1.Fill = new SolidColorBrush(Windows.UI.Colors.LightBlue);

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polygon1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polygon1);
```

Veja a seguir o [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) renderizado.

![Um Polígono renderizado.](images/shapes-polygon.jpg)

**Dica**um valor de [**ponto**](https://msdn.microsoft.com/library/windows/apps/BR225870) geralmente é usado como um tipo em XAML para cenários que não estejam declarando os vértices de formas. Por exemplo, **Point** faz parte dos dados de evento para os eventos de toque, para que você saiba exatamente onde ocorreu a ação de toque no espaço de uma coordenada. Para saber mais sobre **Point** e como usá-lo na XAML ou no código, consulte o tópico de referência de API para [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870).

## <a name="line"></a>Line

Uma [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) é simplesmente uma linha desenhada entre dois pontos no espaço coordenado. Uma **Line** ignora todos os valores fornecidos para [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), pois não possui espaço interno. Para uma **Line**, especifique os valores para as propriedades [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) e [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness); caso contrário, a **Line** não será renderizada.

Não use os valores [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) para especificar uma forma de [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line), em vez disso, use valores [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) discretos para [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx), [**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx), [**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) e [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx). Isso permite a marcação mínima para as linhas horizontais ou verticais. Por exemplo, `<Line Stroke="Red" X2="400"/>` define uma linha horizontal com comprimento de 400 pixels. Por padrão, as outras propriedades X,Y são 0, portanto, em termos de pontos, este XAML desenha uma linha de `(0,0)` a `(400,0)`. Você pode então usar [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) para mover **Line** na íntegra, caso queira começar em um ponto que não seja (0,0).

```xaml
<Line Stroke="Red" X2="400"/>
```

```csharp
var line1 = new Line();
line1.Stroke = new SolidColorBrush(Windows.UI.Colors.Red);
line1.X2 = 400;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(line1);
```

## <a name="span-idpolylinespanspan-idpolylinespanspan-idpolylinespan-polyline"></a><span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> Polilinha

Uma [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) é semelhante a um [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) por ser delimitada por um conjunto de pontos. A única diferença é que o último ponto de uma **Polyline** não está conectado ao primeiro ponto.

**Observação**  explicitamente, você poderia ter um ponto inicial idênticos e ponto final nos [**pontos**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) definido para o [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline), mas nesse caso, você provavelmente poderia usar um [**polígono**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) em vez disso.

Quando você especifica um [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) de uma [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline), **Fill** pinta o espaço interior da forma, mesmo se os pontos inicial e final dos [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) definidos para a **Polyline** não fizerem interseção. Quando você não especifica um **Fill**, a **Polyline** é semelhante ao que teria sido renderizado se você tivesse especificado vários elementos [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) individuais, com pontos iniciais e finais de linhas consecutivas em interseção.

Assim como ocorre com o [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon), a propriedade [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) define a coleção de pontos que formam o limite. Na XAML, os pontos são definidos por uma lista separada por vírgulas. Em code-behind, uma [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) é usada para definir os pontos e cada ponto é acrescentado como uma estrutura de [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) à coleção.

Este exemplo cria uma [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) com quatro pontos definidos como `(10,200)`, `(60,140)`, `(130,140)`e `(180,200)`. Um [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) está definido, mas não um [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

```xaml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polyline1 = new Polyline();
polyline1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
polyline1.StrokeThickness = 4;

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polyline1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polyline1);
```

Veja a seguir a [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) renderizada. Observe que o primeiro e o último pontos não estão conectados pelo contorno do [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) como se estivessem em um [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon).

![Uma Polilinha renderizada.](images/shapes-polyline.jpg)

## <a name="path"></a>Path

Um [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) é o objeto [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) mais versátil, pois pode ser usado para definir uma geometria arbitrária. Mas essa versatilidade resulta em maior complexidade. Vejamos como criar um **Path** básico em XAML.

A geometria de um caminho é definida com a propriedade [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data). Há duas técnicas para definir **Data**:

- Você pode definir um valor de cadeia de caracteres para [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) em XAML. Neste formato, o valor **Path.Data** está consumindo um formato de serialização para elementos gráficos. Você geralmente não edita o texto desse valor no formato de cadeia de caracteres depois que ele é estabelecido pela primeira vez. Em vez disso, você usa as ferramentas de design que permitem que você trabalhe em uma metáfora de design ou de desenho em uma superfície. Em seguida, você salva ou exporta a saída, e isso gera um arquivo XAML ou fragmento da cadeia de caracteres XAML com as informações de **Path.Data**.
- Você pode definir a propriedade [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) como um único objeto [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry). Isso pode ser feito em código ou em XAML. Essa única **Geometry** é geralmente um [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup), que atua como um contêiner que pode compor várias definições de geometria em um único objeto para fins do modelo de objeto. A razão mais comum para fazer isso é porque você quer usar uma ou mais curvas e formas complexas que podem ser definidas como valores [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) de uma [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143), por exemplo [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068).

Este exemplo mostra um [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) que pode ter resultado do uso do Blend for Visual Studio para produzir apenas algumas formas vetoriais e do salvamento do resultado como XAML. O **Path** completo consiste em um segmento de curva de Bézier e um segmento de linha. O exemplo tem a principal intenção de fornecer alguns exemplos de quais elementos existem no formato de serialização do [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) e o que os números representam.

Este [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) começa com o comando mover, indicado por "M", que estabelece um ponto inicial absoluto para o caminho.

O primeiro segmento é uma curva de Bézier cúbica que começa em `(100,200)` e termina em `(400,175)`, que é desenhada usando os dois pontos de controle `(100,25)` e `(400,350)`. Esse segmento é indicado pelo comando "C" na cadeia de caracteres de atributo [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data).

O segundo segmento começa com um comando de linha horizontal absoluto "H", que especifica uma linha desenhada do ponto de extremidade do subcaminho precedente `(400,175)` até um novo ponto de extremidade `(280,175)`. Por se tratar de um comando de linha horizontal, o valor especificado é uma coordenada X.

```xaml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
```

Aqui está o [**Caminho**](/uwp/api/Windows.UI.Xaml.Shapes.Path).

![Um Caminho renderizado.](images/shapes-path.jpg)

O exemplo a seguir mostra a utilização de outra técnica que discutimos: [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup) com [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168). Este exemplo exercitas alguns tipos de geometria auxiliar que podem ser usados como parte de **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) e os diversos elementos que podem ser um segmento em [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164).

```xaml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
    <Path.Data>
        <GeometryGroup>
            <RectangleGeometry Rect="50,5 100,10" />
            <RectangleGeometry Rect="5,5 95,180" />
            <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
            <RectangleGeometry Rect="50,175 100,10" />
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                            <PathFigure.Segments>
                                <PathSegmentCollection>
                                    <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                                    <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                                </PathSegmentCollection>
                            </PathFigure.Segments>
                        </PathFigure>
                    </PathFigureCollection>
                </PathGeometry.Figures>
            </PathGeometry>
        </GeometryGroup>
    </Path.Data>
</Path>
```

```csharp
var path1 = new Windows.UI.Xaml.Shapes.Path();
path1.Fill = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 204, 204, 255));
path1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
path1.StrokeThickness = 1;

var geometryGroup1 = new GeometryGroup();
var rectangleGeometry1 = new RectangleGeometry();
rectangleGeometry1.Rect = new Rect(50, 5, 100, 10);
var rectangleGeometry2 = new RectangleGeometry();
rectangleGeometry2.Rect = new Rect(5, 5, 95, 180);
geometryGroup1.Children.Add(rectangleGeometry1);
geometryGroup1.Children.Add(rectangleGeometry2);

var ellipseGeometry1 = new EllipseGeometry();
ellipseGeometry1.Center = new Point(100, 100);
ellipseGeometry1.RadiusX = 20;
ellipseGeometry1.RadiusY = 30;
geometryGroup1.Children.Add(ellipseGeometry1);

var pathGeometry1 = new PathGeometry();
var pathFigureCollection1 = new PathFigureCollection();
var pathFigure1 = new PathFigure();
pathFigure1.IsClosed = true;
pathFigure1.StartPoint = new Windows.Foundation.Point(50, 50);
pathFigureCollection1.Add(pathFigure1);
pathGeometry1.Figures = pathFigureCollection1;

var pathSegmentCollection1 = new PathSegmentCollection();
var pathSegment1 = new BezierSegment();
pathSegment1.Point1 = new Point(75, 300);
pathSegment1.Point2 = new Point(125, 100);
pathSegment1.Point3 = new Point(150, 50);
pathSegmentCollection1.Add(pathSegment1);

var pathSegment2 = new BezierSegment();
pathSegment2.Point1 = new Point(125, 300);
pathSegment2.Point2 = new Point(75, 100);
pathSegment2.Point3 = new Point(50, 50);
pathSegmentCollection1.Add(pathSegment2);
pathFigure1.Segments = pathSegmentCollection1;

geometryGroup1.Children.Add(pathGeometry1);
path1.Data = geometryGroup1;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(path1);
```

Aqui está o [**Caminho**](/uwp/api/Windows.UI.Xaml.Shapes.Path).

![Um Caminho renderizado.](images/shapes-path-2.png)

Usar [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) pode ser mais legível do que preencher uma cadeia de caracteres de [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data). Por outro lado, [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) usa uma sintaxe compatível com definições de caminho de imagem SVG (Elementos Gráficos Vetoriais Escaláveis). Então, isso pode ser útil para a portabilidade de gráficos SVG ou como resultado de uma ferramenta, como o Blend.

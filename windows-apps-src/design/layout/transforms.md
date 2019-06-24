---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Visão geral das transformações
description: Saiba como usar transformações em tempo de execução do Windows &\#160; API, alterando os sistemas de coordenadas relativos de elementos na interface do usuário.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da0031dbb87bffb457786170494140dbee0b2a6e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317114"
---
# <a name="transforms-overview"></a>Visão geral das transformações

Saiba como usar as transformações na API do Windows Runtime alterando o sistema de coordenadas relativas de elementos na interface do usuário. Use esse método para ajustar a aparência de elementos XAML individuais, como dimensionar, girar ou transformar a posição no espaço x-y.

## <a name="span-idwhatisatransformspanspan-idwhatisatransformspanspan-idwhatisatransformspanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>O que é uma transformação?

Uma *transformação* define como mapear, ou transformar, pontos de um espaço de coordenadas em outro espaço de coordenadas. Quando uma transformação é aplicada a um elemento da interface do usuário, ela muda como esse elemento da interface do usuário é renderizado para a tela como parte da interface do usuário.

Pense em transformações em quatro amplas classificações: conversão, rotação, dimensionamento e distorção. Para a finalidade de usar APIs de elementos gráficos para mudar a aparência dos elementos da interface do usuário, normalmente é mais fácil criar transformações que só definem uma operação por vez. Assim, o Windows Runtime define uma classe discreta para cada uma destas classificações de transformação:

-   [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform): converte um elemento no espaço de x-y, definindo valores para [ **X** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) e [ **Y** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform): dimensiona a transformação com base em um ponto central, definindo valores para [ **CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centerx), [ **CenterY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centery), [ **ScaleX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) e [ **ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform): gira no espaço de x-y, definindo valores para [ **ângulo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle), [ **CenterX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centerx) e [ **CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform): inclina ou cisalha no espaço de x-y, definindo valores para [ **AngleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.anglex), [ **AngleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.angley), [ **CenterX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.centerx) e [ **CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centeryproperty).

Destas, você provavelmente usará [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) e [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) com mais frequência para cenários da interface do usuário.

Você pode combinar transformações, e há duas classes de tempo de execução do Windows que dão suporte a isso: [**CompositeTransform** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) e [ **TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup). Em uma **CompositeTransform**, as transformações são aplicadas nessa ordem: dimensionamento, distorção, rotação e conversão. Use **TransformGroup** em vez de **CompositeTransform** para que as transformações sejam aplicadas em uma ordem diferente. Para saber mais, veja [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform).

## <a name="span-idtransformsandlayoutspanspan-idtransformsandlayoutspanspan-idtransformsandlayoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformações e layout

No layout XAML, as transformações são aplicadas depois que o cálculo de layout é concluído, então os cálculos de espaço disponível e outras decisões de layout foram tomadas antes da aplicação das transformações. Como o layout vem primeiro, você às vezes obterá resultados inesperados se transformar elementos que estão em uma célula [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) ou um contêiner de layout semelhante que aloca espaço durante o layout. O elemento transformado poderá parecer truncado ou obscurecido porque está tentando desenhar em uma área que não calculou as dimensões pós-transformação ao dividir espaço em seu contêiner pai. Talvez seja necessário experimentar os resultados da transformação e ajustar algumas configurações. Por exemplo, em vez de usar o layout adaptável e o dimensionamento em estrela, você pode precisar alterar as propriedades **Center** ou declarar medidas de pixel fixo para o espaço do layout, a fim de verificar se o pai aloca espaço suficiente.

**Observação de migração:**  Windows Presentation Foundation (WPF) tinha um **LayoutTransform** propriedade aplicado transformações antes da passagem do layout. Porém, o XAML do Windows Runtime não aceita uma propriedade **LayoutTransform**. (O Microsoft Silverlight não tinha essa propriedade também.)

## <a name="span-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Aplicar uma transformação a um elemento de interface do usuário

Ao aplicar uma transformação a um objeto, normalmente você a usa para definir a propriedade [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). A definição dessa propriedade não muda literalmente o objeto pixel por pixel. O que essa propriedade realmente faz é aplicar a transformação dentro do espaço de coordenadas local em que o objeto existe. Assim, a lógica e a operação de renderização (pós-layout) renderiza os espaços de coordenadas combinados, dando a impressão de que o objeto mudou a aparência e também possivelmente a posição do layout (se a [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) foi aplicada).

Por padrão, cada transformação de renderização é centralizada na origem do sistema de coordenadas local do objeto de destino, ou seja, (0,0). A única exceção é uma [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), que não tem propriedades centrais a ser definidas porque o efeito de conversão é o mesmo independentemente de onde esteja centralizado. Mas cada uma das outras transformações tem propriedades que definem os valores **CenterX** e **CenterY**.

Sempre que você pode usar transformações com [ **UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), lembre-se de que há outra propriedade no [ **UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) que afeta a transformação de comportamento: [**RenderTransformOrigin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransformorigin). O que **RenderTransformOrigin** declara é se a transformação inteira deve ser aplicada ao ponto padrão (0,0) de um elemento ou a algum outro ponto de origem dentro do espaço de coordenadas relativas desse elemento. Para elementos típicos, (0,0) coloca a transformação no canto superior esquerdo. Dependendo do efeito que você está tentando, você pode escolher mudar **RenderTransformOrigin**, em vez de ajustar os valores **CenterX** e **CenterY** nas transformações. Observe que, se você aplicar os valores **RenderTransformOrigin** e **CenterX** / **CenterY**, os resultados poderão ser bastante confusos, principalmente se estiver animando qualquer um dos valores.

Para fins de teste por pressionamento, um objeto para o qual uma transformação é aplicada continua respondendo para inserir de uma maneira que seja consistente com sua aparência visual no espaço x-y. Por exemplo, se você usou [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) para mover um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 400 pixels lateralmente na interface do usuário, esse **Rectangle** poderá responder a eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) quando o usuário pressionar o ponto em que o **Rectangle** aparece visualmente. Você não obterá falsos eventos se o usuário pressionar a área em que o **Rectangle** estava antes de ser convertido. Para quaisquer considerações de índice z que afetam o teste de acertos, a aplicação de uma transformação não faz diferença; o índice z que rege qual elemento manipula eventos de entrada para um ponto no espaço x-y ainda é avaliado usando a ordem de filho conforme declarado em um contêiner. Essa ordem normalmente é a mesma da ordem em que você declararia os elementos em XAML, apesar de que, para os elementos filho de um objeto [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), é possível ajustá-la aplicando a propriedade anexada [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) aos elementos filho.

## <a name="span-idothertransformpropertiesspanspan-idothertransformpropertiesspanspan-idothertransformpropertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Outras propriedades de transformação

-   [**Brush.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.transform), [**Brush.RelativeTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.relativetransform): Esses influenciam como um [ **pincel** ](/uwp/api/Windows.UI.Xaml.Media.Brush) usa espaço de coordenadas em área de que o **pincel** é aplicada para definir propriedades visuais, como colorir e planos de fundo. Essas transformações não são relevantes para os pincéis mais comuns (que geralmente estão definindo cores sólidas com [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)), mas podem ser ocasionalmente úteis ao pintar áreas com [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) ou [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush).
-   [**Geometry.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.geometry.transform): Você pode usar essa propriedade para aplicar uma transformação a uma geometria antes de usar esse geometria para um [ **Path** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) valor da propriedade.

## <a name="span-idanimatingatransformspanspan-idanimatingatransformspanspan-idanimatingatransformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animando uma transformação

[**Transforme** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) objetos podem ser animados. Para animar uma **Transform**, aplique uma animação de um tipo compatível à propriedade que você quer animar. Isso normalmente significa que você está usando os objetos [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) ou [**DoubleAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) para definir a animação, pois todas as propriedades das transformações são do tipo [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN). Animações que afetam uma transformação que é usada para um valor [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) não são consideradas animações dependentes, mesmo se têm uma duração diferente de zero. Para saber mais sobre animações dependentes, consulte [Animações com storyboard](/windows/uwp/design/motion/storyboarded-animations).

Se você animar as propriedades para produzir um efeito semelhante a uma transformação em termos da aparência visual líquida, por exemplo, animando a [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de um [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) em vez de aplicar um [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)—essas animações são quase sempre tratadas como animações dependentes. Você teria de habilitar as animações e poderia haver problemas de desempenho significativos com a animação, principalmente se você estiver tentando dar suporte à interação do usuário enquanto esse objeto está sendo animado. Por isso, é preferível usar uma transformação e animá-la em vez de animar qualquer outra propriedade em que a animação seria tratada como uma animação dependente.

Para direcionar a transformação, é necessário haver uma [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) existente como valor de [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). Normalmente, você coloca um elemento para o tipo de transformação apropriado no XAML inicial, às vezes sem propriedades definidas nessa transformação.

Geralmente você usa uma técnica de direcionamento indireto para aplicar animações às propriedades de uma transformação. Para saber mais sobre a sintaxe de direcionamento indireto, consulte [Animações com storyboard](/windows/uwp/design/motion/storyboarded-animations) e [Sintaxe de caminhos de propriedades](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax).

Os estilos padrão para controles às vezes definem as animações das transformações como parte de seu comportamento de estado visual. Por exemplo, os estados visuais de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) usam valores [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) animados para "girar" os pontos do anel.

A seguir está um exemplo simples de como animar uma transformação. Nesse caso, ocorre a animação do [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle) de uma [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) para girar um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) no lugar em torno de seu centro visual. Esse exemplo nomeia **RotateTransform** por isso não precisa de direcionamento de animação indireto, mas uma alternativa é deixar a transformação sem nome, nomear o elemento ao qual essa transformação é aplicada e usar o direcionamento indireto como `(UIElement.RenderTransform).(RotateTransform.Angle)`.

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Estatísticas para coordenadas quadros de referência no tempo de execução

[**UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) tem um método chamado [ **TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual), que pode gerar um [ **transformar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) que correlaciona os coordenadas quadros de referência para dois elementos de interface do usuário. Você pode usar esse método para comparar um elemento com o quadro de referência de coordenadas padrão do aplicativo se calcular o visual da raiz como primeiro parâmetro. Isso pode ser útil se você capturou um evento de entrada de um elemento diferente ou se está tentando prever o comportamento do layout sem na verdade solicitar um cálculo de layout.

Dados de evento obtidos de eventos de ponteiro oferecem acesso a um método [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpoint.getcurrentpoint), em que é possível especificar um parâmetro *relativeTo* para mudar o quadro de referência de coordenadas para um elemento específico em vez do aplicativo padrão. Isso simplesmente aplica uma transformação de conversão internamente e transforma os dados de coordenadas x-y para você ao criar o objeto [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) retornado.

## <a name="span-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Descrevendo uma transformação matematicamente

Uma transformação pode ser descrita em termos de uma matriz de transformação. Uma matriz 3x3 é usada para descrever as transformações em um plano x-y bidimensional. Matrizes de transformação afins podem ser multiplicadas para formar qualquer número de transformações lineares, como rotação e distorção, seguidas de conversão. A coluna final de uma matriz de transformação afim é igual a (0, 0, 1), portanto você precisa especificar apenas os membros das duas primeiras colunas na descrição matemática.

A descrição matemática de uma transformação pode ser útil se você tiver conhecimentos matemáticos ou familiaridade com técnicas de programação de elementos gráficos que também usem matrizes para descrever transformações do espaço de coordenadas. Há um [ **transformar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)-derivado da classe que permite expressar uma transformação diretamente em termos de sua matriz 3 × 3: [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). **MatrixTransform** tem uma [ **Matrix** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrixtransform.matrix) propriedade, que contém uma estrutura que tem seis propriedades: [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11), [ **M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12), [ **M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21), [ **M22** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22), [ **OffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) e [ **OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety). Cada propriedade [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) usa um valor **Double** e corresponde a seis valores relevantes (colunas 1 e 2) de uma matriz de transformação afim.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)         | [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)         | 0   |
| [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)         | [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)         | 0   |
| [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) | [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety) | 1   |

 

Qualquer transformação que você poderia descrever com um objeto [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform), [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) ou [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform) poderia ser perfeitamente descrita por uma [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform) com um valor [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix). Mas normalmente você usa **TranslateTransform** e as outras, porque as propriedades nessas classes de transformação são mais fáceis de conceitualizar do que definir os componentes vetoriais em uma **Matrix**. Também é mais fácil animar as propriedades discretas dessas transformações; enquanto uma **Matrix** na verdade é uma estrutura e não um [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject), por isso não pode dar suporte a valores individuais animados.

Algumas ferramentas de design XAML que permitem aplicar operações de transformação serializarão os resultados como uma [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). Nesse caso, convém usar a mesma ferramenta de design outra vez para mudar o efeito de transformação e serializar novamente o XAML, em vez de tentar manipular os valores [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) diretamente sozinho no XAML.

## <a name="span-id3-dtransformsspanspan-id3-dtransformsspanspan-id3-dtransformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>Transformações 3D

No Windows 10, o XAML introduziu uma nova propriedade, [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), que pode ser usada para criar efeitos 3D com interface de usuário. Para fazer isso, use [**PerspectiveTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.perspectivetransform3d) para adicionar uma perspectiva 3D compartilhada ou "câmera" à sua cena e, em seguida, use [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.compositetransform3d) para transformar um elemento no espaço 3D, como você usaria [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform). Veja [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d) para transformações de uma discussão sobre como implementar 3D.

 Para efeitos 3D mais simples que se aplicam somente a um único objeto, a propriedade [**UIElement.Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) pode ser usada. Usar um [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) como o valor dessa propriedade é equivalente a aplicar uma transformação de perspectiva fixa e uma ou mais transformações 3D ao elemento. Esse tipo de transformação é descrito em mais detalhe em [Efeitos de perspectiva 3D para interface do usuário de XAML](3-d-perspective-effects.md).

## <a name="span-idrelatedtopicsspanrelated-topics"></a><span id="related_topics"></span>Tópicos relacionados

* [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
* [Efeitos de perspectiva 3D para o XAML UI](3-d-perspective-effects.md)
* [**Transformação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)
 

 






---
author: Jwmsft
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: "Visão geral das transformações"
description: "Saiba como usar as transformações na API do Windows Runtime&\\#160; alterando o sistema de coordenadas relativas de elementos na interface do usuário."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 6534e7337ecaa081d0bb05335425f5b58557ab32

---

# Visão geral das transformações

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Saiba como usar as transformações na API do Windows Runtime alterando o sistema de coordenadas relativas de elementos na interface do usuário. Use esse método para ajustar a aparência de elementos XAML individuais, como dimensionar, girar ou transformar a posição no espaço x-y.

## <span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>O que é uma transformação?

Uma *transformação* define como mapear, ou transformar, pontos de um espaço de coordenadas em outro espaço de coordenadas. Quando uma transformação é aplicada a um elemento da interface do usuário, ela muda como esse elemento da interface do usuário é renderizado para a tela como parte da interface do usuário.

Pense em transformações em quatro amplas classificações: conversão, rotação, dimensionamento e distorção. Para a finalidade de usar APIs de elementos gráficos para mudar a aparência dos elementos da interface do usuário, normalmente é mais fácil criar transformações que só definem uma operação por vez. Assim, o Windows Runtime define uma classe discreta para cada uma destas classificações de transformação:

-   [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027): traduz um elemento no espaço x-y definindo valores para [**X**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) e [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940): dimensiona a transformação com base em um ponto central definindo valores para [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx), [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx), [**ScaleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) e [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932): gira no espaço x-y definindo valores para [**Angle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) e [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950): distorce no espaço x-y definindo valores para [**AngleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx), [**AngleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) e [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty).

Destas, você provavelmente usará [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) e [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) com mais frequência para cenários da interface do usuário.

Você pode combinar as transformações, e há duas classes de Windows Runtime que dão suporte a isso: [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) e [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022). Em uma **CompositeTransform**, as transformações são aplicadas nessa ordem: dimensionamento, distorção, rotação e conversão. Use **TransformGroup** em vez de **CompositeTransform** para que as transformações sejam aplicadas em uma ordem diferente. Para saber mais, veja [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105).

## <span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformações e layout

No layout XAML, as transformações são aplicadas depois que o cálculo de layout é concluído, então os cálculos de espaço disponível e outras decisões de layout foram tomadas antes da aplicação das transformações. Como o layout vem primeiro, você às vezes obterá resultados inesperados se transformar elementos que estão em uma célula [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) ou um contêiner de layout semelhante que aloca espaço durante o layout. O elemento transformado poderá parecer truncado ou obscurecido porque está tentando desenhar em uma área que não calculou as dimensões pós-transformação ao dividir espaço em seu contêiner pai. Talvez seja necessário experimentar os resultados da transformação e ajustar algumas configurações. Por exemplo, em vez de usar o layout adaptável e o dimensionamento em estrela, você pode precisar alterar as propriedades **Center** ou declarar medidas de pixel fixo para o espaço do layout, a fim de verificar se o pai aloca espaço suficiente.

**Observação sobre migração:** o WPF (Windows Presentation Foundation) tinha uma propriedade **LayoutTransform** que aplicava transformações antes do cálculo de layout. Porém, o XAML do Windows Runtime não aceita uma propriedade **LayoutTransform**. (O Microsoft Silverlight não tinha essa propriedade também.)

## <span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Aplicando uma transformação em um elemento de interface do usuário

Ao aplicar uma transformação a um objeto, normalmente você a usa para definir a propriedade [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). A definição dessa propriedade não muda literalmente o objeto pixel por pixel. O que essa propriedade realmente faz é aplicar a transformação dentro do espaço de coordenadas local em que o objeto existe. Assim, a lógica e a operação de renderização (pós-layout) renderiza os espaços de coordenadas combinados, dando a impressão de que o objeto mudou a aparência e também possivelmente a posição do layout (se a [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) foi aplicada).

Por padrão, cada transformação de renderização é centralizada na origem do sistema de coordenadas local do objeto de destino, ou seja, (0,0). A única exceção é uma [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), que não tem propriedades centrais a ser definidas porque o efeito de conversão é o mesmo independentemente de onde esteja centralizado. Mas cada uma das outras transformações tem propriedades que definem os valores **CenterX** e **CenterY**.

Sempre que você usar transformações com [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), lembre-se de que há outra propriedade em [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) que afeta a maneira como a transformação se comporta: [**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx). O que **RenderTransformOrigin** declara é se a transformação inteira deve ser aplicada ao ponto padrão (0,0) de um elemento ou a algum outro ponto de origem dentro do espaço de coordenadas relativas desse elemento. Para elementos típicos, (0,0) coloca a transformação no canto superior esquerdo. Dependendo do efeito que você está tentando, você pode escolher mudar **RenderTransformOrigin**, em vez de ajustar os valores **CenterX** e **CenterY** nas transformações. Observe que, se você aplicar os valores **RenderTransformOrigin** e **CenterX** / **CenterY**, os resultados poderão ser bastante confusos, principalmente se estiver animando qualquer um dos valores.

Para fins de teste por pressionamento, um objeto para o qual uma transformação é aplicada continua respondendo para inserir de uma maneira que seja consistente com sua aparência visual no espaço x-y. Por exemplo, se você usou [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) para mover um [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 400 pixels lateralmente na interface do usuário, esse **Rectangle** poderá responder a eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) quando o usuário pressionar o ponto em que o **Rectangle** aparece visualmente. Você não obterá falsos eventos se o usuário pressionar a área em que o **Rectangle** estava antes de ser convertido. Para quaisquer considerações de índice z que afetam o teste de acertos, a aplicação de uma transformação não faz diferença; o índice z que rege qual elemento manipula eventos de entrada para um ponto no espaço x-y ainda é avaliado usando a ordem de filho conforme declarado em um contêiner. Essa ordem normalmente é a mesma da ordem em que você declararia os elementos em XAML, apesar de que, para os elementos filho de um objeto [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267), é possível ajustá-la aplicando a propriedade anexada [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) aos elementos filho.

## <span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Outras propriedades de uma transformação

-   [**Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082), [**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080): elas influenciam a maneira como [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) usa o espaço de coordenadas na área em que **Brush** é aplicado para definir propriedades visuais como primeiros planos e planos de fundo. Essas transformações não são relevantes para os pincéis mais comuns (que geralmente estão definindo cores sólidas com [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)), mas podem ser ocasionalmente úteis ao pintar áreas com [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) ou [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108).
-   [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066): você pode usar essa propriedade para aplicar uma transformação a uma geometria antes de usar essa geometria para um valor de propriedade [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356).

## <span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animando uma transformação

[Os objetos **Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) podem ser animados. Para animar uma **Transform**, aplique uma animação de um tipo compatível à propriedade que você quer animar. Isso normalmente significa que você está usando os objetos [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) ou [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) para definir a animação, pois todas as propriedades das transformações são do tipo [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Animações que afetam uma transformação que é usada para um valor [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) não são consideradas animações dependentes, mesmo se têm uma duração diferente de zero. Para saber mais sobre animações dependentes, consulte [Animações com storyboard](storyboarded-animations.md).

Se você animar as propriedades para produzir um efeito semelhante a uma transformação em termos da aparência visual líquida, por exemplo, animando a [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) e [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) de um [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) em vez de aplicar um [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)—essas animações são quase sempre tratadas como animações dependentes. Você teria de habilitar as animações e poderia haver problemas de desempenho significativos com a animação, principalmente se você estiver tentando dar suporte à interação do usuário enquanto esse objeto está sendo animado. Por isso, é preferível usar uma transformação e animá-la em vez de animar qualquer outra propriedade em que a animação seria tratada como uma animação dependente.

Para direcionar a transformação, é necessário haver uma [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) existente como valor de [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). Normalmente, você coloca um elemento para o tipo de transformação apropriado no XAML inicial, às vezes sem propriedades definidas nessa transformação.

Geralmente você usa uma técnica de direcionamento indireto para aplicar animações às propriedades de uma transformação. Para saber mais sobre a sintaxe de direcionamento indireto, consulte [Animações com storyboard](storyboarded-animations.md) e [Sintaxe de caminhos de propriedades](https://msdn.microsoft.com/library/windows/apps/Mt185586).

Os estilos padrão para controles às vezes definem as animações das transformações como parte de seu comportamento de estado visual. Por exemplo, os estados visuais de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) usam valores [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) animados para "girar" os pontos do anel.

A seguir está um exemplo simples de como animar uma transformação. Nesse caso, ocorre a animação do [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx) de uma [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) para girar um [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) no lugar em torno de seu centro visual. Esse exemplo nomeia **RotateTransform** por isso não precisa de direcionamento de animação indireto, mas uma alternativa é deixar a transformação sem nome, nomear o elemento ao qual essa transformação é aplicada e usar o direcionamento indireto como `(UIElement.RenderTransform).(RotateTransform.Angle)`.

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

## <span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Justificando quadros de referência de coordenadas em tempo de execução

[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) tem um método chamado [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx), que pode gerar uma [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) que correlaciona os quadros de referência de coordenadas de dois elementos de interface do usuário. Você pode usar esse método para comparar um elemento com o quadro de referência de coordenadas padrão do aplicativo se calcular o visual da raiz como primeiro parâmetro. Isso pode ser útil se você capturou um evento de entrada de um elemento diferente ou se está tentando prever o comportamento do layout sem na verdade solicitar um cálculo de layout.

Dados de evento obtidos de eventos de ponteiro oferecem acesso a um método [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141), em que é possível especificar um parâmetro *relativeTo* para mudar o quadro de referência de coordenadas para um elemento específico em vez do aplicativo padrão. Isso simplesmente aplica uma transformação de conversão internamente e transforma os dados de coordenadas x-y para você ao criar o objeto [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038) retornado.

## <span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Descrevendo uma transformação matematicamente

Uma transformação pode ser descrita em termos de uma matriz de transformação. Uma matriz 3x3 é usada para descrever as transformações em um plano x-y bidimensional. Matrizes de transformação afins podem ser multiplicadas para formar qualquer número de transformações lineares, como rotação e distorção, seguidas de conversão. A coluna final de uma matriz de transformação afim é igual a (0, 0, 1), portanto você precisa especificar apenas os membros das duas primeiras colunas na descrição matemática.

A descrição matemática de uma transformação pode ser útil se você tiver conhecimentos matemáticos ou familiaridade com técnicas de programação de elementos gráficos que também usem matrizes para descrever transformações do espaço de coordenadas. Há uma classe derivada de [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)que permite expressar uma transformação diretamente em termos de sua matriz: [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). **MatrixTransform** tem uma propriedade [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx), que mantém uma estrutura que tem seis propriedades: [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847), [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853), [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851), [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849), [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) e [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816). Cada propriedade [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) usa um valor **Double** e corresponde a seis valores relevantes (colunas 1 e 2) de uma matriz de transformação afim.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

Qualquer transformação que você poderia descrever com um objeto [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940), [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) ou [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) poderia ser perfeitamente descrita por uma [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137) com um valor [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127). Mas normalmente você usa **TranslateTransform** e as outras, porque as propriedades nessas classes de transformação são mais fáceis de conceitualizar do que definir os componentes vetoriais em uma **Matrix**. Também é mais fácil animar as propriedades discretas dessas transformações; enquanto uma **Matrix** na verdade é uma estrutura e não um [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356), por isso não pode dar suporte a valores individuais animados.

Algumas ferramentas de design XAML que permitem aplicar operações de transformação serializarão os resultados como uma [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). Nesse caso, convém usar a mesma ferramenta de design outra vez para mudar o efeito de transformação e serializar novamente o XAML, em vez de tentar manipular os valores [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) diretamente sozinho no XAML.

## <span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>Transformações 3D

No Windows 10, o XAML introduziu uma nova propriedade, [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), que pode ser usada para criar efeitos 3D com interface de usuário. Para fazer isso, use [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.perspectivetransform3d.aspx) para adicionar uma perspectiva 3D compartilhada ou "câmera" à sua cena e, em seguida, use [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.compositetransform3d.aspx) para transformar um elemento no espaço 3D, como você usaria [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105). Veja [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx) para transformações de uma discussão sobre como implementar 3D.

 Para efeitos 3D mais simples que se aplicam somente a um único objeto, a propriedade [**UIElement.Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) pode ser usada. Usar um [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/br210192) como o valor dessa propriedade é equivalente a aplicar uma transformação de perspectiva fixa e uma ou mais transformações 3D ao elemento. Esse tipo de transformação é descrito em mais detalhe em [Efeitos de perspectiva 3D para interface do usuário de XAML](3-d-perspective-effects.md).

## <span id="related_topics"></span>Tópicos relacionados

* [Desenhando formas](drawing-shapes.md)
* [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)
* [Efeitos da perspectiva 3D na interface do usuário XAML](3-d-perspective-effects.md)
* [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 








<!--HONumber=Aug16_HO3-->



---
title: Animações de quadro chave e animações com função de easing
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: Animações de quadro chave lineares, animações de quadro chave com um valor KeySpline ou funções de easing são três técnicas diferentes para praticamente o mesmo cenário.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea6ec3b879ebfe997e565488828ee9942d4ecb13
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366116"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>Animações de quadro chave e animações com função de easing



Animações de quadro chave lineares, animações de quadro chave com um **KeySpline** ou funções de easing são três técnicas diferentes para basicamente o mesmo cenário: criar uma animação de storyboard um pouco mais complexa e que usa um comportamento de animação não linear de um estado inicial até um estado final.

## <a name="prerequisites"></a>Pré-requisitos

Leia o tópico [Animações de storyboard](storyboarded-animations.md). Este tópico faz referência aos conceitos de animação que foram explicados em [Animações de storyboard](storyboarded-animations.md) e não os descreverá novamente. Por exemplo, [Animações de storyboard](storyboarded-animations.md) descreve como direcionar animações, storyboards como recursos, os valores da propriedade [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) como [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) etc.

## <a name="animating-using-key-frame-animations"></a>Animando usando animações de quadro chave

As animações de quadro chave permitem mais de um valor de destino, que é atingido em um ponto ao longo da linha do tempo da animação. Em outras palavras, cada quadro chave pode especificar um valor intermediário diferente, e o último quadro chave atingido é o valor de animação final. Especificando vários valores a serem animados, você pode criar animações mais complexas. As animações de quadro chave também permitem uma lógica de interpolação diferente, sendo cada uma implementada como uma subclasse **KeyFrame** diferente por tipo de animação. Mais especificamente, cada tipo de animação de quadro chave tem uma variação de **Discrete**, **Linear**, **Spline** and **Easing** de sua classe **KeyFrame** para a especificação de seus quadros chave. Por exemplo, para especificar uma animação que direciona um [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) e usa quadros chave, você pode declarar quadros chave com [**DiscreteDoubleKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteDoubleKeyFrame), [**LinearDoubleKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.LinearDoubleKeyFrame), [**SplineDoubleKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame) e [**EasingDoubleKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EasingDoubleKeyFrame). Você pode usar qualquer um e todos esses tipos em uma única coleção de **KeyFrames** para alterar a interpolação sempre que um novo quadro chave for atingido.

Para o comportamento de interpolação, cada quadro chave controla a interpolação até que o tempo de seu **KeyTime** seja atingido. Seu **Value** também é atingido nesse momento. Se houver mais quadros chaves depois, o valor então se torna o valor inicial para o próximo quadro chave em uma sequência.

No início da animação, se nenhum quadro chave com um **KeyTime** de "0:0:0" existir, o valor inicial será o valor não animado da propriedade. Isso é semelhante à maneira como uma animação **From**/**To**/**By** funciona quando não há **From**.

A duração de uma animação de quadro chave é implicitamente a duração equivalente ao valor **KeyTime** mais alto definido em qualquer um de seus quadros chave. Você pode definir um [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) explícito se desejar, mas tome cuidado para que ele não seja menor do que um **KeyTime** em seus próprios quadros chave, caso contrário, parte da animação será cortada.

Além de [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration), você pode definir todas as propriedades baseadas em [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) em uma animação de quadro chave, da mesma maneira que é possível com uma animação **From**/**To**/**By** porque as classes de animação de quadro chave também são derivadas de **Timeline**. Elas são:

-   [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse): quando o último quadro-chave é atingido, os quadros são repetidos na ordem inversa do final. Isso duplica a duração aparente da animação.
-   [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime): atrasa o início da animação. A linha do tempo dos valores **KeyTime** nos quadros não começa a contar até que **BeginTime** seja atingido, assim, não há risco de que os quadros sejam cortados
-   [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior): controla o que acontece quando o último quadro-chave é atingido. **FillBehavior** não tem efeito sobre quadros chave intermediários.
-   [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   Se definidos como **Forever**, os quadros chave e sua linha do tempo serão repetidos infinitamente.
    -   Se definida como uma contagem de iteração, a linha do tempo será repetida várias vezes.
    -   Se definida como [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration), a linha do tempo será repetida até que esse tempo seja atingido. Isso pode truncar a animação no meio da sequência de quadros chave se não for um fator inteiro da duração implícita da linha do tempo.
-   [**SpeedRatio** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.speedratioproperty) (não são comumente usados)

### <a name="linear-key-frames"></a>Quadros chave lineares

Quadros chave lineares resultam em uma interpolação linear mais simples do valor até que o **KeyTime** do quadro seja atingido. Esse comportamento de interpolação é o mais semelhante às animações **From**/**To**/**By** mais simples descritas no tópico [Animações de storyboard](storyboarded-animations.md).

Consulte a seguir como usar uma animação de quadro chave para redimensionar a altura de renderização de um retângulo usando quadros chave linears. Esse exemplo executa uma animação em que a altura do retângulo aumenta ligeiramente de maneira linear nos primeiros quatro segundos e depois e redimensionada rapidamente nos últimos dois segundos até que o retângulo tenha o dobro da altura inicial.

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>Quadros chave separados

Os quadros chave separados não usam interpolação. Quando um **KeyTime** é atingido, o novo **Value** é simplesmente aplicado. Dependendo de qual propriedade da interface do usuário estiver sendo animada, isso geralmente produz uma animação que parece "saltar". Certifique-se de que esse é o comportamento estético realmente desejado. Você pode atenuar os saltos aparentes aumentando o número de quadros chave declarados, mas, se desejar uma animação suave, é melhor usar quadros chave lineares ou de spline.

> [!NOTE]
> Quadros chave discretos são a única maneira de animar um valor que não é do tipo [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) e [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), com um [**DiscreteObjectKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame). Abordaremos isso em mais detalhes adiante neste tópico.

### <a name="spline-key-frames"></a>Quadros chave de spline

Um quadro chave de spline cria uma transição variável entre valores de acordo com o valor da propriedade **KeySpline**. Essa propriedade especifica o primeiro e o segundo ponto de controle de uma curva de Bézier, que descreve a aceleração da animação. Basicamente, um [**KeySpline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.KeySpline) define uma relação de função ao longo do tempo em que o gráfico função-tempo é a forma dessa curva de Bézier. Geralmente, você especifica um valor **KeySpline** em uma cadeia de caracteres de atributo XAML abreviada que tem quatro valores [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) separados por espaços ou vírgulas. Esses valores são pares "X, Y" para dois pontos de controle da curva de Bézier. "X" é o tempo e "Y" é o modificador de função para o valor. Cada valor deve estar sempre entre 0 e 1. Sem modificação do ponto de controle para um **KeySpline**, a linha reta de 0,0 a 1,1 é a representação de uma função ao longo do tempo para uma interpolação linear. Seus pontos de controle alteram a forma dessa curva e, assim, o comportamento da função ao longo do tempo da animação de spline. Provavelmente, é melhor conferir isso visualmente em um gráfico. Você pode executar o [Exemplo de visualizador de spline chave do Silverlight](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) em um navegador para ver como os pontos de controle modificam a curva e como uma animação de exemplo é executada quando é usada como um valor **KeySpline**

Este próximo exemplo mostra três quadros chave diferentes aplicados a uma animação, em que o último é uma animação de spline chave para um valor [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) ([**SplineDoubleKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame)). Observe a cadeia de caracteres "0.6,0.0 0.9,0.00" aplicada a **KeySpline**. Isso gera uma curva em que a animação parece ser executada devagar inicialmente, mas depois ela rapidamente atinge o valor logo antes de **KeyTime** ser atingido.

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>Quadros chave de easing

Um quadro chave de easing é um quadro chave em que a interpolação está sendo aplicada e a função ao longo do tempo da interpolação é controlada por várias fórmulas matemáticas predefinidas. Na realidade, você pode gerar, com um quadro chave de spline, quase o mesmo resultado obtido com alguns tipos de função de easing, mas também há algumas funções de easing, como [**BackEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase), que você não pode reproduzir com um spline.

Para aplicar uma função de easing a um quadro chave de easing, você define a propriedade **EasingFunction** como um elemento de propriedade no XAML para esse quadro chave. Para o valor, especifique um elemento de objeto para um dos tipos de função de easing.

Esse exemplo aplica um [**CubicEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) e um [**BounceEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) como quadros chave sucessivos a um [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) para criar um efeito de salto.

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Este é apenas um exemplo de função de easing. Abordaremos mais na próxima seção.

## <a name="easing-functions"></a>Funções de easing

As funções de facilitação permitem aplicar fórmulas matemáticas personalizadas a suas animações. As operações matemáticas geralmente são úteis para produzir animações que simulam a física real em um sistema de coordenadas 2D. Por exemplo, você pode querer que um objeto salte de forma realista ou se comporte como se estivesse preso a uma mola. Você pode usar animações de quadro chave ou até mesmo animações **From**/**To**/**By** para simular esses efeitos, mas seria trabalhoso e a animação seria menos precisa do que se uma fórmula matemática fosse usada.

As funções de easing podem ser aplicadas a animações de três maneiras:

-   Com o uso de um quadro chave de easing em uma animação de quadro chave, conforme descrito na seção anterior. Use [**EasingColorKeyFrame.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.easingcolorkeyframe.easingfunction), [**EasingDoubleKeyFrame.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction) ou [**EasingPointKeyFrame.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.easingpointkeyframe.easingfunction).
-   Definindo a propriedade **EasingFunction** em um dos tipos de animação **From**/**To**/**By**. Use [**ColorAnimation.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.coloranimation.easingfunction), [**DoubleAnimation.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) ou [**PointAnimation.EasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.pointanimation.easingfunction).
-   Definindo [**GeneratedEasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) como parte de um [**VisualTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualTransition). Isso é específico da definição de estados visuais para controles. Para obter mais informações, consulte [**GeneratedEasingFunction**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) ou [Storyboards para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

Consulte a seguir uma lista das funções de easing:

-   [**BackEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase): Retrai o movimento de uma animação um pouco antes de começar a ser animada no caminho indicado.
-   [**BounceEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase): Cria um efeito de devolução.
-   [**CircleEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase): Cria uma animação que acelera ou desacelerada usando uma função circular.
-   [**CubicEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase): Cria uma animação que acelera ou desacelerada usando a fórmula f(t) = t3.
-   [**ElasticEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ElasticEase): Cria uma animação que se parece com uma mola oscilando para parar e para trás até que se trata de rest.
-   [**ExponentialEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase): Cria uma animação que acelera ou desacelerada usando uma fórmula exponencial.
-   [**PowerEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase): Cria uma animação que acelera ou desacelerada usando a fórmula f(t) = tp onde p é igual a [ **Power** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.powerease.power) propriedade.
-   [**QuadraticEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase): Cria uma animação que acelera ou desacelerada usando a fórmula f(t) = t2.
-   [**QuarticEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.QuarticEase): Cria uma animação que acelera ou desacelerada usando a fórmula f(t) = t4.
-   [**QuinticEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.QuinticEase): Criar uma animação que acelera ou desacelerada usando a fórmula f(t) = t5.
-   [**SineEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SineEase): Cria uma animação que acelera ou desacelerada usando uma fórmula de seno.

Algumas das funções de easing têm suas próprias propriedades. Por exemplo, [**BounceEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) tem duas propriedades [**Bounces**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.bounceease.bounces) e [**Bounciness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.bounceease.bounciness) que modificam o comportamento da função ao longo do tempo desse **BounceEase** específico. Outras funções de easing, como [**CubicEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) não têm propriedades adicionais além da propriedade [**EasingMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.easingfunctionbase.easingmode) compartilhada por todas as funções de easeing e sempre produzem o mesmo comportamento de função ao longo do tempo.

Algumas dessas funções de easing têm um pouco de sobreposição, dependendo de como você define as propriedades nas funções de easing que têm propriedades. Por exemplo, [**QuadraticEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase) é exatamente o mesmo que um [**PowerEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase) com [**Power**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.powerease.power) igual a 2. Além disso, [**CircleEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase) é basicamente um [**ExponentialEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase) de valor padrão.

A função de easing [**BackEase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase) é exclusiva porque pode alterar o valor fora do intervalo normal definido por **From**/**To** ou valores de quadros chave. Ela começa a animação alterando o valor na direção oposta ao que seria esperado de um comportamento de **From**/**To** normal, volta para **From** ou o valor inicial e depois executa a animação normalmente.

Em um exemplo anterior, mostramos como declarar uma função de easing para uma animação de quadro chave. Este próximo exemplo aplica uma função de easing a uma animação **From**/**To**/**By**.

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

Quando uma função de easing é aplicada a uma animação **From**/**To**/**By**, ela altera as características de função ao longo do tempo relacionadas à maneira como o valor é interpolado entre **From** e **To** ao longo da [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) da animação. Sem uma função de easing, seria uma interpolação linear.

## <a name="span-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Animações de valor de objeto discreto

Um tipo de animação merece uma menção especial por ser a única maneira de aplicar um valor animado a propriedades que não são do tipo [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) ou [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color). Este é o [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) da animação de quadro chave. Animar usando valores [**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN) é diferente porque não há a possibilidade de interpolar os valores entre os quadros. Quando o [**KeyTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.keytime) do quadro é atingido, o valor animado é imediatamente definido como o valor especificado no **Value** do quadro chave. Porque não há nenhum interpolação, há apenas um quadro chave que você usar o **ObjectAnimationUsingKeyFrames** coleção quadros-chave: [**DiscreteObjectKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame).

O [**Value**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) de um [**DiscreteObjectKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) geralmente é definido com o uso da sintaxe de elemento de propriedade porque o valor do objeto que você está tentando definir geralmente não pode ser expressado como uma cadeia de caracteres para preencher **Value** na sintaxe de atributo. Você ainda pode usar a sintaxe de atributo se usar uma referência como [StaticResource](https://docs.microsoft.com/windows/uwp/xaml-platform/staticresource-markup-extension).

Um caso em que você verá um [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) usado nos modelos padrão é quando uma propriedade de modelo referencia um recurso [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Esses recursos são objetos [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), não apenas um valor [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), e usam recursos que são definidos como temas do sistema ([**ThemeDictionaries**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)). Eles podem ser atribuídos diretamente a um valor do tipo **Brush**, como [**TextBlock.Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground) e não precisam usar o direcionamento indireto. No entanto, como um **SolidColorBrush** não é [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) ou **Color**, você precisa usar um **ObjectAnimationUsingKeyFrames** para usar o recurso.

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Você também pode usar [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) para animar propriedades que usam um valor de enumeração. Este é outro exemplo de um estado denominado que se origina dos modelos padrão do Windows Runtime. Observe como ele define a propriedade [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) que recebe uma constante de enumeração [**Visibility**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility). Nesse caso, você pode definir o valor usando a sintaxe de atributo. Você só precisa do nome da constante não qualificado de uma enumeração para definir uma propriedade com um valor de enumeração, por exemplo "Collapsed".

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

Você pode usar mais de um [**DiscreteObjectKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) para um conjunto de quadros [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). Essa pode ser uma maneira interessante de criar uma animação de "apresentação de slides" animando o valor de [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source), como um cenário de exemplo em que vários valores de objeto podem ser úteis.

 ## <a name="related-topics"></a>Tópicos relacionados

* [Sintaxe do caminho da propriedade](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)
* [Visão geral das propriedades de dependência](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)
* [**storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard.TargetProperty**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)
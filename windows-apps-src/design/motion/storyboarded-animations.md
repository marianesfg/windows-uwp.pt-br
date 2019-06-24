---
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Animações com storyboard
description: As animações com storyboard não são apenas animações no sentido visual.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62f603a6ff5aadc1c3e5342db6a7d771f8c37a7b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320836"
---
# <a name="storyboarded-animations"></a>Animações com storyboard

As animações com storyboard não são apenas animações no sentido visual. Uma animação com storyboard é uma maneira de alterar o valor de uma propriedade de dependência como uma função de tempo. Um dos principais motivos pelos quais você pode precisar de uma animação com storyboard que não faça parte da Biblioteca de Animação é para definir o estado visual de um controle, como parte de um modelo de controle ou de uma definição de página.

## <a name="differences-with-silverlight-and-wpf"></a>Diferenças com o Silverlight e WPF

Se você conhece o Microsoft Silverlight ou o WPF (Windows Presentation Foundation), leia esta seção; caso contrário, pode ignorá-la.

Em geral, a criação de animações com storyboard em um aplicativo do Windows Runtime é como o Silverlight ou o WPF. Porém, existem muitas diferenças importantes:

-   As animações com storyboard não são a única maneira de animar visualmente uma interface do usuário, e não são necessariamente a maneira mais fácil de os desenvolvedores de aplicativos fazerem isso. Em vez de usar animações com storyboard, geralmente, é uma prática de design mais adequada usar animações de tema e animações de transição. Elas podem rapidamente criar animações da interface do usuário recomendadas sem entrar nas complexidades da segmentação da propriedade de animação. Para obter mais informações, consulte [Visão geral das animações](xaml-animation.md).
-   No Windows Runtime, muitos controles XAML incluem animações de tema e de transição como parte de seu funcionamento interno. Em grande parte, os controles do WPF e do Silverlight não tinham um comportamento de animação padrão.
-   Nem todas as animações personalizadas que você cria podem ser executadas por padrão em um aplicativo do Windows Runtime quando o sistema de animação determina que elas podem comprometer o desempenho da sua interface do usuário. As animações nas quais o sistema determina que pode haver um impacto no desempenho são denominadas *animações dependentes*. É dependente porque a sincronia de sua animação está funcionando diretamente em relação ao thread de interface do usuário, que é onde a entrada do usuário ativo e outras atualizações também estão tentando aplicar as alterações de tempo de execução para a interface do usuário. Uma animação dependente que está consumindo muitos recursos de sistema no thread de interface do usuário pode fazer com que o aplicativo pareça não responder em determinadas situações. Quando sua animação gera uma alteração de layout ou tem a capacidade de afetar o desempenho no thread de interface do usuário, você geralmente precisa habilitá-la explicitamente para que ela seja executada. É para isso que serve a propriedade **EnableDependentAnimation** em classes de animação específicas. Para obter mais informações, consulte [Animações dependentes e independentes](./storyboarded-animations.md#dependent-and-independent-animations) .
-   No momento, não há suporte a funções de easing personalizadas no Windows Runtime.

## <a name="defining-storyboarded-animations"></a>Definindo animações de storyboard

Uma animação com storyboard é uma maneira de alterar o valor de uma propriedade de dependência como uma função de tempo. A propriedade que você está animando nem sempre é uma propriedade que afeta diretamente a interface do usuário do seu aplicativo. Porém, como o XAML envolve a definição da interface de um aplicativo, geralmente, são propriedades relacionadas à interface do usuário que são animadas. Por exemplo, é possível animar o ângulo de um [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) ou o valor de cor da tela de fundo de um botão.

Um dos principais motivos pelos quais você pode definir uma animação de storyboard é se for um autor de controle ou se estiver remodelando um controle e definindo estados visuais. Para obter mais informações, consulte [Animações de storyboard para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

Tanto se você estiver definindo estados visuais ou uma animação personalizada para um aplicativo, os conceitos e as APIs de animações de storyboard descritos neste tópico geralmente são aplicáveis a ambos.

Para ser animada, a propriedade à qual você está direcionando uma animação de storyboard deve ser uma *propriedade de dependência*. Uma propriedade de dependência é um recurso essencial da implementação de XAML do Windows Runtime. As propriedades que podem ser criadas dos elementos mais comuns da interface do usuário geralmente são implementadas como propriedades de dependência, de modo que você pode animá-las, aplicar valores vinculados a dados ou aplicar um [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) e direcionar um [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) à propriedade. Para saber mais sobre como as propriedades de dependência funcionam, consulte [Visão geral das propriedades de dependência](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).

Em geral, uma animação com storyboard é definida através da criação de XAML. Se você usar uma ferramenta como o Microsoft Visual Studio, ele criará o XAML para você. Também é possível definir uma animação com storyboard usando código, mas é menos comum.

Consultemos um exemplo simples. Neste XAML de exemplo, a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) é animada em um objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) específico.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>Identificando o objeto a ser animado

No exemplo anterior, o storyboard animou a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). As animações não são declaradas no objeto propriamente dito. Em vez disso, isso é feito na definição da animação de um storyboard. Normalmente, os storyboards são definidos no XAML que não está nas proximidades imediatas da definição da interface do usuário XAML do objeto a ser animado. Em vez disso, eles geralmente são configurados como um recurso XAML.

Para conectar uma animação a um destino, você deve referenciar o destino por seu nome de programação de identificação. Sempre aplique o [atributo x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) na definição da interface do usuário do XAML para nomear o objeto que você deseja animar. Em seguida, direcione o objeto a ser animado definindo [**Storyboard.TargetName**](https://docs.microsoft.com/dotnet/api/system.windows.media.animation.storyboard.targetname?view=netframework-4.8) na definição da animação. Para o valor de **Storyboard.TargetName**, use a cadeia de caracteres do nome do objeto de destino que foi definido anteriormente em outro lugar com o atributo x:Name.

### <a name="targeting-the-dependency-property-to-animate"></a>Direcionando a propriedade de dependência a ser animada

Defina um valor para [**Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)) na animação. Isso determina qual propriedade específica do objeto direcionado será animada.

Às vezes, é necessário direcionar uma propriedade que não é uma propriedade imediata do objeto de destino, mas que está aninhada mais profundamente em uma relação objeto-propriedade. Geralmente é necessário fazer isso para filtrar um conjunto de valores de objeto e propriedade auxiliares até que seja possível referenciar um tipo de propriedade que pode ser animado ([**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)). Esse conceito se chama *direcionamento indireto*, e a sintaxe para direcionar uma propriedade dessa maneira é conhecida como *caminho de propriedade*.

Aqui está um exemplo. Um cenário comum de uma animação de storyboard é alterar a cor de uma parte da interface do usuário de um aplicativo ou um controle para representar o estado específico desse controle. Suponha que você deseje animar o [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground) de um [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para que ele mude de vermelho para verde. Você esperaria que um [**ColorAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation) fosse usado e isso está correto. Porém, na realidade, nenhuma das propriedades nos elementos da interface do usuário que afetam a cor do objeto é do tipo [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color). Em vez disso, são do tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Assim, o que realmente é necessário direcionar para animação é a propriedade [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) da classe [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), que é um tipo derivado de **Brush** geralmente usado para essas propriedades da interface do usuário relacionadas a cores. Consulte a seguir a formação de um caminho de propriedade para o direcionamento de propriedades da sua animação:

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

Consulte a seguir uma representação das partes da sintaxe:

- Cada conjunto de parênteses () engloba um nome de propriedade.
- No nome da propriedade, há um ponto que separa o nome do tipo e o nome da propriedade, para que a propriedade sendo identificada não seja ambígua.
- O ponto no meio (fora dos parênteses) é uma etapa. Isso é interpretado pela sintaxe como o seguinte procedimento: extrair o valor da primeira propriedade (que é um objeto), entrar em seu modelo de objeto e direcionar uma subpropriedade específica do valor da primeira propriedade.

Consulte a seguir uma lista dos cenários de direcionamento de animação em que você provavelmente usará o direcionamento indireto de propriedades e algumas cadeias de caracteres de caminho de propriedade semelhantes à sintaxe a ser usada:

- Animando o valor [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) de um [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), conforme aplicado a um [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform): `(UIElement.RenderTransform).(TranslateTransform.X)`
- Animando um [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) em um [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) de um [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush), conforme aplicado a um [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- Animando o valor [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) de um [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), que é 1 de 4 transformações em um [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup), conforme aplicado a um [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform):`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Você perceberá que alguns desses exemplos usam colchetes ao redor dos números. Isso é um indexador. Ele indica que o nome da propriedade anterior tem uma coleção como valor e que você deseja um item (identificado por um índice baseado em zero) dessa coleção.

Você também pode animar propriedades anexadas XAML. Sempre coloque o nome completo da propriedade anexada em parênteses, como em `(Canvas.Left)`. Para obter mais informações, consulte [Animando propriedades anexadas XAML](./storyboarded-animations.md#animating-xaml-attached-properties).

Para saber mais sobre como usar um caminho de propriedade para o direcionamento indireto da propriedade a ser animada, consulte [Sintaxe de Property-path](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax) ou [**Propriedade anexada Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

### <a name="animation-types"></a>Tipos de animação

O sistema de animação do Windows Runtime possui três tipos específicos aos quais animações de storyboard podem ser aplicadas:

-   [**Duplo**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN), podem ser animados com qualquer [ **DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation)
-   [**Ponto**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), podem ser animados com qualquer [ **PointAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointAnimation)
-   [**Cor**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), podem ser animados com qualquer [ **ColorAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation)

Também há um tipo de animação [**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN) generalizado para valores de referência de objetos que abordaremos mais adiante.

### <a name="specifying-the-animated-values"></a>Especificando os valores animados

Até agora, mostramos como direcionar o objeto e a propriedade a ser animada, mas ainda não descrevemos o que a animação faz com o valor da propriedade durante sua execução.

Os tipos de animação descritos as vezes são chamados de animações **From**/**To**/**By**. Isso significa que a animação altera o valor de uma propriedade com o passar do tempo usando uma ou mais dessas entradas provenientes da definição da animação:

-   O valor começa no valor **From**. Se você não especificar um valor **From**, o valor inicial será qualquer valor contido na propriedade animada no momento que precede a execução da animação. Ele pode ser um valor padrão, um valor de um estilo ou modelo ou um valor aplicado especificamente por uma definição de interface do usuário XAML ou pelo código do aplicativo.
-   No final da animação, o valor será o valor **To**.
-   Ou, para especificar um valor final relativo ao valor inicial, defina a propriedade **By**. Você a definiria em vez da propriedade **To**.
-   Se você não especificar um valor **To** ou um valor **By**, o valor final será qualquer valor contido na propriedade animada no momento que precede a execução da animação. Nesse caso, é melhor ter um valor **From**, pois, caso contrário, a animação simplesmente não mudará o valor. Seus valores inicial e final serão os mesmos.
-   Uma animação geralmente tem pelo menos um **From**, **By** ou **To**, mas nunca os três de uma vez.

Vamos revisar o exemplo de XAML anterior e observar novamente os valores **From**, **To** e **Duration**. O exemplo anima a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) e o tipo de **Opacity** é [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN). Assim, a animação a ser usada aqui é [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation).

`From="1.0" To="0.0"` especifica que, quando a animação for executada, a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) comece no valor 1 e seja animada em 0. Em outras palavras, em termos do que esses valores [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) significam para a propriedade **Opacity**, essa animação fará com que o objeto comece opaco e depois fique transparente.

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` especifica quanto tempo a animação dura, ou seja, a rapidez com que o retângulo é esmaecido. A propriedade [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) é especificada na forma de *hours*:*minutes*:*seconds*. A duração nesse exemplo é de um segundo.

Para saber mais sobre os valores [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) e a sintaxe XAML, consulte [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration).

> [!NOTE]
> No exemplo mostrado, se você tivesse certeza de que o estado inicial do objeto que está sendo animado tem [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) sempre igual a 1 por meio do padrão ou de uma definição explícita, poderia omitir o valor **From**. Nesse caso, a animação usaria o valor inicial implícito e o resultado seria o mesmo.

### <a name="fromtoby-are-nullable"></a>From/To/By permitem valor nulo

Mencionamos anteriormente que você pode omitir **From**, **To** ou **By** e, assim, usar valores não animados atuais como substitutos de um valor ausente. Não é possível adivinhar as propriedades **From**, **To** ou **By** de uma animação. Por exemplo, o tipo da propriedade [**DoubleAnimation.To**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) não é [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN). Em vez disso, é um [**Nullable**](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN) para **Double**. Além disso, seu valor padrão é **null**, não 0. É com esse valor **null** que o sistema de animação distingue que você não definiu especificamente um valor para uma propriedade **From**, **To** ou **By**. Visual C++ extensões de componentes (C++/CX) não tem um **Nullable** digitar, portanto, ele usa [ **IReference** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_) em vez disso.

### <a name="other-properties-of-an-animation"></a>Outras propriedades de uma animação

As próximas propriedades descritas nesta seção são todas opcionais, pois têm padrões que são adequados para a maioria das animações.

### <a name="autoreverse"></a>**AutoReverse**

Se você não especificar [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse) ou [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) em uma animação, essa animação será executada uma única vez e durante o tempo especificado como [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration).

A propriedade [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse) especifica se uma linha do tempo será executada ao contrário após atingir o final de [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration). Se você defini-la como **true**, a animação será invertida depois que atingir o final de sua [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) declarada, alterando o valor do valor final (**To**) para o valor inicial (**From**). Desse modo, a animação será efetivamente executada pelo dobro do tempo em [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration).

### <a name="repeatbehavior"></a>**RepeatBehavior**

A propriedade [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) especifica quantas vezes uma linha do tempo deve ser executada ou uma duração maior na qual a linha do tempo deve ser repetida. Por padrão, uma linha do tempo tem uma contagem de iteração de "1x", o que significa que será executada uma vez por sua [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) e não será repetida.

Você pode fazer com que a animação execute várias iterações. Por exemplo, um valor "3x" faz com que a animação seja executada três vezes. Você também pode especificar uma [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) diferente para [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior). Essa **Duration** deve ser maior do que a **Duration** da própria animação para ser efetiva. Por exemplo, se você especificar um **RepeatBehavior** de "0:0:10" para uma animação com uma [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) de "0:0:2", essa animação será repetida cinco vezes. Se não houver uma divisão proporcional, a animação ficará truncada no momento em que o tempo de **RepeatBehavior** for atingido, que pode estar pela metade. Por fim, você pode especificar o valor especial "Forever", que faz com que a animação seja executada infinitamente até ser interrompida manualmente.

Para saber mais sobre os valores [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior) e a sintaxe XAML, consulte [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior).

### <a name="fillbehaviorstop"></a>**FillBehavior="Stop"**

Por padrão, quando uma animação termina, ela deixa o valor da propriedade como o **To** final ou o valor modificado por **By** mesmo depois que sua duração é ultrapassada. No entanto, se você definir o valor da propriedade [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) como [**FillBehavior.Stop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior), o valor do valor animado será revertido para o valor anterior à aplicação da animação ou, mais precisamente, o valor efetivo atual conforme determinado pelo sistema de propriedades de dependência (para saber mais sobre essa distinção, consulte [Visão geral das propriedades de dependência](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)).

### <a name="begintime"></a>**BeginTime**

Por padrão, o [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime) de uma animação é "0:0:0", de modo que ele é iniciado assim que o [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) que o contém é executado. É possível alterar isso se o **Storyboard** contiver mais de uma animação e você desejar programar horários de início diferentes para a animação inicial e as outras animações ou criar um pequeno atraso deliberado.

### <a name="speedratio"></a>**SpeedRatio**

Se houver mais de uma animação em um [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), você pode alterar a taxa de tempo de uma ou mais das animações relativamente ao **Storyboard**. É o **Storyboard** pai que acabará controlando como o tempo de [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) é transcorrido durante a execução da animação. Essa propriedade não é usada com frequência. Para obter mais informações, consulte [**SpeedRatio**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.speedratio).

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>Definindo mais de uma animação em um **Storyboard**

Um [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) pode conter mais de uma definição de animação. Você pode ter mais de uma animação se for aplicar animações relacionadas a duas propriedades do mesmo objeto de destino. Por exemplo, você pode alterar as propriedades [**TranslateX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatex) e [**TranslateY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatey) de um [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) usado como o [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) de um elemento da interface do usuário. Isso fará com que o elemento seja convertido diagonalmente. Você precisa de duas animações diferentes para fazer isso, mas as animações devem fazer parte do mesmo **Storyboard** porque elas sempre deverão ser executadas juntas.

As animações não precisam ser do mesmo tipo ou direcionar mesmo objeto. Elas podem ter durações diferentes e não precisam compartilhar valores de propriedades.

Quando o [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) pai for executado, cada uma das animações internas também será executada.

Na realidade, a classe [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) tem muitas das mesmas propriedades de animação que os tipos de animação têm, pois ambos compartilham a classe base [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline). Assim, um **Storyboard** pode ter um [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) ou um [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime). Geralmente, eles não são definidos em um **Storyboard**, a menos que você queria que todas as animações contidas tenham esse comportamento. Como regra geral, qualquer propriedade **Timeline** definida em um **Storyboard** aplica-se a todas as animações filhas. Se não for definida, **Storyboard** terá uma duração implícita que é calculada a partir do maior valor [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) das animações contidas. Uma [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) definida explicitamente em um **Storyboard** que é menor do que uma de suas animações filhas fará com que essa animação seja cortada, o que geralmente não é desejado.

Um storyboard não pode conter duas animações que tentam direcionar e animar a mesma propriedade no mesmo objeto. Se você tentar isso, receberá um erro de tempo de execução quando o storyboard tentar ser executado. Essa restrição se aplicará até mesmo se as animações não se sobrepuserem no tempo devido a valores [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime) e durações deliberadamente diferentes. Se você realmente desejar aplicar uma linha do tempo de animação mais complexa à mesma propriedade em um único storyboard, a maneira de fazer isso é usar uma animação de quadro chave. Consulte [Animações de quadro chave e com função de easing](key-frame-and-easing-function-animations.md).

O sistema de animação pode aplicar mais de uma animação ao valor de uma propriedade quando essas entradas vierem de vários storyboards. Não é comum usar esse comportamento deliberadamente para executar storyboards simultaneamente. No entanto, é possível que uma animação definida por aplicativo que você aplica a uma propriedade de controle modifique o valor **HoldEnd** de uma animação que foi executada anteriormente como parte do modelo de estado visual do controle.

## <a name="defining-a-storyboard-as-a-resource"></a>Definindo um storyboard como um recurso

Um [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) é o contêiner no qual você coloca objetos de animação. Você geralmente define o **Storyboard** como um recurso disponível para o objeto que desejar animar no [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) ou [**Application.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources) no nível da página.

O próximo exemplo mostra como o exemplo anterior [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) seria contido em uma definição [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) no nível da página, em que o **Storyboard** é um recurso de chave do [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) raiz. Observe o [atributo x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). É com esse atributo que você define um nome de variável para o **Storyboard**, para que outros elementos no XAML bem como o código possam consultar o **Storyboard** posteriormente.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

A definição de recursos na raiz do XAML de um arquivo XAML, como page.xaml ou app.xaml, é uma prática comum para organizar recursos de chave em seu XAML. Você também pode dividir recursos em arquivos separá-los e mesclá-los em aplicativos ou páginas. Para obter mais informações, consulte [Referências de recursos de ResourceDictionary e XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

> [!NOTE]
> O XAML do Windows Runtime oferece suporte à identificação de recursos pelo uso do [atributo x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) ou do [atributo x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). O uso do atributo x:Name é mais comum para um [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), pois você irá querer referenciá-lo pelo nome de variável em algum momento para poder chamar seu método [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) e executar as animações. Se você usar o [atributo x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), precisará usar métodos [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) como o indexador [**Item**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.item) para recuperá-lo como um recurso de chave e depois converter o objeto recuperado em **Storyboard** para usar os métodos **Storyboard**.

### <a name="storyboards-for-visual-states"></a>Storyboards para estados visuais

Você também pode colocar suas animações em uma unidade de [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) quando está declarando as animações de estado visual para uma aparência visual do controle. Nesse caso, os elementos **Storyboard** definidos vão para um contêiner de [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) que está aninhado mais profundamente em um [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) (é o **Style** que representa o recurso de chave). Você não precisa de uma chave nem nome para seu **Storyboard** nesse caso, porque é o **VisualState** que tem um nome de destino a ser invocado pelo [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager). Os estilos para controles são geralmente divididos em arquivos XAML [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) separados em vez de serem colocados em uma página ou coleção de **Resources** do aplicativo. Para obter mais informações, consulte [Animações de storyboard para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

## <a name="dependent-and-independent-animations"></a>Animações dependentes e independentes

Neste ponto, precisamos introduzir alguns aspectos importantes sobre como o sistema de animação funciona. Em particular, a animação interage fundamentalmente com a forma como um aplicativo do Windows Runtime é renderizado na tela e com a forma como essa renderização usa threads de processamento. Um aplicativo do Windows Runtime sempre tem um thread principal da interface do usuário e esse thread é responsável por atualizar a tela com as informações atuais. Além disso, um aplicativo do Windows Runtime tem um thread de composição, que é usado para pré-calcular os layouts imediatamente antes de serem exibidos. Quando você anima a interface do usuário, há a possibilidade de causar uma grande quantidade de trabalho para o thread da interface do usuário. O sistema precisa redesenhar áreas grandes da tela usado intervalos de tempo relativamente curtos entre cada atualização. Isso é necessário para capturar o valor mais recente da propriedade animada. Se você não tomar cuidado, há um risco de uma animação diminuir a capacidade de resposta da interface do usuário ou comprometer o desempenho de outros recursos do aplicativo que também estão no mesmo thread da interface do usuário.

As diversas animações que apresentam risco de deixar o thread da interface do usuário lento são chamadas de *animações dependentes*. Uma animação não sujeita a esse risco é uma *animação independente*. A distinção entre animações dependentes e independentes não é determinada apenas pelos tipos de animação ([**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) etc.) como descrevemos anteriormente. Em vez disso, ela é determinada pelas propriedades específicas sendo animadas e outros fatores como herança e composição de controles. Há circunstâncias em que, até mesmo se uma animação alterar a interface do usuário, ela pode ter um impacto mínimo sobre o thread da interface do usuário e pode ser manipulada pelo thread de composição como uma animação independente.

Uma animação é independente quando tem qualquer uma destas características:

-   A [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) da animação é de 0 segundo (veja o aviso)
-   A animação direciona [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   A animação atinge um valor de subpropriedade desses [ **UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) propriedades: [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection), [**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)
-   A animação direciona [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left?view=netframework-4.8) ou [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top?view=netframework-4.8)
-   A animação direciona um valor [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) e usa um [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), animando seu [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   A animação é um [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)

> [!WARNING]
> Para que sua animação seja tratada como independente, você deve definir explicitamente a `Duration="0"`. Por exemplo, se você remover a `Duration="0"` desse XAML, a animação será tratada como dependente, mesmo que o [**KeyTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doublekeyframe.keytime) do quadro seja "0:0:0".

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Se sua animação não atender a esses critérios, provavelmente é uma animação dependente. Por padrão, o sistema de animação não executa animações dependentes. Portanto, durante o processo de desenvolvimento e teste, você talvez nem veja sua animação em execução. Você ainda poderá usar essa animação, mas deverá habilitar especificamente cada animação dependente. Para habilitar sua animação, defina a propriedade **EnableDependentAnimation** do objeto de animação como **true**. Cada subclasse [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) que representa uma animação tem uma implementação diferente da propriedade, mas todas são chamadas `EnableDependentAnimation`.

A obrigação do desenvolvedor de aplicativo em habilitar animações dependentes é um aspecto de criação reconhecido dentro da experiência de desenvolvimento do sistema de animação. Queremos que os desenvolvedores reconheçam o quanto as animações influenciam o desempenho da capacidade de resposta da interface do usuário. Animações com desempenho insatisfatório são difíceis de isolar e depurar em um aplicativo completo. Portanto, é indicado habilitar somente as animações dependentes que são essenciais para a experiência da interface do usuário do seu aplicativo. Quisemos evitar que o desempenho do seu aplicativo fosse comprometido por conta de animações decorativas que usam muitos ciclos. Para obter mais dicas sobre o desempenho de animações, consulte [Otimizar animações e mídia](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media).

Como um desenvolvedor de aplicativo, você também pode optar por aplicar uma configuração válida para todo o aplicativo que sempre desabilite animações dependentes, até mesmo aquelas em que **EnableDependentAnimation** é **true**. Consulte [**Timeline.AllowDependentAnimations**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

> [!TIP]
> Se você estiver usando o painel Animação no Blend for Visual Studio 2017, sempre que você tentar aplicar uma animação dependente a uma propriedade de estado visual, os avisos serão exibidos no designer. Avisos não serão exibidas na saída da compilação ou lista de erros. Se você estiver editando o XAML manualmente, o designer não mostrará um aviso. Em tempo de execução durante a depuração, a saída de depuração do painel de saída mostrará um aviso de que a animação não é independente e será ignorada.


## <a name="starting-and-controlling-an-animation"></a>Iniciando e controlando uma animação

Tudo que mostramos até agora não faz com que uma animação seja realmente executada ou aplicada. Até que a animação seja iniciada e esteja em execução, as alterações de valores que uma animação declara no XAML são latentes e ainda não ocorrerão. Você deve iniciar explicitamente uma animação de alguma maneira relacionada ao tempo de vida do aplicativo ou à experiência do usuário. No nível mais simples, você inicia uma animação chamando o método [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) no [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) que é o pai dessa animação. Você pode chamar métodos diretamente do XAML, de modo que, seja lá o que fizer para habilitar suas animações, o fará a partir do código. Isso será o code-behind das páginas ou dos componentes do seu aplicativo ou talvez a lógica do seu controle se você estiver definindo uma classe de controle personalizada.

Geralmente, você chama [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) e apenas deixa a animação ser executada até a conclusão de sua duração. No entanto, você também pode usar os métodos [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause), [**Resume**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) e [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop) para controlar o [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) no tempo de execução, bem como outras APIs que são usadas para cenários mais avançados de controle de animação.

Quando você chama [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) em um storyboard que contém uma animação que se repete infinitamente (`RepeatBehavior="Forever"`), a animação é executada até que a página que a contém seja descarregada ou até que você chame especificamente [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause) ou [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop).

### <a name="starting-an-animation-from-app-code"></a>Iniciando uma animação a partir do código do aplicativo

Você pode iniciar animações automaticamente ou em resposta a ações do usuário. Para o caso automático, você geralmente usa um evento de tempo de vida de objeto como [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) para atuar como o gatilho da animação. O evento **Loaded** é um bom evento a ser usado para essa finalidade porque, nesse ponto, a interface do usuário está pronta para interação e a animação não será cortada no início porque outra parte da interface do usuário ainda está sendo carregada.

Neste exemplo, o evento [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) está anexado ao retângulo para que, quando o usuário clicar no retângulo, a animação comece.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

O manipulador de eventos inicia o [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) (a animação) usando o método [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) do **Storyboard**.

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

Você pode manipular o evento [**Completed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.completed) se desejar executar outra lógica após a animação terminar a aplicação de valores. Além disso, para solucionar problemas de interações do sistema/animação de propriedade, o método [**GetAnimationBaseValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue) pode ser útil.

> [!TIP]
> Sempre que você criar código para um cenário de app em que uma animação será iniciada a partir do código do app, é recomendável verificar novamente se uma animação ou transição já existe na biblioteca de animações para o cenário da interface do usuário. As animações da biblioteca permitem uma experiência da interface do usuário mais consistente em todos os aplicativos do Windows Runtime e são mais fáceis de usar.

 

### <a name="animations-for-visual-states"></a>Animações para estados visuais

O comportamento de execução de um [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) que é usado para definir o estado visual de um controle é diferente da maneira como um aplicativo pode executar um storyboard diretamente. Quando aplicado a uma definição de estado visual no XAML, o **Storyboard** é um elemento de um [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) contêiner e o estado como um todo é controlado pelo uso da API [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager). Qualquer animação interna será executada de acordo com seus valores de animação e propriedades [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) quando o **VisualState** contêiner for usado por um controle. Para obter mais informações, consulte [Storyboards para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)). Para estados visuais, o [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) aparente é diferente. Se um estado visual for alterado para outro estado, todas as alterações de propriedade aplicadas pelo estado visual anterior e suas animações serão cancelados, mesmo se o novo estado visual não aplicar especificamente uma nova animação a uma propriedade.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** e **EventTrigger**

Só há uma maneira de iniciar uma animação que pode ser declarada completamente no XAML. No entanto, essa técnica já não é mais usada com freqüência. Trata-se de uma sintaxe herdada do WPF e de versões anteriores do Silverlight antes do suporte a [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager). Essa sintaxe [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger) ainda funciona no XAML do Windows Runtime para fins de importação/compatibilidade, mas só funciona para um comportamento de gatilho baseado no evento [**FrameworkElement.Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded). Uma tentativa de acionar outros eventos lançará exceções ou causará falha na compilação. Para obter mais informações, consulte [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger) ou [**BeginStoryboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BeginStoryboard).

## <a name="animating-xaml-attached-properties"></a>Animando propriedades anexadas XAML

Não é um cenário comum, mas é possível aplicar um valor animado a uma propriedade anexada XAML. Para saber mais sobre o que são as propriedades anexadas e como elas funcionam, consulte [Visão geral das propriedades anexadas](https://docs.microsoft.com/windows/uwp/xaml-platform/attached-properties-overview). O direcionamento de uma propriedade anexada exige uma [sintaxe de property-path](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax) que coloca o nome da propriedade entre parênteses. Você pode animar as propriedades anexadas internas, como [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)), usando uma [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) que aplica valores inteiros discretos. Entretanto, uma limitação existente da implementação XAML de Windows Runtime é que não é possível animar uma propriedade anexada personalizada.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>Mais tipos de animação e próximas etapas para saber mais sobre como animar a interface do usuário

Até agora, mostramos as animações personalizadas que são animadas entre dois valores e depois interpolam linearmente os valores conforme necessário durante a execução da animação. Elas são chamadas de animações **From**/**To**/**By** Mas há outro tipo de animação que permite declarar valores intermediários que ficam entre o início e o fim. Elas são chamadas de *animações de quadro chave*. Há também uma maneira de alterar a lógica de interpolação em uma animação **From**/**To**/**By** ou uma animação de quadro chave. Isso envolve a aplicação de uma função de easing. Para saber mais sobre esses conceitos, consulte [Animações de quadro chave e com função de easing](key-frame-and-easing-function-animations.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Sintaxe do caminho da propriedade](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)
* [Visão geral das propriedades de dependência](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)
* [Quadro-chave e animações de função de easing](key-frame-and-easing-function-animations.md)
* [Animações storyboarded para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10))
* [Modelos de controle](https://docs.microsoft.com/windows/uwp/controls-and-patterns/control-templates)
* [**storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95))
 

 





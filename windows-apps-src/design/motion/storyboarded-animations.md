---
author: Jwmsft
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Animações com storyboard
description: As animações com storyboard não são apenas animações no sentido visual.
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c03d99781114c4fefff04cc25930748ec16182f
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2906302"
---
# <a name="storyboarded-animations"></a>Animações de storyboard

As animações com storyboard não são apenas animações no sentido visual. Uma animação com storyboard é uma maneira de alterar o valor de uma propriedade de dependência como uma função de tempo. Um dos principais motivos pelos quais você pode precisar de uma animação com storyboard que não faça parte da Biblioteca de Animação é para definir o estado visual de um controle, como parte de um modelo de controle ou de uma definição de página.

## <a name="differences-with-silverlight-and-wpf"></a>Diferenças com o Silverlight e WPF

Se você conhece o Microsoft Silverlight ou o WPF (Windows Presentation Foundation), leia esta seção; caso contrário, pode ignorá-la.

Em geral, a criação de animações com storyboard em um aplicativo do Windows Runtime é como o Silverlight ou o WPF. Porém, existem muitas diferenças importantes:

-   As animações com storyboard não são a única maneira de animar visualmente uma interface do usuário, e não são necessariamente a maneira mais fácil de os desenvolvedores de aplicativos fazerem isso. Em vez de usar animações com storyboard, geralmente, é uma prática de design mais adequada usar animações de tema e animações de transição. Elas podem rapidamente criar animações da interface do usuário recomendadas sem entrar nas complexidades da segmentação da propriedade de animação. Para obter mais informações, consulte [Visão geral das animações](xaml-animation.md).
-   No Windows Runtime, muitos controles XAML incluem animações de tema e de transição como parte de seu funcionamento interno. Em grande parte, os controles do WPF e do Silverlight não tinham um comportamento de animação padrão.
-   Nem todas as animações personalizadas que você cria podem ser executadas por padrão em um aplicativo do Windows Runtime quando o sistema de animação determina que elas podem comprometer o desempenho da sua interface do usuário. As animações nas quais o sistema determina que pode haver um impacto no desempenho são denominadas *animações dependentes*. É dependente porque a sincronia de sua animação está funcionando diretamente em relação ao thread de interface do usuário, que é onde a entrada do usuário ativo e outras atualizações também estão tentando aplicar as alterações de tempo de execução para a interface do usuário. Uma animação dependente que está consumindo muitos recursos de sistema no thread de interface do usuário pode fazer com que o aplicativo pareça não responder em determinadas situações. Quando sua animação gera uma alteração de layout ou tem a capacidade de afetar o desempenho no thread de interface do usuário, você geralmente precisa habilitá-la explicitamente para que ela seja executada. É para isso que serve a propriedade **EnableDependentAnimation** em classes de animação específicas. Para obter mais informações, consulte [Animações dependentes e independentes](./storyboarded-animations.md#dependent-and-independent-animations) .
-   No momento, não há suporte a funções de easing personalizadas no Windows Runtime.

## <a name="defining-storyboarded-animations"></a>Definindo animações de storyboard

Uma animação de storyboard é uma maneira de alterar o valor de uma propriedade de dependência como uma função de tempo. A propriedade que você está animando nem sempre é uma propriedade que afeta diretamente a interface do usuário do seu aplicativo. Porém, como o XAML envolve a definição da interface de um aplicativo, geralmente, são propriedades relacionadas à interface do usuário que são animadas. Por exemplo, é possível animar o ângulo de um [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) ou o valor de cor da tela de fundo de um botão.

Um dos principais motivos pelos quais você pode definir uma animação de storyboard é se for um autor de controle ou se estiver remodelando um controle e definindo estados visuais. Para obter mais informações, consulte [Animações de storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

Tanto se você estiver definindo estados visuais ou uma animação personalizada para um aplicativo, os conceitos e as APIs de animações de storyboard descritos neste tópico geralmente são aplicáveis a ambos.

Para ser animada, a propriedade à qual você está direcionando uma animação de storyboard deve ser uma *propriedade de dependência*. Uma propriedade de dependência é um recurso essencial da implementação de XAML do Windows Runtime. As propriedades que podem ser criadas dos elementos mais comuns da interface do usuário geralmente são implementadas como propriedades de dependência, de modo que você pode animá-las, aplicar valores vinculados a dados ou aplicar um [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) e direcionar um [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) à propriedade. Para saber mais sobre como as propriedades de dependência funcionam, consulte [Visão geral das propriedades de dependência](https://msdn.microsoft.com/library/windows/apps/Mt185583).

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

Para conectar uma animação a um destino, você deve referenciar o destino por seu nome de programação de identificação. Sempre aplique o [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788) na definição da interface do usuário do XAML para nomear o objeto que você deseja animar. Em seguida, direcione o objeto a ser animado definindo [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823) na definição da animação. Para o valor de **Storyboard.TargetName**, use a cadeia de caracteres do nome do objeto de destino que foi definido anteriormente em outro lugar com o atributo x:Name.

### <a name="targeting-the-dependency-property-to-animate"></a>Direcionando a propriedade de dependência a ser animada

Defina um valor para [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824) na animação. Isso determina qual propriedade específica do objeto direcionado será animada.

Às vezes, é necessário direcionar uma propriedade que não é uma propriedade imediata do objeto de destino, mas que está aninhada mais profundamente em uma relação objeto-propriedade. Geralmente é necessário fazer isso para filtrar um conjunto de valores de objeto e propriedade auxiliares até que seja possível referenciar um tipo de propriedade que pode ser animado ([**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870), [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)). Esse conceito se chama *direcionamento indireto*, e a sintaxe para direcionar uma propriedade dessa maneira é conhecida como *caminho de propriedade*.

Consulte um exemplo. Um cenário comum de uma animação de storyboard é alterar a cor de uma parte da interface do usuário de um aplicativo ou um controle para representar o estado específico desse controle. Suponha que você deseje animar o [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) de um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) para que ele mude de vermelho para verde. Você esperaria que um [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066) fosse usado e isso está correto. Porém, na realidade, nenhuma das propriedades nos elementos da interface do usuário que afetam a cor do objeto é do tipo [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Em vez disso, são do tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Assim, o que realmente é necessário direcionar para animação é a propriedade [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) da classe [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), que é um tipo derivado de **Brush** geralmente usado para essas propriedades da interface do usuário relacionadas a cores. Consulte a seguir a formação de um caminho de propriedade para o direcionamento de propriedades da sua animação:

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

- Animando o valor [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) de um [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), conforme aplicado a um [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980): `(UIElement.RenderTransform).(TranslateTransform.X)`
- Animando um [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) em um [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) de um [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108), conforme aplicado a um [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- Animando o valor [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) de um [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), que é 1 de 4 transformações em um [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022), conforme aplicado a um [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980):`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Você perceberá que alguns desses exemplos usam colchetes ao redor dos números. Isso é um indexador. Ele indica que o nome da propriedade anterior tem uma coleção como valor e que você deseja um item (identificado por um índice baseado em zero) dessa coleção.

Você também pode animar propriedades anexadas XAML. Sempre coloque o nome completo da propriedade anexada em parênteses, como em `(Canvas.Left)`. Para obter mais informações, consulte [Animando propriedades anexadas XAML](./storyboarded-animations.md#animating-xaml-attached-properties).

Para saber mais sobre como usar um caminho de propriedade para o direcionamento indireto da propriedade a ser animada, consulte [Sintaxe de Property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586) ou [**Propriedade anexada Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824).

### <a name="animation-types"></a>Tipos de animação

O sistema de animação do Windows Runtime possui três tipos específicos aos quais animações de storyboard podem ser aplicadas:

-   [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) pode ser animado com qualquer [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)
-   [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) pode ser animado com qualquer [**PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346)
-   [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) pode ser animado com qualquer [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)

Também há um tipo de animação [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) generalizado para valores de referência de objetos que abordaremos mais adiante.

### <a name="specifying-the-animated-values"></a>Especificando os valores animados

Até agora, mostramos como direcionar o objeto e a propriedade a ser animada, mas ainda não descrevemos o que a animação faz com o valor da propriedade durante sua execução.

Os tipos de animação descritos as vezes são chamados de animações **From**/**To**/**By**. Isso significa que a animação altera o valor de uma propriedade com o passar do tempo usando uma ou mais dessas entradas provenientes da definição da animação:

-   O valor começa no valor **From**. Se você não especificar um valor **From**, o valor inicial será qualquer valor contido na propriedade animada no momento que precede a execução da animação. Ele pode ser um valor padrão, um valor de um estilo ou modelo ou um valor aplicado especificamente por uma definição de interface do usuário XAML ou pelo código do aplicativo.
-   No final da animação, o valor será o valor **To**.
-   Ou, para especificar um valor final relativo ao valor inicial, defina a propriedade **By**. Você a definiria em vez da propriedade **To**.
-   Se você não especificar um valor **To** ou um valor **By**, o valor final será qualquer valor contido na propriedade animada no momento que precede a execução da animação. Nesse caso, é melhor ter um valor **From**, pois, caso contrário, a animação simplesmente não mudará o valor. Seus valores inicial e final serão os mesmos.
-   Uma animação geralmente tem pelo menos um **From**, **By** ou **To**, mas nunca os três de uma vez.

Vamos revisar o exemplo de XAML anterior e observar novamente os valores **From**, **To** e **Duration**. O exemplo anima a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) e o tipo de **Opacity** é [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Assim, a animação a ser usada aqui é [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136).

`From="1.0" To="0.0"` especifica que, quando a animação for executada, a propriedade [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) comece no valor 1 e seja animada em 0. Em outras palavras, em termos do que esses valores [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) significam para a propriedade **Opacity**, essa animação fará com que o objeto comece opaco e depois fique transparente.

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

`Duration="0:0:1"` especifica quanto tempo a animação dura, ou seja, a rapidez com que o retângulo é esmaecido. A propriedade [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) é especificada na forma de *hours*:*minutes*:*seconds*. A duração nesse exemplo é de um segundo.

Para saber mais sobre os valores [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) e a sintaxe XAML, consulte [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377).

> [!NOTE]
> No exemplo mostrado, se você tivesse certeza de que o estado inicial do objeto que está sendo animado tem [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) sempre igual a 1 por meio do padrão ou de uma definição explícita, poderia omitir o valor **From**. Nesse caso, a animação usaria o valor inicial implícito e o resultado seria o mesmo.

### <a name="fromtoby-are-nullable"></a>From/To/By permitem valor nulo

Mencionamos anteriormente que você pode omitir **From**, **To** ou **By** e, assim, usar valores não animados atuais como substitutos de um valor ausente. Não é possível adivinhar as propriedades **From**, **To** ou **By** de uma animação. Por exemplo, o tipo da propriedade [**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) não é [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Em vez disso, é um [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) para **Double**. Além disso, seu valor padrão é **null**, não 0. É com esse valor **null** que o sistema de animação distingue que você não definiu especificamente um valor para uma propriedade **From**, **To** ou **By**. As extensões de componente do VisualC++ (C++/CX) não têm um tipo **Nullable** e, em vez dele, usam [**IReference**](https://msdn.microsoft.com/library/windows/apps/BR225864).

### <a name="other-properties-of-an-animation"></a>Outras propriedades de uma animação

As próximas propriedades descritas nesta seção são todas opcionais, pois têm padrões que são adequados para a maioria das animações.

### **<a name="autoreverse"></a>AutoReverse**

Se você não especificar [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) ou [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) em uma animação, essa animação será executada uma única vez e durante o tempo especificado como [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207).

A propriedade [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) especifica se uma linha do tempo será executada ao contrário após atingir o final de [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207). Se você defini-la como **true**, a animação será invertida depois que atingir o final de sua [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) declarada, alterando o valor do valor final (**To**) para o valor inicial (**From**). Desse modo, a animação será efetivamente executada pelo dobro do tempo em [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207).

### **<a name="repeatbehavior"></a>RepeatBehavior**

A propriedade [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) especifica quantas vezes uma linha do tempo deve ser executada ou uma duração maior na qual a linha do tempo deve ser repetida. Por padrão, uma linha do tempo tem uma contagem de iteração de "1x", o que significa que será executada uma vez por sua [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) e não será repetida.

Você pode fazer com que a animação execute várias iterações. Por exemplo, um valor "3x" faz com que a animação seja executada três vezes. Você também pode especificar uma [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) diferente para [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211). Essa **Duration** deve ser maior do que a **Duration** da própria animação para ser efetiva. Por exemplo, se você especificar um **RepeatBehavior** de "0:0:10" para uma animação com uma [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) de "0:0:2", essa animação será repetida cinco vezes. Se não houver uma divisão proporcional, a animação ficará truncada no momento em que o tempo de **RepeatBehavior** for atingido, que pode estar pela metade. Por fim, você pode especificar o valor especial "Forever", que faz com que a animação seja executada infinitamente até ser interrompida manualmente.

Para saber mais sobre os valores [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411) e a sintaxe XAML, consulte [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411).

### **<a name="fillbehaviorstop"></a>FillBehavior="Stop"**

Por padrão, quando uma animação termina, ela deixa o valor da propriedade como o **To** final ou o valor modificado por **By** mesmo depois que sua duração é ultrapassada. No entanto, se você definir o valor da propriedade [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) como [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306), o valor do valor animado será revertido para o valor anterior à aplicação da animação ou, mais precisamente, o valor efetivo atual conforme determinado pelo sistema de propriedades de dependência (para saber mais sobre essa distinção, consulte [Visão geral das propriedades de dependência](https://msdn.microsoft.com/library/windows/apps/Mt185583)).

### **<a name="begintime"></a>BeginTime**

Por padrão, o [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) de uma animação é "0:0:0", de modo que ele é iniciado assim que o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) que o contém é executado. É possível alterar isso se o **Storyboard** contiver mais de uma animação e você desejar programar horários de início diferentes para a animação inicial e as outras animações ou criar um pequeno atraso deliberado.

### **<a name="speedratio"></a>SpeedRatio**

Se houver mais de uma animação em um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490), você pode alterar a taxa de tempo de uma ou mais das animações relativamente ao **Storyboard**. É o **Storyboard** pai que acabará controlando como o tempo de [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) é transcorrido durante a execução da animação. Essa propriedade não é usada com frequência. Para obter mais informações, consulte [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213).

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>Definindo mais de uma animação em um **Storyboard**

Um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) pode conter mais de uma definição de animação. Você pode ter mais de uma animação se for aplicar animações relacionadas a duas propriedades do mesmo objeto de destino. Por exemplo, você pode alterar as propriedades [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122) e [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124) de um [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) usado como o [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) de um elemento da interface do usuário. Isso fará com que o elemento seja convertido diagonalmente. Você precisa de duas animações diferentes para fazer isso, mas as animações devem fazer parte do mesmo **Storyboard** porque elas sempre deverão ser executadas juntas.

As animações não precisam ser do mesmo tipo ou direcionar mesmo objeto. Elas podem ter durações diferentes e não precisam compartilhar valores de propriedades.

Quando o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) pai for executado, cada uma das animações internas também será executada.

Na realidade, a classe [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) tem muitas das mesmas propriedades de animação que os tipos de animação têm, pois ambos compartilham a classe base [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517). Assim, um **Storyboard** pode ter um [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) ou um [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204). Geralmente, eles não são definidos em um **Storyboard**, a menos que você queria que todas as animações contidas tenham esse comportamento. Como regra geral, qualquer propriedade **Timeline** definida em um **Storyboard** aplica-se a todas as animações filhas. Se não for definida, **Storyboard** terá uma duração implícita que é calculada a partir do maior valor [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) das animações contidas. Uma [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) definida explicitamente em um **Storyboard** que é menor do que uma de suas animações filhas fará com que essa animação seja cortada, o que geralmente não é desejado.

Um storyboard não pode conter duas animações que tentam direcionar e animar a mesma propriedade no mesmo objeto. Se você tentar isso, receberá um erro de tempo de execução quando o storyboard tentar ser executado. Essa restrição se aplicará até mesmo se as animações não se sobrepuserem no tempo devido a valores [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) e durações deliberadamente diferentes. Se você realmente desejar aplicar uma linha do tempo de animação mais complexa à mesma propriedade em um único storyboard, a maneira de fazer isso é usar uma animação de quadro chave. Consulte [Animações de quadro chave e com função de easing](key-frame-and-easing-function-animations.md).

O sistema de animação pode aplicar mais de uma animação ao valor de uma propriedade quando essas entradas vierem de vários storyboards. Não é comum usar esse comportamento deliberadamente para executar storyboards simultaneamente. No entanto, é possível que uma animação definida por aplicativo que você aplica a uma propriedade de controle modifique o valor **HoldEnd** de uma animação que foi executada anteriormente como parte do modelo de estado visual do controle.

## <a name="defining-a-storyboard-as-a-resource"></a>Definindo um storyboard como um recurso

Um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) é o contêiner no qual você coloca objetos de animação. Você geralmente define o **Storyboard** como um recurso disponível para o objeto que desejar animar no [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) ou [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338) no nível da página.

O próximo exemplo mostra como o exemplo anterior [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) seria contido em uma definição [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) no nível da página, em que o **Storyboard** é um recurso de chave do [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503) raiz. Observe o [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788). É com esse atributo que você define um nome de variável para o **Storyboard**, para que outros elementos no XAML bem como o código possam consultar o **Storyboard** posteriormente.

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

A definição de recursos na raiz do XAML de um arquivo XAML, como page.xaml ou app.xaml, é uma prática comum para organizar recursos de chave em seu XAML. Você também pode dividir recursos em arquivos separá-los e mesclá-los em aplicativos ou páginas. Para obter mais informações, consulte [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/Mt187273).

> [!NOTE]
> O XAML do Windows Runtime oferece suporte à identificação de recursos pelo uso do [atributo x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787) ou do [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788). O uso do atributo x:Name é mais comum para um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490), pois você irá querer referenciá-lo pelo nome de variável em algum momento para poder chamar seu método [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) e executar as animações. Se você usar o [atributo x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787), precisará usar métodos [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) como o indexador [**Item**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item) para recuperá-lo como um recurso de chave e depois converter o objeto recuperado em **Storyboard** para usar os métodos **Storyboard**.

### <a name="storyboards-for-visual-states"></a>Storyboards para estados visuais

Você também pode colocar suas animações em uma unidade de [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) quando está declarando as animações de estado visual para obter a aparência de um controle. Nesse caso, os elementos **Storyboard** definidos vão para um contêiner de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) que está aninhado mais profundamente em um [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) (é o **Style** que representa o recurso de chave). Você não precisa de uma chave nem nome para seu **Storyboard** nesse caso, porque é o **VisualState** que tem um nome de destino a ser invocado pelo [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Os estilos para controles são geralmente divididos em arquivos XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) separados em vez de serem colocados em uma página ou coleção de **Resources** do aplicativo. Para obter mais informações, consulte [Animações de storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

## <a name="dependent-and-independent-animations"></a>Animações dependentes e independentes

Neste ponto, precisamos introduzir alguns aspectos importantes sobre como o sistema de animação funciona. Em particular, a animação interage fundamentalmente com a forma como um aplicativo do Windows Runtime é renderizado na tela e com a forma como essa renderização usa threads de processamento. Um aplicativo do Windows Runtime sempre tem um thread principal da interface do usuário e esse thread é responsável por atualizar a tela com as informações atuais. Além disso, um aplicativo do Windows Runtime tem um thread de composição, que é usado para pré-calcular os layouts imediatamente antes de serem exibidos. Quando você anima a interface do usuário, há a possibilidade de causar uma grande quantidade de trabalho para o thread da interface do usuário. O sistema precisa redesenhar áreas grandes da tela usado intervalos de tempo relativamente curtos entre cada atualização. Isso é necessário para capturar o valor mais recente da propriedade animada. Se você não tomar cuidado, há um risco de uma animação diminuir a capacidade de resposta da interface do usuário ou comprometer o desempenho de outros recursos do aplicativo que também estão no mesmo thread da interface do usuário.

As diversas animações que apresentam risco de deixar o thread da interface do usuário lento são chamadas de *animações dependentes*. Uma animação não sujeita a esse risco é uma *animação independente*. A distinção entre animações dependentes e independentes não é determinada apenas pelos tipos de animação ([**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) etc.) como descrevemos anteriormente. Em vez disso, ela é determinada pelas propriedades específicas sendo animadas e outros fatores como herança e composição de controles. Há circunstâncias em que, até mesmo se uma animação alterar a interface do usuário, ela pode ter um impacto mínimo sobre o thread da interface do usuário e pode ser manipulada pelo thread de composição como uma animação independente.

Uma animação é independente quando tem qualquer uma destas características:

-   A [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) da animação é de 0 segundo (veja o aviso)
-   A animação direciona [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   A animação direciona um valor de subpropriedade destas propriedades [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911): [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx), [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)
-   A animação direciona [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) ou [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)
-   A animação direciona um valor [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) e usa um [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), animando seu [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   A animação é um [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)

> [!WARNING]
> Para que sua animação seja tratada como independente, você deve definir explicitamente a `Duration="0"`. Por exemplo, se você remover a `Duration="0"` desse XAML, a animação será tratada como dependente, mesmo que o [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169) do quadro seja "0:0:0".

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

Se sua animação não atender a esses critérios, provavelmente é uma animação dependente. Por padrão, o sistema de animação não executa animações dependentes. Portanto, durante o processo de desenvolvimento e teste, você talvez nem veja sua animação em execução. Você ainda poderá usar essa animação, mas deverá habilitar especificamente cada animação dependente. Para habilitar sua animação, defina a propriedade **EnableDependentAnimation** do objeto de animação como **true**. Cada subclasse [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) que representa uma animação tem uma implementação diferente da propriedade, mas todas são chamadas `EnableDependentAnimation`.

A obrigação do desenvolvedor de aplicativo em habilitar animações dependentes é um aspecto de criação reconhecido dentro da experiência de desenvolvimento do sistema de animação. Queremos que os desenvolvedores reconheçam o quanto as animações influenciam o desempenho da capacidade de resposta da interface do usuário. Animações com desempenho insatisfatório são difíceis de isolar e depurar em um aplicativo completo. Portanto, é recomendável ligar somente as animações dependentes que são essenciais para a experiência da interface do usuário do seu app. Quisemos evitar que o desempenho do seu aplicativo fosse comprometido por conta de animações decorativas que usam muitos ciclos. Para obter mais dicas sobre o desempenho de animações, consulte [Otimizar animações e mídia](https://msdn.microsoft.com/library/windows/apps/Mt204774).

Como um desenvolvedor de aplicativo, você também pode optar por aplicar uma configuração válida para todo o aplicativo que sempre desabilite animações dependentes, até mesmo aquelas em que **EnableDependentAnimation** é **true**. Consulte [**Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

> [!TIP]
> Se você estiver compondo estados visuais para um controle usando o Visual Studio, o designer gerará avisos sempre que você tentar aplicar uma animação dependente a uma propriedade de estado visual.

## <a name="starting-and-controlling-an-animation"></a>Iniciando e controlando uma animação

Tudo que mostramos até agora não faz com que uma animação seja realmente executada ou aplicada. Até que a animação seja iniciada e esteja em execução, as alterações de valores que uma animação declara no XAML são latentes e ainda não ocorrerão. Você deve iniciar explicitamente uma animação de alguma maneira relacionada ao tempo de vida do aplicativo ou à experiência do usuário. No nível mais simples, você inicia uma animação chamando o método [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) no [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) que é o pai dessa animação. Você pode chamar métodos diretamente do XAML, de modo que, seja lá o que fizer para habilitar suas animações, o fará a partir do código. Isso será o code-behind das páginas ou dos componentes do seu aplicativo ou talvez a lógica do seu controle se você estiver definindo uma classe de controle personalizada.

Geralmente, você chama [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) e apenas deixa a animação ser executada até a conclusão de sua duração. No entanto, você também pode usar os métodos [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx), [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) e [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) para controlar o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) no tempo de execução, bem como outras APIs que são usadas para cenários mais avançados de controle de animação.

Quando você chama [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) em um storyboard que contém uma animação que se repete infinitamente (`RepeatBehavior="Forever"`), a animação é executada até que a página que a contém seja descarregada ou até que você chame especificamente [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) ou [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop).

### <a name="starting-an-animation-from-app-code"></a>Iniciando uma animação a partir do código do aplicativo

Você pode iniciar animações automaticamente ou em resposta a ações do usuário. Para o caso automático, você geralmente usa um evento de tempo de vida de objeto como [**Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) para atuar como o gatilho da animação. O evento **Loaded** é um bom evento a ser usado para essa finalidade porque, nesse ponto, a interface do usuário está pronta para interação e a animação não será cortada no início porque outra parte da interface do usuário ainda está sendo carregada.

Neste exemplo, o evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed) está anexado ao retângulo para que, quando o usuário clicar no retângulo, a animação comece.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

O manipulador de eventos inicia o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) (a animação) usando o método [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) do **Storyboard**.

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

Você pode manipular o evento [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx) se desejar executar outra lógica após a animação terminar a aplicação de valores. Além disso, para solucionar problemas de interações do sistema/animação de propriedade, o método [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358) pode ser útil.

> [!TIP]
> Sempre que você criar código para um cenário de app em que uma animação será iniciada a partir do código do app, é recomendável verificar novamente se uma animação ou transição já existe na biblioteca de animações para o cenário da interface do usuário. As animações da biblioteca permitem uma experiência da interface do usuário mais consistente em todos os aplicativos do Windows Runtime e são mais fáceis de usar.

 

### <a name="animations-for-visual-states"></a>Animações para estados visuais

O comportamento de execução de um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) que é usado para definir o estado visual de um controle é diferente da maneira como um aplicativo pode executar um storyboard diretamente. Quando aplicado a uma definição de estado visual no XAML, o **Storyboard** é um elemento de um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) contêiner e o estado como um todo é controlado pelo uso da API [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Qualquer animação interna será executada de acordo com seus valores de animação e propriedades [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) quando o **VisualState** contêiner for usado por um controle. Para obter mais informações, consulte [Storyboards para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808). Para estados visuais, o [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) aparente é diferente. Se um estado visual for alterado para outro estado, todas as alterações de propriedade aplicadas pelo estado visual anterior e suas animações serão cancelados, mesmo se o novo estado visual não aplicar especificamente uma nova animação a uma propriedade.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** e **EventTrigger**

Só há uma maneira de iniciar uma animação que pode ser declarada completamente no XAML. No entanto, essa técnica já não é mais usada com freqüência. Trata-se de uma sintaxe herdada do WPF e de versões anteriores do Silverlight antes do suporte a [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Essa sintaxe [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) ainda funciona no XAML do Windows Runtime para fins de importação/compatibilidade, mas só funciona para um comportamento de gatilho baseado no evento [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723). Uma tentativa de acionar outros eventos lançará exceções ou causará falha na compilação. Para obter mais informações, consulte [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) ou [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053).

## <a name="animating-xaml-attached-properties"></a>Animando propriedades anexadas XAML

Não é um cenário comum, mas é possível aplicar um valor animado a uma propriedade anexada XAML. Para saber mais sobre o que são as propriedades anexadas e como elas funcionam, consulte [Visão geral das propriedades anexadas](https://msdn.microsoft.com/library/windows/apps/Mt185579). O direcionamento de uma propriedade anexada exige uma [sintaxe de property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586) que coloca o nome da propriedade entre parênteses. Você pode animar as propriedades anexadas internas, como [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/Hh759773), usando uma [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) que aplica valores inteiros discretos. Entretanto, uma limitação existente da implementação XAML de Windows Runtime é que não é possível animar uma propriedade anexada personalizada.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>Mais tipos de animação e próximas etapas para saber mais sobre como animar a interface do usuário

Até agora, mostramos as animações personalizadas que são animadas entre dois valores e depois interpolam linearmente os valores conforme necessário durante a execução da animação. Elas são chamadas de animações **From**/**To**/**By** Mas há outro tipo de animação que permite declarar valores intermediários que ficam entre o início e o fim. Elas são chamadas de *animações de quadro chave*. Há também uma maneira de alterar a lógica de interpolação em uma animação **From**/**To**/**By** ou uma animação de quadro chave. Isso envolve a aplicação de uma função de easing. Para saber mais sobre esses conceitos, consulte [Animações de quadro chave e com função de easing](key-frame-and-easing-function-animations.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Sintaxe de Property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Visão geral das propriedades de dependência](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [Animações de quadro chave e com função de easing](key-frame-and-easing-function-animations.md)
* [Animações de storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [Modelos de controle](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 





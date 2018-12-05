---
title: Navegação por foco para teclado, gamepad, controle remoto e ferramentas de acessibilidade
Description: ''
label: ''
template: detail.hbs
keywords: teclado, controlador de jogo, controle remoto, navegação, navegação interna direcional, área direcional, estratégia de navegação, entrada, interação do usuário, acessibilidade, usabilidade
ms.date: 03/02/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c76e62a4fcccec91fc6b3a083671ff68af2202e
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8693969"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Navegação por foco para teclado, gamepad, controle remoto e ferramentas de acessibilidade

![Teclado, remoto e direcional](images/dpad-remote/dpad-remote-keyboard.png)

Use a navegação por foco para fornecer experiências de interação abrangentes e consistentes em seus aplicativos UWP e controles personalizados para usuários avançados de teclado, pessoas com deficiências e outras necessidades de acessibilidade, bem como a experiência de 3 metros de telas de televisão e o Xbox One.

## <a name="overview"></a>Visão geral

A navegação por foco refere-se ao mecanismo subjacente que permite que os usuários naveguem e interajam com a interface do usuário de um aplicativo UWP usando um teclado, um gamepad ou um controle remoto.

> [!NOTE] 
> Dispositivos de entrada geralmente são classificados como dispositivos apontadores, como touch, touchpad, caneta e mouse, e dispositivos não apontadores, como teclado, gamepad e controle remoto.

Este tópico descreve como otimizar um aplicativo UWP e criar experiências de interação personalizadas para os usuários que dependem de tipos de entradas não apontadoras. 

Apesar de focarmos na entrada de teclado para controles personalizados em aplicativos UWP em computadores, uma experiência de teclado bem projetada também é importante para teclados de software como o teclado virtual e o OSK, oferecendo suporte às ferramentas de acessibilidade, como o Narrador do Windows, e à experiência de 3 metros.

Consulte [Manipular entrada de ponteiro](handle-pointer-input.md) para obter orientações sobre a criação de experiências personalizadas em aplicativos UWP para dispositivos apontadores.

Para obter mais informações gerais sobre a criação de aplicativos e experiências para teclado, consulte [Interações por teclado](keyboard-interactions.md).

## <a name="general-guidance"></a>Orientações gerais
Apenas os elementos de interface do usuário que exigem a interação do usuário devem oferecer suporte à navegação por foco, os elementos que não exigem uma ação, como imagens estáticas, não precisam de foco do teclado. Leitores de tela e ferramentas de acessibilidade semelhantes ainda anunciam esses elementos estáticos, mesmo quando não estão incluídos na navegação por foco. 

É importante lembrar que, ao contrário da navegação com um dispositivo de ponteiro como um mouse ou touch, a navegação por foco é linear. Ao implementar a navegação por foco, considere como um usuário interagirá com o seu aplicativo e qual deve ser a lógica de navegação. Na maioria dos casos, recomendamos que o comportamento da navegação por foco personalizada siga o padrão de leitura preferencial da cultura do usuário.

Outras considerações da navegação por foco incluem:
- Os controles são agrupados logicamente?
- Há grupos de controles com maior importância? 
   - Em caso afirmativo, esses grupos contêm subgrupos?
- O layout exige navegação direcional personalizada (teclas de seta) e ordem de tabulação?

O livro eletrônico [Software de engenharia para acessibilidade](https://www.microsoft.com/download/details.aspx?id=19262) tem um capítulo excelente chamado *Projetando a hierarquia lógica*.

## <a name="2d-directional-navigation-for-keyboard"></a>Navegação direcional 2D para teclado

A região de navegação interna 2D de um controle, ou grupo de controles, é conhecida como sua "área direcional". Quando o foco muda para esse objeto, as teclas de seta do teclado (esquerda, direita, para cima e para baixo) podem ser usadas para navegar entre os elementos filho dentro da área direcional.

![área direcional](images/keyboard/directional-area-small.png)

*Região de navegação interna 2D, ou área direcional, de um grupo de controles*

Você pode usar a propriedade [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (que tem como valores possíveis [Automático](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [Habilitado](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) ou [Desabilitado](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) para gerenciar a navegação interna 2D com as teclas de seta do teclado.

> [!NOTE]  
> A ordem de tabulação não é afetada por essa propriedade. Para evitar uma experiência de navegação confusa, recomendamos que os elementos filho de uma área direcional *não* sejam especificados explicitamente na ordem de navegação por guias do seu aplicativo. Consulte as propriedades [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) e [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obter mais detalhes sobre o comportamento de tabulação de um elemento.  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[Automático](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (comportamento padrão)

Quando definido como Automático, o comportamento de navegação direcional é determinado pelos ancestrais, ou hierarquia herdada, do elemento. Se todos os ancestrais estiverem no modo padrão (definidos como **Automático**), a navegação direcional com o teclado *não* terá suporte.

### [<a name="disabled"></a>Desabilitado](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Defina **XYFocusKeyboardNavigation** como **Desabilitado** para bloquear a navegação direcional para o controle e seus elementos filho.

![Comportamento de XYFocusKeyboardNavigation desabilitado](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*Comportamento de XYFocusKeyboardNavigation desabilitado*

Neste exemplo, o [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) primário (ContainerPrimary) tem **XYFocusKeyboardNavigation** definido como **Habilitado**. Todos os elementos filho herdam essa configuração e podem ser navegados com as teclas de seta. No entanto, os elementos B3 e B4 estão em um [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secundário (ContainerSecondary) com **XYFocusKeyboardNavigation** definido como **Desabilitado**, que substitui o contêiner primário e desabilita a navegação por teclas de seta nele próprio e entre seus elementos filho.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>Habilitado](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Defina **XYFocusKeyboardNavigation** como **Habilitado** para oferecer suporte à navegação direcional 2D para um controle e cada um dos seus objetos filho [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement).

Quando definida, a navegação com as teclas de seta será restrita aos elementos dentro da área direcional. A navegação por guias não é afetada, pois todos os controles permanecem acessíveis por meio de sua hierarquia de ordem de guias.

![Comportamento de XYFocusKeyboardNavigation habilitado](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*Comportamento de XYFocusKeyboardNavigation habilitado*

Neste exemplo, o [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) primário (ContainerPrimary) tem **XYFocusKeyboardNavigation** definido como **Habilitado**. Todos os elementos filho herdam essa configuração e podem ser navegados com as teclas de seta. Os elementos B3 e B4 estão em um [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secundário (ContainerSecondary) no qual **XYFocusKeyboardNavigation** não está definido e, por isso, herda a configuração do contêiner primário. O elemento B5 não está em uma área direcional declarada e não oferece suporte à navegação por teclas de seta, mas oferece suporte ao comportamento de navegação por guias padrão.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

É possível ter vários níveis de áreas direcionais aninhadas. Se todos os elementos pai tiverem XYFocusKeyboardNavigation definido como Habilitado, os limites de região da navegação interna serão ignorados.

Aqui está um exemplo de duas áreas direcionais aninhadas dentro de um elemento que não oferece suporte explícito à navegação direcional 2D. Nesse caso, a navegação direcional não tem suporte entre as duas áreas aninhadas.

![Comportamento de XYFocusKeyboardNavigation habilitado e aninhado](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*Comportamento de XYFocusKeyboardNavigation habilitado e aninhado*

Aqui está um exemplo mais complexo de três áreas direcionais aninhadas nas quais:

-   Quando B1 tiver o foco, só é possível navegar para B5 (e vice-versa) porque há um limite da área direcional no qual XYFocusKeyboardNavigation está definido como Desabilitado, tornando B2, B3 e B4 inacessíveis com as teclas de seta
-   Quando B2 tiver o foco, só é possível navegar para B3 (e vice-versa) porque o limite da área direcional impede a navegação por teclas de seta para B1, B4 e B5
-   Quando B4 tiver o foco, a tecla Tab deverá ser usada para navegar entre os controles

![Comportamento de XYFocusKeyboardNavigation habilitado e com aninhamento complexo](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*Comportamento de XYFocusKeyboardNavigation habilitado e com aninhamento complexo*

## <a name="tab-navigation"></a>Navegação por tabulação

Enquanto as teclas de seta podem ser usadas para navegação direcional 2D dentro de um controle ou grupo de controles, a tecla Tab pode ser usada para navegar entre todos os controles em um aplicativo UWP. 

Todos os controles interativos oferecem suporte à navegação pela tecla Tab, por padrão (as propriedades [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) e [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) são **true**), com a ordem de tabulação lógica derivada do layout de controle no seu aplicativo. Contudo, a ordem padrão não corresponde necessariamente à ordem visual. A posição de exibição real poderia depender do contêiner de layout pai e de certas propriedades que você pode definir nos elementos filho para influenciar o layout.

Evite uma ordem de tabulação personalizada que faz com que o foco fique saltando em seu aplicativo. Por exemplo, uma lista de controles em um formulário deve ter uma ordem de tabulação que flui de cima para baixo e da esquerda para a direita (dependendo da localidade).

Nesta seção, descrevemos como essa ordem de tabulação pode ser totalmente personalizada para se adequar ao seu aplicativo.

### <a name="set-the-tab-navigation-behavior"></a>Definir o comportamento de navegação por guias

A propriedade [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) do [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) especifica o comportamento de navegação por guias para toda a árvore de objetos (ou área direcional).

> [!NOTE]
> Use essa propriedade em vez da [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) para objetos que não usam um [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) para definir sua aparência.

Conforme mencionamos na seção anterior, para evitar uma experiência de navegação confusa, recomendamos que os elementos filho de uma área direcional *não* sejam especificados explicitamente na ordem de navegação por guias do seu aplicativo. Consulte as propriedades [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) e [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obter mais detalhes sobre o comportamento de tabulação de um elemento.   
> Para versões anteriores à Atualização do Windows 10 para Criadores (compilação 10.0.15063), as configurações de guia foram limitadas a objetos [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate). Para obter mais informações, consulte [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) tem um valor do tipo [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) com os possíveis valores a seguir (observe que esses exemplos não são grupos de controles personalizados e não exigem a navegação interna com as teclas de seta):

- **Local** (padrão)   
  Índices de guia são reconhecidos na subárvore local dentro do contêiner. Neste exemplo, a ordem de tabulação é B1, B2, B3, B4, B5, B6, B7, B1.

   ![Comportamento de navegação da guia "Local"](images/keyboard/tabnav-local.gif)

   *Comportamento de navegação da guia "Local"*

- **Uma vez**  
  O contêiner e todos os elementos filho recebem o foco uma vez. Neste exemplo, a ordem de tabulação é B1, B2, B7, B1 (a navegação interna com a tecla de seta também é demonstrada).

   ![Comportamento de navegação da guia "Uma vez"](images/keyboard/tabnav-once.gif)

   *Comportamento de navegação da guia "Uma vez"*

- **Ciclo**   
  O foco volta para o elemento focalizável inicial em um contêiner. Neste exemplo, a ordem de tabulação é B1, B2, B3, B4, B5, B6, B2...

   ![Comportamento de navegação da guia "Ciclo"](images/keyboard/tabnav-cycle.gif)

   *Comportamento de navegação da guia "Ciclo"*

Aqui está o código para os exemplos anteriores (com TabFocusNavigation = "Ciclo").

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Use [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para especificar a ordem na qual os elementos recebem o foco quando o usuário navega por meio de controles usando a tecla Tab. Um controle com um índice de guia inferior recebe o foco antes de um controle com um índice superior.

Quando um controle não tem um [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) especificado, é atribuído a ele um valor de índice maior do que o mais alto valor de índice atual (e a prioridade mais baixa) de todos os controles interativos na árvore visual, com base no escopo. 

Todos os elementos filho de um controle são considerados um escopo, e se um desses elementos também tiver elementos filho, estes serão considerados outro escopo. Qualquer ambiguidade é resolvida escolhendo o primeiro elemento na árvore visual do escopo. 

Para excluir um controle da ordem de tabulação, defina a propriedade [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) como **false**.

Substitua a ordem de tabulação padrão definindo a propriedade [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex).

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) funciona da mesma maneira com [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) e [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Aqui, mostramos como a navegação por foco pode ser afetada pela propriedade [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) em elementos específicos. 

![Comportamento de navegação da guia "Local" com TabIndex](images/keyboard/tabnav-tabindex.gif)

*Comportamento de navegação da guia "Local" com TabIndex*

No exemplo anterior, há dois escopos: 
- B1, área direcional (B2 - B6) e B7
- área direcional (B2 - B6)

Quando B3 (na área direcional) recebe o foco, o escopo muda e a navegação por guias é transferida para a área direcional na qual o melhor candidato para o foco subsequente é identificado. Nesse caso, B2 seguido de B4, B5 e B6. Em seguida, o escopo muda novamente e o foco é movido para B1.

Aqui está o código desse exemplo.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>Navegação direcional 2D para teclado, gamepad e controle remoto

Os tipos de entradas não apontadoras, como teclado, gamepad, controle remoto e ferramentas de acessibilidade, como o Narrador do Windows, compartilham um mecanismo comum e subjacente de navegação e interação com a interface do usuário do seu aplicativo UWP.

Nesta seção, abordamos como especificar uma estratégia de navegação preferencial e ajustar a navegação por foco no seu aplicativo por meio de um conjunto de propriedades de estratégia de navegação que oferecem suporte a todos os tipos de entradas baseadas em foco e não apontadoras.

Para obter mais informações gerais sobre a criação de aplicativos e experiências para Xbox/TV, consulte [Interações por teclado](keyboard-interactions.md) e [Projetando para Xbox e TV](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction).

### <a name="navigation-strategies"></a>Estratégias de navegação

> Estratégias de navegação se aplicam a teclado, gamepad, controle remoto e várias ferramentas de acessibilidade.

As seguintes propriedades de estratégia de navegação permitem que você exerça influência sobre qual controle recebe o foco com base no pressionamento da tecla de seta, do botão direcional (D-pad) ou semelhantes. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Essas propriedades tem como valores possíveis [Automático](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (padrão), [NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projeção](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) ou [RectilinearDistance ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Se for definido como **Automático**, o comportamento do elemento tem por base os ancestrais do elemento. Se todos os elementos forem definidos como **Automático**, a **Projeção** é usada.

> [!NOTE]
> Outros fatores, como o elemento focalizado anteriormente ou a proximidade ao eixo da direção de navegação, podem influenciar o resultado.

### <a name="projection"></a>Projeção

A estratégia Projeção move o foco para o primeiro elemento encontrado quando a borda do elemento focalizado no momento é *projetada* na direção da navegação.

Neste exemplo, cada direção de navegação por foco está definida como Projeção. Observe como o foco se move para baixo de B1 para B4, ignorando B3. Isso ocorre porque B3 não está na zona de projeção. Observe também como um candidato de foco não é identificado ao se mover para a esquerda de B1. Isso ocorre porque a posição de B2 relativa a B1 elimina B3 como um candidato. Se B3 estivesse na mesma linha que B2, seria um candidato viável para navegação à esquerda. B2 é um candidato viável devido à sua proximidade sem obstruções ao eixo da direção de navegação.

![Estratégia de navegação Projeção](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Estratégia de navegação Projeção*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

A estratégia NavigationDirectionDistance move o foco para o elemento mais próximo ao eixo da direção de navegação.

A borda do retângulo delimitador correspondente à direção de navegação é *estendida* e *projetada* para identificar os destinos de candidatos. O primeiro elemento encontrado é identificado como o destino. No caso de vários candidatos, o elemento mais próximo é identificado como o destino. Se ainda houver vários candidatos, o elemento na extremidade superior esquerda é identificado como o candidato.

![Estratégia de navegação NavigationDirectionDistance](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Estratégia de navegação NavigationDirectionDistance*

### <a name="rectilineardistance"></a>RectilinearDistance

A estratégia RectilinearDistance move o foco para o elemento mais próximo com base na distância retilínea 2D ([geometria do táxi](https://en.wikipedia.org/wiki/Taxicab_geometry)).

A soma das distâncias principal e secundária para cada candidato potencial é usada para identificar o melhor candidato. Em caso de empate, o primeiro elemento à esquerda será selecionado se a direção solicitada for para cima ou para baixo, e o primeiro elemento da parte superior será selecionado se a direção solicitada for direita ou esquerda.

![Estratégia de navegação RectilinearDistance](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Estratégia de navegação RectilinearDistance*

Esta imagem mostra como, quando B1 tem o foco e a direção solicitada é para baixo, B3 é o candidato de foco de RectilinearDistance. Isso se baseia nos seguintes cálculos para este exemplo:
-   Distância (B1, B3, Para baixo) é 10 + 0 = 10
-   Distância (B1, B2, Para baixo) é 0 + 40 = 30
-   Distância (B1, D, Para baixo) é 30 + 0 = 30


## <a name="related-articles"></a>Artigos relacionados
- [Navegação por foco programática](focus-navigation-programmatic.md)
- [Interações por teclado](keyboard-interactions.md)
- [Acessibilidade do teclado](../accessibility/keyboard-accessibility.md) 




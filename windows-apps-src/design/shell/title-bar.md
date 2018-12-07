---
description: Personalize a barra de título de um aplicativo da área de trabalho para corresponder a personalidade do aplicativo.
title: Personalização da barra de título
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, barra de título
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 88c613456525648883735850fe831cb3b67f145c
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8793807"
---
# <a name="title-bar-customization"></a>Personalização da barra de título



Quando seu aplicativo é executado em uma janela da área de trabalho, você pode personalizar as barras de título para corresponder a personalidade do seu aplicativo. Os APIs de personalização da barra de título permitem que você especifique cores para elementos da barra de título, ou estenda o conteúdo do seu aplicativo para a área da barra de título e assuma o controle total dele.

> **APIs importantes**: [Propriedade ApplicationView.TitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview), [Classe ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [Classe CoreApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Quanto para personalizar a barra de título

Há dois níveis de personalização que você pode aplicar à barra de título.

Para personalização de cores simples, você pode definir as propriedades [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para especificar as cores que você deseja usar para elementos da barra de título. Nesse caso, o sistema mantém responsabilidade de todos os outros aspectos da barra de título, como o título do aplicativo de desenho e definindo áreas arrastáveis.

A outra opção é ocultar a barra de título padrão e substituí-la com seu próprio conteúdo XAML. Por exemplo, você pode colocar texto, botões ou menus personalizados na área da barra de título. Você também precisará usar essa opção para estender uma tela de fundo [acrylic](../style/acrylic.md) na área da barra de título.

Quando você optar por personalização completa, você é responsável por colocar o conteúdo na área de barra de título, e você pode definir sua próprias região arrastável. Os botões Voltar, Fechar, Minimizar e Maximizar ainda estão disponíveis e são manipulados pelo sistema, mas não são como elementos, como o título do aplicativo. Você precisará criar esses elementos sozinho, conforme necessário pelo seu aplicativo.

> [!NOTE]
> Personalização de cores simples está disponível para aplicativos UWP em DirectX, XAML e HTML. A personalização completa está disponível somente para aplicativos UWP usando XAML.

## <a name="simple-color-customization"></a>Personalização de cor simples

Se você quiser apenas personalizar as cores da barra de título e não fazer nada muito elaborado (por exemplo, colocar guias na sua barra de título), você pode definir as propriedades de cor no [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) na janela do aplicativo.

Este exemplo mostra como obter uma instância de ApplicationViewTitleBar e definir suas propriedades de cor.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> Este código pode ser colocado em seu método de aplicativo [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) (_App.xaml.cs_), após a chamada [Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), ou na primeira página do seu aplicativo.

> [!TIP]
> O Kit de Ferramentas da Comunidade do Windows fornece extensões que permitem que você defina essas propriedades de cor em XAML. Para obter mais informações, consulte a [Documentação do Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions).

Há algumas coisas que devem ser lembradas ao definir cores da barra de título:

- A cor de fundo do botão não é aplicada ao botão Fechar quando você passa o mouse sobre ou clica nele. O botão Fechar sempre usa a cor definida pelo sistema para esses estados.
- As propriedades de cor de botão são aplicadas ao botão Voltar do sistema quando é usado. ([Consulte Histórico de navegação e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md).)
- Definir uma propriedade de cor **null** restaura a cor padrão do sistema.
- Você não pode definir cores transparentes. O canal alfa da cor é ignorado.

O Windows oferece a um usuário a opção de aplicar suas [cores de destaque](https://docs.microsoft.com/windows/uwp/style/color#accent-color) selecionadas à barra de título. Se você definir qualquer cor da barra de título, recomendamos que você defina explicitamente todas as cores. Isso garante que não haja nenhuma combinação de cores não intencional que ocorrem devido às configurações de cor definida pelo usuário.

## <a name="full-customization"></a>Personalização completa

Quando você concorda com a personalização da barra de título completa, a área de cliente do seu aplicativo é estendida para cobrir toda a janela, incluindo a área da barra de título. Você é responsável por desenho e manipulação de entrada para toda a janela, exceto os botões de legenda, que são sobrepostos na parte superior da tela do aplicativo.

Para ocultar a barra de título padrão e estender seu conteúdo para a área da barra de título, defina a propriedade [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) como **true**.

Este exemplo mostra como obter o CoreApplicationViewTitleBar e definir a propriedade ExtendViewIntoTitleBar como **true**. Isso pode ser feito no método de seu aplicativo [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) (_App.xaml.cs_), ou na primeira página do seu aplicativo.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Essa configuração persiste quando seu aplicativo é fechado e reiniciado. No Visual Studio, se você definir ExtendViewIntoTitleBar como **true** e depois quiser reverter para o padrão, você deve defini-lo explicitamente como **false** e executar seu aplicativo para substituir a configuração persistente.

### <a name="draggable-regions"></a>Regiões arrastáveis

A região da barra de título arrastável define onde o usuário pode clicar e arrastar para mover a janela (em vez de simplesmente arrastar conteúdo dentro de tela do aplicativo). Você especifica a região arrastável chamando o método [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) e passando um UIElement que define a região arrastável. (O UIElement costuma ser um painel que contém outros elementos.)

Aqui está como definir uma grade de conteúdo como a região de barra de título arrastável. Esse código entra em XAML e code-behind para a primeira página do seu aplicativo. Consulte a seção [Exemplo de personalização completa](./title-bar.md#full-customization-example) para o código completo.

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

O UIElement (`AppTitleBar`) faz parte do XAML para o seu aplicativo. Você pode declarar e definir a barra de título em uma página raiz que não mudam, ou declarar e definir uma região da barra de título em cada página que seu aplicativo pode navegar. Se você defini-lo em cada página, você deve verificar se a região arrastável permanece consistente conforme um usuário navega em torno de seu aplicativo.

Você pode chamar SetTitleBar para alternar para um novo elemento de barra de título enquanto seu aplicativo está em execução. Você também pode passar **null** como o parâmetro para SetTitleBar para reverter para o padrão de comportamento de arrastar. (Consulte "Região de padrão arrastável" para obter mais informações.)

> [!IMPORTANT]
> A região arrastável que você especifica precisa poder executar teste de clique, que significa que, para alguns elementos, talvez seja necessário definir um pincel de plano de fundo transparente. Consulte os comentários sobre [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) para obter mais informações.
>
>Por exemplo, se você definir uma grade como sua região arrastável, defina `Background="Transparent"` para torná-la arrastável.
>
>Essa grade não pode ser arrastada (mas os elementos visíveis dentro dele podem): `<Grid x:Name="AppTitleBar">`.
>
>Essa grade tem a mesma aparência, mas a grade inteira pode ser arrastada: `<Grid x:Name="AppTitleBar" Background="Transparent">`.

#### <a name="default-draggable-region"></a>Região arrastável padrão

Se você não especificar uma região arrastável, um retângulo que é a largura da janela, a altura dos botões de legenda e posicionado na borda superior da janela é definido como a região arrastável padrão.

Se você definir uma região arrastável, o sistema reduz a região arrastável padrão até uma pequena área do tamanho de um botão de legenda, posicionado à esquerda dos botões de legenda (ou à direita se os botões de legendas estiverem no lado esquerdo da janela). Isso garante que sempre há uma área consistente que usuário pode arrastar.

### <a name="system-caption-buttons"></a>Botões de legenda do sistema

O sistema reserva o canto superior esquerdo ou direito superior da janela do aplicativo para os botões de legenda do sistema (Voltar, Minimizar, Maximizar, Fechar). O sistema mantém o controle da área de controle de legenda para garantir que a funcionalidade mínima seja fornecida para arrastar, minimizar, maximizar e fechar a janela. O sistema desenha o botão Fechar no canto superior direito para idiomas da esquerda para a direita e no canto superior esquerdo para idiomas da direita para a esquerda.

A dimensão e a posição da área de controle de legenda é comunicada pela classe CoreApplicationViewTitleBar para que você possa considerá-la no layout de sua interface do usuário da barra de títulos. A largura da região reservada em cada lado é determinada pelas propriedades [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) ou [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) e a altura é determinado pela propriedade [altura](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height).

Você pode desenhar conteúdo sob a área de controle de legenda definida por essas propriedades, como seu plano de fundo do aplicativo, mas você não deve colocar qualquer interface do usuário que você espera que o usuário possa interagir com. Ele não receberá qualquer entrada porque a entrada para os controles de legenda é manipulada pelo sistema.

Você pode manipular o evento [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) para responder a alterações no tamanho dos botões de legenda. Por exemplo, isso pode acontecer quando o botão Voltar do sistema é mostrado ou ocultado. Manipule esse evento para verificar e atualizar o posicionamento dos elementos de interface do usuário que dependem do tamanho da barra de título.

Este exemplo mostra como ajustar o layout de sua barra de título para levar em conta alterações como o botão Voltar do sistema sendo mostrado ou ocultado. `AppTitleBar`, `LeftPaddingColumn` e `RightPaddingColumn` são declarados no XAML mostrado anteriormente.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>Conteúdo interativo

Você pode colocar controles interativos, como botões, menus ou uma caixa de pesquisa, na parte superior do aplicativo para que pareçam estar na barra de título. No entanto, existem algumas regras que você deve seguir para garantir que seus elementos interativos recebem entrada do usuário.
- Você deve chamar SetTitleBar para definir uma área como a região de barra de título arrastável. Se você não fizer isso, o sistema define a região arrastável padrão na parte superior da página. O sistema, em seguida, manipula todas as entradas de usuário nessa área e impede que a entrada alcance seus controles.
- Coloque seus controles interativos sobre a parte superior da região arrastável definida pela chamada para SetTitleBar (com uma ordem de z superior). Não deixe seus controles interativos secundários do UIElement passado para SetTitleBar. Depois que você passa um elemento para SetTitleBar, o sistema trata isso como a barra de título do sistema e manipula todas as entradas de ponteiro para esse elemento.

Aqui, o elemento `TitleBarButton` tem uma ordem de z superior ao `AppTitleBar`, portanto, ele recebe entrada do usuário.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>Transparência em botões de legenda

Quando você define ExtendViewIntoTitleBar como **true**, você pode fazer com que a tela de fundo dos botões de legenda transparente permitam que o seu plano de fundo do aplicativo seja mostrado. Você normalmente define o plano de fundo como [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent) para transparência total. Para transparência parcial, defina o canal alfa para a [cor](https://docs.microsoft.com/uwp/api/windows.ui.color) para a qual você definiu a cor.

Essas propriedades ApplicationViewTitleBar podem ser transparentes:

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

A cor de fundo do botão não é aplicada ao botão Fechar quando você passa o mouse sobre ou clica nele. O botão Fechar sempre usa a cor definida pelo sistema para esses estados.

Todas as outras propriedades de cor continuarão ignorando o canal alfa. Se ExtendViewIntoTitleBar estiver definido como **false**, o canal alfa é sempre ignorado para todas as propriedades de cor ApplicationViewTitleBar.

### <a name="full-screen-and-tablet-mode"></a>Modo de tela e tablet completos

Quando seu aplicativo é executado em _em tela inteira_ ou em _modo tablet_, o sistema oculta a barra de título e os botões do controle de legenda. No entanto, o usuário pode invocar a barra de título para que ela seja mostrada como uma sobreposição no topo da interface do usuário do aplicativo.
Você pode manipular o evento [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) para ser notificado quando a barra de título estiver oculta ou invocada e mostrar ou ocultar o conteúdo da barra de título personalizado conforme necessário.

Este exemplo mostra como manipular IsVisibleChanged para mostrar e ocultar o elemento `AppTitleBar` mostrado anteriormente.

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>O modo _Tela inteira_ pode ser inserido somente se suportado por seu aplicativo. Consulte [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) para obter mais informações. [_Modo Tablet_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) é uma opção de usuário em hardware com suporte, portanto, um usuário pode optar por executar qualquer aplicativo no modo tablet.

## <a name="full-customization-example"></a>Exemplo de personalização completa

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Tornar é óbvio quando a janela está ativo ou inativo. No mínimo, altere a cor dos botões na barra de título, ícones e texto.
- Defina uma região arrastável ao longo da borda superior da tela do aplicativo. O posicionamento das barras de título do sistema de correspondência torna mais fácil para os usuários o encontrarem.
- Defina uma região arrastável que corresponde a barra de título visual (se houver) na tela do aplicativo.

## <a name="related-articles"></a>Artigos relacionados

- [Acrílico](../style/acrylic.md)
- [Cor](../style/color.md)

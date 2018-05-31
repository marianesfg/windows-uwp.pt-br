---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: Modo de exibição de navegação
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7fc365a7dbc69819ce88a22db2490b327412c8b4
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675363"
---
# <a name="navigation-view"></a>Modo de exibição de navegação

O controle de exibição de Navegação fornece um menu de navegação recolhível para a navegação de nível superior em seu app. Esse controle implementa o padrão de painel de navegação, ou menu hambúrguer, e adapta automaticamente o modo de exibição do painel a diferentes tamanhos de janela.

> **APIs importantes**: [classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [classe NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [enumeração NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Exemplo de NavigationView](images/navview_wireframe.png)

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>Este é o controle correto?

O NavigationView funciona bem em:

-  Muitos aplicativos com muitos itens de navegação de nível superior que são de tipo similar. (Por exemplo, um aplicativo de esportes com categorias como Futebol, Beisebol, Basquete, etc.)
-  Um número de médio a alto (de 5 a 10) de categorias de navegação de nível superior.
-  Fornecendo uma experiência de navegação consistente em todos os aplicativos. O painel de navegação deve incluir somente elementos de navegação, não ações.
-  Preservando o estado real da tela de janelas menores.

NavigationView é apenas um dos vários elementos de navegação que você pode usar. Para saber mais sobre outros padrões de navegação e os elementos, consulte [Noções básicas de design de navegação ](../basics/navigation-basics.md).

O controle NavigationView tem muitos comportamentos internos que implementam o padrão de painel de navegação simples. Se sua navegação requer um comportamento mais complexo que não é suportado pela NavigationView, convém considerar o padrão [mestre/detalhes](master-details.md) padrão.

## <a name="examples"></a>Exemplos
<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/NavigationView">abrir o aplicativo e ver o NavigationView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>Seções de NavigationView

![Seções de NavigationView](images/navview_sections.png)

### <a name="pane"></a>Painel

O botão de navegação interna ("hambúrguer") permite aos usuários abrir e fechar o painel. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible). O rótulo de texto ao lado do hambúrguer é a propriedade [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle).

O botão Voltar interno aparece no canto superior esquerdo no painel. O controle NavigationView não automaticamente adicionar conteúdo a pilha voltar, mas para trás habilitar a navegação, consulte a seção [para trás navegação](#backwards-navigation).

O painel NavigationView pode conter:

- Itens de navegação, na forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar até páginas específicas
- Separadores, na forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar itens de navegação
- Cabeçalhos, na forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para rotular grupos de itens
- Um [AutoSuggestBox](auto-suggest-box.md) opcional para possibilitar a pesquisa no nível do aplicativo
- Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, use a propriedade [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)
- Conteúdo de forma livre no rodapé do painel, quando adicionado à propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

#### <a name="visual-style"></a>Estilo visual

Os itens NavigationView têm suporte para os estados visuais selecionado, desabilitado, ponteiro sobre, pressionado e concentrado.

![Estados de itens de NavigationView: desabilitado, ponteiro sobre, pressionado e concentrado](images/navview_item-states.png)

Quando os requisitos de hardware e software são atendidos, o NavigationView usa automaticamente o novo [material Acrílico](../style/acrylic.md) e [Revelar destaque](../style/reveal.md) no seu painel.

### <a name="header"></a>Cabeçalho

A área de cabeçalho está verticalmente alinhada ao botão de navegação e tem uma altura de 52 px. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho deve estar visível quando o NavigationView está no modo Mínimo. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para fazer isso, defina a propriedade [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) como **false**.

### <a name="content"></a>Conteúdo

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas. 

É recomendável usar margens de 12 px nos lados da área do conteúdo quando NavigationView está no modo mínimo, do contrário, use margens de 24 px.

## <a name="navigationview-display-modes"></a>Modos de exibição NavigationView
O painel NavigationView pode ser aberto ou fechado e tem três opções de modo de exibição:
-  **Mínimo** Somente o botão de hambúrguer permanece fixo enquanto o painel é mostrado e oculto conforme necessário.
-  **Compacto** O painel sempre é exibido como um fragmento estreito que pode ser expandido até a largura máxima.
-  **Expandido** O painel é aberto com o conteúdo. Quando fechado pela ativação do botão hambúrguer, a largura do painel se torna um fragmento estreito.

Por padrão, o sistema seleciona automaticamente o modo de exibição ideal com base no espaço disponível na tela para o controle. (Você pode [override](#overriding-the-default-adaptive-behavior) esta configuração.)

### <a name="minimal"></a>Mínimo

![NavigationView no modo Mínimo, mostrando o painel fechado e aberto](images/navview_minimal.png)

-  Quando fechado, o painel fica oculto por padrão, apenas com ícones e o botão de navegação visíveis.
-  Fornece navegação sob demanda que preserva o estado real da tela. Ideal para aplicativos em telefones e phablets.
-  Ao pressionar o botão de navegação, o painel abre e fecha, aparecendo como uma sobreposição acima do cabeçalho e do conteúdo. O conteúdo não flui novamente.
-  Quando aberto, o painel é transitório e pode ser fechado com um gesto rápido de ignorar, como fazer uma seleção, pressionar o botão Voltar ou tocando fora do painel.
-  O item selecionado fica visível quando a sobreposição do painel abre.
-  Quando os requisitos são atendidos, o fundo do painel aberto fica em [acrílico no aplicativo](../style/acrylic.md#acrylic-blend-types).
-  Por padrão, NavigationView está no modo Mínimo quando a largura geral for inferior ou igual a 640 px.

### <a name="compact"></a>Compacto

![NavigationView no modo Compacto, mostrando o painel fechado e aberto](images/navview_compact.png)

-  Quando fechado, um fragmento vertical do painel mostrando apenas ícones e o botão de navegação ficam visíveis.
-  Fornece alguma indicação da localização selecionada enquanto usa uma pequena quantidade do estado real da tela.
-  Esse modo é mais adequado para telas médias, como tablets e [experiências de 3 metros](../devices/designing-for-tv.md).
-  Ao pressionar o botão de navegação, o painel abre e fecha, aparecendo como uma sobreposição acima do cabeçalho e do conteúdo. O conteúdo não flui novamente.
-  O cabeçalho não é obrigatório e pode ser ocultado para proporcionar mais espaço vertical ao conteúdo.
-  O item selecionado mostra um indicador visual para destacar onde o usuário está na árvore de navegação.
-  Quando os requisitos são atendidos, o fundo do painel fica em [acrílico no aplicativo](../style/acrylic.md#acrylic-blend-types).
-  Por padrão, NavigationView está no modo Compacto quando sua largura geral é entre 641 px e 1007 px.

### <a name="expanded"></a>Expandido

![NavigationView no modo Expandido, mostrando o painel aberto](images/navview_expanded.png)

-  Por padrão, o painel permanece aberto. Esse modo é mais adequado para telas maiores.
-  O painel aparece lado a lado com o cabeçalho e o conteúdo, que reflui no espaço disponível.
-  Quando o painel é fechado usando o botão de navegação, ele aparece como um fragmento estreito lado a lado com o cabeçalho e o conteúdo.
-  O cabeçalho não é obrigatório e pode ser ocultado para proporcionar mais espaço vertical ao conteúdo.
-  O item selecionado mostra um indicador visual para destacar onde o usuário está na árvore de navegação.
-  Quando os requisitos são atendidos, o fundo do painel é pintado com [acrílico em segundo plano](../style/acrylic.md#acrylic-blend-types).
-  Por padrão, NavigationView está no modo de Expandido, quando a largura geral é superior a 1007 px.

### <a name="overriding-the-default-adaptive-behavior"></a>Substituição do comportamento adaptável padrão

O modo de exibição NavigationView muda automaticamente o modo de exibição com base na quantidade de espaço disponível na tela.

> [!NOTE] 
NavigationView deve servir como contêiner de raiz do aplicativo, uma vez que esse controle foi projetado para abranger a largura inteira e a altura da janela do aplicativo.
Se você deseja alterar estas larguras nas quais a visualização de navegação altera os modos de exibição por usar as propriedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) e [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth). 

Considere estes cenários que ilustram quando você pode personalizar o comportamento do modo de exibição.

- **Navegação frequente** Se você espera que os usuários naveguem entre áreas de aplicativo com frequência, considere a possibilidade de manter o painel no modo de exibição em larguras de janela mais estreitas. Um aplicativo de música com áreas de navegação com Músicas/Álbuns/Artistas podem optar por uma largura de painel de 280 px e permanece no modo expandido enquanto a janela do aplicativo for superior a 560 px.
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **Navegação rara** Se você espera que os usuários naveguem entre áreas do aplicativo com pouca frequência, considere manter o painel oculto em larguras mais amplas de janela. Um aplicativo de calculadora com vários layouts pode optar por permanecer no modo Mínimo, mesmo quando o aplicativo é maximizado em uma tela de 1080 p.
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **Diferenciação de ícone** Se áreas de navegação do aplicativo não derem margem a ícones significativos, evite usar o modo Compacto. Um aplicativo de exibição de imagens com áreas de navegação Coleções/Álbuns/Pastas pode optar por mostrar o NavigationView no modo Mínimo em larguras estreitas e médias, e no modo Expandido em larguras grandes.
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>Interação

Quando os usuários tocam em um item de navegação no Painel, o NavigationView mostrará esse item como selecionado e acionará um evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Se o toque resultar na seleção de um novo item, NavigationView também acionará um evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged). 

O aplicativo é responsável por atualizar o Cabeçalho e o Conteúdo com as informações adequadas em resposta a essa interação do usuário. Além disso, é recomendável mover o [focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState) programaticamente do item de navegação para o conteúdo. Ao definir o foco inicial na carga, você simplifica o fluxo do usuário e minimiza a quantidade esperada de movimentos de foco do teclado.

## <a name="backwards-navigation"></a>Navegação para trás
NavigationView tem um botão de voltar integrado, que pode ser ativado com as seguintes propriedades:
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) é uma enumeração NavigationViewBackButtonVisible e "Auto" por padrão. Ele é usado para mostrar/ocultar o botão Voltar. Quando o botão não estiver visível, o espaço para desenhar o botão Voltar será recolhido.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) é false por padrão e pode ser usado para alternar os estados de botão Voltar.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é acionado quando um usuário clica no botão Voltar.
    - No modo mínimo/compacto, quando o NavigationView.Pane estiver aberto como um submenu, clicar no botão Voltar fechará o painel e acionará o evento **PaneClosing**.
    - Não é acionado se IsBackEnabled é false.

![Botão voltar do NavigationView](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>Exemplo de código

Este é um exemplo simples de como você pode incorporar o NavigationView no seu aplicativo. 

Demonstramos como implementar a navegação com o botão Voltar do NavigationView para trás. Observe que, para usar propriedades de navegação regressiva do NavigationView, você vai precisar a [Windows 10 Insider Preview (introduzido v10.0.17110.0)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).

Também demonstramos localização das cadeias de caracteres de item de conteúdo de navegação com `x:Uid`. Para obter mais informações sobre localização, consulte [Localizar as cadeias de caracteres da interface do usuário](../../app-resources/localize-strings-ui-manifest.md).

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
        </NavigationView.MenuItems>

        <NavigationView.AutoSuggestBox>
            <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
        </NavigationView.AutoSuggestBox>

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid Margin="24,10,0,0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                        <AppBarButton Label="Refresh" Icon="Refresh"/>
                        <AppBarButton Label="Import" Icon="Import"/>
                    </CommandBar>
                </Grid>
            </DataTemplate>
        </NavigationView.HeaderTemplate>

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

        case "apps":
            ContentFrame.Navigate(typeof(AppsPage));
            break;

        case "games":
            ContentFrame.Navigate(typeof(GamesPage));
            break;

        case "music":
            ContentFrame.Navigate(typeof(MusicPage));
            break;

        case "content":
            ContentFrame.Navigate(typeof(MyContentPage));
            break;
    }           
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>Personalizar planos de fundo

Para alterar o plano de fundo da área principal do NavigationView, defina sua `Background`propriedade para o pincel preferencial.

O plano de fundo do painel mostra o acrílico no aplicativo quando NavigationView está no modo Mínimo ou Compacto, além de acrílico em segundo plano no modo Expandido. Para atualizar esse comportamento ou personalizar a aparência de acrílico do painel, modifique os recursos de dois temas substituindo-os no App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>Estendendo seu app até sua barra de título

Para obter um visual fluido e integrado na janela do seu aplicativo, recomendamos estender o NavigationView e seu acrílico em sua área da barra de título. Isso evita a forma visualmente atraente criada pela barra de título, o conteúdo de NavigationView de cor sólida e acrylic do painel do NavigationView.

Para fazer isso, adicione o seguinte código ao seu App.xaml.cs.

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

Desenho na barra de título tem o efeito colateral de ocultar o título do seu aplicativo. Para ajudar os usuários, restaurá-lo, adicionando seu próprios TextBlock. Adicione a seguinte marcação para a página-raiz que contém seu NavigationView.

```xaml
<Grid>

    <TextBlock x:Name="AppTitle" 
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}" 
        Style="{StaticResource CaptionTextBlockStyle}" 
        IsHitTestVisible="False" 
        Canvas.ZIndex="1"/>

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

Você também precisará ajustar as margens do AppTitle dependendo visibilidade do botão Voltar. E, quando o aplicativo está em FullScreenMode, você precisará remover o espaçamento para a seta Voltar, mesmo que a barra de título reserva espaço para ele.

```csharp
void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

Para obter mais informações sobre como personalizar barras de título, consulte [personalização da barra de título ](../shell/title-bar.md).

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Controle Pivot](tabs-pivot.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral de Design Fluente para UWP](../fluent-design-system/index.md)


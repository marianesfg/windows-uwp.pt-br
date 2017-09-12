---
author: Jwmsft
Description: "Controle que dispõe a navegação de nível superior sem ocupar o estado real da tela."
title: "Modo de exibição de navegação"
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>Modo de exibição de navegação

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

O controle do modo de exibição de navegação fornece um layout vertical comum para áreas de nível superior do aplicativo por meio de um menu de navegação recolhível. Esse controle é projetado para implementar o padrão de painel de navegação ou menu hambúrguer e adapta automaticamente o layout para diferentes tamanhos de janela.

> **APIs importantes**: [classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [classe NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [enumeração NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Exemplo de NavigationView](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>Este é o controle correto?

O NavigationView funciona bem em:

-  Aplicativos com muitos itens de navegação de nível superior que são de tipo similar. Por exemplo, um aplicativo de esportes com categorias como Futebol, Beisebol, Basquete, etc.
-  Fornecendo uma experiência de navegação consistente em todos os aplicativos. O painel de navegação deve incluir somente elementos de navegação, não ações.
-  Um número de médio a alto (de 5 a 10) de categorias de navegação de nível superior.
-  Preservando o estado real da tela de janelas menores.

O modo de exibição de navegação é apenas um dos vários elementos de navegação que você pode usar. Para saber mais sobre padrões de navegação e outros elementos de navegação, consulte [Noções básicas de design de navegação dos aplicativos da UWP (Plataforma Universal do Windows)](../layout/navigation-basics.md).

Para ver um exemplo de código de como criar o padrão do painel de navegação com SplitView e ListView, baixe a [solução de navegação XAML](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation) do GitHub.

## <a name="navigationview-parts"></a>Partes de NavigationView
O controle é amplamente subdividido em três seções: um painel de navegação à esquerda, o cabeçalho e áreas de conteúdo à direita.

![Seções de NavigationView](images/navview_sections.png)

### <a name="pane"></a>Painel

O painel de navegação pode conter:

- Itens de navegação, na forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar até páginas específicas
- Separadores, na forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar itens de navegação
- Cabeçalhos, na forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para rotular grupos de itens
- Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, use a propriedade [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible)
- Conteúdo de forma livre no rodapé do painel, quando adicionado à propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter)

O botão de navegação interna ("hambúrguer") permite aos usuários abrir e fechar o painel. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible).

### <a name="header"></a>Cabeçalho

A área de cabeçalho está verticalmente alinhada ao botão de navegação e tem uma altura fixa. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho deve estar visível quando o NavigationView está no modo Mínimo. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para fazer isso, defina a propriedade [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) como **false**.

### <a name="content"></a>Conteúdo

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas. Ela pode conter um ou mais elementos e é uma boa área para navegação de subnível inferior adicional, como [Pivot](tabs-pivot.md).

É recomendável usar margens de 12 px nos lados do conteúdo quando NavigationView está no modo mínimo, do contrário, use margens de 24 px.

## <a name="visual-style"></a>Estilo visual

<div class="microsoft-internal-note">
As linhas vermelhas para esse controle estão disponíveis em [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView).<br/><br/>
</div>

Os itens de navegação têm suporte para os estados visuais selecionado, desabilitado, ponteiro sobre, pressionado e concentrado.

![Estados de itens de NavigationView: desabilitado, ponteiro sobre, pressionado e concentrado](images/navview_item-states.png)

Quando os requisitos de hardware e software são atendidos, o NavigationView usa automaticamente o novo [material Acrílico](../style/acrylic.md) e [Revelar destaque](../style/reveal.md) no seu painel.


## <a name="navigationview-modes"></a>Modos de NavigationView
O painel NavigationView pode ser aberto ou fechado e tem três opções de modo de exibição:
-  **Mínimo** Somente o botão de hambúrguer permanece fixo enquanto o painel é mostrado e oculto conforme necessário.
-  **Compacto** O painel sempre é exibido como um fragmento estreito que pode ser expandido até a largura máxima.
-  **Expandido** O painel é aberto com o conteúdo. Quando fechado pela ativação do botão hambúrguer, a largura do painel se torna um fragmento estreito.

Por padrão, o sistema seleciona automaticamente o modo de exibição ideal com base no espaço disponível na tela para o controle. (você pode substituir essa configuração; consulte a próxima seção para obter detalhes.)

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
-  Esse modo é mais adequado para telas médias, como tablets e [experiências de 3 metros](../input-and-devices/designing-for-tv.md).
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

## <a name="overriding-the-default-adaptive-behavior"></a>Substituição do comportamento adaptável padrão

O modo de exibição de navegação muda automaticamente o modo de exibição com base na quantidade de espaço disponível na tela.
[!NOTE] NavigationView deve servir como contêiner de raiz do aplicativo. Esse controle foi projetado para abranger a largura inteira e a altura da janela do aplicativo.
Você pode substituir as larguras nas quais a exibição de navegação altera os modos de exibição usando as propriedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) e [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth). Considere estes cenários que ilustram quando você pode personalizar o comportamento do modo de exibição.

-  **Navegação frequente** Se você espera que os usuários naveguem entre áreas de aplicativo com frequência, considere a possibilidade de manter o painel no modo de exibição em larguras de janela mais estreitas. Um aplicativo de música com áreas de navegação com Músicas/Álbuns/Artistas podem optar por uma largura de painel de 280 px e permanece no modo expandido enquanto a janela do aplicativo for superior a 560 px.
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **Navegação rara** Se você espera que os usuários naveguem entre áreas do aplicativo com pouca frequência, considere manter o painel oculto em larguras mais amplas de janela. Um aplicativo de calculadora com vários layouts pode optar por permanecer no modo Mínimo, mesmo quando o aplicativo é maximizado em uma tela de 1080 p.
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **Diferenciação de ícone** Se áreas de navegação do aplicativo não derem margem a ícones significativos, evite usar o modo Compacto. Um aplicativo de exibição de imagens com áreas de navegação Coleções/Álbuns/Pastas pode optar por mostrar o NavigationView no modo Mínimo em larguras estreitas e médias, e no modo Expandido em larguras grandes.
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>Interação

Quando os usuários tocam em uma categoria de navegação no Painel, o NavigationView mostrará esse item como selecionado e acionará um evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked). Se o toque resultar na seleção de um novo item, NavigationView também acionará um evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged). O aplicativo é responsável por atualizar o Cabeçalho e o Conteúdo com as informações adequadas em resposta a essa interação do usuário. Além disso, é recomendável mover o foco programaticamente do item de navegação para o conteúdo. Ao definir o foco inicial na carga, você simplifica o fluxo do usuário e minimiza a quantidade esperada de movimentos de foco do teclado.

## <a name="how-to-use-navigationview"></a>Como usar o NavigationView

Este é um exemplo simples de como você pode incorporar o NavigationView no seu aplicativo.

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
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
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

        <Frame x:Name="ContentFrame">
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
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
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
}
```

## <a name="navigation"></a>Navegação

O NavigationView não mostra o botão Voltar automaticamente na barra de título do aplicativo, nem adiciona conteúdo à pilha de retorno. O controle não responde automaticamente ao pressionamento do botão Voltar no software ou hardware. Consulte a seção [histórico e histórico de navegação](../layout/navigation-history-and-backwards-navigation.md) para obter mais informações sobre este tópico e como adicionar suporte para navegação ao aplicativo.


## <a name="related-topics"></a>Tópicos relacionados

* [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [Mestre/detalhes](master-details.md)
* [Controle Pivot](tabs-pivot.md)
* [Noções básicas de navegação](../layout/navigation-basics.md)
 

 

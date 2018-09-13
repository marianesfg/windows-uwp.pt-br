---
author: QuinnRadich
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Modo de exibição de navegação
template: detail.hbs
ms.author: quradic
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c75169f118e2c8ef575fa251a7badc8cfe44247
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3956878"
---
# <a name="navigation-view-preview-version"></a>Modo de exibição de navegação (versão prévia)

> **Esta é uma versão de visualização**: Este artigo descreve uma nova versão do controle NavigationView que ainda está em desenvolvimento. Para usá-lo agora, você precisa a [compilação do Windows Insider e o SDK mais recente](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

O controle NavigationView fornece navegação de nível superior para seu aplicativo. Ele se adapta a uma variedade de dá suporte a tamanhos de tela vários estilos de navegação.

> **APIs de biblioteca de interface do usuário do Windows**: [Microsoft.UI.Xaml.Controls.NavigationView classe](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **APIs da plataforma**: [Windows.UI.Xaml.Controls.NavigationView classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Baixar a biblioteca de interface do usuário do Windows

Esse controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte a [Visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). 

## <a name="navigation-styles"></a>Estilos de navegação

NavigationView dá suporte a:

**Painel de navegação esquerdo ou menu**

![Painel de navegação expandido](images/displaymode-left.png)

**Painel de navegação superior ou menu**

![navegação superior](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

NavigationView é um controle de navegação adaptável que funciona bem para:

- Fornecendo uma experiência de navegação consistente em todo o aplicativo.
- Preservando o estado real da tela em janelas menores.
- Organização acesso a várias categorias de navegação.

Para outros controles de navegação, consulte [Noções básicas de design de navegação](../basics/navigation-basics.md).

Se sua navegação requer um comportamento mais complexo que não é suportado pela NavigationView, convém considerar o padrão [mestre/detalhes](master-details.md) padrão.

:::row:::
    :::column:::
        ![Alguns imagem](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: extensão da coluna = "2"::: **XAML Controls Gallery**<br>
        Se você tiver o aplicativo XAML Controls Gallery instalado, clique <a href="xamlcontrolsgallery:/item/NavigationView">aqui</a> para abrir o aplicativo e ver o NavigationView em ação.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>Os modos de exibição

NavigationView pode ser definido como modos de exibição diferentes, por meio do `PaneDisplayMode` propriedade:

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

É recomendável navegação à esquerda quando:

- Você tem um número de médio a alto (de 5 a 10) de categorias de navegação de nível superior igualmente importante.
- Desejar categorias de navegação muito proeminente com menos espaço para outros conteúdos do aplicativo.

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

É recomendável navegação superior quando:

- Você tem 5 ou menos categorias de navegação de nível superior igualmente importante, que as categorias de navegação de nível superior adicionais que acabam na lista suspensa de estouro menu são considerados menos importantes.
- Você precisa mostrar todas as opções de navegação na tela.
- Desejam mais espaço para conteúdo do aplicativo.
- Ícones não podem descrever claramente categorias de navegação do seu aplicativo.

:::row:::
    :::column:::
    ### LeftCompact
    Displays a thin sliver with icons on the left.
    :::column-end:::
    :::column span="2":::
    ![nav pane compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### LeftMinimal
    Displays only the menu button.
    :::column-end:::
    :::column span="2":::
    ![nav pane minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

![comportamento adaptável do GIF leftnav padrão](images/displaymode-auto.png)

Se adapta entre LeftMinimal em telas pequenas, LeftCompact telas médias, e à esquerda em telas grandes. Consulte a seção de [comportamento adaptável](#adaptive-behavior) para obter mais informações.

## <a name="anatomy"></a>Anatomia

<b>Navegação esquerda</b><br>

![seções do NavigationView esquerdas](images/leftnav-anatomy.png)

<b>Navegação superior</b><br>

![seções do NavigationView superior](images/topnav-anatomy.png)

## <a name="pane"></a>Painel

O painel pode ser posicionado na parte superior ou esquerda, por meio do `PanePosition` propriedade.

Aqui está a anatomia de painel detalhadas para as posições do painel esquerdo e superior:

<b>Navegação esquerda</b><br>

![Anatomia de NavigationView](images/navview-pane-anatomy-vertical.png)

1. Botão Menu
1. Itens de navegação
1. Separadores
1. Cabeçalhos
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

<b>Navegação superior</b><br>

![Anatomia de NavigationView](images/navview-pane-anatomy-horizontal.png)

1. Cabeçalhos
1. Itens de navegação
1. Separadores
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

O botão Voltar é exibido no canto superior esquerdo do painel, mas NavigationView não automaticamente adicionar conteúdo a pilha voltar. Para habilitar a navegação regressiva, consulte o [para trás navegação](#backwards-navigation) seção.

O painel NavigationView pode conter:

1. Itens de navegação, na forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar até páginas específicas.
2. Separadores, na forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar itens de navegação. Defina a propriedade de [opacidade](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) como 0 para renderizar o separador de espaço.
3. Cabeçalhos, na forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para rotular grupos de itens.
4. Uma opcional [AutoSuggestBox](auto-suggest-box.md) para permitir a pesquisa de nível do aplicativo.
5. Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, use a propriedade [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) .

O painel esquerdo contém:

6. Botão de menu para ativar o painel aberto e fechar. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

### <a name="pane-footer"></a>Rodapé do painel

Conteúdo de forma livre no rodapé do painel, quando adicionado à propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

:::row:::
    :::column:::
    <b>Navegação esquerda</b><br>
    ![Navegação esquerdo do painel rodapé](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Navegação superior do painel cabeçalho](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>Cabeçalho do painel

Conteúdo de forma livre no cabeçalho do painel, quando adicionado à propriedade [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)

:::row:::
    :::column:::
    <b>Navegação esquerda</b><br>
    ![Navegação esquerdo do painel cabeçalho](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Navegação superior do painel cabeçalho](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>Conteúdo do painel

Conteúdo de forma livre no painel, quando adicionado à propriedade [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)

:::row:::
    :::column:::
    <b>Navegação esquerda</b><br>
    ![Navegação do painel personalizado contentleft](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Painel Navegação de principais de conteúdo personalizada](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>Estilo visual

Quando os requisitos de hardware e software são atendidos, o NavigationView usa automaticamente o [material acrílico](../style/acrylic.md) no seu painel e o [Realce do revelação](../style/reveal.md) apenas em seu painel esquerdo.

## <a name="header"></a>Cabeçalho

![imagem genérica navview da área de cabeçalho](images/nav-header.png)

A área de cabeçalho está verticalmente alinhada ao botão de navegação na posição painel esquerdo e fica abaixo do painel na posição painel superior. Ele tem uma altura de 52 px. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho deve estar visível quando NavigationView está no modo de exibição mínima. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para fazer isso, defina a propriedade [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) como **false**.

## <a name="content"></a>Conteúdo

![navview imagem genérica de área de conteúdo](images/nav-content.png)

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas.

É recomendável usar margens de 12 px nos lados da área do conteúdo quando NavigationView está no modo mínimo, do contrário, use margens de 24 px.

## <a name="adaptive-behavior"></a>Comportamento adaptável

O modo de exibição NavigationView muda automaticamente o modo de exibição com base na quantidade de espaço disponível na tela. No entanto, convém personalizar o comportamento de modo de exibição adaptável.

### <a name="default"></a>Padrão

O comportamento adaptável padrão de NavigationView é mostrar um painel esquerdo expandido em larguras de janela grande, um painel de navegação esquerdo de somente ícone em larguras de janela média e um botão de menu hambúrguer em larguras de janela pequena. Para obter mais informações sobre tamanhos de janela para comportamento adaptável, consulte [pontos de interrupção e tamanhos de tela](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![comportamento adaptável do GIF leftnav padrão](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>Mínimo

Um segundo padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e um menu hambúrguer em ambas as larguras de janela de pequenas e médias.

![comportamento adaptável GIF leftnav 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

É recomendável quando:

- Desejam mais espaço para conteúdo de aplicativo em larguras de janela menores.
- As categorias de navegação não podem ser representadas claramente com ícones.

### <a name="compact"></a>Compacto

Um terceiro padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e um painel de navegação esquerdo de somente ícone em ambas as larguras de janela de pequenas e médias. Um bom exemplo disso é o aplicativo de email.

![comportamento adaptável GIF leftnav 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

É recomendável quando:

- É importante para sempre mostrar todas as opções de navegação na tela.
- as categorias de navegação podem ser representadas claramente com ícones.

### <a name="no-adaptive-behavior"></a>Nenhum comportamento adaptável

Às vezes, pode não desejar qualquer comportamento adaptável todo o tempo. Você pode definir o painel para estar sempre expandido, sempre compacto ou sempre mínimo.

![comportamento adaptável GIF leftnav 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>Cima para navegação à esquerda

É recomendável usar navegação superior nos tamanhos de janela grande e navegação à esquerda no pequeno tamanhos de janela quando:

- Você tem um conjunto de igualmente categorias de navegação de nível superior importantes sejam exibidas juntas, que se uma categoria neste conjunto não se enquadra na tela, você recolhe para navegação à esquerda para lhes dar importância igual.
- Você deseja preservar o conteúdo muito espaço possível em tamanhos de janela pequena.

Aqui está um exemplo:

![comportamento adaptável GIF navegação superior ou esquerda 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

Às vezes, os aplicativos precisam associar dados diferentes para o painel superior e esquerda. Geralmente, o painel esquerdo inclui mais elementos de navegação.

Aqui está um exemplo:

![comportamento adaptável GIF navegação superior ou esquerda 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
    {
        public DataTemplate NavItemTemplate { get; set; }

        public DataTemplate NavItemTopTemplate { get; set; }    

     public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            Item currItem = item as Item;
            if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
                return NavItemTopTemplate;
            else 
                return NavItemTemplate;
        }   

    }

```

## <a name="interaction"></a>Interação

Quando os usuários tocam em um item de navegação no Painel, o NavigationView mostrará esse item como selecionado e acionará um evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Se o toque resultar na seleção de um novo item, NavigationView também acionará um evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged).

O aplicativo é responsável por atualizar o Cabeçalho e o Conteúdo com as informações adequadas em resposta a essa interação do usuário. Além disso, é recomendável mover o [focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState) programaticamente do item de navegação para o conteúdo. Ao definir o foco inicial na carga, você simplifica o fluxo do usuário e minimiza a quantidade esperada de movimentos de foco do teclado.

### <a name="tabs"></a>Guias

No modelo de guias, seleção e foco são vinculados. Uma ação que normalmente desloca o foco também mudará seleção. No exemplo abaixo, seta direita moveria o indicador de seleção da exibição Lupa. Você pode obter isso definindo a propriedade [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) como ativado.

![captura de tela de somente texto navview superior](images/nav-tabs.png)

Aqui está o XAML de exemplo para isso:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

Para trocar o conteúdo ao alterar a seleção da guia, você pode usar o método de [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) do quadro com FrameNavigationOptions.IsNavigationStackEnabled definido como False e NavigateOptions.TransitionInfoOverride definido no lado a lado apropriado animação. Por exemplo, veja o [exemplo de código](#code-example) abaixo.

Se você quiser alterar o estilo padrão, você pode substituir a propriedade de [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) do NavigationView. Você também pode definir a propriedade [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) para especificar um modelo de dados diferentes.

## <a name="backwards-navigation"></a>Navegação para trás

NavigationView tem um botão de voltar integrado, que pode ser ativado com as seguintes propriedades:

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) é uma enumeração NavigationViewBackButtonVisible e "Auto" por padrão. Ele é usado para mostrar/ocultar o botão Voltar. Quando o botão não estiver visível, o espaço para desenhar o botão Voltar será recolhido.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) é false por padrão e pode ser usado para alternar os estados de botão Voltar.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é acionado quando um usuário clica no botão Voltar.
    - No modo mínimo/compacto, quando o NavigationView.Pane estiver aberto como um submenu, clicar no botão Voltar fechará o painel e acionará o evento **PaneClosing**.
    - Não é acionado se IsBackEnabled é false.

:::row:::
    :::column:::
    <b>Navegação esquerda</b><br>
    ![Navegação à esquerda do botão Voltar do NavigationView em](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Botão Voltar do NavigationView na navegação superior](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemplo de código

> [!NOTE]
> NavigationView deve servir como contêiner de raiz do aplicativo, uma vez que esse controle foi projetado para abranger a largura inteira e a altura da janela do aplicativo.
Se você deseja alterar estas larguras nas quais a visualização de navegação altera os modos de exibição por usar as propriedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) e [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth).

Este é um exemplo de ponta a ponta de como você pode incorporar o NavigationView com um painel de navegação superior tamanhos de janela grande e um painel de navegação esquerdo em tamanhos de janela pequena.

Neste exemplo, esperamos que os usuários finais selecionem com frequência novas categorias de navegação e, portanto, podemos:

- Definir a propriedade [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) como ativado
- Use navegações de quadro que não adicione à pilha de navegação.
- Mantenha o valor padrão na propriedade [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) , que é usada para indicar se botões superiores esquerdo/direito em um gamepad navegam as categorias de navegação de nível superior do seu aplicativo. O padrão é "WhenSelectionFollowsFocus". Os valores possíveis são "Sempre" e "Nunca".

Também demonstramos como implementar a navegação com o botão Voltar do NavigationView para trás.

Aqui está uma gravação de que o exemplo demonstra:

![Exemplo de ponta a ponta do NavigationView](images/nav-code-example.gif)

Aqui está o código de exemplo:

> [!NOTE]
> Se você estiver usando a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/)e, em seguida, você precisará adicionar uma referência para o Kit de ferramentas: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
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

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> Se você estiver usando a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/)e, em seguida, você precisará adicionar uma referência para o Kit de ferramentas: `using MUXC = Microsoft.UI.Xaml.Controls;`.

```csharp
// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    ContentFrame.Navigate(item.Page);
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
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>Personalizar planos de fundo

Para alterar o plano de fundo da área principal do NavigationView, defina sua `Background`propriedade para o pincel preferencial.

O fundo do painel mostra o acrílico no aplicativo quando NavigationView está no modo compacto ou superior, mínima. Para atualizar esse comportamento ou personalizar a aparência de acrílico do painel, modifique os recursos de dois temas substituindo-os no App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>Conteúdo de rolagem em painel superior

Para uma aparência perfeita à + sensação, se seu aplicativo tem páginas que usam um ScrollViewer e o painel de navegação superior posicionado, é recomendável ter a rolagem de conteúdo sob o painel de navegação superior.

Isso pode ser obtido, definindo a propriedade [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) no ScrollViewer relevante como true.

![Painel de navegação de rolagem navview](images/nav-scroll-content.png)

Se seu aplicativo tiver conteúdo de rolagem muito longa, convém considerar a incorporação cabeçalhos fixos que se conectam ao painel de navegação superior e uma superfície suave de formulário. 

![cabeçalho fixo de rolagem navview](images/nav-scroll-stickyheader.png)

Você pode obter isso definindo a propriedade [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) no NavigationView. 

Às vezes, se o usuário é rolar para baixo, convém ocultar o painel de navegação, obtido, definindo a propriedade [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) no NavigationView para false.

![navview rolagem Ocultar navegação](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Controle Pivot](tabs-pivot.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral de Design Fluente para UWP](../fluent-design-system/index.md)
---
author: serenaz
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Modo de exibição de navegação
template: detail.hbs
ms.author: sezhen
ms.date: 08/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c0857005d584b1fde0eb52a6ab0ef5ec29eaf44
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2860553"
---
# <a name="navigation-view-preview-version"></a>Modo de exibição de navegação (versão de avaliação)

> **Esta é uma versão de visualização**: Este artigo descreve uma nova versão do controle NavigationView que ainda está em desenvolvimento. Para usá-lo agora, você precisa o [mais recente compilação Insider Windows e o SDK](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

O controle de NavigationView fornece navegação de nível superior do seu aplicativo. Ele se adapta a uma variedade de tela tamanhos suporta vários estilos de navegação.

> **APIs de biblioteca de interface do usuário do Windows**: [classe Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **APIs de plataforma**: [classe Windows.UI.Xaml.Controls.NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Obtenha a biblioteca de interface do usuário do Windows

Esse controle é incluído como parte da biblioteca de interface do usuário do Windows, um pacote do NuGet que contém os novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo as instruções de instalação, consulte a [Visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). 

## <a name="navigation-styles"></a>Estilos de navegação

Oferece suporte a NavigationView:

**Painel de navegação à esquerda ou menu**

![Painel de navegação expandido](images/displaymode-left.png)

**Painel de navegação superior ou menu**

![navegação superior](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

NavigationView é um controle de navegação adaptável que funciona bem para:

- Fornecendo uma experiência de navegação consistente em todo o seu aplicativo.
- A preservação de imóveis da tela em janelas menores.
- Organizar o acesso ao número de categorias de navegação.

Para outros controles de navegação, consulte [Noções básicas sobre o design de navegação](../basics/navigation-basics.md).

Se sua navegação requer um comportamento mais complexo que não é suportado pela NavigationView, convém considerar o padrão [mestre/detalhes](master-details.md) padrão.

:::row:::
    :::column:::
        ![Alguns imagens](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: extensão da coluna = "2"::: **Galeria de controles de XAML**<br>
        Se você tiver o aplicativo de galeria de controles XAML instalado, clique <a href="xamlcontrolsgallery:/item/NavigationView">aqui</a> para abrir o aplicativo e veja NavigationView em ação.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>Modos de exibição

NavigationView pode ser definido como modos de exibição diferentes, através do `PaneDisplayMode` propriedade:

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

É recomendável de navegação à esquerda quando:

- Você tem um número de médio a alto (5-10) de categorias de navegação de nível superior igualmente importante.
- Categorias de navegação muito destaque com menos espaço para outro conteúdo de aplicativo desejado.

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

- Você tiver 5 ou menos categorias de navegação de nível superior igualmente importante, de tal modo que qualquer categorias adicionais de navegação de nível superior que terminam na lista suspensa de estouro menu serão consideradas menos importante.
- Você precisa mostrar todas as opções de navegação na tela.
- Você desejar mais espaço para o seu conteúdo de aplicativo.
- Ícones claramente não descrevem as categorias de navegação do seu aplicativo.

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

![comportamento de adaptável GIF leftnav padrão](images/displaymode-auto.png)

Se adapta entre LeftMinimal nas telas pequenas, LeftCompact em telas médias e à esquerda em grandes telas. Consulte a seção de [comportamento adaptável](#adaptive-behavior) para obter mais informações.

## <a name="anatomy"></a>Anatomia

<b>De navegação esquerdo</b><br>

![seções de NavigationView à esquerda](images/leftnav-anatomy.png)

<b>Navegação superior</b><br>

![principais seções de NavigationView](images/topnav-anatomy.png)

## <a name="pane"></a>Painel

O painel pode ser posicionado na parte superior ou à esquerda, via o `PanePosition` propriedade.

Aqui está a anatomia de painel detalhadas para as posições de painel à esquerda e superior:

<b>De navegação esquerdo</b><br>

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

O botão Voltar aparece no canto superior esquerdo do painel, mas NavigationView não adiciona automaticamente conteúdo à pilha de volta. Para habilitar a navegação com versões anteriores, consulte o [com versões anteriores navegação](#backwards-navigation) seção.

O painel de NavigationView também pode conter:

1. Itens de navegação, na forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegação em páginas específicas.
2. Separadores, na forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar itens de navegação. Defina a propriedade de [opacidade](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) como 0 para renderizar o separador de espaço.
3. Cabeçalhos, na forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para rotular grupos de itens.
4. Um opcional [AutoSuggestBox](auto-suggest-box.md) para permitir a pesquisa no nível do aplicativo.
5. Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, use a propriedade [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) .

No painel esquerdo contém:

6. Botão menu para alternar o painel abrir e fechar. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

### <a name="pane-footer"></a>Rodapé do painel

Conteúdo de forma livre no rodapé do painel, quando adicionado à propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

:::row:::
    :::column:::
    <b>De navegação esquerdo</b><br>
    ![Painel de rodapé navegação esquerdo](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Navegação superior do cabeçalho de painel](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>Cabeçalho do painel

Conteúdo de forma livre no cabeçalho do painel, quando adicionado à propriedade [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)

:::row:::
    :::column:::
    <b>De navegação esquerdo</b><br>
    ![Painel cabeçalho de navegação esquerdo](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Navegação superior do cabeçalho de painel](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>Conteúdo do painel

Conteúdo de forma livre no painel, quando adicionado à propriedade [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)

:::row:::
    :::column:::
    <b>De navegação esquerdo</b><br>
    ![Navegação do painel contentleft personalizado](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegação superior</b><br>
    ![Painel Navegação de superior de conteúdo personalizada](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>Estilo visual

Quando os requisitos de hardware e software são atendidos, NavigationView usa automaticamente o [material Acrylic](../style/acrylic.md) no seu painel e [Revelar realce](../style/reveal.md) apenas em seu painel esquerdo.

## <a name="header"></a>Cabeçalho

![imagem de navview genérica da área de cabeçalho](images/nav-header.png)

A área de cabeçalho alinhada verticalmente com o botão de navegação na posição mais à esquerda do painel e fica abaixo de painel a posição do painel superior. Ele tem uma altura fixa de 52 px. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho deve estar visível quando NavigationView está no modo de exibição mínima. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para fazer isso, defina a propriedade [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) como **false**.

## <a name="content"></a>Conteúdo

![imagem de navview genérica da área de conteúdo](images/nav-content.png)

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas.

É recomendável usar margens de 12 px nos lados da área do conteúdo quando NavigationView está no modo mínimo, do contrário, use margens de 24 px.

## <a name="adaptive-behavior"></a>Comportamento adaptável

O modo de exibição NavigationView muda automaticamente o modo de exibição com base na quantidade de espaço disponível na tela. No entanto, talvez você queira personalizar o comportamento do modo de exibição adaptável.

### <a name="default"></a>Padrão

Comportamento padrão adaptável do NavigationView é mostrar um painel esquerdo expandido em larguras de janela grande, um painel somente ícone de navegação esquerdo larguras de janela médio e um botão de menu hambúrguer em larguras de pequena janela. Para obter mais informações sobre os tamanhos de janela para o comportamento adaptável, consulte [pontos de interrupção e tamanhos de tela](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![comportamento de adaptável GIF leftnav padrão](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>Mínimo

Um segundo padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grandes e um menu de hambúrguer em ambas as larguras de janela pequenas e médias.

![comportamento do GIF leftnav adaptável 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

Recomendamos que isso quando:

- Você desejar mais espaço para conteúdo de aplicativo em larguras de janela menores.
- Suas categorias de navegação não podem ser representadas claramente com ícones.

### <a name="compact"></a>Compacto

Um terceiro padrão de adaptável comum é usar um painel esquerdo expandido em larguras de janela grandes e um painel de navegação esquerdo somente ícone em ambas as larguras de janela pequenas e médias. Um bom exemplo disso é o aplicativo de email.

![comportamento do GIF leftnav adaptável 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

Recomendamos que isso quando:

- É importante para sempre exibir todas as opções de navegação na tela.
- suas categorias de navegação podem ser representadas claramente com ícones.

### <a name="no-adaptive-behavior"></a>Nenhum comportamento adaptável

Em alguns casos, talvez não desejado qualquer comportamento adaptável nisso. Você pode definir o painel para ser sempre expandida, sempre compact ou sempre mínimas.

![comportamento do GIF leftnav adaptável 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>Principais de navegação à esquerda

Recomendamos o uso de navegação superior em tamanhos de janela grande e de navegação à esquerda em pequenos tamanhos de janela quando:

- Você tem um conjunto de igualmente importante de navegação de nível superior categorias a serem exibidos juntos, tal que se uma categoria nesse conjunto não se ajustar na tela, você recolhe para navegação esquerda, para fornecer-lhes importância igual.
- Deseja preservar como muito conteúdo espaço possível em tamanhos de janela pequena.

Aqui está um exemplo:

![comportamento adaptável de navegação esquerda ou superior de GIF 1](images/navigation-top-to-left.png)

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

Às vezes apps necessário vincular dados diferentes para o painel superior e esquerda. Muitas vezes, no painel esquerdo inclui mais elementos de navegação.

Aqui está um exemplo:

![comportamento adaptável de navegação esquerda ou superior de GIF 2](images/navigation-top-to-left2.png)

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

No modelo de guias, a seleção e o foco são vinculados. Uma ação que normalmente desloca o foco também mudará seleção. Na seguir o exemplo, arrowing direita seria mover o indicador de seleção de exibição para Lente de aumento. Você pode obter isso definindo a propriedade [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) como habilitada.

![captura de tela de navview superior somente texto](images/nav-tabs.png)

Aqui está o exemplo XAML para que:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

Para trocar o conteúdo ao alterar a seleção da guia, você pode usar o método de [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) do quadro com FrameNavigationOptions.IsNavigationStackEnabled definido como False e NavigateOptions.TransitionInfoOverride definido como o apropriado ao lado animação de slide. Para obter um exemplo, consulte o [exemplo de código](#code-example) abaixo.

Se você desejar alterar o estilo padrão, você pode substituir a propriedade de [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) do NavigationView. Você também pode definir a propriedade [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) para especificar um modelo de dados diferentes.

## <a name="backwards-navigation"></a>Navegação para trás

NavigationView tem um botão de voltar integrado, que pode ser ativado com as seguintes propriedades:

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) é uma enumeração NavigationViewBackButtonVisible e "Auto" por padrão. Ele é usado para mostrar/ocultar o botão Voltar. Quando o botão não estiver visível, o espaço para desenhar o botão Voltar será recolhido.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) é false por padrão e pode ser usado para alternar os estados de botão Voltar.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é acionado quando um usuário clica no botão Voltar.
    - No modo mínimo/compacto, quando o NavigationView.Pane estiver aberto como um submenu, clicar no botão Voltar fechará o painel e acionará o evento **PaneClosing**.
    - Não é acionado se IsBackEnabled é false.

:::row:::
    :::column:::
    <b>De navegação esquerdo</b><br>
    ![Botão Voltar do NavigationView na navegação da esquerda](images/leftnav-back.png)
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

O exemplo a seguir é um exemplo de ponta a ponta de como você pode incorporar NavigationView com um painel de navegação superior em tamanhos de janela grande e de um painel de navegação à esquerda em tamanhos de janela pequena.

Neste exemplo, esperamos que os usuários finais para selecionar frequentemente novas categorias de navegação e isso é:

- Definir a propriedade [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) como habilitada
- Use navegações quadro que não adicione a pilha de navegação.
- Mantenha o valor padrão na propriedade [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) , que é usada para indicar se complementos esquerda/direita em um gamepad navegue as categorias de navegação de nível superior do seu aplicativo. O padrão é "WhenSelectionFollowsFocus". Os valores possíveis são "Sempre" e "Nunca".

Podemos também demonstram como implementar a navegação com o botão Voltar do NavigationView com versões anteriores.

Aqui está uma gravação da amostra demonstra:

![Amostra de ponta a ponta do NavigationView](images/nav-code-example.gif)

Aqui está o código de exemplo:

> [!NOTE]
> Se você estiver usando a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/), você precisará adicionar uma referência ao Kit de ferramentas: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

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
> Se você estiver usando a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/), você precisará adicionar uma referência ao Kit de ferramentas: `using MUXC = Microsoft.UI.Xaml.Controls;`.

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

Plano de fundo do painel mostra-app acrylic quando NavigationView está no topo, mínimo, ou modo compacto. Para atualizar esse comportamento ou personalizar a aparência de acrílico do painel, modifique os recursos de dois temas substituindo-os no App.xaml.

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

## <a name="scroll-content-under-top-pane"></a>Conteúdo de rolagem sob o painel superior

Para uma aparência perfeita + aparência, se seu aplicativo tem páginas que usam um ScrollViewer e seu painel de navegação é superior posicionado, recomendamos ter a conteúdo rolagem sob o painel de navegação superior.

Isso pode ser feito definindo-se a propriedade [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) no ScrollViewer relevante como true.

![Painel de navegação de rolagem navview](images/nav-scroll-content.png)

Se seu aplicativo tiver muito longa rolar o conteúdo, convém considerar incorporação auto-adesivas cabeçalhos que se conectam ao painel de navegação superior e formam uma superfície suave. 

![cabeçalho do navview rolagem Autoadesivas](images/nav-scroll-stickyheader.png)

Você pode obter isso definindo a propriedade [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) em NavigationView. 

Às vezes, se o usuário é rolagem para baixo, convém ocultar o painel de navegação, obtido definindo a propriedade [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) em NavigationView como false.

![navview rolagem Ocultar navegação](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Controle Pivot](tabs-pivot.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral de Design Fluente para UWP](../fluent-design-system/index.md)
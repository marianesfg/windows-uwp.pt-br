---
Description: NavigationView é um controle adaptável que implementa os padrões de navegação de nível superior para seu aplicativo.
title: Modo de exibição de navegação
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ba3a45701d82ad0b43591469bf390190ec18db0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642221"
---
# <a name="navigation-view"></a>Modo de exibição de navegação

O controle NavigationView fornece navegação de nível superior para seu aplicativo. Ele se adapta a uma variedade de tamanhos de tela e dá suporte a ambos _superior_ e _esquerdo_ estilos de navegação.

![Painel de navegação superior](images/nav-view-header.png)<br/>
_Exibição de navegação dá suporte à parte superior e o painel de navegação à esquerda ou menu_

> **APIs de plataforma**: [Classe Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **APIs da biblioteca de interface do usuário do Windows**: [Classe Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Alguns recursos do NavigationView, tais como _superior_ painel de navegação, exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

NavigationView é um controle de navegação adaptável que funciona bem para:

- Fornecendo uma experiência de navegação consistente em todo o aplicativo.
- Preservando o espaço de tela em janelas menores.
- Organizar o acesso a várias categorias de navegação.

Para outros padrões de navegação, consulte [Noções básicas sobre o design de navegação](../basics/navigation-basics.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/NavigationView">abrir o aplicativo e ver o NavigationView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo da Galeria de controles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de exibição

> A propriedade PaneDisplayMode requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Você pode usar a propriedade PaneDisplayMode para configurar os estilos de navegação diferente ou modos de exibição, para que o NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Maior
    O painel é posicionado acima do conteúdo.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de navegação superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

É recomendável _superior_ navegação quando:

- Você tem 5 ou menos categorias de navegação de nível superior que são igualmente importantes e qualquer adicionais categorias que terminam no menu suspenso de estouro são consideradas menos importantes de navegação de nível superior.
- Você precisa mostrar todas as opções de navegação na tela.
- Você deseja mais espaço para o conteúdo de seu aplicativo.
- Ícones não é possível descrever claramente as categorias de navegação do seu aplicativo.

:::row:::
    :::column:::
    ### <a name="left"></a>Esquerda
    O painel é expandido e posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação esquerdo expandido](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

É recomendável _esquerdo_ navegação quando:

- Você tem categorias de navegação de nível superior igualmente importante de 5 a 10.
- Você deseja que as categorias de navegação seja proeminente, com menos espaço para outro conteúdo do aplicativo.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    O painel mostra apenas os ícones até aberto e está posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação esquerdo compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Somente o botão de menu é exibido até o painel é aberto. Quando aberto, ele está posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação esquerdo mínima](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

Por padrão, PaneDisplayMode é definido como Auto. No modo Auto, o modo de exibição de navegação se adapta entre LeftMinimal quando a janela for estreita, a LeftCompact e, em seguida, deixado conforme a janela fica mais ampla. Para obter mais informações, consulte o [comportamento adaptável](#adaptive-behavior) seção.

![Comportamento adaptável de padrão de navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável de padrão de exibição de navegação_

## <a name="anatomy"></a>Anatomia

Essas imagens mostram o layout do painel, cabeçalho e áreas de conteúdo do controle quando configurado para _superior_ ou _esquerdo_ navegação.

![Layout de exibição de painel de navegação superior](images/topnav-anatomy.png)<br/>
_Layout de navegação superior_

![Layout de exibição de navegação à esquerda](images/leftnav-anatomy.png)<br/>
_Layout de navegação à esquerda_

### <a name="pane"></a>Painel

Você pode usar a propriedade PaneDisplayMode para posicionar o painel acima do conteúdo ou à esquerda do conteúdo.

O painel do NavigationView pode conter:

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) objetos. Itens de navegação para navegar para páginas específicas.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) objetos. Separadores para agrupar itens de navegação. Defina as [opacidade](/uwp/api/windows.ui.xaml.uielement.opacity) propriedade como 0 para renderizar o separador de espaço.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) objetos. Cabeçalhos para rotular os grupos de itens.
- Um recurso opcional [AutoSuggestBox](auto-suggest-box.md) controle para permitir a pesquisa de nível de aplicativo. Atribuir o controle para o [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) propriedade.
- Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, defina as [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) propriedade **falso**.

O painel esquerdo também contém:

- Um botão de menu para alternar o painel aberto e fechado. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

O modo de exibição de navegação tem um botão Voltar que é colocado no canto superior esquerdo do painel. No entanto, ele automaticamente lida com a navegação para trás e adicionar conteúdo a pilha voltar. Para habilitar a navegação com versões anteriores, consulte o [retroativamente navegação](#backwards-navigation) seção.

Aqui está a anatomia do painel detalhado para as posições do painel superior e esquerda.

#### <a name="top-navigation-pane"></a>Painel de navegação superior

![Anatomia de painel superior do modo de exibição de navegação](images/navview-pane-anatomy-horizontal.png)

1. Cabeçalhos
1. Itens de navegação
1. Separadores
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

#### <a name="left-navigation-pane"></a>Painel de navegação à esquerda

![Modo de exibição de navegação à esquerda anatomia do painel](images/navview-pane-anatomy-vertical.png)

1. Botão Menu
1. Itens de navegação
1. Separadores
1. Cabeçalhos
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

#### <a name="pane-footer"></a>Rodapé do painel

Você pode colocar conteúdo no rodapé do painel forma livre, adicionando-o para o [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) propriedade.

:::row:::
    :::column:::
    ![Barra de navegação superior do painel rodapé](images/navview-freeform-footer-top.png)<br>
     _Rodapé do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Barra de navegação à esquerda do painel rodapé](images/navview-freeform-footer-left.png)<br>
    _Rodapé do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Cabeçalho e o título do painel

Você pode colocar o conteúdo de texto na área de cabeçalho do painel, definindo a [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) propriedade. Ele usa uma cadeia de caracteres e mostra o texto ao lado do botão de menu.

Para adicionar o conteúdo não textual, como uma imagem ou logotipo, você pode colocar qualquer elemento no cabeçalho do painel, adicionando-o para o [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) propriedade.

Se PaneTitle e PaneHeader estiverem definidas, o conteúdo é empilhado horizontalmente ao lado do botão de menu, com o PaneTitle mais próxima ao botão de menu.

:::row:::
    :::column:::
    ![Barra de navegação superior do painel cabeçalho](images/navview-freeform-header-top.png)<br>
     _Cabeçalho do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Barra de navegação à esquerda do painel cabeçalho](images/navview-freeform-header-left.png)<br>
    _Cabeçalho do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Conteúdo do painel

Você pode colocar conteúdo no painel de forma livre, adicionando-o para o [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) propriedade.

:::row:::
    :::column:::
    ![Painel nav de principais de conteúdo personalizado](images/navview-freeform-pane-top.png)<br>
     _Conteúdo personalizado do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Conteúdo personalizado do painel nav esquerda](images/navview-freeform-pane-left.png)<br>
    _Conteúdo personalizado do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Cabeçalho

Você pode adicionar um título de página, definindo o [cabeçalho](/uwp/api/windows.ui.xaml.controls.navigationview.header) propriedade.

![Exemplo de área de cabeçalho do modo de navegação](images/nav-header.png)<br/>
_Cabeçalho da exibição de navegação_

A área do cabeçalho é alinhada verticalmente com o botão de navegação na posição do painel esquerdo e se encontra abaixo do painel na posição superior do painel. Ele tem uma altura fixa de 52 px. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho fica visível a qualquer momento em que o NavigationView é no modo de exibição mínima. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para ocultar o cabeçalho, defina as [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) propriedade **falso**.

### <a name="content"></a>Conteúdo

![Exemplo de área de conteúdo de modo de exibição de navegação](images/nav-content.png)<br/>
_Conteúdo do modo de exibição de navegação_

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas.

É recomendável 12px margens para sua área de conteúdo quando o NavigationView fica em **mínimo** margens de modo e 24px caso contrário.

## <a name="adaptive-behavior"></a>Comportamento adaptável

Por padrão, o modo de exibição de navegação muda automaticamente seu modo de exibição com base na quantidade de espaço disponível para ele na tela. O [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) e [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) propriedades especificam os pontos de interrupção em que altera o modo de exibição. Você pode modificar esses valores para personalizar o comportamento do modo de exibição adaptável.

### <a name="default"></a>Padrão

Quando PaneDisplayMode é definida como seu valor padrão de **automática**, é mostrar o comportamento adaptável:

- Um painel expandido à esquerda em larguras grande janela (1008px ou superior).
- À esquerda, somente ícone, de um painel de navegação (LeftCompact) em larguras médias de janela (641px para 1007px).
- Apenas um botão de menu (LeftMinimal) em larguras de janela pequena (640 px ou menos).

Para obter mais informações sobre tamanhos de janela para o comportamento adaptável, consulte [pontos de interrupção e tamanhos de tela](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamento adaptável de padrão de navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável de padrão de exibição de navegação_

### <a name="minimal"></a>Mínimo

Um segundo padrão de adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e apenas um botão de menu em ambas as larguras da janela de pequenas e médias.

Recomendamos isso quando:

- Você deseja mais espaço para o conteúdo do aplicativo em larguras menores de janela.
- Suas categorias de navegação não podem ser representadas claramente com ícones.

![Comportamento adaptável mínima de navegação à esquerda](images/adaptive-behavior-minimal.png)<br/>
_Comportamento de adaptável "mínimo" da exibição de navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth à largura no qual você deseja que o painel para recolher. Aqui, ele é alterado do padrão de 640 para 1007. Você também deve definir ExpandedModeThresholdWidth para garantir que os valores não entrem em conflito.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Um terceiro padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e um LeftCompact, somente ícone, o painel de navegação em ambas as larguras da janela de pequenas e médias.

Recomendamos isso quando:

- É importante sempre mostrar todas as opções de navegação na tela.
- Suas categorias de navegação podem ser representadas claramente com ícones.

![Comportamento adaptável compact de navegação à esquerda](images/adaptive-behavior-compact.png)<br/>
_Comportamento adaptável de "compact" do modo de exibição de navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth como 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Nenhum comportamento adaptável

Para desabilitar o comportamento adaptável automático, defina PaneDisplayMode como um valor diferente de Auto. Aqui, ele é definido para LeftMinimal, portanto, somente o botão de menu é mostrado, independentemente da largura da janela.

![Não deixado nenhum comportamento adaptável de navegação](images/adaptive-behavior-none.png)<br/>
_Modo de exibição de navegação com PaneDisplayMode definido como LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Conforme descrito anteriormente a _modos de exibição_ seção, você pode definir o painel para estar sempre na parte superior, sempre expandido, sempre compact ou sempre mínimo. Você também pode gerenciar os modos de exibição no código do aplicativo. Um exemplo disso é mostrado na próxima seção.

### <a name="top-to-left-navigation"></a>Parte superior para o painel de navegação esquerdo

Quando você usa o painel de navegação superior no seu aplicativo, itens de navegação colapsada um menu de estouro como a redução de largura de janela. Quando a janela do aplicativo for estreita, ele pode fornecer uma melhor experiência de usuário para alternar o PaneDisplayMode de cima para navegação LeftMinimal, em vez de permitir que todos os itens colapsada o menu de estouro.

É recomendável usar o painel de navegação superior em tamanhos de janela grande e pequeno de navegação esquerdo quando a tamanhos de janela:

- Você tem um conjunto de categorias de navegação de nível superior importante igualmente sejam exibidos juntos, que se uma categoria neste conjunto não couber na tela, você recolher a navegação à esquerda para fornecer-lhes a mesma importância.
- Você deseja preservar o espaço de volume de conteúdo quanto possível em tamanhos de janela pequena.

Este exemplo mostra como usar um [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) e [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) propriedade para alternar entre a parte superior e LeftMinimal a navegação.

![Exemplo de comportamento adaptável superior ou esquerdo 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> Quando você usa AdaptiveTrigger.MinWindowWidth, o estado visual é disparado quando a janela é maior do que a largura mínima especificada. Isso significa que o padrão XAML define a janela estreita e VisualState define as modificações que são aplicadas quando a janela obtém mais ampla. O padrão PaneDisplayMode para o modo de exibição de navegação é automática, portanto, quando a largura da janela é menor ou igual a CompactModeThresholdWidth, navegação LeftMinimal é usada. Quando a janela se estende, VisualState substitui o padrão, e o painel de navegação superior é usado.

## <a name="navigation"></a>Navegação

O modo de exibição de navegação não realiza automaticamente quaisquer tarefas de navegação. Quando o usuário toca em um item de navegação, o modo de exibição de navegação mostra esse item como selecionado e gera uma [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) eventos. Se o toque resulta em um novo item que está sendo selecionado, uma [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) também é gerado.

Você pode manipular um evento para executar tarefas relacionadas para a navegação solicitada. Qual deles você devem tratar depende o comportamento desejado para seu aplicativo. Normalmente, você pode navegar até a página solicitada e atualizar o cabeçalho da exibição de navegação em resposta a esses eventos.

**ItemInvoked** é gerado sempre que o usuário toca um item de navegação, mesmo se ele já está selecionado. (O item também pode ser invocado com uma ação equivalente usando o mouse, teclado ou outra entrada. Para obter mais informações, consulte [de entrada e interações](../input/index.md).) Se você navegar no manipulador de ItemInvoked, por padrão, a página será recarregada e uma entrada duplicada é adicionada à pilha de navegação. Se você navegar quando um item é invocado, você deve não permitir recarregar a página ou certifique-se de que uma entrada duplicada não é criada a navegação backstack quando a página for recarregada. (Consulte exemplos de código).

**SelectionChanged** pode ser gerado por um usuário invocar um item que não está selecionado ou, programaticamente, alterando o item selecionado. Se a alteração da seleção ocorre porque um usuário acionasse um item, o evento ItemInvoked ocorre pela primeira vez. Se a alteração da seleção é através de programação, ItemInvoked não será gerado.

### <a name="backwards-navigation"></a>Navegação para trás

NavigationView tem um botão Voltar internos; mas, assim como acontece com navegação progressiva, ele não realiza com versões anteriores navegação automaticamente. Quando o usuário toca no botão Voltar, o [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é gerado. Você manipular esse evento para executar a navegação para trás. Para obter mais informações e exemplos de código, consulte [histórico de navegação e para trás navegação](../basics/navigation-history-and-backwards-navigation.md).

No modo mínimo ou Compact, o painel do modo de exibição de navegação está aberto como um submenu. Nesse caso, o clicando no botão Voltar será fechar o painel e gerar a **PaneClosing** eventos em vez disso.

Você pode ocultar ou desabilitar o botão Voltar ao definir essas propriedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): use para mostrar e ocultar o botão Voltar. Essa propriedade tem um valor de [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) enumeração e é definido como **automático** por padrão. Quando o botão é recolhido, nenhum espaço é reservado para ele no layout.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): use para habilitar ou desabilitar o botão Voltar. Você pode associar dados essa propriedade para o [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) propriedade de seu quadro de navegação. **BackRequested** não será gerado se **IsBackEnabled** é **false**.

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemplo de código

Este exemplo mostra como você pode usar o NavigationView com um painel de navegação superior tamanhos grandes de janela e um painel de navegação à esquerda em tamanhos de janela pequena. Ele pode ser adaptado para navegação somente à esquerda, removendo o _superior_ configurações de navegação no VisualStateManager.

O exemplo demonstra uma maneira recomendada para configurar dados de navegação que funcionarão para muitos cenários comuns. Ele também demonstra como implementar a navegação com navegação regressiva de botão e o teclado do NavigationView com versões anteriores.

Esse código pressupõe que seu aplicativo contém páginas com os seguintes nomes para navegar para: _Home page_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_, e _SettingsPage_ . Código para essas páginas não é mostrado.

> [!IMPORTANT]
> Informações sobre as páginas do aplicativo são armazenadas em um [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Esse struct requer que a versão mínima para seu projeto de aplicativo deve ser SDK 17763 ou maior. Se você usar a versão WinUI NavigationView para direcionar versões anteriores do Windows 10, você pode usar o [pacote NuGet de System. valuetuple](https://www.nuget.org/packages/System.ValueTuple/) em vez disso.

> [!IMPORTANT]
> Este código mostra como usar o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) versão do NavigationView. Se você usar a versão da plataforma do NavigationView em vez disso, a versão mínima para seu projeto de aplicativo deve ser SDK 17763 ou maior. Para usar a versão da plataforma, remova todas as referências a `muxc:`.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> Este código mostra como usar o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) versão do NavigationView. Se você usar a versão da plataforma do NavigationView em vez disso, a versão mínima para seu projeto de aplicativo deve ser SDK 17763 ou maior. Para usar a versão da plataforma, remova todas as referências a `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
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

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

Abaixo está uma [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/index) versão do **NavView_ItemInvoked** manipulador o C# exemplo de código acima. A técnica no C + c++ /CLI WinRT manipulador envolve você primeiro armazenar (na marca do [ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) o nome de tipo completo da página para o qual você deseja navegar. No manipulador, você pode converter esse valor, transformá-lo em um [ **Windows::UI::Xaml::Interop::TypeName** ](/uwp/api/windows.ui.xaml.interop.typename) de objeto e usá-lo para navegar até a página de destino. Não é necessário para a variável de mapeamento nomeada `_pages` que você vê o C# exemplo; e você poderá criar testes de unidade para confirmar que os valores dentro de suas marcas são de um tipo válido. Consulte também [conversão Boxing e unboxing valores escalares para IInspectable com C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>Personalização da exibição de navegação

### <a name="pane-backgrounds"></a>Planos de fundo do painel

Por padrão, o painel do NavigationView usa um plano de fundo diferente dependendo do modo de exibição:

- o painel é uma cor cinza sólida quando expandido à esquerda, lado a lado com o conteúdo (no modo esquerdo).
- o painel usa tinta acrílica de no aplicativo quando é aberto como uma sobreposição sobre o conteúdo (no modo compacto, mínimo ou superior).

Para modificar o plano de fundo do painel, você pode substituir os recursos de tema do XAML usados para renderizar a tela de fundo em cada modo. (Essa técnica é usada em vez de uma única propriedade PaneBackground para dar suporte a planos de fundo diferentes para diferentes modos de exibição).

Esta tabela mostra quais recursos de tema é usado em cada modo de exibição.

| Modo de exibição | Recursos de tema |
| ------------ | -------------- |
| Esquerda | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Maior | NavigationViewTopPaneBackground |

Este exemplo mostra como substituir os recursos de tema em App. XAML. Quando você substituir os recursos de tema, você deve sempre fornecer dicionários de recursos "Default" e "HighContrast", no mínimo e dicionários para "Claro" ou "Escuro" recursos conforme necessário. Para obter mais informações, consulte [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código mostra como usar o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) versão do AcrylicBrush. Se você usar a versão da plataforma de AcrylicBrush em vez disso, a versão mínima para seu projeto de aplicativo deve ser SDK 16299 ou maior. Para usar a versão da plataforma, remova todas as referências a `muxm:`.

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>Tópicos relacionados

- [Classe do NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Design Fluent para visão geral UWP](../fluent-design-system/index.md)

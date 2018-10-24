---
author: jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Modo de exibição de navegação
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0e53a02723475c61898fdd152eaf30fcbf7f3500
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5478630"
---
# <a name="navigation-view"></a>Modo de exibição de navegação

O controle NavigationView fornece navegação de nível superior para seu aplicativo. Ele se adapta a uma variedade de tamanhos de tela e dá suporte a estilos de navegação _superior_ e _esquerda_ .

![navegação superior](images/nav-view-header.png)<br/>
_Modo de exibição Navegação dá suporte à parte superior e o painel de navegação esquerdo ou menu_

> **APIs da plataforma**: [Windows.UI.Xaml.Controls.NavigationView classe](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **APIs de biblioteca de interface do usuário do Windows**: [Microsoft.UI.Xaml.Controls.NavigationView classe](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Alguns recursos do NavigationView, como navegação _superior_ , exigir o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Este é o controle correto?

NavigationView é um controle de navegação adaptável que funciona bem para:

- Fornecendo uma experiência de navegação consistente em todo o aplicativo.
- Preservando o estado real da tela em janelas menores.
- A organização acesso a várias categorias de navegação.

Para outros padrões de navegação, consulte [Noções básicas de design de navegação](../basics/navigation-basics.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/NavigationView">abrir o aplicativo e ver o NavigationView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de exibição

> A propriedade PaneDisplayMode requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Você pode usar a propriedade PaneDisplayMode para definir estilos de navegação diferentes ou modos de exibição, para o NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Superior
    O painel é posicionado acima do conteúdo.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de navegação superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

É recomendável navegação _superior_ quando:

- Você tem 5 ou menos categorias de navegação de nível superior que são igualmente importantes e qualquer navegação de nível superior adicional categorias acabam no menu de estouro de lista suspensa são consideradas menos importantes.
- Você precisa mostrar todas as opções de navegação na tela.
- Você deseja mais espaço para conteúdo do aplicativo.
- Ícones não podem descrever claramente categorias de navegação do seu aplicativo.

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

É recomendável navegação _à esquerda_ quando:

- Você tem categorias de navegação de nível superior igualmente importante de 5 a 10.
- Você quer categorias de navegação para ser proeminente, com menos espaço para outros conteúdos do aplicativo.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    O painel mostra apenas os ícones até aberto e é posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação esquerdo compacto](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Somente o botão de menu é mostrado até que o painel é aberto. Quando aberto, ele é posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação esquerdo mínimo](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

Por padrão, PaneDisplayMode é definido como automático. No modo automático, o modo de exibição de navegação se adapte entre LeftMinimal quando a janela tem estreita, para LeftCompact e, em seguida, deixado conforme a janela fica mais ampla. Para obter mais informações, consulte a seção de [comportamento adaptável](#adaptive-behavior) .

![Comportamento adaptável de padrão de navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável de padrão de modo de exibição de navegação_

## <a name="anatomy"></a>Anatomia

Essas imagens mostram o layout do painel, cabeçalho e áreas de conteúdo do controle quando configurado para navegação na _parte superior_ ou para a _esquerda_ .

![Layout de modo de exibição de navegação superior](images/topnav-anatomy.png)<br/>
_Layout de navegação superior_

![Layout de modo de exibição de navegação à esquerda](images/leftnav-anatomy.png)<br/>
_Layout de navegação à esquerda_

### <a name="pane"></a>Painel

Você pode usar a propriedade PaneDisplayMode para posicionar o painel acima do conteúdo ou para a esquerda do conteúdo.

O painel NavigationView pode conter:

- Objetos [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) . Itens de navegação para navegar até páginas específicas.
- Objetos [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) . Separadores para agrupar itens de navegação. Defina a propriedade de [opacidade](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) como 0 para renderizar o separador como espaço.
- Objetos [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) . Cabeçalhos para rotular grupos de itens.
- Um controle [AutoSuggestBox](auto-suggest-box.md) opcional para permitir a pesquisa no nível do aplicativo. Atribua o controle à propriedade [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) .
- Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, defina a propriedade [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) como **false**.

O painel esquerdo também contém:

- Um botão de menu para ativar/desativar o painel abertos e fechados. Em janelas maiores do aplicativo quando o painel estiver aberto, você pode optar por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

O modo de exibição de navegação tem um botão Voltar que é colocado no canto superior esquerdo do painel. No entanto, ele não manipular a navegação regressiva e automaticamente adicionar conteúdo à pilha de volta. Para habilitar a navegação regressiva, consulte o [para trás navegação](#backwards-navigation) seção.

Aqui está a anatomia de painel detalhadas para as posições do painel superior e esquerda.

#### <a name="top-navigation-pane"></a>Painel de navegação superior

![Anatomia de painel superior do modo de exibição de navegação](images/navview-pane-anatomy-horizontal.png)

1. Cabeçalhos
1. Itens de navegação
1. Separadores
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

#### <a name="left-navigation-pane"></a>Painel de navegação esquerdo

![Modo de exibição de navegação à esquerda anatomia do painel](images/navview-pane-anatomy-vertical.png)

1. Botão Menu
1. Itens de navegação
1. Separadores
1. Cabeçalhos
1. AutoSuggestBox (opcional)
1. Botão de configurações (opcional)

#### <a name="pane-footer"></a>Rodapé do painel

Você pode colocar o conteúdo de forma livre no rodapé do painel, adicionando-o à propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) .

:::row:::
    :::column:::
    ![Navegação superior do painel rodapé](images/navview-freeform-footer-top.png)<br>
     _Rodapé do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Navegação esquerda do painel rodapé](images/navview-freeform-footer-left.png)<br>
    _Rodapé do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Cabeçalho e o título do painel

Você pode colocar o conteúdo de texto na área de cabeçalho do painel, definindo a propriedade [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) . Ele aceita uma cadeia de caracteres e mostra o texto ao lado do botão menu.

Para adicionar conteúdo que não é de texto, como uma imagem ou logotipo, você pode colocar qualquer elemento no cabeçalho do painel, adicionando-o à propriedade [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) .

Se PaneTitle e PaneHeader forem definidas, o conteúdo é empilhado horizontalmente ao lado do botão menu, com o PaneTitle mais próxima ao botão de menu.

:::row:::
    :::column:::
    ![Navegação superior do painel cabeçalho](images/navview-freeform-header-top.png)<br>
     _Cabeçalho do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Navegação esquerda do painel cabeçalho](images/navview-freeform-header-left.png)<br>
    _Cabeçalho do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Conteúdo do painel

Você pode colocar o conteúdo de forma livre no painel, adicionando-o à propriedade [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) .

:::row:::
    :::column:::
    ![Painel Navegação de principais de conteúdo personalizada](images/navview-freeform-pane-top.png)<br>
     _Conteúdo personalizado do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Conteúdo personalizado do painel à esquerda de navegação](images/navview-freeform-pane-left.png)<br>
    _Conteúdo personalizado do painel esquerdo_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Cabeçalho

Você pode adicionar um título da página, definindo a propriedade do [cabeçalho](/uwp/api/windows.ui.xaml.controls.navigationview.header) .

![Exemplo de área de cabeçalho de modo de exibição de navegação](images/nav-header.png)<br/>
_Cabeçalho de modo de exibição de navegação_

A área de cabeçalho está verticalmente alinhada ao botão de navegação na posição painel esquerdo e fica abaixo do painel na posição painel superior. Ele tem uma altura de 52 px. Sua finalidade é conter o título da página de categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho fica visível a qualquer momento que o NavigationView está no modo de exibição mínima. Você pode optar por ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para ocultar o cabeçalho, defina a propriedade [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) como **false**.

### <a name="content"></a>Conteúdo

![Exemplo de área de conteúdo de modo de exibição de navegação](images/nav-content.png)<br/>
_Conteúdo de modo de exibição de navegação_

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada são exibidas.

É recomendável margens 12px para sua área de conteúdo quando NavigationView está no modo **mínimo** e margens de 24 px caso contrário.

## <a name="adaptive-behavior"></a>Comportamento adaptável

Por padrão, o modo de exibição de navegação muda automaticamente o modo de exibição com base na quantidade de espaço de tela disponível. As propriedades [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) e [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) especificam os pontos de interrupção em que o modo de exibição é alterado. Você pode modificar esses valores para personalizar o comportamento de modo de exibição adaptável.

### <a name="default"></a>Padrão

Quando PaneDisplayMode é definido como seu valor padrão de **automático**, o comportamento adaptável é mostrar:

- Um painel esquerdo expandido em larguras de janela grande (1008 px ou superior).
- À esquerda, somente ícone, de um painel de navegação (LeftCompact) em larguras de janela média (641 a 1007 px).
- Apenas um botão de menu (LeftMinimal) em larguras de janela pequena (640 px ou menos).

Para obter mais informações sobre tamanhos de janela para comportamento adaptável, consulte [pontos de interrupção e tamanhos de tela](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamento adaptável de padrão de navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável de padrão de modo de exibição de navegação_

### <a name="minimal"></a>Mínimo

Um segundo padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e apenas um botão de menu em ambas as larguras de janela de pequenas e médias.

Recomendamos que este item quando:

- Você deseja mais espaço para conteúdo de aplicativo em larguras de janela menores.
- As categorias de navegação não podem ser representadas claramente com ícones.

![Comportamento adaptável mínimo de navegação à esquerda](images/adaptive-behavior-minimal.png)<br/>
_Comportamento adaptável "mínimo" de modo de exibição Navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth como a largura em que você deseja que o painel para recolher. Aqui, ele é alterado do padrão de 640 a 1007. Você também deve definir ExpandedModeThresholdWidth para garantir que os valores não entram em conflito.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Um terceiro padrão adaptável comum é usar um painel esquerdo expandido em larguras de janela grande e um LeftCompact, somente ícone, o painel de navegação em ambas as larguras de janela de pequenas e médias.

Recomendamos que este item quando:

- É importante para sempre mostrar todas as opções de navegação na tela.
- As categorias de navegação podem ser representadas claramente com ícones.

![Comportamento adaptável compacto de navegação à esquerda](images/adaptive-behavior-compact.png)<br/>
_Comportamento adaptável de "compactar" do modo de exibição de navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth como 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Nenhum comportamento adaptável

Para desabilitar o comportamento adaptável automático, defina PaneDisplayMode como um valor que não seja automática. Aqui, ele é definido para LeftMinimal, para que somente o botão de menu é mostrado, independentemente da largura da janela.

![À esquerda navegação nenhum comportamento adaptável](images/adaptive-behavior-none.png)<br/>
_Modo de exibição de navegação com PaneDisplayMode definido como LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Conforme descrito anteriormente na seção _modos de exibição_ , você pode definir o painel para estar sempre em superior, sempre expandido, sempre compacto ou sempre mínimo. Você também pode gerenciar os modos de exibição no código do aplicativo. Um exemplo disso é mostrado na próxima seção.

### <a name="top-to-left-navigation"></a>Cima para navegação à esquerda

Quando você usa a navegação superior em seu aplicativo, itens de navegação colapsada um menu de estouro como o diminui de largura da janela. Quando a janela do aplicativo é estreita, ele pode fornecer uma melhor experiência de usuário para alternar o PaneDisplayMode de cima para navegação LeftMinimal, em vez de permitir que todos os itens recolhem o menu de estouro.

É recomendável usar navegação superior nos tamanhos de janela grande e navegação à esquerda em pequenas a tamanhos de janela quando:

- Você tem um conjunto de categorias de navegação de nível superior importante igualmente sejam exibidas juntas, forma que, se uma categoria neste conjunto não couber na tela, você recolhe para navegação à esquerda para lhes dar importância igual.
- Desejar preservar como conteúdo muito espaço possível em tamanhos de janela pequena.

Este exemplo mostra como usar uma propriedade [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) e [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) para alternar entre navegação superior e LeftMinimal.

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
> Quando você usa AdaptiveTrigger.MinWindowWidth, o estado visual é disparado quando a janela for maior do que a largura mínima especificada. Isso significa que o XAML padrão define a janela estreita e o VisualState define as modificações são aplicadas quando a janela seja mais larga. O padrão PaneDisplayMode para o modo de exibição de navegação é automática, portanto, quando a largura da janela é menor ou igual a CompactModeThresholdWidth, navegação LeftMinimal é usada. Quando a janela seja mais larga, o VisualState substitui o padrão e navegação superior é usada.

## <a name="navigation"></a>Navegação

O modo de exibição de navegação não executa quaisquer tarefas de navegação automaticamente. Quando o usuário toca em um item de navegação, o modo de exibição de navegação mostra esse item como selecionado e aciona um evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) . Se o toque resultar em um novo item selecionado, um evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) também é acionado.

Você pode manipular qualquer evento para executar tarefas relacionadas para a navegação solicitada. Qual aquele que você deve tratar depende o comportamento desejado para o seu aplicativo. Normalmente, você navegue até a página solicitada e atualizar o cabeçalho de modo de exibição de navegação em resposta a esses eventos.

**ItemInvoked** é acionado sempre que o usuário toca um item de navegação, mesmo se ele já está selecionado. (O item também pode ser chamado com uma ação equivalente, usando o mouse, teclado ou outra entrada. Para obter mais informações, consulte [entrada e interações](../input/index.md).) Se você navegar no manipulador ItemInvoked, por padrão, a página será recarregada e uma entrada duplicada é adicionada à pilha de navegação. Se você navegar quando um item é invocado, você deve impede recarregar a página ou, certifique-se de que uma entrada duplicada não é criada no backstack de navegação quando a página é recarregada. (Consulte exemplos de código).

**SelectionChanged** pode ser gerado por um usuário invocar um item que não está selecionado no momento, ou alterando programaticamente do item selecionado. Se a alteração de seleção ocorre porque um usuário invocado um item, o evento ItemInvoked ocorre primeiro. Se a alteração de seleção for programática, ItemInvoked não é gerado.

### <a name="backwards-navigation"></a>Navegação para trás

NavigationView tem um botão Voltar integrado; Porém, assim como acontece com navegação a frente, ele não realiza para trás navegação automaticamente. Quando o usuário toca no botão Voltar, o evento [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é acionado. Você manipula esse evento para executar a navegação regressiva. Para obter mais informações e exemplos de código, consulte [histórico de navegação e para trás navegação](../basics/navigation-history-and-backwards-navigation.md).

No modo mínimo ou compacto, o modo de exibição de navegação painel é aberto como um submenu. Nesse caso, clicar no botão Voltar fechará o painel e acionar o evento **PaneClosing** em vez disso.

Você pode ocultar ou desabilitar o botão Voltar ao definir essas propriedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): use para mostrar e ocultar o botão Voltar. Essa propriedade usa um valor da enumeração [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) e é definida como **automático** por padrão. Quando o botão é recolhido, nenhum espaço é reservado para ele no layout.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): use para habilitar ou desabilitar o botão Voltar. Você pode usar a associação dados essa propriedade à propriedade [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) do seu quadro de navegação. **BackRequested** não será acionado se **IsBackEnabled** é **false**.

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

Este exemplo mostra como você pode usar o NavigationView com um painel de navegação superior tamanhos de janela grande e um painel de navegação esquerdo em tamanhos de janela pequena. Ele se adaptem a navegação somente esquerda removendo as configurações de navegação _superior_ no VisualStateManager.

O exemplo demonstra uma maneira recomendada para configurar os dados de navegação que funcionarão para muitos cenários comuns. Ele também demonstra como implementar a navegação com navegação regressiva do NavigationView botão e teclado para trás.

Esse código pressupõe que seu aplicativo contém páginas com os seguintes nomes para navegar até: _home page_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_e _SettingsPage_. O código para essas páginas não é mostrado.

> [!IMPORTANT]
> Informações sobre as páginas do aplicativo são armazenadas em um [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Essa estrutura exige que a versão mínima para o seu projeto de aplicativo deve ser SDK 17763 ou superior. Se você usar a versão WinUI do NavigationView para direcionar versões anteriores do Windows 10, você pode usar o [pacote System.ValueTuple NuGet](https://www.nuget.org/packages/System.ValueTuple/) em vez disso.

> [!IMPORTANT]
> Este código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) do NavigationView. Se você usar a versão da plataforma de NavigationView em vez disso, a versão mínima para o seu projeto de aplicativo deve ser SDK 17763 ou superior. Para usar a versão da plataforma, remova todas as referências a `muxc:`.

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
> Este código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) do NavigationView. Se você usar a versão da plataforma de NavigationView em vez disso, a versão mínima para o seu projeto de aplicativo deve ser SDK 17763 ou superior. Para usar a versão da plataforma, remova todas as referências a `muxc`.

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

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral de Design Fluente para UWP](../fluent-design-system/index.md)
---
Description: O NavigationView é um controle adaptável que implementa os padrões de navegação de nível superior para seu aplicativo.
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
ms.openlocfilehash: 7431e9e41c008471fccdb955a64d44316855de0d
ms.sourcegitcommit: 77df36d2a7391cbc588d44c47ac02d0701092264
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976211"
---
# <a name="navigation-view"></a>Modo de exibição de navegação

O controle NavigationView fornece navegação de nível superior para seu aplicativo. Ele se adapta a vários tamanhos de tela e é compatível com os estilos de navegação _superior_ e _esquerdo_.

![navegação superior](images/nav-view-header.png)<br/>
_A exibição de navegação é compatível com o menu ou painel de navegação superior e esquerdo_

> **APIs da plataforma**: [Classe Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **APIs da biblioteca de interface do usuário do Windows**: [Classe Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Alguns recursos do NavigationView, tais como a navegação _superior_, exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

NavigationView é um controle de navegação adaptável que funciona bem para:

- Fornecer uma experiência de navegação consistente em todos os aplicativos.
- Preservar o estado real da tela de janelas menores.
- Organizar o acesso a várias categorias de navegação.

Para outros padrões de navegação, confira [Noções básicas de design de navegação](../basics/navigation-basics.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/NavigationView">abri-lo e ver o NavigationView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de exibição

> A propriedade PaneDisplayMode requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Use a propriedade PaneDisplayMode para configurar os diferentes estilos de navegação ou modos de exibição para o NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Maior
    O painel está posicionado acima do conteúdo.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de navegação superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

É recomendável a navegação _superior_ quando:

- Você tem cinco ou menos categorias de navegação de nível superior que são igualmente importantes, e qualquer categoria adicional de navegação de nível superior que termina no menu suspenso de estouro é considerada menos importante.
- Você precisa mostrar todas as opções de navegação na tela.
- Você deseja mais espaço para o conteúdo do aplicativo.
- Os ícones não conseguem descrever claramente as categorias de navegação do seu aplicativo.

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

É recomendável a navegação à _esquerda_ quando:

- Você tem de cinco a dez categorias de navegação de nível superior igualmente importantes.
- Você deseja que as categorias de navegação sejam proeminentes, com menos espaço para outros conteúdos do aplicativo.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    O painel mostra apenas os ícones até que seja aberto e está posicionado à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação compacto à esquerda](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Somente o botão de menu é exibido até abrir o painel. Quando está aberto, ele se posiciona à esquerda do conteúdo.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemplo de painel de navegação mínimo à esquerda](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

Por padrão, o PaneDisplayMode é definido como Auto. No modo automático, a exibição de navegação se adapta entre LeftMinimal quando a janela é estreita, para LeftCompact e, em seguida, Left conforme a janela fica mais ampla. Para saber mais, confira a seção [comportamento adaptável](#adaptive-behavior).

![Comportamento adaptável padrão da navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável padrão do modo de exibição de navegação_

## <a name="anatomy"></a>Anatomia

Essas imagens mostram o layout do painel, cabeçalho e áreas de conteúdo do controle quando configurado para navegação _superior_ ou _esquerda_.

![Layout de exibição de navegação superior](images/topnav-anatomy.png)<br/>
_Layout de navegação superior_

![Layout de exibição de navegação à esquerda](images/leftnav-anatomy.png)<br/>
_Layout de navegação à esquerda_

### <a name="pane"></a>Painel

É possível usar a propriedade PaneDisplayMode para posicionar o painel acima do conteúdo ou à esquerda do conteúdo.

O painel NavigationView pode conter:

- Objetos [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem). Itens de navegação para navegar para páginas específicas.
- Objetos [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator). Separadores para agrupar itens de navegação. Defina a propriedade [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) como 0 para renderizar o separador como espaço.
- Objetos [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader). Cabeçalhos para rotular os grupos de itens.
- Um controle [AutoSuggestBox](auto-suggest-box.md) opcional para permitir a pesquisa no nível do aplicativo. Atribua o controle à propriedade [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox).
- Um ponto de entrada opcional para [configurações do aplicativo](../app-settings/app-settings-and-data.md). Para ocultar o item de configurações, defina a propriedade [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) como **false**.

O painel esquerdo também contém:

- Um botão de menu para alternar o painel entre aberto e fechado. Em janelas maiores do aplicativo quando o painel está aberto, opte por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

O modo de exibição de navegação tem um botão Voltar posicionado no canto superior esquerdo do painel. No entanto, ele não controla automaticamente a navegação regressiva e adiciona conteúdo à pilha Voltar. Para habilitar a [Navegação regressiva](#backwards-navigation) confira a seção correspondente.

Esta é a anatomia detalhada do painel nas posições superior e à esquerda.

#### <a name="top-navigation-pane"></a>Painel de navegação superior

![Anatomia do painel superior do modo de exibição de navegação](images/navview-pane-anatomy-horizontal.png)

1. Cabeçalhos
1. Itens de navegação
1. Separadores
1. AutoSuggestBox (opcional)
1. Botão Configurações (opcional)

#### <a name="left-navigation-pane"></a>Painel de navegação à esquerda

![Anatomia do painel à esquerda do modo de exibição de navegação](images/navview-pane-anatomy-vertical.png)

1. Botão Menu
1. Itens de navegação
1. Separadores
1. Cabeçalhos
1. AutoSuggestBox (opcional)
1. Botão Configurações (opcional)

#### <a name="pane-footer"></a>Rodapé do painel

Adicione conteúdo livremente ao rodapé do painel com a propriedade [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter).

:::row:::
    :::column:::
    ![Rodapé do painel na navegação superior](images/navview-freeform-footer-top.png)<br>
     _Rodapé do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Rodapé do painel na navegação à esquerda](images/navview-freeform-footer-left.png)<br>
    _Rodapé do painel à esquerda_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Cabeçalho e título do painel

Adicione conteúdo de texto à área de cabeçalho do painel definindo a propriedade [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle). Ela usa uma cadeia de caracteres e mostra o texto ao lado do botão de menu.

Para adicionar conteúdo não textual, como uma imagem ou logotipo, coloque qualquer elemento no cabeçalho do painel adicionando-o à propriedade [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader).

Se PaneTitle e PaneHeader estão definidas, o conteúdo é empilhado horizontalmente ao lado do botão de menu, com PaneTitle mais próximo ao botão do menu.

:::row:::
    :::column:::
    ![Cabeçalho do painel de navegação superior](images/navview-freeform-header-top.png)<br>
     _Cabeçalho do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Cabeçalho do painel de navegação à esquerda](images/navview-freeform-header-left.png)<br>
    _Cabeçalho do painel à esquerda_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Conteúdo do painel

Adicione conteúdo livremente ao painel com a propriedade [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent).

:::row:::
    :::column:::
    ![Conteúdo personalizado do painel na navegação superior](images/navview-freeform-pane-top.png)<br>
     _Conteúdo personalizado do painel superior_<br>
    :::column-end:::
    :::column:::
    ![Conteúdo personalizado do painel na navegação à esquerda](images/navview-freeform-pane-left.png)<br>
    _Conteúdo personalizado do painel à esquerda_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Cabeçalho

Adicione um título de página definindo a propriedade [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

![Exemplo da área de cabeçalho do modo de exibição de navegação](images/nav-header.png)<br/>
_Cabeçalho do modo de exibição de navegação_

A área do cabeçalho é alinhada verticalmente com o botão de navegação na posição do painel à esquerda e se encontra abaixo do painel na posição do painel superior. Ele tem uma altura fixa de 52 px. Sua finalidade é conter o título da página para a categoria de navegação selecionada. O cabeçalho é ancorado à parte superior da página e atua como um ponto de corte de rolagem para a área de conteúdo.

O cabeçalho fica visível sempre que NavigationView está no modo de exibição Minimal. É possível escolher ocultar o cabeçalho em outros modos, que são usados em larguras de janela maiores. Para ocultar o cabeçalho, defina a propriedade [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) como **false**.

### <a name="content"></a>Conteúdo

![Exemplo da área de conteúdo do modo de exibição de navegação](images/nav-content.png)<br/>
_Conteúdo do modo de exibição de navegação_

A área de conteúdo é onde a maioria das informações da categoria de navegação selecionada é exibida.

É recomendável usar margens de 12 px na área do conteúdo quando NavigationView está no modo **Minimal**, caso contrário, use margens de 24 px.

## <a name="adaptive-behavior"></a>Comportamento adaptável

Por padrão, o modo de exibição da navegação muda automaticamente a exibição com base na quantidade de espaço disponível na tela. As propriedades [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) e [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) especificam os pontos de interrupção nos quais o modo de exibição é alterado. É possível modificar esses valores para personalizar o comportamento do modo de exibição adaptável.

### <a name="default"></a>Padrão

Quando PaneDisplayMode é definido com o valor padrão **Auto**, o comportamento adaptável é exibir:

- Um painel expandido à esquerda em larguras de janelas grandes (1008px ou superior).
- À esquerda, somente ícones, painel de navegação (LeftCompact) em larguras de janelas médias (641px a 1007px).
- Apenas um botão de menu (LeftMinimal) em larguras de janelas pequenas (640 px ou menos).

Para saber mais sobre os tamanhos de janelas para o comportamento adaptável, confira [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamento adaptável padrão da navegação à esquerda](images/displaymode-auto.png)<br/>
_Comportamento adaptável padrão do modo de exibição de navegação_

### <a name="minimal"></a>Minimal

Um segundo padrão adaptável comum é usar um painel expandido à esquerda em larguras de janelas grandes e apenas um botão de menu em larguras de janelas médias e pequenas.

Recomendamos isso quando:

- Você deseja mais espaço para o conteúdo do aplicativo em larguras de janela pequenas.
- Suas categorias de navegação não podem ser representadas claramente com ícones.

![Comportamento adaptável mínimo de navegação à esquerda](images/adaptive-behavior-minimal.png)<br/>
_Comportamento adaptável "mínimo" do modo de exibição de navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth à largura na qual você deseja que o painel seja recolhido. Aqui, ele é alterado do padrão de 640 para 1007. Você também precisa definir ExpandedModeThresholdWidth para garantir que os valores não entrem em conflito.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Um terceiro padrão adaptável comum é usar um painel expandido à esquerda em larguras de janela grandes e apenas um painel de navegação LeftCompact, somente com ícones, nas larguras de janela médias e pequenas.

Recomendamos isso quando:

- É importante sempre mostrar todas as opções de navegação na tela.
- Suas categorias de navegação podem ser representadas claramente com ícones.

![Comportamento adaptável compacto de navegação à esquerda](images/adaptive-behavior-compact.png)<br/>
_Comportamento adaptável "compacto" do modo de exibição de navegação_

Para configurar esse comportamento, defina CompactModeThresholdWidth como 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Sem comportamento adaptável

Para desabilitar o comportamento adaptável automático, defina PaneDisplayMode com um valor diferente de Auto. Aqui, ele está definido como LeftMinimal, portanto, somente o botão de menu é exibido, independentemente da largura da janela.

![Navegação à esquerda sem comportamento adaptável](images/adaptive-behavior-none.png)<br/>
_Modo de exibição de navegação com PaneDisplayMode definido como LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Como descrito anteriormente na seção _Modos de exibição_, é possível configurar o painel para estar sempre visível, sempre expandido, sempre compacto ou sempre mínimo. Também é possível gerenciar os modos de exibição no código do aplicativo. Um exemplo disso é exibido na próxima seção.

### <a name="top-to-left-navigation"></a>Navegação da parte superior para a esquerda

Ao usar o painel de navegação superior no seu aplicativo, os itens de navegação são recolhidos em um menu de estouro conforme a largura de janela diminui. Se a janela de aplicativo for estreita, a experiência do usuário pode melhorar quando se alterna o PaneDisplayMode da navegação de Top para LeftMinimal, ao invés de recolher todos os itens em um menu de estouro.

É recomendável usar a navegação superior em tamanhos de janela grandes e a navegação à esquerda em tamanhos de janela pequenos quando:

- Você tem um conjunto de categorias de navegação de nível superior igualmente importantes para serem exibidas juntas de modo que, se uma categoria desse conjunto não couber na tela, você recolhe para navegação à esquerda para dar a todas a mesma importância.
- Você deseja preservar o máximo de espaço de conteúdo possível em tamanhos de janela pequenos.

Este exemplo mostra como usar uma propriedade [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) e [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) para alternar entre a navegação Top e LeftMinimal.

![Exemplo de comportamento adaptável superior ou à esquerda 1](images/navigation-top-to-left.png)

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
> Ao usar AdaptiveTrigger.MinWindowWidth, o estado visual é acionado quando a janela é maior do que a largura mínima especificada. Isso significa que o XAML padrão define a janela estreita e o VisualState define as modificações aplicadas quando a janela é ampliada. O PaneDisplayMode padrão para o modo de exibição de navegação é Auto, portanto, quando a largura da janela é menor ou igual a CompactModeThresholdWidth, usa-se a navegação LeftMinimal. Quando a janela é ampliada, o VisualState substitui o padrão, e usa-se o painel de navegação superior.

## <a name="navigation"></a>Navegação

O modo de exibição de navegação não realiza nenhuma tarefa de navegação automaticamente. Quando o usuário toca em um item de navegação, o modo de exibição de navegação mostra esse item como selecionado e gera um evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Se o toque resultar em um novo item sendo selecionado, também é gerado um evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged).

É possível manipular o evento para executar tarefas relacionadas à navegação solicitada. O comportamento desejado para seu aplicativo vai definir com qual deles você lidará. Normalmente é possível navegar até a página solicitada e atualizar o cabeçalho do modo de exibição de navegação em resposta a esses eventos.

O **ItemInvoked** é gerado sempre que o usuário toca em um item de navegação, mesmo se ele já estiver selecionado. O item também pode ser chamado com uma ação equivalente usando o mouse, teclado ou outra entrada. Para saber mais, confira [Entrada e interações](../input/index.md). Se você navegar no manipulador de ItemInvoked, por padrão a página é recarregada e uma entrada duplicada é adicionada à pilha de navegação. Se você navegar quando um item for chamado, precisará proibir o recarregamento da página ou garantir que uma entrada duplicada não seja criada no backstack de navegação ao recarregar a página. (Veja os exemplos de código.)

**SelectionChanged** pode ser gerado por um usuário chamando um item que não está selecionado ou alterando o item selecionado via programação. Se a alteração da seleção ocorreu porque um usuário chamou um item, o evento ItemInvoked ocorre primeiro. Se a alteração da seleção ocorreu via programação, ItemInvoked não será gerado.

### <a name="backwards-navigation"></a>Navegação regressiva

O NavigationView tem um botão Voltar interno, mas, assim como acontece com a navegação progressiva, ele não realiza a navegação regressiva automaticamente. Quando o usuário toca no botão Voltar, o evento [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) é gerado. Manipule esse evento para executar a navegação regressiva. Para saber mais e obter exemplos de código, confira [Histórico de navegação e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md).

No modo Minimal ou Compact, o Painel do modo de exibição de navegação abre como um submenu. Nesse caso, clicar no botão Voltar fecha o Painel e gera o evento **PaneClosing**.

É possível ocultar ou desabilitar o botão Voltar configurando essas propriedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): use para mostrar e ocultar o botão Voltar. Essa propriedade usa um valor da enumeração [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) e é configurada como **Auto** por padrão. Quando o botão é recolhido, nenhum espaço fica reservado para ele no layout.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): use para habilitar ou desabilitar o botão Voltar. É possível associar os dados dessa propriedade à propriedade [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) de seu quadro de navegação. **BackRequested** não é gerado se **IsBackEnabled** é **false**.

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

Este exemplo mostra como usar o NavigationView com um painel de navegação superior em tamanhos de janela grandes e um painel de navegação à esquerda em tamanhos de janela pequenos. Ele pode ser adaptado para navegação somente à esquerda ao remover as configurações de navegação _top_ no VisualStateManager.

O exemplo demonstra a maneira recomendada para configurar dados de navegação que funcionarão para diversos cenários comuns. Também demonstra como implementar a navegação regressiva com o botão Voltar e a navegação de teclado de NavigationView.

Esse código pressupõe que seu aplicativo contém páginas com os seguintes nomes: _HomePage_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_ e _SettingsPage_. O código dessas páginas não é exibido.

> [!IMPORTANT]
> As informações sobre as páginas do aplicativo são armazenadas em [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Essa estrutura requer que a versão mínima de seu projeto de aplicativo seja a SDK 17763 ou posterior. Caso queira usar a versão WinUI do NavigationView visando versões anteriores do Windows 10, é possível usar o [pacote System.ValueTuple do NuGet](https://www.nuget.org/packages/System.ValueTuple/).

> [!IMPORTANT]
> Esse código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) do NavigationView. Se, em vez disso, você usar a versão da plataforma do NavigationView, a versão mínima para seu projeto de aplicativo deverá ser a SDK 17763 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxc:`.

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
> Esse código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) do NavigationView. Se, em vez disso, você usar a versão da plataforma do NavigationView, a versão mínima para seu projeto de aplicativo deverá ser a SDK 17763 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxc`.

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

Abaixo apresentamos uma versão do [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) para o manipulador **NavView_ItemInvoked** do exemplo de código C# acima. A técnica no manipulador C++/WinRT envolve primeiro armazenar o nome de tipo completo da página para a qual você deseja navegar na marca do [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem). No manipulador, é possível converter esse valor, transformá-lo em um objeto [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) e usá-lo para navegar até a página de destino. A variável de mapeamento `_pages`, vista no exemplo de C#, não é necessária e é possível criar testes de unidade para confirmar que os valores dentro de suas marcas são de um tipo válido. Confira também [Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

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

## <a name="navigation-view-customization"></a>Personalização do modo de exibição de navegação

### <a name="pane-backgrounds"></a>Telas de fundo do painel

Por padrão, o painel do NavigationView usa uma tela de fundo diferente que depende do modo de exibição:

- O painel tem uma cor cinza sólida quando expandido à esquerda, lado a lado com o conteúdo (no modo à esquerda).
- O painel usa acrílico do aplicativo quando abre como uma sobreposição sobre o conteúdo (no modo compacto, mínimo ou superior).

Para modificar a tela de fundo do painel, substitua os recursos de tema do XAML usados para renderizar a tela de fundo em cada modo. (Essa técnica é usada em vez da propriedade PaneBackground individual para dar suporte a telas de fundo diferentes para vários modos de exibição.)

Essa tabela mostra quais recursos de temas são usados em cada modo de exibição.

| Modo de exibição | Recursos de tema |
| ------------ | -------------- |
| Esquerda | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Maior | NavigationViewTopPaneBackground |

Este exemplo mostra como substituir os recursos de tema em App.xaml. Ao substituir os recursos de tema, forneça sempre no mínimo os dicionários de recursos "Default" e "HighContrast" e os dicionários para os recursos "Light" ou "Dark", conforme necessário. Para saber mais, confira [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) do AcrylicBrush. Se, em vez disso, você usar a versão da plataforma do AcrylicBrush, a versão mínima para seu projeto de aplicativo deverá ser a SDK 16299 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxm:`.

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

### <a name="top-whitespace"></a>Espaço em branco na parte superior
Alguns aplicativos optam por [personalizar a barra de título](https://docs.microsoft.com/windows/uwp/design/shell/title-bar) da janela, potencialmente estendendo o conteúdo do aplicativo para a área da barra de título. Quando NavigationView é o elemento raiz em aplicativos que se estendem para a barra de título **usando a API [ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar)** , o controle ajusta automaticamente a posição dos próprios elementos interativos para evitar sobreposição com [a região arrastável](https://docs.microsoft.com/windows/uwp/design/shell/title-bar#draggable-regions). 
![Um aplicativo que se estende para a barra de título](images/navigation-view-with-titlebar-padding.png)

Se o aplicativo especifica a região arrastável chamando o método [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) e você prefere fazer com que os botões voltar e menu fiquem mais perto da parte superior da janela do aplicativo, defina `IsTitleBarAutoPaddingEnabled` como False.

![Aplicativo estendendo-se para a barra de título sem preenchimento extra](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>Comentários
Para ajustar ainda mais a posição da área de cabeçalho de NavigationView, substitua o recurso de tema XAML *NavigationViewHeaderMargin*, por exemplo, em seus Recursos de página.

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

Esse recurso de tema modifica a margem em volta de [NavigationView.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral de Design Fluente para UWP](/windows/apps/fluent-design-system)

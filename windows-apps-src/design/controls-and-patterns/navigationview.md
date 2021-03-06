---
Description: O NavigationView é um controle adaptável que implementa os padrões de navegação de nível superior para seu aplicativo.
title: Modo de exibição de navegação
template: detail.hbs
ms.date: 05/02/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2ac2e95e4233a490187066e0d19f1eeb4f330265
ms.sourcegitcommit: 3b8fac693c0b031def5cedc8ae3632a2aa00f1f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84467746"
---
# <a name="navigation-view"></a>Modo de exibição de navegação

O controle NavigationView fornece navegação de nível superior para seu aplicativo. Ele se adapta a vários tamanhos de tela e é compatível com os estilos de navegação _superior_ e _esquerdo_.

![navegação superior](images/nav-view-header.png)<br/>
_A exibição de navegação é compatível com o menu ou painel de navegação superior e esquerdo_

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **NavigationView** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos do Windows. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/). |

> **APIs da plataforma**: [Classe Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **APIs da biblioteca de interface do usuário do Windows**: [Classe Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Alguns recursos do NavigationView, como a navegação _superior_ e _hierárquica_, exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou superior ou a [Biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/).

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
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
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

> A propriedade PaneDisplayMode requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [Biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/).

Use a propriedade PaneDisplayMode para configurar os diferentes estilos de navegação ou modos de exibição para o NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Parte superior
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

- Um botão de menu para alternar o painel entre aberto e fechado. Em janelas maiores do aplicativo quando o painel está aberto, opte por ocultar este botão usando a propriedade [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

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

Adicione conteúdo livremente ao rodapé do painel com a propriedade [PaneFooter](/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter).

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

Adicione conteúdo de texto à área de cabeçalho do painel definindo a propriedade [PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle). Ela usa uma cadeia de caracteres e mostra o texto ao lado do botão de menu.

Para adicionar conteúdo não textual, como uma imagem ou logotipo, coloque qualquer elemento no cabeçalho do painel adicionando-o à propriedade [PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader).

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

Adicione conteúdo livremente ao painel com a propriedade [PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent).

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

### <a name="header"></a>parâmetro

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
        ![Botão voltar de exibição de navegação no painel de navegação à esquerda](images/leftnav-back.png)<br/>
        _O botão voltar no painel de navegação à esquerda_
    :::column-end:::
    :::column:::
        ![Botão voltar de exibição de navegação no painel de navegação superior](images/topnav-back.png)<br/>
        _O botão voltar no painel de navegação superior_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemplo de código

> [!IMPORTANT]
> Para qualquer projeto que usa o kit de ferramentas da Biblioteca de WinUI (Interface do Usuário do Windows), siga as mesmas etapas preliminares de configuração. Para obter mais informações de contexto, configuração e suporte, confira [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started).

Este exemplo mostra como é possível usar o **NavigationView** com um painel de navegação superior em tamanhos de janela grandes e com um painel de navegação à esquerda em tamanhos de janela pequenos. Ele pode ser adaptado à navegação somente à esquerda removendo as configurações de navegação *superior* no **VisualStateManager**.

O exemplo demonstra a maneira recomendada para configurar dados de navegação que funcionarão para diversos cenários comuns. Ele também demonstra como implementar a navegação regressiva com o botão Voltar e com a navegação de teclado do **NavigationView**.

Esse código pressupõe que seu aplicativo contém páginas com os seguintes nomes: *HomePage*, *AppsPage*, *GamesPage*, *MusicPage*, *MyContentPage* e *SettingsPage*. O código dessas páginas não é exibido.

> [!IMPORTANT]
> As informações sobre as páginas do aplicativo são armazenadas em [ValueTuple](/dotnet/api/system.valuetuple). Essa estrutura requer que a versão mínima de seu projeto de aplicativo seja a SDK 17763 ou posterior. Caso queira usar a versão WinUI do NavigationView visando versões anteriores do Windows 10, é possível usar o [pacote System.ValueTuple do NuGet](https://www.nuget.org/packages/System.ValueTuple/).

> [!IMPORTANT]
> Esse código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/) do NavigationView. Se, em vez disso, você usar a versão da plataforma do NavigationView, a versão mínima para seu projeto de aplicativo deverá ser a SDK 17763 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxc:`.

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
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
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
> Esse código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/) do NavigationView. Se, em vez disso, você usar a versão da plataforma do NavigationView, a versão mínima para seu projeto de aplicativo deverá ser a SDK 17763 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

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
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = Windows.System.VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = Windows.System.VirtualKey.Left,
        Modifiers = Windows.System.VirtualKeyModifiers.Menu
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

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
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

private void NavView_Navigate(string navItemTag, Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
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

> [!NOTE]
> Para a versão [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) deste exemplo de código, comece criando um projeto com base no modelo de projeto **Aplicativo em Branco (C++/WinRT)** e adicione o código da lista aos arquivos de código-fonte indicados. Para usar o código-fonte exatamente como mostrado na lista, dê ao novo projeto o nome *NavigationViewCppWinRT*

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage()
        {
            InitializeComponent();
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
        }

        double MainPage::NavViewCompactModeThresholdWidth()
        {
            return NavView().CompactModeThresholdWidth();
        }

        void ContentFrame_NavigationFailed(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
        {
            throw winrt::hresult_error(E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
        }

        // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

        void NavView_Loaded(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
        {
            // You can also add items in code.
            NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
            muxc::NavigationViewItem navigationViewItem;
            navigationViewItem.Content(winrt::box_value(L"My content"));
            navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
            navigationViewItem.Tag(winrt::box_value(L"content"));
            NavView().MenuItems().Append(navigationViewItem);
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

            // Add handler for ContentFrame navigation.
            ContentFrame().Navigated({ this, &MainPage::On_Navigated });

            // NavView doesn't load any page by default, so load home page.
            NavView().SelectedItem(NavView().MenuItems().GetAt(0));
            // If navigation occurs on SelectionChanged, this isn't needed.
            // Because we use ItemInvoked to navigate, we need to call Navigate
            // here to load the home page.
            NavView_Navigate(L"home", Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

            // Add keyboard accelerators for backwards navigation.
            Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
            goBack.Key(Windows::System::VirtualKey::GoBack);
            goBack.Invoked({ this, &MainPage::BackInvoked });
            KeyboardAccelerators().Append(goBack);

            // ALT routes here
            Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
            goBack.Key(Windows::System::VirtualKey::Left);
            goBack.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
            goBack.Invoked({ this, &MainPage::BackInvoked });
            KeyboardAccelerators().Append(altLeft);
        }

        void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const& /* sender */, muxc::NavigationViewItemInvokedEventArgs const& args)
        {
            if (args.IsSettingsInvoked())
            {
                NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
            }
            else if (args.InvokedItemContainer())
            {
                NavView_Navigate(winrt::unbox_value_or<winrt::hstring>(args.InvokedItemContainer().Tag(), L"").c_str(), args.RecommendedNavigationTransitionInfo());
            }
        }

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(muxc::NavigationView const& /* sender */, muxc::NavigationViewSelectionChangedEventArgs const& args)
        {
            if (args.IsSettingsSelected())
            {
                NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
            }
            else if (args.SelectedItemContainer())
            {
                NavView_Navigate(winrt::unbox_value_or<winrt::hstring>(args.SelectedItemContainer().Tag(), L"").c_str(), args.RecommendedNavigationTransitionInfo());
            }
        }

        void NavView_Navigate(std::wstring navItemTag, Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
        {
            Windows::UI::Xaml::Interop::TypeName pageTypeName;
            if (navItemTag == L"settings")
            {
                pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
            }
            else
            {
                for (auto&& eachPage : m_pages)
                {
                    if (eachPage.first == navItemTag)
                    {
                        pageTypeName = eachPage.second;
                        break;
                    }
                }
            }
            // Get the page type before navigation so you can prevent duplicate
            // entries in the backstack.
            Windows::UI::Xaml::Interop::TypeName preNavPageType = ContentFrame().CurrentSourcePageType();

            // Navigate only if the selected page isn't currently loaded.
            if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
            {
                ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
            }
        }

        void NavView_BackRequested(muxc::NavigationView const& /* sender */, muxc::NavigationViewBackRequestedEventArgs const& /* args */)
        {
            On_BackRequested();
        }

        void BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */, Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
        {
            On_BackRequested();
            args.Handled(true);
        }

        bool On_BackRequested()
        {
            if (!ContentFrame().CanGoBack())
                return false;

            // Don't go back if the nav pane is overlaid.
            if (NavView().IsPaneOpen() &&
                (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                    NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
                return false;

            ContentFrame().GoBack();
            return true;
        }

        void On_Navigated(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
        {
            NavView().IsBackEnabled(ContentFrame().CanGoBack());

            if (ContentFrame().SourcePageType().Name == winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
            {
                // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
                NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
                NavView().Header(winrt::box_value(L"Settings"));
            }
            else if (ContentFrame().SourcePageType().Name != L"")
            {
                for (auto&& eachPage : m_pages)
                {
                    if (eachPage.second.Name == args.SourcePageType().Name)
                    {
                        for (auto&& eachMenuItem : NavView().MenuItems())
                        {
                            auto navigationViewItem = eachMenuItem.try_as<muxc::NavigationViewItem>();
                            {
                                if (navigationViewItem)
                                {
                                    winrt::hstring hstringValue = winrt::unbox_value_or<winrt::hstring>(navigationViewItem.Tag(), L"");
                                    if (hstringValue == eachPage.first)
                                    {
                                        NavView().SelectedItem(navigationViewItem);
                                        NavView().Header(navigationViewItem.Content());
                                    }
                                }
                            }
                        }
                        break;
                    }
                }
            }
        }
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
}
```

### <a name="alternative-cwinrt-implementation"></a>Implementação alternativa do C++/WinRT

O código C# e C++/WinRT mostrado acima foi desenvolvido para que você possa usar a mesma marcação XAML para ambas as versões. No entanto, há outra maneira de implementar a versão C++/WinRT descrita nesta seção, a qual você talvez prefira.

Abaixo é mostrada uma versão alternativa do manipulador de **NavView_ItemInvoked**. A técnica nessa versão do manipulador requer que primeiro você armazene (na marca do [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) o nome de tipo completo da página até a qual você deseja navegar. No manipulador, é possível converter esse valor, transformá-lo em um objeto [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) e usá-lo para navegar até a página de destino. A variável de mapeamento chamada `_pages`, que pode ser vista nos exemplos acima, não é necessária. Além disso, será possível criar testes de unidade que confirmam que os valores dentro das marcas são de um tipo válido. Confira também [Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

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

## <a name="hierarchical-navigation"></a>Navegação hierárquica
Alguns aplicativos podem ter uma estrutura hierárquica mais complexa que exija mais do que apenas uma lista simples de itens de navegação. Talvez seja interessante usar itens de navegação de nível superior para exibir categorias de páginas, com itens filhos exibindo páginas específicas. Também será útil se você tiver páginas de estilo de hub que só se vinculem a outras páginas. Para esses tipos de casos, você deve criar um NavigationView hierárquico.

Para mostrar uma lista hierárquica de itens de navegação aninhados no painel, use a propriedade [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems?view=winui-2.4) ou a propriedade [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource?view=winui-2.4) de **NavigationViewItem**.
Cada NavigationViewItem pode conter outros NavigationViewItems e elementos de organização, como cabeçalhos e separadores de itens. Para mostrar uma lista hierárquica ao usar `MenuItemsSource`, defina o `ItemTemplate` como um NavigationViewItem e associe a propriedade `MenuItemsSource` dele ao próximo nível da hierarquia.

Embora NavigationViewItem possa conter qualquer número de níveis aninhados, é recomendável manter superficial a hierarquia de navegação do seu aplicativo. Acreditamos que dois níveis são ideais para usabilidade e compreensão.

NavigationView mostra a hierarquia nos modos de exibição de painel Top, Left e LeftCompact. Veja como é a aparência de uma subárvore expandida em cada um dos modos de exibição de painel:

![NavigationView com hierarquia](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>Adicionar uma hierarquia de itens na marcação
Declare uma hierarquia de navegação de aplicativo na marcação.

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>Adicionar uma hierarquia de itens usando a associação de dados

Adicione uma hierarquia de itens de menu ao NavigationView 
* associando a propriedade MenuItemsSource aos dados hierárquicos
* que definem o modelo de item a ser um NavigationViewMenuItem, com o conjunto de conteúdo definido como o rótulo do item de menu e a propriedade MenuItemsSource associada ao próximo nível da hierarquia

Este exemplo também demonstra os eventos de [Expansão](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding?view=winui-2.4) e [Recolhimento](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed?view=winui-2.4). Esses eventos são gerados para um item de menu com filhos.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
    <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
</DataTemplate>
<muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
    
    <StackPanel Margin="10,10,0,0">
        <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
        <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
    </StackPanel>    
</muxc:NavigationView>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String Icon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}
    
public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  
    
    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu Item 1",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
               new Category(){
                    Name = "Menu Item 2",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { 
                            Name  = "Menu Item 2", 
                            Icon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu Item 3", Icon = "Icon" },
                                new Category() { Name  = "Menu Item 4", Icon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu Item 5",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 6",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu Item 7", Icon = "Icon" },
                        new Category() { Name  = "Menu Item 8", Icon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu Item 9", Icon = "Icon" }
    };
    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last Expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last Collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```
### <a name="selection"></a>Seleção
Por padrão, qualquer item pode conter filhos, ser invocado ou ser selecionado.
Ao fornecer aos usuários uma árvore hierárquica de opções de navegação, você pode optar por tornar itens pai não selecionáveis, por exemplo, quando seu aplicativo não tem uma página de destino associada a esse item pai. Se os itens pai _forem_ selecionáveis, é recomendável usar os modos de exibição de painel Left-Expanded ou Superior. O modo LeftCompact fará o usuário navegar até o item pai a fim de abrir a subárvore filha sempre que ela for chamada.

Os itens selecionados desenharão indicadores de seleção ao longo da borda esquerda quando estiverem no modo esquerdo ou na borda inferior quando estiverem no modo superior. Abaixo estão NavigationViews nos modos esquerdo e superior em que um item pai está selecionado.

![NavigationView no modo esquerdo com pai selecionado](images/navigation-view-selection.png)

![NavigationView no modo superior com pai selecionado](images/navigation-view-selection-top.png)

O item selecionado pode nem sempre permanecer visível. Se um filho em uma subárvore recolhida/não expandida for selecionado, o primeiro ancestral visível será exibido como selecionado. O indicador de seleção voltará para o item selecionado se/quando a subárvore for expandida.

Por exemplo, na imagem acima, o item Calendário pode ser selecionado pelo usuário e o usuário pode recolher a subárvore dele. Nesse caso, o indicador de seleção apareceria abaixo do item Conta, pois a Conta é o primeiro ancestral visível do Calendário. O indicador de seleção será movido de volta para o item Calendário à medida que o usuário expandir a subárvore novamente. 

O NavigationView inteiro mostrará, no máximo, um indicador de seleção.

Nos modos Superior e Esquerdo, clicar nas setas em NavigationViewItems expandirá ou recolherá a subárvore. Clicar ou tocar em _em outro lugar_ no NavigationViewItem vai disparar o evento `ItemInvoked` e também vai recolher ou expandir a subárvore.

Para impedir que um item mostre o indicador de seleção quando invocado, defina a propriedade [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked?view=winui-2.3) dele como False, conforme mostrado abaixo:

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
    <muxc:NavigationViewItem Content="{x:Bind Name}" 
        MenuItemsSource="{x:Bind Children}"
        SelectsOnInvoked="{x:Bind IsLeaf}" />
</DataTemplate>
<muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
   
</muxc:NavigationView>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String Icon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}
    
public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }      
    
    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu Item 1",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 2",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { 
                            Name  = "Menu Item 2", 
                            Icon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu Item 3", Icon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu Item 4", Icon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu Item 5",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 6",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu Item 7", Icon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu Item 8", Icon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu Item 9", Icon = "Icon", IsLeaf = true }
    };
}
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>Teclado no NavigationView hierárquico
Os usuários podem mover o foco ao redor do modo de exibição de navegação usando o [teclado](/windows/uwp/design/input/keyboard-interactions). As teclas de seta expõem a “navegação interna” com o painel e seguem as interações fornecidas no [modo de exibição de árvore](/windows/uwp/design/controls-and-patterns/tree-view). As principais ações mudam ao navegar pelo NavigationView ou pelo submenu dele, exibido nos modos Superior e Left-compact do HierarchicalNavigationView. Abaixo estão as ações específicas de cada tecla em um NavigationView hierárquico:

| Chave      |      No modo Left      |  No modo Superior | No submenu  |
|----------|------------------------|--------------|------------|
| Up |Move o foco para o item diretamente acima do item em foco no momento. | Não faz nada. |Move o foco para o item diretamente acima do item em foco no momento.|
| Down|Move o foco diretamente abaixo do item em foco no momento.* | Não faz nada. | Move o foco diretamente abaixo do item em foco no momento.* |
| Direita |Não faz nada.  |Move o foco para o item diretamente à direita do item em foco no momento. |Não faz nada.|
| Esquerda |Não faz nada. | Move o foco para o item diretamente à esquerda do item em foco no momento.  |Não faz nada. |
| Espaço/Enter |Se o item tem filhos, expande/recolhe o item e não altera o foco.   | Se o item tem filhos, expande os filhos para um submenu e coloca o foco no primeiro item do submenu. | Invoca/seleciona o item e fecha o submenu. |
| Esc | Não faz nada. | Não faz nada. | Fecha o submenu.|

A tecla de espaço ou enter sempre invoca/seleciona um item.

*Observe que os itens não precisam ser visualmente adjacentes, o foco será movido do último item na lista do painel para o item de configurações. 

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
| Parte superior | NavigationViewTopPaneBackground |

Este exemplo mostra como substituir os recursos de tema em App.xaml. Ao substituir os recursos de tema, forneça sempre no mínimo os dicionários de recursos "Default" e "HighContrast" e os dicionários para os recursos "Light" ou "Dark", conforme necessário. Para saber mais, confira [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código mostra como usar a versão da [Biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/) do AcrylicBrush. Se, em vez disso, você usar a versão da plataforma do AcrylicBrush, a versão mínima para seu projeto de aplicativo deverá ser a SDK 16299 ou posterior. Para usar a versão da plataforma, remova todas as referências a `muxm:`.

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
Alguns aplicativos optam por [personalizar a barra de título](/windows/uwp/design/shell/title-bar) da janela, potencialmente estendendo o conteúdo do aplicativo para a área da barra de título. Quando NavigationView é o elemento raiz em aplicativos que se estendem para a barra de título **usando a API [ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar)** , o controle ajusta automaticamente a posição dos próprios elementos interativos para evitar sobreposição com [a região arrastável](/windows/uwp/design/shell/title-bar#draggable-regions). 
![Um aplicativo que se estende para a barra de título](images/navigation-view-with-titlebar-padding.png)

Se o aplicativo especifica a região arrastável chamando o método [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) e você prefere fazer com que os botões voltar e menu fiquem mais perto da parte superior da janela do aplicativo, defina `IsTitleBarAutoPaddingEnabled` como False.

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

Esse recurso de tema modifica a margem em volta de [NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Tópicos relacionados

- [Classe NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
- [Mestre/detalhes](master-details.md)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Visão geral do Fluent Design](/windows/apps/fluent-design-system)

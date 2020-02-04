---
Description: O TabView é uma maneira flexível de organizar vários documentos em guias dinâmicas
title: Modo de exibição de guia
template: detail.hbs
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 24c7bd8828ec036135233f569ee7add5d39ffb32
ms.sourcegitcommit: 136416e8e2eb0565bb6eb99e42482c1723ccb8c7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76890421"
---
# <a name="tabview"></a>TabView

O controle TabView é uma maneira de exibir um conjunto de guias e seu respectivo conteúdo. TabViews são úteis para exibir várias páginas (ou documentos) de conteúdo e, ao mesmo tempo, dar a um usuário a capacidade de reorganizar, abrir ou fechar novas guias.

> **APIs importantes**: [classe TabView](/uwp/api/microsoft.ui.xaml.controls.tabview), [classe TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

![Exemplo de um TabView](images/tabview/tab-introduction.png)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Em geral, as interfaces do usuário com guias vêm em um dos dois estilos distintos que diferem na função e na aparência: As **guias estáticas** são o tipo de guias geralmente encontradas em janelas de configurações. Elas contêm um número definido de páginas em uma ordem fixa que geralmente contém conteúdo predefinido.
As **guias do documento** são o tipo de guias encontrado em um navegador, como o Microsoft Edge. Os usuários podem criar, remover e reorganizar guias; mover guias entre janelas; e alterar o conteúdo das guias.

O [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) oferece guias de documento para aplicativos UWP. Use um TabView quando:

- Os usuários puderem abrir, fechar ou reorganizar tabelas dinamicamente.
- Os usuários puderem abrir documentos ou páginas da Web diretamente em guias.
- Os usuários puderem arrastar e soltar as guias entre janelas.

Se um TabView não for apropriado para seu aplicativo, considere usar controles como [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot) ou [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview).

## <a name="anatomy"></a>Anatomia

A imagem abaixo mostra as partes do controle [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). O TabStrip tem um cabeçalho e um rodapé, mas ao contrário de um documento, o cabeçalho e o rodapé do TabStrip estão no lado mais à esquerda e mais à direita da faixa, respectivamente.

![Anatomia do controle TabView](images/tabview/tab-view-anatomy.png)

A imagem a seguir mostra as partes do controle [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem). Observe que, embora o conteúdo seja exibido dentro do controle TabView, o conteúdo normalmente faz parte do TabViewItem.

![Anatomia do controle TabViewItem](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>Criar um modo de exibição de guia

Este exemplo cria um [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) simples junto com manipuladores de evento para abrir e fechar guias.

```xaml
<TabView AddTabButtonClick="Tabs_AddTabButtonClick"
         TabCloseRequested="Tabs_TabCloseRequested" />
```

```csharp
// Add a new Tab to the TabView
private void Tabs_AddTabButtonClick(TabView sender, TabViewAddTabButtonClickEventArgs e)
{
    var newTab = new TabViewItem();
    newTab.IconSource = new SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(BaconIpsumPage));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void Tabs_TabCloseRequested(TabView sender, TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>Comportamento

Há várias maneiras de aproveitar ou estender a funcionalidade de um [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview).

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>Associar o TabItemsSource a um TabViewItemCollection

```csharp
<TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>Exibir guias TabView na barra de títulos de uma janela

Em vez de fazer as guias ocuparem sua própria linha embaixo do título de uma janela, é possível mesclá-las na mesma área. Isso economiza espaço vertical do seu conteúdo e confere ao seu aplicativo uma sensação moderna.

Como um usuário pode arrastar uma janela por sua barra de títulos para reposicionar a janela, é importante que a barra de títulos não seja completamente preenchida com guias. Portanto, ao exibir guias em uma barra de títulos, é necessário especificar uma parte da barra de títulos para ser reservada como uma área que pode ser arrastada. Se você não especificar uma região que pode ser arrastada, toda a barra de títulos poderá ser arrastada, o que impedirá que suas guias recebam eventos de entrada. Se o TabView for exibido na barra de títulos de uma janela, você sempre deverá incluir um [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) em seu [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) e marcá-lo como uma região que pode ser arrastada.

Para obter mais informações, confira [Personalização da barra de título](https://docs.microsoft.com/windows/uwp/design/shell/title-bar)

![Guias na barra de títulos](images/tabview/tab-extend-to-title.png)

```xaml
<Page>
    <TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
        <TabViewItem Icon="Home" Header="Home" IsClosable="False" />
        <TabViewItem Icon="Document" Header="Document 1" />
        <TabViewItem Icon="Document" Header="Document 2" />
        <TabViewItem Icon="Document" Header="Document 3" />

        <TabView.TabStripHeader>
            <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
        </TabView.TabStripHeader>
        <TabView.TabStripFooter>
            <Grid x:Name="CustomDragRegion" Background="Transparent" />
        </TabView.TabStripFooter>
    </TabView>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> Para verificar se as guias na barra de títulos não são obstruídas pelo conteúdo do shell, você deve contabilizar as sobreposições esquerdas e direitas. Em layouts LTR, a inserção direita inclui os botões de legenda e a região de arrastar. O inverso é verdadeiro em RTL. Os valores SystemOverlayLeftInset e SystemOverlayRightInset estão em termos de esquerda e direita físicas, então inverta-os também quando estiverem em RTL.

### <a name="control-overflow-behavior"></a>Controlar comportamento de estouro

Como a barra de guias fica cheia de guias, é possível controlar como elas são exibidas definindo [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode).

| Valor TabWidthMode | Comportamento                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Igual              | À medida que novas guias forem adicionadas, todas elas serão reduzidas horizontalmente até chegarem a uma largura mínima muito pequena.                                                       |
| SizeToContent      | As guias sempre terão seu "tamanho natural", o tamanho mínimo necessário para exibir seu ícone e cabeçalho. Elas não serão expandidas ou recolhidas conforme novas guias forem adicionadas ou fechadas. |

Independentemente do valor escolhido, eventualmente pode haver muitas guias a serem exibidas em sua faixa de guias. Nesse caso, serão exibidos amortecedores de rolagem que permitem que o usuário role a TabStrip para a esquerda e para a direita.

### <a name="guidance-for-tab-selection"></a>Diretrizes para a seleção de guia

A maioria dos usuários tem experiência com o uso de guias de documentos simplesmente usando um navegador da Web. Quando eles usam guias de documentos em seu aplicativo, a experiência deles informa suas expectativas para a maneira como as guias devem se comportar.

Não importa como o usuário interage com um conjunto de guias de documento, sempre deve haver uma guia ativa. Se o usuário fechar a guia selecionada ou quebrar a guia selecionada em outra janela, outra guia deverá se tornar a ativa. O [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) tenta fazer isso automaticamente selecionando a próxima guia. Se você tiver um bom motivo por que seu aplicativo deve permitir um TabView com uma guia não selecionada, a área de conteúdo do TabView estará simplesmente em branco.

## <a name="keyboard-navigation"></a>Navegação por teclado

O [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) é compatível com muitos cenários de navegação de teclado comuns por padrão. Esta seção explica a funcionalidade interna e fornece recomendações sobre funcionalidades adicionais que podem ser úteis para alguns aplicativos.

### <a name="tab-and-cursor-key-behavior"></a>Comportamento-chave da guia e do cursor

Quando o foco passa para a área _TabStrip_, o [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) selecionado ganha foco. O usuário pode usar as teclas de seta para a esquerda e para a direita para mover o foco (não seleção) para outras guias no TabStrip. O foco da seta é interceptado dentro da faixa de guias e do botão adicionar guia (+), se houver um. Para mover o foco para fora da área de TabStrip, o usuário pode pressionar a tecla Tab, que moverá o foco para o próximo elemento de foco.

Mover o foco por meio de Tab

![Mover o foco por meio de tab](images/tabview/tab-keyboard-behavior-1.png)

As teclas de seta não circulam o foco

![As teclas de seta não circulam o foco](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>Selecionar uma guia

Quando um TabViewItem tiver foco, pressionar Espaço ou Enter selecionará esse TabViewItem.

Use as teclas de seta para mover o foco e pressione Espaço para selecionar a guia.

![Espaço para selecionar a guia](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>Atalhos para selecionar guias adjacentes

Ctrl + Tab selecionará o próximo [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem). Ctrl + Shift + Tab selecionará o TabViewItem anterior. Para essas finalidades, a lista de guias está "em loop"; portanto, selecionar a próxima guia enquanto a última estiver selecionada fará a primeira guia ficar selecionada.

### <a name="closing-a-tab"></a>Fechar uma guia

Pressionar Ctrl + F4 gerará o evento [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested). Manipule o evento e feche a guia, se apropriado.

### <a name="keyboard-guidance-for-app-developers"></a>Diretrizes de teclado para Desenvolvedores de Aplicativo

Alguns aplicativos podem exigir um controle de teclado mais avançado. Considere implementar os seguintes atalhos se eles forem apropriados para seu aplicativo.

> [!WARNING]
> Se estiver adicionando um [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) a um aplicativo existente, você já poderá ter criado atalhos de teclado mapeados para as combinações de teclas dos atalhos de teclado TabView recomendados. Nesse caso, você precisará considerar se deseja manter seus atalhos existentes ou oferecer uma experiência de guia intuitiva para o usuário.

- CTRL + T deve abrir uma nova guia. Geralmente, essa guia é preenchida com um documento predefinido ou é criada vazia com uma maneira simples de escolher seu conteúdo. Se o usuário precisar escolher o conteúdo para uma nova guia, considere fornecer o foco de entrada para o controle de seleção de conteúdo.
- Ctrl + W deve fechar a guia selecionada. Lembre-se de que o TabView selecionará a próxima guia automaticamente.
- Ctrl + Shift + T deve abrir as guias recentemente fechadas (ou, mais precisamente, abrir novas guias com o mesmo conteúdo que as guias fechadas recentemente). Comece com a guia fechada mais recentemente e mova-se para trás no tempo para cada vez anterior em que o atalho foi invocado. Observe que isso exigirá a manutenção de uma lista de guias fechadas recentemente.
- Ctrl + 1 deve selecionar a primeira guia na lista de guias. Da mesma forma, Ctrl + 2 deve selecionar a segunda guia, Ctrl + 3 deve selecionar a terceira e assim por diante até Ctrl + 8.
- Ctrl + 9 deve selecionar a última guia da lista, independentemente de quantas guias estão nela.
- Se as guias oferecerem mais do que apenas o comando fechar (como duplicar ou fixar uma guia), use um menu de contexto para mostrar todas as ações disponíveis que podem ser executadas em uma guia.

### <a name="implement-browser-style-keyboarding-behavior"></a>Implementar comportamento de teclado do estilo do navegador

Esse exemplo implementa várias das recomendações acima em um [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). Especificamente, esse exemplo implementa Ctrl + T, Ctrl + W, Ctrl + 1-8 e Ctrl + 9.

```xaml
<controls:TabView x:Name="TabRoot">
    <controls:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </controls:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</controls:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // See previous sample
    CreateNewTab();
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only close the selected tab if it is closeable
    if (((TabViewItem)TabRoot.SelectedItem).IsCloseable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>Artigos relacionados

- [MasterDetails](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/master-details)
- [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)
- [Pivô](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot)

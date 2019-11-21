---
Description: Submenus da barra de comandos dão aos usuários acesso embutido às tarefas mais comuns do seu aplicativo.
title: Submenu da barra de comandos
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f4d2443370d285322e94c4ca21e7d616f96794b7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257732"
---
# <a name="command-bar-flyout"></a>Submenu da barra de comandos

O submenu da barra de comandos permite que você forneça aos usuários acesso fácil a tarefas comuns mostrando comandos em uma barra de ferramentas flutuante relacionada a um elemento em sua tela da interface do usuário.

![Um submenu da barra de comandos de texto expandido](images/command-bar-flyout-header.png)

> CommandBarFlyout exige o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **APIs da plataforma**: [classe CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [classe AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [classe AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **APIs da biblioteca de interface do usuário do Windows**: [classe CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Assim como [CommandBar](app-bars.md), CommandBarFlyout tem as propriedades **PrimaryCommands** e **SecondaryCommands** que você pode usar para adicionar comandos. É possível colocar comandos em uma das coleções ou em ambas. Quando e como os comandos principais e secundários são exibidos depende do modo de exibição.

O submenu da barra de comandos tem dois modos de exibição: *recolhido* e *expandido*.

- No modo recolhido, somente os comandos principais são mostrados. Se o submenu da barra de comandos tiver comandos principais e secundários, um botão "ver mais", representado por um sinal de reticências \[•••\], será exibido. Isso permite que o usuário tenha acesso aos comandos secundários fazendo a transição para o modo expandido.
- No modo expandido, os comandos principais e secundários são mostrados. (Se o controle tiver apenas itens secundários, eles serão exibidos de forma semelhante ao controle MenuFlyout).

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o controle CommandBarFlyout para mostrar uma coleção de comandos ao usuário, como botões e itens de menu, no contexto de um elemento na tela do aplicativo.

O TextCommandBarFlyout exibe os comandos de texto nos controles TextBox, TextBlock, RichEditBox, RichTextBlock e PasswordBox. Os comandos são configurados automaticamente de maneira adequada à seleção de texto atual. Use um CommandBarFlyout para substituir os comandos de texto padrão nos controles de texto.

Para mostrar comandos contextuais em itens de lista, siga as diretrizes em [Comandos contextuais para coleções e listas](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout versus MenuFlyout

Para mostrar comandos em um menu de contexto, você pode usar CommandBarFlyout ou MenuFlyout. Recomendamos o uso de CommandBarFlyout porque ele fornece mais funcionalidades do que MenuFlyout. É possível usar CommandBarFlyout apenas com comandos secundários para obter o comportamento e a aparência de um MenuFlyout ou usar o submenu da barra de comandos completo, com comandos principais e secundários.

> Para obter informações relacionadas, confira [Submenus](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus e menus de contexto](menus.md) e [Barras de comandos](app-bars.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">abri-lo e ver o CommandBarFlyout em funcionamento</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Invocação proativa versus reativa

Geralmente, há duas maneiras de invocar um submenu ou menu associado a um elemento em sua tela de interface do usuário: a _invocação proativa_ e a _invocação reativa_.

Na invocação proativa, os comandos aparecem automaticamente quando o usuário interage com o item a que estão associados. Por exemplo, comandos de formatação de texto podem surgir quando o usuário seleciona o texto em uma caixa de texto. Nesse caso, o submenu da barra de comandos não entra em foco. Em vez disso, ele apresenta os comandos relevantes próximos ao item com que o usuário está interagindo. Se o usuário não interagir com os comandos, eles serão ignorados.

Na invocação reativa, os comandos são mostrados em resposta a uma ação explícita do usuário para solicitá-los, por exemplo, clicando com o botão direito do mouse. Isso corresponde ao conceito tradicional de um [menu de contexto](menus.md).

Você pode usar CommandBarFlyout das duas formas ou até mesmo combinando-as.

## <a name="create-a-command-bar-flyout"></a>Criar um submenu da barra de comandos

Este exemplo mostra como criar um submenu da barra de comandos e usá-lo de forma proativa e reativa. Quando a imagem é tocada, o submenu é mostrado no modo recolhido. Quando mostrado como um menu de contexto, o submenu é mostrado no modo expandido. Em ambos os casos, o usuário pode expandir ou recolher o submenu após ele ser aberto.

![Exemplo de submenu de barra de comandos recolhido](images/command-bar-flyout-img-collapsed.png)

> _Um submenu de barra de comandos recolhido_

![Exemplo de submenu de barra de comandos expandido](images/command-bar-flyout-img-expanded.png)

> _Um submenu de barra de comandos expandido_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Mostrar comandos proativamente

Quando você mostra comandos contextuais proativamente, somente os comandos principais devem ser mostrados por padrão (o submenu da barra de comandos deve estar recolhido). Coloque os comandos mais importantes na coleção de comandos principais e os comandos adicionais, que tradicionalmente iriam em um menu de contexto, na coleção de comandos secundários.

Para mostrar os comandos proativamente, você normalmente manipulará os eventos [Clicar](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tocar](/uwp/api/windows.ui.xaml.uielement.tapped) para exibir o submenu da barra de comandos. Defina o submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) como **ShowMode** ou **TransientWithDismissOnPointerMoveAway** para abrir o submenu no modo recolhido sem remover o foco.

Começando no Windows 10 Insider Preview, controles de texto têm uma propriedade **SelectionFlyout**. Quando você atribui um submenu a essa propriedade, ele é mostrado automaticamente quando o texto é selecionado.

### <a name="show-commands-reactively"></a>Mostrar comandos reativamente

Quando você mostra comandos contextuais de reativamente, como um menu de contexto, os comandos secundários são mostrados por padrão (o submenu da barra de comandos deve ser expandido). Nesse caso, o submenu da barra de comandos pode ter comandos principais e secundários ou somente comandos secundários.

Para mostrar comandos em um menu de contexto, normalmente você atribui o submenu à propriedade [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) de um elemento de interface do usuário. Dessa forma, a abertura do submenu é manipulada pelo elemento e você não precisa fazer mais nada.

Se você manipular a exibição do submenu por conta própria (por exemplo, em um evento [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)), defina o submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) como **Padrão** para abrir o submenu no modo expandido e colocá-lo em foco.

> [!TIP]
> Para obter mais informações sobre as opções de exibição de um submenu e como controlar seu posicionamento, confira [Submenus](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos e conteúdo

O controle CommandBarFlyout tem duas propriedades que você pode usar para adicionar comandos e conteúdo: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) e [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Por padrão, os itens da barra de comandos são adicionados à coleção **PrimaryCommands**. Esses comandos são mostrados na barra de comandos e são visíveis nos modos recolhido e expandido. Diferente de CommandBar, comandos principais não estouram automaticamente para os comandos secundários e podem ser truncados.

Também é possível adicionar comandos à coleção **SecondaryCommands**. Comandos secundários são mostrados na parte do menu do controle e ficam visíveis apenas no modo expandido.

### <a name="app-bar-buttons"></a>Botões da barra de aplicativos

É possível popular PrimaryCommands e SecondaryCommands diretamente nos controles [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) e [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator).

Os controles de botão da barra de aplicativos são caracterizados por um ícone e um rótulo de texto. Esses controles são otimizados para uso em uma barra de comandos e sua aparência muda dependendo dele ser mostrado na barra de comandos ou no menu de estouro.

- Botões da barra de aplicativos usados como comandos principais são mostrados na barra de comandos apenas com seu ícone; o rótulo de texto não é mostrado. Recomendamos usar uma dica de ferramenta para mostrar uma descrição de texto do comando, conforme mostrado aqui.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Botões da barra de aplicativos usados como comandos secundários são mostrados no menu, com o rótulo e o ícone visíveis.

### <a name="other-content"></a>Outros tipos de conteúdo

Você pode adicionar outros controles a um submenu da barra de comandos encapsulando-os em um AppBarElementContainer. Isso permite adicionar controles, como [DropDownButton](buttons.md) ou [SplitButton](buttons.md), ou contêineres, como [StackPanel](buttons.md), para criar uma interface do usuário mais complexa.

Para ser adicionado às coleções de comandos principais ou secundários de um submenu da barra de comandos, um elemento precisa implementar a interface [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement). AppBarElementContainer é um wrapper que implementa essa interface, de modo que você pode adicionar um elemento a uma barra de comandos mesmo que ele não implemente a interface em si.

Aqui, um AppBarElementContainer é usado para adicionar elementos extra a um submenu da barra de comandos. Um SplitButton é adicionado aos comandos principais para permitir a seleção de cores. Um StackPanel é adicionado aos comandos secundários para possibilitar um layout mais complexo para os controles de zoom.

> [!TIP]
> Por padrão, elementos projetados para a tela do aplicativo podem não parecer corretos em uma barra de comandos. Quando você adiciona um elemento usando AppBarElementContainer, é necessário executar algumas etapas para fazer com que o elemento corresponda a outros elementos da barra de comandos:
>
> - Substitua os pincéis padrão pelo [estilo leve](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) para fazer a tela de fundo e a borda do elemento combinarem com os botões da barra de aplicativos.
> - Ajuste o tamanho e a posição do elemento.
> - Encapsule ícones em uma Viewbox com largura e altura de 16px.

> [!NOTE]
> Este exemplo mostra apenas a interface do usuário do submenu da barra de comandos, ele não implementa nenhum dos comandos mostrados. Para obter mais informações sobre como implementar os comandos, confira [Botões](buttons.md) e [Noções básicas de design de comandos](../basics/commanding-basics.md).

![Um submenu da barra de comandos com um botão de divisão](images/command-bar-flyout-split-button.png)

> _Um submenu da barra de comandos recolhido com um SplitButton aberto_

![Um submenu da barra de comandos com interface do usuário complexa](images/command-bar-flyout-custom-ui.png)

> _Um submenu da barra de comandos expandido com interface do usuário de zoom personalizada no menu_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Criar um menu de contexto apenas com comandos secundários

Você pode usar um CommandBarFlyout tendo apenas comandos secundários como [menu de contexto](menus.md), em vez de um MenuFlyout.

![Um submenu da barra de comandos apenas com comandos secundários](images/command-bar-flyout-context-menu.png)

> _Submenu da barra de comandos como menu de contexto_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Você também pode usar um CommandBarFlyout com um DropDownButton para criar um menu padrão.

![Um submenu da barra de comandos com como um botão de menu suspenso](images/command-bar-flyout-dropdown.png)

> _Um menu de botão de lista suspensa em um submenu da barra de comandos_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Submenus de barra de comandos para controles de texto

O [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) é um submenu da barra de comandos especializado que contém comandos de edição de texto. Cada controle de texto mostra o TextCommandBarFlyout automaticamente como um menu de contexto (clique com o botão direito do mouse) ou quando texto é selecionado. O submenu da barra de comandos de texto se adapta à seleção de texto para mostrar apenas comandos relevantes.

![Um submenu de barra de comandos de texto recolhido](images/command-bar-flyout-text-selection.png)

> _Um submenu de barra de comandos de texto em seleção de texto_

![Um submenu da barra de comandos de texto expandido](images/command-bar-flyout-text-full.png)

> _Um submenu da barra de comandos de texto expandido_


### <a name="available-commands"></a>Comandos disponíveis

Esta tabela mostra os comandos incluídos em um TextCommandBarFlyout e quando eles são mostrados.

| Comando | Mostrado... |
| ------- | -------- |
| Bold | quando o controle de texto não é somente leitura (RichEditBox somente). |
| Italic | quando o controle de texto não é somente leitura (RichEditBox somente). |
| Underline | quando o controle de texto não é somente leitura (RichEditBox somente). |
| Revisão de texto | quando IsSpellCheckEnabled é **verdadeiro** e texto digitado incorretamente é selecionado. |
| Recortar | quando o controle de texto não é somente leitura e texto é selecionado. |
| Copiar | quando texto é selecionado. |
| Colar | quando o controle de texto não é somente leitura e a área de transferência tem conteúdo. |
| Desfazer | quando há uma ação que pode ser desfeita. |
| Selecionar tudo | quando texto pode ser selecionado. |

### <a name="custom-text-command-bar-flyouts"></a>Submenus de barra de comandos personalizados

TextCommandBarFlyout não pode ser personalizado e é gerenciado automaticamente por cada controle de texto. No entanto, você pode substituir o TextCommandBarFlyout padrão por comandos personalizados.

- Para substituir o TextCommandBarFlyout padrão mostrado na seleção de texto, você pode criar um CommandBarFlyout personalizado (ou outro tipo de submenu) e atribuí-lo à propriedade **SelectionFlyout**. Se você definir SelectionFlyout como **nulo**, nenhum comando será mostrado na seleção.
- Para substituir o TextCommandBarFlyout padrão mostrado como menu de contexto, atribua um CommandBarFlyout personalizado (ou outro tipo de submenu) à propriedade **ContextFlyout** em um controle de texto. Se você definir ContextFlyout como **nulo**, o submenu do menu mostrado nas versões anteriores do controle de texto será mostrado em vez do TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.
- [Exemplo de execução de comandos XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de design de comandos para aplicativos UWP](../basics/commanding-basics.md)
- [Classe CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)

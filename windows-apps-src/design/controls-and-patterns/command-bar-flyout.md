---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: Submenu da barra de comandos
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 10/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 22d965d14c4f10f904a4d94a18ce83721c49491c
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4950517"
---
# <a name="command-bar-flyout"></a>Submenu da barra de comandos

O submenu de barra de comando permite que você fornecer aos usuários acesso fácil às tarefas comuns, mostrando comandos em uma barra de ferramentas flutuante relacionada a um elemento na sua tela de interface do usuário.

![Um submenu de barra de comandos de texto expandida](images/command-bar-flyout-text-full.png)

> Para obter informações relacionadas, consulte [submenus](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus e menus de contexto](menus.md)e [barras de comandos](app-bars.md).

Como o [CommandBar](app-bars.md), CommandBarFlyout tem propriedades **PrimaryCommands** e **SecondaryCommands** , que você pode usar para adicionar comandos. Você pode colocar comandos na coleção ou ambos. Quando e como os comandos principais e secundários são exibidos dependem do modo de exibição.

O submenu de barra de comando tem dois modos de exibição: *expandida*e *recolhida* .

- O modo recolhido, somente os comandos principais são mostrados. Se tiver o submenu de barra de comandos principais e secundárias comandos, um botão "Veja mais", que é representado por um sinal de reticências \ [• • • \], é exibida. Isso permite que o usuário obtenha acesso aos comandos secundários fazendo a transição para o modo expandido.
- No modo expandido, os comandos principais e secundários são mostrados. (Se o controle tiver apenas itens secundários, eles serão mostrados de forma semelhante ao controle MenuFlyout).

| **Baixar a biblioteca de interface do usuário do Windows** |
| - |
| Esse controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte a [Visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **APIs da plataforma** | **APIs de biblioteca de interface do usuário do Windows** |
| - | - |
| [Classe CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [classe AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [classe AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator) | [Classe CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) |

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use o controle CommandBarFlyout para mostrar um conjunto de comandos para o usuário, como botões e itens de menu no contexto de um elemento na tela do aplicativo.

O TextCommandBarFlyout exibe comandos de texto em controles de caixa de texto, TextBlock, RichTextBlock e RichEditBox, PasswordBox. Os comandos são automaticamente configurados adequadamente para a seleção de texto atual. Use um CommandBarFlyout para substituir os comandos de texto padrão em controles de texto.

Para mostrar contextuais comandos em itens de lista sigam as diretrizes [contextuais comandos para coleções e listas](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

Para mostrar comandos em um menu de contexto, você pode usar CommandBarFlyout ou MenuFlyout. É recomendável CommandBarFlyout porque ele fornece mais funcionalidades que MenuFlyout. Você pode usar CommandBarFlyout com apenas os comandos secundários para obter o comportamento e aparência de um MenuFlyout ou usar o submenu de barra de comando completo com comandos primários e secundários.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">Abrir o aplicativo e ver o CommandBarFlyout em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proativa versus reativa invocação

Normalmente, existem duas maneiras invocar um submenu ou um menu que está associado a um elemento na sua tela de interface do usuário: _invocação proativa_ e _reativa invocação_.

Em invocação proativa comandos são exibidos automaticamente quando o usuário interage com o item que os comandos estão associados. Por exemplo, comandos de formatação do texto podem pop-up quando o usuário seleciona o texto em uma caixa de texto. Nesse caso, o submenu de barra de comando não tem foco. Em vez disso, ele apresenta os comandos relevantes perto do item que o usuário está interagindo com. Se o usuário não interagir com os comandos, eles são ignorados.

Reativa invocação comandos são mostrados em resposta a uma ação explícita do usuário para solicitar os comandos; Por exemplo, um botão direito do mouse. Isso corresponde ao conceito tradicional de um [menu de contexto](menus.md).

Você pode usar o CommandBarFlyout de forma ou até mesmo uma mistura dos dois.

## <a name="create-a-command-bar-flyout"></a>Criar um submenu de barra de comando

> **Visualização**: CommandBarFlyout requer a [compilação do Windows 10 Insider Preview e o SDK mais recente](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Este exemplo mostra como criar um submenu de barra de comando e usá-la de forma proativa e reativa. Quando a imagem é tocada, o submenu é mostrado no modo recolhido. Quando mostrada como um menu de contexto, o submenu é mostrado no modo expandido. Em ambos os casos, o usuário pode expandir ou recolher o submenu depois que ele for aberto.

:::row:::
    :::column:::
        A collapsed command bar flyout<br/>
        ![Example of a collapsed command bar flyout](images/command-bar-flyout-img-collapsed.png)
    :::column-end:::
    :::column:::
        An expanded command bar flyout<br/>
        ![Example of an expanded command bar flyout](images/command-bar-flyout-img-expanded.png)
    :::column-end:::
:::row-end:::

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Rotate" Icon="Rotate"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
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

### <a name="show-commands-proactively"></a>Mostrar comandos de forma proativa

Quando você mostra os comandos contextuais proativamente, somente os comandos principais devem ser mostrados por padrão (o submenu de barra de comando deve ser recolhido). Coloque os comandos mais importantes no conjunto de comandos principais e comandos adicionais que tradicionalmente fica em um menu de contexto para a coleção de comandos secundários.

Para mostrar os comandos proativamente, você normalmente manipule o evento de [clique](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) para mostrar o submenu de barra de comando. Defina do submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **transitório** ou **TransientWithDismissOnPointerMoveAway** para abrir o submenu no modo recolhido sem colocar o foco.

A partir do Windows 10 Insider Preview, controles de texto têm uma propriedade **SelectionFlyout** . Quando você atribuir um submenu para essa propriedade, ela será mostrada automaticamente quando o texto é selecionado.

### <a name="show-commands-reactively"></a>Mostrar comandos reativamente

Quando você mostra os comandos contextuais reativamente, como um menu de contexto, os comandos secundários são mostrados por padrão (o submenu de barra de comando deve ser expandido). Nesse caso, o submenu de barra de comando pode ter comandos principais e secundários, ou apenas comandos secundários.

Para mostrar comandos em um menu de contexto, você normalmente atribui o submenu à propriedade [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) de um elemento de interface do usuário. Dessa forma, abrir o submenu é manipulada pelo elemento, e você não precisa fazer nada mais.

Se você manipular mostrando o submenu (por exemplo, em um evento [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) ), defina do submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) no **padrão** para abrir o submenu no modo expandido e dar a ele foco.

> [!TIP]
> Para obter mais informações sobre as opções ao mostrar um submenu e como controlar o posicionamento do submenu, consulte [submenus](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos e conteúdo

O controle CommandBarFlyout tem 2 propriedades que você pode usar para adicionar comandos e conteúdo: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) e [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Por padrão, os itens da barra de comandos são adicionados à coleção **PrimaryCommands**. Esses comandos são mostrados na barra de comandos e ficam visíveis em ambos os modos expandidos e recolhidos. Ao contrário de CommandBar, comandos principais não fará automaticamente estouro aos comandos secundários e podem ser truncados.

Você também pode adicionar comandos à coleção **SecondaryCommands** . Comandos secundários são mostrados na parte do menu do controle e ficam visíveis somente no modo expandido.

### <a name="app-bar-buttons"></a>Botões da barra de aplicativos

Você pode preencher PrimaryCommands e SecondaryCommands diretamente com controles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)e [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) .

Os controles de botão da barra de aplicativos são caracterizados por um ícone e um rótulo de texto. Esses controles são otimizados para uso em uma barra de comandos, e sua aparência muda se o controle é mostrado na barra de comandos ou no menu de estouro.

- Botões da barra de aplicativos usados como comandos principais são mostradas na barra de comandos com apenas o ícone; o rótulo de texto não é mostrado. Recomendamos que você use uma dica de ferramenta para mostrar uma descrição de texto do comando, conforme mostrado aqui.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Botões da barra de aplicativos usados como comandos secundários são mostrados no menu, com o rótulo e o ícone visível.

### <a name="other-content"></a>Outros tipos de conteúdo

Você pode adicionar outros controles a um submenu de barra de comando encapsulá-los em um AppBarElementContainer. Isso permite que você adicionar controles, como [DropDownButton]() ou [SplitButton]()ou adicionar contêineres como [StackPanel]() para criar a interface do usuário mais complexo.

> [!NOTE]
> Para ser adicionado às coleções de comando principal ou secundária de um submenu de barra de comando, um elemento deve implementar a interface [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) . AppBarElementContainer é um wrapper que implementa essa interface para que você pode adicionar um elemento para uma barra de comandos, mesmo se ele não implementa a interface em si.

Aqui, um AppBarElementContainer é usado para adicionar elementos extras para um submenu de barra de comando. Um SplitButton é adicionado aos comandos principais para permitir a seleção de cores. Um StackPanel é adicionado aos comandos secundários para permitir que um layout mais complexo para controles de zoom.

> [!NOTE]
> Este exemplo mostra apenas o comando submenu da barra de interface do usuário, que ele não implementa qualquer um dos comandos que são mostrados. Para obter mais informações sobre como implementar os comandos, consulte [Noções básicas de design de comando](../basics/commanding-basics.md)e [botões](buttons.md) .

:::row:::
    :::column:::
        A collapsed command bar flyout with an open SplitButton<br/>
        ![A command bar flyout with a split button](images/command-bar-flyout-split-button.png)
    :::column-end:::
    :::column:::
        An expanded command bar flyout with custom zoom UI in the menu<br/>
        ![A command bar flyout with complex UI](images/command-bar-flyout-complex-ui.png)
    :::column-end:::
:::row-end:::

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Color controls -->
    <AppBarElementContainer>
        <SplitButton Height="Auto" Margin="0,4,0,0"
                     ToolTipService.ToolTip="Colors"
                     Background="{ThemeResource AppBarItemBackgroundThemeBrush}">
            <SplitButton.Content>
                <Rectangle Width="20" Height="20">
                    <Rectangle.Fill>
                        <SolidColorBrush Color="Red"/>
                    </Rectangle.Fill>
                </Rectangle>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Text="Red"/>
                    <MenuFlyoutItem Text="Yellow"/>
                    <MenuFlyoutItem Text="Green"/>
                    <MenuFlyoutItem Text="Blue"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Color controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <Style TargetType="Button">
                    <Setter Property="Background"
                            Value="{ThemeResource AppBarItemBackgroundThemeBrush}"/>
                </Style>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="86"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Zoom"/>
                <StackPanel Orientation="Horizontal" Grid.Column="1">
                    <Button>
                        <SymbolIcon Symbol="Remove"/>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button>
                        <SymbolIcon Symbol="Add"/>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Criar um menu de contexto com apenas comandos secundários

Você pode usar um CommandBarFlyout com apenas os comandos secundários como um [menu de contexto](menus.md), no lugar de um MenuFlyout.

![Um submenu de barra de comando com apenas os comandos secundários](images/command-bar-flyout-context-menu.png)

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Pin" Icon="Pin"/>
                <AppBarButton Label="Unpin" Icon="UnPin"/>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Você também pode usar um CommandBarFlyout com um DropDownButton para criar um menu padrão.

![Um submenu de barra de comando com como um botão de menu suspenso](images/command-bar-flyout-button-menu.png)

```xaml
<DropDownButton Content="Mail">
    <DropDownButton.Flyout>
        <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Icon="MailForward" Label="Forward"/>
                <AppBarButton Icon="MailReply" Label="Reply"/>
                <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Submenus da barra de comandos para controles de texto

O [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) é um submenu de barra de comandos especializados que contém comandos de edição de texto. Cada controle de texto mostra o TextCommandBarFlyout automaticamente como um menu de contexto (botão direito do mouse), ou quando o texto é selecionado. O submenu de barra de comandos de texto se adapte a seleção de texto para mostrar apenas os comandos relevantes.

:::row:::
    :::column:::
        A text command bar flyout on text selection<br/>
        ![A collapsed text command bar flyout](images/command-bar-flyout-text-selection.png)
    :::column-end:::
    :::column:::
        An expanded text command bar flyout<br/>
        ![An expanded text command bar flyout](images/command-bar-flyout-text-full.png)
    :::column-end:::
:::row-end:::

### <a name="available-commands"></a>Comandos disponíveis

Esta tabela mostra os comandos que estão incluídos em um TextCommandBarFlyout e quando eles são mostrados.

| Comando | Mostrado... |
| ------- | -------- |
| Negrito | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Itálico | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Sublinhado | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Revisão de texto | Quando IsSpellCheckEnabled é **verdadeiro** e incorreto texto estiver selecionado. |
| Cortar | Quando o controle de texto não é somente leitura e texto estiver selecionado. |
| Copiar | Quando o texto estiver selecionado. |
| Colar | Quando o controle de texto não é somente leitura e a área de transferência tem conteúdo. |
| Desfazer | Quando há uma ação que pode ser desfeita. |
| Selecionar tudo | Quando o texto pode ser selecionado. |

### <a name="custom-text-command-bar-flyouts"></a>Submenus de barra de comandos de texto personalizado

TextCommandBarFlyout não pode ser personalizada e é gerenciado automaticamente por cada controle de texto. No entanto, você pode substituir o padrão TextCommandBarFlyout com comandos personalizados.

- Para substituir o padrão TextCommandBarFlyout que é mostrada na seleção de texto, você pode criar um CommandBarFlyout personalizado (ou outro tipo de submenu) e atribuí-lo à propriedade **SelectionFlyout** . Se você definir SelectionFlyout como **nula**, nenhuma comandos são mostrados na seleção.
- Para substituir o padrão TextCommandBarFlyout que é mostrado como o menu de contexto, atribua um CommandBarFlyout personalizado (ou outro tipo de submenu) para a propriedade **ContextFlyout** em um controle de texto. Se você definir ContextFlyout como **Nulo**, o submenu de menu mostrado nas versões anteriores do controle de texto é mostrado em vez do TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo de XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.
- [Exemplo de execução de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de design de comandos para aplicativos UWP](../basics/commanding-basics.md)
- [Classe CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)

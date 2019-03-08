---
Description: Submenus de barra de comando dar aos usuários acesso embutido para tarefas mais comuns do seu aplicativo.
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
ms.openlocfilehash: cb87bea001492e39a0f60b96f884db70b5bd28ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592521"
---
# <a name="command-bar-flyout"></a>Submenu da barra de comandos

Submenu da barra de comando permite que você fornecer aos usuários acesso fácil a tarefas comuns de comandos, mostrando uma barra de ferramentas flutuante relacionada a um elemento em sua tela de interface do usuário.

![Um submenu de barra de comando de texto expandido](images/command-bar-flyout-header.png)

> CommandBarFlyout requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **APIs de plataforma**: [Classe CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [classe AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [ Classe AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **APIs da biblioteca de interface do usuário do Windows**: [Classe CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout classe](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Como o [CommandBar](app-bars.md), tem CommandBarFlyout **PrimaryCommands** e **SecondaryCommands** propriedades que você pode usar para adicionar comandos. Você pode colocar comandos na coleção, ou ambos. Quando e como os comandos primários e secundários são exibidos dependem do modo de exibição.

Submenu da barra de comando tem dois modos de exibição: *recolhido* e *expandido*.

- No modo recolhido, somente os comandos principais são mostrados. Se o submenu da barra de comando tem primários e secundários de comandos, um botão "Veja mais", que é representado por um sinal de reticências \[• • •\], é exibida. Isso permite que o usuário obtenha acesso aos comandos secundários fazendo a transição para o modo expandido.
- No modo expandido, os comandos primários e secundários são mostrados. (Se o controle tem apenas os itens secundários, eles são exibidos de forma semelhante ao controle MenuFlyout).

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o controle CommandBarFlyout para mostrar uma coleção de comandos para o usuário, como botões e itens de menu, no contexto de um elemento na tela do aplicativo.

O TextCommandBarFlyout exibe os comandos de texto nos controles TextBox, TextBlock, RichEditBox, RichTextBlock e PasswordBox. Os comandos são automaticamente configurados adequadamente para a seleção de texto atual. Use um CommandBarFlyout para substituir os comandos de texto padrão nos controles de texto.

Mostrar contextuais comandos em itens de lista seguem as diretrizes [comandos contextuais para listas e coleções](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>Vs CommandBarFlyout MenuFlyout

Para mostrar os comandos em um menu de contexto, você pode usar CommandBarFlyout ou MenuFlyout. É recomendável CommandBarFlyout porque fornece mais funcionalidade do que MenuFlyout. Você pode usar CommandBarFlyout com apenas os comandos secundários para obter o comportamento e aparência de um MenuFlyout ou usar o submenu da barra de comando completo com comandos primários e secundários.

> Para obter informações relacionadas, consulte [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus e menus de contexto](menus.md), e [barras de comandos](app-bars.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">abrir o aplicativo e ver o CommandBarFlyout em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo da Galeria de controles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proativa versus invocação reativa

Geralmente há duas maneiras para invocar um submenu ou menu associado a um elemento na tela de interface do usuário: _invocação proativa_ e _invocação reativa_.

Na invocação proativa, comandos aparecem automaticamente quando o usuário interage com o item que os comandos estão associados. Por exemplo, comandos de formatação de texto podem pop-up quando o usuário seleciona o texto em uma caixa de texto. Nesse caso, o submenu da barra de comando não tem foco. Em vez disso, ele apresenta os comandos relevantes perto do item que o usuário está interagindo com. Se o usuário não interage com os comandos, eles são ignorados.

Na chamada reativa, os comandos são mostrados em resposta a uma ação explícita do usuário para solicitar os comandos; Por exemplo, o botão direito do mouse. Isso corresponde ao conceito tradicional de um [menu de contexto](menus.md).

Você pode usar o CommandBarFlyout de forma ou até mesmo uma combinação dos dois.

## <a name="create-a-command-bar-flyout"></a>Criar um submenu da barra de comando

Este exemplo mostra como criar um submenu da barra de comando e usá-lo de forma proativa e reativa. Quando a imagem é tocada, o submenu é mostrado em seu modo recolhido. Quando mostrada como um menu de contexto, o submenu é mostrado em seu modo expandido. Em ambos os casos, o usuário pode expandir ou recolher o submenu depois que ele é aberto.

![Exemplo de um submenu da barra de comando recolhido](images/command-bar-flyout-img-collapsed.png)

> _Um submenu da barra de comando recolhido_

![Exemplo de um submenu da barra de comando expandido](images/command-bar-flyout-img-expanded.png)

> _Um submenu da barra de comando expandido_

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

### <a name="show-commands-proactively"></a>Mostrar comandos de forma proativa

Quando você Mostrar comandos contextuais proativamente, somente os comandos primários devem ser mostrados por padrão (o submenu da barra de comando deve ser recolhido). Coloque os comandos mais importantes na coleção de comandos principal e comandos adicionais que tradicionalmente deveria ir em um menu de contexto para a coleção de comandos secundário.

Para mostrar proativamente os comandos, você normalmente manipulará as [clique em](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) evento para exibir o submenu da barra de comando. Defina o submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) à **transitórios** ou **TransientWithDismissOnPointerMoveAway** para abrir o menu suspenso em seu modo recolhido sem colocar o foco.

A partir do Windows 10 Insider Preview, controles de texto têm uma **SelectionFlyout** propriedade. Quando você atribui um submenu para essa propriedade, ela é mostrada automaticamente quando o texto é selecionado.

### <a name="show-commands-reactively"></a>Mostrar comandos de forma reativa

Quando você Mostrar comandos contextuais de forma reativa, como um menu de contexto, os comandos secundários são mostrados por padrão (o submenu da barra de comando deve ser expandido). Nesse caso, o submenu da barra de comando pode ter comandos primários e secundários, ou apenas comandos secundários.

Para mostrar os comandos em um menu de contexto, você normalmente atribui o submenu para o [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) propriedade de um elemento de interface do usuário. Dessa forma, abrir o submenu é tratada pelo elemento, e você não precisa fazer mais nada.

Se você manipular mostrando o menu suspenso por conta própria (por exemplo, em um [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) evento), defina o submenu [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) para **padrão** para abrir o menu suspenso em seu modo expandido e Dê a ele foco.

> [!TIP]
> Para obter mais informações sobre as opções durante a exibição de um submenu e como controlar o posicionamento do submenu, consulte [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos e conteúdo

O controle de CommandBarFlyout tem 2 propriedades, que você pode usar para adicionar comandos e conteúdo: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) e [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Por padrão, os itens da barra de comandos são adicionados à coleção **PrimaryCommands**. Esses comandos são mostrados na barra de comandos e são visíveis em ambos os modos recolhidos e expandidos. Ao contrário de CommandBar, comandos primários não automaticamente estouro aos comandos secundários e poderão ser truncados.

Você também pode adicionar comandos para o **SecondaryCommands** coleção. Comandos secundários são mostrados na parte do menu do controle e são visíveis apenas no modo expandido.

### <a name="app-bar-buttons"></a>Botões da barra de aplicativos

Você pode preencher o PrimaryCommands e SecondaryCommands diretamente com [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), e [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) controles.

Os controles de botão da barra de aplicativos são caracterizados por um ícone e um rótulo de texto. Esses controles são otimizados para uso em uma barra de comandos e sua aparência é alterado dependendo se o controle é mostrado na barra de comando ou o menu de estouro.

- Botões da barra de aplicativo usados como os comandos principais são mostrados na barra de comandos com apenas seu ícone; o rótulo de texto não é mostrado. É recomendável que você use uma dica de ferramenta para mostrar uma descrição de texto do comando, conforme mostrado aqui.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Botões da barra de aplicativo usados como comandos secundários são mostrados no menu, com o rótulo e o ícone visível.

### <a name="other-content"></a>Outros tipos de conteúdo

Você pode adicionar outros controles para um submenu da barra de comando, encapsulando-as em um AppBarElementContainer. Isso permite que você adicione controles, como [DropDownButton](buttons.md) ou [SplitButton](buttons.md), ou adicionar como contêineres [StackPanel](buttons.md) para criar a interface do usuário mais complexa.

Para ser adicionada às coleções de comando primária ou secundária de um submenu da barra de comando, um elemento deve implementar o [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) interface. AppBarElementContainer é um wrapper que implementa essa interface, assim você pode adicionar um elemento de uma barra de comandos, mesmo se ele não implementa a interface em si.

Aqui, um AppBarElementContainer é usado para adicionar elementos adicionais a um submenu da barra de comando. Um botão de divisão é adicionado aos comandos primários para permitir a seleção de cores. Um StackPanel é adicionado aos comandos secundários para permitir que um layout mais complexo para controles de zoom.

> [!TIP]
> Por padrão, elementos projetados para a tela do aplicativo podem não parecer corretos em uma barra de comandos. Quando você adiciona um elemento usando AppBarElementContainer, há algumas etapas que você deve executar para fazer com que o elemento corresponder a outros elementos da barra de comando:
>
> - Substituir os pincéis de padrão com [estilo leve](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) para tornar o plano de fundo e a borda corresponder os botões da barra de aplicativo do elemento.
> - Ajuste o tamanho e a posição do elemento.
> - Encapsule ícones em uma caixa de exibição com uma largura e altura de 16px.

> [!NOTE]
> Este exemplo mostra apenas o comando barra submenu da interface do usuário, que ele não implementa qualquer um dos comandos que são mostrados. Para obter mais informações sobre como implementar os comandos, consulte [botões](buttons.md) e [Noções básicas sobre o design de comando](../basics/commanding-basics.md).

![Um submenu da barra de comando com um botão de divisão](images/command-bar-flyout-split-button.png)

> _Um submenu da barra de comando recolhida com um botão de divisão aberta_

![Um submenu da barra de comando com a interface do usuário complexa](images/command-bar-flyout-custom-ui.png)

> _Um submenu da barra de comando expandido com zoom personalizado da interface do usuário no menu_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Criar um menu de contexto com comandos secundários apenas

Você pode usar um CommandBarFlyout com apenas comandos secundários como um [menu de contexto](menus.md), no lugar de um MenuFlyout.

![Um submenu da barra de comando com apenas os comandos secundários](images/command-bar-flyout-context-menu.png)

> _Submenu da barra de comando como um menu de contexto_

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

![Um submenu da barra de comando com como um botão de menu suspenso](images/command-bar-flyout-dropdown.png)

> _Uma lista suspensa menu de botão em um submenu da barra de comando_

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

## <a name="command-bar-flyouts-for-text-controls"></a>Submenus de barra de comando para controles de texto

O [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) é um submenu da barra de comandos especializados que contém os comandos de edição de texto. Cada controle de texto mostra o TextCommandBarFlyout automaticamente como um menu de contexto (atalho), ou quando o texto é selecionado. Submenu da barra de comando de texto se adapta à seleção de texto para mostrar apenas os comandos relevantes.

![Um submenu de barra de comando de texto recolhido](images/command-bar-flyout-text-selection.png)

> _Um submenu de barra de comando de texto na seleção de texto_

![Um submenu de barra de comando de texto expandido](images/command-bar-flyout-text-full.png)

> _Um submenu de barra de comando de texto expandido_


### <a name="available-commands"></a>Comandos disponíveis

Esta tabela mostra os comandos que estão incluídos em um TextCommandBarFlyout e quando elas são mostradas.

| Comando | Mostrado... |
| ------- | -------- |
| Bold | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Italic | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Underline | Quando o controle de texto não é somente leitura (RichEditBox somente). |
| Revisão de texto | Quando for IsSpellCheckEnabled **verdadeira** e texto digitado incorretamente é selecionado. |
| Recortar | Quando o controle de texto não é somente leitura e o texto estiver selecionado. |
| Copiar | Quando o texto é selecionado. |
| Colar | Quando o controle de texto não é somente leitura e a área de transferência tem conteúdo. |
| Desfazer | Quando há uma ação que pode ser desfeita. |
| Selecionar tudo | Quando o texto pode ser selecionado. |

### <a name="custom-text-command-bar-flyouts"></a>Submenus de barra de comando de texto personalizado

TextCommandBarFlyout não pode ser personalizado e é gerenciada automaticamente por cada controle de texto. No entanto, você pode substituir o padrão TextCommandBarFlyout com comandos personalizados.

- Para substituir o padrão TextCommandBarFlyout é mostrada na seleção de texto, você pode criar um CommandBarFlyout personalizado (ou outro tipo de submenu) e atribuí-lo para o **SelectionFlyout** propriedade. Se você definir SelectionFlyout como **nulo**, nenhum comando é mostrado na seleção.
- Para substituir o padrão TextCommandBarFlyout é mostrado como o menu de contexto, atribua um CommandBarFlyout personalizado (ou outro tipo de submenu) para o **ContextFlyout** propriedade em um controle de texto. Se você definir ContextFlyout como **nulo**, o submenu do menu mostrado nas versões anteriores do texto de controle é mostrado em vez do TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.
- [Exemplo de XAML dos comandos](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de design de comandos para aplicativos UWP](../basics/commanding-basics.md)
- [Classe de CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)

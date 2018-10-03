---
author: QuinnRadich
Description: Command bars give users easy access to your app's most common tasks.
title: Barra de comandos
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce64e6002bd71bd0806fb5574dc404ac4df856a9
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4265037"
---
# <a name="command-bar"></a>Barra de comandos

As barras de comandos fornecem aos usuários acesso fácil às tarefas mais comuns do seu aplicativo. As barras de comandos podem fornecer acesso aos comandos no nível do aplicativo ou específicos da página e podem ser usadas com qualquer padrão de navegação.

> **APIs importantes**: [classe CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), [classe AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [classe AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx), [classe AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![Exemplo de uma barra de comandos com ícones](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

O controle CommandBar é um controle de finalidade geral, flexível e leve que pode exibir conteúdo complexo, como imagens ou blocos de texto, e comandos simples, como os controles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) e [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx).

> [!NOTE]
XAML fornece o controle [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) e o controle [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar). Use AppBar somente quando você estiver atualizando um aplicativo Universal do Windows 8 que usa AppBar e precisar minimizar as alterações. Para novos aplicativos no Windows 10, recomendamos usar o controle CommandBar. Este documento pressupõe que você está usando o controle CommandBar.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CommandBar">abrir o aplicativo e ver o CommandBar em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Baixe o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma barra de comandos expandida no aplicativo Fotos Microsoft.

![Barra de comandos no aplicativo Fotos da Microsoft](images/control-examples/command-bar-photos.png)

Uma barra de comandos no calendário do Outlook no Windows Phone.

![Barra de comandos no aplicativo de Calendário do Outlook](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>Anatomia

Por padrão, a barra de comandos mostra uma linha de botões de ícones e um botão "veja mais" opcional, que é representado por um sinal de reticências \[•••\]. Esta é a barra de comandos criada pelo código de exemplo mostrado mais adiante. Ela é mostrada em seu estado fechado e compacto.

![Uma barra de comandos fechada](images/command-bar-compact.png)

A barra de comandos também pode ser mostrada em um estado fechado e minimizado com a seguinte aparência. Consulte a seção [Estados abertos e fechados](#open-and-closed-states) para obter mais informações.

![Uma barra de comandos fechada](images/command-bar-minimal.png)

Esta é a mesma barra de comandos em seu estado aberto. Os rótulos identificam as principais partes do controle.

![Uma barra de comandos fechada](images/commandbar_anatomy_open.png)

A barra de comandos é dividida em 4 áreas principais:
- A área de conteúdo está alinhada à esquerda da barra. Será exibida se a propriedade [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) estiver preenchida.
- A área de comando primária está alinhada à direita da barra. Será exibida se a propriedade [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) estiver preenchida.  
- O botão "veja mais" \[•••\] é exibido à direita da barra. Pressionar o botão "veja mais" \[•••\] revela rótulos de comando principal e abre o menu de estouro se houver comandos secundários. O botão não ficará visível quando não houver rótulos de comandos primários ou secundários. Para alterar o comportamento padrão, use a propriedade [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx).
- O menu de estouro será mostrado somente quando a barra de comandos estiver aberta e a propriedade [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) estiver preenchida. Quando o espaço é limitado, os comandos principais se movem para a área SecondaryCommands. Para alterar o comportamento padrão, use a propriedade [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx).

O layout é invertido quando [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) é **RightToLeft**.

## <a name="create-a-command-bar"></a>Criar uma barra de comandos
Este exemplo cria a barra de comandos mostrada anteriormente.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>Comandos e conteúdo
O controle CommandBar tem 3 propriedades que você pode usar para adicionar comandos e conteúdo: [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) e [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx).


### <a name="commands"></a>Comandos

Por padrão, os itens da barra de comandos são adicionados à coleção **PrimaryCommands**. Você deve adicionar os comandos na ordem de sua importância para que os comandos mais importantes estejam sempre visíveis. Quando a largura de barra de comandos é alterada, como quando os usuários redimensionam a janela de aplicativo, os comandos principais se movem dinamicamente entre a barra de comandos e o menu de estouro nos pontos de interrupção. Para alterar esse comportamento padrão, use a propriedade [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx). 

Nas telas menores (largura de 320 epx), no máximo 4 comandos primários caberão na barra de comandos. 

Você também pode adicionar comandos à coleção **SecondaryCommands**, exibidos no menu de estouro.

![Exemplo de barra de comandos com área e ícones "Mais"](images/appbar_rs2_overflow_icons.png)

Você pode mover programaticamente comandos entre PrimaryCommands e SecondaryCommands, conforme necessário.

- *Se houver um comando que deva aparecer consistentemente em várias páginas, é melhor manter esse comando em um local consistente.*
- *Recomendamos colocar os comandos Aceitar, Sim e OK à esquerda de Rejeitar, Não e Cancelar. A consistência dá aos usuários a confiança para se movimentar no sistema e ajuda-os a transferir seu conhecimento da navegação no aplicativo de um produto para outro.*

### <a name="app-bar-buttons"></a>Botões da barra de aplicativos

PrimaryCommands e SecondaryCommands podem ser preenchidos somente com os elementos de comando [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) e [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx). 

Os controles de botão da barra de aplicativos são caracterizados por um ícone e um rótulo de texto. Esses controles são otimizados para uso em uma barra de comandos, e sua aparência muda se o controle é usado na barra de comandos ou no menu de estouro.

O tamanho dos ícones no menu de estouro é 16 x 16 px, ou seja, é menor do que os ícones na área de comando principal (com 20 x 20 px). Se você usar SymbolIcon, FontIcon ou PathIcon, o ícone será dimensionado automaticamente para o tamanho correto sem perda de fidelidade quando o comando entra na área de comando secundário. 

### <a name="button-labels"></a>Rótulos de botão
A propriedade [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) de AppBarButton determina se o rótulo é exibido. Em um controle CommandBar, a barra de comandos substitui automaticamente a propriedade IsCompact do botão, conforme a barra de comandos é aberta ou fechada.

Para posicionar os rótulos do botão da barra de aplicativos, use a propriedade [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) da CommandBar.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Barra de comandos com rótulos à direita](images/app-bar-labels-on-right.png)

Em janelas maiores, considere mover os rótulos para a direita dos ícones de botões da barra de aplicativos para melhorar a legibilidade. Os rótulos na parte inferior exigem que os usuários abram a barra de comandos para revelar rótulos, enquanto os rótulos à direita ficam visíveis mesmo quando a barra de comandos é fechada.

Nos menus de estouro, os rótulos são posicionados à direita dos ícones por padrão, e **LabelPosition** é ignorado. Você pode ajustar o estilo definindo a propriedade [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) como um estilo cujo destino seja [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter). 

Rótulos de botões devem ser curtos, preferencialmente uma única palavra. Rótulos mais longos posicionados abaixo de um ícone serão ajustados em várias linhas, aumentando assim a altura geral da barra de comandos aberta. Você pode incluir um caractere de hífen condicional (0x00AD) no texto de um rótulo para indicar o limite do caractere onde uma quebra de palavra deve ocorrer. Em XAML, isso é expresso usando-se uma sequência de escape, como esta:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Quando o rótulo quebra automaticamente no local indicado, ele tem esta aparência.

![Botão de barra de aplicativos com quebra automática de rótulo](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Submenus da barra de comandos

Considere agrupamentos lógicos dos comandos como, por exemplo, colocar Responder, Responder a Todos e Encaminhar em um menu Responder. Em geral, embora um botão da barra do aplicativo ative um único comando, ele também pode ser usado para mostrar um [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) ou um [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx) com conteúdo personalizado.

![Exemplo de submenus em uma barra de comandos](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Outros tipos de conteúdo

Você pode adicionar qualquer elemento XAML à área de conteúdo definindo a propriedade **Content**. Se quiser adicionar mais de um elemento, você precisa colocá-los em um contêiner de painel e tornar o painel o único filho da propriedade Content.

Quando o estouro dinâmico estiver habilitado, o conteúdo não será recortado porque os comandos principais podem ser movidos para o menu de estouro. Caso contrário, os comandos principais têm precedência e poderão fazer com que o conteúdo seja cortado.

Quando [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) é **Compact**, o conteúdo poderá ser recortado se for maior do que o tamanho compacto da barra de comandos. Você deve tratar os eventos [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) e [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) para mostrar ou ocultar partes da interface do usuário na área de conteúdo para que eles não sejam recortados. Consulte a seção [Estados abertos e fechados](#open-and-closed-states) para obter mais informações.


## <a name="open-and-closed-states"></a>Estados abertos e fechados

A barra de comandos pode estar aberta ou fechada. Quando abertos, os botões de comando principal são exibidos com rótulos de texto e o menu de estouro será aberto se comandos secundários estiverem presentes.

Um usuário pode alternar entre esses estados pressionando o botão "veja mais" \[•••\]. Você pode alternar entre eles programaticamente definindo a propriedade [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx). 

Você pode usar os eventos [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) e [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) para responder à barra de comandos que está sendo aberta ou fechada.  
- Os eventos Opening e Closing ocorrem antes de começar a animação de transição.
- Os eventos Opened e Closed ocorrem após a conclusão da transição.

Neste exemplo, os eventos Opening e Closing são usados para alterar a opacidade da barra de comandos. Quando a barra de comandos está fechada, ela é semitransparente para que o plano de fundo do aplicativo apareça. Quando a barra de comandos está aberta, ela se torna opaca para que o usuário possa se concentrar nos comandos.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

Se um usuário interagir com outras partes de um aplicativo quando uma barra de comandos estiver aberta, a barra de comandos será fechada automaticamente. Isso é chamado de *light dismiss*. Você pode controlar o comportamento de light dismiss por definir a propriedade [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx). Quando `IsSticky="true"`, a barra permanece aberta até que o usuário pressione o botão "veja mais" \[•••\] ou selecione um item no menu de estouro. 

Recomendamos evitar barras de comandos fixas porque elas não estão em conformidade com as expectativas dos usuários em relação a light dismiss.

### <a name="display-mode"></a>Modo de Exibição

Você pode controlar como a barra de comandos é mostrada em seu estado fechado definindo a propriedade [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx). Há 3 modos de exibição fechada à sua escolha:
- **Compact**: o modo padrão. Mostra o conteúdo, os ícones de comando principal sem rótulos e o botão "veja mais" \[•••\].
- **Minimal**: mostra apenas uma barra fina que age como o botão "veja mais" \[•••\]. O usuário pode pressionar em qualquer lugar na barra para abri-lo.
- **Hidden**: a barra de comandos não é mostrada quando está fechada. Isso pode ser útil para mostrar os comandos contextuais com uma barra de comandos embutida. Nesse caso, você deve abrir a barra de comandos programaticamente definindo a propriedade **IsOpen** ou alterando ClosedDisplayMode para **Minimal** ou **Compact**.

Aqui, uma barra de comandos é usada para manter os comandos de formatação simples para um [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx). Quando a caixa de edição não tiver foco, os comandos de formatação podem distrair o usuário, por isso eles ficam ocultos. Quando a caixa de edição está sendo usada, ClosedDisplayMode da barra de comando é alterado para Compact para que os comandos de formatação fiquem visíveis.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**Observação**&nbsp;&nbsp;A implementação dos comandos de edição está fora do escopo deste exemplo. Para obter mais informações, consulte o artigo [RichEditBox](rich-edit-box.md).

Embora os modos Minimal e Hidden sejam úteis em algumas situações, tenha em mente que os usuários poderão ficar confusos se todas as ações foram ocultadas.

A alteração de ClosedDisplayMode para fornecer mais ou menos dicas para o usuário afeta o layout dos elementos ao redor. Em contraste, quando CommandBar faz a transição entre fechada e aberta, ela não afeta o layout dos outros elementos.

## <a name="placement"></a>Posicionamento
As barras de comandos podem ser colocadas na parte superior da janela do aplicativo, na parte inferior da janela do aplicativo e embutidas.

![Exemplo 1 de colocação de barra de aplicativos](images/AppbarGuidelines_Placement1.png)

-   Para dispositivos portáteis pequenos, recomendamos o posicionamento das barras de comandos na parte inferior da tela para fácil acessibilidade.
-   Para dispositivos com telas maiores, colocar barras de comando próximas ao topo da janela as torna mais perceptíveis e detectáveis.

Use a API [DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) para determinar o tamanho físico da tela.

As barras de comandos podem ser posicionadas nas seguintes regiões da tela, em telas de exibição única (exemplo à esquerda) e em telas com várias exibições (exemplo à direita). As barras de comandos embutidas podem ser colocadas em qualquer lugar no espaço de ação.

![Exemplo 2 de colocação de barra de aplicativos](images/AppbarGuidelines_Placement2.png)

>**Dispositivos de toque**: caso seja necessário que a barra de comandos fique visível para o usuário quando o teclado virtual ou o painel de entrada virtual (SIP) for exibido, você poderá atribuir a barra de comandos à propriedade [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) de uma página e ela será movida para permanecer visível quando o SIP estiver presente. Caso contrário, você deve colocar a barra de comandos embutida e posicionada em relação ao conteúdo do aplicativo.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo de XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.
- [Exemplo de execução de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artigos relacionados

* [Noções básicas de design de comandos para aplicativos UWP](../basics/commanding-basics.md)
* [Classe CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)

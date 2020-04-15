---
Description: As barras de comandos oferecem aos usuários um acesso fácil às tarefas mais comuns do seu aplicativo.
title: Barra de comandos
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 483e5d33f67ad2cd27403d7a1b229edebedfebb9
ms.sourcegitcommit: 23c5d8dfaeb6edbca780637ffd26fe892db27519
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123631"
---
# <a name="command-bar"></a>Barra de comandos

As barras de comandos proporcionam aos usuários um acesso fácil às tarefas mais comuns do seu aplicativo. As barras de comandos podem oferecer um acesso aos comandos no nível do aplicativo ou específicos da página e podem ser usadas com qualquer padrão de navegação.

> **APIs de plataforma:** [classe CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar), [classe AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [classe AppBarSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarseparator)

![Exemplo de uma barra de comandos com ícones](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

O controle CommandBar é um controle de finalidade geral, flexível e leve que pode exibir conteúdo complexo, como imagens ou blocos de texto, e comandos simples, como os controles [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) e [AppBarSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarseparator).

> [!NOTE]
> O XAML fornece os controles [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) e [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar). Use AppBar somente quando você estiver atualizando um aplicativo Universal do Windows 8 que usa AppBar e precisar minimizar as alterações. Para novos aplicativos no Windows 10, recomendamos usar o controle CommandBar. Este documento pressupõe que você está usando o controle CommandBar.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CommandBar">abri-lo e ver o CommandBar em funcionamento</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma barra de comandos expandida.

![Barra de comandos expandida](images/control-examples/command-bar-photos.png)

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
- A área de conteúdo está alinhada à esquerda da barra. Ela será mostrada se a propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content) estiver preenchida.
- A área de comandos primários está alinhada à direita da barra. Ela será mostrada se a propriedade [PrimaryCommands](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) estiver preenchida.  
- O botão "veja mais" \[•••\] é mostrado à direita da barra. Pressionar o botão "veja mais" \[•••\] revelará os rótulos dos comandos primários e abrirá o menu Estouro de capacidade se houver comandos secundários. O botão não ficará visível quando não houver rótulos de comandos primários ou secundários. Para alterar o comportamento padrão, use a propriedade [OverflowButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility).
- O menu Estouro de capacidade é mostrado somente quando a barra de comandos é aberta e a propriedade [SecondaryCommands](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) é preenchida. Quando o espaço é limitado, os comandos primários se movem para a área SecondaryCommands. Para alterar esse comportamento padrão, use a propriedade [IsDynamicOverflowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled).

O layout é invertido quando [FlowDirection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) é **RightToLeft**.

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
O controle CommandBar tem três propriedades que você pode usar para adicionar comandos e conteúdo: [PrimaryCommands](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands), [SecondaryCommands](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) e [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content).


### <a name="commands"></a>Comandos

Por padrão, os itens da barra de comandos são adicionados à coleção **PrimaryCommands**. Você deve adicionar os comandos em sua ordem de importância para que aqueles mais importantes fiquem sempre visíveis. Quando a largura de barra de comandos é alterada, como quando os usuários redimensionam a janela do aplicativo, os comandos primários se movem dinamicamente entre a barra de comandos e o menu Estouro de capacidade nos pontos de interrupção. Para alterar esse comportamento padrão, use a propriedade [IsDynamicOverflowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled). 

Nas telas menores (largura de 320 epx), caberão, no máximo, quatro comandos primários na barra de comandos. 

Você também pode adicionar comandos à coleção **SecondaryCommands**, exibidos no menu Estouro de capacidade.

![Exemplo da barra de comandos com área e ícones "Mais"](images/appbar_rs2_overflow_icons.png)

Você pode mover programaticamente comandos entre PrimaryCommands e SecondaryCommands conforme necessário.

- *Se houver um comando que deva aparecer de forma consistente em várias páginas, é melhor mantê-lo em um local consistente.*
- *Recomendamos colocar os comandos Aceitar, Sim e OK à esquerda de Rejeitar, Não e Cancelar. A consistência oferece aos usuários a confiança para se movimentarem no sistema e os ajuda a transferir o conhecimento que possuem sobre a navegação de aplicativos de um produto para outro.*

### <a name="app-bar-buttons"></a>Botões da barra de aplicativos

Tanto o PrimaryCommands quanto o SecondaryCommands podem ser preenchidos somente com os elementos de comando [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) e [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator). 

Os controles de botão da barra do aplicativo são caracterizados por um ícone e um rótulo de texto. Esses controles são otimizados para uso em uma barra de comandos, e sua aparência muda se o controle é usado na barra de comandos ou no menu Estouro de capacidade.

O tamanho dos ícones no menu Estouro de capacidade é 16 x 16 px, ou seja, é menor do que os ícones na área de comandos primários (que são de 20 x 20 px). Se você usar SymbolIcon, FontIcon ou PathIcon, o ícone será dimensionado automaticamente para o tamanho correto, sem perda de fidelidade, quando o comando entrar na área de comando secundário. 

### <a name="button-labels"></a>Rótulos de botão
A propriedade [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) de AppBarButton determina se o rótulo é exibido. Em um controle CommandBar, a barra de comandos substitui automaticamente a propriedade IsCompact do botão, conforme a barra de comandos é aberta ou fechada.

Para posicionar os rótulos do botão da barra do aplicativo, use a propriedade [DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelposition) da CommandBar.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Barra de comandos com rótulos à direita](images/app-bar-labels-on-right.png)

Em janelas maiores, considere mover os rótulos para a direita dos ícones de botões da barra do aplicativo para melhorar a legibilidade. Os rótulos na parte inferior exigem que os usuários abram a barra de comandos para revelar rótulos, enquanto os rótulos à direita ficam visíveis mesmo quando a barra de comandos é fechada.

Nos menus Estouro de capacidade, os rótulos são posicionados à direita dos ícones por padrão, e **LabelPosition** é ignorado. Você pode ajustar o estilo definindo a propriedade [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) como um estilo cujo destino seja [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter). 

Os rótulos de botões devem ser curtos, preferencialmente uma única palavra. Rótulos mais longos, posicionados abaixo de um ícone, serão ajustados em várias linhas, aumentando assim a altura geral da barra de comandos aberta. Você pode incluir um caractere de hífen condicional (0x00AD) no texto de um rótulo para indicar o limite do caractere onde uma quebra de palavra deve ocorrer. Em XAML, isso é expresso usando-se uma sequência de escape, como esta:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Quando o rótulo quebra automaticamente no local indicado, ele tem esta aparência.

![Botão de barra de aplicativos com quebra automática de rótulo](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Submenus da barra de comandos

Considere agrupamentos lógicos dos comandos como, por exemplo, colocar Responder, Responder a Todos e Encaminhar em um menu Responder. Em geral, embora um botão da barra do aplicativo ative um único comando, ele também pode ser usado para mostrar um [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) ou [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyout) com conteúdo personalizado.

![Exemplo de submenus em uma barra de comandos](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Outros tipos de conteúdo

Você pode adicionar qualquer elemento XAML à área de conteúdo definindo a propriedade **Content**. Se quiser adicionar mais de um elemento, você precisa colocá-los em um contêiner de painel e tornar o painel o único filho da propriedade Content.

Quando o estouro dinâmico estiver habilitado, o conteúdo não será recortado porque os comandos primários podem ser movidos para o menu Estouro de capacidade. Caso contrário, os comandos primários têm preferência e poderão fazer com que o conteúdo seja recortado.

Quando [ClosedDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) é **Compact**, o conteúdo poderá ser recortado se for maior do que o tamanho compacto da barra de comandos. Você deverá tratar os eventos [Opening](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.opening) e [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closed) para mostrar ou ocultar partes da interface do usuário na área de conteúdo para que eles não sejam recortados. Consulte a seção [Estados abertos e fechados](#open-and-closed-states) para obter mais informações.


## <a name="open-and-closed-states"></a>Estados abertos e fechados

A barra de comandos pode estar aberta ou fechada. Quando estiver aberta, ela mostrará os botões de comandos primários com rótulos de texto e abrirá o menu Estouro de capacidade (se não houver comandos secundários).
A barra de comandos abrirá o menu Estouro de capacidade para cima (acima dos comandos primários) ou para baixo (abaixo dos comandos primários). A direção padrão está ativa, mas se não houver espaço suficiente para abrir o menu Estouro de capacidade para cima, a barra de comandos irá abri-lo para baixo. 

O usuário poderá alternar entre esses estados pressionando o botão "veja mais" \[•••\]. Você pode alternar entre eles programaticamente definindo a propriedade [IsOpen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen). 

Você pode usar os eventos [Opening](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.opening), [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.opened), [Closing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closing) e [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closed) para responder à barra de comandos que está sendo aberta ou fechada.  
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

Se um usuário interagir com outras partes de um aplicativo quando a barra de comandos estiver aberta, ela será fechada automaticamente. Esse recurso é chamado de *light dismiss*. Você pode controlar o comportamento de light dismiss ao definir a propriedade [IsSticky](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky). Quando `IsSticky="true"`, a barra permanecerá aberta até que o usuário pressione o botão "veja mais" \[•••\] ou selecione um item de menu Estouro de capacidade. 

Recomendamos evitar barras de comandos fixas porque elas não estão em conformidade com as expectativas dos usuários em relação aos [comportamentos de light dismiss e de foco do teclado](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#light-dismiss).

### <a name="display-mode"></a>Modo de Exibição

Você pode controlar como a barra de comandos é mostrada em seu estado “fechada” definindo a propriedade [ClosedDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode). Há 3 modos de exibição fechada à sua escolha:
- **Compacto**: O modo padrão. Mostra o conteúdo, os ícones de comandos primários sem rótulos e o botão "veja mais" \[•••\].
- **Mínimo**: mostra apenas uma barra fina que age como o botão "veja mais" \[•••\]. O usuário pode pressionar em qualquer lugar na barra para abri-lo.
- **Oculto**: a barra de comandos não é mostrada quando é fechada. Isso pode ser útil para mostrar os comandos contextuais com uma barra de comandos embutida. Nesse caso, você deve abrir a barra de comandos programaticamente definindo a propriedade **IsOpen** ou alterando ClosedDisplayMode para **Minimal** ou **Compact**.

Aqui, uma barra de comandos é usada para manter os comandos de formatação simples para um [RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). Quando a caixa de edição não tiver foco, os comandos de formatação podem distrair o usuário, por isso eles ficam ocultos. Quando a caixa de edição está sendo usada, ClosedDisplayMode da barra de comando é alterado para Compact para que os comandos de formatação fiquem visíveis.

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
-   Para dispositivos com telas maiores, colocar as barras de comandos próximas do topo da janela as torna mais perceptíveis e detectáveis.

Use a API [DiagonalSizeInInches](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.diagonalsizeininches) para determinar o tamanho da tela física.

As barras de comandos podem ser posicionadas nas seguintes regiões da tela, em telas de exibição única (exemplo à esquerda) e em telas com várias exibições (exemplo à direita). As barras de comandos embutidas podem ser colocadas em qualquer lugar no espaço de ação.

![Exemplo 2 de colocação de barra de aplicativos](images/AppbarGuidelines_Placement2.png)

>**Dispositivos de toque**: caso seja necessário que a barra de comandos fique visível para o usuário quando o teclado virtual ou o painel de entrada virtual (SIP) for exibido, você poderá atribuir a barra de comandos à propriedade [BottomAppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.bottomappbar) de uma página e ela será movida para permanecer visível quando o SIP estiver presente. Caso contrário, você deve colocar a barra de comandos embutida e posicionada em relação ao conteúdo do aplicativo.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.
- [Exemplo de execução de comandos XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>Artigos relacionados

* [Noções básicas de design de comandos para aplicativos UWP](../basics/commanding-basics.md)
* [Classe CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)

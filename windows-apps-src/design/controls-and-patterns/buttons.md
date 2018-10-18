---
author: QuinnRadich
Description: A button gives the user a way to trigger an immediate action.
title: Botões
label: Buttons
template: detail.hbs
ms.author: quradic
ms.date: 10/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: badaefc406daa5f4500c76262d916f47d82e7a52
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4754238"
---
# <a name="buttons"></a>Botões

Um botão dá ao usuário uma forma de acionar uma ação imediata. Alguns botões são especializados em tarefas específicas, como navegação, ações repetidas ou apresentar menus.

![Exemplo de botões](images/controls/button.png)

A estrutura XAML fornece um controle de botão padrão, bem como vários controles de botão especializado.

Controle | Descrição
------- | -----------
[Botão](/uwp/api/windows.ui.xaml.controls.button) | Inicia uma ação imediata. Pode ser usado com uma associação de comando ou o evento Click.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Um botão que gera um evento de clique continuamente enquanto pressionado.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Um botão que foi criado como um hiperlink, usado para navegação. Para saber mais, consulte [Hiperlinks](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | (Visualização) Um botão com uma divisa para abrir um submenu anexado.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | (Visualização) Um botão com dois lados. Um lado inicia uma ação, e o outro lado abre um menu.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | (Visualização) Um botão de alternância com dois lados. Um lado alterna ativar/desativar, e o outro lado abre um menu.

| **Baixar a biblioteca de interface do usuário do Windows** |
| - |
| DropDownButton, divisão e ToggleSplitButton são incluídas como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte a [Visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **APIs da plataforma** | **APIs de biblioteca de interface do usuário do Windows** |
| - | - |
| [Evento click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [propriedade de comando](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Classe DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [classe SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [classe ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use um **botão** para permitir que o usuário inicie uma ação imediata, como enviar um formulário.

Não use um botão quando a ação é navegar para outra página; Use um [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) . Para saber mais, consulte [Hiperlinks](hyperlinks.md).
> Exceção: na navegação do assistente, use botões rotulados como "Voltar" e "Próximo". Para outros tipos de navegação regressiva ou para um nível superior, use um [botão Voltar](../basics/navigation-history-and-backwards-navigation.md).

Use um **RepeatButton** quando o usuário pode disparar uma ação repetidamente. Por exemplo, use um RepeatButton para incrementar ou diminuir um valor em um contador.

Use um **DropDownButton** quando o botão tem um submenu que contém mais opções. Na divisa padrão fornece uma indicação visual de que o botão inclui um submenu.

Use um **SplitButton** quando você deseja que o usuário seja capaz de iniciar uma ação imediata ou escolher opções adicionais de forma independente.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Button">abrir o aplicativo e ver o Botão em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Baixe o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Este exemplo usa dois botões, Permitir e Bloquear, em uma caixa de diálogo solicitando acesso à localização.

![Exemplo de botões, usados em uma caixa de diálogo](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Criar um botão

Este exemplo mostra um botão que responde a um clique.

Crie o botão em XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Ou crie o botão em código.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Manipule o evento Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Interação de botão

Quando você toca em um botão com um dedo ou uma caneta, ou pressiona o botão esquerdo do mouse enquanto o ponteiro está sobre ele, o botão gera o evento [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Se um botão tem foco do teclado, pressionar a tecla Enter ou a barra de espaço também aciona o evento Click.

Geralmente, não se pode manipular eventos de baixo nível [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) em um Botão porque, em vez disso, ele tem o comportamento Click. Para saber mais, consulte [Events and routed events overview](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Você pode alterar a forma como um botão aciona o evento Click alterando a propriedade [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). O valor ClickMode padrão é **Release**, mas você também pode definir ClickMode de um botão como **Passar o mouse sobre** ou **Pressionar**. Se ClickMode for **Passar o mouse sobre**, o evento Click não poderá ser chamado com o teclado ou o toque.


### <a name="button-content"></a>Conteúdo do botão

Botão é um [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sua propriedade de conteúdo XAML é [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita uma sintaxe semelhante para XAML: `<Button>A button's content</Button>`. Você pode definir qualquer objeto como conteúdo do botão. Se o conteúdo for um [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), ele é renderizado no botão. Se o conteúdo for outro tipo de objeto, a representação da cadeia de caracteres é exibida no botão.

Geralmente, o conteúdo de um botão é texto. Veja as recomendações de design para botões com conteúdo de texto:
-   Utilize um texto conciso, específico e autoexplicativo que descreva claramente a ação executada pelo botão. Geralmente, o conteúdo de texto do botão é uma única palavra, um verbo.
-   Use a fonte padrão a menos que suas diretrizes de marca digam para usar algo diferente.
-   Para texto mais curto, evite botões de comando estreitos usando uma largura mínima do botão de 120px.
- Para texto mais longo, evite botões de comando largos limitando o texto a um tamanho máximo de 26 caracteres.
-   Se o conteúdo em texto de um botão for dinâmico (isto é, [localizado](../globalizing/globalizing-portal.md)), considere como o botão será redimensionado e o que acontecerá para controles em volta dele.

<table>
<tr>
<td> <b>Você precisa corrigir:</b><br> Botões com estouro de texto. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Opção 1:</b><br> Aumente a largura do botão, empilhe botões e delimite se o comprimento do texto for maior que 26 caracteres. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Opção 2:</b><br> Aumente a altura do botão e delimite o texto. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Você também pode personalizar elementos visuais que compõem a aparência do botão. Por exemplo, você poderia substituir o texto por um ícone ou usar um ícone mais texto.

Aqui, um **StackPanel** que contém uma imagem e o texto está definido como conteúdo de um botão.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

O botão fica assim.

![Um botão com conteúdo de imagem e texto](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Criar um botão de repetição

Um [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) é um botão que gera eventos [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidas vezes a partir do momento em que é pressionado até ser liberado. Defina a propriedade [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar o tempo que o RepeatButton aguarda após ser pressionado antes de começar a repetir a ação de clique. Defina a propriedade [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar o tempo entre as repetições da ação de clique. O tempo para as duas propriedades são especificados em milissegundos.

O exemplo a seguir mostra dois controles RepeatButton cujos respectivos eventos Click são usados para aumentar e diminuir o valor mostrado em um bloco de texto.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>Criar um botão suspenso

> **Visualização**: DropDownButton requer a [compilação do Windows 10 Insider Preview e o SDK mais recente](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Um [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) é um botão que mostra uma divisa como um indicador visual que ele tenha um submenu anexado que contém mais opções. Ele tem o mesmo comportamento que um botão padrão com um submenu; somente a aparência é diferente.

Botão suspenso herda o evento Click, mas você normalmente não usá-lo. Em vez disso, use a propriedade de submenu para anexar um submenu e invocar ações usando as opções de menu no menu suspenso. O submenu é aberta automaticamente quando o botão é clicado.

> [!TIP]
> Para obter mais informações sobre submenus, consulte [Menus e menus de contexto](menus.md).

### <a name="example---drop-down-button"></a>Exemplo - botão suspenso

Este exemplo mostra como criar um botão suspenso com um submenu que contém comandos para alinhamento de parágrafo em uma RichEditBox. (Para obter mais informações e código, consulte a [caixa de edição com formato](rich-edit-box.md)).

![Um botão com comandos de alinhamento suspenso](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>Criar um botão de divisão

> **Visualização**: SplitButton requer a [compilação do Windows 10 Insider Preview e o SDK mais recente](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Um [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) tem duas partes que podem ser invocados separadamente. Uma parte se comporta como um botão padrão e invoca uma ação imediata. A outra parte invoca um submenu que contém as opções adicionais que o usuário pode escolher.

> [!NOTE]
> Quando invocado com toque, o botão de divisão se comporta como um botão; suspenso as duas metades do botão invocar o submenu. Com outros métodos de entrada, um usuário pode invocar qualquer parte do botão separadamente.

O comportamento típico para um botão de divisão é:

- Quando o usuário clica a parte do botão, manipule o evento Click para invocar a opção que está atualmente selecionada no menu suspenso.
- Quando a lista suspensa estiver aberta, invocação de identificador dos itens a lista suspensa para ambas as alteração qual opção é selecionado e chamá-lo, em seguida. É importante invocar o item de submenu porque o evento Click não ocorre usando o recurso touch do botão.

> [!TIP]
> Há muitas maneiras de colocar itens na lista para baixo e manipular a invocação. Se você usar um ListView ou GridView, uma maneira é manipular o evento SelectionChanged. Se você fizer isso, defina [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) como **false**. Isso permite aos usuários navegar as opções usando um teclado sem chamar o item em cada alteração.

### <a name="example---split-button"></a>Exemplo - botão de divisão

Este exemplo mostra como criar um botão de divisão que é usado para alterar a cor de primeiro plano do texto selecionado em uma RichEditBox. (Para obter mais informações e código, consulte a [caixa de edição com formato](rich-edit-box.md)).

![Um botão de divisão para a seleção de cor de primeiro plano](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout" Placement="BottomEdgeAlignedLeft">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}" 
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>Criar um botão de alternância de divisão

> **Visualização**: ToggleSplitButton requer a [compilação do Windows 10 Insider Preview e o SDK mais recente](https://insider.windows.com/for-developers/) ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Um [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) tem duas partes que podem ser invocados separadamente. Uma parte se comporta como um botão de alternância que pode ser ativado ou desativado. A outra parte invoca um submenu que contém as opções adicionais que o usuário pode escolher.

Um botão de alternância de divisão normalmente é usado para habilitar ou desabilitar um recurso quando o recurso tem várias opções que o usuário pode escolher. Por exemplo, em um editor de documento, ela pode ser usada para ativar ou desativar, a listas enquanto suspenso é usado para escolher o estilo da lista.

> [!NOTE]
> Quando invocado com toque, o botão de divisão se comporta como um botão suspenso. Com outros métodos de entrada, um usuário pode invocar qualquer parte do botão separadamente. Com o toque, as duas metades do botão invocar o submenu. Portanto, você deve incluir uma opção em seu conteúdo de submenu para o botão de alternância ativado ou desativado.

### <a name="differences-with-togglebutton"></a>Diferenças com ToggleButton

Ao contrário de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), ToggleSplitButton não tem um estado indeterminado. Como resultado, você deve ter em mente essas diferenças:

- ToggleSplitButton não tem uma propriedade **IsThreeState** ou evento **indeterminado** .
- A propriedade [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) é apenas um **bool**, não um **bool anulável**.
- ToggleSplitButton tem apenas o evento [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) ; ele não tem eventos **Checked** e **Unchecked** separados.

### <a name="example---toggle-split-button"></a>Exemplo - botão de alternância de divisão

O exemplo a seguir mostra como uma botão de divisão de alternância pode ser usado para ativar ou desativar a formatação de lista e alterar o estilo da lista, em uma RichEditBox. (Para obter mais informações e código, consulte a [caixa de edição com formato](rich-edit-box.md)).

![Um botão de divisão de alternância para selecionar estilos de lista](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout Placement="BottomEdgeAlignedLeft">
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged" 
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>Recomendações

- Assegure que a finalidade e o estado de um botão estejam claros para o usuário.
- Quando houver vários botões para a mesma decisão (como em uma caixa de diálogo de confirmação), apresente os botões de confirmação nesta ordem, onde [Faça isso] e [Não faça isso] são respostas específicas para a instrução principal:
    - OK/[Faça]/Sim
    - [Não faça]/Não
    - Cancelar
- Exponha apenas um ou dois botões de cada vez para o usuário, por exemplo, Aceitar e Cancelar. Se precisar expor mais ações para o usuário, considere usar [checkboxes](checkbox.md) or [radio buttons](radio-button.md), de onde o usuário pode selecionar ações com apenas um botão de comando para acionar tais ações.
- Para uma ação que precise ser avaliada em múltiplas páginas em seu aplicativo, em vez de duplicar um botão em múltiplas páginas, considere usar uma [barra inferior do aplicativo](app-bars.md).

### <a name="recommended-single-button-layout"></a>Layout de botão único recomendado

Se o layout requer apenas um botão, ele deve ser alinhado à esquerda ou à direita com base no contexto de contêiner.

- As caixas de diálogo com apenas um botão devem **alinhar o botão à direita**. Se a caixa de diálogo contém apenas um botão, verifique se o botão executa a ação segura e não destrutiva. Se você usar [ContentDialog](dialogs.md) e especificar um único botão, ele será automaticamente para alinhar à direita.

![Um botão dentro de uma caixa de diálogo](images/pushbutton_doc_dialog.png)

- Se o botão é exibido dentro de um contêiner da interface do usuário (por exemplo, em uma notificação do sistema, um submenu ou um item de modo de exibição de lista), você deve **alinhar o botão à direita** no contêiner.

![Um botão em um contêiner](images/pushbutton_doc_container.png)

- Nas páginas com um único botão (por exemplo, um botão de "Aplicação" na parte inferior de uma página de configurações), você deve **alinhar o botão à esquerda**. Isso garante que o botão esteja alinhado com o restante do conteúdo da página.

![Um botão em uma página](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Botões Voltar

O botão Voltar é um elemento de interface do usuário fornecida pelo sistema que permite a navegação regressiva através da pilha Voltar ou do histórico de navegação de um usuário. Você não precisa criar seu próprio botão Voltar, mas talvez precise trabalhar um pouco para permitir uma boa experiência de navegação regressiva. Para obter mais informações, consulte [Histórico e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Botões de opção](radio-button.md)
- [Caixas de seleção](checkbox.md)
- [Chaves de alternância](toggles.md)

---
Description: Um botão dá ao usuário uma forma de acionar uma ação imediata.
title: Botões
label: Buttons
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a3cd8a0c988df08047b10911a4d4f55e3ba1cb6e
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684102"
---
# <a name="buttons"></a>Botões

Um botão dá ao usuário uma forma de acionar uma ação imediata. Alguns botões são especializados para tarefas específicas, como navegação, ações repetidas ou apresentação de menus.

![Exemplo de botões](images/controls/button.png)

A estrutura [XAML (Extensible Application Markup Language)](../../xaml-platform/xaml-overview.md) fornece um controle de botão padrão, bem como vários controles de botão especializados.

Control | Descrição
------- | -----------
[Botão](/uwp/api/windows.ui.xaml.controls.button) | Um botão que inicia uma ação imediata. Pode ser usado com um evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou uma associação [Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Um botão que aciona um evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) continuamente enquanto pressionado.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Um botão que tem o estilo de um hiperlink e é usado para navegação. Para obter mais informações sobre hiperlinks, consulte [Hiperlinks](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Um botão com uma divisa para abrir um submenu anexado.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | Um botão com dois lados. Um dos lados inicia uma ação e o outro abre um menu.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Um botão de alternância com dois lados. Um lado ativa/desativa e o outro abre um menu.
[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) | Um botão que pode estar ativado ou desativado.

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| **DropDownButton**, **SplitButton** e **ToggleSplitButton** estão incluídos como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **APIs da plataforma** | **APIs da biblioteca de interface do usuário do Windows** |
| - | - |
| [Evento Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)<br/> [Propriedade Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Classe DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)<br/> [Classe SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton)<br/> [Classe ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um controle **Button** para permitir que o usuário inicie uma ação imediata, como enviar um formulário.

Não use um controle **Button** quando a ação for de navegar para outra página; em vez disso, use um controle [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton). Para obter mais informações sobre hiperlinks, consulte [Hiperlinks](hyperlinks.md).

> [!IMPORTANT]
> para navegação do assistente, use botões rotulados como *Back* e *Next*. Para outros tipos de navegação regressiva ou para um nível superior, use um [botão voltar](../basics/navigation-history-and-backwards-navigation.md).

Use um controle **RepeatButton** quando o usuário quiser disparar uma ação repetidamente. Por exemplo, use um controle **RepeatButton** para incrementar ou reduzir um valor em um contador.

Use um controle **DropDownButton** quando o botão tiver um submenu que contém mais opções. A divisa padrão fornece uma indicação visual de que o botão inclui um submenu.

Use um controle **SplitButton** quando quiser que o usuário seja capaz de iniciar uma ação imediata ou escolher opções adicionais de forma independente.

Use um controle **ToggleButton** quando quiser que o usuário seja capaz de alternar imediatamente entre dois estados mutuamente exclusivos e um botão for a melhor opção para as necessidades da sua interface do usuário. Exceto quando a interface do usuário se beneficia do uso desse botão, pode ser uma melhor opção usar um [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), uma [CheckBox](checkbox.md), um [RadioButton](radio-button.md) ou um [ToggleSwitch](toggles.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Button">abrir o aplicativo e ver o Botão em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Este exemplo usa dois botões, **Allow** e **Block**, em uma caixa de diálogo que solicita acesso à localização.

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

Manipular o evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click).

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

Quando você toca em um controle **Button** com um dedo ou uma caneta, ou pressiona o botão esquerdo do mouse enquanto o ponteiro está sobre ele, o botão gera o evento [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Se um botão tiver foco do teclado, pressionar a tecla Enter ou a barra de espaço também acionará o evento **Click**.

Geralmente, não se pode manipular eventos de baixo nível [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) em um objeto **Button** porque, em vez disso, ele tem o comportamento de **Click**. Para saber mais, confira [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

Você pode alterar a forma como um botão aciona o evento **Click**, alterando a propriedade [ClickMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.clickmode). O valor padrão de **ClickMode** é **Release**, mas você também pode definir o **ClickMode** de um botão como **Hover** ou **Press**. Se **ClickMode** for **Hover**, o evento **Click** não poderá ser chamado usando o teclado ou o toque.


### <a name="button-content"></a>Conteúdo do botão

**Button** é um controle de conteúdo da classe [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl). Sua propriedade de conteúdo XAML é [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content), que habilita uma sintaxe assim para XAML: `<Button>A button's content</Button>`. Você pode definir qualquer objeto como conteúdo do botão. Se o conteúdo for um objeto [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), ele será renderizado no botão. Se o conteúdo for outro tipo de objeto, a representação da cadeia de caracteres será mostrada no botão.

Geralmente, o conteúdo de um botão é texto. Quando projetar o texto, use as seguintes recomendações:

-  Utilize um texto conciso, específico e autoexplicativo que descreva claramente a ação executada pelo botão. Geralmente, o texto do botão é uma única palavra que é um verbo.

-  Use a fonte padrão, a menos que suas diretrizes de marca digam para usar algo diferente.

-  Para texto mais curto, evite botões de comando estreitos usando uma largura mínima do botão de 120px.

- Para texto mais longo, evite botões de comando largos, limitando o texto a um tamanho máximo de 26 caracteres.

-  Se o conteúdo de texto de um botão for dinâmico (isto é, [localizado](../globalizing/globalizing-portal.md)), considere como o botão será redimensionado e o que acontecerá com controles em volta dele.

<table>
<tr>
<td> <b>Necessário corrigir:</b><br> botões com estouro de texto. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Opção 1:</b><br> aumente a largura do botão, empilhe botões e delimite se o comprimento do texto for maior que 26 caracteres. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Opção 2:</b><br> aumente a altura do botão e delimite o texto. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Você também pode personalizar elementos visuais que compõem a aparência do botão. Por exemplo, você poderia substituir o texto por um ícone ou usar um ícone além do texto.

Aqui, um **StackPanel** que contém uma imagem e texto está definido como conteúdo de um botão.

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

Um controle [RepeatButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) é um botão que gera eventos [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) repetidas vezes a partir do momento em que é pressionado até ser liberado. Defina a propriedade [Delay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) para especificar o tempo que o controle **RepeatButton** aguarda após ser pressionado antes de começar a repetir a ação de clique. Defina a propriedade [Interval](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) para especificar o tempo entre as repetições da ação de clique. O tempo para as duas propriedades são especificados em milissegundos.

O exemplo a seguir mostra dois controles **RepeatButton** cujos respectivos eventos **Click** são usados para aumentar e diminuir o valor mostrado em um bloco de texto.

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

> **DropDownButton** exige a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou o Windows 10, versão 1809 (SDK 17763) ou posterior. Para baixar o SDK mais recente, consulte [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk); para baixar um SDK anterior, consulte [Arquivo morto de emulador e SDK do Windows](https://developer.microsoft.com/windows/downloads/sdk-archive).

Um [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) é um botão que mostra uma divisa como um indicador visual de que ele tem um submenu anexado que contém mais opções. Ele tem o mesmo comportamento de um controle **Button** padrão com um submenu, apenas a aparência é diferente.

O botão suspenso herda o evento de **Click**, mas normalmente você não o usa. Em vez disso, você usa a propriedade **Flyout** para anexar um submenu e invocar ações usando as opções de menu no submenu. O submenu é aberto automaticamente quando o botão é clicado.
Especifique a propriedade [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) do submenu para garantir o posicionamento desejado em relação ao botão. O algoritmo de posicionamento padrão pode não produzir o posicionamento desejado em todas as situações.

> [!TIP]
> Para obter mais informações sobre submenus, confira [Menus e menus de contexto](menus.md).

### <a name="example---drop-down-button"></a>Exemplo – Botão suspenso

Este exemplo mostra como criar um botão suspenso com um submenu que contém comandos para alinhamento de parágrafo em um controle **RichEditBox**. (Para obter mais informações e código, confira [Caixa de edição com formato](rich-edit-box.md)).

![Um botão suspenso com comandos de alinhamento](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
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

 > [!IMPORTANT]
 > **SplitButton** exige a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou o Windows 10, versão 1809 (SDK 17763) ou posterior. Para baixar o SDK mais recente, consulte [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk); para baixar um SDK anterior, consulte [Arquivo morto de emulador e SDK do Windows](https://developer.microsoft.com/windows/downloads/sdk-archive).

Um controle [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) tem duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão padrão e invoca uma ação imediata. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher.

> [!NOTE]
> Quando invocado com toque, o botão de divisão se comporta como um botão suspenso; as duas metades do botão invocam o submenu. Com outros métodos de entrada, um usuário pode invocar qualquer metade do botão separadamente.

O comportamento típico de um botão de divisão é:

- Quando o usuário clica na parte do botão, manipule o evento **Click** para invocar a opção selecionada no momento na lista suspensa.

- Quando a lista suspensa é aberta, manipule a invocação dos itens no menu suspenso para alterar a opção que está selecionada e invocá-la. É importante invocar o item do submenu, pois o evento **Click** do botão não ocorre ao usar o toque.

> [!TIP]
> Há muitas maneiras de colocar itens na lisa suspensa e manipular sua invocação. Se você usar um **ListView** ou **GridView**, uma maneira será manipular o evento **SelectionChanged**. Se fizer isso, defina [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) como **false**. Isso permite aos usuários navegar pelas opções usando um teclado, sem invocar o item a cada alteração.

### <a name="example---split-button"></a>Exemplo – Botão de divisão

Este exemplo mostra como criar um botão de divisão usado para alterar a cor de primeiro plano do texto selecionado em um controle **RichEditBox**. (Para obter mais informações e código, confira [Caixa de edição com formato](rich-edit-box.md)).
O submenu do botão de divisão usa [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) como valor padrão para sua propriedade [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). Você não pode substituir esse valor.

![Um botão de divisão para selecionar a cor de primeiro plano](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
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

## <a name="create-a-toggle-split-button"></a>Criar um botão de divisão de alternância

> [!NOTE]
> **ToggleSplitButton** exige a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/) ou o Windows 10, versão 1809 (SDK 17763) ou posterior. Para baixar o SDK mais recente, consulte [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk); para baixar um SDK anterior, consulte [Arquivo morto de emulador e SDK do Windows](https://developer.microsoft.com/windows/downloads/sdk-archive).

Um controle [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) tem duas partes que podem ser invocadas separadamente. Uma parte se comporta como um botão de alternância que pode ser ativado ou desativado. A outra parte invoca um submenu que contém opções adicionais dentre as quais o usuário pode escolher.

Um botão de divisão de alternância normalmente é usado para habilitar ou desabilitar um recurso quando o recurso tem várias opções que o usuário pode escolher. Por exemplo, em um editor de documentos, ele pode ser usado para ativar ou desativar listas, enquanto a lista suspensa é usada para escolher o estilo da lista.

> [!NOTE]
> Quando invocado com toque, o botão de divisão de alternância se comporta como um botão suspenso. Com outros métodos de entrada, um usuário pode alternar e invocar as duas metades do botão separadamente. Com toque, as duas metades do botão invocam o menu suspenso. Portanto, você precisa incluir uma opção no conteúdo do submenu para alternar o botão entre ativado ou desativado.


### <a name="differences-with-togglebutton"></a>Diferenças com ToggleButton

Diferente de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), **ToggleSplitButton** não tem estado indeterminado. Como resultado, você deve ter em mente as seguintes diferenças:

- **ToggleSplitButton** não tem uma propriedade **IsThreeState** ou um evento **Indeterminate**.
- A propriedade [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) é apenas um booliano, e não um **Nullable<bool>** .
- **ToggleSplitButton** tem apenas o evento [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged); ele não tem eventos **Checked** e **Unchecked** separados.


### <a name="example---toggle-split-button"></a>Exemplo – Botão de divisão de alternância

O exemplo a seguir mostra como um botão de divisão de alternância pode ser usado para ativar ou desativar a formatação de lista, bem como alterar o estilo da lista, em um controle **RichEditBox**. (Para obter mais informações e código, confira [Caixa de edição com formato](rich-edit-box.md)).
O submenu do botão de divisão de alternância usa [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) como valor padrão para sua propriedade [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). Você não pode substituir esse valor.

![Um botão de divisão de alternância para selecionar estilos de lista](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
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

- Quando houver vários botões para a mesma decisão (como em uma caixa de diálogo de confirmação), apresente os botões de confirmação nesta ordem, em que [Faça] e [Não faça] são respostas específicas para a instrução principal:
  - OK/[Faça]/Sim
    - [Não faça]/Não
    - Cancelar

- Exponha apenas um ou dois botões de cada vez para o usuário, por exemplo, **Aceitar** e **Cancelar**. Se precisar expor mais ações para o usuário, considere usar [caixas de seleção](checkbox.md) ou [botões de opção](radio-button.md), de onde o usuário poderá selecionar ações com apenas um botão de comando para acionar tais ações.

- Para uma ação que precise ser avaliada em múltiplas páginas em seu aplicativo, em vez de duplicar um botão em múltiplas páginas, considere usar uma [barra inferior do aplicativo](app-bars.md).


### <a name="recommended-single-button-layout"></a>Layout de botão único recomendado

Se o layout exige apenas um botão, ele deve ser alinhado à esquerda ou à direita com base no contexto de contêiner.

  - As caixas de diálogo com apenas um botão devem **alinhar o botão à direita**. Se a caixa de diálogo contiver apenas um botão, verifique se o botão executa a ação segura e não destrutiva. Se você usar [ContentDialog](dialogs.md) e especificar um único botão, ele será automaticamente alinhado à direita.

    ![Um botão dentro de uma caixa de diálogo](images/pushbutton_doc_dialog.png)

  - Se o botão é exibido dentro de um contêiner da interface do usuário (por exemplo, em uma notificação do sistema, um submenu ou um item de modo de exibição de lista), você deve **alinhar o botão à direita** no contêiner.

    ![Um botão dentro de um contêiner](images/pushbutton_doc_container.png)

  - Em páginas com um único botão (por exemplo, um botão **Aplicar** na parte inferior de uma página de configurações), você deve **alinhar o botão à esquerda**. Isso garante que o botão esteja alinhado com o restante do conteúdo da página.

    ![Um botão em uma página](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>Botões Voltar

O botão Voltar é um elemento de interface do usuário fornecida pelo sistema que permite a navegação regressiva através da pilha Voltar ou do histórico de navegação de um usuário. Você não precisa criar seu próprio botão Voltar, mas talvez precise trabalhar um pouco para permitir uma boa experiência de navegação regressiva. Para obter mais informações, consulte [Histórico de navegação e navegação retroativa para aplicativos UWP](../basics/navigation-history-and-backwards-navigation.md).


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Galeria de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): este exemplo mostra todos os controles XAML em um formato interativo.


## <a name="related-articles"></a>Artigos relacionados

- [Classe Button](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)
- [Botões de opção](radio-button.md)
- [Caixas de seleção](checkbox.md)
- [Botões de alternância](toggles.md)

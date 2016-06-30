---
author: Jwmsft
Description: "É possível usar um controle RichEditBox para inserir e editar documentos rich text que contenham texto formatado, hiperlinks e imagens. Você pode tornar um RichEditBox somente leitura definindo sua propriedade IsReadOnly como \"true\"."
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: Rich edit box
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: fc685b952db7292a9eea4d8a54bd6e2685cb13c0

---
# Caixa de edição com formato
É possível usar um controle RichEditBox para inserir e editar documentos rich text que contenham texto formatado, hiperlinks e imagens. Você pode tornar um RichEditBox somente leitura definindo sua propriedade IsReadOnly como **true**.

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)
-   [**Propriedade Document**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx)
-   [**Propriedade IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isreadonly.aspx)
-   [**Propriedade IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx)

## Esse é o controle correto?

Use **RichEditBox** para exibir e editar arquivos Rich Text. Você não usa um RichEditBox para obter a entrada do usuário em seu aplicativo da maneira que você usa outras caixas de entrada de texto padrão. Em vez disso, você o usa para trabalhar com arquivos de texto que são separados de seu aplicativo. Em geral, você salva o texto inserido em um RichEditBox em um arquivo. rtf.
-   Se a finalidade principal da caixa de texto multilinha for criar documentos (como entradas de blog ou o conteúdo de uma mensagem de email), e esses documentos exigirem rich text, utilize uma caixa RTF.
-   Se você quiser que os usuários consigam formatar seu texto, use uma caixa RTF.
-   Ao capturar texto que só será consumido e não exibido mais tarde aos usuários, use um controle de entrada de texto sem formatação.
-   Para todos os outros cenários, use um controle de entrada de texto sem formatação.

Para saber mais sobre como escolher o controle de texto certo, consulte o artigo [Controles de texto](text-controls.md).

## Exemplos

Essa caixa de edição com formato contém um documento rich text aberto. Os botões de formatação e arquivo não fazem parte da caixa de edição com formato, mas você deve fornecer pelo menos um conjunto mínimo de botões de estilo e implementar suas ações.

![Uma caixa rich text com um documento aberto](images/rich-edit-box.png)

## Criar uma caixa de edição com formato

Por padrão, RichEditBox dá suporte à verificação ortográfica. Para desabilitar o corretor ortográfico, defina a propriedade [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx) como **false**. Para saber mais, consulte Diretrizes e lista de verificação de verificação ortográfica.

Use a propriedade [Document](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx) de RichEditBox para obter seu conteúdo. O conteúdo de RichEditBox é um objeto [Windows.UI.Text.ITextDocument](https://msdn.microsoft.com/library/windows/apps/xaml/bb774052.aspx), ao contrário do controle RichTextBlock, que usa objetos [Windows.UI.Xaml.Documents.Block](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.block.aspx) como conteúdo. A interface ITextDocument fornece um modo de carregar e salvar o documento em um fluxo, recuperar faixas de texto, obter a seleção ativa, desfazer e refazer alterações, definir atributos de formatação padrão, entre outras coisas.

Este exemplo mostra como editar, carregar e salvar um arquivo .rtf (Rich Text Format) em RichEditBox.

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile" 
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click" 
                  ToolTipService.ToolTip="Save file" 
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold" 
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click" 
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click" 
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton" 
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we 
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the 
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## Escolher o teclado correto para seu controle de texto

Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira. O layout do teclado padrão é geralmente apropriado para trabalhar com documentos rich text.

Para saber mais sobre como usar escopos de entrada, consulte [Usar o escopo de entrada para alterar o teclado virtual]().

## Recomendações

-   Ao criar uma caixa de texto Rich Text, forneça botões de estilo e implemente as ações comandadas por eles.
-   Use uma fonte que seja consistente com o estilo de seu aplicativo.
-   Faça com que a altura do controle de texto seja suficiente para acomodar entradas típicas.
-   Não deixe que seus controles de entrada de texto aumentem de tamanho enquanto os usuários digitam.
-   Não use uma caixa de texto multilinha quando os usuários só precisarem de uma linha única.
-   Não use um controle Rich Text se um controle de texto sem formatação for adequado.





## Artigos relacionados

[Controles de texto](text-controls.md)

**Para designers**
- [Diretrizes para verificação ortográfica](spell-checking-and-prediction.md)
- [Adicionando pesquisa](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Diretrizes para entrada de texto](text-controls.md)

**Para desenvolvedores (XAML)**
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe Windows.UI.Xaml.Controls PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519)




<!--HONumber=Jun16_HO4-->



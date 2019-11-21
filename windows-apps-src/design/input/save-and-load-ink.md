---
Description: Os aplicativos UWP que dão suporte ao Windows Ink podem serializar e desserializar traços de tinta para um arquivo Ink Serialized Format (ISF). O arquivo ISF é uma imagem GIF com metadados adicionais para todos os comportamentos e propriedades de traço de tinta. Aplicativos que não são habilitados para tinta podem exibir a imagem GIF estática, incluindo transparência de plano de fundo de canal alfa.
title: Armazenar e recuperar dados de traço do Windows Ink
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, escrita à tinta do Windows, DirectInk, InkPresenter, InkCanvas, ISF, Ink Serialized Format, interação do usuário, entrada
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2919a2f61f3185d85b91bdf6fd6be22402eb77d0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258264"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>Armazenar e recuperar dados de traço do Windows Ink


Os aplicativos UWP que dão suporte ao Windows Ink podem serializar e desserializar traços de tinta para um arquivo Ink Serialized Format (ISF). O arquivo ISF é uma imagem GIF com metadados adicionais para todos os comportamentos e propriedades de traço de tinta. Aplicativos que não são habilitados para tinta podem exibir a imagem GIF estática, incluindo transparência de plano de fundo de canal alfa.

> [!NOTE]
> O ISF é a representação mais compacta e persistente de tinta. Ele pode ser inserido em um formato de documento binário, como um arquivo GIF, ou colocado diretamente na Área de Transferência.

> **APIs importantes**: [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows.UI.Input.Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="save-ink-strokes-to-a-file"></a>Salvar traços de tinta em um arquivo

Aqui, demonstramos como salvar traços de tinta desenhados em um controle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

**Baixe este exemplo de [salvar e carregar traços de tinta de um arquivo de formato serializado da tinta (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  Primeiro, definimos a interface do usuário.

    A interface do usuário inclui os botões "Salvar", "Carregar", "Limpar", bem como [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    [  **InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)), e os ouvintes dos eventos de clique nos botões são declarados.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Por fim, salvamos os dados à tinta no manipulador de eventos de clique do botão **Salvar**.

    [  **FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) permite que o usuário selecione o arquivo e o local onde os dados à tinta serão salvos.

    Depois de selecionar um arquivo, abrimos um fluxo [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) definido como [**ReadWrite**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode).

    Em seguida, chamamos [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.iinkstrokecontainer.saveasync) para serializar os traços de tinta gerenciados por [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) para o fluxo.

```csharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn't be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

> [!NOTE]
> O formato GIF é o único formato de arquivo compatível para salvar dados à tinta. Entretanto, o método [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) (demonstrado na próxima seção) dá suporte a outros formatos para compatibilidade com versões anteriores.

## <a name="load-ink-strokes-from-a-file"></a>Carregar traços de tinta de um arquivo

Aqui, demonstramos como carregar traços de tinta de um arquivo e renderizá-los em um controle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

**Baixe este exemplo de [salvar e carregar traços de tinta de um arquivo de formato serializado da tinta (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  Primeiro, definimos a interface do usuário.

    A interface do usuário inclui os botões "Salvar", "Carregar", "Limpar", bem como [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    [  **InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)), e os ouvintes dos eventos de clique nos botões são declarados.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Por fim, carregamos os dados à tinta no manipulador de eventos de clique do botão **Carregar**.

    [  **FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) permite que o usuário selecione o arquivo e o local de onde recuperar os dados à tinta salvos.

    Depois de selecionar um arquivo, abrimos um fluxo [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) definido como [**Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode).

    Em seguida, chamamos [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) para ler, desserializar e carregar os traços de tinta salvos em [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Carregar os traços em **InkStrokeContainer** faz com que [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) os renderize imediatamente para [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

    > [!NOTE]
    > Todos os traços existentes em InkStrokeContainer são apagados antes de novos traços serem carregados.

``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

> [!NOTE]
> O formato GIF é o único formato de arquivo compatível para salvar dados à tinta. Entretanto, o método [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) oferece suporte aos formatos a seguir para compatibilidade com versões anteriores.

| Formato                    | Descrição |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | Especifica dados à tinta persistentes usando ISF. Essa é a representação mais compacta e persistente de tinta. Ela pode ser inserida em um formato de documento binário ou colocada diretamente na Área de Transferência.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | Especifica dados à tinta persistentes codificando o ISF como um fluxo base64. Esse formato é oferecido para que dados à tinta possam ser codificados diretamente em um arquivo XML ou HTML.                                                                                                                                                                                                                                                |
| Gif                       | Especifica dados à tinta persistentes usando um arquivo GIF que contém ISF como metadados inseridos no arquivo. Isso permite que dados a tinta sejam visualizados em aplicativos não habilitados para tinta e mantém toda a fidelidade da tinta quando retorna para um aplicativo habilitado para tinta. Esse formato é ideal para o transporte de conteúdo de tinta dentro de um arquivo HTML e para torná-lo utilizável por aplicativos com e sem tinta. |
| Base64Gif                 | Especifica dados a tinta persistentes usando um GIF fortificado codificado em base64. Esse formato é oferecido quando dados à tinta devem ser codificados diretamente em um arquivo XML ou HTML para conversão posterior em uma imagem. Um possível uso disso é em um formato XML gerado para conter todas as informações à tinta e usado para gerar HTML por meio de XSLT (Extensible Stylesheet Language Transformations). 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>Copiar e colar traços de tinta com a área de transferência

Aqui, demonstramos como usar a área de transferência para transferir os traços de tinta entre aplicativos.

Para dar suporte à funcionalidade de área de transferência, os comandos recortar e colar [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) internos precisam que um ou mais traços de tinta sejam selecionados.

Para este exemplo, habilitamos a seleção de traço quando a entrada é modificada com um botão da caneta (ou o botão direito do mouse). Para obter um exemplo completo de como implementar a seleção de traço, consulte Entrada de passagem para processamento avançado em [Interações por caneta](pen-and-stylus-interactions.md).

**Baixe este exemplo de [salvar e carregar traços de tinta da área de transferência](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)**

1.  Primeiro, definimos a interface do usuário.

    A interface do usuário inclui os botões "Recortar", "Copiar" e "Limpar", juntamente com [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e uma tela de seleção.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    O [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Os ouvintes dos eventos de clique nos botões, bem como os eventos de ponteiro e de traço para a funcionalidade de seleção, também são declarados aqui.

    Para obter um exemplo completo de como implementar a seleção de traço, consulte Entrada de passagem para processamento avançado em [Interações por caneta](pen-and-stylus-interactions.md).
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased +=
            InkPresenter_StrokesErased;
    }
```

3.  Por fim, depois de adicionar suporte à seleção de traço, implementamos a funcionalidade de área de transferência nos manipuladores de eventos de clique nos botões **Recortar**, **Copiar** e **Colar**.

    Para recortar, chamamos primeiro [**CopySelectedToClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) em [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) de [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter).

    Em seguida, chamamos [**DeleteSelected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.deleteselected) para remover os traços da tela de tinta.

    Por fim, excluímos todos os traços de seleção da tela de seleção.
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```csharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

Para copiar, basta chamar [**CopySelectedToClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) no [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) do [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter).


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

Para colar, chamamos [**CanPasteFromClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.canpastefromclipboard) para garantir que o conteúdo na área de transferência pode ser colado na tela de tinta.

Nesse caso, chamamos [**PasteFromClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.pastefromclipboard) para inserir os traços de tinta da área de transferência no [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) do [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter), que renderiza os traços na tela de tinta.

```csharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <a name="related-articles"></a>Artigos relacionados

* [Interações por caneta](pen-and-stylus-interactions.md)

**Exemplos de tópico**
* [Salvar e carregar traços de tinta de um arquivo de formato serializado da tinta (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Salvar e carregar traços de tinta da área de transferência](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**Outros exemplos**
* [Exemplo de tinta simplesC#(C++/)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Amostra de tinta complexaC++()](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Exemplo de tinta (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de introdução: suporte à tinta em seu aplicativo UWP](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Exemplo de livro de cores](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Exemplo de notas da família](https://github.com/Microsoft/Windows-appsample-familynotes)




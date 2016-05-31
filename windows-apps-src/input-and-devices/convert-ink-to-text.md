---
author: Karl-Bridge-Microsoft
Description: Converta traços de tinta em texto usando o reconhecimento de manuscrito, ou em formas usando o reconhecimento personalizado.
title: Reconhecer traços do Windows Ink como texto
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, handwriting recognition
---

# Reconhecer traços do Windows Ink como texto

Converta traços de tinta em texto usando o reconhecimento de manuscrito, ou em formas usando o reconhecimento personalizado.

**APIs importantes**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


O reconhecimento de manuscrito está integrado à plataforma de tinta do Windows e oferece suporte a um amplo conjunto de localidades e idiomas.

Para todos os exemplos aqui, adicione as referências de namespace necessárias para a funcionalidade de tinta. Isso inclui "Windows.UI.Input.Inking".

## <span id="Basic_handwriting_recognition"></span><span id="basic_handwriting_recognition"></span><span id="BASIC_HANDWRITING_RECOGNITION"></span>Reconhecimento de manuscrito básico


Aqui, demonstramos como usar o mecanismo de reconhecimento de manuscrito, associado ao pacote de idiomas instalado por padrão, para interpretar um conjunto de traços em [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

O reconhecimento é iniciado quando o usuário clica em um botão ao terminar de escrever.

1.  Primeiro, definimos a interface do usuário.

    A interface do usuário inclui um botão "Recognize", [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) e uma área para exibir os resultados de reconhecimento.    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink recognition sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas" 
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult" 
                       Grid.Row="1" 
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    O [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Os traços de tinta são renderizados em [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) usando [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados. Um ouvinte para o evento de clique no botão "Recognize" também é declarado.
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

3.  Por fim, executamos o reconhecimento de manuscrito básico. Para este exemplo, usamos o manipulador de eventos de clique do botão "Recognize" para executar o reconhecimento de manuscrito.

    Um [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) armazena todos os traços de tinta em um objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Os traços são expostos por meio da propriedade [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de **InkPresenter** e recuperados usando o método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object 
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer = 
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults = 
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer, 
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object 
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer = 
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults = 
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer, 
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <span id="International_recognition"></span><span id="international_recognition"></span><span id="INTERNATIONAL_RECOGNITION"></span>Reconhecimento internacional


Um subconjunto abrangente de idiomas com suporte no Windows pode ser usado para reconhecimento de manuscrito.

A tabela a seguir lista os idiomas compatíveis com [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478). A primeira coluna contém os valores possíveis para [**Name**](https://msdn.microsoft.com/library/windows/apps/br208484).

| Nome                                                            | Marca de idioma IETF | Cobertura                                                                      |
|-----------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------|
| Microsoft English (US) Handwriting Recognizer                   | en-US             | Inglês nos EUA e nas Filipinas                                       |
| Microsoft English (UK) Handwriting Recognizer                   | en-GB             | Inglês na Grã-Bretanha e em todos os outros locais não especificados nesta tabela |
| Microsoft English (Canada) Handwriting Recognizer               | en-CA             | Inglês no Canadá                                                             |
| Microsoft English (Australia) Handwriting Recognizer            | en-AU             | Inglês na Austrália                                                          |
| Microsoft-Handschrifterkennung - Deutsch                        | de-DE             | Alemão na Alemanha, na Áustria, em Luxemburgo e na Namíbia                           |
| Microsoft-Handschrifterkennung - Deutsch (Schweiz)              | de-CH             | Alemão na Suíça e em Liechtenstein                                       |
| Reconocimiento de escritura a mano en español de Microsoft      | es-ES             | Espanhol na Espanha e em todos os outros locais não especificados nesta tabela         |
| Reconocedor de escritura en Español (México) de Microsoft       | es-MX             | Espanhol no México e nos Estados Unidos                                       |
| Reconocedor de escritura en Español (Argentina) de Microsoft    | es-AR             | Espanhol na Argentina, no Paraguai e no Uruguai                                   |
| Reconnaissance d'écriture Microsoft - Français                  | fr                | Francês na França, no Canadá, na Bélgica, na Suíça e em todos os outros locais       |
| Microsoft 日本語手書き認識エンジン                              | ja                | Japonês em todos os locais                                                     |
| Riconoscimento grafia italiana Microsoft                        | it                | Italiano na Itália, na Suíça e em todos os outros locais                        |
| Microsoft Nederlandstalige handschriftherkenning                | nl-NL             | Holandês nos Países Baixos, no Suriname e nas Antilhas                           |
| Microsoft Nederlandstalige (België) handschriftherkenning       | nl-BE             | Holandês na Bélgica (Flamengo)                                                    |
| Microsoft 中文(简体)手写识别器                                  | zh                | Chinês em script simplificado                                                  |
| Microsoft 中文(繁體)手寫辨識器                                  | zh-hant           | Chinês em script tradicional                                                 |
| Microsoft система распознавания русского рукописного ввода      | ru                | Russo em todos os locais                                                      |
| Reconhecedor de Manuscrito da Microsoft para Português (Brasil) | pt-BR             | Português no Brasil                                                          |
| Reconhecedor de escrita manual da Microsoft para português      | pt-PT             | Português em Portugal e em todos os outros locais                               |
| Microsoft 한글 필기 인식기                                      | ko                | Coreano em todos os locais                                                       |
| System rozpoznawania polskiego pisma odręcznego firmy Microsoft | pl                | Polonês em todos os locais                                                       |
| Microsoft Handskriftstolk för svenska                           | sv                | Sueco em todos os locais                                                      |
| Microsoft rozpoznávač rukopisu pro český jazyk                  | cs                | Tcheco em todos os locais                                                        |
| Microsoft Genkendelse af dansk håndskrift                       | da                | Dinamarquês em todos os locais                                                       |
| Microsoft Håndskriftsgjenkjenner for norsk                      | nb                | Norueguês (Bokmal) em todos os locais                                           |
| Microsoft Håndskriftsgjenkjenner for nynorsk                    | nn                | Norueguês (Nynorsk) em todos os locais                                          |
| Microsoftin suomenkielinen käsinkirjoituksen tunnistus          | fi                | Finlandês em todos os locais                                                      |
| Microsoft recunoaştere grafie - Română                          | ro                | Romeno em todos os locais                                                     |
| Microsoftov hrvatski rukopisni prepoznavač                      | hr                | Croata em todos os locais                                                     |
| Microsoft prepoznavač rukopisa za srpski (latinica)             | sr-Latn           | Sérvio, em latim, em todos os locais                                           |
| Microsoft препознавач рукописа за српски (ћирилица)             | sr                | Sérvio, em cirílico, em todos os locais                                        |
| Reconeixedor d'escriptura manual en català de Microsoft         | ca                | Catalão em todos os locais                                                      |

 

Seu aplicativo pode consultar o conjunto de mecanismos de reconhecimento de manuscrito instalados e usar um deles ou permitir que o usuário selecione o idioma preferencial.

**Observação**  
Os usuários podem ver uma lista de idiomas instalados em Configurações -&gt; Hora e Idioma. Os idiomas instalados estão listados em Idiomas.

Para instalar novos pacotes de idiomas e habilitar o reconhecimento de manuscrito para o idioma:

1.  Vá para **Configurações &gt; Hora e Idioma &gt; Região e Idioma**.
2.  Selecione **Adicionar um idioma**.
3.  Selecione um idioma na lista e escolha a versão da região. O idioma agora é listado na página **Região e Idioma**.
4.  Clique no idioma e selecione **Opções**.
5.  Na página **Opções de Idioma**, baixe o **Handwriting recognition engine** (também é possível baixar o pacote de idiomas completo, o mecanismo de reconhecimento de fala e o layout do teclado aqui).

 

Aqui, demonstramos como usar o mecanismo de reconhecimento de manuscrito para interpretar um conjunto de traços em [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) com base no reconhecedor selecionado.

O reconhecimento é iniciado quando o usuário clica em um botão ao terminar de escrever.

1.  Primeiro, definimos a interface do usuário.

    A interface do usuário inclui um botão "Recognize", uma caixa de combinação que lista todos os reconhecedores de manuscrito instalados, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) e uma área para exibir resultados de reconhecimento.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Advanced international ink recognition sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers" 
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize" 
                    Content="Recognize" 
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas" 
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult" 
                       Grid.Row="1" 
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    O [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Os traços de tinta são renderizados em [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) usando [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados.

    Chamamos uma função `InitializeRecognizerList` para popular a caixa de combinação do reconhecedor com uma lista de reconhecedores de manuscrito instalados.

    Também declaramos ouvintes para o evento de clique no botão "Recognize" e o evento de alteração de seleção na caixa de combinação do reconhecedor.
```    CSharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged += 
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  Populamos a caixa de combinação do reconhecedor com uma lista de reconhecedores de manuscrito instalados.

    Um [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) é criado para gerenciar o processo de reconhecimento de manuscrito. Use esse objeto para chamar [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480) e recuperar a lista de reconhecedores instalados para popular a caixa de combinação do reconhecedor.
```    CSharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers = 
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  Atualize o reconhecedor de manuscrito se a seleção da caixa de combinação do reconhecedor for alterada.

    Use [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) para chamar [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328) com base no reconhecedor selecionado na caixa de combinação do reconhecedor.
```    CSharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  Por fim, executamos o reconhecimento de manuscrito com base no reconhecedor de manuscrito selecionado. Para este exemplo, usamos o manipulador de eventos de clique do botão "Recognize" para executar o reconhecimento de manuscrito.

    Um [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) armazena todos os traços de tinta em um objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Os traços são expostos por meio da propriedade [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de **InkPresenter** e recuperados usando o método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = 
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = 
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = 
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = 
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = 
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <span id="Dynamic_handwriting_recognition"></span><span id="dynamic_handwriting_recognition"></span><span id="DYNAMIC_HANDWRITING_RECOGNITION"></span>Reconhecimento de manuscrito dinâmico


Os dois exemplos anteriores exigem que o usuário pressione um botão para iniciar o reconhecimento. Seu aplicativo também pode executar reconhecimento dinâmico usando entrada de traço em conjunto com uma função de tempo básica.

Para este exemplo, usaremos a mesma interface do usuário e as configurações de traço que o exemplo anterior de reconhecimento internacional.

1.  Assim como os exemplos anteriores, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)), e traços de tinta são renderizados em [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) usando [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados.

    Em vez de um botão para iniciar o reconhecimento, adicionamos ouvintes de dois eventos de traço [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) e [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) e configuramos um temporizador básico ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) com um intervalo [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) de um segundo.    
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged += 
            comboInstalledRecognizers_SelectionChanged;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += 
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  Aqui estão os manipuladores para os três eventos que adicionamos na primeira etapa.

    <span id="StrokesCollected"></span><span id="strokescollected"></span><span id="STROKESCOLLECTED"></span>[**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Inicia o temporizador de reconhecimento quando o usuário parar de escrever à tinta ao levantar a caneta ou o dedo, ou liberar o botão do mouse. Depois de um segundo sem nenhuma entrada à tinta, o reconhecimento é iniciado.

    <span id="StrokeStarted"></span><span id="strokestarted"></span><span id="STROKESTARTED"></span>[**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Se um novo traço for iniciado antes do próximo evento de tique do temporizador, interrompa o temporizador, pois o novo traço é provavelmente a continuação de uma única entrada de manuscrito.

    <span id="Tick"></span><span id="tick"></span><span id="TICK"></span>[**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    Chama a função de reconhecimento após um segundo sem nenhuma entrada à tinta.
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  Por fim, executamos o reconhecimento de manuscrito com base no reconhecedor de manuscrito selecionado. Para este exemplo, usamos o manipulador de eventos [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) de [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) para iniciar o reconhecimento de manuscrito.

    Um [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) armazena todos os traços de tinta em um objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Os traços são expostos por meio da propriedade [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de **InkPresenter** e recuperados usando o método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## <span id="related_topics"></span>Artigos relacionados

* [Interações com caneta](pen-and-stylus-interactions.md)

**Exemplos**
* [Amostra de tinta](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Amostra de tinta simples](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Amostra de tinta complexa](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 






<!--HONumber=May16_HO2-->



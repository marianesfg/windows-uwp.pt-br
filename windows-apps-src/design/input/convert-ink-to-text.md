---
Description: Use análise de tinta e de reconhecimento de manuscrito para reconhecer traços do Windows Ink como formas e texto.
title: Reconhecer traços do Windows Ink como texto e formas
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, escrita à tinta do Windows, DirectInk, InkPresenter, InkCanvas, reconhecimento de manuscrito, interação do usuário, entrada
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d496051a066ffcf9e8df5d4798415e6089cad4d1
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970911"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Reconhecer traços do Windows Ink como texto e formas

Converta traços de tinta em texto e formas usando os recursos de reconhecimento integrados ao Windows Ink.

> **APIs importantes**: [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows.UI.Input.Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="free-form-recognition-with-ink-analysis"></a>Reconhecimento de forma livre com análise de tinta

Aqui, demonstramos como usar o mecanismo de análise do Windows Ink ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) para classificar, analisar e reconhecer um conjunto de traços de forma livre em um [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) como texto ou formas. (Além do reconhecimento de texto e forma, a análise de tinta também pode ser usada para reconhecer a estrutura do documento, listas com marcadores e desenhos genéricos.)

> [!NOTE]
> Para cenários básicos, de uma única linha e textos sem formatação, como a entrada de formulário, consulte a seção [Reconhecimento de manuscrito restrito](#constrained-handwriting-recognition) neste tópico.

Neste exemplo, o reconhecimento é iniciado quando o usuário clica em um botão para indicar que terminou de desenhar.

**Baixar este exemplo do [exemplo de análise de tinta (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1. Primeiro, configuramos a interface do usuário (MainPage.xaml). 

   A interface do usuário inclui um botão "Reconhecer", um [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e um [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) padrão. Quando o botão "Reconhecer" é pressionado, todos os traços de tinta na tela de tinta são analisados e (se reconhecido) as formas e o texto correspondentes são desenhados na tela padrão. Os traços de tinta originais são então excluídos da tela de tinta.

   ```xaml
   <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
   </Grid>
   ```

2. No arquivo UI code-behind (MainPage.xaml.cs), adicione as referências de tipo de namespace necessárias para a nossa funcionalidade de análise de tinta e tinta:
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows. UI. Input. teleking. Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. Em seguida, podemos especificar nossas variáveis globais:

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. Em seguida, definimos alguns comportamentos básicos de entrada à tinta:
    - O [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada de caneta, mouse e toque como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). 
    - Os traços de tinta são renderizados em [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) usando [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class) especificados. 
    - Um ouvinte para o evento de clique no botão "Reconhecer" também é declarado.

    ```csharp
    /// <summary>
    /// Initialize the UI page.
    /// </summary>
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
    ```

5. Para este exemplo, realizamos a análise de tinta no manipulador de eventos de clique do botão "Reconhecer".
    - Primeiro, chame [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) no [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) do [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) para obter a coleção de todos os traços de tinta atual.
    - Se os traços de tinta estiverem presentes, passe-os em uma chamada para [**AddDataForStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) do InkAnalyzer.
    - Tentamos reconhecer desenhos e texto, mas você pode usar o método [**SetStrokeDataKind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar se está interessado apenas em texto (incluindo a estrutura do documento e as listas com marcadores) ou apenas em desenhos (incluindo o reconhecimento de formas).
    - Chame [**AnalyzeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar a análise de tinta e obter o [**InkAnalysisResult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Se [**Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) retornar um estado de **Updated**, chame [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) para ambos [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) e [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itere através de ambos os conjuntos de tipos de nó e desenhe o respectivo texto ou forma na tela de reconhecimento (abaixo da tela de tinta).
    - Por fim, exclua os nós reconhecidos do InkAnalyzer e os traços de tinta correspondentes da tela de tinta.

    ```csharp
    /// <summary>
    /// The "Analyze" button click handler.
    /// Ink recognition is performed here.
    /// </summary>
    /// <param name="sender">Source of the click event</param>
    /// <param name="e">Event args for the button click routed event</param>
    private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
    ```

6. Aqui está a função para desenhar um TextBlock na nossa tela de reconhecimento. Usamos o retângulo delimitador do traço de tinta associado na tela de tinta para definir a posição e o tamanho da fonte do TextBlock.

   ```csharp
    /// <summary>
    /// Draw ink recognition text string on the recognitionCanvas.
    /// </summary>
    /// <param name="recognizedText">The string returned by text recognition.</param>
    /// <param name="boundingRect">The bounding rect of the original ink writing.</param>
    private void DrawText(string recognizedText, Rect boundingRect)
    {
        TextBlock text = new TextBlock();
        Canvas.SetTop(text, boundingRect.Top);
        Canvas.SetLeft(text, boundingRect.Left);
    
        text.Text = recognizedText;
        text.FontSize = boundingRect.Height;
    
        recognitionCanvas.Children.Add(text);
    }
   ```

7. Aqui estão as funções para desenhar elipses e polígonos em nossa tela de reconhecimento. Usamos o retângulo delimitador do traço de tinta associado na tela de tinta para definir a posição e o tamanho da fonte das formas.

   ```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
   ```

Aqui está essa amostra em ação:

| Antes da análise | Depois da análise |
| --- | --- |
| ![Antes da análise](images/ink/ink-analysis-raw2-small.png) | ![Depois da análise](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>Reconhecimento de manuscrito restrito

Na seção anterior ([Reconhecimento de forma livre com análise de tinta](#free-form-recognition-with-ink-analysis)), demonstramos como usar as [APIs de análise de tinta](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis) para analisar e reconhecer traços de tinta arbitrários dentro da área de InkCanvas.

Nesta seção, demonstramos como usar o mecanismo de reconhecimento de manuscrito Windows Ink (não é a análise de tinta), para converter um conjunto de traços de um [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) em texto (com base no pacote de idiomas padrão instalado).

> [!NOTE]
> O reconhecimento básico de manuscrito mostrado nesta seção é mais adequado para cenários de entrada de texto de linha única, como a entrada de formulário. Para cenários de reconhecimento mais ricos que incluem análise e interpretação da estrutura do documento, itens de lista, formas e desenhos (além do reconhecimento de texto), veja a seção anterior: [Reconhecimento de forma livre com análise de tinta](#free-form-recognition-with-ink-analysis).

Neste exemplo, o reconhecimento é iniciado quando o usuário clica em um botão para indicar que terminou de escrever.

**Baixe este exemplo do [exemplo de reconhecimento de manuscrito de tinta](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1. Primeiro, configuramos a interface do usuário.

   A interface do usuário inclui um botão "Reconhecer", o [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e uma área para exibir os resultados de reconhecimento.    

   ```xaml
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

2. Para este exemplo, você precisa primeiro adicionar as referências de tipo de namespace necessárias para nossa funcionalidade de tinta:
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)

3. Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

    O [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Os traços de tinta são renderizados em [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) usando [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class) especificados. Um ouvinte para o evento de clique no botão "Reconhecer" também é declarado.

    ```csharp
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

4. Por fim, executamos o reconhecimento de manuscrito básico. Para este exemplo, usamos o manipulador de eventos de clique do botão "Recognize" para executar o reconhecimento de manuscrito.

   - Um [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) armazena todos os traços de tinta em um objeto [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Os traços são expostos por meio da propriedade [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) de **InkPresenter** e recuperados usando o método [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes). 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Um [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) é criado para gerenciar o processo de reconhecimento de manuscrito.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) é chamado para recuperar um conjunto de objetos [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) . Os resultados de reconhecimento são produzidos para cada palavra detectada por um [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - Cada objeto [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contém um conjunto de candidatos a texto. O item superior dessa lista é considerado o mecanismo de reconhecimento para ser a melhor correspondência, seguido pelos candidatos restantes, em ordem de redução da confiança.

       Iteramos por cada [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) e compilamos a lista de candidatos. Os candidatos são exibidos e o [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) é limpo (o que também limpa o [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
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

    - Aqui está o exemplo do manipulador de cliques, por completo.

    ```csharp
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

## <a name="international-recognition"></a>Reconhecimento internacional

O reconhecimento de manuscrito integrado à plataforma de tinta do Windows inclui um amplo subconjunto de localidades e idiomas com suporte no Windows.

Consulte o tópico da propriedade [**InkRecognizer.Name**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizer.name) parar obter uma lista de idiomas compatíveis com a [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

Seu aplicativo pode consultar o conjunto de mecanismos de reconhecimento de manuscrito instalados e usar um deles ou permitir que o usuário selecione o idioma que preferir.

**Observação**    os usuários podem ver uma lista de idiomas instalados acessando **configurações&gt; -hora & idioma**. Os idiomas instalados estão listados em **Idiomas**.

Para instalar novos pacotes de idiomas e habilitar o reconhecimento de manuscrito para o idioma:

1. Vá para **Configurações &gt; Hora e Idioma &gt; Região e Idioma**.
2. Selecione **Adicionar um idioma**.
3. Selecione um idioma na lista e escolha a versão da região. O idioma agora é listado na página **Região e Idioma**.
4. Clique no idioma e selecione **Opções**.
5. Na página **Opções de Idioma**, baixe o **Handwriting recognition engine** (também é possível baixar o pacote de idiomas completo, o mecanismo de reconhecimento de fala e o layout do teclado aqui).

Aqui, demonstramos como usar o mecanismo de reconhecimento de manuscrito para interpretar um conjunto de traços em [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) com base no reconhecedor selecionado.

O reconhecimento é iniciado quando o usuário clica em um botão ao terminar de escrever.

1. Primeiro, configuramos a interface do usuário.

   A interface do usuário inclui um botão "Recognize", uma caixa de combinação que lista todos os reconhecedores de manuscrito instalados, [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) e uma área para exibir resultados de reconhecimento.

    ```xaml
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

2. Em seguida, definimos alguns comportamentos básicos de entrada à tinta.

   O [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar dados de entrada da caneta e do mouse como traços de tinta ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Os traços de tinta são renderizados em [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) usando [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class) especificados.

   Chamamos uma função `InitializeRecognizerList` para popular a caixa de combinação do reconhecedor com uma lista de reconhecedores de manuscrito instalados.

   Também declaramos ouvintes para o evento de clique no botão "Recognize" e o evento de alteração de seleção na caixa de combinação do reconhecedor.

    ```csharp
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

3. Populamos a caixa de combinação do reconhecedor com uma lista de reconhecedores de manuscrito instalados.

   Um [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) é criado para gerenciar o processo de reconhecimento de manuscrito. Use esse objeto para chamar [**GetRecognizers**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers) e recuperar a lista de reconhecedores instalados para popular a caixa de combinação do reconhecedor.

    ```csharp
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
    
4. Atualize o reconhecedor de manuscrito se a seleção da caixa de combinação do reconhecedor for alterada.

   Use [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) para chamar [**SetDefaultRecognizer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer) com base no reconhecedor selecionado na caixa de combinação do reconhecedor.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. Por fim, executamos o reconhecimento de manuscrito com base no reconhecedor de manuscrito selecionado. Para este exemplo, usamos o manipulador de eventos de clique do botão "Recognize" para executar o reconhecimento de manuscrito.

   - Um [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) armazena todos os traços de tinta em um objeto [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Os traços são expostos por meio da propriedade [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) de **InkPresenter** e recuperados usando o método [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes).

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) é chamado para recuperar um conjunto de objetos [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) .

      Os resultados de reconhecimento são produzidos para cada palavra detectada por um [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - Cada objeto [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contém um conjunto de candidatos a texto. O item superior dessa lista é considerado o mecanismo de reconhecimento para ser a melhor correspondência, seguido pelos candidatos restantes, em ordem de redução da confiança.

       Iteramos por cada [**InkRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) e compilamos a lista de candidatos. Os candidatos são exibidos e o [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) é limpo (o que também limpa o [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
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

    - Aqui está o exemplo do manipulador de cliques, por completo.

    ```csharp
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

## <a name="dynamic-recognition"></a>Reconhecimento dinâmico

Embora os dois exemplos anteriores exijam que o usuário pressione um botão para iniciar o reconhecimento, você também pode executar o reconhecimento dinâmico usando uma entrada de traço em conjunto com uma função de tempo básica.

Para este exemplo, usaremos as mesmas configurações de traço e interface do usuário que o exemplo de reconhecimento internacional anterior.

1. Estes objetos globais ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) são usados em todo o nosso aplicativo.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. Em vez de um botão para iniciar o reconhecimento, adicionamos ouvintes de dois eventos de traço [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ([**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) e [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)) e configuramos um temporizador básico ([**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)) com um intervalo [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick) de um segundo.

    ```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
    ```

3. Em seguida, definimos os manipuladores para os eventos InkPresenter que declaramos na primeira etapa (também substituímos o evento de página [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) para gerenciar nosso temporizador).

    - [**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    Adicione traços de tinta ([**AddDataForStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) ao InkAnalyzer e inicie o temporizador de reconhecimento quando o usuário parar de escrever à tinta (levantando a caneta ou o dedo, ou soltando o botão do mouse). Depois de um segundo sem nenhuma entrada à tinta, o reconhecimento é iniciado.  

        Use o método [**SetStrokeDataKind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar se você está interessado apenas em texto (incluindo a estrutura do documento e as listas com marcadores) ou apenas em desenhos (incluindo o reconhecimento de formas).

    - [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
    Se um novo traço for iniciado antes do próximo evento de tique do temporizador, interrompa o temporizador, pois o novo traço é provavelmente a continuação de uma única entrada de manuscrito.

    ```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    }
    ```

4. Por fim, executamos o reconhecimento de manuscrito. Para este exemplo, usamos o manipulador de eventos [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick) de [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) para iniciar o reconhecimento de manuscrito.
    - Chame [**AnalyzeAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar a análise de tinta e obter o [**InkAnalysisResult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Se [**Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) retornar um estado de **Updated**, chame [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) para tipos de nó de  [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itere pelos nós e exiba o texto reconhecido.
    - Por fim, exclua os nós reconhecidos do InkAnalyzer e os traços de tinta correspondentes da tela de tinta.

    ```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
    ```

## <a name="related-articles"></a>Artigos relacionados

- [Interações por caneta](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Amostras de tópico

- [Exemplo de análise de tinta (básico) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [Exemplo de reconhecimento de manuscrito à tinta (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>Outras amostras

- [Exemplo de tinta simples (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Amostra de tinta complexa (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Exemplo de tinta (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Tutorial de introdução: suporte à tinta em seu aplicativo do Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Exemplo de livro de colorir](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Exemplo de anotações da família](https://github.com/Microsoft/Windows-appsample-familynotes)

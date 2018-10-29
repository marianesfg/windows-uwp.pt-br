---
author: Karl-Bridge-Microsoft
Description: Build Universal Windows Platform (UWP) apps that support custom interactions from pen and stylus devices, including digital ink for natural writing and drawing experiences.
title: Interações por caneta e Windows Ink em aplicativos UWP
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, escrita à tinta do Windows, DirectInk, InkPresenter, InkCanvas, reconhecimento de manuscrito, interação do usuário, entrada
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3d04ea3506fc909b115ba9aab397ded9e4464479
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5750318"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Interações com caneta e Windows Ink em apps UWP

![Caneta Surface](images/ink/hero-small.png)  
*Caneta Surface* (disponível para compra na [Microsoft Store](https://aka.ms/purchasesurfacepen)).

## <a name="overview"></a>Visão geral

Otimize seu aplicativo da Plataforma Universal do Windows (UWP) para entrada de caneta para fornecer o recurso padrão [**dispositivo de ponteiro**](https://msdn.microsoft.com/library/windows/apps/br225633) e a melhor experiência com o Windows Ink para seus usuários.

> [!NOTE]
> Este tópico se concentra na plataforma Windows Ink. Para manipulação de entrada de ponteiro geral (semelhante ao mouse, toque e touchpad), consulte [Identificar entrada do ponteiro](handle-pointer-input.md).

| Vídeos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Usando escrita à tinta em seu aplicativo UWP* | *Usar Windows caneta e tinta para criar enterpriseapps mais envolvente* |

A plataforma Windows Ink, juntamente com um dispositivo de caneta, oferece uma maneira natural de criar anotações manuscritas digitais, desenhos e anotações. A plataforma dá suporte à captura de dados de tinta por entrada de digitalizador, gerenciamento e geração de dados de tinta, renderização desses dados como traços de tinta no dispositivo de saída e conversão de tinta em texto por meio do reconhecimento de manuscrito.

Além de capturar os movimentos e posições básicos da caneta enquanto o usuário escreve ou desenha, seu aplicativo também pode rastrear e coletar os diferentes valores de pressão usados em um traço. Estas informações, juntamente com configurações de formato da ponta da caneta, tamanho, rotação, cor da tinta, rotação e função (tinta simples, apagar, destacar e selecionar) permitem que você ofereça experiências ao usuário que se aproximam do ato de escrever ou desenhar em papel com caneta, lápis ou pincel.

> [!NOTE]
> Seu aplicativo também pode ser compatível com a entrada à tinta de outros dispositivos baseados em ponteiro, inclusive digitalizadores touch e mouses. 

A plataforma de tinta é muito flexível. Ele é projetado para dar suporte a vários níveis de funcionalidade, dependendo dos seus requisitos.

Para obter diretrizes sobre a experiência do usuário com o Windows Ink, consulte [Controles de escrita à tinta](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Componentes da plataforma Windows Ink

| Componente | Descrição |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | Um controle de plataforma XAMLUI que, por padrão, recebe e exibe todas as entradas de uma caneta como um traço de tinta ou um traço para apagar.<br/>Para obter mais informações sobre como usar o InkCanvas, consulte [Reconhecer traços do Windows Ink como texto](convert-ink-to-text.md) e [Armazenar e recuperar dados de traço do Windows Ink](save-and-load-ink.md). |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Objeto code-behind, instanciado juntamente com um controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposto por meio da propriedade [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)). Esse objeto fornece todas as funcionalidades de escrita à tinta expostas pelo **InkCanvas**, juntamente com um conjunto que compreende APIs de customização e personalização adicional.<br/>Para obter mais informações sobre como usar o InkPresenter, consulte [Reconhecer traços do Windows Ink como texto](convert-ink-to-text.md) e [Armazenar e recuperar dados de traço do Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Um controle de plataforma XAMLUI que contém uma coleção personalizável e extensível de botões que ativam recursos relacionados à tinta em um associado [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).<br/>Para obter mais informações sobre como usar o InkToolbar, consulte [Adicionar um InkToolbar a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP)](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) | Permite a renderização de traços de tinta para o contexto de dispositivo Direct2D designado de um aplicativo Universal do Windows, em vez do controle padrão [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535). Isso permite a personalização completa da experiência de escrita à tinta.<br/>Para obter mais informações, consulte o [Exemplo de tinta complexa](http://go.microsoft.com/fwlink/p/?LinkID=620314). |

## <a name="basic-inking-with-inkcanvas"></a>Escrita à tinta básica com InkCanvas

Para adicionar funcionalidade básica de escrita à tinta, basta colocar um controle de plataforma UWP [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) na página apropriada do aplicativo.

Por padrão, o [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) dá suporte à entrada de tinta apenas de canetas. A entrada é renderizada como um traço de tinta usando configurações padrão de cor e espessura (uma caneta esferográfica preta com espessura de 2 pixels) ou tratada como um traço de apagar (quando a entrada é da ponta de uma borracha ou da ponta da caneta modificada por um botão de apagar).

> [!NOTE]
> Se uma ponta de borracha ou o botão não estiver presente, o InkCanvas poderá ser configurado para processar a entrada da ponta da caneta como um traço para apagar.

Neste exemplo, um [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) sobrepõe uma imagem em segundo plano.

> [!NOTE]
> Um InkCanvas tem propriedades padrão [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) e [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) igual a zero, a menos que ele seja o filho de um elemento que dimensiona automaticamente seus elementos filhos. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Essa série de imagens mostra como entrada de caneta é renderizada por este controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

| ![InkCanvas em branco com uma imagem de plano de fundo](images/ink_basic_1_small.png) | ![InkCanvas com traços de tinta](images/ink_basic_2_small.png) | ![InkCanvas com um traço apagado](images/ink_basic_3_small.png) |
| --- | --- | ---|
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) em branco com uma imagem de plano de fundo. | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) com traços de tinta. | O [ **InkCanvas** ](https://msdn.microsoft.com/library/windows/apps/dn858535) com um traço apagado (observe como apagar funciona em um traço inteiro, não uma parte). |

A funcionalidade de escrita à tinta com suporte do controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) é fornecida por um objeto de código-behind chamado [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).

Para escrita à tinta básica, você não precisa se preocupar com o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011). Entretanto, para personalizar e configurar o comportamento de escrita à mão no [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), você deve acessar o seu objeto correspondente **InkPresenter**.

## <a name="basic-customization-with-inkpresenter"></a>Personalização básica com InkPresenter

Um objeto [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) é instanciado com cada controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

> [!NOTE]
> O [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) não pode ser instanciado diretamente. Em vez disso, ele é acessado pela propriedade [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) do [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535). 

Além de fornecer todos os comportamentos padrão de escrita à tinta do seu controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) correspondente, o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) fornece um conjunto que compreende APIs para personalização adicional de traços e gerenciamento mais refinado da entrada da tinta (padrão e modificada). Isso inclui propriedades de traços, tipos de dispositivos de entrada com suporte e se a entrada é processada pelo objeto ou passada para o app para processamento.

> [!NOTE]
> A entrada de tinta padrão (da ponta da caneta ou da borracha/botão) não é modificada com uma funcionalidade de hardware secundária, como um botão de caneta, botão direito do mouse ou mecanismo semelhante. 

Por padrão, há suporte à tinta somente para entrada de caneta. Aqui, configuramos o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) para interpretar dados de entrada da caneta e do mouse como traços de tinta. Também definimos alguns atributos iniciais de traço de tinta usados para renderizar traços para o [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

Para habilitar a escrita à tinta por mouse e toque, defina a propriedade [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) do [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) para a combinação de valores [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) que você deseja.

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
}
```

Atributos de traço de tinta podem ser definidos dinamicamente para acomodar as preferências do usuário ou requisitos do aplicativo.

Aqui, permitimos que um usuário escolha suas preferências em uma lista de cores de tinta.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Em seguida, manipulamos as alterações feitas na cor selecionada e atualizamos os atributos de traço de tinta de acordo.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Estas imagens mostram como a entrada de caneta é processada e personalizada pelo [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081).

| ![o inkcanvas com traços de tinta preta padrão](images/ink-basic-custom-1-small.png) | ![o inkcanvas com traços de tinta vermelha selecionados pelo usuário](images/ink-basic-custom-2-small.png) |
| --- | --- |
| O [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) com traços de tinta preta padrão. | O [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) com traços de tinta vermelha selecionados pelo usuário. | 

Para oferecer funcionalidades além de escrever à tinta e apagar, como seleção de traço, seu aplicativo deve identificar entrada específica para o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) passar por não processado por manipular pelo seu aplicativo.

## <a name="pass-through-input-for-advanced-processing"></a>Entrada de passagem para processamento avançado

Por padrão, o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) processa todas as entradas como um traço de tinta ou de apagar, incluindo entradas modificadas por uma funcionalidade secundária de hardware, como um botão de caneta, um botão direito do mouse ou semelhantes. No entanto, os usuários normalmente esperam algumas funcionalidades adicionais ou um comportamento modificado com essas funcionalidades secundárias.

Em alguns casos, talvez também seja necessário expor a funcionalidade adicional para canetas sem funcionalidades secundárias (funcionalidade geralmente não associada à ponta da caneta), outros tipos de dispositivos de entrada ou algum tipo de comportamento modificado, com base na seleção de um usuário na interface do usuário do seu aplicativo.

Para dar suporte a isso, o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) pode ser configurado para deixar uma entrada específica não processada. Essa entrada não processada é, então, passada pelo seu aplicativo para processamento.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Exemplo - Use uma entrada não processada para implementar a seleção de traço 

A plataforma Windows Ink não fornece suporte interno a ações que exigem entrada modificada, como a seleção de traço. Para dar suporte a recursos como esse, você deve fornecer uma solução personalizada em seus aplicativos. 

As seguintes etapas de exemplo de código (todo o código está nos arquivos MainPage.xaml e MainPage.xaml.cs) mostram como habilitar a seleção de traço quando a entrada é modificada com um botão de caneta (ou o botão direito do mouse).

1.  Primeiro, definimos a interface do usuário em MainPage.xaml.

    Aqui, adicionados uma tela (abaixo do [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)) para desenhar o traço de seleção. Usar uma camada separada para desenhar o traço de seleção deixa o **InkCanvas** e seu conteúdo intocado.

    ![o inkcanvas em branco com uma tela de seleção subjacente](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  Em MainPage.xaml.cs, declaramos algumas variáveis globais para manter como referência em aspectos da seleção da IU. Especificamente, a seleção de traço de Laço e o retângulo delimitador que destaca os traços selecionados.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  Em seguida, configuramos o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) para interpretar dados de entrada tanto da caneta quanto do mouse como traços de tinta, e definimos alguns atributos iniciais de traço de tinta usados para renderizar traços para o [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

    E, o mais importante, usamos a propriedade [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) do [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081) para indicar que qualquer entrada modificada deve ser processada pelo aplicativo. A entrada modificada é especificada atribuindo a **InputProcessingConfiguration.RightDragAction** um valor de [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760). Quando esse valor é definido, o [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081) passa para a classe [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput), um conjunto de eventos de ponteiro para você manipular.

    Atribuímos ouvintes para os eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) e [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) não processados passados pelo [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). Todas as funcionalidades de seleção são implementadas nos manipuladores para esses eventos.

    Por fim, atribuímos ouvintes para os eventos [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) e [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) do [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). Usamos os manipuladores para esses limparem a seleção da interface do usuário se um novo traço for iniciado ou um traço existente for apagado.

    ![o inkcanvas com traços de tinta preta padrão](images/ink-unprocessed-2-small.png)

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

4.  Então, definimos manipuladores para os eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) e [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) não processados passados pelo [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081).

    Todas as funcionalidades de seleção são implementadas nesses manipuladores, incluindo o traço de Laço e o retângulo delimitador.

    ![o Laço de seleção](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  Para concluir o manipulador de eventos PointerReleased, podemos limpar a camada de seleção de todo o conteúdo (o traço de Laço) e desenhar um retângulo delimitador único ao redor dos traços de tinta circundados pela área de Laço.

    ![o retângulo delimitador de seleção](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  Por fim, definimos manipuladores para os eventos [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) e [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) do InkPresenter.

    Esses dois chamam a mesma função de limpeza para limpar a seleção atual sempre que um novo traço é detectado.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  Consulte a função para remover toda a interface do usuário de seleção da tela de seleção quando um novo traço for iniciado ou um traço existente for apagado.

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

## <a name="custom-ink-rendering"></a>Renderização de tinta personalizada

Por padrão, a entrada de tinta é processada em um thread de baixa latência em segundo plano e renderizada em andamento ou "molhada" conforme é desenhada. Quando o traço está concluído (caneta ou dedo param de pressionar ou botão do mouse é liberado), o traço é processado do thread de interface do usuário e renderizado como "seco" na camada [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (acima do conteúdo do aplicativo e substituindo a tinta molhada).

Você pode substituir esse comportamento padrão e controlar completamente a experiência de escrita à tinta "secando de forma personalizada" os traços de tinta molhada. Embora o comportamento padrão geralmente seja suficiente para a maioria dos aplicativos, há alguns casos em que a secagem personalizada pode ser necessária, isso inclui:
- Gerenciamento mais eficiente de coleções grandes ou complexas de traços de tinta
- Suporte mais eficiente ao movimento panorâmico e ao zoom em telas de tinta grandes
- Intercalação da escrita à tinta e de outros objetos, como formas ou texto, mantendo a ordem z 
- Secagem e conversão de tinta sincronicamente em uma forma DirectX (por exemplo, uma forma ou uma linha reta rasterizada e integrada ao conteúdo do aplicativo, e não a uma camada **InkCanvas** separada).

Secar de forma personalizada exige que um objeto [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) gerencie a entrada de tinta e a renderize para o contexto de dispositivo Direct2D do seu aplicativo Universal do Windows, em vez de o controle padrão [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

Chamar [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (antes de o [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) ser carregado) faz com que um aplicativo crie um objeto [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) para personalizar como um traço de tinta é renderizado como seco para um [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) ou [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource). 

[**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) e [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) fornecem uma superfície compartilhada do DirectX para o seu aplicativo desenhar e compor no seu conteúdo, embora o VSIS forneça uma superfície virtual maior do que a tela para obter movimento panorâmico e zoom eficientes. Como as atualizações visuais dessas superfícies são sincronizadas com o thread de interface do usuário XAML, quando a tinta é renderizada para qualquer uma delas, a tinta molhada pode ser removida do InkCanvas simultaneamente. 

Você também pode secar a tinta de forma personalizada para um [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel), mas não há garantia de sincronização com o thread de interface do usuário e pode haver um atraso entre o momento em que a tinta é renderizada para o seu SwapChainPanel e o momento em que ela é removida do InkCanvas.

Para ver um exemplo completo dessa funcionalidade, consulte [Amostra de tinta complexa](http://go.microsoft.com/fwlink/p/?LinkID=620314).

> [!NOTE]
> Secagem personalizada e o [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Se o aplicativo substituir o comportamento de renderização da tinta padrão do [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) por uma implementação de secagem personalizada, os traços da tinta renderizada não estarão mais disponíveis para o InkToolbar e os comandos de exclusão internos do InkToolbar não funcionarão conforme esperado. Para oferecer uma funcionalidade de exclusão, você deve lidar com todos os eventos de ponteiro, realizar o teste de clique em cada traço e substituir o comando "Apagar toda a tinta" interno.

## <a name="other-articles-in-this-section"></a>Outros artigos nesta seção

| Tópico | Descrição |
| --- | --- |
| [Reconhecer traços de tinta](convert-ink-to-text.md) | Converta traços de tinta em texto usando o reconhecimento de manuscrito, ou em formas usando o reconhecimento personalizado. |
| [Armazenar e recuperar traços de tinta](save-and-load-ink.md) | Armazene dados de traço de tinta em um arquivo Graphics Interchange Format (GIF) usando metadados incorporados Ink Serialized Format (ISF). |
| [Adicionar um InkToolbar a um aplicativo de escrita à tinta de UWP](ink-toolbar.md) | Adicione um InkToolbar padrão a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP), adicione um botão de caneta personalizada ao InkToolbar e vincule o botão de caneta personalizada a uma definição de caneta personalizada. |

## <a name="related-articles"></a>Artigos relacionados

* [Introdução: oferecer suporte à tinta em seu aplicativo UWP](../../get-started/ink-walkthrough.md)
* [Manusear entrada do ponteiro](handle-pointer-input.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)

**APIs**

* [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
* [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
* [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

**Exemplos**
* [Tutorial de Introdução: oferecer suporte à tinta em seu aplicativo UWP](https://aka.ms/appsample-ink)
* [Amostra de tinta simples (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Amostra de tinta complexa (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Amostra de tinta (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemplo de livro de colorir](https://aka.ms/cpubsample-coloringbook)
* [Exemplo de anotações da família](https://aka.ms/cpubsample-familynotessample)
* [Exemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo morto**
* [Entrada: amostra de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: gestos e interações com o GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)

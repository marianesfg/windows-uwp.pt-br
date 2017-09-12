---
author: Karl-Bridge-Microsoft
Description: "Adicione um InkToolbar padrão a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP), adicione um botão de caneta personalizada ao InkToolbar e vincule o botão de caneta personalizada a uma definição de caneta personalizada."
title: "Adicionar um InkToolbar a um app de escrita à tinta da Plataforma Universal do Windows (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) app
template: detail.hbs
keywords: "Windows Ink, escrita à tinta Windows, DirectInk, InkPresenter, InkCanvas, InkToolbar, Plataforma Universal do Windows, UWP, interação do usuário, entrada"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.openlocfilehash: a4bff46c2ab0f0f1f9a689f2744c9a77ac90630d
ms.sourcegitcommit: c519e3d34bef37f87bb44f02b295187849bb5eea
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2017
---
# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-app"></a>Adicionar um InkToolbar a um app de escrita à tinta da Plataforma Universal do Windows (UWP)
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">


Há dois controles diferentes que facilitam a escrita à tinta em aplicativos da Plataforma Universal do Windows (UWP): [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

O controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) fornece a funcionalidade básica do Windows Ink. Use-o para renderizar a entrada à caneta como traço de tinta (usando as configurações padrão de cor e espessura) ou como traço de apagar.

> Para obter detalhes sobre a implementação do InkCanvas, consulte [Interações com caneta em aplicativos UWP](pen-and-stylus-interactions.md).

Como uma sobreposição completamente transparente, o InkCanvas não fornece qualquer interface do usuário interna para definir propriedades de traço de tinta. Se você quiser alterar a experiência de escrita à tinta padrão, deixar que os usuários definam as propriedades de traço de tinta e dar suporte a outros recursos de escrita à tinta personalizados, há duas opções:

- No code-behind, use objeto [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) subjacente associado ao InkCanvas.

  As APIs do InkPresenter permitem dar suporte à personalização abrangente da experiência de escrita à tinta. Para obter mais detalhes, consulte [Interações com caneta em aplicativos UWP](pen-and-stylus-interactions.md).

- Associar um [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) ao InkCanvas. Por padrão, o InkToolbar fornece uma coleção de botões personalizável e extensível para ativar recursos relacionados à tinta, como tamanho do traço, cor da tinta e tipo de ponta da caneta.

  Vamos falar sobre o InkToolbar neste tópico.

<div class="important-apis" >
<b>APIs Importantes</b><br/>
<ul>
<li>[**Classe InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)</li>
<li>[**Classe InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)</li>
<li>[**Classe InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)</li>
<li>[**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)</li>
</ul>
</div>

## <a name="default-inktoolbar"></a>InkToolbar padrão

Por padrão, o [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) inclui botões para desenhar, apagar, realçar e exibir um estêncil (régua ou transferidor). Dependendo do recurso, outras configurações e comandos, como a cor da tinta, a espessura do traço, apagar toda a tinta, são fornecidos em um submenu.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*Barra de ferramentas do Windows Ink padrão*

Para adicionar um [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) padrão a um aplicativo de escrita à tinta, basta colocá-lo na mesma página que o [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) e associar os dois controles.

1. Em MainPage.xaml, declare um objeto de contêiner (para este exemplo, usamos um controle Grid) para a superfície de escrita à tinta.
2. Declare um objeto InkCanvas como filho do contêiner. (O tamanho do InkCanvas é herdado do contêiner).
3. Declare um InkToolbar e use o atributo TargetInkCanvas para associá-lo ao InkCanvas.
    > [!NOTE]  
    > Certifique-se de que o InkToolbar seja declarado após o InkCanvas. Caso contrário, a sobreposição do InkCanvas torna o InkToolbar inacessível.

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
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Personalização básica

Nesta seção, abordamos alguns cenários de personalização básica de barra de ferramentas do Windows Ink.

### <a name="specify-the-selected-button"></a>Especificar o botão selecionado  
![Botão de lápis selecionado na inicialização](.\images\ink\ink-tools-default-toolbar.png)  
*Barra de ferramentas do Windows Ink com botão de lápis selecionado na inicialização*

Por padrão, o primeiro botão (à esquerda) é selecionado quando seu aplicativo é iniciado e a barra de ferramentas é inicializada. Na barra de ferramentas do Windows Ink padrão, esse é o botão de caneta esferográfica.

Como a estrutura define a ordem dos botões internos, o primeiro botão pode não ser a caneta ou ferramenta que você deseja ativar por padrão.

Você pode substituir esse comportamento padrão e especificar o botão selecionado na barra de ferramentas.

Para este exemplo, inicializamos a barra de ferramentas padrão com o botão de lápis selecionado e o lápis ativado (em vez da caneta esferográfica).

1. Use a declaração XAML para o InkCanvas e InkToolbar do exemplo anterior.
2. No code-behind, defina um manipulador para o evento [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) do objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. No manipulador para o evento [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx):
  1. Obtenha uma referência para o [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) interno.

    Passar um objeto [InkToolbarTool.Pencil](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) no método [GetToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) retorna um objeto [InkToolbarToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) para o [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

  2. Defina [ActiveTool](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) para o objeto retornado na etapa anterior.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Especificar os botões internos

![Botões específicos incluídos na inicialização](.\images\ink\ink-tools-specific.png)  
*Botões específicos incluídos na inicialização*

Conforme mencionado, a barra de ferramentas do Windows Ink contém uma coleção de botões internos padrão. Esses botões são exibidos na seguinte ordem (da esquerda para a direita):

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

Para este exemplo, inicializamos a barra de ferramentas com apenas os botões internos de caneta esferográfica, lápis e borracha.

Você pode fazer isso usando XAML ou code-behind.

**XAML**

Modifique a declaração XAML para o InkCanvas e InkToolbar do primeiro exemplo.
- Adicione um atributo [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) e defina seu valor como "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)". Isso limpa a coleção padrão de botões internos.
- Adicione os botões InkToolbar específicos exigidos pelo seu aplicativo. Aqui, adicionamos somente [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
> [!NOTE]
> Os botões são adicionados à barra de ferramentas na ordem definida pela estrutura, não na ordem especificada aqui.

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
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**Code-behind**
1. Use a declaração XAML para o InkCanvas e InkToolbar do primeiro exemplo.

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
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. No code-behind, defina um manipulador para o evento [Loading](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) do objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Defina [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) como "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)".
4. Crie referências de objeto para os botões exigidos pelo seu aplicativo. Aqui, adicionamos somente [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
  > [!NOTE]
  > Os botões são adicionados à barra de ferramentas na ordem definida pela estrutura, não na ordem especificada aqui.

5. [Adicione](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) os botões ao InkToolbar.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Recursos de escrita à tinta e botões personalizados

Você pode personalizar e estender o conjunto de botões (e os recursos de escrita à tinta associados) que são fornecidos pelo InkToolbar.

O InkToolbar consiste em dois grupos distintos de tipos de botões:

1. Um grupo de botões de "ferramentas" que contém os botões internos para desenhar, apagar e realçar. Canetas e ferramentas personalizadas são adicionadas aqui.
> **Observação**&nbsp;&nbsp;A seleção de recursos é mutuamente exclusiva.

2. Um grupo de botões de "alternância" que contém o botão de régua interno. Alternâncias personalizadas são adicionadas aqui.
> **Observação**&nbsp;&nbsp;Os recursos não são mutuamente exclusivos e podem ser usados concomitantemente com outras ferramentas ativas.

Dependendo de seu aplicativo e da funcionalidade de escrita à tinta necessária, você pode adicionar qualquer um dos seguintes botões (associados aos seus recursos de tinta personalizados) ao InkToolbar:

- Caneta personalizada – uma caneta para a qual as propriedades de paleta de cores de tinta e ponta da caneta, como tamanho, rotação e forma, são definidas pelo aplicativo host.
- Ferramenta personalizada – uma ferramenta sem caneta, definida pelo aplicativo host.
- Alternância personalizada – define o estado de um recurso definido pelo aplicativo como ativado ou desativado. Quando ativado, o recurso funciona em conjunto com a ferramenta ativa.

> **Observação**&nbsp;&nbsp;Você não pode alterar a ordem de exibição dos botões internos. A ordem de exibição padrão é: caneta esferográfica, lápis, marca-texto, borracha e régua. Canetas personalizadas são acrescentadas à última caneta padrão, botões de ferramenta personalizados são adicionados entre o último botão de caneta e o botão de borracha e botões de alternância personalizados são adicionados após o botão de régua. (Os botões personalizados são adicionados na ordem em que são especificados.)

### <a name="custom-pen"></a>Caneta personalizada

Você pode criar uma caneta personalizada (ativada por meio de um botão da caneta personalizada) onde é possível definir a paleta de cores de tinta e propriedades de dica, como tamanho, rotação e forma de caneta.

![Botão de caneta caligráfica personalizado](.\images\ink\ink-tools-custompen.png)  
*Botão de caneta caligráfica personalizado*

Para este exemplo, definimos uma caneta personalizada com uma ponta ampla que permite traços de tinta caligráficos básicos. Também personalizamos a coleção de pincéis na paleta exibida no submenu do botão.

**Code-behind**

Primeiro, definimos nossa caneta personalizada e especificamos os atributos de desenho no code-behind. Vamos nos referir a essa caneta personalizada do XAML mais tarde.

1. Clique com o botão direito no projeto no Gerenciador de Soluções e selecione Adicionar -> Novo item.
2. Em Visual C# -> Código, adicione um novo arquivo de classe e nomeie-o como CalligraphicPen.cs.
3. Em Calligraphic.cs, substitua o padrão que usa o bloco pelo seguinte:
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Especifique que a classe CalligraphicPen é derivada de [InkToolbarCustomPen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Substitua [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) para especificar seu próprio tamanho de pincel e traço.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Crie um objeto [InkDrawingAttributes](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) e defina o [formato da ponta da caneta](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), a [rotação da ponta](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), o [tamanho do traço](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) e a [cor da tinta](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

Em seguida, adicionamos as referências necessárias à caneta personalizada em MainPage.xaml.

1. Declaramos um dicionário de recursos de página local que cria uma referência à caneta personalizada (`CalligraphicPen`) definida em CalligraphicPen.cs e uma [coleção de pincéis](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) com suporte da caneta personalizada (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. Em seguida, adicionamos um InkToolbar com um elemento [InkToolbarCustomPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) filho.

  O botão de caneta personalizada inclui as duas referências de recurso estático declaradas nos recursos da página: `CalligraphicPen` e `CalligraphicPenPalette`.

  Também podemos especificar o intervalo para o controle deslizante de tamanho de traço ([MinStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) e [SelectedStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), o pincel selecionado ([SelectedBrushIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) e o ícone do botão de caneta personalizada ([SymbolIcon](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Alternância personalizada

Você pode criar uma alternância personalizada (ativada por meio de um botão de alternância personalizada) para definir o estado de um recurso definido pelo aplicativo como ativado ou desativado. Quando ativado, o recurso funciona em conjunto com a ferramenta ativa.

Neste exemplo, definimos um botão de alternância personalizada que permite escrever à tinta com entrada por toque (por padrão, escrita à tinta por toque não está habilitada).

> [!NOTE]  
> Se você precisar oferecer suporte para escrita à tinta com touch, é recomendável ativá-la usando um CustomToggleButton, com o ícone e a dica de ferramenta especificados neste exemplo.

Normalmente, a entrada por toque é usada para manipulação direta de um objeto ou da interface do usuário do aplicativo. Para demonstrar as diferenças de comportamento quando a escrita à tinta por toque está habilitada, vamos colocar InkCanvas dentro de um contêiner ScrollViewer e definir as dimensões de ScrollViewer como menores do que as de InkCanvas. 

Quando o aplicativo é iniciado, somente a escrita com caneta é compatível e o toque é usado para obter panorâmica ou aplicar zoom à superfície de escrita. Quando a escrita à tinta por toque está habilitada, não é possível obter panorâmica ou aplicar zoom por meio de entrada por toque na superfície de escrita.

> [!NOTE]
> Consulte [Controles de escrita à tinta](..\controls-and-patterns\inking-controls.md) para as diretrizes de experiência do usuário [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar). As seguintes recomendações são relevantes para este exemplo:
> - O [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar), e a escrita à tinta em geral, oferece a melhor experiência com uma caneta ativa. No entanto, a escrita à tinta com mouse e toque pode ter suporte, se exigido por seu aplicativo. 
> - Para dar suporte à escrita à tinta com entrada por toque, recomendamos usar o ícone "ED5F" da fonte "Segoe MLD2 Assets" para o botão de alternância, com uma dica de ferramenta "Escrita por toque". 

**XAML**

1. Primeiro, nós declaramos um elemento [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) (toggleButton) com um ouvinte de eventos Click que especifica o manipulador de eventos (Toggle_Custom).

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**Code-behind**

2. No snippet anterior, declaramos um ouvinte de eventos Click e o manipulador (Toggle_Custom) no botão de alternância personalizada para escrita por toque (toggleButton). Esse manipulador simplesmente alterna o suporte para CoreInputDeviceTypes.Touch por meio da propriedade InputDeviceTypes de InkPresenter.

   Também especificamos um ícone para o botão usando o elemento SymbolIcon e a extensão de marcação {x:Bind} que o associa a um campo definido no arquivo code-behind (TouchWritingIcon).

   O snippet a seguir inclui o manipulador de eventos Click e a definição de TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Ferramenta personalizada

Você pode criar um botão de ferramenta personalizada para invocar uma ferramenta que não é de caneta definida por seu aplicativo.

Por padrão, o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) processa todas as entradas como traço de tinta ou de apagar. Isso inclui entrada modificada por funcionalidade secundária de hardware, como botão de caneta, botão direito do mouse e afins. No entanto, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) pode ser configurado para deixar uma entrada específica não processada, que pode ser transmitida por meio de seu aplicativo para processamento personalizado.

Neste exemplo, definimos um botão de ferramenta personalizada que, quando selecionado, faz com que os traços subsequentes sejam processados e renderizados como um laço de seleção (linha tracejada) em vez de tinta. Todos os traços de tinta dentro dos limites da área de seleção são definidos como [**Selecionado**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected).

> [!NOTE]
> Consulte Controles de escrita à tinta para as diretrizes de experiência do usuário InkCanvas e InkToolbar. A recomendação a seguir é relevante para este exemplo:
> - Se você fornecer seleção de traço, recomendamos usar o ícone "EF20" da fonte "Segoe MLD2 Assets" para o botão de ferramenta, com uma dica de ferramenta "Ferramenta de seleção". 
 
**XAML**

1. Primeiro, nós declaramos um elemento [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) (customToolButton) com um ouvinte de eventos Click que especifica o manipulador de eventos (customToolButton_Click) onde a seleção de traço está configurada. (Também adicionamos um conjunto de botões para copiar, recortar e colar a seleção de traço.)

2. Também adicionamos um elemento Canvas para desenhar o traço de seleção. Usar uma camada separada para desenhar o traço de seleção garante que [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) e seu conteúdo permaneçam intocados. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**Code-behind**

2. Em seguida, manipulamos o evento Click para o [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) no arquivo code-behind MainPage.xaml.cs.

   Esse manipulador configura o [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) para transmitir a entrada não processada pelo aplicativo. 

   Para uma etapa mais detalhada por meio deste código: veja a seção de entrada de passagem para processamento avançado em [Windows Ink e interações de caneta em aplicativos UWP](pen-and-stylus-interactions.md).

   Também especificamos um ícone para o botão usando o elemento SymbolIcon e a extensão de marcação {x:Bind} que o associa a um campo definido no arquivo code-behind (SelectIcon).

   O snippet a seguir inclui o manipulador de eventos Click e a definição de SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
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
        }

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

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
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

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
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
    }
}
```



### <a name="custom-ink-rendering"></a>Renderização de tinta personalizada

Por padrão, a entrada de tinta é processada em um thread em segundo plano e renderizada como "molhada" conforme é desenhada. Quando o traço está concluído (caneta ou dedo param de pressionar ou botão do mouse é liberado), o traço é processado do thread de interface do usuário e renderizado como "seco" na camada [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (acima do conteúdo do aplicativo e substituindo a tinta molhada).

A plataforma de tinta permite substituir esse comportamento e personalizar completamente a experiência de escrita à tinta secando de forma personalizada a entrada de tinta.

Para saber mais sobre a secagem personalizada, veja [Windows Ink e interações de caneta em aplicativos UWP](https://msdn.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> Secagem personalizada e o [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Se o aplicativo substituir o comportamento de renderização da tinta padrão do [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) por uma implementação de secagem personalizada, os traços da tinta renderizada não estarão mais disponíveis para o InkToolbar e os comandos de exclusão internos do InkToolbar não funcionarão conforme esperado. Para oferecer uma funcionalidade de exclusão, você deve lidar com todos os eventos de ponteiro, realizar o teste de clique em cada traço e substituir o comando "Apagar toda a tinta" interno.

## <a name="related-articles"></a>Artigos relacionados

* [Interações com caneta](pen-and-stylus-interactions.md)

**Exemplos**
* [Exemplo de tinta simples (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemplo de tinta complexa (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemplo de tinta (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de Introdução: oferecer suporte à tinta em seu aplicativo UWP](https://aka.ms/appsample-ink)
* [Exemplo de livro de colorir](https://aka.ms/cpubsample-coloringbook)
* [Exemplo de anotações da família](https://aka.ms/cpubsample-familynotessample)


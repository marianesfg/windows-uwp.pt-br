---
author: Karl-Bridge-Microsoft
Description: "Adicione um InkToolbar padrão a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP), adicione um botão de caneta personalizada ao InkToolbar e vincule o botão de caneta personalizada a uma definição de caneta personalizada."
title: "Adicionar um InkToolbar a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP)"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 71b73605bab71dad36977d0506c090c34359a3e2
ms.openlocfilehash: c4a5b0ae2893fda7697457b9e7449a996707de4b

---

# Adicionar um InkToolbar a um aplicativo de escrita à tinta da Plataforma Universal do Windows (UWP)

Há dois controles diferentes que facilitam a escrita à tinta em aplicativos da Plataforma Universal do Windows (UWP): [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

O controle [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) fornece a funcionalidade básica do Windows Ink. Use-o para renderizar a entrada à caneta como traço de tinta (usando as configurações padrão de cor e espessura) ou como traço de apagar.

> Para obter detalhes sobre a implementação do InkCanvas, consulte [Interações com caneta em aplicativos UWP](pen-and-stylus-interactions.md).

Como uma sobreposição completamente transparente, o InkCanvas não fornece qualquer interface do usuário interna para definir propriedades de traço de tinta. Se você quiser alterar a experiência de escrita à tinta padrão, deixar que os usuários definam as propriedades de traço de tinta e dar suporte a outros recursos de escrita à tinta personalizados, há duas opções:

- No code-behind, use objeto [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) subjacente associado ao InkCanvas.

  As APIs do InkPresenter permitem dar suporte à personalização abrangente da experiência de escrita à tinta. Para obter mais detalhes, consulte [Interações com caneta em aplicativos UWP](pen-and-stylus-interactions.md).

- Associar um [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) ao InkCanvas. Por padrão, o InkToolbar fornece uma interface do usuário básica para ativar recursos de tinta e definir propriedades de tinta, como tamanho do traço, cor da tinta e forma da ponta da caneta.

  Vamos falar sobre o InkToolbar neste tópico.

## APIs Importantes

  -   [**Classe InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**Classe InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**Classe InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## InkToolbar padrão

Por padrão, o InkToolbar inclui botões para desenhar, apagar, realçar e exibir uma régua. Dependendo do recurso, outras configurações e comandos, como a cor da tinta, a espessura do traço, apagar toda a tinta, são fornecidos em um submenu.

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*Barra de ferramentas do Windows Ink padrão*

Para adicionar um InkToolbar padrão básico:
1. Em MainPage.xaml, declare um objeto de contêiner (para este exemplo, usamos um controle Grid) para a superfície de escrita à tinta.
2. Declare um objeto InkCanvas como filho do contêiner. (O tamanho do InkCanvas é herdado do contêiner).
3. Declare um InkToolbar e use o atributo TargetInkCanvas para associá-lo ao InkCanvas.
  Certifique-se de que o InkToolbar seja declarado após o InkCanvas. Caso contrário, a sobreposição do InkCanvas torna o InkToolbar inacessível.

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

## Personalização básica

Nesta seção, abordamos alguns cenários de personalização básica de barra de ferramentas do Windows Ink.

### Especificar o botão selecionado  
![Botão de lápis selecionado na inicialização](.\images\ink\ink-tools-default-toolbar.png)  
*Barra de ferramentas do Windows Ink com botão de lápis selecionado na inicialização*

Por padrão, o primeiro botão (à esquerda) é selecionado quando seu aplicativo é iniciado e a barra de ferramentas é inicializada. Na barra de ferramentas do Windows Ink padrão, esse é o botão de caneta esferográfica.

Como a estrutura define a ordem dos botões internos, o primeiro botão pode não ser a caneta ou ferramenta que você deseja ativar por padrão.

Você pode substituir esse comportamento padrão e especificar o botão selecionado na barra de ferramentas.

Para este exemplo, inicializamos a barra de ferramentas padrão com o botão de lápis selecionado e o lápis ativado (em vez da caneta esferográfica).

1. Use a declaração XAML para o InkCanvas e InkToolbar do exemplo anterior.
2. No code-behind, defina um manipulador para o evento [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) do objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

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

3. No manipulador para o evento [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx):
  1. Obtenha uma referência para o [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) interno.

    Passar um objeto [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) no método [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) retorna um objeto [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) para o [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

  2. Defina [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) para o objeto retornado na etapa anterior.

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

### Especificar os botões internos

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
- Adicione um atributo [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) e defina seu valor como "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)". Isso limpa a coleção padrão de botões internos.
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

2. No code-behind, defina um manipulador para o evento [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) do objeto [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

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

3. Defina [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) como "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)".
4. Crie referências de objeto para os botões exigidos pelo seu aplicativo. Aqui, adicionamos somente [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) e [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx).
  > [!NOTE]
  > Os botões são adicionados à barra de ferramentas na ordem definida pela estrutura, não na ordem especificada aqui.

5. [Adicione](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) os botões ao InkToolbar.

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

## Recursos de escrita à tinta e botões personalizados

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

### Caneta personalizada

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

4. Especifique que a classe CalligraphicPen é derivada de [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Substitua [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) para especificar seu próprio tamanho de pincel e traço.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Crie um objeto [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) e defina o [formato da ponta da caneta](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), a [rotação da ponta](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), o [tamanho do traço](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) e a [cor da tinta](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
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

1. Declaramos um dicionário de recursos de página local que cria uma referência à caneta personalizada (`CalligraphicPen`) definida em CalligraphicPen.cs e uma [coleção de pincéis](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) com suporte da caneta personalizada (`CalligraphicPenPalette`).
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

2. Em seguida, adicionamos um InkToolbar com um elemento [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) filho.

  O botão de caneta personalizada inclui as duas referências de recurso estático declaradas nos recursos da página: `CalligraphicPen` e `CalligraphicPenPalette`.

  Também podemos especificar o intervalo para o controle deslizante de tamanho de traço ([MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) e [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), o pincel selecionado ([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) e o ícone do botão de caneta personalizada ([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
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

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## Artigos relacionados

* [Interações com caneta](pen-and-stylus-interactions.md)

**Exemplos**
* [Amostra de tinta](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Amostra de tinta simples](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Amostra de tinta complexa](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Sep16_HO2-->



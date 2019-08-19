---
Description: Use a classe AppWindow para exibir partes diferentes de seu aplicativo em janelas separadas.
title: Usar a classe AppWindow para mostrar janelas secundárias para um aplicativo
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867460"
---
# <a name="show-multiple-views-with-appwindow"></a>Mostrar várias exibições com AppWindow

O [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) e suas APIs relacionadas simplificam a criação de aplicativos com várias janelas, permitindo que você mostre o conteúdo do aplicativo em janelas secundárias enquanto ainda trabalha no mesmo thread da interface do usuário em cada janela.

> [!NOTE]
> O AppWindow está atualmente em versão prévia. Isso significa que você pode enviar aplicativos que usam o AppWindow para a loja, mas alguns componentes de plataforma e estrutura são conhecidos por não funcionar com AppWindow (consulte [limitações](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Aqui, mostramos alguns cenários para várias janelas com um aplicativo de exemplo `HelloAppWindow`chamado. O aplicativo de exemplo demonstra a seguinte funcionalidade:

- Desencaixe um controle da página principal e abra-o em uma nova janela.
- Abra novas instâncias de uma página em novas janelas.
- Dimensione e posicione programaticamente novas janelas no aplicativo.
- Associe um ContentDialog com a janela apropriada no aplicativo.

![Aplicativo de exemplo com uma única janela](images/hello-app-window-single.png)
  
> _Aplicativo de exemplo com uma única janela_

![Aplicativo de exemplo com seletor de cores desencaixado e janela secundária](images/hello-app-window-multi.png)

> _Aplicativo de exemplo com seletor de cores desencaixado e janela secundária_

> **APIs importantes**: [Namespace Windows. UI. WindowManagement](/uwp/api/windows.ui.windowmanagement), [classe AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>Visão geral de API

A classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) e outras APIs no namespace [WindowManagement](/uwp/api/windows.ui.windowmanagement) estão disponíveis a partir do Windows 10, versão 1903 (SDK 18362). Se seu aplicativo tiver como alvo versões anteriores do Windows 10, você deverá [usar o ApplicationView para criar janelas secundárias](application-view.md). As APIs do WindowManagement ainda estão em desenvolvimento e têm [limitações](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) , conforme descrito nos documentos de referência da API.

Aqui estão algumas das APIs importantes que você usa para mostrar o conteúdo em um AppWindow.

### <a name="appwindow"></a>AppWindow

A classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) pode ser usada para exibir uma parte de um aplicativo Windows Runtime em uma janela secundária. Ele é semelhante em conceito a um [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), mas não o mesmo no comportamento e no tempo de vida. Um recurso principal do AppWindow é que cada instância compartilha o mesmo thread de processamento da interface do usuário (incluindo o Dispatcher de eventos) do qual eles foram criados, o que simplifica os aplicativos de várias janelas.

Você só pode conectar conteúdo XAML ao seu AppWindow, não há suporte para conteúdo nativo do DirectX ou do Holographic. No entanto, você pode mostrar um [SWAPCHAINPANEL](/uwp/api/windows.ui.xaml.controls.swapchainpanel) XAML que hospeda conteúdo do DirectX.

### <a name="windowingenvironment"></a>WindowingEnvironment

A API do [WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) permite que você saiba sobre o ambiente em que seu aplicativo está sendo apresentado para que você possa adaptar seu aplicativo conforme necessário. Ele descreve o tipo de janela que o ambiente dá suporte; por exemplo, `Overlapped` se o aplicativo estiver em execução em um computador ou `Tiled` se o aplicativo estiver em execução em um Xbox. Ele também fornece um conjunto de objetos DisplayRegion que descrevem as áreas em que um aplicativo pode ser mostrado em uma exibição lógica.

### <a name="displayregion"></a>DisplayRegion

A API [DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) descreve a região na qual uma exibição pode ser mostrada para um usuário em uma exibição lógica; por exemplo, em um PC desktop, essa é a exibição completa menos a área da barra de tarefas. Não é necessariamente um mapeamento 1:1 com a área de exibição física do monitor de backup. Pode haver várias regiões de exibição dentro do mesmo monitor ou uma DisplayRegion pode ser configurada para abranger vários monitores se esses monitores forem homogêneos em todos os aspectos.

### <a name="appwindowpresenter"></a>AppWindowPresenter

A API [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) permite que você alterne facilmente janelas para configurações predefinidas `FullScreen` como `CompactOverlay`ou. Essas configurações fornecem ao usuário uma experiência consistente em qualquer dispositivo que dê suporte à configuração.

### <a name="uicontext"></a>UIContext

[UIContext](/uwp/api/windows.ui.uicontext) é um identificador exclusivo para uma janela ou exibição de aplicativo. Ele é criado automaticamente e você pode usar a propriedade [UIElement. UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext) para recuperar o UIContext. Todo UIElement na árvore XAML tem o mesmo UIContext.

 UIContext é importante porque as APIs como [Window. Current](/uwp/api/Windows.UI.Xaml.Window.Current) e `GetForCurrentView` o padrão dependem de ter um único ApplicationView/CoreWindow com uma única árvore XAML por thread com a qual trabalhar. Esse não é o caso quando você usa um AppWindow, portanto, você usa UIContext para identificar uma janela específica em vez disso.

### <a name="xamlroot"></a>XamlRoot

A classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) mantém uma árvore de elementos XAML, conecta-a ao objeto de host Window (por exemplo, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) ou [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) e fornece informações como tamanho e visibilidade. Você não cria um objeto XamlRoot diretamente. Em vez disso, um é criado quando você anexa um elemento XAML a um AppWindow. Em seguida, você pode usar a propriedade [UIElement. XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) para recuperar o XamlRoot.

Para obter mais informações sobre UIContext e XamlRoot, consulte [tornar o código portátil entre](show-multiple-views.md#make-code-portable-across-windowing-hosts)os hosts de janelas.

## <a name="show-a-new-window"></a>Mostrar uma nova janela

Vamos dar uma olhada nas etapas para mostrar o conteúdo em um novo AppWindow.

**Para mostrar uma nova janela**

1. Chame o método estático [AppWindow. TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) para criar um novo [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow).

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Crie o conteúdo da janela.

    Normalmente, você cria um [quadro](/uwp/api/Windows.UI.Xaml.Controls.Frame)XAML e, em seguida, navega o quadro para uma [página](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML em que você definiu o conteúdo do aplicativo. Para obter mais informações sobre quadros e páginas, consulte [navegação ponto a ponto entre duas páginas](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    No entanto, você pode mostrar qualquer conteúdo XAML no AppWindow, não apenas um quadro e uma página. Por exemplo, você pode mostrar apenas um único controle, como [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), ou pode mostrar um [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) que hospeda conteúdo do DirectX.

1. Chame o método [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) para anexar o conteúdo XAML ao AppWindow.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    A chamada para esse método cria um objeto [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) e o define como a propriedade [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) para o UIElement especificado.

    Você só pode chamar esse método uma vez por instância de AppWindow. Depois que o conteúdo tiver sido definido, outras chamadas para SetAppWindowContent para essa instância de AppWindow falharão. Além disso, se você tentar desconectar o conteúdo do AppWindow passando um objeto UIElement nulo, a chamada falhará.

1. Chame o método [AppWindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) para mostrar a nova janela.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Liberar recursos quando uma janela for fechada

Você sempre deve manipular o evento [AppWindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) para liberar recursos XAML (o conteúdo de AppWindow) e referências a AppWindow.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Rastrear instâncias de AppWindow

Dependendo de como você usa várias janelas em seu aplicativo, você pode ou não precisar manter o controle das instâncias do AppWindow que criar. O `HelloAppWindow` exemplo mostra algumas maneiras diferentes em que você normalmente pode usar um [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow). Aqui, veremos por que essas janelas devem ser controladas e como fazer isso.

### <a name="simple-tracking"></a>Acompanhamento simples

A janela Seletor de cores hospeda um único controle XAML e o código para interagir com o seletor de cores reside no `MainPage.xaml.cs` arquivo. A janela Seletor de cores só permite uma única instância e é essencialmente uma `MainWindow`extensão do. Para garantir que apenas uma instância seja criada, a janela Seletor de cores é rastreada com uma variável de nível de página. Antes de criar uma nova janela de seletor de cores, verifique se existe uma instância e, se ela existir, ignore as etapas para criar uma nova janela e apenas chamar [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) na janela existente.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Rastrear uma instância do AppWindow em seu conteúdo hospedado

A `AppWindowPage` janela hospeda uma página XAML completa e o código para interagir com a página reside no `AppWindowPage.xaml.cs`. Ele permite várias instâncias, cada uma das quais funções de forma independente.

A funcionalidade da página permite que você manipule a janela, definindo-a `FullScreen` como `CompactOverlay`ou e também escuta eventos [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) para mostrar informações sobre a janela. Para chamar essas APIs, `AppWindowPage` é necessário ter uma referência à instância AppWindow que a está hospedando.

Se isso for tudo o que você precisa, você poderá criar uma propriedade `AppWindowPage` no e atribuir a instância [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) a ela ao criá-la.

**AppWindowPage.xaml.cs**

No `AppWindowPage`, crie uma propriedade para manter a referência AppWindow.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

No `MainPage`, obtenha uma referência à instância Page e atribua o AppWindow recém-criado à propriedade no `AppWindowPage`.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>Acompanhamento de janelas de aplicativo usando UIContext

Talvez você também queira ter acesso às instâncias de [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) de outras partes do seu aplicativo. Por exemplo, `MainPage` pode ter um botão ' fechar tudo ' que feche todas as instâncias rastreadas de AppWindow.

Nesse caso, você deve usar o identificador exclusivo [UIContext](/uwp/api/windows.ui.uicontext) para rastrear as instâncias de janela em um [dicionário](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0).

**MainPage.xaml.cs**

No `MainPage`, crie o dicionário como uma propriedade estática. Em seguida, adicione a página ao dicionário ao criá-la e remova-a quando a página for fechada. Você pode obter o UIContext do [quadro](/uwp/api/Windows.UI.Xaml.Controls.Frame) de conteúdo (`appWindowContentFrame.UIContext`) depois de chamar [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent).

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

Para usar a instância [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) em seu `AppWindowPage` código, use o [UIContext](/uwp/api/windows.ui.uicontext) da página para recuperá-la do dicionário estático no `MainPage`. Você deve fazer isso no manipulador de eventos [carregado](/uwp/api/windows.ui.xaml.frameworkelement.loaded) da página em vez de no construtor para que UIContext não seja nulo. Você pode obter o UIContext da página: `this.UIContext`.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> O `HelloAppWindow` exemplo mostra as duas maneiras de rastrear a janela `AppWindowPage`no, mas você normalmente usará uma ou outra, não ambas.

## <a name="request-window-size-and-placement"></a>Tamanho e posicionamento da janela de solicitação

A classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) tem vários métodos que você pode usar para controlar o tamanho e o posicionamento da janela. Conforme implícito pelos nomes de método, o sistema pode ou não honrar as alterações solicitadas dependendo dos fatores ambientais.

Chame [solicite](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) para especificar um tamanho de janela desejado, como este.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Os métodos para gerenciar o posicionamento da janela são nomeados _RequestMove *_ : [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

Neste exemplo, esse código move a janela para que fique ao lado da exibição principal da qual a janela é gerada.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Para obter informações sobre o tamanho atual e o posicionamento da janela, chame [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement). Isso retorna um objeto [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) que fornece o [DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion), o [deslocamento](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)e o [tamanho](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) atuais da janela.

Por exemplo, você poderia chamar esse código para mover a janela para o canto superior direito da tela. Este código deve ser chamado depois que a janela for exibida; caso contrário, o tamanho da janela retornado pela chamada para GetPlacement será 0, 0 e o deslocamento estará incorreto.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Solicitar uma configuração de apresentação

A classe [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) permite mostrar um [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) usando uma configuração predefinida apropriada para o dispositivo em que é mostrado. Você pode usar um valor [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) para posicionar a janela `FullScreen` no `CompactOverlay` modo ou.

Este exemplo mostra como fazer o seguinte:

- Use o evento [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) para ser notificado se as apresentações de janela disponíveis forem alteradas.
- Use a propriedade [AppWindow. Presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) para obter o [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)atual.
- Chame [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) para ver se há suporte para um [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) específico.
- Chame [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) para verificar que tipo de configuração está sendo usado no momento.
- Chame [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) para alterar a configuração atual.

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>Reutilizar elementos XAML

Um [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) permite que você tenha várias árvores XAML com o mesmo thread de interface do usuário. No entanto, um elemento XAML só pode ser adicionado a uma árvore XAML uma vez. Se desejar mover uma parte da interface do usuário de uma janela para outra, você precisará gerenciá-la na árvore XAML.

Este exemplo mostra como reutilizar um controle [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) ao movê-lo entre a janela principal e uma janela secundária.

O seletor de cor é declarado no XAML `MainPage`para, que o coloca `MainPage` na árvore XAML.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

Quando o seletor de cores é desanexado para ser colocado em um novo AppWindow, primeiro você precisa removê-lo `MainPage` da árvore XAML removendo-o de seu contêiner pai. Embora não seja necessário, este exemplo também oculta o contêiner pai.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Em seguida, você pode adicioná-lo à nova árvore XAML. Aqui, você primeiro cria uma [grade](/uwp/api/windows.ui.xaml.controls.grid) que será o contêiner pai do ColorPicker e adiciona o ColorPicker como um filho da grade. (Isso permite que você remova facilmente o ColorPicker desta árvore XAML posteriormente.) Em seguida, você define a grade como a raiz da árvore XAML na nova janela.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Quando o [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) é fechado, você reverte o processo. Primeiro, remova o [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) da [grade](/uwp/api/windows.ui.xaml.controls.grid)e, em seguida, adicione-o como um filho do `MainPage` [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) em.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>Mostrar uma caixa de diálogo

Por padrão, as caixas de diálogo de conteúdo são exibidas modalmente em relação à [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) raiz. Ao usar um [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) dentro de um [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow), você precisará definir manualmente o XamlRoot na caixa de diálogo para a raiz do host XAML.

Para fazer isso, defina a propriedade [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) do ContentDialog como o mesmo [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) como um elemento que já está no AppWindow. Aqui, esse código está dentro do manipulador de eventos de [clique](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) de um botão, para que você possa usar o _remetente_ (o botão clicado) para obter o XamlRoot.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Se você tiver um ou mais AppWindows abertos além da janela principal (ApplicationView), cada janela poderá tentar abrir uma caixa de diálogo, pois a caixa de diálogo modal bloqueará apenas a janela na qual ela está enraizada. No entanto, pode haver apenas um [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) aberto por thread por vez. A tentativa de abrir duas ContentDialogs lançará uma exceção, mesmo que estejam tentando abrir em AppWindows separadas.

Para gerenciar isso, você deve, pelo menos, abrir a caixa de `try/catch` diálogo em um bloco para capturar a exceção caso outra caixa de diálogo já esteja aberta.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

Outra maneira de gerenciar caixas de diálogo é controlar a caixa de diálogo aberta no momento e fechá-la antes de tentar abrir uma nova caixa de diálogo. Aqui, você cria uma propriedade estática em `MainPage` chamada `CurrentDialog` para essa finalidade.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

Em seguida, você verifica se há uma caixa de diálogo aberta no momento e, se houver, chame o método [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) para fechá-lo. Por fim, atribua a nova caixa `CurrentDialog`de diálogo e tente mostrá-la.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

Se não for desejável ter uma caixa de diálogo fechada programaticamente, não a atribua `CurrentDialog`como o. Aqui, `MainPage` mostra uma caixa de diálogo importante que só deve ser ignorada quando o uso `Ok`é clicado. Como não está atribuído como o `CurrentDialog`, nenhuma tentativa é feita para fechá-lo programaticamente.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>Código completo

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage. XAML

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>Tópicos relacionados

- [Mostrar várias exibições](show-multiple-views.md)
- [Mostrar várias exibições com ApplicationView](application-view.md)

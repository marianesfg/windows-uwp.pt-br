---
title: Usar a Camada Visual com WPF
description: Saiba sobre técnicas para usar as APIs de Camada Visual em combinação com o conteúdo WPF existente para criar animações e efeitos avançados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215153"
---
# <a name="using-the-visual-layer-with-wpf"></a>Usar a Camada Visual com WPF

Você pode usar as APIs de Composição do Windows Runtime (também conhecidas como [Camada visual](/windows/uwp/composition/visual-layer)) em seus aplicativos Windows Presentation Foundation (WPF) para criar experiências modernas que se destacam para os usuários do Windows 10.

O código completo para este tutorial está disponível no GitHub: [Exemplo do HelloComposition para WPF](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem XAML de UWP tem estes pré-requisitos.

- Supomos que você tem alguma familiaridade com o desenvolvimento de aplicativos usando WPF e UWP. Para saber mais, veja:
  - [Introdução (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 ou posterior
- Windows 10, versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-in-wpf"></a>Como usar APIs de composição no WPF

Neste tutorial, você criará uma interface de usuário de aplicativo WPF simples e adicionará elementos de composição animada a ele. Os componentes WPF e composição são mantidos simples, no entanto, o código de interoperabilidade mostrado é o mesmo, independentemente da complexidade dos componentes. O aplicativo concluído tem esta aparência.

![A interface do usuário do aplicativo em execução](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Criar um projeto WPF

A primeira etapa é criar o projeto de aplicativo WPF, que inclui uma definição de aplicativo e a página XAML para a interface do usuário.

Para criar um projeto de aplicativo WPF no C# visual chamado _HelloComposition_:

1. Abra o Visual Studio e selecione **Arquivo** > **Novo** > **Projeto**.

    A caixa de diálogo **Novo Projeto** é aberta.
1. Na categoria **Instalado**, expanda o nó **Visual C#** e selecione **Área de trabalho do Windows**.
1. Selecione o modelo de **aplicativo WPF (.NET Framework)** .
1. Nomeie-o como _HelloComposition_, selecione a estrutura **.NET Framework 4.7.2** e clique em **OK**.

    O Visual Studio cria o projeto e abre o designer para a janela de aplicativo padrão chamada MainWindow.xaml.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar APIs do Windows Runtime

Para usar as APIs do Windows Runtime (WinRT) em seu aplicativo WPF, é preciso configurar seu projeto do Visual Studio para acessar o Windows Runtime. Além disso, os vetores são usados amplamente pelas APIs de composição, portanto, é preciso adicionar as referências necessárias para usar vetores.

Os pacotes NuGet estão disponíveis para atender a essas duas necessidades. Instale as versões mais recentes desses pacotes para adicionar as referências necessárias ao projeto.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (Requer o formato de gerenciamento de pacote padrão definido como PackageReference.)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Apesar de recomendarmos usar pacotes NuGet para configurar seu projeto, você pode adicionar as referências necessárias manualmente. Para saber mais, veja [Aprimorar o aplicativo de área de trabalho para o Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). A tabela a seguir mostra os arquivos aos quais você deve adicionar referências.

|Arquivo|Local|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Arquivos de Programas (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Configurar o projeto para que seja com reconhecimento de DPI por monitor

O conteúdo da camada visual que você adiciona ao aplicativo não é dimensionado automaticamente para corresponder às configurações de DPI da tela na qual ele é mostrado. É preciso habilitar o reconhecimento de DPI por monitor para seu aplicativo e, em seguida, verificar se o código usado para criar o conteúdo da camada visual leva em conta a escala de DPI atual quando o aplicativo é executado. Aqui, vamos configurar o projeto para que seja com reconhecimento de DPI. Nas seções posteriores, mostramos como usar as informações de DPI para dimensionar o conteúdo da camada visual.

Os aplicativos WPF têm reconhecimento de DPI de sistema por padrão, mas precisam declarar ter reconhecimento de DPI por monitor em um arquivo app.manifest. Para ativar o reconhecimento de DPI por monitor no nível do Windows no arquivo do manifesto de aplicativo:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
1. No menu de contexto, selecione **Adicionar** > **Novo item...** .
1. Na caixa de diálogo **Adicionar Novo Item**, selecione "Arquivo de Manifesto do Aplicativo" e clique em **Adicionar**. (Você pode deixar o nome padrão.)
1. No arquivo app.manifest, encontre este XML e remova o comentário:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Adicione essa configuração após a marca `<windowsSettings>` de abertura:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. Você também precisa definir a configuração **DoNotScaleForDpiChanges** no arquivo App.config.

    Abra App.Config e adicione esse XML dentro do elemento `<configuration>`:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** só pode ser definido uma vez. Se seu aplicativo já tiver um definido, você deverá delimitar com ponto e vírgula essa opção dentro do atributo de valor.

(Para saber mais, confira [Exemplos e guia do desenvolvedor de DPI por monitor](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) no GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Criar classe derivada HwndHost para hospedar elementos de composição

Para hospedar o conteúdo criado com a camada visual, é preciso criar uma classe derivada de [HwndHost](/dotnet/api/system.windows.interop.hwndhost). É aqui que a maior parte da configuração para hospedagem de APIs de Composição é feita. Nessa classe, você usa [PInvoke (Serviços de Invocação de Plataforma)](/cpp/dotnet/calling-native-functions-from-managed-code) e [Interoperabilidade COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para trazer APIs de Composição para o aplicativo WPF. Para saber mais sobre PInvoke e Interoperabilidade COM, confira [Interoperabilidade com código não gerenciado](/dotnet/framework/interop/index).

> [!TIP]
> Se necessário, verifique o código completo no final do tutorial para garantir que todo o código esteja nos locais certos enquanto você trabalha no tutorial.

1. Adicione um novo arquivo de classe personalizado ao seu projeto que derive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
    - No menu de contexto, selecione **Adicionar** > **Classe....** .
    - Na caixa de diálogo **Adicionar Novo Item**, nomeie a classe _CompositionHost.cs_ e clique em **Adicionar**.
1. Em CompositionHost.cs, edite a definição de classe para derivar de **HwndHost**.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. Adicione este código e construtor à classe.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. Substitua os métodos **BuildWindowCore** e **DestroyWindowCore**.
    > [!NOTE]
    > Em BuildWindowCore, são chamados os métodos _InitializeCoreDispatcher_ e _InitComposition_. Você criará esses métodos nas próximas etapas.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) e [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) requerem uma declaração PInvoke. Coloque essa declaração no final do código para a classe.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. Inicialize um thread com um [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). O dispatcher principal é responsável por processar mensagens de janela e expedir eventos para APIs do WinRT. As novas instâncias do **CoreDispatcher** devem ser criadas em um thread contendo um CoreDispatcher.
    - Crie um método chamado _InitializeCoreDispatcher_ e adicione o código para configurar a fila do dispatcher.

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - A fila do dispatcher também requer uma declaração PInvoke. Coloque essa declaração dentro da região de declarações _PInvoke_ que você criou na etapa anterior.

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    Agora que a fila do dispatcher está pronta, você pode começar a inicializar e criar o conteúdo de Composição.

1. Inicialize o [Compositor](/uwp/api/windows.ui.composition.compositor). O Compositor é uma fábrica que cria uma variedade de tipos no namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition), abrangendo os visuais, o sistema de efeitos e o sistema de animação. A classe Compositor também gerencia a vida útil dos objetos criados na fábrica.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** e **ICompositionTarget** exigem importações COM. Coloque este código após a classe _CompositionHost_, mas dentro da declaração do namespace.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Criar um UserControl para adicionar o conteúdo à árvore visual do WPF

A última etapa para configurar a infraestrutura necessária e hospedar o conteúdo da composição é adicionar o HwndHost à árvore visual do WPF.

### <a name="create-a-usercontrol"></a>Criar um UserControl

Um UserControl é uma maneira conveniente de empacotar seu código que cria e gerencia conteúdo de composição, adicionando facilmente o conteúdo ao seu XAML.

1. Adicionar um novo arquivo de controle de usuário ao projeto.
    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
    - No menu de contexto, selecione **Adicionar** > **Controle de usuário....** .
    - Na caixa de diálogo **Adicionar Novo Item**, nomeie o controle de usuário _CompositionHostControl.xaml_, e clique em **Adicionar**.

    Os arquivos CompositionHostControl.xaml e CompositionHostControl.xaml.cs são criados e adicionados ao seu projeto.
1. Em CompositionHostControl.xaml, substitua as `<Grid> </Grid>`marcas com esse elemento de **Borda**, que é o contêiner XAML em que seu HwndHost será inserido.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

No código do controle de usuário, você cria uma instância da classe CompositionHost, criada na etapa anterior, e a adiciona como um elemento filho de _CompositionHostElement_, a borda que você criou na página XAML.

1. No CompositionHostControl.xaml.cs, adicione variáveis privadas para os objetos que serão usados em seu código de composição. Adicione-as após a definição de classe.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Adicione um manipulador para o evento **Loaded** do controle de usuário. Aqui, você configura sua instância do CompositionHost.

    - No construtor, conecte o manipulador de eventos como mostrado aqui (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Adicione o método do manipulador de eventos com o nome *CompositionHostControl_Loaded*.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    Nesse método, você configura os objetos que serão usados em seu código de composição. Veja um resumo rápido do que está acontecendo.

    - Primeiro, certifique-se de que a configuração seja feita apenas uma vez, verificando se uma instância de CompositionHost já existe.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Obter DPI atual Isso é usado para dimensionar corretamente seus elementos de composição.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Crie uma instância de CompositionHost e atribua-a como o filho de borda, _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Obtenha o compositor de CompositionHost.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Use o compositor para criar um visual de contêiner. Este é o contêiner de composição ao qual você adiciona os elementos de composição.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Adicionar elementos de composição

Com a criação da infraestrutura, agora você pode gerar o conteúdo de composição que deseja mostrar.

Neste exemplo, o código é adicionado, criando e animando um [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) quadrado simples.

1. Adicione um elemento de composição. No CompositionHostControl.xaml.cs, adicione esses métodos à classe CompositionHostControl.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>Manipular alterações de DPI

O código para adicionar e animar um elemento leva em conta a escala de DPI atual quando elementos são criados, mas também é preciso levar em conta as alterações de DPI enquanto o aplicativo está em execução. Você pode manipular o evento [HwndHost. DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) para ser notificado sobre as alterações e ajustar seus cálculos com base no novo DPI.

1. No método CompositionHostControl_Loaded, após a última linha, adicione isso para conectar o manipulador de eventos DpiChanged.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Adicione o método do manipulador de eventos com o nome _CompositionHostDpiChanged_. Esse código ajusta a escala e o deslocamento de cada elemento, recalculando todas as animações que não estão completas.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>Adicionar o controle de usuário à página XAML

Agora, você pode adicionar o controle de usuário à interface do usuário XAML.

1. Em MainWindow.XAML, defina a altura de janela como 600 e a largura como 840.
1. Adicione XAML para a interface do usuário. Em MainWindow.XAML, adicione este XAML entre as marcas `<Grid> </Grid>` raiz.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. Manipule o clique do botão para criar elementos. (O evento de Clique já está conectado no XAML.)

    Em MainWindow.xaml.cs, adicione este método manipulador de eventos *Button_Click*. Este código chama _CompositionHost.AddElement_ para criar um elemento com um tamanho e deslocamento gerados aleatoriamente.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Agora você pode compilar e executar seu aplicativo WPF. Se necessário, verifique o código completo no final do tutorial para garantir que todo o código esteja nos locais certos.

Ao executar o aplicativo e clicar no botão, você deverá ver quadrados animados adicionados à interface do usuário.

## <a name="next-steps"></a>Próximas etapas

Para ver um exemplo mais completo desenvolvido com a mesma infraestrutura, confira o [exemplo de integração de camada Visual do WPF](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) no GitHub.

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interoperação com código não gerenciado](/dotnet/framework/interop/) (.NET)
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) (UWP)
- [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Namespace Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este é o código completo deste tutorial.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```
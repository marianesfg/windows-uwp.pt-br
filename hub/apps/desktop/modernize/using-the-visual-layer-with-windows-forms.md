---
title: Usando a camada Visual com o Windows Forms
description: Aprenda técnicas para usar as APIs de camada Visual em combinação com o conteúdo existente do Windows Forms para criar animações avançadas e efeitos.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0a3aff0bee68b971accd96f895601343726024d0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985026"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Usando a camada Visual com o Windows Forms

Você pode usar as APIs de composição de tempo de execução do Windows (também chamado de [camada Visual](/windows/uwp/composition/visual-layer)) em seus aplicativos do Windows Forms para criar experiências modernas ou luz para os usuários do Windows 10.

O código completo para este tutorial está disponível no GitHub: [Exemplo de Windows Forms HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem de UWP tem esses pré-requisitos.

- Vamos supor que você tenha alguma familiaridade com o desenvolvimento de aplicativos usando o Windows Forms e UWP. Para saber mais, veja:
  - [Introdução ao Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimore seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 ou posterior
- Windows 10 versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Como usar APIs de composição no Windows Forms

Neste tutorial, você cria uma interface do usuário simples do Windows Forms e adicionar elementos animados de composição a ele. O Windows Forms e a composição de componentes são mantidos simples, mas o código de interoperabilidade mostrado é o mesmo, independentemente da complexidade dos componentes. O aplicativo concluído se parece com isso.

![O interface do usuário do aplicativo em execução](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Criar um projeto Windows Forms

A primeira etapa é criar o projeto de aplicativo do Windows Forms, que inclui uma definição de aplicativo e o formulário principal para a interface do usuário.

Para criar um novo projeto de aplicativo do Windows Forms no Visual C# nomeado _HelloComposition_:

1. Abra o Visual Studio e selecione **arquivo** > **New** > **projeto**.<br/>O **novo projeto** caixa de diálogo é aberta.
1. Sob o **Installed** categoria, expanda o **Visual C#**  nó e, em seguida, selecione **área de trabalho do Windows**.
1. Selecione o **aplicativo do Windows Forms (.NET Framework)** modelo.
1. Insira o nome _HelloComposition,_ selecionar Framework **.NET Framework 4.7.2**, em seguida, clique em **Okey**.

Visual Studio cria o projeto e abre o designer para a janela do aplicativo padrão chamado Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar APIs do Windows Runtime

Para usar o tempo de execução do Windows (WinRT) APIs em seu aplicativo Windows Forms, você precisará configurar seu projeto do Visual Studio para acessar o tempo de execução do Windows. Além disso, o vetores são usados amplamente por APIs de composição, portanto, você precisa adicionar as referências necessárias para usar vetores.

Pacotes do NuGet estão disponíveis para abordar essas necessidades. Instale as versões mais recentes desses pacotes para adicionar as referências necessárias ao seu projeto.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requer o conjunto de formato de gerenciamento de pacote de padrão para PackageReference).
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Embora seja recomendável usar os pacotes do NuGet para configurar seu projeto, você pode adicionar as referências necessárias manualmente. Para obter mais informações, consulte [Aprimore seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). A tabela a seguir mostra os arquivos que você precisa adicionar referências a.

|Arquivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Criar um controle personalizado para gerenciar a interoperabilidade

Para hospedar conteúdo que você criar com a camada visual, você cria um controle personalizado derivado [controle](/dotnet/api/system.windows.forms.control). Esse controle fornece acesso a uma janela [manipular](/dotnet/api/system.windows.forms.control.handle), que você precisa para criar o contêiner para o seu conteúdo de camada visual.

Isso é onde você executa a maioria da configuração para a hospedagem de APIs de composição. Esse controle, você usa [serviços de invocação de plataforma (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) e [interoperabilidade COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para trazer as APIs de composição para seu aplicativo de formulários do Windows. Para obter mais informações sobre PInvoke e interoperabilidade COM, consulte [interoperação com código não gerenciado](/dotnet/framework/interop/index).

> [!TIP]
> Se você precisar, verifique o código completo no final do tutorial para garantir que todo o código está nos lugares certos enquanto você trabalha com o tutorial.

1. Adicione um novo arquivo de controle personalizado ao seu projeto que deriva de [controle](/dotnet/api/system.windows.forms.control).
    - Na **Gerenciador de soluções**, clique com botão direito do _HelloComposition_ projeto.
    - No menu de contexto, selecione **Add** > **Novo Item...** .
    - No **Adicionar Novo Item** caixa de diálogo, selecione **Custom Control**.
    - Nomeie o controle _CompositionHost.cs_, em seguida, clique em **Add**. CompositionHost.cs é aberto no modo de exibição de Design.

1. Alterne para modo de exibição de código para CompositionHost.cs e adicione o seguinte código à classe.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. Adicione código ao construtor.

    No construtor, você chama o _InitializeCoreDispatcher_ e _InitComposition_ métodos. Você pode criar esses métodos nas próximas etapas.

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

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

    - Fila do dispatcher requer uma declaração PInvoke. Colocar essa declaração no final do código para a classe. (Vamos colocar esse código dentro de uma região para manter o código da classe organizada).

    ```csharp
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

    #endregion PInvoke declarations
    ```

    Agora você tem pronto de fila do distribuidor e pode começar a inicializar e criar conteúdo de composição.

1. Inicializar o [Compositor](/uwp/api/windows.ui.composition.compositor). O Compositor é uma fábrica que cria uma variedade de tipos na [Windows.UI.Composition](/uwp/api/windows.ui.composition) namespace abrangendo a camada visual, o sistema effects e o sistema de animação. A classe Compositor também gerencia o tempo de vida de objetos criados pela fábrica.

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop** e **ICompositionTarget** exigem importações de COM. Coloque esse código após o _CompositionHost_ classe, mas dentro a declaração de namespace.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Criar um controle personalizado para elementos de composição do host

É uma boa ideia colocar o código que gera e gerencia seus elementos de composição em um controle separado que deriva de CompositionHost. Isso mantém o código de interoperabilidade que você criou na classe CompositionHost reutilizáveis.

Aqui, você pode criar um controle personalizado derivado de CompositionHost. Esse controle é adicionado à caixa de ferramentas do Visual Studio, portanto, você pode adicioná-lo ao seu formulário.

1. Adicione um novo arquivo de controle personalizado ao seu projeto que deriva de CompositionHost.
    - Na **Gerenciador de soluções**, clique com botão direito do _HelloComposition_ projeto.
    - No menu de contexto, selecione **Add** > **Novo Item...** .
    - No **Adicionar Novo Item** caixa de diálogo, selecione **Custom Control**.
    - Nomeie o controle _CompositionHostControl.cs_, em seguida, clique em **Add**. CompositionHostControl.cs é aberto no modo de exibição de Design.

1. No painel Propriedades para o modo de exibição de design CompositionHostControl.cs, defina as **BackColor** propriedade **ControlLight**.

    Configurando a cor do plano de fundo é opcional. Fazemos isso aqui para que você possa ver seu controle personalizado, o plano de fundo do formulário.

1. Alternar para modo de exibição de código para CompositionHostControl.cs e atualizar a declaração de classe para derivar de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Atualize o construtor para chamar o construtor base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Adicionar elementos de composição

Com a infra-estrutura in-loco, agora você pode adicionar conteúdo de composição para o aplicativo de interface do usuário.

Neste exemplo, você deve adicionar código à classe CompositionHostControl que cria e anima um simples [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Adicione um elemento de composição.

    No CompositionHostControl.cs, adicione esses métodos à classe CompositionHostControl.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
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

## <a name="add-the-control-to-your-form"></a>Adicionar o controle ao formulário

Agora que você tem um controle personalizado para hospedar conteúdo de composição, você pode adicioná-lo para o interface do usuário do aplicativo. Aqui, você pode adicionar uma instância da CompositionHostControl que você criou na etapa anterior. CompositionHostControl é adicionado automaticamente a caixa de ferramentas do Visual Studio sob  **_nome do projeto_ componentes**.

1. No modo de design Form1.CS, adicione um botão na interface do usuário.

    - Arraste um botão da caixa de ferramentas para Form1. Coloque-o no canto superior esquerdo do formulário. (Consulte a imagem no início do tutorial para verificar o posicionamento dos controles.)
    - No painel Propriedades, altere o **texto** propriedade de _button1_ para _adicionar elemento de composição_.
    - Redimensione o botão para que todo o texto seja exibida.

    (Para obter mais informações, consulte [como: Adicionar controles ao Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Adicione um CompositionHostControl na interface do usuário.

    - Arraste um CompositionHostControl da caixa de ferramentas para Form1. Coloque-à direita do botão.
    - Redimensione o CompositionHost para que ele preencha o restante do formulário.

1. Evento de clique no botão de identificador.

   - No painel Propriedades, clique o raio para alternar para o modo de exibição de eventos.
   - Na lista de eventos, selecione a **clique em** eventos, digite *Button_Click*, e pressione Enter.
   - Esse código é adicionado em Form1. cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Adicione código ao botão de manipulador para criar novos elementos de clique.

    - No Form1, adicione código para o *Button_Click* manipulador de eventos que você criou anteriormente. Esse código chama _CompositionHostControl1.AddElement_ para criar um novo elemento com um tamanho gerado aleatoriamente e deslocamento. (A instância do CompositionHostControl foi nomeada automaticamente _compositionHostControl1_ quando arrastado para o formulário.)

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Agora você pode compilar e executar seu aplicativo de formulários do Windows. Quando você clica no botão, você deverá ver quadrados animados adicionados na interface do usuário.

## <a name="next-steps"></a>Próximas etapas

Para obter um exemplo mais completo que se baseia na mesma infraestrutura, consulte o [exemplo de integração de camada Visual do Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) no GitHub.

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução ao Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interoperação com código não gerenciado](/dotnet/framework/interop/) (.NET)
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) (UWP)
- [Aprimore seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Namespace Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Aqui está o código completo para este tutorial.

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
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
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
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

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
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
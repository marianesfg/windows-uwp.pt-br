---
title: Usando a camada visual com Windows Forms
description: Aprenda técnicas para usar as APIs de camada Visual em combinação com conteúdo de Windows Forms existente para criar animações e efeitos avançados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999635"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Usando a camada visual com Windows Forms

Você pode usar Windows Runtime APIs de composição (também chamadas de [camada Visual](/windows/uwp/composition/visual-layer)) em seus aplicativos Windows Forms para criar experiências modernas que acendem para os usuários do Windows 10.

O código completo deste tutorial está disponível no GitHub: [Windows Forms exemplo de HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem UWP tem esses pré-requisitos.

- Supomos que você tenha alguma familiaridade com o desenvolvimento de aplicativos usando Windows Forms e UWP. Para saber mais, veja:
  - [Guia de introdução ao Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimore seu aplicativo de área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 ou posterior
- Windows 10 versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Como usar as APIs de composição no Windows Forms

Neste tutorial, você cria uma interface do usuário Windows Forms simples e adiciona elementos de composição animados a ela. Os componentes de Windows Forms e composição são mantidos simples, mas o código de interoperabilidade mostrado é o mesmo, independentemente da complexidade dos componentes. O aplicativo concluído tem esta aparência.

![A interface do usuário do aplicativo em execução](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Criar um projeto Windows Forms

A primeira etapa é criar o projeto de aplicativo Windows Forms, que inclui uma definição de aplicativo e o formulário principal para a interface do usuário.

Para criar um novo projeto de aplicativo Windows Forms no C# Visual chamado _HelloComposition_:

1. Abra o Visual Studio e selecione **arquivo** > **novo** > **projeto**.<br/>A caixa de diálogo **novo projeto** é aberta.
1. Na categoria **instalado** , expanda o **nó C# Visual** e selecione área de **trabalho do Windows**.
1. Selecione o modelo **aplicativo de Windows Forms (.NET Framework)** .
1. Insira o nome _HelloComposition,_ selecione estrutura **.NET Framework 4.7.2**e clique em **OK**.

O Visual Studio cria o projeto e abre o designer para a janela de aplicativo padrão chamada Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar Windows Runtime APIs

Para usar as APIs do Windows Runtime (WinRT) em seu aplicativo Windows Forms, você precisa configurar seu projeto do Visual Studio para acessar o Windows Runtime. Além disso, os vetores são usados extensivamente pelas APIs de composição, portanto, você precisa adicionar as referências necessárias para usar vetores.

Os pacotes NuGet estão disponíveis para atender a essas duas necessidades. Instale as versões mais recentes desses pacotes para adicionar as referências necessárias ao seu projeto.  

- [Microsoft. Windows. Sdk. Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (requer o formato de gerenciamento de pacote padrão definido como PackageReference.)
- [Sistema. numéricos. vetores](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Embora seja recomendável usar os pacotes NuGet para configurar seu projeto, você pode adicionar as referências necessárias manualmente. Para obter mais informações, consulte [aprimorar seu aplicativo de área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). A tabela a seguir mostra os arquivos aos quais você precisa adicionar referências.

|Arquivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Arquivos de programas (x86) \Windows\<Kits\10\References*SDK versão*>\<\Windows.Foundation.UniversalApiContract*versão*>|
|Windows.Foundation.FoundationContract.winmd|C:\Arquivos de programas (x86) \Windows\<Kits\10\References*SDK versão*>\<\Windows.Foundation.FoundationContract*versão*>|
|System. Numerics. vetores. dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System. Numerics. dll|C:\Arquivos de programas (x86) \Reference\.Assemblies\Microsoft\Framework NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Criar um controle personalizado para gerenciar a interoperabilidade

Para hospedar o conteúdo criado com a camada Visual, você cria um controle personalizado derivado do [controle](/dotnet/api/system.windows.forms.control). Esse controle fornece acesso a um [identificador](/dotnet/api/system.windows.forms.control.handle)de janela, que você precisa para criar o contêiner para o conteúdo da camada Visual.

É aí que você faz a maior parte da configuração para hospedar APIs de composição. Nesse controle, você usa o [PInvoke (serviços de invocação de plataforma)](/cpp/dotnet/calling-native-functions-from-managed-code) e a interoperabilidade [com](/dotnet/api/system.runtime.interopservices.comimportattribute) para colocar as APIs de composição em seu aplicativo Windows Forms. Para obter mais informações sobre o PInvoke e a interoperabilidade COM, consulte [interoperação com código não gerenciado](/dotnet/framework/interop/index).

> [!TIP]
> Se você precisar, verifique o código completo no final do tutorial para certificar-se de que todo o código está nos locais certos enquanto você trabalha no tutorial.

1. Adicione um novo arquivo de controle personalizado ao seu projeto que deriva de [Control](/dotnet/api/system.windows.forms.control).
    - Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto _HelloComposition_ .
    - No menu de contexto, selecione **Adicionar** > **novo item...** .
    - Na caixa de diálogo **Adicionar novo item** , selecione **controle personalizado**.
    - Nomeie o controle _CompositionHost.cs_e clique em **Adicionar**. CompositionHost.cs é aberto no modo de exibição de Design.

1. Alterne para a exibição de código para CompositionHost.cs e adicione o código a seguir à classe.

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

1. Adicione o código ao construtor.

    No construtor, você chama os métodos _InitializeCoreDispatcher_ e _InitComposition_ . Você cria esses métodos nas próximas etapas.

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
    ```

1. Inicialize um thread com um [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). O Dispatcher principal é responsável por processar mensagens de janela e expedir eventos para APIs do WinRT. Novas instâncias do **compositor** devem ser criadas em um thread que tenha um CoreDispatcher.
    - Crie um método chamado _InitializeCoreDispatcher_ e adicione o código para configurar a fila do Dispatcher.

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

    - A fila do Dispatcher requer uma declaração PInvoke. Coloque essa declaração no final do código para a classe. (Colocamos esse código dentro de uma região para manter o código de classe organizado.)

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

    Agora você tem a fila do Dispatcher pronta e pode começar a inicializar e criar o conteúdo da composição.

1. Inicialize o [compositor](/uwp/api/windows.ui.composition.compositor). O compositor é uma fábrica que cria uma variedade de tipos no namespace [Windows. UI. composição](/uwp/api/windows.ui.composition) que abrangem a camada Visual, o sistema de efeitos e o sistema de animação. A classe como compositor também gerencia o tempo de vida de objetos criados a partir da fábrica.

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

    - **ICompositorDesktopInterop** e **ICOMPOSITIONTARGET** exigem importações com. Coloque esse código após a classe _CompositionHost_ , mas dentro da declaração do namespace.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Criar um controle personalizado para hospedar elementos de composição

É uma boa ideia colocar o código que gera e gerencia seus elementos de composição em um controle separado que deriva de CompositionHost. Isso mantém o código de interoperabilidade que você criou na classe CompositionHost reutilizável.

Aqui, você cria um controle personalizado derivado de CompositionHost. Esse controle é adicionado à caixa de ferramentas do Visual Studio para que você possa adicioná-lo ao formulário.

1. Adicione um novo arquivo de controle personalizado ao seu projeto que deriva de CompositionHost.
    - Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto _HelloComposition_ .
    - No menu de contexto, selecione **Adicionar** > **novo item...** .
    - Na caixa de diálogo **Adicionar novo item** , selecione **controle personalizado**.
    - Nomeie o controle _CompositionHostControl.cs_e clique em **Adicionar**. CompositionHostControl.cs é aberto no modo de exibição de Design.

1. No painel Propriedades do modo de exibição de design CompositionHostControl.cs, defina a propriedade **BackColor** como **ControlLight**.

    A configuração da cor do plano de fundo é opcional. Fazemos isso aqui para que você possa ver seu controle personalizado em relação ao plano de fundo do formulário.

1. Alterne para a exibição de código para CompositionHostControl.cs e atualize a declaração de classe para derivar de CompositionHost.

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

Com a infraestrutura em vigor, agora você pode adicionar conteúdo de composição à interface do usuário do aplicativo.

Para este exemplo, você adiciona código à classe CompositionHostControl que cria e anima um [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)simples.

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

Agora que você tem um controle personalizado para hospedar o conteúdo de composição, você pode adicioná-lo à interface do usuário do aplicativo. Aqui, você adiciona uma instância do CompositionHostControl que você criou na etapa anterior. O CompositionHostControl é adicionado automaticamente à caixa de ferramentas do Visual Studio em componentes do **_nome do projeto_** .

1. No modo de exibição de design Form1.CS, adicione um botão à interface do usuário.

    - Arraste um botão da caixa de ferramentas para o Form1. Coloque-o no canto superior esquerdo do formulário. (Consulte a imagem no início do tutorial para verificar o posicionamento dos controles.)
    - No painel Propriedades, altere a propriedade **texto** de _Button1_ para _Adicionar elemento de composição_.
    - Redimensione o botão para que todo o texto seja mostrado.

    (Para obter mais informações, [consulte Como: Adicionar controles a Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Adicione um CompositionHostControl à interface do usuário.

    - Arraste um CompositionHostControl da caixa de ferramentas para o Form1. Coloque-o à direita do botão.
    - Redimensione o CompositionHost para que ele preencha o restante do formulário.

1. Manipule o evento de clique no botão.

   - No painel Propriedades, clique no raio para alternar para a exibição eventos.
   - Na lista de eventos, selecione o evento de **clique** , digite *Button_Click*e pressione Enter.
   - Esse código é adicionado em Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Adicione código ao manipulador de clique de botão para criar novos elementos.

    - No Form1.cs, adicione o código ao manipulador de eventos *Button_Click* criado anteriormente. Esse código chama o _elemento CompositionHostControl1._ Addpara criar um novo elemento com um tamanho e um deslocamento gerados aleatoriamente. (A instância de CompositionHostControl foi automaticamente nomeada _compositionHostControl1_ quando você a arrastou para o formulário.)

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

Agora você pode criar e executar seu aplicativo Windows Forms. Ao clicar no botão, você deverá ver os quadrados animados adicionados à interface do usuário.

## <a name="next-steps"></a>Próximas etapas

Para obter um exemplo mais completo que se baseia na mesma infraestrutura, consulte o [exemplo de integração de camada de Windows Forms visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) no github.

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução com Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) Studio.net
- [Interoperação com código não gerenciado](/dotnet/framework/interop/) Studio.net
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) UWP
- [Aprimore seu aplicativo de área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) UWP
- [Namespace Windows. UI. composição](/uwp/api/windows.ui.composition) UWP

## <a name="complete-code"></a>Código completo

Este é o código completo deste tutorial.

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
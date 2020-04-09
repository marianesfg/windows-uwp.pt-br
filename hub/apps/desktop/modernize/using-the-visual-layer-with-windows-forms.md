---
title: Usar a Camada visual com Windows Forms
description: Conheça técnicas para usar as APIs de Camada visual em combinação com o conteúdo do Windows Forms existente para criar animações e efeitos avançados.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999635"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Usar a Camada visual com Windows Forms

Você pode usar as APIs de Composição do Windows Runtime (também conhecidas como [Camada visual](/windows/uwp/composition/visual-layer)) em seus aplicativos Windows Forms para criar experiências modernas que se destaquem para os usuários do Windows 10.

O código completo para este tutorial está disponível no GitHub: [Exemplo de HelloComposition para Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem da UWP tem estes pré-requisitos.

- Estamos assumindo que você tem alguma familiaridade com o desenvolvimento de aplicativos usando Windows Forms e UWP. Para saber mais, veja:
  - [Introdução ao Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 ou posterior
- Windows 10, versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Como usar as APIs de Composição no Windows Forms

Neste tutorial, você criará uma interface do usuário do Windows Forms simples e adicionará elementos de Composição animados a ela. Embora os componentes do Windows Forms e de Composição continuem simples, o código de interoperabilidade mostrado será o mesmo independentemente da complexidade dos componentes. O aplicativo concluído tem esta aparência.

![A interface do usuário do aplicativo em execução](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Criar um projeto no Windows Forms

Primeiro, você precisará criar o projeto de aplicativo Windows Forms, que inclui a definição do aplicativo e do formulário principal da interface do usuário.

Para criar um projeto de aplicativo Windows Forms no Visual C# chamado _HelloComposition_:

1. Abra o Visual Studio e selecione **Arquivo** > **Novo** > **Projeto**.<br/>A caixa de diálogo **Novo Projeto** é aberta.
1. Na categoria **Instalado**, expanda o nó **Visual C#** e selecione **Área de trabalho do Windows**.
1. Selecione o modelo de **aplicativo Windows Forms (.NET Framework)** .
1. Nomeie-o como _HelloComposition_, selecione a estrutura **.NET Framework 4.7.2** e clique em **OK**.

O Visual Studio cria o projeto e abre o designer para a janela de aplicativo padrão chamada Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar APIs do Windows Runtime

Para usar as APIs do WinRT (Windows Runtime) em seu aplicativo Windows Forms, será preciso configurar seu projeto do Visual Studio para acessar o WinRT. Além disso, os vetores são usados amplamente pelas APIs de Composição, portanto, é preciso adicionar as referências necessárias para usar vetores.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Criar um controle personalizado para gerenciar a interoperabilidade

Para hospedar o conteúdo criado com a camada visual, crie um controle personalizado derivado de [Control](/dotnet/api/system.windows.forms.control). Esse controle fornece acesso a uma janela de [Handle](/dotnet/api/system.windows.forms.control.handle), necessária para criar o contêiner para o conteúdo da camada visual.

É aqui que a maior parte da configuração para hospedagem de APIs de Composição é feita. Nesse controle, são usados [PInvoke (Serviços de Invocação de Plataforma)](/cpp/dotnet/calling-native-functions-from-managed-code) e [Interoperabilidade COM](/dotnet/api/system.runtime.interopservices.comimportattribute) para trazer APIs de Composição para o aplicativo Windows Forms. Para saber mais sobre PInvoke e Interoperabilidade COM, confira [Interoperabilidade com código não gerenciado](/dotnet/framework/interop/index).

> [!TIP]
> Se necessário, verifique o código completo no final do tutorial para garantir que todo o código esteja nos locais certos enquanto você trabalha no tutorial.

1. Adicione um novo arquivo de controle personalizado ao seu projeto que derive de [Control](/dotnet/api/system.windows.forms.control).
    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
    - No menu de contexto, selecione **Adicionar** > **Novo Item...** .
    - Na caixa de diálogo **Adicionar Novo Item**, selecione **Controle Personalizado**.
    - Nomeie o controle como _CompositionHost.cs_ e clique em **Adicionar**. O CompositionHost.cs é aberto no modo de exibição de Design.

1. Alterne para o modo de exibição de código no CompositionHost.cs e adicione o código a seguir à classe.

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

    No construtor, são chamados os métodos _InitializeCoreDispatcher_ e _InitComposition_. Você criará esses métodos nas próximas etapas.

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

1. Inicialize um thread com um [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). O dispatcher principal é responsável por processar mensagens de janela e expedir eventos para APIs do WinRT. As novas instâncias do **Compositor** devem ser criadas em um thread contendo um CoreDispatcher.
    - Crie um método chamado _InitializeCoreDispatcher_ e adicione o código para configurar a fila do dispatcher.

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

    - A fila do dispatcher requer uma declaração PInvoke. Coloque essa declaração no final do código para a classe. (Colocamos esse código dentro de uma região para manter o código da classe organizado.)

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

    Agora que a fila do dispatcher está pronta, você pode começar a inicializar e criar o conteúdo de Composição.

1. Inicialize o [Compositor](/uwp/api/windows.ui.composition.compositor). O Compositor é uma fábrica que cria uma variedade de tipos no namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition), abrangendo a camada visual, o sistema de efeitos e o sistema de animação. A classe Compositor também gerencia a vida útil dos objetos criados na fábrica.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Criar um controle personalizado para hospedar elementos de composição

Recomendamos colocar o código que gera e gerencia seus elementos de composição em um controle separado que derive de CompositionHost. Isso faz com que o código de interoperabilidade criado na classe CompositionHost seja reutilizável.

Aqui, você cria um controle personalizado derivado de CompositionHost. Esse controle é adicionado à caixa de ferramentas do Visual Studio para que você possa adicioná-lo a seu formulário.

1. Adicione um novo arquivo de controle personalizado a seu projeto que derive de CompositionHost.
    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
    - No menu de contexto, selecione **Adicionar** > **Novo Item...** .
    - Na caixa de diálogo **Adicionar Novo Item**, selecione **Controle Personalizado**.
    - Nomeie o controle como _CompositionHostControl.cs_ e clique em **Adicionar**. O CompositionHostControl.cs é aberto no modo de exibição de Design.

1. No painel Propriedades do modo de exibição de Design de CompositionHostControl.cs, defina a propriedade **BackColor** como **ControlLight**.

    A configuração da cor de fundo é opcional. Fazemos isso aqui para que você possa ver seu controle personalizado na tela de fundo do formulário.

1. Alterne para o modo de exibição de código no CompositionHostControl.cs e atualize a declaração de classe para que ela derive de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Atualize o construtor de modo a chamar o construtor base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Adicionar elementos de composição

Com a infraestrutura em vigor, agora você pode adicionar o conteúdo de Composição à interface do usuário do aplicativo.

Neste exemplo, o código é adicionado à classe CompositionHostControl, que cria e anima um [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) simples.

1. Adicione um elemento de composição.

    No CompositionHostControl.cs, adicione estes métodos à classe CompositionHostControl.

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

Agora que há um controle personalizado para hospedar o conteúdo de Composição, é possível adicioná-lo à interface do usuário do aplicativo. Aqui, você adiciona uma instância do CompositionHostControl, criado na etapa anterior. O CompositionHostControl é adicionado automaticamente à caixa de ferramentas do Visual Studio em **_nome do projeto_ Componentes**.

1. Adicione um botão à interface do usuário no modo de exibição de Design de Form1.CS.

    - Arraste um botão da caixa de ferramentas para o Form1. Coloque-o no canto superior esquerdo do formulário. (Confira a imagem no início do tutorial para verificar o posicionamento dos controles.)
    - No painel Propriedades, altere a propriedade **Text** de _button1_ para _Adicionar elemento de composição_.
    - Redimensione o botão para que todo o texto seja mostrado.

    (Para saber mais, confira [Como adicionar controles ao Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Adicione um CompositionHostControl à interface do usuário.

    - Arraste um CompositionHostControl da caixa de ferramentas para o Form1. Coloque-o à direita do botão.
    - Redimensione o CompositionHost para que ele preencha o restante do formulário.

1. Manipule o evento de clique no botão.

   - No painel Propriedades, clique no ícone de relâmpago a fim de alternar para o modo de exibição de Eventos.
   - Na lista exibida, selecione o evento de **Clique**, digite *Button_Click* e pressione Enter.
   - O seguinte código é adicionado ao Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Adicione código ao manipulador de clique de botão para criar novos elementos.

    - Em Form1.cs, adicione código ao manipulador de eventos *Button_Click* que você criou anteriormente. Este código chama _CompositionHostControl1.AddElement_ para criar um novo elemento com um tamanho e deslocamento gerados aleatoriamente. (A instância de CompositionHostControl é nomeada automaticamente como _compositionHostControl1_ quando arrastada para o formulário.)

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

Agora você pode compilar e executar seu aplicativo Windows Forms. Ao clicar no botão, você deverá ver quadrados animados adicionados à interface do usuário.

## <a name="next-steps"></a>Próximas etapas

Para ver um exemplo mais completo desenvolvido com a mesma infraestrutura, confira o [Exemplo de integração de Camada visual do Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) no GitHub.

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução ao Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interoperação com código não gerenciado](/dotnet/framework/interop/) (.NET)
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) (UWP)
- [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Namespace Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

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
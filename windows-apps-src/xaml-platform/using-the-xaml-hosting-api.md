---
description: Este artigo descreve como hospedar o XAML de UWP da interface do usuário em seu aplicativo da área de trabalho.
title: Usando o XAML UWP API de hospedagem em um aplicativo da área de trabalho
ms.date: 01/11/2019
ms.topic: article
keywords: Windows 10, uwp, do windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: efd7dc687b9aba2e3c07b0afefa2e4fa49b882b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618831"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usando o XAML UWP API de hospedagem em um aplicativo da área de trabalho

> [!NOTE]
> As Ilhas XAML e a API de hospedagem do XAML UWP estão disponíveis atualmente como uma visualização do desenvolvedor. Embora incentivamos você a experimentá-las em seu próprio código de protótipo agora, não recomendamos que você usá-los no código de produção neste momento. Esses recursos continuará a amadurecer e se estabilizar em futuras versões do Windows. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.
>
> Se você tiver comentários sobre Ilhas XAML, crie um novo problema nos [WindowsCommunityToolkit repositório](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) e deixe seus comentários lá. Se você preferir a enviar seus comentários em particular, você pode enviá-lo para XamlIslandsFeedback@microsoft.com. Seus insights e os cenários são extremamente importantes para nós.

A partir do SDK de visualização do Windows 10 Insider build 17709, aplicativos de área de trabalho não UWP (incluindo aplicativos C++ do Win32, Windows Forms e WPF) podem usar o *XAML UWP API de hospedagem* para hospedar controles da UWP em qualquer elemento de interface do usuário que está associado com um identificador de janela (HWND). Essa API permite que os aplicativos de área de trabalho não UWP usar os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles da UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [Fluent Design System](../design/fluent-design-system/index.md) e suporte [Windows Ink](../design/input/pen-and-stylus-interactions.md).

O XAML UWP API de hospedagem fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam de interface do usuário Fluent para não UWP aplicativos da área de trabalho. Esse cenário às vezes é chamado *Ilhas XAML*. Para obter mais detalhes sobre esse cenário de desenvolvedor, consulte [controles UWP em aplicativos da área de trabalho](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>É o XAML UWP API de hospedagem ideal para seu aplicativo da área de trabalho?

O XAML UWP API de hospedagem fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho tem a opção de usar APIs alternativas, mais convenientes para atingir essa meta.  

* Se você tiver um aplicativo de desktop do Win32 em C++ e você deseja hospedar controles da UWP em seu aplicativo, você deve usar o XAML UWP API de hospedagem. Não há nenhum alternativas para esses tipos de aplicativos.

* Para aplicativos WPF e Windows Forms, é recomendável que você use o [encapsulado controles](xaml-host-controls.md#wrapped-controls) e [hospedar controles](xaml-host-controls.md#host-controls) no Kit de ferramentas de comunidade do Windows em vez da XAML UWP API de hospedagem. Esses controles usam o XAML UWP API de hospedagem internamente e fornecem uma experiência de desenvolvimento mais simples. No entanto, você pode usar o XAML UWP API diretamente nesses tipos de aplicativos de hospedagem, se você escolher.

## <a name="related-samples"></a>Exemplos relacionados

A maneira de você usar o XAML UWP API de hospedagem em seu código depende do tipo de aplicativo, o design do seu aplicativo e outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código de exemplos a seguir.

### <a name="c-win32"></a>C++ Win32

Há vários exemplos que demonstram como usar o XAML UWP API de hospedagem em um aplicativo Win32 C++ no GitHub:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Este exemplo demonstra como adicionar a UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), e [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) controles a um aplicativo Win32 em C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Este exemplo demonstra como adicionar vários controles UWP básicos para um aplicativo Win32 em C++ e manipular as alterações DPI.

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) controle no Kit de ferramentas da comunidade Windows atua como um exemplo de referência para usar a API de hospedagem em aplicativos WPF e Windows Forms UWP. O código-fonte está disponível nos seguintes locais:

  * Para a versão do controle do WPF [aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF deriva [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * Para obter a versão do Windows Forms do controle, [aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão do Windows Forms deriva [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Pré-requisitos

O XAML UWP API de hospedagem tem esses pré-requisitos.

* Windows 10 Insider Preview compilar 17709 (ou posterior) e a compilação correspondente do Insider Preview do SDK do Windows. Como esse é um recurso em constante evolução, para uma melhor experiência é recomendável usar a compilação mais recente disponível.

* Para usar o XAML UWP API de hospedagem em seu aplicativo da área de trabalho, você precisará configurar seu projeto para que você possa chamar as APIs da UWP.

    * **C++ Win32:** É recomendável que você configure seu projeto para usar [C + + c++ /CLI WinRT](../cpp-and-winrt-apis/index.md). Para obter instruções, consulte [modificar um projeto de aplicativo de área de trabalho do Windows para adicionar o C + + c++ /CLI WinRT suporte](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

    * **Windows Forms e WPF:** Siga [estas instruções](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Arquitetura de ilhas XAML

Inclui o XAML UWP API de hospedagem [ **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [ **WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)e outros tipos relacionados no [ **Windows.UI.Xaml.Hosting** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) namespace. Aplicativos da área de trabalho podem usar essa API para renderizar controles UWP e navegação de foco do teclado para dentro e fora os elementos de rota. Aplicativos da área de trabalho também podem dimensionar e posicionar os controles UWP conforme desejado.

Quando você cria uma ilha XAML usando a API de hospedagem em um aplicativo da área de trabalho de XAML, você terá a seguinte hierarquia de objetos:

* No nível de base é o elemento de interface do usuário em seu aplicativo no qual você deseja hospedar a ilha XAML. Este elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário na qual você pode hospedar uma ilha XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos do WPF [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicativos do Windows Forms e uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicativos Win32 em C++.

* No próximo nível é um **DesktopWindowXamlSource** objeto. Esse objeto fornece a infraestrutura para hospedar a ilha XAML. Seu código é responsável por criar este objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativo para hospedar o controle UWP. Essa janela filho nativo basicamente é abstraída de seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior é o controle UWP que você deseja hospedar em seu aplicativo da área de trabalho. Isso pode ser qualquer objeto UWP que deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecida pelo SDK do Windows, bem como controles de usuário personalizada.

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha XAML.

![Arquitetura de DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Como hospedar controles UWP XAML

Aqui estão as etapas principais para usar o XAML UWP API de hospedagem para hospedar um controle do UWP em seu aplicativo.

1. Inicializar a estrutura UWP XAML para o thread atual antes de seu aplicativo cria qualquer um dos [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objetos que ele irá hospedar no [  **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Se seu aplicativo cria o **DesktopWindowXamlSource** objeto antes de criar qualquer um dos **Windows.UI.Xaml.UIElement** objetos, essa estrutura será inicializada para você quando você instancia o **DesktopWindowXamlSource** objeto. Nesse cenário, você não precisa adicionar nenhum código de sua preferência para inicializar a estrutura.

    * No entanto, se seu aplicativo cria o **Windows.UI.Xaml.UIElement** objetos antes de criar o **DesktopWindowXamlSource** objeto que será hospedá-los, seu aplicativo deve chamar estático[ **WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método para inicializar explicitamente a estrutura UWP XAML antes do **Windows.UI.Xaml.UIElement** são objetos criar uma instância. Seu aplicativo normalmente deve chamar esse método quando o elemento de interface do usuário pai que hospeda o **DesktopWindowXamlSource** é instanciado.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Esse método retorna um [ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) objeto que contém uma referência para a estrutura UWP XAML. Você pode criar tantos **WindowsXamlManager** objetos quanto você deseja em um determinado thread. No entanto, porque cada objeto contém uma referência para a estrutura XAML UWP, você deve descartar os objetos para garantir que os recursos XAML, eventualmente, sejam liberados.

2. Criar uma [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) do objeto e anexá-lo a um elemento de interface do usuário pai em seu aplicativo que está associado com um identificador de janela.

    Para fazer isso, você precisará seguir estas etapas:

    1. Criar uma **DesktopWindowXamlSource** do objeto e convertê-lo para o **IDesktopWindowXamlSourceNative** interface COM. Essa interface é declarada no ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` arquivo de cabeçalho no SDK do Windows. Em um projeto do Win32 em C++, você pode referenciar esse arquivo de cabeçalho diretamente. Em um projeto WPF ou Windows Forms, você precisará declarar esta interface em seu código de aplicativo usando o [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) atributo. Certifique-se de sua declaração de interface corresponde exatamente a declaração de interface em ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Chamar o **AttachToWindow** método o **IDesktopWindowXamlSourceNative** de interface e passa o identificador de janela do elemento pai da interface do usuário em seu aplicativo.

    3. Defina o tamanho inicial da janela filho internos contido na **DesktopWindowXamlSource**. Por padrão, essa janela filho interno é definida para uma largura e altura de 0. Se você não definir o tamanho da janela, todos os controles UWP Adicionar para o **DesktopWindowXamlSource** não estarão visíveis. Para acessar a janela filho interno na **DesktopWindowXamlSource**, use o **WindowHandle** propriedade do **IDesktopWindowXamlSourceNative** interface. Os exemplos a seguir usam o [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) função para definir o tamanho da janela.

    Aqui estão alguns exemplos de código que demonstram esse processo.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI elemnt for your XAML island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. Defina a **Windows.UI.Xaml.UIElement** você deseja hospedar para o [ **conteúdo** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propriedade do seu **DesktopWindowXamlSource** objeto. O exemplo a seguir define uma [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) denominada ```myGrid``` para o **conteúdo** propriedade.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obter exemplos completos que demonstram essas tarefas no contexto de um aplicativo de exemplo funcional, consulte os arquivos de código a seguir:

  * **C++ Win32:** Consulte a [cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) arquivo na [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) exemplo ou o [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) de arquivo no [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemplo.
  * **WPF:** Consulte a [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) arquivos no Kit de ferramentas de comunidade do Windows.  
  * **Windows Forms:** Consulte a [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) arquivos no Kit de ferramentas de comunidade do Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Como para o host personalizado XAML UWP controles

> [!IMPORTANT]
> Atualmente, os controles UWP XAML personalizados de partes 3ª só têm suporte em C# aplicativos WPF e Windows Forms. Portanto, você pode compilar em relação a ela em seu aplicativo, você deve ter o código-fonte para os controles.

Se você quiser hospedar um controle XAML UWP personalizado (um controle que você defina você mesmo ou um controle fornecido por um dia 3 de terceiros), você deve executar as seguintes tarefas adicionais além do processo descrito na [seção anterior](#how-to-host-uwp-xaml-controls).

1. Definir um tipo personalizado que deriva [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) e também implementa [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Esse tipo atua como um provedor de metadados raiz para carregar os metadados para tipos de XAML UWP personalizados em assemblies no diretório atual do seu aplicativo.

    Para obter um exemplo que demonstra como fazer isso, consulte a [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) arquivo de código no Kit de ferramentas de comunidade do Windows. Este arquivo faz parte da implementação de compartilhado a **WindowsXamlHost** classes para o WPF e Windows Forms, que ajudam a ilustrar como usar o XAML UWP API nesses tipos de aplicativos de hospedagem.

2. Chame o [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) método do seu provedor de metadados raiz quando o nome do tipo do controle XAML UWP é atribuído (Isso pode ser atribuído no código em tempo de execução, ou você pode optar por habilitá-lo para ser atribuído na janela Propriedades do Visual Studio).

    Para obter um exemplo que demonstra como fazer isso, consulte a [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) arquivo de código no Kit de ferramentas de comunidade do Windows. Este arquivo faz parte da implementação de compartilhado a **WindowsXamlHost** classes para WPF e Windows Forms.

3. Integrar o código-fonte para o controle personalizado do UWP XAML em sua solução de aplicativo de host, criar o controle personalizado e usá-lo em seu aplicativo seguindo [estas instruções](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Como lidar com navegação de foco do teclado

Quando o usuário navega por meio dos elementos de interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **guia** ou a tecla de seta de direção /), você precisará mover o foco por meio de programação dentro e fora do  **DesktopWindowXamlSource** objeto. Quando a navegação de teclado do usuário atinge o **DesktopWindowXamlSource**, mover o foco para a primeira [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objeto na ordem de navegação para sua interface do usuário, continue para mover o foco para o seguinte **Windows.UI.Xaml.UIElement** objetos como o usuário alterna entre os elementos e, em seguida, mover o foco de volta do **DesktopWindowXamlSource**e no elemento pai da interface do usuário.  

O XAML UWP API de hospedagem fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

1. Quando a navegação pelo teclado entra em seu **DesktopWindowXamlSource**, o [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipular esse evento e por meio de programação move o foco para o primeiro hospedados **Windows.UI.Xaml.UIElement** usando o [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) método.

2. Quando o usuário estiver no último elemento focalizável na sua **DesktopWindowXamlSource** e pressiona o **guia** chave ou uma tecla de direção, o [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é gerado. Manipular esse evento e mover o foco para o próximo elemento focalizável no aplicativo host por meio de programação. Por exemplo, em um aplicativo WPF no qual o **DesktopWindowXamlSource** está hospedado em um [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) método para transferir o foco para o próximo elemento focalizável no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os arquivos de código a seguir:
  * **WPF:** Consulte a [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) arquivo no Kit de ferramentas de comunidade do Windows.  
  * **Windows Forms:** Consulte a [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) arquivo no Kit de ferramentas de comunidade do Windows.

## <a name="how-to-handle-layout-changes"></a>Como manipular as alterações de layout

Quando o usuário altera o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que os controles UWP exibir conforme o esperado. Aqui estão alguns cenários importantes a considerar.

1. Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessário para se ajustarem a **Windows.UI.Xaml.UIElement** que você estiver hospedando na **DesktopWindowXamlSource**, chame o [ **Medida** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) método da **Windows.UI.Xaml.UIElement**. Por exemplo:
    * Em um aplicativo WPF, você pode fazer isso a partir de [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) método do [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o  **DesktopWindowXamlSource**.
    * Em um aplicativo Windows Forms, você pode fazer isso a partir de [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) método da [ **controle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**.

2. Quando altera o tamanho do elemento pai da interface do usuário, chame o [ **Arrange** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) método da raiz **Windows.UI.Xaml.UIElement** que você estiver hospedando no  **DesktopWindowXamlSource**. Por exemplo:
    * Em um aplicativo WPF, você pode fazer isso a partir de [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) método da [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objeto que hospeda o **DesktopWindowXamlSource**.
    * Em um aplicativo Windows Forms, você pode fazer isso do manipulador para o [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) evento do [ **controle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os arquivos de código a seguir:
  * **WPF:** Consulte a [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.  
  * **Windows Forms:** Consulte a [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.

## <a name="how-to-handle-dpi-changes"></a>Como manipular as alterações DPI

Se você quiser manipular as alterações DPI na janela que hospeda seu UWP de controle (por exemplo, se o usuário arrasta a janela entre monitores de DPI da tela diferente), você precisará configurar o controle UWP com uma transformação de renderização, as alterações de DPI em seu aplicativo e atualize a posição da janela e renderizar a transformação do controle UWP em resposta às alterações DPI.

As etapas a seguir ilustram uma maneira de lidar com esse processo no contexto de um aplicativo Win32 em C++. Para obter um exemplo completo, consulte o [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) e [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) arquivos de código do [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemplo no GitHub.

1. Manter uma [ **ScaleTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) do objeto em seu aplicativo e atribuí-lo para o [ **RenderTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) método do seu controle UWP. O exemplo a seguir faz isso para um [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) controle em um aplicativo Win32 em C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. No seu [ **WindowProc** ](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) funcionar, ouvir as [ **WM_DPICHANGED** ](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) mensagem. Em resposta a esta mensagem:
    * Use o [ **SetWindowPos** ](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) função para redimensionar a janela que contém o controle UWP para o retângulo passado à mensagem.
    * Atualização dos eixos x e y dimensionar fatores de sua **ScaleTransform** objeto de acordo com o novo valor DPI.
    * Fazer os ajustes necessários para a aparência e o layout do seu controle UWP. O exemplo de código a seguir ajusta a [ **preenchimento** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) de hospedado **Windows.UI.Xaml.Controls.Grid** controle em resposta às alterações DPI.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. Para configurar seu aplicativo para reconhecerem o DPI por monitor, adicione uma [manifesto do assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) em seu projeto e defina ```<dpiAwareness>``` elemento ```PerMonitorV2```. Para obter mais informações sobre esse valor, consulte a descrição para o [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <application xmlns="urn:schemas-microsoft-com:asm.v3">
            <windowsSettings>
                <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
            </windowsSettings>
        </application>
    </assembly>
    ```

    Para um manifesto do assembly lado a lado de exemplo completo, consulte o [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) arquivo na [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemplo no GitHub.

## <a name="limitations"></a>Limitações

A API de hospedagem de XAML compartilha as mesmas limitações que todos os outros tipos de controles de host do XAML para Windows 10. Para obter uma lista detalhada, consulte [limitações de controle de host do XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar o XAML UWP API de hospedagem em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "Não é possível ativar DesktopWindowXamlSource. Esse tipo não pode ser usado em um aplicativo UWP." ou "não é possível ativar WindowsXamlManager. Esse tipo não pode ser usado em um aplicativo UWP." | Esse erro indica que você está tentando usar o XAML UWP API de hospedagem (especificamente, você está tentando criar uma instância de [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) tipos) em um aplicativo UWP. O XAML UWP API de hospedagem é destinada apenas a ser usado em aplicativos da área de trabalho não UWP, como aplicativos C++ do Win32, Windows Forms e WPF. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo é chamado de **IDesktopWindowXamlSourceNative.AttachToWindow** método e passado a ele o HWND de uma janela que foi criado em um thread diferente. Você deve passar o HWND de uma janela que foi criado no mesmo thread que o código do qual você está chamando o método para esse método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "Falha no método AttachToWindow porque o HWND especificado descende de uma janela de nível superior diferente que o HWND que anteriormente foi passado para AttachToWindow no mesmo thread." | Esse erro indica que seu aplicativo é chamado de **IDesktopWindowXamlSourceNative.AttachToWindow** método e passado a ele o HWND de uma janela que descenda de uma janela de nível superior diferente que uma janela que você especificou em um chamada anterior a esse método no mesmo thread.</p></p>Depois de seu aplicativo chamará **IDesktopWindowXamlSourceNative.AttachToWindow** em um determinado thread, todas as outras [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objetos na mesma thread pode anexar somente para windows que são descendentes da mesma janela de nível superior que foi passado na primeira chamada para **IDesktopWindowXamlSourceNative.AttachToWindow**. Quando todos os as **DesktopWindowXamlSource** objetos estão fechados para um determinado thread, a próxima **DesktopWindowXamlSource** , em seguida, é gratuito para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os **DesktopWindowXamlSource** objetos que estão associadas a outras janelas de nível superior neste thread ou criar um novo thread para essa **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Controles da UWP em aplicativos da área de trabalho](xaml-host-controls.md)

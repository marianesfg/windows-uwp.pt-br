---
description: Este artigo descreve como hospedar o XAML de UWP da interface do usuário em seu aplicativo da área de trabalho.
title: Usando o XAML UWP API de hospedagem em um aplicativo da área de trabalho
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, uwp, do windows forms, wpf, win32, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9b318b922541180108dfdd053ba28ce98ad9ebcb
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985006"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usando o XAML UWP API de hospedagem em um aplicativo da área de trabalho

A partir do Windows 10, versão 1903, aplicativos de área de trabalho não UWP (incluindo WPF, Windows Forms, e C++ aplicativos Win32) pode usar o *XAML UWP API de hospedagem* para hospedar controles da UWP em qualquer elemento de interface do usuário que está associado com um Identificador de janela (HWND). Essa API permite que os aplicativos de área de trabalho não UWP usar os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles da UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [Fluent Design System](/windows/uwp/design/fluent-design-system/index) e suporte [Windows Ink](/windows/uwp/design/pen-and-stylus-interactions).

O XAML UWP API de hospedagem fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam de interface do usuário Fluent para não UWP aplicativos da área de trabalho. Esse recurso é chamado *XAML Ilhas*. Para obter uma visão geral desse recurso, consulte [controles UWP em aplicativos da área de trabalho](xaml-islands.md).

> [!NOTE]
> Se você tiver comentários sobre ilhas de XAML, crie um novo problema nos [Microsoft.Toolkit.Win32 repositório](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se você preferir a enviar seus comentários em particular, você pode enviá-lo para XamlIslandsFeedback@microsoft.com. Seus insights e os cenários são extremamente importantes para nós.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Você deve usar o XAML UWP API de hospedagem?

O XAML UWP API de hospedagem fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho tem a opção de usar APIs alternativas, mais convenientes para atingir essa meta.  

* Se você tiver um aplicativo de desktop do Win32 em C++ e você deseja hospedar controles da UWP em seu aplicativo, você deve usar o XAML UWP API de hospedagem. Não há nenhum alternativas para esses tipos de aplicativos.

* Para aplicativos WPF e Windows Forms, é altamente recomendável que você use o [encapsulado controles](xaml-islands.md#wrapped-controls) e [hospedar controles](xaml-islands.md#host-controls) no Kit de ferramentas de comunidade do Windows em vez de usar o XAML UWP hospeda a API diretamente. Esses controles usam o XAML UWP API de hospedagem internamente e implementam todo o comportamento que seriam necessários para lidar com você mesmo se você tiver usado o XAML UWP API diretamente, incluindo alterações de layout e navegação de teclado de hospedagem. No entanto, você pode usar o XAML UWP API diretamente nesses tipos de aplicativos de hospedagem, se você escolher.

Este artigo descreve como usar o XAML UWP API de hospedagem diretamente em seu aplicativo em vez de controles fornecidos pelo Kit de ferramentas de comunidade do Windows.

## <a name="prerequisites"></a>Pré-requisitos

O XAML UWP API de hospedagem tem estes pré-requisitos:

* Windows 10, versão 1903 (ou posterior) e correspondente de compilação do SDK do Windows.
* Configurar seu projeto para usar APIs do Windows Runtime e habilitar XAML Ilhas seguindo [estas instruções](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Arquitetura de ilhas de XAML

O XAML UWP API de hospedagem inclui esses tipos principais de tempo de execução do Windows e interfaces COM:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Essa classe representa a estrutura UWP XAML. Essa classe fornece um único estático [ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método que inicializa a estrutura UWP XAML no thread atual no aplicativo da área de trabalho.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Essa classe representa uma instância de conteúdo XAML UWP que você estiver hospedando em seu aplicativo da área de trabalho. O membro mais importante dessa classe é o [ **conteúdo** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propriedade. Você atribui essa propriedade como uma [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que você deseja hospedar. Essa classe também tem outros membros para navegação de foco do teclado roteamento dentro e fora de ilhas do XAML.

* **IDesktopWindowXamlSourceNative** e **IDesktopWindowXamlSourceNative2** interfaces COM. **IDesktopWindowXamlSourceNative** fornece a **AttachToWindow** método, que você usa para anexar uma ilha de XAML em seu aplicativo para um elemento de interface do usuário do pai. **IDesktopWindowXamlSourceNative2** fornece a **PreTranslateMessage** método, que permite que o framework de UWP XAML processar determinadas mensagens do Windows corretamente.
    > [!NOTE]
    > Essas interfaces são declaradas na **windows.ui.xaml.hosting.desktopwindowxamlsource.h** arquivo de cabeçalho no SDK do Windows. Por padrão, esse arquivo está em % programfiles (x86) %\Windows Kits\10\Include\\< número da compilação\>\um. Em um projeto do Win32 em C++, você pode referenciar esse arquivo de cabeçalho diretamente. Em um projeto WPF ou Windows Forms, você precisará declarar as interfaces em seu código de aplicativo usando o [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) atributo. Certifique-se de suas declarações de interface correspondem exatamente às declarações **windows.ui.xaml.hosting.desktopwindowxamlsource.h**.

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha de XAML que está hospedado em um aplicativo da área de trabalho.

![Arquitetura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

* No nível de base é o elemento de interface do usuário em seu aplicativo no qual você deseja hospedar a ilha de XAML. Este elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário na qual você pode hospedar uma ilha de XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos do WPF [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicativos do Windows Forms e uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para C++ aplicativos Win32.

* No próximo nível é um **DesktopWindowXamlSource** objeto. Esse objeto fornece a infraestrutura para hospedar a ilha de XAML. Seu código é responsável por criar este objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativo para hospedar o controle UWP. Essa janela filho nativo basicamente é abstraída de seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior é o controle UWP que você deseja hospedar em seu aplicativo da área de trabalho. Isso pode ser qualquer objeto UWP que deriva de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecida pelo SDK do Windows, bem como controles de usuário personalizada.

## <a name="related-samples"></a>Exemplos relacionados

A maneira de você usar o XAML UWP API de hospedagem em seu código depende do tipo de aplicativo, o design do seu aplicativo e outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código de exemplos a seguir.

### <a name="c-win32"></a>C++ Win32

[C++Exemplo do Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island). Este exemplo demonstra uma implementação completa de hospedando um controle de usuário do UWP em um desagrupadas C++ aplicativo do Win32 (ou seja, um aplicativo que não é compilado em um pacote MSIX).

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) controle no Kit de ferramentas da comunidade Windows atua como um exemplo de referência para usar a API de hospedagem em aplicativos WPF e Windows Forms UWP. O código-fonte está disponível nos seguintes locais:

  * Para a versão do controle do WPF [aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF deriva [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Para obter a versão do Windows Forms do controle, [aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão do Windows Forms deriva [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Controles de host UWP XAML

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

    1. Criar uma **DesktopWindowXamlSource** do objeto e convertê-lo para o **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** interface COM.

    2. Chamar o **AttachToWindow** método o **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** de interface e passa o identificador de janela do elemento de interface do usuário pai em seu aplicativo.

    3. Defina o tamanho inicial da janela filho internos contido na **DesktopWindowXamlSource**. Por padrão, essa janela filho interno é definida para uma largura e altura de 0. Se você não definir o tamanho da janela, todos os controles UWP Adicionar para o **DesktopWindowXamlSource** não estarão visíveis. Para acessar a janela filho interno na **DesktopWindowXamlSource**, use o **WindowHandle** propriedade do **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** interface. Os exemplos a seguir usam o [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) função para definir o tamanho da janela.

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
    // that will act as the parent UI element for your XAML Island. It also assumes
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

  * **C++ Win32:** Consulte a [XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp) arquivo na [ C++ Win32 sample](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * **WPF:** Consulte a [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) arquivos no Kit de ferramentas de comunidade do Windows.  

  * **Windows Forms:** Consulte a [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) arquivos no Kit de ferramentas de comunidade do Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Lidar com navegação de entrada e o foco do teclado

Há várias coisas que seu aplicativo precisa fazer para lidar adequadamente com navegação de entrada e o foco do teclado quando ele hospeda ilhas de XAML.

### <a name="keyboard-input"></a>Entrada por teclado

Para tratar corretamente a entrada do teclado para cada ilha de XAML, seu aplicativo deve passar todas as mensagens do Windows para a estrutura XAML UWP para que determinadas mensagens podem ser processadas corretamente. Para fazer isso, em algum lugar no seu aplicativo que possa acessar o loop de mensagem, converta a **DesktopWindowXamlSource** objeto para cada ilha de XAML para um **IDesktopWindowXamlSourceNative2** interface COM. Em seguida, chame o **PreTranslateMessage** método dessa interface e passe a mensagem atual.

  * Um C++ aplicativo do Win32 pode chamar **PreTranslateMessage** diretamente em seu loop de mensagem principal. Por exemplo, consulte o [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61) arquivo de código na [ C++ Win32 sample](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * Um aplicativo WPF pode chamar **PreTranslateMessage** do manipulador de eventos para o [ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) eventos. Por exemplo, consulte o [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) arquivo no Kit de ferramentas de comunidade do Windows.

  * Um aplicativo do Windows Forms pode chamar **PreTranslateMessage** de uma substituição para o [ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) método. Por exemplo, consulte o [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) arquivo no Kit de ferramentas de comunidade do Windows.

### <a name="keyboard-focus-navigation"></a>Navegação de foco do teclado

Quando o usuário navega por meio dos elementos de interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **guia** ou a tecla de seta de direção /), você precisará mover o foco por meio de programação dentro e fora do  **DesktopWindowXamlSource** objeto. Quando a navegação de teclado do usuário atinge o **DesktopWindowXamlSource**, mover o foco para a primeira [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objeto na ordem de navegação para sua interface do usuário, continue para mover o foco para o seguinte **Windows.UI.Xaml.UIElement** objetos como o usuário alterna entre os elementos e, em seguida, mover o foco de volta do **DesktopWindowXamlSource**e no elemento pai da interface do usuário.  

O XAML UWP API de hospedagem fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

* Quando a navegação pelo teclado entra em seu **DesktopWindowXamlSource**, o [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipular esse evento e por meio de programação move o foco para o primeiro hospedados **Windows.UI.Xaml.UIElement** usando o [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) método.

* Quando o usuário estiver no último elemento focalizável na sua **DesktopWindowXamlSource** e pressiona o **guia** chave ou uma tecla de direção, o [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é gerado. Manipular esse evento e mover o foco para o próximo elemento focalizável no aplicativo host por meio de programação. Por exemplo, em um aplicativo WPF no qual o **DesktopWindowXamlSource** está hospedado em um [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) método para transferir o foco para o próximo elemento focalizável no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os arquivos de código a seguir:

  * **WPF:** Consulte a [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) arquivo no Kit de ferramentas de comunidade do Windows.  

  * **Windows Forms:** Consulte a [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) arquivo no Kit de ferramentas de comunidade do Windows.

## <a name="handle-layout-changes"></a>Lidar com as alterações de layout

Quando o usuário altera o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que os controles UWP exibir conforme o esperado. Aqui estão alguns cenários importantes a considerar.

* Em um C++ aplicativo do Win32, quando seu aplicativo lida com a mensagem WM_SIZE que ele pode reposicionar a ilha de XAML hospedado usando o [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) função. Por exemplo, consulte o [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) arquivo de código na [ C++ Win32 sample](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessário para se ajustarem a **Windows.UI.Xaml.UIElement** que você estiver hospedando na **DesktopWindowXamlSource**, chame o [ **Medida** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) método da **Windows.UI.Xaml.UIElement**. Por exemplo:

    * Em um aplicativo WPF, você pode fazer isso a partir de [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) método do [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o  **DesktopWindowXamlSource**. Por exemplo, consulte o [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.

    * Em um aplicativo Windows Forms, você pode fazer isso a partir de [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) método da [ **controle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Por exemplo, consulte o [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.

* Quando altera o tamanho do elemento pai da interface do usuário, chame o [ **Arrange** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) método da raiz **Windows.UI.Xaml.UIElement** que você estiver hospedando no  **DesktopWindowXamlSource**. Por exemplo:

    * Em um aplicativo WPF, você pode fazer isso a partir de [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) método da [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objeto que hospeda o **DesktopWindowXamlSource**. Por exemplo, consulte o [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.

    * Em um aplicativo Windows Forms, você pode fazer isso do manipulador para o [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) evento do [ **controle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Por exemplo, consulte o [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) arquivo no Kit de ferramentas de comunidade do Windows.

## <a name="handle-dpi-changes"></a>Lidar com alterações DPI

A estrutura de UWP XAML manipula automaticamente as alterações DPI para controles da UWP hospedados (por exemplo, quando o usuário arrasta a janela entre monitores de DPI da tela diferente). Para obter a melhor experiência, é recomendável que os formulários do Windows, WPF, ou C++ aplicativo Win32 está configurado para estar ciente o DPI por monitor.

Para configurar seu aplicativo para reconhecerem o DPI por monitor, adicione uma [manifesto do assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) em seu projeto e defina ```<dpiAwareness>``` elemento ```PerMonitorV2```. Para obter mais informações sobre esse valor, consulte a descrição para o [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Controles de host personalizado UWP XAML

> [!IMPORTANT]
> Para hospedar um controle XAML UWP personalizado, você deve ter o código-fonte para o controle para que você pode compilar em relação a ela em seu aplicativo.

Se você quiser hospedar um controle XAML UWP personalizado (um controle que você defina você mesmo ou um controle fornecido por um dia 3 de terceiros), você deve executar as seguintes tarefas adicionais além do processo descrito na [seção anterior](#host-uwp-xaml-controls).

1. Definir um tipo personalizado que deriva [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) e também implementa [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Esse tipo atua como um provedor de metadados raiz para carregar os metadados para tipos de XAML UWP personalizados em assemblies no diretório atual do seu aplicativo.

2. Chame o [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) método do seu provedor de metadados raiz quando o nome do tipo do controle XAML UWP é atribuído (Isso pode ser atribuído no código em tempo de execução, ou você pode optar por habilitá-lo para ser atribuído na janela Propriedades do Visual Studio).

    Para obter exemplos, consulte os arquivos de código a seguir:
      * **C++ Win32:** Consulte a [XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp) arquivo de código na [ C++ Win32 sample](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

      * **WPF e Windows Forms**: Consulte a [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) e [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) arquivos no Kit de ferramentas de comunidade do Windows de código. Esses arquivos fazem parte da implementação de compartilhado a **WindowsXamlHost** classes para o WPF e Windows Forms, que ajudam a ilustrar como usar o XAML UWP API nesses tipos de aplicativos de hospedagem.

3. Integrar o código-fonte para o controle personalizado do UWP XAML em sua solução de aplicativo de host, criar o controle personalizado e usá-lo em seu aplicativo. Para obter instruções para um aplicativo WPF ou Windows Forms, consulte [estas instruções](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control). Para obter um exemplo de um C++ aplicativo Win32, consulte o [Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup) e [MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp) projetos no [ C++ Win32 sample](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar o XAML UWP API de hospedagem em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "Não é possível ativar DesktopWindowXamlSource. Esse tipo não pode ser usado em um aplicativo UWP." ou "não é possível ativar WindowsXamlManager. Esse tipo não pode ser usado em um aplicativo UWP." | Esse erro indica que você está tentando usar o XAML UWP API de hospedagem (especificamente, você está tentando criar uma instância de [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) tipos) em um aplicativo UWP. O XAML UWP API de hospedagem é destinada apenas a ser usado em aplicativos da área de trabalho não UWP, como aplicativos C++ do Win32, Windows Forms e WPF. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo é chamado de **IDesktopWindowXamlSourceNative::AttachToWindow** método e passado a ele o HWND de uma janela que foi criado em um thread diferente. Você deve passar o HWND de uma janela que foi criado no mesmo thread que o código do qual você está chamando o método para esse método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "Falha no método AttachToWindow porque o HWND especificado descende de uma janela de nível superior diferente que o HWND que anteriormente foi passado para AttachToWindow no mesmo thread." | Esse erro indica que seu aplicativo é chamado de **IDesktopWindowXamlSourceNative::AttachToWindow** método e passado a ele o HWND de uma janela que descenda de uma janela de nível superior diferente que uma janela que você especificou em um chamada anterior a esse método no mesmo thread.</p></p>Depois de seu aplicativo chamará **IDesktopWindowXamlSourceNative::AttachToWindow** em um determinado thread, todas as outras [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objetos na mesma thread pode anexar somente para windows que são descendentes da mesma janela de nível superior que foi passado na primeira chamada para **IDesktopWindowXamlSourceNative::AttachToWindow**. Quando todos os as **DesktopWindowXamlSource** objetos estão fechados para um determinado thread, a próxima **DesktopWindowXamlSource** , em seguida, é gratuito para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os **DesktopWindowXamlSource** objetos que estão associadas a outras janelas de nível superior neste thread ou criar um novo thread para essa **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Controles da UWP em aplicativos da área de trabalho](xaml-islands.md)
* [C++Exemplo de ilhas de XAML do Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)

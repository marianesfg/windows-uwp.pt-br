---
description: Este artigo descreve como hospedar a interface do usuário do UWP XAML em seu aplicativo de área de trabalho.
title: Usando a API de hospedagem XAML do UWP em um aplicativo de área de trabalho
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601537"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usando a API de hospedagem XAML do UWP em um aplicativo de área de trabalho

A partir do Windows 10, versão 1903, aplicativos de área de trabalho não UWP (incluindo WPF, C++ Windows Forms e aplicativos Win32) podem usar a *API de hospedagem XAML do UWP* para hospedar controles UWP em qualquer elemento de interface do usuário que esteja associado a um identificador de janela (HWND). Essa API permite que aplicativos de área de trabalho não UWP usem os recursos mais recentes da interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [sistema de design fluente](/windows/uwp/design/fluent-design-system/index) e dão suporte ao [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

A API de hospedagem XAML do UWP fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam a interface do usuário fluente para aplicativos de área de trabalho não UWP. Esse recurso é chamado de *ilhas XAML*. Para obter uma visão geral desse recurso, consulte [controles UWP em aplicativos de área de trabalho](xaml-islands.md).

> [!NOTE]
> Se você tiver comentários sobre as ilhas XAML, crie um novo problema no [repositório Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se preferir enviar seus comentários de forma privada, você poderá enviá-los para XamlIslandsFeedback@microsoft.como. Suas ideias e cenários são extremamente importantes para nós.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Você deve usar a API de Hospedagem de XAML do UWP?

A API de hospedagem do UWP XAML fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos de área de trabalho. Alguns tipos de aplicativos de área de trabalho têm a opção de usar APIs alternativas e mais convenientes para atingir esse objetivo.  

* Se você tiver um C++ aplicativo de área de trabalho Win32 e quiser hospedar os controles UWP em seu aplicativo, deverá usar a API de hospedagem XAML do UWP. Não há alternativas para esses tipos de aplicativos.

* Para aplicativos do WPF e Windows Forms, é altamente recomendável que você use [controles encapsulados](xaml-islands.md#wrapped-controls) e controles de [host](xaml-islands.md#host-controls) no kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML do UWP diretamente. Esses controles usam a API de Hospedagem de XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisaria lidar se usou a API de hospedagem XAML do UWP diretamente, incluindo a navegação do teclado e as alterações de layout. No entanto, você pode usar a API de hospedagem XAML do UWP diretamente nesses tipos de aplicativos, se escolher.

Este artigo descreve como usar a API de hospedagem XAML do UWP diretamente em seu aplicativo em vez dos controles fornecidos pelo kit de ferramentas da Comunidade do Windows.

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem do UWP XAML tem estes pré-requisitos:

* Windows 10, versão 1903 (ou posterior) e a compilação correspondente do SDK do Windows.
* Configure seu projeto para usar Windows Runtime APIs e habilite as ilhas XAML seguindo [estas instruções](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Arquitetura de ilhas XAML

A API de hospedagem XAML do UWP inclui estes tipos de Windows Runtime principais e interfaces COM:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Essa classe representa a estrutura de XAML do UWP. Essa classe fornece um único método [**InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático que inicializa a estrutura de XAML do UWP no thread atual no aplicativo da área de trabalho.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Essa classe representa uma instância do conteúdo do UWP XAML que você está hospedando em seu aplicativo de área de trabalho. O membro mais importante dessa classe é a propriedade [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Atribua essa propriedade a um [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que você deseja hospedar. Essa classe também tem outros membros para direcionar a navegação de foco de teclado para dentro e fora das Ilhas XAML.

* Interfaces com **IDesktopWindowXamlSourceNative** e **IDesktopWindowXamlSourceNative2** . **IDesktopWindowXamlSourceNative** fornece o método **AttachToWindow** , que você usa para anexar uma ilha XAML em seu aplicativo a um elemento pai da interface do usuário. O **IDesktopWindowXamlSourceNative2** fornece o método **PreTranslateMessage** , que permite que a estrutura de XAML do UWP processe determinadas mensagens do Windows corretamente.
    > [!NOTE]
    > Essas interfaces são declaradas no arquivo de cabeçalho **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h** na SDK do Windows. Por padrão, esse arquivo está em% ProgramFiles (x86)% \ Windows\\Kits\10\Include < número\>de Build \um. Em um C++ projeto Win32, você pode fazer referência a esse arquivo de cabeçalho diretamente. Em um projeto do WPF ou Windows Forms, você precisará declarar as interfaces no código do aplicativo usando o atributo [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute). Verifique se suas declarações de interface correspondem exatamente às declarações em **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h**.

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha XAML hospedada em um aplicativo de área de trabalho.

![Arquitetura do DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

* No nível base está o elemento de interface do usuário em seu aplicativo em que você deseja hospedar a ilha XAML. Este elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário nos quais você pode hospedar uma ilha XAML incluem [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos WPF, [**System. Windows. Forms. Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para Windows Forms aplicativos e uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para para aplicativos C++ Win32.

* No próximo nível está um objeto **DesktopWindowXamlSource** . Esse objeto fornece a infraestrutura para hospedar a ilha XAML. Seu código é responsável por criar esse objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativa para hospedar seu controle UWP. Essa janela filho nativa é basicamente abstraida para fora do seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior está o controle UWP que você deseja hospedar em seu aplicativo de área de trabalho. Pode ser qualquer objeto UWP derivado de [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles de usuário personalizados.

## <a name="related-samples"></a>Exemplos relacionados

A maneira como você usa a API de hospedagem XAML do UWP em seu código depende do tipo de aplicativo, do design do seu aplicativo e de outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código dos exemplos a seguir.

### <a name="c-win32"></a>C++Win32

Exemplo do Win32. [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App) Este exemplo demonstra uma implementação completa de Hospedagem de um controle de usuário UWP em um aplicativo C++ Win32 não empacotado (ou seja, um aplicativo que não é compilado em um pacote MSIX).

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows atua como um exemplo de referência para usar a API de hospedagem UWP no WPF e Windows Forms aplicativos. O código-fonte está disponível nos seguintes locais:

  * Para a versão do WPF do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF deriva de [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Para a versão Windows Forms do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão Windows Forms deriva de [**System. Windows. Forms. Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Hospedar controles XAML UWP

Estas são as principais etapas para usar a API de Hospedagem de XAML do UWP para hospedar um controle UWP em seu aplicativo.

1. Inicialize a estrutura do UWP XAML para o thread atual antes que seu aplicativo crie qualquer um dos objetos [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que ele hospedará no [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Se seu aplicativo criar o objeto **DesktopWindowXamlSource** antes de criar qualquer um dos objetos **Windows. UI. XAML. UIElement** , essa estrutura será inicializada para você quando você criar uma instância do objeto **DesktopWindowXamlSource** . Nesse cenário, você não precisa adicionar nenhum código próprio para inicializar a estrutura.

    * No entanto, se seu aplicativo criar os objetos **Windows. UI. XAML. UIElement** antes de criar o objeto **DesktopWindowXamlSource** que os hospedará, seu aplicativo deverá chamar o static [ **O método WindowsXamlManager. InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explicitamente a estrutura do UWP XAML antes dos objetos **Windows. UI. XAML. UIElement** serem instanciados. Seu aplicativo normalmente deve chamar esse método quando o elemento pai da interface do usuário que hospeda o **DesktopWindowXamlSource** é instanciado.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Esse método retorna um objeto [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contém uma referência à estrutura de XAML do UWP. Você pode criar quantos objetos **WindowsXamlManager** desejar em um determinado thread. No entanto, como cada objeto contém uma referência à estrutura de XAML do UWP, você deve descartar os objetos para garantir que os recursos XAML sejam eventualmente liberados.

2. Crie um objeto [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) e anexe-o a um elemento pai da interface do usuário em seu aplicativo associado a um identificador de janela.

    Para fazer isso, você precisará seguir estas etapas:

    1. Crie um objeto **DesktopWindowXamlSource** e converta-o na interface com **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .

    2. Chame o método **AttachToWindow** da interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** e passe o identificador de janela do elemento pai da interface do usuário em seu aplicativo.

    3. Defina o tamanho inicial da janela filho interna contida no **DesktopWindowXamlSource**. Por padrão, essa janela filho interna é definida com uma largura e uma altura de 0. Se você não definir o tamanho da janela, todos os controles UWP adicionados ao **DesktopWindowXamlSource** não estarão visíveis. Para acessar a janela filho interna no **DesktopWindowXamlSource**, use a propriedade **WindowHandle** da interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** . Os exemplos a seguir usam a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para definir o tamanho da janela.

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

3. Defina o **Windows. UI. XAML. UIElement** que você deseja hospedar para a propriedade [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) do seu objeto **DesktopWindowXamlSource** . O exemplo a seguir define um [**Windows. UI. XAML. Controls. Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) chamado ```myGrid``` para a propriedade **Content** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obter exemplos completos que demonstram essas tarefas no contexto de um aplicativo de exemplo funcional, consulte os seguintes arquivos de código:

  * **C++Win32** Consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WFP** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.  

  * **Windows Forms:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Manipular a entrada de teclado e navegação de foco

Há várias coisas que seu aplicativo precisa fazer para lidar corretamente com a entrada de teclado e a navegação de foco quando hospeda ilhas XAML.

### <a name="keyboard-input"></a>Entrada por teclado

Para lidar corretamente com a entrada de teclado para cada ilha XAML, seu aplicativo deve passar todas as mensagens do Windows para a estrutura de XAML do UWP para que determinadas mensagens possam ser processadas corretamente. Para fazer isso, em algum lugar em seu aplicativo que possa acessar o loop de mensagem, converta o objeto **DesktopWindowXamlSource** para cada ilha XAML em uma interface com **IDesktopWindowXamlSourceNative2** . Em seguida, chame o método **PreTranslateMessage** dessa interface e passe a mensagem atual.

  * Um C++ aplicativo Win32 pode chamar **PreTranslateMessage** diretamente em seu loop de mensagem principal. Para obter um exemplo, consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * Um aplicativo WPF pode chamar **PreTranslateMessage** do manipulador de eventos para o evento [**ComponentDispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) no kit de ferramentas da Comunidade do Windows.

  * Um aplicativo Windows Forms pode chamar **PreTranslateMessage** de uma substituição para o método [**Control. PreprocessMessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) no kit de ferramentas da Comunidade do Windows.

### <a name="keyboard-focus-navigation"></a>Navegação de foco de teclado

Quando o usuário navega pelos elementos da interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **Tab** ou direção/tecla de seta), você precisará mover o foco de forma programática para dentro e para fora do objeto **DesktopWindowXamlSource** . Quando a navegação do teclado do usuário atinge o **DesktopWindowXamlSource**, mova o foco para o primeiro objeto [**Windows. UI. XAML. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) na ordem de navegação da sua interface do usuário, continue a mover o foco para o seguinte  **Objetos Windows. UI. XAML. UIElement** conforme o usuário percorre os elementos e, em seguida, move o foco de volta do **DesktopWindowXamlSource** e para o elemento pai da interface do usuário.  

A API de hospedagem XAML do UWP fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

* Quando a navegação do teclado entra em seu **DesktopWindowXamlSource**, o evento [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipule esse evento e mova o foco programaticamente para o primeiro **Windows. UI. XAML. UIElement** hospedado usando o método [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Quando o usuário está no último elemento focado em seu **DesktopWindowXamlSource** e pressiona a tecla **Tab** ou uma tecla de direção, o evento [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é gerado. Manipule esse evento e mova o foco programaticamente para o próximo elemento focado no aplicativo host. Por exemplo, em um aplicativo WPF em que o **DesktopWindowXamlSource** está hospedado em um [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o método [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir o foco para o próximo elemento focado no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os seguintes arquivos de código:

  * **WFP** Consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) no kit de ferramentas da Comunidade do Windows.  

  * **Windows Forms:** Consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) no kit de ferramentas da Comunidade do Windows.

  * **C++/Win32**: Consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

## <a name="handle-layout-changes"></a>Manipular alterações de layout

Quando o usuário alterar o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que seus controles UWP sejam exibidos como esperado. Aqui estão alguns cenários importantes a considerar.

* Em um C++ aplicativo Win32, quando seu aplicativo manipula a mensagem WM_SIZE, ele pode reposicionar a ilha XAML hospedada usando a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obter um exemplo, consulte o arquivo de código [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessária para ajustar o **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**, chame o método [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) do **Windows. UI. XAML. UIElement** . Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) do [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms você pode fazer isso a partir do método [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) do [**controle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

* Quando o tamanho do elemento pai da interface do usuário for alterado, chame o método [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) da raiz **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**. Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) do objeto [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms, você pode fazer isso a partir do manipulador para o evento [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) do [**controle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="handle-dpi-changes"></a>Tratar alterações de DPI

A estrutura de XAML do UWP lida com alterações de DPI para controles UWP hospedados automaticamente (por exemplo, quando o usuário arrasta a janela entre monitores com DPI de tela diferente). Para obter a melhor experiência, recomendamos que seu aplicativo Windows Forms, WPF C++ ou Win32 esteja configurado para ter reconhecimento de DPI por monitor.

Para configurar seu aplicativo para ter reconhecimento de DPI por monitor, adicione um [manifesto de assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e defina ```<dpiAwareness>``` o elemento como. ```PerMonitorV2``` Para obter mais informações sobre esse valor, consulte a descrição de [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Hospedar controles XAML do UWP personalizados

Siga estas etapas gerais para hospedar um controle XAML do UWP personalizado (um controle que você mesmo define ou um controle fornecido por um terceiro) em uma ilha XAML. Você deve ter o código-fonte do controle personalizado para que possa compilá-lo com seu aplicativo.

1. Na solução de aplicativo host, integre o código-fonte para o controle XAML personalizado do UWP e crie o controle personalizado.

2. Adicione um projeto de aplicativo UWP à solução de aplicativo host e adicione o pacote NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) a este projeto.

3. No projeto de aplicativo UWP, modifique sua `App` classe para que ela seja da classe **Microsoft. Toolkit. Win32. UI. XamlApplication** exposta pelo pacote NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) . Esse tipo atua como um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML em assemblies no diretório atual do seu aplicativo.

4. No construtor da sua `App` classe, chame o método **Initialize** da classe base **Microsoft. Toolkit. Win32. UI. XamlApplication** .

5. No projeto de aplicativo host, siga o processo descrito na [seção anterior](#host-uwp-xaml-controls) para hospedar o controle personalizado em uma ilha XAML.

Para obter um exemplo de C++ um aplicativo Win32, consulte os seguintes projetos no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): Esse projeto implementa um controle XAML personalizado UWP chamado `MyUserControl` que contém uma caixa de texto, vários botões e uma caixa de combinação.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Este é um projeto de aplicativo UWP com as alterações descritas nas etapas anteriores.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): Este é o C++ projeto de aplicativo Win32 que hospeda o controle personalizado UWP XAML em uma ilha XAML.

Para obter instruções detalhadas e exemplos para um aplicativo do WPF ou Windows Forms, consulte [estas instruções](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar a API de hospedagem XAML do UWP em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "Não é possível ativar DesktopWindowXamlSource. Este tipo não pode ser usado em um aplicativo UWP. " ou "não é possível ativar WindowsXamlManager. Este tipo não pode ser usado em um aplicativo UWP. " | Esse erro indica que você está tentando usar a API de hospedagem XAML do UWP (especificamente, está tentando instanciar os tipos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) em um aplicativo UWP. A API de hospedagem XAML do UWP destina-se apenas a ser usada em aplicativos de área de trabalho não UWP, como WPF C++ , Windows Forms e aplicativos Win32. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que foi criada em um thread diferente. Você deve passar esse método a HWND de uma janela que foi criada no mesmo thread que o código do qual você está chamando o método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "O método AttachToWindow falhou porque o HWND especificado descende de uma janela de nível superior diferente da HWND que foi passada anteriormente para AttachToWindow no mesmo thread." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que descende de uma janela de nível superior diferente de uma janela que você especificou em uma chamada anterior para esse método no mesmo thread.</p></p>Depois que o aplicativo chama **IDesktopWindowXamlSourceNative:: AttachToWindow** em um thread específico, todos os outros objetos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) no mesmo thread só podem ser anexados ao Windows que sejam descendentes do mesmo nível superior janela que foi passada na primeira chamada para **IDesktopWindowXamlSourceNative:: AttachToWindow**. Quando todos os objetos **DesktopWindowXamlSource** são fechados para um thread específico, a próxima **DesktopWindowXamlSource** é gratuita para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os objetos **DesktopWindowXamlSource** que estão vinculados a outras janelas de nível superior nesse thread ou crie um novo thread para esse **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Controles UWP em aplicativos de área de trabalho](xaml-islands.md)
* [C++Exemplo de ilhas XAML do Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)

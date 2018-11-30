---
description: Este artigo descreve como hospedar UWP XAML da interface do usuário em seu aplicativo da área de trabalho.
title: Usando o XAML da UWP que hospeda a API em um aplicativo da área de trabalho
ms.date: 11/27/2018
ms.topic: article
keywords: Windows 10, uwp, formulários do windows, win32, wpf
ms.localizationpriority: medium
ms.openlocfilehash: df6c47fd93c3f42721fd072d6406a2d32f7889db
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329136"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usando o XAML da UWP que hospeda a API em um aplicativo da área de trabalho

> [!NOTE]
> As ilhas de API e XAML de hospedagem do XAML da UWP estão disponíveis atualmente como uma visualização de desenvolvedor. Embora Encorajamos você a experimentá-los em seu próprio código de protótipo agora, não recomendamos que você usá-los no código de produção neste momento. Esses recursos continuam a se desenvolver e estabilizar em futuras versões do Windows. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.
>
> Se você tiver comentários sobre o XAML hospedagem ilhas de API e XAML, envie seus comentários para XamlIslandsFeedback@microsoft.com. Suas ideias e cenários são extremamente importantes para nós.

A partir do SDK do Windows 10 Insider Preview build 17709, aplicativos da área de trabalho não UWP (incluindo aplicativos C++ Win32, Windows Forms e WPF) podem usar o *XAML da UWP que hospeda a API* para hospedar controles UWP em qualquer elemento de interface do usuário que está associado um (identificador de janela HWND). Essa API permite que os aplicativos da área de trabalho não UWP usar os recursos de interface do usuário do Windows 10 mais recentes que só estão disponíveis por meio de controles UWP. Por exemplo, aplicativos da área de trabalho do UWP não podem usar essa API para hospedar controles UWP que usam o [Sistema de Design fluente](../design/fluent-design-system/index.md) e oferecer suporte à [Tinta do Windows](../design/input/pen-and-stylus-interactions.md).

O XAML da UWP que hospeda a API fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores exibir interface do usuário fluente para UWP não aplicativos da área de trabalho. Esse cenário é chamado, às vezes, *Ilhas XAML*. Para obter mais detalhes sobre esse cenário de desenvolvedor, consulte [controles UWP em aplicativos da área de trabalho](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>O XAML da UWP está hospedando API ideal para seu aplicativo da área de trabalho?

O XAML da UWP que hospeda a API fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho têm a opção de usar APIs de alternativos, mais conveniente para realizar essa meta.  

* Se você tiver um aplicativo da área de trabalho do C++ Win32 e você deseja hospedar controles UWP em seu aplicativo, você deve usar o XAML da UWP que hospeda a API. Não há nenhum alternativas para esses tipos de aplicativos.

* Para aplicativos WPF e Windows Forms, é recomendável que você use os [controles de host](xaml-host-controls.md#host-controls) e [controles empacotados](xaml-host-controls.md#wrapped-controls) no Kit de ferramentas de comunidade do Windows em vez do XAML da UWP que hospeda a API. Esses controles usam o XAML da UWP que hospeda a API internamente e proporcionar uma experiência de desenvolvimento mais simples. No entanto, você pode usar o XAML da UWP que hospeda API diretamente nesses tipos de aplicativos, se você escolher.

## <a name="related-samples"></a>Exemplos relacionados

Da maneira que você usa o XAML da UWP que hospeda a API no seu código depende do seu tipo de aplicativo, o design do seu aplicativo e de outros fatores. Para ilustrar como usar essa API no contexto de um aplicativo completo, este artigo se refere ao código dos exemplos a seguir.

### <a name="c-win32"></a>C++ Win32

Há vários exemplos que demonstram como usar o XAML da UWP hospedando API em um aplicativo C++ Win32 no GitHub:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Este exemplo demonstra como adicionar os controles UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)e [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) para um aplicativo C++ Win32.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Este exemplo demonstra como adicionar vários controles UWP básicos para um aplicativo C++ Win32 e manipular alterações de DPI.

### <a name="wpf-and-windows-forms"></a>Windows Forms e WPF

O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no Kit de ferramentas de comunidade Windows atua como um exemplo de referência para usar a UWP, API de hospedagem em aplicativos WPF e Windows Forms. O código-fonte está disponível nos seguintes locais:

  * Para a versão do WPF do controle, [clique aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF é derivado de [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * Para a versão do Windows Forms do controle, [clique aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão do Windows Forms é derivado de [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Pré-requisitos

O XAML da UWP que hospeda a API tem esses pré-requisitos.

* Windows 10 Insider Preview build 17709 (ou uma compilação posterior) e a compilação do Insider Preview correspondente do SDK do Windows. Como esse é um recurso em evolução, para a melhor experiência é recomendável usar a compilação mais recente disponível.

* Para usar o XAML da UWP que hospeda a API em seu aplicativo da área de trabalho, você precisará configurar seu projeto para que você pode chamar APIs UWP:

    * **C++ Win32:** Recomendamos que você configure seu projeto para usar [C++ c++ WinRT](../cpp-and-winrt-apis/index.md). Baixe e instale o [C++ c++ WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) do Visual Studio Marketplace e, em seguida, adicione o ```<CppWinRTEnabled>true</CppWinRTEnabled>``` propriedade seu arquivo, .vcxproj conforme descrito [aqui](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

    * **Windows Forms e WPF:** Siga [estas instruções](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Arquitetura de ilhas XAML

O XAML da UWP que hospeda a API inclui [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)e outros tipos relacionados no namespace [**Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) . Aplicativos da área de trabalho podem usar essa API para renderizar controles UWP e navegação de foco do teclado dentro e fora os elementos de rota. Aplicativos da área de trabalho também podem dimensionar e posicionar os controles UWP conforme desejado.

Quando você cria uma ilha XAML usando o XAML que hospeda a API em um aplicativo da área de trabalho, você terá a hierarquia de objetos a seguir:

* No nível de base é o elemento de interface do usuário em seu aplicativo em que você deseja hospedar a ilha XAML. Esse elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário em que você pode hospedar um island XAML incluem [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos WPF, [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicativos Windows Forms e uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicativos C++ Win32.

* No próximo nível é um objeto **DesktopWindowXamlSource** . Esse objeto fornece a infraestrutura para hospedar a ilha XAML. Seu código é responsável por criar esse objeto e anexá-lo ao elemento de interface do usuário pai.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto automaticamente cria uma janela filha nativo para hospedar seu controle UWP. Essa janela filha nativo é abstrair principalmente seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior é o controle UWP que você deseja hospedar em seu aplicativo da área de trabalho. Isso pode ser qualquer objeto UWP que deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles personalizados do usuário.

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha XAML.

![Arquitetura de DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Como hospedar controles UWP XAML

Aqui estão as etapas principais para usar o XAML da UWP que hospeda a API para hospedar um controle UWP em seu aplicativo.

1. Inicialize a estrutura XAML da UWP para o thread atual antes de seu aplicativo cria qualquer um dos objetos [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que ele hospedará no [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Se seu aplicativo cria o objeto **DesktopWindowXamlSource** antes que ele crie nenhum dos objetos **Windows.UI.Xaml.UIElement** , essa estrutura será inicializada para você quando você instancia o objeto **DesktopWindowXamlSource** . Nesse cenário, você não precisa adicionar nenhum código de sua preferência para inicializar a estrutura.

    * No entanto, se seu aplicativo cria os objetos **Windows.UI.Xaml.UIElement** antes que ele cria o objeto **DesktopWindowXamlSource** que hospedará-los, seu aplicativo deve chamar estática [** WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método para inicializar explicitamente a estrutura XAML da UWP antes dos objetos **Windows.UI.Xaml.UIElement** são instanciados. Seu aplicativo normalmente deve chamar esse método quando o elemento de interface do usuário pai que hospeda o **DesktopWindowXamlSource** é instanciado.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Esse método retorna um objeto [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contém uma referência para a estrutura XAML da UWP. Você pode criar quantas objetos **WindowsXamlManager** quanto você deseja em um determinado segmento. No entanto, porque cada objeto mantém uma referência para a estrutura XAML da UWP, você deve descartar os objetos para garantir que os recursos XAML eventualmente são lançados.

2. Crie um objeto [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) e anexá-lo a um elemento de interface do usuário pai em seu aplicativo que está associado um identificador de janela.

    Para fazer isso, você precisará seguir estas etapas:

    1. Crie um objeto **DesktopWindowXamlSource** e convertê-lo para a interface **IDesktopWindowXamlSourceNative** COM. Essa interface é declarada no ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` arquivo de cabeçalho no SDK do Windows. Em um projeto C++ Win32, você pode fazer referência a esse arquivo de cabeçalho diretamente. Em um projeto do WPF ou Windows Forms, você precisará declarar essa interface no código do seu aplicativo usando o atributo [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) . Certifique-se de sua declaração de interface corresponde exatamente a declaração da interface no ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Chame o método **AttachToWindow** da interface **IDesktopWindowXamlSourceNative** e passe o identificador da janela do elemento pai da interface do usuário em seu aplicativo.

    3. Defina o tamanho da janela filho interno contido no **DesktopWindowXamlSource**inicial. Por padrão, essa janela filha interno é definida como uma largura e altura de 0. Se você não definir o tamanho da janela, quaisquer controles UWP que adicionar para a **DesktopWindowXamlSource** não ficará visíveis. Para acessar a janela filho interno no **DesktopWindowXamlSource**, use a propriedade **WindowHandle** da interface **IDesktopWindowXamlSourceNative** . Os exemplos a seguir usam a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para definir o tamanho da janela.

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

3. Defina o **Windows.UI.Xaml.UIElement** que você deseja do host para a propriedade de [**conteúdo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) do seu objeto **DesktopWindowXamlSource** . O exemplo a seguir define um [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) chamado ```myGrid``` para a propriedade de **conteúdo** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para exemplos completos que demonstram estas tarefas no contexto de um aplicativo de exemplo de trabalho, consulte os seguintes arquivos de código:

  * **C++ Win32:** Consulte o arquivo [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) no exemplo de [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) ou o arquivo [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) no exemplo de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) .
  * **WPF:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) no Kit de ferramentas de comunidade do Windows.  
  * **Formulários do Windows:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) no Kit de ferramentas de comunidade do Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Como host personalizado UWP controles XAML

> [!IMPORTANT]
> Atualmente, controles personalizados de XAML da UWP de partes 3ª somente têm suporte em aplicativos c# WPF e Windows Forms. Você deve ter o código-fonte para os controles para que você pode compilar em relação a ele em seu aplicativo.

Se você quiser hospedar um controle personalizado do XAML da UWP (um controle que você defina por conta própria ou um controle fornecido por um 3ª de terceiros), você deve executar as seguintes tarefas adicionais além o processo descrito na [seção anterior](#how-to-host-uwp-xaml-controls).

1. Defina um tipo personalizado que deriva de [**UI**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) e também implementa [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Esse tipo funciona como um provedor de metadados raiz para carregar os metadados para tipos personalizados de XAML da UWP em assemblies no diretório atual do seu aplicativo.

    Para obter um exemplo que demonstra como fazer isso, consulte o arquivo de código [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) no Kit de ferramentas de comunidade Windows. Esse arquivo é parte da implementação das classes **WindowsXamlHost** compartilhada para WPF e Windows Forms, que ajudam a ilustrar como usar o XAML da UWP que hospeda a API nesses tipos de aplicativos.

2. Chame o método [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de seu provedor de metadados raiz quando o nome do tipo do controle XAML da UWP é atribuído (isso poderia ser atribuído no código em tempo de execução, ou você pode optar por habilitar isso seja atribuído na janela Propriedades do Visual Studio).

    Para obter um exemplo que demonstra como fazer isso, consulte o arquivo de código [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) no Kit de ferramentas de comunidade Windows. Esse arquivo é parte da implementação das classes **WindowsXamlHost** compartilhada para WPF e Windows Forms.

3. Integrar o código-fonte para o controle personalizado do XAML da UWP em sua solução de aplicativo host, compilar o controle personalizado e usá-lo em seu aplicativo por [estas instruções](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)a seguir.

## <a name="how-to-handle-keyboard-focus-navigation"></a>Como manipular a navegação de foco do teclado

Quando o usuário navega por meio dos elementos de interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando a tecla **Tab** ou seta de direção /), você precisará mover o foco programaticamente dentro e fora do objeto **DesktopWindowXamlSource** . Quando a navegação de teclado do usuário atinge o **DesktopWindowXamlSource**, move o foco para o primeiro objeto [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) na ordem de navegação da interface do usuário, continuam a mover o foco para o seguinte ** Windows.UI.Xaml.UIElement** objetos conforme os usuário alterna entre os elementos e, em seguida, movem o foco volta fora do **DesktopWindowXamlSource** e para o elemento de interface do usuário pai.  

O XAML da UWP que hospeda a API fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

1. Quando a navegação de teclado entra em seu **DesktopWindowXamlSource**, o evento [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipular esse evento e programaticamente move o foco para o primeiro hospedado **Windows.UI.Xaml.UIElement** usando o método [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

2. Quando o usuário está no último elemento focalizável na sua **DesktopWindowXamlSource** e pressiona a tecla **Tab** ou uma tecla de seta, o evento [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é acionado. Manipular esse evento e programaticamente move o foco para o próximo elemento focalizável no aplicativo host. Por exemplo, em um aplicativo WPF onde o **DesktopWindowXamlSource** está hospedado em um [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o método [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir o foco para o próximo elemento focalizável no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo de trabalho, consulte os seguintes arquivos de código:
  * **WPF:** Consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) no Kit de ferramentas de comunidade Windows.  
  * **Formulários do Windows:** Consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) no Kit de ferramentas de comunidade Windows.

## <a name="how-to-handle-layout-changes"></a>Como manipular alterações de layout

Quando o usuário altera o tamanho do elemento de interface do usuário pai, você precisará manipular todas as alterações de layout necessárias para garantir que seus controles UWP exibir conforme o esperado. Aqui estão alguns cenários importantes a serem considerados.

1. Quando o elemento de interface do usuário pai precisa obter o tamanho da área retangular necessário para ajustar o **Windows.UI.Xaml.UIElement** que você estiver hospedando no **DesktopWindowXamlSource**, chame o método de [**medida**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) da Windows.UI.Xaml.UIElement ** **. Por exemplo:
    * Em um aplicativo WPF, você pode fazer isso do método [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**.
    * Em um aplicativo Windows Forms, você pode fazer isso do método [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) do [**controle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**.

2. Quando o tamanho das alterações de elemento de interface do usuário pai, chame o método [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) da raiz **Windows.UI.Xaml.UIElement** que você estiver hospedando no **DesktopWindowXamlSource**. Por exemplo:
    * Em um aplicativo WPF, você pode fazer isso do método [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) do objeto [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**.
    * Em um aplicativo Windows Forms você pode fazer isso no manipulador para o evento [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) do [**controle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo de trabalho, consulte os seguintes arquivos de código:
  * **WPF:** Consulte o arquivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Kit de ferramentas de comunidade Windows.  
  * **Formulários do Windows:** Consulte o arquivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no Kit de ferramentas de comunidade Windows.

## <a name="how-to-handle-dpi-changes"></a>Como manipular alterações de DPI

Se você quiser manipular alterações de DPI na janela que hospeda o UWP controlar (por exemplo, se o usuário arrasta a entre monitores com DPI de tela diferentes), você precisará configurar o controle UWP com uma transformação de renderização, ouvir as alterações DPI em seu aplicativo e atualizar a posição da janela e renderizar a transformação do controle UWP em resposta às alterações de DPI.

As etapas a seguir ilustram uma maneira de lidar com esse processo no contexto de um aplicativo C++ Win32. Para obter um exemplo completo, consulte os arquivos de código [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) e [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) no exemplo de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) no GitHub.

1. Manter um objeto [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) em seu aplicativo e atribuí-lo para o método [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) do seu controle UWP. O exemplo a seguir faz isso para um controle [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) em um aplicativo C++ Win32.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. Na função [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) , escute a mensagem [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) . Em resposta a esta mensagem:
    * Use a função [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para redimensionar a janela que contém o controle UWP para o retângulo passado para a mensagem.
    * Atualize os fatores de escala eixos x e y de seu objeto **ScaleTransform** acordo com o novo valor DPI.
    * Faça os ajustes necessários para a aparência e o layout do seu controle UWP. O exemplo de código a seguir ajusta o [**preenchimento**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) do controle **Windows.UI.Xaml.Controls.Grid** hospedado em resposta às alterações DPI.

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

2. Para configurar seu aplicativo a serem consideradas DPI por monitor, adicione um [manifesto do assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e definir ```<dpiAwareness>``` elemento ```PerMonitorV2```. Para obter mais informações sobre esse valor, consulte a descrição para [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

    Para um manifesto de assembly lado a lado de exemplo completo, consulte o arquivo [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) no exemplo de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) no GitHub.

## <a name="limitations"></a>Limitações

O XAML que hospeda a API compartilha as mesmas limitações que todos os outros tipos de controles de host XAML para Windows 10. Para obter uma lista detalhada, consulte [limitações de controle de host XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro usando XAML da UWP que hospeda a API em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "não é possível ativar DesktopWindowXamlSource. Esse tipo não pode ser usado em um aplicativo UWP." ou "não é possível ativar WindowsXamlManager. Esse tipo não pode ser usado em um aplicativo UWP." | Esse erro indica que você está tentando usar o XAML da UWP que hospeda a API (especificamente, você está tentando instanciar os tipos de [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) em um aplicativo UWP. O XAML da UWP que hospeda a API apenas se destina a ser usado em aplicativos da área de trabalho não UWP, como aplicativos WPF, Windows Forms e C++ Win32. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro conectando a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "o método de AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative.AttachToWindow** e o passou o HWND de uma janela que foi criado em um thread diferente. Você deve passar esse método HWND de uma janela que foi criada no mesmo thread em que o código do qual você está chamando o método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro conectando a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe um **COMException** com a seguinte mensagem: "o método de AttachToWindow falhou porque o HWND especificado provém de uma janela de nível superior diferente que o HWND anteriormente foi passado para AttachToWindow no mesmo thread." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative.AttachToWindow** e o passou o HWND de uma janela originada uma janela de nível superior diferente de uma janela especificada em uma chamada para esse método anterior no mesmo thread.</p></p>Depois que seu aplicativo chama **IDesktopWindowXamlSourceNative.AttachToWindow** em um thread específico, todos os outros objetos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) no mesmo thread podem anexar apenas ao windows que são descendentes da janela de nível superior do mesmo que foi passado na primeira chamada para **IDesktopWindowXamlSourceNative.AttachToWindow**. Quando todos os objetos **DesktopWindowXamlSource** são fechados para um determinado segmento, a próxima **DesktopWindowXamlSource** fica livre para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os objetos de **DesktopWindowXamlSource** que são associados a outras janelas de nível superior nesse thread ou criar um novo segmento para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Controles UWP em aplicativos da área de trabalho](xaml-host-controls.md)

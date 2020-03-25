---
description: Este artigo demonstra como hospedar um controle UWP padrão em um C++ aplicativo Win32 usando a API de hospedagem XAML.
title: Hospedar um controle UWP padrão em um C++ aplicativo Win32 usando ilhas XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, Ilhas XAML, controles encapsulados, controles padrão
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226270"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>Hospedar um controle UWP padrão em um C++ aplicativo Win32

Este artigo demonstra como usar a [API de hospedagem XAML do UWP](using-the-xaml-hosting-api.md) para hospedar um controle UWP padrão (ou seja, um controle fornecidos pelo SDK do Windows) em um novo C++ aplicativo Win32. O código é baseado no [exemplo de ilha XAML simples](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), e esta seção discute algumas das partes mais importantes do código. Se você tiver um projeto C++ de aplicativo Win32 existente, poderá adaptar estas etapas e exemplos de código para seu projeto.

> [!NOTE]
> O cenário demonstrado neste artigo não dá suporte à edição direta de marcação XAML para controles UWP hospedados em seu aplicativo. Esse cenário restringe você a modificar a aparência e o comportamento de controles de UWP hospedados por meio de código. Para obter instruções que permitem editar diretamente a marcação XAML ao hospedar controles UWP, consulte [hospedar um controle UWP personalizado em um C++ aplicativo Win32](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Criar um projeto de aplicativo de área de trabalho

1. No Visual Studio 2019 com o Windows 10, versão 1903 SDK (versão 10.0.18362) ou uma versão posterior instalada, crie um novo projeto de **aplicativo da área de trabalho do Windows** e nomeie-o **MyDesktopWin32App**. Esse tipo de projeto está disponível nos **C++** filtros de projeto, **Windows**e **Desktop** .

2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução, clique em **redirecionar solução**, selecione o **10.0.18362.0** ou uma versão do SDK posterior e clique em **OK**.

3. Instale o pacote NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para habilitar o suporte para [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) em seu projeto:

    1. Clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.
    2. Selecione a guia **procurar** , procure o pacote [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale a versão mais recente deste pacote.

    > [!NOTE]
    > Para novos projetos, você pode instalar a extensão do [ C++/WinRT Visual Studio (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) como alternativa e usar um dos C++modelos de projeto/WinRT incluídos nessa extensão. Para obter mais detalhes, veja [este artigo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado.
    2. Selecione a guia **procurar** , procure o pacote **Microsoft. Toolkit. Win32. UI. SDK** e instale a versão v 6.0.0 (ou posterior) deste pacote. Esse pacote fornece vários ativos de compilação e tempo de execução que permitem que as ilhas XAML funcionem em seu aplicativo.

5. Defina o valor `maxVersionTested` no [manifesto do aplicativo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) para especificar que seu aplicativo é compatível com o Windows 10, versão 1903 ou posterior.

    1. Se você ainda não tiver um manifesto do aplicativo em seu projeto, adicione um novo arquivo XML ao seu projeto e nomeie-o **app. manifest**.
    2. No manifesto do aplicativo, inclua o elemento de **compatibilidade** e os elementos filho mostrados no exemplo a seguir. Substitua o atributo **ID** do elemento **maxVersionTested** pelo número de versão do Windows 10 que você está direcionando (este deve ser o windows 10, versão 1903 ou uma versão posterior).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Usar a API de hospedagem XAML para hospedar um controle UWP

O processo básico de usar a API de hospedagem XAML para hospedar um controle UWP segue estas etapas gerais:

1. Inicialize a estrutura de XAML do UWP para o thread atual antes que seu aplicativo crie qualquer um dos objetos [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que ele hospedará. Há várias maneiras de fazer isso, dependendo de quando você planeja criar o objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) que hospedará os controles.

    * Se seu aplicativo cria o objeto **DesktopWindowXamlSource** antes de criar qualquer um dos objetos **Windows. UI. XAML. UIElement** que ele hospedará, essa estrutura será inicializada para você quando você criar uma instância do objeto **DesktopWindowXamlSource** . Nesse cenário, você não precisa adicionar nenhum código próprio para inicializar a estrutura.

    * No entanto, se seu aplicativo criar os objetos **Windows. UI. XAML. UIElement** antes de criar o objeto **DesktopWindowXamlSource** que os hospedará, seu aplicativo deverá chamar o método estático [WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explicitamente a estrutura do UWP XAML antes de os objetos **Windows. UI. XAML. UIElement** serem instanciados. Seu aplicativo normalmente deve chamar esse método quando o elemento pai da interface do usuário que hospeda o **DesktopWindowXamlSource** é instanciado.

    > [!NOTE]
    > Esse método retorna um objeto [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contém uma referência à estrutura de XAML do UWP. Você pode criar quantos objetos **WindowsXamlManager** desejar em um determinado thread. No entanto, como cada objeto contém uma referência à estrutura de XAML do UWP, você deve descartar os objetos para garantir que os recursos XAML sejam eventualmente liberados.

2. Crie um objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) e anexe-o a um elemento pai da interface do usuário em seu aplicativo associado a um identificador de janela.

    Para fazer isso, você precisará seguir estas etapas:

    1. Crie um objeto **DesktopWindowXamlSource** e converta-o na interface com **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .
        > [!NOTE]
        > Essas interfaces são declaradas no arquivo de cabeçalho **Windows. UI. XAML. Hosting. desktopwindowxamlsource. h** na SDK do Windows. Por padrão, esse arquivo está em% ProgramFiles (x86)% \ Windows Kits\10\Include\\< número de Build\>\um.

    2. Chame o método **AttachToWindow** da interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** e passe o identificador de janela do elemento pai da interface do usuário em seu aplicativo.

    3. Defina o tamanho inicial da janela filho interna contida no **DesktopWindowXamlSource**. Por padrão, essa janela filho interna é definida com uma largura e uma altura de 0. Se você não definir o tamanho da janela, todos os controles UWP adicionados ao **DesktopWindowXamlSource** não estarão visíveis. Para acessar a janela filho interna no **DesktopWindowXamlSource**, use a propriedade **WindowHandle** da interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .

3. Por fim, atribua o **Windows. UI. XAML. UIElement** que você deseja hospedar à propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) do seu objeto **DesktopWindowXamlSource** .

As etapas e os exemplos de código a seguir demonstram como implementar o processo acima:

1. Na pasta **arquivos de origem** do projeto, abra o arquivo **MyDesktopWin32App. cpp** padrão. Exclua todo o conteúdo do arquivo e adicione as instruções `include` e `using` a seguir. Além dos cabeçalhos e C++ dos namespaces padrão e UWP, essas instruções incluem vários itens específicos para as ilhas XAML.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. Copie o código a seguir após a seção anterior. Esse código define a função **WinMain** para o aplicativo. Essa função Inicializa uma janela básica e usa a API de hospedagem XAML para hospedar um controle simples do UWP **TextBlock** na janela.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. Copie o código a seguir após a seção anterior. Esse código define o [procedimento de janela](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure) para a janela.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. Salve o arquivo de código e compile e execute o aplicativo. Confirme que você vê o controle do UWP **TextBlock** na janela do aplicativo.
    > [!NOTE]
    > Você pode ver os vários avisos de compilação, incluindo `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` e `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Esses avisos são problemas conhecidos com as ferramentas atuais e os pacotes NuGet, e eles podem ser ignorados.

Para obter exemplos completos que demonstram essas tarefas, consulte os seguintes arquivos de código:

* **C++Win32**
  * Consulte o arquivo [HelloWindowsDesktop. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) .
  * Consulte o arquivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .
* **WPF:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.  
* **Windows Forms:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="package-the-app"></a>Empacotar o aplicativo

Opcionalmente, você pode empacotar o aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação. O MSIX é a tecnologia de empacotamento de aplicativo moderna para Windows e é baseado em uma combinação das tecnologias de instalação MSI,. Appx, App-V e ClickOnce.

As instruções a seguir mostram como empacotar todos os componentes na solução em um pacote MSIX usando o [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) no Visual Studio 2019. Essas etapas serão necessárias apenas se você quiser empacotar o aplicativo em um pacote MSIX.

> [!NOTE]
> Se você optar por não empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix) para implantação, os computadores que executam seu aplicativo deverão ter o [tempo de execução C++ Visual](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) instalado.

1. Adicione um novo [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução. Ao criar o projeto, selecione **Windows 10, versão 1903 (10,0; Build 18362)** para a **versão de destino** e a **versão mínima**.

2. No projeto de empacotamento, clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Na lista de projetos, selecione o C++projeto de aplicativo da área de trabalho do/Win32 em sua solução e clique em **OK**.

3. Compile e execute o projeto de empacotamento. Confirme se o aplicativo é executado e exibe os controles UWP conforme esperado.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Os exemplos de código neste artigo o ajudarão a começar a usar o cenário básico de Hospedagem de um controle C++ UWP padrão em um aplicativo Win32. As seções a seguir introduzem cenários adicionais para os quais seu aplicativo pode precisar dar suporte.

### <a name="host-a-custom-uwp-control"></a>Hospedar um controle UWP personalizado

Para muitos cenários, talvez seja necessário hospedar um controle personalizado UWP XAML que contenha vários controles individuais que funcionam juntos. O processo para hospedar um controle UWP personalizado (um controle que você define por conta própria ou um controle fornecido por terceiros) em um C++ aplicativo Win32 é mais complexo do que hospedar um controle padrão e requer código adicional.

Para obter uma explicação completa, consulte [hospedar um controle UWP personalizado em C++ um aplicativo Win32 usando a API de hospedagem XAML](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Cenários avançados

Muitos aplicativos de área de trabalho que hospedam ilhas XAML precisarão lidar com cenários adicionais para proporcionar uma experiência de usuário tranqüila. Por exemplo, os aplicativos da área de trabalho podem precisar manipular a entrada do teclado em ilhas XAML, concentrar a navegação entre as ilhas XAML e outros elementos da interface do usuário e as alterações de layout.

Para obter mais informações sobre como lidar com esses cenários e ponteiros para exemplos de código relacionados, consulte [cenários C++ avançados para ilhas XAML em aplicativos Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Hospedar controles XAML UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md)
* [Usando a API de hospedagem XAML do UWP C++ em um aplicativo Win32](using-the-xaml-hosting-api.md)
* [Hospedar um controle UWP personalizado em um C++ aplicativo Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Cenários avançados para ilhas XAML em C++ aplicativos Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemplos de código de ilhas XAML](https://github.com/microsoft/Xaml-Islands-Samples)

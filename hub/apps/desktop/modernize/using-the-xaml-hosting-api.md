---
description: Este artigo descreve como hospedar a interface do usuário do UWP XAML C++ em seu aplicativo Win32 da área de trabalho.
title: Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Ilhas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cdcef66dc1f0026ff369eeb3f3c7881385d6e5ba
ms.sourcegitcommit: 412bf5bb90e1167d118699fbf71d0e6864ae79bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "71339298"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Usar a API de hospedagem XAML UWP em um aplicativo C++ Win32

A partir do Windows 10, versão 1903, aplicativos de área de trabalho não C++ UWP (incluindo Win32, WPF e aplicativos de Windows Forms) podem usar a *API de hospedagem XAML do UWP* para hospedar controles UWP em qualquer elemento de interface do usuário que esteja associado a um identificador de janela (HWND). Essa API permite que aplicativos de área de trabalho não UWP usem os recursos mais recentes da interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP. Por exemplo, aplicativos de área de trabalho não UWP podem usar essa API para hospedar controles UWP que usam o [sistema de design fluente](/windows/uwp/design/fluent-design-system/index) e dão suporte ao [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

A API de hospedagem XAML do UWP fornece a base para um conjunto mais amplo de controles que estamos fornecendo para permitir que os desenvolvedores tragam a interface do usuário fluente para aplicativos de área de trabalho não UWP. Esse recurso é chamado de *ilhas XAML*. Para obter uma visão geral desse recurso, consulte [hospedar controles XAML do UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md).

> [!NOTE]
> Se você tiver comentários sobre as ilhas XAML, crie um novo problema no [repositório Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) e deixe seus comentários lá. Se preferir enviar seus comentários de forma privada, você poderá enviá-los para XamlIslandsFeedback@microsoft.com. Suas ideias e cenários são extremamente importantes para nós.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Você deve usar a API de Hospedagem de XAML do UWP?

A API de hospedagem do UWP XAML fornece a infraestrutura de baixo nível para hospedar controles UWP em aplicativos da área de trabalho. Alguns tipos de aplicativos da área de trabalho têm a opção de usar APIs alternativas e mais convenientes para atingir esse objetivo.  

* Se você tiver um C++ aplicativo de área de trabalho Win32 e quiser hospedar os controles UWP em seu aplicativo, deverá usar a API de hospedagem XAML do UWP. Não há alternativas para esses tipos de aplicativos.

* Para aplicativos do WPF e Windows Forms, é altamente recomendável que você use os [controles .NET da ilha XAML](xaml-islands.md#wpf-and-windows-forms-applications) no kit de ferramentas da Comunidade do Windows em vez de usar a API de hospedagem XAML do UWP diretamente. Esses controles usam a API de Hospedagem de XAML do UWP internamente e implementam todo o comportamento que, de outra forma, você precisaria lidar se usou a API de hospedagem XAML do UWP diretamente, incluindo a navegação do teclado e as alterações de layout.

Como recomendamos que apenas C++ os aplicativos Win32 usem a API de hospedagem XAML do UWP, este artigo fornece principalmente instruções C++ e exemplos para aplicativos Win32. No entanto, você pode usar a API de hospedagem XAML do UWP no WPF e Windows Forms aplicativos se escolher. Este artigo aponta para o código-fonte relevante para os [controles de host](xaml-islands.md#host-controls) para WPF e Windows Forms no kit de ferramentas da Comunidade do Windows para que você possa ver como a API de hospedagem XAML do UWP é usada por esses controles.

## <a name="prerequisites"></a>Pré-requisitos

As ilhas XAML exigem o Windows 10, versão 1903 (ou posterior) e a compilação correspondente do SDK do Windows. Para usar as ilhas XAML em C++ seu aplicativo Win32, você deve primeiro configurar seu projeto.

### <a name="support-for-cwinrt"></a>Suporte para C++/WinRT

Certifique-se de que seu [ C++](/windows/uwp/cpp-and-winrt-apis)projeto dá suporte a/WinRT:

* Para novos projetos, você pode instalar o [ C++VSIX (/WinRT Visual Studio Extension)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) e usar um dos modelos C++de projeto/WinRT incluídos nessa extensão.
* Para projetos existentes, você pode instalar o pacote NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) no projeto.

Para obter mais detalhes sobre essas opções, consulte [Este artigo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="configure-your-project-for-app-deployment"></a>Configurar seu projeto para implantação de aplicativo

Escolha uma das seguintes opções para preparar seu projeto para implantação:

* **Empacote seu aplicativo em um pacote MSIX**. Empacotar seu aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix/) fornece muitos benefícios de implantação e tempo de execução.
    1. Instale o SDK do Windows 10, versão 1903 (versão 10.0.18362) ou uma versão posterior.
    2. Empacote seu aplicativo em um pacote MSIX adicionando um [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à sua solução e adicionando uma C++referência ao seu projeto/Win32.

* **Instale o pacote Microsoft. Toolkit. Win32. UI. SDK**. Se você não quiser empacotar seu aplicativo em um pacote MSIX, poderá instalar o [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versão v 6.0.0-preview7 ou posterior). Esse pacote fornece vários ativos de compilação e tempo de execução que permitem que as ilhas XAML funcionem em seu aplicativo. Verifique se a opção **incluir pré-lançamento** está selecionada para que você possa ver as versões de visualização mais recentes deste pacote.

> [!NOTE]
> As versões anteriores dessas instruções tinham que adicionar o elemento `maxversiontested` a um manifesto do aplicativo em seu projeto. Desde que você esteja usando uma das opções listadas acima, você não precisa mais adicionar esse elemento ao seu manifesto.

### <a name="additional-requirements-for-custom-uwp-controls"></a>Requisitos adicionais para controles UWP personalizados

Se você estiver hospedando um controle UWP personalizado (por exemplo, um controle de usuário que consiste em vários controles UWP que funcionam juntos), você precisará seguir as instruções adicionais nesta [seção](#host-a-custom-uwp-control). Você também deve ter o código-fonte do controle personalizado para que possa compilá-lo com seu aplicativo.

## <a name="architecture-of-the-api"></a>Arquitetura da API

A API de hospedagem XAML do UWP inclui esses tipos de Windows Runtime principais e interfaces COM.

|  Tipo ou interface | Descrição |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Essa classe representa a estrutura de XAML do UWP. Essa classe fornece um único método [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático que inicializa a estrutura de XAML do UWP no thread atual no aplicativo da área de trabalho. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Essa classe representa uma instância do conteúdo do UWP XAML que você está hospedando em seu aplicativo de área de trabalho. O membro mais importante dessa classe é a propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Atribua essa propriedade a um [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que você deseja hospedar. Essa classe também tem outros membros para direcionar a navegação de foco de teclado para dentro e fora das Ilhas XAML. |
| IDesktopWindowXamlSourceNative | Essa interface COM fornece o método **AttachToWindow** , que você usa para anexar uma ilha XAML em seu aplicativo a um elemento pai da interface do usuário. Cada objeto **DesktopWindowXamlSource** implementa essa interface. |
| IDesktopWindowXamlSourceNative2 | Essa interface COM fornece o método **PreTranslateMessage** , que permite que a estrutura de XAML do UWP processe determinadas mensagens do Windows corretamente. Cada objeto **DesktopWindowXamlSource** implementa essa interface. |

O diagrama a seguir ilustra a hierarquia de objetos em uma ilha XAML hospedada em um aplicativo de área de trabalho.

* No nível base está o elemento de interface do usuário em seu aplicativo em que você deseja hospedar a ilha XAML. Este elemento de interface do usuário deve ter um identificador de janela (HWND). Exemplos de elementos de interface do usuário nos quais você pode hospedar uma ilha XAML incluem C++ uma [janela](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicativos Win32, um [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicativos do WPF e um [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para Windows Forms aplicativos.

* No próximo nível está um objeto **DesktopWindowXamlSource** . Esse objeto fornece a infraestrutura para hospedar a ilha XAML. Seu código é responsável por criar esse objeto e anexá-lo ao elemento pai da interface do usuário.

* Quando você cria um **DesktopWindowXamlSource**, esse objeto cria automaticamente uma janela filho nativa para hospedar seu controle UWP. Essa janela filho nativa é basicamente abstraida para fora do seu código, mas você pode acessar seu identificador (HWND), se necessário.

* Por fim, no nível superior está o controle UWP que você deseja hospedar em seu aplicativo de desktop. Pode ser qualquer objeto UWP derivado de [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluindo qualquer controle UWP fornecido pelo SDK do Windows, bem como controles de usuário personalizados.

![Arquitetura do DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>Exemplos relacionados

A maneira como você usa a API de hospedagem XAML do UWP em seu código depende do tipo de aplicativo, do design do seu aplicativo e de outros fatores. Para ajudar a ilustrar como usar essa API no contexto de um aplicativo completo, este artigo refere-se ao código dos exemplos a seguir.

### <a name="c-win32"></a>C++Win32

Os exemplos a seguir demonstram como usar a API de hospedagem XAML do C++ UWP em um aplicativo Win32:

* [Exemplo de ilha XAML simples](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp). Este exemplo demonstra uma implementação básica de Hospedagem de um controle UWP em um aplicativo C++ Win32 não empacotado (ou seja, um aplicativo que não é compilado em um pacote MSIX).

* [Ilha XAML com exemplo de controle personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App). Este exemplo demonstra uma implementação completa da Hospedagem de um controle UWP personalizado em um aplicativo C++ Win32 não empacotado, bem como o tratamento de outro comportamento, como entrada de teclado e navegação de foco. 

### <a name="wpf-and-windows-forms"></a>WPF e Windows Forms

O controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows serve como um exemplo de referência para usar a API de hospedagem UWP no WPF e Windows Forms aplicativos. O código-fonte está disponível nos seguintes locais:

* Para a versão do WPF do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). A versão do WPF deriva de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para a versão Windows Forms do controle, [acesse aqui](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). A versão Windows Forms deriva de [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-a-standard-uwp-control"></a>Hospedar um controle UWP padrão

Esta seção orienta você pelo processo de usar a API de hospedagem XAML do UWP para hospedar um controle UWP padrão (ou seja, um controle fornecido pela biblioteca SDK do Windows ou WinUI) em um novo C++ aplicativo Win32. O código é baseado no [exemplo de ilha XAML simples](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp), e esta seção discute algumas das partes mais importantes do código. Se você tiver um projeto C++ de aplicativo Win32 existente, poderá adaptar estas etapas e exemplos de código para seu projeto.

### <a name="configure-the-project"></a>Configurar o projeto

1. No Visual Studio 2019 com o SDK do Windows 10, versão 1903 (versão 10.0.18362) ou uma versão posterior instalada, crie um novo projeto de **aplicativo da área de trabalho do Windows** . Esse projeto está disponível nos filtros **C++** de projeto, **Windows**e **Desktop** .

2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó da solução, clique em **redirecionar solução**, selecione o **10.0.18362.0** ou uma versão do SDK posterior e clique em **OK**.

3. Instale o pacote NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) :

    1. Clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.
    2. Selecione a guia **procurar** , procure o pacote [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale a versão mais recente deste pacote.

4. Instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado.
    2. Selecione a guia **procurar** , procure o pacote [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) e instale a versão v 6.0.0-preview7 (ou posterior) deste pacote.

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Usar a API de hospedagem XAML para hospedar um controle UWP

O processo básico de usar a API de hospedagem XAML para hospedar um controle UWP segue estas etapas gerais:

1. Inicialize a estrutura de XAML do UWP para o thread atual antes que seu aplicativo crie qualquer um dos objetos [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que ele hospedará. Há várias maneiras de fazer isso, dependendo de quando você planeja criar o objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) que hospedará os controles.

    * Se seu aplicativo cria o objeto **DesktopWindowXamlSource** antes de criar qualquer um dos objetos **Windows. UI. XAML. UIElement** que ele hospedará, essa estrutura será inicializada para você quando você criar uma instância do  **Objeto DesktopWindowXamlSource** . Nesse cenário, você não precisa adicionar nenhum código próprio para inicializar a estrutura.

    * No entanto, se seu aplicativo criar os objetos **Windows. UI. XAML. UIElement** antes de criar o objeto **DesktopWindowXamlSource** que os hospedará, seu aplicativo deverá chamar o static [ O método WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explicitamente a estrutura do UWP XAML antes dos objetos **Windows. UI. XAML. UIElement** serem instanciados. Seu aplicativo normalmente deve chamar esse método quando o elemento pai da interface do usuário que hospeda o **DesktopWindowXamlSource** é instanciado.

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

1. Na pasta **arquivos de origem** do projeto, abra o arquivo **WindowsProject. cpp** padrão. Exclua todo o conteúdo do arquivo e adicione as instruções `include` e `using` a seguir. Além dos cabeçalhos e C++ dos namespaces padrão e UWP, essas instruções incluem vários itens específicos para as ilhas XAML.

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

4. Copie o código a seguir após a seção anterior. Esse código define o [procedimento de janela](https://docs.microsoft.com/en-us/windows/win32/learnwin32/writing-the-window-procedure) para a janela.

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
  * Consulte o arquivo [HelloWindowsDesktop. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp) no [exemplo da ilha XAML simples](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp).
  * Consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) na [ilha XAML com o exemplo de controle personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

* **WPF:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.  

* **Windows Forms:** Consulte os arquivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) e [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) no kit de ferramentas da Comunidade do Windows.

## <a name="host-a-custom-uwp-control"></a>Hospedar um controle UWP personalizado

O processo de Hospedagem de um controle UWP personalizado (um controle que você define por conta própria ou um controle fornecido por terceiros) em C++ um aplicativo Win32 começa com as mesmas etapas que [hospedam um controle padrão](#host-a-standard-uwp-control). No entanto, hospedar um controle UWP personalizado requer projetos e etapas adicionais.

Para hospedar um controle UWP personalizado, você precisará dos seguintes projetos e componentes:

* **O código-fonte e o projeto para seu aplicativo**. Verifique se você configurou seu projeto para atender aos [pré-requisitos](#prerequisites) para hospedar ilhas XAML.

* **O controle UWP personalizado**. Você precisará do código-fonte para o controle UWP personalizado que deseja hospedar para que possa compilá-lo com seu aplicativo. Normalmente, o controle personalizado é definido em um projeto de biblioteca de classes UWP que você faz referência na mesma solução C++ que o seu projeto Win32.

* **Um projeto de aplicativo UWP que define um objeto XamlApplication**. Seu C++ projeto Win32 deve ter acesso a uma instância da classe `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` fornecida pelo kit de ferramentas da Comunidade do Windows. Esse tipo atua como um provedor de metadados raiz para carregar metadados para tipos personalizados UWP XAML em assemblies no diretório atual do seu aplicativo. A maneira recomendada para fazer isso é adicionar um projeto de **aplicativo em branco (universal do Windows)** à mesma solução que C++ o seu projeto do Win32 e revisar a classe de `App` padrão neste projeto.
  > [!NOTE]
  > Sua solução pode conter apenas um projeto que define um objeto `XamlApplication`. Todos os controles UWP personalizados em seu aplicativo compartilham o mesmo objeto `XamlApplication`. O projeto que define o objeto `XamlApplication` deve incluir referências a todas as outras bibliotecas UWP e projetos que são usados para hospedar controles UWP na ilha XAML.

Para hospedar um controle UWP personalizado em um C++ aplicativo Win32, siga estas etapas gerais.

1. Na solução que contém seu projeto C++ de aplicativo de área de trabalho do Win32, adicione um projeto de **aplicativo em branco (universal do Windows)** e configure-o seguindo as instruções detalhadas nesta [seção](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project) no passo a passos do WPF relacionado.

2. Na mesma solução, adicione o projeto que contém o código-fonte para o controle XAML do UWP personalizado (normalmente é um projeto de biblioteca de classes UWP) e compile o projeto.

3. No projeto de aplicativo UWP, adicione uma referência ao projeto de biblioteca de classes UWP.

4. No seu C++ projeto Win32, adicione uma referência ao projeto de aplicativo UWP e ao projeto de biblioteca de classes UWP em sua solução.

5. Siga o processo descrito na seção [usar a API de hospedagem XAML para hospedar um controle UWP](#use-the-xaml-hosting-api-to-host-a-uwp-control) para hospedar o controle personalizado em uma ilha XAML em seu aplicativo. Atribua uma instância do controle personalizado para hospedar a propriedade [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) do objeto **DesktopWindowXamlSource** em seu código.

Para obter um exemplo completo de C++ um aplicativo Win32, consulte os seguintes projetos na [ilha XAML com o exemplo de controle personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): este projeto implementa um controle XAML do UWP personalizado chamado `MyUserControl` que contém uma caixa de texto, vários botões e uma caixa de combinação.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Este é um projeto de aplicativo UWP com as alterações descritas acima.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): esse é o C++ projeto de aplicativo Win32 que hospeda o controle XAML do UWP personalizado em uma ilha XAML.

## <a name="handle-keyboard-layout-and-dpi"></a>Manipular teclado, layout e DPI

As seções a seguir fornecem orientações e links para exemplos de código sobre como lidar com cenários de teclado e layout.

* [Entrada de teclado](#keyboard-input)
* [Navegação de foco de teclado](#keyboard-focus-navigation)
* [Manipular alterações de layout](#handle-layout-changes)
* [Tratar alterações de DPI](#handle-dpi-changes)

### <a name="keyboard-input"></a>Entrada por teclado

Para lidar corretamente com a entrada de teclado para cada ilha XAML, seu aplicativo deve passar todas as mensagens do Windows para a estrutura de XAML do UWP para que determinadas mensagens possam ser processadas corretamente. Para fazer isso, em algum lugar em seu aplicativo que possa acessar o loop de mensagem, converta o objeto **DesktopWindowXamlSource** para cada ilha XAML em uma interface com **IDesktopWindowXamlSourceNative2** . Em seguida, chame o método **PreTranslateMessage** dessa interface e passe a mensagem atual.

  * Win32:: o aplicativo pode chamar **PreTranslateMessage** diretamente em seu loop de mensagem principal. **C++** Para obter um exemplo, consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF:** O aplicativo pode chamar **PreTranslateMessage** do manipulador de eventos para o evento [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) no kit de ferramentas da Comunidade do Windows.

  * **Windows Forms:** O aplicativo pode chamar **PreTranslateMessage** de uma substituição para o método [Control. PreprocessMessage](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage) . Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) no kit de ferramentas da Comunidade do Windows.

### <a name="keyboard-focus-navigation"></a>Navegação de foco de teclado

Quando o usuário navega pelos elementos da interface do usuário em seu aplicativo usando o teclado (por exemplo, pressionando **Tab** ou direção/tecla de seta), você precisará mover o foco de forma programática para dentro e para fora do objeto **DesktopWindowXamlSource** . Quando a navegação do teclado do usuário atinge o **DesktopWindowXamlSource**, mova o foco para o primeiro objeto [Windows. UI. XAML. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) na ordem de navegação da sua interface do usuário, continue a mover o foco para o seguinte  **Objetos Windows. UI. XAML. UIElement** conforme o usuário percorre os elementos e, em seguida, move o foco de volta do **DesktopWindowXamlSource** e para o elemento pai da interface do usuário.  

A API de hospedagem XAML do UWP fornece vários tipos e membros para ajudá-lo a realizar essas tarefas.

* Quando a navegação do teclado entra em seu **DesktopWindowXamlSource**, o evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) é gerado. Manipule esse evento e mova o foco programaticamente para o primeiro **Windows. UI. XAML. UIElement** hospedado usando o método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Quando o usuário está no último elemento focado em seu **DesktopWindowXamlSource** e pressiona a tecla **Tab** ou uma tecla de direção, o evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) é gerado. Manipule esse evento e mova o foco programaticamente para o próximo elemento focado no aplicativo host. Por exemplo, em um aplicativo WPF em que o **DesktopWindowXamlSource** está hospedado em um [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), você pode usar o método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir o foco para o próximo elemento focado no aplicativo host.

Para obter exemplos que demonstram como fazer isso no contexto de um aplicativo de exemplo funcional, consulte os seguintes arquivos de código:

  * /Win32: consulte o arquivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).  **C++**

  * **WPF:** Consulte o arquivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) no kit de ferramentas da Comunidade do Windows.  

  * **Windows Forms:** Consulte o arquivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) no kit de ferramentas da Comunidade do Windows.

### <a name="handle-layout-changes"></a>Manipular alterações de layout

Quando o usuário alterar o tamanho do elemento pai da interface do usuário, você precisará lidar com as alterações de layout necessárias para garantir que seus controles UWP sejam exibidos como esperado. Aqui estão alguns cenários importantes a considerar.

* Em um C++ aplicativo Win32, quando seu aplicativo manipula a mensagem WM_SIZE, ele pode reposicionar a ilha XAML hospedada usando a função [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obter um exemplo, consulte o arquivo de código [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) no [ C++ exemplo do Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Quando o elemento pai da interface do usuário precisa obter o tamanho da área retangular necessária para ajustar o **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**, chame o método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) do **Windows. UI. XAML. UIElement** . Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) do [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms você pode fazer isso a partir do método [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) do [controle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHostBase.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

* Quando o tamanho do elemento pai da interface do usuário for alterado, chame o método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) da raiz **Windows. UI. XAML. UIElement** que você está hospedando no **DesktopWindowXamlSource**. Por exemplo:

    * Em um aplicativo do WPF, você pode fazer isso a partir do método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) do objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

    * Em um aplicativo Windows Forms, você pode fazer isso a partir do manipulador para o evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) do [controle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda o **DesktopWindowXamlSource**. Para obter um exemplo, consulte o arquivo [WindowsXamlHost.layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) no kit de ferramentas da Comunidade do Windows.

### <a name="handle-dpi-changes"></a>Tratar alterações de DPI

A estrutura de XAML do UWP lida com alterações de DPI para controles UWP hospedados automaticamente (por exemplo, quando o usuário arrasta a janela entre monitores com DPI de tela diferente). Para obter a melhor experiência, recomendamos que seu aplicativo Windows Forms, WPF C++ ou Win32 esteja configurado para ter reconhecimento de DPI por monitor.

Para configurar seu aplicativo para ter reconhecimento de DPI por monitor, adicione um [manifesto de assembly lado a lado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) ao seu projeto e defina o elemento **\<DpiAwareness\>** como **PerMonitorV2**. Para obter mais informações sobre esse valor, consulte a descrição de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="troubleshooting"></a>Solução de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erro ao usar a API de hospedagem XAML do UWP em um aplicativo UWP

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "não é possível ativar DesktopWindowXamlSource. Este tipo não pode ser usado em um aplicativo UWP. " ou "não é possível ativar WindowsXamlManager. Este tipo não pode ser usado em um aplicativo UWP. " | Esse erro indica que você está tentando usar a API de hospedagem XAML do UWP (especificamente, está tentando instanciar os tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) em um aplicativo UWP. A API de hospedagem XAML do UWP destina-se apenas a ser usada em aplicativos de área de trabalho não UWP, como WPF C++ , Windows Forms e aplicativos Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erro ao tentar usar os tipos WindowsXamlManager ou DesktopWindowXamlSource

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma exceção com a seguinte mensagem: "WindowsXamlManager e DesktopWindowXamlSource têm suporte para aplicativos destinados à versão 10.0.18226.0 do Windows e posterior. Verifique o manifesto do aplicativo ou o manifesto do pacote e certifique-se de que a propriedade MaxTestedVersion seja atualizada. " | Esse erro indica que seu aplicativo tentou usar os tipos **WindowsXamlManager** ou **DesktopWindowXamlSource** na API de hospedagem XAML do UWP, mas o sistema operacional não pode determinar se o aplicativo foi criado para o Windows 10 de destino, versão 1903 ou posterior. A API de hospedagem XAML do UWP foi introduzida pela primeira vez como uma visualização em uma versão anterior do Windows 10, mas só tem suporte a partir do Windows 10, versão 1903.</p></p>Para resolver esse problema, crie um pacote MSIX para o aplicativo e execute-o do pacote ou instale o pacote NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) em seu projeto. Para obter mais informações, consulte [esta seção](#configure-your-project-for-app-deployment). |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erro ao anexar a uma janela em um thread diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "o método AttachToWindow falhou porque o HWND especificado foi criado em um thread diferente." | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que foi criada em um thread diferente. Você deve passar esse método a HWND de uma janela que foi criada no mesmo thread que o código do qual você está chamando o método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erro ao anexar a uma janela em uma janela de nível superior diferente

| Problema | Resolução |
|-------|------------|
| Seu aplicativo recebe uma **COMException** com a seguinte mensagem: "o método AttachToWindow falhou porque o HWND especificado descende de uma janela de nível superior diferente da HWND que foi passada anteriormente para AttachToWindow no mesmo thread". | Esse erro indica que seu aplicativo chamou o método **IDesktopWindowXamlSourceNative:: AttachToWindow** e passou a ele o HWND de uma janela que descende de uma janela de nível superior diferente de uma janela que você especificou em uma chamada anterior para esse método no mesmo thread.</p></p>Depois que o aplicativo chama **AttachToWindow** em um thread específico, todos os outros objetos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) no mesmo thread só podem se anexar a janelas descendentes da mesma janela de nível superior que foi passada na primeira chamada para **AttachToWindow**. Quando todos os objetos **DesktopWindowXamlSource** são fechados para um thread específico, a próxima **DesktopWindowXamlSource** é gratuita para anexar a qualquer janela novamente.</p></p>Para resolver esse problema, feche todos os objetos **DesktopWindowXamlSource** que estão vinculados a outras janelas de nível superior nesse thread ou crie um novo thread para esse **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Tópicos relacionados

* [Controles UWP em aplicativos de área de trabalho](xaml-islands.md)
* [C++Exemplo de ilhas XAML do Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)

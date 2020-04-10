---
title: Usar a Camada visual com Win32
description: Use a Camada visual para aprimorar a interface do usuário do seu aplicativo da área de trabalho Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, renderização, composição, Win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215197"
---
# <a name="using-the-visual-layer-with-win32"></a>Usar a Camada visual com Win32

Você pode usar as APIs de Composição do Windows Runtime (também conhecidas como [Camada visual](/windows/uwp/composition/visual-layer)) em seus aplicativos Win32 para criar experiências modernas que se destacam para os usuários do Windows 10.

O código completo para este tutorial está disponível no GitHub: [Exemplo do HelloComposition para Win32](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Os aplicativos universais do Windows que necessitam de controle preciso sobre a composição da interface do usuário têm acesso ao namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition) para exercer controle refinado sobre como a interface do usuário é composta e renderizada. No entanto, essa API de composição não está limitada a aplicativos UWP. Aplicativos Win32 da área de trabalho podem aproveitar os sistemas de composição modernos na UWP e no Windows 10.

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem UWP tem estes pré-requisitos.

- Supomos que você tem alguma familiaridade com o desenvolvimento de aplicativos usando Win32 e UWP. Para saber mais, veja:
  - [Introdução ao Win32 e C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10, versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Como usar APIs de Composição de um aplicativo Win32 da área de trabalho

Neste tutorial, você criará um aplicativo Win32 C++ simples e adicionará elementos de composição UWP a ele. O foco está na configuração correta do projeto, na criação do código de interoperabilidade e no design de algo simples usando APIs Windows Composition. O aplicativo concluído tem esta aparência.

![A interface do usuário do aplicativo em execução](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Criar um projeto C++ Win32 no Visual Studio

A primeira etapa é criar o projeto de aplicativo Win32 no Visual Studio.

Para criar um projeto de aplicativo Win32 no C++ chamado _HelloComposition_:

1. Abra o Visual Studio e selecione **Arquivo** > **Novo** > **Projeto**.

    A caixa de diálogo **Novo projeto** é aberta.
1. Na categoria **Instalado**, expanda o nó **Visual C++** e, em seguida, selecione **Área de trabalho do Windows**.
1. Selecione o modelo **Aplicativo da área de trabalho do Windows**.
1. Insira o nome _HelloComposition_ e clique em **OK**.

    O Visual Studio cria o projeto e abre o editor do arquivo do aplicativo principal.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar APIs do Windows Runtime

Para usar APIs do WinRT (Windows Runtime) em seu aplicativo Win32, usamos C++/WinRT. Você precisa configurar seu projeto do Visual Studio para adicionar suporte a C++/WinRT.

(Para ver mais detalhes, confira [Introdução ao C++/WinRT – Modificar um projeto de aplicativo da área de trabalho do Windows para adicionar suporte a C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. No menu **Projeto**, abra as propriedades do projeto (_Propriedades do HelloComposition_) e verifique se as seguintes configurações estão definidas com os valores especificados:

    - Para **Configuração**, selecione _Todas as Configurações_. Para **Plataforma**, selecione _Todas as Plataformas_.
    - **Propriedades de Configuração** > **Geral** > **Versão do SDK do Windows** = _10.0.17763.0_ ou posterior

    ![Definir versão do SDK](images/visual-layer-interop/sdk-version.png)

    - **C/C++**  > **Linguagem** > **Linguagem C++ Padrão** = _ISO C++ 17 Padrão (/STF: c++ 17)_

    ![Definir padrão de linguagem](images/visual-layer-interop/language-standard.png)

    - **Vinculador** > **Entrada** > **Dependências Adicionais** deve incluir "_windowsapp.lib_". Se não estiver incluído na lista, adicione-o.

    ![Adicionar dependência do vinculador](images/visual-layer-interop/linker-dependencies.png)

1. Atualizar o cabeçalho pré-compilado

    - Renomeie `stdafx.h` e `stdafx.cpp` como `pch.h` e `pch.cpp`, respectivamente.
    - Defina a propriedade do projeto **C/C++**  > **Cabeçalhos Pré-compilados** > **Arquivo de Cabeçalho Pré-compilado** para *pch.h*.
    - Localize e substitua `#include "stdafx.h"` por `#include "pch.h"` em todos os arquivos.

        (**Editar** > **Localizar e Substituir** > **Localizar nos Arquivos**)

        ![Localizar e substituir stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - Em `pch.h`, inclua `winrt/base.h` e `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

É uma boa ideia compilar o projeto neste momento para verificar se não há erros antes de continuar.

## <a name="create-a-class-to-host-composition-elements"></a>Criar uma classe para hospedar elementos de composição

Para hospedar o conteúdo criado com a camada visual, crie uma classe (_CompositionHost_) para gerenciar a interoperabilidade e criar elementos de composição. Neste momento, você faz a maior parte da configuração para hospedar APIs de composição, incluindo:

- obter um [Compositor](/uwp/api/windows.ui.composition.compositor), que cria e gerencia objetos no namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition).
- criar um [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) para gerenciar tarefas para as APIs do WinRT.
- criar um [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) e um contêiner de Composição para exibir os objetos de composição.

Tornamos essa classe um singleton para evitar problemas de threading. Por exemplo, só é possível criar uma fila de dispatcher por thread, portanto, criar uma segunda instância de CompositionHost no mesmo thread causaria um erro.

> [!TIP]
> Se necessário, verifique o código completo no final do tutorial para garantir que todo o código esteja nos locais certos enquanto você trabalha no tutorial.

1. Adicione um novo arquivo de classe ao projeto.
    - No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto _HelloComposition_.
    - No menu de contexto, selecione **Adicionar** > **Classe....** .
    - Na caixa de diálogo **Adicionar Classe**, nomeie a classe _CompositionHost.cs_ e clique em **Adicionar**.

1. Inclua cabeçalhos e _usos_ necessários para a interoperabilidade de composição.
    - Em CompositionHost.h, adicione estas _inclusões_ na parte superior do arquivo.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - Em CompositionHost.cpp, adicione estes _usos_ na parte superior do arquivo, depois de qualquer _inclusão_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Edite a classe para usar o padrão singleton.
    - Em CompositionHost.h, torne o Construtor privado.
    - Declare um método _GetInstance_ público.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - Em CompositionHost.cpp, adicione a definição do método _GetInstance_.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. Em CompositionHost.h, declare variáveis de membro privado para o Compositor, DispatcherQueueController e DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Adicione um método público para inicializar os objetos de interoperabilidade de composição.
    > [!NOTE]
    > Em _Inicializar_, chame os métodos _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_ e _CreateCompositionRoot_. Você criará esses métodos nas próximas etapas.

    - Em CompositionHost.h, declare um método público chamado _Initialize_ que usa um HWND como argumento.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - Em CompositionHost.cpp, adicione a definição do método _Initialize_.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Crie uma fila de dispatcher no thread que usará Windows Composition.

    Um Compositor deve ser criado em um thread que tenha uma fila de dispatcher, para que esse método seja chamado primeiro durante a inicialização.

    - Em CompositionHost.h, declare um método particular chamado _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - Em CompositionHost.cpp, adicione a definição do método _EnsureDispatcherQueue_.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Registre a janela do seu aplicativo como um destino de composição.
    - Em CompositionHost.h, declare um método privado chamado _CreateDesktopWindowTarget_ que usa um HWND como argumento.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - Em CompositionHost.cpp, adicione a definição do método _CreateDesktopWindowTarget_.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Crie um contêiner visual raiz para manter objetos visuais.
    - Em CompositionHost.h, declare um método particular chamado _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - Em CompositionHost.cpp, adicione a definição do método _CreateCompositionRoot_.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Compile o projeto agora para verificar se não há erros.

Esses métodos configuram os componentes necessários para a interoperabilidade entre a camada visual UWP e as APIs do Win32. Agora você pode adicionar conteúdo ao seu aplicativo.

### <a name="add-composition-elements"></a>Adicionar elementos de composição

Com a criação da infraestrutura, agora você pode gerar o conteúdo de composição que deseja mostrar.

Para este exemplo, você adiciona um código que cria um quadrado de cor aleatória [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) com uma animação que faz com que ele seja descartado após um pequeno período.

1. Adicione um elemento de composição.
    - Em CompositionHost.h, declare um método público chamado _AddElement_ que usa três valores de **float** como argumentos.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - Em CompositionHost.cpp, adicione a definição do método _AddElement_.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Criar e mostrar a janela

Agora, você pode adicionar um botão e o conteúdo de composição UWP à sua interface do usuário do Win32.

1. No HelloComposition.cpp, na parte superior do arquivo, inclua _CompositionHost.h_, defina BTN_ADD e obtenha uma instância de CompositionHost.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. No método `InitInstance`, altere o tamanho da janela que é criada. (Nesta linha, altere `CW_USEDEFAULT, 0` para `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. Na função WndProc, adicione `case WM_CREATE` ao bloco de opção de _mensagem_. Nesse caso, você inicializa o CompositionHost e cria o botão.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. Também na função WndProc, manipule o clique do botão para adicionar um elemento de composição à interface do usuário. 

    Adicione `case BTN_ADD` ao bloco de opção _wmId_ dentro do bloco WM_COMMAND.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

Agora você pode compilar e executar seu aplicativo. Se necessário, verifique o código completo no final do tutorial para garantir que todo o código esteja nos locais certos.

Ao executar o aplicativo e clicar no botão, você deverá ver quadrados animados adicionados à interface do usuário.

## <a name="additional-resources"></a>Recursos adicionais

- [Exemplo do HelloComposition para Win32 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Introdução ao Win32 e C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) (UWP)
- [Aprimorar seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Namespace Windows.UI.Composition](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>Código completo

Confira o código completo para a classe CompositionHost e o método InitInstance.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (Parcial)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```
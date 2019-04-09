---
title: Usando a camada Visual com o Win32
description: Use a camada Visual para aprimorar a interface do usuário do seu aplicativo de desktop do Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, rendering, composition, win32
ms.localizationpriority: medium
ms.openlocfilehash: cfaa0d19b7a7361c5d604636c30beda0f416d063
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58174033"
---
# <a name="using-the-visual-layer-with-win32"></a>Usando a camada Visual com o Win32

Você pode usar as APIs de composição de tempo de execução do Windows (também chamado de [camada Visual](visual-layer.md)) em seus aplicativos do Win32 para criar experiências modernas ou luz para os usuários do Windows 10.

O código completo para este tutorial está disponível no GitHub: [Exemplo do Win32 HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Aplicativos universais do Windows que precisam de um controle preciso sobre sua composição de interface do usuário tem acesso para o [Windows.UI.Composition](/uwp/api/windows.ui.composition) namespace exercer controle refinado sobre como sua interface do usuário é composto e renderizado. Essa API de composição não está limitado a aplicativos UWP, no entanto. Aplicativos de área de trabalho do Win32 podem tirar proveito dos sistemas modernos de composição no UWP e Windows 10.

## <a name="prerequisites"></a>Pré-requisitos

A API de hospedagem de UWP tem esses pré-requisitos.

- Vamos supor que você tenha alguma familiaridade com o desenvolvimento de aplicativos usando o Win32 e UWP. Para saber mais, veja:
  - [Introdução ao Win32 eC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/)
  - [Aprimore seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 versão 1803 ou posterior
- SDK do Windows 10 17134 ou posterior

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Como usar as APIs de composição de um aplicativo de desktop do Win32

Neste tutorial, você criará um simples do Win32 C++ aplicativo e adicionar elementos de composição de UWP a ele. O foco é corretamente Configurando o projeto, criando o código de interoperabilidade e desenhar algo simples usando as APIs de composição do Windows. O aplicativo concluído se parece com isso.

![O interface do usuário do aplicativo em execução](images/interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Criar um C++ projeto do Win32 no Visual Studio

A primeira etapa é criar o projeto de aplicativo Win32 no Visual Studio.

Para criar um novo projeto de aplicativo Win32 no C++ nomeado _HelloComposition_:

1. Abra o Visual Studio e selecione **arquivo** > **New** > **projeto**.

    O **novo projeto** caixa de diálogo é aberta.
1. Sob o **Installed** categoria, expanda o **Visual C++**  nó e, em seguida, selecione **área de trabalho do Windows**.
1. Selecione o **aplicativo de área de trabalho do Windows** modelo.
1. Insira o nome _HelloComposition_, em seguida, clique em **Okey**.

    Visual Studio cria o projeto e abre o editor para o arquivo de aplicativo principal.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar o projeto para usar APIs do Windows Runtime

Para usar o tempo de execução do Windows (WinRT) APIs em seu aplicativo do Win32, podemos usar C++/WinRT. Você precisa configurar seu projeto do Visual Studio para adicionar C++/WinRT suporte.

(Para obter detalhes, consulte [Introdução C++/WinRT - modificar um projeto de aplicativo de área de trabalho do Windows para adicionar C++suporte /WinRT](../cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Dos **Project** menu, abra as propriedades do projeto (_HelloComposition propriedades_) e verifique se as seguintes configurações estão definidas para os valores especificados:

    - Para **Configuration**, selecione _todas as configurações de_. Para **plataforma**, selecione _todas as plataformas_.
    - **Propriedades de configuração** > **gerais** > **Windows SDK versão** = _10.0.17763.0_ ou maior

    ![Defina a versão do SDK](images/interop/sdk-version.png)

    - **C /C++** > **idioma**  >   **C++ idioma padrão** = _ISO C++ padrão c++17 (/ stf:c + + 17)_

    ![Conjunto padrão de linguagem](images/interop/language-standard.png)

    - **Vinculador** > **entrada** > **dependências adicionais** deve incluir "_windowsapp.lib_". Se ele não está incluído na lista, adicione-o.

    ![Adicionar a dependência do vinculador](images/interop/linker-dependencies.png)

1. Atualizar o cabeçalho pré-compilado

    - Renomeie `stdafx.h` e `stdafx.cpp` à `pch.h` e `pch.cpp`, respectivamente.
    - Defina a propriedade do projeto **C/C++** > **cabeçalhos pré-compilados** > **arquivo de cabeçalho pré-compilado** para *pch*.
    - Localizar e substituir `#include "stdafx.h"` com `#include "pch.h"` em todos os arquivos.

        (**Edite** > **localizar e substituir** > **localizar nos arquivos**)

        ![Localizar e substituir Stdafx. h](images/interop/replace-stdafx.png)

    - Na `pch.h`, inclua `winrt/base.h` e `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

É uma boa ideia para compilar o projeto neste momento para se certificar de que não existem erros antes de continuar.

## <a name="create-a-class-to-host-composition-elements"></a>Criar uma classe para elementos de composição do host

Para hospedar conteúdo você criar com a camada visual, crie uma classe (_CompositionHost_) para gerenciar a interoperabilidade e criar elementos de composição. Isso é onde você executa a maioria da configuração para a hospedagem de APIs de composição, incluindo:

- Obtendo uma [Compositor](/uwp/api/windows.ui.composition.compositor), que cria e gerencia objetos do [Windows.UI.Composition](/uwp/api/windows.ui.composition) namespace.
- Criando um [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) para gerenciar as tarefas para as APIs do WinRT.
- Criando um [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) e contêiner de composição para exibir os objetos de composição.

Podemos fazer isso classe um singleton para evitar problemas de threading. Por exemplo, você pode criar uma fila de dispatcher por thread, somente para que instanciar uma segunda instância do CompositionHost no mesmo thread causaria um erro.

> [!TIP]
> Se você precisar, verifique o código completo no final do tutorial para garantir que todo o código está nos lugares certos enquanto você trabalha com o tutorial.

1. Adicione um novo arquivo de classe ao projeto.
    - Na **Gerenciador de soluções**, clique com botão direito do _HelloComposition_ projeto.
    - No menu de contexto, selecione **Add** > **classe...** .
    - No **Add Class** caixa de diálogo, nomeie a classe _CompositionHost.cs_, em seguida, clique em **adicionar**.

1. Incluir cabeçalhos e _usos_ necessária para a interoperabilidade de composição.
    - No CompositionHost.h, adicioná-las _inclui_ na parte superior do arquivo.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - No CompositionHost.cpp, adicioná-las _usos_ na parte superior do arquivo, após qualquer _inclui_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Edite a classe para usar o padrão singleton.
    - No CompositionHost.h, torne o construtor particular.
    - Declarar um membro público estático _GetInstance_ método.

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

    - No CompositionHost.cpp, adicione a definição do _GetInstance_ método.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. No CompositionHost.h, declare membro privado variáveis para o Compositor, DispatcherQueueController e DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Adicione um método público para inicializar os objetos de interoperabilidade de composição.
    > [!NOTE]
    > Na _inicializar_, você chama o _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_, e _CreateCompositionRoot_ métodos. Você pode criar esses métodos nas próximas etapas.

    - No CompositionHost.h, declare um método público chamado _inicializar_ que usa um HWND como um argumento.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - No CompositionHost.cpp, adicione a definição do _inicializar_ método.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Crie uma fila do dispatcher no thread que usará a composição do Windows.

    Um Compositor deve ser criado em um thread que tem uma fila do distribuidor, portanto, esse método é chamado primeiro durante a inicialização.

    - No CompositionHost.h, declare um método privado nomeado _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - No CompositionHost.cpp, adicione a definição do _EnsureDispatcherQueue_ método.

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

1. Registre a janela de seu aplicativo como um destino de composição.
    - No CompositionHost.h, declare um método privado nomeado _CreateDesktopWindowTarget_ que usa um HWND como um argumento.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - No CompositionHost.cpp, adicione a definição do _CreateDesktopWindowTarget_ método.

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

1. Crie um contêiner visual de raiz para armazenar objetos visuais.
    - No CompositionHost.h, declare um método privado nomeado _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - No CompositionHost.cpp, adicione a definição do _CreateCompositionRoot_ método.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 24, 24, 0 });
        m_target.Root(root);
    }
    ```

Compile o projeto agora para garantir que não existem erros.

Esses métodos de configurar os componentes necessários para a interoperabilidade entre a camada visual do UWP e as APIs do Win32. Agora você pode adicionar conteúdo ao seu aplicativo.

### <a name="add-composition-elements"></a>Adicionar elementos de composição

Com a infra-estrutura in-loco, agora você pode gerar o conteúdo de composição que você deseja mostrar.

Neste exemplo, você adicionar o código que cria um quadrado simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Adicione um elemento de composição.
    - No CompositionHost.h, declare um método público chamado _AddElement_ que usa 3 **float** valores como argumentos.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - No CompositionHost.cpp, adicione a definição do _AddElement_ método.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
            visual.Size({ size, size });
            visual.Offset({ x, y, 0.0f, });

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Criar e exibir a janela

Agora, você pode adicionar o conteúdo de composição de UWP à sua interface do usuário do Win32. Usar seu aplicativo _InitInstance_ método para inicializar a composição do Windows e gerar o conteúdo.

1. No HelloComposition.cpp, inclua _CompositionHost.h_ na parte superior do arquivo.

    ```cppwinrt
    #include "CompositionHost.h"
    ```

1. No `InitInstance` método, adicione código para inicializar e usar a classe CompositionHost.

    Adicione este código depois que o HWND é criado e antes da chamada para `ShowWindow`.

    ```cppwinrt
    CompositionHost* compHost = CompositionHost::GetInstance();
    compHost->Initialize(hWnd);
    compHost->AddElement(150, 10, 10);
    ```

Agora você pode compilar e executar seu aplicativo. Se for necessário, verifique o código completo no final do tutorial para garantir que todo o código está nos lugares certos.

Quando você executa o aplicativo, você deverá ver um quadrado azul adicionado na interface do usuário.

## <a name="additional-resources"></a>Recursos adicionais

- [Exemplo do Win32 HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Introdução ao Win32 eC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/) (UWP)
- [Aprimore seu aplicativo da área de trabalho para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Namespace Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Aqui está o código completo para a classe CompositionHost e o método InitInstance.

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
    root.Offset({ 24, 24, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
        visual.Size({ size, size });
        visual.Offset({ x, y, 0.0f, });

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp---initinstance"></a>HelloComposition.cpp - InitInstance

```cppwinrt
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   CompositionHost* compHost = CompositionHost::GetInstance();
   compHost->Initialize(hWnd);
   compHost->AddElement(150, 10, 10);

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}
```
---
title: Definir a estrutura do aplicativo UWP do jogo
description: A primeira parte da codificação de um jogo UWP (Plataforma Universal do Windows) com DirectX é criar a estrutura que permite que o objeto de jogo interaja com o Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 16af4bcabbc21c60a5dc0006da51f5bd23eef791
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966449"
---
#  <a name="define-the-uwp-app-framework"></a>Definir a estrutura do aplicativo UWP

Crie uma estrutura para permitir que o objeto do jogo interaja com o Windows, incluindo as propriedades do Windows Runtime, como a manipulação de eventos de suspensão/retomada, o foco da janela e os ajustes.

Para configurar essa estrutura, primeiro obtenha um provedor de exibição para que o singleton do aplicativo, que é o objeto do Windows Runtime que define uma instância do aplicativo em execução, possa acessar os recursos gráficos necessários. Por meio do Windows Runtime, o jogo também estabelece uma conexão direta com a interface gráfica, permitindo que você especifique os recursos de que precisa e a forma de tratá-los.

O objeto de provedor de exibição implementa a interface __IFrameworkView__, que consiste em uma série de métodos que precisam ser configurados para criar esse jogo de exemplo.

Você precisará implementar esses cinco métodos que o singleton do aplicativo chama:
* [__Initialize__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behavior)
* [__Load__](#load-method-of-the-view-provider)
* [__Run__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

O método __Initialize__ é chamado na inicialização do aplicativo. O método __SetWindow__ é chamado após __Initialize__. E depois o método __Load__ é chamado. O método __Run__ é chamado quando o jogo está sendo executado. Quando o jogo chega ao fim, o método __Uninitialize__ é chamado. Para obter mais informações, consulte a [Referência de API __IFrameworkView__](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview). 

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Configure a estrutura de um jogo UWP (Plataforma Universal do Windows) DirectX e implemente a máquina de estado que define o fluxo geral do jogo.

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>Definir a fábrica do provedor de exibição e o objeto do provedor de exibição

Vamos examinar o loop __main__ em __App.cpp__. 

Nesta etapa, criamos uma fábrica para a exibição (que implementa __IFrameworkViewSource__), que por sua vez cria instâncias do objeto de provedor de exibição (que implementa __IFrameworkView__) que define a exibição.

### <a name="main-method"></a>Método principal

Crie um __DirectXApplicationSource__ se você tiver o código de exemplo do GitHub carregado. (Use __Direct3DApplicationSource__ se você estiver usando o modelo original do DirectX) Esta é a fábrica de provedores de visualização que implementa __IFrameworkViewSource__. A interface __IFrameworkViewSource__ da fábrica de provedores de visualização tem um único método definido, __CreateView__.

Em __CoreApplication::Run__, o método __CreateView__ é chamado pelo singleton do aplicativo quando o __Direct3DApplicationSource__ ou __DirectXApplicationSource__ é passado.

O __CreateView__ retorna uma referência para uma nova instância do objeto de aplicativo que implementa __IFrameworkView__, que é o objeto de classe __App__ neste exemplo. O objeto de classe __App__ é o objeto do provedor de exibição.

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>Inicializar o provedor de visualização

Após a criação do objeto do provedor de exibição, o singleton do aplicativo chama o método [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) na inicialização do aplicativo. Por isso, é essencial que esse método manipule a maioria dos comportamentos fundamentais de um jogo UWP; por exemplo, manipular a ativação da janela principal e garantir que o jogo possa lidar com um evento de suspensão repentina (e, posteriormente, com uma possível retomada).

Neste ponto, o aplicativo de jogo pode manipular uma mensagem de suspensão (ou retomada). Contudo, ainda não há janela com a qual trabalhar e o jogo não foi iniciado. Há mais algumas coisas que devem ser feitas.

### <a name="appinitialize-method"></a>Método App::Initialize

Nesse método, crie vários manipuladores de eventos para ativar, suspender e retomar o jogo.

Obtenha acesso aos recursos do dispositivo. A função __make_shared__ é usada para criar um __shared_ptr__ quando o recurso de memória é criado pela primeira vez. Uma vantagem de usar a __make_shared__ é que ela é à prova de exceções. Também usa a mesma chamada para alocar a memória para o bloco de controle e o recurso e, portanto, reduz a sobrecarga de construção.

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>Configurar os comportamentos da janela e de exibição

Agora vamos analisar a implementação de [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509). No método __SetWindow__, você configura os comportamentos da janela e de exibição.

### <a name="appsetwindow-method"></a>Método App::SetWindow

O singleton do aplicativo fornece um objeto [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) que representa a janela principal do jogo, disponibilizando os recursos e eventos para o jogo. Agora que há uma janela com a qual trabalhar, o jogo pode começar a adicionar os eventos e os componentes básicos de interface do usuário.

Em seguida, crie um ponteiro usando o método __CoreCursor__ que pode ser usado por controles de mouse e toque.

Por fim, crie eventos básicos de redimensionamento de janela, fechamento e alterações de DPI (se o dispositivo de vídeo mudar). Para obter mais informações sobre os eventos, acesso a [Manipulação de eventos](tutorial-game-flow-management.md#events-handling).

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>Método Load do provedor de visualização

Após a janela principal ser definida, o singleton do aplicativo chama [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501). Um conjunto de tarefas assíncronas é usado nesse método para criar os objetos do jogo, carregar os recursos de gráficos e iniciar a máquina de estado do jogo. Se você quiser pré-extrair dados ou ativos do jogo, este é um lugar melhor para isso, em vez de **SetWindow** ou **Initialize**. 

Como o Windows impõe restrições ao tempo que um jogo pode levar antes que precise começar a processar a entrada, usando o padrão de tarefa assíncrona, você precisa criar o método __Load__ a ser completado rapidamente para que possa iniciar o processamento da entrada. Se o carregamento levar algum tempo, ou se houver muitos recursos, forneça aos usuários uma barra de progresso atualizada regularmente. Esse método também é usado para fazer todas as preparações necessárias antes do início do jogo, como definir os estados iniciais ou valores globais.

Se você for novo em programação assíncrona e conceitos de paralelismo de tarefas, acesse [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

### <a name="appload-method"></a>Método App::Load

Crie a classe __GameMain__ que contém as tarefas de carregamento.

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>Construtor GameMain

* Crie e inicialize o renderizador do jogo. Para obter mais informações, consulte [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).
* Crie e inicialize o objeto Simple3Dgame. Para obter mais informações, consulte [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md).    
* Crie o objeto de controle de interface de usuário do jogo e exiba a sobreposição de informações do jogo para mostrar uma barra de progresso à medida que os arquivos de recurso são carregados. Para saber mais, consulte [Adicionando uma interface do usuário](tutorial--adding-a-user-interface.md).
* Crie o controlador para que possa ler entradas do controlador (toque, mouse, controle sem fio do XBox). Para obter mais informações, consulte [Adicionando controles](tutorial--adding-controls.md).
* Depois que o controlador é inicializado, definimos duas áreas retangulares nos cantos inferiores esquerdo e direito da tela para os controles de movimento e de toque da câmera, respectivamente. O jogador usa o retângulo inferior esquerdo, definido pela chamada para **SetMoveRect**, como um painel de controle virtual para mover a câmera para frente e para trás, de um lado para outro. O retângulo inferior direito, definido pelo método **SetFireRect** é usado como um botão virtual para disparar a munição.
* Use __create_task__ e __create_task::then__ para dividir o carregamento de recursos em dois estágios separados. Como o acesso ao contexto de dispositivo do Direct3D 11 é restrito ao thread em que o contexto de dispositivo foi criado enquanto o acesso ao dispositivo Direct3D 11 para criação de objetos é livre de threads, isso significa que a tarefa **CreateGameDeviceResourcesAsync** pode ser executada em um thread separado da tarefa de conclusão (*FinalizeCreateGameDeviceResources*), que é executada no thread original. Usamos um padrão semelhante para carregar os recursos de nível com o **LoadLevelAsync** e **FinalizeLoadLevel**.

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::TooSmall;
            m_controller->Active(false);
            m_uiControl->HideGameInfoOverlay();
            m_uiControl->ShowTooSmall();
            m_renderNeeded = true;
        }
        else if (!m_haveFocus)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            m_controller->Active(false);
            m_uiControl->SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
        }
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>Método Run do provedor de visualização

Os três métodos anteriores: __Initialize__, __SetWindow__ e __Load__ prepararam o estágio. Agora o jogo pode avançar para o método **Run**, dando início à diversão! Os eventos que ele usa para passar de um estado do jogo para outro são enviados e processados. Os elementos gráficos são atualizados, assim como os ciclos de loop do jogo.

### <a name="apprun"></a>App::Run

Inicie um loop __while__ que termina quando o jogador fecha a janela do jogo.

O código de exemplo transita para um dos dois estados na máquina de estado do mecanismo do jogo:
    * __Deactivated__: a janela do jogo é desativada ou (perde o foco) ou ajustada. Quando isso acontece, o jogo suspende o processamento de eventos e aguarda a janela focar ou desajustar.
    * __TooSmall__: o jogo atualiza seu próprio estado e renderiza os gráficos para exibição.

Quando seu jogo tem foco, você deve lidar com todos os eventos na fila de mensagens conforme eles chegam. Por isso, é preciso chamar [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) com a opção **ProcessAllIfPresent**. Outras opções podem causar atrasos no processamento de eventos de mensagens, que podem fazer com que o jogo pareça não responder, ou resultar em comportamentos de toque lentos, e não "fluidos".

Quando o jogo não estiver visível, suspenso ou encaixado, você não quer que ele consuma ciclos de recursos para enviar mensagens que nunca chegarão. Nesse caso, o jogo precisa usar o **ProcessOneAndAllPending**, que é bloqueado até receber um evento e processa esse evento e todos os outros que chegam na fila do processo durante o processamento da primeira. [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) retorna, em seguida, imediatamente após o processamento da fila.

```cpp
void App::Run()
{
    m_main->Run();
}

void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
                    switch (m_pressResult)
                    {
                    case PressResultState::LoadGame:
                        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                        break;

                    case PressResultState::PlayLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                        break;

                    case PressResultState::ContinueLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::Pause);
                        break;
                    }
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="uninitialize-method-of-the-view-provider"></a>Método Uninitialize do provedor de visualização

Quando o usuário encerra, por fim, a sessão de jogo, é necessário limpar tudo. Esta é a origem da propriedade **Uninitialize**.

No Windows 10, fechamento da janela do aplicativo não interromper o processo do aplicativo, mas em vez disso, ele grava o estado do singleton do aplicativo na memória. Se for necessário que ocorra algo especial quando o sistema precisar usar essa memória, incluindo algum tipo de limpeza especial de recursos, insira o código da limpeza nesse método.

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>Dicas

Ao desenvolver seu próprio jogo, crie o código de inicialização com base nesses métodos. Aqui está uma lista simples de sugestões básicas para cada método:

-   Use **Initialize** para alocar suas classes principais e conectar os manipuladores de eventos básicos.
-   Use **SetWindow** para criar sua janela principal e conectar os eventos específicos de janela.
-   Use **Load** para manipular a configuração restante e iniciar a criação assíncrona de objetos e o carregamento de recursos. Se você precisar criar arquivos ou dados temporários, como ativos gerados por procedimentos, faça-o aqui também.


## <a name="next-steps"></a>Próximas etapas

Abrange a estrutura básica de um jogo UWP com o DirectX. Tenha esses cinco métodos em mente quando nos referirmos a eles em outras partes do passo a passo. Em seguida, vamos nos aprofundar em como gerenciar os estados do jogo e a manipulação de eventos para manter o jogo funcionando em [Gerenciamento de fluxo de jogo](tutorial-game-flow-management.md).




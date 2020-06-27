---
title: Definir a estrutura do aplicativo UWP do jogo
description: A primeira etapa na codificação de um jogo de Plataforma Universal do Windows (UWP) é a criação da estrutura que permite ao objeto de aplicativo interagir com o Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jogos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5052b4e71196950b6a8a91aaa271b5550fb448b5
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409615"
---
#  <a name="define-the-games-uwp-app-framework"></a>Definir a estrutura do aplicativo UWP do jogo

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

A primeira etapa na codificação de um jogo da Plataforma Universal do Windows (UWP) é criar a estrutura que permite que o objeto de aplicativo interaja com o Windows, incluindo recursos de Windows Runtime, como a manipulação de eventos de suspensão/retomada, alterações no foco da janela e encaixe.

## <a name="objectives"></a>Objetivos

* Configure a estrutura para um jogo do DirectX Plataforma Universal do Windows (UWP) e implemente o computador de estado que define o fluxo de jogo geral.

> [!NOTE]
> Para acompanhar este tópico, examine o código-fonte do jogo de exemplo [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que você baixou.

## <a name="introduction"></a>Introdução

No tópico [Configurar o projeto de jogo](tutorial--setting-up-the-games-infrastructure.md) , apresentamos a função **wWinMain** , bem como as interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) e [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Aprendemos que a classe de **aplicativo** (que você pode ver definida no `App.cpp` arquivo de código-fonte no projeto **Simple3DGameDX** ) serve como *fábrica de provedor de exibição* e *provedor de exibição*.

Este tópico escolhe a partir daí e entra em muito mais detalhes sobre como a classe de **aplicativo** em um jogo deve implementar os métodos de **IFrameworkView**.

## <a name="the-appinitialize-method"></a>O método App:: Initialize

Após a inicialização do aplicativo, o primeiro método que o Windows chama é nossa implementação de [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize).

Sua implementação deve lidar com os comportamentos mais fundamentais de um jogo UWP, como certificar-se de que o jogo possa lidar com um evento de suspensão (e um possível reinício posterior) inscrevendo-se nesses eventos. Também temos acesso ao dispositivo adaptador de vídeo aqui, portanto, podemos criar recursos gráficos que dependem do dispositivo.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

Evite ponteiros brutos sempre que possível (e é quase sempre possível).

- Para tipos de Windows Runtime, muitas vezes você pode evitar ponteiros completamente e apenas construir um valor na pilha. Se você precisar de um ponteiro, use [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (Vamos ver um exemplo disso em breve).
- Para ponteiros exclusivos, use [**std:: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) e **std:: make_unique**.
- Para ponteiros compartilhados, use [**std:: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) e **std:: make_shared**.

## <a name="the-appsetwindow-method"></a>O método App:: SetWindow

Após a **inicialização**, o Windows chama nossa implementação de [**IFrameworkView:: SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), passando um objeto [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) que representa a janela principal do jogo.

Em **App:: SetWindow**, assinamos eventos relacionados à janela e configuramos alguns comportamentos de janela e exibição. Por exemplo, construímos um ponteiro do mouse (por meio da classe [**CoreCursor**](/uwp/api/windows.ui.core.corecursor) ), que pode ser usado por controles de toque e mouse. Também passamos o objeto Window para nosso objeto de recursos dependente de dispositivo.

Falaremos mais sobre como manipular eventos no tópico [Gerenciamento de fluxo de jogos](tutorial-game-flow-management.md#event-handling) .

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>O método App:: Load

Agora que a janela principal está definida, nossa implementação de [**IFrameworkView:: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) é chamada. A **carga** é um local melhor para buscar previamente dados de jogos ou ativos do que **inicializar** e **SetWindow**.

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

Como você pode ver, o trabalho real é delegado para o construtor do objeto **GameMain** que fazemos aqui. A classe **GameMain** é definida em `GameMain.h` e `GameMain.cpp` .

### <a name="the-gamemaingamemain-constructor"></a>O Construtor GameMain:: GameMain

O construtor **GameMain** (e as outras funções de membro que ele chama) inicia um conjunto de operações de carregamento assíncronas para criar os objetos do jogo, carregar recursos gráficos e inicializar a máquina de estado do jogo. Também fazemos os preparativos necessários antes do jogo começar, como a definição de quaisquer Estados iniciais ou valores globais.

O Windows impõe um limite no tempo que seu jogo pode tomar antes de começar a processar a entrada. Portanto, usar asyc, como fazemos aqui, significa que a **carga** pode retornar rapidamente enquanto o trabalho que ele iniciou continua em segundo plano. Se o carregamento levar muito tempo ou se houver muitos recursos, então fornecer aos usuários uma barra de progresso atualizada com frequência é uma boa ideia. 

Se você for novo na programação assíncrona, consulte [operações de simultaneidade e assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
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
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

Aqui está uma descrição da sequência de trabalho que é inicializada pelo construtor.

- Crie e inicialize um objeto do tipo **GameRenderer**. Para obter mais informações, consulte [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).
- Crie e inicialize um objeto do tipo **Simple3DGame**. Para obter mais informações, consulte [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md).
- Crie o objeto de controle de interface do usuário do jogo e exiba a sobreposição de informações do jogo para mostrar uma barra de progresso conforme os arquivos de recurso são carregados. Para saber mais, consulte [Adicionando uma interface do usuário](tutorial--adding-a-user-interface.md).
- Crie um objeto de controlador para ler a entrada do controlador (toque, mouse ou controlador sem fio Xbox). Para obter mais informações, consulte [Adicionando controles](tutorial--adding-controls.md).
- Defina duas áreas retangulares nos cantos inferior esquerdo e inferior direito da tela para os controles de toque mover e câmera, respectivamente. O Player usa o retângulo inferior esquerdo (definido na chamada para **SetMoveRect**) como um painel de controle virtual para mover a câmera para frente e para trás e lado a lado. O retângulo inferior direito (definido pelo método **SetFireRect** ) é usado como um botão virtual para acionar o Ammo.
- Use corrotinas para interromper o carregamento de recursos em estágios separados. O acesso ao contexto do dispositivo Direct3D é restrito ao thread no qual o contexto do dispositivo foi criado; Embora o acesso ao dispositivo Direct3D para a criação de objetos seja de segmento livre. Consequentemente, a corotina **GameRenderer:: CreateGameDeviceResourcesAsync** pode ser executada em um thread separado da tarefa de conclusão (**GameRenderer:: FinalizeCreateGameDeviceResources**), que é executada no thread original.
- Usamos um padrão semelhante para carregar recursos de nível com **Simple3DGame:: LoadLevelAsync** e **Simple3DGame:: FinalizeLoadLevel**.

Veremos mais de **GameMain:: InitializeGameState** no próximo tópico (gerenciamento de[fluxo de jogos](tutorial-game-flow-management.md)).

## <a name="the-apponactivated-method"></a>O método App:: OnActivated

Em seguida, o evento [**CoreApplicationView:: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) é gerado. Portanto, qualquer manipulador de eventos **OnActivated** que você tenha (como nosso método **App:: OnActivated** ) é chamado.

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

O único trabalho que fazemos aqui é ativar o [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)principal. Como alternativa, você pode optar por fazer isso em **App:: SetWindow**.

## <a name="the-apprun-method"></a>O método App:: Run

**Inicializar**, **SetWindow**e **Load** definiram o estágio. Agora que o jogo está em funcionamento, nossa implementação de [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) é chamada.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Novamente, o trabalho é delegado para **GameMain**.

### <a name="the-gamemainrun-method"></a>O método GameMain:: Run

**GameMain:: Run** é o loop principal do jogo; Você pode encontrá-lo em `GameMain.cpp` . A lógica básica é que, embora a janela do seu jogo permaneça aberta, despache todos os eventos, atualize o temporizador e, em seguida, processe e apresente os resultados do pipeline de gráficos. Além disso, os eventos usados para fazer a transição entre os Estados do jogo são expedidos e processados.

O código aqui também se preocupa com dois dos Estados na máquina de estado do mecanismo de jogo.

- **UpdateEngineState::D eactivated**. Isso especifica que a janela do jogo está desativada (tem o foco perdido) ou está encaixada. 
- **UpdateEngineState:: TooSmall**. Isso especifica que a área do cliente é muito pequena para renderizar o jogo.

Em qualquer um desses Estados, o jogo suspende o processamento de eventos e aguarda a janela se concentrar, desencaixar ou ser redimensionado.

Quando seu jogo tem foco, você deve lidar com todos os eventos na fila de mensagens conforme eles chegam. Por isso, é preciso chamar [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) com a opção **ProcessAllIfPresent**. Outras opções podem causar atrasos no processamento de eventos de mensagem, que podem fazer com que seu jogo fique sem resposta ou que resulte em comportamentos de toque que se sintam lentos.

Quando o jogo não está visível, suspenso nem encaixado, você não deseja que ele consuma nenhum ciclo de recursos para enviar mensagens que nunca chegarão. Nesse caso, o jogo deve usar a opção **ProcessOneAndAllPending** . Essa opção é bloqueada até obter um evento e, em seguida, processa esse evento (bem como qualquer outro que chegue na fila de processos durante o processamento do primeiro). **CoreWindowDispatch. ProcessEvents** , em seguida, retorna imediatamente depois que a fila é processada.

```cppwinrt
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
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>O método App:: Uninitialize

Quando o jogo termina, nossa implementação de [**IFrameworkView:: Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) é chamada. Essa é a nossa oportunidade para executar a limpeza. Fechar a janela do aplicativo não interrompe o processo do aplicativo; Mas, em vez disso, ele grava o estado do singleton do aplicativo na memória. Se algo especial precisar ocorrer quando o sistema recuperar essa memória, incluindo qualquer limpeza especial de recursos, coloque o código para essa limpeza em **uninitializeize**.

Em nosso caso, **App:: Uninitialize** é um não op.

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>Dicas

Ao desenvolver seu próprio jogo, crie seu código de inicialização em volta dos métodos descritos neste tópico. Aqui está uma lista simples de sugestões básicas para cada método.

- Use **Initialize** para alocar suas classes principais e conecte os manipuladores de eventos básicos.
- Use **SetWindow** para assinar qualquer evento específico de janela e passe sua janela principal para o objeto de recursos dependente de dispositivo para que ele possa usar essa janela ao criar uma cadeia de permuta.
- Use **carregar** para lidar com qualquer configuração restante e para iniciar a criação assíncrona de objetos e o carregamento de recursos. Se você precisar criar arquivos temporários ou dados, como ativos gerados em procedimentos, faça isso aqui também.

## <a name="next-steps"></a>Próximas etapas

Este tópico abordou parte da estrutura básica de um jogo UWP que usa o DirectX. É uma boa ideia manter esses métodos em mente, pois iremos nos referir a alguns deles em tópicos posteriores.

No próximo tópico &mdash; [Gerenciamento de fluxo de jogos](tutorial-game-flow-management.md) &mdash; , vamos analisar detalhadamente como gerenciar Estados de jogos e manipulação de eventos para manter o jogo fluindo.

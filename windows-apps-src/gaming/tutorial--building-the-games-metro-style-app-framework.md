---
author: mtoepke
title: Definir a estrutura do aplicativo UWP (Plataforma Universal do Windows) do jogo
description: "A primeira parte da codificação de um jogo UWP (Plataforma Universal do Windows) com DirectX é criar a estrutura que permite que o objeto de jogo interaja com o Windows."
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2ebc7bca06454f78ab375058e49f012cacb00cc8

---

#  Definir a estrutura do aplicativo UWP (Plataforma Universal do Windows) do jogo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A primeira parte da codificação de um jogo UWP (Plataforma Universal do Windows) com DirectX é criar a estrutura que permite que o objeto de jogo interaja com o Windows. Isso inclui propriedades do Windows Runtime, como manipulação de eventos de suspensão/retomada, foco da janela e ajuste, além de eventos, interações e transições da interface do usuário. Vamos examinar como o jogo de exemplo é estruturado e como ele define a máquina de estado de alto nível para a interação entre jogador e sistema.

## Objetivo


-   Configurar a estrutura de um jogo UWP DirectX e implementar a máquina de estado que define o fluxo geral do jogo.

## Inicializando e iniciando o provedor de visualização


Em qualquer jogo UWP DirectX, você precisa obter um provedor de exibição que o singleton do aplicativo (o objeto do Windows Runtime que define uma instância do aplicativo em execução) possa usar para acessar os recursos gráficos necessários. Por meio do Windows Runtime, o aplicativo estabelece conexão direta com a interface gráfica, mas você deve especificar os recursos de que precisa e a forma de tratá-los.

Conforme abordado em [Configurando o projeto de jogo](tutorial--setting-up-the-games-infrastructure.md), o Microsoft Visual Studio 2015 oferece uma implementação de um renderizador básico para DirectX no arquivo **Sample3DSceneRenderer.cpp** disponível quando você seleciona o modelo do aplicativo **DirectX 11 (Universal do Windows)**.

Para obter mais detalhes sobre como entender e criar um provedor de exibição e um renderizador, veja [Como configurar UWP com C++ e DirectX para obter uma visualização do DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Basta dizer que você deve fornecer a implementação de cinco métodos chamados pelo singleton do aplicativo:

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

No modelo do aplicativo DirectX11 (Universal do Windows), esses cinco métodos são definidos no objeto **App** em [App.h](#code_sample). Vamos dar uma olhada no modo como eles são implementados neste jogo.

O método Initialize do provedor de visualização

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

O singleton do aplicativo chama primeiro **Initialize**. Por isso, é essencial que esse método manipule a maioria dos comportamentos fundamentais de um jogo UWP; por exemplo, manipular a ativação da janela principal e garantir que o jogo possa lidar com um evento de suspensão repentina (e, posteriormente, com uma possível retomada).

Quando o aplicativo do jogo é iniciado, ele também memória específica para o controlador para permitir que o jogador comece a fornecer entradas. Ele também cria novas instâncias não inicializadas da máquina de renderização e estado do jogo. Abordamos os detalhes no tópico sobre [definição do objeto principal do jogo](tutorial--defining-the-main-game-loop.md).

Neste ponto, o aplicativo do jogo pode lidar com uma mensagem de suspensão (ou retomada) e tem memória alocada para o controlador, o renderizador e o próprio jogo. Mas não há janela com a qual trabalhar e o jogo não foi iniciado. Há mais algumas coisas que devem ser feitas.

O método SetWindow do provedor de visualização

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

Agora, com uma chamada para uma implementação de [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), o singleton do aplicativo fornece um objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que representa a janela principal do jogo, disponibilizando os recursos e eventos para o jogo. Como há uma janela com a qual trabalhar, o jogo pode começar a adicionar os componentes e eventos básicos da interface do usuário: um ponteiro (usado por controles de mouse e toque) e os eventos básicos de redimensionamento, fechamento e alterações de DPI (se o dispositivo de exibição da janela mudar).

O aplicativo do jogo também inicia o controlador (pois há uma janela com a qual interagir) e inicia o objeto do jogo. Ele pode ler entradas do controlador (toque, mouse, controle do XBox 360).

Depois que o controlador é inicializado, o aplicativo define duas áreas retangulares nos cantos inferiores esquerdo e direito da tela para os controles de movimento e de toque da câmera, respectivamente. O jogador usa o retângulo inferior esquerdo, definido pela chamada para **SetMoveRect**, como um painel de controle virtual para mover a câmera para frente e para trás, de um lado para outro. O retângulo inferior direito, definido pelo método **SetFireRect** é usado como um botão virtual para disparar a munição.

Tudo começa a se encaixar.

O método Load do provedor de visualização

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

Após a janela principal ser definida, o singleton do aplicativo chama **Load**. No exemplo, esse método usa um conjunto de tarefas assíncronas (a sintaxe definida na [Biblioteca de Padrões Paralelos](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx)) para criar os objetos do jogo, carregar os recursos de gráficos e iniciar a máquina de estado do jogo. Usando o padrão de tarefa assíncrona, o método Load é concluído rapidamente e permite que o aplicativo inicie o processamento de entrada. Nesse método, o aplicativo também exibe uma barra de progresso à medida que os arquivos de recurso são carregados.

Dividimos o carregamento de recursos em dois estágios separados, porque o acesso ao contexto de dispositivo Direct3D 11 é restrito ao thread em que o contexto de dispositivo foi criado, enquanto o acesso ao dispositivo Direct3D 11 para a criação de objetos é livre de threads. A tarefa **CreateGameDeviceResourcesAsync** é executada em um thread separado da tarefa de conclusão (*FinalizeCreateGameDeviceResources*), que é executada no thread original. Usamos um padrão semelhante para carregar os recursos de nível com o **LoadLevelAsync** e **FinalizeLoadLevel**.

Depois de criar objetos do jogo e carregar os recursos de gráficos, inicializamos a máquina de estado do jogo para as condições de partida (por exemplo: definir a contagem de munição inicial, o número do nível e as posições do objeto). Se o estado do jogo indicar que o jogador está continuando um jogo, carregamos o nível atual (o nível no qual o jogador estava quando o jogo foi suspenso).

No método **Load**, fazemos todas as preparações necessárias antes do início do jogo, como definir os estados iniciais ou valores globais. Se você quiser pré-extrair dados ou ativos do jogo, este é um lugar melhor para isso, em vez de **SetWindow** ou **Initialize**. Use tarefas assíncronas em seu jogo para qualquer carregamento, pois o Windows impõe restrições ao tempo que um jogo pode levar antes que ele precise começar a processar a entrada. Se o carregamento levar algum tempo (se houver muitos recursos), forneça aos usuários uma barra de progresso atualizada regularmente.

Ao desenvolver seu próprio jogo, crie o código de inicialização com base nesses métodos. Aqui está uma lista simples de sugestões básicas para cada método:

-   Use **Initialize** para alocar suas classes principais e conectar os manipuladores de eventos básicos.
-   Use **SetWindow** para criar sua janela principal e conectar os eventos específicos de janela.
-   Use **Load** para manipular a configuração restante e iniciar a criação assíncrona de objetos e o carregamento de recursos. Se você precisar criar arquivos ou dados temporários, como ativos gerados por procedimentos, faça-o aqui também.

Então, o jogo de amostra cria uma instância da máquina de estado do jogo e a define para a configuração inicial. Ele lida com todos os eventos do sistema e de entrada. Ele fornece uma janela para exibição de conteúdo. O código de jogabilidade já está pronto para execução.

O método Run do provedor de visualização

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

É aqui que jogaremos parte do aplicativo de jogo. Com três métodos em execução e tudo preparado, o aplicativo de jogo executa o método **Run** e a diversão começa!

No exemplo de jogo, iniciamos um while loop que termina quando o jogador fecha a janela do jogo. O código de exemplo transita para um dos dois estados na máquina de estado do mecanismo do jogo:

-   A janela do jogo é desativada ou (perde o foco) ou ajustada. Quando isso acontece, o jogo suspende o processamento de eventos e aguarda a janela focar ou desajustar.
-   Caso contrário, o jogo atualiza seu próprio estado e renderiza os gráficos para exibição.

Quando seu jogo tem foco, você deve lidar com todos os eventos na fila de mensagens conforme eles chegam. Por isso, é preciso chamar [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) com a opção **ProcessAllIfPresent**. Outras opções podem causar atrasos no processamento de eventos de mensagens, que fazem com que o jogo pareça não responder, ou resultar em comportamentos de toque lentos, e não "fluidos".

Quando o aplicativo não estiver visível, suspenso ou encaixado, não queremos que ele consuma ciclos de recursos para enviar mensagens que nunca chegarão. Se o jogo precisar usar o **ProcessOneAndAllPending**, que é bloqueado até receber um evento e processa esse evento e todos os outros que chegam na fila do processo durante o processamento da primeira. [
              **ProcessEvents**
            ](https://msdn.microsoft.com/library/windows/apps/br208215) retorna, em seguida, imediatamente após o processamento da fila.

O jogo está em execução. Os eventos que ele usa para passar de um estado do jogo para outro estão sendo enviados e processados. Os elementos gráficos são atualizados, assim como os ciclos de loop do jogo. Esperamos que o jogador se divirta. Mas, por fim, a diversão tem que acabar...

...E precisamos organizar tudo. Esta é a origem da propriedade **Uninitialize**.

O método Uninitialize do provedor de visualização

```cpp
void App::Uninitialize()
{
}
```

No jogo de exemplo, permitimos que o singleton do aplicativo do jogo organize tudo após o encerramento do jogo. No Windows 10, o fechamento da janela do aplicativo não encerra seu processo. Em vez disso, essa ação grava o estado do singleton do aplicativo na memória. Se for necessário que ocorra algo especial quando o sistema precisar usar essa memória (algum tipo de limpeza especial de recursos), insira o código da limpeza nesse método.

Nós voltaremos a esses cinco métodos neste tutorial. Por isso, lembre-se deles. Agora, veremos a estrutura geral da engine do jogo e as máquinas de estado que a definem.

## Inicializando o estado da engine do jogo


Como um usuário pode retomar um aplicativo de jogo UWP a partir de um estado suspenso, a qualquer momento, o aplicativo pode ter qualquer quantidade de estados possíveis.

O exemplo de jogo pode ser em um dos três estados quando ele inicia:

-   O loop do jogo estava sendo executado e estava na metade de um nível.
-   O loop do jogo não estava sendo executado, porque um jogo tinha acabado de ser finalizado. (A pontuação máxima foi definida.)
-   Nenhum jogo foi iniciado ou o jogo estava entre níveis. (A pontuação máxima é 0.)

Obviamente, em seu próprio jogo, você poderia ter mais ou menos estados. Novamente, esteja sempre atento ao fato de que o jogo UWP pode ser finalizado a qualquer momento e, ao retomá-lo, o jogador espera que o jogo se comporte como se não tivesse sido interrompido.

No exemplo do jogo, o fluxo do código fica semelhante ao seguinte.

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

A inicialização não se trata tanto de uma inicialização a frio do aplicativo, mas sim da reinicialização do aplicativo após ele ter sido finalizado. O jogo de exemplo sempre salva o estado, o que dá a impressão de que o aplicativo está constantemente em execução. O estado suspenso é quando: a execução do jogo é suspensa, mas seus recursos permanecem na memória. De forma semelhante, o evento de retomada indica que o jogo de exemplo recomeça a partir de onde foi suspenso ou finalizado. Quando o jogo de exemplo reinicia após o término, ele inicia normalmente e depois determina qual o último estado conhecido, assim o jogador pode continuar a jogar.

O fluxograma organiza os estados iniciais e as transições para o processo de inicialização do exemplo de jogo.

![o processo para inicializar e preparar nosso jogo antes que o loop principal comece](images/simple3dgame-appstartup.png)

Dependendo do estado, diferentes opções são apresentadas para o jogador. Se o jogo for retomado a partir no nível médio, ele aparecerá como pausado e a sobreposição apresentará uma opção de continuidade. Se o jogo for retomado a partir de um estado em que foi concluído, ele exibirá as pontuações máximas e uma opção para executar um novo jogo. Por último, se o jogo for retomado antes de um nível ter sido iniciado, a sobreposição apresentará para o usuário uma opção de início.

O exemplo de jogo não diferencia entre o início a frio do jogo em si, que é um jogo que está sendo iniciado pela primeira vez sem um evento suspenso e o jogo que está sendo retomado a partir de um estado suspenso. Este é o design adequado para qualquer aplicativo UWP.

## Manipulando eventos


Nosso código de exemplo registrou vários manipuladores de eventos específicos em **Initialize**, **SetWindow** e **Load**. Provavelmente, você notou que esses eventos eram importantes, pois o código de exemplo fez esse trabalho bem antes de se envolver com a mecânica do jogo ou com o desenvolvimento de elementos gráficos. Você está certo! Esses eventos são fundamentais para uma experiência adequada com um aplicativo UWP. Além disso, como um aplicativo UWP pode ser ativado, desativado, redimensionado, ajustado, desajustado, suspenso ou retomado a qualquer momento, o jogo deve ser registrado para esses eventos o mais cedo possível e deve manipulá-los de modo a manter a experiência fluida e previsível para o jogador.

Consulte os manipuladores de eventos na amostra e os eventos que manipulam. Você pode encontrar o código completo desses manipuladores de eventos em [Código completo desta seção](#code_sample).

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Manipulador de eventos</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Manipula [<strong>CoreApplicationView::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br225018). O aplicativo de jogo foi trazido para o primeiro plano. Por isso, a janela principal foi ativada.</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">Manipula [<strong>DisplayProperties::LogicalDpiChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br226150). O DIP da janela principal do jogo mudou e o aplicativo do jogo ajusta devidamente seus recursos.
<div class="alert">
<strong>Observação</strong> As coordenadas [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559) são em DIPs (Pixels Independentes de Dispositivo), como em [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Como resultado, você deve notificar o Direct2D sobre a alteração no DIP para exibir quaisquer ativos ou primitivas 2D corretamente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Manipula [<strong>CoreApplication::Resuming</strong>](https://msdn.microsoft.com/library/windows/apps/br205859). O aplicativo de jogo restaura o jogo de um estado suspenso.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Manipula [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860). O aplicativo de jogo salva seu estado em disco. Ele tem cinco segundos para salvar o estado no armazenamento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Manipula [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591). O aplicativo de jogo tem visibilidade alterada e se torna visível ou foi tornado invisível por outro aplicativo que se tornou visível.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Manipula [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255). A janela principal do aplicativo de jogo foi desativada ou ativada. Por isso, ela deve remover o foco e pausar o jogo ou obter o foco novamente. Nos dois casos, a sobreposição indica que o jogo está em pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Manipula [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261). O aplicativo de jogo fecha a janela principal e suspende o jogo.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Manipula [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283). O aplicativo de jogo realoca os recursos gráficos e a sobreposição para que acomodem a mudança de tamanho, atualizando o destino de renderização em seguida.</td>
</tr>
</tbody>
</table>

 

Seu jogo deve manipular esses eventos, pois eles fazem parte do design do aplicativo UWP.

## Atualizando o mecanismo do jogo


No loop do jogo em **Run**, o exemplo implementou uma máquina de estado básico para manipular todas as ações principais que o jogador pode realizar. O nível mais alto dessa máquina de estado lida com ações como carregar um jogo, jogar um nível específico ou continuar um nível depois que um jogo foi pausado (pelo sistema ou pelo jogador).

No exemplo de jogo, há três estados principais (UpdateEngineState) em que o jogo pode estar:

-   **Aguardando recursos**. O loop do jogo está em exibição cíclica, não será possível transitar até os recursos (especificamente os recursos gráficos) estarem disponíveis. Quando as tarefas assíncronas para o carregamento de recursos são concluídas, o estado é atualizado para **ResourcesLoaded**. Isso geralmente acontece entre os níveis, quando o nível precisa carregar novos recursos do disco. No exemplo de jogo, simulamos esse comportamento porque o exemplo não precisa de nenhum recurso adicional por nível naquele momento.
-   **Aguardando pressionar**. O loop do jogo está em exibição cíclica, aguardando uma entrada específica do usuário. Esta entrada é um ação do jogador para carregar um jogo, iniciar um nível ou continuar um nível. O código de exemplo se refere a esses subestados como valores de enumeração PressResultState.
-   **Dynamics**. O loop do jogo está sendo executado com o usuário jogando. Enquanto o usuário está jogando, o jogo verifica três condições para as quais ele pode transitar: a expiração do tempo definido para um nível, a conclusão de um nível pelo jogador ou a conclusão de todos os níveis pelo jogador.

Consulte a estrutura do código. O código completo está em [Código completo desta seção](#code_sample).

A estrutura da máquina de estado usada para atualizar o mecanismo do jogo

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }

        break;
    }
}
```

Visualmente, a máquina de estado principal do jogo assemelha-se ao seguinte:

![a máquina de estado prinicpal do nosso jogo](images/simple3dgame-mainstatemachine.png)

Falaremos sobre a lógica do jogo em si com mais detalhes em [Definindo o objeto principal do jogo](tutorial--defining-the-main-game-loop.md). Por enquanto, o importante é que seu jogo é uma máquina de estado. Cada estado deve ter critérios bastante específicos que o definam, e as transições de um estado para outro devem ser baseadas em ações do sistema ou entradas do usuário discretas (como carregamento de recursos gráficos). Ao planejar o jogo, desenhe um diagrama como o que usamos, certificando-se de especificar todas as ações possíveis que um usuário ou sistema pode realizar de modo geral. Os jogos podem ser bastante complicados, e a máquina de estado é uma ferramenta poderosa para visualizar essa complexidade e torná-la gerenciável.

Como você pôde observar, é claro que há máquinas de estado dentro de máquinas de estado. Há uma para o controlador, que manipula todas as entradas aceitáveis que o jogador pode gerar. No diagrama, pressionar é uma forma de entrada do usuário. A máquina de estado não se importa com o que é isso, pois ela trabalha em um nível superior, partindo do pressuposto de que a máquina de estado do controlador manipulará todas as transições que afetam comportamentos de movimento e tiro, além das atualizações de renderização associadas. Falaremos sobre o gerenciamento de estados de entrada no tópico sobre [adição de controles](tutorial--adding-controls.md).

## Atualizando a interface do usuário


Precisamos manter o jogador informado a respeito do estado do sistema e permitir que ele mude o estado de alto nível de acordo com as regras do jogo. Para a maioria dos jogos (inclusive esta amostra), isso é feito com uma exibição informativa que contém representações do estado de jogo, além de outras informações específicas ao jogo, como pontuação, munição ou o número de chances restantes. Chamamos isso de sobreposição, pois ela é renderizada separadamente do pipeline gráfico principal e posicionada sobre a projeção 3D. No jogo de exemplo, criamos essa sobreposição usando APIs Direct2D. Também podemos criar essa sobreposição usando XAML. Isso será abordado no tópico sobre [extensão do jogo de amostra](tutorial-resources.md).

Há dois componentes na interface do usuário:

-   A exibição informativa, que contém a pontuação e informações sobre o estado atual do jogo.
-   O bitmap de pausa, que é um retângulo preto com texto sobreposto durante o estado pausado/suspenso do jogo. Esta é a sobreposição do jogo. Ela será abordada com mais detalhes em [Adicionando uma interface do usuário](tutorial--adding-a-user-interface.md)

Como era de se esperar, a sobreposição também tem uma máquina de estado. A sobreposição pode exibir uma mensagem de início de nível ou de fim de jogo. Basicamente, é uma tela que gera as informações sobre o estado do jogo que exibimos ao jogador quando o jogo é pausado ou suspenso.

Consulte como o jogo de amostra estrutura a máquina de estado da sobreposição.

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

Há seis telas de estado exibidas pela sobreposição dependendo do estado do jogo em si: uma tela de carregamento de recursos no início do jogo, uma tela de jogo, uma com mensagem de início de nível, uma de fim de jogo quando todos os níveis são concluídos antes do tempo esgotar, uma de fim de jogo quando o tempo é esgotado, e uma com uma menu de pausa.

Separar a interface do usuário do pipeline gráfico do jogo permite trabalhar nele independentemente da engine de renderização gráfica do jogo, reduzindo consideravelmente a complexidade do código do jogo.

## Próximas etapas


Este tópico aborda a estrutura básica do jogo de exemplo e apresenta um modelo bem adequado ao desenvolvimento de aplicativos de jogos UWP com DirectX. Claro, há mais que isso. Só passamos pelo esqueleto do jogo. Agora, nos aprofundaremos no jogo e sua mecânica e veremos como ela é implementada como objeto principal do jogo. Revisaremos essa parte no tópico sobre [definição do objeto principal do jogo](tutorial--defining-the-main-game-loop.md).

Também é hora de observar a engine gráfica do jogo de amostra com mais detalhes. Essa parte é abordada em [Montando o pipeline de renderização](tutorial--assembling-the-rendering-pipeline.md).

## Código de exemplo completo desta seção


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
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
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
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
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 







<!--HONumber=Jun16_HO4-->



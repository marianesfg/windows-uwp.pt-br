---
author: joannaleecy
title: Gerenciamento de fluxo de jogo
description: Saiba como inicializar estados de jogo, manipular eventos e configurar loops de atualização de jogo.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 610b794c0ded6791e93c14d8960366132afd973b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7577272"
---
# <a name="game-flow-management"></a>Gerenciamento de fluxo de jogo

Agora, o jogo tem uma janela, registrou alguns manipuladores de eventos e carrega ativos de forma assíncrona. Esta seção descreve o uso de estados de jogo, como gerenciar estados de jogos principais e como criar um loop de atualização para o mecanismo do jogo. Em seguida, conheceremos o fluxo da interface do usuário e, por fim, compreenderemos melhor os eventos e os manipuladores de eventos necessários em um jogo UWP.

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="game-states-used-to-manage-game-flow"></a>Estados de jogo usados para gerenciar o fluxo do jogo

Usamos os estados do jogo para gerenciar o fluxo do jogo. Seu jogo pode ter qualquer quantidade de estados possíveis, pois um usuário pode retomar um app de jogo UWP a partir de um estado suspenso a qualquer momento.

Neste exemplo de jogo, ele pode estar em um dos três estados quando iniciar:
* O loop do jogo está em execução e está no meio de um nível.
* O loop de jogo não está em execução porque um jogo tinha acabado de ser finalizado. (A pontuação máxima foi definida)
* Nenhum jogo foi iniciado ou o jogo está entre níveis. (A pontuação máxima é 0)

Você pode definir o número de estados necessários, dependendo das necessidades do jogo. Mais uma vez, saiba que o jogo UWP pode ser finalizado a qualquer momento e, ao retomá-lo, o jogador espera que o jogo se comporte como se não tivesse sido interrompido.

## <a name="game-states-initialization"></a>Inicialização de estados do jogo

Ao inicializar o jogo, você não apenas está focado na inicialização a frio, mas também na reinicialização após ele ter sido pausado ou encerrado. O jogo de exemplo sempre salva o estado do jogo deixando a impressão que ele permaneceu em execução. 

No estado suspenso, a partida é suspensa, mas os recursos do jogo permanecem na memória. 

Da mesma forma, o evento de retomada serve para garantir que o jogo de exemplo recomeçará do ponto onde foi suspenso ou encerrado pela última vez. Quando o jogo de exemplo reinicia após o término, ele inicia normalmente e depois determina qual o último estado conhecido, assim o jogador pode continuar a jogar.

Dependendo do estado, diferentes opções são apresentadas para o jogador. 

* Se o jogo for retomado a partir no meio do nível, ele aparecerá como pausado e a sobreposição apresentará uma opção de continuidade.
* Se o jogo for retomado em um estado no qual o jogo foi concluído, ele exibirá as pontuações máximas e uma opção para executar um novo jogo.
* Por último, se o jogo for retomado antes que um nível seja iniciado, a sobreposição apresentará uma opção de início para o usuário.

O exemplo de jogo não faz distinção entre inicialização a frio, inicialização pela primeira vez sem um evento suspenso ou retomada de um estado suspenso. Este é o design adequado para qualquer aplicativo UWP.

Neste exemplo, a inicialização dos estados de jogo ocorre em [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).

Este fluxograma ajudará você a visualizar o fluxo; ele abordará a inicialização e o loop de atualização.

* A inicialização começa no nó __Iniciar__ quando você verifica o estado atual do jogo. No código do jogo, acesse [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).
* Para saber mais sobre o loop de atualização, acesse [Atualizar mecanismo do jogo](#update-game-engine). No código do jogo, acesse [__App:: Update__](#appupdate-method).

![a máquina de estado principal do nosso jogo](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>Método GameMain::InitializeGameState

O método __InitializeGameState__ é chamado na classe de construtor [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131), que, por sua vez, é chamada quando o objeto de classe __GameMain__ é criado no método [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123).

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>Atualizar o mecanismo do jogo

No método [__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130), ele chama [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Neste método, o exemplo implementou uma máquina de estado básico para manipular todas as ações principais que o jogador pode realizar. O nível mais alto dessa máquina de estado lida com ações como carregar um jogo, jogar um nível específico ou continuar um nível depois que um jogo foi pausado (pelo sistema ou pelo jogador).

No exemplo de jogo, há três estados principais (__UpdateEngineState__) em que o jogo pode estar:

1. __Waiting for resources__: O loop do jogo está em exibição cíclica, não será possível transitar enquanto os recursos (especificamente os recursos gráficos) não estiverem disponíveis. Quando as tarefas assíncronas para o carregamento de recursos são concluídas, o estado é atualizado para __ResourcesLoaded__. Isso geralmente acontece entre os níveis, quando o nível está carregando novos recursos do disco, do servidor do jogo ou do back-end da nuvem. No exemplo de jogo, simulamos esse comportamento porque o exemplo não precisa de nenhum recurso adicional por nível nesse momento.
2. __Waiting for press__: O loop do jogo está em exibição cíclica, aguardando uma entrada específica do usuário. Esta entrada é um ação do jogador para carregar um jogo, iniciar um nível ou continuar um nível. O código de exemplo faz referência a esses subestados como valores de enumeração __PressResultState__.
3. In __Dynamics__: O loop do jogo está sendo executado com o usuário jogando. Enquanto o usuário está jogando, o jogo verifica três condições na qual ele pode transitar: 
    * __TimeExpired__: expiração do tempo definido para um nível
    * __LevelComplete__: conclusão de um nível pelo jogador 
    * __GameComplete__: conclusão de todos os níveis pelo jogador

Seu jogo é simplesmente uma máquina de estado que contém várias máquinas de estado menores. Em cada estado específico, ele deve ser definido por um critério muito específico. A transição de um estado para outro devem se basear em ações do sistema ou na entrada discreta do usuário (como carregamento de recursos gráficos). Ao planejar seu jogo, é recomendável desenhar o fluxo completo de jogo para garantir que todas as ações possíveis que o usuário ou o sistema pode executar serão abordadas. Os jogos podem ser bastante complicados; por isso, uma máquina de estado é uma ferramenta poderosa para visualizar essa complexidade e torná-la mais gerenciável.

Vamos dar uma olhada nos códigos do loop de atualização abaixo.

### <a name="appupdate-method"></a>Método App::Update

A estrutura da máquina de estado usada para atualizar o mecanismo do jogo

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
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

## <a name="update-user-interface"></a>Atualizar interface do usuário

Precisamos manter o jogador informado sobre o estado do sistema e permitir que o estado do jogo seja alterado de acordo com as ações do jogador e as regras que definem o jogo. Muitos jogos, incluindo este exemplo de jogo, comumente usam elementos de interface do usuário para apresentar essas informações ao jogador. A interface do usuário contém representações do estado do jogo e outras informações específicas do jogo, como pontuação, munição ou o número de chances restantes. A interface do usuário também é denominada sobreposição, pois ela é renderizada separadamente do pipeline gráfico principal e posicionada sobre a projeção 3D. Algumas informações da interface do usuário também são apresentadas como um painel transparente (HUD) para permitir que os usuários obtenham essas informações sem tirar os olhos da área de jogo principal. No jogo de exemplo, criamos essa sobreposição usando APIs Direct2D. Também podemos criar essa sobreposição usando XAML. Isso será abordado no tópico sobre [extensão do jogo de amostra](tutorial-resources.md).

Há dois componentes na interface do usuário:

-   A HUD, que contém a pontuação e as informações sobre o estado atual do jogo.
-   O bitmap de pausa, que é um retângulo preto com texto sobreposto durante o estado pausado/suspenso do jogo. Esta é a sobreposição do jogo. Ela será abordada com mais detalhes em [Adicionando uma interface do usuário](tutorial--adding-a-user-interface.md)

Como era de se esperar, a sobreposição também tem uma máquina de estado. A sobreposição pode exibir uma mensagem de início de nível ou de fim de jogo. Basicamente, é uma tela que gera as informações sobre o estado do jogo que exibimos ao jogador quando o jogo é pausado ou suspenso.

A sobreposição renderizada pode ser uma das seis telas, dependendo do estado do jogo: 
1. Tela de carregamento de recursos no início do jogo
2. Tela de estatísticas do jogo
3. Tela de mensagem de início de nível
4. Tela de término do jogo quando todos os níveis são concluídos sem esgotar o tempo
5. Tele de término do jogo quando o tempo é esgotado
6. Tela do menu de pausa

Separar a interface do usuário do pipeline gráfico do jogo permite trabalhar nele independentemente da engine de renderização gráfica do jogo, reduzindo consideravelmente a complexidade do código do jogo.

Saiba como o exemplo de jogo estrutura a máquina de estado da sobreposição.

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>Manipulação de eventos

Nosso código de exemplo registrou vários manipuladores de eventos específicos em **Initialize**, **SetWindow** e **Load** no App.cpp. Esses são eventos importantes que precisam funcionar para que possamos adicionar a mecânica do jogo ou iniciar o desenvolvimento de elementos gráficos. Esses eventos são fundamentais para uma experiência adequada com aplicativos UWP. Como um aplicativo UWP pode ser ativado, desativado, redimensionado, ajustado, desajustado, suspenso ou retomado a qualquer momento, o jogo deve registrar esses eventos o quanto antes e manipulá-los de modo a manter a experiência fluida e previsível para o jogador.

Estes são os manipuladores de eventos usados neste exemplo e os eventos manipulados por eles.

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
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>. O app de jogo foi trazido para o primeiro plano. Por isso, a janela principal foi ativada.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Manipula <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>. O DPI da janela mudou e o jogo ajusta devidamente seus recursos.
<div class="alert">
<strong>Observação</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559) coordenadas são em DIPs (Pixels independentes de dispositivo) para [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Como resultado, você deve notificar o Direct2D sobre a alteração no DPI para exibir quaisquer ativos ou primitivas 2D corretamente.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Manipula <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>. A orientação da tela é alterada e a renderização precisa ser atualizada.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Manipula <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>. A exibição precisa ser redesenhada e o jogo precisa ser renderizado novamente.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>. O aplicativo de jogo restaura o jogo de um estado suspenso.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>. O aplicativo de jogo salva seu estado em disco. Ele tem cinco segundos para salvar o estado no armazenamento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>. O aplicativo de jogo tem visibilidade alterada e se torna visível ou foi tornado invisível por outro aplicativo que se tornou visível.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>. A janela principal do aplicativo de jogo foi desativada ou ativada. Por isso, ela deve remover o foco e pausar o jogo ou obter o foco novamente. Nos dois casos, a sobreposição indica que o jogo está em pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>. O aplicativo de jogo fecha a janela principal e suspende o jogo.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Manipula <a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>. O app de jogo realoca os recursos gráficos e a sobreposição para acomodar a mudança de tamanho e, em seguida, atualiza o destino de renderização.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Próximas etapas

Neste tópico, falamos sobre como o fluxo geral do jogo é gerenciado por meio dos estados do jogo e que um jogo é composto de várias máquinas de estado diferentes. Aprendemos também como atualizar a interface do usuário e gerenciar os manipuladores de eventos principais do app. Agora estamos prontos para nos aprofundar no loop de renderização, no jogo e em sua mecânica.
 
Você pode passar pelos outros componentes que compõem este jogo em qualquer ordem:
* [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md)
* [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md)
* [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md)
* [Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md)
* [Adicionar controles](tutorial--adding-controls.md)
* [Adicionar som](tutorial--adding-sound.md)
---
title: Gerenciamento de fluxo de jogo
description: Saiba como inicializar estados de jogo, manipular eventos e configurar loops de atualização de jogo.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jogos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409625"
---
# <a name="game-flow-management"></a>Gerenciamento de fluxo de jogo

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

O jogo agora tem uma janela, registrou alguns manipuladores de eventos e carregou ativos de forma assíncrona. Este tópico explica o uso de Estados de jogo, como gerenciar Estados de jogos-chave específicos e como criar um loop de atualização para o mecanismo de jogo. Em seguida, aprenderemos sobre o fluxo da interface do usuário e, por fim, entenderei mais sobre os manipuladores de eventos que são necessários para um jogo UWP.

## <a name="game-states-used-to-manage-game-flow"></a>Estados de jogo usados para gerenciar o fluxo do jogo

Usamos os estados do jogo para gerenciar o fluxo do jogo.

Quando o jogo de exemplo **Simple3DGameDX** é executado pela primeira vez em um computador, ele está em um estado em que nenhum jogo foi iniciado. Próximas vezes que o jogo é executado, ele pode estar em qualquer um desses Estados.

- Nenhum jogo foi iniciado ou o jogo está entre os níveis (a pontuação alta é zero).
- O loop de jogo está em execução e está no meio de um nível.
- O loop de jogo não está em execução devido a um jogo ter sido concluído (a pontuação alta tem um valor diferente de zero).

Seu jogo pode ter tantos Estados quantos forem necessários. Mas lembre-se de que ele pode ser encerrado a qualquer momento. E quando ele é retomado, o usuário espera que ele retome no estado em que estava quando foi encerrado.

## <a name="game-state-management"></a>Gerenciamento de estado do jogo

Portanto, durante a inicialização do jogo, você precisará dar suporte à inicialização a frio e retomar o jogo depois de interrompê-lo em trânsito. O exemplo **Simple3DGameDX** sempre salva seu estado de jogo para dar a impressão de que ele nunca parou.

Em resposta a um evento de suspensão, o jogo é suspenso, mas os recursos dele ainda estão na memória. Da mesma forma, o evento resume é tratado para garantir que o jogo de exemplo seja escolhido no estado em que estava quando foi suspenso ou encerrado. Dependendo do estado, diferentes opções são apresentadas para o jogador. 

- Se o jogo retomar o nível intermediário, ele aparecerá em pausa e a sobreposição oferecerá a opção de continuar.
- Se o jogo for retomado em um estado em que o jogo está concluído, ele exibirá as pontuações altas e uma opção para jogar um novo jogo.
- Por fim, se o jogo for retomado antes de um nível ser iniciado, a sobreposição apresentará uma opção start para o usuário.

O jogo de exemplo não distingue se o jogo está começando por frio, iniciando pela primeira vez sem um evento de suspensão ou retomando-se de um estado suspenso. Este é o design adequado para qualquer aplicativo UWP.

Neste exemplo, a inicialização dos Estados do jogo ocorre em [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) (um contorno desse método é mostrado na próxima seção).

Aqui está um fluxograma para ajudá-lo a visualizar o fluxo. Ele abrange a inicialização e o loop de atualização.

- A inicialização começa no nó **Iniciar** quando você verifica o estado atual do jogo. Para o código de jogo, consulte [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) na próxima seção.
* Para saber mais sobre o loop de atualização, acesse [Atualizar mecanismo do jogo](#update-game-engine). Para o código de jogo, acesse [**GameMain:: Update**](#the-gamemainupdate-method).

![a máquina de estado principal do nosso jogo](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>O método GameMain:: InitializeGameState

O método **GameMain:: InitializeGameState** é chamado indiretamente por meio do construtor da classe **GameMain** , que é o resultado da criação de uma instância **GameMain** dentro do **aplicativo:: Load**.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>Atualizar o mecanismo do jogo

O método **App:: Run** chama **GameMain:: Run**. Em **GameMain:: Run** é um computador de estado básico para lidar com todas as principais ações que um usuário pode executar. O nível mais alto dessa máquina de estado lida com o carregamento de um jogo, a reprodução de um nível específico ou a continuação de um nível após o jogo ter sido pausado (pelo sistema ou pelo usuário).

No jogo de exemplo, há três Estados principais (representados pela enumeração **UpdateEngineState** ) em que o jogo pode estar.

1. **UpdateEngineState:: WaitingForResources**. O loop do jogo está em exibição cíclica, não será possível transitar até os recursos (especificamente os recursos gráficos) estarem disponíveis. Quando as tarefas de carregamento de recursos assíncronos forem concluídas, atualizaremos o estado para **UpdateEngineState:: ResourcesLoaded**. Isso geralmente ocorre entre os níveis quando o nível está carregando novos recursos do disco, de um servidor de jogos ou de um back-end de nuvem. No jogo de exemplo, simulamos esse comportamento, porque o exemplo não precisa de nenhum recurso adicional por nível no momento.
2. **UpdateEngineState:: WaitingForPress**. O loop do jogo está em exibição cíclica, aguardando uma entrada específica do usuário. Essa entrada é uma ação do jogador para carregar um jogo, para iniciar um nível ou para continuar um nível. O código de exemplo refere-se a esses subcaminhos por meio da enumeração **PressResultState** .
3. **UpdateEngineState::D ynamics**. O loop do jogo está sendo executado com o usuário jogando. Enquanto o usuário está jogando, o jogo verifica três condições na qual ele pode transitar: 
 - **GameState:: expira**. Expiração do limite de tempo para um nível.
 - **GameState:: LevelComplete**. Conclusão de um nível pelo Player.
 - **GameState:: GameComplete**. Conclusão de todos os níveis pelo Player.

Um jogo é simplesmente uma máquina de estado que contém várias máquinas de estado menores. Cada estado específico deve ser definido por critérios muito específicos. As transições de um estado para outro devem ser baseadas na entrada discreta do usuário ou em ações do sistema (como o carregamento de recursos gráficos).

Ao planejar seu jogo, considere desenhar todo o fluxo de jogos para garantir que você tenha abordado todas as ações possíveis que o usuário ou o sistema pode tomar. Um jogo pode ser muito complicado, portanto, uma máquina de estado é uma ferramenta poderosa para ajudá-lo a Visualizar essa complexidade e torná-la mais gerenciável.

Vamos dar uma olhada no código para o loop de atualização.

### <a name="the-gamemainupdate-method"></a>O método GameMain:: Update

Esta é a estrutura do computador de estado usado para atualizar o mecanismo de jogo.

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>Atualizar a interface do usuário

Precisamos manter o jogador informado sobre o estado do sistema e permitir que o estado do jogo seja alterado de acordo com as ações do jogador e as regras que definem o jogo. Muitos jogos, incluindo este jogo de exemplo, normalmente usam elementos da interface do usuário para apresentar essas informações ao Player. A interface do usuário contém representações do estado do jogo e outras informações específicas de reprodução, como score, ammo ou o número de chances restantes. A interface do usuário também é chamada de sobreposição porque é renderizada separadamente do pipeline de gráficos principal e colocada sobre a projeção 3D.

Algumas informações da interface do usuário também são apresentadas como uma HUD (exibição de cabeçotes) para permitir que o usuário Veja essas informações sem levar seus olhos completamente da área de jogos principal. No jogo de exemplo, criamos essa sobreposição usando as APIs Direct2D. Como alternativa, poderíamos criar essa sobreposição usando XAML, que abordamos ao [estender o jogo de exemplo](tutorial-resources.md).

Há dois componentes para a interface do usuário.

- O HUD que contém a pontuação e as informações sobre o estado atual de jogos.
- O bitmap de pausa, que é um retângulo preto com texto sobreposto durante o estado pausado/suspenso do jogo. Esta é a sobreposição do jogo. Ela será abordada com mais detalhes em [Adicionando uma interface do usuário](tutorial--adding-a-user-interface.md)

Como era de se esperar, a sobreposição também tem uma máquina de estado. A sobreposição pode exibir um início de nível ou uma mensagem de jogo. É essencialmente uma tela na qual podemos gerar informações sobre o estado do jogo que desejamos exibir para o jogador enquanto o jogo é pausado ou suspenso.

A sobreposição renderizada pode ser uma dessas seis telas, dependendo do estado do jogo.

1. Tela de progresso do carregamento de recursos no início do jogo.
2. Tela de estatísticas cheia.
3. Tela de início de mensagem de nível.
4. Tela de jogo quando todos os níveis forem concluídos sem o tempo esgotado.
5. Tela de jogo quando o tempo esgotar.
6. Pausar tela do menu.

Separar a interface do usuário do pipeline gráfico do jogo permite que você trabalhe com ele independentemente do mecanismo de renderização de gráficos do jogo e reduz significativamente a complexidade do código do jogo.

Veja como o jogo de exemplo estrutura o computador de estado da sobreposição.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>Manipulação de eventos

Como vimos no tópico [definir o aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md) , muitos dos métodos do provedor de exibição da classe de **aplicativo** registram manipuladores de eventos. Esses métodos precisam lidar corretamente com esses eventos importantes antes de adicionar a mecânica de jogos ou iniciar o desenvolvimento de gráficos.

A manipulação adequada dos eventos em questão é fundamental para a experiência do aplicativo UWP. Como um aplicativo UWP pode ser ativado, desativado, redimensionado, encaixado, desajustado, suspenso ou retomado, o jogo deve se registrar nesses eventos assim que possível e tratá-los de uma forma que mantenha a experiência tranqüila e previsível para o jogador.

Esses são os manipuladores de eventos usados neste exemplo e os eventos que eles manipulam.

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
<td align="left">Manipula <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView::Activated</strong></a>. O app de jogo foi trazido para o primeiro plano. Por isso, a janela principal foi ativada.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Manipula <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>. O DPI da janela mudou e o jogo ajusta devidamente seus recursos.
<div class="alert">
<strong>Observe</strong>que as coordenadas <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a> estão em pixels independentes de dispositivo (DIPs) para <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>. Como resultado, você deve notificar o Direct2D sobre a alteração no DPI para exibir quaisquer ativos ou primitivas 2D corretamente.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Manipula <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>. A orientação da tela é alterada e a renderização precisa ser atualizada.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Manipula <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>. A exibição precisa ser redesenhada e o jogo precisa ser renderizado novamente.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Manipula <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication::Resuming</strong></a>. O aplicativo de jogo restaura o jogo de um estado suspenso.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Manipula <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication::Suspending</strong></a>. O aplicativo de jogo salva seu estado em disco. Ele tem cinco segundos para salvar o estado no armazenamento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Manipula <a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow::VisibilityChanged</strong></a>. O aplicativo de jogo tem visibilidade alterada e se torna visível ou foi tornado invisível por outro aplicativo que se tornou visível.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Manipula <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow::Activated</strong></a>. A janela principal do aplicativo de jogo foi desativada ou ativada. Por isso, ela deve remover o foco e pausar o jogo ou obter o foco novamente. Nos dois casos, a sobreposição indica que o jogo está em pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Manipula <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow::Closed</strong></a>. O aplicativo de jogo fecha a janela principal e suspende o jogo.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Manipula <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow::SizeChanged</strong></a>. O app de jogo realoca os recursos gráficos e a sobreposição para acomodar a mudança de tamanho e, em seguida, atualiza o destino de renderização.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Próximas etapas

Neste tópico, vimos como o fluxo de jogo geral é gerenciado usando Estados de jogo e que um jogo é composto por várias máquinas de estado diferentes. Também vimos como atualizar a interface do usuário e gerenciar os principais manipuladores de eventos do aplicativo. Agora estamos prontos para mergulhar no loop de renderização, no jogo e em sua mecânica.
 
Você pode percorrer os tópicos restantes que documentam este jogo em qualquer ordem.

- [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md)
- [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md)
- [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md)
- [Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md)
- [Adicionar controles](tutorial--adding-controls.md)
- [Adicionar som](tutorial--adding-sound.md)

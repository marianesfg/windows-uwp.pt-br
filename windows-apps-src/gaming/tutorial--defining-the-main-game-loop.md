---
title: Definir o objeto principal do jogo
description: Agora, observaremos os detalhes do objeto principal do exemplo de jogo e como as regras implementadas são convertidas em interações com o ambiente do jogo.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: a3c47f3c22c41e7ca73c8a8b5d4e26dc27fab343
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367656"
---
# <a name="define-the-main-game-object"></a>Definir o objeto principal do jogo

Depois de dispostos a estrutura básica do jogo de exemplo e implementado de uma máquina de estado que lida com os usuários de alto nível e os comportamentos de sistema, você desejará examinar as regras e os mecanismos que transformam o exemplo do jogo em um jogo. Vamos examinar os detalhes do objeto principal de jogo do exemplo e como converter de regras do jogo em interações com o mundo do jogo.

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Saiba como aplicar as técnicas de desenvolvimento básico para implementar regras do jogo e a mecânica de um jogo UWP DirectX.

## <a name="main-game-object"></a>Objeto principal do jogo

Nesse jogo de exemplo, __Simple3DGame__ é a classe de objeto do jogo principal. Uma instância do __Simple3DGame__ objeto é construído na __App::Load__ método.

O __Simple3DGame__ objeto da classe:
* Especifica a implementação da lógica do jogo
* Contém métodos que se comunicam:
    * Alterações no estado do jogo para a máquina de estado definido na estrutura de aplicativo.
    * Alterações no estado do aplicativo de jogo para o objeto do jogo em si.
    * Detalhes para a atualização da interface do usuário (sobreposição e HUD) do jogo, animações e física (o dynamics).

    >[!Note]
    >Atualização de gráficos é tratada pelos __GameRenderer__ classe, que contém métodos para obter e usar recursos de dispositivo de gráficos usados pelo jogo. Para obter mais informações, consulte [framework de renderização i: Introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).

* Serve como um contêiner para os dados que define uma sessão de jogo, nível, ou o tempo de vida, dependendo de como você define seu jogo em um alto nível. Nesse caso, os dados de estado do jogo são o tempo de vida do jogo e são inicializados uma vez quando um usuário iniciar o jogo.

Para exibir os métodos e dados definidos neste objeto de classe, vá para [Simple3DGame objeto](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar o jogo

Quando um jogado inicia o jogo, o objeto de jogo deve iniciar seu estado, criar e adicionar a sobreposição, definir as variáveis que acompanham o desempenho do jogador e instanciar os objetos usados para criar os níveis. Neste exemplo, isso é feito quando o novo __GameMain__ instância é criada em [ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

O objeto do jogo __Simple3DGame__, é criado na __GameMain__ construtor. É então inicializada usando o [ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) método durante o [async criar tarefa o __GameMain__ construtor](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Método Simple3DGame::Initialize

O exemplo do jogo configura os seguintes componentes no objeto do jogo:

* Um novo objeto de reprodução de áudio foi criado.
* As matrizes para as primitivas gráficas do jogo são criadas, incluindo matrizes para as primitivas de nível, munição e obstáculos.
* Um local para salvar os dados de estado do jogo é criado, chamado *Jogo* e colocado no local de armazenamento das configurações de dados do aplicativo especificado pelo [**ApplicationData::Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current).
* Um temporizador de jogo e o bitmap de sobreposição inicial no jogo são criados.
* Uma nova câmera é criada com um conjunto específico de parâmetros de exibição e projeção.
* O dispositivo de entrada (o controlador) é definido para a mesma arfagem e guinada da câmera, para que o jogador tenha uma correspondência de 1 para 1 entre a posição inicial do controle e a posição da câmera.
* O objeto do jogador é criado e definido como ativo. Podemos usar um objeto de esfera para detectar a proximidade do player paredes e obstáculos e impedir que a câmera obtendo colocado em uma posição que pode interromper a imersão.
* A primitiva do ambiente do jogo é criada.
* Os obstáculos cilíndricos são criados.
* Os destinos (objetos **Face**) são criados e numerados.
* As esferas de munição são criadas.
* Os níveis são criados.
* A pontuação máxima é carregada.
* Qualquer estado de jogo salvo anteriormente é carregado.

Agora, o jogo tem instâncias de todos os componentes principais: o ambiente, o jogador, os obstáculos, os alvos e as esferas de munição. Ele também tem instâncias dos níveis, que representam configurações de todos os componentes anteriores e seus comportamentos para cada nível de modo específico. Consultemos como o jogo gera os níveis.

## <a name="build-and-load-game-levels"></a>Criar e carregar os níveis de jogos

A maioria do trabalho pesado para a construção de nível é feita na __Level.h/.cpp__ arquivos encontrados na __GameLevels__ pasta da solução de exemplo. Porque ele se concentra em uma implementação muito específica, não falaremos sobre eles aqui. A parte importante é que o código de cada nível é executado como um objeto __LevelN__ separado. Se você quiser estender o jogo, você pode criar uma **nível** objeto que recebe um número atribuído como um parâmetro e aleatoriamente coloca os obstáculos e destinos. Ou, você pode fazer com os dados de configuração de nível de carga de um arquivo de recurso, ou até mesmo a Internet.

## <a name="define-the-game-play"></a>Definir o jogo

A esta altura, temos todos os componentes necessários para montar o jogo. Os níveis foram construídos na memória a partir os primitivos e estão prontos para começar a interagir com o player.

TNão melhores jogos instantaneamente reagem à entrada do player e fornecem feedback imediato. Isso é verdadeiro para qualquer tipo de um jogo de ação de twitch, atiradores em primeira pessoa em tempo real aos jogos de estratégia profunda, baseada em turno.

### <a name="simple3dgamerungame-method"></a>Método Simple3DGame::RunGame

Durante a reprodução de um nível, o jogo está no __Dynamics__ estado. 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) é o loop principal de atualização que atualiza o estado do aplicativo uma vez por quadro, conforme mostrado abaixo. No loop de atualização, ele chama o [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) método para tratar do trabalho, se o jogo estiver no __Dynamics__ estado.

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) manipula o conjunto de dados que define o estado atual do jogo para a iteração atual do que o loop do jogo.

Lógica de fluxo em de jogos __RunGame__:
*  O método atualiza o temporizador que faz a contagem regressiva dos segundos até que o nível seja concluído e os testes para ver se o tempo do nível expirou. Essa é uma das regras do jogo: quando o tempo de execução se esgotar, e não tem sido captura todos os destinos, ele é jogo.
*  Quando o tempo acaba, o método define o estado de jogo **TimeExpired** e retorna ao método ao método **Update** no código anterior.
*  Quando ainda há tempo, o controlador de movimento/visão é consultado para atualizar a posição da câmera. O que ocorre, especificamente, é uma atualização do ângulo do vetor normal de visão que se projeta do plano da câmera (quando o jogador está olhando) e a distância percorrida por esse ângulo desde o momento da consulta anterior ao controlador.
*  A câmera é atualizada com base nos novos dados do controlador de movimento/visão.
*  A dinâmica (ou seja, as animações e os comportamentos dos objetos no ambiente de jogo que não dependem do controle do jogador) é atualizada. Neste exemplo de jogo, o [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) método é chamado para atualizar o movimento das esferas ammo que foi acionado, a animação dos obstáculos pilar e a movimentação dos destinos. Para obter mais informações, consulte [atualizar o mundo do jogo](#update-the-game-world).
*  O método confere se os critérios de conclusão de um nível foram atendidos. Em caso positivo, ele finaliza a pontuação do nível e verifica se é o último nível (de seis). Se for, o método retornará o estado de jogo **GameComplete**; caso contrário, gerará o estado __LevelComplete__.
*  +Quando o nível não é concluído, o método define o estado de jogo como __Active__ e retorna.

## <a name="update-the-game-world"></a>Atualizar o mundo do jogo

Neste exemplo, quando o jogo está em execução, o [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) método é chamado do [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)método (que é chamado de [ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) para atualizar os objetos que são renderizados em uma cena de jogo.

No __UpdateDynamics__ loop, chamada de métodos que são usados para definir o mundo do jogo em movimento, independentemente da entrada do jogador, criar uma experiência de jogo imersiva e verifique o nível vêm *alive*. Isso inclui elementos gráficos que precisa ser renderizado e executa um loop de animação em execução para trazer sobre a vida respirar world, mesmo quando não há nenhuma entrada do jogador. Por exemplo, árvores swaying em vento, ondas cresting ao longo de linhas de mar, fumante maquinário e monstros alienígena, redimensionar e mover-se. Ela também compreende a interação entre objetos, inclusive colisões entre a esfera do jogador e o ambiente ou entre a munição e os obstáculos/alvos.

O loop do jogo deve sempre manter a atualizar o mundo do jogo se ela se baseia na lógica do jogo, algoritmos de físicas, ou se ele é simplesmente aleatório, exceto quando especificamente está em pausa o jogo. 

No exemplo de jogo, esse princípio é chamado de *dinâmica* e abrange a ascensão e queda dos obstáculos em forma de pilar, o movimento e os comportamentos físicos de esferas de munição quando são lançadas. 

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics 

Este método lida com quatro onjuntos de cálculos:

* As posições das esferas de munição disparadas no mundo.
* A animação dos obstáculos de pilar.
* A intersecção do jogador e as fronteiras do mundo.
* As colisões das esferas de munição com os obstáculos, os alvos, outras esferas de munição e o ambiente.

A animação dos obstáculos é um loop definido em **Animate.h/.cpp**. O comportamento do ammo e colisões são definidos pelos algoritmos de física simplificada, fornecido no código e parametrizado por um conjunto de constantes globais para o mundo de jogo, incluindo propriedades material e gravidade. Isso tudo é calculado no espaço da coordenada do ambiente do jogo.

### <a name="review-the-flow"></a>Analisar o fluxo

Agora que podemos ter atualizado todos os objetos na cena e calculado colisões, precisamos usar essas informações para desenhar as alterações correspondentes de visual. 

Após __GameMain::Update()__ conclui a iteração atual do que o loop do jogo, o exemplo chama imediatamente __Render ()__ pegar os dados do objeto atualizado e gerar uma nova cena para apresentar ao player, como mostrado aqui. Em seguida, vamos dar uma olhada a renderização.

```cpp
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
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
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

## <a name="render-the-game-worlds-graphics"></a>Renderizar elementos gráficos de jogos do mundo

Recomendamos que os elementos gráficos de um jogo sejam atualizados sempre que possível. Isso corresponde a, no máximo, toda vez que o loop do jogo principal realiza uma iteração. À medida que o loop realiza uma iteração, o jogo é atualizado, com ou sem entradas do jogador. Isso permite que as animações e os comportamentos sejam calculados para serem exibidos de forma fluida. Imagine se tivéssemos uma cena simples de água que só se movesse quando o jogador pressionasse um botão. Isso geraria um visual terrivelmente tedioso. Um bom jogo é estável e fluido.

Lembre-se o loop do jogo de exemplo conforme mostrado na [ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Quando a janela principal do jogo está visível e não está ajustada nem desativada, o jogo continua a atualizar e renderizar os resultados da atualização. O [ __renderizar__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) método podemos estiver examinando agora renderiza uma representação de estado. Isso é feito imediatamente após uma chamada para **atualize**, que inclui **RunGame** para estados de atualização, que foi discutido na seção anterior.

Esse método desenha a projeção do mundo 3D e gera a sobreposição em Direct2D sobre ela. Quando termina, ele apresenta a cadeia de permuta final com os buffers combinados para exibição.

>[!Note]
>Há dois estados de sobreposição do Direct2D do jogo de exemplo: uma em que o jogo exibirá a sobreposição de informações do jogo que contém o bitmap para o menu de pausa e um em que o jogo exibe a mira juntamente com os retângulos para obter a aparência de movimentação de tela sensível ao toque controlador. O texto da pontuação é gerado nos dois estados. Para obter mais informações, consulte [framework de renderização i: Introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).

### <a name="gamerendererrender-method"></a>Método GameRenderer::Render

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Objeto Simple3DGame

Esses são os métodos e os dados que são definidos na __Simple3DGame__ classe de objeto.

### <a name="methods"></a>Métodos

Os métodos internos definidos **Simple3DGame** incluem:

-   **Inicializar**: Define os valores iniciais das variáveis globais e inicializa os objetos do jogo. Isso é abordado na [inicializar e iniciar o jogo](#initialize-and-start-the-game) seção.
-   **LoadGame**: Inicia um novo nível e começa a carregá-lo.
-   **LoadLevelAsync**: Inicia uma tarefa assíncrona (se você estiver familiarizado com tarefas assíncronas, consulte [Parallel Patterns Library](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) para inicializar o nível e, em seguida, chamar uma tarefa assíncrona no renderizador para carregar os recursos de nível específico de dispositivo. Esse método é executado em um thread separado; assim, apenas métodos [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (ao contrário de métodos [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) podem ser chamados nesse thread. Os métodos de contexto de dispositivo são chamados no método **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: Conclua todo o trabalho para o carregamento de nível que precisa ser feito no thread principal. Isso inclui todas as chamadas para os métodos de contexto de dispositivo do Direct3D 11 ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)).
-   **StartLevel**: Inicia a partida de um novo nível.
-   **PauseGame**: Pausa o jogo.
-   **RunGame**: Executa uma iteração do loop do jogo. Ele é chamado em **App::Update** uma vez a cada iteração do loop do jogo caso o estado do jogo seja **Active**.
-   **OnSuspending** e **OnResuming**: Suspende e retoma o áudio do jogo, respectivamente.

E os métodos privados:

-   **LoadSavedState** e **SaveState**: Carrega e salva o estado atual do jogo, respectivamente.
-   **SaveHighScore** e **LoadHighScore**: Salva e carrega as pontuações máximas entre jogos, respectivamente.
-   **InitializeAmmo**: Redefine o estado de cada objeto de esfera usado como munição de volta ao seu estado original para o início de cada rodada.
-   **UpdateDynamics**: Este é um método importante, pois atualiza todos os objetos do jogo com base em rotinas de animação, física e entrada de controles repetidas. Esse é o núcleo da interatividade que define o jogo. Isso é abordado na [atualizar o mundo do jogo](#update-the-game-world) seção.

Os outros métodos públicos são getters de propriedades que enviam informações específicas sobre o jogo e sobreposições à estrutura do aplicativo para exibição.

### <a name="data"></a>Dados

Na parte superior do exemplo do código, há quatro objetos cujas instâncias são atualizadas à medida que o loop do jogo é executado.

-   **MoveLookController** objeto: Representa a entrada do jogador. Para obter mais informações, consulte [Adicionando controles](tutorial--adding-controls.md).
-   **GameRenderer** objeto: Representa o renderizador do Direct3D 11 derivado de **DirectXBase** classe que lida com todos os objetos específicos do dispositivo e sua renderização. Para obter mais informações, consulte [framework de renderização eu](tutorial--assembling-the-rendering-pipeline.md).
-   **Áudio** objeto: Controla a reprodução de áudio para o jogo. Para obter mais informações, consulte [adicionando som](tutorial--adding-sound.md).

As outras variáveis do jogo contêm as listas de primitivas e seus respectivos valores no jogo, além de dados e restrições específicos à jogabilidade.

## <a name="next-steps"></a>Próximas etapas

Agora, você provavelmente deve está curioso sobre o mecanismo de renderização real: como chamadas para o __renderizar__ métodos em primitivos de atualizado obterem tornou pixels na tela. Isso é abordado em duas partes &mdash; [framework de renderização i: Introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) e [II do framework de renderização: Renderização de jogos](tutorial-game-rendering.md). Se estiver mais interessado em como os controles do jogador atualizam o estado do jogo, confira o tópico sobre [adição de controles](tutorial--adding-controls.md).

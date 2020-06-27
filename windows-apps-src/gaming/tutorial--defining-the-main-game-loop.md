---
title: Definir o objeto principal do jogo
description: Agora, veremos os detalhes do objeto principal do jogo de exemplo e como as regras que ele implementa são traduzidas em interações com o mundo do jogo.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jogos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: 9a6d087be6df93ee6798c29147f7fd1c820bd225
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409555"
---
# <a name="define-the-main-game-object"></a>Definir o objeto principal do jogo

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

Depois de definir a estrutura básica do jogo de exemplo e implementar uma máquina de estado que lide com os comportamentos de usuário e sistema de alto nível, você desejará examinar as regras e a mecânica que transformam o jogo de exemplo em um jogo. Vamos examinar os detalhes do objeto principal do jogo de exemplo e como traduzir as regras do jogo em interações com o mundo do jogo.

## <a name="objectives"></a>Objetivos

- Saiba como aplicar técnicas de desenvolvimento básicas para implementar regras de jogos e mecânica para um jogo do UWP DirectX.

## <a name="main-game-object"></a>Objeto principal do jogo

No jogo de exemplo **Simple3DGameDX** , **Simple3DGame** é a principal classe de objeto Game. Uma instância de **Simple3DGame** é construída, indiretamente, por meio do método **App:: Load** .

Aqui estão alguns dos recursos da classe **Simple3DGame** .

- Contém a implementação da lógica cheia.
- Contém métodos que comunicam esses detalhes.
  - Alterações no estado do jogo para o computador de estado definido na estrutura do aplicativo.
  - Alterações no estado do jogo do aplicativo para o próprio objeto Game.
  - Detalhes da atualização da interface do usuário do jogo (exibição de sobreposição e de cabeçotes), animações e física (o Dynamics).
  > [!NOTE]
  > A atualização de elementos gráficos é tratada pela classe **GameRenderer** , que contém métodos para obter e usar recursos de dispositivos gráficos usados pelo jogo. Para obter mais informações, consulte [Framework de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).
- Serve como um contêiner para os dados que definem uma sessão de jogo, um nível ou um tempo de vida, dependendo de como você define seu jogo em um alto nível. Nesse caso, os dados de estado do jogo são para o tempo de vida do jogo e são inicializados uma vez quando um usuário inicia o jogo.

Para exibir os métodos e os dados definidos por essa classe, consulte [a classe Simple3DGame](#the-simple3dgame-class) abaixo.

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar o jogo

Quando um jogado inicia o jogo, o objeto de jogo deve iniciar seu estado, criar e adicionar a sobreposição, definir as variáveis que acompanham o desempenho do jogador e instanciar os objetos usados para criar os níveis. Neste exemplo, isso é feito quando a instância **GameMain** é criada em **App:: Load**.

O objeto Game, do tipo **Simple3DGame**, é criado no construtor **GameMain:: GameMain** . Em seguida, ele é inicializado usando o método **Simple3DGame:: Initialize** durante a corotina **GameMain:: ConstructInBackground** Fire-and-esqueça, que é chamada de **GameMain:: GameMain**.

### <a name="the-simple3dgameinitialize-method"></a>O método Simple3DGame:: Initialize

O jogo de exemplo configura esses componentes no objeto Game.

- Um novo objeto de reprodução de áudio foi criado.
- As matrizes para as primitivas gráficas do jogo são criadas, incluindo matrizes para as primitivas de nível, munição e obstáculos.
- Um local para salvar os dados de estado do jogo é criado, chamado *Jogo* e colocado no local de armazenamento das configurações de dados do aplicativo especificado pelo [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current).
- Um temporizador de jogo e o bitmap de sobreposição inicial no jogo são criados.
- Uma nova câmera é criada com um conjunto específico de parâmetros de exibição e projeção.
- O dispositivo de entrada (o controlador) é definido para a mesma arfagem e guinada da câmera, para que o jogador tenha uma correspondência de 1 para 1 entre a posição inicial do controle e a posição da câmera.
- O objeto do jogador é criado e definido como ativo. Usamos um objeto Sphere para detectar a proximidade do jogador com as paredes e os obstáculos e impedir que a câmera seja colocada em uma posição que possa quebrar imersão.
- A primitiva do ambiente do jogo é criada.
- Os obstáculos cilíndricos são criados.
- Os destinos (objetos **Face**) são criados e numerados.
- As esferas de munição são criadas.
- Os níveis são criados.
- A pontuação máxima é carregada.
- Qualquer estado de jogo salvo anteriormente é carregado.

Agora, o jogo tem instâncias de todos os principais componentes &mdash; do mundo, do jogador, dos obstáculos, dos alvos e do ammo. Ele também tem instâncias dos níveis, que representam configurações de todos os componentes anteriores e seus comportamentos para cada nível de modo específico. Agora, vamos ver como o jogo cria os níveis.

## <a name="build-and-load-game-levels"></a>Criar e carregar os níveis de jogos

A maior parte do trabalho pesado para a construção de nível é feita nos `Level[N].h/.cpp` arquivos encontrados na pasta **GameLevels** da solução de exemplo. Como ele se concentra em uma implementação muito específica, não as abordaremos aqui. O importante é que o código de cada nível seja executado como um objeto de **nível separado [N]** . Se você quiser estender o jogo, poderá criar um objeto de **nível [N]** que usa um número atribuído como um parâmetro e coloca aleatoriamente os obstáculos e os destinos. Ou você pode fazer com que ele carregue dados de configuração de nível de um arquivo de recurso ou até mesmo da Internet.

## <a name="define-the-gameplay"></a>Definir o jogo

Neste ponto, temos todos os componentes de que precisamos para desenvolver o jogo. Os níveis foram construídos na memória a partir dos primitivos e estão prontos para que o jogador comece a interagir com o.

Os melhores jogos reagem instantaneamente para a entrada do jogador e fornecem comentários imediatos. Isso é verdadeiro para qualquer tipo de jogo, desde twitchs de primeira e-ação, em tempo real, de primeiros-pessoas a jogos de estratégia baseados em virados.

### <a name="the-simple3dgamerungame-method"></a>O método Simple3DGame:: RunGame

Enquanto um nível de jogo está em andamento, o jogo está no estado do **Dynamics** . 

**GameMain:: Update** é o principal loop de atualização que atualiza o estado do aplicativo uma vez por quadro, conforme mostrado abaixo. O loop Update chama o método **Simple3DGame:: RunGame** para manipular o trabalho se o jogo estiver no estado **Dynamics** .

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
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
                ...
```
          
**Simple3DGame:: RunGame** manipula o conjunto de dados que define o estado atual da reprodução do jogo para a iteração atual do loop de jogo.

Aqui está a lógica de fluxo de jogos em **Simple3DGame:: RunGame**.

- O método atualiza o temporizador que conta os segundos até que o nível seja concluído e testa para ver se o tempo do nível expirou. Essa é uma das regras do jogo quando o &mdash; tempo se esgotar, se nem todos os destinos tiverem sido capturados, então ele será o jogo.
- Se o tempo tiver expirado, o método definirá o estado do jogo **TimeExpired** e retornará ao método **Update** no código anterior.
- Se o tempo permanecer, o controlador de mudança de movimento será sondado em busca de uma atualização para a posição da câmera; especificamente, uma atualização do ângulo da exibição projeto normal do plano da câmera (onde o jogador está olhando) e a distância que o ângulo moveu desde que o controlador foi sondado por último.
- A câmera é atualizada com base nos novos dados do controlador de movimento/visão.
- A dinâmica (ou seja, as animações e os comportamentos dos objetos no ambiente de jogo que não dependem do controle do jogador) é atualizada. Neste jogo de exemplo, o método **Simple3DGame:: UpdateDynamics** é chamado para atualizar o movimento dos Ammos que foram acionados, a animação dos obstáculos da Pilar e a movimentação dos destinos. Para obter mais informações, consulte [atualizar o mundo do jogo](#update-the-game-world).
- O método verifica se os critérios para a conclusão bem-sucedida de um nível foram atendidos. Nesse caso, ele finaliza a pontuação para o nível e verifica se esse é o último nível (de 6). Se for o último nível, o método retornará o estado do jogo **GameState:: GameComplete** ; caso contrário, ele retornará o estado do jogo **GameState:: LevelComplete** .
- Se o nível não estiver completo, o método definirá o estado do jogo como **GameState:: active**e retornará.

## <a name="update-the-game-world"></a>Atualizar o mundo do jogo

Neste exemplo, quando o jogo está em execução, o método **Simple3DGame:: UpdateDynamics** é chamado do método **Simple3DGame:: RunGame** (que é chamado de **GameMain:: Update**) para atualizar objetos que são renderizados em uma cena de jogo.

Um loop como **UpdateDynamics** chama quaisquer métodos que são usados para definir o mundo do jogo em movimento, independentemente da entrada do Player, para criar uma experiência de jogo imersiva e fazer o nível ficar ativo. Isso inclui elementos gráficos que precisam ser renderizados e execução de loops de animação para trazer um mundo dinâmico mesmo quando não há nenhuma entrada de Player. Em seu jogo, isso poderia incluir árvores que estão sendo sways no vento, ondas crestings ao longo das linhas reforçás, à prova de máquinas e ao alongamento de monstros alienígena e à movimentação. Ela também compreende a interação entre objetos, inclusive colisões entre a esfera do jogador e o ambiente ou entre a munição e os obstáculos/alvos.

Exceto quando o jogo é especificamente pausado, o loop de jogos deve continuar atualizando o mundo do jogo; Se isso se baseia na lógica do jogo, nos algoritmos físicos ou se é apenas aleatório.

No jogo de exemplo, esse princípio é chamado de *Dynamics*, e abrange o aumento e o surgimento dos obstáculos de pilar, e os comportamentos físicos e de movimento do ammo, como são acionados e em movimento.

### <a name="the-simple3dgameupdatedynamics-method"></a>O método Simple3DGame:: UpdateDynamics 

Esse método lida com esses quatro conjuntos de cálculos.

- As posições das esferas de munição disparadas no mundo.
- A animação dos obstáculos de pilar.
- A intersecção do jogador e as fronteiras do mundo.
- As colisões das esferas de munição com os obstáculos, os alvos, outras esferas de munição e o ambiente.

A animação dos obstáculos ocorre em um loop definido nos arquivos de código-fonte de **Animate. h/. cpp** . O comportamento do ammo e de quaisquer colisões é definido por algoritmos de física simplificados, fornecidos no código e parametrizados por um conjunto de constantes globais para o mundo do jogo, incluindo propriedades de gravidade e material. Isso tudo é calculado no espaço da coordenada do ambiente do jogo.

### <a name="review-the-flow"></a>Examinar o fluxo

Agora que atualizamos todos os objetos na cena e calculamos as colisões, precisamos usar essas informações para desenhar as alterações visuais correspondentes. 

Depois que **GameMain:: Update** tiver concluído a iteração atual do loop do jogo, o exemplo chama imediatamente **GameRenderer:: render** para obter os dados do objeto atualizado e gerar uma nova cena para apresentar ao Player, conforme mostrado abaixo.

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>Renderizar os gráficos do mundo do jogo

Recomendamos que os gráficos em um jogo sejam atualizados com frequência, o ideal é exatamente o mesmo que o loop de jogo principal itera. À medida que o loop é iterado, o estado do mundo do jogo é atualizado, com ou sem entrada do Player. Isso permite que as animações e os comportamentos calculados sejam exibidos sem problemas. Imagine se tivéssemos uma simples cena de água que foi movida somente quando o jogador pressionou um botão. Isso não seria realista; um bom jogo parece suave e flui o tempo todo.

Lembre-se do loop do jogo de exemplo, conforme mostrado acima em **GameMain:: Run**. Se a janela principal do jogo estiver visível e não estiver encaixada ou desativada, o jogo continuará atualizando e renderizando os resultados dessa atualização. O método **GameRenderer:: render** que examinamos em seguida renderiza uma representação desse Estado. Isso é feito imediatamente após uma chamada para **GameMain:: Update**, que inclui **Simple3DGame:: RunGame** para atualizar os Estados, conforme discutido na seção anterior.

**GameRenderer:: render** desenha a projeção do mundo 3D e, em seguida, desenha a sobreposição de Direct2D sobre ela. Quando termina, ele apresenta a cadeia de permuta final com os buffers combinados para exibição.

> [!NOTE]
> Há dois Estados para a sobreposição de Direct2D do jogo de exemplo &mdash; um em que o jogo exibe a sobreposição de informações do jogo que contém o bitmap para o menu pausar e um em que o jogo exibe as cruzas com os retângulos do controlador de aparência de movimentação Touch-look. O texto da pontuação é gerado nos dois estados. Para obter mais informações, consulte [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).

### <a name="the-gamerendererrender-method"></a>O método GameRenderer:: render

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>A classe Simple3DGame

Esses são os métodos e membros de dados que são definidos pela classe **Simple3DGame** .

### <a name="member-functions"></a>Funções de membro

As funções de membro público definidas por **Simple3DGame** incluem aquelas abaixo.

- **Inicializar**. Define os valores iniciais das variáveis globais e inicializa os objetos do jogo. Isso é abordado na seção [inicializar e iniciar o jogo](#initialize-and-start-the-game) .
- **LoadGame**. Inicializa um novo nível e começa a carregá-lo.
- **LoadLevelAsync**. Uma corrotina que inicializa o nível e, em seguida, invoca outra corrotina no processador para carregar os recursos de nível específico do dispositivo. Esse método é executado em um thread separado; assim, apenas métodos [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (ao contrário de métodos [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) podem ser chamados nesse thread. Os métodos de contexto de dispositivo são chamados no método **FinalizeLoadLevel**. Se você for novo na programação assíncrona, consulte [operações de simultaneidade e assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).
- **FinalizeLoadLevel**. Conclua todo o trabalho para o carregamento de nível que precisa ser feito no thread principal. Isso inclui todas as chamadas para os métodos de contexto de dispositivo do Direct3D 11 ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)).
- **StartLevel**. Inicia o jogo para um novo nível.
- **PauseGame**. Pausa o jogo.
- **RunGame**. Executa uma iteração do loop do jogo. Ele é chamado em **App::Update** uma vez a cada iteração do loop do jogo caso o estado do jogo seja **Active**.
- **OnSuspending** e **OnResuming**. Suspenda/retome o áudio do jogo, respectivamente.

Aqui estão as funções de membro privado.

- **LoadSavedState** e **SaveState**. Carregar/salvar o estado atual do jogo, respectivamente.
- **LoadHighScore** e **SaveHighScore**. Carregue/salve a pontuação alta entre os jogos, respectivamente.
- **InitializeAmmo**. Redefine o estado de cada objeto de esfera usado como munição de volta ao seu estado original para o início de cada rodada.
- **UpdateDynamics**. Esse é um método importante, pois atualiza todos os objetos de jogo com base em rotinas de animação, física e entrada de controle gravadas. Esse é o núcleo da interatividade que define o jogo. Isso é abordado na seção [atualizar o mundo do jogo](#update-the-game-world) .

Os outros métodos públicos são acessadores de propriedade que retornam informações específicas do jogo e da sobreposição para a estrutura do aplicativo para exibição.

### <a name="data-members"></a>Membros de dados

Esses objetos são atualizados à medida que o loop do jogo é executado.

- Objeto **MoveLookController** . Representa a entrada do Player. Para obter mais informações, consulte [Adicionando controles](tutorial--adding-controls.md).
- Objeto **GameRenderer** . Representa um processador Direct3D 11, que manipula todos os objetos específicos do dispositivo e sua renderização. Para obter mais informações, consulte [Framework de renderização I](tutorial--assembling-the-rendering-pipeline.md).
- Objeto de **áudio** . Controla a reprodução de áudio para o jogo. Para obter mais informações, consulte [adicionando som](tutorial--adding-sound.md).

O restante das variáveis de jogo contém as listas dos primitivos e seus respectivos valores no jogo e os dados e restrições específicos do jogo.

## <a name="next-steps"></a>Próximas etapas

Ainda falaremos sobre o mecanismo de renderização real de &mdash; como as chamadas para os métodos **render** nos primitivos atualizados são transformadas em pixels na tela. Esses aspectos são abordados em duas partes &mdash; [renderizando o Framework I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) e renderização [Framework II: renderização de jogos](tutorial-game-rendering.md). Se você estiver mais interessado em como os controles do jogador atualizam o estado do jogo, consulte [adicionando controles](tutorial--adding-controls.md).

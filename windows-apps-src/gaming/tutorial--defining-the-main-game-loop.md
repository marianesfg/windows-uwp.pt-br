---
title: Definir o objeto principal do jogo
description: Agora, observaremos os detalhes do objeto principal do exemplo de jogo e como as regras implementadas são convertidas em interações com o ambiente do jogo.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8350810"
---
# <a name="define-the-main-game-object"></a>Definir o objeto principal do jogo

Depois que você tenha dispostos a estrutura básica do jogo de exemplo e implementado uma máquina de estado que manipula os usuários de alto nível e comportamentos de sistema, você vai querer examinar as regras e a mecânica que transformam o jogo de exemplo em um jogo. Vamos examinar os detalhes do objeto principal do jogo de exemplo e como traduzir regras do jogo para interações com o ambiente do jogo.

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Saiba como aplicar as técnicas básicas de desenvolvimento para implementar regras do jogo e a mecânica de um jogo UWP DirectX.

## <a name="main-game-object"></a>Objeto principal do jogo

Neste jogo de exemplo, __Simple3DGame__ é a classe de objeto principal do jogo. No método __App:: Load__ é composta por uma instância do objeto __Simple3DGame__ .

O objeto de classe __Simple3DGame__ :
* Especifica a implementação da lógica do jogo
* Contém métodos que informam:
    * Alterações no estado de jogo para a máquina de estado definida na estrutura do aplicativo.
    * Alterações no estado de jogo do aplicativo para o objeto do jogo.
    * Detalhes para atualizar a interface do usuário (sobreposição e transparente), animações e física (dinâmica) do jogo.

    >[!Note]
    >Atualização de elementos gráficos é manipulado pela classe __GameRenderer__ , que contém métodos para obter e usar recursos de dispositivo de gráficos usados pelo jogo. Para obter mais informações, consulte [a estrutura de renderização i: Introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).

* Serve como um contêiner para os dados que definem uma sessão de jogo, nível ou tempo de vida, dependendo de como você define seu jogo em um alto nível. Nesse caso, os dados de estado do jogo é para o tempo de vida do jogo e são inicializados uma vez quando o usuário inicia o jogo.

Para exibir dados definidos no objeto de classe e métodos, vá para o [objeto Simple3DGame](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar o jogo

Quando um jogado inicia o jogo, o objeto de jogo deve iniciar seu estado, criar e adicionar a sobreposição, definir as variáveis que acompanham o desempenho do jogador e instanciar os objetos usados para criar os níveis. Neste exemplo, isso é feito quando a nova instância de __GameMain__ é criada no [__App:: Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

O objeto do jogo, __Simple3DGame__, é criado no construtor __GameMain__ . Em seguida, ele é inicializado usando o método [__simple3dgame:: Initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) durante o [async criar tarefa no construtor __GameMain__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Método simple3dgame:: Initialize

O exemplo de jogo configura os seguintes componentes no objeto do jogo:

* Um novo objeto de reprodução de áudio foi criado.
* As matrizes para as primitivas gráficas do jogo são criadas, incluindo matrizes para as primitivas de nível, munição e obstáculos.
* Um local para salvar os dados de estado do jogo é criado, chamado *Jogo* e colocado no local de armazenamento das configurações de dados do aplicativo especificado pelo [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619).
* Um temporizador de jogo e o bitmap de sobreposição inicial no jogo são criados.
* Uma nova câmera é criada com um conjunto específico de parâmetros de exibição e projeção.
* O dispositivo de entrada (o controlador) é definido para a mesma arfagem e guinada da câmera, para que o jogador tenha uma correspondência de 1 para 1 entre a posição inicial do controle e a posição da câmera.
* O objeto do jogador é criado e definido como ativo. Usamos um objeto de esfera para detectar a proximidade do jogador paredes e os obstáculos e impedir que a câmera obtendo colocados em uma posição que possa interromper a imersão.
* A primitiva do ambiente do jogo é criada.
* Os obstáculos cilíndricos são criados.
* Os destinos (objetos **Face**) são criados e numerados.
* As esferas de munição são criadas.
* Os níveis são criados.
* A pontuação máxima é carregada.
* Qualquer estado de jogo salvo anteriormente é carregado.

Agora, o jogo tem instâncias de todos os componentes principais: o ambiente, o jogador, os obstáculos, os alvos e as esferas de munição. Ele também tem instâncias dos níveis, que representam configurações de todos os componentes anteriores e seus comportamentos para cada nível de modo específico. Consultemos como o jogo gera os níveis.

## <a name="build-and-load-game-levels"></a>Compilar e carregar os níveis de jogos

Grande parte do trabalho pesado para a construção de nível é feita nos arquivos __Level.h/.cpp__ encontrados na pasta __GameLevels__ da solução de exemplo. Porque ele se concentra em uma implementação muito específica, não falaremos sobre eles aqui. A parte importante é que o código de cada nível é executado como um objeto __LevelN__ separado. Se você deseja estender o jogo, você pode criar um objeto de **nível** que leva um número atribuído como parâmetro e aleatoriamente posiciona os obstáculos e os destinos. Ou, você pode fazê-lo carregar dados de configuração do nível de um arquivo de recurso, ou até mesmo da Internet.

## <a name="define-the-game-play"></a>Definir a jogabilidade

A esta altura, temos todos os componentes necessários para montar o jogo. Os níveis foram gerados na memória a partir das primitivas e estão prontos para o jogador comece a interagir com.

Tthe melhores jogos reagem instantaneamente a entradas do jogador e fornecem feedback imediato. Isso é fato para qualquer tipo de um jogo, desde os dinâmicos de ação, em tempo real atiradores em primeira pessoa para jogos de estratégia fases.

### <a name="simple3dgamerungame-method"></a>Método Simple3DGame::RunGame

Durante a reprodução de um nível, o jogo está no estado __Dynamics__ . 

[__Gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) é o loop de atualização principal que atualiza o estado do aplicativo uma vez por quadro, como mostrado a seguir. No loop de atualização, ele chama o método [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) para lidar com o trabalho se o jogo está no estado __Dynamics__ .

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
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) manipula o conjunto de dados que define o estado atual do jogo para a iteração atual do loop do jogo.

Lógica de fluxo do jogo em __RunGame__:
*  O método atualiza o temporizador que faz a contagem regressiva dos segundos até que o nível seja concluído e os testes para ver se o tempo do nível expirou. Essa é uma das regras do jogo: ao tempo é esgotado e não tem sido captura de todos os destinos, vale a fim de jogo.
*  Quando o tempo acaba, o método define o estado de jogo **TimeExpired** e retorna ao método ao método **Update** no código anterior.
*  Quando ainda há tempo, o controlador de movimento/visão é consultado para atualizar a posição da câmera. O que ocorre, especificamente, é uma atualização do ângulo do vetor normal de visão que se projeta do plano da câmera (quando o jogador está olhando) e a distância percorrida por esse ângulo desde o momento da consulta anterior ao controlador.
*  A câmera é atualizada com base nos novos dados do controlador de movimento/visão.
*  A dinâmica (ou seja, as animações e os comportamentos dos objetos no ambiente de jogo que não dependem do controle do jogador) é atualizada. Neste exemplo de jogo, o método [__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) é chamado para atualizar o movimento das esferas de munição que foram arremessadas, a animação dos obstáculos de pilar e o movimento dos alvos. Para obter mais informações, consulte [atualizar o ambiente do jogo](#update-the-game-world).
*  O método confere se os critérios de conclusão de um nível foram atendidos. Em caso positivo, ele finaliza a pontuação do nível e verifica se é o último nível (de seis). Se for, o método retornará o estado de jogo **GameComplete**; caso contrário, gerará o estado __LevelComplete__.
*  +Quando o nível não é concluído, o método define o estado de jogo como __Active__ e retorna.

## <a name="update-the-game-world"></a>Atualizar o ambiente do jogo

Neste exemplo, quando o jogo é executado, o método [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) é chamado pelo método [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) (que é chamado de [__gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) para atualizar objetos que são renderizados em uma cena de jogo.

No loop __UpdateDynamics__ , chamar métodos que são usados para definir o ambiente do jogo em movimento, independente do player de entrada, criar uma experiência de jogo imersiva em fazer com que o nível de ganham *vida*. Isso inclui elementos gráficos que precisam ser renderizado e animação em execução executa loops para trazer sobre viver, mundo vivo, mesmo quando não há nenhuma entrada do jogador. Por exemplo, árvores swaying em wind, ondas cresting ao longo da Costa, fumante máquinas e alienígenas se expansão e se movimentando. Ela também compreende a interação entre objetos, inclusive colisões entre a esfera do jogador e o ambiente ou entre a munição e os obstáculos/alvos.

O loop de jogo deve sempre manter a atualizar o ambiente do jogo se ele se baseia na lógica do jogo, algoritmos físicos, ou se ele é simplesmente aleatórias, exceto quando o jogo está especificamente pausado. 

No exemplo de jogo, esse princípio é chamado de *dinâmica* e abrange a ascensão e queda dos obstáculos em forma de pilar, o movimento e os comportamentos físicos de esferas de munição quando são lançadas. 

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics 

Este método lida com quatro onjuntos de cálculos:

* As posições das esferas de munição disparadas no mundo.
* A animação dos obstáculos de pilar.
* A intersecção do jogador e as fronteiras do mundo.
* As colisões das esferas de munição com os obstáculos, os alvos, outras esferas de munição e o ambiente.

A animação dos obstáculos é um loop definido em **Animate.h/.cpp**. O comportamento da munição e todas as colisões são definidos por algoritmos físicos simplificados, fornecidos em códigos e parametrizados por um conjunto de constantes globais para o ambiente do jogo, inclusive gravidade e propriedades relevantes. Isso tudo é calculado no espaço da coordenada do ambiente do jogo.

### <a name="review-the-flow"></a>Examine o fluxo

Agora que atualizamos todos os objetos na cena e calculamos as colisões, precisamos usar essas informações para desenhar as mudanças visuais correspondentes. 

Após __GameMain::Update()__ a iteração atual do loop do jogo, o exemplo chama imediatamente __Render ()__ para obter os dados de objetos atualizados e gerar uma nova cena apresentar para o jogador, conforme mostrado aqui. Em seguida, vamos dar uma olhada na renderização.

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

## <a name="render-the-game-worlds-graphics"></a>Renderizar os elementos gráficos do mundo do jogo

Recomendamos que os elementos gráficos de um jogo sejam atualizados sempre que possível. Isso corresponde a, no máximo, toda vez que o loop do jogo principal realiza uma iteração. À medida que o loop realiza uma iteração, o jogo é atualizado, com ou sem entradas do jogador. Isso permite que as animações e os comportamentos sejam calculados para serem exibidos de forma fluida. Imagine se tivéssemos uma cena simples de água que só se movesse quando o jogador pressionasse um botão. Isso geraria um visual terrivelmente tedioso. Um bom jogo é estável e fluido.

Lembre-se de loop do jogo de exemplo conforme mostrado acima na [__gamemain:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Quando a janela principal do jogo está visível e não está ajustada nem desativada, o jogo continua a atualizar e renderizar os resultados da atualização. O método [__de renderização__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) que está examinando agora renderiza uma representação do estado. Isso é feito imediatamente após uma chamada para **Atualizar**, que inclui **RunGame** para atualizar estados, que foi abordado na seção anterior.

Esse método desenha a projeção do mundo 3D e gera a sobreposição em Direct2D sobre ela. Quando termina, ele apresenta a cadeia de permuta final com os buffers combinados para exibição.

>[!Note]
>Há dois estados de sobreposição em Direct2D do jogo de exemplo: uma em que o jogo exibe a sobreposição de informações que contém o bitmap para o menu de pausa e uma em que o jogo exibe as miras com os retângulos para a tela sensível ao toque move-look controlador. O texto da pontuação é gerado nos dois estados. Para obter mais informações, consulte [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md).

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

Estes são os métodos e os dados que são definidos na classe de objeto __Simple3DGame__ .

### <a name="methods"></a>Métodos

Os métodos internos definidos **Simple3DGame** incluem:

-   **Inicializar**: define os valores iniciais das variáveis globais e inicializa os objetos do jogo. Isso é abordado na seção [inicializar e iniciar o jogo](#initialize-and-start-the-game) .
-   **LoadGame**: inicia um novo nível e começa a carregá-lo.
-   **LoadLevelAsync**: inicia uma tarefa assíncrona (se você estiver familiarizado com tarefas assíncronas, consulte a [Biblioteca de padrões paralelos](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) para inicializar o nível e invocar uma tarefa assíncrona no renderizador para carregar os recursos de nível específicos do dispositivo. Esse método é executado em um thread separado; assim, apenas métodos [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (ao contrário de métodos [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) podem ser chamados nesse thread. Os métodos de contexto de dispositivo são chamados no método **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: Conclua todo o trabalho de carregamento de nível que precisa ser feito no thread principal. Isso inclui todas as chamadas para os métodos de contexto de dispositivo do Direct3D 11 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)).
-   **StartLevel**: inicia a partida de um novo nível.
-   **PauseGame**: pausa o jogo.
-   **RunGame**: executa uma iteração do loop do jogo. Ele é chamado em **App::Update** uma vez a cada iteração do loop do jogo caso o estado do jogo seja **Active**.
-   **OnSuspending** e **OnResuming**: suspende e retoma o áudio do jogo, respectivamente.

E os métodos privados:

-   **LoadSavedState** e **SaveState**: carrega e salva o estado atual do jogo, respectivamente.
-   **SaveHighScore** e **LoadHighScore**: salva e carrega as pontuações máximas entre jogos, respectivamente.
-   **InitializeAmmo**: redefine o estado de cada objeto de esfera usado como munição de volta ao estado original para o início de cada rodada.
-   **UpdateDynamics**: Este é um método importante, pois atualiza todos os objetos do jogo com base em rotinas de animação repetidas, física e entrada de controle. Esse é o núcleo da interatividade que define o jogo. Isso é abordado na seção [atualizar o ambiente do jogo](#update-the-game-world) .

Os outros métodos públicos são getters de propriedades que enviam informações específicas sobre o jogo e sobreposições à estrutura do aplicativo para exibição.

### <a name="data"></a>Dados

Na parte superior do exemplo do código, há quatro objetos cujas instâncias são atualizadas à medida que o loop do jogo é executado.

-   Objeto de **MoveLookController** : representa a entrada do jogador. Para obter mais informações, consulte [Adicionando controles](tutorial--adding-controls.md).
-   Objeto de **GameRenderer** : representa o renderizador Direct3D 11 derivado da classe **DirectXBase** que manipula todos os objetos de dispositivo específico e suas renderizações. Para obter mais informações, consulte [estrutura de renderização I](tutorial--assembling-the-rendering-pipeline.md).
-   Objeto de **áudio** : controla a reprodução de áudio para o jogo. Para obter mais informações, consulte [adicionando som](tutorial--adding-sound.md).

As outras variáveis do jogo contêm as listas de primitivas e seus respectivos valores no jogo, além de dados e restrições específicos à jogabilidade.

## <a name="next-steps"></a>Próximas etapas

Agora, você já está curioso a respeito do mecanismo de renderização real: como chamadas para os métodos __de renderização__ nas primitivas atualizadas são convertidas em pixels na tela. Isso é abordado em duas partes &mdash; [estrutura de renderização i: Introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) e [estrutura de renderização II: jogo](tutorial-game-rendering.md). Se estiver mais interessado em como os controles do jogador atualizam o estado do jogo, confira o tópico sobre [adição de controles](tutorial--adding-controls.md).

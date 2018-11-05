---
author: eliotcowley
title: Estrutura do aplicativo Marble Maze
description: A estrutura de um app UWP (Plataforma Universal do Windows) DirectX é diferente daquela de um app de área de trabalho tradicional.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.author: elcowle
ms.date: 09/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, amostra, directx, estrutura
ms.localizationpriority: medium
ms.openlocfilehash: 1272200bf128443c82807aec9df5559f207819e1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035462"
---
# <a name="marble-maze-application-structure"></a>Estrutura do app Marble Maze




A estrutura de um aplicativo UWP (Plataforma Universal do Windows) DirectX é diferente daquela de um aplicativo de área de trabalho tradicional. Em vez de funcionar com tipos de identificador como [HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751) e funções como [CreateWindow](https://msdn.microsoft.com/library/windows/desktop/ms632679), o Windows Runtime oferece interfaces como [Windows::UI::Core::ICoreWindow](https://msdn.microsoft.com/library/windows/apps/br208296) para que seja possível desenvolver aplicativos UWP de uma maneira mais moderna orientada a objetos. Esta seção da documentação mostra como o código do app Marble Maze está estruturado.

> [!NOTE]
> O exemplo de código que corresponde a este documento pode ser encontrado no [Exemplo do jogo Marble Maze em DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Consulte a seguir alguns dos pontos-chave que este documento discute para quando você estruturar o código do seu jogo:

-   Na fase de inicialização, configure os componentes de tempo de execução e biblioteca usados pelo jogo e carregue recursos específicos do jogo.
-   Os aplicativos UWP devem iniciar o processamento de eventos em até cinco segundos após a inicialização. Portanto, carregue apenas os recursos essenciais ao carregar seu aplicativo. Os jogos devem carregar recursos grandes em segundo plano e exibir uma tela de progresso.
-   No loop do jogo, responda a eventos do Windows, leia a entrada do usuário, atualize objetos de cena e renderize a cena.
-   Use manipuladores de eventos para responder a eventos de janela. (Estes substituem as mensagens de janela de aplicativos da área de trabalho do Windows.)
-   Use uma máquina de estados para controlar o fluxo e a ordem da lógica do jogo.

##  <a name="file-organization"></a>Organização de arquivos


Alguns dos componentes no Marble Maze podem ser reutilizados com qualquer jogo, com pouca ou nenhuma modificação. Para seu próprio jogo, você pode adaptar a organização e as ideias fornecidas por esses arquivos. A tabela a seguir descreve resumidamente os arquivos de código-fonte importantes.

| Arquivos                                      | Descrição                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h, App.cpp               | Define as classes **App** e **DirectXApplicationSource**, que encapsulam a exibição (janela, thread e eventos) do app.                                                     |
| Audio.h, Audio.cpp                         | Define a classe **Áudio**, que gerencia recursos de áudio.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Define a classe **BasicLoader**, que fornece métodos utilitários que ajudam a carregar texturas, malhas e sombreadores.                                                                  |
| BasicMath.h                                | Define estruturas e funções que ajudam você a trabalhar com cálculos e dados de vetor e matriz. Muitas dessas funções são compatíveis com tipos de sombreadores HLSL.                     |
| BasicLeiaerWriter.h, BasicLeiaerWriter.cpp | Define a classe **BasicReaderWriter**, que usa o Windows Runtime para ler e gravar dados de arquivos em um aplicativo UWP.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Define a classe **BasicShapes**, que fornece métodos utilitários para a criação de formas básicas, como cubos e esferas. (Esses arquivos não são usados pela implementação do Marble Maze). |                                                                                  |
| Camera.h, Camera.cpp                       | Define a classe **Câmera**, que fornece a posição e a orientação de uma câmera.                                                                                               |
| Collision.h, Collision.cpp                 | Gerencia informações de colisão entre a bolinha e outros objetos, como o labirinto.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Define a função **CreateDDSTextureFromMemory**, que carrega texturas no formato .dds a partir de um buffer de memória.                                                              |
| DirectXHelper.h             | Define as funções auxiliares do DirectX que são úteis para vários aplicativos UWP DirectX.                                                                            |
| LoadScreen.h, LoadScreen.cpp               | Define a classe **LoadScreen**, que exibe uma tela de carregamento durante a inicialização do app.                                                                                         |
| MarbleMazeMain.h, MarbleMazeMain.cpp               | Define a classe **MarbleMazeMain**, que administra recursos específicos do jogo e define grande parte da lógica do jogo.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Define a classe **MediaStreamer**, que usa o Media Foundation para ajudar o jogo a gerenciar recursos de áudio.                                                                            |
| PersistentState.h, PersistentState.cpp     | Define a classe **PersistentState**, que lê e escreve tipos de dados primitivos de e em um repositório de suporte.                                                                      |
| Physics.h, Physics.cpp                     | Define a classe **Physics**, que implementa a simulação física entre a bolinha e o labirinto.                                                                              |
| Primitives.h                               | Define tipos geométricos que são usados pelo jogo.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Define a classe **SampleOverlay**, que fornece dados e operações 2D comuns e de interface do usuário.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Define a classe **SDKMesh**, que carrega e renderiza malhas no formato de Malha SDK (.sdkmesh).                                                                                |
| StepTimer.h               | Define a classe **StepTimer**, que fornece uma maneira fácil de obter totais e tempos decorridos.
| UserInterface.h, UserInterface.cpp         | Define a funcionalidade relacionada à interface do usuário, como o sistema de menus e a tabela de pontuações altas.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>Formatos de recursos de tempo de design versus tempo de execução


Quando você puder, use formatos de tempo de execução em vez de formatos de tempo de design para carregar de forma mais eficiente os recursos do jogo.

Um formato de *tempo de design* é o formato usado ao projetar um recurso. Normalmente, os designers 3D trabalham com formatos de tempo de design. Alguns formatos de tempo de design também são baseados em texto para que você possa modificá-los em qualquer editor de texto. Formatos de tempo de design podem ser detalhados e conter mais informações do que o jogo exige. Um formato de *tempo de execução* é o formato binário lido pelo jogo. Formatos de tempo de execução são normalmente mais compactos e mais eficientes de carregar do que os formatos de tempo de design correspondentes. É por isso que a maioria dos jogos usa ativos de tempo de execução em tempo de execução.

Embora o jogo possa ler diretamente um formato de tempo de design, existem várias vantagens de se usar um formato de tempo de execução separado. Como formatos de tempo de execução são muitas vezes mais compacto, eles exigem menos espaço em disco e menos tempo para serem transferidos através de uma rede. Além disso, formatos de tempo de execução são muitas vezes representados como estruturas de dados mapeadas para a memória. Portanto, eles podem ser carregados na memória com muito mais rapidez do que, por exemplo, um arquivo de texto baseado em XML. Por mim, como formatos de tempo de execução separados são geralmente codificados como binários, eles são mais difíceis de serem modificados pelo usuário final.

Sombreadores HLSL são um exemplo de recursos que usam diferentes formatos de tempo de design e de tempo de execução. O Marble Maze usa o .hlsl  como o formato de tempo de design e o .cso como o formato de tempo de execução. Um arquivo .hlsl tem um código-fonte para o sombreador. Um arquivo .cso contém o código de byte do sombreador correspondente. Ao converter arquivos .hlsl offline e fornecer arquivos .cso com o seu jogo, você evita a necessidade de converter os arquivos de origem HLSL no código de byte quando o jogo é carregado.

Por motivos didáticos, o projeto Marble Maze inclui tanto o formato de tempo de design quanto o formato de tempo de execução para muitos recursos, mas você só precisa manter os formatos de tempo de design no projeto de origem para o seu próprio jogo, pois será possível convertê-los em formatos de tempo de execução quando necessário. Esta documentação mostra como converter os formatos de tempo de design nos formatos de tempo de execução.

##  <a name="application-life-cycle"></a>Ciclo de vida do aplicativo


O Marble Maze segue o ciclo de vida de um aplicativo UWP típico. Para saber mais sobre o ciclo de vida um aplicativo UWP, consulte [Ciclo de vida do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt243287).

Quando um jogo UWP é inicializado, ele normalmente inicializa componentes de tempo de execução, como o Direct3D, o Direct2D e qualquer biblioteca de entrada, áudio ou física que precisa usar. Ele também carrega recursos específicos que são necessários antes de o jogo começar. Essa inicialização ocorre uma única vez durante uma sessão do jogo.

Após a inicialização, os jogos geralmente executam o *loop de jogo*. Nesse loop, os jogos normalmente realizam quatro ações: processam eventos do Windows, coleta a entrada, atualizam objetos de cena e renderizam a cena. Quando o jogo atualiza a cena, ele pode aplicar o estado de entrada atual aos objetos de cena e simular eventos físicos, como colisões de objetos. O jogo também pode realizar outras atividades, como reproduzir efeitos sonoros ou enviar dados através da rede. Quando o jogo renderiza a cena, ele captura o estado atual da cena e a desenha no dispositivo de vídeo. As seguintes seções descrevem essas atividades com mais detalhes.

##  <a name="adding-to-the-template"></a>Adicionando ao modelo


O modelo **App DirectX 11 (Universal do Windows)** cria uma janela principal na qual você pode fazer a renderização com o Direct3D. O modelo também inclui a classe **DeviceResources** que cria todos os recursos do dispositivo Direct3D necessários para a renderização de conteúdo 3D em um aplicativo UWP.

A classe **App** cria o objeto de classe **MarbleMazeMain**, inicia o carregamento de recursos, executa loops para atualizar o temporizador e chama o método **MarbleMazeMain::Render** para cada quadro. Os métodos **App::OnWindowSizeChanged**, **App::OnDpiChanged** e **App::OnOrientationChanged** chamam individualmente o método **MarbleMazeMain::CreateWindowSizeDependentResources**, enquanto o método **App::Run** chama os métodos **MarbleMazeMain::Update** e **MarbleMazeMain::Render**.

O exemplo a seguir mostra onde o método **App::SetWindow** cria o objeto de classe **MarbleMazeMain**. A classe **DeviceResources** é passada para o método, para que ele possa usar os objetos Direct3D para renderização.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

A classe **App** também começa a carregar os recursos adiados para o jogo. Consulte a próxima seção para saber mais detalhes.

Além disso, a classe **App** configura os manipuladores dos eventos [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Quando os manipuladores desses eventos são chamados, eles passam a entrada para a classe **MarbleMazeMain***.

## <a name="loading-game-assets-in-the-background"></a>Carregando ativos de jogo em segundo plano


Para garantir que o seu jogo possa responder a eventos de janela em até 5 segundos depois de ser iniciado, convém carregar os ativos do jogo de maneira assíncrona ou em segundo plano. Como os ativos são carregados em segundo plano, seu jogo pode responder a eventos de janela.

> [!NOTE]
> Você também pode exibir o menu principal quando ele estiver pronto e permitir que os ativos restantes continuem a ser carregados em segundo plano. Se o usuário selecionar uma opção no menu antes de todos os recursos serem carregados, você poderá indicar que os recursos da cena ainda estão sendo carregados exibindo uma barra de progresso, por exemplo.

 

Mesmo que o jogo contenha relativamente poucos ativos de jogo, uma prática recomendada é carregá-los de forma assíncrona, por dois motivos. Um dos motivos é que é difícil garantir que todos os seus recursos serão carregados rapidamente em todos os dispositivos e configurações. Além disso, incorporando o carregamento assíncrono com antecedência, o código estará pronto para ser dimensionado à medida que você adicionar funcionalidades.

O carregamento assíncrono de ativos começa com o método **App::Load**. Esse método usa a classe [task](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) para carregar ativos de jogo em segundo plano.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

A classe **MarbleMazeMain** define o sinalizador *m\_deferredResourcesReady* para indicar que o carregamento assíncrono está concluído. O método **MarbleMazeMain::LoadDeferredResources** carrega os recursos do jogo e, em seguida, define esse sinalizador. As fases de atualização (**MarbleMazeMain::Update**) e renderização (**MarbleMazeMain::Render**) do app verificam esse sinalizador. Quando esse sinalizador é definido, o jogo continua normalmente. Se o sinalizador ainda não estiver definido, o jogo mostrará a tela de carregamento.

Para saber mais sobre programação assíncrona para aplicativos UWP, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

> [!TIP]
> Se você estiver escrevendo um código de jogo que faz parte de uma Biblioteca C++ do Windows Runtime (em outras palavras, uma DLL), considere a leitura de [Criando operações assíncronas em C++ para aplicativos UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps) para aprender a criar operações assíncronas que podem ser consumidas por apps e outras bibliotecas.

 

## <a name="the-game-loop"></a>O loop do jogo


O método **App:: Run** executa o loop principal do jogo (**MarbleMazeMain::Update**). Esse método é chamado a cada quadro.

Para ajudar a separar o código de janela e exibição do código específico do jogo, implementamos o método **App::Run** para encaminhar chamadas de atualização e renderização ao objeto **MarbleMazeMain**.

O exemplo a seguir mostra o método **App::Run**, que inclui o loop principal do jogo. O loop do jogo atualiza as variáveis de tempo total e tempo de quadro e, depois, atualiza e renderiza a cena. Isso também garante que o conteúdo seja renderizado apenas quando a janela estiver visível.

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>A máquina de estados


Em geral, jogos contêm uma *máquina de estados* (também conhecida como *máquina de estados finitos*, ou FSM) para controlar o fluxo e a ordem da lógica do jogo. A máquina de estados contém um determinado número de estados e tem a capacidade de transicionar entre eles. Uma máquina de estados normalmente começa em um estado *inicial*, faz transições para um ou mais *estados intermédios* e termina possivelmente em um estado *terminal*.

Geralmente, um loop de jogo usa uma máquina de estados para poder realizar a lógica específica para o estado atual do jogo. O Marble Maze define a enumeração **GameState**, que define cada estado possível do jogo.

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

O estado **MainMenu**, por exemplo, define que o menu principal está visível e que o jogo não está ativo. De forma contrária, o estado **InGameActive** define que o jogo está ativo e que o menu não está visível. A classe **MarbleMazeMain** define a variável de membro **m\_gameState** para manter o estado ativo do jogo.

Os métodos **MarbleMazeMain::Update** e **MarbleMazeMain::Render** usam instruções "switch" para realizar a lógica do estado atual. O exemplo a seguir mostra a possível aparência dessa instrução "switch" para o método **MarbleMazeMain::Update** (os detalhes foram removidos para ilustrar a estrutura).

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

Quando a lógica do jogo ou a renderização depende de um estado de jogo específico, enfatizamos isso nesta documentação.

## <a name="handling-app-and-window-events"></a>Manipulando eventos de janela e aplicativo


O Windows Runtime fornece um sistema de manipulação de eventos orientado a objetos para que você possa gerenciar mensagens do Windows com mais facilidade. Para consumir um evento em um aplicativo, você deve fornecer um manipulador de eventos, ou o método de manipulação de eventos, que responde a esse evento. Você também deve registrar o manipulador de eventos na origem do evento. Esse processo é geralmente conhecido como conexão de eventos.

### <a name="supporting-suspend-resume-and-restart"></a>Dando suporte para processos de suspensão, retomada e reinicialização

O Marble Maze é suspenso quando o usuário alterna para outro aplicativo ou quando o Windows entra em um estado de baixo consumo de energia. O jogo é retomado quando o usuário o move para o primeiro plano ou quando o Windows sai de um estado de baixo consumo de energia. Geralmente, você não fecha aplicativos. O Windows pode encerrar o aplicativo quando ele está no estado suspenso e o Windows precisa dos recursos que o aplicativo está usando, como a memória. O Windows notifica um aplicativo quando ele está prestes a ser suspenso ou retomado, mas não o notifica quando ele está prestes a ser encerrado. Portanto, seu aplicativo deve ser capaz de salvar (no momento em que o Windows notificar o aplicativo de que ele está prestes a ser suspenso) todos os dados necessários para restaurar o estado do usuário atual quando o aplicativo for reiniciado. Se o seu aplicativo tiver um estado de usuário significativo que é importante salvar, talvez também seja necessário salvar o estado regularmente, mesmo antes de o aplicativo receber a notificação de suspensão. O Marble Maze responde a notificações de suspensão e retomada notificações por dois motivos:

1.  Quando o aplicativo é suspenso, o jogo salva o estado atual e interrompe a reprodução do áudio. Quando o aplicativo é retomado, o jogo retoma a reprodução do áudio.
2.  Quando o aplicativo é fechado e reiniciado mais tarde, o jogo recomeça a partir do seu estado anterior.

O Marble Maze realiza as seguintes tarefas para dar suporte aos processos de suspensão e retomada:

-   Ele salva seu estado no armazenamento persistente em pontos-chave do jogo, como quando o usuário atinge um determinado ponto de controle.
-   Ele responde a notificações de suspensão salvando seu estado no armazenamento persistente.
-   Ele responde a notificações de retomada carregando seu estado a partir do armazenamento persistente. Ele também carrega o estado anterior durante a inicialização.

Para dar suporte a processos de suspensão e retomada, o Marble Maze define a classe **PersistentState**. (Consulte **PersistentState.h** e **PersistentState.cpp**). Essa classe usa a interface [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet) para ler e gravar propriedades. A classe **PersistentState** fornece métodos que fazem a leitura e a gravação de tipos de dados primitivos (como **bool**, **int**, **float**, [XMFLOAT3](https://msdn.microsoft.com/library/windows/desktop/ee419475), e [Platform:: string](https://docs.microsoft.com/cpp/cppcx/platform-string-class)), de e em um repositório de suporte.

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

A classe **MarbleMazeMain** contém um objeto **PersistentState**. O construtor **MarbleMazeMain** inicializa esse objeto e fornece o repositório de dados do aplicativo local como repositório de dados de suporte.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

O Marble Maze salva seu estado quando a bolinha passa sobre um ponto de verificação ou sobre a meta (no método **MarbleMazeMain::Update**) e quando a janela perde o foco (no método **MarbleMazeMain::OnFocusChange**). Se o seu jogo tem uma grande quantidade de dados de estado, convém ocasionalmente salvar o estado no armazenamento persistente de maneira semelhante, pois você tem apenas alguns segundos para responder à notificação de suspensão. Portanto, quando o app receber uma notificação de suspensão, ele só precisará salvar os dados que mudaram.

Para responder a notificações de suspensão e retomada, a classe **MarbleMazeMain** define os métodos **SaveState** e **LoadState** que são chamados mediante suspensão e retomada. O método **MarbleMazeMain::OnSuspending** manipula o evento de suspensão, enquanto o método **MarbleMazeMain::OnResuming** manipula o evento de retomada.

O método **MarbleMazeMain::OnSuspending** salva o estado do jogo e suspende o áudio.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

O método **MarbleMazeMain::SaveState** salva valores de estado do jogo, como a posição atual e a velocidade da bolinha, o ponto de verificação mais recente e a tabela de pontuações altas.

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0;
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));

    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

Quando o jogo recomeça, ele só precisa retomar o áudio. Ele não precisa carregar o estado a partir do armazenamento persistente porque esse estado já está carregado na memória.

A forma como o jogo suspende e retoma o áudio é explicada no documento [Adicionando áudio a exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md).

Para dar suporte à reinicialização, o construtor **MarbleMazeMain**, que é chamado durante a inicialização, chama o método **MarbleMazeMain::LoadState**. O método **MarbleMazeMain::LoadState** lê e aplica o estado aos objetos do jogo. Esse método também define o estado atual do jogo como pausado se o jogo estava pausado ou ativo no momento em que foi suspenso. Pausamos o jogo para que o usuário não se surpreenda com atividades inesperadas. Além disso, o método move para o menu principal se o jogo não estava em estado de interação com o usuário no momento em que foi suspenso.

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> O Marble Maze não faz distinção entre partida a frio, ou seja, começar pela primeira vez sem um evento de suspensão anterior, e retomada a partir de um estado suspenso. Este é um design recomendado para todos os aplicativos UWP.

Para obter mais informações sobre dados de aplicativo, consulte [Armazenar e recuperar configurações e outros dados de app](https://msdn.microsoft.com/library/windows/apps/mt299098).

##  <a name="next-steps"></a>Próximas etapas


Leia [Adicionando conteúdo visual à amostra do Marble Maze](adding-visual-content-to-the-marble-maze-sample.md) para saber mais sobre algumas das principais práticas que você deve ter em mente ao trabalhar com recursos visuais.

## <a name="related-topics"></a>Tópicos relacionados

* [Adicionando conteúdo visual à amostra do Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Princípios básicos da amostra do Marble Maze](marble-maze-sample-fundamentals.md)
* [Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 





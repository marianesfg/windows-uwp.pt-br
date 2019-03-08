---
title: Adicionando entrada e interatividade ao exemplo do Marble Maze
description: Saiba mais sobre as principais práticas que você deve ter em mente ao trabalhar com dispositivos de entrada.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, entrada, amostra
ms.localizationpriority: medium
ms.openlocfilehash: d545f696a93bfa8416e1a772ecc015867a3615c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611811"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Adicionando entrada e interatividade ao exemplo do Marble Maze




Jogos da Plataforma Universal do Windows (UWP) funcionam em uma variedade de dispositivos, como computadores de mesa, laptops e tablets. Um dispositivo pode ter diversos mecanismos de entrada e controle. Este documento descreve as principais práticas a serem consideradas quando se trabalha com dispositivos de entrada e mostra como o Marble Maze as aplica.

> [!NOTE]
> O exemplo de código que corresponde a este documento pode ser encontrado no [Exemplo do jogo Marble Maze em DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

 
Seguem alguns dos pontos principais discutidos neste documento para quando você trabalhar com entrada no seu jogo:

-   Quando possível, dê suporte a vários dispositivos de entrada para permitir que o seu jogo acomode uma ampla variedade de preferências e recursos entre seus clientes. Embora o uso do controlador de jogo e do sensor seja opcional, recomendamos-o para melhorar a experiência do jogador. Nós projetamos as APIs do controlador de jogo e do sensor para ajudá-lo a integrar mais facilmente esses dispositivos de entrada.

-   Para inicializar o touch, você deve se registrar para eventos de janela como quando o ponteiro estiver ativado, liberado e movido. Para inicializar o acelerômetro, crie um objeto [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) quando inicializar o aplicativo. O controle do Xbox não exige inicialização.

-   Para jogos de um jogador, considere a possibilidade de combinar a entrada de todos os controladores possíveis do Xbox. Desta forma, você não precisa acompanhar a entrada que vem de cada controlador. Ou basta acompanhar a entrada apenas do controlador adicionado mais recentemente, como mostrado neste exemplo.

-   Processe os eventos do Windows antes de processar os dispositivos de entrada.

-   O controlador do Xbox e o acelerômetro dão suporte a sondagem. Isso é, você pode sondar dados quando precisar. Para o toque, grave os eventos de toque em estruturas de dados que estão disponíveis para o seu código de processamento de entrada.

-   Considere a possibilidade de normalizar os valores de entrada para um formato comum. Isso pode simplificar como a entrada é interpretada por outros componentes do seu jogo, como a simulação física, e pode tornar mais fácil a gravação de jogos que funcionam em resoluções de tela diferentes.

## <a name="input-devices-supported-by-marble-maze"></a>Dispositivos de entrada compatíveis com o Marble Maze


O Marble Maze aceita o controlador do Xbox, mouse e toque para selecionar itens de menu e o controlador do Xbox, mouse, toque e acelerômetro para controlar o jogo. O Marble Maze usa as APIs [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) para sondar a entrada do controlador. O toque permite aos aplicativos acompanhar e responder à entrada pela ponta do dedo. Um acelerômetro é um sensor que mede a força aplicada ao longo dos eixos X, Y e Z. Usando o Windows Runtime, você pode pesquisar o estado atual do dispositivo do acelerômetro, e também receber eventos de touch através mecanismo que lida com eventos do Windows Runtime.

> [!NOTE]
> Este documento usa o toque para se referir à entrada por toque e de mouse e o ponteiro para se referir a qualquer dispositivo que usa eventos de ponteiro. Pelo touch e o mouse usarem eventos de ponteiro padrão, você pode usar qualquer um desses dispositivos para selecionar itens do menu e controlar o jogo.

 

> [!NOTE]
> O manifesto do pacote define **Paisagem** como a única rotação suportada para o jogo prevenir a orientação de mudar quando você gira o dispositivo para rolar a bola de gude. Para visualizar o manifesto do pacote, abre o **Package.appxmanifest** no **Gerenciador de soluções** no Visual Studio.

 

## <a name="initializing-input-devices"></a>Inicializando dispositivos de entrada


O controlador do Xbox não precisa ser inicializado. Para inicializar o touch, você deve se registrar para eventos de janela como quando o ponteiro estiver ativado (por exemplo, o player pressiona o botão do mouse ou toca a rela), liberado e movido. Para inicializar o acelerômetro, você deve criar um objeto [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) quando inicializar o aplicativo.

O exemplo a seguir mostra como o método **App::SetWindow** registra para os eventos de ponteiro [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) e [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved). Esses eventos são registrados durante a inicialização do aplicativo e antes do loop do jogo.

Esses eventos são tratados em um thread separado que evoca os tratadores de eventos.

Para saber mais como o aplicativo é inicializado, consulte a [estrutura do aplicativo Marble Maze.](marble-maze-application-structure.md)

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

A classe **MarbleMazeMain** também criam um objeto **std::map** para abrigar eventos de toque. A chave para este objeto de mapa é um valor que identifica exclusivamente o ponteiro de entrada. Cada chave é mapeada para a distância entre cada ponto de contato e o centro da tela. Em seguida, o Marble Maze usa esses valores para calcular o quanto o labirinto é inclinado.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

A classe **MarbleMazeMain** também contém um objeto [Acelerômetro](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer).

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

O objeto **Acelerômetro** é inicializado no construtor **MarbleMazeMain**, como visto no exemplo a seguir. O método [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) retorna uma instância do acelerômetro padrão. Se não houver um acelerômetro padrão, o **Accelerometer::GetDefault** retorna **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navegando nos menus

Você pode usar mouse, touch ou o controle do Xbox para navegar nos menus, dessa forma:

-   Use o direcional para alterar o item de menu ativo.
-   Use o toque, o botão A ou o botão de menu para selecionar um item de menu ou fechar o menu atual, como a tabela de melhores pontuações.
-   Use o botão de menu para pausar ou reiniciar o jogo.
-   Clique em um item de menu usando o mouse para escolher essa ação.

###  <a name="tracking-xbox-controller-input"></a>Rastreando a entrada do controle do Xbox

Para manter o controle dos gamepads atualmente conectados ao dispositivo, o **MarbleMazeMain** define uma variável de membro, **m_myGamepads**, que é um conjunto de objetos [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Ela será inicializada no construtor da seguinte forma:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

Além disso, o construtor **MarbleMazeMain** registra eventos de quando os gamepads são adicionados ou removidos:

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

Quando um gamepad é adicionado, ele é acrescentado a **m_myGamepads**; quando um gamepad é removido, verificamos se o gamepad está em **m_myGamepads** e, em caso afirmativo, o removemos. Em ambos os casos, definimos **m_currentGamepadNeedsRefresh** como **true**, indicando que precisamos reatribuir **m_gamepad**.

Por fim, atribuímos um gamepad a **m_gamepad** e definimos **m_currentGamepadNeedsRefresh** como **false**:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

No método **Update**, verificamos se **m_gamepad** precisa ser reatribuído:

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

Se o **m_gamepad** precisar ser reatribuído, atribuímos a ele o gamepad adicionado mais recentemente, usando **GetLastGamepad**, que é definido da seguinte forma:

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

Esse método simplesmente retorna o último gamepad em **m_myGamepads**.

Você pode conectar até quatro controladores do Xbox a um dispositivo do Windows 10. Para evitar ter que descobrir qual controlador é o ativo, simplesmente acompanhamos o gamepad adicionado mais recentemente. Se o seu jogo dá suporte a mais de um jogador, você terá que rastrear a entrada de cada jogador separadamente.

O método **MarbleMazeMain::Update** sonda o gamepad de entrada:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Continuamos acompanhando a leitura da entrada obtida no último quadro com **m_oldReading** e a leitura de entrada mais recente com **m_newReading**, que podemos obter chamando [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Isso retorna um objeto [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading), que contém as informações sobre o estado atual do gamepad.

Para verificar se um botão acabou de ser pressionado ou liberado, definimos **MarbleMazeMain::ButtonJustPressed** e **MarbleMazeMain::ButtonJustReleased**, que comparam as leituras de botão desse quadro e do último quadro. Dessa forma, podemos executar uma ação somente quando um botão é inicialmente pressionado ou liberado e não quando é mantido pressionado:

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

As leituras de [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) são comparadas por meio de operações bit a bit&mdash;verificamos se um botão é pressionado por meio de *bit a bit e* (&). Podemos determinar se um botão acabou de ser pressionado ou liberado comparando a leitura antiga e a leitura nova.

Usando os métodos acima, verificamos se determinados botões foram pressionadas e executamos todas as ações correspondentes que devem ocorrer. Por exemplo, se o botão de menu (**GamepadButtons::Menu**) for pressionado, o estado do jogo muda de ativo para pausado ou pausado para ativo.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

Também verificamos se o player pressiona o botão Exibir, caso no qual reiniciamos o jogo ou limpamos a tabela de recordes.

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

Se o menu principal está ativo, o item de menu ativo muda quando o direcional é pressionado para cima ou para baixo. Se o usuário escolher a seleção atual, o elemento de interface do usuário adequado será marcado como escolhido.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>Acompanhar a entrada por toque e de mouse

Para entradas de touch e de mouse, um item do menu é escolhido quando o usuário clicar ou tocar nele. O exemplo a seguir mostra como o método **MarbleMazeMain::Update** processa a entrada do ponteiro para selecionar itens do menu. O **m\_pointQueue** variável de membro rastreia os locais em que o usuário tocadas ou clicou na tela. A forma como o Marble Maze coleta entradas de ponteiro será descrita com mais detalhes neste documento, na seção [Processando a entrada de ponteiro](#processing-pointer-input).

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

O método **UserInterface::HitTest** determina de o ponto fornecida está localizado nos limites de algum elemento de interface do usuário. Qualquer elemento de interface do usuário que passe nesse teste é marcado como sendo tocado. Esse método usa a função auxiliar **PointInRect** para determinar se o ponto fornecido está localizado nos limites de cada elemento de interface do usuário.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>Atualizando o estado do jogo

Depois de o método **MarbleMazeMain::Update** processar a entrada do controle e do touch, ele atualiza o estado do jogo se algum botão for pressionado.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>Controlando seu jogo


O loop de jogo e o método **MarbleMazeMain::Update** trabalham juntos para atualizar o estado dos objetos de jogo. Se o seu jogo aceita a entrada de vários dispositivos, você pode acumular a entrada de todos os dispositivos em um conjunto de variáveis de modo que possa escrever um código mais fácil de manter. O método **MarbleMazeMain::Update** define um conjunto de variáveis que acumulam movimentos de todos os dispositivos.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

O mecanismo de entrada pode variar de um dispositivo de entrada para outro. Por exemplo, a entrada de ponteiro é tratada usando o modelo de manipulação de eventos do Windows Runtime. Por outro lado, você pode sondar os dados de entrada do controlador do Xbox quando precisar deles. Recomendamos que você siga sempre o mecanismo de entrada que é prescrito para um determinado dispositivo. Esta seção descreve como o Marble Maze lê a entrada de cada dispositivo, como atualiza os valores de entrada combinados e como usa os valores de entrada combinados para atualizar o estado do jogo.

###  <a name="processing-pointer-input"></a>Processante a entrada de ponteiro

Quando você trabalhar com entrada de ponteiro, chame o método [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) para processar eventos de janela. Chame esse método no loop do jogo antes de atualizar ou renderizar a cena. O Marble Maze chama isso no método **App::Run**: 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Se a janela estiver visível, passamos **CoreProcessEventsOption::ProcessAllIfPresent** para **ProcessEvents** para processar todos os eventos em fila e retornarmos imediatamente; caso contrário, passamos **CoreProcessEventsOption::ProcessOneAndAllPending** para processar todos os eventos em fila e aguardamos o próximo evento novo. Depois que todos os eventos são processados, o Marble Maze renderiza e apresenta o quadro seguinte.

O Windows Runtime chama o manipulador registrado para cada evento que ocorreu. O método **App::SetWindow** registra-se em eventos e encaminha informações de ponteiro para a classe **MarbleMazeMain**.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

A classe **MarbleMazeMain** reage aos eventos de ponteiro atualizando o objeto de mapa que armazena eventos de touch. O método **MarbleMazeMain::AddTouch** é chamado quando o ponteiro é pressionado pela primeira vez, por exemplo, quando o usuário toca a tela inicialmente em um dispositivo touch. O método **MarbleMazeMain::UpdateTouch** é chamado quando a posição do ponteiro se move. O método **MarbleMazeMain::RemoveTouch** é chamado quando o ponteiro é liberado, por exemplo, quando o usuário para de tocar na tela.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

A função **PointToTouch** converte a posição atual do ponteiro, para que a origem esteja no centro da tela e dimensiona as coordenadas para que variem aproximadamente entre -1,0 e +1,0. Isso facilita o cálculo da inclinação do labirinto de forma consistente em diferentes métodos de entrada.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

O método **MarbleMazeMain::Update** atualiza os valores de entrada atualizados incrementando o fator de inclinação em um valor de dimensionamento constante. Esse valor de dimensionamento foi determinado pela tentativa de vários valores diferentes.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>Processando a entrada do acelerômetro

Para processar a entrada do acelerômetro, o método **MarbleMazeMain::Update** chama o método [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699). Esse método devolve um objeto [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688), que representa uma leitura de acelerador. As propriedades **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** e **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** armazenam a aceleração da força-g ao longo dos eixos X e Y, respectivamente.

O exemplo a seguir mostra como o método **MarbleMazeMain::Update** pesquisa o acelerômetro e atualiza os valores de entrada combinados. À medida que você inclina o aparelho, a gravidade faz com que a bolinha se mova mais rápido.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

Como você não pode ter certeza de que um acelerômetro está presente no computador do usuário, verifique se possui um objeto [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) válido antes de sondar o acelerômetro.

### <a name="processing-xbox-controller-input"></a>Processando a entrada do controlador do Xbox

No método **MarbleMazeMain::Update**, usamos **m_newReading** para processar a entrada do direcional analógico esquerdo:

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

Verificamos se a entrada do direcional analógico esquerdo está fora da zona morta e, em caso afirmativo, o adicionamos a **combinedTiltX** e **combinedTiltY** (multiplicados por um fator de escala) para inclinar o palco.

> [!IMPORTANT]
> Quando você trabalhar com o controlador do Xbox, considere sempre a zona morta. A zona morta se refere a variação de sensibilidade ao movimento inicial entre os consoles de jogos. Em alguns controladores, um pequeno movimento pode não gerar nenhuma leitura, mas em outros pode gerar uma leitura mensurável. Para considerar isio em seu jogo, crie uma zona de não movimento para o movimento inicial do thumbstick. Para obter mais informações sobre a zona morta, consulte [Lendo os botões de controle](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Aplicando entrada ao estado de jogo

Os dispositivos relatam valores de entrada de diferentes maneiras. Por exemplo, a entrada de ponteiro pode ser em coordenadas de tela e a entrada de controlador pode ser em um formato completamente diferente. Um desafio ao se combinarem entradas de vários dispositivos em um conjunto de valores de entrada é a normalização, ou a conversão de valores para um formato comum. Marble Maze normaliza valores escalando-os para o intervalo \[-1,0, 1,0\]. A função **PointToTouch**, que já foi descrita nesta seção, converte coordenadas de tela para valores normalizados que vão, aproximadamente, de -1,0 a +1,0.

> [!TIP]
> Mesmo se seu aplicativo usa um método de entrada, recomendamos que você sempre normalize os valores de entrada. Isso pode simplificar como a entrada é interpretada por outros componentes do seu jogo, como a simulação física, e facilita a gravação de jogos que funcionam em resoluções de tela diferentes.

 

Depois de o método **MarbleMazeMain::Update** processar a entrada, ele cria um vetor que representa o efeito da inclinação do labirinto na bola de gude. O exemplo a seguir mostra como o Marble Maze usa a função [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize) para criar um vetor de gravidade normalizado. A variável **maxTilt** impulsiona a quantia que faz o labirinto inclinar e evita que ele se incline para o lado.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Para concluir a atualização de objetos de cena, o Marble Maze passa o vetor gravidade atualizado para a simulação física, atualiza a simulação física para o tempo decorrido desde o quadro anterior e atualiza a posição e a orientação da bolinha de gude. Se a bola de gude caiu através do labirinto, o método **MarbleMazeMain::Update** a coloca de volta no último ponto de verificação que ela tocou e redefine o estado de simulação de física.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

Esta seção não descreve o funcionamento da simulação física. Para saber mais sobre esse assunto, consulte os arquivos **Physics.h** e **Physics.cpp** nas origens do Marble Maze.

## <a name="next-steps"></a>Próximas etapas


Leia [Adicionando áudio ao exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md) para saber mais sobre as principais práticas a serem consideradas quando você trabalha com áudio. O documento discute como o Marble Maze usa o Microsoft Media Foundation e XAudio2 para carregar, mixar e reproduzir recursos de áudio.

## <a name="related-topics"></a>Tópicos relacionados


* [Adicionando áudio ao exemplo Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Adicionando conteúdo visual ao exemplo Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Desenvolvendo o Marble Maze, um jogo UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 





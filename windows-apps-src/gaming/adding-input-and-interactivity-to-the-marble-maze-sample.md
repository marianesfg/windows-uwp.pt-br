---
author: mtoepke
title: Adicionando entrada e interatividade ao exemplo do Marble Maze
description: Jogos de aplicativos da Plataforma Universal do Windows (UWP) funcionam em uma variedade de dispositivos, como computadores de mesa, laptops e tablets.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 563ee292ec7189b0c365ae5ee0d1c41fd6fd1a09

---

# Adicionando entrada e interatividade ao exemplo do Marble Maze


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Jogos de aplicativos da Plataforma Universal do Windows (UWP) funcionam em uma variedade de dispositivos, como computadores de mesa, laptops e tablets. Um dispositivo pode ter diversos mecanismos de entrada e controle. Dê suporte a vários dispositivos de entrada para permitir que o seu jogo acomode uma ampla variedade de preferências e recursos entre seus clientes. Este documento descreve as principais práticas a serem consideradas quando se trabalha com dispositivos de entrada e mostra como o Marble Maze as aplica.

> **Observação**   O código de exemplo que corresponde a este documento pode ser encontrado em [DirectX Marble Maze game sample](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Seguem alguns dos pontos principais discutidos neste documento para quando você trabalhar com entrada no seu jogo:

-   Quando possível, dê suporte a vários dispositivos de entrada para permitir que o seu jogo acomode uma ampla variedade de preferências e recursos entre seus clientes. Embora o uso do controlador de jogo e do sensor seja opcional, recomendamos-o para melhorar a experiência do jogador. Nós projetamos a API do controlador de jogo e do sensor para ajudá-lo a integrar mais facilmente esses dispositivos de entrada.
-   Para inicializar o touch, você deve se registrar para eventos de janela como quando o ponteiro estiver ativado, liberado e movido. Para inicializar o acelerômetro, crie um objeto [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) quando inicializar o aplicativo. O controle do Xbox 360 não exige inicialização.
-   Para jogos de um jogador, considere a possibilidade de combinar a entrada de todos os controladores possíveis do Xbox 360. Desta forma, você não precisa acompanhar a entrada que vem de cada controlador.
-   Processe os eventos do Windows antes de processar os dispositivos de entrada.
-   O controlador do Xbox 360 e o acelerômetro dão suporte a sondagem. Isso é, você pode sondar dados quando precisar. Para o toque, grave os eventos de toque em estruturas de dados que estão disponíveis para o seu código de processamento de entrada.
-   Considere a possibilidade de normalizar os valores de entrada para um formato comum. Isso pode simplificar como a entrada é interpretada por outros componentes do seu jogo, como a simulação física, e pode tornar mais fácil a gravação de jogos que funcionam em resoluções de tela diferentes.

## Dispositivos de entrada compatíveis com o Marble Maze


O Marble Maze aceita dispositivos controladores comuns do Xbox 360, mouse e toque para selecionar itens de menu e o controlador do Xbox 360, mouse, toque e acelerômetro para controlar o jogo. O Marble Maze usa a API XInput para sondar a entrada do controlador. O toque permite aos aplicativos acompanhar e responder à entrada pela ponta do dedo. Um acelerômetro é um sensor que mede a força aplicada ao longo dos eixos x, y e z. Usando o Windows Runtime, você pode pesquisar o estado atual do dispositivo do acelerômetro, e também receber eventos de touch através mecanismo que lida com eventos do Windows Runtime.

> **Observação**  Este documento usa touch para referir-se tanto a touch quanto à entrada de mouse e ponteiro para referir-se a qualquer dispositivo que use eventos de ponteiro. Pelo touch e o mouse usarem eventos de ponteiro padrão, você pode usar qualquer um desses dispositivos para selecionar itens do menu e controlar o jogo.

 

> **Observação**  O manifesto do pacote define Paisagem como a rotação suportada para o jogo prevenir a orientação de mudar quando você gira o dispositivo para rolar a bola de gude.

 

## Inicializando dispositivos de entrada


O controlador do Xbox 360 não precisa ser inicializado. Para inicializar o touch, você deve se registrar para eventos de janela como quando o ponteiro estiver ativado (por exemplo, seu usuário pressiona o botão do mouse ou toca a rela), liberado e movido. Para inicializar o acelerômetro, você deve criar um objeto [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) quando inicializar o aplicativo.

O exemplo a seguir mostra como o construtor **DirectXPage** se registra para os eventos de ponteiro [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471), [**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472) e [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) para o [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Esses eventos são registrados durante a inicialização do aplicativo e antes do loop do jogo.

Esses eventos são tratados em um thread separado que evoca os tratadores de eventos.

Para saber mais como o aplicativo é inicializado, consulte a [estrutura do aplicativo Marble Maze.](marble-maze-application-structure.md)

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

A classe MarbleMaze também criam um objeto std::map para abrigar eventos de toque. A chave para este objeto de mapa é um valor que identifica exclusivamente o ponteiro de entrada. Cada chave é mapeada para a distância entre cada ponto de contato e o centro da tela. Em seguida, o Marble Maze usa esses valores para calcular o quanto o labirinto é inclinado.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

A classe MarbleMaze contém um objeto do acelerômetro.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

O objeto Acelerômetro é inicializado no método MarbleMaze::Initialize, como visto no exemplo a seguir. O método Windows::Devices::Sensors::Accelerometer::GetDefault retorna uma instância do acelerômetro padrão. Se não houver um acelerômetro padrão Accelerometer::GetDefault, o valor de m\_accelerometer continuar nullptr.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  Navegando nos menus


###  Rastreando a entrada do controle do Xbox 360

Você pode usar mouse, touch ou o controle do Xbox 360 para navegar nos menus, dessa forma:

-   Use o direcional para alterar o item de menu ativo.
-   Use o toque, o botão A ou o botão Iniciar para selecionar um item de menu ou fechar o menu atual, como a tabela de melhores pontuações.
-   Use o botão Iniciar para pausar ou reiniciar o jogo.
-   Clique em um item de menu usando o mouse para escolher essa ação.

###  Acompanhar a entrada por toque e de mouse

Para rastrear a entrada controle do Xbox 360 o método **MarbleMaze::Update** define uma matriz de botões que definem os comportamentos de entrada. O XInput fornece apenas o estado atual do controle. Portanto, o **MarbleMaze::Update** também define duas matrizes que rastreiam, para cada possível controle do Xbox 360, se cada botão foi pressionado durante o quadro anterior ou se cada botão está pressionado no momento.

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

Você pode conectar até quatro controladores do Xbox 360 a um dispositivo do Windows. Para evitar ter que descobrir qual é o controle ativo, o método **MarbleMaze::Update** combina a entrada através de todos os controles.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

Se o seu jogo dá suporte a mais de um jogador, você terá que rastrear a entrada de cada jogador separadamente.

Em um loop, o método **MarbleMaze::Update** pesquisa cada controle pela entrada e lê o estado de cada botão.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

Depois de o método **MarbleMaze::Update** pesquisar pela entrada, ele atualiza a matriz de entrada combinada. A matriz de entrada combinada acompanha somente quais botões estão pressionados, mas que não foram pressionados anteriormente. Isso permite que o jogo execute uma ação somente quando um botão for inicialmente pressionado e não quando se manter pressionado.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

Depois de o método **MarbleMaze::Update** recolher a saída do botão, ele executa qualquer ação que deve acontecer. Por exemplo, se o botão Iniciar(XINPUT\_GAMEPAD\_START) for pressionado, o estado do jogo muda de ativo para pausado ou de pausado para ativo.

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

Se o menu principal está ativo, o item de menu ativo muda quando o direcional é pressionado para cima ou para baixo. Se o usuário escolher a seleção atual, o elemento de interface do usuário adequado será marcado como escolhido.

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
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
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
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

Depois de o método **MarbleMaze::Update** processar a entrada do controle, ele salva o estado atual da entrada para o próximo quadro.

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### Acompanhar a entrada por toque e de mouse

Para entradas de touch e de mouse, um item do menu é escolhido quando o usuário clicar ou tocar nele. O exemplo a seguir mostra como o método **MarbleMaze::Update** processa a entrada do ponteiro para selecionar itens do menu. A variável do membro **m\_pointQueue** rastreia os locais em que o usuário tocou ou clicou na tela. A forma como o Marble Maze coleta entradas de ponteiro será descrita com mais detalhes neste documento, na seção Processando a entrada de ponteiro.

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

### Atualizando o estado do jogo

Depois de o método **MarbleMaze::Update** processar a entrada do controle e do touch, ele atualiza o estado do jogo se algum botão for pressionado.

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

##  Controlando seu jogo


O loop de jogo e o método **MarbleMaze::Update** trabalham juntos para atualizar o estado dos objetos de jogo. Se o seu jogo aceita a entrada de vários dispositivos, você pode acumular a entrada de todos os dispositivos em um conjunto de variáveis de modo que possa escrever um código mais fácil de manter. O método **MarbleMaze::Update** define um conjunto de variáveis que acumulam movimentos de todos os dispositivos.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

O mecanismo de entrada pode variar de um dispositivo de entrada para outro. Por exemplo, a entrada de ponteiro é tratada usando o modelo de manipulação de eventos do Windows Runtime. Por outro lado, você pode sondar os dados de entrada do controlador do Xbox 360 quando precisar deles. Recomendamos que você siga sempre o mecanismo de entrada que é prescrito para um determinado dispositivo. Esta seção descreve como o Marble Maze lê a entrada de cada dispositivo, como atualiza os valores de entrada combinados e como usa os valores de entrada combinados para atualizar o estado do jogo.

###  Processante a entrada de ponteiro

Quando você trabalhar com entrada de ponteiro, chame o método [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217) para processar eventos de janela. Chame esse método no loop do jogo antes de atualizar ou renderizar a cena. O Marble Maze passa o **CoreProcessEventsOption::ProcessAllIfPresent** para esse métodos processar todos os eventos em fila e, então, retorna imediatamente. Depois que todos os eventos são processados, o Marble Maze renderiza e apresenta o quadro seguinte.

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

O Windows Runtime chama o manipulador registrado para cada evento que ocorreu. A classe **DirectXApp** registra-se em eventos e encaminha informações de ponteiro para a classe **MarbleMaze**.

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

A classe **MarbleMaze** reage aos eventos de ponteiro atualizando o objeto de mapa que armazena eventos de touch. O método **MarbleMaze::AddTouch** é chamado quando o ponteiro é pressionado pela primeira vez, por exemplo, quando o usuário toca a tela inicialmente em um dispositivo touch. O método **MarbleMaze::AddTouch** é chamado quando a posição do ponteiro se move. O método **MarbleMaze::RemoveTouch** é chamado quando o ponteiro é liberado, por exemplo, quando o usuário para de tocar na tela.

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

A função PointToTouch converte a posição atual do ponteiro, para que a origem esteja no centro da tela e dimensiona as coordenadas para que variem aproximadamente entre -1,0 e +1,0. Isso facilita o cálculo da inclinação do labirinto de forma consistente em diferentes métodos de entrada.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

O método **MarbleMaze::Update** atualiza os valores de entrada atualizados incrementando o fator de inclinação em um valor de dimensionamento constante. Esse valor de dimensionamento foi determinado pela tentativa de vários valores diferentes.

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### Processando a entrada do acelerômetro

Para processar a entrada do acelerômetro, o método **MarbleMaze::Update** chama o método [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699). Esse método devolve um objeto [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688), que representa uma leitura de acelerador. As propriedades **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** e **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** armazenam a aceleração da força-g ao longo dos eixos x e y, respectivamente.

O exemplo a seguir mostra como o método **MarbleMaze::Update** pesquisa o acelerômetro e atualiza os valores de entrada combinados. À medida que você inclina o aparelho, a gravidade faz com que a bolinha se mova mais rápido.

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

Como você não pode ter certeza de que um acelerômetro está presente no computador do usuário, verifique se possui um objeto Accelerometer válido antes de sondar o acelerômetro.

### Processando a entrada do controlador do Xbox 360

O exemplo a seguir mostra como o método **MarbleMaze::Update** lê a partir do controle do Xbox 360 e atualiza os valores de entrada combinados. O método **MarbleMaze::Update** usa um loop for para permitir que a entrada seja recebida de qualquer controle conectado. O método **XInputGetState** preenche um objeto XINPUT\_STATE com o estado atual do controle. Os valores **combinedTiltX** e **combinedTiltY** são atualizados de acordo com os valores x e y da alavanca esquerda.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput define a constante **XINPUT\_GAMEPAD\_LEFT\_THUMB\_DEADZONE** da alavanca esquerda. Essa é um limiar de dead zone para a maioria dos jogos.

> **Importante**  Quando você tralhar com o controle do Xbox 360, sempre leve em consideração a dead zone. A zona morta se refere a variação de sensibilidade ao movimento inicial entre os consoles de jogos. Em alguns controladores, um pequeno movimento pode não gerar nenhuma leitura, mas em outros pode gerar uma leitura mensurável. Para considerar isio em seu jogo, crie uma zona de não movimento para o movimento inicial do thumbstick. Para saber mais sobre a zona morta, consulte [Introdução ao XInput.](https://msdn.microsoft.com/library/windows/desktop/ee417001)

 

###  Aplicando entrada ao estado de jogo

Os dispositivos relatam valores de entrada de diferentes maneiras. Por exemplo, a entrada de ponteiro pode ser em coordenadas de tela e a entrada de controlador pode ser em um formato completamente diferente. Um desafio ao se combinarem entradas de vários dispositivos em um conjunto de valores de entrada é a normalização, ou a conversão de valores para um formato comum. O Marble Maze normaliza valores dimensionando-os no intervalo \[-1,0, 1,0\]. Para normalizar a entrada de controle do Xbox 360, o Marble Maze divide os valores de entrada por 32768 porque os valores de entrada da alavanca sempre ficam entre -32768 e 32767. A função **PointToTouch**, que já foi descrita nesta seção, alcança um resultado semelhante ao converter coordenadas de tela para valores normalizados que vão, aproximadamente, de -1,0 a +1,0.

> **Dica**  Mesmo que o seu aplicativo use um método de entrada, aconselhamos que você sempre use valores de entrada normalizados. Isso pode simplificar como a entrada é interpretada por outros componentes do seu jogo, como a simulação física, e facilita a gravação de jogos que funcionam em resoluções de tela diferentes.

 

Depois de o método **MarbleMaze::Update** processar a entrada, ele cria um vetor que representa o efeito da inclinação do labirinto na bola de gude. O exemplo a seguir mostra como o Marble Maze usa a função **XMVector3Normalize** para criar um vetor de gravidade normalizado. A variável *MaxTilt* impulsiona a quantia que faz o labirinto inclinar e evita que ele se incline para o lado.

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

Para concluir a atualização de objetos de cena, o Marble Maze passa o vetor gravidade atualizado para a simulação física, atualiza a simulação física para o tempo decorrido desde o quadro anterior e atualiza a posição e a orientação da bolinha de gude. Se a bola de gude caiu através do labirinto, o método **MarbleMaze::Update** a coloca de volta no último ponto de verificação que ela tocou e redefine o estado de simulação de física.

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
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

Esta seção não descreve o funcionamento da simulação física. Para saber mais sobre esse assunto, consulte os arquivos Physics.h e Physics.cpp nas origens do Marble Maze.

## Próximas etapas


Leia [Adicionando áudio ao exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md) para saber mais sobre as principais práticas a serem consideradas quando você trabalha com áudio. O documento discute como o Marble Maze usa o Microsoft Media Foundation e XAudio2 para carregar, mixar e reproduzir recursos de áudio.

## Tópicos relacionados


* [Adicionando áudio ao exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Adicionando conteúdo visual ao exemplo do Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->



---
author: mtoepke
title: Adicionar controles
description: "Agora veremos como o jogo de exemplo implementa controles move-look em um jogo 3D e como desenvolver controles básicos de toque, mouse e controlador de jogo."
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, controles, entrada
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d70e9ef8efffd2a78f6c49596e716770a9162b5c
ms.lasthandoff: 02/07/2017

---

# <a name="add-controls"></a>Adicionar controles


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Agora veremos como o jogo de exemplo implementa controles move-look em um jogo 3D e como desenvolver controles básicos de toque, mouse e controlador de jogo.

## <a name="objective"></a>Objetivo


-   Implementar controles de mouse/teclado, toque e controlador Xbox em um jogo da Plataforma Universal do Windows (UWP) com DirectX.

## <a name="uwp-game-apps-and-controls"></a>Controles e aplicativos de jogos UWP


Um bom jogo UWP dá suporte a uma grande variedade de interfaces. Um jogador em potencial pode ter o Windows 10 em um tablet sem botões físicos, ou em um PC multimídia com um controlador Xbox conectado, ou no mais avançado equipamento desktop para jogos, com mouse e teclado de alto desempenho para jogos. Seu jogo, caso o design o permita, deve ser compatível com todos esses dispositivos.

Este exemplo dá suporte a todos os três. Trata-se de um jogo simples de tiro em primeira pessoa; os controles move-look tradicionais para esse gênero são implementados facilmente em todos os três tipos de entrada.

Para saber mais sobre controles e especificamente sobre os controles move-look, veja [Controles move-look para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md) e [Controles de toque para jogos](tutorial--adding-touch-controls-to-your-directx-game.md).

## <a name="common-control-behaviors"></a>Comportamentos comuns de controle


Os controles de toque e de mouse/teclado possuem uma implementação básica muito semelhante. Em um aplicativo UWP, um ponteiro é simplesmente um ponto na tela. Você pode movê-lo deslizando o mouse ou deslizando seu dedo na tela touch. Consequentemente, você pode se registrar para um único conjunto de eventos, sem se preocupar se o jogador usará um mouse ou uma tela sensível ao toque para mover e pressionar o ponteiro.

Ao ser inicializada, a classe **MoveLookController** no jogo de exemplo é registrada para quatro eventos específicos de ponteiro e um evento específico de mouse:

-   [**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278). O botão esquerdo ou direito do mouse foi pressionado (e mantido nessa posição) ou a superfície sensível ao toque foi tocada.
-   [**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276). O mouse foi movido ou uma ação de arrastar foi executada na superfície sensível ao toque.
-   [**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279). O botão esquerdo do mouse foi liberado ou o objeto em contato com a superfície sensível ao toque foi erguido.
-   [**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275). O ponteiro moveu-se para fora da janela principal.
-   [**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356). O mouse se moveu em uma determinada distância. Lembre-se de que só estamos interessados nos valores delta de movimento do mouse e não na atual posição x-y.

```cpp
void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}
```

O controlador Xbox é tratado separadamente usando-se as APIs [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053). Discutiremos a implementação dos controles do controlador de jogo mais adiante.

No jogo de exemplo, a classe **MoveLookController** tem três estados específicos de controlador, independentemente do tipo de controle:

-   **Nenhum**. Esse é o estado inicializado para o controlador. O jogo não está esperando nenhuma entrada do controlador.
-   **WaitForInput**. O jogo está em pausa e aguardando que o jogador continue.
-   **Ativo**. O jogo está em execução e processando a entrada do jogador.

O estado **Ativo** é o estado em que o jogador está jogando ativamente. Durante esse estado, a instância **MoveLookController** está processando eventos de entrada de todos os dispositivos de entrada habilitados e interpretando as intenções do jogador com base nos dados de evento agregados. Como resultado, ela atualiza a velocidade e a direção do olhar (o normal do plano de visão) do ponto de vista do jogador e compartilha os dados atualizados com o jogo depois que Update é chamado de dentro do loop do jogo.

Lembre-se de que o jogador pode executar mais de uma ação ao mesmo tempo. Por exemplo, ele pode estar disparando esferas enquanto move a câmera. Todas essas entradas são rastreadas no estado **Ativo** com diferentes IDs de ponteiro correspondendo a diferentes ações do ponteiro. Isso é necessário porque, do ponto de vista do jogador, um evento de ponteiro no retângulo de tiro é diferente de um evento de ponteiro no retângulo de movimento ou no restante da tela.

Quando um evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) é recebido, o **MoveLookController** obtém o valor do ID do ponteiro criado pela janela. O ID do ponteiro representa um tipo específico de entrada. Por exemplo, em um dispositivo multitoque, pode haver várias entradas ativas diferentes ao mesmo tempo. Os IDs são usados para rastrear a entrada que o jogador está usando. Se um dos eventos está no retângulo de movimento na tela sensível ao toque, um ID de ponteiro é atribuído para rastrear qualquer evento de ponteiro no retângulo de movimento. Outros eventos de ponteiro no retângulo de tiro são rastreados separadamente, com um ID de ponteiro separado. (Discutiremos isso mais detalhadamente na seção sobre controles de toque.)

A entrada de mouse tem outro ID e também é manipulada separadamente.

Depois que os eventos de ponteiro são mapeados para uma ação específica do jogo, é chegado o momento de atualizar os dados que o objeto **MoveLookController** compartilha com o loop principal do jogo.

Ao ser chamado, o método **Update** no exemplo de jogo processa a entrada e atualiza as variáveis de velocidade e direção do olhar (**m\_velocity** e **m\_lookdirection**), que são recuperadas em seguida pelo loop do jogo chamando-se os métodos públicos **Velocity** e **LookDirection** na instância **MoveLookController**.

```cpp
void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

Para determinar se o jogador está disparando, o loop do jogo pode executar um teste chamando o método **IsFiring** na instância **MoveLookController**. O **MoveLookController** verifica se o jogador pressionou o botão de disparo em um dos três tipos de entrada.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```

Se o jogador mover o ponteiro para fora da janela principal do jogo ou pressionar o botão de pausa (a tecla P ou o botão Iniciar do controlador Xbox), o jogo deverá ser colocado em pausa. O **MoveLookController** registra o pressionamento desse botão e informa o loop do jogo quando chama o método **IsPauseRequested**. Nesse ponto, se **IsPauseRequested** retorna **true**, o loop do jogo chama **WaitForPress** em **MoveLookController** para mover o controlador para o estado **WaitForInput**. Em seguida, **MoveLookController** espera que o jogador selecione um dos itens de menu para carregar, continuar ou sair do jogo e para de processar os eventos de entrada do jogo até que ele retorne ao estado **Ativo**.

Veja o [código de exemplo completo desta seção](#complete-sample-code-for-this-section).

Agora examinaremos a implementação de cada um dos três tipos de controle com um pouco mais de detalhe.

## <a name="implementing-relative-mouse-controls"></a>Implementando controles relativos do mouse


Se for detectado o movimento do mouse, usaremos esse movimento para determinar a nova arfagem e guinada da câmera. Fazemos isso implementando os controles relativos do mouse, onde manipulamos a distância relativa na qual o mouse se moveu (o delta entre o início do movimento e a parada) ao contrário da gravação das coordenadas de pixel x-y absolutas do movimento.

Para fazer isso, obtemos as alterações nas coordenadas X (o movimento horizontal) e Y (o movimento vertical) examinando os campos [**MouseDelta::X**](https://msdn.microsoft.com/library/windows/apps/hh758353) e **MouseDelta::Y** no objeto de argumento [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://msdn.microsoft.com/library/windows/apps/hh758358) retornado pelo evento [**MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356).

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="implementing-touch-controls"></a>Implementando controles de toque


Os controles de toque são os mais difíceis de desenvolver, porque são os mais complexos e exigem muitos ajustes finos para serem eficazes. No exemplo de jogo, um retângulo no quadrante inferior esquerdo da tela é usado como painel direcional, dentro do qual o deslizamento do seu polegar para a esquerda e para a direita move a câmera para a esquerda e a direita e o deslizamento do seu polegar para cima e para baixo move a câmera para a frente e para trás. Um retângulo no quadro inferior direito da tela pode ser pressionado para disparar as esferas. A mira (rotação sobre o eixo x e rotação sobre o eixo y) é controlada deslizando o seu dedo nas partes da tela não reservadas para movimento e disparo; a câmera (com a retícula de mira fixa) acompanha a direção de movimento do seu dedo.

Os retângulos de movimento e disparo são criados por dois métodos no exemplo de código:

```cpp
void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
 void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
```

Os eventos de ponteiro do dispositivo sensível ao toque em outras regiões da tela são tratados como comandos de visão. Se a tela for redimensionada, esses retângulos terão que ser recalculados (e redesenhados).

Quando um evento de ponteiro de dispositivo sensível ao toque é acionado em uma dessas regiões e o estado do jogo está definido como **Ativo**, um ID de ponteiro é atribuído ao evento, como discutimos anteriormente.

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        // ...
        // Game is paused, wait for click inside the game window.
        // ...
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            // ...
            // Handle mouse input here.
                                                // ...
            break;
        }
        break;
    }
    return;
}
```

Caso um evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) tenha ocorrido em uma das três regiões de controle, o retângulo de movimento, o retângulo de tiro ou o restante da tela (o controle de visão), o **MoveLookController** atribui a ID do ponteiro que disparou o evento a uma variável específica que corresponde à região da tela em que o evento foi gerado. Por exemplo, se o evento ocorreu no retângulo de movimento, a variável **m\_movePointerID** é definida com o ID do ponteiro que disparou o evento. Uma variável Booliana "em uso" (**m\_lookInUse**, no exemplo) também é definida para indicar que o controle ainda não foi liberado.

Agora examinaremos como o exemplo de jogo processa o evento de tela sensível ao toque [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276).

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            m_pitch = __max(-XM_PI / 2.0f, m_pitch);
            m_pitch = __min(+XM_PI / 2.0f, m_pitch);
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

O **MoveLookController** verifica a ID do ponteiro para determinar onde o evento ocorreu e executa uma das seguintes ações:

-   Se o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) ocorreu no retângulo de movimento ou de tiro, atualizar a posição do ponteiro do controlador.
-   Se o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) ocorreu em algum lugar do restante da tela (definido como a região dos controles de visão), calcular a alteração de rotação sobre o eixo x e rotação sobre o eixo y do vetor de direção do olhar.

Finalmente, examinaremos como o exemplo de jogo processa o evento de tela sensível ao toque [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

Se a ID do ponteiro que disparou o evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) é a ID do ponteiro de movimento registrado anteriormente, o **MoveLookController** define a velocidade como 0 porque o jogador parou de tocar o retângulo de movimento. Se a velocidade não fosse definida como 0, o jogador continuaria se movendo! Se você quiser implementar alguma forma de movimento inercial, é nesse ponto que adicionará o método que inicia o retorno à velocidade 0 sobre chamadas futuras a **Update** a partir do loop do jogo.

Do contrário, se o evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) foi disparado no retângulo de tiro ou na região de visão, o **MoveLookController** redefine as IDs de ponteiro específicas.

Esses são os aspectos básicos da implementação de controles de tela sensível ao toque no exemplo de jogo. Agora examinaremos os controles de mouse e teclado.

## <a name="implementing-mouse-and-keyboard-controls"></a>Implementando controles de mouse e teclado


O exemplo de jogo implementa estes controles de mouse e teclado:

-   As teclas W, S, A e D movem o ponto de vista do jogador para a frente, para trás, para a esquerda e para a direita, respectivamente. A tecla X e a barra de espaço movem o ponto de vista para cima e para baixo, respectivamente.
-   A tecla P faz uma pausa no jogo.
-   Ao mover o mouse, o jogador assume o controle da rotação (inclinação e movimento lateral) do ponto de vista da câmera.
-   Um clique no botão esquerdo dispara uma esfera.

Para usar o teclado, o exemplo de jogo registra-se para dois eventos adicionais: [**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) e [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270), que manipulam, respectivamente, o pressionamento e a liberação de uma tecla.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

O mouse é tratado de maneira ligeiramente diferente dos controles de toque, embora use um ponteiro. Obviamente, ele não usa os retângulos de movimento e tiro, porque isso seria impraticável para o jogador: como ele poderia pressionar os controles de movimento e disparo ao mesmo tempo? Conforme observado anteriormente, o controlador **MoveLookController** aciona os controles de visão sempre que o mouse é movido e os controles de tiro quando o botão esquerdo do mouse é pressionado, como aqui.

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until the button is released before setting the variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            else
            {

            }
            break;
        }

        break;
    }

    return;
}
```

Agora examinaremos como o exemplo de jogo processa o evento de mouse [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```

Quando o jogador para de pressionar um dos botões do mouse, a entrada está concluída: as esferas param de ser disparadas. Porém, como a visão sempre está habilitada, o jogo continua usando o mesmo ponteiro do mouse para rastrear os eventos de visão que ocorrem continuamente.

Agora examinaremos o último tipo de controle: o controlador Xbox. Ele é manipulado separadamente dos controles de toque e de mouse porque não usa o objeto de ponteiro.

## <a name="implementing-xbox-controller-controls"></a>Implementando controles de controlador Xbox


No exemplo de jogo, o suporte a controlador Xbox é adicionado por meio de chamadas às APIs [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053), que são um conjunto de APIs projetadas para simplificar a programação para controladores de jogo. No exemplo de jogo, usamos o joystick analógico esquerdo do controle do Xbox para o movimento do jogador, o joystick analógico direito para os controles de visão e o gatilho direito para disparar. Usamos o botão Iniciar para pausar e retomar o jogo.

O método **Update** na instância **MoveLookController** verifica imediatamente se há um controlador de jogo conectado e, em seguida, verifica o estado do controlador.

```cpp
void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // Device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // Device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

Se o controlador de jogo está no estado **Ativo**, esse método verifica se o usuário moveu o joystick analógico esquerdo em uma direção específica. Porém, o movimento do joystick em uma direção específica deve ser registrado como maior do que o raio da zona morta; caso contrário, nada acontecerá. O raio da zona morta é necessário para evitar os movimentos erráticos que ocorrem quando o controlador detecta movimentos mínimos do polegar do jogador em repouso sobre o joystick. Se não implantarmos essa zona morta, o jogador pode se aborrecer rapidamente, porque os controles lhe parecerão sensíveis demais.

O método **Update** executa a mesma verificação no joystick direito para determinar se o jogador alterou a direção de visão da câmera, contanto que o movimento no joystick seja maior que outro raio da zona morta.

**Update** calcula as novas rotações sobre os eixos x e y e, em seguida, verifica se o usuário pressionou o gatilho analógico direito, que é nosso botão de tiro.

E é assim que nosso exemplo implementa um conjunto completo de opções de controle. Novamente, lembre-se que um bom aplicativo UWP dá suporte a uma ampla gama de opções de controle, para que jogadores com diferentes fatores forma e dispositivos possam jogar da maneira que preferirem!

## <a name="next-steps"></a>Próximas etapas


Examinamos todos os principais componentes de um jogo DirectX UWP, exceto um: o áudio! A música e os efeitos sonoros são importantes em qualquer jogo. Portanto, discutiremos agora [como adicionar som](tutorial--adding-sound.md).

## <a name="complete-sample-code-for-this-section"></a>Código de exemplo completo desta seção


MoveLookController.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Uncomment to print debug tracing.
// #define MOVELOOKCONTROLLER_TRACE 1

enum class MoveLookControllerState
{
    None,
    WaitForInput,
    Active,
};

ref class MoveLookController
{
internal:
    MoveLookController();

    void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window
        );
    void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void WaitForPress(
        _In_ DirectX::XMFLOAT2 UpperLeft,
        _In_ DirectX::XMFLOAT2 LowerRight
        );
    void WaitForPress();

    void Update();
    bool IsFiring();
    bool IsPressComplete();
    bool IsPauseRequested();

    DirectX::XMFLOAT3 Velocity();
    DirectX::XMFLOAT3 LookDirection();
    float Pitch();
    void  Pitch(_In_ float pitch);
    float Yaw();
    void  Yaw(_In_ float yaw);
    bool  Active();
    void  Active(_In_ bool active);

    bool AutoFire();
    void AutoFire(_In_ bool AutoFire);

protected:
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerExited(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );
    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnMouseMoved(
        _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
        _In_ Windows::Devices::Input::MouseEventArgs^ args
        );

#ifdef MOVELOOKCONTROLLER_TRACE
    void DebugTrace(const wchar_t *format, ...);
#endif

private:
    // Properties of the controller object
    MoveLookControllerState     m_state;
    DirectX::XMFLOAT3           m_velocity;             // How far we move it this frame
    float                       m_pitch;
    float                       m_yaw;                  // Orientation euler angles in radians

    // Properties of the Move control
    DirectX::XMFLOAT2           m_moveUpperLeft;        // Bounding box where this control will activate
    DirectX::XMFLOAT2           m_moveLowerRight;
    bool                        m_moveInUse;            // The move control is in use.
    uint32                      m_movePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_moveFirstDown;        // The point where the initial contact occurred.
    DirectX::XMFLOAT2           m_movePointerPosition;  // The point where the move pointer is currently located.
    DirectX::XMFLOAT3           m_moveCommand;          // The net command from the move control.

    // Properties of the Look control
    bool                        m_lookInUse;            // The look control is in use.
    uint32                      m_lookPointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_lookLastPoint;        // The last point (from last frame)
    DirectX::XMFLOAT2           m_lookLastDelta;        // for smoothing.

    // Properties of the Fire control
    bool                        m_autoFire;
    bool                        m_firePressed;
    DirectX::XMFLOAT2           m_fireUpperLeft;        // The bounding box where this control will activate.
    DirectX::XMFLOAT2           m_fireLowerRight;
    bool                        m_fireInUse;            // The fire control is in use.
    UINT32                      m_firePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_fireLastPoint;        // The last fire position.

    // Properties of the Mouse control. This is a combination of Look and Fire.
    bool                        m_mouseInUse;
    uint32                      m_mousePointerID;
    DirectX::XMFLOAT2           m_mouseLastPoint;
    bool                        m_mouseLeftInUse;
    bool                        m_mouseRightInUse;

    bool                        m_buttonInUse;
    uint32                      m_buttonPointerID;
    DirectX::XMFLOAT2           m_buttonUpperLeft;
    DirectX::XMFLOAT2           m_buttonLowerRight;
    bool                        m_buttonPressed;
    bool                        m_pausePressed;

    // XBox Input related members
    bool                        m_isControllerConnected;  // Do we have a controller connected?
    XINPUT_CAPABILITIES         m_xinputCaps;             // The capabilites of the controller.
    XINPUT_STATE                m_xinputState;            // The current state of the controller.
    bool                        m_xinputStartButtonInUse;
    bool                        m_xinputTriggerInUse;

    // Input states for Keyboard
    bool                        m_forward;
    bool                        m_back;                    // States for movement
    bool                        m_left;
    bool                        m_right;
    bool                        m_up;
    bool                        m_down;
    bool                        m_pause;

private:
    void     ResetState();
    void     UpdateGameController();
};
```

MoveLookController.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MoveLookController.h"
#include "DirectXSample.h"

using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI;
using namespace Windows::Foundation;
using namespace Microsoft::WRL;
using namespace DirectX;
using namespace Windows::Devices::Input;
using namespace Windows::System;

#define ROTATION_GAIN 0.008f        // The sensitivity adjustment for the look controller.
#define MOVEMENT_GAIN 2.f           // The sensitivity adjustment for the move controller.

// A basic Move/Look Controller class such as in an FPS
// horizontal (x-z-plane) movement on left virtual joystick
// also supports WASD keyboard input
// steering and orientation via left mouse down or touch drag.

//----------------------------------------------------------------------

MoveLookController::MoveLookController():
    m_autoFire(true),
    m_isControllerConnected(false)
{
}

//----------------------------------------------------------------------
// Set up the controls supported by this controller.

void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}

//----------------------------------------------------------------------

bool MoveLookController::IsPauseRequested()
{
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        UpdateGameController();
        if (m_pausePressed)
        {

            m_pausePressed = false;
            return true;
        }
        else
        {
            return false;
        }
    }
    return false;
}

//----------------------------------------------------------------------

bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}

//----------------------------------------------------------------------

bool MoveLookController::IsPressComplete()
{
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        UpdateGameController();
        if (m_buttonPressed)
        {

            m_buttonPressed = false;
            return true;
        }
        else
        {
            return false;
        }
        break;
    }

    return false;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until button released before setting variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the point for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            else
            {

            }
            break;
        }

        break;
    }

    return;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move?
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            float limit = XM_PI / 2.0f - 0.01f;
            m_pitch = __max(-limit, m_pitch);
            m_pitch = __min(+limit, m_pitch);

            // Keep longitude in the same range by wrapping.
            if (m_yaw >  XM_PI)
            {
                m_yaw -= XM_PI * 2.0f;
            }
            else if (m_yaw < -XM_PI)
            {
                m_yaw += XM_PI * 2.0f;
            }
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseLeftInUse  = pointProperties->IsLeftButtonPressed;
            m_mouseRightInUse = pointProperties->IsRightButtonPressed;;
            m_mouseLastPoint = position;                            // save for next time through

            // Handle mouse movement via a separate relative mouse movement handler (OnMouseMoved).
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerExited(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position  = XMFLOAT2(pointerPosition.X, pointerPosition.Y);        // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = false;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseInUse = false;
            m_mousePointerID = 0;
            m_mouseLeftInUse = false;
            m_mouseRightInUse = false;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyDown(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // forward
        m_forward = true;
    if (Key == VirtualKey::S)       // back
        m_back = true;
    if (Key == VirtualKey::A)       // left
        m_left = true;
    if (Key == VirtualKey::D)       // right
        m_right = true;
    if (Key == VirtualKey::Space)   // up
        m_up = true;
    if (Key == VirtualKey::X)       // down
        m_down = true;
    if (Key == VirtualKey::P)       // Pause
        m_pause = true;
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyUp(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // Forward
        m_forward = false;
    if (Key == VirtualKey::S)       // Back
        m_back = false;
    if (Key == VirtualKey::A)       // Left
        m_left = false;
    if (Key == VirtualKey::D)       // Right
        m_right = false;
    if (Key == VirtualKey::Space)   // Up
        m_up = false;
    if (Key == VirtualKey::X)       // Down
        m_down = false;
    if (Key == VirtualKey::P)
    {
        if (m_pause)
        {
            // Trigger pause only one time on button release.
            m_pausePressed = true;
            m_pause = false;
        }
    }
}

//----------------------------------------------------------------------

void MoveLookController::ResetState()
{
    // Reset the state of the controller.
    // Disable any active pointer IDs to stop all interaction.
    m_buttonPressed = false;
    m_pausePressed = false;
    m_buttonInUse = false;
    m_moveInUse = false;
    m_lookInUse = false;
    m_fireInUse = false;
    m_mouseInUse = false;
    m_mouseLeftInUse = false;
    m_mouseRightInUse = false;
    m_movePointerID = 0;
    m_lookPointerID = 0;
    m_firePointerID = 0;
    m_mousePointerID = 0;
    m_velocity = XMFLOAT3(0.0f, 0.0f, 0.0f);

    m_xinputStartButtonInUse = false;
    m_xinputTriggerInUse = false;

    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_forward = false;
    m_back = false;
    m_left = false;
    m_right = false;
    m_up = false;
    m_down = false;
    m_pause = false;
}

//----------------------------------------------------------------------

void MoveLookController::SetMoveRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_moveUpperLeft  = upperLeft;
    m_moveLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::SetFireRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_fireUpperLeft  = upperLeft;
    m_fireLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress(
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{

    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft  = upperLeft;
    m_buttonLowerRight = lowerRight;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress()
{
    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft.x = 0.0f;
    m_buttonUpperLeft.y = 0.0f;
    m_buttonLowerRight.x = 0.0f;
    m_buttonLowerRight.y = 0.0f;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::Velocity()
{
    return m_velocity;
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::LookDirection()
{
    XMFLOAT3 lookDirection;

    float r = cosf(m_pitch);              // In the plane
    lookDirection.y = sinf(m_pitch);      // Vertical
    lookDirection.z = r * cosf(m_yaw);    // Fwd-back
    lookDirection.x = r * sinf(m_yaw);    // Left-right

    return lookDirection;
}

//----------------------------------------------------------------------

float MoveLookController::Pitch()
{
    return m_pitch;
}

//----------------------------------------------------------------------

void MoveLookController::Pitch(_In_ float pitch)
{
    m_pitch = pitch;
}

//----------------------------------------------------------------------

float MoveLookController::Yaw()
{
    return m_yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Yaw(_In_ float yaw)
{
    m_yaw = yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Active(_In_ bool active)
{
    ResetState();

    if (active)
    {
        m_state = MoveLookControllerState::Active;
        // Turn the mouse cursor off (hidden).
        CoreWindow::GetForCurrentThread()->PointerCursor = nullptr;
    }
    else
    {
        m_state = MoveLookControllerState::None;
        // Turn the mouse cursor on.
        auto window = CoreWindow::GetForCurrentThread();
        if (window)
        {
            // Protect case where there isn't a window associated with the current thread.
            // This happens on initialization.
            window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
        }
    }
}

//----------------------------------------------------------------------

bool MoveLookController::Active()
{
    if (m_state == MoveLookControllerState::Active)
    {
        return true;
    }
    else
    {
        return false;
    }
}

//----------------------------------------------------------------------

void MoveLookController::AutoFire(_In_ bool autoFire)
{
    m_autoFire = autoFire;
}

//----------------------------------------------------------------------

bool MoveLookController::AutoFire()
{
    return m_autoFire;
}

//----------------------------------------------------------------------

void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // leave 32 pixel-wide dead spot for being still
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45 deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear the movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}

//----------------------------------------------------------------------

void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // The device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // The device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // scale for control sensitivity
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <a name="related-topics"></a>Tópicos relacionados


[Criar um jogo UWP simples com o DirectX](tutorial--create-your-first-metro-style-directx-game.md)

 

 






---
title: Adicionar controles
description: Agora veremos como o jogo de exemplo implementa controles move-look em um jogo 3D e como desenvolver controles básicos de toque, mouse e controlador de jogo.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, controles, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 01c0d1361dcc0858a54792adc0d17408ed281c99
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8828675"
---
# <a name="add-controls"></a>Adicionar controles


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Um bom jogo da Plataforma Universal do Windows (UWP) dá suporte a uma ampla variedade de interfaces. Um jogador em potencial pode ter o Windows 10 em um tablet sem botões físicos, um computador com um controlador Xbox conectado, ou o desktop para jogos mais recente multimídia com alto desempenho mouse e teclado de jogos. No nosso jogo, os controles são implementados na classe [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp). Essa classe agrega todos os três tipos de entrada (mouse e teclado, touch e gamepad) em um único controlador. O resultado final é um jogo de tiros em primeira pessoa que usa controles move-look padrão no gênero que funcionam em vários dispositivos.

> [!NOTE]
> Para obter mais informações sobre controles, consulte [Controles move-look para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md) e [Controles de toque para jogos](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Objetivo

Neste ponto, temos um jogo que renderiza, mas não podemos mover nosso player ou atirar nos alvos. Vamos analisar como nosso jogo implementa controles move-look de jogos de tiros em primeira pessoa nos seguintes tipos de entrada em nosso jogo DirectX UWP.
- Mouse e teclado
- Touch
- Gamepad

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Comportamentos comuns de controle


Os controles de toque e de mouse/teclado possuem uma implementação básica muito semelhante. Em um aplicativo UWP, um ponteiro é simplesmente um ponto na tela. Você pode movê-lo deslizando o mouse ou deslizando seu dedo na tela touch. Consequentemente, você pode se registrar para um único conjunto de eventos, sem se preocupar se o jogador usará um mouse ou uma tela sensível ao toque para mover e pressionar o ponteiro.

Ao ser inicializada, a classe **MoveLookController** no jogo de exemplo é registrada para quatro eventos específicos de ponteiro e um evento específico de mouse:

Evento | Descrição
:------ | :-------
[**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) | O botão esquerdo ou direito do mouse foi pressionado (e mantido nessa posição) ou a superfície sensível ao toque foi tocada.
[**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) |O mouse foi movido ou uma ação de arrastar foi executada na superfície sensível ao toque.
[**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) |O botão esquerdo do mouse foi liberado ou o objeto em contato com a superfície sensível ao toque foi erguido.
[**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275) |O ponteiro moveu-se para fora da janela principal.
[**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) | O mouse se moveu em uma determinada distância. Lembre-se de que só estamos interessados nos valores delta de movimento do mouse e não na atual posição X-Y.


Esses manipuladores de eventos são definidos para iniciar a escuta de entrada do usuário assim que o **MoveLookController** é inicializado na janela do aplicativo.
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

O código completo de [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) pode ser visto no GitHub.


Para determinar quando o jogo deve escutar uma determinada entrada, a classe **MoveLookController** tem três estados específicos de controlador, independentemente do tipo de controlador:

Estado | Descrição
:----- | :-------
**Nenhum(a)** | Esse é o estado inicializado para o controlador. Todas as entradas são ignoradas desde que o jogo não esteja esperando nenhuma entrada do controlador.
**WaitForInput** | O controlador está esperando que o player confirme uma mensagem do jogo usando um clique esquerdo do mouse, um evento por toque ou o botão do mouse em um gamepad.
**Ativo** | O controlador está no modo de reprodução do jogo ativo.



### <a name="waitforinput-state-and-pausing-the-game"></a>Estado WaitForInput e pausar o jogo

O jogo insere o estado **WaitForInput** quando foi pausado. Isso ocorre quando o jogador move o ponteiro para fora da janela principal do jogo ou pressiona o botão de pausa (a tecla P ou o botão **Iniciar** do gamepad). O **MoveLookController** registra o pressionamento desse botão e informa o loop do jogo quando chama o método [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127). Nesse ponto, se **IsPauseRequested** retorna **verdadeiro**, o loop do jogo chama **WaitForPress** em **MoveLookController** para mover o controlador para o estado **WaitForInput**. 


Uma vez no estado **WaitForInput**, o jogo interrompe o processamento de quase todos os eventos de entrada do jogo até retornar ao estado **Ativo**. A exceção é o botão de pausa, que ao ser pressionado faz com que o jogo volte ao estado ativo. Diferentemente do botão de pausa, para que o jogo volte ao estado **ativo** o player precisa selecionar um item de menu. 



### <a name="the-active-state"></a>O estado Ativo

Durante o estado **Ativo**, a instância **MoveLookController** está processando eventos de todos os dispositivos de entrada habilitados e interpretando as intenções do jogador. Como resultado, ela atualiza a velocidade e a direção do olhar do ponto de vista do jogador e compartilha os dados atualizados com o jogo depois que **Update** é chamado de dentro do loop do jogo.


Todas as entradas do ponteiro são rastreadas no estado **Ativo** com diferentes IDs de ponteiro correspondendo a diferentes ações do ponteiro.
Quando um evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) é recebido, o **MoveLookController** obtém o valor da ID do ponteiro criado pela janela. O ID do ponteiro representa um tipo específico de entrada. Por exemplo, em um dispositivo multitoque, pode haver várias entradas ativas diferentes ao mesmo tempo. Os IDs são usados para rastrear a entrada que o jogador está usando. Se um dos eventos está no retângulo de movimento na tela sensível ao toque, uma ID de ponteiro é atribuída para rastrear qualquer evento de ponteiro no retângulo de movimento. Outros eventos de ponteiro no retângulo de tiro são rastreados separadamente, com uma ID de ponteiro separada.


> [!NOTE]
> A entrada do mouse e o botão direito de um gamepad também têm IDs que são manipuladas separadamente.

Depois que os eventos de ponteiro são mapeados para uma ação específica do jogo, é chegado o momento de atualizar os dados que o objeto **MoveLookController** compartilha com o loop principal do jogo.

Ao ser chamado, o método [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) no exemplo de jogo processa a entrada e atualiza as variáveis de velocidade e direção da observação (**m\_velocity** e **m\_lookdirection**), que são recuperadas em seguida pelo loop do jogo chamando-se os métodos públicos [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) e [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923).

> [!NOTE]
> Mais detalhes sobre o método [**Update**](#the-update-method) podem ser vistos posteriormente nesta página.




Para determinar se o jogador está disparando, o loop do jogo pode executar um teste chamando o método **IsFiring** na instância **MoveLookController**. O **MoveLookController** verifica se o jogador pressionou o botão de disparo em um dos três tipos de entrada.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
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









Agora examinaremos a implementação de cada um dos três tipos de controle com um pouco mais de detalhe.

## <a name="adding-relative-mouse-controls"></a>Adicionar controles relativos do mouse


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
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

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

## <a name="adding-touch-support"></a>Adicionar suporte a toque

Controles de toque são ótimos para dar suporte a usuários com tablets. Esse jogo reúne a entrada touch realizando o zoneamento de certas áreas da tela, com cada uma delas alinhando-se a ações específicas no jogo.
Essa entrada touch do jogo usa três zonas.

![layout de entrada move-look](images/simple-dx-game-controls-touchzones.png)

Os comandos a seguir resumem nosso comportamento de controle de toque.
Entrada do usuário | Ação
:------- | :--------
Retângulo de movimento | A entrada touch é convertida em um joystick virtual no qual o movimento vertical será traduzido em movimento de posição para frente/para trás e o movimento horizontal será traduzido em movimento de posição para esquerda/direita.
Retângulo de tiro | Dispare uma esfera.
Toque fora do retângulo de movimento e tiro | Altere a rotação (a inclinação e o eixo) do ponto de vista da câmera.

O **MoveLookController** verifica a ID do ponteiro para determinar onde o evento ocorreu e executa uma das seguintes ações:

-   Se o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) ocorreu no retângulo de movimento ou de tiro, atualizar a posição do ponteiro do controlador.
-   Se o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) ocorreu em algum lugar do restante da tela (definido como a região dos controles de visão), calcular a alteração de rotação sobre o eixo x e rotação sobre o eixo y do vetor de direção do olhar.


Após a implementação de nossos controles de toque, os retângulos que desenhamos anteriormente usando o Direct2D vão indicar aos players onde estão as zonas de movimento, tiro e observação.


![controles de toque](images/simple-dx-game-controls-showzones.png)


Agora vamos verificar como implementar cada controle.


### <a name="move-and-fire-controller"></a>Controlador de movimento e tiro
O retângulo do controlador de movimento no quadrante inferior esquerdo da tela é usado como um direcional. Deslizar seu polegar para a esquerda e para a direita nesse espaço move o player para a esquerda e para a direita, enquanto deslizar para cima e para baixo move a câmera para frente e para trás.
Após a configuração, tocar no controlador de tiro no quadrante inferior direito da tela dispara uma esfera.

Os métodos [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) e [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) criam nossos retângulos de entrada, usando dois vetores 2D para especificar posições do canto superior esquerdo e inferior direito de cada retângulo na tela. 


Os parâmetros então são atribuídos a **m\_fireUpperLeft** e **m\_fireLowerRight** que vão nos ajudar a determinar se o usuário está tocando dentro dos retângulos. 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

Se a tela for redimensionada, esses retângulos serão redesenhados para o tamanho apropriado.


Agora que já zoneamos nossos controles, é hora de determinar quando um usuário realmente os está utilizando.
Para fazer isso, definimos alguns manipuladores de eventos no método **MoveLookController::InitWindow** para quando o usuário pressiona, move ou libera o ponteiro.

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


Antes de tudo, vamos determinar o que acontece quando o usuário pressiona primeiro dentro dos retângulos de movimento e tiro por meio do método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313).
Neste ponto, verificamos onde eles estão tocando um controle e se um ponteiro já está nesse controlador. Se esse for o primeiro dedo a tocar o controle específico de toque, fazemos o seguinte:
- Armazene a localização da recepção em **m\_moveFirstDown** ou **m\_fireFirstDown** como um vetor 2D.
- Atribua a ID do ponteiro a **m\_movePointerID** ou **m\_firePointerID**.
- Defina o sinalizador **InUse** próprio (**m\_moveInUse** ou **m\_fireInUse**) como `true`, já que agora temos um ponteiro ativo para esse controle.


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

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
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
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
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


Agora que já determinamos se o usuário está tocando um controle de movimento ou tiro, verificamos se o player está fazendo qualquer movimento com o dedo pressionado.
Usando o método [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395), verificamos qual ponteiro foi movido e depois armazenamos sua nova posição como um vetor 2D.  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


Após o usuário ter feito seus gestos nos controles, ele liberará o ponteiro. Usando o método [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), determinamos qual ponteiro foi liberado e realizar uma série de redefinições.


Se o controle de movimento já foi liberado, fazemos o seguinte:
- Defina a velocidade do player como `0` em todas as direções para evitar que ele se mova no jogo.
- Alterne **m\_moveInUse** para `false`, desde que o usuário não esteja mais tocando o controle de movimento.
- Defina a ID do ponteiro de movimento como `0`, desde que não haja mais um ponteiro no controlador de movimento.

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


No caso do controle de tiro, se já foi liberado, apenas alternamos o sinalizador **m_fireInUse** para `false` e a ID do ponteiro de tiro para `0`, desde que não haja mais um ponteiro no controle de tiro.
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>Controlador de observação
Os eventos de ponteiro do dispositivo sensível ao toque nas regiões não usadas da tela são tratados como o controlador de observação. Deslizar o dedo por esta zona altera a inclinação e o eixo (rotação) da câmera do player.


Se o evento **MoveLookController::OnPointerPressed** for acionado em um dispositivo de toque nessa região e o estado do jogo estiver definido **Ativo**, uma ID de ponteiro é atribuída ao evento.

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
É possível ver o código completo do método **MoveLookController::OnPointerPressed** no [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259).




Aqui o **MoveLookController** atribui a ID de ponteiro do ponteiro que acionou o evento a uma variável específica correspondente à região de observação. No caso de um toque ocorre na região de observação, a variável **m\_lookPointerID** é definida como a ID do ponteiro que disparou o evento. Uma variável booliana, **m\_lookInUse**, também é definida para indicar que o controle ainda não foi liberado.

Agora examinaremos como o exemplo de jogo processa o evento de tela sensível ao toque [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276).


No método **MoveLookController::OnPointerMoved**, verificamos qual tipo de ID do ponteiro foi atribuído ao evento. Se for **m_lookPointerID**, calculamos a alteração na posição do ponteiro.
Depois usamos esse delta para calcular o quanto a rotação deve mudar. Por fim, estamos em um ponto no qual podemos atualizar o **m\_pitch** e **m\_yaw** para ser usado no jogo para alterar a rotação do player. 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





Finalmente, examinaremos como o exemplo de jogo processa o evento de tela sensível ao toque [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279).
Após o usuário ter finalizado o gesto de toque e removido o dedo da tela, [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) é iniciado.
Se a ID do ponteiro que disparou o evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) é a ID do ponteiro de movimento registrado anteriormente, o **MoveLookController** define a velocidade como `0` porque o jogador parou de tocar a área da observação.

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>Adicionar o mouse e suporte ao teclado


Este jogo tem o seguinte layout de controle para teclado e mouse.

Entrada do usuário | Ação
:------- | :--------
W | Mover o player para frente
A | Mover o player para esquerda
S | Mover o player para trás
D | Mover o player para direita
X | Mover visualização para cima
Barra de espaço | Mover visualização para baixo
P | Pausar o jogo
Movimento do mouse | Alterar a rotação (a inclinação e o eixo) do ponto de vista da câmera
Botão do mouse esquerdo | Disparar uma esfera


Para usar o teclado, o exemplo do jogo registra dois novos eventos, [**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) e [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270), no método [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88). Esses eventos controlam o pressionamento e a liberação de uma tecla.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

O mouse é tratado de maneira ligeiramente diferente dos controles de toque, embora use um ponteiro. Para se alinhar com nosso layout de controle, o **MoveLookController** gira a câmera sempre que o mouse é movido e dispara quando botão esquerdo do mouse é pressionado.


Isso é manipulado no método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) do **MoveLookController**.

Nesse método, verificamos que tipo de dispositivo de ponteiro está sendo usado com a enumeração [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType). Se o jogo estiver **Ativo** e o **PointerDeviceType** não for **Toque**, podemos presumir que é a entrada do mouse.

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
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
            break;
        }
```

Quando o player para de pressionar um dos botões do mouse, o evento de mouse [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) é acionado, chamando o método [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), e a entrada é concluída. Neste ponto, as esferas param de ser disparadas se o botão esquerdo do mouse estiver sendo pressionado e agora está liberado. Como a visão sempre está habilitada, o jogo continua usando o mesmo ponteiro do mouse para rastrear os eventos de visão que ocorrem continuamente.

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
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



Agora examinaremos o último tipo de controle ao qual daremos suporte: gamepads. Os gamepads são manipulados separadamente dos controles de toque e de mouse, já que eles não usam o objeto de ponteiro. Por isso, alguns novos manipuladores de eventos e métodos terão que ser adicionados.

## <a name="adding-gamepad-support"></a>Adicionar suporte a gamepad


No caso desse jogo, o suporte a gamepad é adicionado por chamadas às APIs [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input). Esse conjunto de APIs fornece acesso a entradas do controlador de jogo, como volantes de corrida e joysticks para simulador de voo. 


Estes serão nossos controles de gamepad.

Entrada do usuário | Ação
:------- | :--------
Joystick analógico esquerdo | Mover o player
Joystick analógico direito | Alterar a rotação (a inclinação e o eixo) do ponto de vista da câmera
Gatilho direito | Disparar uma esfera
Botão Iniciar/de menu | Pausar ou retomar o jogo




No método [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103), adicionamos dois novos eventos para determinar se um gamepad foi [adicionado](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) ou [removido](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114). Esses eventos atualizam a propriedade **m_gamepadsChanged**. Isso é usado no método **UpdatePollingDevices** para verificar se a lista de gamepads conhecidos foi alterada. 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

### <a name="the-updatepollingdevices-method"></a>O método UpdatePollingDevices

O método [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) da instância **MoveLookController** verifica imediatamente se um gamepad está conectado. Se estiver, começaremos a ler seu estado com [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Isso retorna a estrutura [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading), permitindo que verifiquemos quais botões foram clicados ou quais thumbsticks foram movidos.


Se o estado do jogo for **WaitForInput**, escutamos apenas o botão Iniciar/de menu do controlador para que o jogo possa ser retomado.


Se estiver **Ativo**, verificamos a entrada do usuário e determinamos qual ação no jogo precisa ocorrer.
Por exemplo, se o usuário moveu o joystick analógico esquerdo em uma direção específica, isso permite que o jogo saiba que precisamos mover o player na direção em que joystick está sendo movido. O movimento do joystick em uma direção específica deve ser registrado como maior do que o raio da **zona morta**; caso contrário, nada acontecerá. O raio da zona morta é necessário para evitar os movimentos erráticos que ocorrem quando o controlador detecta pequenos movimentos do polegar do jogador em repouso sobre o joystick. Sem zonas mortas, os controles podem parecer muito sensíveis ao usuário.


A entrada de thumbstick está entre -1 e 1 para os eixos x e y. A constante a seguir especifica o raio da zona morta do thumbstick.

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

Usando essa variável, começaremos a processar a entrada de thumbstick acionável. O movimento ocorreria com um valor de [-1, -0,26] ou [0,26, 1] nesses eixos.

![zona morta para thumbsticks](images/simple-dx-game-controls-deadzone.png)

Essa parte do método **UpdatePollingDevices** controla os thumbsticks esquerdo e direito.
Os valores X e Y de cada joystick são verificados para se descobrir se eles estão fora da zona morta. Se um ou os dois estiverem, atualizaremos o componente correspondente.
Por exemplo, se o thumbstick esquerdo estiver sendo movido para esquerda ao longo do eixo X, adicionaremos -1 ao componente **x** do vetor **m_moveCommand**. Esse vetor é o que será usado para agregar todos os movimentos em todos os dispositivos e será usado posteriormente para calcular onde o player deve se mover. 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

Da mesma maneira como o joystick esquerdo controla o movimento, o joystick direito controla a rotação da câmera.


O comportamento do thumbstick direito alinha-se com o comportamento do movimento do mouse em nossa configuração de mouse e teclado.
Se o joystick estiver fora da zona morta, calculamos a diferença entre a posição de ponteiro atual e o local onde o usuário agora está tentando a observação.
Essa alteração na posição do ponteiro (**pointerDelta**) é usada para calcular a inclinação e o eixo da rotação da câmera que são aplicados posteriormente em nosso método **Update**.
O vetor **pointerDelta** pode parecer familiar porque também é usado no método [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) para manter o controle de alteração na posição do ponteiro para nossas entradas de mouse e toque.


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
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
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Os controles do jogo não estariam completos sem a possibilidade de disparar esferas!


Esse método **UpdatePollingDevices** também verifica se o gatilho direito está sendo pressionado. Se estiver, nossa propriedade **m_firePressed** é mudada para verdadeiro, sinalizando para o jogo que as esferas devem começar a disparar.
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>O método Update

Para concluir, vamos analisar mais profundamente o método [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096).
Esse método mescla qualquer método ou rotação que o player fez com qualquer entrada com suporte para gerar um vetor de velocidade e atualizar nossos valores de inclinação e eixo para que o loop do jogo acesse.


O método **Update** começa chamando [**UpdatePollingDevices**](#the-updatepollingdevices-method) para atualizar o estado do controlador. Esse método também reúne qualquer entrada de um gamepad e adiciona seus movimentos ao vetor **m_moveCommand**. 

Em nosso método **Update**, realizamos as verificações de entrada a seguir.
- Se o player estiver usando o retângulo do controlador de movimento, determinaremos a alteração na posição do ponteiro e usaremos isso para calcular se o usuário moveu o ponteiro para fora da zona morta do controlador. Se estiver fora da zona morta, a propriedade do vetor **m_moveCommand** será atualizada com o valor do joystick virtual.
- Se qualquer uma das entradas de teclado de movimento estiver pressionada, um valor de `1.0f` ou `-1.0f` será adicionado ao componente correspondente no vetor **m_moveCommand** &mdash; `1.0f` para frente e `-1.0f` para trás.


Uma vez que todas as entradas de movimento foram consideradas, executamos o vetor **m_moveCommand** por meio de alguns cálculos para gerar outro vetor que represente a direção do player em relação ao mundo do jogo.
Também consideramos nossos movimentos em relação ao mundo e os aplicamos ao player como velocidade nessa direção.
Por fim, redefinimos o vetor **m_moveCommand** como `(0.0f, 0.0f, 0.0f)` para que tudo esteja pronto para o próximo quadro do jogo.


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
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
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>Próximas etapas

Agora que já adicionamos nossos controles, existe outro recurso que precisa ser adicionado para que criemos um jogo imersivo: o som!
A música e os efeitos sonoros são importantes em qualquer jogo. Portanto, discutiremos [como adicionar som](tutorial--adding-sound.md) a seguir.
 

 

 





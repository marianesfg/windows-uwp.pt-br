---
author: mtoepke
title: Controles move-look para jogos
description: "Aprenda a adicionar controles move-look tradicionais (também conhecidos como controles mouselook) usando o mouse e o teclado ao seu jogo do DirectX."
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7adbfdb77af6992be9448969f635bdebac58344b

---

# <span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>Controles move-look para jogos


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Aprenda a adicionar controles move-look tradicionais (também conhecidos como controles mouselook) usando o mouse e o teclado ao seu jogo do DirectX.

Também é discutido o suporte a move-look para dispositivos touch, com o controlador de movimento definido como a seção inferior esquerda da tela, que se comporta como uma entrada direcional, e o controlador de visão definido para o restante da tela, com a câmera centralizada no último lugar que o jogador tocou nessa área.

Se esse conceito de controle lhe parecer pouco familiar, pense nele da seguinte maneira: o teclado (ou a caixa de entrada direcional baseada no toque) controla suas pernas nesse espaço 3D, comportando-se como se suas pernas só pudessem se mover para a frente, para trás ou lateralmente para a esquerda e a direita. O mouse (ou o ponteiro de toque) controla a sua cabeça. Você usa sua cabeça para olhar em uma direção – para a esquerda, para a direita, para cima, para baixo ou para algum lugar dentro desse plano. Caso haja um alvo no seu campo de visão, você deve usar o mouse para centralizar o ponto de vista da câmera nesse alvo e então pressionar a tecla de avanço para se mover em sua direção ou a tecla de recuo para se afastar dele. Para girar em torno do alvo, você mantém o ponto de vista da câmera centralizado no alvo e, simultaneamente, move-se para a esquerda ou a direita. Como você pode perceber, esse é um método de controle extremamente eficaz para a navegação em ambientes 3D!

Em jogos, esses controles costumam ser chamados de controles WASD, nos quais as teclas W, S, A e D são usadas para movimentos da câmera fixos no plano x-z e o mouse é usado para controlar a rotação da câmera em torno dos eixos x e y.

## Objetivos


-   Adicionar controles move-look básicos ao seu jogo DirectX, tanto para mouse e teclado como para telas sensíveis ao toque.
-   Implementar uma câmera em primeira pessoa usada para navegar em um ambiente 3D.

## Uma observação sobre implementações de controle de toque


Para controles de toque, implementamos dois controladores: o controle de movimento, que se ocupa do movimento no plano x-z com relação ao ponto de vista da câmera; e o controlador de visão, que direciona o ponto de vista da câmera. O controlador de movimento é mapeado aos botões WASD do teclado e o controlador de visão é mapeado ao mouse. Entretanto, para controles de toque, precisamos definir uma região da tela que corresponda às entradas direcionais ou botões WASD virtuais, com o restante da tela servindo como espaço de entrada para os controles de visão.

Nossa tela tem a seguinte aparência.

![o layout do controlador move-look](images/movelook-touch.png)

Quando você move o ponteiro de toque (e não o mouse!) no canto inferior esquerdo da tela, qualquer movimento para cima fará a câmera mover para frente. Qualquer movimento para baixo faz a câmera mover-se para trás. O mesmo se aplica aos movimentos para a esquerda e para a direita dentro do espaço do ponteiro do controlador de movimento. Fora desse espaço, a tela torna-se um controlador de visão – você simplesmente toca ou arrasta a câmera para onde deseja olhar.

## Configurar a infraestrutura básica de eventos de entrada


Em primeiro lugar, temos que criar a classe de controle que usaremos para manipular os eventos de entrada provenientes do mouse e do teclado e atualizar a perspectiva da câmera com base nessa entrada. Como estamos implementando controles move-look, nós a chamamos de **MoveLookController**.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

Agora criaremos um cabeçalho para definir o estado do controlador move-look e de sua câmera em primeira pessoa, além dos manipuladores de eventos e métodos básicos que implementam os controles e atualizam o estado da câmera.

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
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

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

Nosso código contém quatro grupos de campos privados. Examinaremos a finalidade de cada um deles.

Primeiro, definimos alguns campos úteis para conter informações atualizadas sobre o ponto de vista da câmera.

-   **m\_position** é a posição da câmera (e, portanto, do plano de visão) na cena 3D, usando coordenadas da cena.
-   **m\_pitch** é a rotação sobre o eixo x da câmera, ou sua rotação para cima e para baixo em torno do eixo x do plano de visão, em radianos.
-   **m\_yaw** é a rotação sobre o eixo y da câmera, ou sua rotação para a esquerda e para a direita em torno do eixo y do plano de visão, em radianos.

Agora definiremos os campos que serão usados para armazenar informações sobre o status e a posição de nossos controladores. Primeiro definimos os campos necessários para o controlador de movimento baseado em toque. (A implementação do teclado como controlador de movimento não exige nada de especial. Simplesmente lemos os eventos de teclado com manipuladores específicos.)

-   **m\_moveInUse** indica se o controlador de movimento está em uso.
-   **m\_movePointerID** é o ID exclusivo do ponteiro de movimento atual. Nós o usamos para diferenciar o ponteiro de visão do ponteiro de movimento quando verificamos o valor do ID de ponteiro.
-   **m\_moveFirstDown** é o ponto na tela onde o jogador tocou inicialmente a área de ponteiro do controlador de movimento. Esse valor será usado posteriormente para definir uma zona morta a fim de evitar que movimentos minúsculos façam o ponto de vista oscilar.
-   **m\_movePointerPosition** é o ponto na tela para o qual o jogador moveu o ponteiro no momento. Nós o usamos para determinar a direção para a qual o jogador quis se mover examinando-a em relação a **m\_moveFirstDown**.
-   **m\_moveCommand** é o comando final calculado para o controlador de movimento: para cima (para a frente), para baixo (para trás), para a esquerda ou para a direita.

Agora definimos os campos que usaremos para o controlador de visão, nas implementações para mouse e touch.

-   **m\_lookInUse** indica se o controle de visão está em uso.
-   **m\_lookPointerID** é o ID exclusivo do ponteiro de visão atual. Nós o usamos para diferenciar o ponteiro de visão do ponteiro de movimento quando verificamos o valor do ID de ponteiro.
-   **m\_lookLastPoint** é o último ponto, em coordenadas da cena, que foi capturado no quadro anterior.
-   **m\_lookLastDelta** é a diferença calculada entre o atual **m\_position** e **m\_lookLastPoint**.

Finalmente, definimos seis valores booleanos para os seis graus de movimento, que usamos para indicar o estado atual de cada ação de movimento direcional (ativa ou inativa):

-   **m\_forward**, **m\_back**, **m\_left**, **m\_right**, **m\_up** e **m\_down**.

Usamos os seis manipuladores de eventos para capturar os dados de entrada empregados para atualizar o estado dos controladores:

-   **OnPointerPressed**. O jogador pressionou o botão esquerdo do mouse com o ponteiro na tela do jogo ou tocou na tela.
-   **OnPointerMoved**. O jogador moveu o mouse com o ponteiro na tela do jogo ou arrastou o ponteiro de toque na tela.
-   **OnPointerReleased**. O jogador liberou o botão esquerdo do mouse com o ponteiro na tela do jogo ou parou de tocar na tela.
-   **OnKeyDown**. O jogador pressionou uma tecla.
-   **OnKeyUp**. O jogador liberou uma tecla.

Finalmente, usamos esses métodos e propriedades para inicializar, acessar e atualizar as informações de estado dos controladores.

-   **Initialize**. O aplicativo chama esse manipulador de eventos para inicializar os controles e anexá-los ao objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que descreve a janela de exibição.
-   **SetPosition**. O aplicativo chama esse método para definir as coordenadas (x, y e z) dos controles no espaço da cena.
-   **SetOrientation**. O aplicativo chama esse método para definir a rotação sobre o eixo x e a rotação sobre o eixo y da câmera.
-   **get\_Position**. O aplicativo acessa essa propriedade para obter a posição atual da câmera no espaço da cena. Essa propriedade é usada como um método para comunicar a posição atual da câmera ao aplicativo.
-   **get\_LookPoint**. O aplicativo acessa essa propriedade para obter o ponto para o qual a câmera do controlador está voltada atualmente.
-   **Update**. Lê o estado dos controladores de movimento e visão e atualiza a posição da câmera. Esse método é chamado continuamente a partir do loop principal do aplicativo para atualizar os dados do controlador da câmera e a posição da câmera no espaço da cena.

Agora já temos todos os componentes necessários para implementar os controles move-look. Portanto, chegou o momento de conectar todas essas peças.

## Criar os eventos de entrada básicos


O dispatcher de eventos do Windows Runtime fornece cinco eventos que deverão ser manipulados por instâncias da classe **MoveLookController** para manipular:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

Esses eventos são implementados no tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Presumimos que você tenha um objeto **CoreWindow** para trabalhar. Se não souber como obter um, veja [Como configurar o seu aplicativo C++ da Plataforma Universal do Windows (UWP) para mostrar uma visualização DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Como esses eventos são acionados enquanto o aplicativo está sendo executado, os manipuladores atualizam as informações de estado dos controladores definidas nos campos privados.

Primeiro, preencheremos os manipuladores de eventos dos ponteiros de mouse e de toque. No primeiro manipulador de eventos, **OnPointerPressed()**, obtemos as coordenadas x-y do ponteiro do [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que gerencia a exibição quando o usuário clica o mouse ou toca a tela na região do controlador de visão.

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

Esse manipulador de eventos verifica se o ponteiro não é o mouse (para os fins deste exemplo, que dá suporte a mouse e toque) e se está na área do controlador de movimento. Caso ambos os critérios sejam verdadeiros, ele verifica se o ponteiro acabou de ser pressionado – especificamente, se esse clique não está relacionado a uma entrada de movimento ou visão anterior – testando se **m\_moveInUse** é false. Em caso afirmativo, o manipulador captura o ponto na área do controlador de movimento em que o pressionamento ocorreu e define **m\_moveInUse** como true para que, ao ser chamado novamente, esse manipulador não sobregrave a posição inicial da interação de entrada do controlador de movimento. Ele também atualiza a ID do ponteiro do controlador de movimento para a ID do ponteiro atual.

Caso o ponteiro seja o mouse ou o ponteiro de toque não esteja na área do controlador de movimento, ele deverá estar na área do controlador de visão. O manipulador define **m\_lookLastPoint** como a posição atual em que o usuário pressionou o botão do mouse, ou tocou e pressionou a tela, redefine o delta e atualiza a ID do ponteiro do controlador de visão para a ID do ponteiro atual. Além disso, define o estado do controlador de visão como ativo.

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

O manipulador de eventos **OnPointerMoved** é acionado sempre que o ponteiro se move; nesse caso, o ponteiro da tela sensível ao toque sendo arrastado ou o ponteiro do mouse sendo movido com o botão esquerdo pressionado. Se a ID do ponteiro for idêntico à ID do ponteiro do controlador de movimento, trata-se do ponteiro de movimento; caso contrário, verificamos se o ponteiro ativo é o controlador de visão.

Se for o controlador de movimento, simplesmente atualizamos a posição do ponteiro. Continuamos atualizando a posição enquanto o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) continuar sendo acionado, porque queremos comparar a posição final à primeira posição capturada com o manipulador de eventos **OnPointerPressed**.

Se for o controlador de visão, as coisas são um pouco mais complicadas. Como precisamos calcular um novo ponto de visão e centralizar a câmera nele, calculamos o delta entre o último ponto de visão e a posição atual da tela e o multiplicamos pelo nosso fator de escala, que pode ser ajustado para tornar os movimentos de visão menores ou maiores com relação à distância do movimento na tela. Usando esse valor, calculamos a inclinação longitudinal e a rotação.

Finalmente, temos que desativar os comportamentos do controlador de movimento ou de visão quando o jogador para de mover o mouse ou de tocar a tela. Usamos **OnPointerReleased**, que é chamado quando [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) é acionado, para definir **m\_moveInUse** ou **m\_lookInUse** como FALSE e interromper o movimento panorâmico da câmera e zerar a ID do ponteiro.

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

Até aqui, cuidamos de todos os eventos da tela touch. Agora cuidaremos dos eventos de entrada de tecla para um controlador de movimento baseado no teclado.

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

Enquanto uma dessas teclas permanece pressionada, esse manipulador de eventos define o estado do movimento direcional correspondente como verdadeiro.

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

Quando a tecla é liberada, esse manipulador de eventos define-o novamente como falso. Ao ser chamado, **Update**, verifica esses estados de movimento direcional e produz o movimento correspondente da câmera. Isso é um pouco mais simples do que a implementação de toque.

## Inicializar os controles de toque e o estado dos controladores


Agora conectaremos os eventos e inicializaremos todos os campos de estado de controladores.

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to recieve touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** recebe uma referência à instância de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) do aplicativo como um parâmetro e registra os manipuladores de eventos que desenvolvemos para os eventos apropriados nessa **CoreWindow**. Ele inicializa os IDs dos ponteiros de movimento e visão, zera o vetor de comando para nossa implementação do controlador de movimento de tela sensível ao toque e coloca a câmera voltada diretamente para a frente quando o aplicativo é iniciado.

## Obtendo e definindo a posição e a orientação da câmera


Definiremos alguns métodos para obter e definir a posição da câmera em relação ao visor.

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## Atualizando as informações de estado do controlador


Agora efetuamos os cálculos para converter as informações de coordenadas do ponteiro rastreadas em **m\_movePointerPosition** em novas informações de coordenadas relativas ao sistema de coordenadas do nosso mundo. Nosso aplicativo chama esse método cada vez que seu loop principal é atualizado. Portanto, é aqui que calculamos as informações de posição do novo ponto de visão que queremos passar ao aplicativo para que a matriz de visualização seja atualizada antes da projeção no visor.

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

Como não queremos movimentos erráticos quando o jogador usar o controlador de movimento baseado em toque, definimos uma zona morta virtual com diâmetro de 32 pixels em torno do ponteiro. Também adicionamos velocidade, que é o valor do comando mais um taxa de incremento do movimento. (Você pode ajustar esse comportamento como preferir, para reduzir ou acelerar a taxa de movimento com base na distância percorrida pelo ponteiro na área do controlador de movimento.)

Quando calculamos a velocidade, também traduzimos as coordenadas recebidas dos controladores de movimento e visão para o movimento do ponto de visão real que enviamos ao método responsável pelo cálculo da matriz de visualização da cena. Primeiro invertemos a coordenada x, porque quando clicamos-movemos ou arrastamos para a esquerda ou para a direita com o controlador de visão, o ponto de visão gira em direção oposta à cena, como na rotação de uma câmera sobre o seu eixo central. Em seguida, trocamos os eixos y e z, porque o pressionamento de uma tecla para cima/para baixo ou um movimento de tocar e arrastar (lidos como um comportamento no eixo y) no controlador de movimento devem se traduzir em uma ação da câmera que move o ponto de visão para perto ou para longe da tela (o eixo z).

A posição final do ponto de visão para o jogador é a última posição mais a velocidade calculada, e é isso que é lido pelo renderizador quando chama o método **get\_Position** (muito provavelmente durante a configuração para cada quadro). Depois disso, redefinimos o comando de movimento para zero.

## Atualizando a matriz de visualização com a nova posição da câmera


Podemos obter a coordenada do espaço da cena em que a câmera está focalizada, que é atualizada com a frequência programada no aplicativo (a cada 60 segundos no loop principal do aplicativo, por exemplo). Este pseudocódigo sugere o comportamento de chamada que você pode implementar:

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

Parabéns! Você implementou controles move-look básicos para telas sensíveis ao toque e controles de toque para entrada via teclado/mouse em seu jogo.

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 







<!--HONumber=Jun16_HO4-->



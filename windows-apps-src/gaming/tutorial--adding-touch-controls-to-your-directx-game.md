---
title: Controles de toque para jogos
description: Aprenda a adicionar controles de toque básicos ao seu jogo C++ da Plataforma Universal do Windows (UWP) com DirectX.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, toque, controles, directx, entrada
ms.localizationpriority: medium
ms.openlocfilehash: e8892219b485d320bb77f90ac0d172e8e2403392
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618731"
---
# <a name="touch-controls-for-games"></a>Controles de toque para jogos



Aprenda a adicionar controles de toque básicos ao seu jogo C++ da Plataforma Universal do Windows (UWP) com DirectX. Mostraremos como adicionar controles baseados em toque para mover uma câmera com plano fixo em um ambiente Direct3D, no qual arrastar com o dedo ou a caneta muda a perspectiva da câmera.

Você pode incorporar esses controles a jogos nos quais deseja que o jogador arraste para rolar ou ter uma visão panorâmica de um ambiente 3D, como um mapa ou espaço de jogo. Por exemplo, em um jogo de estratégia ou quebra-cabeças, você pode usar esses controles para permitir que o jogador veja um ambiente de jogo maior do que a tela realizando um movimento panorâmico para a esquerda ou direita.

> **Observação**  nosso código também funciona com controles de panorâmica baseados no mouse. Os eventos relacionados a ponteiros são abstraídos pelas APIs do Windows Runtime para que possam lidar com eventos de ponteiro baseados em toque ou mouse.

 

## <a name="objectives"></a>Objetivos


-   Criar um controle simples de arraste por toque para realizar o movimento panorâmico de uma câmera com plano fixo em um jogo em DirectX.

## <a name="set-up-the-basic-touch-event-infrastructure"></a>Configurar a infraestrutura básica de eventos por toque


Primeiro, definimos o tipo de controlador básico **CameraPanController**, neste caso. Aqui, definimos um controlador como uma ideia abstrata, o conjunto de comportamentos que o usuário pode desempenhar.

A classe **CameraPanController** é uma coleção de informações (atualizadas regularmente) sobre o estado do controlador da câmera, permitindo que seu aplicativo consiga essas informações de seu loop de atualizações.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

Agora, vamos criar um cabeçalho que define o estado do controlador da câmera, além dos métodos básicos e manipuladores de eventos que implementam as interações com o controlador da câmera.

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

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

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

Os campos privados contêm o estado atual do controlador da câmera. Vamos revisá-los.

-   **m\_posição** é a posição da câmera no espaço de cena. Neste exemplo, o valor da coordenada z está fixo em 0. Podemos usar DirectX::XMFLOAT2 para representar esse valor, mas para os fins desta amostra e de extensibilidade futura, usamos DirectX::XMFLOAT3. Podemos passar esse valor por meio de **Obtenha\_posição** propriedade para o aplicativo em si para que ele possa atualizar o visor adequadamente.
-   **m\_panInUse** é um valor booliano que indica se uma operação de panorâmica estiver ativa; ou, mais especificamente, se o jogador está tocar na tela e mover a câmera.
-   **m\_panPointerID** é uma ID exclusiva para o ponteiro. Não o usaremos nesta amostra, mas recomendamos associar a classe de estado do controlador a um ponteiro específico.
-   **m\_panFirstDown** é o ponto na tela em que o player primeiro tocadas a tela ou clicou o mouse durante a ação de panorâmica da câmera. Usaremos esse valor mais adiante para definir uma zona morta a fim de evitar distorções quando a tela for tocada, ou se o mouse balançar um pouco.
-   **m\_panPointerPosition** é o ponto na tela onde o jogador atualmente foi movido para o ponteiro. Podemos usá-lo para determinar qual direção o player desejava mover examinando-a relação à **m\_panFirstDown**.
-   **m\_panCommand** é o comando final computado para o controlador de câmera: backup, para baixo, para a esquerda ou direita. Como estamos trabalhando com uma câmera fixa no plano x-y, esse valor pode ser DirectX::XMFLOAT2.

Usamos estes três manipuladores de eventos para atualizar as informações de estado do controlador da câmera.

-   **OnPointerPressed** é um manipulador de evento chamado pelo aplicativo quando os jogadores pressionam o dedo sobre a superfície de toque e o ponteiro é movido para as coordenadas do ponto pressionado.
-   **OnPointerMoved** é um manipulador de evento chamado pelo aplicativo quando o jogador desliza um dedo pela superfície de toque. Ele é atualizado com as novas coordenadas do caminho de arraste.
-   **OnPointerReleased** é um manipulador de evento chamado pelo aplicativo quando o jogador retira o dedo da superfície de toque.

Por fim, usamos estes métodos e propriedades para iniciar, acessar e atualizar as informações de estado do controlador da câmera.

-   **Initialize** é um manipulador de evento chamado pelo aplicativo para iniciar os controles e associá-los ao objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que descreve sua janela de exibição.
-   **SetPosition** é um método chamado pelo aplicativo para definir as coordenadas (x,y e z) dos controles no espaço da cena. Observe que as coordenadas z são 0 neste tutorial.
-   **Obtenha\_posição** é uma propriedade que nosso aplicativo acessa para obter a posição atual da câmera no espaço de cena. Use essa propriedade como meio de comunicar o aplicativo a respeito da posição atual da câmera.
-   **Obtenha\_FixedLookPoint** é uma propriedade que nosso aplicativo acessa para obter o ponto atual em direção ao qual a câmera do controlador está enfrentando. Neste exemplo, ela está bloqueada em um vetor normal em relação ao plano x-y.
-   **Update** é um método que lê o estado do controlador e atualiza a posição da câmera. Você chama esse &lt;algo&gt; constantemente do loop principal do aplicativo para atualizar os dados do controlador e a posição da câmera no espaço da cena.

Agora, você tem todos os componentes necessários para implementar os controles de toque. Você pode detectar onde e quando os eventos de toque ou ponteiro do mouse ocorrerão e qual foi a ação. Você pode definir a posição e a orientação da câmera em relação ao espaço da cena, além de controlar as alterações. Por fim, você pode informar a nova posição da câmera ao aplicativo que fez a chamada.

Agora, vamos juntar todos os pedaços.

## <a name="create-the-basic-touch-events"></a>Criar eventos de toque básicos


O dispatcher de eventos do Windows Runtime fornece três eventos com os quais queremos que nosso aplicativo lide:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

Esses eventos são implementados no tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Presumimos que você tenha um objeto **CoreWindow** para trabalhar. Para obter mais informações, veja [Como configurar o seu aplicativo C++ UWP para mostrar uma visualização DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Como esses eventos são acionados durante a execução do aplicativo, os manipuladores atualizam as informações de estado do controlador da câmera definidas nos campos privados.

Primeiro, vamos preencher os manipuladores de eventos de ponteiro de toque. No primeiro manipulador de evento, **OnPointerPressed**, obtemos as coordenadas x-y do ponteiro na [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que gerencia a exibição quando o usuário toca na tela ou clica no mouse.

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

Podemos usar esse manipulador para permitir que o atual **CameraPanController** instância saber esse controlador de câmera deve ser tratado como ativo, definindo **m\_panInUse** como TRUE. Desse modo, quando o aplicativo chama **Update**, ele usa os dados da posição atual para atualizar o visor.

Agora que estabelecemos os valores de base para o movimento da câmera quando o usuário toca na tela ou pressiona a janela de exibição com um clique, devemos determinar o que fazer quando o usuário arrasta a tela ou move o ouse com o botão pressionado.

O manipulador de eventos **OnPointerMoved** é acionado sempre que o ponteiro se move, a cada "tique" que o jogador arrasta na tela. Precisamos manter o aplicativo a par da localização atual do ponteiro, e isso é feito desse modo.

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

Por fim, precisamos desativar o comportamento de movimento panorâmico da câmera quando o jogador para de tocar na tela. Usamos **OnPointerReleased**, que é chamado quando [ **PointerReleased** ](https://msdn.microsoft.com/library/windows/apps/br208279) é acionado, para definir **m\_panInUse** como FALSE e desativar o movimento de panorâmica da câmera e defina a ID de ponteiro como 0.

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Inicializar os controles de toque e o estado dos controladores


Vamos associar os eventos e iniciar todos os campos básicos de estado do controlador da câmera.

**inicializar**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** recebe uma referência à instância de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) do aplicativo como um parâmetro e registra os manipuladores de eventos que desenvolvemos para os eventos apropriados nessa **CoreWindow**.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>Obtendo e definindo a posição do controlador da câmera


Vamos definir alguns métodos para obter e definir a posição do controlador da câmera no espaço da cena.

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** é um método público que podemos chamar a partir do aplicativo quando precisamos definir a posição do controlador da câmera para um ponto específico.

**Obtenha\_posição** é nossa propriedade pública mais importante: é a maneira de nosso aplicativo obtém a posição atual do controlador de câmera no espaço de cena para que ele possa atualizar o visor adequadamente.

**Obtenha\_FixedLookPoint** é uma propriedade pública que, neste exemplo, obtém um ponto de aparência que é normal para o plano x-y. Você pode alterar esse método para usar funções trigonométricas, senos e cossenos ao calcular os valores das coordenadas x, y e z quando quiser criar ângulos mais oblíquos para a câmera fixa.

## <a name="updating-the-camera-controller-state-information"></a>Atualizando as informações de estado do controlador da câmera


Agora, podemos executar nossos cálculos que convertem as informações de coordenadas do ponteiro rastreadas em **m\_panPointerPosition** em novas informações de coordenadas respectivas de nosso espaço de cena 3D. Nosso aplicativo chama esse método cada vez que seu loop principal é atualizado. Nele, calculamos as novas informações de posição que queremos passar ao aplicativo, que são usadas para atualizar a matriz de visualização antes da projeção no visor.

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

Como não queremos causar distorções com toque ou mouse (o que tornaria o movimento panorâmico da câmera irregular), definimos uma zona morta em torno do ponteiro com um diâmetro de 32 pixels. Também temos um valor de velocidade, que, neste caso, é 1:1, com o pixel transversal do ponteiro além da zona morta. Você pode ajustar esse comportamento para desacelerar ou acelerar a taxa de movimento.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Atualizando a matriz de visualização com a nova posição da câmera


Agora, podemos obter uma coordenada do espaço da cena na qual a câmera está focada, e que é atualizada sempre que você informa ao aplicativo para fazer isso (a cada 60 segundos no loop principal do aplicativo, por exemplo). Este pseudocódigo sugere o comportamento de chamada que você pode implementar:

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

Parabéns! Você implementou um conjunto simples de controles de movimento panorâmico da câmera de toque no seu jogo.


 

 

 





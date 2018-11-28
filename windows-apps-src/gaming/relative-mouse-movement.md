---
title: Movimento relativo do mouse
description: Use os controles relativos de mouse, que não usam o cursor do sistema e não retornam coordenadas absolutas da tela, e rastreiam o delta de pixel entre os movimentos do mouse em jogos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, mouse, entrada
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: 71985841e6c0fa764201c179fb12408581823e5e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966216"
---
# <a name="relative-mouse-movement-and-corewindow"></a>Movimento relativo do mouse e CoreWindow

Em jogos, o mouse é uma opção de controle comum que é familiar para muitos jogadores, além de ser essencial para muitos gêneros de jogos, incluindo atiradores em primeira e terceira pessoas, e jogos de estratégia em tempo real. Aqui discutimos a implementação de controles relativos de mouse, que não usam o cursor do sistema e não retornam coordenadas absolutas da tela; em vez disso, rastreiam o delta de pixel entre os movimentos do mouse.

Alguns apps, como jogos, usam o mouse como um dispositivo de entrada mais geral. Por exemplo, um modelador 3D pode usar a entrada de mouse para orientar um objeto 3D simulando um trackball virtual; ou um jogo pode usar o mouse para mudar a direção da câmera de visualização por meio de controles de mouse-look. 

Nesses cenários, o aplicativo requer dados relativos do mouse. Os valores relativos do mouse representam a distância que o mouse se moveu desde o último quadro, em vez de valores absolutos da coordenada x-y em uma janela ou tela. Além disso, os aplicativos geralmente ocultam o cursor do mouse desde que a posição do cursor com relação às coordenadas da tela não seja relevante durante a manipulação de um objeto ou cena 3D. 

Quando o usuário coloca o aplicativo em um modo de manipulação relativa de objeto/cena 3D, o aplicativo deve: 
- Ignorar a manipulação padrão do mouse.
- Habilitar a manipulação relativa do mouse.
- Ocultar o cursor do mouse definindo-o como ponteiro nulo (nullptr). 

Quando o usuário retira o aplicativo de um modo de manipulação relativa de objeto/cena 3D, o aplicativo deve: 
- Habilitar a manipulação padrão/absoluta do mouse.
- Desativar a manipulação relativa do mouse. 
- Definir o cursor do mouse como um valor que não seja nulo (o que o torna visível).

> **Observação**  
Com esse padrão, o local do cursor do mouse absoluto é preservado ao entrar no modo relativo sem cursor. O cursor reaparece no mesmo local da coordenada da tela como era antes da habilitação do modo de movimento relativa do mouse.

 

## <a name="handling-relative-mouse-movement"></a>Manipulando o movimento relativo do mouse


Para acessar os valores delta relativos do mouse, registre o evento [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx) conforme mostrado aqui.


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

O manipulador de eventos neste exemplo de código, **OnMouseMoved**, renderiza a visualização com base nos movimentos do mouse. A posição do ponteiro do mouse é transmitida para o manipulador como um objeto [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx). 

Pule o processamento de dados absolutos do mouse do evento [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) quando o aplicativo mudar para manipular valores de movimentos relativos do mouse. Entretanto, só pule essa entrada se o evento **CoreWindow::PointerMoved** tiver ocorrido como resultado da entrada do mouse (em oposição à entrada por toque). O cursor é ocultado ao definir o [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) como **nullptr**. 

## <a name="returning-to-absolute-mouse-movement"></a>Retornando para o movimento absoluto do mouse

Quando o aplicativo sai do modo de manipulação de objeto ou cena 3D e deixa de usar o movimento relativo do mouse (como quando retorna para uma tela de menu), retorna para o processamento normal do movimento absoluto do mouse. Neste momento, para de ler os dados relativos do mouse, reinicia o processamento de eventos do mouse padrão (e ponteiro) e define o [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) como um valor que não é nulo. 

> **Observação**  
Quando o aplicativo está no modo de manipulação de objeto/cena 3D (processamento de movimentos relativos do mouse com o cursor desativado), o mouse não pode chamar a interface do usuário da borda como os botões, pilha voltar ou barra de aplicativos. Portanto, é importante fornecer um mecanismo para sair desse modo específico, como a tecla **Esc** normalmente usada.

## <a name="related-topics"></a>Tópicos relacionados

* [Controles move-look para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [Controles de toque para jogos](tutorial--adding-touch-controls-to-your-directx-game.md)
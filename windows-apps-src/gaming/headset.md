---
title: Fone de ouvido
description: Use as APIs de fone de ouvido Windows.Gaming.Input para detectar fones de ouvido, capturar a voz do jogador e reproduzir áudio.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, fone de ouvido
ms.localizationpriority: medium
ms.openlocfilehash: b3de68cc59c9928a52eba5caeb840e9e825eecf0
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7835251"
---
# <a name="headset"></a>Fone de ouvido

Esta página descreve as noções básicas de programação para fones de ouvido que usam [Windows.Gaming.Input.Headset][headset] e APIs relacionadas para a Plataforma Universal do Windows (UWP).

Ao ler esta página, você saberá como:
* Acessar um fone de ouvido conectado a um dispositivo de entrada ou de navegação
* Detectar se um fone de ouvido foi conectado ou desconectado


## <a name="headset-overview"></a>Visão geral do fone de ouvido

Fones de ouvido são dispositivos de captura e reprodução de áudio muito usados para se comunicar com outros jogadores em jogos online, mas também podem ser usados no jogo ou para outros usos criativos. Fones de ouvido são compatíveis em aplicativos UWP do Windows 10 e do Xbox pelo namespace [Windows.Gaming.Input][].


## <a name="detect-and-track-headsets"></a>Detectar e acompanhar fones de ouvido

Os fones de ouvido são gerenciados pelo sistema. Portanto, você não precisa criar ou inicializá-los. O sistema dá acesso a um fone de ouvido por meio do dispositivo de entrada conectado e dos eventos para notificar você quando um fone de ouvido está conectado ou desconectado.

### <a name="igamecontrollerheadset"></a>IGameController.Headset

Todos os dispositivos de entrada no namespace [Windows.Gaming.Input][] implementam a interface [IGameController][] que define a propriedade [Headset][igamecontroller.headset] a ser o fone de ouvido conectado atualmente ao dispositivo.

### <a name="connecting-and-disconnecting-headsets"></a>Como conectar e desconectar fones de ouvido.

Quando um fone de ouvido está conectado ou desconectado, os eventos [HeadsetConnected][igamecontroller.headsetconnected] e [HeadsetDisconnected][igamecontroller.headsetdisconnected] são acionados. É possível registrar manipuladores para esses eventos a fim de acompanhar se um dispositivo de entrada tem atualmente um fone de ouvido conectado.

O exemplo a seguir mostra como registrar um manipulador para o evento `HeadsetConnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

O exemplo a seguir mostra como registrar um manipulador para o evento `HeadsetDisconnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>Como usar o fone de ouvido

A classe [Headset][] é composta de duas cadeias de caracteres que representam IDs de ponto de extremidade XAudio – uma para captura de áudio (gravação do fone de ouvido com microfone) e uma para renderização de áudio (reprodução por meio do fone de ouvido).

Os detalhes do trabalho com XAudio não são abordados aqui. Para obter mais informações, consulte o [Guia de programação do XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) e a [Referência de API do XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx).


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx

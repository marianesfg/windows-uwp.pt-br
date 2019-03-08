---
title: Recuperação de usuário do sistema Windows na UWP
description: Saiba como recuperar o usuário do sistema Windows em um jogo de plataforma Universal do Windows (UWP).
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, o usuário do sistema
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632751"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>Recuperando o usuário do sistema Windows em um título de plataforma Universal do Windows (UWP)

## <a name="windowssystemuser"></a>Windows.System.User

Um [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) objeto representa um usuário do Windows local. No console do Xbox, ele permite que vários usuário do windows ao ser conectado ao mesmo tempo em uma única sessão interativa, se seu aplicativo é um aplicativo de vários usuário, em seguida, um objeto Windows.System.User seria necessário para construir um XboxLiveUser para acessar serviços em tempo real. Em outras plataformas windows, como o PC ou telefone, ele tem apenas permitir que um usuário do windows em um único dispositivo ou uma sessão interativa, passando um Windows.System.User para construir um XboxLiveUser não é necessário.

## <a name="ways-to-retrieve-windows-system-user"></a>Maneiras de recuperar o usuário do sistema Windows

* **Usando métodos estáticos de [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) classe.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) classe fornece um conjunto de método estático para ajudar a recuperar objetos Windows.System.User. Por exemplo, você pode chamar FindAllAsync para obter todos os usuários do Active Directory do windows.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) classe fornece métodos para iniciar o selecionador de usuário da interface do usuário e selecione um usuário do sistema windows em cenários de vários usuários.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) é a interface principal para todos os dispositivos de controlador (gamepad, roda de corrida, pen drive de voo etc.). Você pode obter o objeto de Windows.System.User associado com o controlador de jogo chamando sua propriedade de usuário.  

  Aqui estão o controlador de jogo alguns implementado pelo windows [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), [Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad), [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel).

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  Você pode chamar o método estático FindUserFromDeviceId para localizar o usuário associado com a id do dispositivo. Você geralmente pode obter a id do dispositivo de eventos de entrada do windows, como [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)

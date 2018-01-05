---
author: drewbatgit
ms.assetid: 
description: "Mostra como gerenciar a transmissão de jogo de um aplicativo UWP."
title: "Gerenciar transmissão de jogo"
ms.author: drewbat
ms.date: 09/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, jogo, transmissão"
ms.localizationpriority: medium
ms.openlocfilehash: 613dd69c00257ac5d750bc67b174d7ff010e0b59
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-game-broadcasting"></a>Gerenciar transmissão de jogo
Este artigo mostra como gerenciar a transmissão de jogo de um aplicativo UWP. Os usuários devem iniciar a transmissão usando a interface do usuário do sistema integrada ao Windows, mas, a partir do Windows 10, versão 1709, os apps poderão iniciar a interface do usuário de transmissão do sistema e receber notificações quando a transmissão for iniciada e interrompida.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Adicionar as Extensões de Área de Trabalho do Windows para a UWP ao app
As APIs para gerenciamento de transmissão de app, encontradas no namespace **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)**, não estão incluídas no contrato de API Universal. Para acessar as APIs, você deve adicionar ao app uma referência às Extensões de Área de Trabalho do Windows para a UWP executando as etapas a seguir.

1. No Visual Studio, em **Gerenciador de Soluções**, expanda o projeto UWP, clique com botão direito do mouse em **Referências** e selecione **Adicionar Referência...**. 
2. Expanda o nó **Universal do Windows** e selecione **Extensões**.
3. Na lista de extensões, marque a caixa de seleção ao lado da entrada ***Extensões de Área de Trabalho do Windows para a UWP** que corresponde à compilação de destino do projeto. Para os recursos de transmissão de app, a versão deve ser 1709 ou superior.
4. Clique em **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Iniciar a interface do usuário do sistema para permitir que o usuário inicie a transmissão
O app pode não ser capaz de realizar a transmissão por vários motivos, inclusive se o dispositivo atual não atender aos requisitos de hardware para transmissão ou se outro app estiver realizando a transmissão. Antes de iniciar a interface do usuário do sistema, verifique se o app é capaz de realizar a transmissão. Primeiro, verifique se as APIs de transmissão estão disponíveis no dispositivo atual. As APIs não estão disponíveis em dispositivos que executam uma versão do sistema operacional anterior ao Windows 10, versão 1709. Em vez de procurar uma versão específica de sistema operacional, use o método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsApiContractPresent_System_String_System_UInt16_System_UInt16_)** para procurar o *Windows.Media.AppBroadcasting.AppBroadcastingContract* versão 1.0. Se esse contrato estiver presente, as APIs de transmissão estarão disponíveis no dispositivo.

Em seguida, obtenha uma instância da classe **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** chamando o método de fábrica **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui#Windows_Media_AppBroadcasting_AppBroadcastingUI_GetDefault)** no computador, no qual há um único usuário conectado por vez. No XBox, em que vários usuários podem estar conectados, chame **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui#Windows_Media_AppBroadcasting_AppBroadcastingUI_GetForUser_Windows_System_User_)**. Em seguida, chame **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui#Windows_Media_AppBroadcasting_AppBroadcastingUI_GetStatus)** para obter o status de transmissão do app.

A propriedade **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus#Windows_Media_AppBroadcasting_AppBroadcastingStatus_CanStartBroadcast)** da classe **AppBroadcastingStatus** informa se o app pode iniciar a transmissão no momento. Em caso negativo, você pode verificar a propriedade **[Details](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus#Windows_Media_AppBroadcasting_AppBroadcastingStatus_Details)** para descobrir por que a transmissão não está disponível. Dependendo do motivo, convém exibir o status para o usuário ou mostrar instruções para habilitar a transmissão.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

Solicite que a interface do usuário de transmissão do app seja mostrado pelo sistema chamando **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui#Windows_Media_AppBroadcasting_AppBroadcastingUI_ShowBroadcastUI)**.

> [!NOTE] 
> O método **ShowBroadcastUI** representa uma solicitação que pode não ser bem-sucedida, dependendo do estado atual do sistema. O app não deve assumir que a transmissão foi iniciada depois que esse método foi chamado. Use o evento **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor#Windows_Media_AppBroadcasting_AppBroadcastingMonitor_IsCurrentAppBroadcastingChanged)** para ser notificado quando a transmissão iniciar ou parar.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Receber notificações que informam o início e a interrupção da transmissão
Registre-se para receber notificações quando o usuário utilizar a interface do usuário do sistema para iniciar ou interromper a transmissão do app por meio da inicialização de uma instância da classe **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** e do registro de um manipulador para o evento **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor#Windows_Media_AppBroadcasting_AppBroadcastingMonitor_IsCurrentAppBroadcastingChanged)**. Conforme discutido na seção anterior, não deixe de usar o **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsApiContractPresent_System_String_System_UInt16_System_UInt16_)** em algum momento para verificar se as APIs de transmissão estão presentes no dispositivo antes de tentar usá-las. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

No manipulador do evento **IsCurrentAppBroadcastingChanged**, talvez seja necessário atualizar a interface do usuário do app para refletir o estado atual da transmissão.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>Tópicos relacionados

* [Jogos](index.md)

 

 





---
ms.assetid: ''
description: Este artigo mostra como se conectar a câmeras remotas e obter um MediaFrameSourceGroup para recuperar quadros de cada câmera.
title: Conectar-se a câmeras remotas
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 253eea00ba6c4188197224111909c28a53932b88
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257355"
---
# <a name="connect-to-remote-cameras"></a>Conectar-se a câmeras remotas

Este artigo mostra como se conectar a uma ou mais câmeras remotas e obter um objeto [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) que permite que você leia quadros de cada câmera. Para obter mais informações sobre como ler quadros de uma fonte de mídia, consulte [processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md). Para obter mais informações sobre emparelhamento com dispositivos, consulte [emparelhar dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Os recursos discutidos neste artigo só estão disponíveis a partir do Windows 10, versão 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Criar uma classe DeviceWatcher para observar as câmeras remotas disponíveis

A classe [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) monitora os dispositivos disponíveis para seu aplicativo e notifica seu aplicativo quando os dispositivos são adicionados ou removidos. Obtenha uma instância de **DeviceWatcher** chamando [**DeviceInformation. createassister**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), passando uma cadeia de caracteres AQS (sintaxe de consulta avançada) que identifica o tipo de dispositivos que você deseja monitorar. A cadeia de caracteres AQS especificando dispositivos de câmera de rede é a seguinte:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> O método auxiliar [**MediaFrameSourceGroup. GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) retorna uma cadeia de caracteres AQS que monitorará as câmeras de rede remota e conectadas localmente. Para monitorar apenas câmeras de rede, você deve usar a cadeia de caracteres AQS mostrada acima.


Quando você iniciar o **DeviceWatcher** retornado chamando o método [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) , ele gerará o evento [**adicionado**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) para cada câmera de rede disponível no momento. Até que você pare o observador chamando [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), o evento **adicionado** será gerado quando novos dispositivos de câmera de rede ficarem disponíveis e o evento [**removido**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) será gerado quando um dispositivo de câmera ficar indisponível.

Os args de evento passados para os manipuladores de eventos **adicionados** e **removidos** são um objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) ou [**DeviceInformationUpdate**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) , respectivamente. Cada um desses objetos tem uma propriedade **ID** que é o identificador para a câmera de rede para a qual o evento foi acionado. Passe essa ID para o método [**MediaFrameSourceGroup. FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) para obter um objeto [**MediaFrameSourceGroup**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) que você pode usar para recuperar quadros da câmera.

## <a name="remote-camera-pairing-helper-class"></a>Classe auxiliar de emparelhamento de câmera remota

O exemplo a seguir mostra uma classe auxiliar que usa um **DeviceWatcher** para criar e atualizar uma **ObservableCollection** de objetos **MediaFrameSourceGroup** para dar suporte à ligação de dados com a lista de câmeras. Aplicativos típicos encapsulariam o **MediaFrameSourceGroup** em uma classe de modelo Personalizada. Observe que a classe auxiliar mantém uma referência ao [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) do aplicativo e atualiza a coleção de câmeras em chamadas para [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para garantir que a interface do usuário vinculada à coleção seja atualizada no thread da interface do usuário.

Além disso, este exemplo manipula o evento [**DeviceWatcher. updated**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) além dos eventos **adicionados** e **removidos** . No manipulador **atualizado** , o dispositivo de câmera remota associado é removido de e, em seguida, adicionado de volta à coleção.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Foto básica, vídeo e captura de áudio com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Exemplo de quadros de câmera](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 





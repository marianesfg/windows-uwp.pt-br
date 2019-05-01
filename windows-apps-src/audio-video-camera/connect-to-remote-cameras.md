---
ms.assetid: ''
description: Este artigo mostra como conectar-se para câmeras remotas e obter um MediaFrameSourceGroup para recuperar os quadros de cada câmera.
title: Conectar-se para câmeras remotas
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789591"
---
# <a name="connect-to-remote-cameras"></a>Conectar-se para câmeras remotas

Este artigo mostra como se conectar a um ou mais câmeras remotas e obter um [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) objeto que permite que você leia os quadros de cada câmera. Para obter mais informações sobre a leitura de quadros de uma fonte de mídia, consulte [processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md). Para obter mais informações sobre o emparelhamento com dispositivos, consulte [Emparelhe dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Os recursos discutidos neste artigo só estão disponíveis a partir do Windows 10, versão 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Criar uma classe DeviceWatcher para assistir a câmera remota disponível

O [ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) classe monitora os dispositivos disponíveis para seu aplicativo e notifica seu aplicativo quando dispositivos são adicionados ou removidos. Obtenha uma instância de **DeviceWatcher** chamando [ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), passando uma cadeia de caracteres de sintaxe de consulta avançada (AQS) que identifica o tipo de dispositivos que você deseja monitorar. A cadeia de caracteres AQS especificar dispositivos de câmera de rede é o seguinte:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> O método auxiliar [ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) retorna uma cadeia de caracteres AQS monitorará câmeras de rede conectados localmente e remota. Para monitorar apenas as câmeras de rede, você deve usar a cadeia de caracteres AQS mostrada acima.


Quando você inicia o retornada **DeviceWatcher** chamando o [ **iniciar** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) método, ele irá disparar o [ **adicionado** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) evento para cada câmera de rede que está disponível no momento. Até que você interrompa o Inspetor chamando [ **interromper**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), o **adicionado** evento será gerado quando novos dispositivos de câmera de rede se tornarem disponíveis e o [ **Removed** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) evento será gerado quando um dispositivo de câmera se torna indisponível.

Os args de evento passados para o **adicionado** e **removido** manipuladores de eventos são um [ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) ou uma [  **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) do objeto, respectivamente. Cada um desses objetos tem um **Id** propriedade que é o identificador para a câmera de rede para o qual o evento foi disparado. Passar essa ID para o [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) método para obter um [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) objeto que você pode usar para recupera os quadros da câmera.

## <a name="remote-camera-pairing-helper-class"></a>Classe auxiliar de emparelhamento de câmera remota

O exemplo a seguir mostra uma classe auxiliar que usa um **DeviceWatcher** para criar e atualizar uma **ObservableCollection** dos **MediaFrameSourceGroup** objetos para dar suporte associação de dados à lista de câmeras. Aplicativos típicos encapsularia a **MediaFrameSourceGroup** em uma classe de modelo personalizado. Observe que a classe auxiliar mantém uma referência para o aplicativo [ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) e atualiza a coleção de câmeras em chamadas para [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para garantir que a interface do usuário associado à coleção é atualizada no thread da interface do usuário.

Além disso, este exemplo trata a [ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) eventos além os **adicionado** e **removido** eventos. No **Updated** manipulador, o dispositivo de câmera remota associada é removida do e, em seguida, adicionado para a coleção.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Exemplo de quadros de câmera](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Quadros de processos de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 





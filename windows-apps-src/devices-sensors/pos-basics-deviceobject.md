---
title: Objetos de dispositivo do PointOfService
description: Saiba mais sobre a criação de objetos de dispositivo do PointOfService
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: a2fa7e107d890a5be7c8d27af03289b839ec3c09
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209982"
---
# <a name="pointofservice-device-objects"></a>Objetos de dispositivo do PointOfService

## <a name="creating-a-device-object"></a>Criar um objeto de dispositivo
Depois de identificar o dispositivo do PointOfService que você deseja usar, de uma enumeração nova ou uma DeviceID armazenada, chame [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) com a[**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) escolhida programaticamente ou que o usuário selecionou para criar um novo objeto de dispositivo do Ponto de Serviço.

Esse exemplo tenta criar um novo objeto BarcodeScanner com FromIdAsync usando uma DeviceID. Em caso de falha ao criar o objeto, uma mensagem de depuração é gravada.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

Quando você tiver um objeto de dispositivo, é possível acessar os métodos, as propriedades e os eventos do dispositivo.  

## <a name="device-object-lifecycle"></a>Ciclo de vida de objeto de dispositivo
Antes do Windows 8, os aplicativos tinham um ciclo de vida simples. Os aplicativos do Win32 e .NET estão em execução ou não e os periféricos do PointOfService geralmente foram solicitados para o ciclo de vida do aplicativo completo. Quando um usuário minimizava-os ou saia deles, eles continuavam em execução. Isso não era um problema até que os dispositivos portáteis e o gerenciamento de energia se tornassem cada vez mais importantes.

O Windows 8 introduziu um novo modelo de aplicativo com os aplicativos UWP. Em um nível alto, um novo estado suspenso foi adicionado. Um aplicativo UWP é suspenso assim que o usuário minimiza ou alterna para outro aplicativo. Isso significa que os threads do aplicativo param, o aplicativo é deixado na memória exceto quando o sistema operacional precisa solicitar recursos, e qualquer objeto de dispositivo representando periféricos do PointOfService é fechado automaticamente para permitir que outros aplicativos acessem os periféricos. Quando o usuário alterna para o aplicativo, é possível restaurá-lo rapidamente para um estado de execução e restaurar conexões de periféricos do PointOfService desde que ainda estão disponíveis ao retomar.

Você pode detectar quando um objeto é fechado por qualquer motivo com um \<deviceobject\>. Manipulador de eventos fechado anote a ID do dispositivo para restabelecer a conexão no futuro.   Como alternativa, você pode escolher processar isso em uma notificação de suspensão do aplicativo para salvar a ID do dispositivo para estabelecer novamente as conexões do dispositivo em uma notificação de Retomada do aplicativo.  Certifique-se de não dobrar os manipuladores de eventos e as ações duplicadas para o objeto de dispositivo na\>\<deviceobject. Fechado e suspensão de aplicativo.

> [!TIP]
> Consulte os tópicos a seguir para obter mais informações sobre o ciclo de vida do aplicativo da Plataforma Universal do Windows (UWP) para Windows 10:
> - [Ciclo de vida do aplicativo Windows 10 Plataforma Universal do Windows (UWP)](../launch-resume/app-lifecycle.md)
> - [Lidar com a suspensão do aplicativo](../launch-resume/suspend-an-app.md)
> - [Tratar a retomada do aplicativo](../launch-resume/resume-an-app.md)

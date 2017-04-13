---
author: msatranjr
title: Perguntas frequentes de desenvolvedores de Bluetooth
description: "Este artigo contém respostas para perguntas frequentes relacionadas às APIs Bluetooth da UWP."
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.openlocfilehash: 3dabc5ad2833eecfec1f397bdd5bf7f2b807a48d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="bluetooth-developer-faq"></a>Perguntas frequentes de desenvolvedores para recursos Bluetooth

Este artigo contém respostas às perguntas mais comuns sobre a API Bluetooth da UWP.

## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Por que meu dispositivo Bluetooth LE para de responder após uma perda de conexão?

O motivo comum que faz com que isso aconteça é porque o dispositivo remoto perde as informações de emparelhamento. Anteriormente, os dispositivos Bluetooth não exigiam autenticação. Para proteger o usuário, todos os eventos de emparelhamento realizados no aplicativo Configurações exigem autenticação e alguns dispositivos não sabem como lidar com isso. 

A partir do Windows 10 versão 1511, os desenvolvedores têm controle sobre o evento de emparelhamento. A [enumeração do dispositivo e o exemplo de emparelhamento](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) fornecem detalhes sobre os vários aspectos de associação de novos dispositivos.

Neste exemplo, iniciamos o emparelhamento com um dispositivo sem usar nenhuma criptografia. Observe que isso funcionará somente se o dispositivo remoto não exigir criptografia ou autenticação para funcionar.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>É necessário emparelhar dispositivos Bluetooth antes de usá-los?

Não é necessário para dispositivos Bluetooth RFCOMM (clássicos). A partir do Windows 10 versão 1607, você pode simplesmente consultar os dispositivos próximos e conectar-se a eles. O [exemplo do RFCOMM Chat](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) atualizado mostra essa funcionalidade. 

Este recurso não está disponível para Bluetooth de Baixa Energia (GATT Client); portanto ainda será necessário emparelhar por meio da página Configurações ou usando as APIs [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) para acessar esses dispositivos.


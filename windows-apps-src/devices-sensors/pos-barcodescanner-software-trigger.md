---
author: eliotcowley
title: Usar um gatilho de software
description: Saiba como controlar o ato de verificação de software.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 03fbed5a0145a093b1e7a2535012077644aaf2e2
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6445486"
---
# <a name="use-a-software-trigger"></a>Usar um gatilho de software

Pode ser útil controlar o ato de verificação de software se você estiver usando um scanner de código de barras no modo de apresentação ou se o scanner não tiver um gatilho físico, como um scanner de código de barras baseado em câmera. Você pode iniciar o processo de verificação chamando [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Dependendo do valor de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) o scanner pode verificar apenas um código de barras e depois parar ou verificar continuamente até que você chame [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Defina o valor desejado de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar o comportamento do scanner quando um código de barras é decodificado.

| Valor | Description |
| ----- | ----------- |
| True   | Verificar apenas um código de barras e depois parar |
| False  | Verificar continuamente o códigos de barras sem parar |


> [!Important]
> Certifique-se de que o scanner de código de barras permite o uso de gatilho de software verificando primeiro a propriedade [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

O exemplo a seguir mostra como iniciar verificação usando um gatilho de software, que interromperá a verificação depois que ele verifica um código de barras:

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]
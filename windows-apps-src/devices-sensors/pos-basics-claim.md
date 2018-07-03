---
author: TerryWarwick
title: Modelo de declaração do dispositivo de PointOfService
description: Saiba mais sobre o modelo de declaração de PointOfService
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983723"
---
# <a name="point-of-service-device-claim-model"></a>Modelo de declaração do dispositivo de Ponto de Serviço

## <a name="claiming-a-device-for-exclusive-use"></a>Solicitar um dispositivo para uso exclusivo

Depois de criar um objeto de dispositivo de PointOfService com sucesso, você deve declará-lo usando o método adequado para o tipo de dispositivo antes de usá-lo para entrada ou saída.  A declaração concede ao aplicativo acesso exclusivo a diversas funções do dispositivo para garantir que um aplicativo não interfira com o uso do dispositivo por outro aplicativo.  Apenas um aplicativo pode reivindicar um dispositivo de PointOfService para uso exclusivo por vez. 

Esse exemplo mostra como declarar um dispositivo de scanner de código de barras depois de criar com êxito um objeto de scanner de código de barras.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

> [!Warning]
> Uma declaração pode ser perdida nas seguintes circunstâncias:
> 1. Outro aplicativo solicitou uma declaração do mesmo dispositivo e o aplicativo não emitiu uma **RetainDevice** em resposta ao evento **ReleaseDeviceRequested**.  (Consulte [Negociação de declaração](#Claim-negotiation) abaixo para saber mais.)
> 2. O aplicativo foi suspenso, o que resultou no objeto do dispositivo ser fechado e consequentemente a declaração não é mais válida. (Consulte [Ciclo de vida do objeto do dispositivo](pos-basics-deviceobject.md#device-object-lifecycle) para obter mais informações.)

### <a name="apis-used-for-claiming"></a>APIs usadas para a declaração

|Dispositivo|Declaração |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>Negociação de solicitação

Como o Windows é um ambiente multitarefa é possível que diversos aplicativos no mesmo computador exijam acesso aos periféricos de forma cooperativa.  As APIs de PointOfService fornecem um modelo de negociação que permite a vários aplicativos compartilhar periféricos conectados ao computador.

Quando um segundo aplicativo no mesmo computador solicita uma declaração para um periférico de PointOfService já declarado por outro aplicativo, uma notificação de evento **ReleaseDeviceRequested** é publicada. O aplicativo com a declaração ativa deve responder à notificação de evento chamando **RetainDevice** se o aplicativo está atualmente usando o dispositivo para evitar a perda da declaração. 

Se o aplicativo com a declaração ativa não responde com **RetainDevice** imediatamente, consideremos que o aplicativo foi suspenso ou não necessita do dispositivo, e a declaração é revogada e fornecida para o novo aplicativo. 

Este exemplo mostra como manter um scanner de código de barras declarado depois que outro aplicativo solicitou que o dispositivo seja lançado.  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>APIs usadas para negociação de solicitação

|Dispositivo solicitado|Notificação de versão| Manter o dispositivo |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|

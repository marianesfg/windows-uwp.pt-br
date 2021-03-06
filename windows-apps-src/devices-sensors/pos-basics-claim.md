---
title: PointOfService declaração de dispositivo e habilitar modelo
description: Saiba mais sobre a declaração PointOfService e habilite o modelo
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: bc3a8afbc0d3ca4655e0b1745090db633bcd92b7
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684672"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Declaração de dispositivo de ponto de serviço e habilitar modelo

## <a name="claiming-for-exclusive-use"></a>Reivindicação de uso exclusivo

Depois de criar um objeto de dispositivo de PointOfService com sucesso, você deve declará-lo usando o método adequado para o tipo de dispositivo antes de usá-lo para entrada ou saída.  A declaração concede ao aplicativo acesso exclusivo a diversas funções do dispositivo para garantir que um aplicativo não interfira com o uso do dispositivo por outro aplicativo.  Apenas um aplicativo pode reivindicar um dispositivo de PointOfService para uso exclusivo por vez. 

> [!Note]
> A ação de declaração estabelece um bloqueio exclusivo para um dispositivo, mas não o coloca em um estado operacional.  Consulte [Habilitar dispositivo para operações de e/s](#enable-device-for-io-operations) para obter mais informações.

### <a name="apis-used-to-claim--release"></a>APIs usadas para reivindicar/liberar

|Dispositivo|Declaração | Liberação | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Habilitar dispositivo para operações de e/s

A ação de declaração simplesmente estabelece um direito exclusivo para o dispositivo, mas não o coloca em um estado operacional.  Para receber eventos ou executar qualquer operação de saída, você deve habilitar o dispositivo usando o **EnableAsync**.  Por outro lado, você pode chamar **DisableAsync** para parar de escutar eventos do dispositivo ou executar a saída.  Você também pode usar **IsEnabled** para determinar o estado do seu dispositivo.

### <a name="apis-used-enable--disable"></a>APIs usadas habilitar/desabilitar

| Dispositivo | Enable | Desativar | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | ¹ não aplicável | ¹ não aplicável | ¹ não aplicável | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

a exibição de linha ¹ não exige que você habilite explicitamente o dispositivo para operações de e/s.  Habilitar é executado automaticamente pelas APIs PointOfService LineDisplay que executam e/s.

## <a name="code-sample-claim-and-enable"></a>Exemplo de código: declaração e habilitação

Esse exemplo mostra como declarar um dispositivo de scanner de código de barras depois de criar com êxito um objeto de scanner de código de barras.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> Uma declaração pode ser perdida nas seguintes circunstâncias:
> 1. Outro aplicativo solicitou uma declaração do mesmo dispositivo e o aplicativo não emitiu uma **RetainDevice** em resposta ao evento **ReleaseDeviceRequested**.  (Consulte [Negociação de declaração](#claim-negotiation) abaixo para saber mais.)
> 2. O aplicativo foi suspenso, o que resultou no objeto do dispositivo ser fechado e consequentemente a declaração não é mais válida. (Consulte [Ciclo de vida do objeto do dispositivo](pos-basics-deviceobject.md#device-object-lifecycle) para obter mais informações.)


## <a name="claim-negotiation"></a>Negociação de solicitação

Como o Windows é um ambiente multitarefa é possível que diversos aplicativos no mesmo computador exijam acesso aos periféricos de forma cooperativa.  As APIs de PointOfService fornecem um modelo de negociação que permite a vários aplicativos compartilhar periféricos conectados ao computador.

Quando um segundo aplicativo no mesmo computador solicita uma declaração para um periférico de PointOfService já declarado por outro aplicativo, uma notificação de evento **ReleaseDeviceRequested** é publicada. O aplicativo com a declaração ativa deve responder à notificação de evento chamando **RetainDevice** se o aplicativo está atualmente usando o dispositivo para evitar a perda da declaração. 

Se o aplicativo com a declaração ativa não responde com **RetainDevice** imediatamente, consideremos que o aplicativo foi suspenso ou não necessita do dispositivo, e a declaração é revogada e fornecida para o novo aplicativo. 

A primeira etapa é criar um manipulador de eventos que responda ao evento **ReleaseDeviceRequested** com **RetainDevice**.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

Em seguida, registre o manipulador de eventos em associação ao seu dispositivo declarado

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
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

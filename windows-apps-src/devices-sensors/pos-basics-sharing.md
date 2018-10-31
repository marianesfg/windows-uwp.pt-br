---
author: TerryWarwick
title: Compartilhamento de dispositivos de PointOfService
description: Compartilhar periféricos do PointOfService com outras pessoas
ms.author: jken
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 591ba592d1c17b03ae29c6fb98ef546bc18b8bdc
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821541"
---
# <a name="pointofservice-device-sharing"></a>Compartilhamento de dispositivos de PointOfService

Saiba como compartilhar periféricos Bluetooth conectado ou rede com outros computadores em um ambiente onde vários computadores contam com periféricos compartilhados em vez de dedicado periféricos conectados para cada computador.

## <a name="device-sharing"></a>Compartilhamento de dispositivos

Rede e Bluetooth periféricos do PointOfService conectados são normalmente usados em um ambiente wheere vários dispositivos de cliente estão compartilhando os mesmos periféricos durante o dia.  Em um ambiente de varejo ou serviços de alimentos ocupado qualquer atraso na capacidade de um dispositivo cliente para se conectar a um periférico tem um impacto sobre a eficiência em que um colega pode fechar uma transação com o cliente e passar para a próxima. Em um cenário de restaurante serviço rápida onde uma impressora de recibo é usada como uma impressora cozinha para transferir os detalhes do pedido do cliente para a cozinha para preparação, haverá vários dispositivos cliente levando pedidos de clientes.  Depois que a ordem for concluída cada dispositivo cliente deve ser capaz de solicitar a impressora compartilhada e imprimir imediatamente a ordem para a cozinha.

Nesses ambientes, é importante para o aplicativo totalmente **Descartar** o objeto de dispositivo para que outra pode reivindicar o mesmo dispositivo.

Descartar uma PosPrinter no final de um bloco 'usando'

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Descartar uma PosPrinter chamando explicitamente Dispose)

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Métodos da API usados 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]

---
title: Compartilhamento de dispositivo PointOfService
description: Compartilhando PointOfService periféricos com outras pessoas
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618941"
---
# <a name="pointofservice-device-sharing"></a>Compartilhamento de dispositivo PointOfService

Aprenda a compartilhar periféricos de Bluetooth conectado ou de rede com outros computadores em um ambiente onde vários PCs dependem de periféricos compartilhados em vez de periféricos dedicados anexados a cada computador.

## <a name="device-sharing"></a>Compartilhamento de dispositivo

Rede e Bluetooth periféricos conectados de PointOfService normalmente são usados em um ambiente wheere vários dispositivos de cliente estão compartilhando os mesmos periféricos ao longo do dia.  Em um ambiente de varejo ou serviços de alimentos ocupado qualquer atraso na capacidade de um dispositivo cliente anexar a um periférico tem um impacto sobre a eficiência em que um colega pode fechar uma transação com o cliente e para a próxima. Em um cenário de restaurante rápida do serviço onde uma impressora de recebimento é usada como uma impressora cozinha para transferir os detalhes do pedido do cliente para a cozinha para a preparação, haverá vários dispositivos de cliente receber ordens dos clientes.  Após a conclusão da ordem de cada dispositivo cliente deve ser capaz de declaração de impressora compartilhada e imprimir imediatamente a ordem de cozinha.

Nesses ambientes, é importante para que o aplicativo totalmente **dispose** o dispositivo do objeto, de modo que outro pode solicitar o mesmo dispositivo.

Como descartar um PosPrinter no final de um bloco 'using'

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


Como descartar um PosPrinter chamando Dispose () explicitamente

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

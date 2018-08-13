---
author: msatranjr
title: Perguntas frequentes de desenvolvedores de Bluetooth
description: Este artigo contém respostas para perguntas frequentes relacionadas às APIs Bluetooth da UWP.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: c0af6a19e17a62ed82c32e68ea1732e1f51d4641
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "301922"
---
# <a name="bluetooth-developer-faq"></a>Perguntas frequentes de desenvolvedores para recursos Bluetooth

Este artigo contém respostas às perguntas mais comuns sobre a API Bluetooth da UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quais as APIs que usar? Bluetooth clássico (RFCOMM) ou Bluetooth baixa energia (GATT)?
Há vários diálogos online sobre este tópico geral vamos manter essa resposta diretamente sobre a diferença em relação ao Windows. Aqui estão algumas diretrizes gerais:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Use as APIs de GATT quando você estiver se comunicando com um dispositivo que ofereça suporte a Bluetooth baixa energia. Se você estiver usar case é esporádica, baixa largura de banda ou requer baixa energia, Bluetooth baixa energia é a resposta. O namespace principal que inclui essa funcionalidade é [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quando não usar LE Bluetooth**
- Alta largura de banda alta frequência cenários. Se você precisar manter constantemente sync com grandes quantidades de dados, considere o uso de WiFi clássico ou talvez par de Bluetooth. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth clássico (Windows.Devices.Bluetooth.Rfcomm)

As APIs de RFCOMM dar aos desenvolvedores um soquete para realizar a comunicação do estilo e bidirecional de porta serial. Depois que você acaba de criar um soquete, os métodos de gravação e leitura a partir dele são razoavelmente padrão. Uma implementação disso é apresentada na [amostra de bate-papo de Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quando não usar Bluetooth Rfcomm** 
- Notificações. O protocolo GATT Bluetooth tem um comando específico para que isso e resultará em consideravelmente menor gasto de energia e tempos de resposta mais rápidos. 
- Verificando a detecção de proximidade ou presença. Melhor usar as [APIs de anúncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) e se conectam via Bluetooth LE. 


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

**(14393 e abaixo)** Esse recurso não está disponível para Bluetooth baixa energia (GATT cliente), portanto você ainda precisará par através de página de configurações ou usando o [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) APIs no access ordem esses dispositivos.

**(15030 e acima)** Emparelhamento de dispositivos Bluetooth não é mais necessária. Use as novas APIs Async como GetGattServicesAsync e GetCharacteristicsAsync para consultar o estado atual do dispositivo remoto. Consulte os [documentos de cliente](gatt-client.md) para obter mais detalhes. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quando deve eu emparelhar com um dispositivo antes de se comunicar com ele?
Em geral, se você precisar de um título de longo prazo confiável com um dispositivo, Emparelhe com ele (direcionando o usuário para a página de configurações ou usando o dispositivo enumeração e o emparelhamento APIs). Se você precisar simplesmente ler informações desativa o dispositivo que estiver exposto publicamente (um sensor de temperatura ou beacon), conecte-se ou escutar anúncios sem fazer qualquer esforço emparelhar com o dispositivo. Isto evitará a longo prazo problemas de interoperabilidade porque uma ampla gama de dispositivos não suportam emparelhamento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Todos os dispositivos do Windows têm suporte para a função periféricos?

Não – esse é um recurso dependentes de hardware, mas um método é fornecido (BluetoothAdapter.IsPeripheralRoleSupported) à consulta se ou não é suportado.  Dispositivos suportados atualmente incluem Windows Phone em 8992 + e RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Pode acessar essas APIs do Win32?

Sim, todas essas APIs deve funcionar. Este blog fornece detalhes sobre a maneira de chamar [APIs de aplicativos de Desktop do Windows](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Essa funcionalidade deve existir em *- Aqui para inserir SKU -*?

**Bluetooth LE**: Sim, todas as funcionalidades que se situa OneCore e deve estar disponível em dispositivos mais recentes c / uma pilha de Bluetooth LE está funcionando. 
> Limitação: Função periférica é dependente de hardware e algumas edições do Windows Server não oferece suporte a Bluetooth. 

**Bluetooth BR/EDR (clássico)**: existem algumas variações, mas de modo geral, têm suporte de nível muito similar ao perfil. Consulte os documentos sobre [RFCOMM](send-or-receive-files-with-rfcomm.md) e esses documentos de perfil com suporte para [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) e [telefone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

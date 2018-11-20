---
author: msatranjr
title: Perguntas frequentes de desenvolvedores de Bluetooth
description: Este artigo contém respostas para perguntas frequentes relacionadas às APIs Bluetooth da UWP.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 03ee8074a64b210d33498c8de135a76900d968f0
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284487"
---
# <a name="bluetooth-developer-faq"></a>Perguntas frequentes de desenvolvedores para recursos Bluetooth

Este artigo contém respostas às perguntas mais comuns sobre a API Bluetooth da UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quais APIs usar? Bluetooth clássico (RFCOMM) ou Bluetooth de baixa energia (GATT)?
Há várias discussões on-line em torno esse tópico geral vamos manter essa resposta diretamente sobre a diferença em relação ao Windows. Aqui estão algumas diretrizes gerais:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (genericattributeprofile)

Use as APIs de GATT quando você está se comunicando com um dispositivo compatível com Bluetooth de baixa energia. Se você estiver usar caso não frequente e de baixa largura de banda ou requer baixo consumo de energia, a resposta é de Bluetooth de baixa energia. O namespace principal que inclui essa funcionalidade é [genericattributeprofile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quando não usar Bluetooth LE**
- Grande largura de banda, cenários de alta frequência. Se você precisar manter constantemente a sincronização com grandes quantidades de dados, considere usar Wi-Fi clássico ou talvez até mesmo de Bluetooth. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth clássico (RFCOMM)

As APIs de RFCOMM fornecer aos desenvolvedores um soquete para realizar a comunicação de estilo de porta serial bidirecional. Depois que você tenha um soquete, os métodos de gravação e leitura dele são razoavelmente padrão. Uma implementação de isso é apresentada no [exemplo de Rfcomm Chat](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quando não usar Bluetooth Rfcomm** 
- Notificações. O protocolo de GATT de Bluetooth tem um comando específico para isso e resultará em significativamente menos consumo de energia e tempos de resposta mais rápidos. 
- Verificando a detecção de presença ou de proximidade. Melhor usar as [APIs de anúncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) e conectar-se por Bluetooth LE. 


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

**(14393 e abaixo)** Esse recurso não está disponível para Bluetooth de baixa energia (GATT Client), portanto, você ainda terá de par por meio da página de configurações ou usando o [Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) APIs em acesso de ordem esses dispositivos.

**(15030 e acima)** Emparelhamento de dispositivos Bluetooth não é mais necessária. Use as novas APIs Async como GetGattServicesAsync e GetCharacteristicsAsync para consultar o estado atual do dispositivo remoto. Veja os [documentos de cliente](gatt-client.md) para obter mais detalhes. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quando deve posso emparelhar com um dispositivo antes de se comunicar com ele?
Em geral, se você precisar de um título confiável, a longo prazo com um dispositivo, Emparelhe com ele (direcionando o usuário para a página de configurações ou usando as APIs de emparelhamento e enumeração do dispositivo). Se você só precisa ler as informações desativar o dispositivo que está exposto publicamente (um sensor de temperatura ou beacon), conecte ou escutar anúncios sem fazer qualquer esforço emparelhar com o dispositivo. Isso impedirá a longo prazo problemas de interoperabilidade como um host de dispositivos não dão suporte a emparelhamento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Todos os dispositivos Windows dão suporte a função periférica?

Não – esse é um recurso de hardware dependentes, mas um método é fornecido (BluetoothAdapter.IsPeripheralRoleSupported) para consultar se ele é compatível ou não.  Dispositivos com suporte no momento incluem Windows Phone em 8992 + e RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Pode acessar essas APIs do Win32?

Sim, todas essas APIs devem funcionar. Este blog fornece detalhes sobre a maneira de chamar [APIs do Windows de aplicativos da área de trabalho](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Essa funcionalidade deve existir no *- Aqui para inserir SKU -*?

**Bluetooth LE**: Sim, todas as funcionalidades está em OneCore e devem estar disponíveis em dispositivos mais recentes com uma pilha de Bluetooth LE funcional. 
> Limitação: A função periférica é dependente de hardware e algumas edições do Windows Server não oferece suporte a Bluetooth. 

**Bluetooth BR/EDR (clássico)**: existem algumas variações, mas de modo geral, eles têm suporte em nível de perfil muito semelhante. Veja os documentos em [RFCOMM](send-or-receive-files-with-rfcomm.md) e esses documentos do perfil com suporte para [computador](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) e [telefone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)


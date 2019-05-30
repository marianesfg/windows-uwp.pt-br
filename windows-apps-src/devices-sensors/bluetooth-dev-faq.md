---
title: Perguntas frequentes de desenvolvedores para Bluetooth
description: Este artigo contém respostas para perguntas frequentes relacionadas às APIs Bluetooth da UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 72e45f8ef0f5684b3a712056eb367975f8e6103a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370357"
---
# <a name="bluetooth-developer-faq"></a>Perguntas frequentes de desenvolvedores de Bluetooth

Este artigo contém respostas às perguntas mais comuns sobre a API Bluetooth da UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quais APIs devo usar? Bluetooth Clássico (RFCOMM) ou Bluetooth de Baixa Energia (GATT)?
Há várias discussões online sobre este tópico geral, então vamos manter essa resposta diretamente sobre a diferença em relação ao Windows. Veja algumas diretrizes gerais:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Use as APIs GATT quando estiver se comunicando com um dispositivo que dê suporte ao Bluetooth de Baixa Energia. Se o seu caso de uso não for frequente, tiver largura de banda baixa ou se exigir baixo consumo de energia, o Bluetooth de Baixa Energia é a resposta. O namespace principal que inclui essa funcionalidade é [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quando não usar o Bluetooth LE**
- Grande largura de banda, cenários de alta frequência. Se você precisar manter constantemente a sincronização com grandes quantidades de dados, considere o uso do Bluetooth clássico ou talvez até Wi-Fi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Clássico (Windows.Devices.Bluetooth.Rfcomm)

As APIs RFCOMM dão aos desenvolvedores um soquete para realizar a comunicação de estilo de porta serial bidirecional. Depois que você tiver um soquete, os métodos de gravação e de leitura dele são bem padrão. Uma implementação disso é apresentada no [Exemplo de Chat Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quando não usar o Bluetooth Rfcomm** 
- Notificações. O protocolo GATT do Bluetooth tem um comando específico para isso e resultará em significativamente menos consumo de energia e tempos de resposta. 
- Verificação de detecção de presença ou de proximidade. Melhor usar as [APIs de anúncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) e conectar-se por Bluetooth LE. 


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

**(14393 e inferior)** Este recurso não está disponível para Bluetooth de Baixa Energia (GATT Client); portanto ainda será necessário emparelhar por meio da página Configurações ou usando as APIs [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) para acessar esses dispositivos.

**(15030 e superior)** O emparelhamento de dispositivos Bluetooth não é mais necessário. Use as novas APIs Assíncronas como GetGattServicesAsync e GetCharacteristicsAsync para consultar o estado atual do dispositivo remoto. Veja os [Documentos de cliente](gatt-client.md) para obter mais detalhes. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quando devo emparelhar com um dispositivo antes de me comunicar com ele?
Em geral, se você precisar de uma associação confiável a longo prazo com um dispositivo, emparelhe com ele (direcionando o usuário para a página de configurações ou usando as APIs Enumeração de Dispositivos e Emparelhamento). Se você simplesmente precisa ler informações de desativar o dispositivo que está expostos publicamente (um sensor de temperatura ou beacon), conecte-se ou escutar anúncios sem fazer qualquer esforço para emparelhar com o dispositivo. Isso evitará problemas de interoperabilidade a longo prazo porque um host de dispositivos não dá suporte ao emparelhamento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Todos os dispositivos Windows dão suporte à Função Periférica?

Não – esse é um recurso dependente de hardware, mas um método é fornecido (BluetoothAdapter.IsPeripheralRoleSupported) para consultar se ele tem suporte ou não.  Os dispositivos com suporte no momento incluem o Windows Phone no 8992+ e RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Posso acessar essas APIs do Win32?

Sim, todas essas APIs devem funcionar. Este blog detalha a maneira de chamar [APIs do Windows de aplicativos da área de trabalho](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Essa funcionalidade deve existir em *-Inserir SKU aqui-* ?

**Bluetooth LE**: Sim, toda a funcionalidade está em OneCore e deve estar disponível em dispositivos mais recentes com uma pilha de Bluetooth LE está funcionando. 
> Limitação: Função periférica é dependente de hardware e algumas edições do Windows Server não oferece suporte a Bluetooth. 

**Bluetooth BR/EDR (clássico)** : Existem algumas variações, mas de modo geral, têm suporte de nível muito semelhante ao perfil. Veja os documentos em [RFCOMM](send-or-receive-files-with-rfcomm.md) e esses documentos de perfil com suporte para [Computador](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) e [Telefone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)


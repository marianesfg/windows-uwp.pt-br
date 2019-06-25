---
title: Perguntas frequentes de desenvolvedores para Bluetooth
description: Este artigo contém respostas para perguntas frequentes relacionadas às APIs Bluetooth da UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 704ce146f95ed64a5891c130fee4d78c90dff034
ms.sourcegitcommit: 58d35b89662d4ad240650933e43fee0b00e9a962
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67344520"
---
# <a name="bluetooth-developer-faq"></a>Perguntas frequentes de desenvolvedores de Bluetooth

Este artigo contém respostas às perguntas mais comuns sobre a API Bluetooth da UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quais APIs devo usar? Bluetooth Clássico (RFCOMM) ou Bluetooth de Baixa Energia (GATT)?
Há várias discussões online sobre este tópico geral, então vamos manter essa resposta diretamente sobre a diferença em relação ao Windows. Veja algumas diretrizes gerais:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Use as APIs GATT quando estiver se comunicando com um dispositivo que dê suporte ao Bluetooth de Baixa Energia. Se o seu caso de uso é raro, de baixa largura de banda ou exige baixo consumo de energia, a resposta é Bluetooth baixa energia. O namespace principal que inclui essa funcionalidade é [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quando não usar o Bluetooth LE**
- Grande largura de banda, cenários de alta frequência. Se você precisar manter constantemente a sincronização com grandes quantidades de dados, considere o uso do Bluetooth clássico ou talvez até Wi-Fi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Clássico (Windows.Devices.Bluetooth.Rfcomm)

As APIs de RFCOMM dar aos desenvolvedores um soquete no qual executar a comunicação de estilo de porta serial bidirecional. Depois que você tem um soquete, os métodos para gravar e ler são razoavelmente padrão. Uma implementação disso é apresentada no [Exemplo de Chat Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quando não usar o Bluetooth Rfcomm** 
- Notificações. O protocolo GATT do Bluetooth tem um comando específico para isso e resultará em significativamente menos consumo de energia e tempos de resposta. 
- Verificação de detecção de presença ou de proximidade. Melhor usar as [APIs de anúncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) e conectar-se por Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Por que meu dispositivo Bluetooth LE para de responder após uma perda de conexão?

O motivo mais comum que isso ocorre é porque o dispositivo remoto perdeu a informações de junção. Um grande número de dispositivos de Bluetooth mais antigos não exige autenticação. Para proteger o usuário, todas as transações de emparelhamento realizadas pelo aplicativo em configurações exigirá a autenticação e alguns dispositivos não foram projetados tendo isso em mente. 

Começando com o Windows 10 versão 1511, os desenvolvedores têm controle sobre o handshake de emparelhamento. A [enumeração do dispositivo e o exemplo de emparelhamento](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) fornecem detalhes sobre os vários aspectos de associação de novos dispositivos.

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

Você não tem a dispositivos de par antes de usá-los se aproveitando Bluetooth RFCOMM (clássico). A partir do Windows 10 versão 1607, você pode simplesmente consultar os dispositivos próximos e conectar-se a eles. O [exemplo do RFCOMM Chat](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) atualizado mostra essa funcionalidade. 

**(14393 e inferior)** Este recurso não está disponível para Bluetooth de Baixa Energia (GATT Client); portanto ainda será necessário emparelhar por meio da página Configurações ou usando as APIs [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) para acessar esses dispositivos.

**(15030 e superior)** O emparelhamento de dispositivos Bluetooth não é mais necessário. Use as novas APIs Assíncronas como GetGattServicesAsync e GetCharacteristicsAsync para consultar o estado atual do dispositivo remoto. Veja os [Documentos de cliente](gatt-client.md) para obter mais detalhes. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quando devo emparelhar com um dispositivo antes de me comunicar com ele?
Em geral, se você precisar de um título confiável e de longo prazo com um dispositivo, o par com ele pelo direcionando o usuário para a página de configurações ou usando as APIs de emparelhamento e a enumeração de dispositivos. Se você simplesmente precisa ler informações do dispositivo que é exposto publicamente (um sensor de temperatura ou beacon), conecte-se ou escutar anúncios sem fazer qualquer esforço para emparelhar com o dispositivo. Isso impedirá a longo prazo, problemas de interoperabilidade porque um grande número de dispositivos não têm suporte para o emparelhamento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Todos os dispositivos Windows dão suporte à Função Periférica?

Não. Esse é um recurso dependentes de hardware, mas um método é fornecido, BluetoothAdapter.IsPeripheralRoleSupported, para consultar se ele é compatível ou não.  Os dispositivos com suporte no momento incluem o Windows Phone no 8992+ e RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Posso acessar essas APIs do Win32?

Sim, todas essas APIs devem funcionar. Este blog detalha a maneira de chamar [APIs do Windows de aplicativos da área de trabalho](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Essa funcionalidade deve existir em *-Inserir SKU aqui-* ?

**Bluetooth LE**: Sim, toda a funcionalidade está em OneCore e deve estar disponível em dispositivos mais recentes com uma pilha de Bluetooth LE está funcionando. 
> Limitação: Função periférica dependentes de hardware e algumas edições do Windows Server não oferece suporte a Bluetooth. 

**Bluetooth BR/EDR (clássico)** : Algumas variações existem, mas principalmente, têm suporte de nível muito semelhante ao perfil. Consulte os documentos sobre [RFCOMM](send-or-receive-files-with-rfcomm.md) e esses documentos para criar o perfil de suporte [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) e [telefone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

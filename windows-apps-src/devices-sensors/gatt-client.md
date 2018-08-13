---
author: msatranjr
title: Cliente de GATT Bluetooth
description: Este artigo fornece uma visão geral do cliente do perfil de atributo genérico Bluetooth (GATT) para aplicativos de plataforma de Windows Universal (UWP), juntamente com o código de exemplo para casos de uso comuns.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304442"
---
# <a name="bluetooth-gatt-client"></a>Cliente de GATT Bluetooth


**APIs importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Este artigo demonstra o uso das APIs do cliente Bluetooth genérico atributo (GATT) para aplicativos de plataforma de Windows Universal (UWP), juntamente com o código de exemplo para tarefas comuns de cliente GATT:
- Consulta dispositivos próximos
- Conectar-se ao dispositivo
- Enumerar os serviços com suporte e as características do dispositivo
- Ler e gravar em uma característica
- Inscrever-se para notificações quando característica o valor é alterado

## <a name="overview"></a>Visão geral
Os desenvolvedores podem usar as APIs no namespace [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para acessar os dispositivos Bluetooth LE. Os dispositivos Bluetooth LE expõem sua funcionalidade por meio de coleção de:

-   Serviços
-   Características
-   Descritores

Serviços definem o contrato funcional do dispositivo LE e contêm um conjunto de características que definem o serviço. Essas características, por sua vez, contêm descritores que descrevem as características. Estes 3 termos forma genérica são conhecidos como os atributos de um dispositivo.

As APIs de GATT Bluetooth LE expor objetos e funções, em vez de acesso ao transporte bruto. As APIs GATT também permitem que os desenvolvedores trabalhar com dispositivos Bluetooth LE com a capacidade de executar as seguintes tarefas:

-   Executar a descoberta de atributo
-   Ler e gravar valores de atributo
-   Registrar um retorno de chamada para o evento ValueChanged característica

Para criar uma implementação útil um desenvolvedor deve ter conhecimento prévio dos serviços de GATT e características que o aplicativo pretende consumir e ao processo de característica específica valores, de forma que os dados binários fornecidos pela API são transformados em dados úteis antes que está sendo apresentado ao usuário. As APIs de GATT de Bluetooth expõem somente primitivas básicas necessárias para comunicação com um dispositivo Bluetooth LE. Para interpretar dados, um perfil de aplicativo deve ser definido, tanto pelo perfil padrão de um Bluetooth SIG, como por um perfil personalizado implementado por um fornecedor de dispositivo. Um perfil cria um contrato de ligação entre o aplicativo e o dispositivo, como o que os dados de intercâmbio representam e como ele os interpreta.

Por conveniência, o Bluetooth SIG mantém uma [lista de perfis públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponíveis.

## <a name="query-for-nearby-devices"></a>Consulta dispositivos próximos
Há dois métodos principais para consultar dispositivos próximos:
- DeviceWatcher em Windows.Devices.Enumeration
- AdvertisementWatcher em Windows.Devices.Bluetooth.Advertisement

O método 2ª é discutido em detalhes na documentação de [anúncio](ble-beacon.md) para que ele não será discutido muito aqui, mas a ideia básica é possível encontrar o endereço de Bluetooth do dispositivos próximos que satisfazem o [Filtro de anúncio](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)de determinado. Depois que você tiver o endereço, é possível chamar [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) para obter uma referência para o dispositivo. 

Agora, de volta para o método DeviceWatcher. Um dispositivo Bluetooth LE é exatamente como qualquer outro dispositivo do Windows e pode ser consultado usando as [APIs de enumeração](https://msdn.microsoft.com/library/windows/apps/BR225459). Use a classe [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) e passe uma sequência de caracteres de consulta especificando os dispositivos para procurar por: 

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```
Depois que você tiver iniciado o DeviceWatcher, você receberá [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo que atenda a consulta no manipulador para o evento [foi adicionado](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) para os dispositivos em questão. Para uma visão mais detalhada sobre DeviceWatcher consulte concluir [Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)de exemplo. 

## <a name="connecting-to-the-device"></a>Conectar-se ao dispositivo
Depois que um dispositivo desejado é descoberto, use o [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) para obter o objeto Bluetooth LE dispositivo para o dispositivo em questão: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Por outro lado, descartar todas as referências a um BluetoothLEDevice objeto para um dispositivo (e se nenhum outro aplicativo no sistema tem uma referência para o dispositivo) irá disparar automático desconectar após um período de tempo limite pequeno. 

```csharp
bluetoothLeDevice.Dispose();
```
Se o aplicativo precisar acessar o dispositivo novamente, simplesmente recriando o objeto de dispositivo e acessando uma característica (discutida na próxima seção) irá disparar o sistema operacional para se conectar novamente quando necessário. Se o dispositivo estiver próximo, você obterá acesso ao dispositivo caso contrário, que ele retornará c / um erro DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumeração serviços com suporte e características
Agora que você tem um objeto BluetoothLEDevice, a próxima etapa é descobrir quais dados expõe o dispositivo. A primeira etapa para fazer isso é para serviços de consulta: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Depois que o serviço de interesse tiver sido identificado, a próxima etapa é para consultar características. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
O sistema operacional retorna que uma lista de somente leitura de GattCharacteristic objetos, em seguida, você pode executar operações em.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Realizar operações de leitura/gravação em uma característica

A característica é que a unidade fundamental de GATT com base em comunicação. Ele contém um valor que representa um dado distinto no dispositivo. Por exemplo, a característica de nível de bateria tem um valor que representa o nível de bateria do dispositivo.

Leia as propriedades características para determinar as operações que são suportadas:
```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

Se houver suporte leitura, você pode ler o valor: 
```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```
A gravação em uma característica segue um padrão semelhante: 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **Dica**: Familiarize-se com usando [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) e [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Sua funcionalidade será indispensável ao trabalhar com os buffers brutos que fazer a partir de muitas das APIs Bluetooth. 
## <a name="subscribing-for-notifications"></a>Inscreva-se para notificações

Verifique se a característica suporta indique ou Notify (verificar as propriedades características para certificar-se). 

> **Reserve**: indicar é considerado mais confiável, pois cada evento de valor alterado é associado a uma confirmação do dispositivo do cliente. Notificar é mais predominante, pois a maioria das transações de GATT seriam preferência seja conservar energia em vez de ser extremamente confiável. Em qualquer caso, tudo isso é manipulado na camada de controlador para que o aplicativo não obter envolvido. Vamos nos coletivamente referir a eles como simplesmente "notificações", mas agora você sabe. 

Existem duas coisas para cuidar dos antes de obter notificações:
- Gravar descritor de característica de configuração do cliente (CCCD)
- Manipular o evento Characteristic.ValueChanged

Gravação de CCCD informa o dispositivo de servidor que esse cliente quer saber que alterações de determinado valor característica de cada vez. Para fazer isso: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Agora, evento de ValueChanged do GattCharacteristic será chamado sempre que o valor obtém alterado no dispositivo remoto. Resta é implementar o manipulador: 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```



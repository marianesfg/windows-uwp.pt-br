---
author: msatranjr
title: Cliente de GATT de Bluetooth
description: Este artigo fornece uma visão geral do cliente de perfil de atributo genérico (GATT) Bluetooth para aplicativos da plataforma Universal do Windows (UWP), juntamente com o código de exemplo para casos de uso comuns.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 345e6f82ddf97c2595dad0029ca432f075a6190b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572699"
---
# <a name="bluetooth-gatt-client"></a>Cliente de GATT de Bluetooth


**APIs importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Este artigo demonstra o uso das APIs de cliente do Bluetooth GATT (atributo genérico) para aplicativos da plataforma Universal do Windows (UWP), juntamente com o código de amostra para tarefas de cliente GATT comuns:
- Consulta para dispositivos próximos
- Se conectar ao dispositivo
- Enumerar os serviços com suporte e características do dispositivo
- Ler e gravar uma característica
- Inscrever-se para alterações de valor de notificações quando característica

## <a name="overview"></a>Visão geral
Os desenvolvedores podem usar as APIs no namespace [**genericattributeprofile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para acessar dispositivos Bluetooth LE. Os dispositivos Bluetooth LE expõem sua funcionalidade por meio de coleção de:

-   Serviços
-   Características
-   Descritores

Serviços definem o contrato funcional do dispositivo LE e contêm uma coleção de características que definem o serviço. Essas características, por sua vez, contêm descritores que descrevem as características. Esses 3 termos são genericamente conhecidos como os atributos de um dispositivo.

As APIs de GATT Bluetooth LE expõem objetos e funções, em vez de acessarem o transporte bruto. As APIs de GATT também permitem que os desenvolvedores trabalhar com dispositivos Bluetooth LE com a capacidade de executar as seguintes tarefas:

-   Executar descoberta de atributo
-   Ler e gravar valores de atributo
-   Registrar um retorno de chamada para o evento ValueChanged de característica

Para criar uma implementação útil, um desenvolvedor deve ter conhecimento prévio os serviços GATT e características que o aplicativo pretende consumir e para processar a característica específica valores, de forma que os dados binários fornecidos pela API são transformados em dados úteis antes de serem apresentados ao usuário. As APIs de GATT de Bluetooth expõem somente primitivas básicas necessárias para comunicação com um dispositivo Bluetooth LE. Para interpretar dados, um perfil de aplicativo deve ser definido, tanto pelo perfil padrão de um Bluetooth SIG, como por um perfil personalizado implementado por um fornecedor de dispositivo. Um perfil cria um contrato de ligação entre o aplicativo e o dispositivo, como o que os dados de intercâmbio representam e como ele os interpreta.

Por conveniência, o Bluetooth SIG mantém uma [lista de perfis públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponíveis.

## <a name="query-for-nearby-devices"></a>Consulta para dispositivos próximos
Há dois métodos principais para consultar os dispositivos próximos:
- DeviceWatcher em Enumeration
- APIs AdvertisementWatcher em Advertisement

O método 2º é discutido em detalhes na documentação do [anúncio](ble-beacon.md) para que ele não será discutido muito aqui, mas a ideia básica é encontrar o endereço de Bluetooth de dispositivos próximos que satisfazem o [Filtro de anúncio](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)específico. Quando você tiver o endereço, você pode chamar [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) para obter uma referência para o dispositivo. 

Agora, de volta para o método DeviceWatcher. Um dispositivo Bluetooth LE é exatamente igual a qualquer outro dispositivo no Windows e pode ser consultado usando as [APIs de enumeração](https://msdn.microsoft.com/library/windows/apps/BR225459). Use a classe [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) e passe uma cadeia de caracteres de consulta especificando os dispositivos para procurar: 

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
Depois que você já começou a DeviceWatcher, você receberá [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo que atenda a consulta no manipulador para o evento [adicionado](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) para os dispositivos em questão. Para uma visão mais detalhada DeviceWatcher consulte completo de amostra [no Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Conectando-se ao dispositivo
Depois que um dispositivo desejado é descoberto, use o [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) para obter o objeto de dispositivo Bluetooth LE para o dispositivo em questão: 

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
Se o aplicativo precisa acessar o dispositivo novamente, simplesmente recriar o objeto de dispositivo e acessar uma característica (explicada na próxima seção) irá disparar o sistema operacional para se conectar novamente quando necessário. Se o dispositivo está próximo, você terá acesso ao dispositivo; caso contrário, que ele retornará com um erro de DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumerando características e serviços com suporte
Agora que você tem um objeto BluetoothLEDevice, a próxima etapa é descobrir quais dados expõe o dispositivo. A primeira etapa para fazer isso é para consultar os serviços: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Depois que o serviço de interesse foi identificado, a próxima etapa é consultar características. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
O sistema operacional retorna que uma lista somente leitura de GattCharacteristic objetos que, em seguida, você pode executar operações em.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Executar operações de leitura/gravação em uma característica

A característica é a unidade fundamental de GATT com base em comunicação. Ele contém um valor que represente um pedaço distinto de dados no dispositivo. Por exemplo, a característica de nível de bateria tem um valor que representa o nível de bateria do dispositivo.

Leia as propriedades de característica para determinar quais operações são suportadas:
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

Se houver suporte para a leitura, você pode ler o valor: 
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
Gravar uma característica segue um padrão semelhante: 
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
> **Dica**: Familiarize-se com usando [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) e [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Sua funcionalidade será indispensável ao trabalhar com os buffers brutos obtido muitas das APIs Bluetooth. 
## <a name="subscribing-for-notifications"></a>Inscrever-se para notificações

Verifique se a característica suporta indicar ou notificar (Verifique as propriedades características para certificar-se). 

> **Reserve**: indicar é considerada mais confiável porque cada evento de valor alterado é combinado com uma confirmação do dispositivo cliente. Notificar é mais predominante porque a maioria das transações de GATT seriam em vez disso, economizar energia em vez de ser extremamente confiável. Em qualquer caso, tudo isso é manipulado na camada do controlador para que o aplicativo não se envolve. Vamos coletivamente nos referir a eles como simplesmente "notificações", mas agora você sabe. 

Há dois coisas para cuidar antes recebendo notificações:
- Gravar no descritor de característica de configuração de cliente (CCCD)
- Manipular o evento Characteristic.ValueChanged

A gravação do CCCD informa o dispositivo de servidor que esse cliente quer saber que alterações de valor característica específico de cada vez. Para fazer isso: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Agora, evento ValueChanged do GattCharacteristic será chamado sempre que o valor é alterado no dispositivo remoto. Tudo o que resta é implementar o manipulador: 

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



---
title: Cliente GATT do Bluetooth
description: Este artigo fornece uma visão geral do Cliente GATT (Perfil de Atributo Genérico) de Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para casos de uso comuns.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5c17351cf964ffb05dc60dbaf5c6ced1db467f78
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469531"
---
# <a name="bluetooth-gatt-client"></a>Cliente GATT do Bluetooth

Este artigo demonstra o uso das APIs de Cliente GATT (Atributo Genérico) do Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para tarefas comuns de cliente GATT:

- Consulta de dispositivos próximos
- Conectar ao dispositivo
- Enumerar os serviços e as características do dispositivo com suporte
- Ler e gravar em uma característica
- Assinar notificações de quando o valor da característica é alterado

> [!Important]
> Você deve declarar o recurso "Bluetooth" em *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **APIs importantes**
>
> - [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows. Devices. Bluetooth. GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Visão geral

Os desenvolvedores podem usar as APIs no namespace [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) para acessar serviços, descritores e características de dispositivos Bluetooth LE. Os dispositivos Bluetooth LE expõem sua funcionalidade por meio de coleção de:

- Serviços
- Características
- Descritores

Os serviços definem o contrato funcional do dispositivo LE e contêm uma coleção de características que definem o serviço. Essas características, por sua vez, contêm descritores que descrevem as características. Estes três termos são conhecidos genericamente como os atributos de um dispositivo.

As APIs de GATT de Bluetooth LE expõem objetos e funções, em vez de acessarem o transporte bruto. As APIs de GATT também permitem que os desenvolvedores usem dispositivos Bluetooth LE com a capacidade de executar as seguintes tarefas:

- Executar a descoberta de atributos
- Ler e Gravar valores de atributo
- Registrar um retorno de chamada para o evento ValueChanged de Characteristic

Para criar uma implementação útil, um desenvolvedor deve antes conhecer os serviços GATT e características que o aplicativo pretende consumir, e para processar os valores característicos específicos, como aqueles dados binários fornecidos pela API, são transformados em dados úteis antes de serem apresentados ao usuário. As APIs de GATT de Bluetooth expõem somente primitivas básicas necessárias para comunicação com um dispositivo Bluetooth LE. Para interpretar dados, um perfil de aplicativo deve ser definido, tanto pelo perfil padrão de um Bluetooth SIG, como por um perfil personalizado implementado por um fornecedor de dispositivo. Um perfil cria um contrato de ligação entre o aplicativo e o dispositivo, como o que os dados de intercâmbio representam e como ele os interpreta.

Por conveniência, o Bluetooth SIG mantém uma [lista de perfis públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponíveis.

## <a name="query-for-nearby-devices"></a>Consulta de dispositivos próximos

Há dois métodos principais para consultar os dispositivos próximos:

- DeviceWatcher em Windows.Devices.Enumeration
- AdvertisementWatcher em Windows.Devices.Bluetooth.Advertisement

O segundo método será discutido em detalhes na documentação [Anúncio](ble-beacon.md) e, portanto, não será mencionado aqui, mas a ideia básica é encontrar o endereço Bluetooth de dispositivos próximos que atendam ao [Filtro de Anúncio](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter) em particular. Quando você tiver o endereço, poderá chamar [BluetoothLEDevice.FromBluetoothAddressAsync](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) para obter uma referência ao dispositivo.

Agora, de volta ao método DeviceWatcher. Um dispositivo Bluetooth LE é como qualquer outro dispositivo no Windows e pode ser consultado usando as [APIs de enumeração](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Use a classe [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) e passe uma cadeia de caracteres de consulta que especifique os dispositivos a serem procurados:

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

Depois de iniciar o DeviceWatcher, você receberá [DeviceInformation](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para cada dispositivo que atenda à consulta no manipulador para o evento [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) para os dispositivos em questão. Para obter uma visão mais detalhada do DeviceWatcher, veja o exemplo completo [no Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="connecting-to-the-device"></a>Conexão ao dispositivo

Depois que um dispositivo desejado for descoberto, use a [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) para obter o objeto do Dispositivo Bluetooth LE para o dispositivo em questão:

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

Por outro lado, descartar todas as referências a um objeto BluetoothLEDevice para um dispositivo (e se nenhum outro aplicativo no sistema tiver uma referência ao dispositivo) disparará uma desconexão automática após um breve período de tempo limite.

```csharp
bluetoothLeDevice.Dispose();
```

Se o aplicativo precisar acessar o dispositivo novamente, a simples recriação do objeto de dispositivo e o acesso a uma característica (discutida na próxima seção) acionarão o sistema operacional para que ele se conecte novamente quando necessário. Se o dispositivo estiver próximo, você terá acesso ao dispositivo; caso contrário, ele retornará com um erro DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumeração de serviços e características com suporte

Agora que você tem um objeto BluetoothLEDevice, a próxima etapa será descobrir quais dados são expostos pelo dispositivo. A primeira etapa para fazer isso é consultar serviços:

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

Depois que o serviço desejado tiver sido identificado, a próxima etapa será consultar características.

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

O sistema operacional retorna uma lista somente leitura de objetos GattCharacteristic em que você poderá executar operações.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Executar operações de Leitura/Gravação em uma característica

A característica é a unidade fundamental da comunicação baseada em GATT. Ela contém um valor que representa uma parte distinta dos dados no dispositivo. Por exemplo, a característica de nível de bateria tem um valor que representa o nível da bateria do dispositivo.

Leia as propriedades das características para determinar quais operações têm suporte:

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

Se houver suporte para leitura, você poderá ler o valor:

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

Gravar em uma característica segue um padrão semelhante:

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **Dica**: o [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) e o [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) são indispensáveis ao trabalhar com os buffers brutos obtidos de muitas APIs do Bluetooth.

## <a name="subscribing-for-notifications"></a>Assinatura de notificações

Verifique se a característica dá suporte a Indicar ou Notificar (verifique as propriedades das características para assegurar-se).

> **Aparte**: Indicar é considerado mais confiável porque cada evento de valor alterado é associado a uma confirmação do dispositivo cliente. Notificar é predominante porque a maioria das transações de GATT conserva energia em vez de ser extremamente confiável. Em qualquer caso, tudo isso é manipulado na camada do controlador para que o aplicativo não se envolva. Vamos nos referir a eles coletivamente como simples "notificações", mas agora você sabe.

Há duas coisas que devem ser consideradas antes de receber notificações:

- Gravar no CCCD (Descritor de Configuração de Característica do Cliente)
- Manipular o evento Characteristic.ValueChanged

Gravar no CCCD informa ao dispositivo Servidor que esse cliente quer ser informado sobre cada alteração de valor daquela característica em particular. Para fazer isso:

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

Agora, evento ValueChanged da GattCharacteristic será chamado sempre que o valor for alterado no dispositivo remoto. Tudo o que está à esquerda serve para implementar o manipulador:

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```

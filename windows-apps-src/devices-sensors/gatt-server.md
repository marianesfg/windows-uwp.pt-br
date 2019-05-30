---
title: Servidor GATT do Bluetooth
description: Este artigo fornece uma visão geral do Servidor GATT (Perfil de Atributo Genérico) de Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para casos de uso comuns.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f59ae45486ee72f9d901898f6b03674e6b3e299c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370089"
---
# <a name="bluetooth-gatt-server"></a>Servidor GATT do Bluetooth


**APIs importantes**
- [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)


Este artigo demonstra as APIs de Servidor GATT (Atributo Genérico) do Bluetooth para aplicativos UWP (Plataforma Universal do Windows), juntamente com o código de exemplo para tarefas comuns de servidor GATT: 
- Definir os serviços com suporte
- Publicar o servidor para que ele possa ser descoberto por clientes remotos
- Anunciar o suporte para o serviço
- Responder a solicitações de leitura e gravação
- Enviar notificações aos clientes inscritos

## <a name="overview"></a>Visão geral
Geralmente, o Windows opera na função de cliente. No entanto, surgem muitos cenários que exigem que o Windows também atue como um servidor GATT do Bluetooth LE. Quase todos os cenários para dispositivos IoT, juntamente com a maioria das comunicações BLE de plataforma cruzada exigirão que o Windows seja um servidor GATT. Além disso, o envio de notificações para os dispositivos acessórios próximos tornou-se um cenário popular que também exige essa tecnologia.  
> Verifique se todos os conceitos dos [documentos de Cliente GATT](gatt-client.md) estão claros antes de prosseguir.  

As operações de servidor girarão em torno de Service Provider e de GattLocalCharacteristic. Essas duas classes fornecerá a funcionalidade necessária para declarar, implementar e expor uma hierarquia de dados a um dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir os serviços com suporte
Seu aplicativo pode declarar um ou mais serviços que serão publicados pelo Windows. Cada serviço é exclusivamente identificado por um UUID. 

### <a name="attributes-and-uuids"></a>Atributos e UUIDs
Cada serviço, característica e descritor é definido por seu próprio UUID exclusivo de 128 bits.
> Todas as APIs do Windows usam o termo GUID, mas o padrão Bluetooth as define como UUIDs. Para os nossos objetivos, esses dois termos são intercambiáveis e, portanto, continuaremos a usar o termo UUID. 

Se o atributo for padrão e definido pela definido pelo Bluetooth SIG, ele também terá uma ID curta de 16 bits correspondente (por exemplo, o UUID Nível de Bateria é do nível de bateria é 0000**2A19**-0000-1000-8000-00805F9B34FB e a ID curta é 0x2A19). Esses UUIDs padrão podem ser vistos em [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) e em [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids).

Se seu app estiver implementando seu próprio serviço personalizado, um UUID personalizado deverá ser gerado. Isso é facilmente obtido no Visual Studio por meio de Ferramentas -> CreateGuid (use a opção 5 para obtê-lo no formato "xxxxxxxx-xxxx-...xxxx"). Agora este uuid pode ser usado para declarar novos serviços, características ou descritores locais.

#### <a name="restricted-services"></a>Serviços Restritos
Os seguintes Serviços são reservados pelo sistema e não podem ser publicados no momento:
1. DIS (Serviço de Informações de Dispositivo)
2. GATT (Serviço de Perfil de Atributo Genérico)
3. GAP (Serviço de Perfil de Acesso Genérico)
4. HOGP (Serviço de Dispositivos de Interface Humana)
5. SCP (Serviço de Parâmetros de Digitalização)

> A tentativa de criar um serviço bloqueado resultará em BluetoothError.DisabledByPolicy sendo retornado da chamada a CreateAsync.

#### <a name="generated-attributes"></a>Atributos gerados
Os descritores de seguir são gerados automaticamente pelo sistema, com base nos GattLocalCharacteristicParameters fornecidos durante a criação da característica:
1. Configuração de Característica do Cliente (se a característica estiver marcada como indicatable ou notifiable).
2. Descrição de Usuário de Característica (se a propriedade UserDescription estiver definida). Consulte a propriedade GattLocalCharacteristicParameters.UserDescription para saber mais.
3. Formato da Característica (um descritor para cada formato de apresentação especificado).  Consulte a propriedade GattLocalCharacteristicParameters.PresentationFormats para saber mais.
4. Formato Agregado de Característica (se mais de um formato de apresentação for especificado).  Consulte a propriedade GattLocalCharacteristicParameters.See PresentationFormats para saber mais.
5. Propriedades Estendidas da Característica (se a característica for marcada com o bit de propriedades estendidas).

> O valor do descritor Propriedades Estendidas é determinado por meio das propriedades de característica ReliableWrites e WritableAuxiliaries.

> A tentativa de criar um descritor reservado resultará em uma exceção.

> Observe que a difusão não tem suporte no momento.  A especificação da GattCharacteristicProperty de Broadcast resultará em uma exceção.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Criar a hierarquia de serviços e características
O GattServiceProvider é usado para criar e anunciar a definição de serviço primário raiz.  Cada serviço exige seu próprio objeto ServiceProvider, que recebe um GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Os serviços primários são o nível superior da árvore de GATT. Os serviços primários contêm características, bem como outros serviços (chamados de serviços 'Incluídos' ou secundários). 

Agora, preencha o serviço com as características e os descritores necessários:

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
Como mostrado acima, esse também é um bom lugar para declarar os manipuladores de eventos para as operações com suporte de cada característica.  Para responder às solicitações corretamente, um app deve definir um manipulador de eventos para cada tipo de solicitação com suporte do atributo.  A falha em registrar um manipulador resultará na conclusão imediata da solicitação com *UnlikelyError* feita pelo sistema.

### <a name="constant-characteristics"></a>Características constantes
Às vezes, há valores de característica que não serão alterados no decorrer do tempo de vida do aplicativo. Nesse caso, é aconselhável declarar uma característica constante para evitar a ativação desnecessária do app: 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>Publicar o serviço
Depois que o serviço tiver sido totalmente definido, a próxima etapa será publicar o suporte para o serviço. Isso informa ao sistema operacional que o serviço deve ser retornado quando dispositivos remotos executarem uma descoberta de serviço.  Você precisará definir duas propriedades - IsDiscoverable e IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: Anuncia o nome amigável para dispositivos remotos no anúncio, possibilitando o dispositivo possa ser descoberto.
- **IsConnectable**:  Anuncia um anúncio conectável para uso na função periférico.

> Quando um serviço for detectável e conectável, o sistema irá adicionar o Uuid do Serviço ao pacote de anúncio.  Há apenas 31 bytes no pacote de anúncio e um UUID de 128 bits ocupa 16 deles!

> Observe que quando um serviço é publicado em primeiro plano, um aplicativo deverá chamar StopAdvertising quando for suspenso.

## <a name="respond-to-read-and-write-requests"></a>Responder a solicitações de leitura e gravação
Como vimos acima durante a declaração das características necessárias, GattLocalCharacteristics tem três tipos de eventos - ReadRequested, WriteRequested e SubscribedClientsChanged.

### <a name="read"></a>Read
Quando um dispositivo remoto tenta ler um valor de uma característica (e não é um valor constante), o evento ReadRequested é chamado. A característica de que leitura foi chamada, bem como os argumentos (com as informações sobre o dispositivo remoto), são passados para o representante: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Gravação
Quando um dispositivo remoto tenta gravar um valor de uma característica, o evento WriteRequested é chamado com detalhes sobre o dispositivo remoto, em qual característica será feita a gravação e o próprio valor: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
Há dois tipos de gravações - com e sem resposta. Use GattWriteOption (uma propriedade do objeto GattWriteRequest) para descobrir qual tipo de gravação que o dispositivo remoto está realizando. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificações aos clientes inscritos
As operações mais frequentes do Servidor GATT, as notificações realizam a função crítica de envio de dados para os dispositivos remotos. Às vezes, você vai querer notificar todos os clientes inscritos mas em outras talvez convenha selecionar os dispositivos para os quais o novo valor será enviado: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Quando um novo dispositivo se inscreve para notificações, o evento SubscribedClientsChanged é chamado: 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> Observe que um aplicativo pode obter o tamanho máximo de notificação para um determinado cliente com a propriedade MaxNotificationSize.  Quaisquer dados maiores do que o tamanho máximo serão truncados pelo sistema.

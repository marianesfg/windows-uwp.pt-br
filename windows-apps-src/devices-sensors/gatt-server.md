---
author: msatranjr
title: Servidor de GATT de Bluetooth
description: Este artigo fornece uma visão geral do servidor de perfil de atributo genérico (GATT) Bluetooth para aplicativos de plataforma Universal do Windows (UWP), juntamente com o código de exemplo para casos de uso comuns.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8a941b7b80bd5d34e88798ec586d9c1d52e2887
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5994163"
---
# <a name="bluetooth-gatt-server"></a>Servidor de GATT de Bluetooth


**APIs importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


Este artigo demonstra APIs de servidor de Bluetooth GATT (atributo genérico) para aplicativos de plataforma Universal do Windows (UWP), juntamente com o código de exemplo para tarefas comuns de servidor GATT: 
- Definir os serviços com suporte
- Publicar o servidor para que ele possa ser descoberto por clientes remotos
- Anunciar o suporte para serviço
- Responder a solicitações de gravação
- Enviar notificações aos clientes inscritos

## <a name="overview"></a>Visão geral
Geralmente, o Windows opera na função de cliente. No entanto, muitos cenários surgirem que exigem o Windows atuar como um servidor de GATT de LE de Bluetooth também. Quase todos os cenários para dispositivos IoT, juntamente com a maioria dos comunicação de BLE de plataforma cruzada exigirá Windows para um servidor GATT. Além disso, enviar notificações para dispositivos próximos acessório tornou-se um cenário popular que requeira essa tecnologia também.  
> Verifique se que todos os conceitos do [cliente GATT documentos](gatt-client.md) são claros antes de prosseguir.  

Operações do servidor serão giram em torno do provedor de serviços e o GattLocalCharacteristic. Essas duas classes fornecerá a funcionalidade necessária para declarar, implementar e expor uma hierarquia de dados em um dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir os serviços com suporte
Seu aplicativo pode declarar um ou mais serviços que serão publicados pelo Windows. Cada serviço é identificado exclusivamente por um UUID. 

### <a name="attributes-and-uuids"></a>Atributos e UUIDs
Cada serviço, característica e descritor é definido por é próprio UUID de 128 bits exclusivo.
> Todas as APIs do Windows usa o termo GUID, mas o padrão de Bluetooth define encontram UUIDs. Para os nossos objetivos, esses dois termos são intercambiáveis, portanto, continuaremos a usar o termo UUID. 

Se o atributo é definido pelo definido Bluetooth SIG e padrão, ele também tem uma ID de curta de 16 bits correspondente (por exemplo, o UUID do nível de bateria é 0000**2A19**-0000-1000-8000-00805F9B34FB e a ID curta é 0x2A19). Esses UUIDs padrão podem ser vistos no [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) e [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Se seu aplicativo está implementando é próprio serviço personalizado, terá um UUID personalizado a ser gerado. Isso é facilmente no Visual Studio por meio de ferramentas -> CreateGuid (opção de usar 5 obtê-lo no formato "xxxxxxxx-xxxx-... xxxx"). Este uuid agora pode ser usado para declarar novos serviços locais, características ou descritores.

#### <a name="restricted-services"></a>Serviços restritos
Os seguintes serviços são reservados pelo sistema e não podem ser publicados no momento:
1. Serviço de informações do dispositivo (distribuição)
2. Serviço de perfil de atributo genérico (GATT)
3. Serviço de perfil de acesso genérico (intervalo)
4. Serviço de dispositivo de Interface Humana (HOGP)
5. Verificar os parâmetros de serviço (SCP)

> Tentativa de criar um serviço bloqueado resultará em BluetoothError.DisabledByPolicy sendo retornado da chamada para CreateAsync.

#### <a name="generated-attributes"></a>Atributos gerados
Os seguintes descritores são geradas pelo sistema, com base no GattLocalCharacteristicParameters fornecido durante a criação da característica:
1. Configuração de característica do cliente (se a característica é marcada como indicatable ou notifiable).
2. Característica descrição de usuário (se a propriedade UserDescription estiver definida). Consulte a propriedade GattLocalCharacteristicParameters.UserDescription para obter mais informações.
3. Formato de característica (um descritor para cada formato de apresentação especificado).  Consulte a propriedade GattLocalCharacteristicParameters.PresentationFormats para obter mais informações.
4. Característica agregados formato de (se mais de um formato de apresentação é especificado).  Propriedade GattLocalCharacteristicParameters.See PresentationFormats para obter mais informações.
5. Característica propriedades estendidas (se a característica é marcada com o bit de propriedades estendidas).

> O valor do descritor de propriedades estendidas é determinado por meio das propriedades de característica ReliableWrites e WritableAuxiliaries.

> Tentativa de criar um descritor reservado resultará em uma exceção.

> Observe que a difusão não é suportada no momento.  Especificar o GattCharacteristicProperty transmissão resultará em uma exceção.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Criar a hierarquia de serviços e características
O GattServiceProvider é usado para criar e anunciar a definição do serviço principal raiz.  Cada serviço requer o próprio objeto de provedor de serviços que recebe um GUID é: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Serviços primários são o nível superior da árvore de GATT. Serviços primários contêm características, bem como outros serviços (chamados 'Incluídas' ou serviços secundários). 

Agora, preencha o serviço com as características necessárias e os descritores:

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
Conforme mostrado acima, isso também é um bom lugar para declarar os manipuladores de eventos para as operações que dá suporte a cada característica.  Para responder às solicitações corretamente, um aplicativo deve definidos e definir um manipulador de eventos para cada tipo de solicitação é compatível com o atributo.  Falha ao registrar um manipulador resultará na solicitação está sendo concluída imediatamente com *UnlikelyError* pelo sistema.

### <a name="constant-characteristics"></a>Características constantes
Às vezes, há características valores que não serão alterada durante o tempo de vida do aplicativo. Nesse caso, é aconselhável declarar uma característica constante para impedir que a ativação do aplicativo desnecessários: 

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
Depois que o serviço tiver sido totalmente definido, a próxima etapa é publicar o suporte para o serviço. Isso informa o sistema operacional que o serviço deve ser retornado quando dispositivos remotos executam uma descoberta de serviço.  Você precisará definir duas propriedades - IsDiscoverable e IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: anuncia o nome amigável a dispositivos remotos no anúncio, tornando o dispositivo detectável.
- **IsConnectable**: anuncia um anúncio conectável para uso na função periférica.

> Quando um serviço é detectável e Connectable, o sistema adicionará o Uuid do serviço para o pacote de anúncio.  Há apenas 31 bytes no pacote de anúncio e um UUID de 128 bits ocupa 16 deles!

> Observe que, quando um serviço é publicado em primeiro plano, um aplicativo deve chamar StopAdvertising quando suspende o aplicativo.

## <a name="respond-to-read-and-write-requests"></a>Responder a solicitações de gravação
Como vimos acima enquanto declarando as características necessárias, GattLocalCharacteristics ter 3 tipos de eventos - ReadRequested, WriteRequested e SubscribedClientsChanged.

### <a name="read"></a>Leitura
Quando um dispositivo remoto tenta ler um valor de uma característica (e não é um valor constante), o evento ReadRequested é chamado. A característica que a leitura foi chamada em, bem como argumentos (que contém informações sobre o dispositivo remoto) é passada para o representante: 

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
Quando um dispositivo remoto tenta gravar um valor para uma característica, o evento WriteRequested é chamado com detalhes sobre o dispositivo remoto, quais característica para gravar e o próprio valor: 

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
Há 2 tipos de gravações - com e sem resposta. Use GattWriteOption (uma propriedade no objeto GattWriteRequest) para descobrir qual tipo de gravação do dispositivo remoto está realizando. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificações aos clientes inscritos
As operações de servidor GATT, notificações mais frequentes executam a função crítica pode enviar dados para os dispositivos remotos de. Às vezes, você vai querer notificar todos os clientes inscritos mas othertimes que talvez você queira escolher quais dispositivos para enviar o novo valor: 

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
> Observe que um aplicativo pode obter o tamanho máximo de notificação para um determinado cliente com a propriedade MaxNotificationSize.  Todos os dados maiores que o tamanho máximo serão truncados pelo sistema.

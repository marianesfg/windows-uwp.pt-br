---
author: msatranjr
title: Servidor de GATT Bluetooth
description: Este artigo fornece uma visão geral do servidor de perfil de atributo genérico Bluetooth (GATT) para aplicativos de plataforma de Windows Universal (UWP), juntamente com o código de exemplo para casos de uso comuns.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610761"
---
# <a name="bluetooth-gatt-server"></a>Servidor de GATT Bluetooth


**APIs importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


Este artigo demonstra as APIs de servidor Bluetooth genérico atributo (GATT) para aplicativos de plataforma de Windows Universal (UWP), juntamente com o código de exemplo para tarefas comuns de servidor GATT: 
- Definem os serviços com suporte
- Publicar o servidor para que ele possa ser descoberto por clientes remotos
- Anunciar o suporte para o serviço
- Responder para ler e gravar solicitações
- Enviar notificações aos clientes inscritos

## <a name="overview"></a>Visão geral
Geralmente, o Windows opera à função de cliente. No entanto, muitos cenários surgem que exigem o Windows agir como um servidor de GATT de LE de Bluetooth também. Quase todos os cenários para os dispositivos IoT, juntamente com a maioria das comunicações de var plataforma cruzada exigirá Windows a ser um servidor GATT. Além disso, o envio de notificações para dispositivos wearable próximos tornou um cenário popular que exija essa tecnologia também.  
> Verifique se todos os conceitos do [cliente GATT docs](gatt-client.md) claro antes de continuar.  

As operações do servidor irão girar em torno do provedor de serviços e o GattLocalCharacteristic. Essas duas classes fornecerá a funcionalidade necessária para declarar, implementar e expor uma hierarquia de dados para um dispositivo remoto.

## <a name="define-the-supported-services"></a>Definem os serviços com suporte
Seu aplicativo pode declarar um ou mais serviços que serão publicados pelo Windows. Cada serviço é identificado exclusivamente por um UUID. 

### <a name="attributes-and-uuids"></a>Atributos e UUIDs
Cada serviço, característica e descritor é definido por é próprio exclusivo UUID de 128 bits.
> Todas as APIs do Windows usar o GUID do termo, mas o padrão de Bluetooth define essas áreas como UUIDs. Para nossos objetivos, esses dois termos são intercambiáveis para que continuará a usar o termo UUID. 

Se o atributo for definido pela definido Bluetooth SIG e padrão, ele também terá uma ID de curta de 16 bits correspondente (por exemplo, UUID de nível de bateria é 0000**2A19**-0000-1000-8000-00805F9B34FB e a ID de curta é 0x2A19). Esses UUIDs padrão podem ser vistos em [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) e [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Se seu aplicativo está implementando é próprio serviço personalizado, um UUID personalizado terá a serem gerados. Isso é facilmente no Visual Studio por meio de ferramentas -> CreateGuid (opção de uso de 5 para obtê-lo no formato "xxxxxxxx-xxxx-… xxxx"). Este uuid agora pode ser usada para declarar os novos serviços locais, características ou descritores.

#### <a name="restricted-services"></a>Serviços restritos
Os seguintes serviços são reservados pelo sistema e não podem ser publicados neste momento:
1. Serviço de informações do dispositivo (distribuição)
2. Serviço de perfil de atributo genérico (GATT)
3. Serviço de perfil de acesso genérico (GAP)
4. Serviço de dispositivo de Interface Humana (HOGP)
5. Examinar os parâmetros de serviço (SCP)

> Tentando criar um serviço bloqueado resultará em BluetoothError.DisabledByPolicy sendo retornados da chamada para CreateAsync.

#### <a name="generated-attributes"></a>Atributos gerados
Os seguintes descritores são geradas pelo sistema, com base no GattLocalCharacteristicParameters fornecido durante a criação dessas características:
1. Configuração de característica do cliente (se a característica é marcada como indicatable ou notifiable).
2. Característica descrição de usuário (se a propriedade UserDescription está definida). Consulte a propriedade GattLocalCharacteristicParameters.UserDescription para obter mais informações.
3. Formato de característica (descritor de um para cada formato de apresentação especificado).  Consulte a propriedade GattLocalCharacteristicParameters.PresentationFormats para obter mais informações.
4. Característica agregadas formato, (se mais de um formato de apresentação for especificado).  Propriedade GattLocalCharacteristicParameters.See PresentationFormats para obter mais informações.
5. Característica propriedades estendidas (se a característica é marcada com o bit de propriedades estendidas).

> O valor do descritor de propriedades estendidas é determinado por meio das propriedades de característica ReliableWrites e WritableAuxiliaries.

> Tentando criar um descritor reservado resultará em uma exceção.

> Observe que a difusão não é suportada no momento.  Especificar o GattCharacteristicProperty transmissão resultará em uma exceção.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Criar a hierarquia de serviços e as características
O GattServiceProvider é usado para criar e anunciar a definição de serviço primário de raiz.  Cada serviço exige que o próprio objeto de provedor de serviços que leva em um GUID é: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Principais serviços são o nível superior da árvore GATT. Principais serviços contêm características, assim como outros serviços (chamados 'Included' ou serviços secundários). 

Agora, preencha o serviço com as características necessárias e descritores:

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
Como mostrado acima, isso também é um bom lugar para declarar manipuladores de eventos para as operações que oferece suporte a cada característica.  Para responder às solicitações corretamente, um aplicativo deve definidas e definir um manipulador de eventos para cada tipo de solicitação que o atributo oferece suporte.  Com falha registrar um manipulador resultará na solicitação sejam concluída imediatamente com *UnlikelyError* pelo sistema.

### <a name="constant-characteristics"></a>Características constantes
Às vezes, há característicos valores que não serão alterado no decorrer do tempo de vida do aplicativo. Nesse caso, é aconselhável declarar uma característica constante para impedir que a ativação desnecessários app: 

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
- **IsDiscoverable**: anuncia o nome amigável a dispositivos remotos no anúncio, tornando-o dispositivo detectável.
- **IsConnectable**: anuncia um anúncio conectável para uso na função periférico.

> Quando um serviço é detectável e Connectable, o sistema irá adicionar o Uuid de serviço para o pacote de anúncio.  Existem apenas 31 bytes no pacote de anúncio e um UUID de 128 bits ocupa 16 deles!

> Observe que, quando um serviço é publicado em primeiro plano, um aplicativo deve chamar StopAdvertising quando o aplicativo suspende.

## <a name="respond-to-read-and-write-requests"></a>Responder para ler e gravar solicitações
Como vimos acima enquanto declarando as características necessárias, GattLocalCharacteristics ter 3 tipos de eventos - ReadRequested, WriteRequested e SubscribedClientsChanged.

### <a name="read"></a>Leitura
Quando um dispositivo remoto tenta ler um valor de uma característica (e não é um valor de constante), o evento ReadRequested é chamado. A característica que leia foi chamado em, bem como args (contendo informações sobre o dispositivo remoto) é passada para o delegado: 

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
Quando um dispositivo remoto tenta gravar um valor em uma característica, o evento WriteRequested é chamado com detalhes sobre o dispositivo remoto, qual característica para gravar e o próprio valor: 

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
Há 2 tipos de gravações - com e sem resposta. Use GattWriteOption (uma propriedade no objeto GattWriteRequest) para descobrir qual tipo de gravação do dispositivo remoto está executando. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificações aos clientes inscritos
Mais frequentes das operações GATT Server, as notificações de realizam a função crítica de envio de dados para os dispositivos remotos. Às vezes, você vai querer notificar todos os clientes inscritos mas othertimes, para que talvez você queira escolher quais dispositivos para enviar o novo valor: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Quando um novo dispositivo se inscreve para notificações, o evento SubscribedClientsChanged obtém chamado: 

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
> Observe que um aplicativo pode obter o tamanho máximo de notificação para um cliente específico com a propriedade MaxNotificationSize.  Quaisquer dados maiores que o tamanho máximo serão truncados pelo sistema.

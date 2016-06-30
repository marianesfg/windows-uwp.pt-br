---
author: msatranjr
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: "Este artigo apresenta uma visão geral do Bluetooth RFCOMM em aplicativos da Plataforma Universal do Windows (UWP), além do código de exemplo sobre como enviar ou receber um arquivo."
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: 61e5844f18d09aa170498d261ca1a6fd60ef170c

---
# Bluetooth RFCOMM

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529)

Este artigo apresenta uma visão geral do Bluetooth RFCOMM em aplicativos da Plataforma Universal do Windows (UWP), além do código de exemplo sobre como enviar ou receber um arquivo.

## Visão geral

As APIs no namespace [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) são compiladas nos padrões existentes para Dispositivos Windows, incluindo [**enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) e [**instantiation**](https://msdn.microsoft.com/library/windows/apps/BR225654). A leitura e a gravação de dados é projetada para tirar vantagens de [**padrões de fluxo de dados estabelecidos**](https://msdn.microsoft.com/library/windows/apps/BR208119) e objetos em [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/BR241791). Os atributos do Service Discovery Protocol (SDP) têm um valor e um tipo esperado. Entretanto, alguns dispositivos comuns possuem implementações inválidas de atributos SDP em que o valor não é do tipo esperado. Além disso, muitos usos de RFCOMM não requerem atributos SDP adicionais. Por esses motivos, essa API oferece acesso aos dados SDP não analisados, dos quais os desenvolvedores podem obter as informações que precisam.

As APIs de RFCOMM utilizam o conceito de identificadores de serviço. Embora um identificador de serviço seja simplesmente um GUID de 128 bits, ele também é comumente especificado como número inteiro de 16 ou 32 bits. A API de RFCOMM oferece um wrapper para identificadores de serviços que permitem que eles sejam especificados e consumidos como GUIDs de 128 bits, bem como número inteiro de 32 bits, mas não oferecem inteiros de 16 bits. Isso não é um problema para a API, pois a linguagem será automaticamente redimensionada para inteiro de 32 bits e o identificador ainda poderá ser gerado corretamente.

Os aplicativos de um dispositivo podem executar operações de dispositivos de multietapas em uma tarefa em segundo plano, portanto eles podem executar até concluírem, mesmo se o aplicativo for movido para segundo plano e suspenso. Isso permite um serviço do dispositivo confiável, como alterações em configurações persistentes ou firmware, e sincronização de conteúdo, sem precisar que o usuário sente-se e observe a barra de progresso. Use o [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297315) para serviço de dispositivo e o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297337) para sincronização de conteúdo. Observe que essas tarefas em segundo plano limitam a quantidade de tempo que o aplicativo pode ser executado em segundo plano e não têm a intenção de permitir operação indefinida nem sincronização infinita.

Para ver um exemplo de código completo que fornece detalhes sobre a operação de RFCOMM, consulte o [ **Exemplo do Bluetooth Rfcomm Chat** ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) no Github.  
## Enviar um arquivo como cliente

Ao enviar um arquivo, o cenário mais básico é estabelecer conexão com um dispositivo emparelhado com base em um serviço desejado. Isso envolve as seguintes etapas:

-   Use as funções **RfcommDeviceService.GetDeviceSelector\*** para ajudar a gerar uma consulta AQS que possa ser usada para instâncias de dispositivos emparelhados enumerados do serviço desejado.
-   Escolha um dispositivo enumerado, crie um [**RfcommDeviceService**](https://msdn.microsoft.com/library/windows/apps/Dn263463) e leia os atributos SDP conforme necessário (usando [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) para analisar os dados do atributo).
-   Crie um soquete e use as propriedades [**RfcommDeviceService.ConnectionHostName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname.aspx) e [**RfcommDeviceService.ConnectionServiceName**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename.aspx) para [**StreamSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701504) para o serviço de dispositivo remoto com os parâmetros apropriados.
-   Siga os padrões de fluxo de dados estabelecidos para ler dados do arquivo e enviá-lo no [**StreamSocket.OutputStream**](https://msdn.microsoft.com/library/windows/apps/BR226920) do soquete para o dispositivo.

```csharp
Windows.Devices.Bluetooth.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    auto services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0) 
    {
        // Initialize the target Bluetooth BR device
        auto service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, e.g. click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    auto attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cpp
Windows::Devices::Bluetooth::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0) 
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App's minimum
                // requirement
                if (SupportsProtection(service)
                    && IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, e.g. click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## Receber um arquivo como servidor

Outro cenário de aplicativo comum RFCOMM é hospedar um serviço no computador e expô-lo para outros dispositivos.

-   Crie um [**RfcommServiceProvider**](https://msdn.microsoft.com/library/windows/apps/Dn263511) para anunciar o serviço desejado.
-   Defina o atributo SDP conforme necessário (usando [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) para gerar os dados do atributo) e inicie o anúncio dos registros SDP para outros dispositivos recuperarem.
-   Para conectar a um dispositivo cliente, crie um ouvinte de soquete para começar a escutar as solicitações de conexão de entrada.
-   Quando uma conexão é recebida, armazene o soquete conectado para processamento posterior.
-   Siga os padrões de fluxo de dados estabelecidos para ler dados do InputStream do soquete e salvá-los em um arquivo.

Para manter um serviço RFCOMM em segundo plano, use o [ **RfcommConnectionTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.rfcommconnectiontrigger.aspx). A tarefa em segundo plano é disparada em conexão com o serviço. O desenvolvedor recebe um identificador para o soquete na tarefa em segundo plano. A tarefa em segundo plano é de longa duração e persiste enquanto o soquete está em uso.    

```csharp
Windows.Devices.Bluetooth.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising();
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer.WriteUint32(SERVICE_VERSION);
    
    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, e.g. click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cpp
Windows::Devices::Bluetooth::RfcommServiceProvider^ _provider;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising();
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer->WriteUint32(SERVICE_VERSION);
    
    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, e.g. click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```




<!--HONumber=Jun16_HO4-->



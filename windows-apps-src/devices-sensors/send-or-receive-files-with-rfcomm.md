---
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: Este artigo apresenta uma visão geral do Bluetooth RFCOMM em aplicativos da Plataforma Universal do Windows (UWP), além do código de exemplo sobre como enviar ou receber um arquivo.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 41facafe5a1f42a94ed91a4b5e6d34bd43c806f2
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448316"
---
# <a name="bluetooth-rfcomm"></a>Bluetooth RFCOMM

**APIs importantes**

- [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm)

Este artigo apresenta uma visão geral do Bluetooth RFCOMM em aplicativos da Plataforma Universal do Windows (UWP), além do código de exemplo sobre como enviar ou receber um arquivo.

## <a name="overview"></a>Visão geral

As APIs no namespace [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm) são compiladas nos padrões existentes para Dispositivos Windows, incluindo [**enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) e [**instantiation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice). A leitura e a gravação de dados é projetada para tirar vantagens de [**padrões de fluxo de dados estabelecidos**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) e objetos em [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams). Os atributos do Service Discovery Protocol (SDP) têm um valor e um tipo esperado. Entretanto, alguns dispositivos comuns possuem implementações inválidas de atributos SDP em que o valor não é do tipo esperado. Além disso, muitos usos de RFCOMM não requerem atributos SDP adicionais. Por esses motivos, essa API oferece acesso aos dados SDP não analisados, dos quais os desenvolvedores podem obter as informações que precisam.

As APIs de RFCOMM utilizam o conceito de identificadores de serviço. Embora um identificador de serviço seja simplesmente um GUID de 128 bits, ele também é comumente especificado como número inteiro de 16 ou 32 bits. A API de RFCOMM oferece um wrapper para identificadores de serviços que permitem que eles sejam especificados e consumidos como GUIDs de 128 bits, bem como número inteiro de 32 bits, mas não oferecem inteiros de 16 bits. Isso não é um problema para a API, pois a linguagem será automaticamente redimensionada para inteiro de 32 bits e o identificador ainda poderá ser gerado corretamente.

Os aplicativos de um dispositivo podem executar operações de dispositivos de multietapas em uma tarefa em segundo plano, portanto eles podem executar até concluírem, mesmo se o aplicativo for movido para segundo plano e suspenso. Isso permite um serviço do dispositivo confiável, como alterações em configurações persistentes ou firmware, e sincronização de conteúdo, sem precisar que o usuário sente-se e observe a barra de progresso. Use o [**DeviceServicingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger) para serviço de dispositivo e o [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) para sincronização de conteúdo. Observe que essas tarefas em segundo plano limitam a quantidade de tempo que o aplicativo pode ser executado em segundo plano e não têm a intenção de permitir operação indefinida nem sincronização infinita.

Para ver um exemplo de código completo que fornece detalhes sobre a operação de RFCOMM, consulte o [**Exemplo do Bluetooth Rfcomm Chat**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) no Github.  

## <a name="send-a-file-as-a-client"></a>Enviar um arquivo como cliente

Ao enviar um arquivo, o cenário mais básico é estabelecer conexão com um dispositivo emparelhado com base em um serviço desejado. Isso envolve as seguintes etapas:

- Use as funções **RfcommDeviceService. \* GetDeviceSelector** para ajudar a gerar uma consulta AQS que pode ser usada para enumerar instâncias de dispositivo emparelhado do serviço desejado.
- Escolha um dispositivo enumerado, crie um [**RfcommDeviceService**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService) e leia os atributos SDP conforme necessário (usando [**established data helpers**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) para analisar os dados do atributo).
- Crie um soquete e use as propriedades [**RfcommDeviceService.ConnectionHostName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname) e [**RfcommDeviceService.ConnectionServiceName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename) para [**StreamSocket.ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) para o serviço de dispositivo remoto com os parâmetros apropriados.
- Siga os padrões de fluxo de dados estabelecidos para ler dados do arquivo e enviá-lo no [**StreamSocket.OutputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.outputstream) do soquete para o dispositivo.

```csharp
using System;
using System.Threading.Tasks;
using Windows.Devices.Bluetooth.Rfcomm;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;
using Windows.Devices.Bluetooth;

Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    var services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0)
    {
        // Initialize the target Bluetooth BR device
        var service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        bool isCompatibleVersion = await IsCompatibleVersionAsync(service);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && isCompatibleVersion)
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
            // the user to take some action, for example, click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
        case SocketProtectionLevel.PlainSocket:
            if ((service.MaxProtectionLevel == SocketProtectionLevel
                    .BluetoothEncryptionWithAuthentication)
                || (service.MaxProtectionLevel == SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication))
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
async Task<bool> IsCompatibleVersionAsync(RfcommDeviceService service)
{
    var attributes = await service.GetSdpRawAttributesAsync(
        BluetoothCacheMode.Uncached);
    var attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    var reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
    else return false;
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService m_service{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Enumerate devices with the object push service.
    Windows::Devices::Enumeration::DeviceInformationCollection services{
        co_await Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::GetDeviceSelector(
                Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush())) };

    if (services.Size() > 0)
    {
        // Initialize the target Bluetooth BR device.
        Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService service{
            co_await Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::FromIdAsync(services.GetAt(0).Id()) };

        // Check that the service meets this App's minimum
        // requirement
        if (SupportsProtection(service)
            && co_await IsCompatibleVersion(service))
        {
            m_service = service;

            // Create a socket and connect to the target
            co_await m_socket.ConnectAsync(
                m_service.ConnectionHostName(),
                m_service.ConnectionServiceName(),
                Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can
            // wait for the user to take some action, for example, click
            // a button to send a file to the device, which could
            // invoke the Picker and then send the picked file.
            // The transfer itself would use the Sockets API and
            // not the Rfcomm API, and so is omitted here for
            //brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    switch (service.ProtectionLevel())
    {
    case Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket:
        if ((service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication))
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
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This application relies on CRC32 checking available in version 2.0 of the service.
const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A }; // UINT32.
const uint32_t MINIMUM_SERVICE_VERSION{ 200 };

Windows::Foundation::IAsyncOperation<bool> IsCompatibleVersion(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    auto attributes{
        co_await service.GetSdpRawAttributesAsync(Windows::Devices::Bluetooth::BluetoothCacheMode::Uncached) };

    auto attribute{ attributes.Lookup(SERVICE_VERSION_ATTRIBUTE_ID) };
    auto reader{ Windows::Storage::Streams::DataReader::FromBuffer(attribute) };

    // The first byte contains the attribute's type.
    byte attributeType{ reader.ReadByte() };
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint32_t version{ reader.ReadUInt32() };
        co_return (version >= MINIMUM_SERVICE_VERSION);
    }
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService^ _service;
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
                        // wait for the user to take some action, for example, click
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
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication))
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

    // The first byte contains the attribute's type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## <a name="receive-file-as-a-server"></a>Receber um arquivo como servidor

Outro cenário de aplicativo comum RFCOMM é hospedar um serviço no computador e expô-lo para outros dispositivos.

- Crie um [**RfcommServiceProvider**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider) para anunciar o serviço desejado.
- Defina o atributo SDP conforme necessário (usando [**established data helpers**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) para gerar os dados do atributo) e inicie o anúncio dos registros SDP para outros dispositivos recuperarem.
- Para conectar a um dispositivo cliente, crie um ouvinte de soquete para começar a escutar as solicitações de conexão de entrada.
- Quando uma conexão é recebida, armazene o soquete conectado para processamento posterior.
- Siga os padrões de fluxo de dados estabelecidos para ler dados do InputStream do soquete e salvá-los em um arquivo.

Para manter um serviço RFCOMM em segundo plano, use o [**RfcommConnectionTrigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.rfcommconnectiontrigger). A tarefa em segundo plano é disparada em conexão com o serviço. O desenvolvedor recebe um identificador para o soquete na tarefa em segundo plano. A tarefa em segundo plano é de longa duração e persiste enquanto o soquete está em uso.    

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider =
        await Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider.CreateAsync(
            RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceivedAsync;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising(listener);
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;

void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    Windows.Storage.Streams.DataWriter writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(MINIMUM_SERVICE_VERSION);

    IBuffer data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceivedAsync(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    listener.Dispose();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, for example, click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider m_provider{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Initialize the provider for the hosted RFCOMM service.
    auto provider{ co_await Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider::CreateAsync(
        Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush()) };

    m_provider = provider;

    // Create a listener for this service and start listening.
    Windows::Networking::Sockets::StreamSocketListener listener;
    listener.ConnectionReceived({ this, &MainPage::OnConnectionReceived });

    co_await listener.BindServiceNameAsync(m_provider.ServiceId().AsString(),
        Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes();
    m_provider.StartAdvertising(listener);
}

const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A };   // UINT32.
const uint32_t SERVICE_VERSION{ 200 };

void InitializeServiceSdpAttributes()
{
    Windows::Storage::Streams::DataWriter writer;

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data{ writer.DetachBuffer() };
    m_provider.SdpRawAttributes().Insert(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    Windows::Networking::Sockets::StreamSocketListener const& listener,
    Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
{
    // Stop advertising/listening so that we're only serving one client
    m_provider.StopAdvertising();
    listener.Close();
    m_socket = args.Socket();

    // The client socket is connected. At this point the application can wait for
    // the user to take some action, for example, click a button to receive a
    // file from the device, which could invoke the Picker and then save
    // the received file to the picked location. The transfer itself
    // would use the Sockets API and not the Rfcomm API, and so is
    // omitted here for brevity.
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider^ _provider;
Windows::Networking::Sockets::StreamSocket^ _socket;

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
        _provider->StartAdvertising(listener);
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer->WriteUInt32(SERVICE_VERSION);

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
        // the user to take some action, for example, click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```

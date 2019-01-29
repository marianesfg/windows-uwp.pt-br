---
description: Os WebSockets fornecem um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web usando HTTP(S), suportando tanto mensagens UTF-8 quanto binárias.
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, rede, websocket, messagewebsocket, streamwebsocket
ms.localizationpriority: medium
ms.openlocfilehash: 05f56f07aed0c9f97daffe3842952ce142f8159a
ms.sourcegitcommit: 1901a43b9e40a05c28c7799e0f9b08ce92f8c8a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035407"
---
# <a name="websockets"></a>WebSockets
Os WebSockets fornecem um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web usando HTTP(S), suportando tanto mensagens UTF-8 quanto binárias.

De acordo com o [Protocolo WebSocket](http://tools.ietf.org/html/rfc6455), os dados são transferidos imediatamente através de uma única conexão de soquete full-duplex, permitindo que as mensagens sejam enviadas e recebidas de ambos os pontos de extremidade em tempo real. WebSockets são ideais para uso em jogos multijogador (tanto em tempo real, quanto por fases), notificações instantâneas de rede social, exibições atualizadas de ações e informações meteorológicas, bem como em outros aplicativos que exigem transferências de dados rápidas e seguras.

Para estabelecer uma conexão de WebSocket, um handshake baseado em HTTP é trocado entre o cliente e o servidor. Em caso de êxito, o protocolo de camada de aplicativo é "atualizado" de HTTP para WebSockets usando a conexão TCP estabelecida anteriormente. Assim que isso ocorre, o HTTP fica totalmente fora de cogitação. Os dados podem ser enviados ou recebidos usando o protocolo WebSocket por ambos os pontos de extremidade até o encerramento da conexão com WebSocket.

**Observação** Um cliente não pode usar WebSockets para transferir dados, a menos que o servidor também use o protocolo WebSocket. Se o servidor não puder dar suporte a WebSockets, use outro método de transferência de dados.

A Plataforma Universal do Windows (UWP) dá suporte ao uso de WebSockets pelo cliente e pelo servidor. O namespace [**Windows.Networking.Sockets**](/uwp/api/windows.networking.sockets) define duas classes de WebSocket a serem usadas por clientes&mdash;[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) e [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket). Está é uma comparação dessas duas classes de WebSocket.

| [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket) | [StreamWebSocket](/uwp/api/windows.networking.sockets.streamwebsocket) |
| - | - |
| Uma mensagem WebSocket inteira é lida/gravada em uma operação única. | Seções de uma mensagem podem ser lidas com cada operação de leitura. |
| Apropriado quando as mensagens não são muito grandes. | Adequado quando arquivos muito grandes (como fotos ou vídeos) são transferidos. |
| Suporta tanto mensagens UTF-8 quanto binárias. | Suporta apenas mensagens binárias. |
| Semelhante a um [soquete de datagrama ou UDP](sockets.md#build-a-basic-udp-socket-client-and-server) (no sentido de ser projetado para pequenas mensagens frequentes), mas com a confiabilidade do TCP, garantia de ordem de pacote e controle de congestionamento. | Semelhante a um [soquete de fluxo ou TCP](sockets.md#build-a-basic-tcp-socket-client-and-server). |

## <a name="secure-your-connection-with-tlsssl"></a>Proteja sua conexão com TLS/SSL
Na maioria dos casos, você usará uma conexão WebSocket segura para que seus dados enviados e recebidos sejam criptografados. Isso também aumentará as chances de sucesso da conexão, já que muitos intermediários, como firewalls e proxies, rejeitam conexões WebSocket não criptografadas. O [protocolo WebSocket](https://tools.ietf.org/html/rfc6455#section-3) define esses dois esquemas de URI.

| Esquema de URI | Finalidade |
| - | - |
| wss: | usado para conexões seguras que devem ser criptografadas. |
| ws: | usado para conexões não criptografadas. |

Para criptografar a conexão WebSocket, use o esquema de URI `wss:`. Veja um exemplo.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var webSocket = new Windows.Networking.Sockets.MessageWebSocket();
    await webSocket.ConnectAsync(new Uri("wss://www.contoso.com/mywebservice"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
{
    Windows::Networking::Sockets::MessageWebSocket webSocket;
    co_await webSocket.ConnectAsync(Uri{ L"wss://www.contoso.com/mywebservice" });
}
```

## <a name="use-messagewebsocket-to-connect"></a>Use MessageWebSocket para conectar
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) permite que uma mensagem WebSocket inteira seja lida/gravada em uma operação única. Consequentemente, ele é apropriado quando as mensagens não são muito grandes. A classe suporta tanto mensagens UTF-8 quanto binárias.

Este exemplo de código abaixo usa o servidor de eco WebSocket.org&mdash;um serviço que exibe de volta para o remetente qualquer mensagem enviada a ele.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
    this.messageWebSocket.Control.MessageType = Windows.Networking.Sockets.SocketMessageType.Utf8;

    this.messageWebSocket.MessageReceived += WebSocket_MessageReceived;
    this.messageWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();
        connectTask.ContinueWith(_ => this.SendMessageUsingMessageWebSocketAsync("Hello, World!"));
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private async Task SendMessageUsingMessageWebSocketAsync(string message)
{
    using (var dataWriter = new DataWriter(this.messageWebSocket.OutputStream))
    {
        dataWriter.WriteString(message);
        await dataWriter.StoreAsync();
        dataWriter.DetachStream();
    }
    Debug.WriteLine("Sending message using MessageWebSocket: " + message);
}

private void WebSocket_MessageReceived(Windows.Networking.Sockets.MessageWebSocket sender, Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs args)
{
    try
    {
        using (DataReader dataReader = args.GetDataReader())
        {
            dataReader.UnicodeEncoding = Windows.Storage.Streams.UnicodeEncoding.Utf8;
            string message = dataReader.ReadString(dataReader.UnconsumedBufferLength);
            Debug.WriteLine("Message received from MessageWebSocket: " + message);
            this.messageWebSocket.Dispose();
        }
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;
    winrt::event_token m_messageReceivedEventToken;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        m_messageWebSocket.Control().MessageType(Windows::Networking::Sockets::SocketMessageType::Utf8);

        m_messageReceivedEventToken = m_messageWebSocket.MessageReceived({ this, &MessageWebSocketPage::OnWebSocketMessageReceived });
        m_closedEventToken = m_messageWebSocket.Closed({ this, &MessageWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            SendMessageUsingMessageWebSocketAsync(L"Hello, World!");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingMessageWebSocketAsync(std::wstring message)
    {
        DataWriter dataWriter{ m_messageWebSocket.OutputStream() };
        dataWriter.WriteString(message);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();
        std::wstringstream wstringstream;
        wstringstream << L"Sending message using MessageWebSocket: " << message.c_str() << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
    }

    void OnWebSocketMessageReceived(Windows::Networking::Sockets::MessageWebSocket const& /* sender */, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs const& args)
    {
        try
        {
            DataReader dataReader{ args.GetDataReader() };

            dataReader.UnicodeEncoding(Windows::Storage::Streams::UnicodeEncoding::Utf8);
            auto message = dataReader.ReadString(dataReader.UnconsumedBufferLength());
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message.c_str() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_messageWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const& /* sender */, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        this->messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;

        this->messageWebSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::MessageWebSocket^, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^>(this, &MessageWebSocketPage::WebSocket_MessageReceived);
        this->messageWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &MessageWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
            connectTask.then([this] { this->SendMessageUsingMessageWebSocketAsync(L"Hello, World!"); });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingMessageWebSocketAsync(Platform::String^ message)
    {
        auto dataWriter = ref new DataWriter(this->messageWebSocket->OutputStream);
        dataWriter->WriteString(message);

        Concurrency::create_task(dataWriter->StoreAsync()).then(
            [=](unsigned int)
        {
            dataWriter->DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending message using MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        });
    }

    void WebSocket_MessageReceived(Windows::Networking::Sockets::MessageWebSocket^ sender, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^ args)
    {
        try
        {
            DataReader^ dataReader = args->GetDataReader();

            dataReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
            Platform::String^ message = dataReader->ReadString(dataReader->UnconsumedBufferLength);
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            this->messageWebSocket->Close(1000, L"");
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-messagewebsocketmessagereceived-and-messagewebsocketclosed-events"></a>Manipule os eventos MessageWebSocket.MessageReceived e MessageWebSocket.Closed
Conforme mostrado no exemplo acima, antes de estabelecer uma conexão e enviar dados com um **MessageWebSocket**, você deve assinar os eventos [**MessageWebSocket.MessageReceived**](/uwp/api/windows.networking.sockets.messagewebsocket.MessageReceived) e [**MessageWebSocket.Closed**](/uwp/api/windows.networking.sockets.messagewebsocket.Closed).
 
**MessageReceived** é acionado quando dados são recebidos. Os dados podem ser acessados por meio de [**MessageWebSocketMessageReceivedEventArgs**](/uwp/api/windows.networking.sockets.messagewebsocketmessagereceivedeventargs). **Closed** é acionado quando o cliente ou servidor fecha o soquete.
 
### <a name="send-data-on-a-messagewebsocket"></a>Envie dados em um MessageWebSocket
Quando uma conexão é estabelecida, é possível enviar dados ao servidor. Você pode fazer isso usando a propriedade [**MessageWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream) e um [**DataWriter**](/uwp/api/windows.storage.streams.datawriter), para gravar os dados. 

**Observação** O **DataWriter** se apropria do fluxo de saída. Quando o **DataWriter** sai do escopo, se o fluxo de saída for anexado a ele, o **DataWriter** desalocará o fluxo de saída. Após isso, as tentativas subsequentes para usar o fluxo de saída falharão com um valor HRESULT de 0x80000013. Mas você pode chamar [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream) para desanexar o fluxo de saída do **DataWriter** e retornar a propriedade do fluxo para o **MessageWebSocket**.

## <a name="use-streamwebsocket-to-connect"></a>Use StreamWebSocket para conectar
[**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) permite que seções de uma mensagem sejam lidas com cada operação de leitura. Consequentemente, é adequado quando arquivos muito grandes (como fotos ou vídeos) são transferidos. A classe oferece suporte para mensagens binárias somente.

Este exemplo de código abaixo usa o servidor de eco WebSocket.org&mdash;um serviço que exibe de volta para o remetente qualquer mensagem enviada a ele.

```csharp
private Windows.Networking.Sockets.StreamWebSocket streamWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

    this.streamWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();

        connectTask.ContinueWith(_ =>
        {
            Task.Run(() => this.ReceiveMessageUsingStreamWebSocket());
            Task.Run(() => this.SendMessageUsingStreamWebSocket(new byte[] { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 }));
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void ReceiveMessageUsingStreamWebSocket()
{
    try
    {
        using (var dataReader = new DataReader(this.streamWebSocket.InputStream))
        {
            dataReader.InputStreamOptions = InputStreamOptions.Partial;
            await dataReader.LoadAsync(256);
            byte[] message = new byte[dataReader.UnconsumedBufferLength];
            dataReader.ReadBytes(message);
            Debug.WriteLine("Data received from StreamWebSocket: " + message.Length + " bytes");
        }
        this.streamWebSocket.Dispose();
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void SendMessageUsingStreamWebSocket(byte[] message)
{
    try
    {
        using (var dataWriter = new DataWriter(this.streamWebSocket.OutputStream))
        {
            dataWriter.WriteBytes(message);
            await dataWriter.StoreAsync();
            dataWriter.DetachStream();
        }
        Debug.WriteLine("Sending data using StreamWebSocket: " + message.Length.ToString() + " bytes");
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket m_streamWebSocket;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_closedEventToken = m_streamWebSocket.Closed({ this, &StreamWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            ReceiveMessageUsingStreamWebSocket();
            SendMessageUsingStreamWebSocket({ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingStreamWebSocket(std::vector< byte > message)
    {
        try
        {
            DataWriter dataWriter{ m_streamWebSocket.OutputStream() };
            dataWriter.WriteBytes(message);

            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending data using StreamWebSocket: " << message.size() << L" bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    IAsyncAction ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader dataReader{ m_streamWebSocket.InputStream() };
            dataReader.InputStreamOptions(InputStreamOptions::Partial);

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            std::vector< byte > message(bytesLoaded);
            dataReader.ReadBytes(message);
            std::wstringstream wstringstream;
            wstringstream << L"Data received from StreamWebSocket: " << message.size() << " bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_streamWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const&, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket^ streamWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

        this->streamWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &StreamWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));

            connectTask.then(
                [=]
            {
                this->ReceiveMessageUsingStreamWebSocket();
                this->SendMessageUsingStreamWebSocket(ref new Platform::Array< byte >{ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingStreamWebSocket(const Platform::Array< byte >^ message)
    {
        try
        {
            auto dataWriter = ref new DataWriter(this->streamWebSocket->OutputStream);
            dataWriter->WriteBytes(message);

            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](Concurrency::task< unsigned int >) // task< unsigned int > instead of unsigned int in order to handle any exceptions thrown in StoreAsync().
            {
                dataWriter->DetachStream();
                std::wstringstream wstringstream;
                wstringstream << L"Sending data using StreamWebSocket: " << message->Length << L" bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader^ dataReader = ref new DataReader(this->streamWebSocket->InputStream);
            dataReader->InputStreamOptions = InputStreamOptions::Partial;

            Concurrency::create_task(dataReader->LoadAsync(256)).then(
                [=](unsigned int bytesLoaded)
            {
                auto message = ref new Platform::Array< byte >(bytesLoaded);
                dataReader->ReadBytes(message);
                std::wstringstream wstringstream;
                wstringstream << L"Data received from StreamWebSocket: " << message->Length << " bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
                this->streamWebSocket->Close(1000, L"");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-streamwebsocketclosed-event"></a>Manipule o evento Streamwebsocket.Closed.
Antes de estabelecer uma conexão e enviar dados com um **StreamWebSocket**, você deve assinar o evento [**Streamwebsocket.Closed**](/uwp/api/windows.networking.sockets.streamwebsocket.Closed). **Closed** é acionado quando o cliente ou servidor fecha o soquete.
 
### <a name="send-data-on-a-streamwebsocket"></a>Envie dados em um StreamWebSocket
Quando uma conexão é estabelecida, é possível enviar dados ao servidor. Você pode fazer isso usando a propriedade [**StreamWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream) e um [**DataWriter**](/uwp/api/windows.storage.streams.datawriter), para gravar os dados.

**Observação** Se você quiser gravar mais dados no mesmo soquete, certifique-se de chamar [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream) para desanexar o fluxo de saída do **DataWriter** antes que o **DataWriter** saia do escopo. Isso retorna a propriedade do fluxo para o **MessageWebSocket**.

### <a name="receive-data-on-a-streamwebsocket"></a>Receba dados em um StreamWebSocket
Use a propriedade [**StreamWebSocket.InputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream) e um [**DataReader**](/uwp/api/windows.storage.streams.datareader), para ler os dados.

## <a name="advanced-options-for-messagewebsocket-and-streamwebsocket"></a>Opções avançadas para MessageWebSocket e StreamWebSocket
Antes de estabelecer uma conexão, você pode definir opções avançadas em um soquete definindo propriedades em [**MessageWebSocketControl**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol) ou [**StreamWebSocketControl**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol). Você acessa uma instância dessas classes do objeto do soquete através da propriedade [**MessageWebSocket.Control**](/uwp/api/windows.networking.sockets.messagewebsocket.control) ou [**StreamWebSocket.Control**](/uwp/api/windows.networking.sockets.streamwebsocket.control), conforme apropriado.

Veja um exemplo usando **StreamWebSocket**. O mesmo padrão se aplica a **MessageWebSocket**.

```csharp
var streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control.NoDelay = false;

await streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org"));
```

```cppwinrt
Windows::Networking::Sockets::StreamWebSocket streamWebSocket;

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control().NoDelay(false);

auto connectAsyncAction = streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
```

```cpp
auto streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket->Control->NoDelay = false;

auto connectTask = Concurrency::create_task(streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
```

**Observação** Não tente alterar uma propriedade de controle *depois* de chamar **ConnectAsync**. A única exceção a essa regra é [MessageWebSocketControl.MessageType](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.MessageType).

## <a name="websocket-information-classes"></a>Classes de informações de WebSocket
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) e [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) têm uma classe correspondente que fornece informações adicionais sobre o objeto.

[**MessageWebSocketInformation**](/uwp/api/windows.networking.sockets.messagewebsocketinformation) fornece informações sobre um **MessageWebSocket**. Recupere uma instância dele usando a propriedade [**MessageWebSocket.Information**](/uwp/api/windows.networking.sockets.messagewebsocket.Information).

[**StreamWebSocketInformation**](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation) fornece informações sobre um **StreamWebSocket**. Recupere uma instância da classe de informações usando a propriedade [**StreamWebSocket.Information**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Information).

Observe que todas as propriedades destas classes de informações são somente leitura, porém você pode usá-la para recuperar informações a qualquer momento durante o tempo de vida de um objeto WebSocket.

## <a name="handling-exceptions"></a>Tratando exceções
Um erro encontrado em uma operação [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) ou [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) é retornado como um valor **HRESULT**. Você pode passar o valor **HRESULT** para o método [**WebSocketError.GetStatus**](/uwp/api/windows.networking.sockets.websocketerror.getstatus) para convertê-lo em um valor de enumeração [**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus).

A maioria dos valores de enumeração **WebErrorStatus** corresponde a um erro retornado pela operação nativa de cliente HTTP. Seu aplicativo pode alternar valores de enumeração **WebErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, é possível usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro. Possíveis valores de **HRESULT** são listados em `Winerror.h`, que podem ser encontrado em sua instalação do SDK (por exemplo, na pasta `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`). Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E_INVALIDARG**.

## <a name="setting-timeouts-on-websocket-operations"></a>Definindo tempos limites em operações de WebSocket
**MessageWebSocket** e **StreamWebSocket** usam um serviço de sistema interno para enviar solicitações de cliente do WebSocket e receber respostas de um servidor. O valor de tempo limite padrão usado para uma operação de conexão de WebSocket é 60 segundos. Se o servidor HTTP com suporte a WebSockets não conseguir responder à solicitação de conexão de WebSocket (ficar temporariamente inativo ou for bloqueado por uma interrupção de rede), o serviço de sistema interno aguardará os 60 segundos padrão antes de retornar um erro. Esse erro faz com que seja gerada uma exceção no método **ConnectAsync** do WebSocket. Para operações de envio e recebimento após o estabelecimento de uma conexão WebSocket, o tempo limite padrão é 30 segundos.

Se a consulta de nome de um servidor HTTP na URI retornar vários endereços IP para o nome, o serviço de sistema interno tentará até cinco endereços IP para o site (cada um com um tempo limite padrão de 60 segundos), antes de falhar. Consequentemente, seu aplicativo poderá aguardar vários minutos tentando se conectar a vários endereços IP antes de manipular uma exceção. Esse comportamento pode dar ao usuário a impressão de que o aplicativo parou de funcionar. 

Para tornar seu aplicativo mais responsivo e minimizar esses problemas, você pode definir um tempo limite mais curto em solicitações de conexão. Defina tempos limite da mesma forma para **MessageWebSocket** e **StreamWebSocket**.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    try
    {
        var cancellationTokenSource = new CancellationTokenSource();
        var connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask(cancellationTokenSource.Token);

        // Cancel connectTask after 5 seconds.
        cancellationTokenSource.CancelAfter(TimeSpan.FromMilliseconds(5000));

        connectTask.ContinueWith((antecedent) =>
        {
            if (antecedent.Status == TaskStatus.RanToCompletion)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask timed out, or faulted.
            }
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;

    IAsyncAction TimeoutAsync()
    {
        // Return control to the caller, and resume again to complete the async action after the timeout period.
        // 5 seconds, in this example.
        co_await(std::chrono::seconds{ 5 });
    }

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        try
        {
            // Return control to the caller, and then immediately resume on a thread pool thread.
            co_await winrt::resume_background();

            auto connectAsyncAction = m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });

            TimeoutAsync().Completed([connectAsyncAction](IAsyncAction const& sender, AsyncStatus const)
            {
                // TimeoutAsync completes after the timeout period. After that period, it's safe
                // to cancel the ConnectAsync action even if it has already completed.
                connectAsyncAction.Cancel();
            });

            try
            {
                // Block until the ConnectAsync action completes or is canceled.
                connectAsyncAction.get();
            }
            catch (winrt::hresult_error const& ex)
            {
                std::wstringstream wstringstream;
                wstringstream << L"ConnectAsync threw an exception: " << ex.message().c_str() << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            }

            if (connectAsyncAction.Status() == AsyncStatus::Completed)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask did not run to completion.
            }
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }
```

```cpp
#include <agents.h>
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        try
        {
            Concurrency::cancellation_token_source cancellationTokenSource;
            Concurrency::cancellation_token cancellationToken = cancellationTokenSource.get_token();

            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")), cancellationToken);

            // This continuation task returns true should connectTask run to completion.
            Concurrency::task< bool > taskRanToCompletion = connectTask.then([](void)
            {
                return true;
            });

            // This task returns false after the specified timeout. 5 seconds, in this example.
            Concurrency::task< bool > taskTimedout = Concurrency::create_task([]() -> bool
            {
                Concurrency::task_completion_event< void > taskCompletionEvent;

                // A call object that sets the task completion event.
                auto call = std::make_shared< Concurrency::call< int > >([taskCompletionEvent](int)
                {
                    taskCompletionEvent.set();
                });

                // A non-repeating timer that calls the call object when the timer fires.
                auto nonRepeatingTimer = std::make_shared< Concurrency::timer < int > >(5000, 0, call.get(), false);
                nonRepeatingTimer->start();

                // A task that completes after the completion event is set.
                Concurrency::task< void > taskWaitForCompletionEvent(taskCompletionEvent);

                return taskWaitForCompletionEvent.then([]() {return false; }).get();
            });

            (taskRanToCompletion || taskTimedout).then([this, cancellationTokenSource](bool connectTaskRanToCompletion)
            {
                if (connectTaskRanToCompletion)
                {
                    // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                    // Add additional code here to use the MessageWebSocket.
                }
                else
                {
                    // taskTimedout ran to completion, so we should cancel connectTask via the cancellation_token_source.
                    cancellationTokenSource.cancel();
                }
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }
```

## <a name="important-apis"></a>APIs Importantes
* [DataReader](/uwp/api/Windows.Storage.Streams.DataReader)
* [DataWriter](/uwp/api/Windows.Storage.Streams.DataWriter)
* [DataWriter.DetachStream](/uwp/api/windows.storage.streams.datawriter.DetachStream)
* [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket)
* [MessageWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Closed)
* [MessageWebSocket.ConnectAsync](/uwp/api/windows.networking.sockets.messagewebsocket.connectasync)
* [MessageWebSocket.Control](/uwp/api/windows.networking.sockets.messagewebsocket.control)
* [MessageWebSocket.Information](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Information)
* [MessageWebSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.MessageReceived)
* [MessageWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)
* [MessageWebSocketControl](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl)
* [MessageWebSocketControl.MessageType](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl.MessageType)
* [MessageWebSocketInformation](/uwp/api/Windows.Networking.Sockets.MessageWebSocketInformation)
* [MessageWebSocketMessageReceivedEventArgs](/uwp/api/Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs)
* [SocketMessageType](/uwp/api/windows.networking.sockets.socketmessagetype)
* [StreamWebSocket](/uwp/api/Windows.Networking.Sockets.StreamWebSocket)
* [StreamWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Closed)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamWebSocket.Control](/uwp/api/windows.networking.sockets.streamwebsocket.control)
* [StreamWebSocket.Information](/uwp/api/windows.networking.sockets.streamwebsocket.Information)
* [StreamWebSocket.InputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)
* [StreamWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)
* [StreamWebSocketControl](/uwp/api/Windows.Networking.Sockets.StreamWebSocketControl)
* [StreamWebSocketInformation](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation)
* [WebErrorStatus](/uwp/api/Windows.Web.WebErrorStatus) 
* [WebSocketError.GetStatus](/uwp/api/windows.networking.sockets.websocketerror.getstatus)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>Tópicos relacionados
* [Protocolo WebSocket](http://tools.ietf.org/html/rfc6455)
* [Soquetes](sockets.md)

## <a name="samples"></a>Exemplos
* [Exemplo de WebSocket](http://go.microsoft.com/fwlink/p/?LinkId=620623)

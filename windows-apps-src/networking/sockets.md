---
description: Soquetes são uma tecnologia de transferência de dados de baixo nível sobre qual muitos protocolos de rede são implementados. UWP oferece classes de soquete de TCP e UDP para o cliente-servidor ou aplicativos ponto a ponto, se as conexões tiverem vida longa ou se uma conexão estabelecida não for necessária.
title: Soquetes
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.date: 06/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d75afd17d5aa7edf64fda36b3a35b3a101c1d89
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7973324"
---
# <a name="sockets"></a>Soquetes
Soquetes são uma tecnologia de transferência de dados de baixo nível sobre o qual muitos protocolos de rede são implementados. UWP oferece classes de soquete de TCP e UDP para o cliente-servidor ou aplicativos ponto a ponto, se as conexões tiverem vida longa ou se uma conexão estabelecida não for necessária.

Este tópico se concentra em como usar as classes de soquete da Plataforma Universal do Windows (UWP) que estão no namespace [**Windows.Networking.Networking**](/uwp/api/Windows.Networking.Sockets). Mas você também pode usar [Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673) em um aplicativo UWP.

> [!NOTE]
> Como consequência do [isolamento de rede](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx), o Windows não permite estabelecer conexões de soquete (soquetes ou WinSock) entre dois aplicativos UWP em execução no mesmo computador, seja um endereço de loopback local (127.0.0.0) ou especificando explicitamente o endereço IP local. Para obter detalhes sobre mecanismos pelo qual os aplicativos UWP podem se comunicar uns com os outros, consulte [Comunicação entre aplicativos](/windows/uwp/app-to-app/index).

## <a name="build-a-basic-tcp-socket-client-and-server"></a>Crie um cliente de soquete TCP básico e servidor
Um soquete TCP (protocolo TCP) fornece transferências de dados de rede em baixo nível em qualquer direção para conexões de longa duração. Os soquetes TCP são o recurso subjacente usado pela maioria dos protocolos de rede disponíveis na Internet. Para demonstrar as operações básicas de TCP, o código de exemplo abaixo mostra um [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) e um [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener) enviando e recebendo dados por TCP para formar um cliente de eco e o servidor.

Para começar com menos partes móveis possível&mdash;e para evitar problemas de isolamento de rede para o presente&mdash;crie um novo projeto e coloque o cliente e o código de servidor abaixo no mesmo projeto.

Você precisará [declarar uma funcionalidade do aplicativo](../packaging/app-capability-declarations.md) em seu projeto. Abra o arquivo de origem do manifesto do pacote do aplicativo (o arquivo `Package.appxmanifest`) e, na guia Funcionalidades, marque **Redes Privadas (Cliente e Servidor)**. Isso é o que é exibido na marcação `Package.appxmanifest`.

```xml
<Capability Name="privateNetworkClientServer" />
```

Em vez de `privateNetworkClientServer`, você pode declarar `internetClientServer` se estiver se conectando pela Internet. **StreamSocket** e **StreamSocketListener** precisam que uma ou outra destas funcionalidades sejam declaradas.

### <a name="an-echo-client-and-server-using-tcp-sockets"></a>Um cliente de eco e o servidor, usando soquetes TCP
Construa um [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener) e inicie a escuta de conexões TCP de entrada. O evento [**StreamSocketListener.Connectionreceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived) é acionado sempre que um cliente estabelece uma conexão com o **StreamSocketListener**.

Também construa um [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket), estabeleça uma conexão com o servidor, envie uma solicitação e receba uma resposta.

Crie uma nova **Página** chamada `StreamSocketAndListenerPage`. Coloque a marcação XAML no `StreamSocketAndListenerPage.xaml` e o coloque a necessidade de código dentro da classe `StreamSocketAndListenerPage`.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="TCP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="StreamSocket &amp; StreamSocketListener"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose an arbitrary port number.
static string PortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();

        // The ConnectionReceived event is raised when connections are received.
        streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await streamSocketListener.BindServiceNameAsync(StreamSocketAndListenerPage.PortNumber);

        this.serverListBox.Items.Add("server is listening...");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    string request;
    using (var streamReader = new StreamReader(args.Socket.InputStream.AsStreamForRead()))
    {
        request = await streamReader.ReadLineAsync();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = args.Socket.OutputStream.AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the StreamSocket and establish a connection to the echo server.
        using (var streamSocket = new Windows.Networking.Sockets.StreamSocket())
        {
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            var hostName = new Windows.Networking.HostName("localhost");

            this.clientListBox.Items.Add("client is trying to connect...");

            await streamSocket.ConnectAsync(hostName, StreamSocketAndListenerPage.PortNumber);

            this.clientListBox.Items.Add("client connected");

            // Send a request to the echo server.
            string request = "Hello, World!";
            using (Stream outputStream = streamSocket.OutputStream.AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));

            // Read data from the echo server.
            string response;
            using (Stream inputStream = streamSocket.InputStream.AsStreamForRead())
            {
                using (StreamReader streamReader = new StreamReader(inputStream))
                {
                    response = await streamReader.ReadLineAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\" ", response));
        }

        this.clientListBox.Items.Add("client closed its socket");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener m_streamSocketListener;
    Windows::Networking::Sockets::StreamSocket m_streamSocket;

public:
    void OnNavigatedTo(NavigationEventArgs const& /* e */)
    {
        StartServer();
        StartClient();
    }

private:
    IAsyncAction StartServer()
    {
        try
        {
            // The ConnectionReceived event is raised when connections are received.
            m_streamSocketListener.ConnectionReceived({ this, &StreamSocketAndListenerPage::OnConnectionReceived });

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            co_await m_streamSocketListener.BindServiceNameAsync(L"1337");
            serverListBox().Items().Append(winrt::box_value(L"server is listening..."));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    IAsyncAction OnConnectionReceived(Windows::Networking::Sockets::StreamSocketListener /* sender */, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs args)
    {
        try
        {
            auto socket{ args.Socket() }; // Keep the socket referenced, and alive.
            DataReader dataReader{ socket.InputStream() };

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(sizeof(unsigned int));

            unsigned int stringLength = dataReader.ReadUInt32();
            bytesLoaded = co_await dataReader.LoadAsync(stringLength);
            winrt::hstring request = dataReader.ReadString(bytesLoaded);

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request.c_str() << L"\"";
                serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
            });

            // Echo the request back as the response.
            DataWriter dataWriter{ socket.OutputStream() };
            dataWriter.WriteUInt32(request.size());
            dataWriter.WriteString(request);
            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                std::wstringstream wstringstream;
                wstringstream << L"server sent back the response: \"" << request.c_str() << L"\"";
                serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
            });

            m_streamSocketListener = nullptr;

            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                serverListBox().Items().Append(winrt::box_value(L"server closed its socket"));
            });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
            {
                serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
            });
        }
    }

    IAsyncAction StartClient()
    {
        try
        {
            // Establish a connection to the echo server.

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            Windows::Networking::HostName hostName{ L"localhost" };

            clientListBox().Items().Append(winrt::box_value(L"client is trying to connect..."));

            co_await m_streamSocket.ConnectAsync(hostName, L"1337");
            clientListBox().Items().Append(winrt::box_value(L"client connected"));

            // Send a request to the echo server.
            DataWriter dataWriter{ m_streamSocket.OutputStream() };
            winrt::hstring request{ L"Hello, World!" };
            dataWriter.WriteUInt32(request.size());
            dataWriter.WriteString(request);

            co_await dataWriter.StoreAsync();

            std::wstringstream wstringstream;
            wstringstream << L"client sent the request: \"" << request.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            co_await dataWriter.FlushAsync();
            dataWriter.DetachStream();

            // Read data from the echo server.
            DataReader dataReader{ m_streamSocket.InputStream() };
            unsigned int bytesLoaded = co_await dataReader.LoadAsync(sizeof(unsigned int));
            unsigned int stringLength = dataReader.ReadUInt32();
            bytesLoaded = co_await dataReader.LoadAsync(stringLength);
            winrt::hstring response{ dataReader.ReadString(bytesLoaded) };

            wstringstream.str(L"");
            wstringstream << L"client received the response: \"" << response.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            m_streamSocket = nullptr;

            clientListBox().Items().Append(winrt::box_value(L"client closed its socket"));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();

            // The ConnectionReceived event is raised when connections are received.
            streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &StreamSocketAndListenerPage::StreamSocketListener_ConnectionReceived);

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            Concurrency::create_task(streamSocketListener->BindServiceNameAsync(L"1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is listening...");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        try
        {
            auto dataReader = ref new DataReader(args->Socket->InputStream);

            Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                [=](unsigned int bytesLoaded)
            {
                unsigned int stringLength = dataReader->ReadUInt32();
                Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                    [=](unsigned int bytesLoaded)
                {
                    Platform::String^ request = dataReader->ReadString(bytesLoaded);
                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                        [=]
                    {
                        std::wstringstream wstringstream;
                        wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                        this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                    }));

                    // Echo the request back as the response.
                    auto dataWriter = ref new DataWriter(args->Socket->OutputStream);
                    dataWriter->WriteUInt32(request->Length());
                    dataWriter->WriteString(request);
                    Concurrency::create_task(dataWriter->StoreAsync()).then(
                        [=](unsigned int)
                    {
                        dataWriter->DetachStream();

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                            [=]()
                        {
                            std::wstringstream wstringstream;
                            wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                        }));

                        delete this->streamSocketListener;
                        this->streamSocketListener = nullptr;

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message); }));
        }
    }

    void StartClient()
    {
        try
        {
            // Create the StreamSocket and establish a connection to the echo server.
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is trying to connect...");

            Concurrency::create_task(this->streamSocket->ConnectAsync(hostName, L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->clientListBox->Items->Append(L"client connected");

                // Send a request to the echo server.
                auto dataWriter = ref new DataWriter(this->streamSocket->OutputStream);
                auto request = ref new Platform::String(L"Hello, World!");
                dataWriter->WriteUInt32(request->Length());
                dataWriter->WriteString(request);

                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](Concurrency::task< unsigned int >)
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    Concurrency::create_task(dataWriter->FlushAsync()).then(
                        [=](Concurrency::task< bool >)
                    {
                        dataWriter->DetachStream();

                        // Read data from the echo server.
                        auto dataReader = ref new DataReader(this->streamSocket->InputStream);
                        Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                            [=](unsigned int bytesLoaded)
                        {
                            unsigned int stringLength = dataReader->ReadUInt32();
                            Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                                [=](unsigned int bytesLoaded)
                            {
                                Platform::String^ response = dataReader->ReadString(bytesLoaded);
                                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                                    [=]
                                {
                                    std::wstringstream wstringstream;
                                    wstringstream << L"client received the response: \"" << response->Data() << L"\"";
                                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                                    delete this->streamSocket;
                                    this->streamSocket = nullptr;

                                    this->clientListBox->Items->Append(L"client closed its socket");
                                }));
                            });
                        });
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }
```

## <a name="references-to-streamsockets-in-c-ppl-continuations-applies-to-ccx-primarily"></a>Referências a StreamSockets em continuações do PPL em C++ (aplicável a C++/CX, primariamente)
> [!NOTE]
> Se você usar corrotinas C++/WinRT e passar parâmetros por valor, esse problema não se aplica. Para ver recomendações de passagem de parâmetros, consulte [Operações de concorrência e assíncrona com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing).

Um [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) permanece ativo, desde que seja uma leitura/gravação ativa em seu fluxo de entrada/saída (por exemplo o [**StreamSocketListenerConnectionReceivedEventArgs.Socket**](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs.Socket) que você tem acesso em seu manipulador de eventos [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)). Ao chamar [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync) (ou `ReadAsync/WriteAsync/StoreAsync`), então, ele mantém uma referência para o soquete (por meio do fluxo de entrada do soquete) até que o identificador do evento **Concluído** (se houver) do **LoadAsync** tenha terminado a execução.

O Padrão PPL (PPL) não agenda continuações de tarefa embutidas por padrão. Em outras palavras, adicionar uma tarefa de continuação (com `task::then()`) não garante que a tarefa de continuação será executada embutida como o manipulador de conclusão.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);
    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        // Work in here isn't guaranteed to execute inline as the completion handler of the LoadAsync.
    });
}
```

Da perspectiva do **StreamSocket**, o manipulador de conclusão é feito em execução (e o soquete está qualificado para descarte) antes do corpo de continuação ser executado. Portanto, para fazer com que o soquete não seja descartado se você quiser usá-lo dentro dessa continuação, você precisa referenciar o soquete diretamente (através da captura de lambda) e usá-lo, ou indiretamente (continuando a acessar `args->Socket` dentro das continuações), ou forçando as tarefas de continuação para ser embutido. Você pode ver a primeira técnica (captura de lambda) em ação no [StreamSocket sample](http://go.microsoft.com/fwlink/p/?LinkId=620609). O código C++/CX na seção [Criar um cliente de soquete TCP básico e servidor](#build-a-basic-tcp-socket-client-and-server) acima usa a segunda técnica&mdash;ela exibe a solicitação de volta como resposta, e ele acessa `args->Socket` de dentro de uma das mais internas continuações.

A terceira técnica é apropriada quando você não estiver exibindo uma resposta de volta. Você usa a opção `task_continuation_context::use_synchronous_execution()` para forçar o PPL a executar a continuação do corpo embutido. Aqui está um exemplo de código mostrando como fazê-lo.

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);

    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        unsigned int messageLength = dataReader->ReadUInt32();
        Concurrency::create_task(dataReader->LoadAsync(messageLength)).then(
            [=](unsigned int bytesLoaded)
        {
            Platform::String^ request = dataReader->ReadString(bytesLoaded);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                [=]
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
            }));
        });
    }, Concurrency::task_continuation_context::use_synchronous_execution());
}
```

Este comportamento se aplica a todos os soquetes e classes WebSockets no namespace [**Networking**](/uwp/api/Windows.Networking.Sockets?branch=live). Mas cenários do lado do cliente geralmente armazenam soquetes em variáveis membro, portanto, o problema é mais aplicável ao cenário [**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived), conforme ilustrado acima.

## <a name="build-a-basic-udp-socket-client-and-server"></a>Criar um cliente de soquete UDP básico e servidor
Um soquete UDP (User Datagram Protocol) é semelhante a um soquete TCP que também fornece transferências de dados de rede de baixo nível em qualquer direção. Porém, enquanto um soquete TCP é usado para conexões de longa duração, um soquete UDP é usado para aplicativos em que não é necessária uma conexão estabelecida. Como os soquetes UDP não mantêm conexão nos dois pontos de extremidade, eles fornecem uma solução rápida e simples de rede entre computadores remotos. Porém, os soquetes UDP não garantem a integridade dos pacotes de rede ou se eles são recebidos no destino remoto. Então, seu aplicativo precisa ser projetado para tolerar isso. Alguns exemplos de aplicativos que usam soquetes UDP são a descoberta de rede local e clientes de chat local.

Para demonstrar as operações básicas de UDP, o código de exemplo abaixo mostra a classe [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) sendo usada tanto para enviar como para receber dados por UDP para formar um cliente de eco e o servidor. Crie um novo projeto e coloque o cliente e o código do servidor abaixo no mesmo projeto. Assim como acontece com um soquete TCP, será necessário declarar a funcionalidade de aplicativo **Redes Privadas (Cliente & Servidor)**.

### <a name="an-echo-client-and-server-using-udp-sockets"></a>Um cliente de eco e o servidor, usando soquetes UDP
Construa um [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) para reproduzir a função do servidor de eco, associá-lo a um número de porta específico, e escutar uma mensagem de UDP de entrada e reproduzi-la de volta. O evento [**DatagramSocket.MessageReceived**](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived) é acionado quando uma mensagem é recebida no soquete.

Construa um outro **DatagramSocket** para reproduzir a função do cliente de eco, associe-o a um número de porta específico, envie uma mensagem de UDP de entrada e receba a resposta.

Crie uma nova **Página** chamada `DatagramSocketPage`. Coloque a marcação XAML no `DatagramSocketPage.xaml` e o coloque a necessidade de código dentro da classe `DatagramSocketPage`.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="UDP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="DatagramSocket"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose different arbitrary port numbers for client and server, since both will be running on the same machine.
static string ClientPortNumber = "1336";
static string ServerPortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        // The ConnectionReceived event is raised when connections are received.
        serverDatagramSocket.MessageReceived += ServerDatagramSocket_MessageReceived;

        this.serverListBox.Items.Add("server is about to bind...");

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await serverDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ServerPortNumber);

        this.serverListBox.Items.Add(string.Format("server is bound to port number {0}", DatagramSocketPage.ServerPortNumber));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ServerDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string request;
    using (DataReader dataReader = args.GetDataReader())
    {
        request = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = (await sender.GetOutputStreamAsync(args.RemoteAddress, DatagramSocketPage.ClientPortNumber)).AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the DatagramSocket and establish a connection to the echo server.
        var clientDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        clientDatagramSocket.MessageReceived += ClientDatagramSocket_MessageReceived;

        // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
        var hostName = new Windows.Networking.HostName("localhost");

        this.clientListBox.Items.Add("client is about to bind...");

        await clientDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ClientPortNumber);

        this.clientListBox.Items.Add(string.Format("client is bound to port number {0}", DatagramSocketPage.ClientPortNumber));

        // Send a request to the echo server.
        string request = "Hello, World!";
        using (var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket())
        {
            using (Stream outputStream = (await serverDatagramSocket.GetOutputStreamAsync(hostName, DatagramSocketPage.ServerPortNumber)).AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }
        }

        this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ClientDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string response;
    using (DataReader dataReader = args.GetDataReader())
    {
        response = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\"", response)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add("client closed its socket"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::DatagramSocket m_clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket m_serverDatagramSocket;

public:
    void OnNavigatedTo(NavigationEventArgs const& /* e */)
    {
        StartServer();
        StartClient();
    }

private:
    IAsyncAction StartServer()
    {
        try
        {
            // The ConnectionReceived event is raised when connections are received.
            m_serverDatagramSocket.MessageReceived({ this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived });

            serverListBox().Items().Append(winrt::box_value(L"server is about to bind..."));

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            co_await m_serverDatagramSocket.BindServiceNameAsync(L"1337");
            serverListBox().Items().Append(winrt::box_value(L"server is bound to port number 1337"));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    IAsyncAction ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs args)
    {
        DataReader dataReader{ args.GetDataReader() };
        winrt::hstring request{ dataReader.ReadString(dataReader.UnconsumedBufferLength()) };

        serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request.c_str() << L"\"";
            serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        });

        // Echo the request back as the response.
        IOutputStream outputStream = co_await sender.GetOutputStreamAsync(args.RemoteAddress(), L"1336");
        DataWriter dataWriter{ outputStream };
        dataWriter.WriteString(request);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();

        serverListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"server sent back the response: \"" << request.c_str() << L"\"";
            serverListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));

            m_serverDatagramSocket = nullptr;

            serverListBox().Items().Append(winrt::box_value(L"server closed its socket"));
        });
    }

    IAsyncAction StartClient()
    {
        try
        {
            m_clientDatagramSocket.MessageReceived({ this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived });

            // Establish a connection to the echo server.
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            Windows::Networking::HostName hostName{ L"localhost" };

            clientListBox().Items().Append(winrt::box_value(L"client is about to bind..."));

            co_await m_clientDatagramSocket.BindServiceNameAsync(L"1336");
            clientListBox().Items().Append(winrt::box_value(L"client is bound to port number 1336"));

            // Send a request to the echo server.
            Windows::Networking::Sockets::DatagramSocket serverDatagramSocket;
            IOutputStream outputStream = co_await m_serverDatagramSocket.GetOutputStreamAsync(hostName, L"1337");

            winrt::hstring request{ L"Hello, World!" };
            DataWriter dataWriter{ outputStream };
            dataWriter.WriteString(request);
            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();

            std::wstringstream wstringstream;
            wstringstream << L"client sent the request: \"" << request.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(ex.to_abi()) };
            serverListBox().Items().Append(webErrorStatus != Windows::Networking::Sockets::SocketErrorStatus::Unknown ? winrt::box_value(winrt::to_hstring((int32_t)webErrorStatus)) : winrt::box_value(winrt::to_hstring(ex.to_abi())));
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket const& /* sender */, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs const& args)
    {
        DataReader dataReader{ args.GetDataReader() };
        winrt::hstring response{ dataReader.ReadString(dataReader.UnconsumedBufferLength()) };
        clientListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response.c_str() << L"\"";
            clientListBox().Items().Append(winrt::box_value(wstringstream.str().c_str()));
        });

        m_clientDatagramSocket = nullptr;

        clientListBox().Dispatcher().RunAsync(CoreDispatcherPriority::Normal, [=]()
        {
            clientListBox().Items().Append(winrt::box_value(L"client closed its socket"));
        });
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::DatagramSocket^ clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket^ serverDatagramSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            // The ConnectionReceived event is raised when connections are received.
            this->serverDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived);

            this->serverListBox->Items->Append(L"server is about to bind...");

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            Concurrency::create_task(this->serverDatagramSocket->BindServiceNameAsync("1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is bound to port number 1337");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ request = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request->Data() << L"\"";
            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        // Echo the request back as the response.
        Concurrency::create_task(sender->GetOutputStreamAsync(args->RemoteAddress, "1336")).then(
            [=](IOutputStream^ outputStream)
        {
            auto dataWriter = ref new DataWriter(outputStream);
            dataWriter->WriteString(request);
            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](unsigned int)
            {
                dataWriter->DetachStream();

                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                    [=]()
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                    this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    delete this->serverDatagramSocket;
                    this->serverDatagramSocket = nullptr;

                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                }));
            });
        });
    }

    void StartClient()
    {
        try
        {
            // Create the DatagramSocket and establish a connection to the echo server.
            this->clientDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            this->clientDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived);

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is about to bind...");

            Concurrency::create_task(this->clientDatagramSocket->BindServiceNameAsync("1336")).then(
                [=]
            {
                this->clientListBox->Items->Append(L"client is bound to port number 1336");
            });

            // Send a request to the echo server.
            auto serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();
            Concurrency::create_task(serverDatagramSocket->GetOutputStreamAsync(hostName, "1337")).then(
                [=](IOutputStream^ outputStream)
            {
                auto request = ref new Platform::String(L"Hello, World!");
                auto dataWriter = ref new DataWriter(outputStream);
                dataWriter->WriteString(request);
                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](unsigned int)
                {
                    dataWriter->DetachStream();
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                });

            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ response = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response->Data() << L"\"";
            this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        delete this->clientDatagramSocket;
        this->clientDatagramSocket = nullptr;

        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->clientListBox->Items->Append(L"client closed its socket"); }));
    }
```

## <a name="background-operations-and-the-socket-broker"></a>Operações em segundo plano e o agente de soquete
Você pode usar o agente de soquete e gatilhos de canal, para garantir que seu aplicativo receba corretamente dados ou conexões em soquetes quando ele não estiver em primeiro plano. Para obter mais informações, consulte [Comunicações de rede em segundo plano](network-communications-in-the-background.md).

## <a name="batched-sends"></a>Envios em lote
Sempre que você gravar no fluxo associado a um soquete, uma transição ocorre do modo de usuário (seu código) para o modo de kernel (onde a pilha de rede está). Se você estiver escrevendo vários buffers de cada vez, então estas transições repetidas irão compor em considerável sobrecarga. Envios em lote é uma maneira de enviar vários buffers de dados juntos e evitar essa sobrecarga. Isso é especialmente útil se seu aplicativo está fazendo VoIP, VPN ou outras tarefas que envolvem mover uma grande quantidade de dados de forma mais eficiente possível.

Esta seção demonstra algumas técnicas envios em lote que você pode usar com um [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) ou um [**DatagramSocket **](/uwp/api/Windows.Networking.Sockets.DatagramSocket) conectado.

Para obter uma linha de base, vamos ver como enviar um grande número de buffers de forma ineficiente. Aqui está uma demonstração mínima, usando um **StreamSocket**.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();
    streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;
    await streamSocketListener.BindServiceNameAsync("1337");

    var streamSocket = new Windows.Networking.Sockets.StreamSocket();
    await streamSocket.ConnectAsync(new Windows.Networking.HostName("localhost"), "1337");
    this.SendMultipleBuffersInefficiently(streamSocket, "Hello, World!");
    //this.BatchedSendsCSharpOnly(streamSocket, "Hello, World!");
    //this.BatchedSendsAnyUWPLanguage(streamSocket, "Hello, World!");
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    using (var dataReader = new DataReader(args.Socket.InputStream))
    {
        dataReader.InputStreamOptions = InputStreamOptions.Partial;
        while (true)
        {
            await dataReader.LoadAsync(256);
            if (dataReader.UnconsumedBufferLength == 0) break;
            IBuffer requestBuffer = dataReader.ReadBuffer(dataReader.UnconsumedBufferLength);
            string request = Windows.Security.Cryptography.CryptographicBuffer.ConvertBinaryToString(Windows.Security.Cryptography.BinaryStringEncoding.Utf8, requestBuffer);
            Debug.WriteLine(string.Format("server received the request: \"{0}\"", request));
        }
    }
}

// This implementation incurs kernel transition overhead for each packet written.
private async void SendMultipleBuffersInefficiently(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    foreach (IBuffer packet in packetsToSend)
    {
        await streamSocket.OutputStream.WriteAsync(packet);
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener m_streamSocketListener;
    Windows::Networking::Sockets::StreamSocket m_streamSocket;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_streamSocketListener.ConnectionReceived({ this, &BatchedSendsPage::OnConnectionReceived });
        co_await m_streamSocketListener.BindServiceNameAsync(L"1337");

        co_await m_streamSocket.ConnectAsync(Windows::Networking::HostName{ L"localhost" }, L"1337");
        SendMultipleBuffersInefficientlyAsync(L"Hello, World!");
        //BatchedSendsAnyUWPLanguageAsync(L"Hello, World!");
    }

private:
    IAsyncAction OnConnectionReceived(Windows::Networking::Sockets::StreamSocketListener const& /* sender */, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
    {
        DataReader dataReader{ args.Socket().InputStream() };
        dataReader.InputStreamOptions(Windows::Storage::Streams::InputStreamOptions::Partial);

        while (true)
        {
            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            if (bytesLoaded == 0) break;
            winrt::hstring message{ dataReader.ReadString(bytesLoaded) };
            ::OutputDebugString(message.c_str());
        }
    }

    // This implementation incurs kernel transition overhead for each packet written.
    IAsyncAction SendMultipleBuffersInefficientlyAsync(winrt::hstring message)
    {
        co_await winrt::resume_background();

        std::vector< IBuffer > packetsToSend;
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto const& element : packetsToSend)
        {
            m_streamSocket.OutputStream().WriteAsync(element).get();
        }
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();
        streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &BatchedSendsPage::StreamSocketListener_ConnectionReceived);
        Concurrency::create_task(this->streamSocketListener->BindServiceNameAsync(L"1337")).then(
            [=]
        {
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();
            Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->SendMultipleBuffersInefficiently(L"Hello, World!");
                // this->BatchedSendsAnyUWPLanguage(L"Hello, World!");
            }, Concurrency::task_continuation_context::use_synchronous_execution());
        });
    }

private:
    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        auto dataReader = ref new DataReader(args->Socket->InputStream);
        dataReader->InputStreamOptions = Windows::Storage::Streams::InputStreamOptions::Partial;
        this->ReceiveStringRecurse(dataReader, args->Socket);
    }

    void ReceiveStringRecurse(DataReader^ dataReader, Windows::Networking::Sockets::StreamSocket^ streamSocket)
    {
        Concurrency::create_task(dataReader->LoadAsync(256)).then(
            [this, dataReader, streamSocket](unsigned int bytesLoaded)
        {
            if (bytesLoaded == 0) return;
            Platform::String^ message = dataReader->ReadString(bytesLoaded);
            ::OutputDebugString(message->Data());
            this->ReceiveStringRecurse(dataReader, streamSocket);
        });
    }

    // This implementation incurs kernel transition overhead for each packet written.
    void SendMultipleBuffersInefficiently(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            Concurrency::create_task(this->streamSocket->OutputStream->WriteAsync(element)).wait();
        }
    }
```

Esse primeiro exemplo de uma técnica mais eficiente só é adequado se você estiver usando C#. Altere `OnNavigatedTo` para chamar `BatchedSendsCSharpOnly`, ao invés de `SendMultipleBuffersInefficiently` ou `SendMultipleBuffersInefficientlyAsync`.

```csharp
// A C#-only technique for batched sends.
private async void BatchedSendsCSharpOnly(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingTasks = new System.Threading.Tasks.Task[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingTasks[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]).AsTask();
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete.
    System.Threading.Tasks.Task.WaitAll(pendingTasks);
}
```

O exemplo a seguir é adequado para qualquer linguagem UWP, não apenas para C#. Ele depende do comportamento em [**StreamSocket.OutputStream**](/uwp/api/windows.networking.sockets.streamsocket.OutputStream) e [**DatagramSocket.OutputStream**](/uwp/api/windows.networking.sockets.datagramsocket.OutputStream) que envia em lotes juntos. A técnica chama [**FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync) nesse fluxo de saída que, a partir do Windows 10, é garantido que irá retornar somente depois que todas as operações no fluxo de saída são concluídas.

```csharp
// An implementation of batched sends suitable for any UWP language.
private async void BatchedSendsAnyUWPLanguage(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingWrites = new IAsyncOperationWithProgress<uint, uint>[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]);
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    await streamSocket.OutputStream.FlushAsync();
}
```

```cppwinrt
// An implementation of batched sends suitable for any UWP language.
IAsyncAction BatchedSendsAnyUWPLanguageAsync(winrt::hstring message)
{
    std::vector< IBuffer > packetsToSend{};
    std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int > > pendingWrites{};
    for (unsigned int count = 0; count < 5; ++count)
    {
        packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
    }

    for (auto const& element : packetsToSend)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites.push_back(m_streamSocket.OutputStream().WriteAsync(element));
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    co_await m_streamSocket.OutputStream().FlushAsync();
}
```

```cpp
private:
    // An implementation of batched sends suitable for any UWP language.
    void BatchedSendsAnyUWPLanguage(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int >^ >pendingWrites{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            // track all pending writes as tasks, but don't wait on one before beginning the next.
            pendingWrites.push_back(this->streamSocket->OutputStream->WriteAsync(element));
            // Don't modify any buffer's contents until the pending writes are complete.
        }

        // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
        Concurrency::create_task(this->streamSocket->OutputStream->FlushAsync());
    }
```

Existem algumas limitações importantes impostas por usar envios em lote no seu código.

-   Você não pode modificar o conteúdo das instâncias **IBuffer** que estão sendo gravadas até que a gravação assíncrona seja concluída.
-   O padrão **FlushAsync** só funciona em **StreamSocket.OutputStream** e **DatagramSocket.OutputStream**.
-   O padrão de **FlushAsync** só funciona no Windows 10 em diante.
-   Em outros casos, use [**Task.WaitAll**](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitall?view=netcore-2.0#System_Threading_Tasks_Task_WaitAll_System_Threading_Tasks_Task___), em vez do padrão **FlushAsync**.

## <a name="port-sharing-for-datagramsocket"></a>Compartilhamento de DatagramSocket de porta
Você pode configurar um [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) para coexistir com outros soquetes multicast do Win32 ou UWP associados ao mesmo endereço/porta. Você faz isso definindo [**DatagramSocketControl.MulticastOnly**](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly) para `true` antes da associação ou conexão do soquete. Acessar uma instância do **DatagramSocketControl** a partir do próprio objeto **DatagramSocket** por meio de sua propriedade [**DatagramSocket.Control**](/uwp/api/windows.networking.sockets.datagramsocket.Control).

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fornecendo um certificado de cliente com a classe StreamSocket
[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) dá suporte ao uso de SSL/TLS para autenticar o servidor o qual o aplicativo do cliente se comunica. Em certos casos, o aplicativo cliente também precisa autenticar-se ao servidor usando um certificado cliente SSL/TLS. Você pode fornecer um certificado de cliente com a propriedade [**StreamSocketControl.ClientCertificate**](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate) antes da associação ou conexão do soquete (ele deve ser definido antes do handshake SSL/TLS ser iniciado). Acessar uma instância do **StreamSocketControl** a partir do próprio objeto **StreamSocket** por meio de sua propriedade [**DatagramSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.Control). Se o servidor solicitar o certificado cliente, então o Windows responderá com o certificado cliente fornecido.

Use uma substituição dos [**StreamSocket.ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) que leva um [**SocketProtectionLevel**](/uwp/api/windows.networking.sockets.socketprotectionlevel), conforme mostrado neste exemplo mínimo de código.

> [!IMPORTANT]
> Conforme indicado pelo comentário nos exemplos de código abaixo, seu projeto precisa declarar a funcionalidade de aplicativo sharedUserCertificates para que esse código funcione.

```csharp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
var certificateQuery = new Windows.Security.Cryptography.Certificates.CertificateQuery();
certificateQuery.StoreName = "MY";
IReadOnlyList<Windows.Security.Cryptography.Certificates.Certificate> certificates = await Windows.Security.Cryptography.Certificates.CertificateStores.FindAllAsync(certificateQuery);
if (certificates.Count > 0)
{
    streamSocket.Control.ClientCertificate = certificates[0];
    await streamSocket.ConnectAsync(hostName, "1337", Windows.Networking.Sockets.SocketProtectionLevel.Tls12);
}
```

```cppwinrt
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
Windows::Security::Cryptography::Certificates::CertificateQuery certificateQuery;
certificateQuery.StoreName(L"MY");
IVectorView< Windows::Security::Cryptography::Certificates::Certificate > certificates = co_await Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery);

if (certificates.Size() > 0)
{
    m_streamSocket.Control().ClientCertificate(certificates.GetAt(0));
    co_await m_streamSocket.ConnectAsync(Windows::Networking::HostName{ L"localhost" }, L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
    ...
}
```

```cpp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
auto certificateQuery = ref new Windows::Security::Cryptography::Certificates::CertificateQuery();
certificateQuery->StoreName = L"MY";
Concurrency::create_task(Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery)).then(
    [=](IVectorView< Windows::Security::Cryptography::Certificates::Certificate^ >^ certificates)
{
    if (certificates->Size > 0)
    {
        this->streamSocket->Control->ClientCertificate = certificates->GetAt(0);
        Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12)).then(
            [=]
        {
            ...
        });
    }
});
```

## <a name="handling-exceptions"></a>Tratando exceções
Um erro encontrado em uma operação [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) ou [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener) é retornado como um valor **HRESULT**. Você pode passar o valor **HRESULT** para o método [**SocketError.GetStatus**](/uwp/api/windows.networking.sockets.socketerror.getstatus) convertê-lo em um valor de enumeração [**SocketErrorStatus**](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus).

A maioria dos valores de enumeração **SocketErrorStatus** correspondem a um erro retornado pela operação nativa do Windows Sockets. Seu aplicativo pode alternar um valor específico de enumeração **SocketErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, é possível usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro. Possíveis valores de **HRESULT** são listados em `Winerror.h`, que podem ser encontrado em sua instalação do SDK (por exemplo, na pasta `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`). Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E_INVALIDARG**.

O construtor [**HostName**](/uwp/api/Windows.Networking.HostName) pode gerar uma exceção se a sequência passada não for um nome do host válido. Por exemplo, ele contém caracteres que não são permitidos, que é provável que se o nome do host é digitado em seu aplicativo pelo usuário. Construir um **HostName** dentro de um bloco try/catch. Desta forma, se uma exceção for gerada, o aplicativo poderá notificar o usuário e solicitar um novo nome do host.

## <a name="important-apis"></a>APIs Importantes
* [CertificateQuery](/uwp/api/windows.security.cryptography.certificates.certificatequery)
* [CertificateStores.FindAllAsync](/uwp/api/windows.security.cryptography.certificates.certificatestores.findallasync)
* [DatagramSocket](/uwp/api/Windows.Networking.Sockets.DatagramSocket)
* [DatagramSocket.BindServiceNameAsync](/uwp/api/windows.networking.sockets.datagramsocket.bindservicenameasync)
* [DatagramSocket.Control](/uwp/api/windows.networking.sockets.datagramsocket.Control)
* [DatagramSocket.GetOutputStreamAsync](/uwp/api/windows.networking.sockets.datagramsocket.getoutputstreamasync)
* [DatagramSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived)
* [DatagramSocketControl.MulticastOnly](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)
* [DatagramSocketMessageReceivedEventArgs](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs)
* [DatagramSocketMessageReceivedEventArgs.GetDataReader](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs.GetDataReader)
* [DataReader.LoadAsync](/uwp/api/windows.storage.streams.datareader.loadasync)
* [IOutputStream.FlushAsync](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)
* [SocketError.GetStatus](/uwp/api/windows.networking.sockets.socketerror.getstatus)
* [SocketErrorStatus](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus)
* [SocketProtectionLevel](/uwp/api/windows.networking.sockets.socketprotectionlevel)
* [StreamSocket](/uwp/api/Windows.Networking.Sockets.StreamSocket)
* [StreamSocketControl.ClientCertificate](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamSocket.InputStream](/uwp/api/windows.networking.sockets.streamsocket.InputStream)
* [StreamSocket.OutputStream](/uwp/api/windows.networking.sockets.streamsocket.OutputStream)
* [StreamSocketListener](/uwp/api/Windows.Networking.Sockets.StreamSocketListener)
* [StreamSocketListener.BindServiceNameAsync](/uwp/api/windows.networking.sockets.streamsocketlistener.bindservicenameasync)
* [StreamSocketListener.ConnectionReceived](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)
* [StreamSocketListenerConnectionReceivedEventArgs](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>Tópicos relacionados
* [Comunicação entre aplicativos](/windows/uwp/app-to-app/index)
* [Simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [Como definir recursos de rede](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)
* [Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673)

## <a name="samples"></a>Exemplos
* [Exemplo do StreamSocket](http://go.microsoft.com/fwlink/p/?LinkId=620609)

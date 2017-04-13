---
author: DelfCo
description: "Você pode usar Windows.Networking.Sockets e Winsock para se comunicar com outros dispositivos como um desenvolvedor de aplicativo da Plataforma Universal do Windows (UWP)."
title: Soquetes
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8933fb5c970203746fe1a00c71c0630fa264ebf6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="sockets"></a>Soquetes

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

Você pode usar [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) e [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) para se comunicar com outros dispositivos como um desenvolvedor de aplicativo da Plataforma Universal do Windows (UWP). Este tópico fornece orientações detalhadas sobre como usar o namespace **Windows.Networking.Sockets** para executar operações de rede.

>**Observação** Como parte do [isolamento de rede](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx), o sistema proíbe estabelecer conexões de soquete (soquetes ou WinSock) entre dois aplicativos UWP em execução no mesmo computador por um endereço de loopback local (127.0.0.0) ou especificando explicitamente o endereço IP local. Isso significa que você não pode usar soquetes para se comunicar entre dois aplicativos UWP. A UWP fornece outros mecanismos de comunicação entre aplicativos. Consulte [Comunicações entre aplicativos](https://msdn.microsoft.com/windows/uwp/app-to-app/index) para obter detalhes.

## <a name="basic-tcp-socket-operations"></a>Operações de soquete TCP básicas

Um soquete TCP fornece transferências de dados de rede em baixo nível em qualquer direção para conexões de longa duração. Os soquetes TCP são o recurso subjacente usado pela maioria dos protocolos de rede disponíveis na Internet. Esta seção mostra como habilitar um aplicativo UWP para enviar e receber dados com um soquete de fluxo TCP usando as classes [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) e [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) como parte do namespace [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960). Para esta seção, estamos criando um aplicativo muito simples que funcionará como um servidor e um cliente de eco para demonstrar as operações básicas de TCP.

**Criando um servidor de eco TCP**

O exemplo de código a seguir demonstra como criar um objeto [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) e iniciar a escuta de conexões TCP de entrada.

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

O exemplo de código a seguir implementa o manipulador de eventos SocketListener\_ConnectionReceived que foi anexado ao evento [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) no exemplo acima. Esse manipulador de eventos é chamado pela classe [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) sempre que um cliente remoto tiver estabelecido uma conexão com o servidor de eco.

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**Criando um cliente de eco TCP**

O exemplo de código a seguir demonstra como criar um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), estabelecer uma conexão com o servidor remoto, enviar uma solicitação e receber uma resposta.

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## <a name="basic-udp-socket-operations"></a>Operações de soquete UDP básicas

Um soquete UDP fornece transferências de dados de rede de baixo nível em qualquer direção para comunicação de rede que não requer uma conexão estabelecida. Como os soquetes UDP não mantêm conexão nos dois pontos de extremidade, eles fornecem uma solução rápida e simples de rede entre computadores remotos. No entanto, os soquetes UDP não garantem a integridade dos pacotes de rede ou se eles são recebidos no destino remoto. Alguns exemplos de aplicativos que usam soquetes UDP são a descoberta de rede local e clientes de chat local. Esta seção demonstra o uso da classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) para enviar e receber mensagens UDP criando um servidor e um cliente de eco simples.

**Criando um servidor de eco UDP**

O exemplo de código a seguir demonstra como criar um objeto [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) e associá-lo a uma porta específica para que você possa escutar mensagens UDP de entrada.

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

O código de exemplo a seguir implementa o manipulador de eventos **Socket\_MessageReceived** para ler uma mensagem que foi recebida de um cliente e enviar a mesma mensagem de volta.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**Criando um cliente de eco UDP**

O exemplo de código a seguir demonstra como criar um objeto [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) e associá-lo a uma porta específica para que você possa escutar mensagens UDP de entrada e enviar uma mensagem UDP para o servidor de eco UDP.

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

O código de exemplo a seguir implementa o manipulador de eventos **Socket\_MessageReceived** para ler uma mensagem que foi recebida do servidor de eco UDP.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## <a name="background-operations-and-the-socket-broker"></a>Operações em segundo plano e o agente de soquete

Se seu aplicativo recebe dados ou conexões em soquetes, você deve estar preparado para executar essas operações corretamente enquanto seu aplicativo não estiver em primeiro plano. Para fazer isso, você usa o agente de soquete. Para obter mais informações sobre como usar o agente de soquete, consulte [Comunicações de rede em segundo plano](network-communications-in-the-background.md).

## <a name="batched-sends"></a>Envios em lote

Começando com o Windows 10, o Windows.Networking.Sockets dá suporte a envios em lote, uma maneira de enviar vários buffers de dados juntos com muito menos sobrecarga de alternância do que caso você envie cada um dos buffers separadamente. Isso é especialmente útil se seu aplicativo está fazendo VoIP, VPN ou outras tarefas que envolvem mover uma grande quantidade de dados de forma mais eficiente possível.

Cada chamada para o WriteAsync em um soquete dispara uma transição de kernel para alcançar a pilha de rede. Quando um aplicativo grava vários buffers de cada vez, cada gravação provoca uma transição de kernel separada, e isso cria uma considerável sobrecarga. O novo padrão de envio em lote otimiza a frequência das transições de kernel. Essa funcionalidade é limitada a [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) e conectado as instância [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) no momento.

Aqui está um exemplo de como um aplicativo pode enviar um grande número de buffers de maneira não ideal.

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

Este exemplo mostra uma maneira mais eficiente para enviar um grande número de buffers. Como essa técnica usa recursos exclusivos para a linguagem C#, ela só está disponível para programadores de C#. Ao enviar vários pacotes de uma vez, este exemplo permite que o sistema faça envios em lote e, portanto, otimize as transições de kernel para melhorar o desempenho.

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

Este exemplo mostra outra maneira de enviar um grande número de buffers de maneira que seja compatível com envios em lote. E desde que não use qualquer recurso específico do C#, ele é aplicável para todas as linguagens (embora seja demonstrado aqui em C#). Em vez disso, ele usa o comportamento alterado no membro **OutputStream** das classes [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) e [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) que são novas no Windows 10.

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

Em versões anteriores do Windows, **FlushAsync** é retornado imediatamente e não garante que todas as operações no fluxo tinham sido concluídas ainda. No Windows 10, o comportamento mudou. Agora é garantido que o**FlushAsync** retorne depois que todas as operações no fluxo de saída são concluídas.

Existem algumas limitações importantes impostas por usar gravações em lote no seu código.

-   Você não pode modificar o conteúdo das instâncias **IBuffer** que estão sendo gravadas até que a gravação assíncrona seja concluída.
-   O padrão **FlushAsync** só funciona em **StreamSocket.OutputStream** e **DatagramSocket.OutputStream**.
-   O padrão **FlushAsync** só funciona no Windows 10 em diante.
-   Em outros casos, use **Task.WaitAll**, em vez do padrão **FlushAsync**.

## <a name="port-sharing-for-datagramsocket"></a>Compartilhamento de DatagramSocket de porta

O Windows 10 apresenta uma nova propriedade [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190), [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368), que permite especificar que o **DatagramSocket** em questão é capaz de coexistir com outros soquetes multicast do Win32 ou WinRT associados ao mesmo endereço/porta.

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fornecendo um certificado cliente com a classe StreamSocket

A classe [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) dá suporte ao uso de SSL/TLS para autenticar o servidor o qual o aplicativo se comunica. Em certos casos, o aplicativo também precisa autenticar-se ao servidor usando um certificado de cliente TLS. No Windows 10, você pode fornecer um certificado cliente sobre o objeto [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) (isso deve ser definido antes do handshake TLS ser iniciado). Se o servidor solicitar o certificado cliente, o Windows responderá com o certificado fornecido.

Aqui está um trecho de código mostrando como implementar isso:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## <a name="exceptions-in-windowsnetworkingsockets"></a>Exceções em Windows.Networking.Sockets

O construtor da classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) usado com soquetes pode gerar uma exceção se a cadeia de caracteres passada não for um nome de host válido (ou seja, contém caracteres que não são permitidos em um nome de host). Se um aplicativo receber entrada do usuário para o **HostName**, o construtor deverá estar em um bloco try/catch. Se uma exceção for lançada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

O namespace [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) tem métodos auxiliares e enumerações práticas para resolver erros durante o uso de soquetes e WebSockets. Eles são úteis para resolver exceções de rede específicas de uma outra forma em seu aplicativo.

Um erro encontrado em uma operação [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) é retornado como um valor **HRESULT**. O método [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) é usado para converter um erro de rede de uma operação de soquete em um valor de enumeração [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457). A maioria dos valores de enumeração **SocketErrorStatus** corresponde a um erro retornado pela operação nativa de soquetes do Windows. Um aplicativo pode filtrar por um valor específico de enumeração **SocketErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Um erro encontrado em uma operação [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) é retornado como um valor **HRESULT**. O método [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) é usado para converter um erro de rede de uma operação WebSocket em um valor de enumeração [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). A maioria dos valores de enumeração **WebErrorStatus** corresponde a um erro retornado pela operação nativa de cliente HTTP. Um aplicativo pode filtrar por um valor específico de enumeração **WebErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, um aplicativo também pode usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro causador da exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**.

## <a name="the-winsock-api"></a>A API de Winsock

Você também pode usar o [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673) em seu aplicativo UWP. A API de Winsock suportada é baseada na API do Windows Phone 8.1 e Microsoft Silverlight, e continua a dar suporte a maioria dos tipos, propriedades e métodos (algumas APIs que são consideradas obsoletas foram removidas). É possível encontrar mais informações sobre a programação Winsock [aqui](https://msdn.microsoft.com/library/windows/desktop/ms740673).



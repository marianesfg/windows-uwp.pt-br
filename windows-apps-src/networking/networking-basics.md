---
description: Coisas que você deve fazer para qualquer aplicativo habilitado por rede.
title: Noções básicas de rede
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c1c23bc205c5f9e2ad24e201e9583e19f2d6ec35
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320668"
---
# <a name="networking-basics"></a>Noções básicas de rede
Coisas que você deve fazer para qualquer aplicativo habilitado por rede.

## <a name="capabilities"></a>Funcionalidades
Para usar a rede, você deve adicionar elementos de recurso apropriados ao manifesto do aplicativo. Se nenhum recurso de rede for especificado no manifesto do aplicativo, o aplicativo não terá nenhum recurso de rede, e qualquer tentativa de conexão com a rede falhará.

Estes são os recursos de rede mais usados.

| Capacidade | Descrição |
|------------|-------------|
| **internetClient** | Dá acesso de saída para a Internet e redes em lugares públicos, como aeroportos e restaurantes. A maioria dos aplicativos que exige o acesso à Internet deve usar esse recurso. |
| **internetClientServer** | Dá ao aplicativo acesso à rede de entrada e de saída a partir da Internet e de redes em lugares públicos, como aeroportos e restaurantes. |
| **privateNetworkClientServer** | Dá ao aplicativo acesso à rede de entrada e de saída nos lugares confiáveis do usuário, como a residência e o trabalho. |

Há outros recursos que podem ser necessários para o seu aplicativo, em determinadas circunstâncias.

| Capacidade | Descrição |
|------------|-------------|
| **enterpriseAuthentication** | Permite que um aplicativo se conecte a recursos de rede que exigem credenciais de domínio. Por exemplo, um aplicativo que recupera dados de servidores do SharePoint em uma intranet privada. Com esse recurso, suas credenciais podem ser usadas para acessar os recursos de rede em uma rede que exige credenciais. Um aplicativo com esse recurso pode representá-lo na rede. Você não precisa dessa funcionalidade para que seu aplicativo acesse a Internet por meio de um proxy de autenticação.<br/><br/>Para obter mais detalhes, consulte a documentação do cenário do recurso *Enterprise* em [Recursos restritos](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities). |
| **proximity** | Obrigatório para comunicação por proximidade a curta distância com dispositivos próximos ao computador. A proximidade a curta distância pode ser usada para enviar para ou se conectar a um aplicativo em um dispositivo próximo. <br/><br/> Essa funcionalidade permite que um aplicativo acesse a rede para conectar-se a um dispositivo em proximidade a curta distância, com o consentimento do usuário para enviar ou aceitar um convite. |
| **sharedUserCertificates** | Esta funcionalidade permite que um aplicativo acesse certificados de software e de hardware, como certificados de cartão inteligente. Quando a funcionalidade é invocada no tempo de execução, o usuário deve agir, por exemplo, inserindo um cartão ou selecionando um certificado. <br/><br/> Com esse recurso, os certificados de software e de hardware ou um cartão inteligente são usados para a identificação no aplicativo. Ele pode ser usado pelo seu empregador, banco ou serviços governamentais para identificação. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>Comunicando-se quando seu aplicativo não está em primeiro plano
[Dar suporte a seu aplicativo com tarefas em segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks) contém informações gerais sobre o uso de tarefas em segundo plano para trabalhar quando o aplicativo não está no primeiro plano Mais especificamente, seu código deve seguir etapas especiais para ser notificado quando ele não for o aplicativo em primeiro plano atual e chegarem dados pela rede para ele. Você usou gatilhos de Canal de Controle com essa finalidade no Windows 8, e ainda há suporte para eles no Windows 10. Informações completas sobre o uso de gatilhos de canal de controle estão disponíveis [**aqui**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Uma nova tecnologia no Windows 10 oferece uma funcionalidade melhor com menos sobrecarga para alguns cenários, como soquetes de fluxo habilitados por push: os gatilhos de agente de soquete e atividade de soquete.

Se o seu aplicativo usa [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) ou [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener), então ele pode transferir a propriedade de um soquete aberto para um agente de soquete fornecido pelo sistema, e sair do primeiro plano ou, até mesmo, terminar. Quando uma conexão é estabelecida no soquete transferido, ou quando chega tráfego nesse soquete, seu aplicativo ou a tarefa em segundo plano designada é ativada. Se seu aplicativo não estiver em execução, ele será iniciado. Em seguida, usando um [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger), o agente de soquete notifica o aplicativo de que novo tráfego chegou. O aplicativo recupera o soquete de agente de soquete e processa o tráfego no soquete. Isso significa que seu aplicativo consome muito menos recursos do sistema quando não está processando ativamente o tráfego de rede.

O agente de soquete destina-se a substituir gatilhos de canal de controle, onde for aplicável, pois ele fornece a mesma funcionalidade, mas com menos restrições e um menor volume de memória. O agente de soquete pode ser usado por aplicativos que não são aplicativos de tela de bloqueio e ele é usado da mesma maneira em telefones como em outros dispositivos. Os aplicativos não precisam ser executados quando o tráfego chega para ser ativado pelo agente de soquete. E o agente de soquete dá suporte à escuta em soquetes TCP, que não são suportados por gatilhos de canal de controle.

### <a name="choosing-a-network-trigger"></a>Escolhendo um gatilho de rede
Existem alguns cenários onde qualquer tipo de gatilho seria adequado. Ao escolher o tipo de gatilho para usar em seu aplicativo, considere o seguinte aviso.

-   Se estiver usando [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2), [**System.Net.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) ou [System.Net.Http.HttpClientHandler](https://go.microsoft.com/fwlink/p/?linkid=241638), você deverá usar [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).
-   Se você está usando **StreamSockets** habilitados por push, pode usar gatilhos de canal de controle, mas prefira [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). Esta última opção permite que o sistema libere memória e reduza os requisitos de energia quando a conexão não está sendo usada ativamente.
-   Se você deseja minimizar o volume de memória de seu aplicativo quando ele não está atendendo ativamente as solicitações de rede, prefira [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) quando possível.
-   Se você deseja que seu aplicativo seja capaz de receber dados enquanto o sistema estiver no modo de espera conectado, use [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger).

Para obter detalhes e exemplos de como usar o agente de soquete, consulte [Comunicações de rede em segundo plano](network-communications-in-the-background.md).

## <a name="secured-connections"></a>Conexões seguras
Secure Sockets Layer (SSL) e o mais recente Transport Layer Security (TLS) são protocolos criptográficos projetados para oferecer autenticação e criptografia para comunicações por rede. Esses protocolos foram desenvolvidos para impedir a interceptação e a manipulação de dados enviados ou recebidos pela rede. Esses protocolos usam um modelo cliente-servidor para as trocas de protocolos. Esses protocolos também usam certificados digitais e autoridades de certificação para verificar se o servidor é realmente quem diz ser.

### <a name="creating-secure-socket-connections"></a>Criando conexões de soquete seguras
Um objeto [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) pode ser configurado para usar SSL/TLS para comunicações entre o cliente e o servidor. Esse suporte a SSL/TLS está limitado ao uso do objeto **StreamSocket** como o cliente na negociação SSL/TLS. Você não pode usar SSL/TLS com o **StreamSocket** criado por um [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) quando são recebidas comunicações de entrada, porque a negociação SSL/TLS como um servidor não é implementada pela classe **StreamSocket**.

Há duas maneiras de proteger uma conexão [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) com SSL/TLS:

-   [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) – estabeleça a conexão inicial com um serviço de rede e negocie imediatamente para usar SSL/TLS em todas as comunicações.
-   [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) – conecte inicialmente a um serviço de rede sem criptografia. O aplicativo pode enviar ou receber dados. Feito isso, atualize a conexão para usar SSL/TLS em todas as comunicações adicionais.

O SocketProtectionLevel especifica o nível de proteção de soquete desejado que o aplicativo quer estabelecer ou para atualizar a conexão. No entanto, o nível de proteção eventual da conexão estabelecida é determinado em um processo de negociação entre ambos os pontos de extremidade da conexão. O resultado pode ser um nível de proteção menor do que o especificado, se o outro ponto de extremidade solicitar um nível inferior. 

 Após a conclusão da operação assíncrona, é possível recuperar o nível de proteção solicitado usado na chamada de [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) ou [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) pela propriedade [**StreamSocketinformation.ProtectionLevel**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel). No entanto, isso não reflete o nível de proteção real que a conexão está usando.

> [!NOTE]
> Seu código não deve depender implicitamente do uso de um nível específico de proteção, ou da suposição de que um dado nível de segurança é usado por padrão. O panorama da segurança muda constantemente, e protocolos e níveis de proteção padrão são alterados com o passar do tempo para evitar o uso de protocolos com pontos fracos conhecidos. Os padrões podem variar dependendo da configuração do computador individual ou de qual software está instalado e de quais patches foram aplicados. Se o seu aplicativo depende do uso de um nível específico de segurança, você deve especificar explicitamente esse nível e, depois, verificar se ele está efetivamente em uso na conexão estabelecida.

### <a name="use-connectasync"></a>Usar ConnectAsync
[**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) pode ser usado para estabelecer a conexão inicial com um serviço de rede e, em seguida, negociar imediatamente para usar SSL/TLS em todas as comunicações. Há dois métodos **ConnectAsync** que dão suporte a um parâmetro *protectionLevel*:

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)** ](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) – Inicia uma operação assíncrona em um objeto [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) para se conectar a um destino de rede remoto especificado como um objeto [**EndpointPair**](https://docs.microsoft.com/uwp/api/Windows.Networking.EndpointPair) e um [**SocketProtectionLevel**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.SocketProtectionLevel).
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)** ](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) – inicia uma operação assíncrona em um objeto [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) para conectar-se a um destino remoto especificado por um nome de host remoto, um nome de serviço remoto e um [**SocketProtectionLevel**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.SocketProtectionLevel).

Se o parâmetro *protectionLevel* é definido como **Windows.Networking.Sockets.SocketProtectionLevel.Ssl** ao chamar um dos métodos [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) acima, o [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) será estabelecido para usar SSL/TLS para criptografia. Esse valor exige criptografia e jamais permite o uso de uma criptografia NULL.

A sequência normal a ser usada com um desses métodos [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) é a mesma.

-   Crie um [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket).
-   Se for necessária uma opção avançada no soquete, use a propriedade [**StreamSocket.Control**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.control) para obter a instância [**StreamSocketControl**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketControl) associada a um objeto [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). Defina uma propriedade no **StreamSocketControl**.
-   Chame um dos métodos [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) acima para iniciar uma operação para conectar a um destino remoto e negociar imediatamente o uso de SSL/TLS.
-   A força da SSL efetivamente negociada usando [**ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) pode ser determinada obtendo-se a propriedade [**StreamSocketinformation.ProtectionLevel**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel) depois que a operação assíncrona for concluída com êxito.

O exemplo a seguir cria um [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) e tenta estabelecer uma conexão com o serviço de rede e negociar imediatamente o uso de SSL/TLS. Se a negociação for bem-sucedida, toda a comunicação de rede que usar o **StreamSocket** entre o cliente e o servidor de rede será criptografada.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>Usar UpgradeToSslAsync
Quando o código usa [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync), primeiro ele estabelece uma conexão com um serviço de rede sem criptografia. O aplicativo pode enviar ou receber alguns dados e, em seguida, atualizar a conexão para usar SSL/TLS em todas as demais comunicações.

O método [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) aceita dois parâmetros. O parâmetro *protectionLevel* indica o nível de proteção desejado. O parâmetro *validationHostName* é o nome do host do destino de rede remoto usado para validação durante a atualização para SSL. Normalmente *validationHostName* será o mesmo nome de host que o aplicativo usou para estabelecer inicialmente a conexão. Se o parâmetro *protectionLevel* é definido como **Windows.System.Socket.SocketProtectionLevel.Ssl** ao chamar **UpgradeToSslAsync**, o [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) deve usar SSL/TLS para criptografia em comunicações posteriores pelo soquete. Esse valor exige criptografia e jamais permite o uso de uma criptografia NULL.

A sequência normal a ser usada com o método [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) é a seguinte:

-   Crie um [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket).
-   Se for necessária uma opção avançada no soquete, use a propriedade [**StreamSocket.Control**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.control) para obter a instância [**StreamSocketControl**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketControl) associada a um objeto [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). Defina uma propriedade no **StreamSocketControl**.
-   Se for necessário enviar e receber dados não criptografados, faça isso neste momento.
-   Chame o método [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) para iniciar uma operação para atualizar a conexão para usar SSL/TLS.
-   A força da SSL efetivamente negociada usando [**UpgradeToSslAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) pode ser determinada obtendo-se a propriedade [**StreamSocketinformation.ProtectionLevel**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel) depois que a operação assíncrona for concluída com êxito.

O exemplo a seguir cria um [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket), tenta estabelecer uma conexão com o serviço de rede, envia dados iniciais e negocia o uso de SSL/TLS. Se a negociação for bem-sucedida, toda a comunicação de rede que usar o **StreamSocket** entre o cliente e o servidor de rede será criptografada.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>Criação de conexões WebSocket protegidas
Assim como as conexões de soquete tradicionais, as conexões WebSocket também podem ser criptografadas com os protocolos TLS/SSL ao usar os recursos [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) e [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) para um aplicativo UWP. Na maioria dos casos, você vai usar uma conexão WebSocket segura. Isso aumentará as chances de sucesso da sua conexão, já que muitos proxies rejeitam conexões WebSocket não criptografadas.

Para obter exemplos de como criar ou atualizar uma conexão de soquete segura a um serviço de rede, consulte [Como proteger conexões WebSocket com TLS/SSL (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh994399(v=win.10)).

Além da criptografia TLS/SSL, um servidor por exigir um valor de cabeçalho **Sec-WebSocket-Protocol** para concluir o handshake inicial. Esse valor, representado pelas propriedades [**StreamWebSocketInformation.Protocol**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocketinformation.protocol) e [**MessageWebSocketInformation.Protocol**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocketinformation.protocol), indica a versão do protocolo da conexão e habilita o servidor a interpretar corretamente o handshake de abertura e os dados trocados posteriormente. Usando essas informações de protocolo, se, a qualquer momento, o servidor não puder interpretar os dados de entrada de maneira segura, a conexão poderá ser fechada.

Caso a solicitação inicial do cliente não contenha esse valor ou forneça um valor que não corresponde ao esperado pelo servidor, o valor esperado será enviado do servidor para o cliente no erro de handshake WebSocket.

## <a name="authentication"></a>Authentication
Como fornecer as credenciais de autenticação ao conectar-se pela rede.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fornecendo um certificado cliente com a classe StreamSocket
A classe [**Windows.Networking.StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) dá suporte ao uso de SSL/TLS para autenticar o servidor o qual o aplicativo se comunica. Em certos casos, o aplicativo também precisa autenticar-se ao servidor usando um certificado de cliente TLS. No Windows 10, você pode fornecer um certificado cliente sobre o objeto [**StreamSocket.Control**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketControl) (isso deve ser definido antes do handshake TLS ser iniciado). Se o servidor solicitar o certificado cliente, o Windows responderá com o certificado fornecido.

Aqui está um trecho de código mostrando como implementar isso:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>Fornecendo a autenticação de credenciais para um servidor Web
As APIs de rede que habilitam os aplicativos a interagir com serviços Web seguros fornecem seus próprios métodos para inicializar um cliente ou definir um cabeçalho de solicitação com credenciais de autenticação de servidor e proxy. Cada método é definido com um objeto [**PasswordCredential**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordCredential), o qual indica um nome de usuário, uma senha e o recurso para o qual as credenciais são usadas. A tabela a seguir fornece um mapeamento dessas APIs:

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocketcontrol.servercredential) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.messagewebsocketcontrol.proxycredential) |
|  | [**StreamWebSocketControl.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocketcontrol.servercredential) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocketcontrol.proxycredential) |
| **Transferência em Segundo Plano** | [**BackgroundDownloader.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.servercredential) |
|  | [**BackgroundDownloader.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.proxycredential) |
|  | [**BackgroundUploader.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.servercredential) |
|  | [**BackgroundUploader.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.proxycredential) |
| **Sindicalização** | [**SyndicationClient(PasswordCredential)** ](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.-ctor#Windows_Web_Syndication_SyndicationClient__ctor_Windows_Security_Credentials_PasswordCredential_) |
|  | [**SyndicationClient.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.servercredential) |
|  | [**SyndicationClient.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.proxycredential) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)** ](https://docs.microsoft.com/uwp/api/windows.web.atompub.atompubclient.-ctor#Windows_Web_AtomPub_AtomPubClient__ctor_Windows_Security_Credentials_PasswordCredential_) |
|  | [**AtomPubClient.ServerCredential**](https://docs.microsoft.com/uwp/api/windows.web.atompub.atompubclient.servercredential) |
|  | [**AtomPubClient.ProxyCredential**](https://docs.microsoft.com/uwp/api/windows.web.atompub.atompubclient.proxycredential) |

## <a name="handling-network-exceptions"></a>Manipulando exceções de rede
Na maioria das áreas de programação, uma exceção indica um problema ou uma falha grave, causada por algum defeito no programa. Na programação de rede, há uma fonte adicional para exceções: a rede em si e a natureza das comunicações de rede. As comunicações de rede são inerentemente não confiáveis e propensas a falhas inesperadas. Para cada uma das maneiras de que seu aplicativo usa a rede, você deverá manter algumas informações de estado; e o código do aplicativo deve tratar exceções de rede atualizando essas informações de estado e inicializando a lógica apropriada para o seu aplicativo restabelecer ou repetir falhas de comunicação.

Quando um aplicativo Universal do Windows lança uma exceção, seu manipulador de exceção pode recuperar informações mais detalhadas sobre a causa da exceção para melhor compreender a falha e tomar decisões apropriadas.

Cada projeção de linguagem dá suporte a um método para acessar essas informações mais detalhadas. Uma exceção é projetada como um valor **HRESULT** em aplicativos Universais do Windows. O arquivo de inclusão *Winerror.h* contém uma lista muito grande de valores **HRESULT** possíveis, incluindo erros de rede.

As APIs de rede dão suporte a métodos diferentes para recuperar essas informações detalhadas sobre a causa de uma exceção.

-   Algumas APIs fornecem um método auxiliar que converte o valor **HRESULT** da exceção em um valor de enumeração.
-   Outras APIs fornecem um método para recuperar efetivamente o valor **HRESULT**.

## <a name="related-topics"></a>Tópicos relacionados
* [Melhorias de API de rede no Windows 10](https://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)

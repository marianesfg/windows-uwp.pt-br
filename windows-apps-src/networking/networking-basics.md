---
author: stevewhims
description: Coisas que você deve fazer para qualquer aplicativo habilitado por rede.
title: Noções básicas de rede
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.author: stwhi
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50ac9fcf984fa6c4ebad7e480ebfc2d002256e26
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938446"
---
# <a name="networking-basics"></a>Noções básicas de rede
Coisas que você deve fazer para qualquer aplicativo habilitado por rede.

## <a name="capabilities"></a>Recursos
Para usar a rede, você deve adicionar elementos de recurso apropriados ao manifesto do aplicativo. Se nenhum recurso de rede for especificado no manifesto do aplicativo, o aplicativo não terá nenhum recurso de rede, e qualquer tentativa de conexão com a rede falhará.

Estes são os recursos de rede mais usados.

| Funcionalidade | Descrição |
|------------|-------------|
| **internetClient** | Dá acesso de saída para a Internet e redes em lugares públicos, como aeroportos e restaurantes. A maioria dos aplicativos que precisam de acesso à Internet deve usar esta funcionalidade. |
| **internetClientServer** | Dá ao aplicativo acesso à rede de entrada e de saída a partir da Internet e de redes em lugares públicos, como aeroportos e restaurantes. |
| **privateNetworkClientServer** | Dá ao aplicativo acesso à rede de entrada e de saída nos lugares confiáveis do usuário, como a residência e o trabalho. |

Há outros recursos que podem ser necessários para o seu aplicativo, em determinadas circunstâncias.

| Funcionalidade | Descrição |
|------------|-------------|
| **enterpriseAuthentication** | Permite que um aplicativo se conecte a recursos de rede que exigem credenciais de domínio. Esse recurso exigirá que um administrador do domínio habilite a funcionalidade em todos os aplicativos. Um exemplo seria um aplicativo que recupera dados de servidores do SharePoint em uma intranet privada. <br/> Com esse recurso, suas credenciais podem ser usadas para acessar os recursos de rede em uma rede que exige credenciais. Um aplicativo com esse recurso pode representá-lo na rede. <br/> Essa funcionalidade não é obrigatória para que um aplicativo acesse a Internet por meio de um proxy de autenticação. |
| **proximity** | Obrigatório para comunicação por proximidade a curta distância com dispositivos próximos ao computador. A proximidade a curta distância pode ser usada para enviar para ou se conectar a um aplicativo em um dispositivo próximo. <br/> Essa funcionalidade permite que um aplicativo acesse a rede para conectar-se a um dispositivo em proximidade a curta distância, com o consentimento do usuário para enviar ou aceitar um convite. |
| **sharedUserCertificates** | Esta funcionalidade permite que um aplicativo acesse certificados de software e de hardware, como certificados de cartão inteligente. Quando a funcionalidade é invocada no tempo de execução, o usuário deve agir, por exemplo, inserindo um cartão ou selecionando um certificado. <br/> Com esse recurso, os certificados de software e de hardware ou um cartão inteligente são usados para a identificação no aplicativo. Ele pode ser usado pelo seu empregador, banco ou serviços governamentais para identificação. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>Comunicando-se quando seu aplicativo não está em primeiro plano
[Dar suporte a seu app com tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt299103) contém informações gerais sobre o uso de tarefas em segundo plano para trabalhar quando o aplicativo não está no primeiro plano Mais especificamente, seu código deve seguir etapas especiais para ser notificado quando ele não for o aplicativo em primeiro plano atual e chegarem dados pela rede para ele. Você usou gatilhos de canal de controle para essa finalidade na Windows8, e eles ainda têm suporte no Windows 10. Informações completas sobre o uso de gatilhos de canal de controle estão disponíveis [**aqui**](https://msdn.microsoft.com/library/windows/apps/hh701032). Uma nova tecnologia no Windows 10 oferece uma funcionalidade melhor com menos sobrecarga para alguns cenários, como soquetes de fluxo habilitados por push: o agente de soquete e gatilhos de atividade de soquete.

Se o seu aplicativo usa [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906), então ele pode transferir a propriedade de um soquete aberto para um agente de soquete fornecido pelo sistema, e sair do primeiro plano ou, até mesmo, terminar. Quando uma conexão é estabelecida no soquete transferido, ou quando chega tráfego nesse soquete, seu aplicativo ou a tarefa em segundo plano designada é ativada. Se seu aplicativo não estiver em execução, ele será iniciado. Em seguida, usando um [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009), o agente de soquete notifica o aplicativo de que novo tráfego chegou. O aplicativo recupera o soquete de agente de soquete e processa o tráfego no soquete. Isso significa que seu aplicativo consome muito menos recursos do sistema quando não está processando ativamente o tráfego de rede.

O agente de soquete destina-se a substituir gatilhos de canal de controle, onde for aplicável, pois ele fornece a mesma funcionalidade, mas com menos restrições e um menor volume de memória. O agente de soquete pode ser usado por aplicativos que não são aplicativos de tela de bloqueio e ele é usado da mesma maneira em telefones como em outros dispositivos. Os aplicativos não precisam ser executados quando o tráfego chega para ser ativado pelo agente de soquete. E o agente de soquete dá suporte à escuta em soquetes TCP, que não são suportados por gatilhos de canal de controle.

### <a name="choosing-a-network-trigger"></a>Escolhendo um gatilho de rede
Existem alguns cenários onde qualquer tipo de gatilho seria adequado. Ao escolher o tipo de gatilho para usar em seu aplicativo, considere o seguinte aviso.

-   Se estiver usando [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) ou [System.Net.Http.HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638), você deverá usar [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).
-   Se você está usando **StreamSockets** habilitados por push, pode usar gatilhos de canal de controle, mas prefira [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009). Esta última opção permite que o sistema libere memória e reduza os requisitos de energia quando a conexão não está sendo usada ativamente.
-   Se você deseja minimizar o volume de memória de seu aplicativo quando ele não está atendendo ativamente as solicitações de rede, prefira [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) quando possível.
-   Se você deseja que seu aplicativo seja capaz de receber dados enquanto o sistema estiver no modo de espera conectado, use [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009).

Para obter detalhes e exemplos de como usar o agente de soquete, consulte [Comunicações de rede em segundo plano](network-communications-in-the-background.md).

## <a name="secured-connections"></a>Conexões seguras
Secure Sockets Layer (SSL) e o mais recente Transport Layer Security (TLS) são protocolos criptográficos projetados para oferecer autenticação e criptografia para comunicações por rede. Esses protocolos foram desenvolvidos para impedir a interceptação e a manipulação de dados enviados ou recebidos pela rede. Esses protocolos usam um modelo cliente-servidor para as trocas de protocolos. Esses protocolos também usam certificados digitais e autoridades de certificação para verificar se o servidor é realmente quem diz ser.

### <a name="creating-secure-socket-connections"></a>Criando conexões de soquete seguras
Um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) pode ser configurado para usar SSL/TLS para comunicações entre o cliente e o servidor. Esse suporte a SSL/TLS está limitado ao uso do objeto **StreamSocket** como o cliente na negociação SSL/TLS. Você não pode usar SSL/TLS com o **StreamSocket** criado por um [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) quando são recebidas comunicações de entrada, porque a negociação SSL/TLS como um servidor não é implementada pela classe **StreamSocket**.

Há duas maneiras de proteger uma conexão [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) com SSL/TLS:

-   [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) - estabeleça a conexão inicial com um serviço de rede e negocie imediatamente para usar SSL/TLS em todas as comunicações.
-   [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) - conecte inicialmente a um serviço de rede sem criptografia. O aplicativo pode enviar ou receber dados. Feito isso, atualize a conexão para usar SSL/TLS em todas as comunicações adicionais.

O SocketProtectionLevel especifica o nível de proteção de soquete desejado que o aplicativo deseja estabelecer ou para atualizar a conexão. No entanto, o nível de proteção eventual da conexão estabelecida é determinado em um processo de negociação entre ambos os pontos de extremidade da conexão. O resultado pode ser um nível de proteção menor do que o especificado, se o outro ponto de extremidade solicita um nível inferior. 

 Depois que a operação assíncrona for concluída com sucesso, é possível recuperar o nível de proteção solicitado usado na chamada [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) ou [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) pela propriedade [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868). No entanto, isso não reflete que o nível de proteção real que a conexão está usando.

> [!NOTE]
> Seu código não deve depender implicitamente do uso de um determinado nível de proteção ou da suposição de que um dado nível de segurança é usado por padrão. O panorama da segurança muda constantemente, e protocolos e níveis de proteção padrão são alterados com o passar do tempo para evitar o uso de protocolos com pontos fracos conhecidos. Os padrões podem variar dependendo da configuração do computador individual ou de qual software está instalado e de quais patches foram aplicados. Se o aplicativo depende do uso de um determinado nível de segurança, é necessário especificar explicitamente esse nível e, em seguida, verificar se ele está efetivamente em uso na conexão estabelecida.

### <a name="use-connectasync"></a>Usar ConnectAsync
[**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) pode ser usado para estabelecer a conexão inicial com um serviço de rede e, em seguida, negociar imediatamente para usar SSL/TLS em todas as comunicações. Há dois métodos **ConnectAsync** que dão suporte a um parâmetro *protectionLevel*:

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/hh701511) - inicia uma operação assíncrona em um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) para se conectar a um destino de rede remoto especificado como um objeto [**EndpointPair**](https://msdn.microsoft.com/library/windows/apps/hh700953) e um [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)**](https://msdn.microsoft.com/library/windows/apps/br226916) - inicia uma operação assíncrona em um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) para se conectar a um destino remoto especificado por um nome de host remoto, um nome de serviço remoto e um [**SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).

Se o parâmetro *protectionLevel* é definido como **Windows.Networking.Sockets.SocketProtectionLevel.Ssl** ao chamar um dos métodos [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) acima, o [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) será estabelecido para usar SSL/TLS para criptografia. Esse valor exige criptografia e jamais permite o uso de uma criptografia NULL.

A sequência normal a ser usada com um desses métodos [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) é a mesma.

-   Crie um [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Se for necessária uma opção avançada no soquete, use a propriedade [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) para obter a instância [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) associada a um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Defina uma propriedade no **StreamSocketControl**.
-   Chame um dos métodos [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) acima para iniciar uma operação para conectar a um destino remoto e negociar imediatamente o uso de SSL/TLS.
-   A força da SSL efetivamente negociada usando [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) pode ser determinada obtendo-se a propriedade [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) depois que a operação assíncrona for concluída com êxito.

O exemplo a seguir cria um [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) e tenta estabelecer uma conexão com o serviço de rede e negociar imediatamente o uso de SSL/TLS. Se a negociação for bem-sucedida, toda a comunicação de rede que usar o **StreamSocket** entre o cliente e o servidor de rede será criptografada.

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
Quando o código usa [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922), primeiro ele estabelece uma conexão com um serviço de rede sem criptografia. O aplicativo pode enviar ou receber alguns dados e, em seguida, atualizar a conexão para usar SSL/TLS em todas as demais comunicações.

O método [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) aceita dois parâmetros. O parâmetro *protectionLevel* indica o nível de proteção desejado. O parâmetro *validationHostName* é o nome do host do destino de rede remoto usado para validação durante a atualização para SSL. Normalmente *validationHostName* será o mesmo nome de host que o aplicativo usou para estabelecer inicialmente a conexão. Se o parâmetro *protectionLevel* é definido como **Windows.System.Socket.SocketProtectionLevel.Ssl** ao chamar **UpgradeToSslAsync**, o [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) deve usar SSL/TLS para criptografia em comunicações posteriores pelo soquete. Esse valor exige criptografia e jamais permite o uso de uma criptografia NULL.

A sequência normal a ser usada com o método [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) é a seguinte:

-   Crie um [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Se for necessária uma opção avançada no soquete, use a propriedade [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) para obter a instância [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) associada a um objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Defina uma propriedade no **StreamSocketControl**.
-   Se for necessário enviar e receber dados não criptografados, faça isso neste momento.
-   Chame o método [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) para iniciar uma operação para atualizar a conexão para usar SSL/TLS.
-   A força da SSL efetivamente negociada usando [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) pode ser determinada obtendo-se a propriedade [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) depois que a operação assíncrona for concluída com êxito.

O exemplo a seguir cria um [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), tenta estabelecer uma conexão com o serviço de rede, envia dados iniciais e negocia o uso de SSL/TLS. Se a negociação for bem-sucedida, toda a comunicação de rede que usar o **StreamSocket** entre o cliente e o servidor de rede será criptografada.

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
Da mesma forma que as conexões de soquete tradicionais, as conexões WebSocket também podem ser criptografadas com os protocolos TLS/SSL quando usarem os recursos [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) e [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) para um aplicativo UWP. Na maioria dos casos, você vai usar uma conexão WebSocket segura. Isso aumentará as chances de sucesso da sua conexão, já que muitos proxies rejeitam conexões WebSocket não criptografadas.

Para obter exemplos de como criar ou atualizar uma conexão de soquete segura a um serviço de rede, consulte [Como proteger conexões WebSocket com TLS/SSL (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399).

Além da criptografia TLS/SSL, um servidor por exigir um valor de cabeçalho **Sec-WebSocket-Protocol** para concluir o handshake inicial. Esse valor, representado pelas propriedades [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514) e [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358), indica a versão do protocolo da conexão e habilita o servidor a interpretar corretamente o handshake de abertura e os dados trocados posteriormente. Usando essas informações de protocolo, se, a qualquer momento, o servidor não puder interpretar os dados de entrada de maneira segura, a conexão poderá ser fechada.

Caso a solicitação inicial do cliente não contenha esse valor ou forneça um valor que não corresponde ao esperado pelo servidor, o valor esperado será enviado do servidor para o cliente no erro de handshake WebSocket.

## <a name="authentication"></a>Autenticação
Como fornecer as credenciais de autenticação ao conectar-se pela rede.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Fornecendo um certificado de cliente com a classe StreamSocket
A classe [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) dá suporte ao uso de SSL/TLS para autenticar o servidor o qual o aplicativo se comunica. Em certos casos, o aplicativo também precisa autenticar-se ao servidor usando um certificado de cliente TLS. No Windows 10, você pode fornecer um certificado de cliente no objeto [**StreamSocket. Control**](https://msdn.microsoft.com/library/windows/apps/br226893) (isso deve ser definido antes do handshake TLS ser iniciado). Se o servidor solicitar o certificado cliente, o Windows responderá com o certificado fornecido.

Veja a seguir um trecho de código que mostra como implementar isso:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>Fornecendo a autenticação de credenciais para um servidor Web
As APIs de rede que habilitam os aplicativos a interagir com serviços Web seguros fornecem seus próprios métodos para inicializar um cliente ou definir um cabeçalho de solicitação com credenciais de autenticação de servidor e proxy. Cada método é definido com um objeto [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061), o qual indica um nome de usuário, uma senha e o recurso para o qual as credenciais são usadas. A tabela a seguir fornece um mapeamento dessas APIs:

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
| **Transferência em segundo plano** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
| **Sindicalização** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |

## <a name="handling-network-exceptions"></a>Manipulando exceções de rede
Na maioria das áreas de programação, uma exceção indica um problema ou uma falha grave, causada por algum defeito no programa. Na programação de rede, há uma fonte adicional para exceções: a rede em si e a natureza das comunicações de rede. As comunicações de rede são inerentemente não confiáveis e propensas a falhas inesperadas. Para cada uma das maneiras de que seu aplicativo usa a rede, você deverá manter algumas informações de estado; e o código do aplicativo deve tratar exceções de rede atualizando essas informações de estado e inicializando a lógica apropriada para o seu aplicativo restabelecer ou repetir falhas de comunicação.

Quando um aplicativo Universal do Windows lança uma exceção, seu manipulador de exceção pode recuperar informações mais detalhadas sobre a causa da exceção para melhor compreender a falha e tomar decisões apropriadas.

Cada projeção de linguagem dá suporte a um método para acessar essas informações mais detalhadas. Uma exceção é projetada como um valor **HRESULT** em aplicativos Universais do Windows. O arquivo de inclusão *Winerror.h* contém uma lista muito grande de valores **HRESULT** possíveis, incluindo erros de rede.

As APIs de rede dão suporte a métodos diferentes para recuperar essas informações detalhadas sobre a causa de uma exceção.

-   Algumas APIs fornecem um método auxiliar que converte o valor **HRESULT** da exceção em um valor de enumeração.
-   Outras APIs fornecem um método para recuperar efetivamente o valor **HRESULT**.

## <a name="related-topics"></a>Tópicos relacionados
* [Melhorias de API de rede no Windows 10](http://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)

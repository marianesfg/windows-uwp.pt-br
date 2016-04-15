---
description: Os WebSockets fornecem um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web usando HTTP(S).
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
---

# WebSockets

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

Os WebSockets fornecem um mecanismo para comunicações bidirecionais rápidas e seguras entre um cliente e um servidor na Web usando HTTP(S).

De acordo com o [Protocolo WebSocket](http://tools.ietf.org/html/rfc6455), os dados são transferidos imediatamente através de uma única conexão de soquete full-duplex, permitindo que as mensagens sejam enviadas e recebidas de ambos os pontos de extremidade em tempo real. WebSockets são ideais para uso em jogos em tempo real nos quais notificações instantâneas de redes sociais e exibições atualizadas de informações (estatísticas de jogo) precisam ser seguras e usar transferências de dados rápidas. Os desenvolvedores da Plataforma Universal do Windows (UWP) podem usar as classes [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) e [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) para se conectar com servidores que dão suporte ao protocolo Websocket.

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Adequado para cenários típicos onde as mensagens não são muito grandes.   | Adequado para cenários nos quais arquivos grandes (como fotos ou vídeos) são transferidos. |
| Habilita a notificação de que uma mensagem WebSocket completa foi recebida. | Permite que seções de uma mensagem sejam lidas com cada operação de leitura.                             |
| Suporta tanto mensagens UTF-8 quanto binárias.                                 | Suporta apenas mensagens binárias.                                                                |
| Semelhante a um soquete de datagrama ou UDP.                                     | Semelhante a um soquete de fluxo ou TCP.                                                            |

Na maioria dos casos, você usará uma conexão WebSocket segura para criptografar dados enviados e recebidos. Isso também aumentará as chances de sucesso da conexão, já que muitos proxies rejeitam conexões WebSocket não criptografadas. O protocolo WebSocket define estes dois esquemas de URI.

-   ws: - usado para conexões não criptografadas.
-   wss: - usado para conexões seguras que devem ser criptografadas.

Para criptografar uma conexão WebSocket, use o esquema de URI wss:, por exemplo, `wss://www.contoso.com/mywebservice`.

## Usando MessageWebSocket

O [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) allows sections of a message to be read with each read operation. Um **MessageWebSocket** normalmente é usado em cenários nos quais as mensagens não são muito grandes. Existe suporte tanto para arquivos UTF-8 quanto para arquivos binários.

O código nesta seção cria um novo [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), conecta-se a um servidor WebSocket e envia dados para o servidor. Quando uma conexão é estabelecida com êxito, o aplicativo aguarda até que o evento [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) seja disparado, indicando que os dados foram recebidos.

Este exemplo usa o servidor de eco WebSocket.org, um serviço que simplesmente exibe volta para o remetente qualquer cadeia de caracteres enviada a ele. Usando o especificador de protocolo "wss:", este exemplo usa uma conexão segura para enviar e receber mensagens.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

Depois que você inicializa a conexão WebSocket, o código deve realizar as seguintes atividades para enviar e receber dados corretamente.

### Implementar um retorno de chamada para o evento MessageWebSocket.MessageReceived

Antes de estabelecer uma conexão e enviar dados com um WebSocket, seu aplicativo precisa registrar um retorno de chamada do evento para receber notificação quando dados forem recebidos. Quando o evento [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) ocorre, o retorno de chamada registrado é chamado e recebe dados de [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852). Este exemplo foi escrito partindo do pressuposto de que as mensagens enviadas estão no formato UTF-8.

A função de exemplo a seguir recebe uma cadeia de caracteres de um servidor WebSocket conectado e imprime a cadeia de caracteres na janela de saída do depurador.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  Implementar um retorno de chamada para o evento MessageWebSocket.Closed

Antes de estabelecer uma conexão e enviar dados com um WebSocket, seu aplicativo precisa registrar um retorno de chamada do evento para receber notificação quando o WebSocket for fechado pelo servidor WebSocket. Quando o [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364) evento ocorre, o retorno de chamada registrado é chamado para indicar que a conexão foi fechada pelo servidor WebSocket.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  Enviar uma mensagem em um WebSocket

Quando uma conexão é estabelecida, o cliente de WebSocket pode enviar dados ao servidor. O método [**DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) retorna um parâmetro que é mapeado para um int não assinado. Isso altera como definimos a tarefa para enviar a mensagem em comparação com a tarefa para estabelecer a conexão.

**Observação** quando você cria um novo objeto DataWriter usando OutputStream do MessageWebSocket, a DataWriter se apropria do OutputStream e desalocará o Outputstream quando o DataWriter sair do escopo. Isso faz com que tentativas subsequentes de usar o OutputStream falhem com um valor HRESULT de 0x80000013. Para evitar a desalocação do OutputStream, esse código chama o método DetachStream do DataWriter, que devolve a propriedade do fluxo para o objeto WebSocket.

A função a seguir envia a cadeia de caracteres fornecida a um WebSocket conectado e imprime uma mensagem de verificação na janela de saída do depurador.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## Usando controles avançados com WebSockets

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) e [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) seguem o mesmo modelo para o uso de controles avançados. Cada uma das classes primárias acima corresponde a classes relacionadas de acesso a controles avançados.

[**MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843) fornece dados de controle de soquete sobre um objeto [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).
[**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) fornece dados de controle de soquete sobre um objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
O modelo básico para usar controles avançados é o mesmo para os dois tipos de WebSockets. O assunto abordado a seguir usa [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) como exemplo, mas o mesmo processo pode ser usado com [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).

1.  Crie o objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
2.  Use a propriedade [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) para recuperar a instância [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) associada ao objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
3.  Obtenha ou defina propriedades na instância [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) para obter ou definir controles avançados específicos.

[
            **StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) e [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) impõem requisitos sobre quando os controles avançados podem ser definidos.

-   Para todos os controles avançados em [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), o aplicativo deve antes de definir a propriedade antes de emitir uma operação Connect. Por causa desse requisito, é recomendável definir todas as propriedades de controle imediatamente após criar o objeto **StreamWebSocket**. Não tente definir uma propriedade de controle depois que o método [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933) tiver sido chamado.
-   Para todos os controles avançados no [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), exceto o tipo de mensagem, você deve definir a propriedade antes de emitir uma operação Connect. É recomendável definir todas as propriedades de controle imediatamente após criar o **MessageWebSocket**. Exceto para o tipo de mensagem, não tente alterar uma propriedade de controle depois que [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859) tiver sido chamado.

## Classes de informações de WebSocket

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) e [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) têm uma classe correspondente que fornece informações adicionais sobre uma instância de WebSocket.

[**MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849) fornece informações sobre um [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842). Recupere uma instância da classe de informações usando a propriedade [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861).

[**StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929) fornece informações sobre um [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923). Recupere uma instância da classe de informações usando a propriedade [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935).

Observe que todas as propriedades nas duas classes de informações são somente leitura, e que você pode recuperar informações atuais a qualquer momento durante o tempo de vida de um objeto WebSocket.

## Manipulando exceções de rede

Um erro encontrado em uma operação [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) é retornado como um valor **HRESULT**. O método [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) é usado para converter um erro de rede de uma operação WebSocket em um valor de enumeração [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). A maioria dos valores de enumeração **WebErrorStatus** corresponde a um erro retornado pela operação nativa de cliente HTTP. Seu aplicativo pode filtrar por um valor específico de enumeração **WebErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, um aplicativo também pode usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro causador da exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**.

## Definindo tempos limites em operações de WebSocket

As classes MessageWebSocket e StreamWebSocket usam um serviço de sistema interno para enviar solicitações de cliente do WebSocket e receber respostas de um servidor. O valor de tempo limite padrão usado para uma operação de conexão de WebSocket é 60 segundos. Se o servidor HTTP com suporte a WebSockets ficar temporariamente inativo ou for bloqueado por uma interrupção de rede e o servidor não conseguir responder à solicitação de conexão de WebSocket, o serviço de sistema interno aguardará os 60 segundos padrão antes de retornar um erro, o que faz com que seja gerada uma exceção no método ConnectAsync do WebSocket. Se a consulta de nome de um servidor HTTP na URI retornar vários endereços IP para o nome, o serviço de sistema interno tentará até cinco endereços IP para o site, cada um com um tempo limite padrão de 60 segundos, antes de falhar. Um aplicativo que estiver fazendo uma solicitação de conexão de WebSocket poderá aguardar vários minutos fazendo tentativas de conectar a vários endereços IP antes que um erro seja retornado e uma exceção seja gerada. Esse comportamento pode dar ao usuário a impressão de que o aplicativo parou de funcionar. O tempo limite padrão usado para operações de envio e recebimento após o estabelecimento de uma conexão WebSocket é 30 segundos.

Para melhorar a resposta do aplicativo e minimizar esses problemas, o aplicativo pode definir um tempo limite mais curto para solicitações de conexão, para que a operação falhe antes com o tempo limite em vez de fazê-lo com as configurações padrão.

Defina tempos limite da mesma forma para StreamWebSockets e MessageWebSockets. O exemplo a seguir mostra como definir um tempo limite em StreamWebSocket, mas o processo é semelhante para MessageWebSocket.

1.  Crie uma tarefa que seja concluída após um atraso especificado usando um temporizador.
2.  Crie uma tarefa para a operação WebSocket com um cancellation\_token\_source para dar suporte ao cancelamento.
3.  Se a tarefa com o atraso de tempo limite especificado for concluída antes da operação de conexão do WebSocket, cancele a tarefa para a operação de WebSocket.

O exemplo a seguir cria uma tarefa que é concluída após um atraso especificado e cria uma segunda tarefa que é cancelada após um atraso especificado. Essas classes podem ser usadas com StreamWebSocket e MessageWebSocket quando for tentada uma conexão para definir um limite de tempo específico. Um exemplo de uso seria chamar o método StreamWebSocket.ConnectAsync em uma tarefa com um cancellation\_token\_source que dá suporte ao cancelamento. Se o tempo limite for concluído primeiro, o cancellation\_token\_source será usado para cancelar a tarefa para operação de conexão do WebSocket.

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```



<!--HONumber=Mar16_HO3-->



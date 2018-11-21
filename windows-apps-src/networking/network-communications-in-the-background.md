---
author: stevewhims
description: Para continuar a comunicação de rede quando não estiver em segundo plano, um aplicativo pode usar tarefas em segundo plano e qualquer agente de soquete ou gatilhos de canal de controle.
title: Comunicações de rede em segundo plano
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.author: stwhi
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34fad804bb36ad1b4ce92a56772c33318e10faa8
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7424214"
---
# <a name="network-communications-in-the-background"></a>Comunicações de rede em segundo plano
Para continuar a comunicação de rede quando não estiver em primeiro plano, seu aplicativo pode usar tarefas em segundo plano e uma destas duas opções.
- Agente de soquete. Se seu aplicativo usa soquetes para conexões de longo prazo em seguida, quando ele sai do primeiro plano, ele pode delegar a propriedade de um soquete para um agente de soquete do sistema. Em seguida, o agente: ativa seu aplicativo quando chega tráfego no soquete; transfere a propriedade de volta para seu aplicativo; e seu aplicativo processa o tráfego de chegada.
- Gatilhos de canal de controle. 

## <a name="performing-network-operations-in-background-tasks"></a>Realizando operações de rede em tarefas em segundo plano
- Use um [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger) para ativar a tarefa em segundo plano quando um pacote é recebido e você precisa executar uma tarefa de curta duração. Depois de executar a tarefa, deve encerrar a tarefa em segundo plano para economizar energia.
- Use um [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) para ativar a tarefa em segundo plano quando um pacote é recebido e você precisa executar uma tarefa de longa duração.

**Condições relacionadas à rede e sinalizadores**

- Adicione a condição **InternetAvailable** à sua tarefa em segundo plano [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para atrasar o disparo da tarefa em segundo plano até que a pilha de rede esteja em execução. Essa condição economiza energia, pois a tarefa em segundo plano não é executada até a rede funcionar. Essa condição não fornece ativação em tempo real.

Independentemente do gatilho usado, defina [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder) na tarefa em segundo plano para garantir que a rede permaneça ativa enquanto a tarefa em segundo plano é executada. Isso solicita que a infraestrutura de tarefa em segundo plano acompanhe a rede enquanto a tarefa está em execução, mesmo se o dispositivo entrar em Modo de espera conectado. Se sua tarefa em segundo plano não usar **IsNetworkRequested**, sua tarefa em segundo plano não será capaz de acessar a rede quando estiver em modo de espera conectado (por exemplo, quando a tela do telefone está desativada).

## <a name="socket-broker-and-the-socketactivitytrigger"></a>Agente de soquete e SocketActivityTrigger
Se o aplicativo usa conexões [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906), você deve usar [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) e o agente de soquete para ser notificado quando chegar tráfego para seu aplicativo enquanto ele não está no primeiro plano.

Para que seu aplicativo receba e processe os dados recebidos em um soquete quando o aplicativo não está ativo, o aplicativo deve executar uma configuração única na inicialização e, em seguida, transferir a propriedade do soquete para o agente de soquete quando ele estiver fazendo a transição para um estado não ativo.

As etapas de configuração únicas se destinam a criar um gatilho, registrar uma tarefa em segundo plano para o gatilho e habilitar o soquete para o agente de soquete:
  - Crie um **SocketActivityTrigger** e registre uma tarefa em segundo plano para o gatilho com o parâmetro TaskEntryPoint definido como seu código para processar um pacote recebido.
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - Chame **EnableTransferOwnership** no soquete antes de associar o soquete.
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

Depois de configurar corretamente o soquete, quando o aplicativo estiver prestes a ser suspenso, chame **TransferOwnership** no soquete para transferi-lo para um agente de soquete. O agente monitora o soquete e ativa a tarefa em segundo plano quando são recebidos dados. O exemplo a seguir inclui uma função utilitária **TransferOwnership** para realizar a transferência para soquetes **StreamSocketListener**. (Observe que tipos diferentes de soquetes têm seu próprio método **TransferOwnership**, sendo assim, você deve chamar o método apropriado para o soquete cuja propriedade está transferindo. O código provavelmente conterá um auxiliar sobrecarregado **TransferOwnership** com uma implementação para cada tipo de soquete que você usa, de forma que o código **OnSuspending** continua sendo de leitura fácil.)

Um aplicativo transfere a propriedade de um soquete para um agente de soquete e passa a ID da tarefa em segundo plano usando um dos métodos apropriados a seguir:
-   Um dos métodos [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804256) em um [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319).
-   Um dos métodos [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn781433) em um [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Um dos métodos [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804407) em um [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906).

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
No manipulador de eventos da sua tarefa em segundo plano:
   -  Primeiramente, obtenha um adiamento da tarefa em segundo plano para que você possa manipular o evento usando métodos assíncronos.
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  Em seguida, extraia SocketActivityTriggerDetails dos argumentos do evento e descubra o motivo pelo qual o evento foi gerado:
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   Se o evento foi gerado por atividade de soquete, crie um DataReader no soquete, carregue o leitor de forma assíncrona e, em seguida, use os dados de acordo com o design do seu aplicativo. Observe que você deve retornar a propriedade do soquete para o agente de soquete, para ser notificado de outras atividades do soquete novamente.

   No exemplo a seguir, o texto recebido no soquete é exibido em uma notificação do sistema.

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Se o evento foi gerado porque um temporizador de keep alive expirou, seu código deverá enviar alguns dados pelo soquete para manter o soquete ativo e reiniciar o temporizador de keep alive. Mais uma vez, é importante retornar a propriedade do soquete de volta para o agente de soquete para receber notificações de eventos adicionais:

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Se o evento foi gerado porque o soquete foi fechado, restabeleça o soquete, certificando-se de que, depois de criar o novo soquete, você transfira a propriedade para o agente de soquete. Neste exemplo, o nome do host e a porta são armazenados nas configurações locais para que elas possam ser usadas para estabelecer uma nova conexão de soquete:

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   Não se esqueça de concluir o adiamento depois que terminar de processar a notificação de evento:

```csharp
  deferral.Complete();
```

Para obter uma amostra completa demonstrando o uso de [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) e do agente de soquete, consulte [SocketActivityStreamSocket sample](http://go.microsoft.com/fwlink/p/?LinkId=620606). A inicialização do soquete é realizada em Scenario1\_Connect.xaml.cs, e a implementação da tarefa em segundo plano está em SocketActivityTask.cs.

Você provavelmente perceberá que a amostra chama **TransferOwnership** assim que cria um novo soquete ou adquire um soquete existente, em vez de usar o manipulador de evento **OnSuspending** para fazer isso, como descrito neste tópico. Isso ocorre porque a amostra se concentra em demonstrar [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) e não usa o soquete para outra atividade enquanto está em execução. Seu aplicativo provavelmente será mais complexo e deverá usar **OnSuspending** para determinar quando chamar **TransferOwnership**.

## <a name="control-channel-triggers"></a>Gatilhos de canal de controle
Em primeiro lugar, verifique se você está usando gatilhos de canal de controle (CCTs) adequadamente. Se você estiver usando conexões [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) , [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)ou [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), é recomendável que você use [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009). É possível usar CCTs para **StreamSocket**, mas eles usam mais recursos e podem não funcionar no modo de espera conectado.

Se você estiver usando WebSockets, [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)ou [**HttpClient**](/uwp/api/windows.web.http.httpclient), você deve usar [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

## <a name="controlchanneltrigger-with-websockets"></a>ControlChannelTrigger com WebSockets
Algumas considerações especiais se aplicam quando usamos [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Há alguns padrões de uso específicos de transporte e práticas recomendadas que devem ser seguidos ao usar um **MessageWebSocket** ou um **StreamWebSocket** com **ControlChannelTrigger**. Além disso, essas considerações afetam o modo como são manipuladas as solicitações para receber pacotes em **StreamWebSocket**. As solicitações para receber pacotes no **MessageWebSocket** não são afetadas.

Os seguintes padrões de uso e práticas recomendadas devem ser seguidos ao usar [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032):

-   Um recebimento de soquete em aberto deve ficar postado o tempo todo. Isso é necessário para permitir que as tarefas de notificação de envio por push ocorram.
-   O protocolo WebSocket define um modelo padrão para mensagens keep alive. A classe [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) pode enviar mensagens keep alive do protocolo WebSocket iniciadas pelo cliente para o servidor. A classe **WebSocketKeepAlive** deve ser registrada como o TaskEntryPoint para um KeepAliveTrigger pelo aplicativo.

Algumas considerações especiais afetam o modo como são manipuladas as solicitações para receber pacotes em [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923). Em particular, quando usar um **StreamWebSocket** com o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), seu aplicativo deve usar um padrão assíncrono bruto para manipular leituras em vez do modelo **await** em C# e em VB.NET ou Tasks em C++. O padrão assíncrono bruto é mostrado em um exemplo de código posteriormente nesta seção.

O uso do padrão assíncrono bruto permite que o Windows sincronize o método [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) na tarefa em segundo plano para o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) com o retorno do retorno de chamada de conclusão do recebimento. O método **Run** é invocado depois que o retorno de chamada de conclusão é retornado. Isso assegura que o aplicativo recebeu os dados/erros antes de o método **Run** ser invocado.

É importante observar que o aplicativo precisa postar outra leitura antes de retornar o controle do retorno de chamada de conclusão. Também é importante observar que [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) não pode ser usado diretamente com o transporte [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) pois isso interrompe a sincronização descrita acima. Não há suporte para o uso do método [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) diretamente sobre o transporte. Em vez disso, o [**IBuffer**](https://msdn.microsoft.com/library/windows/apps/br241656) retornado pelo método [**IInputStream.ReadAsync**](https://msdn.microsoft.com/library/windows/apps/br241719) na propriedade [**StreamWebSocket.InputStream**](https://msdn.microsoft.com/library/windows/apps/br226936) pode ser passado posteriormente para o método [**DataReader.FromBuffer**](https://msdn.microsoft.com/library/windows/apps/br208133) para processamento adicional.

A amostra a seguir mostra como usar um padrão assíncrono bruto para manipular solicitações em [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

O manipulador de conclusão de leitura com certeza vai disparar antes do método [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) na tarefa em segundo plano para que [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) seja invocado. O Windows tem sincronização interna para aguardar que um aplicativo retorne do retorno de chamada de conclusão de leitura. O aplicativo geralmente processa com rapidez os dados ou o erro de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) no retorno de chamada de conclusão de leitura. A mensagem propriamente dita é processada dentro do contexto do método **IBackgroundTask.Run**. Na amostra abaixo, esse ponto é ilustrado pelo uso de uma fila de mensagens na qual o manipulador de conclusão de leitura insere a mensagem e a qual a tarefa em segundo plano processa depois.

A amostra a seguir mostra o manipulador de conclusão de leitura para uso com um padrão assíncrono bruto para manipular solicitações em [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Um detalhe adicional relativo a Websockets é o manipulador keep alive. O protocolo WebSocket define um modelo padrão para mensagens keep alive.

Quando usar [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), registre uma instância da classe [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) como o [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) de um KeepAliveTrigger para permitir que o aplicativo não seja suspenso e envie periodicamente mensagens keep alive para o servidor (ponto de extremidade remoto). Isso deve ser feito como parte do código do aplicativo de registro em segundo plano bem como no manifesto do pacote.

O ponto de entrada da tarefa de [**Windows.Sockets.WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) precisa ser especificado em dois lugares:

-   Ao criar o gatilho KeepAliveTrigger no código-fonte (consulte o exemplo abaixo).
-   No manifesto do pacote do aplicativo para a declaração de tarefa em segundo plano do keep-alive.

A amostra a seguir adiciona uma notificação de gatilho de rede e um gatilho keep-alive sob o elemento &lt;Application&gt; em um manifesto de aplicativo.

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

Um aplicativo deve usar com muito cuidado uma instrução **await** no contexto de um [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) e uma operação assíncrona em um [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). Um objeto **Task&lt;bool&gt;** pode ser usado para registrar um **ControlChannelTrigger** para notificação por push e keep alives WebSocket no **StreamWebSocket** e para conectar o transporte. Como parte do registro, o transporte **StreamWebSocket** é definido como o transporte do **ControlChannelTrigger** e é postada uma leitura. O **Task.Result** bloqueará o thread atual até que todas as etapas da tarefa sejam executadas e retornem instruções no corpo da mensagem. A tarefa não é resolvida até que o método retorne true ou false. Isso garante que todo o método seja executado. A **Task** pode conter várias instruções **await** que são protegidas pela **Task**. Esse padrão deve ser usado com o objeto **ControlChannelTrigger** quando um **StreamWebSocket** ou um **MessageWebSocket** é usado como transporte. Para operações que possam demorar muito para serem concluídas (por exemplo, uma operação de leitura assíncrona típica), o aplicativo deve usar o padrão assíncrono bruto discutido anteriormente.

O exemplo a seguir registra [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) para notificações por push e keep alives WebSocket no [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

Para obter mais informações sobre o uso de [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), consulte [ControlChannelTrigger StreamWebSocket sample](http://go.microsoft.com/fwlink/p/?linkid=251232).

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger com HttpClient
Algumas considerações especiais se aplicam quando usamos [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Há alguns padrões de uso específicos de transporte e práticas recomendadas que devem ser seguidos ao usar um [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com **ControlChannelTrigger**. Além disso, essas considerações afetam o modo como são manipuladas as solicitações para receber pacotes em [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637).

**Observação** [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) usando SSL não é atualmente suportado usando o recurso de gatilho de rede e [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).
 
Os seguintes padrões de uso e práticas recomendadas devem ser seguidos ao usar [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032):

-   É possível que o aplicativo precise definir várias propriedades e cabeçalhos no objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) ou [HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638) no namespace [System.Net.Http](http://go.microsoft.com/fwlink/p/?linkid=227894) antes de enviar a solicitação para o URI específico.
-   Um aplicativo talvez precise fazer uma solicitação inicial para testar e configurar o transporte adequadamente antes de criar o transporte [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) para uso com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Depois que o aplicativo determina que o transporte pode ser configurado adequadamente, um objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) pode ser configurado como o objeto de transporte usado com o objeto **ControlChannelTrigger**. Esse processo foi desenvolvido para impedir que alguns cenários interrompam a conexão estabelecida por meio do transporte. Usando SSL com um certificado, um aplicativo pode exigir a exibição de uma caixa de diálogo para entrada do PIN ou caso haja vários certificados disponíveis para escolha. A autenticação de proxy e a autenticação de servidor podem ser necessárias. Se a autenticação de proxy ou de servidor expirar, a conexão poderá ser fechada. Uma maneira pela qual um aplicativo pode lidar com esses problemas de expiração da autenticação é definindo um temporizador. Quando um redirecionamento HTTP é necessário, não é garantido que a segunda conexão possa ser estabelecida de maneira confiável. Uma solicitação de teste inicial garante que o aplicativo pode usar a URL redirecionada mais recente antes de usar o objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) como transporte com o objeto **ControlChannelTrigger**.

Diferentemente de outros transportes de rede, o objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) não pode ser passado diretamente no método [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) do objeto [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Em vez disso, um objeto [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) deve ser construído especialmente para uso com o objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) e o **ControlChannelTrigger**. O objeto [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) é criado usando o método [RtcRequestFactory.Create](http://go.microsoft.com/fwlink/p/?linkid=259154). Em seguida, o objeto [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) criado é passado para o método **UsingTransport**.

A amostra a seguir ilustra como construir um objeto [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153) para uso com o objeto [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) e o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

Algumas considerações especiais afetam o modo como são manipuladas as solicitações para envio de solicitações HTTP no [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) para iniciar o recebimento de uma resposta. Em particular, ao usar um [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), seu aplicativo deve usar uma tarefa para manipular envios em vez do modelo **await**.

Usando [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637), não há sincronização com o método [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) na tarefa em segundo plano para o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) com o retorno do retorno de chamada de conclusão do recebimento. Por esse motivo, o aplicativo pode usar apenas a técnica HttpResponseMessage de bloqueio no método **Run** e aguardar até que toda a resposta seja recebida.

O uso de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) é nitidamente diferente do transporte [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ou [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923). O retorno de chamada de recebimento de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) é entregue ao aplicativo por meio de uma tarefa desde o código [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637). Isso significa que a tarefa de notificação por push **ControlChannelTrigger** será disparada assim que os dados ou o erro for despachado para o aplicativo. Na amostra abaixo, o código armazena a responseTask retornada pelo método [HttpClient.SendAsync](http://go.microsoft.com/fwlink/p/?linkid=241637) no armazenamento global que a tarefa de notificação por push selecionará e processará em linha.

A amostra a seguir ilustra como manipular solicitações de envio no [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) quando usado com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

A amostra a seguir ilustra como ler respostas recebidas no [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) quando usado com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

Para obter mais informações sobre o uso de [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), consulte [ControlChannelTrigger HttpClient sample](http://go.microsoft.com/fwlink/p/?linkid=258323).

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger com IXMLHttpRequest2
Algumas considerações especiais se aplicam quando usamos [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Há alguns padrões de uso específicos de transporte e práticas recomendadas que devem ser seguidos ao usar um **IXMLHTTPRequest2** com **ControlChannelTrigger**. O uso de **ControlChannelTrigger** não afeta o modo como são manipuladas as solicitações para enviar ou receber solicitações HTTP no **IXMLHTTPRequest2**.

Padrões de uso e práticas recomendadas ao usar [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)

-   Um objeto [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), quando usado como transporte, tem uma vida útil de apenas uma solicitação/resposta. Quando usado com o objeto [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), é conveniente criar e configurar o objeto **ControlChannelTrigger** uma vez e depois chamar o método [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175) repetidamente, cada vez associando um novo objeto **IXMLHTTPRequest2**. Um aplicativo deve excluir o objeto **IXMLHTTPRequest2** anterior antes de fornecer um novo objeto **IXMLHTTPRequest2** para assegurar que o aplicativo não exceda os limites de recursos alocados.
-   O aplicativo pode precisar chamar os métodos [**SetProperty**](https://msdn.microsoft.com/library/windows/desktop/hh831167) e [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/desktop/hh831168) para configurar o transporte HTTP antes de chamar o método [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164).
-   Um aplicativo talvez precise fazer uma solicitação inicial para testar e configurar o transporte adequadamente antes de criar o transporte [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164) para uso com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Depois que o aplicativo determina que o transporte está configurado adequadamente, o objeto [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) pode ser configurado como o objeto de transporte usado com o objeto **ControlChannelTrigger**. Esse processo foi desenvolvido para impedir que alguns cenários interrompam a conexão estabelecida por meio do transporte. Usando SSL com um certificado, um aplicativo pode exigir a exibição de uma caixa de diálogo para entrada do PIN ou caso haja vários certificados disponíveis para escolha. A autenticação de proxy e a autenticação de servidor podem ser necessárias. Se a autenticação de proxy ou de servidor expirar, a conexão poderá ser fechada. Uma maneira pela qual um aplicativo pode lidar com esses problemas de expiração da autenticação é definindo um temporizador. Quando um redirecionamento HTTP é necessário, não é garantido que a segunda conexão possa ser estabelecida de maneira confiável. Uma solicitação de teste inicial garante que o aplicativo pode usar a URL redirecionada mais recente antes de usar o objeto **IXMLHTTPRequest2** como transporte com o objeto **ControlChannelTrigger**.

Para obter mais informações sobre o uso de [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) com [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), consulte [ControlChannelTrigger with IXMLHTTPRequest2 sample](http://go.microsoft.com/fwlink/p/?linkid=258538).

## <a name="important-apis"></a>APIs Importantes
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)

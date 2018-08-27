---
author: PatrickFarley
title: Conectar dispositivos por meio de sessões remotas
description: Crie experiências compartilhadas em vários dispositivos ingressando-os em uma sessão remota.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.author: pafarley
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, Roma de projeto
ms.localizationpriority: medium
ms.openlocfilehash: 8e5226b23a454bf48add22d590a3ff247c629e4f
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855544"
---
# <a name="connect-devices-through-remote-sessions"></a>Conectar dispositivos por meio de sessões remotas

O recurso de sessões remotas permite que um aplicativo se conecte a outros dispositivos por meio de uma sessão, seja por mensagem em aplicativo ou por troca agenciada de dados gerenciados do sistema, como o **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)** para compartilhamento holográfico entre dispositivos Windows Holographic.

Sessões remotas podem ser criadas por qualquer dispositivo Windows, e qualquer dispositivo Windows pode solicitar participação (embora as sessões possam ser visíveis apenas por convite), incluindo dispositivos registrados por outros usuários. Este guia fornece código de exemplo básico para todos os principais cenários que fazem uso de sessões remotas. Esse código pode ser incorporado em um projeto de aplicativo existente e modificado conforme necessário. Para obter uma implementação de ponta a ponta, consulte o [Aplicativo de amostra do jogo de perguntas](https://github.com/microsoft/Windows-appsample-remote-system-sessions).

## <a name="preliminary-setup"></a>Configuração preliminar

### <a name="add-the-remotesystem-capability"></a>Adicionar funcionalidade remoteSystem

Para seu app iniciar um app em um dispositivo remoto, você deve adicionar a funcionalidade `remoteSystem` ao manifesto do conjunto de aplicativo. Você pode usar o designer de manifesto de pacote para adicioná-lo selecionando Sistema Remoto na guia **Remote System** em **Funcionalidades**, ou adicionar manualmente a linha a seguir ao arquivo _Package.appxmanifest_ do projeto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Habilitar a descoberta de usuários no dispositivo
Sessões remotas são direcionadas para se conectarem a vários usuários diferentes, para que os dispositivos envolvidos precisarão ter o Compartilhamento entre usuários habilitado. Essa é uma configuração de sistema que pode ser consultada com um método estático na classe **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Para alterar essa configuração, o usuário deve abrir o aplicativo de **Configurações**. No menu **Sistema** > **Experiências compartilhadas** > **Compartilhar entre dispositivos**, há uma caixa de lista suspensa na qual o usuário pode especificar com quais dispositivos o sistema pode ser compartilhado.

![página de configurações de experiências compartilhadas](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Incluir os namespaces necessários
Para usar todos os trechos de código neste guia, você precisará das seguintes instruções `using` nos seus arquivos de classe.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Criar uma sessão remota

Para criar uma instância de sessão remota, você deve começar com um objeto **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**. Use a seguinte estrutura para criar uma nova sessão e manipular solicitações de associação de outros dispositivos.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>Fazer uma sessão remota somente por convite

Se você quiser manter sua sessão remota oculta publicamente, você pode torná-la aparente apenas por convite. Somente os dispositivos que recebem um convite serão capazes de enviar solicitações de associação. 

O procedimento é basicamente o mesmo acima, mas ao construir a instância **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**, você passará em um objeto **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** configurado.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Para enviar um convite, você deve ter uma referência para o sistema remoto de recebimento (adquirido por meio de descoberta normal do sistema remoto). Basta passar essa referência para o método **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** do objeto da sessão. Todos os participantes em uma sessão têm uma referência para a sessão remota (veja a próxima seção), então, qualquer participante pode enviar um convite.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Descobrir e ingressar em uma sessão remota

O processo de descoberta de sessões remotas é manipulado pela classe **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** e é semelhante a descoberta de sistemas remotos individuais.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

Quando uma instância **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** é obtida, ela pode ser usada para emitir uma solicitação de associação para o dispositivo que controla a sessão correspondente. Uma solicitação de associação aceita retornará assincronamente um objeto **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** que contém uma referência para a sessão associada.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

Um dispositivo pode ingressar em várias sessões ao mesmo tempo. Por esse motivo, pode ser desejável separar a funcionalidade do ingresso da interação real com cada sessão. Contanto que uma referência para a instância **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** seja mantida no aplicativo, a comunicação pode ser tentada sobre essa sessão.

## <a name="share-messages-and-data-through-a-remote-session"></a>Compartilhar mensagens e dados por meio de uma sessão remota

### <a name="receive-messages"></a>Receber mensagens

Você pode trocar mensagens e dados com outros dispositivos participantes na sessão usando uma instância **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)**, que representa um canal de comunicação de nível de sessão única. Assim que ele é inicializado, ele começa a escutar mensagens de entrada.

>[!NOTE]
>Mensagens devem ser serializadas e desserializadas das matrizes de bytes após enviar e receber mensagens. Essa funcionalidade está incluída nos exemplos a seguir, mas ela pode ser implementada separadamente para melhor modularidade de código. Veja o [aplicativo de amostra](https://github.com/microsoft/Windows-appsample-remote-system-sessions)) para um exemplo disso.

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>Enviar mensagens

Quando o canal é estabelecido, enviar uma mensagem para todos os participantes da sessão é simples.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

Para enviar uma mensagem apenas a determinados participantes, primeiro você deve iniciar um processo de detecção para adquirir referências para os sistemas remotos participando na sessão. Isso é semelhante ao processo de descoberta de sistemas remotos fora de uma sessão. Use uma instância **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** para localizar dispositivos participantes da sessão.

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

Quando uma lista de referências para os participantes da sessão é obtida, você pode enviar uma mensagem para qualquer conjunto deles.

Para enviar uma mensagem a um único participante (selecionado, de preferência, na tela pelo usuário), basta passar a referência para um método como o seguinte.

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

Para enviar uma mensagem a vários participantes (selecionados, de preferência, na tela pelo usuário), adicione-os a um objeto de lista e passe a lista para um método como o seguinte.

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>Tópicos relacionados
* [Aplicativos e dispositivos conectados (Projeto Rome)](connected-apps-and-devices.md)
* [Referência de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)

---
title: Usar projeções do jogo de bate-papo WinRT 2
description: Saiba como usar o Xbox Live jogo bate-papo 2 com projeções do WinRT para adicionar a comunicação de voz ao seu jogo.
ms.date: 04/11/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, jogo bate-papo 2, jogo bate-papo, comunicação de voz
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639161"
---
# <a name="using-game-chat-2-winrt-projections"></a>Usando o jogo bate-papo 2 (projeções de WinRT)

Isso é um breve passo a passo sobre como usar o jogo de bate-papo 2 C# API. Os desenvolvedores de jogos que desejam acessar o jogo bate-papo 2 por meio de C++ devem ver [2 de bate-papo de jogo usando](using-game-chat-2.md).

## <a name="prerequisites-a-nameprereq"></a>Pré-requisitos <a name="prereq">

Para consumir 2 bate-papo do jogo, você deve incluir a [pacote do nuget Microsoft.Xbox.Services.GameChat2](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/).

Antes de começar a codificar com 2 de bate-papo do jogo, você deve ter configurado AppXManifest do seu aplicativo para declarar a funcionalidade do dispositivo "microfone". Recursos de AppXManifest são descritos mais detalhadamente em suas respectivas seções da documentação plataforma; o trecho a seguir mostra o nó de capacidade do dispositivo "microfone" deve existir sob o nó de pacote e os recursos ou então bate-papo será bloqueado:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>Inicialização <a name="init">

Começar a interagir com a biblioteca instanciando um `GameChat2ChatManager` objeto com o número máximo de usuários simultâneos de bate-papo deve ser adicionados à instância. Para alterar esse valor, o `GameChat2ChatManager` objeto deve ser descartado e recriado com o valor desejado. Você pode ter apenas um `GameChat2ChatManager` instanciado por vez.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

O `GameChat2ChatManager` objeto tem propriedades adicionais e opcionais que podem ser configuradas na qualquer momento. O exemplo de código a seguir pressupõe que as variáveis terá um valor antes de serem usadas para definir as propriedades no `myGameChat2ChatManager` objeto.

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>Configurar usuários <a name="config">

Depois que a instância será inicializada, você deve adicionar os usuários locais para a instância de GC2. Neste exemplo, o usuário A representará um usuário local.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

Em seguida, você deve adicionar os usuários remotos e os identificadores que serão usado para representar os "pontos de extremidade remotos" que cada usuário remoto está em. "Pontos de extremidade" são representados por objetos de propriedade do aplicativo que implementam o `IGameChat2Endpoint` interface. O exemplo a seguir mostra uma implementação de exemplo `MyEndpoint` classe que implementa o `IGameChat2Endpoint`.

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

No exemplo de código a seguir, os usuários B, C e D são usuários remotos que está sendo adicionados. O usuário B em um dispositivo remoto e usuários C e D são em outro dispositivo remoto. Este exemplo supõe que as variáveis serão definidas e que `myGameChat2ChatManager` é uma instância de `GameChat2ChatManager` e se "MyEndpoint" é uma classe que implementa `IGameChat2Endpoint`.

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

Agora você pode configurar a relação de comunicação entre cada usuário remoto e o usuário local. Neste exemplo, suponha que o usuário A e B estão na mesma equipe e a comunicação bidirecional é permitida. `GameChat2CommunicationRelationship.SendAndReceiveAll` é definido para representar a comunicação bidirecional. Defina a relação do usuário para o usuário B com:

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Agora, suponha que os usuários C e D são 'spectators' e devem ter permissão para escutar o usuário A, mas não falar. `GameChat2CommunicationRelationship.ReceiveAll` é definida essa comunicação unidirecional. Defina as relações com:

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

Se, a qualquer momento, há usuários remotos que foram adicionados para o `GameChat2ChatManager` da instância, mas ainda não foi configurado para se comunicar com quaisquer usuários locais - isso okey! Isso pode ser esperado em cenários em que os usuários determinam as equipes ou podem alterar os canais falantes arbitrariamente. 2 de bate-papo jogo armazenará em cache apenas informações (por exemplo, relações de privacidade e reputação) para usuários que foram adicionados à instância, por isso é útil informar ao jogo 2 de bate-papo de todos os usuários possíveis, mesmo se eles não falam para todos os usuários locais em um ponto específico no tempo.

Por fim, suponha que o usuário D saiu do jogo e devem ser removido da instância local do jogo 2 bate-papo. Isso pode ser feito com a chamada a seguir:

```cs
remoteUserD.Remove();
```

O objeto de usuário é invalidado imediatamente quando `Remove()` é chamado. O último estado do usuário é armazenado em cache quando ele é removido. Todos os métodos informativos chamados depois que o objeto de usuário é invalidado refletirá o estado do usuário antes de ser removido. Outros métodos lançará um erro quando chamado.

## <a name="processing-data-frames-a-namedata"></a>Quadros de dados de processamento <a name="data">

2 de bate-papo jogo não tem sua própria camada de transporte; Isso deve ser fornecido pelo aplicativo. Este plug-in é gerenciado por chamadas de regulares e frequentes do aplicativo para o `GameChat2ChatManager.GetDataFrames()`. Esse método é como o jogo de bate-papo 2 fornece dados de saída para o aplicativo. Ele é projetado para operar rapidamente, de modo que pode ser controlado com frequência em um thread dedicado de rede. Isso fornece um local conveniente para recuperar todos os dados na fila sem se preocupar com a imprevisibilidade de tempo de rede ou complexidade de retorno de chamada com multithread.

Quando `GameChat2ChatManager.GetDataFrames()` é chamado, todos os dados em fila são relatados em uma lista de `IGameChat2DataFrame` objetos. Aplicativos devem iterar sobre a lista, inspecione o `TargetEndpointIdentifiers`e usar a camada de rede do aplicativo para entregar os dados para as instâncias de aplicativo remoto apropriado. Neste exemplo, `HandleOutgoingDataFrame` é uma função que envia os dados no `Buffer` para cada usuário em cada "ponto de extremidade" especificado na `TargetEndpointIdentifiers` acordo com o `TransportRequirement`.

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

Quanto mais frequentemente os quadros de dados são processados, menor será a latência de áudio aparente para o usuário final. O áudio é Unido em quadros de dados de 40 ms; Esse é o período de sondagem sugeridas.

## <a name="processing-state-changes-a-namestate"></a>Alterações de estado de processamento <a name="state">

2 de bate-papo jogo fornece atualizações para o aplicativo, como mensagens de texto recebidas, por meio de chamadas de regulares e frequentes do aplicativo para o `GameChat2ChatManager.GetStateChanges()` método. Ele foi projetado para operar rapidamente, de modo que ele possa ser chamado todos os quadros de gráficos em seu loop de renderização da interface do usuário. Isso fornece um local conveniente para recuperar todas as alterações na fila sem se preocupar com a imprevisibilidade de tempo de rede ou complexidade de retorno de chamada com multithread.

Quando `GameChat2ChatManager.GetStateChanges()` é chamado, todas as atualizações em fila são relatadas em uma lista de `IGameChat2StateChange` objetos. Aplicativos devem:
1. Iterar através da lista
2. Inspecionar a estrutura de base para seu tipo mais específico
3. Converter a estrutura de base para os respectivos mais detalhadas de tipo
4. Lidar com essa atualização conforme apropriado. 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>Bate-papo do texto <a name="text">

Para enviar o bate-papo de texto, use `GameChat2ChatUserLocal.SendChatText()`. Por exemplo:

```cs
localUserA.SendChatText("Hello");
```

2 de bate-papo jogo irá gerar um quadro de dados que contém esta mensagem; os pontos de extremidade de destino para o quadro de dados serão aquelas associadas com usuários que foram configurados para receber o texto do usuário local. Quando os dados são processados pelos pontos de extremidade remotos, a mensagem será exposta por meio de um `GameChat2TextChatReceivedStateChange`. Assim como acontece com bate-papo, restrições de privacidade e privilégio serão respeitadas para bate-papo do texto. Se um par de usuários foram configurados para permitir bate-papo de texto, mas as restrições de privilégio ou de privacidade não permitir que a comunicação, a mensagem de texto será removida.

Suporte a entrada de bate-papo de texto e a exibição é necessária para acessibilidade (consulte [acessibilidade](#access) para obter mais detalhes).

## <a name="accessibility-a-nameaccess"></a>Acessibilidade <a name="access">

Suporte de bate-papo de texto de entrada e a exibição é necessária. Entrada de texto é necessária porque, mesmo em plataformas ou gêneros de jogos que ainda não tiveram generalizado físico do teclado usar, os usuários podem configurar o sistema para usar tecnologias assistenciais texto em fala. Da mesma forma, a exibição de texto é necessária porque os usuários podem configurar o sistema para usar a fala em texto. Essas preferências podem ser detectadas em usuários locais, verificando o `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` e `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` propriedades respectivamente, e talvez você queira habilitar condicionalmente os mecanismos de texto.

### <a name="text-to-speech"></a>Conversão de texto em fala

Quando um usuário tiver habilitado, de texto em fala `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` será ser 'true'. Quando esse estado é detectado, o aplicativo deve fornecer um método de entrada de texto. Depois que a entrada de texto fornecida por um teclado real ou virtual, passe a cadeia de caracteres para o `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` método. 2 de bate-papo jogo detectará e sintetizador de dados de áudio com base na cadeia de caracteres e a preferência do usuário voice acessível. Por exemplo:

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

O áudio sintetizado como parte dessa operação será transportado para todos os usuários que foram configurados para receber o áudio do usuário local. Se `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` é chamado em um usuário que não tem texto em fala será habilitado do jogo bate-papo 2 não executar nenhuma ação.

### <a name="speech-to-text"></a>Conversão de fala em texto

Quando um usuário tiver habilitada, conversão de fala para texto `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` será true. Quando esse estado é detectado, o aplicativo deve estar preparado para fornecer a que interface do usuário associado com mensagens de bate-papo transcrita. GC automaticamente transcrição de áudio de cada usuário remoto e expô-lo por meio de um `GameChat2TranscribedChatReceivedStateChange`.

### <a name="speech-to-text-performance-considerations"></a>Considerações sobre desempenho de fala em texto

Quando a fala em texto é habilitado, a instância de 2 de bate-papo do jogo em cada dispositivo remoto inicia uma conexão de soquete da web com o ponto de extremidade de serviços de fala. Cada cliente remoto do jogo bate-papo 2 carrega áudio para o ponto de extremidade de serviços de fala por meio de neste websocket; o ponto de extremidade de serviços de fala, ocasionalmente, retornará uma mensagem de transcrição ao dispositivo remoto. O dispositivo remoto, em seguida, envia a mensagem de transcrição (ou seja, uma mensagem de texto) para o dispositivo local, em que a mensagem transcrita é determinada por 2 de bate-papo de jogo para o aplicativo para processar.

Portanto, o custo de desempenho principal de fala em texto é o uso de rede. A maioria do tráfego de rede é o carregamento de áudio codificado. O websocket carrega áudio que já foi codificado por 2 de bate-papo do jogo no caminho de bate-papo de voz "normal"; o aplicativo tem controle sobre a taxa de bits por meio de `GameChat2ChatManager.AudioEncodingTypeAndBitrate`.

## <a name="ui-a-nameui"></a>UI <a name="UI">

É recomendável que em qualquer lugar os jogadores são mostrados, especialmente em uma lista de gamertags como um placar também exibem ícones mudo/falando como comentários do usuário. O `IGameChat2ChatUser.ChatIndicator` propriedade representa o status atual, o instantâneo de bate-papo para esse jogador. O exemplo a seguir demonstra o recuperando o valor do indicador para um `IGameChat2ChatUser` objeto apontado pela variável de userA para determinar um valor constante do ícone específico para atribuir a uma variável 'iconToShow':

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

O valor de `IGameChat2ChatUser.ChatIndicator` deve ser alterado com frequência como players de início e parada falando, por exemplo. Ele é projetado para dar suporte a aplicativos sondando cada quadro de interface do usuário como resultado.

## <a name="muting-a-namemute"></a>Um silenciament <a name="mute">

O `GameChat2ChatUserLocal.MicrophoneMuted` propriedade pode ser usada para alternar o estado ativar mudo do microfone do usuário local. Quando o microfone estiver mudo, sem áudio do que microfone será capturado. Se o usuário estiver em um dispositivo compartilhado, como Kinect, o estado sem som se aplica a todos os usuários. Esse método não reflete um controle Ativar Mudo do hardware (por exemplo, um botão no fone de ouvido do usuário). Não há nenhum método para recuperar o estado de sem som do hardware de dispositivo de áudio de um usuário por meio de 2 de bate-papo do jogo.

O `GameChat2ChatUserLocal.SetRemoteUserMuted()` método pode ser usado para alternar o estado de sem som de um usuário remoto em relação a um determinado usuário local. Quando o usuário remoto estiver mudo, o usuário local não ouvir o áudio ou receber qualquer mensagem de texto do usuário remoto.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>Reputação ruim mudo automático <a name="automute">

Normalmente os usuários remotos começará unmuted. 2 de bate-papo jogo iniciará os usuários em um estado mudo quando (1) o usuário remoto não amigos com o usuário local e (2) o usuário remoto tem um sinalizador de Reputação ruim. Quando os usuários são mudo devido a essa operação `IGameChat2ChatUser.ChatIndicator` retornará `GameChat2UserChatIndicator.ReputationRestricted`. Esse estado será substituído pela primeira chamada para `GameChat2ChatUserLocal.SetRemoteUserMuted()` que inclui o usuário remoto como o usuário de destino.

## <a name="privilege-and-privacy-a-namepriv"></a>Privilégio e privacidade <a name="priv">

Sobre a relação de comunicação configurada pelo jogo, o jogo 2 bate-papo impõe restrições de privilégio e privacidade. 2 de bate-papo jogo executa pesquisas de restrição de privilégio e privacidade quando um usuário é adicionado pela primeira vez; o usuário `IGameChat2ChatUser.ChatIndicator` sempre retornará `GameChat2UserChatIndicator.Silent` até que conclua essas operações. Se a comunicação com um usuário é afetada por um privilégio ou privacidade restrição, o usuário `IGameChat2ChatUser.ChatIndicator` retornará `GameChat2UserChatIndicator.PlatformRestricted`. Restrições de comunicação de plataforma se aplicam a voz e texto bate-papo; nunca haverá uma instância em que bate-papo do texto está bloqueado por uma restrição de plataforma, mas bate-papo é não, ou vice-versa.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` pode ser usado para ajudar a distinguir quando os usuários não podem se comunicar devido ao privilégio incompleto e operações de privacidade. Ele retorna a relação de comunicação imposta por 2 bate-papo do jogo na forma de `GameChat2CommunicationRelationship` e o motivo pelo qual a relação não pode ser igual à relação configurada na forma de `GameChat2CommunicationRelationshipAdjuster`. Por exemplo, se as operações de pesquisa ainda estão em andamento, o `GameChat2CommunicationRelationshipAdjuster` será `GameChat2CommunicationRelationshipAdjuster.Initializing`. Esse método deve ser usado em cenários de desenvolvimento e depuração; não deve ser usado para influenciar a interface do usuário (consulte [interface do usuário](#UI)).

## <a name="cleanup-a-namecleanup"></a>Limpeza <a name="cleanup">

Quando o aplicativo não precisa mais comunicações via 2 bate-papo do jogo, você deverá chamar `GameChat2ChatManager.Dispose()`. Isso permite que o jogo de bate-papo 2 recuperar os recursos que foram alocados para gerenciar as comunicações.

## <a name="how-to-configure-popular-scenarios"></a>Como configurar cenários populares

### <a name="push-to-talk"></a>Envio por push a palestra

Envio por push a conversa deve ser implementada com `GameChat2ChatUserLocal.MicrophoneMuted`. Definir `MicrophoneMuted` como falso para permitir que a conversão de fala e `MicrophoneMuted` como true para restringi-lo. Essa alteração de propriedade fornecerá a resposta de latência mais baixa de 2 de bate-papo do jogo.

### <a name="teams"></a>Equipes

Suponha que o usuário A e B estão na equipe azul, e o usuário C e D de usuário estão na equipe vermelha. Cada usuário está em uma instância exclusiva do aplicativo.

No dispositivo do usuário:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

No dispositivo do usuário B:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

No dispositivo do usuário C:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

No dispositivo do usuário D:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>Transmissão

Suponha que o usuário A é a líder dando pedidos e usuários B, C e D só podem escutar. Cada jogador está em um dispositivo exclusivo.

No dispositivo do usuário:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

No dispositivo do usuário B:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

No dispositivo do usuário C:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

No dispositivo do usuário D:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```

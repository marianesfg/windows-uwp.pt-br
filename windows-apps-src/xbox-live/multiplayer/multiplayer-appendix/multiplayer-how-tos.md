---
title: Instruções para múltiplos jogadores
description: Descreve como implementar tarefas comuns no Xbox Live Multiplayer 2015.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, com vários participantes de 2015
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648631"
---
# <a name="multiplayer-how-tos"></a>Explicações do sistema Multijogador

Este tópico contém informações sobre como implementar tarefas específicas relacionadas ao uso de vários jogadores 2015.

* Assine para receber notificações de alteração de sessão MPSD
* Criar uma sessão MPSD
* Definir um arbitrador para uma sessão MPSD
* Gerenciar a ativação do título
* Fazer com que o usuário permite junções
* Enviar convites para jogos
* Ingressar em uma sessão de jogo de uma sessão do corredor
* Ingressar em uma sessão MPSD a ativação de um título
* Defina a atividade do usuário atual
* Atualizar uma sessão MPSD
* Deixar uma sessão MPSD
* Preencher os slots de sessão aberta durante a compatibilidade
* Crie um tíquete de correspondência
* Obter o status da correspondência de tíquete

## <a name="subscribe-for-mpsd-session-change-notifications"></a>Assine para receber notificações de alteração de sessão MPSD

| Observação                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inscrever-se para que as alterações a uma sessão requer o player associado esteja ativa na sessão. O campo connectionRequiredForActiveMembers também deve ser definido como true no objeto /constants/system/capabilities para a sessão. Geralmente, esse campo é definido no modelo de sessão. Ver [modelos de sessão MPSD](multiplayer-session-directory.md). |



Para receber notificações de alteração de sessão MPSD, o título pode seguir o procedimento a seguir.

1.  Use o mesmo **XboxLiveContext classe** objeto para todas as chamadas pelo mesmo usuário. As assinaturas estão vinculadas ao tempo de vida deste objeto. Se houver vários usuários locais, use um separado **XboxLiveContext** objeto para cada usuário.
2.  Implementar manipuladores de eventos para o **evento RealTimeActivityService.MultiplayerSessionChanged** e o **RealTimeActivityService.MultiplayerSubscriptionsLost evento**.
3.  Se assinar as alterações para mais de um usuário, adicione código ao seu **MultiplayerSessionChanged** manipulador de eventos para evitar o trabalho desnecessário. Use o **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** e o **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriedade**. Uso dessas propriedades permite o acompanhamento da última alteração visto e ignorando das alterações mais antigas.
4.  Chame o **MultiplayerService.EnableMultiplayerSubscriptions método** para permitir que as assinaturas.
5.  Crie um objeto de sessão local e a associação naquela sessão como o Active Directory.
6.  Fazer chamadas para cada usuário para o **MultiplayerSession.SetSessionChangeSubscription método**, passando a sessão de alterar o tipo para o qual ser notificado.
7.  Agora gravar a sessão MPSD conforme descrito em **como: Atualizar uma sessão para múltiplos jogadores**.

O fluxograma a seguir ilustra como iniciar Multplayer Assinando eventos descritos neste procedimento.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>Analisando as notificações de alteração de sessão duplicada

Quando houver vários usuários tiver assinados as notificações para a mesma sessão, todas as alterações para a sessão irá disparar um toque shoulder para cada usuário. Todos, exceto um deles será duplicatas. Enquanto ainda é recomendável que um título de assinar todos os usuários em uma sessão para notificações, um título deve ignorar as alterações que já foi notificado sobre; Você pode fazer isso usando as propriedades de ramificação e ChangeNumber.

Para detectar vários toques de shoulder, um título deve:

-   Para cada **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** valor visto, a versão mais recente do repositório **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriedade**.
-   Se um toque shoulder tem uma maior **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** que o último visto para que **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriedade** , processá-lo e atualizar a versão mais recente **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriedade**.
-   Se um toque shoulder não tem uma maior **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** para que **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.Branch**, ignorá-la. Essa alteração já foi tratada.

| Observação                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** valores precisam ser controlados pelo **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriedade**, e não por sessão. É possível que o **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** valor a ser alterado (e o **propriedade RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber**para redefinir) dentro do tempo de vida de uma sessão. |

## <a name="create-an-mpsd-session"></a>Criar uma sessão MPSD


| Observação                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Por padrão, uma sessão MPSD é criada quando o primeiro membro ingressa-la. Se sua lógica de título espera que o título para existe ou não existe em tempo de associação, ele pode passar um valor do modo de gravação apropriadas para o método de gravação durante a atualização da sessão. |



O título deve fazer o seguinte para criar uma nova sessão:

1.  Criar um novo **XboxLiveContext classe** objeto. Seu título cria esse objeto de uma vez, armazena e reutiliza-os conforme necessário em todo o código-fonte. Especialmente ao trabalhar com assinaturas de sessão, é necessário usar exatamente o mesmo contexto.
2.  Criar um novo **MultiplayerSession classe** objeto preparar todos os dados de sessão que o MPSD precisa para criar uma nova sessão.
3.  Faça as alterações necessárias antes de gravar a sessão MPSD. Por exemplo, ao ingressar em um membro para a sessão com uma chamada para **MultiplayerSession.Join método**, o cliente adiciona dados de solicitação de local oculto que informa ao MPSD ingressar após a chamada para atualizar a sessão.
4.  Quando terminar de fazer as alterações locais, gravá-los MPSD conforme descrito em **como: Atualizar uma sessão para múltiplos jogadores**.
5.  Receber o novo **MultiplayerSession** objeto de MPSD, com muitos campos preenchidos.
6.  Use o novo objeto de sessão no futuro e descartar a cópia antiga, que contém uma solicitação ocultada para criar uma nova sessão.

### <a name="example"></a>Exemplo

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>Definir um arbitrador para uma sessão MPSD




O título usa o procedimento a seguir para definir um arbitrador para uma sessão que já foi criada.

| Observação                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tokens de dispositivo para os membros (hosts em potencias) não estão disponíveis até que os membros tenham unida a sessão e incluído seus endereços de seguro de dispositivos. |

1.  Recuperar os tokens de dispositivo para candidatos a host do MPSD usando o **MultiplayerSession.Members propriedade**.

| Observação                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Se a sessão foi criada por SmartMatch cruzamento, seus clientes podem usar os candidatos a host disponíveis no MPSD por meio de **MultiplayerSession.HostCandidates propriedade**. |

2.  Selecione o host necessário da lista de candidatos a host.
3.  Chame o **MultiplayerSession.SetHostDeviceToken método** para definir o token do dispositivo no cache local do MPSD. Se a chamada para definir o host de token do dispositivo for bem-sucedida, o token de dispositivo local substitui o token do host.
4.  Se um código de status 412/HTTP é recebido ao tentar definir o host de token do dispositivo, os dados da sessão de consulta e ver se o token de dispositivo de host é para o console local. Se não for para o console local, outro console tiver sido designado como o arbitrador.

| Observação                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O cliente deve lidar com o código de status HTTP/412 separadamente de outros códigos HTTP, como HTTP/412 não indica uma falha padrão. Para obter mais informações sobre o código de status, consulte [códigos de Status de sessão com vários participantes](multiplayer-session-status-codes.md). |

5.  Atualize a sessão na MPSD, conforme descrito em **como: Atualizar uma sessão para múltiplos jogadores**.

| Observação                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se você não tiver nenhum algoritmo melhor, o cliente pode implementar um algoritmo greedy no qual cada candidato de host tenta se definir como o host se ninguém tiver feito isso. Para obter mais informações, consulte [sessão arbitrador](mpsd-session-details.md). |

## <a name="manage-title-activation"></a>Gerenciar a ativação do título

É acionado Xbox One a **CoreApplicationView.Activated evento** durante a ativação de protocolo. No contexto da API para múltiplos jogadores, este evento é disparado quando um usuário aceita um convite ou acessa outro usuário. Essas ações disparam uma ativação que o título deve reagir a levando o ingresso de usuário no jogo com o usuário de destino.

| Observação                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| O título deve esperar novos argumentos de ativação a qualquer momento e nunca deve ser codificado com base no comprimento. |

O título deve executar as seguintes etapas principais para manipular a ativação do título.

1.  Configurar um manipulador de eventos para o **CoreApplicationView.Activated evento**. Esse manipulador dispara sempre que ocorre a ativação de protocolo, mesmo se o título já está em execução.
2.  Na ativação do título, iniciar uma sessão e assine para receber notificações de alteração de sessão. Consulte **como: Assine para receber notificações de alteração de sessão MPSD**.
3.  Junte-se o usuário para a sessão como ativa. Consulte **como: Junte-se a ativação de um título de uma sessão de MPSD**.
4.  Defina a sessão lobby como a sessão de atividade, exposta por meio do perfil da interface do usuário. Consulte **como: Defina a atividade do usuário atual**.
5.  Junte-se o usuário para o sessão de jogo como ativa. Agora o usuário pode se conectar a colegas e digite jogo ou corredor.

O fluxograma a seguir ilustra como manipular a ativação do título.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>Fazer com que o usuário permite junções

Para tornar o usuário permite junções, o título deve fazer o seguinte:

1.  Criar um objeto de sessão e modificar os atributos conforme necessário.
2.  Junte-se o usuário para a sessão como ativa. Consulte **como: Junte-se a ativação de um título de uma sessão de MPSD**.
3.  Determine se o usuário tiver sido designado como o arbitrador de sessão.
4.  Se o usuário não for o arbitrador, vá para a etapa 7.
5.  Se o usuário é o arbitrador, chame o **MultiplayerSession.SetHostDeviceToken método**.
6.  Tentativa de gravar a sessão usando uma chamada para o **MultiplayerService.TryWriteSessionAsync método**.
7.  Defina a sessão como a sessão ativa. Consulte **como: Defina a atividade do usuário atual**.

O fluxograma a seguir ilustra as etapas necessárias para permitir que um usuário ser unidas por outros jogadores durante um jogo.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>Enviar convites para jogos

O título pode habilitar um player enviar convites de jogo das seguintes maneiras:

-   Envie os convites para a sessão de corredor.
-   Envie os convites usando a plataforma Xbox genérica convidar da interface do usuário com a referência de sessão de jogo.

Para enviar convites de jogo de um player, o título deve fazer o seguinte:

1.  Verifique o player de jogo que está convidando permite junções. Consulte **como: Fazer com que o usuário permite junções**.
2.  Determinar se os convites devem ser enviadas por meio da sessão de corredor ou usando o interface do usuário do convite.
3.  Se usando a sessão lobby, enviar os convites usando uma chamada para **MultiplayerService.SendInvitesAsync método**. Esse método pode exigir a criação de uma lista de interface do usuário no jogo usando o **método SystemUI.ShowPeoplePickerAsync** ou o **PartyChat.GetPartyChatViewAsync método**.
4.  Se usar o convite da interface do usuário, chame o **SystemUI.ShowSendGameInvitesAsync método** para mostrar o convite da interface do usuário.
5.  Lidar com o **RealTimeActivityService.MultiplayerSessionChanged evento** para que o jogador local depois das junções de player remoto.
6.  Para que o jogador remoto, implemente o código de ativação do título. Consulte **como: Gerenciar a ativação de título**.

O fluxograma a seguir ilustra como enviar convites.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>Ingressar em uma sessão de jogo de uma sessão do corredor

Sessões de jogo em dispositivos Windows 10 devem ter o `userAuthorizationStyle` capacidade definida como **verdadeiro** se eles não forem grandes sessões. Isso significa que o `joinRestriction` propriedade não pode ser "none", que significa que a sessão não pode ser publicamente permite junções diretamente.

Um cenário comum é criar uma sessão de lobby para reunir os jogadores e, em seguida, mova esses players para um jogo para que haja correspondência ou uma sessão. Mas se a sessão do jogo não estiver disponível publicamente, os clientes do jogos não poderão ingressar na sessão do jogo, a menos que eles atendam a `joinRestriction` configuração, que é muito restritiva na maioria dos casos para esse cenário.

A solução é usar um identificador de transferência para vincular a sessão lobby e a sessão de jogo.  O título pode fazer isso da seguinte maneira:

1. Quando você cria a sessão de jogo, use o `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API para criar um identificador de transferência que vincula-se a sessão de lobby e a sessão de jogo.
2. Store o GUID do identificador de transferência na sessão lobby em vez de referência de sessão da sessão de jogo.
3. Quando o título deseja mover os membros da sessão lobby para a sessão de jogo, cada cliente usaria o identificador de transferência da sessão lobby para ingressar na sessão de jogo usando o `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. MPSD procura a sessão lobby para verificar se os membros que a tentativa de ingressar na sessão de jogo usando o identificador de transferência também estão na sessão lobby.
5. Se os membros são na sessão lobby, eles poderão acessar a sessão de jogo.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>Ingressar em uma sessão MPSD a ativação de um título

Quando um usuário escolhe unir a atividade de um amigo ou aceitar um convite usando o shell do Xbox da interface do usuário, o título é ativado com parâmetros que indicam qual sessão o usuário deseja ingressar. O título deve lidar com essa ativação e adicione o usuário para a sessão correspondente.

Aqui estão as etapas que o título deve seguir:

1.  Implementar um manipulador de eventos para o **CoreApplicationView.Activated evento**. Esse evento notifica de ativações do título.
2.  Quando o manipulador é acionado, examine os **IActivatedEventArgs.Kind propriedade**. Se ele for definido como protocolo, converta os argumentos do evento para **ProtocolActivatedEventArgs classe**.
3.  Examine os **ProtocolActivatedEventArgs** objeto. Se o URI indicado na **ProtocolActivatedEventArgs.Uri propriedade** corresponde a qualquer um dos inviteHandleAccept (correspondente a um convite aceito) ou activityHandleJoin (correspondente a uma junção por meio do shell da interface do usuário), analisa a cadeia de caracteres de consulta do URI, que é formatado como uma cadeia de caracteres de consulta URI normal com pares chave/valor, extraindo os campos a seguir:
    -   Para um convite aceito:
        1.  Identificador
        2.  invitedXuid
        3.  senderXuid
    -   Para uma junção:
        1.  Identificador
        2.  joinerXuid
        3.  joineeXuid

4.  Iniciar o código para múltiplos jogadores do título, que deve incluir a chamada a **MultiplayerService.EnableMultiplayerSubscriptions método**.
5.  Criar um local **classe MultiplayerSession** do objeto, usando o **MultiplayerSession construtor (XboxLiveContext)**.
6.  Chame o **MultiplayerSession.Join método (String, Boolean, Boolean)** para ingressar na sessão. Use as seguintes configurações de parâmetro para que a junção é definida como ativa:
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  Chame o **MultiplayerSession.SetSessionChangeSubscription método** sejam shoulder-tocados quando a sessão é alterado depois de ingressar.
8.  Chame o **MultiplayerService.WriteSessionByHandleAsync método**, usando o identificador adquirido conforme descrito na etapa 3. Agora o usuário é um membro da sessão e pode usar os dados da sessão para se conectar ao jogo.

## <a name="set-the-users-current-activity"></a>Defina a atividade do usuário atual

A atividade do usuário atual é exibida no Xbox experiências de usuário de painel para o título. Atividade de um usuário pode ser definida por meio de uma sessão ou ativação do título. No último caso, o usuário insere uma sessão por meio de relacionamento de pessoas ou iniciar um jogo.

| Observação                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Atividade definida por meio de uma sessão pode ser excluída com uma chamada para o **MultiplayerService.ClearActivityAsync método**. |

Para definir uma sessão como a atividade do usuário atual, as chamadas de título do **MultiplayerService.SetActivityAsync método**, passando a referência de sessão para a sessão.

Para definir a atividade atual do usuário por meio da ativação de título, consulte **como: Junte-se a ativação de um título de uma sessão de MPSD**.

## <a name="update-an-mpsd-session"></a>Atualizar uma sessão MPSD

| Observação                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Quando o seu título atualiza uma sessão existente usando a API para múltiplos jogadores, lembre-se de que ele está funcionando com uma cópia local, até que ele faz uma chamada para gravar a sessão. |

Para atualizar uma sessão existente, o título deve:

1.  Fazer alterações à sessão atual, conforme necessário, por exemplo, chamando o **MultiplayerSession.Leave método**.
2.  Quando todas as alterações são feitas, grava as alterações locais MPSD, usando qualquer um destes métodos:

    -   **Método MultiplayerService.WriteSessionAsync**
    -   **Método MultiplayerService.WriteSessionByHandleAsync**.
    -   **Método MultiplayerService.TryWriteSessionAsync**
    -   **Método MultiplayerService.TryWriteSessionByHandleAsync**

    Defina o modo de gravação para **SynchronizedUpdate** se gravar em uma parte compartilhada que outros títulos também pode modificar. Ver [sincronização de atualizações de sessão](multiplayer-session-directory.md) para obter mais informações.

    O método write grava a junção no servidor e obtém a sessão mais recente, do qual a descobrir outros membros da sessão e os endereços de seguro de dispositivos (SDAs) dos seus consoles. Para obter mais informações sobre como estabelecer uma conexão de rede entre esses consoles, consulte **Introdução ao Winsock no Xbox One**.

3.  Descartar o objeto de sessão local antigo e usar o objeto de sessão recuperado recentemente, para que as ações futuras são baseadas no estado de sessão conhecidos mais recente.

## <a name="leave-an-mpsd-session"></a>Deixar uma sessão MPSD

Para permitir que um usuário deixar uma sessão, o título deve fazer o seguinte.

1.  Chame o **MultiplayerSession.Leave método** para a sessão de jogo.
2.  Atualize a sessão de jogo em MPSD, conforme descrito em **como: Atualizar uma sessão para múltiplos jogadores**.
3.  Se necessário, chame **deixe** método para a sessão do corredor e atualize essa sessão.
4.  Se for necessário para a sessão de corredor, desligue a API para múltiplos jogadores pelo cancelamento de registro a **evento RealTimeActivityService.MultiplayerSubscriptionsLost** e o  **Evento RealTimeActivityService.MultiplayerSessionChanged**.

O fluxograma a seguir ilustra como sair da sessão e desligar.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>Preencher os slots de sessão aberta durante a compatibilidade

Para preencher os slots abertos em uma sessão de tíquete durante o relacionamento de pessoas, o título deve seguir etapas semelhantes ao seguinte:

1.  Acesse o estado de sessão mais recente para a sessão de tíquete criada durante o relacionamento de pessoas.
2.  Adicione jogadores disponíveis para o jogo da sessão lobby.
3.  Determine se a sessão de tíquete está cheio.
4.  Se a sessão estiver cheio, continue o jogo.
5.  Se a sessão ainda não estiver completa, crie o tíquete de correspondência conforme descrito em **como: Crie um tíquete de correspondência**. Certifique-se de criar o tíquete com o *preserveSession* parâmetro definido como sempre.
6.  Continue com o emparelhamento. Ver [usando o emparelhamento de SmartMatch](using-smartmatch-matchmaking.md).

O fluxograma a seguir ilustra como preencher os slots de sessão aberta durante a compatibilidade.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>Crie um tíquete de correspondência

Para criar um tíquete de correspondência, o scout de relacionamento de pessoas deve:

1.  Chame o **MatchmakingService.CreateMatchTicketAsync método**, passando uma referência para a sessão de tíquete. O método lê a sessão de tíquete de MPSD e começa o cruzamento para os usuários na sessão. Internamente, as chamadas de método as **POST (/hoppers/ /serviceconfigs/ {scid} de {hoppername})**.
2.  Defina as *preserveSession* parâmetro como nunca se o serviço de cruzamento é coincidir com os membros da sessão em uma nova sessão ou outra sessão existente. Defina as *preserveSession* parâmetro como sempre para permitir que o título para reutilizar uma sessão de jogo existente como uma sessão de tíquete para continuar a execução do jogo. O serviço de cruzamento, em seguida, pode garantir que a sessão enviada é preservada e qualquer players correspondentes são adicionados a essa sessão.

3.  Use o **propriedade CreateMatchTicketResponse.EstimatedWaitTime** retornados na **CreateMatchTicketResponse classe** objeto para definir as expectativas dos usuários de tempo para que haja correspondência.
4.  Use o **CreateMatchTicketResponse.MatchTicketId propriedade** retornado no objeto de resposta para cancelar o emparelhamento para a sessão, se necessário, excluindo o tíquete. Tíquete de exclusão usa a **MatchmakingService.DeleteMatchTicketAsync método**.

## <a name="get-match-ticket-status"></a>Obter o status da correspondência de tíquete

O título deve fazer o seguinte para recuperar o status de tíquete de correspondência:

1.  Obter o **MultiplayerSession classe** objeto para a sessão de tíquete.
2.  Use o **propriedade MultiplayerSession.MatchmakingServer** para acessar o **MatchmakingServer classe** objeto usado no emparelhamento.
3.  Verifique as **MatchmakingServer** objeto para determinar o status do processo de emparelhamento, o tempo de espera típico para a sessão e a referência de sessão de destino, se uma correspondência foi encontrada.

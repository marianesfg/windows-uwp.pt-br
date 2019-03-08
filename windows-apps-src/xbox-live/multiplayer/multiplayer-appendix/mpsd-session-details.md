---
title: Detalhes da sessão para múltiplos jogadores
description: Saiba mais sobre os detalhes das sessões para múltiplos jogadores Xbox Live.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: o Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes de 2015, sessão, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616081"
---
# <a name="mpsd-session-details"></a>Detalhes da sessão MPSD

Este artigo contém as seguintes seções:
* Visão geral da sessão
* Recursos de sessão
* Tamanho da sessão
* Estados de sessão do usuário
* Visibilidade e Joinability
* Tempos limite de sessão
* Vários usuários conectados em um único Console
* Gerenciamento de ciclo de vida do processo
* Limpeza de sessões inativas
* Sessão Arbiter

## <a name="session-overview"></a>Visão geral da sessão

Uma sessão de diretório de sessão com vários participantes (MPSD) tem um nome de sessão e é identificada como uma instância de um modelo de sessão, que é um documento JSON que fornece configurações padrão para a sessão. O modelo é parte de uma configuração de serviço com um identificador de configuração de serviço (SCID), que é um GUID. Esse modelo pode ser encontrado na [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com) e [Partner Center](https://partner.microsoft.com/dashboard). Configurações de serviço são recursos voltados para o desenvolvedor usados para ingestão, gerenciamento e política de segurança. Quando uma sessão é acessada por meio de MPSD, autorização de entidade é executada em relação a configuração do serviço de acordo com as políticas de acesso definidas pelo desenvolvedor por meio de XDP ou Partner Center. Verificações de acesso secundária, como validação de associação de sessão, são executadas no nível da sessão quando a sessão é carregada depois que o acesso à configuração de serviço é autorizado.

Este tópico pressupõe que seu modelo usa a versão de contrato 107, que é a versão usada pelo MPSD atual para o Xbox One. Se você tiver definido os modelos com base na versão do contrato 105 (idêntica a 104), você deve alterar isso para dar suporte à versão 107. Para obter instruções, consulte [problemas de migração comuns de 2015 Multiplayer](common-issues-when-adapting-multiplayer.md).

| Importante                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Funcionalidade que é definida por meio de um modelo não pode ser alterada por meio de gravações para o MPSD. Para alterar os valores, você deve criar e enviar um novo modelo com as alterações necessárias. Todos os itens que não são definidos por meio de um modelo podem ser alterados por meio de gravações para MPSD. |

### <a name="session-reference"></a>Referência de sessão

Cada sessão MPSD exclusivamente é referenciado por uma referência de sessão, representada na API do WinRT para múltiplos jogadores pela **MultiplayerSessionReference classe**. A referência de sessão contém esses valores de cadeia de caracteres:

-   Identificador de configuração de serviço (SCID)
-   Nome do modelo de sessão
-   Nome da sessão

A referência de sessão é mapeado para o URI para identificar sessões, conforme mostrado abaixo. O mapeamento de exemplo a seguir, "autoridade" é sessiondirectory.xboxlive.com.

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>Elementos de uma sessão

Cada sessão contém grupos de elementos que impõem regras Mutabilidade e a segurança, que variam de acordo com o elemento de sessão, juntamente com informações de manutenção do sistema somente leitura (metadados). Esta seção descreve os grupos de elementos de sessão incluídos nos arquivos JSON para configurar a sua sessão e no arquivo JSON para o modelo que você escolher.

| Observação                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se você estiver usando wrappers do WinRT para uma implementação de HTTP/REST, sua sessão e o modelo devem definir objetos JSON que reflitam com precisão a funcionalidade do WinRT. |

Dentro de cada um dos grupos de elementos, há dois objetos internos:

-   Objetos do sistema – esses objetos têm um esquema fixo que é imposto e interpretado por MPSD. Elas são validadas e mescladas. Como MPSD define e sabe o que eles significam, ele pode agir sobre eles. Para obter a definição completa de cada um dos objetos do sistema, consulte as referências para o **classe MultiplayerSession** e as referências para o **URIs do diretório de sessão**.
-   Objetos personalizados – esses objetos são opcionais e não têm esquema. Eles são usados para armazenar metadados sobre um jogo com vários participantes. Uma vez que MPSD não pode interpretar esses dados, ele não é tratado. Dados de jogos ou salvas informações devem ser armazenadas no armazenamento gerenciado pelo título (TMS). Para obter mais informações sobre TMS, consulte **armazenamento de título do Xbox Live**.

Aqui está um exemplo de um objeto JSON personalizado:
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>Constantes de sessão

Constantes de sessão são definidas somente no momento da criação, pelo criador ou pelo modelo de sessão. O objeto /constants/system é usado para definir constantes para o sistema com vários participantes, como é conhecido por meio de MPSD. O wrapper do WinRT associado a este objeto é representado pela **MultiplayerSessionConstants classe**.

O objeto /constants/system pode definir um número de itens, incluindo um objeto de recursos, um objeto de métricas, um managedInitialization (versão do contrato de modelo 104/105) ou objeto memberInitialization (versão de contrato 107), um peerToPeerRequirements um objeto measurementsServerAddresses, um objeto peerToHostRequirements e objeto.


#### <a name="session-properties"></a>Propriedades da sessão

Use o objeto /properties/system para definir propriedades de sessão para MPSD. O wrapper do WinRT associado a este objeto é o **MultiplayerSessionProperties classe**. Propriedades da sessão são graváveis por membros da sessão a qualquer momento. São exemplos de propriedades de sessão no formato JSON: joinRestriction, initializationSucceeded e o objeto de emparelhamento. Para obter um exemplo do uso desse grupo de elemento, consulte [inicialização da sessão de destino e QoS](smartmatch-matchmaking.md).


#### <a name="member-constants"></a>Constantes de membro

Membro do conjunto de constantes em tempo de associação para cada membro da sessão. O objeto JSON é /members/ {index} / constantes/sistema. A classe de wrapper do WinRT que representa um membro de sessão é o **MultiplayerSessionMember classe**.


## <a name="member-properties"></a>Propriedades do membro

Propriedades do membro são graváveis apenas por um membro de sessão. Eles são definidos no objeto /members/ {index} / propriedades/sistema e refletir os elementos do **MultiplayerSessionMember classe**. Aqui está um exemplo:

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>Elementos do servidor

Os servidores são não-os usuários que têm Unido ou foi convidados para uma sessão. Os objetos JSON associados são /servers/ {server-name} / constantes/sistema e /servers/ {server-name} / propriedades/sistema. Esses objetos são graváveis somente pelos servidores.

| Observação                                                         |
|---------------------------------------------------------------------------|
| O objeto do sistema /servers/ {server-name} / constantes/não é usado no momento. |


### <a name="session-configuration"></a>Configuração de sessão

Há duas maneiras de controlar a configuração de sessões:

-   Use modelos de sessão ingeridos por meio de XDP ou Partner Center.
-   Use chamadas para o cruzamento de APIs do WinRT ou APIs REST e com vários participantes. Você ainda deve usar um modelo, mas não tem o modelo conter os valores que você deseja configurar. Observe que o título não pode substituir as constantes já definidas no modelo.

Um documento JSON separado é fornecido para definir a sessão propriamente dita. Além disso, o desenvolvedor deve implementar qualquer funcionalidade de wrapper do WinRT necessária para um título específico. O conteúdo dos documentos JSON e qualquer código de wrapper do WinRT uns aos outros deve refletir com precisão e deve refletir a versão mais recente do contrato de modelo.

O esquema para uma sessão é com controle de versão com a versão de sessão (versão principal) e a revisão de protocolo (versão secundária). As versões são combinadas no cabeçalho X-Xbl-versão de contrato como "100 \* principal + secundário". Por exemplo, um título de v1.7 inclui o seguinte cabeçalho em cada solicitação REST, supondo que a última versão do contrato de modelo do 107: X-Xbl-Contract-Version: 107.

| Observação                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| É recomendável para a maioria dos títulos (usando XSAPI) para usar a versão do contrato 105 e versão do modelo de sessão 107. |


### <a name="session-templates"></a>Modelos de sessão

Cada modelo de sessão é um documento JSON, parte da configuração do serviço, que define a estrutura para a sessão que está sendo criada e fornece constantes para a nova sessão. Para obter mais informações, consulte [modelos de sessão MPSD](../service-configuration/session-templates.md).

## <a name="session-capabilities"></a>Recursos de sessão

Recursos são constantes na sessão MPSD que configurar o comportamento que o MPSD deve ser aplicados a essa sessão. Mais comumente em que você usar XDP e Centro de parceiros para definir recursos no modelo de sessão. Elas são definidas no objeto /constants/system/capabilities. Se não há recursos que são necessários, use um objeto de recursos vazio.

| Observação                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| Títulos quase nunca altere ou acessar recursos de sessão usando a API do WinRT para múltiplos jogadores ou o emparelhamento de API do WinRT. |

Recursos de sessão são representados pela **MultiplayerSessionCapabilities classe**. Eles são valores boolianos que indicam o que pode dar suporte a sessão:

-   Conectividade
-   Jogo
-   Tamanho grande
-   Conexão necessária para membros ativos

O **MultiplayerSessionConstants classe** define as propriedades a seguir que se referem a recursos de sessão:

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| Observação                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| Se o título define um recurso dinâmico de sessão, a propriedade correspondente é definida como true para constantes de sessão. |

## <a name="session-size"></a>Tamanho da sessão

O tamanho de uma sessão MPSD é determinado pelo número de membros na sessão.


### <a name="maximum-session-size"></a>Tamanho máximo de sessão

O tamanho máximo de uma sessão é o número máximo de membros de sessão, que ele pode acomodar. Ele é representado pela **MultiplayerSessionConstants.MaxMembersInSession propriedade**. O tamanho máximo do membro é definido no objeto /constants/system.

O tamanho máximo de sessão está entre 1 e 100 membros da sessão e o padrão é 100, se não definido na criação. Se o tamanho necessário estiver acima de 100, a sessão é chamada de uma sessão do "grande" e é definida de forma especial.

Definindo um tamanho máximo para uma sessão pode causar um slot aberto apareça como completo durante determinadas desconectar cenários. Por exemplo, se um jogador for desconectado como resultado de uma falha de energia ou de rede, o atraso não será imediatamente refletido na sessão. O membro é definido como inativo, usando o recurso de detecção de desconexão descrito em [detecção de desconectar e tratamento de notificação de alteração de MPSD](multiplayer-session-directory.md).

Em comparação, uma malha ponto a ponto que usa uma pulsação para detectar uma desconexão geralmente está ciente de uma desconexão dentro de duas a três segundos e pode abrir o slot de jogador imediatamente. No entanto, o arbitrador não é possível remover outros membros.

### <a name="large-sessions"></a>Sessões grandes

Uma sessão MPSD grande pode ter até 1000 membros, mas ele tem alguns recursos de sessão desabilitados, como obter uma lista de todos os membros. Largeness de sessão é representado pela **MultiplayerSessionCapabilities.Large propriedade**. Essa propriedade é definida como true para indicar uma sessão de grande e a capacidade de "grande" é indicada no objeto /constants/system/capabilities. 

## <a name="session-user-states"></a>Estados de sessão do usuário



MPSD define o estado do usuário como o status de um usuário que foi adicionado a uma sessão. Estados do usuário possíveis são definidos pela **MultiplayerSessionStatus enumeração**. O usuário também é considerado para ter um status de "disponível" antes de serem adicionados a uma sessão.

O **MultiplayerSession.SetCurrentUserStatus método** pode ser usado para alterar o estado de sessão do usuário. Essa alteração pode ser feita para REST pelo /members/ {index} / propriedades/sistema de configuração corretamente no documento JSON de sessão de jogo.


### <a name="reserved-user-state"></a>Estado de usuário reservado

O usuário é colocado no estado de usuário reservado quando o arbitrador tiver selecionado o usuário para preencher um dos slots abertos dentro da sessão. Nesse estado, o usuário ainda não foram oficialmente aceitarem o convite para a sessão ou associado a sessão para iniciar a conexão com os colegas.


### <a name="active-user-state"></a>Estado de usuário do Active Directory

Quando um usuário estiver no estado ativo, o título ingressou a sessão em nome do usuário e o usuário está participando ativamente na sessão. O usuário continua nesse estado, desde que ele é jogar o jogo.

Quando um título é iniciado pela primeira vez, ele deve verificar se o usuário já é um membro do todas as sessões, normalmente, verificando o estado de sessão. Se o usuário for um membro de sessão, o título pode ir diretamente para o jogo e definir quaisquer membros participantes de locais para o estado do usuário ativo.

Um usuário deve permanecer no estado ativo quando estiver em execução na sessão. Se um usuário deixar a sessão por meio da interface do usuário no jogo, o usuário deve ser removido da sessão com o **MultiplayerSession.Leave método**. Se o usuário é apenas temporariamente fora do jogo, como quando o título é restrito, o título deve manter o usuário no estado ativo por um período razoável de tempo. Ele é apropriado alterar o estado do usuário para inativo se o usuário não tiver retornado após um período de tempo especificado de título.


### <a name="inactive-user-state"></a>Estado de usuário inativo

No estado inativo, o usuário não está atualmente envolvido com o jogo, mas ainda tem um slot salvo na sessão. Em outras palavras, o usuário é "não ativo".

É o console do usuário que tem a responsabilidade de definir esse estado de usuário para usuário inativo na sessão. O arbitrador não é possível executar esta ação. Cenários de exemplo em que um usuário é colocado em estado inativo incluem:

-   O título recebe um evento de suspender.
-   O usuário ficar inativo (sem resposta de entrada ou controlador) por um período de tempo definido pelo título. É recomendável dois minutos para vários jogadores competitivo.
-   O título foi no modo restrito por mais de dois minutos, ou por um período definido pelo título. Esse período de tempo limite de modo restrito é o valor esperado de tempo pelo qual um usuário pode ficar longe o título usando um aplicativo relacionado ou outras experiências relacionadas ao título.
-   O usuário foi desconectado maneira brusca da sessão. Ver [MPSD alterar o tratamento de notificação e desconectar detecção](multiplayer-session-directory.md).

Se o título é iniciado e o estado do usuário para um membro específico de sessão é definido como inativa, o título foi suspenso ou o usuário estava inativo por muito tempo na sessão. Como o título está iniciando novamente, a indicação é que o usuário deseja continuar com a sessão de jogo para o qual ele pertence. Se o estado do usuário está ativo na inicialização, o título, essa situação é provavelmente devido a uma desconexão da rede ou outro cenário em que o título foi possível definir o usuário para inativo antes da interrupção. Em ambos os casos, seu título deve tentar reconectar-se o usuário com o jogo e os outros usuários para continuar a reproduzir ou remova o usuário da sessão.

### <a name="user-state-when-the-session-is-over"></a>Estado quando a sessão do usuário está em

Quando a sessão terminar, o jogo foi descontinuado. O título deve permitir que todos os usuários a ser removido por conta própria usando o **MultiplayerSession.Leave método**. As atividades de sessão associadas aos usuários são removidas automaticamente quando eles saírem da sessão.

## <a name="visibility-and-joinability"></a>Visibilidade e Joinability

Acesso de sessão é controlado no nível do MPSD pelas duas configurações: visibilidade de sessão e joinability de sessão. As recomendações de visibilidade e joinability neste tópico se aplicam para os cenários mais comuns do título. Títulos devem seguir essas configurações, se possível, e eles devem usar lógica no título para fazer a determinação final e autoritativa sobre se um novo player é admitido em uma sessão.


### <a name="session-visibility"></a>Visibilidade de sessão

Visibilidade de sessão é representada por uma constante que é definida na criação de sessão. Ele é normalmente definido no modelo de sessão e determina quais tipos de usuários que leu e acesso de gravação a uma sessão. Os valores possíveis para a visibilidade de sessão são definidos pela **MultiplayerSessionVisibility enumeração**. As configurações de permissão para a constante de visibilidade em um arquivo JSON são abertos, visíveis e privada.


#### <a name="recommended-game-session-visibility-open"></a>Recomendado a visibilidade da sessão de jogo: abrir

Sessões abertas de jogos não exigem reservas de player, que simplifica o processo de convite. O arbitrador não reserva jogadores em MPSD após um convite foi enviado, mas controla somente convidados players localmente. Assim, os jogadores imediatamente podem se conectar o arbitrador e determinar se eles devem ingressar em uma sessão, são rejeitados ou devem aguardar (se há suporte para os jogadores de espera). O arbitrador é a autoridade ultimate e responde e instrui o novo membro para permanecer em ou sair da sessão.

Usando a visibilidade de sessão aberta de jogo requer o player de convidado iniciar um título e conecte-se para o arbitrador antes que a decisão final foi feita. É aceitável para exibir uma mensagem de erro para o usuário se uma sessão estiver cheio ou se um convite foi rejeitado.

Para estabelecer uma conexão para o arbitrador, é necessário um endereço de seguro de dispositivos. O **MultiplayerSessionProperties.HostDeviceToken propriedade** é usado para descobrir qual membro de sessão é o arbitrador atual de uma sessão e qual proteger um player de convidado deve usar para conexão de endereço do dispositivo.

### <a name="session-joinability"></a>Sessão Joinability

Joinability sessão determina quais tipos de usuários podem ingressar em uma sessão. Ele pode ser definido dinamicamente durante uma sessão. Os valores possíveis para joinability de sessão são:

-   None (padrão) – não há nenhuma restrição sobre quem pode ingressar na sessão.
-   Local – apenas usuários locais poderão ingressar na sessão.
-   Seguido – apenas usuários locais e os usuários que são seguidos por outros membros da sessão poderão ingressar na sessão sem reserva.

Um arbitrador sessão pode criar uma sessão privada por meio da configuração joinability. Tornar joinability local ou seguido restringe o acesso à sessão e torna-o particular.

Além disso, o arbitrador deve manter o controle de sessão joinability, de modo que a sessão mais antigo convida pode ser rejeitada no nível do host, se necessário. Por exemplo, se qualquer players convidados não chegaram para ingressar em uma sessão até que a sessão já está cheia, o arbitrador pode instruir os players de junção que a sessão foi bloqueada e eles precisam sair da sessão automaticamente.

## <a name="session-timeouts"></a>Tempos limite de sessão

As sessões podem ser alteradas por outros eventos externos e temporizadores. Tempos limite de sessão define os períodos em que os membros da sessão podem permanecer em estados específicos antes de serem automaticamente tornadas inativas ou removidos da sessão. MPSD também dá suporte a tempos limite para gerenciar a vida útil da sessão.

| Observação                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurações de tempo limite são feitas em /constants/system/timeouts ou dentro do objeto gerenciado de inicialização, para a versão do modelo de contrato 104/105. Para versão 107 ou posterior, as configurações são feitas individualmente no sistema/constantes/ou em um objeto gerenciado de inicialização. |

Quando um temporizador expira, MPSD atualizar a sessão não automaticamente e notificar o arbitrador nesse momento todas as alterações. Os estados de sessão e o tempo limite só são atualizados imediatamente antes de uma leitura ou gravação solicitação é enviada. Atualização imediata garante que os dados retornados são o mais atualizada.

| Observação                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| Tempos limite de sessão não é empilhada, e somente um é aplicado a uma transição de estado em relação a cada membro da sessão em uma atualização. |


### <a name="currently-defined-timeouts"></a>Tempos limite definido no momento

Esta seção descreve os limites que são definidos no momento pelo MPSD. Todos os tempos limite são especificados em milissegundos. Um valor de 0 é permitido e indica um tempo limite imediato. Um tempo limite sem nenhum valor é considerado infinito. Como os tempos limite tem padrões, você deve especificar explicitamente nulo para um tempo limite infinito.
#### <a name="evaluationtimeout"></a>evaluationTimeout

Esse tempo limite indica a quantidade de tempo para um membro de sessão fazer e carregar a decisão de avaliação. Se nenhuma decisão for recebido, a decisão de conta como uma falha. Esse tempo limite é colocado no objeto de inicialização gerenciado.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

Esse limite é definido para um membro de sessão que ingressou em uma sessão, mas atualmente não está envolvido no jogo. O membro é removido da sessão após 2 horas, por padrão.

| Observação                                                                      |
|----------------------------------------------------------------------------------------|
| Esse tempo limite é designado o tempo limite de inativo para a versão do modelo de contrato 104/105. |

Em muitos casos, é recomendável definir o tempo limite de inativo para 0, fazendo com que qualquer usuário que é definido como o estado inativo a ser removido imediatamente da sessão e o slot correspondente a ser apagado. Esse comportamento é desejável para jogos para múltiplos jogadores mais competitivos, para que, se um usuário tem ficado inativo ou atingido um estado inativo, um novo jogador pode ser adicionado rapidamente. Para cooperação ou outros designs com vários participantes, você talvez queira seu título para permitir que os usuários mais tempo para se reconectar se eles são desconectados ou não se empenham no título por períodos de tempo. Observe que nenhuma solução se encaixa em todos os cenários de design.

#### <a name="jointimeout"></a>joinTimeout

Esse tempo limite indica o número de milissegundos que um usuário tem para ingressar na sessão. Reservas de usuários que não conseguir ingressar são removidas. Esse tempo limite é colocado no objeto de inicialização gerenciado.


#### <a name="measurementtimeout"></a>measurementTimeout

Esse tempo limite indica a quantidade de tempo que um membro da sessão tem para carregar as medidas. Um membro que não puder ser carregado de medidas é marcado com um motivo da falha de "tempo limite". Esse tempo limite é colocado no objeto de inicialização gerenciado.

| Observação                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Durante o emparelhamento, um tempo limite de 45 segundos para as medidas de QoS é imposto. Portanto, recomendamos o uso de um tempo limite de medição que é menor que ou igual a 30 segundos durante o relacionamento de pessoas. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

Esse tempo limite é definido para um membro da sessão que ingressou em sessão e está tentando obter ao jogo. Isso geralmente significa que o shell ingressou no usuário em nome do título e o título está sendo iniciado. O membro é removido da sessão e colocado no estado inativo após 3 minutos, por padrão.

| Observação                                                          |
|----------------------------------------------------------------------------|
| Esse tempo limite é designado o tempo limite de pronto para a versão de contrato 104/105. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

Esse tempo limite é definido para um membro da sessão que foi adicionado à sessão por outra pessoa, mas ainda não ingressou a sessão. A reserva é excluída e o membro será considerado inativo quando o tempo limite expirar. O valor padrão é 30 segundos.

| Observação                                                             |
|-------------------------------------------------------------------------------|
| Esse tempo limite é designado o tempo limite de reservado para a versão de contrato 104/105. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

Esse tempo limite indica o número de milissegundos depois que uma sessão se torna vazia quando ela é excluída. O valor padrão é 0.

| Observação                                                                 |
|-----------------------------------------------------------------------------------|
| Esse tempo limite é designado o tempo limite de sessionEmpty para a versão de contrato 104/105. |


### <a name="session-timeout-example"></a>Exemplo de tempo limite de sessão

1.  Uma sessão é iniciada com quatro jogadores.
2.  Dois jogadores, A e B, estão desconectados devido a uma falha de energia. Seu status no jogo permanece ativo.
3.  Os outros dois jogadores, C e D, encerrar corretamente usando o **MultiplayerSession.Leave método**.
4.  A sessão permanece aberta, com os jogadores A e B desconectado, mas ainda está no estado ativo.
5.  Alguns dias mais tarde, o Player A retorna e inicia o jogo.
6.  Jogo de Player do verifica se há sessões que um é um membro de (executa uma leitura) e localiza a sessão órfão de alguns dias atrás.
7.  A sessão faz uma verificação de presença em relação as dois jogadores que ainda estão na sessão (A e B).
    1.  Como o Player um está executando o título, a verificação de presença em relação a um do Player for bem-sucedida, e a estado ativo do jogador na correspondência permanece o mesmo.
    2.  B Player não está executando o título. Assim, a verificação da presença do Player B falhar e o serviço define o estado do Player B como inativa. Neste ponto, o tempo limite da inativo inicia para b Player.

8.  Sai de um Player da sessão corretamente usando o **deixe** método.
9.  Expira o tempo de limite de inativo para o Player B, que é removido da sessão na próxima leitura ou gravação por qualquer pessoa.
10. Agora, a sessão tem zero membros e é removida do serviço.

Se o tempo limite de inativo para a sessão de exemplo é definido como 0, B Player expirar imediatamente após a verificação da presença na etapa 7a e provavelmente será removido pela gravação de sessão. Nesse caso, fecha a sessão sem a necessidade de mais ler ou gravar para a sessão.


## <a name="multiple-signed-in-users-on-a-single-console"></a>Vários usuários conectados em um único Console


Quando vários usuários estão entrou no mesmo console, é possível que alguns usuários para estar em uma sessão de jogo, enquanto outros usuários não estão na sessão ou não estão ativa no título atual. Convites do jogos também podem ser recebidos e aceita para vários usuários, tendo um impacto na associação de sessão de jogo. Um título precisa considere estes pontos para ser capaz de lidar com todos os cenários de associação de sessão corretamente.

Em um cenário comum, um novo player faz logon, se torna ativo no jogo e precisa ser adicionado a uma sessão de jogo existente. Como com a criação de uma nova sessão de jogo, um título só deve adicionar um usuário quando é apropriado durante o jogo.

Com vários usuários conectados, um ou mais usuários também podem receber convites a outra sessão de jogo. Títulos não é necessário tratar desses cenários de qualquer maneira específica. Eventos de membro e de estado de sessão notificam o título de todas as atualizações para a associação de sessão e usuário de jogo.

Para lidar com vários usuários conectados para uma sessão online, o título se inscreve para cima do ombro toca para todos os usuários, usando um separado **XboxLiveContext classe** objeto para cada usuário. O título usa o **MultiplayerSession.ChangeNumber propriedade** determinar alterações específicas na sessão e ignorar os ombro duplicado toca.


## <a name="process-lifecycle-management"></a>Gerenciamento de ciclo de vida do processo


Assim como um título de não-multiplayer, um título em uma sessão para múltiplos jogadores pode encontrar a suspensão de título e o término dos eventos de ciclo de vida do processo. O arbitrador de sessão, portanto, deve salvar periodicamente o estado de sessão. No caso do arbitrador é suspenso, o título deve tentar migração arbitrador e salvar o estado do jogo conforme apropriado, para que um arbitrador nova possa restaurar o estado de sessão. Em seguida, é possível de uma sessão com vários participantes completos para ser suspensos e retomados posteriormente, desde que a sessão ainda é válida em MPSD. Apenas um ponto designado, normalmente o host do jogo, deve atualizar o estado global do jogo.


### <a name="storage-of-game-metadata"></a>Armazenamento de metadados de jogo

Um título armazena metadados de jogos na sessão MPSD. Metadados de jogo são as informações necessárias para exibir dados de sessão e habilitar o título localizar e ingressar na sessão de jogo. O título armazena metadados específicos do player na seção de propriedades personalizadas para o sessão membro, por exemplo, cor do player, armas player preferido para a sessão, etc. Metadados de toda a sessão, por exemplo, map atual, é armazenado na seção global de propriedades personalizadas da sessão MPSD.


### <a name="storage-of-game-state"></a>Armazenamento de estado do jogo

Estado do jogo é armazenado no armazenamento gerenciado pelo título (TMS), usando o **serviço de armazenamento do título**. Usando este local de armazenamento permite que um título migrar o arbitrador sem preocupações com a permissão. Ver [migrando um arbitrador](migrating-an-arbiter.md).

| Observação                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| O título não deve tentar salvar o estado do jogo em TMS com mais frequência do que uma vez a cada 5 minutos, a menos que ele está sendo suspenso. |

## <a name="cleanup-of-inactive-sessions"></a>Limpeza de sessões inativas

Se o sessionEmptyTimeout for definido como 0, uma sessão MPSD é excluída automaticamente quando o jogador última sai da sessão. Para saber como impedir que um sessões não utilizados que contém os jogadores após falha ou desconexão, consulte [detecção de desconectar e tratamento de notificação de alteração de MPSD.](multiplayer-session-directory.md). Tratamento inadequado de sessões não utilizados após pane ou desconectar pode causar problemas quando um título está consultando as sessões de um player.

A maneira recomendada para limpar sessões inativas é que a consulta de título todas as sessões de um usuário específico chamando o **MultiplayerService.GetSessionsAsync método** e, em seguida, avaliar as sessões. Quando ele encontra uma sessão obsoleta, o título chama o **MultiplayerSession.Leave método** de todos os jogadores locais na sessão. Essa chamada descarta a contagem de membros como 0, eventualmente e limpa as sessões.

## <a name="session-arbiter"></a>Sessão Arbiter


Alguns métodos para múltiplos jogadores só devem ser chamados por um cliente dentro de uma sessão de jogo. Esse cliente é um dos consoles participam da sessão, chamado o arbitrador ou o host. Se pelo menos um membro de sessão estiver em um jogo, a sessão deve ter um arbitrador para monitorar a junções em andamento.


### <a name="setting-the-arbiter"></a>Definindo o arbitrador

Quando ele cria uma sessão, o cliente designa um único console, como o arbitrador. Consulte [como: Definir um arbitrador para uma sessão MPSD](multiplayer-how-tos.md).


### <a name="saving-session-state"></a>Salvando o estado de sessão

Conforme discutido em **gerenciamento de ciclo de vida do processo**, o arbitrador deve salvar periodicamente o estado de sessão. Um arbitrador novo deve ser capaz de restaurar o estado de sessão no caso de migração do arbiter pelo título. Para obter mais informações, consulte [migrando um arbitrador](multiplayer-how-tos.md).


### <a name="managing-game-session-members-and-joins-in-progress"></a>Gerenciar os membros de sessão de jogo e junções em andamento

A função mais importante do arbiter a sessão é gerenciar os usuários voltarem para a sessão de jogo para reproduzir. Isso inclui tratamento de convites do jogos, notificando os jogadores de espera e trabalhando com jogadores que encerrar o jogo.


#### <a name="receiving-notifications"></a>Recebimento de notificações

O arbitrador deve escutar para novos jogadores que desejam ingressar na sessão de jogo com o **RealTimeActivityService.MultiplayerSessionChanged evento**.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>Localizando os jogadores para preencher os Slots de sessão de jogo vazio

O arbitrador localiza os jogadores para preencher os slots de sessão de jogo vazio por uma dessas operações:   Se seu título usa uma sessão de corredor ou outro mecanismo para permitir junções atrasadas, localize membros de nova sessão usando esse mecanismo.
-   Crie outra sessão de tíquete de correspondência.

Consulte também [como: Preencher os Slots de sessão aberta durante cruzamento](multiplayer-how-tos.md).


#### <a name="handling-invited-session-members"></a>Tratamento de convidados membros da sessão

O arbitrador deve monitorar os membros convidados de sessão e aplicar um intervalo mínimo entre convites para um único usuário. Consulte também [como: Enviar convites do jogos](multiplayer-how-tos.md).

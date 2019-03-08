---
title: Diretório de sessão multijogador do Xbox One
description: Saiba mais sobre como criar sessões para múltiplos jogadores, usando o serviço Xbox Live Mutliplayer sessão diretório (MPSD).
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606811"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Diretório de sessão multijogador do Xbox One

Este tópico fornece uma visão geral da criação da sessão para múltiplos jogadores usando o novo serviço Xbox um com vários participantes sessão diretório (MPSD). O documento seja direcionado principalmente para desenvolvedores de título Xbox One que enviar seus modelos de sessão diretamente para Xbox desenvolvimento Portal (XDP). O serviço MPSD também pode ser configurado com o Centro de parceiros, mas não se concentra neste artigo. Ele destina-se para familiarizá-lo com os termos e conceitos associados à configuração MPSD, uso e solução de problemas de sessões com vários participantes.

## <a name="revision-summary"></a>Resumo da revisão

As bibliotecas do lado do cliente XSAPI (API de serviços do Xbox) utilizam a versão de contrato 104 ao se comunicar com os serviços MPSD. Esta versão do documento atualiza o esquema de modelo de sessão de contrato versão 107, que é a versão mais recente do contrato com suporte dos serviços MPSD. As alterações entre a versão 104 e 107 são resumidas na seção intitulada [atualização do esquema de contrato](#_Contract_schema_update).

Observe que os modelos que foram desenvolvidos para a versão do contrato 104 precisará ser atualizado se eles forem republicados como 107. No entanto, os serviços são compatíveis com versões anteriores para que você possa continuar a usar as bibliotecas atuais, que serão atualizadas em uma versão futura do XDK.

A revisão anterior deste documento atualizado informações sobre sessões do servidor e adicionada uma nova seção sobre APIs de serviços do Xbox Live e chamadas de serviço RESTful. Além disso, a tabela encontrada na consulta para a seção de informações de estado de sessão foi atualizada e a seção de modelos de qualidade de serviço (QoS) foi revisada.

## <a name="introduction"></a>Introdução

No Xbox One, uma sessão para múltiplos jogadores é um documento seguro que reside na nuvem no diretório de sessão para múltiplos jogadores (MPSD), e este documento representa um grupo de pessoas a jogar um jogo. Especificamente, as sessões com vários participantes tem as seguintes qualidades:

-   Cada sessão tem um URI exclusivo.

-   O documento de sessão contém os membros atuais, as configurações do jogo, inicializar dados e informações de servidor do jogo.

-   Títulos de criar e gerenciar suas próprias sessões.

-   Uma sessão permite a conectividade entre seus membros.

O diretório de sessões com vários participantes centraliza os metadados da sessão de jogo em todos os clientes. Ele fornece as informações básicas necessárias sobre uma sessão para ajudar a configurar as associações de seguro de dispositivos entre os clientes. Ele também fornece metadados básicos da primeira inicialização para um cliente se conecte ao jogo antes de começar a passagem de dados mais específicos do jogo. Com o gerenciamento de tempo de vida do processo (PLM) e a natureza da alternância de tarefas de aplicativos no Xbox One, o MPSD garante que os clientes tenham as informações corretas para entrar em contato com colegas e servidores que são identificados como parte da sessão ativa de jogo e coordenadas com o sistema de operacional do shell e console para reservar, ativar e gerenciar o tempo de vida de player para uma sessão de jogo.

## <a name="terminology-used-in-this-document"></a>Terminologia usada neste documento

| Termo                 | Definição                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sessão para múltiplos jogadores  | Um documento seguro que reside na nuvem Xbox Live e representa um grupo de usuários (ou seja) conectadas ao jogar um título no Xbox One. Todos os aspectos da multiplayer — como o relacionamento de pessoas, as partes, junção em andamento e assim por diante — aproveitar a sessão para múltiplos jogadores. |
| Sessão de jogo         | Isso é a sessão de jogo real, exposta em MPSD, em que os usuários jogar juntos. Todos os cenários com vários participantes, por fim, acabam em uma sessão de jogo.                                                                                                                               |
| Sessão de tíquete de correspondência | Essa é uma sessão usada para controlar o envio de tíquetes de correspondência durante a compatibilidade.                                                                                                                                                                                                                 |
| Player inativo      | Um player que foi definido para o estado inativo dentro da sessão. O título define um usuário para o Inactive estado quando o jogo é restrito, suspenso, ou caso contrário, inativa, conforme definido pelo título.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>O diretório de sessão para múltiplos jogadores

O MPSD facilita e ajuda a títulos de coordenar as informações de sessão entre jogadores online. Pode haver diferentes tipos de sessões criadas para realizar tarefas diferentes de jogo com vários participantes. A tabela a seguir apresenta as diferenças em como essas tarefas foram feitas no Xbox 360 em vez de como elas são realizadas no Xbox One.

| Função ou tarefa                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Obter informações de sessão de jogo**     | **XSessionGetDetails**, **XSessionSearchByID**, ou controlar por conta própria.                                              | As informações da sessão de solicitação do serviço.                                                          |
| **Migrar de host**                     | Quando necessário, o título chama **XSessionMigrateHost.**                                                           | Não é necessário criar uma nova sessão, basta atribuir um novo host para o SessionID.                                  |
| **Gerenciar várias sessões de player**  | Difícil de lidar com mais de uma sessão por vez. Por exemplo, **XNetReplaceKey** versus **XNetUnregisterKey**. | Sessão baseada em serviço facilita o gerenciamento de uma sessão mais limpo e torna mais fácil de manipular várias sessões.    |
| **Manipular transferências de entrada e desconecta** | Precisa lidar com desconecta e saída de forma diferente, com **XCloseHandle** ou **XSessionDelete**, respectivamente. | Serviço centralizado simplifica as transferências de logon e desconectar o tratamento e tempos limite é definido na configuração do jogo. |

### <a name="session-patterns"></a>Padrões de sessão

-   Sessões de jogos

    -   Sessão com XUIDs jogadores, os dados de endereço de seguro de dispositivos e estados de propriedade. Isso é considerado como a sessão de jogo real.

    -   Pode ser peer-to-peer, ponto a ponto para o host, servidores de mesmo nível ou híbrida.

-   Sessões de tíquete de correspondência

    -   Uma sessão que é enviada para o emparelhamento para localizar outros jogadores de jogar com. Observe que uma sessão de jogo também pode ser uma sessão de tíquete, se o jogo estiver procurando mais jogadores.

-   Sessões do servidor

    -   Sessões de jogos criado e usado por meio do processamento do Xbox Live Compute.

Figura 1 ilustra os usos de sessões MPSD, onde as caixas azuis representam sessões MPSD e as caixas vermelhas são os serviços que estiverem sendo usados.

Figura 1. Uso de sessão MPSD.

## <a name="mpsd-sessions"></a>Sessões MPSD

Sessões de mantêm as duas listas de entidades relacionadas:

1.  Membros (usuários) que tem Unido ou foi convidados para uma sessão.

2.  Servidores (servidores de nuvem ou servidores dedicados) que se associaram a sessão.

Cada entidade tem:

1.  Valores constantes definidos no momento da criação.

2.  Propriedades mutáveis.

3.  Metadados somente leitura.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>APIs de serviços do Xbox Live e chamadas de serviço RESTful

Há duas maneiras para fazer interface com os serviços de emparelhamento e as sessões de um Xbox. A primeira maneira é usar chamadas HTTPS padrão para URIs do Xbox Live serviços RESTful. Isso permite flexibilidade para títulos de chamada e fazer interface com esses serviços, dependendo de suas configurações de servidor e jogos. Uma lista desses URIs pode ser encontrada na [documentação do Kit de desenvolvimento um Xbox (XDK)](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) sob "Xbox Live referência RESTful de serviços." [1]

O segundo método é usar o Xbox Live APIs de serviço, que atuam como wrappers para o serviço RESTful URIs. Elas permitem que uma abordagem mais tradicional para usando as APIs de código sem ter que lidar com o tráfego HTTPS para cada chamada. O código-fonte para as APIs do serviço Xbox Live é fornecido com o Kit de desenvolvimento do Xbox (XDK) e pode ser modificado e incorporado ao seu título, conforme necessário. Os exemplos são escritos usando as APIs de serviços do Xbox Live. Para obter mais informações sobre as APIs de serviços do Xbox Live podem ser encontradas no Xbox One [documentação do XDK](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) sob "Xbox Live Services referência". [2]

### <a name="mpsd-sessions-and-templates"></a>Modelos e sessões MPSD

Sessões MPSD são criadas com modelos ingeridos por meio do Portal de desenvolvimento de Xbox (XDP). Os modelos são documentos JSON que definem a estrutura para a sessão que está sendo criada. O modelo fornece constantes para a nova sessão.

O seguinte trecho do [exemplo de código de Rendezvous Player](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) é um exemplo de configuração de modelo.

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

A sessão de tíquete de correspondência deve ser usada com um modelo de sessão de jogo configurado com valores de tempo limite de QoS em sua **memberInitialization** objeto.

Figura 2. Hopper de exemplo.

![](../../images/whitepapers/mpsd_image1.png)

O trecho de código que segue é um exemplo de um modelo de sessão de jogo peer-to-peer (gerenciados pelo título QoS).

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

Este é um exemplo de uma sessão de servidor de mesmo nível e o modelo BRUTO.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

O código a seguir mostra um exemplo de um modelo de sessão de tíquete de correspondência, que é usado para corresponder inteligente.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>Verificando quais modelos são configurados para seu SCID

#### <a name="session-templates"></a>Modelos de sessão

A lista de modelos de sessão que existem dentro de um SCID, bem como os detalhes de um modelo de sessão específica, podem ser recuperados do MPSD:

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>Consulta para obter informações de estado de sessão

As sessões podem ser consultadas nos níveis de modelo de sessão e configuração de serviço:

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

Por padrão, isso retornará até 100 sessões não privado. A consulta pode ser modificada usando esses parâmetros de cadeia de caracteres de consulta:

| Cadeia de caracteres de consulta             | Efeito                                                                                                         | Descrição                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | Incluir apenas as sessões que têm a palavra-chave "foo".                                                             |                                                                                                     |
| XUID=123                 | Inclua apenas as sessões que o usuário "123" está em.                                                               | Por padrão, o usuário deve estar ativo na sessão a ser incluído.                                  |
| *as reservas*=**true** | Inclua sessões para o qual o usuário é definido como um player reservado, mas não ingressou para se tornar um player de Active Directory. | Apenas ao consultar suas próprias sessões, ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| *inactive*=**true**      | Inclua as sessões que o usuário aceitou, mas não está ativamente sendo reproduzido.                                     | Contagem de sessões nas quais o usuário está "pronto", mas não "ativo" como inativas.                           |
| *private*=**true**       | Inclua sessões privadas.                                                                                      | Apenas ao consultar suas próprias sessões, ou ao consultar o servidor para servidor.                            |
| *visibilidade*= aberto        | Incluir apenas as sessões que estão "abertas".                                                                         | Se definido como "privado", o "privado = true" diretiva também deve ser definida.                                 |
| *take*=5                 | Retorne até cinco sessões.                                                                                    | Deve estar entre 0 e 100.                                                                          |

O resultado é uma matriz JSON de referências de sessão. Alguns dados de sessão estiver embutido.

**Observação** cada consulta deve incluir um filtro de palavra-chave, um filtro XUID ou ambos.

Configuração tanto *privada* (que retorna sessões privadas) ou *reservas* (que retorna as sessões do usuário não tiver ingressado) para **verdadeiro** requer que o chamador tenha acesso de nível de servidor à sessão ou de declaração XUID do chamador para corresponderem ao filtro XUID no URI. Caso contrário, 403 Proibido é retornado (se realmente existirem tais as sessões).

O trecho de código a seguir mostra um exemplo de uma resposta de consulta.

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>Atributos de modelo de sessão

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>Atualização do esquema de contrato

Como foi mencionado no início deste documento, a versão de contrato de modelo de sessão mais recente é 107, que apresenta algumas alterações no esquema da versão anterior do 104. Modelos que foram desenvolvidos para a versão do contrato 104 precisará ser atualizado se eles forem republicados como 107. A seguir está um resumo das alterações.

-   **/Constants/System/managedInitialization** foi renomeada para **/constants/system/memberInitialization**.

    -   O **autoEvaluate** campo foi renomeado para **externalEvaluation** e as alterações de polaridade, exceto que **false** continua sendo o padrão.

    -   O valor padrão de **membersNeededToStart** muda de 2 para 1.

    -   O valor padrão de **joinTimeout** muda de 5 segundos para 10 segundos.

    -   O **measurementTimeout** padrão será alterado de 5 segundos para 30 segundos.

-   **/Constants/System/timeouts** for removido, e os tempos limite são renomeados e realocados sob **/constantes/sistema**.

    -   O **reservada** tempo limite se torna **reservedRemovalTimeout**.

    -   O **inativos** tempo limite se torna **inactiveRemovalTimeout** e seu novo padrão é 0, em vez de 2 horas.

    -   O **pronto** tempo limite se torna **readyRemovalTimeout**.

    -   O **sessionEmpty** tempo limite se torna **sessionEmptyTimeout**.

-   **/Constants/System/Capabilities/gameplay** deve ser especificado como **verdadeiro** em sessões que representam o jogo real (em vez de sessões de auxiliar, como sessões de correspondência e lobby) para permitir que os jogadores recentes e reputação de relatório.

### <a name="system-objects"></a>Objetos do sistema

Cada um dos objetos de sistema do documento de sessão tem um esquema fixo que é imposto e interpretado por MPSD.

Dentro do corpo de solicitações PUT, os objetos do sistema são validados e mesclados, assim como os objetos personalizados. Mas ao contrário de objetos personalizados, depois que eles são mesclados o sistema objetos adicionais são validados e tratados com base nesses esquemas.

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>Tempos limite

As sessões podem ser alteradas por outros eventos externos e temporizadores. O **tempos limite** objeto em MPSD fornece um mecanismo básico para gerenciar o tempo de vida de sessão e membros.

O **nextTimer** campo da sessão dá a hora do próximo temporizador. Clientes podem usar essas informações para escolher um intervalo de BOM para a próxima sondagem. Esse valor também é retornado a **Expires** cabeçalho HTTP.

Tempos limite é especificada em milissegundos. Zero é permitida e significa que o tempo limite deve ser imediato. Se o tempo limite determinado não for especificado, ele será considerado infinito. Como os tempos limite têm padrões, o modelo de sessão deve especificar explicitamente "nulo" para um tempo limite infinito.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

O **/constants/system/sessionEmptyTimeout** valor configura o número de milissegundos depois que uma sessão se torna vazia que ele será excluído. O padrão é zero, indicando que a sessão será excluída imediatamente. Se o valor for especificado, as sessões vazias ficará ativo por tempo indeterminado.

#### <a name="member-timeouts"></a>Tempos limite de membro

Os três outros tempos limite dentro **/constantes/sistema** controlar a quantidade de tempo que um membro pode permanecer em um estado específico:

-   **reservedRemovalTimeout**

    -   A reserva é excluída quando o tempo limite expirar. O padrão é de 30 segundos.

-   **inactiveRemovalTimeout**

    -   Um membro inativo é removido da sessão após duas horas por padrão.

-   **readyRemovalTimeout**

    -   Os membros que estão "prontos" reverter para o estado inativo após três minutos por padrão.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>Inicialização de membro em documentos de sessão

### <a name="member-initialization"></a>Inicialização de membro


O **memberInitialization** estágios de inicialização após a criação de sessão e/ou novos membros do objeto controla ingressar na sessão. Os estágios de inicialização e tempos limite são rastreados automaticamente pela sessão, incluindo medidas de QoS se quaisquer métricas são definidas.

Esses tempos limite substituir a reserva da sessão e os tempos limite de pronto para os membros que têm o **initializationEpisode** conjunto de propriedades.

Exemplo:

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**Figura 3. Fluxo de inicialização de membro.**

Cada um dos três estágios da inicialização de membro pode atingir o tempo limite:

1.  **joiningTimeout**

    -   A quantidade de tempo, em milissegundos, que os usuários tenham que ingressar na sessão. Reservas de membros que não conseguir ingressar são removidas.

2.  **measuringTimeout**

    -   A quantidade de tempo que os membros têm para carregar suas medidas. Os membros que falham ao carregar as medidas são marcados com um *failureReason* de "tempo limite".

3.  **evaluationTimeout**

    -   A quantidade de tempo para um membro tornar e carregar a decisão de avaliação. Se nenhuma decisão for recebido, ele conta como uma falha.

**externalEvaluation**

-   MPSD pode fazer uma avaliação automática de QoS com base nos requisitos fornecidos com o modelo de sessão. A avaliação é executada quando externalEvaluation é definida como false. **externalEvaluation** precisa ser **verdadeira** quando uma **evaluationTimeout** está definido. Se houver dois requisitos peer-to-peer ou host do par, você ainda deve definir **externalEvaluation** à **falso** para que a sessão terminar automaticamente a inicialização.

**membersNeededToStart**

-   Este é o número mínimo de reservas de membro que precisam existir com "inicializar": "true" e passe a QoS para a avaliação automática seja bem-sucedida.

### <a name="initialization-episodes"></a>Episódios de inicialização


Quando o **memberInitialization** objeto estiver definido, MPSD progredirão as fases de inicialização pelo episódio. O primeiro episódio é iniciado quando a sessão é criada e incluirá todas as fases definidas no modelo.

Os novos membros convidados ou associados enquanto um episódio já está em execução serão marcados para o próximo episódio. O status de um episódio ou **memberInitialization** em geral, pode ser recuperando da **Inicializando** objeto da sessão.

**Observação** esteja ciente de que este objeto é removido após a conclusão da inicialização.

Exemplo:

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

O estágio vai de ingresso para avaliar a avaliar. Se um episódio falha, o estágio é definido como *falha* e a sessão não pode ser inicializada. Caso contrário, quando um episódio de inicialização for concluída, o **Inicializando** objeto é removido.

Falhas de inicialização também podem ser rastreadas para cada membro. Elas são definidas durante a transição fora do estágio de junção ou medição se este membro não passar.

Exemplo:
```
"initializationFailure": "latency",
```
Em ordem de precedência, o valor para esse atributo pode ser definido como *tempo limite, latência, bandwidthdown, bandwidthup, rede e grupo*, ou *episódio*. O valor de rede significa que a configuração de rede e/ou condições (como a conversão de endereços de rede conflitantes \[NAT\]) impediu que as métricas de QoS que está sendo medido. É o único valor possível no final da associação *grupo*. (No tempo limite de junção, a reserva é removida.)

Se **memberInitialization** é definido e o membro foi adicionado com "inicializar": true, isso é definido como o episódio de inicialização que participará de membro. Um valor de 1 é usado para os membros adicionados a uma nova sessão no momento em que ele é criado, e ele será removido quando termina o episódio de inicialização.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>Sessão de tíquete de correspondência

Quando uma sessão MPSD está sendo usada como uma sessão de tíquete de correspondência, algumas propriedades de sessão especial e constantes são usadas.

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

Quando o serviço de cruzamento adiciona usuários a uma sessão, ele fornece algum contexto em torno de como e por que eles foram correspondidos para a sessão na **matchmakingResult** campo.

```
"matchmakingResult": {
```

Esta é uma cópia de um usuário **serverMeasurements** da sessão de emparelhamento.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>Qualidade dos modelos de serviço (QoS)

Para modelos de sessão de jogo, os valores podem ser adicionados que informar MPSD para coordenar com a camada de rede e plataforma social do console. A finalidade dessa coordenação é validar a qualidade do estado de conexão antes de uma sessão é criada e antes que um usuário é informado de que o jogo está pronto para ingressar.

O trecho de código a seguir está um exemplo de um modelo de sessão de jogo de host do par com QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

Esse trecho de código é um exemplo de um modelo de sessão de jogo peer-to-peer com QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>Atributos de modelo de sessão de QoS

Se um **memberInitialization** objeto estiver definido, a sessão espera que o sistema cliente ou o título para realizar a inicialização após a criação de sessão e/ou como novos membros ingressar na sessão.

Os estágios de inicialização e tempos limite são rastreados automaticamente pela sessão, incluindo medidas de QoS se quaisquer métricas são definidas.

Esses tempos limite substituir a reserva da sessão e os tempos limite de pronto para os membros que têm o **initializationEpisode** conjunto de propriedades.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

Uma sessão de jogo com o QoS requer a capacidade de conectividade. Se nenhuma métrica forem especificada, eles têm como padrão o que seria necessário para atender os requisitos de QoS. Se eles forem especificados, eles devem ser suficientes para atender os requisitos de QoS.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

Os seguintes limites se aplicam a cada conexão emparelhada para todos os membros em uma sessão:

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

Os seguintes limites se aplicam a cada conexão de um candidato de host:

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

A seguinte conexão de servidor possíveis cadeias de caracteres devem ser avaliada (Observe que as cadeias de caracteres de conexão devem estar em minúsculas):

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

Esses sinalizadores controlam o status de membro e **activeTitle**, e eles são mutuamente exclusivos (é um erro para definir ambos como **verdadeiro**). Para cada um, **falsos** é igual a "não está presente." O status padrão é "inativo", ou seja, que nenhum deles estiver presente.

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>Detalhes de inicialização de sessão e a fase de QoS

MPSD rastreará e relatar os resultados de QoS para criação de jogo após o modelo de inicialização de membro. O progresso dessa operação será representado por um *Inicializando* do objeto do documento de sessão conforme descrito na [inicialização de membro](#_Member_initialization_in) seção acima. O *Inicializando* objeto tem um *estágio* atributo, que representa o estágio de inicialização atual. O estágio que progride de *unindo* à *medindo* para *avaliar*.

-   Se Inicializando episódio \#1 falhar, estágio será definido como *falha* e a sessão não pode ser inicializada. Caso contrário, quando um episódio de inicialização for concluída, o objeto "Inicializando" é removido. Se **externalEvaluation** é definido como **falso**, o estágio de avaliação será ignorada. Se nem **métricas** nem **measurementServerAddresses** estiver definido, o estágio de medição é ignorado.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   Candidatos a host são tokens de dispositivo listadas na ordem de preferência. Eles são definidos após o *medindo* estágio do episódio de inicialização \#1 se **peerToHostRequirements** é definido e **/properties/system/host** não está definido. Eles são limpos após um **/properties/system/host** objeto é definido.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   O *gamertag* atributo será definido somente se o membro aceitou e se uma declaração de nome de jogador foi encontrada para esse membro.

```json
"gamertag": "stacy",
```

-   O *deviceToken* atributo é definido quando o membro carrega um endereço de seguro de dispositivos. É uma cadeia de caracteres de maiusculas e minúsculas que pode ser usada para comparações de igualdade.

```json
"deviceToken": "9f4032ba7",
```

-   O *reservado* valor é removido depois que o usuário executa sua solicitação PUT primeiro para o documento de sessão. Quando os jogadores são reservados, que significa que foram convidados para a sessão de jogo, mas não aceitaram nem tinham avaliada de suas conexões.

```json
"reserved": true,
```

-   Se o membro estiver ativo, *activeTitleId* é o título em que o membro está ativo, no formato decimal.

```json
"activeTitleId": "8397267",
```

-   Este atributo se refere ao tempo que o usuário associado a sessão. Se *reservada* é **verdadeiro**, em seguida, *joinTime* será a hora em que a reserva foi feita.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   Presentes se este membro da matriz de propriedades/sistema/turn, caso contrário, não é.

```json
"turn": true,
```

-   O *initializationFailure* atributo é definido no objeto membro quando a transição fora o ingresso ou medir o estágio se o membro ainda não aprovado o estágio. Em ordem de precedência, ele pode ser definido como *timeout*, *latência*, *bandwidthdown*, *bandwidthup*, *rede* , *grupo*, ou *episódio*.
    O *rede* valor significa que a configuração de rede e/ou condições (como conversões de endereço de rede conflitantes \[NATs\]) impediu que as métricas de QoS que está sendo medido. É o único valor possível no final da associação *grupo*. (No tempo limite de junção, a reserva é removida.) O *episódio* valor é definido após um estágio de "avaliação" com falha em todos os membros do que o episódio de inicialização que não falham durante o ingresso ou medição.

```json
"initializationFailure": "latency",
```

-   Se **memberInitialization** é definido e o membro foi adicionado com "inicializar": true, isso é definido como o episódio de inicialização que participará de membro. Um valor de 1 é usado para os membros adicionados a uma nova sessão no momento da criação. Removido quando termina o episódio de inicialização.

```json
"initializationEpisode": 1,
```

-   O *próxima* atributo representa o valor de índice do próximo membro na sessão. É o mesmo valor que o *próxima* atributo na **membersInfo** objeto abaixo se não houver nenhum membro para adicionar.

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Sessão de computação em nuvem Xbox

Uma sessão de computação em nuvem Xbox contém a lista ordenada de cadeias de caracteres de conexão diferencia maiusculas de minúsculas que a sessão pode usar para se conectar a um servidor de jogo. Em geral, títulos devem usar a primeira cadeia de caracteres na lista, mas títulos sofisticados poderia usar um mecanismo personalizado (como Xbox Live Compute) para escolher um dos demais (por exemplo, com base na carga).

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

* * / servidores / {nome do servidor} / propriedades/sistema * *

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>Sessão bruto e propriedades do título personalizado

Uma sessão pode ser usada para armazenar informações de manutenção do sistema personalizado (metadados) em torno de um jogo com vários participantes. Dados de jogos ou salvas informações devem ser armazenadas em TMS + +.

### <a name="property-bags"></a>Recipientes de propriedades

Cada um dos objetos acima marcado como um conjunto de propriedades consiste em dois objetos internos opcionais, sistema e personalizados.

Os objetos personalizados podem conter qualquer JSON.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>Decaimento membro ativo

Membros ativos são marcados automaticamente inativos quando MPSD detecta que o usuário não está envolvido no título. Isso pode acontecer, por exemplo, se a presença atinge o tempo limite de registro do usuário. Esse mecanismo é apenas uma barreira; ou seja, títulos ainda são necessários para proativamente marcar membros como inativo (ou removê-los da sessão) quando os membros deixem ou alternar para fora o título, o sinal de saída ou de outra forma soltou.

## <a name="faq-and-troubleshooting"></a>Perguntas Frequentes e solução de problemas

### <a name="how-do-i-call-mpsd-"></a>Como posso chamar MPSD?

Usando a autenticação de certificado: sessiondirectory.xboxlive.com de cliente

Exemplo:

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

Usando a autenticação XToken: sessiondirectory.xboxlive.com

Exemplo:

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Como posso descobrir quais SCID, um modelo de sessão e uma área restrita usar?

Essas informações podem ser encontradas no site XDP do título. Se você ainda não tiver acesso ao XDP entre em contato com seu gerente de conta de desenvolvedor, que pode ajudar a obter as informações para você.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>Há um exemplo de um corpo de solicitação que pode comparar meu próprio para?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>Estou recebendo um erro 404 ao chamar MPSD.

Colete rastreamentos do Fiddler para ajudar a obter mais informações e, em seguida, faça o seguinte:

1.  Verifique a mensagem retornada como parte do corpo do HttpResponse para mensagens de 404 comuns:

    -   *A configuração de serviço solicitado é inválido ou não foi configurado para sessões*. Verifique se o SCID que está sendo usada está correto.

    -   *A sessão solicitada não foi encontrada*. Verifique se a sessão é criada e o modelo de sessão está correto antes de recuperá-los. Você pode criar uma sessão com uma chamada PUT.

2.  Verifique se o URI que você está usando.

3.  Reinicie o console e/ou tente novamente com um novo usuário.

4.  Pesquisa o [fóruns de desenvolvedores de entretenimento](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) para o código de erro ou outras soluções possíveis.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>Estou recebendo um erro 403 ao chamar MPSD.

Isso geralmente é uma permissão ou problema de escopo. Colete rastreamentos do Fiddler para ajudar a obter mais informações e, em seguida, faça o seguinte:

1.  Verificar mensagens retornadas como parte do corpo HttpResponse para 403 mensagens comuns:

-   * A configuração de serviço solicitado não pode ser acessada. *

    -   Verifique se que você está usando uma conta que tenha acesso à área restrita.

    -   Verifique se que você está na área restrita correta.

    -   Se você estiver usando a autenticação de certificado e recebendo esse erro, entre em contato com sua mãe.

-   *A sessão solicitada não pode ser acessada. Sessões privadas só podem ser lidos por membros da sessão.*

    -   Você está tentando acessar uma sessão que tem uma visibilidade de "privado". Somente os membros dentro da sessão podem ler o documento de sessão.

-   *O corpo da solicitação não pode conter referências de membro existente, a menos que a autenticação de entidade de segurança inclui um servidor.*

    -   Você não pode ingressar outro usuário em uma sessão em nome do usuário. Você pode convidar apenas. Definir o índice "reservar\_&lt;número&gt;" convidar um player.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>Estou recebendo um erro 412 Falha na pré-condição.

Isso retornará 412 Falha na pré se a sessão já existe:

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/rápido/sessões/foo HTTP/1.1 Content-Type: application/json If-None-Match: \*

Isso retornará 412 Falha na pré se a etag da sessão não coincidir com o cabeçalho If-Match:

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/rápido/sessões/foo HTTP/1.1 Content-Type: application/json If - Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>Estou obtendo erros como 405, 409, 503 e 400when MPSD de chamada.

Coletar rastreamentos do Fiddler para ajudar a obter mais informações e, em seguida, verifique as mensagens retornadas como parte do corpo HttpResponse. Isso deve fornecer informações suficientes para identificar e corrigir o erro ou pesquisar o [fóruns de desenvolvedores de entretenimento](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) para obter possíveis soluções.

Você também pode obter o corpo da resposta se você estiver usando as APIs do serviço Xbox Live, definindo o **DiagnosticsTraceLevel** propriedade erro, quais serão as informações a saída da depuração de saída, ou você pode usar o  **XboxLiveContextSettings.ServiceCallRouted** eventos conforme demonstrado em várias amostras de saída para o título da interface do usuário.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>O que é possível ou devo alterar nos modelos de título da minha?

Modelos de sessão não são os padrões, mas são mais de um ciclo. No entanto, você não pode substituir as constantes já definidas nos modelos, embora você possa adicionar a eles.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Estou recebendo um erro que está dizendo que minha sessão não foi inicializado.

Quando a inicialização de membro está presente em seu modelo (jogo, da parte e cenários de tíquete de correspondência, normalmente), você precisará certificar-se de que "inicializar = true" é definido para o suficiente, as reservas de membro (membros necessários para iniciar o) para passar a QoS para corrigir esse problema.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>Minha sessão não está sendo criado e estou recebendo um erro de HTTP 204.

Isso indica que não havia nenhum usuário adicionado à sessão quando você o criou. Se não houver nenhum usuário para uma sessão no momento da criação, a sessão não será criada. Certifique-se de que você adicione pelo menos um jogador a uma sessão de criação. Para cenários de servidor dedicado, obtenha um player de quem está tentando criar uma correspondência, ou quem precisa criar uma correspondência, e fazer o que o usuário o player inicial em que correspondem. Como alternativa, você pode alterar ou remover as **sessionEmptyTimeout**.

### <a name="when-should-i-poll-the-mpsd"></a>Quando eu deve sondar o MPSD?

Você deve evitar a sondagem sessões MPSD. Em um alto nível, você pode fazer isso criando seu código de forma que utiliza a sessão MPSD somente para o estabelecimento de conectividade de rede inicial para cada cliente e para restabelecer o estado da rede para um cliente ou clientes que tem perdido a conectividade. Também é importante tirar proveito com base em etag do MPSD primitivos de sincronização para eliminar a necessidade de atualizar o estado de sessão para resolver condições de corrida.

Um aplicativo comum desses princípios é quando você tem um conjunto de clientes de N todos precisam conectar-se em conjunto e tocar em uma malha ponto a ponto. Em vez de consultar regularmente a sessão para novos membros, cada membro pode ingressar na sessão, conecte-se aos membros já na sessão e supor que quaisquer entradas posteriores fará o mesmo. Consulte os exemplos de reunião do Player e bate-papo para obter exemplos de como implementar isso.

Há alguns casos raros em que a sondagem pode ser necessária por breves períodos; Se você achar que precisa fazer isso, entre em contato com seu gerente de conta de desenvolvedor.

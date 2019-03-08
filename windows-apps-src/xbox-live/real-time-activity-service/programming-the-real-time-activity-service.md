---
title: O serviço de atividade em tempo real de programação
description: Saiba mais sobre como programar o Xbox Live em tempo real atividade de serviço com as APIs do C++.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, a atividade em tempo real
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629991"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>O serviço de atividade em tempo real usando as APIs de C++ de programação

Este artigo contém as seções a seguir

* Conectando ao serviço de atividade em tempo real do Xbox Live
* Desconectado do serviço de atividade em tempo real
* Criar uma estatística
* Inscrever-se em uma estatística da atividade em tempo real
* Cancelando a assinatura de uma estatística do serviço de atividade em tempo real
* Exemplo

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Conectando ao serviço de atividade em tempo real do Xbox Live

Aplicativos devem se conectar ao serviço de atividade em tempo real (RTA) para obter informações de evento do Xbox Live. Este tópico mostra como fazer tal uma conexão.

> [!NOTE]
> Os exemplos usados neste tópico indicam chamadas de método para um usuário. No entanto, o título deve fazer essas chamadas para todos os usuários se conectar e desconectar do serviço de atividade em tempo real (RTA).

### <a name="connecting-to-the-real-time-activity-service"></a>Conectando ao serviço de atividade em tempo real

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>Criar uma estatística

Criar estatísticas em XDP se você for um desenvolvedor XDK ou trabalhando em um título de cross-play.  Você pode criar estatísticas no Partner Center se você estiver fazendo um puro UWP em execução no Windows 10.

#### <a name="xdk-developers"></a>Desenvolvedores do XDK

Para obter informações sobre como criar uma estatística em XDP, consulte o [XDP documentação](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events).  Depois que você criou seu stat e definidos seus eventos, você precisará executar o [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) para gerar um cabeçalho usado pelo seu aplicativo.  Esse cabeçalho contém funções que você pode chamar para enviar eventos que modificam as estatísticas.

#### <a name="uwp-developers"></a>Desenvolvedores da UWP

Se você estiver desenvolvendo um UWP no Windows 10 que não é um título de cross-play, você define suas estatísticas na [Partner Center](https://partner.microsoft.com/dashboard). Leia as [artigo de configuração de estatísticas do Partner Center](../leaderboards-and-stats-2017/player-stats-configure-2017.md) para aprender a configurar estatísticas no Partner Center.

> [!NOTE]
> Estatísticas 2013 developer será necessário entrar em contato com sua mãe para informações referentes aos [configuração 2013 Stats](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) na [Partner Center](https://partner.microsoft.com/dashboard).

### <a name="disconnecting-from-the-real-time-activity-service"></a>Desconectar-se de que o serviço de atividade em tempo real

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>Inscrever-se em uma estatística da atividade em tempo real

Aplicativos se inscrever para um em tempo real atividade (RTA) para obter atualizações quando altera as estatísticas configuradas no Portal de desenvolvedor do Xbox (XDP) ou no Partner Center.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>Inscrever-se em uma estatística do serviço de atividade em tempo real

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>Cancelando a assinatura de uma estatística do serviço de atividade em tempo real

Aplicativos assinem uma estatística do serviço de atividade em tempo real (RTA) para obter atualizações quando a estatística é alterado. Quando essas atualizações não são mais necessários, a assinatura pode ser encerrada e o código neste tópico mostra como fazer isso.

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>Cancelando a assinatura de uma estatística de serviços em tempo real

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> A atividade em tempo real serviço desconectará após duas horas de uso, seu código deve ser capaz de detectar isso e restabelecer uma conexão para o serviço de atividade em tempo real, se ele ainda é necessário. Isso é feito principalmente para garantir que os tokens de autenticação sejam atualizadas após a expiração.
> 
> Se um cliente usa RTA para sessões com vários participantes e desconectado de trinta segundos, o Directory(MPSD) de sessão para múltiplos jogadores detecta que a sessão de RTA é fechada e é acionada logoff da sessão de usuário. Cabe ao cliente RTA para detectar quando a conexão é fechada e iniciar uma reconexão e assinar novamente antes de MPSD encerra a sessão.
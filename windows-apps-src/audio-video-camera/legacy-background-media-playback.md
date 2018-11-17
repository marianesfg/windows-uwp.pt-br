---
author: drewbatgit
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: Este artigo fornece suporte para aplicativos que usam o modelo de mídia em segundo plano herdado para reprodução e fornece orientações para migrar para o novo modelo.
title: Reprodução de mídia em segundo plano herdada
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 319343a06eeb49fc4ec0ca2fcd340f655654f718
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7159687"
---
# <a name="legacy-background-media-playback"></a>Reprodução de mídia em segundo plano herdada


Este artigo descreve o modelo herdado de dois processos para adicionar suporte para áudio em segundo plano ao seu aplicativo UWP. A partir do Windows 10, versão 1607, é usado um modelo de processo único para áudio em segundo plano que é muito mais simples de implementar. Para obter mais informações sobre as recomendações atuais para áudio em segundo plano, consulte [Reproduzir mídia em segundo plano](background-audio.md). Este artigo tem o objetivo de fornecer suporte para aplicativos que já foram desenvolvidos usando o modelo herdado de dois processos.

> [!NOTE]
> A partir do Windows, versão 1703, **BackgroundMediaPlayer** foi preterido e pode não estar disponível em versões futuras do Windows.

## <a name="background-audio-architecture"></a>Arquitetura de áudio em segundo plano

Um aplicativo executando a reprodução em segundo plano consiste em dois processos. O primeiro processo é o aplicativo principal, que contém a interface do usuário do aplicativo e a lógica do cliente, executando em primeiro plano. O segundo processo é a tarefa de reprodução em segundo plano, que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) como todas as tarefas em segundo plano de aplicativo UWP. A tarefa em segundo plano contém a lógica de reprodução de áudio e os serviços em segundo plano. A tarefa em segundo plano se comunica com o sistema por meio de controles de transporte de mídia do sistema.

O diagrama a seguir é uma visão geral de como o sistema foi criado.

![Arquitetura de áudio em segundo plano do Windows 10](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

O namespace [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) contém APIs usadas para reproduzir áudio em segundo plano. Há uma única instância de [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) por aplicativo através da qual ocorre a reprodução. Seu aplicativo de áudio em segundo plano chama métodos e configura propriedades na classe **MediaPlayer** para definir a faixa atual, iniciar reprodução, pausar, avançar, voltar e assim por diante. A instância de objeto media player sempre é acessada através da propriedade [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528).

## <a name="mediaplayer-proxy-and-stub"></a>Proxy e Stub do MediaPlayer

Quando **BackgroundMediaPlayer.Current** é acessado do processo em segundo plano de seu aplicativo, a instância **MediaPlayer** é ativada no host de tarefa em segundo plano e pode ser manipulada diretamente.

Quando **BackgroundMediaPlayer.Current** é acessado do aplicativo em primeiro plano, a instância **MediaPlayer** retornada é, na verdade, um proxy que se comunica com um stub no processo em segundo plano. Esse stub se comunica com a verdadeira instância **MediaPlayer**, que também é hospedada no processo em segundo plano.

Os processos em primeiro e segundo plano podem acessar a maioria das propriedades da instância **MediaPlayer**, com exceção de [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) e [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635), que só o processo em segundo plano pode acessar. O aplicativo em primeiro plano e o processo em segundo podem receber notificações de eventos específicos de mídia, como [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/dn652609), [**MediaEnded**](https://msdn.microsoft.com/library/windows/apps/dn652603) e [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/dn652606).

## <a name="playback-lists"></a>Listas de reprodução

Um cenário comum para aplicativos de áudio em segundo plano é reproduzir vários itens em fila. Isso é mais facilmente atingido em seu processo em segundo plano usando um objeto [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955), que pode ser definido como uma fonte no **MediaPlayer** atribuindo-o à propriedade [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010).

Não é possível acessar uma **MediaPlaybackList** a partir do processo em primeiro plano que foi configurada no processo em segundo plano.

## <a name="system-media-transport-controls"></a>Controles de transporte de mídia do sistema

Um usuário pode controlar a reprodução de áudio sem usar diretamente a interface do usuário de seu aplicativo através de meios como dispositivos Bluetooth, SmartGlass e os controles de transporte de mídia do sistema. Sua tarefa em segundo plano usa a classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) para assinar esses eventos de sistema iniciados pelo usuário.

Para obter uma instância **SystemMediaTransportControls** de dentro do processo em segundo plano, use a propriedade [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635). Os aplicativos em primeiro plano obtém uma instância da classe chamando [**SystemMediaTransportControls.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708), mas a instância retornada é somente de primeiro plano e não tem relação com a tarefa em segundo plano.

## <a name="sending-messages-between-tasks"></a>Enviando mensagens entre tarefas

Há ocasiões em que você deseja a comunicação entre os dois processos de um aplicativo de áudio em segundo plano. Por exemplo, você pode querer que a tarefa em segundo plano notifique a tarefa em primeiro plano quando uma nova faixa começar a ser reproduzida e, em seguida, envie o título da nova música à tarefa em primeiro plano para exibição na tela.

Um mecanismo de comunicação simples suscita eventos nos processos em primeiro e segundo plano. Os métodos [**SendMessageToForeground**](https://msdn.microsoft.com/library/windows/apps/dn652533) e [**SendMessageToBackground**](https://msdn.microsoft.com/library/windows/apps/dn652532) invocam eventos nos processos correspondentes. Mensagens podem ser recebidas assinando os eventos [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) e [**MessageReceivedFromForeground**](https://msdn.microsoft.com/library/windows/apps/dn652531).

Os dados podem ser passados como um argumento para os métodos de mensagem de envio que são, então, passados para os manipuladores de eventos de mensagem recebida. Passe dados usando a classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). Essa classe é um dicionário que contém uma cadeia de caracteres como uma chave e outros tipos de valor como valores. Você pode passar tipos de valores simples, como inteiros, cadeias de caracteres e boolianos.

## <a name="background-task-life-cycle"></a>Ciclo de vida de tarefas em segundo plano

O tempo de vida de uma tarefa em segundo plano está estreitamente relacionado ao status de reprodução atual do seu aplicativo. Por exemplo, quando o usuário pausa a reprodução de áudio, o sistema pode encerrar ou cancelar seu aplicativo, dependendo das circunstâncias. Após um período de tempo sem reprodução de áudio, o sistema pode desligar automaticamente a tarefa em segundo plano.

O método [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) é chamado na primeira vez que seu aplicativo acessa [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) do código sendo executado no aplicativo em primeiro plano ou quando você registra um manipulador para o evento [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530), o que quer que aconteça primeiro. É recomendável que você se registre para o manipulador de mensagem recebida antes de chamar **BackgroundMediaPlayer.Current** pela primeira vez, para que o aplicativo em primeiro plano não perca nenhuma mensagem enviada do processo em segundo plano.

Para manter a tarefa em segundo plano viva, seu aplicativo deve solicitar um [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) de dentro do método **Run** e chamar [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) quando a instância de tarefa receber os eventos [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) ou [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788). Não faça um loop nem espere no método **Run**, porque isso consome recursos e pode fazer com que a tarefa em segundo plano de seu aplicativo seja finalizada pelo sistema.

Sua tarefa em segundo plano obtém o evento **Completed** quando o método **Run** é completo e não há solicitação de adiamento. Em alguns casos, quando seu aplicativo obtém o evento **Canceled**, ele também pode ser seguido pelo evento **Completed**. Sua tarefa pode receber um evento **Canceled** enquanto **Run** está sendo executado, então certifique-se de gerenciar essa concorrência potencial.

Situações nas quais a tarefa em segundo plano pode ser cancelada incluem:

-   Um novo aplicativo com recursos de reprodução de áudio é iniciado em sistemas que aplicam a subpolítica de exclusividade. Consulte a seção [Políticas de sistema para o tempo de vida de tarefas de áudio em segundo plano](#system-policies-for-background-audio-task-lifetime) abaixo.

-   Uma tarefa em segundo plano foi iniciada, mas a música ainda não está tocando, então o aplicativo em primeiro plano é suspenso.

-   Outras interrupções de mídia, como a entrada de chamadas telefônicas ou chamadas VoIP.

Situações em que a tarefa em segundo plano pode ser encerrada sem notificação incluem:

-   Uma chamada VoIP entra e não há memória suficiente disponível no sistema para manter a tarefa em segundo plano ativa.

-   Uma política de recurso é violada.

-   O cancelamento ou a conclusão da tarefa não termina corretamente.

## <a name="system-policies-for-background-audio-task-lifetime"></a>Políticas de sistema para o tempo de vida de tarefas de áudio em segundo plano

As políticas a seguir ajudam a determinar como o sistema gerencia o tempo de vida de tarefas de áudio em segundo plano.

### <a name="exclusivity"></a>Exclusividade

Se habilitada, essa subpolítica limita o número de tarefas de áudio em segundo plano para no máximo 1, a qualquer momento. Ela está habilitada em SKUs de dispositivos móveis e outras que não sejam de área de trabalho.

### <a name="inactivity-timeout"></a>Tempo limite de inatividade

Devido a restrições de recursos, o sistema poderá encerrar sua tarefa em segundo plano após um período de inatividade.

Uma tarefa em segundo plano será considerada "inativa" se ambas as seguintes condições forem atendidas:

-   O aplicativo em primeiro plano não está visível (ele está suspenso ou encerrado).

-   O media player em segundo plano não está no estado de execução.

Se ambas as condições forem atendidas, a política de sistema de mídia em segundo plano iniciará um temporizador. Se nenhuma condição mudar quando o temporizador expirar, a política de sistema de mídia em segundo plano encerrará a tarefa em segundo plano.

### <a name="shared-lifetime"></a>Tempo de vida compartilhado

Se habilitada, essa subpolítica força a tarefa em segundo plano a ser dependente do tempo de vida da tarefa em primeiro plano. Se a tarefa em primeiro plano for encerrada, pelo usuário ou pelo sistema, a tarefa em segundo plano também será encerrada.

No entanto, observe que isso não significa que o primeiro plano seja dependente do segundo plano. Se a tarefa em segundo plano for encerrada, isso não força o encerramento da tarefa em primeiro plano.

A tabela a seguir lista as políticas que são aplicadas em quais tipos de dispositivos.

| Subpolítica             | Área de trabalho  | Dispositivos móveis   | Outros    |
|------------------------|----------|----------|----------|
| **Exclusividade**        | Desabilitada | Habilitada  | Habilitada  |
| **Tempo limite de inatividade** | Desabilitada | Habilitada  | Desabilitada |
| **Tempo de vida compartilhado**    | Habilitada  | Desabilitada | Desabilitada |


 

 





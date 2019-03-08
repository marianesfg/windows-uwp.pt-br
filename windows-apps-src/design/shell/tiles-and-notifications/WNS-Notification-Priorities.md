---
title: Prioridades de notificação de WNS
description: Descrição das várias prioridades que podem ser definidas em uma notificação
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API do WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 2c297a04786c6fbf1eb0600e63a04a6d88585864
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648701"
---
# <a name="wns-notification-priorities"></a>Prioridades de notificação de WNS
Ao definir a prioridade de uma notificação com um cabeçalho simple como WNS postar mensagens, você pode controlar como as notificações são entregues em situações confidenciais de bateria.

## <a name="power-on-windows"></a>Ligar o Windows
Como mais usuários estão trabalhando somente em dispositivos alimentado por bateria, minimizar o consumo de energia tornou-se um requisito padrão para todos os aplicativos. Se aplicativos consomem mais energia do que o valor que eles fornecem, os usuários podem desinstalar os aplicativos. Enquanto o sistema operacional Windows reduz o consumo de energia da bateria sempre que possível, é responsabilidade do aplicativo para trabalhar com eficiência. 

As prioridades de WNS é uma maneira de mover o trabalho não críticas desativar a bateria. As prioridades WNS informam ao sistema quais notificações devem ser entregues imediatamente e que pode aguardar até que o dispositivo está conectado a uma fonte de alimentação. Com essas dicas, o sistema pode entregar as notificações a hora exata em que eles são mais importantes para o usuário e o aplicativo. 

## <a name="power-modes-on-the-device"></a>Modos de energia no dispositivo
Todos os dispositivos Windows funciona por meio de uma variedade de modos de energia (bateria, economia de bateria e carga) e os usuários esperam comportamentos diferentes de aplicativos em modos de energia diferente. Quando o dispositivo está ativado, todas as notificações devem ser entregues. No modo de proteção da bateria, apenas as notificações mais importante devem ser entregue. Enquanto o dispositivo está conectado, sincronização ou operações críticas de tempo não podem ser concluídas.

Windows não sabe quais notificações são importantes para qualquer usuário ou aplicativo, portanto, o sistema depende totalmente de aplicativos para definir a prioridade certa para suas notificações. 

## <a name="priorities"></a>Prioridades
Há quatro prioridades disponíveis para um aplicativo usar ao enviar notificações por push. A prioridade é definida em notificações individuais, permitindo que você escolha quais notificações precisam ser entregue instantaneamente (por exemplo, uma mensagem de mensagens Instantâneas) e quais delas podem esperar (por exemplo, entre em contato com as atualizações de fotos).

As prioridades são: 

|    Priority    |    Substituição do usuário    |    Descrição    |    Exemplo    |
|----------------|---------------------|-------------------|---------------|
|    Alto    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo que está sendo limitadas no modo de economia de bateria.    |    As notificações mais importantes que devem ser entregues imediatamente em nenhuma circunstância quando o dispositivo pode receber notificações. Coisas como chamadas VoIP ou alertas críticos que devem ativar o dispositivo se enquadram nessa categoria.    |    Chamadas VoIP, hora - alertas críticos    |
|    Médio    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo que está sendo limitadas no modo de economia de bateria.    |    Essas são coisas que não são tão importantes, coisas que não precisam acontecer imediatamente, mas os usuários seriam bom senso ficaria irritados se eles não estão em execução em segundo plano.    |    Sincronização de conta de Email secundária, atualizações de bloco dinâmico.    |
|    Baixo    |    Sim – o usuário pode bloquear todas as notificações de um aplicativo ou pode impedir que um aplicativo que está sendo limitadas no modo de economia de bateria.    |    Notificações que só fazem sentido quando o usuário está usando o dispositivo ou quando a atividade em segundo plano faz sentido. Esses são armazenados em cache e não processados até que o usuário se autentica em ou plugs no seu dispositivo.    |    Entre em contato com o status (online/offline)    |
|    Muito baixo     |    Não – ele não pode impedir que as notificações de prioridade muito baixa do que está sendo limitadas no modo de economia de bateria.    |    Isso é quase o mesmo como baixa prioridade, exceto que os usuários não é possível substituir a política de proteção da bateria. Essas notificações nunca serão entregue em economia de bateria.    |    Sincronização de arquivos para um serviço de sincronização.    |

Observe que muitos aplicativos terão as notificações de prioridade diferente em todo seu ciclo de vida. Uma vez que a prioridade é definida em uma base por notificação, isso não é um problema. Um aplicativo de VoIP pode enviar uma notificação de alta prioridade para uma chamada de entrada e, em seguida, acompanhamento-lo com uma prioridade baixa um quando um contato online. 

## <a name="setting-the-priority"></a>Definir a prioridade

Definir a prioridade na solicitação de notificação é feito por meio de um cabeçalho adicional na solicitação POST, `X-WNS-PRIORITY`. Isso é um valor inteiro entre 0 e 3, que é mapeada para uma prioridade: 

| Nome da prioridade | Valor de X-WNS-prioridade | Padrão para: |
|---------------|----------------------|------------------|
| Alto | 1 | Notificações do sistema |
| Meduim | 2 | Blocos e notificações |
| Baixo | 3 | Bruta |
| Muito baixo | 4 |  |

Para ser com versões anteriores compatíveis, definir uma prioridade não é necessária. No caso de um aplicativo não define a prioridade de suas notificações, o sistema fornecerá uma prioridade padrão. Os padrões são mostrados no gráfico acima e corresponder ao comportamento das versões existentes do Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Listagem detalhada do comportamento da área de trabalho 

Se você estiver enviando seu aplicativo em várias SKUs diferentes do Windows, é normalmente é melhor seguir o gráfico na seção acima. 

Comportamentos de recomendada mais específicos para cada prioridade estão listados abaixo. Isso não é uma garantia de que cada dispositivo funcionará exatamente de acordo com o gráfico. OEMs são livres para configurar o comportamento de forma diferente, mas a maioria está próximas este gráfico. 

| Status do dispositivo    | PRIORIDADE: Alto    |    PRIORIDADE: Médio        | PRIORIDADE: Baixo    |    PRIORIDADE: Muito baixo    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Tela ou na tomada    |    Entregar    |    Entregar    |    Entregar    |    Entregar    |
|    Tela dentro e fora da bateria    |    Entregar    |    Se o usuário isento: entregar Else: em lotes     |    Se o usuário isento: entregar Else: cache *    |    Cache    |
|    Economia de bateria habilitado    |    Se o usuário isento: entregar Else: cache    |    Se o usuário isento: entregar Else: cache    |    Se o usuário isento: entregar Else: cache    |    Cache     |
|    Na bateria + economia de bateria habilitado + tela    |    Se o usuário isento: entregar Else: cache    |    Se o usuário isento: entregar Else: cache    |    Se o usuário isento: entregar Else: cache    |    Cache    |

Observe que notificações de baixa prioridade serão entregues por padrão para a tela off e bateria somente para Windows Phone com base em dispositivos. Isso é para compatibilidade maintian com a política preexistente do MPNS. Observe também que a quarta e quinta linhas são os mesmos, basta chamar diferentes cenários.

Para isolar um aplicativo em economia de bateria, os usuários devem ir para o "bateria uso pelo aplicativo" nas configurações e selecione "Permitir que o aplicativo para executar tarefas em segundo plano." Essa seleção de usuário isenta o aplicativo de economia de bateria para alto, médio e notificações de baixa prioridade. Você também pode chamar [BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) programaticamente solicitará a permissão do usuário.  

## <a name="related-topics"></a>Tópicos relacionados
- [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Solicitando permissão para executar em segundo plano](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
- 

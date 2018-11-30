---
Description: Periodic notifications, which are also called polled notifications, update tiles and badges at a fixed interval by downloading content from a cloud service.
title: Visão geral de notificações periódicas
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f7a5054fde1a1a24945b193f578b8389519dc2d5
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8217464"
---
# <a name="periodic-notification-overview"></a>Visão geral de notificações periódicas
 


Notificações periódicas, que são chamadas também de notificações de sondagem, atualizam blocos e selos em um intervalo fixo, baixando o conteúdo da nuvem. Para usar notificações periódicas, seu código de aplicativo cliente precisa fornecer duas partes de informações:

-   O URI (Uniform Resource Identifier) de um local na Web para o Windows sondar por atualizações de bloco ou notificação para o seu aplicativo
-   A frequência com que o URI deve ser sondado

As notificações periódicas permitem que seu aplicativo obtenha atualizações de bloco em tempo real com serviço de nuvem mínimo e pouco investimento do cliente. As notificações periódicas são um método de entrega excelente para distribuir o mesmo conteúdo a uma ampla audiência.

**Observação**  você pode saber mais baixando a [amostra de notificações periódicas e por Push](http://go.microsoft.com/fwlink/p/?linkid=231476) para Windows 8.1 e reutilizando seu código-fonte em seu aplicativo do Windows 10.

 

## <a name="how-it-works"></a>Como funciona


As notificações periódicas requerem que seu aplicativo hospede um serviço de nuvem. O serviço será sondado periodicamente por todos os usuários que tiverem o aplicativo instalado. A cada intervalo de sondagem, como uma vez por hora, o Windows envia uma solicitação HTTP GET para o URI, baixa o conteúdo da notificação ou do bloco solicitado (como XML), que é fornecido em resposta à solicitação, e exibe o conteúdo no bloco do aplicativo.

Observe que as atualizações periódicas não podem ser usadas com notificações do sistema. As notificações do sistema funcionam melhor por meio de notificações [programadas](https://msdn.microsoft.com/library/windows/apps/hh465417) ou [por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <a name="uri-location-and-xml-content"></a>Local do URI e conteúdo XML


Qualquer endereço HTTP ou HTTPS válido pode ser usado como o URI a ser sondado.

A resposta do servidor de nuvem inclui o conteúdo baixado. O conteúdo retornado do URI deve estar em conformidade com a especificação de esquema XML do [Bloco](adaptive-tiles-schema.md) ou da [Notificação](https://msdn.microsoft.com/library/windows/apps/br212851) e deve estar codificado em UTF-8. Você pode usar cabeçalhos HTTP definidos para especificar o [tempo de expiração](#expiration-of-tile-and-badge-notifications) ou a marca da notificação.

## <a name="polling-behavior"></a>Comportamento de sondagem


Chame um destes métodos para começar a sondagem:

-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Bloco)
-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Selo)
-   [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Bloco)

Quando você chama um destes métodos, o URI é imediatamente sondado e o bloco ou a notificação será atualizada com o conteúdo recebido. Após essa sondagem inicial, o Windows continua fornecendo as atualizações no intervalo solicitado. A sondagem continua até você parar explicitamente (com [**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)), seu aplicativo ser desinstalado ou, no caso de um bloco secundário, o bloco ser removido. Caso contrário, o Windows continua a sondar atualizações para seu bloco ou notificação mesmo que seu aplicativo não seja mais iniciado.

### <a name="the-recurrence-interval"></a>O intervalo de recorrência

Especifique o intervalo de recorrência como um parâmetro dos métodos listados acima. Observe que embora o Windows se esforce para sondar conforme solicitado, o intervalo não é preciso. O intervalo de sondagem solicitado pode ser atrasado em até 15 minutos, a critério do Windows.

### <a name="the-start-time"></a>O horário de início

Você também pode especificar um determinado horário do dia para começar uma sondagem. Considere um aplicativo que altera o conteúdo do bloco uma vez ao dia. Nesse caso, nós recomendamos que você faça sondagens perto do horário da atualização do seu serviço de nuvem. Por exemplo, se um site de compras diário publica as ofertas do dia às 08:00, sonde o novo conteúdo do bloco logo após às 08:00.

Se você fornecer o horário de início, a primeira chamada para o método fará a sondagem do conteúdo imediatamente. Então, a sondagem regular será iniciada dentro de 15 minutos após o horário de início fornecido.

### <a name="automatic-retry-behavior"></a>Comportamento de repetição automática

O URI é sondado somente se o dispositivo estiver online. Se a rede estiver disponível, mas o URI não puder ser contatado por algum motivo, essa iteração do intervalo de sondagem é ignorada e o URI será sondado novamente no próximo intervalo. Se o dispositivo está em estado desligado, suspenso ou em hibernação quando um intervalo de sondagem é alcançado, o URI é sondado quando o dispositivo retorna do estado desligado ou suspenso.

### <a name="handling-app-updates"></a>Tratamento de atualizações de aplicativos

Se você lançar uma atualização de aplicativo que altere seu URI de sondagem, deverá adicionar uma [tarefa em segundo plano de gatilho de tempo](../../../launch-resume/run-a-background-task-on-a-timer-.md) diária que chame StartPeriodicUpdate com o novo URI para garantir que os blocos usem o novo URI. Caso contrário, se os usuários receberem a atualização do aplicativo, mas não iniciarem o aplicativo, seus blocos ainda usarão o URI antigo, que pode não ser exibido se o URI for inválido ou se o conteúdo retornado referenciar imagens locais que não existem mais.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiração de notificações de selo e bloco


Por padrão, as notificações periódicas de bloco e de selo expiram três dias depois que são baixadas. Quando uma notificação expira, o conteúdo é removido do selo, do bloco ou da fila e não é mais mostrado para o usuário. É recomendável definir um horário de expiração explícito em todas as notificações de bloco e de selo periódicas usando um tempo que faça sentido para o aplicativo ou notificação, para garantir que o conteúdo do bloco não continue além do tempo relevante. Um tempo de expiração explícito é essencial para conteúdo com tempo de vida definido. Isso também garante a remoção de conteúdo obsoleto se seu serviço de nuvem se tornar inacessível ou se o usuário se desconectar da rede por um período de tempo prolongado.

Seu serviço de nuvem configura uma data de expiração e hora para uma notificação incluindo o cabeçalho HTTP X-WNS-Expire na carga da resposta. O cabeçalho HTTP X-WNS-Expires está em conformidade com o [formato de data HTTP](http://go.microsoft.com/fwlink/p/?linkid=253706). Para saber mais, veja [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) ou [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_).

Por exemplo, durante um dia de negociação ativo do mercado de ações, você pode definir a expiração de uma atualização de preços de ações para duas vezes mais do que seu intervalo de sondagem (por exemplo, uma hora após o recebimento, se estiver sondando a cada meia hora). Outro exemplo é um aplicativo de notícias que pode determinar que um dia é um período de expiração adequado para a atualização de blocos de notícias diárias.

## <a name="periodic-notifications-in-the-notification-queue"></a>Notificações periódicas na fila de notificações


Você pode usar atualizações de bloco periódicas com [ciclos de notificações](https://msdn.microsoft.com/library/windows/apps/hh781199). Por padrão, um bloco na tela inicial mostra o conteúdo de uma única notificação até que ela seja substituída por uma nova. Ao habilitar os ciclos de notificações, até cinco notificações são mantidas na fila e os bloco alterna entre elas.

Se a fila tiver atingido a capacidade de cinco notificações, a próxima notificação nova substituirá a mais antiga na fila. Porém, definindo marcas em suas notificações, você pode influenciar a política de substituição da fila. Uma marca é uma cadeia de caracteres de até 16 caracteres alfanuméricos específica do aplicativo e que não diferencia maiúsculas de minúsculas, especificada no cabeçalho HTTP [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) na carga da resposta. O Windows compara a marca de uma notificação de entrada com as marcas de todas as notificações que já estão na fila. Se for encontrada uma correspondência, a nova notificação substituirá a notificação da fila com a mesma marca. Se nenhuma correspondência for encontrada, a regra de substituição padrão será aplicada e a nova notificação substituirá a antiga na fila.

Você pode usar a marcação e a fila de notificações para implementar diversos cenários avançados de notificação. Por exemplo, um aplicativo de ações pode enviar cinco notificações, cada uma sobre uma ação diferente e marcada com um nome de ação. Isso evita que a fila contenha duas notificações para a mesma ação, uma delas mais antiga e desatualizada.

Para saber mais, veja [Usando a fila de notificações](https://msdn.microsoft.com/library/windows/apps/hh781199).

### <a name="enabling-the-notification-queue"></a>Habilitando a fila de notificações

Para implementar uma fila de notificações, primeiro habilite a fila para seu bloco (consulte [Como usar a fila de notificações com notificações locais](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/)). A chamada para habilitar a fila precisa ser feita apenas uma vez durante a vida útil do seu aplicativo, mas não tem problema fazer isso cada vez que seu aplicativo é iniciado.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>Sondando mais de uma notificação por vez

Você deve fornecer um URI exclusivo para cada notificação que quiser que o Windows baixe para o bloco. Ao usar o método [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_), você pode fornecer até cinco URIs por vez para usar com a fila de notificações. Cada URI é sondado para uma carga de notificação única, no mesmo tempo ou perto dele. Cada URI sondado pode retornar o próprio valor de expiração e marca.

## <a name="related-topics"></a>Tópicos relacionados


* [Diretrizes para notificações periódicas](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [Como configurar notificações periódicas para selos](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [Como configurar notificações periódicas para blocos](https://msdn.microsoft.com/library/windows/apps/hh761476)
 

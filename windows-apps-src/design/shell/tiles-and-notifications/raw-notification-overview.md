---
Description: Raw notifications are short, general purpose push notifications.
title: Visão geral de notificações brutas
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad00090fdfc3ce7be34ef6271d16e76541b584bb
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7844355"
---
# <a name="raw-notification-overview"></a>Visão geral de notificações brutas


As notificações brutas são notificações por push curtas com finalidade geral. Elas são estritamente para instrução e não incluem componente de interface do usuário. Assim como nas outras notificações por push, o recurso WNS (Serviços de Notificação por Push do Windows) envia notificações brutas de seu serviço em nuvem para o aplicativo.

Você pode usar as notificações brutas para diversas finalidades, inclusive para disparar seu aplicativo para executar uma tarefa em segundo plano, se o usuário tiver permissão para isso no aplicativo. Ao usar o WNS para se comunicar com o aplicativo, você evita a sobrecarga de processamento na hora de criar conexões de soquete persistentes, enviando mensagens HTTP GET, e outras conexões de serviço para aplicativo.

> [!IMPORTANT]
> Para entender as notificações brutas, é melhor estar familiarizado com os conceitos abordados na [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md).

 

Assim como nas notificações por push do sistema e de bloco, a notificação bruta é enviada por push do serviço em nuvem de seu aplicativo através de um URI (Uniform Resource Identifier) de canal atribuído para o WNS. O WNS, por sua vez, entrega a notificação ao dispositivo e à conta de usuário associada a esse canal. Diferentemente das outras notificações por push, as notificações brutas não têm um formato especificado. O conteúdo da carga é totalmente definido pelo aplicativo.

Para ilustrar um aplicativo que pode se beneficiar usando as notificações brutas, vamos ver um aplicativo teórico de colaboração de documentos. Imagine dois usuários editando o mesmo documento simultaneamente. O serviço em nuvem, que hospeda o documento compartilhado, pode usar as notificações brutas para notificar cada usuário quando o outro fizer alterações. As notificações brutas não incluem necessariamente as alterações feitas no documento, mas sinalizam a cópia de cada usuário do aplicativo para que eles acessem o local central e sincronizem as alterações disponíveis. Usando as notificações brutas, o aplicativo e seu serviço em nuvem evitam a sobrecarga de manutenção das conexões persistentes durante todo o tempo em que o documento fica aberto.

## <a name="how-raw-notifications-work"></a>Como funcionam as notificações brutas


Todas as notificações brutas são notificações por push. Portanto, a configuração necessária para enviar e receber notificações por push vale também para as notificações brutas:

-   Você tem que ter um canal de WNS válido para enviar notificações brutas. Para saber mais sobre como obter um canal de notificações por push, consulte [Como solicitar, criar e salvar um canal de notificação](https://msdn.microsoft.com/library/windows/apps/hh465412).
-   Inclua a funcionalidade de **Internet** no manifesto do seu aplicativo. No editor de manifesto do Microsoft Visual Studio, você encontrará essa opção na guia **Recursos** como **Internet (Cliente)**. Para saber mais, consulte [**Recursos**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities).

O corpo da notificação está em um formato definido pelo aplicativo. O cliente recebe os dados como uma cadeia de caracteres terminada em nulo (**HSTRING**) que só precisa ser reconhecida pelo aplicativo.

Se o cliente estiver offline, as notificações brutas serão armazenadas em cache pelo WNS apenas se o cabeçalho [X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache) for incluído na notificação. Porém, apenas uma notificação bruta será armazenada em cache e enviada assim que o dispositivo ficar online novamente.

Existem apenas três caminhos possíveis para uma notificação bruta chegar até o cliente: ela é enviada a seu aplicativo através de um evento de entrega de notificação, enviada para uma tarefa em segundo plano ou removida. Portanto, se o cliente estiver offline e o WNS tentar enviar uma notificação bruta, a notificação será removida.

## <a name="creating-a-raw-notification"></a>Criando uma notificação bruta


Veja a seguir as diferenças entre enviar uma notificação bruta e uma notificação do sistema por push de bloco, de selo ou do sistema:

-   O cabeçalho do tipo de conteúdo HTTP tem que ser definido como "application/octet-stream".
-   O cabeçalho HTTP [X-WNS-Type](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_type) tem que ser definido como "wns/raw".
-   O corpo da notificação pode incluir qualquer carga de cadeia de caracteres menor que 5 KB.

As notificações brutas são destinadas para uso como mensagens curtas que disparam o aplicativo para executar uma ação, como entrar em contato diretamente com o serviço para sincronizar uma quantidade maior de dados ou modificar o estado local com base no conteúdo da notificação. Observe que as notificações por push do WNS não têm garantia de entrega, por isso seu aplicativo e serviço em nuvem devem considerar a possibilidade de a notificação bruta não chegar até o cliente, por exemplo, quando o cliente está offline.

Para saber mais sobre como enviar notificações por push, veja [Guia de início rápido: enviando uma notificação por push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <a name="receiving-a-raw-notification"></a>Recebendo uma notificação bruta


Existem duas formas de seu aplicativo receber notificações brutas:

-   Através de [eventos de entrega de notificação](#notification-delivery-events) durante a execução do aplicativo.
-   Através de [tarefas em segundo plano disparadas pela notificação bruta](#background-tasks-triggered-by-raw-notifications) quando o aplicativo está habilitado a executar tarefas em segundo plano.

O aplicativo pode usar os dois mecanismos para receber notificações brutas. Quando o aplicativo implementa tanto o manipulador de eventos de entrega de notificação quanto as tarefas em segundo plano que são disparadas pelas notificações brutas, o evento de entrega de notificação tem prioridade quando o aplicativo está em execução.

-   Quando o aplicativo está em execução, o evento de entrega de notificação tem prioridade sobre a tarefa em segundo plano, e o aplicativo processa a notificação na primeira oportunidade.
-   O manipulador de eventos de entrega de notificação pode especificar, definindo a propriedade do evento [**PushNotificationReceivedEventArgs.Cancel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) como **true**, que a notificação bruta não seja passada para sua tarefa em segundo plano depois que o manipulador for encerrado. Se a propriedade **Cancel** for definida como **false** ou não for definida (o valor padrão é **false**), a notificação bruta vai disparar a tarefa em segundo plano depois que o manipulador de eventos de entrega de notificação tiver concluído seu trabalho.

### <a name="notification-delivery-events"></a>Eventos de entrega de notificação

Seu aplicativo pode usar um evento de entrega de notificação ([**PushNotificationReceived**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) para receber notificações brutas durante o uso do aplicativo. Quando o serviço em nuvem envia uma notificação bruta, o aplicativo em execução pode recebê-la manipulando o evento de entrega de notificação pelo URI de canal.

Se o aplicativo não estiver em execução e não usar [tarefas em segundo plano](#background-tasks-triggered-by-raw-notifications), nenhuma notificação bruta enviada a ele será removida pelo WNS no recebimento. Para evitar desperdício de recursos de seu serviço em nuvem, convém implementar a lógica no serviço para acompanhar se o aplicativo está ativo. Há duas fontes para essas informações: um aplicativo pode informar claramente ao serviço que ele está pronto para começar a receber notificações, e o WNS pode informar ao serviço quando parar.

-   **O aplicativo notifica o serviço em nuvem**: o aplicativo entra em contato com seu serviço para informá-lo de que o aplicativo está sendo executado em primeiro plano. A desvantagem dessa abordagem é que o aplicativo pode acabar contatando o serviço com muita frequência. Mas a vantagem é que o serviço sempre vai saber quando o aplicativo está pronto para receber as notificações brutas de entrada. Outra vantagem é que quando o aplicativo entra em contato com o serviço, o serviço sabe quando enviar notificações brutas para a instância específica do aplicativo, em vez de difundir.
-   **O serviço de nuvem responde a mensagens de resposta do WNS** : o serviço de seu aplicativo pode usar as informações de [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) e [X-WNS-DeviceConnectionStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_dcs) retornadas pelo WNS para determinar quando parar de enviar notificações brutas ao aplicativo. Quando o serviço envia uma notificação para um canal como HTTP POST, ele pode receber uma das seguintes mensagens na resposta:

    -   **X-WNS-NotificationStatus: dropped**: indica que a notificação não foi recebida pelo cliente. É seguro afirmar que a resposta **dropped** ocorreu porque seu aplicativo não está mais em primeiro plano no dispositivo do usuário.
    -   **X-WNS-DeviceConnectionStatus: disconnected** ou **X-WNS-DeviceConnectionStatus: tempconnected**: indica que o cliente do Windows não está mais conectado ao WNS. Para receber essa mensagem do WNS, você tem que solicitá-la definindo o cabeçalho [X-WNS-RequestForStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_request) no HTTP POST da notificação.

    O serviço em nuvem de seu aplicativo pode usar as informações nessas mensagens de status para interromper as tentativas de comunicação através das notificações brutas. O serviço pode retomar o envio das notificações brutas assim que for contatado pelo aplicativo, quando ele voltar ao primeiro plano.

    Não confie no [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) para determinar se a notificação foi enviada com sucesso para o cliente.

    Para saber mais, consulte [Cabeçalhos de solicitação e resposta do serviço de notificação por push](https://msdn.microsoft.com/library/windows/apps/hh465435)

### <a name="background-tasks-triggered-by-raw-notifications"></a>Tarefas em segundo plano disparadas pelas notificações brutas

> [!IMPORTANT]
> Antes de usar tarefas em segundo plano de notificação bruta, um app deve ter acesso ao segundo plano via [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).

 

A tarefa em segundo plano tem que ser registrada com um [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger). Se não for registrada, a tarefa não será executada quando a notificação bruta for recebida.

A tarefa em segundo plano disparada por uma notificação bruta permite que o serviço em nuvem de seu aplicativo entre em contato com o aplicativo, mesmo quando ele não estiver em execução (ele pode dispará-lo para ser executado). Isso acontece sem que o aplicativo tenha que manter uma conexão contínua. As notificações brutas são o único tipo de notificação capaz de disparar tarefas em segundo plano. Entretanto, embora as notificações por push do sistema e de bloco não possam disparar tarefas em segundo plano, as tarefas em segundo plano disparadas pelas notificações brutas podem atualizar blocos e invocar notificações do sistema através de chamadas locais à API.

Para ilustrar como funcionam as tarefas em segundo plano disparadas por notificações brutas, vamos imaginar um aplicativo usado para ler livros eletrônicos. Primeiro, o usuário compra um livro online, possivelmente em outro dispositivo. Como resposta, o serviço em nuvem do aplicativo envia uma notificação bruta para cada um dos dispositivos do usuário, com uma carga informando que o livro foi comprado e que o aplicativo precisa baixá-lo. O aplicativo entra em contato diretamente com o serviço em nuvem do aplicativo para começar a transferência em segundo plano do novo livro para que mais tarde, quando o usuário iniciar o aplicativo, o livro já esteja lá, pronto para a leitura.

Para usar uma notificação bruta para acionar uma tarefa em segundo plano, o aplicativo deve:

1.  Solicitar permissão para executar tarefas em segundo plano (que o usuário pode revogar a qualquer momento) usando [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).
2.  Implementar a tarefa em segundo plano. Para obter mais informações, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](../../../launch-resume/support-your-app-with-background-tasks.md)

A tarefa em segundo plano é invocada como resposta a [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) sempre que uma notificação bruta é recebida em seu aplicativo. A tarefa em segundo plano interpreta a carga específica ao aplicativo da notificação bruta e responde de acordo.

Para cada aplicativo, apenas uma tarefa em segundo plano pode ser executada de cada vez. Se a tarefa em segundo plano for disparada em um aplicativo que já tenha outra tarefa em segundo plano em execução, a primeira tarefa em segundo plano tem que ser concluída antes da execução da outra.

## <a name="other-resources"></a>Outros recursos


Você pode saber mais baixando a [amostra de notificações brutas](http://go.microsoft.com/fwlink/p/?linkid=241553) para Windows 8.1 e a [amostra de notificações periódicas e por Push](http://go.microsoft.com/fwlink/p/?LinkId=231476) para Windows 8.1 e reutilizando o código-fonte em seu aplicativo do Windows 10.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para notificações brutas](https://msdn.microsoft.com/library/windows/apps/hh761463)
* [Guia de início rápido: criando e registrando uma tarefa em segundo plano de notificação bruta](https://msdn.microsoft.com/library/windows/apps/jj676800)
* [Guia de início rápido: interceptando notificações por push nos aplicativos em execução](https://msdn.microsoft.com/library/windows/apps/jj709908)
* [**Notificação bruta**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 





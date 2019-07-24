---
Description: Saiba como criar notificações efetivas e focadas no usuário que tornam seus usuários produtivos e satisfeitos.
title: Diretrizes de UX do sistema
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP, notificação, coleção, grupo, UX, diretrizes de UX, orientação, ação, notificação de ação, centro de ações, notificações não interrupções, efetivas, notificações não intrusivas, acionável, gerenciar, organizar
ms.localizationpriority: medium
ms.openlocfilehash: 111ac9a216b87e120e42c9db7761bd8588548029
ms.sourcegitcommit: 720d8778d94ef44d2f86d2f1f0ebb05d6420805d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68415086"
---
# <a name="toast-notification-ux-guidance"></a>Diretrizes do UX de notificação do sistema
As notificações são uma parte necessária da vida moderna; Eles ajudam os usuários a serem mais produtivos e se envolver com aplicativos e sites, bem como manter-se atualizado com qualquer atualização. No entanto, as notificações podem ser rapidamente reativadas para sobretransmissão e invasiva se não forem projetadas de forma centrada no usuário. Suas notificações são um clique com o botão direito do mouse fora de serem desativadas e é improvável que elas sejam desativadas, elas serão ativadas novamente.  Portanto, certifique-se de que suas notificações sejam obedientes do espaço e do tempo da tela do usuário, para que você possa manter esse canal do Engagement aberto.

> **APIs importantes**: [Pacote NuGet de notificações do Windows Community Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Analisamos nossa telemetria do Windows, bem como outros estudos de caso de terceiros, para surgirem quatro regras em relação ao que faz uma boa história de notificação.  Estamos confiantes de que essas regras são aplicáveis universalmente, independentemente da plataforma, e ajudarão suas notificações a ter um impacto positivo sobre os usuários.

## <a name="1-actionable-notifications"></a>1. Notificações acionáveis
As notificações acionáveis permitem que os usuários sejam produtivos sem abrir seu aplicativo.  Embora seja ótimo ter inicializações de aplicativo, essa não é a única medida de sucesso, e permitir que os usuários realizem tarefas pequenas sem precisar entrar em seu aplicativo pode ser uma ferramenta muito poderosa para desiluminar os usuários.

![Notificação acionável com caixa de texto de entrada e botões para definir lembretes e responder à notificação](images/actionable-notification-example01.png)

Acima está um exemplo de uma notificação que utiliza ações. A sensação de concluir tarefas é uma sensação universalmente positiva, e você pode dar essa sensação ao seu aplicativo ou site enviando notificações que têm conteúdo acionável. As notificações acionáveis também podem ajudar a aumentar a produtividade, tanto em cenários empresariais quanto de consumidores, diminuindo o tempo de ação que os usuários passam para realizar essas tarefas menores. É recomendável incluir as ações que os usuários usam regularmente ou as coisas que você está tentando treinar pelos usuários.  Eis alguns exemplos:
* Conteúdo de preferência, adicionando, sinalização ou estrelando
* Aprovar ou negar relatórios de despesas, tempo limite, permissões etc.
* Resposta embutida para mensagens, emails, bate-papos de grupo, comentários etc.
* Concluindo pedidos usando [atualização pendente](toast-pending-update.md)
* Configurando alertas ou lembretes para outra hora, bem como tempo de reserva potencial em um calendário

As notificações acionáveis são uma ferramenta muito poderosa para ajudar os usuários a se sentirem produtivos, realizar tarefas e ter uma experiência ótima e eficiente com seu aplicativo ou site.  Há muitas oportunidades aqui! Se você quiser obter ajuda sobre ideias, fique à vontade para entrar na equipe de notificações do Windows.

## <a name="2-timing-and-urgency"></a>2. Tempo e urgência
Ao contrário de como pensamos nas notificações, o tempo real não é necessariamente o melhor! Incentivamos os desenvolvedores a pensar sobre o usuário e se a notificação que eles estão enviando são informações urgentes ou não. Os usuários podem facilmente ser sobrecarregados com muitas informações e ficar frustrados se estiverem sendo interrompidos enquanto estiverem tentando se concentrar. O Windows fornece algumas opções de como considerar a invasividade das notificações que você está enviando:

**Notificações brutas:** O uso de [notificações brutas](raw-notification-overview.md) pode ser benéfico por vários motivos, especialmente quando se trata de minimizar interrupções para o usuário.  O envio de notificações brutas ativará seu aplicativo em segundo plano, para que você possa avaliar se a notificação faz sentido fornecer imediatamente no contexto do aplicativo. Se for algo que você julgar que deve ser mostrado ao usuário imediatamente, você poderá exibir um [notificação local](send-local-toast.md) a partir daí.  Se for algo que o usuário não precisa ver no momento, você poderá criar um [sistema de notificação agendado](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que será acionado posteriormente.


Notificação de **fantasma:** você também pode disparar um aviso que irá ignorar o pop-up no canto inferior direito da tela e, em vez disso, enviar a notificação diretamente para a central de ações. Isso é feito definindo a [Propriedade SupressPopup](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) como true. Embora possa haver algumas ceticismo em relação à não ativação de notificações fora da central de ações, vemos um compromisso de 2-3 vezes maior para os torradeiras que residem na central de ações sobre o sistema de notificação.  Os usuários são mais responsivos quando estão prontos para receber notificações e podem controlar quando são interrompidos, e é por isso que o conteúdo na central de ações pode ser muito mais eficaz para notificar os usuários de forma não invasiva.

## <a name="3-clear-out-the-clutter"></a>3. Limpar a desordem
As notificações podem persistir na central de ações por um tempo muito longo (padrão, três dias).  É imperativo que você certifique-se de que o conteúdo que reside aqui esteja atualizado e seja relevante sempre que o usuário abrir a central de ações. Você está desperdiçando o espaço da tela do usuário e ocupando slots que poderiam ser usados para algo mais atualizado.  Digamos que o usuário instale seu aplicativo de gerenciamento de email e receba dez emails e dez notificações junto com esses emails.  Dependendo da sua experiência desejada, você poderá considerar a limpeza dessas notificações se o usuário tiver lido o email correspondente ou aberto o aplicativo como uma maneira de remover a aglomeração antiga da central de ações.

Temos uma série de APIs [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) que permitem ver qual conteúdo está na central de ações, bem como gerenciar essas notificações. Seja obedientes do espaço da tela do usuário e tome cuidado para mostrar apenas o conteúdo relevante e atual aos usuários.

## <a name="4-keeping-organized"></a>4. Mantendo organizado
Conforme mencionado anteriormente, o conteúdo na central de ações persiste por três dias.  Para ajudar os usuários a escolherem as informações que estão procurando rapidamente, organize as notificações na central de ações usando [cabeçalhos](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou [coleções](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Você pode ver um exemplo de um cabeçalho abaixo.

![Exemplos do sistema com cabeçalhos rotulados como ' acampar!! '](images/toast-headers-action-center.png)

Agrupe essas notificações de forma que o conteúdo relevante permaneça em conjunto (ou seja, considere a separação de diferentes Leagues esportivos em um aplicativo esportivo ou a classificação de mensagens por chat de grupo). As coleções são uma maneira mais óbvia de agrupar notificações, enquanto os cabeçalhos são mais sutis, mas ambos permitem que os usuários façam a triagem e escolham notificações mais rapidamente.



Esses quatro pontos acima são diretrizes que encontramos em vigor por meio de nossa própria análise de telemetria e por meio de experimentos de primeira e de terceiros. No entanto, tenha em mente que essas diretrizes são apenas: diretrizes.  Estamos confiantes de que essas regras ajudarão a aumentar o envolvimento e a produtividade de suas notificações, mas nada poderá substituir a reflexão centrada no usuário e aprender com seus próprios dados.  

## <a name="related-topics"></a>Tópicos relacionados

* [Conteúdo do sistema](adaptive-interactive-toasts.md)
* [Notificações brutas](raw-notification-overview.md)
* [Atualização pendente](toast-pending-update.md)
* [Biblioteca de notificações no GitHub (parte do kit de ferramentas da Comunidade do Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)

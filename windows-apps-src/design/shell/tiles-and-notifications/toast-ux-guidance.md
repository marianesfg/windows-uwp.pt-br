---
author: manoskow
Description: Learn how to create effective and user-focused notifications that make your users productive and happy.
title: Diretrizes de experiência do usuário de notificação do sistema
label: Toast UX Guidance
template: detail.hbs
ms.author: mijacobs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notificação, coleção, grupo, experiência do usuário, diretrizes de experiência do usuário, diretrizes, ação, notificação do sistema, Central de ações, noninterruptive, notificações efetivas, não intrusivos notificações, acionáveis, gerenciar, organizar
ms.localizationpriority: medium
ms.openlocfilehash: 4ac7eab73f2bcfa57ac37ea6da99e1da6b235159
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032697"
---
# <a name="toast-notification-ux-guidance"></a>Diretrizes de experiência do usuário de notificação do sistema
As notificações são uma parte fundamental da vida moderna; elas ajudam os usuários a ser mais produtivo e estabelecido com aplicativos e sites, bem como mantenha atualizado com as atualizações. No entanto, as notificações podem rapidamente ativem útil para opressivas e intrusivo se eles não são projetados de maneira centrada no usuário. As notificações são um botão direito do mouse para longe sendo desativado e é improvável depois que eles estão desativados, eles serão ativados novamente.  Portanto, verifique se que as notificações estejam respeita espaço na tela do usuário e a hora, para que você pode manter esse canal de envolvimento aberto.

> **APIs importantes**: [pacote nuget de notificações do Kit de ferramentas da comunidade do Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Já analisamos nossa telemetria do Windows, bem como outros primeira e estudos de caso de terceiros, para criar quatro regras em torno do que torna uma história de notificação excelente.  Estamos confiantes essas regras se aplicam universalmente, independentemente da plataforma e ajudarão seu notificaitons ter um impacto positivo sobre os usuários.

## <a name="1-actionable-notifications"></a>1. notificações acionáveis
Notificações acionáveis permitem que os usuários a serem produtivos sem abrir o aplicativo.  Enquanto ele é ótimo para que o aplicativo for iniciado, isso não é a única medida de sucesso e permitindo que os usuários entrem em realizar pequeno tarefas sem precisar ir ao seu aplicativo podem ser uma ferramenta poderosa muito satisfação dos seus usuários.

![Notificação acionável com caixa de entrada de texto e botões para definir lembretes e responder à notificação](images/actionable-notification-example01.png)

Acima é um exemplo de uma notificação que aproveita a ações. A sensação de tarefas acabamento é uma sensação universalmente positiva, e você pode colocar essa sensação ao seu aplicativo ou site enviando notificações que têm conteúdo acionável neles. Notificações acionáveis também podem ajudar a aumentar a produtividade, tanto em cenários corporativos e consumidor, diminuindo o tempo para usuários de ação percorra para realizar essas tarefas menores. É recomendável incluir ações que os usuários tomar regularmente ou coisas que você está tentando treinar os usuários fazer.  Veja a seguir alguns exemplos:
* Preferência, favoriting, sinalizar ou estrelando conteúdo
* Aprovar ou negar relatórios de despesas, tempo desligado, permissões, etc.
* Embutido respondendo a mensagens, emails, chats grupo, comentários, etc.
* Concluir ordens usando [atualização pendente](toast-pending-update.md)
* Definir lembretes ou alertas para outro momento, bem como reserva potencialmente hora no calendário

notificações de ctionable são uma ferramenta poderosa muito para ajudar os usuários a se sentir produtivo, realizar tarefas e ter uma experiência excelente e eficiente com seu aplicativo ou site.  Há muitas oportunidades aqui! Se você quiser ajuda debates ideias, fique à vontade para entrar em contato com a equipe de notificações do windows.  Você 

## <a name="2-timing-and-urgency"></a>2. tempo e urgência
Ao contrário das como consideramos geralmente sobre notificações, em tempo real não é necessariamente melhor! Solicitamos que os desenvolvedores pense sobre o usuário e se a notificação estão enviando é urgentes ou não. Os usuários podem ser facilmente sobrecarregados com muitas informações e frustrado se eles estão sendo interrompidos enquanto eles estão tentando foco. O Windows fornece algumas opções para como considerar o intrusiveness que você está enviando notificações:

**Notificações brutas:** Usar [as notificações brutas](raw-notification-overview.md) pode ser benéfico por várias razões, especialmente quando se trata ou minimizar as interrupções para o usuário.  Enviar notificações brutas serão despertar seu aplicativo em segundo plano, portanto, você pode avaliar se a notificação faz sentido para fornecer imediatamente no contexto do seu aplicativo. Se é algo sentir deve ser mostrado para o usuário imediatamente, você é capaz de exibir uma [notificação do sistema local](send-local-toast.md) a partir daí.  Se ele é algo que o usuário não precisa ver agora, você é capaz de criar uma [notificação do sistema agendada](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que será acionado em um momento posterior.

**Fantasma do sistema:** você também pode acionar uma notificação que ignorar estão aparecendo no canto inferior direito da tela e, em vez disso, enviar a notificação diretamente para a Central de ações. Isso é feito, definindo a [propriedade SuppressPopup](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) como "true". Embora possa haver alguns ceticismo em torno de não enviar notificações fora da Central de ações, podemos ver um 2-3 vezes maiores envolvimento para notificações do sistema que residem na Central de ações ao longo do sistema ser exibido.  Os usuários são mais responsivos quando eles estão prontos para receber notificaitons e podem controlar quando eles forem interrompidos, que é o motivo pelo qual o conteúdo na Central de ações pode ser muito mais eficiente para noninvasively notificar os usuários.

## <a name="3-clear-out-the-clutter"></a>3. clear-out a confusão
Notificações podem persistir na Central de ações para um tempo considerável (padrão três dias).  É fundamental que você se certifique de que o conteúdo que fica aqui é relevante e atualizado sempre que o usuário abrir a Central de ações. Você está ocupando espaço de tela do usuário e ocupando slots que podem ser usados para algo mais atualizado.  Digamos que o usuário instala o aplicativo de gerenciamento de e-mail e recebe dez emails e dez notificações juntamente com esses emails.  Dependendo de sua experiência desejada, você pode considerar limpar essas notificações se o usuário tem ler o email correspondente, ou abrir o aplicativo como uma forma de eliminar a desorganização antiga na Central de ações.

Podemos tem uma série de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) APIs que permitem que você veja o conteúdo que está na Central de ações, bem como gerenciar essas notificações. Respeitar os espaço na tela do usuário e tome cuidado que só estão mostrando conteúdo atual e relevante para os usuários.

## <a name="4-keeping-organized"></a>4. mantendo organizado
Como mencionado anteriormente, o conteúdo na Central de ações persistir por três dias.  Para ajudar os usuários a selecionar as informações que estão procurando rapidamente, organize as notificações na Central de ações usando [cabeçalhos](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou [coleções](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Você pode ver um exemplo de um cabeçalho abaixo.

![Exemplos de notificação do sistema com cabeçalhos rotulada 'No'!!](images/toast-headers-action-center.png)

Ambas essas notificações gorup de uma maneira para conteúdo relevante fique juntos (isto é, pense separando ligas esportes diferentes em um aplicativo de esportes ou classificando mensagens por bate-papo de grupo). Coleções são uma maneira mais óbvia para notificaitons de grupo, enquanto cabeçalhos são mais sutis, mas ambas permitem que os usuários façam triagem e escolha as notificações mais rapidamente. 

## <a name="other-resources"></a>Outros recursos
Esses quatro pontos acima são diretrizes que descobrimos eficientes através de nosso próprio análise de telemetria e primeiro e experimentos de terceiros. Tenha em mente, no entanto, que essas diretrizes são apenas: diretrizes.  Estamos confiantes essas regras ajudará a aumentar o envolvimento e a produtividade de suas notificações, mas nada pode substituir centrada no usuário pensando e aprendendo com seus próprios dados.  

Se você enviar notificações para seu aplicativo UWP hoje, você pode visualizar análises sobre o que aconteceu com suas notificações no [Centro de desenvolvimento](https://developer.microsoft.com/en-us/windows)! Esses dados vem de graça ao usar o [SDK de serviços de armazenamento](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) ou as [APIs de WNS](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Essas métricas lhe dará mais ideias que acontece com suas notificações na plataforma windows, bem como os usuários estão interagindo com notificações. Acessar esse painel indo para o menu no lado esquerdo envolver > notificações, em seguida, clicando na guia "Analisar" dentro da página de notificações.  Isso está localizado no mesmo lugar, que você deve ir para enviar notificações do portal do Centro de desenvolvimento.

## <a name="related-topics"></a>Tópicos relacionados

* [Conteúdo da notificação do sistema](adaptive-interactive-toasts.md)
* [Notificações brutas](raw-notification-overview.md)
* [Atualização pendente](toast-pending-update.md)
* [Biblioteca de notificações no GitHub (parte do Kit de ferramentas da comunidade do Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)

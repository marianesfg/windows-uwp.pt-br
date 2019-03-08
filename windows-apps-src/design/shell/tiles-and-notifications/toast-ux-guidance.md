---
Description: Saiba como criar notificações eficaz e focalizados nos usuários que fazem os usuários produtivos e felizes.
title: Diretrizes de experiência do usuário do sistema
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notificação, coleção, grupo, experiência do usuário, diretrizes de experiência do usuário, diretrizes, ação, do sistema, a Central de ações, noninterruptive, notificações efetivas, notificações não intrusivas, acionáveis, gerencie, organize
ms.localizationpriority: medium
ms.openlocfilehash: 878df85db9ab0e33db06a86ddb726f07dc28f013
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615761"
---
# <a name="toast-notification-ux-guidance"></a>Diretrizes de experiência do usuário de notificação do sistema
As notificações são uma parte necessária de vida moderna; eles ajudam os usuários sejam mais produtivos e envolvidos com aplicativos e sites, bem como mantenha-se atualizado com todas as atualizações. No entanto, as notificações podem se transformar rapidamente de útil opressivas e intrusiva se eles não forem criados de forma centrado no usuário. As notificações são um botão direito do mouse para longe de ser desligada e é improvável que depois que eles são desativados, elas serão ativadas novamente.  Portanto, certifique-se de que suas notificações são respeita de espaço na tela do usuário e a hora, para que você mantenha esse canal engagement aberto.

> **APIs importantes**: [Pacote do nuget de notificações de kit de ferramentas de comunidade do Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Já analisamos nossa telemetria do Windows, bem como outros primeiro e estudos de caso de terceiros, para surgir com quatro regras em torno do que torna uma história de notificação excelente.  Estamos confiantes essas regras sejam universalmente aplicáveis, independentemente da plataforma e o ajudará a suas notificações ter um impacto positivo nos seus usuários.

## <a name="1-actionable-notifications"></a>1. Notificações acionáveis
Notificações acionáveis permitem que os usuários sejam produtivos sem abrir seu aplicativo.  Embora seja ótimo ter o aplicativo inicia, isso não é a única medida de sucesso e permitindo que os usuários entrem realizam pequenas tarefas sem precisar entrar em seu aplicativo podem ser uma ferramenta muito poderosa em satisfazer seus usuários.

![Notificação acionável com botões para definir lembretes e responder à notificação e a caixa de texto de entrada](images/actionable-notification-example01.png)

Acima é um exemplo de uma notificação que aproveita as ações. A sensação de conclusão de tarefas é uma sensação todos positiva, e você pode colocar essa sensação ao seu aplicativo ou site enviando notificações que têm conteúdo acionável neles. Notificações acionáveis também podem ajudar a aumentar a produtividade, tanto em cenários corporativos e de consumidor, diminuindo o tempo para os usuários de ação percorrer para realizar essas tarefas menores. É recomendável incluindo ações que os usuários regularmente, tire ou coisas que você está tentando treinar os usuários a fazer.  Veja a seguir alguns exemplos:
* Gosto, adicionando, sinalizando ou estrelando conteúdo
* Relatórios de despesas de aprovação ou negação, folgas, permissões, etc.
* Embutido respondendo a mensagens, emails, grupo de bate-papos, comentários, etc.
* Concluindo ordena usando [atualização pendente](toast-pending-update.md)
* Definindo alertas ou lembretes para outro momento, bem como potencialmente reserva tempo em um calendário

Notificações acionáveis são uma ferramenta muito poderosa para ajudar os usuários a se sentir produtivo, realizar tarefas e tenham uma experiência excelente e eficiente com seu aplicativo ou site.  Há muitas oportunidades aqui! Se você quiser ajuda debate de ideias, fique à vontade para entrar em contato para a equipe de notificações do windows.

## <a name="2-timing-and-urgency"></a>2. Medição de tempo e a urgência
Ao contrário do como pensamos frequentemente sobre notificações, em tempo real não é necessariamente melhor! Recomendamos enfaticamente que os desenvolvedores pense sobre o usuário e se a notificação que estão enviando é urgentes informações ou não. Os usuários podem facilmente ser sobrecarregados com informações demais e ficam frustrados que estão sendo interrompidas enquanto eles estão tentando foco. Windows fornece algumas opções considerar a intrusão das notificações que você está enviando:

**Notificações brutas que:** Usando o [notificações brutas](raw-notification-overview.md) pode ser benéfico por vários motivos, especialmente quando se trata de ot minimizando interrupções para o usuário.  Envio de notificações brutas despertarão seu aplicativo em segundo plano, para que você possa avaliar se a notificação faz sentido para entregar imediatamente no contexto do seu aplicativo. Se for algo que você acha que deve ser mostrado ao usuário imediatamente, você é capaz de pop-um [toast local](send-local-toast.md) a partir daí.  Se ele é algo que o usuário não precisa ver agora, você é capaz de criar uma [agendadas do sistema](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que será acionado em um momento posterior.


**Toast de fantasmas:** também podem acionar uma notificação que ignorará a pop-up no canto inferior direito da tela e, em vez disso, enviar a notificação diretamente para a Central de ações. Isso é feito definindo a [SupressPopup propriedade](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) como True. Embora possa haver algum ceticismo em torno de popping não notificações fora do Centro de ações, podemos ver que um 2-3 vezes maior engagement para notificações do sistema que residem na Central de ações sobre removidos do sistema.  Os usuários são mais responsivos quando estiver prontos para receber notificações e pode controlar quando eles são interrompidos, que é o motivo pelo qual o conteúdo na Central de ações pode ser muito mais eficiente para noninvasively notificar os usuários.

## <a name="3-clear-out-the-clutter"></a>3. Desmarque out a desordem
Notificações podem persistir na Central de ações para um tempo considerável (padrão de três dias).  É imperativo que você verifique se o conteúdo que mora aqui é relevante e atualizado sempre que o usuário abre a Central de ações. Você está desperdiçando espaço de tela do usuário e ocupando slots que poderiam ser usados para algo mais atualizado.  Digamos que o usuário instala o aplicativo de gerenciamento de email e recebe dez emails e notificações de dez juntamente com os respectivos emails.  Dependendo de sua experiência desejada, você pode considerar limpar essas notificações se o usuário ler o email correspondente ou abriu o aplicativo como uma maneira de remover a confusão antiga da Central de ações.

Temos uma série de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) APIs que permitem que você veja qual conteúdo é na Central de ações, bem como gerenciar essas notificações. Respeitar os espaço na tela do usuário e tome cuidado que estão mostrando apenas conteúdo relevante e atual para os usuários.

## <a name="4-keeping-organized"></a>4. Manter organizado
Conforme mencionado anteriormente, o conteúdo na Central de ações persistir por três dias.  Para ajudar os usuários a escolher as informações que estão procurando rapidamente, organizar as notificações no centro da ação usando [cabeçalhos](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) ou [coleções](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Você pode ver um exemplo de um cabeçalho abaixo.

![Exemplos de notificação do sistema com cabeçalhos rotulada 'No'!!](images/toast-headers-action-center.png)

Agrupar essas notificações de uma maneira para que o conteúdo relevante permaneça em conjunto (ou seja, pense em separar leagues esportes diferentes em um aplicativo de esportes, ou classificando mensagens por bate-papo de grupo). Coleções são uma maneira mais óbvia para receber notificações de grupo, enquanto que os cabeçalhos são mais sutis, mas permitem que os usuários a triagem e escolher as notificações mais rapidamente.

## <a name="other-resources"></a>Outros recursos
Esses quatro pontos acima são diretrizes que encontramos em vigor por meio de nossa própria análise de telemetria e primeiro e testes de terceiros. Tenha em mente, no entanto, que essas diretrizes são apenas: diretrizes.  Estamos confiantes essas regras ajudará a aumentar o envolvimento e a produtividade de suas notificações, mas nada pode substituir centrado no usuário pensando e aprendendo com seus próprios dados.  

Se você enviar notificações para seu aplicativo UWP hoje em dia, você pode exibir a análise sobre o que aconteceu com as notificações na [Partner Center](https://partner.microsoft.com/dashboard)! Esses dados vem de graça ao usar o [SDK dos serviços de Store](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) ou o [WNS APIs](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Essas métricas lhe dará mais informações sobre o que acontece com as notificações na plataforma windows, bem como os usuários estão interagindo com as notificações. Acessar esses dados acessando o menu no lado esquerdo acionar > notificações, em seguida, clicando na guia "Analisar" dentro da página de notificações.  Ele está localizado no mesmo local que você iria para enviar notificações do Partner Center.

## <a name="related-topics"></a>Tópicos relacionados

* [Conteúdo de notificação do sistema](adaptive-interactive-toasts.md)
* [Notificações brutas](raw-notification-overview.md)
* [Atualização pendente](toast-pending-update.md)
* [Biblioteca de notificações no GitHub (parte do Kit de ferramentas de comunidade do Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)

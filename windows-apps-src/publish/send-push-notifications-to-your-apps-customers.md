---
author: JnHs
Description: Learn how to send notifications from Windows Dev Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: Enviar notificações por push direcionadas para clientes do seu app
ms.author: wdg-dev-content
ms.date: 02/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notificações direcionadas, notificações por push, notificações do sistema, bloco
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: high
ms.openlocfilehash: f7b4558b6c5ea2cc9dbc30cb605f1cb06432504f
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
---
# <a name="send-notifications-to-your-apps-customers"></a>Enviar notificações para clientes do seu aplicativo

Interagir com seus clientes no momento certo e com a mensagem certa é essencial para o sucesso como um desenvolvedor de aplicativos. As notificações podem incentivar os clientes a executar uma ação, como classificar um aplicativo, comprar um complemento, experimentar um novo recurso ou baixar outro aplicativo (talvez gratuitamente com um [código promocional](generate-promotional-codes.md) fornecido por você).

O Centro de Desenvolvimento do Windows fornece uma plataforma de interação com o cliente controlada por dados que pode ser usada para enviar notificações para todos os clientes do seu app ou direcionadas apenas a um subconjunto dos clientes do Windows 10 do seu app que atendem aos critérios que você definiu em um [segmento do cliente](create-customer-segments.md). <!-- You can also send a single notification to all of the customers for multiple apps. -->

> [!IMPORTANT]
> Essas notificações podem ser usadas somente com aplicativos UWP.

Ao considerar o conteúdo de suas notificações, tenha em mente:
- O conteúdo em suas notificações deve estar de acordo com as [Políticas de Conteúdo da Loja](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies).
- Seu conteúdo da notificação não deve incluir informações confidenciais ou potencialmente sensíveis.
- Embora nos esforçamos para entregar a notificação conforme agendado, pode haver, ocasionalmente, problemas de latência que impactam a entrega.
- Certifique-se de não enviar notificações muitas vezes. Mais de uma vez a cada 30 minutos pode parecer intrusivo (e para muitos cenários, menos frequência do que isso é preferível).
- Saiba que, se um cliente que usa seu aplicativo (e está atribuído a sua conta da Microsoft no momento em que a associação de segmento é determinada), e o usuário da conta fornece seu dispositivo para outra pessoa usar, a outra pessoa pode ver a notificação que era para o cliente original. Para obter mais informações, consulte [Configurar seu aplicativo para notificações por push direcionada](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
<!-- - If you send the same notification to customers of multiple apps, you can't target a segment; the notification will be sent to all customers for the apps you select. -->


## <a name="getting-started-with-notifications"></a>Introdução às notificações

Em um nível superior, você precisa executar três ações para usar notificações e interagir com seus clientes.

1. **Registre seu aplicativo para receber notificações por push.** Você pode fazer isso adicionando uma referência ao Microsoft Store Services SDK em seu aplicativo e, em seguida, adicionando algumas linhas de código para registrar um canal de notificação entre o Centro de Desenvolvimento e seu aplicativo. Usaremos este canal para disponibilizar as notificações para seus clientes. Para obter mais detalhes, consulte [Configurar seu aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Decida quais serão os clientes de destino.** Você pode enviar a notificação para todos os clientes do seu app, ou (para notificações criadas para um único app) para um grupo de clientes chamado um *segmento*, que você pode definir com base em critérios demográficos ou de receita. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md). 
3. **Crie o conteúdo da notificação e o envie.** Por exemplo, você pode criar uma notificação que incentive os novos clientes a classificar seu app, ou pode enviar uma notificação que promova uma oferta especial para adquirir um complemento.


## <a name="to-create-and-send-a-notification"></a>Para criar e enviar uma notificação

Siga estas etapas para criar uma notificação no painel e enviá-la para um segmento de cliente específico.

> [!NOTE]
> Para que um app possa receber notificações do Centro de Desenvolvimento, primeiro você deverá chamar o método [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) no app a fim de registrá-lo para receber notificações. Esse método está disponível no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Para obter mais informações sobre como chamar esse método, incluindo um exemplo de código, consulte [Configurar o aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1.  No [painel do Centro de Desenvolvimento do Windows](https://developer.microsoft.com/dashboard/overview), expanda a seção **Interagir** e, em seguida, selecione **Notificações**.
2.  Na página **Notificações**, selecione **Nova notificação**.
3.  Na seção **Selecionar um modelo**, escolha o tipo de notificação que deseja enviar. Para obter detalhes, consulte [Tipos de modelo de notificação](#notification-template-types). <!-- ![Notification templates](images/push-notifications-template.png) -->
4.      Na próxima página, <!-- use the drop-down menu to choose either a **Single app** or **Multiple apps** for which you want to generate a notification. --> selecione um app <!-- or apps --> (que deve ser configurado para receber notificações usando o Microsoft Store Services SDK).
5.  Na seção **Configurações de notificação**, escolha um **Nome** para a sua notificação e, se aplicável, escolha o **Grupo de clientes** para o qual você deseja enviar a notificação. <!-- (Notifications sent to multiple apps can only be sent to all customers of those apps.) --> Se quiser usar um segmento que ainda não foi criado, selecione **Criar novo grupo de clientes**. Observe que serão necessárias 24 horas para que você possa usar um novo segmento para notificações. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md).
6.  Se você quiser especificar quando a notificação deverá ser enviada, desmarque a caixa de seleção **Enviar a notificação imediatamente** e escolha uma data e uma hora específicas (no horário UTC para todos os clientes, a menos que você especifique o uso do fuso horário local de cada cliente).
7.  Se você quiser que a notificação expire em algum momento, limpe a caixa de seleção **Notificação nunca expira** e escolha data e hora de expiração específicas (em UTC).
8.  <!-- For notifications to a single app: --> If you want to filter the recipients so that your notification is only delivered to people who use certain languages or are in specific time zones, check the **Use filters** checkbox. You can then specify the language and/or time zone options you want to use.
<!-- and don't forget to update numbers when this comes back... 9.      For notifications to multiple apps: Specify whether to send the notification only to the last active app on each device (per customer), or to all apps on each device. -->
9.  Na seção **Conteúdo da notificação**, no menu **Idioma**, escolha os idiomas em que você deseja que sua notificação seja exibida. Para obter mais informações, consulte [Traduzir suas notificações](#translate-your-notifications).
10. Na seção **Opções**, insira texto e configure as outras opções que desejar. Se você começou com um modelo, parte disso é fornecido por padrão, mas você pode fazer quaisquer alterações que desejar.
   As opções disponíveis variam, dependendo de qual tipo de notificação você está usando. Algumas das opções são:
   - **Tipo de ativação** (tipo de notificação do sistema interativa). Você pode escolher **Em primeiro plano**, **Em segundo plano**, ou **Protocolo**.
   - **Iniciar** (tipo de notificação do sistema interativa). Você pode optar por ter a notificação abrir um aplicativo ou site.
   - **Acompanhar taxa de inicialização do aplicativo** (tipo de notificação do sistema interativa). Se desejar avaliar quão bem você está interagindo com seus clientes por meio de cada notificação, marque essa caixa de seleção. Para obter mais detalhes, consulte [Avaliar o desempenho das notificações](#measure-notification-performance).
   - **Duração** (tipo de notificação do sistema interativa). Você pode escolher **Curta** ou **Longa**.
   - **Cenário** (tipo de notificação do sistema interativa). Você pode escolher **Padrão**, **Alarme**, **Lembrete** ou **Chamada de entrada**.
   - **URI base** (tipo de notificação do sistema interativa). Para obter mais detalhes, veja [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712).
   - **Adicionar consulta de imagem** (tipo de notificação do sistema interativa). Para obter mais detalhes, consulte [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Visual**. Uma imagem, vídeo ou som. Para obter mais detalhes, consulte [visual](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Entrada**/**Ação**/**Seleção** (tipo de notificação do sistema interativa). Permite que você possibilite aos usuários interagirem com a notificação. Para obter mais informações, consulte [Notificações do sistema interativas e adaptáveis](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
   - **Associar** (tipo de bloco interativo). O modelo de notificação do sistema. Para obter mais detalhes, consulte [associação](https://msdn.microsoft.com/library/windows/apps/br230843).

   > [!TIP]
   > Tente usar o aplicativo [Visualizador de Notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1) para projetar e testar os blocos adaptáveis e as notificações interativas do sistema.

11. Selecione **Salvar como rascunho** para continuar trabalhando na notificação mais tarde, ou selecione **Enviar** se você tiver terminado.


## <a name="notification-template-types"></a>Tipos de modelo de notificação

Você pode escolher dentre uma variedade de modelos de notificação.

-   **Em branco (Notificação do sistema).** Comece com uma notificação do sistema vazia que você pode personalizar. Notificação do sistema é uma interface do usuário pop-up que aparece na tela para permitir que seu aplicativo se comunique com o cliente quando o cliente está em outro aplicativo, na tela inicial ou na área de trabalho.
-   **Em branco (Bloco).** Comece com uma notificação de bloco vazia que você pode personalizar. Blocos são a representação de um aplicativo na tela inicial. Os blocos são "dinâmicos", pois o conteúdo exibido se modifica em resposta às notificações.
-   **Peça classificações (Notificação do sistema).** Uma notificação do sistema que pede que seus clientes classifiquem seu aplicativo. Quando o cliente seleciona a notificação, a página de classificações da Loja para seu aplicativo é exibida.
-   **Pedir comentários (Notificação do sistema).** Uma notificação do sistema que pede que seus clientes façam comentários sobre o aplicativo. Quando o cliente seleciona a notificação, a página do Hub de Feedback do seu aplicativo é exibida.

   > [!NOTE]
   > Se você escolher esse tipo de modelo, na caixa **Iniciar**, lembre-se de substituir o valor de espaço reservado {PACKAGE_FAMILY_NAME} pelo PFN (Nome da Família de Pacotes) real do seu aplicativo. Você pode encontrar o PFN do seu aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) (**Gerenciamento de aplicativo** > **Identidade de aplicativo**).

   ![Caixa Iniciar da notificação do sistema de comentários](images/push-notifications-feedback-toast-launch-box.png)
-   **Promover (Notificação do sistema).** Uma notificação do sistema para promover um aplicativo diferente de sua escolha. Quando o cliente seleciona a notificação, os detalhes da listagem da Loja do outro aplicativo são exibidos.
   > [!NOTE]
   > Se você escolher esse tipo de modelo, na caixa **Iniciar**, lembre-se de substituir o valor de espaço reservado **{ProductId que você deseja promover aqui}** pela ID da Loja real do item que você deseja promover. Você pode encontrar a ID da Loja na página [Identidade do aplicativo](view-app-identity-details.md) (**Gerenciamento de aplicativo** > **Identidade do aplicativo**).

  ![Caixa Iniciar da notificação do sistema de promover](images/push-notifications-promote-toast-launch-box.png)
-   **Promover uma venda (Notificação do sistema).** Uma notificação do sistema que você pode usar para anunciar uma promoção para seu aplicativo. Quando o cliente seleciona a notificação, os detalhes de seu aplicativo na Loja são exibidos.
-   **Solicitação de atualização (Notificação do sistema).** Uma notificação do sistema que incentiva os clientes que estejam executando uma versão mais antiga do seu aplicativo a instalarem a versão mais recente. Quando o cliente seleciona a notificação, o app da Store é iniciado, mostrando a lista **Downloads e atualizações**. Observe que esse modelo só pode ser usado com um único app, e você não pode direcionar para um determinado segmento de clientes ou definir um tempo para enviá-lo; sempre agendaremos essa notificação para que ela seja enviada dentro de 24 horas e estamos fazendo nosso melhor esforço para termos como destino todos os usuários que ainda não estejam executando a última versão do seu app.


## <a name="measure-notification-performance"></a>Avalie o desempenho das notificações

Você pode avaliar quão bem está interagindo com seus clientes por meio de cada notificação.


###<a name="to-measure-notification-performance"></a>Avaliar o desempenho das notificações

1.  Quando você cria uma notificação, na seção **Conteúdo da notificação**, selecione a caixa de diálogo **Acompanhar taxa de inicialização do aplicativo**.
2.  Em seu aplicativo, chame o método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) para notificar o Centro de Desenvolvimento que seu aplicativo foi iniciado em resposta a uma notificação direcionada. Esse método é fornecido pelo Microsoft Store Services SDK. Para obter mais informações sobre como chamar esse método, consulte [Configurar seu aplicativo para receber notificações do Centro de Desenvolvimento](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


###<a name="to-view-notification-performance"></a>Para exibir o desempenho de notificação

Quando você tiver configurado a notificação e seu aplicativo para medir o desempenho de notificação conforme descrito acima, você pode usar o painel para ver a qualidade do desempenho de suas notificações.

1.  No [painel do Centro de Desenvolvimento do Windows](https://developer.microsoft.com/dashboard/overview), expanda a seção **Interagir** e, em seguida, selecione **Notificações**.
2.  Na página **Notificações por push direcionadas**, selecione **Em andamento** ou **Completed** e, em seguida, examine as colunas **Taxa de entrega** e **Taxa de inicialização do aplicativo** para ver o desempenho de alto nível de cada notificação.
3.  Para ver mais detalhes de desempenho granular, selecione um nome de notificação. Na seção **Estatísticas de entrega**, você pode exibir as informações **Contagem** e **Porcentagem** para os seguintes tipos de **Status** de notificação:
 - **Falha**: A notificação não foi enviada por algum motivo. Isso pode acontecer, por exemplo, se ocorrer um problema no Serviço de Notificação do Windows (WNS).
 - **Falha de expiração de canal**: A notificação não pôde ser entregue porque o canal entre o aplicativo e o Centro de Desenvolvimento expirou. Isso pode acontecer, por exemplo, se o cliente não abre o aplicativo há muito tempo.
 - **Enviando**: A notificação está na fila para ser enviada.
 - **Enviada**: A notificação foi enviada.
 - **Inicia**: A notificação foi enviada, o cliente clicou nela e o aplicativo foi aberto em resultado disso. Observe que isso apenas rastreia inicializações de aplicativos. Notificações que convidam o cliente para realizar outras ações, como iniciar a Loja para deixar uma classificação, não são incluídas nesse status.
 - **Desconhecido**: Não foi possível determinar o status dessa notificação.


## <a name="translate-your-notifications"></a>Traduza suas notificações

Para maximizar o impacto de suas notificações, considere traduzi-las para os idiomas que seus clientes preferem. O Centro de Desenvolvimento torna mais fácil para você traduzir suas notificações automaticamente, aproveitando a capacidade do serviço [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx).

1.  Depois de escrever sua notificação em seu idioma padrão, selecione **Adicionar idiomas** (abaixo do menu **Idiomas** na seção **Conteúdo da notificação**).
2.  Na janela **Adicionar idiomas**, selecione os idiomas adicionais em que você deseja que suas notificações apareçam e selecione **Atualizar**.
Sua notificação será automaticamente traduzida para os idiomas que você escolheu na janela **Adicionar idiomas** e os idiomas serão adicionados ao menu **Idioma**.
3.  Para ver a tradução da notificação, no menu **Idioma**, selecione o idioma que você acabou de adicionar.

Coisas a serem lembradas sobre tradução:
 - Você pode substituir a tradução automática inserindo algo diferente na caixa **Conteúdo** para aquele idioma.
 - Se você adicionar outra caixa de texto para a versão em inglês da notificação depois de ter substituído uma tradução automática, a nova caixa de texto não será adicionada à notificação traduzida. Nesse caso, você precisaria adicionar manualmente a nova caixa de texto para cada uma das notificações traduzidas.
 - Se você alterar o texto em inglês depois que a notificação for traduzida, vamos atualizar automaticamente as notificações traduzidas para corresponder à alteração. No entanto, isso não vai acontecer se você tiver escolhido previamente substituir a tradução inicial.

## <a name="related-topics"></a>Tópicos relacionados
- [Blocos de aplicativos UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Visão geral do WNS (Serviços de Notificação por Push do Windows)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Aplicativo Visualizador de Notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | método registerNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [Segmentação de clientes e notificações por push: um novo recurso do Programa Insider do Centro de Desenvolvimento do Windows (postagem de blog)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)

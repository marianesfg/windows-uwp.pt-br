---
author: JnHs
Description: "Saiba como enviar notificações do Centro de Desenvolvimento do Windows ao seu aplicativo para incentivar grupos de clientes a executar uma ação, como classificar um aplicativo ou comprar um complemento."
title: "Enviar notificações por push direcionadas para clientes do seu aplicativo"
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.openlocfilehash: e1103639cd324ec0dab9e43159cab57c39747d17
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2017
---
# <a name="send-notifications-to-your-apps-customers"></a>Enviar notificações para clientes do seu aplicativo

Interagir com seus clientes no momento certo e com a mensagem certa é essencial para o sucesso como um desenvolvedor de aplicativos. O Centro de Desenvolvimento do Windows fornece uma plataforma de interação com o cliente controlada por dados que pode ser usada para enviar notificações para todos os seus clientes ou direcionadas apenas a um subconjunto de seus clientes do Windows 10 que atendem aos critérios que você definiu em um [segmento do cliente](create-customer-segments.md).

Você pode usar notificações para incentivar os clientes a executar uma ação, como classificar um aplicativo, comprar um complemento, experimentar um novo recurso ou baixar outro aplicativo (talvez gratuitamente com um [código promocional](generate-promotional-codes.md) fornecido por você).

> [!IMPORTANT]
> Essas notificações podem ser usadas somente com aplicativos UWP.

Ao considerar o conteúdo de suas notificações, tenha em mente:
- O conteúdo em suas notificações deve estar de acordo com as [Políticas de Conteúdo da Loja](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies).
- Seu conteúdo da notificação não deve incluir informações confidenciais ou potencialmente sensíveis.
- Embora nos esforçamos para entregar a notificação conforme agendado, pode haver, ocasionalmente, problemas de latência que impactam a entrega.
- Certifique-se de não enviar notificações muitas vezes. Mais de uma vez a cada 30 minutos pode parecer intrusivo (e para muitos cenários, menos frequência do que isso é preferível).
- Saiba que, se um cliente que usa seu aplicativo (e está atribuído a sua conta da Microsoft no momento em que a associação de segmento é determinada), e o usuário da conta fornece seu dispositivo para outra pessoa usar, a outra pessoa pode ver a notificação que era para o cliente original. Para obter mais informações, consulte [Configurar seu aplicativo para notificações por push direcionada](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).

## <a name="getting-started-with-notifications"></a>Introdução às notificações

Em um nível superior, você precisa executar três ações para usar notificações e interagir com seus clientes.
1. **Registre seu aplicativo para receber notificações por push.** Você pode fazer isso adicionando uma referência ao Microsoft Store Services SDK em seu aplicativo e, em seguida, adicionando algumas linhas de código para registrar um canal de notificação entre o Centro de Desenvolvimento e seu aplicativo. Usaremos este canal para disponibilizar as notificações para seus clientes. Para obter mais detalhes, consulte [Configurar seu aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Crie um ou mais segmentos de clientes aos quais você deseja se dirigir.** Você pode agrupar seus clientes em segmentos com base em critérios demográficos ou de receita. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md). Você também pode enviar uma notificação para todos os clientes do seu aplicativo, se você preferir.
3. **Crie uma notificação por push e envie-a para um segmento de cliente específico.** Por exemplo, envie uma notificação para incentivar os novos clientes a classificar seu aplicativo ou envie uma notificação com uma oferta especial para adquirir um complemento.

## <a name="to-create-and-send-a-notification"></a>Para criar e enviar uma notificação

Siga estas etapas para criar uma notificação no painel e enviá-la para um segmento de cliente específico.

> [!NOTE]
> Para que seu aplicativo possa receber notificações do Centro de Desenvolvimento, primeiro você deverá chamar o método [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) no aplicativo a fim de registrá-lo para receber notificações. Esse método está disponível no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Para obter mais informações sobre como chamar esse método, incluindo um exemplo de código, consulte [Configurar o aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1.  No [painel do Centro de Desenvolvimento do Windows](https://developer.microsoft.com/dashboard/overview), expanda a seção **Interagir** e, em seguida, selecione **Notificações**.
2.  Na página **Notificações**, selecione **Nova notificação**.
3.      No menu suspenso, selecione o aplicativo para o qual você deseja gerar uma notificação.
4.  Na seção **Selecionar um modelo**, escolha o tipo de notificação que deseja enviar. Para obter detalhes, consulte [Tipos de modelo de notificação](#notification-template-types).
  ![Modelos de notificação](images/push-notifications-template.png)
5.  Na seção **Configurações de notificação**, escolha um **Nome** para a sua notificação e escolha o **Grupo de clientes** para o qual deseja enviar a notificação.
Se você ainda não criou um segmento, selecione **Criar novo grupo de clientes**. Observe que leva 24 horas para que um novo segmento esteja disponível para usar notificações. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md).
6.  Se você quiser especificar quando enviar a notificação, limpe a caixa de seleção **Enviar notificação imediatamente** e escolha data e hora específicas.
7.  Se você quiser que a notificação expire em algum momento, limpe a caixa de seleção **Notificação nunca expira** e escolha data e hora de expiração específicas.
8.  Se você deseja filtrar os destinatários para que a notificação seja entregue apenas para pessoas que usam determinados idiomas ou que estejam em fusos horários específicos, marque a caixa de seleção **Filtros**. Em seguida, você poderá selecionar os idiomas e/ou os fusos horários que servirão de base para filtrar os destinatários.
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
   - **Entrada**/**Ação**/**Seleção** (tipo de notificação do sistema interativa). Permite que você possibilite aos usuários interagirem com a notificação. Para obter mais informações, consulte [Notificações do sistema interativas e adaptáveis](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md).
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
-   **Solicitação de atualização (Notificação do sistema).** Uma notificação do sistema que incentiva os clientes que estejam executando uma versão mais antiga do seu aplicativo a instalarem a versão mais recente. Quando o cliente seleciona a notificação, a lista **Transferência e atualizações** no aplicativo da Loja é exibida. Observe que você não precisa criar um segmento de cliente para usar esse modelo. Agendaremos essa notificação dentro de 24 horas e nos esforçaremos ao máximo para ter como alvo todos os usuários que ainda não estejam executando a versão mais recente do seu aplicativo.

## <a name="measure-notification-performance"></a>Avalie o desempenho das notificações

Você pode avaliar quão bem está interagindo com seus clientes por meio de cada notificação.

###<a name="to-measure-notification-performance"></a>Avaliar o desempenho das notificações

1.  Quando você cria uma notificação, na seção **Conteúdo da notificação**, selecione a caixa de diálogo **Acompanhar taxa de inicialização do aplicativo**.
2.  Em seu aplicativo, chame o método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) para notificar o Centro de Desenvolvimento que seu aplicativo foi iniciado em resposta a uma notificação direcionada. Esse método é fornecido pelo Microsoft Store Services SDK. Para obter mais informações sobre como chamar esse método, consulte [Configurar seu aplicativo para receber notificações do Centro de Desenvolvimento](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

###<a name="to-view-notification-performance"></a>Para exibir o desempenho de notificação

Quando você tiver configurado a notificação e seu aplicativo para [medir o desempenho de notificação](#measure-notification-performance) conforme descrito acima, você pode usar o painel para ver a qualidade do desempenho de suas notificações.

1.  No painel, selecione um de seus aplicativos.
2.  Expanda a seção **Serviços** no menu esquerdo e, em seguida, selecione **Notificações por Push** para ver as notificações associadas a esse aplicativo.
3.  Na página **Notificações por push direcionadas**, selecione **Em andamento** ou **Completed** e, em seguida, examine as colunas **Taxa de entrega** e **Taxa de inicialização do aplicativo** para ver o desempenho de alto nível de cada notificação.
4.  Para ver mais detalhes de desempenho granular, selecione um nome de notificação. A seção **Estatísticas de entrega** é exibida e mostra as informações **Contagem** e **Porcentagem** para os seguintes tipos de **Status** de notificação:
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
- [Blocos, selos e notificações para aplicativos UWP](../controls-and-patterns/tiles-badges-notifications.md)
- [Visão geral dos WNS (Serviços de Notificação por Push do Windows)](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [Aplicativo Visualizador de Notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | método registerNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [Segmentação de clientes e notificações por push: um novo recurso do Programa Insider do Centro de Desenvolvimento do Windows (postagem de blog)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)
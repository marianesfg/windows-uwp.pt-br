---
Description: Saiba como enviar notificações do Partner Center para seu aplicativo a fim de incentivar os grupos de clientes a realizar uma ação, como a classificação de um aplicativo ou a compra de um complemento.
title: Enviar notificações por push direcionadas para clientes do seu app
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, notificações direcionadas, notificações por push, notificações do sistema, bloco
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 8ac9d464588cada11cd9d41cf0eb7858f593a0f2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259935"
---
# <a name="send-notifications-to-your-apps-customers"></a>Enviar notificações para clientes do seu aplicativo

Interagir com seus clientes no momento certo e com a mensagem certa é essencial para o sucesso como um desenvolvedor de aplicativos. As notificações podem incentivar os clientes a executar uma ação, como classificar um aplicativo, comprar um complemento, experimentar um novo recurso ou baixar outro aplicativo (talvez gratuitamente com um [código promocional](generate-promotional-codes.md) fornecido por você).

O [Partner Center](https://partner.microsoft.com/dashboard) fornece uma plataforma de envolvimento do cliente controlada por dados que você pode usar para enviar notificações para todos os clientes do seu aplicativo ou direcionado apenas para um subconjunto dos clientes do Windows 10 do seu aplicativo que atendem aos critérios que você definiu em um [segmento de cliente](create-customer-segments.md). Você também pode criar uma notificação a ser enviada aos clientes de mais de um de seus aplicativos.

> [!IMPORTANT]
> Essas notificações podem ser usadas somente com aplicativos UWP.

Ao considerar o conteúdo de suas notificações, tenha em mente:
- O conteúdo em suas notificações deve estar de acordo com as [Políticas de Conteúdo da Loja](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies).
- Seu conteúdo da notificação não deve incluir informações confidenciais ou potencialmente sensíveis.
- Embora nos esforçamos para entregar a notificação conforme agendado, pode haver, ocasionalmente, problemas de latência que impactam a entrega.
- Certifique-se de não enviar notificações muitas vezes. Mais de uma vez a cada 30 minutos pode parecer intrusivo (e para muitos cenários, menos frequência do que isso é preferível).
- Saiba que, se um cliente que usa seu aplicativo (e está atribuído a sua conta da Microsoft no momento em que a associação de segmento é determinada), e o usuário da conta fornece seu dispositivo para outra pessoa usar, a outra pessoa pode ver a notificação que era para o cliente original. Para obter mais informações, consulte [Configurar seu aplicativo para notificações por push direcionada](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
- Se você enviar a mesma notificação para os clientes de vários aplicativos, você não poderá direcionar um segmento, a notificação será enviada para todos os clientes para os aplicativos que você selecionar.


## <a name="getting-started-with-notifications"></a>Introdução às notificações

Em um nível superior, você precisa executar três ações para usar notificações e interagir com seus clientes.

1. **Registre seu aplicativo para receber notificações por push.** Você faz isso adicionando uma referência ao SDK dos serviços de Microsoft Store em seu aplicativo e, em seguida, adicionando algumas linhas de código que registram um canal de notificação entre o Partner Center e seu aplicativo. Usaremos este canal para disponibilizar as notificações para seus clientes. Para obter mais detalhes, consulte [Configurar seu aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Decida quais clientes direcionam.** Você pode enviar a notificação para todos os clientes do seu app, ou (para notificações criadas para um único app) para um grupo de clientes chamado um *segmento*, que você pode definir com base em critérios demográficos ou de receita. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md).
3. **Crie o conteúdo da notificação e o envie.** Por exemplo, você pode criar uma notificação que incentive os novos clientes a classificar seu app, ou pode enviar uma notificação que promova uma oferta especial para adquirir um complemento.


## <a name="to-create-and-send-a-notification"></a>Para criar e enviar uma notificação

Siga estas etapas para criar uma notificação no Partner Center e enviá-la a um segmento de cliente específico.

> [!NOTE]
> Antes que um aplicativo possa receber notificações do Partner Center, primeiro você deve chamar o método [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) em seu aplicativo para registrar seu aplicativo para receber notificações. Esse método está disponível no [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Para obter mais informações sobre como chamar esse método, incluindo um exemplo de código, consulte [Configurar o aplicativo para notificações por push direcionadas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1. No [Partner Center](https://partner.microsoft.com/dashboard), expanda a seção **envolver** e, em seguida, selecione **notificações**.
2. Na página **Notificações**, selecione **Nova notificação**.
3. Na seção **selecionar um modelo** , escolha o [tipo de notificação](#notification-template-types) que você deseja enviar e clique em **OK**.
4. Na próxima página, use o menu suspenso para escolher um **Único aplicativo** ou **Vários aplicativos** para qual (quais) você deseja gerar uma notificação. Você só pode selecionar aplicativos que foram [configurados para receber notificações usando o SDK dos serviços Microsoft Stores](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. Na seção **Configurações de notificação**, escolha um **Nome** para a sua notificação e, se aplicável, escolha o **Grupo de clientes** para o qual você deseja enviar a notificação. (As notificações enviadas para vários aplicativos só podem ser enviadas para todos os clientes desses aplicativos). Se você quiser usar um segmento que você ainda não criou, selecione **Criar novo grupo de cliente**. Observe que serão necessárias 24 horas para que você possa usar um novo segmento para notificações. Para obter mais informações, consulte [Criar segmentos de clientes](create-customer-segments.md).
6. Se você quiser especificar quando a notificação deverá ser enviada, desmarque a caixa de seleção **Enviar a notificação imediatamente** e escolha uma data e uma hora específicas (no horário UTC para todos os clientes, a menos que você especifique o uso do fuso horário local de cada cliente).
7. Se você quiser que a notificação expire em algum momento, limpe a caixa de seleção **Notificação nunca expira** e escolha data e hora de expiração específicas (em UTC).
8. **Para notificações para um único aplicativo:** se você deseja filtrar os destinatários para que sua notificação seja entregue apenas à pessoas que usam certos idiomas ou que estão em um fuso horário específico, marque a caixa de seleção **Usar filtros**. Em seguida, você pode especificar as opções de idioma e/ou fuso horário que deseja usar.
8. **Para notificações para vários aplicativos:** especificar se deseja enviar a notificação somente para o último aplicativo ativo em cada dispositivo (por cliente), ou para todos os aplicativos em cada dispositivo.
10. Na seção **Conteúdo da notificação**, no menu **Idioma**, escolha os idiomas em que você deseja que sua notificação seja exibida. Para obter mais informações, consulte [Traduzir suas notificações](#translate-your-notifications).
11. Na seção **Opções**, insira texto e configure as outras opções que desejar. Se você começou com um modelo, parte disso é fornecido por padrão, mas você pode fazer quaisquer alterações que desejar.

    As opções disponíveis variam, dependendo de qual tipo de notificação você está usando. Algumas das opções são:

    * **Tipo de ativação** (tipo de notificação do sistema interativa). Você pode escolher **Em primeiro plano**, **Em segundo plano**, ou **Protocolo**.
    * **Iniciar** (tipo de notificação do sistema interativa). Você pode optar por ter a notificação abrir um aplicativo ou site.
    * **Acompanhar taxa de inicialização do aplicativo** (tipo de notificação do sistema interativa). Se desejar avaliar quão bem você está interagindo com seus clientes por meio de cada notificação, marque essa caixa de seleção. Para obter mais detalhes, consulte [Avaliar o desempenho das notificações](#measure-notification-performance).
    * **Duração** (tipo de notificação do sistema interativa). Você pode escolher **Curta** ou **Longa**.
    * **Cenário** (tipo de notificação do sistema interativa). Você pode escolher **Padrão**, **Alarme**, **Lembrete** ou **Chamada de entrada**.
    * **URI base** (tipo de notificação do sistema interativa). Para obter mais detalhes, veja [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri).
    * **Adicionar consulta de imagem** (tipo de notificação do sistema interativa). Para obter mais detalhes, consulte [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements).
    * **Visual**. Uma imagem, vídeo ou som. Para obter mais detalhes, consulte [visual](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual).
    * **Entrada**/**Ação**/**Seleção** (tipo de notificação do sistema interativa). Permite que você possibilite aos usuários interagirem com a notificação. Para obter mais informações, consulte [Notificações do sistema interativas e adaptáveis](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
    * **Associar** (tipo de bloco interativo). O modelo de notificação do sistema. Para obter mais detalhes, veja [binding](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding).

    > [!TIP]
    > Tente usar o aplicativo [Visualizador de Notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1) para projetar e testar os blocos adaptáveis e as notificações interativas do sistema.

12. Selecione **Salvar como rascunho** para continuar trabalhando na notificação mais tarde, ou selecione **Enviar** se você tiver terminado.


## <a name="notification-template-types"></a>Tipos de modelo de notificação

Você pode escolher dentre uma variedade de modelos de notificação.

-   **Em branco (sistema de notificação).** Comece com uma notificação do sistema vazia que você pode personalizar. Notificação do sistema é uma interface do usuário pop-up que aparece na tela para permitir que seu aplicativo se comunique com o cliente quando o cliente está em outro aplicativo, na tela inicial ou na área de trabalho.
-   **Em branco (bloco).** Comece com uma notificação de bloco vazia que você pode personalizar. Blocos são a representação de um aplicativo na tela inicial. Os blocos são "dinâmicos", pois o conteúdo exibido se modifica em resposta às notificações.
-   **Solicitar classificações (notificação).** Uma notificação do sistema que pede que seus clientes classifiquem seu aplicativo. Quando o cliente seleciona a notificação, a página de classificações da Loja para seu aplicativo é exibida.
-   **Solicitar comentários (notificação).** Uma notificação do sistema que pede que seus clientes façam comentários sobre o aplicativo. Quando o cliente seleciona a notificação, a página do Hub de Feedback do seu aplicativo é exibida.
    > [!NOTE]
    > Se você escolher esse tipo de modelo, na caixa **Iniciar**, lembre-se de substituir o valor de espaço reservado {PACKAGE_FAMILY_NAME} pelo PFN (Nome da Família de Pacotes) real do seu aplicativo. Você pode encontrar o PFN do seu aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) (**Gerenciamento de aplicativo** > **Identidade de aplicativo**).

    ![Caixa Iniciar da notificação do sistema de comentários](images/push-notifications-feedback-toast-launch-box.png)

-   **Promoção cruzada (notificação).** Uma notificação do sistema para promover um aplicativo diferente de sua escolha. Quando o cliente seleciona a notificação, os detalhes da listagem da Loja do outro aplicativo são exibidos.
    > [!NOTE]
    > Se você escolher esse tipo de modelo, na caixa **Iniciar**, lembre-se de substituir o valor de espaço reservado **{ProductId que você deseja promover aqui}** pela ID da Loja real do item que você deseja promover. Você pode encontrar a ID da Loja na página [Identidade do aplicativo](view-app-identity-details.md) (**Gerenciamento de aplicativo** > **Identidade do aplicativo**).

    ![Caixa Iniciar da notificação do sistema de promover](images/push-notifications-promote-toast-launch-box.png)

-   **Promover uma venda (notificação).** Uma notificação do sistema que você pode usar para anunciar uma promoção para seu aplicativo. Quando o cliente seleciona a notificação, os detalhes de seu aplicativo na Loja são exibidos.
-   **Solicitar atualização (notificação).** Uma notificação do sistema que incentiva os clientes que estejam executando uma versão mais antiga do seu aplicativo a instalarem a versão mais recente. Quando o cliente seleciona a notificação, o app da Store é iniciado, mostrando a lista **Downloads e atualizações**. Observe que esse modelo só pode ser usado com um único app, e você não pode direcionar para um determinado segmento de clientes ou definir um tempo para enviá-lo; sempre agendaremos essa notificação para que ela seja enviada dentro de 24 horas e estamos fazendo nosso melhor esforço para termos como destino todos os usuários que ainda não estejam executando a última versão do seu app.


## <a name="measure-notification-performance"></a>Avalie o desempenho das notificações

Você pode avaliar quão bem está interagindo com seus clientes por meio de cada notificação.


### <a name="to-measure-notification-performance"></a>Avaliar o desempenho das notificações

1.  Quando você cria uma notificação, na seção **Conteúdo da notificação**, selecione a caixa de diálogo **Acompanhar taxa de inicialização do aplicativo**.
2.  Em seu aplicativo, chame o método [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) para notificar o Partner Center de que seu aplicativo foi iniciado em resposta a uma notificação de destino. Esse método é fornecido pelo Microsoft Store Services SDK. Para obter mais informações sobre como chamar esse método, consulte [configurar seu aplicativo para receber notificações do Partner Center](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>Para exibir o desempenho de notificação

Quando você configurou a notificação e seu aplicativo para medir o desempenho de notificação conforme descrito acima, você pode ver como suas notificações estão sendo executadas.

Para examinar os dados detalhados de cada notificação:

1.  No Partner Center, expanda a seção **envolver** e selecione **notificações**.
2.  Na tabela de notificações existentes, selecione **em andamento** ou **concluído**e, em seguida, examine as colunas **taxa de entrega** e taxa de **inicialização do aplicativo** para ver o desempenho de alto nível de cada notificação.
3.  Para ver mais detalhes de desempenho granular, selecione um nome de notificação. Na seção **Estatísticas de entrega**, você pode exibir as informações **Contagem** e **Porcentagem** para os seguintes tipos de **Status** de notificação:
    * **Falha**: A notificação não foi enviada por algum motivo. Isso pode acontecer, por exemplo, se ocorrer um problema no Serviço de Notificação do Windows (WNS).
    * **Falha de expiração do canal**: a notificação não pôde ser entregue porque o canal entre o aplicativo e o Partner Center expirou. Isso pode acontecer, por exemplo, se o cliente não abre o aplicativo há muito tempo.
    * **Enviando**: A notificação está na fila para ser enviada.
    * **Enviada**: A notificação foi enviada.
    * **Inicia**: A notificação foi enviada, o cliente clicou nela e o aplicativo foi aberto em resultado disso. Observe que isso apenas rastreia inicializações de aplicativos. Notificações que convidam o cliente para realizar outras ações, como iniciar a Loja para deixar uma classificação, não são incluídas nesse status.
    * **Desconhecido**: Não foi possível determinar o status dessa notificação.

Para analisar dados de atividade do usuário para todas as suas notificações:

1.  No Partner Center, expanda a seção **envolver** e selecione **notificações**.
2.  Na página **notificações** , clique na guia **analisar** . Essa guia exibe os seguintes dados:
    * Exibições de grafo dos vários Estados de ação do usuário para suas notificações de notificação e da central de ações.
    * Exibições do World Map das taxas de cliques para suas notificações de notificação e da central de ações.
3. Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é 30D (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar. Você também pode expandir **filtros** para filtrar todos os dados por aplicativo e mercado.

## <a name="translate-your-notifications"></a>Traduza suas notificações

Para maximizar o impacto de suas notificações, considere traduzi-las para os idiomas que seus clientes preferem. O Partner Center facilita a tradução automática de suas notificações, aproveitando o poder do serviço [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) .

1.  Depois de escrever sua notificação em seu idioma padrão, selecione **Adicionar idiomas** (abaixo do menu **Idiomas** na seção **Conteúdo da notificação**).
2.  Na janela **Adicionar idiomas**, selecione os idiomas adicionais em que você deseja que suas notificações apareçam e selecione **Atualizar**.
Sua notificação será automaticamente traduzida para os idiomas que você escolheu na janela **Adicionar idiomas** e os idiomas serão adicionados ao menu **Idioma**.
3.  Para ver a tradução da notificação, no menu **Idioma**, selecione o idioma que você acabou de adicionar.

Coisas a serem lembradas sobre tradução:
 - Você pode substituir a tradução automática inserindo algo diferente na caixa **Conteúdo** para aquele idioma.
 - Se você adicionar outra caixa de texto para a versão em inglês da notificação depois de ter substituído uma tradução automática, a nova caixa de texto não será adicionada à notificação traduzida. Nesse caso, você precisaria adicionar manualmente a nova caixa de texto para cada uma das notificações traduzidas.
 - Se você alterar o texto em inglês depois que a notificação for traduzida, vamos atualizar automaticamente as notificações traduzidas para corresponder à alteração. No entanto, isso não vai acontecer se você tiver escolhido previamente substituir a tradução inicial.

## <a name="related-topics"></a>Tópicos relacionados
- [Blocos para aplicativos UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Aplicativo visualizador de notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager. RegisterNotificationChannelAsync () | método registerNotificationChannelAsync ()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)

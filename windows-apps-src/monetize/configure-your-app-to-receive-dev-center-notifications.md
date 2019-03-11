---
Description: Saiba como registrar seu aplicativo UWP para receber notificações por push que você enviar do Partner Center.
title: Configurar seu app para notificações por push direcionadas
ms.date: 02/08/2017
ms.topic: article
keywords: o Windows 10, uwp, de Microsoft Store Services SDK, notificações por push, Partner Center de destino
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: f60780186256e7f78a9596c979c79bfc704ae4c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660161"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurar seu app para notificações por push direcionadas

Você pode usar o **notificações por Push** direcionado de página no Partner Center interagir diretamente com clientes enviando notificações por push para os dispositivos em que seu aplicativo da plataforma Universal do Windows (UWP) está instalado. Por exemplo, você pode usar notificações por push direcionadas para incentivar os clientes a executar uma ação, como a classificação do seu aplicativo ou o uso de um novo recurso. Você pode enviar vários tipos diferentes de notificações por push, incluindo notificações do sistema, notificações de bloco e notificações XML brutas. Você também pode controlar a taxa de inicializações do aplicativo resultantes de suas notificações por push. Para obter mais informações sobre esse recurso, consulte [Enviar notificações por push para clientes do seu aplicativo](../publish/send-push-notifications-to-your-apps-customers.md).

Antes de enviar notificações por push direcionadas para os clientes do Partner Center, você deve usar um método para o [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) classe em que o do Microsoft Store Services SDK para registrar seu aplicativo para receber notificações. Você pode usar métodos adicionais para essa classe para notificar o Partner Center que seu aplicativo foi iniciado em resposta a uma notificação por push direcionadas (se você quiser acompanhar a taxa de inicializações do aplicativo que resultaram de suas notificações) e para interromper o recebimento de notificações.

## <a name="configure-your-project"></a>Configurar seu projeto

Antes de gravar qualquer código, siga estas etapas para adicionar uma referência ao Microsoft Store Services SDK no seu projeto:

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento. 
2. Abra o projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

## <a name="register-for-push-notifications"></a>Registrar para notificações por push

Para registrar seu aplicativo para receber notificações por push direcionadas do Partner Center:

1. No seu projeto, localize uma seção de código que é executado durante a inicialização em que você pode registrar seu app para receber notificações.
2. Adicione a instrução a seguir na parte superior do arquivo de código.

    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Obtenha um objeto [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) e chame uma das sobrecargas [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) no código de inicialização que você identificou anteriormente. Esse método deve ser chamado sempre que seu aplicativo é iniciado.

  * Se você quiser Partner Center para criar seu próprio URI do canal para notificações, chame o [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) de sobrecarga.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > Se o seu aplicativo também chama o [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) para criar um canal de notificações para WNS, certifique-se de que o seu código não chama as sobrecargas [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) e [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) simultaneamente. Se você precisar chamar esses dois métodos, chame-os sequencialmente e espere o retorno de um método antes de chamar o outro.

  * Se você quiser especificar o canal URI a ser usado para notificações por push direcionadas do Partner Center, a chamada a [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) de sobrecarga. Por exemplo, talvez queira fazer isso se seu aplicativo já usa o Serviço de Notificação por Push do Windows (WNS) e você quer usar o mesmo URI do canal. Você deve primeiro criar um objeto [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) e atribuir a propriedade [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) ao URI de canal.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> Quando você chama o método **RegisterNotificationChannelAsync**, um arquivo chamado MicrosoftStoreEngagementSDKId.txt é criado no repositório de dados de aplicativo local para o seu aplicativo (a pasta retornada pela propriedade [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder)). Este arquivo contém uma ID usada pela infraestrutura de notificações por push direcionadas. Certifique-se de que seu aplicativo não modifique ou exclua esse arquivo. Caso contrário, os usuários poderão receber várias instâncias de notificações, ou as notificações podem se comportar de outras maneiras que não sejam adequadas.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Como as notificações por push direcionadas são roteadas para os clientes

Quando o app chama **RegisterNotificationChannelAsync**, esse método coleta a conta da Microsoft do cliente que está atualmente conectado ao dispositivo. Posteriormente, quando você envia uma notificação por push direcionadas para um segmento que inclui esse cliente, o Partner Center envia a notificação para dispositivos que estão associados à conta da Microsoft desse cliente.

Se o cliente que iniciou o app oferecer o dispositivo para outra pessoa usar enquanto ele ainda estiver conectado ao dispositivo com a conta da Microsoft, lembre-se de que a outra pessoa poderá ver a notificação direcionada para o cliente original. Isso pode ter consequências indesejadas, especialmente para os apps que oferecem serviços aos quais os clientes podem se conectar para usar. Para impedir que outros usuários vejam suas notificações direcionadas neste cenário, chame o método [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) quando os clientes saírem de seu aplicativo. Para saber mais, veja [Cancelar registro para notificações por push](#unregister) posteriormente neste artigo.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Como seu app responde quando o usuário inicia seu app

Depois que seu aplicativo está registrado para receber notificações e você [enviar uma notificação por push para clientes do seu aplicativo do Partner Center](../publish/send-push-notifications-to-your-apps-customers.md), um dos seguintes pontos de entrada em seu aplicativo será chamado quando o usuário inicia o aplicativo em resposta para a notificação por push. Se você tiver algum código que deseja executar quando o usuário inicia seu aplicativo, adicione o código a um desses pontos de entrada em seu aplicativo.

  * Se a notificação por push tiver um tipo de ativação em primeiro plano, substitua o método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) da classe **App** em seu projeto e adicione o código a esse método.

  * Se a notificação por push tiver um tipo de ativação em segundo plano, adicione código ao método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) da sua [tarefa em segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

Por exemplo, convém recompensar os usuários do seu aplicativo que adquiriram os complementos pagos em seu aplicativo com um complemento gratuito. Nesse caso, você pode enviar uma notificação por push para um [segmento do cliente](../publish/create-customer-segments.md) direcionado para esses usuários. Em seguida, você pode adicionar código para conceder uma [compra no aplicativo](in-app-purchases-and-trials.md) gratuita em um dos pontos de entrada listados acima.

## <a name="notify-partner-center-of-your-app-launch"></a>Notificar o Centro de parceiros de inicialização de seu aplicativo

Se você selecionar o **taxa de inicialização do aplicativo de faixa** opção para a notificação por push direcionadas no Partner Center, a chamada a [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) método do ponto de entrada apropriada em seu aplicativo para notificar o Partner Center que seu aplicativo foi iniciado em resposta a uma notificação por push.

Além disso, esse método retorna os argumentos de inicialização originais para o seu aplicativo. Quando você escolhe acompanhar a taxa de inicialização do aplicativo para a notificação por push, um rastreamento opaco que ID será adicionada aos argumentos de inicialização para ajudar a controlar o aplicativo Iniciar no Partner Center. Você deve passar os argumentos de inicialização para seu aplicativo para o [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) método e esse método envia a ID de rastreamento ao Partner Center, remove a ID de rastreamento de argumentos de inicialização e retorna o original Inicie os argumentos para o seu código.

A maneira como você chama esse método depende do tipo de ativação da notificação por push:

* Se a notificação por push tiver um tipo de ativação em primeiro plano, chame esse método a partir da substituição do método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) em seu aplicativo e transmita os argumentos que estão disponíveis no objeto [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) que é transmitido para esse método. O exemplo de código a seguir supõe que o arquivo de código tem instruções **using** para os namespaces **Microsoft.Services.Store.Engagement** e **Windows.ApplicationModel.Activation**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Se a notificação por push tiver um tipo de ativação em segundo plano, chame esse método a partir do método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para sua [tarefa em segundo plano](../launch-resume/support-your-app-with-background-tasks.md) e transmita os argumentos que estão disponíveis no objeto [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) que é transmitido para esse método. O exemplo de código a seguir supõe que seu arquivo de código tem instruções **using** para os namespaces **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** e **Windows.UI.Notifications**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Cancelar registro para notificações por push

Se você quiser que seu aplicativo para interromper o recebimento de notificações por push do Partner Center, chamada de destino a [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) método.

[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Observe que esse método invalida o canal que está sendo usado para notificações para que o app não receba mais notificações por push de *nenhum* serviço. Após ele ter sido fechado, o canal não pode ser usado novamente para todos os serviços, incluindo notificações por push direcionadas do Partner Center e outras notificações usando WNS. Para continuar enviando notificações por push para esse aplicativo, o aplicativo deve solicitar um novo canal.

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar notificações por push para clientes do seu aplicativo](../publish/send-push-notifications-to-your-apps-customers.md)
* [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [Como solicitar, criar e salvar um canal de notificação](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)

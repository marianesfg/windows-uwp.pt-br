---
author: mcleanbyron
Description: "Saiba como registrar seu aplicativo UWP para receber notificações por push enviadas do Centro de Desenvolvimento do Windows."
title: "Configurar seu app para receber notificações por push do Centro de Desenvolvimento"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: d840fbe66e5ccb439148c7849e44b923a5586740

---

# <a name="configure-your-app-to-receive-dev-center-push-notifications"></a>Configurar seu app para receber notificações por push do Centro de Desenvolvimento

Você pode usar a página **Notificações por push** no painel do Centro de Desenvolvimento do Windows para interagir diretamente com os clientes enviando notificações por push direcionadas para os dispositivos nos quais seu aplicativo da Plataforma Universal do Windows (UWP) está instalado. Por exemplo, você pode usar notificações por push direcionadas para incentivar os clientes a executar uma ação, como a classificação do seu aplicativo ou o uso de um novo recurso. Você pode enviar vários tipos diferentes de notificações por push, incluindo notificações do sistema, notificações de bloco e notificações XML brutas. Você também pode controlar a taxa de inicializações do aplicativo resultantes de suas notificações por push. Para obter mais informações sobre esse recurso, consulte [Enviar notificações por push para clientes do seu aplicativo](../publish/send-push-notifications-to-your-apps-customers.md).

Antes de enviar notificações por push direcionadas para seus clientes do Centro de Desenvolvimento, você deve usar um método da classe [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) no Microsoft Store Services SDK para registrar seu aplicativo para receber notificações. Você pode usar métodos adicionais dessa classe para notificar o Centro de Desenvolvimento que seu aplicativo foi iniciado em resposta a uma notificação por push direcionada (se quiser controlar a taxa de inicializações do aplicativo resultantes de suas notificações) e para parar de receber notificações.

## <a name="configure-your-project"></a>Configurar seu projeto

Antes de gravar qualquer código, siga estas etapas para adicionar uma referência ao Microsoft Store Services SDK no seu projeto:

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento. Além de API para registrar um aplicativo para receber notificações, esse SDK também fornece APIs para outros recursos como execução de experiências em seus aplicativos com testes A/B e exibição de anúncios.
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

## <a name="register-for-push-notifications"></a>Registrar para notificações por push

Para registrar seu aplicativo para receber notificações por push direcionadas do Centro de Desenvolvimento:

1. No seu projeto, localize uma seção de código que é executado durante a inicialização em que você pode registrar seu app para receber notificações do Centro de Desenvolvimento.
2. Adicione a instrução a seguir na parte superior do arquivo de código.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Obtenha um objeto [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) e chame uma das sobrecargas [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) no código de inicialização que você identificou anteriormente. Esse método deve ser chamado sempre que seu aplicativo é iniciado.

  * Se você quiser que o Centro de Desenvolvimento crie seu próprio URI de canal para as notificações, chame a sobrecarga [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx).

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]

    <span/>
    >**Importante**&nbsp;&nbsp;Se seu app também chamar [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) para criar um canal de notificação para WNS, verifique se seu código não chama as sobrecargas [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) e [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) simultaneamente. Se você precisar chamar esses dois métodos, chame-os sequencialmente e espere o retorno de um método antes de chamar o outro.

  * Se você quiser especificar o URI de canal a ser usado para notificações por push direcionadas do Centro de Desenvolvimento, chame a sobrecarga [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx). Por exemplo, talvez queira fazer isso se seu aplicativo já usa o Serviço de Notificação por Push do Windows (WNS) e você quer usar o mesmo URI do canal. Você deve primeiro criar um objeto [StoreServicesNotificationChannelParameters](https://msdns.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) e atribuir a propriedade [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) ao URI de canal.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

  >#### <a name="understanding-how-your-app-responds-when-the-user-launches-your-app"></a>Noções básicas sobre como seu app responde quando o usuário inicia seu app

  >Depois que seu aplicativo é registrado para receber notificações e você [envia uma notificação por push para clientes do seu aplicativo do Centro de Desenvolvimento](../publish/send-push-notifications-to-your-apps-customers.md), um dos seguintes pontos de entrada em seu aplicativo será chamado quando o usuário iniciar o aplicativo em resposta à sua notificação por push. Se você tiver algum código que deseja executar quando o usuário inicia seu aplicativo, adicione o código a um desses pontos de entrada em seu aplicativo.

  >* Se a notificação por push tiver um tipo de ativação em primeiro plano, substitua o método [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) da classe **App** em seu projeto e adicione o código a esse método.

  >* Se a notificação por push tiver um tipo de ativação em segundo plano, adicione código ao método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) da sua [tarefa em segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

  >Por exemplo, convém recompensar os usuários do seu aplicativo que adquiriram os complementos pagos em seu aplicativo com um complemento gratuito. Nesse caso, você pode enviar uma notificação por push para um [segmento do cliente](../publish/create-customer-segments.md) direcionado para esses usuários. Em seguida, você pode adicionar código para conceder uma [compra no aplicativo](in-app-purchases-and-trials.md) gratuita em um dos pontos de entrada listados acima.

## <a name="notify-dev-center-of-your-app-launch"></a>Notificar o Centro de Desenvolvimento sobre a inicialização do seu aplicativo

Se você selecionar a opção **Controlar taxa de inicialização do aplicativo** para uma notificação por push do Centro de Desenvolvimento, chame o método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) do ponto de entrada apropriado em seu aplicativo para notificar o Centro de Desenvolvimento que seu aplicativo foi iniciado em resposta a uma notificação por push.

Além disso, esse método retorna os argumentos de inicialização originais para o seu aplicativo. Depois que você optar por controlar a taxa de inicialização do aplicativo para uma notificação por push do Centro de Desenvolvimento, uma ID de rastreamento opaco será adicionada aos argumentos de inicialização para ajudar a controlar a inicialização do aplicativo no Centro de Desenvolvimento. Você deve transmitir os argumentos de inicialização do seu aplicativo para o método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx), e esse método enviará a ID de rastreamento para o Centro de Desenvolvimento, removerá a ID de rastreamento dos argumentos de inicialização e retornará os argumentos de inicialização originais ao seu código.

A maneira como você chama esse método depende do tipo de ativação da notificação por push direcionada:

* Se a notificação por push tiver um tipo de ativação em primeiro plano, chame esse método a partir da substituição do método [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) em seu aplicativo e transmita os argumentos que estão disponíveis no objeto [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) que é transmitido para esse método. O exemplo de código a seguir supõe que o arquivo de código tem instruções **using** para os namespaces **Microsoft.Services.Store.Engagement** e **Windows.ApplicationModel.Activation**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Se a notificação por push tiver um tipo de ativação em segundo plano, chame esse método a partir do método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) para sua [tarefa em segundo plano](../launch-resume/support-your-app-with-background-tasks.md) e transmita os argumentos que estão disponíveis no objeto [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) que é transmitido para esse método. O exemplo de código a seguir supõe que seu arquivo de código tem instruções **using** para os namespaces **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** e **Windows.UI.Notifications**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

## <a name="unregister-for-push-notifications"></a>Cancelar registro para notificações por push

Se você deseja que seu app pare de receber notificações por push direcionadas do Centro de Desenvolvimento do Windows, chame o método [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync).

> [!div class="tabbedCodeSnippets"]
[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Observe que esse método invalida o canal que está sendo usado para notificações para que o app não receba mais notificações por push de *nenhum* serviço. Depois de ser fechado, o canal nunca mais poderá ser usado novamente para nenhum serviço, incluindo as notificações por push direcionadas do Centro de Desenvolvimento do Windows e outras notificações usando o WNS. Para continuar enviando notificações por push para esse aplicativo, o aplicativo deve solicitar um novo canal.

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar notificações por push para clientes do seu aplicativo](../publish/send-push-notifications-to-your-apps-customers.md)
* [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [Como solicitar, criar e salvar um canal de notificação](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Dec16_HO1-->



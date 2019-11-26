---
Description: Você pode incentivar os clientes a deixar comentários iniciando o Hub de Feedback do seu aplicativo.
title: Iniciar o Hub de Feedback do seu app
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Hub de Feedback, iniciar
ms.localizationpriority: medium
ms.openlocfilehash: 589cad0c5055f0bfbade457e035702ee2cffd43d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259245"
---
# <a name="launch-feedback-hub-from-your-app"></a>Iniciar o Hub de Feedback do seu app

Você pode incentivar os clientes a deixar comentários adicionando um controle (como um botão) ao seu aplicativo da Plataforma Universal do Windows (UWP) que inicia o Hub de Feedbacks. Hub de Feedback é um aplicativo pré-instalado que oferece um local único para coletar feedback sobre o Windows e os aplicativos instalados. Todos os comentários do cliente enviados para seu aplicativo por meio do hub de comentários são coletados e apresentados a você no [relatório de comentários](../publish/feedback-report.md) no Partner Center, para que você possa ver os problemas, as sugestões e os votos que seus clientes enviaram em um relatório.

Para iniciar o Hub de Feedback do seu aplicativo, use uma API que seja fornecida pelo [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Recomendamos que você use essa API para iniciar o Hub de Feedback de um elemento de interface do usuário em seu app que siga nossas diretrizes de design.

> [!NOTE]
> O Hub de Feedback está disponível apenas em dispositivos que executam a versão 10.0.14271 ou posterior de um SO Windows 10 que se baseia em [famílias de dispositivos móveis e computadores](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Recomendamos que você mostre um controle de feedback no seu app apenas se o Hub de Feedback estiver disponível no dispositivo do usuário. O código neste tópico demonstra como fazer isso.

## <a name="how-to-launch-feedback-hub-from-your-app"></a>Como iniciar o Hub de Feedback do seu aplicativo

Para iniciar o Hub de Feedback do seu aplicativo:

1. [Instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.
6. No seu projeto, adicione o controle que você deseja mostrar aos usuários para iniciar o Hub de Feedback, como um botão. Recomendamos que você configure o controle da seguinte maneira:
  * Defina a fonte do conteúdo exibido no controle como **Segoe MDL2 Assets**.
  * Defina o texto no controle como o código de caractere Unicode hexadecimal E939. Este é o código de caractere do ícone de feedback recomendado na fonte **Segoe MDL2 Assets**.
  * Defina a visibilidade do controle como oculto.
    > [!NOTE]
    > Recomendamos que você oculte o controle de feedback por padrão e mostre-o no seu código de inicialização somente se o Hub de Feedback estiver disponível no dispositivo do usuário. A próxima etapa demonstra como fazer isso.

    O código a seguir demonstra a definição XAML de um [Botão](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) que é configurado conforme descrito acima.

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. Em seu código de inicialização para a página do aplicativo que hospeda o controle de feedback, use o método estático [IsSupported](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported) da classe [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) para determinar se o Hub de Feedback está disponível no dispositivo do usuário. O Hub de Feedback está disponível apenas em dispositivos que executam a versão 10.0.14271 ou posterior de um SO Windows 10 que se baseia em [famílias de dispositivos móveis e computadores](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

    Se essa propriedade retornar **true**, deixe o controle visível. O código a seguir demonstra como fazer isso para um [Botão](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button).

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > Embora o Hub de Feedback não seja compatível com dispositivos Xbox no momento, a propriedade **IsSupported** atualmente retorna **true** em dispositivos Xbox que executam a versão 10.0.14271 ou posterior do Windows 10. Isso é um problema conhecido que será corrigido em uma versão futura do Microsoft Store Services SDK.  

8. No manipulador de eventos que é executado quando o usuário clica no controle, obtenha um objeto [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) e chame o método [LaunchAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync) para iniciar o aplicativo do Hub de Feedback. Há duas sobrecargas para esse método: uma sem parâmetros e outra que aceita um dicionário de pares de chave e valor que contém os metadados que você deseja associar ao feedback. O exemplo a seguir demonstra como iniciar o Hub de Feedback no manipulador de eventos [Clique](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) para um [Botão](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button).

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>Recomendações de design para a interface do usuário de feedback

Para iniciar o Hub de Feedback, recomendamos adicionar um elemento de interface do usuário no aplicativo (como um botão) que exiba o ícone de comentários padrão a seguir da fonte Segoe MDL2 Assets e o código de caractere E939.

![Ícone de comentários](images/feedback_icon.PNG)

Também recomendamos usar uma ou mais das seguintes opções de posicionamento para vinculação ao Hub de Feedback no aplicativo.
* **Diretamente na barra de aplicativos**. Dependendo da implementação, convém usar apenas o ícone ou adicionar texto (conforme mostrado abaixo).

  ![Ícone de comentários](images/feedback_appbar_placement.png)

* **Nas configurações do aplicativo**. Essa é uma maneira mais sutil de dar acesso ao Hub de Feedback. No exemplo abaixo, o link Feedback é exibido como um dos links em Aplicativo.

  ![Ícone de comentários](images/feedback_settings_placement.png)

* **Em um submenu acionado por eventos**. Isso é útil quando você deseja consultar os clientes sobre uma pergunta específica antes de iniciar o Hub do Windows Feedback. Por exemplo, depois que seu aplicativo usar um determinado recurso, você pode fazer ao cliente uma pergunta específica sobre sua satisfação com esse recurso. Se o cliente optar por responder, seu aplicativo iniciará o Hub de Feedback.


## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de comentários](../publish/feedback-report.md)

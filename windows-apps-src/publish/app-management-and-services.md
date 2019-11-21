---
Description: Manage and view details related to each of your apps in Partner Center, and configure services such as A/B testing and maps.
title: Gerenciamento de apps e serviços
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260034"
---
# <a name="app-management-and-services"></a>Gerenciamento de apps e serviços

You can manage and view details related to each of your apps in [Partner Center](https://partner.microsoft.com/dashboard), and configure services such as notifications, A/B testing, and maps.

When working with an app in Partner Center, you'll see sections in the left navigation menu for **Services** and **App management**. Você pode expandir essas seções para acessar a funcionalidade descrita abaixo.

## <a name="services"></a>Serviços

A seção **Serviços** permite que você gerencie vários serviços diferentes para os seus aplicativos.

## <a name="xbox-live"></a>Xbox Live

If you are publishing a game, you can enable the [Xbox Live Creators Program](https://www.xbox.com/developers/creators-program) on this page. This lets you start configuring and testing Xbox Live features, and eventually publish your Xbox Live Creators Program game.

For more info, see [Get started with the Xbox Live Creators Program](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) and [Create a new Xbox Live Creators Program title and publish to the test environment](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Experimentação

Use a página **Experimentação** para criar e executar experimentos para os seus aplicativos da Plataforma Universal do Windows (UWP) com testes A/B. Em um teste A/B, você mede a eficácia de mudanças (ou variações) de recursos no seu aplicativo com alguns clientes antes de ativar as mudanças para todos os usuários.

Para saber mais, veja [Executar experimentos de aplicativos com testes A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Mapas

Para usar serviços de mapa em aplicativos destinados ao Windows 10 ou Windows 8.x, visite o [Centro de Desenvolvimento do Bing Mapas](https://www.bingmapsportal.com/). For info about how to request a maps authentication key from the Bing Maps Developer Center and add it to your app, see [Request a maps authentication key](../maps-and-location/authentication-key.md) for more info. 

Use the **Maps** page only for previously-published apps for Windows Phone 8.1 and earlier. To use map services in these apps, you'll need to request a map service application ID and a token to include in your app's code. When you click **Get token**, we'll generate a Map service Application ID (**ApplicationID**) and Map service Authentication Token (**AuthenticationToken**) for your app. Be sure to add these values to your code before you package and submit your app. Para saber mais, consulte [Como adicionar um controle de mapa a uma página (Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN).

## <a name="product-collections-and-purchases"></a>Compras e coleções de produtos

To use the Microsoft Store collection API and the Microsoft Store purchase API to access ownership information for apps and add-ons, you need to enter the associated Azure AD client IDs here. Observe que essas alterações podem levar até 16 horas para entrar em vigor.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Administrator consent

If your product integrates with Azure AD and calls APIs that request either [application permissions or delegated permissions](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) that require administrator consent, enter your Azure AD Client ID here. This lets administrators who acquire the app for their organization grant consent for your product to act on behalf of all users in the tenant.

For more info, see [Requesting consent for an entire tenant](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>Gerenciamento de aplicativo

A seção **Gerenciamento de aplicativo** permite a você visualizar detalhes de identidade e de pacote, bem como gerenciar os nomes do aplicativo.

## <a name="app-identity"></a>Identidade do aplicativo

Esta página mostra detalhes relacionados à identidade exclusiva do seu aplicativo dentro da loja, incluindo as URLs vinculadas aos detalhes de seu aplicativo.

Para saber mais, consulte [Exibir detalhes da identidade do aplicativo](view-app-identity-details.md).

## <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

É aqui que você pode exibir todos os nomes que reservou para o seu aplicativo. Você pode reservar mais nomes aqui ou excluir nomes que não esteja usando.

Para saber mais, consulte [Gerenciar nomes de aplicativo](manage-app-names.md).

## <a name="current-packages"></a>Pacotes atuais

Essa página permite que você visualize detalhes relacionados a todos os seus pacotes publicados.

> [!NOTE]
> Você não verá as informações aqui até o aplicativo ser publicado.

O nome, a versão e a arquitetura de cada pacote são mostrados. Clique em **Detalhes** para mostrar informações adicionais, idioma com suporte, recursos do aplicativo e tamanhos de arquivo. As informações que você vê de cada pacote podem ser diferentes, dependendo do seu sistema operacional de destino e de outros fatores. 

Os desenvolvedores com permissões de OEM também podem [gerar pacotes de pré-instalação](generate-preinstall-packages-for-oems.md) na página **Pacotes atuais**.

## <a name="wnsmpns"></a>WNS/MPNS

The **WNS/MPNS** section provides options to help you create and send notifications to your app's customers. 

> [!TIP]
> For UWP apps, we suggest using the **Notifications** feature in Partner Center. This feature lets you send notifications to all of your app's customers, or to a targeted subset of your Windows 10 customers who meet the criteria you’ve defined in a [customer segment](create-customer-segments.md). Para obter mais informações, consulte [Enviar notificações para clientes do seu aplicativo](send-push-notifications-to-your-apps-customers.md).

Depending on your app's package type and its specific requirements, you can also use one of the following options: 

-   **Serviços de Notificação por Push do Windows (WNS)** permite que você envie notificações do sistema, blocos, selos e atualizações brutas de seu próprio serviço de nuvem. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicativos Móveis do Microsoft Azure** permite que você envie notificações por push, autentique e gerencie usuários de aplicativo e armazene dados de aplicativo na nuvem. Para saber mais, consulte a [documentação de Aplicativos Móveis](https://docs.microsoft.com/azure/app-service-mobile/).

-   **Microsoft Push Notifications Service (MPNS)** can be used with previously published .xap packages for Windows Phone. Você pode enviar um número limitado de notificações não autenticadas sem fazer qualquer configuração aqui, embora seja recomendável usar notificações autenticadas para evitar limitações. If you're using MPNS, you'll need to upload a certificate to the field provided on the **WNS/MPNS** page. Para saber mais, veja [Configurando um serviço Web autenticado para enviar notificações por push para Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN).
 

 

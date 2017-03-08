---
author: jnHs
Description: "Se seu app usa o controle de anúncios ou exibe anúncios em banners ou vídeos intersticiais do Microsoft Advertising, use a página Monetização &gt; Monetizar com anúncios para gerenciar o uso de anúncios."
title: "Monetizar com anúncios"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e82431c9b39999af9fe19ac147a6c031b9a3edc3
ms.lasthandoff: 02/07/2017

---

# <a name="monetize-with-ads"></a>Monetizar com anúncios


Se seu app usa um controle **AdMediatorControl**, **AdControl** ou **InterstitialAd** para exibir anúncios intersticiais em forma de banner ou vídeo, use a página **Monetização** &gt; **Monetizar com anúncios** para gerenciar o uso de anúncios.

## <a name="windows-ad-mediation"></a>Controle de anúncios do Windows


Se seu app usa o controle de anúncios, use esta seção para definir as configurações de controle e adicionar parâmetros necessários para cada uma das redes de publicidade usadas por seu app. Para obter mais informações sobre as opções desta seção, consulte [Enviar seu app e configurar o controle de anúncios](https://msdn.microsoft.com/library/windows/apps/mt219689).

O controle de anúncios permite que você otimize a receita de publicidade no app controlando solicitações de anúncios em banner de várias redes de publicidade. Para obter mais informações sobre o controle de anúncios, consulte [Usar o controle de anúncios para maximizar a receita](https://msdn.microsoft.com/library/windows/apps/mt219691).

## <a name="coppa-compliance"></a>Conformidade com o COPPA

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve notificar a Microsoft caso seu app seja direcionado para crianças com menos de 13 anos. Se você usar o Centro de Desenvolvimento para indicar à Microsoft que seu app é direcionado a crianças com menos de 13 anos, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu app. Caso seu app seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA.

Para obter mais informações sobre as obrigações estabelecidas pelo COPPA, consulte [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).

## <a name="microsoft-affiliate-ads"></a>Anúncios de afiliada da Microsoft

Marque a caixa nesta seção se você quer mostrar anúncios de afiliadas da Microsoft em seu app. Se você marcar essa caixa, anúncios de produtos na Loja, incluindo música, jogos, filmes, apps, hardware e software, serão disponibilizados para seu app quando nenhum anúncio de outras redes de anúncios estiverem disponíveis. Quando os usuários clicam nos anúncios e produtos na Loja dentro de uma janela de atribuição determinado, receberão uma comissão sobre compras aprovadas.

Se você alterar essa seleção, não precisa publicar novamente seu app para que as alterações entrem em vigor. Para obter mais informações sobre anúncios de afiliada da Microsoft, consulte [Sobre anúncios de afiliada](about-affiliate-ads.md).

> **Observação**  se seu app usa a mediação de anúncios (ou seja, ele usa um **AdMediatorControl** para exibir anúncios), ele poderá mostrar anúncios de afiliada somente se as configurações de mediação de anúncio forem configuradas para mostrar anúncios da Microsoft.

## <a name="community-ads"></a>Anúncios de comunidade

Marque a caixa desta seção se você deseja promover seu app em apps de outros desenvolvedores. Se você marcar essa caixa e depois [criar uma campanha publicitária de comunidade](create-an-ad-campaign-for-your-app.md), seu app irá mostrar anúncios de apps publicados por outros desenvolvedores que também criaram campanhas de anúncios de comunidade e os anúncios de seus apps serão exibidos no seu app. Anúncios de comunidade são gratuitos, e eles são mostrados apenas quando nenhum anúncio de outras redes de anúncios estão disponíveis.

Se você alterar essa seleção, não precisa publicar novamente seu app para que as alterações entrem em vigor. Para obter mais informações sobre anúncios da comunidade, consulte [Sobre anúncios da comunidade](about-community-ads.md).

## <a name="microsoft-advertising-ad-units"></a>Unidades de anúncio do Microsoft Advertising

Use esta seção para criar uma unidade de anúncio do Microsoft Advertising. Você só precisa criar unidades de anúncio nos seguintes cenários:

-   Seu app mostra anúncios em banner do Microsoft Advertising usando um objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Seu app mostra anúncios em vídeos intersticiais do Microsoft Advertising usando um objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

Para criar uma unidade de anúncio para estes cenários:

1.  Dê um nome para a unidade de anúncio.
2.  Selecione o tipo de unidade de anúncio (**Banner** ou **Vídeo intersticial**).
3.  Selecione o tipo de dispositivo (**Celular** ou **Computador/Tablet**).
4.  Clique em **Criar unidade de anúncio**.

Suas unidades de anúncio aparecem em uma tabela na parte inferior desta seção. Para cada unidade de anúncio, você verá uma **ID do Aplicativo** e uma **ID da unidade de anúncio**. Para mostrar anúncios em seu app, você precisará usar estes valores em seu código:

-   Se o app mostrar anúncios em banner, atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) e [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de seu objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Se seu app mostrar anúncios em vídeos intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de seu objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

> **Observação**  Se seu app usa o controle de anúncios para mostrar anúncios em banner do Microsoft Advertising (ou seja, ele usa um objeto **AdMediatorControl**), você não precisará solicitar unidades de anúncio. Nesse cenário, as unidades de anúncio do Microsoft Advertising são geradas automaticamente para você.

 

 

 


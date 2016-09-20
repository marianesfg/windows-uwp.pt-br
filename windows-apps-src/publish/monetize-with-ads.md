---
author: jnHs
Description: "Se seu aplicativo usa o controle de anúncios ou exibe anúncios em banners ou vídeos intersticiais do Microsoft Advertising, use a página Monetização &gt; Monetizar com anúncios para gerenciar o uso de anúncios."
title: "Monetizar com anúncios"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 97eeeedb9e73b6c67abe6e2ff8cadbc744a6a7c4

---

# Monetizar com anúncios


Se seu aplicativo usa um controle **AdMediatorControl**, **AdControl** ou **InterstitialAd** para exibir anúncios intersticiais em forma de banner ou vídeo, use a página **Monetização**&gt;**Monetizar com anúncios** para gerenciar o uso de anúncios.

## Controle de anúncios do Windows


Se seu aplicativo usa o controle de anúncios, use esta seção para definir as configurações de controle e adicionar parâmetros necessários para cada uma das redes de publicidade usadas por seu aplicativo. Para obter mais informações sobre as opções desta seção, consulte [Enviar seu aplicativo e configurar o controle de anúncios](https://msdn.microsoft.com/library/windows/apps/mt219689).

O controle de anúncios permite que você otimize a receita de publicidade no aplicativo controlando solicitações de anúncios em banner de várias redes de publicidade. Para obter mais informações sobre o controle de anúncios, consulte [Usar o controle de anúncios para maximizar a receita](https://msdn.microsoft.com/library/windows/apps/mt219691).

## Conformidade com o COPPA

Para fins do Children's Online Privacy Protection Act (“COPPA”), você deve notificar a Microsoft caso seu aplicativo seja direcionado para crianças com menos de 13 anos. Se você usar o Centro de Desenvolvimento para indicar à Microsoft que seu aplicativo é direcionado a crianças com menos de 13 anos, a Microsoft seguirá as etapas para desabilitar seus serviços de publicidade comportamental ao oferecer um anúncio em seu aplicativo. Caso seu aplicativo seja direcionado para crianças com menos de 13 anos, você tem determinadas obrigações segundo COPPA.

Para obter mais informações sobre as obrigações estabelecidas pelo COPPA, consulte [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).

## Anúncios de afiliada da Microsoft

Marque a caixa nesta seção se você quer mostrar anúncios de afiliadas da Microsoft em seu aplicativo. Se você marcar essa caixa, anúncios de produtos na Loja, incluindo música, jogos, filmes, aplicativos, hardware e software, serão disponibilizados para seu aplicativo quando nenhum anúncio de outras redes de anúncios estiverem disponíveis. Quando os usuários clicam nos anúncios e produtos na Loja dentro de uma janela de atribuição determinado, receberão uma comissão sobre compras aprovadas.

Se você alterar essa seleção, não precisa publicar novamente seu aplicativo para que as alterações entrem em vigor. Para obter mais informações sobre anúncios de afiliada da Microsoft, consulte [Sobre anúncios de afiliada](about-affiliate-ads.md).

> 
            **Observação** se seu aplicativo usa a mediação de anúncios (ou seja, ele usa um **AdMediatorControl** para exibir anúncios), ele poderá mostrar anúncios de afiliada somente se as configurações de mediação de anúncio forem configuradas para mostrar anúncios da Microsoft.

## Anúncios de comunidade

Marque a caixa desta seção se você deseja promover seu aplicativo em aplicativos de outros desenvolvedores. Se você marcar essa caixa e depois [criar uma campanha publicitária de comunidade](create-an-ad-campaign-for-your-app.md), seu aplicativo irá mostrar anúncios de aplicativos publicados por outros desenvolvedores que também criaram campanhas de anúncios de comunidade e os anúncios de seus aplicativos serão exibidos no seu aplicativo. Anúncios de comunidade são gratuitos, e eles são mostrados apenas quando nenhum anúncio de outras redes de anúncios estão disponíveis.

Se você alterar essa seleção, não precisa publicar novamente seu aplicativo para que as alterações entrem em vigor. Para obter mais informações sobre anúncios da comunidade, consulte [Sobre anúncios da comunidade](about-community-ads.md).

## Unidades de anúncio do Microsoft Advertising

Use esta seção para criar uma unidade de anúncio do Microsoft Advertising. Você só precisa criar unidades de anúncio nos seguintes cenários:

-   Seu aplicativo mostra anúncios em banner do Microsoft Advertising usando um objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Seu aplicativo mostra anúncios em vídeos intersticiais do Microsoft Advertising usando um objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

Para criar uma unidade de anúncio para estes cenários:

1.  Dê um nome para a unidade de anúncio.
2.  Selecione o tipo de unidade de anúncio (**Banner** ou **Vídeo intersticial**).
3.  Selecione o tipo de dispositivo (**Celular** ou **Computador/Tablet**).
4.  Clique em **Criar unidade de anúncio**.

Suas unidades de anúncio aparecem em uma tabela na parte inferior desta seção. Para cada unidade de anúncio, você verá uma **ID do Aplicativo** e uma **ID da unidade de anúncio**. Para mostrar anúncios em seu aplicativo, você precisará usar estes valores em seu código:

-   Se o aplicativo mostrar anúncios em banner, atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) e [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de seu objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Se seu aplicativo mostrar anúncios em vídeos intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de seu objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

> 
            **Observação** Se seu aplicativo usa o controle de anúncios para mostrar anúncios em banner do Microsoft Advertising (ou seja, ele usa um objeto **AdMediatorControl**), você não precisará solicitar unidades de anúncio. Nesse cenário, as unidades de anúncio do Microsoft Advertising são geradas automaticamente para você.

 

 

 



<!--HONumber=Jun16_HO4-->



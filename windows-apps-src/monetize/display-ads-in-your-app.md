---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: O SDK de Microsoft Store Engagement and Monetization oferece várias maneiras de monetizar seu aplicativo com anúncios.
title: Apresentar anúncios em seus aplicativos
---

# Apresentar anúncios em seus aplicativos


\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O [SDK de Microsoft Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) oferece várias maneiras de monetizar seus aplicativos com anúncios.

## Exibir anúncios em faixa e anúncios intersticiais em vídeo usando as bibliotecas do Microsoft Advertising

Ganhe mais dinheiro com seus aplicativos do Windows, incluindo anúncios em faixa e intersticiais em vídeo. Os anúncios são exibidos em aplicativos do Windows para computadores, tablets e telefones. Você pode monitorar o desempenho do anúncio em tempo real usando o painel do Centro de Desenvolvimento do Windows.

Para incluir anúncios em seus aplicativos, use os controles **AdControl** e **InterstitialAd** nas bibliotecas de publicidade que são distribuídas no SDK de Microsoft Store Engagement and Monetization. Você pode usar esses controles para mostrar anúncios em faixa e intersticiais em vídeos da Microsoft em seus aplicativos XAML ou HTML/JavaScript para Windows 10, Windows 8.1, Windows Phone 8.1 e Windows Phone 8.

Para obter mais informações, consulte [Exibir anúncios usando as bibliotecas do Microsoft Advertising](display-ads-using-the-microsoft-advertising-libraries.md). Depois que você publicar um aplicativo com anúncios, use o [relatório de desempenho de publicidade](../publish/advertising-performance-report.md) para controlar o desempenho dos anúncios.                                           

## Use o controle de anúncios para anúncios em faixa de várias redes de publicidade

Você pode usar a classe **AdMediatorControl** em seus aplicativos XAML para otimizar sua receita de publicidade, exibindo anúncios em faixa de várias redes de publicidade. Depois de adicionar esse controle ao seu aplicativo, você pode definir suas configurações de mediação de anúncios no painel do Centro de Desenvolvimento do Windows e mediar as solicitações de anúncios em faixa das redes de publicidade que você escolher. Para obter mais informações, consulte [Usar a mediação de anúncios para maximizar a receita de anúncios](use-ad-mediation-to-maximize-revenue.md).

## Diferenças entre as bibliotecas do Microsoft Advertising e a mediação de anúncios

O SDK de Microsoft Store Engagement and Monetization inclui as bibliotecas de mediação de anúncios e do Microsoft Advertising. Entretanto, essas bibliotecas fornecem classes diferentes e têm finalidades distintas.

* Use as classes **AdControl** e **InterstitialAd** nas bibliotecas do Microsoft Advertising se quiser exibir anúncios em banner ou intersticiais em vídeo em um aplicativo XAML ou JavaScript.
* Use a classe **AdMediatorControl** nas bibliotecas de mediação de anúncios, se você quiser exibir anúncios em faixa de várias redes de publicidade em um aplicativo XAML.

Para obter mais informações, consulte [Qual é a diferença: AdMediatorControl ou AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Tópicos relacionados

* [SDK de Microsoft Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [Monetize seus aplicativo com anúncios]( http://go.microsoft.com/fwlink/p/?LinkId=699559)


<!--HONumber=May16_HO2-->



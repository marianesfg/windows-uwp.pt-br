---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Saiba como adicionar valores de ID da unidade de anúncios e do aplicativo do painel do Centro de Desenvolvimento do Windows ao seu aplicativo antes de enviá-lo para a Loja."
title: "Configurar unidades de anúncios em seu aplicativo"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, unidades de anúncio"
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anúncios em seu aplicativo




Quando você usa um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para exibir anúncios em seu aplicativo, você deve especificar um ID do aplicativo e de unidade de anúncios. Enquanto você estiver desenvolvendo seu aplicativo, use os valores de [ID do aplicativo de teste e ID da unidade de anúncios](test-mode-values.md) para ver como seu aplicativo renderiza anúncios durante o teste.

Depois que você terminar o teste de seu aplicativo e estiver pronto para enviá-lo para o Centro de Desenvolvimento do Windows, atualize o código do aplicativo para usar os valores de ID do aplicativo e ID da unidade de anúncios do [painel do Centro de Desenvolvimento do Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Se você tentar usar valores de teste em seu aplicativo dinâmico, seu aplicativo não receberá anúncios ativos.

Para configurar as unidades de anúncios e ID do aplicativo para o seu aplicativo dinâmico:

1.  No painel do Centro de Desenvolvimento do Windows, selecione seu aplicativo e, em seguida, clique em **Monetização > Monetizar com anúncios**.

2.  Na seção **Unidades de anúncios do Microsoft Advertising** nesta página, crie uma unidade de anúncios. Para o tipo de unidade de anúncio, selecione uma das opções a seguir:

  * Se você estiver usando um **AdControl** em seu aplicativo para mostrar anúncios em faixa, selecione **Banner** para o tipo de unidade publicitária.

  * Se você estiver usando um **InterstitialAd** em seu aplicativo para mostrar anúncios intersticiais de vídeo ou anúncios de banner intersticiais, selecione **Vídeo intersticial** ou **Banner intersticial** (certifique-se de selecionar a opção adequada para o tipo de anúncio intersticial você quer mostrar).

3.  Para cada unidade de anúncio gerada, você verá um **ID do Aplicativo** e um **ID da unidade de anúncios** nessa página. Para mostrar anúncios em seu aplicativo, você precisará usar esses valores no código do aplicativo:

  * Se o app mostrar anúncios em banner, atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) e [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) de seu objeto **AdControl**. Para saber mais, consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md) e [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md).

  * Se seu app mostrar anúncios intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) de seu objeto **InterstitialAd**. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

Para obter mais informações sobre a página **Monetizar com anúncios**, consulte [Monetizar com anúncios](../publish/monetize-with-ads.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Valores de modo de teste](test-mode-values.md)
* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md)
* [Anúncios intersticiais](interstitial-ads.md)


 

 

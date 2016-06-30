---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Saiba como adicionar valores de ID da unidade de anúncios e do aplicativo do painel do Centro de Desenvolvimento do Windows ao seu aplicativo antes de enviá-lo para a Loja."
title: "Configurar unidades de anúncios em seu aplicativo"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 96c09de9321f67dc26cc3538f2655bd598f134f9


---

# Configurar unidades de anúncios em seu aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Quando você usa um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para exibir anúncios em seu aplicativo, você deve especificar um ID do aplicativo e de unidade de anúncios. Enquanto você estiver desenvolvendo seu aplicativo, use os valores de [ID do aplicativo de teste e ID da unidade de anúncios](test-mode-values.md) para ver como seu aplicativo renderiza anúncios durante o teste.

Depois que você terminar o teste de seu aplicativo e estiver pronto para enviá-lo para o Centro de Desenvolvimento do Windows, atualize o código do aplicativo para usar os valores de ID do aplicativo e ID da unidade de anúncios do [painel do Centro de Desenvolvimento do Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Se você tentar usar valores de teste em seu aplicativo dinâmico, seu aplicativo não receberá anúncios ativos.

Para configurar as unidades de anúncios e ID do aplicativo para o seu aplicativo dinâmico:

1.  No painel do Centro de Desenvolvimento do Windows, selecione seu aplicativo e, em seguida, clique em **Monetização > Monetizar com anúncios**.
2.  Na seção **Unidades de anúncios do Microsoft Advertising** nesta página, crie uma unidade de anúncios. Para o tipo de unidade de anúncios, selecione **Banner** se estiver usando um **AdControl**, ou selecione **Vídeo intersticial** se estiver usando um **InterstitialAd**. Para obter mais informações sobre esta página, consulte [Monetizar com anúncios](../publish/monetize-with-ads.md).

3.  Para cada unidade de anúncio gerada, você verá um **ID do Aplicativo** e um **ID da unidade de anúncios** nessa página. Para mostrar anúncios em seu aplicativo, você precisará usar esses valores no código do aplicativo:

    * Se o aplicativo mostrar anúncios em banner, atribua esses valores às propriedades **ApplicationId** e **AdUnitId** de seu objeto **AdControl**.

    * Se seu aplicativo mostrar anúncios em vídeos intersticiais, passe esses valores para o método **RequestAd** de seu objeto **InterstitialAd**.

> **Importante**   Se seu aplicativo usa o controle de anúncios para mostrar anúncios em banner do Microsoft Advertising (ou seja, ele usa um objeto **AdMediatorControl**), você não precisará solicitar unidades de anúncios. Nesse cenário, as unidades de anúncios são geradas automaticamente para você. Para obter mais informações, consulte [Qual é a diferença: AdMediatorControl ou AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

 

## Tópicos relacionados

[Valores de modo de teste](test-mode-values.md)


 

 



<!--HONumber=Jun16_HO4-->



---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Use os valores de ID da unidade de anúncios e a ID do aplicativo de teste deste artigo para ver como seu aplicativo renderiza anúncios durante o teste."
title: Valores de modo de teste
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: e1462ae48e8aae8f5ed0e5a7e46a6660bf33e786

---

# Valores de modo de teste




Quando você usa um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para exibir anúncios em seu aplicativo, você deve especificar um ID do aplicativo e de unidade de anúncios. Enquanto você estiver desenvolvendo seu aplicativo, use os valores de ID do aplicativo e a ID da unidade de anúncios de teste para ver como seu aplicativo renderiza anúncios durante o teste.


Se você tentar usar valores de teste em seu aplicativo depois de publicá-lo, seu aplicativo dinâmico não receberá anúncios. Para receber anúncios em seu aplicativo publicado, você deve atualizar seu código para usar um ID do aplicativo um ID da unidade de anúncios fornecidos pelo painel do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).
 

Veja a seguir os valores de teste a serem usados para anúncios intersticiais em vídeo e em banner.

* Para anúncios intersticiais em vídeo:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Para anúncios em banner:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Importante**   O tamanho de um anúncio em tempo real é definido pelas propriedades **Width** e **Height** do **AdControl**. Para obter melhores resultados, certifique-se de que as propriedades **Width** e **Height** em seu código correspondam a [tamanhos aceitos para anúncios em banner](supported-ad-sizes-for-banner-ads.md). As propriedades **Width** e **Height** não mudarão com base no tamanho de um anúncio em tempo real.



 

 



<!--HONumber=Aug16_HO3-->



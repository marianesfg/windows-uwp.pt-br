---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Use os valores de ID da unidade de anúncios e a ID do aplicativo de teste deste artigo para ver como seu aplicativo renderiza anúncios durante o teste."
title: Valores de modo de teste
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, testes"
ms.openlocfilehash: ec5b1c1723a7f58b20234d703fc786647268a3b3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="test-mode-values"></a>Valores de modo de teste

Quando você usa um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para exibir anúncios em seu aplicativo, você deve especificar um ID do aplicativo e de unidade de anúncios. Enquanto você estiver desenvolvendo seu aplicativo, use os valores de ID do aplicativo e a ID da unidade de anúncios de teste para ver como seu aplicativo renderiza anúncios durante o teste.

Se você tentar usar valores de teste em seu aplicativo depois de publicá-lo, seu aplicativo dinâmico não receberá anúncios. Para receber anúncios em seu aplicativo publicado, você deve atualizar seu código para usar um ID do aplicativo um ID da unidade de anúncios fornecidos pelo painel do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Configurar unidades de publicidade no aplicativo](set-up-ad-units-in-your-app.md).
 
Veja a seguir os valores de teste a serem usados para anúncios intersticiais e em banner.

* Para os anúncios intersticiais:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Sistema operacional de destino</th>
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>teste</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>Windows 8.x e Windows Phone 8.x</p></td>
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Para anúncios em banner:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Sistema operacional de destino</th>
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>teste</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>Windows 8.x e Windows Phone 8.x</p></td>
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Importante**&nbsp;&nbsp;Para um **AdControl**, o tamanho de um anúncio em tempo real é definido pelas propriedades **Width** e **Height**. Para obter melhores resultados, certifique-se de que as propriedades **Width** e **Height** em seu código correspondam a [tamanhos aceitos para anúncios em banner](supported-ad-sizes-for-banner-ads.md). As propriedades **Width** e **Height** não mudarão com base no tamanho de um anúncio em tempo real.


 

 

---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Saiba como iniciar um anúncio intersticial em HTML/JavaScript.
title: Código de exemplo de anúncio intersticial em JavaScript
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, intersticial, javascript, código de exemplo
ms.localizationpriority: medium
ms.openlocfilehash: 98ba09b88c96b0f0afb4ba5a786d31442fd9da05
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8213218"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de exemplo de anúncio intersticial em JavaScript

Este tópico fornece o exemplo de código completo para um aplicativo básico da Plataforma Universal do Windows (UWP) em JavaScript e HTML que mostra um anúncio intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos em HTML e JavaScript em um aplicativo básico que mostra um anúncio intersticial. Para usar esses exemplos, copie esse código para um projeto JavaScript **Aplicativo WinJS (Universal do Windows)** no Visual Studio.

Este aplicativo de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Os arquivos main.js e index.html gerados pelo Visual Studio foram modificados e são mostrados abaixo. O arquivo script.js mostrado abaixo contém a maior parte do código no exemplo, e você deve adicionar esse arquivo à pasta **js** no projeto.

Substitua os valores do ```applicationId``` e ```adUnitId``` variáveis por valores ativos do Partner Center antes de enviar seu aplicativo para a loja. Para obter mais informações, consulte [Configurar unidades publicitárias no app](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para alterar este exemplo e mostrar um anúncio intersticial de banner em vez de um anúncio intersticial em vídeo, passe o valor **InterstitialAdType.display** para o primeiro parâmetro do método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) em vez de **InterstitialAdType.video**. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 

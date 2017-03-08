---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: "Saiba como iniciar um anúncio intersticial em HTML/JavaScript."
title: "Código de exemplo de anúncio intersticial em JavaScript"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, intersticial, javascript, código de exemplo"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 91a54bdd2e41b3e7df0ee0aad32448ab9ed66ac0
ms.lasthandoff: 02/07/2017

---

# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de exemplo de anúncio intersticial em JavaScript

Este tópico fornece o exemplo de código completo para um app básico da Plataforma Universal do Windows (UWP) em JavaScript e HTML que mostra um anúncio intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos em HTML e JavaScript em um app básico que mostra um anúncio intersticial. Para usar esses exemplos, copie esse código para um projeto JavaScript **Aplicativo WinJS (Universal do Windows)** no Visual Studio 2015.

Este app de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Os arquivos main.js e index.html gerados pelo Visual Studio foram modificados e são mostrados abaixo. O arquivo script.js mostrado abaixo contém a maior parte do código no exemplo, e você deve adicionar esse arquivo à pasta **js** no projeto.

>**Observação para o Windows 8.x e o Windows Phone 8.1**&nbsp;&nbsp;Se o projeto destinar-se ao Windows 8.1 ou ao Windows Phone 8.1, o arquivo HTML padrão no projeto se chamará default.html, e não index.html, e o arquivo JavaScript padrão no projeto se chamará default.js, e não main.js.

Substitua os valores das variáveis ```applicationId``` e ```adUnitId``` por valores ativos do Centro de Desenvolvimento do Windows antes de enviar o app para a Loja. Para obter mais informações, consulte [Configurar unidades de anúncio no app](set-up-ad-units-in-your-app.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

<span/>
>**Observação para o Windows 8.x e o Windows Phone 8.1**&nbsp;&nbsp;Se o projeto destinar-se ao Windows 8.1 ou ao Windows Phone 8.1, substitua a linha ```<script src="//Microsoft.Advertising.JavaScript/ad.js"></script>``` no exemplo por ```<script src="/MSAdvertisingJS/ads/ad.js"></script>```.

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 


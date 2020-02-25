---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Saiba como iniciar um anúncio intersticial em HTML/JavaScript.
title: Código de exemplo de anúncio intersticial em JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, intersticial, javascript, código de exemplo
ms.localizationpriority: medium
ms.openlocfilehash: a863e7e0da2e932ee28784cef6cef0f651fe8d2c
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507030"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de exemplo de anúncio intersticial em JavaScript

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Este tópico fornece o exemplo de código completo para um aplicativo básico da Plataforma Universal do Windows (UWP) em JavaScript e HTML que mostra um anúncio intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos em HTML e JavaScript em um aplicativo básico que mostra um anúncio intersticial. Para usar esses exemplos, copie esse código para um projeto JavaScript **Aplicativo WinJS (Universal do Windows)** no Visual Studio.

Este aplicativo de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Os arquivos main.js e index.html gerados pelo Visual Studio foram modificados e são mostrados abaixo. O arquivo script.js mostrado abaixo contém a maior parte do código no exemplo, e você deve adicionar esse arquivo à pasta **js** no projeto.

Substitua os valores das variáveis ```applicationId``` e ```adUnitId``` com valores dinâmicos do Partner Center antes de enviar seu aplicativo para a loja. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

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

* [Amostras de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 

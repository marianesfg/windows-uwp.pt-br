---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Saiba como iniciar um anúncio intersticial usando c#.
title: Código exemplo de anúncio intersticial em C#
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anúncios, publicidade, intersticial, c#, código de exemplo
ms.localizationpriority: medium
ms.openlocfilehash: 195f13d3a51925925d320b87cd0142d14d449226
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5405385"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de exemplo de anúncio intersticial em C\# #  

Este tópico fornece o exemplo de código completo para um aplicativo básico da Plataforma Universal do Windows (UWP) em C# e XAML que mostra um anúncio de vídeo intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos MainPage.xaml e MainPage.xaml.cs em um aplicativo básico que mostra um anúncio intersticial. Para usar esses exemplos, copie o código para um projeto **Aplicativo em Branco (Universal do Windows)** em Visual C# no Visual Studio.

Este aplicativo de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Substitua os valores dos campos ```myAppId``` e ```myAdUnitId``` por valores ativos do Centro de Desenvolvimento do Windows antes de enviar o aplicativo para a Loja. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para alterar este exemplo e mostrar um anúncio intersticial de banner em vez de um anúncio intersticial em vídeo, passe o valor **AdType.Display** para o primeiro parâmetro do método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) em vez de **AdType.Video**. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
 

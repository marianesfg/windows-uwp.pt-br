---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "Saiba como iniciar um anúncio intersticial usando c#."
title: "Código de exemplo de anúncio intersticial em C#"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, intersticial, c#, código de exemplo"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4c57cf4909028d5aa81c75d9e1b6f1bf28d41ad7
ms.lasthandoff: 02/07/2017

---

# <a name="interstitial-ad-sample-code-in-c"></a>Código de exemplo de anúncio intersticial em C\# #  

Este tópico fornece o exemplo de código completo para um app básico da Plataforma Universal do Windows (UWP) em C# e XAML que mostra um anúncio intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos MainPage.xaml e MainPage.xaml.cs em um app básico que mostra um anúncio intersticial. Para usar esses exemplos, copie o código para um projeto **Aplicativo em Branco (Universal do Windows)** em Visual C# no Visual Studio 2015.

Este app de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Substitua os valores dos campos ```myAppId``` e ```myAdUnitId``` por valores ativos do Centro de Desenvolvimento do Windows antes de enviar o app para a Loja. Para obter mais informações, consulte [Configurar unidades de anúncio no app](set-up-ad-units-in-your-app.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
 


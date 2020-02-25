---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Saiba como iniciar um anúncio intersticial usando c#.
title: Código de exemplo de anúncio intersticial em C#
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, intersticial, c#, código de exemplo
ms.localizationpriority: medium
ms.openlocfilehash: 9d908151e30510977669f2bc101575754a946560
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507040"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de exemplo do intersticial AD em C\# #  

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Este tópico fornece o exemplo de código completo para um aplicativo básico da Plataforma Universal do Windows (UWP) em C# e XAML que mostra um anúncio de vídeo intersticial. Para obter instruções passo a passo que mostram como configurar o projeto para usar esse código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Exemplo de código

Esta seção mostra o conteúdo dos arquivos MainPage.xaml e MainPage.xaml.cs em um aplicativo básico que mostra um anúncio intersticial. Para usar esses exemplos, copie o código para um projeto **Aplicativo em Branco (Universal do Windows)** em Visual C# no Visual Studio.

Este aplicativo de exemplo usa dois botões para solicitar e, em seguida, iniciar um anúncio intersticial. Substitua os valores dos campos ```myAppId``` e ```myAdUnitId``` com valores dinâmicos do Partner Center antes de enviar seu aplicativo para a loja. Para obter mais informações, consulte [Configurar unidades de anúncio no aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para alterar este exemplo e mostrar um anúncio intersticial de banner em vez de um anúncio intersticial em vídeo, passe o valor **AdType.Display** para o primeiro parâmetro do método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) em vez de **AdType.Video**. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Tópicos relacionados

* [Amostras de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 

---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "O Microsoft Store Services SDK oferece várias maneiras de monetizar seu aplicativo com anúncios."
title: "Apresentar anúncios em seu app"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, barra de notificação, intersticial"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b6343d8a011a3e62a3b714c7dab280c9d9a8f81d
ms.lasthandoff: 02/07/2017

---

# <a name="display-ads-in-your-app"></a>Apresentar anúncios em seu app


A Plataforma Universal do Windows (UWP) e a Windows Store oferecem várias maneiras de monetizar seu aplicativo com anúncios.

## <a name="display-banner-and-video-interstitial-ads-using-the-microsoft-advertising-libraries"></a>Exibir anúncios em faixa e anúncios intersticiais em vídeo usando as bibliotecas do Microsoft Advertising

Ganhe mais dinheiro com seus aplicativos UWP e seus aplicativos para Windows 8.1 e Windows Phone 8.x, incluindo a faixa e anúncios intersticiais em vídeo. Os anúncios aparecem em aplicativos do Windows para PCs, tablets e telefones. Você pode monitorar o desempenho do anúncio em tempo real usando o [relatório de desempenho de anúncios](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento do Windows.

Para incluir esses tipos de anúncios em seus aplicativos, use os controles **AdControl** e **InterstitialAd** nas bibliotecas de publicidade que são distribuídas no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (para aplicativos UWP) e o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicativos do Windows 8.1 e Windows Phone 8.x).


Os tópicos a seguir fornecem informações sobre tarefas comuns envolvendo as bibliotecas de publicidade do Windows.

|  Tarefa    | Tópico |               
|----------|-------|
| Instale e comece a usar as bibliotecas do Microsoft Advertising.     | Consulte [Introdução às bibliotecas do Microsoft Advertising](get-started-with-microsoft-advertising-libraries.md).        |
| Mostrar anúncios em faixa em seu aplicativo XAML/C#.     | Consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md).        |
| Mostrar anúncios em faixa em seu aplicativo HTML/JavaScript.     | Consulte [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md).        |
| Mostre anúncios em barra de notificação em seu aplicativo Silverlight para Windows Phone 8.x.     | Consulte [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).        |
| Mostre um anúncio intersticial em seu aplicativo.     | Consulte [Anúncios intersticiais](interstitial-ads.md).       |
| Adicione anúncios ao conteúdo de vídeo em um aplicativo da Plataforma Universal do Windows (UWP) que foi escrito em JavaScript com HTML.   |  Consulte [Adicionar anúncios ao conteúdo de vídeo em HTML 5 e JavaScript](add-advertisements-to-video-content.md).  |
| Baixe os projetos de exemplo que demonstram como adicionar faixa e anúncios intersticiais a aplicativos.     |Consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).       |
| Manipular erros de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em seu aplicativo.     | Consulte [Tratamento de erros](error-handling-with-advertising-libraries.md) e o guia passo a passo em [Tratamento de erros de AdControl](adcontrol-error-handling.md).       |
| Relate um bug nas bibliotecas do Microsoft Advertising.     | Visite a [página de suporte](https://go.microsoft.com/fwlink/p/?LinkId=331508).        |
| Obtenha suporte da comunidade.     | Visite o [fórum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |

                            

## <a name="use-ad-mediation-for-banner-ads-windows-81-and-windows-phone-8x"></a>Use o controle de anúncios para anúncios em banner (Windows 8.1 e Windows Phone 8.x)

Para aplicativos do Windows 8.1 e Windows Phone 8.x, você pode usar a classe **AdMediatorControl** para otimizar sua receita de publicidade, exibindo anúncios em faixa de várias redes de publicidade. Depois de adicionar esse controle ao seu aplicativo, você pode definir suas configurações de mediação de anúncios no painel do Centro de Desenvolvimento do Windows e mediar as solicitações de anúncios em faixa das redes de publicidade que você escolher. Para saber mais, consulte [Usar o controle de anúncios para maximizar a receita de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Observação**&nbsp;&nbsp;A mediação de anúncios usando a classe **AdMediatorControl** atualmente não é permitida para aplicativos UWP usando o Windows 10. O controle do servidor estará disponível em breve para aplicativos UWP usando as mesmas APIs para anúncios em faixa (**AdControl**) e anúncios intersticiais em vídeo (**InterstitialAd**). Para obter orientação sobre a migração do **AdMediatorControl** para **AdControl** em seu aplicativo UWP, consulte [Como migrar do AdMediatorControl para o AdControl para aplicativos UWP](migrate-from-admediatorcontrol-to-adcontrol.md).

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Suporte de publicidade para projetos do Windows Phone 8.x Silverlight

Não há suporte para alguns cenários de desenvolvedor em projetos do Windows Phone 8.x Silverlight. Para obter mais informações, consulte a seguinte tabela.

|  Versão da plataforma  |  Projetos existentes    |   Novos projetos  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  Se você tem um projeto existente do Windows Phone 8.0 Silverlight que já usa um **AdControl** ou **AdMediatorControl** de uma versão anterior do SDK do Universal Ad Client ou o SDK do Microsoft Advertising e esse aplicativo já está publicado na Windows Store, você pode modificar e recriar o projeto e você pode depurar ou testar suas alterações em um dispositivo. Não há suporte para depuração ou teste do projeto no emulador.  |  Sem suporte.  |
| Windows Phone 8.1 Silverlight    |  Se você tiver um projeto existente do Windows Phone 8.1 Silverlight que usa um **AdControl** ou **AdMediatorControl** de um SDK anterior, você pode modificar e recompilar o projeto. No entanto, para depurar ou testar o aplicativo, você deve executar o aplicativo no emulador e usar [valores de modo de teste](test-mode-values.md) para ID do aplicativo e para a ID da unidade de anúncios. Não há suporte para depuração ou teste do aplicativo em um dispositivo.  |   Você pode adicionar um **AdControl** ou **AdMediatorControl** para um novo projeto do Windows Phone 8.1 Silverlight. No entanto, para depurar ou testar o aplicativo, você deve executar o aplicativo no emulador e usar [valores de modo de teste](test-mode-values.md) para ID do aplicativo e para a ID da unidade de anúncios. Não há suporte para depuração ou teste do aplicativo em um dispositivo. |

## <a name="related-topics"></a>Tópicos relacionados

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [Monetize seus aplicativo com anúncios](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Relatório de desempenho de anúncios](../publish/advertising-performance-report.md)


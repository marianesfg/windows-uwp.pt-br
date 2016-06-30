---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: "Saiba mais sobre as diferenças entre a classe AdControl nas bibliotecas do Microsoft Advertising e a classe AdMediatorControl nas bibliotecas de controle de anúncios."
title: "Qual é a diferença – AdMediatorControl ou AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: 291e1c4d707e8987d29ae5840248918543d7d12a

---

# Qual é a diferença: AdMediatorControl ou AdControl


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Use as bibliotecas do Microsoft Advertising para XAML e JavaScript em seu aplicativo quando você quiser exibir anúncios intersticiais em faixa ou vídeo da Microsoft. Essas bibliotecas são diferentes das bibliotecas de controle de anúncios, que você usa para exibir anúncios de várias redes de publicidade. Use essa documentação para as bibliotecas do Microsoft Advertising (JavaScript e XAML) quando:

* Você estiver usando [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) em um aplicativo XAML ou JavaScript em vez do **AdMediatorControl**.
* Você estiver procurando informações de referência para a API **AdControl** subjacente usada pela controle de anúncios.

As bibliotecas do Microsoft Advertising e as bibliotecas de controle de anúncios estão incluídas no SDK de Microsoft Store Engagement and Monetization. Para obter mais informações sobre como instalar este SDK e as diferentes bibliotecas de publicidade Microsoft que estão incluídas nesse SDK, consulte [Instalar as bibliotecas do Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

>**Observação** Para mostrar anúncios intersticiais, use o controle **InterstitialAd**. O **AdControl** e **AdMediatorControl** não podem mostrar anúncios intersticiais. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

 

## Controle de anúncios


A maneira recomendada de mostrar anúncios da Microsoft (anúncios não intersticiais) é usar o **AdMediatorControl** em seu aplicativo. O **AdMediatorControl** mostra anúncios em faixa de várias redes de publicidade.

Ao usar o **AdMediatorControl** em um projeto, você deve especificar quais redes de publicidade usar com o recurso **Serviços Conectados** do Visual Studio. O Visual Studio tentará buscar os assemblies necessários por meio de programação para algumas redes de publicidade. Se houver assemblies que não possam ser recuperados automaticamente, você deverá instalá-los para cada rede de publicidade. Para obter mais informações sobre o controle de anúncios, consulte [Usar o controle de anúncios para maximizar a receita](use-ad-mediation-to-maximize-revenue.md).

O **AdMediatorControl** abstrai o uso de ids de unidade de anúncio e ids de aplicativo. Essas ids são gerenciadas pelo **AdMediatorControl**, em conjunto com as configurações de controle de anúncios no painel do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md).

O **AdMediatorControl** dá suporte para as APIs para cada um dos serviços de publicidade ele controla usando seus próprios atributos e sintaxe. Para obter mais informações, consulte [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md).

## AdControl


Se você quiser exibir anúncios em faixa da Microsoft (sem outras redes de publicidade), use **AdControl** por si só em aplicativos XAML e JavaScript. Ao testar um aplicativo que usa o **AdControl**, use o [Valores de modo de teste](test-mode-values.md) para a ID do aplicativo e a ID da unidade de anúncio. Depois de terminar o teste de seu aplicativo e você estiver pronto para enviá-lo para o Centro de Desenvolvimento do Windows, use o painel do Centro de Desenvolvimento do Windows para obter a ID da unidade de anúncio ativa e a ID do aplicativo e atualize seu código para usar esses valores. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).

 

 



<!--HONumber=Jun16_HO4-->



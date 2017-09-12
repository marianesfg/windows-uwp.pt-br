---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Saiba como adicionar valores de ID da unidade de anúncios e do aplicativo do painel do Centro de Desenvolvimento do Windows ao seu aplicativo antes de enviá-lo para a Loja."
title: "Configurar unidades de anúncios em seu aplicativo"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, unidades de anúncio"
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anúncios em seu aplicativo

Cada anúncio em faixa, anúncio intersticial ou controle de anúncio nativo em seu app tem uma *unidade publicitária* correspondente que é usada por nossos serviços para veicular anúncios ao controle. Cada unidade publicitária consiste em uma *ID da unidade publicitária* e uma *ID do aplicativo*. Enquanto você estiver desenvolvendo seu aplicativo, atribua os [valores de teste da ID do aplicativo e da ID da unidade publicitária](test-mode-values.md) para seu controle para confirmar se o app mostra anúncios de teste. Esses valores de teste só podem ser usados em uma versão de teste do seu app.

Depois que você terminar o teste de seu aplicativo e estiver pronto para enviá-lo para o Centro de Desenvolvimento do Windows, atualize o código do aplicativo para usar os valores de ID do aplicativo e ID da unidade publicitária da página [Monetizar com anúncios](../publish/monetize-with-ads.md) no painel do Centro de Desenvolvimento do Windows. Se você tentar usar valores de teste em seu aplicativo dinâmico, seu aplicativo não receberá anúncios ativos.

Para configurar as unidades publicitárias para seu aplicativo ativo:

1.  No painel do Centro de Desenvolvimento do Windows, selecione seu aplicativo e, em seguida, clique em **Monetização > Monetizar com anúncios**.

2.  Na seção **Criar unidades de anúncio**, insira um nome para a unidade de publicitária no campo **Nome de unidade de publicitária**.

3. Na lista suspensa **Tipo de unidade de publicitária**, selecione o tipo de unidade publicitária correspondente aos anúncios que você está mostrando no controle:

    -   Se você estiver usando um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em seu aplicativo para mostrar anúncios em faixa, selecione **Faixa** como o tipo de unidade publicitária.

    -   Se você estiver usando um [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) em seu aplicativo para mostrar anúncios intersticiais de vídeo ou anúncios de banner intersticiais, selecione **Vídeo intersticial** ou **Banner intersticial** (certifique-se de selecionar a opção adequada para o tipo de anúncio intersticial você quer mostrar).

    -   Se você estiver usando um [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) em seu aplicativo para mostrar anúncios em faixa, selecione **Nativo** como o tipo de unidade publicitária.
      > [!NOTE]
      > A capacidade de criar unidades publicitárias **Nativas** só está disponível para selecionar os desenvolvedores que estejam participando de um programa piloto, mas nós pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve. Se você estiver interessado em participar do nosso programa piloto, entre em contato conosco pelo email aiacare@microsoft.com.

4.  Na lista suspensa **Família de dispositivos**, selecione a família de dispositivos direcionada pelo aplicativo no qual a unidade publicitária será usada. As opções disponíveis são: **UWP (Windows 10)**, **Computador/Tablet (Windows 8.1)** ou **Celular (Windows Phone 8.x)**.

5.  Clique em **Criar unidade publicitária**. A nova unidade publicitária aparecerá na parte superior da lista na seção **Unidades publicitárias disponíveis** desta página.

6.  Para cada unidade de publicitária gerada, você verá uma **ID de aplicativo** e uma **ID da unidade de publicitária**. Para mostrar anúncios em seu aplicativo, você precisará usar esses valores no código do aplicativo:

    -   Se o app mostrar anúncios em banner, atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) e [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) de seu objeto **AdControl**. Para saber mais, consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md) e [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md).

    -   Se seu app mostrar anúncios intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) de seu objeto **InterstitialAd**. Para saber mais, consulte [Anúncios intersticiais](interstitial-ads.md).

    -   Se o app mostrar anúncios nativos, passe esses valores para os parâmetros *applicationId* e *adUnitId* do construtor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obter mais informações, consulte [Anúncios nativos](../monetize/native-ads.md).

7. Caso o aplicativo seja da UWP para o Windows 10, opcionalmente, você pode habilitar o controle de anúncios para **AdControl** ao definir as configurações na seção [Controle de anúncios](../publish/monetize-with-ads.md#mediation). O controle de anúncios permite que você maximize seus recursos de promoção de aplicativos e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft. Por padrão, automaticamente escolhemos as configurações de mediação de anúncio do aplicativo usando os algoritmos de aprendizado de máquina para ajudá-lo a maximizar a receita de anúncios em todos os mercados que seu aplicativo oferece suporte, mas, como alternativa, você pode definir manualmente as configurações de controle.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/monetize-with-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Valores de modo de teste](test-mode-values.md)
* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md)
* [Anúncios intersticiais](interstitial-ads.md)
* [Anúncios nativos](../monetize/native-ads.md)


 

 

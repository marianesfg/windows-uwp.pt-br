---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Saiba como adicionar valores de ID da unidade de anúncios e do aplicativo do painel do Centro de Desenvolvimento do Windows ao seu aplicativo antes de enviá-lo para a Loja.
title: Configurar unidades de anúncios em seu aplicativo
ms.author: mcleans
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anúncios, publicidade, unidades publicitária, testes
ms.localizationpriority: medium
ms.openlocfilehash: 6473d571ed44f60f8001b8a565d70c58d43407d1
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654655"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anúncios em seu aplicativo

Cada controle de anúncio no aplicativo UWP (Plataforma Universal do Windows) anúncio intersticial ou controle de anúncio nativo em seu aplicativo tem uma *unidade publicitária* correspondente usada por nossos serviços para veicular anúncios ao controle. Cada unidade publicitária consiste em uma *ID da unidade publicitária* e *ID do aplicativo* que você deve atribuir ao [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) ou [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) em seu aplicativo.

Fornecemos [valores de unidade publicitária de teste](#test-ad-units) que você pode usar durante os testes para confirmar se o aplicativo mostra anúncios de teste. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Se você tentar usar valores de teste em seu aplicativo depois de publicá-lo, seu aplicativo dinâmico não receberá anúncios.

Depois de terminar de testar seu aplicativo UWP e se preparar para enviá-lo ao Centro de Desenvolvimento do Windows, você deve [criar uma unidade publicitária ativa](#live-ad-units) na página [Anúncios no app](../publish/in-app-ads.md) no painel do Centro de Desenvolvimento do Windows e atualizar seu código de aplicativo para usar valores da ID de aplicativo e ID da unidade publicitária para essa unidade publicitária.

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Unidades publicitárias de teste

Enquanto você estiver desenvolvendo seu aplicativo, use os valores de ID do aplicativo e a ID da unidade publicitária de teste desta seção para ver como seu aplicativo renderiza anúncios durante o teste.

### <a name="banner-ads-using-the-adcontrol-class"></a>Anúncios em faixa (usando a classe AdControl)

* ID da unidade publicitária: ```test```
* ID do aplicativo:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Para um **AdControl**, o tamanho de um anúncio em tempo real é definido pelas propriedades **Width** e **Height**. Para obter melhores resultados, certifique-se de que as propriedades **Width** e **Height** em seu código correspondam a [tamanhos aceitos para anúncios em banner](supported-ad-sizes-for-banner-ads.md). As propriedades **Width** e **Height** não mudarão com base no tamanho de um anúncio em tempo real.

### <a name="interstitial-ads-and-native-ads"></a>Anúncios intersticiais e nativos

* ID da unidade publicitária: ```test```
* ID do aplicativo:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unidades de anúncio ativas

Para obter uma unidade publicitária ativa do painel do Centro de Desenvolvimento e usá-la em seu app:

1.  [Criar uma unidade publicitária](../publish/in-app-ads.md#create-ad-unit) na página **Anúncios no app** no painel. Especifique o tipo correto de unidade publicitário para o controle de anúncios que você está usando em seu app.
    > [!NOTE]
    > Como opção, você pode habilitar o controle de anúncios para a unidade publicitária ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation). O controle de anúncios permite que você maximize seus recursos de promoção de apps e de receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Por padrão, automaticamente escolhemos as configurações de mediação de anúncio do aplicativo usando os algoritmos de aprendizado de máquina para ajudá-lo a maximizar a receita de anúncios em todos os mercados que seu aplicativo oferece suporte, mas, como alternativa, você pode definir manualmente as configurações de controle.

2.  Depois de criar a nova unidade publicitária, recupere o **ID do Aplicativo** e a **ID da unidade publicitária** para a unidade publicitária na tabela de unidades publicitárias disponíveis na página **Monetizar** &gt; **Anúncios no app**.
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Ao criar uma unidade publicitária dinâmica UWP no painel, o valor da ID de aplicativo da unidade publicitária sempre corresponde à ID da Microsoft Store do seu aplicativo (um valor de valor da ID da Microsoft Store é semelhante a 9NBLGGH4R315).

3.  Atribua os valores de **ID do Aplicativo** e de **ID da unidade publicitária** no código do seu app:

    * Se o app mostrar anúncios em faixa, atribua esses valores às propriedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) e [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) do seu objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Para saber mais, consulte [AdControl em XAML e .NET](../monetize/adcontrol-in-xaml-and--net.md) e [AdControl em HTML5 e JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).

    * Se seu aplicativo mostrar anúncios intersticiais, passe esses valores para o método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de seu objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Para obter mais informações, consulte [Anúncios intersticiais](../monetize/interstitial-ads.md).

    * Se o app mostrar anúncios nativos, passe esses valores para os parâmetros *applicationId* e *adUnitId* do construtor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obter mais informações, consulte [Anúncios nativos](../monetize/native-ads.md).

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md)
* [Anúncios intersticiais](interstitial-ads.md)
* [Anúncios nativos](native-ads.md)


 

 

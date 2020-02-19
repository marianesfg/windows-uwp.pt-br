---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Saiba como adicionar a ID do aplicativo e os valores de ID da unidade do AD do Partner Center ao seu aplicativo antes de enviar seu aplicativo para a loja.
title: Configurar unidades de anúncios em seu aplicativo
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, unidades publicitária, testes
ms.localizationpriority: medium
ms.openlocfilehash: 37f04e8b112df23457c46e4f535309f2fe5dd847
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463898"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anúncios em seu aplicativo

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://aka.ms/ad-monetization-shutdown)

Cada controle de anúncio no aplicativo UWP (Plataforma Universal do Windows) anúncio intersticial ou controle de anúncio nativo em seu aplicativo tem uma *unidade publicitária* correspondente usada por nossos serviços para veicular anúncios ao controle. Cada unidade publicitária consiste em uma *ID da unidade publicitária* e *ID do aplicativo* que você deve atribuir ao código em seu aplicativo.

Fornecemos [valores de unidade publicitária de teste](#test-ad-units) que você pode usar durante os testes para confirmar se o aplicativo mostra anúncios de teste. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Se você tentar usar valores de teste em seu aplicativo depois de publicá-lo, seu aplicativo dinâmico não receberá anúncios.

Depois de concluir o teste do aplicativo UWP e pronto para enviá-lo ao Partner Center, você deve [criar uma unidade do AD ao vivo](#live-ad-units) na página [anúncios no aplicativo](../publish/in-app-ads.md) no Partner Center e atualizar o código do aplicativo para usar a ID do aplicativo e os valores da ID da unidade do AD para esta unidade do AD.

Para obter mais informações sobre como atribuir os valores da ID do aplicativo e ID da unidade de anúncios no código do seu aplicativo, consulte os artigos a seguir:
* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anúncios intersticiais](../monetize/interstitial-ads.md)
* [Anúncios nativos](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Unidades publicitárias de teste

Enquanto você estiver desenvolvendo seu aplicativo, use os valores de ID do aplicativo e a ID da unidade publicitária de teste desta seção para ver como seu aplicativo renderiza anúncios durante o teste.

### <a name="banner-ads-using-the-adcontrol-class"></a>Anúncios em faixa (usando a classe AdControl)

* ID da unidade do AD: ```test```
* ID do aplicativo: ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Para um **AdControl**, o tamanho de um anúncio em tempo real é definido pelas propriedades **Width** e **Height**. Para obter melhores resultados, certifique-se de que as propriedades **Width** e **Height** em seu código correspondam a [tamanhos aceitos para anúncios em banner](supported-ad-sizes-for-banner-ads.md). As propriedades **Width** e **Height** não mudarão com base no tamanho de um anúncio em tempo real.

### <a name="interstitial-ads-and-native-ads"></a>Anúncios intersticiais e nativos

* ID da unidade do AD: ```test```
* ID do aplicativo: ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unidades de anúncio ativas

Para obter uma unidade do AD ao vivo do Partner Center e usá-la em seu aplicativo:

1.  [Crie uma unidade do AD](../publish/in-app-ads.md#create-ad-unit) na página **anúncios no aplicativo** no Partner Center. Especifique o tipo correto de unidade publicitário para o controle de anúncios que você está usando em seu app.
    > [!NOTE]
    > Como opção, você pode habilitar o controle de anúncios para a unidade publicitária ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation). O controle de anúncios permite que você maximize seus recursos de promoção de apps e de receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas e os anúncios não relacionados à geração de receitas para campanhas promocionais de aplicativos da Microsoft. Por padrão, automaticamente escolhemos as configurações de mediação de anúncio do aplicativo usando os algoritmos de aprendizado de máquina para ajudá-lo a maximizar a receita de anúncios em todos os mercados que seu aplicativo oferece suporte, mas, como alternativa, você pode definir manualmente as configurações de controle.

2.  Depois de criar a nova unidade do AD, recupere **a ID do aplicativo** e a ID da **unidade** do AD da unidade do AD na tabela de unidades de anúncio disponíveis no **monetizar** &gt; página **de anúncios no aplicativo** .
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Quando você cria uma unidade de AD UWP ao vivo no Partner Center, o valor da ID do aplicativo para a unidade do AD sempre corresponde à ID da loja do seu aplicativo (um valor de ID de repositório de exemplo é semelhante a 9NBLGGH4R315).

3.  Atribua os valores de ID do aplicativo e de ID da unidade publicitária no código do seu aplicativo. Para obter mais informações, consulte os seguintes artigos:
    * [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
    * [AdControl em HTML 5 e JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Anúncios intersticiais](../monetize/interstitial-ads.md)
    * [Anúncios nativos](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários controles em faixa, intersticiais e nativos em um único app. Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [AdControl em HTML 5 e JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anúncios intersticiais](interstitial-ads.md)
* [Anúncios nativos](native-ads.md)


 

 

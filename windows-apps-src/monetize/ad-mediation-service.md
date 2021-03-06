---
description: O serviço de controle de anúncios da Microsoft permite que você maximize a receita do anúncio e os recursos de promoção do aplicativo ao exibir anúncios de várias redes de anúncios conhecidas.
title: Serviço de controle de anúncios da Microsoft
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, controle de anúncios
ms.localizationpriority: medium
ms.openlocfilehash: 13cdba15e73028e0bf0560dc373e6f06bd0e86d5
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507190"
---
# <a name="microsoft-ad-mediation-service"></a>Serviço de controle de anúncios da Microsoft

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Quando você usa o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) para [exibir anúncios em seus aplicativos](display-ads-in-your-app.md), como alternativa você pode usar o serviço de controle de anúncios da Microsoft para maximizar a receita dos anúncios. Este artigo fornece uma visão geral do serviço de controle de anúncios e suas metas.

O serviço de controle de anúncios é parte da [plataforma de monetização de anúncios da Microsoft](https://developer.microsoft.com/windows/ad-monetization-platform). A plataforma é composta pelas seguintes partes.

![addreferences](images/ad-mediation-service.png)

O serviço de controle de anúncios tem por objetivo maximizar a receita dos anúncios aproveitando esses recursos.

## <a name="diversity-of-demand-by-market-and-format"></a>Diversidade de demanda por mercado e formato

O serviço de controle de anúncios integra-se a várias de redes de publicidade em todos os formatos diferentes de anúncio (faixa, faixa intersticial, vídeo intersticial e nativo). O serviço de controle de anúncios funciona com cada rede de publicidade para garantir que eles podem promover anúncios com o potencial mais alto de envolvimento do usuário final. Continuaremos a adicionar parceiros de publicidade diferentes para ampliar a variedade e a qualidade da demanda.

## <a name="manage-complexity-of-ad-network-relationships"></a>Gerenciar a complexidade das relações da rede de publicidade  

O serviço de controle de anúncios integra-se a uma ampla variedade de redes de anúncios para que você não precise fazer esse trabalho. Depois de usar o SDK do Microsoft Advertising para exibir anúncios em seu aplicativo, você pode modificar as configurações do AD mediaid [no Partner Center](../publish/in-app-ads.md#mediation-settings) para exibir anúncios de várias redes do AD. Você aproveita os anúncios de novas redes de publicidade sem precisar fazer alterações no seu código.

Gerenciamos a relação de ponta a ponta com as redes de publicidade em seu nome. Tudo, desde a integração de rede de publicidade para veiculação de anúncios aos relatórios e pagamentos, é administrados por nós sem o seu envolvimento.

## <a name="machine-learning-based-yield-optimization-algorithms"></a>Algoritmos de otimização de rendimento com base em aprendizado da máquina

O serviço de controle de anúncios funciona para gerar o maior rendimento para os desenvolvedores. Para fazer isso, emprega algoritmos de otimização de rendimento com base em aprendizado da máquina. Para cada chamada de anúncios, os algoritmos determinam a melhor ordem *cachoeira* em que as redes de publicidade precisam ser chamadas para maximizar suas receitas. Isso é feito considerando vários fatores, incluindo:

* O usuário que faz a solicitação.
* O aplicativo para o qual a solicitação foi efetuada.
* Desempenho anterior da rede de publicidade.
* O formato de anúncios e a origem da solicitação.
* A duração da chamada do anúncio.
* A categoria de conteúdo do aplicativo.
* O segmento do usuário.
* A localização do usuário.

Novas redes de anúncios são incluídas automaticamente e avaliadas em relação ao desempenho por meio de um orçamento de aprendizagem. Em um curto período de tempo, eles encontrar seu lugar na cachoeira. Isso aumenta a competitividade das redes de publicidade e ajuda o desenvolvedor a aproveitar ao máximo a monetização por meio de aplicativos.

É altamente recomendável usar nossas [configurações recomendadas de controle](../publish/in-app-ads.md#mediation-settings) para maximizar a receita dos anúncios em seus aplicativos. Isso permite que os algoritmos permitam o melhor rendimento para seu aplicativo. No entanto, você também tem a liberdade de escolher suas próprias configurações de mediação no Partner Center para ter mais controle sobre as redes do AD que atendem anúncios e a ordem em que elas fazem.

## <a name="rich-data-and-signals"></a>Sinais e dados avançados

O serviço de controle de anúncios funciona com várias redes de anúncios para melhorar o direcionamento de usuários a fim de veicular anúncios mais relevantes para cada usuário. Isso é feito ao enviar sinais mais avançados sobre o usuário e o aplicativo para a rede de publicidade, tendo em vista os requisitos de privacidade.

## <a name="related-topics"></a>Tópicos relacionados

* [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Configurações de mediação](../publish/in-app-ads.md#mediation-settings)
* [Relatório de desempenho da publicidade](../publish/advertising-performance-report.md)

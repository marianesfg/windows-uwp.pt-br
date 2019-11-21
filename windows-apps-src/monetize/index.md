---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: O SDK do Windows, SDK do Microsoft Advertising, Microsoft Store Services SDK e a Microsoft Store oferecem muitos recursos que permitem que você ganhe mais dinheiro com seus aplicativos e obtenha clientes interagindo com seus usuários.
title: Monetização, envolvimento e serviços da Store
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, uwp, monetizar, envolvimento, promover, serviços da Store
ms.localizationpriority: medium
ms.openlocfilehash: 7beee974bceceab02984ae6499a9c5db0b0281b9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259297"
---
# <a name="monetization-engagement-and-store-services"></a>Monetização, envolvimento e serviços da Store

O SDK do Windows, SDK do Microsoft Advertising, Microsoft Store Services SDK e a Microsoft Store oferecem recursos que permitem que você ganhe mais dinheiro com seus aplicativos e obtenha clientes interagindo com seus usuários. Os tópicos desta seção mostram como criar esses recursos em seu aplicativo.

Para saber os detalhes de tarifas cobradas pela Microsoft Store e como receber o dinheiro obtido com o aplicativo, consulte [Como receber pagamentos](../publish/getting-paid-apps.md).

## <a name="choose-a-pricing-model"></a>Escolher um modelo de preços

:::row:::
    :::column:::
        ![Cobrar um preço pelo aplicativo](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**Cobrar um preço pelo aplicativo**

É possível cobrar inicialmente um preço pelo aplicativo. Damos suporte a uma grande variedade de tipos de preço, inclusive a opção de definir preços por mercado. É possível até mesmo agendar uma promoção para reduzir o preço do aplicativo por um período limitado.

[Definir preço do aplicativo](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Avaliações gratuitas](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**Avaliações gratuitas**

É possível oferecer uma versão de avaliação gratuita do aplicativo para fazer mais clientes testá-lo. Para estimular os clientes a comprarem a versão completa, é possível limitar os recursos na versão de avaliação (por exemplo, incluindo apenas o primeiro nível de um jogo), mostrar anúncios ou especificar uma avaliação por tempo limitado.

[Oferecer uma avaliação](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Compras no aplicativo](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**Compras no aplicativo**

Não importa se cobra um preço pelo aplicativo ou o oferece gratuitamente, você pode usar compras no aplicativo para oferecer um fluxo de receita constante. Use as compras no aplicativo para permitir que os clientes façam upgrade de uma versão gratuita do aplicativo para uma paga ou ofereça complementos consumíveis ou duráveis para venda dentro do aplicativo.

[Usar compras no aplicativo](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>Monetizar o app com anúncios

:::row:::
    :::column:::
        ![Anúncios para todos os contextos](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**Anúncios para todos os contextos**

Damos suporte a uma grande variedade de experiências de anúncios para atender à maioria das necessidades, inclusive anúncios em faixa, anúncios intersticiais (banners e vídeo), anúncios em vídeo lineares, anúncios executáveis e anúncios nativos. Nossa plataforma é compatível com os padrões OpenRTB, VAST 2.x, MRAID 2 e VPAID 3, além de aceitar MOAT e IAS.

[Explorar as opções de anúncio](../publish/create-an-ad-campaign-for-your-app.md)
[Instalar SDK de anúncio](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Serviço de controle de anúncios](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**Serviço de controle de anúncios**

Maximize a receita de anúncios nos seus aplicativos usando o serviço de controle de anúncios da Microsoft para exibir anúncios de várias redes conhecidas. É possível definir as configurações de controle no Partner Center sem tocar em uma linha de código. Se você nos permitir configurar o controle, nossos algoritmos de aprendizado de máquina ajudarão você a maximizar a receita de anúncios em todos os mercados compatíveis com o aplicativo.

[Usar o serviço de anúncios](https://blogs.windows.com/windowsdeveloper/2017/05/08/announcing-microsofts-ad-mediation-service/)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Análise](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**Análise**

Os relatórios de análise detalhados permitem que você veja como os anúncios em aplicativos estão se saindo, fornecendo as informações necessárias para maximizar a receita de anúncios. Também fornecemos uma API RESTful que você pode usar para obter esses dados programaticamente.

[Examinar desempenho](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>Outras oportunidades de monetização

![Outras oportunidades de monetização](images/monetize-other-opportunities.png)

À procura de outras maneiras de aumentar a monetização? Considere essas opções.

 Tópico                | Descrição                 |
|--------------------|-----------------------------|
| [Programa de afiliados da Microsoft](https://www.microsoftaffiliates.com/) | Ganhe comissões vinculando produtos da Microsoft ao seu aplicativo, blog, página da Web ou outras comunicações. É possível criar vínculos com aplicativos, jogos, músicas, filmes, hardware, acessórios e outros produtos vendidos na Microsoft Store.
| [Experimentação de A/B](https://docs.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing) | Execute testes A/B nos seus aplicativos para mensurar a eficácia das alterações de recursos em alguns clientes antes de habilitar essas alterações para todos os usuários.
| [Envolver os clientes com o Microsoft Store Services SDK](microsoft-store-services-sdk.md) | O Microsoft Store Services SDK fornece bibliotecas e ferramentas que você pode usar para adicionar recursos aos seus aplicativos que o ajudam a se envolver com seus clientes. Esses recursos incluem notificações específicas, testes A/B e lançamento do Hub de Comentários do seu aplicativo.
| [Inicie o Hub de Feedback em seu aplicativo](launch-feedback-hub-from-your-app.md) | Adicione código aos seus aplicativos UWP para direcionar os clientes do Windows 10 ao Hub de Feedback, onde eles podem enviar problemas, sugestões e aprovações. Em seguida, gerencie esses comentários em [Relatório de comentários](../publish/feedback-report.md) no Partner Center. Esse recurso requer o Microsoft Store Services SDK. 
| [Configurar seu aplicativo para receber notificações por push do Partner Center](configure-your-app-to-receive-dev-center-notifications.md) | Registre um canal de notificação para seu aplicativo UWP para que ele possa receber as [notificações por push do Partner Center](../publish/send-push-notifications-to-your-apps-customers.md) e controle a taxa de inicializações do aplicativo resultantes das notificações por push. Esse recurso requer o Microsoft Store Services SDK.
| [Registrar eventos personalizados para o Partner Center](log-custom-events-for-dev-center.md) | Registre eventos personalizados do seu aplicativo UWP e analise os eventos no [Relatório de uso](../publish/usage-report.md) no Partner Center. Esse recurso requer o Microsoft Store Services SDK.
| [Solicitar classificações e opiniões](request-ratings-and-reviews.md) | Incentivar os clientes a classificar ou analisar seu aplicativo mostrando programaticamente uma classificação e uma interface do usuário de análise.
| [Serviços da Microsoft Store](using-windows-store-services.md) | Saiba como usar APIs RESTful para automatizar envios para a Loja, acessar dados de análise dos aplicativos e automatizar outras tarefas relacionadas à Loja.
| [Adicionar recursos de demonstração (RDX) do varejo ao seu aplicativo](retail-demo-experience.md) | Inclua um modo de demonstração de varejo em seu aplicativo do Windows para que os clientes que experimentarem PCs e dispositivos na área de vendas possam começar imediatamente.

## <a name="monetization-analytics"></a>Análise de monetização

![Análise de monetização](images/monetize-analytics.png)

Monitore o desempenho do seu aplicativo na Store usando esses relatórios.

- [Resumo do pagamento](../publish/payout-summary.md)
- [Relatório de aquisições](../publish/acquisitions-report.md)
- [Relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md)
- [Relatório de desempenho da publicidade](../publish/advertising-performance-report.md)
- [Obter dados de análise usando nossa API REST](access-analytics-data-using-windows-store-services.md)
- [Criar segmentos de cliente](../publish/create-customer-segments.md)
- [Relatório de comentários](../publish/feedback-report.md)
- [Relatório de uso](../publish/usage-report.md)
---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "O SDK do Microsoft Advertising oferece várias maneiras de monetizar seu aplicativo com anúncios."
title: "Apresentar anúncios em seu app com o SDK do Microsoft Advertising"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, faixa, controle de anúncio, intersticial"
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Apresentar anúncios em seu app com o SDK do Microsoft Advertising

Aumente suas oportunidades de receita ao colocar anúncios em seus apps usando o SDK do Microsoft Advertising. Nossa plataforma de monetização de anúncios oferece uma variedade de tipos de anúncio e dá suporte ao controle com várias redes de anúncios conhecidas.

Para exibir anúncios em seus apps, siga estas etapas.

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Etapa 1: Instalar o SDK do Microsoft Advertising

O SDK do Microsoft Advertising fornece uma variedade de controles que você pode usar em seu aplicativo para mostrar diferentes tipos de anúncios. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>Etapa 2: Escolher o tipo de anúncios e adicionar código para exibir anúncios de teste em seu app

O SDK do Microsoft Advertising fornece vários tipos diferentes de anúncios que você pode exibir em seu app. Escolha quais tipos de anúncios são recomendados para seu cenário e então adicione código ao seu aplicativo para exibir os anúncios.

Você deve especificar uma ID de aplicativo e a ID da unidade publicitária em seu código para veicular anúncios em seu app. Enquanto você estiver desenvolvendo seu aplicativo, use [os valores de ID do aplicativo de teste e ID da unidade publicitária](test-mode-values.md) para ver como seu aplicativo renderiza anúncios durante o teste.

### <a name="banner-ads"></a>Anúncios em faixa

São anúncios estáticos que utilizam uma parte de uma página em um app, quase sempre na parte superior ou inferior da página.

Para obter instruções e exemplos de código, consulte [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md) e [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md). Para obter projetos de exemplo completos que demonstram como adicionar anúncios em faixa a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>Anúncios intersticiais

São os anúncios de tela inteira que geralmente exigem que o usuário assista a um vídeo ou clique neles para continuar no app ou no jogo. Oferecemos suporte a dois tipos de anúncios intersticiais: vídeo e faixa.

Para obter instruções e exemplos de código, consulte [Anúncios intersticiais](interstitial-ads.md). Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Anúncios nativos

São anúncios baseados em componente. Cada parte da parte criativa do anúncio (por exemplo, o título, a imagem, a descrição e o texto do plano de ação) é entregue ao seu app como um elemento individual que você pode integrar seu app usando suas próprias fontes, cores e outros componentes de IU para compor uma experiência de usuário discreto.

Para obter instruções e exemplos de código, consulte [Anúncios nativos](native-ads.md).

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>Etapa 3: Obter uma unidade publicitária do Centro de Desenvolvimento e configurar seu app para receber anúncios ativos

Depois que você terminar de testar seu app e estiver pronto para enviá-lo para a Loja, crie uma unidade publicitária na página [Monetizar com anúncios](../publish/monetize-with-ads.md) no painel do Centro de Desenvolvimento do Windows. Em seguida, atualize o código do app para usar os valores de ID do aplicativo e de ID da unidade publicitária para essa unidade publicitária. Se você tentar usar os valores de teste da ID do aplicativo e da ID da unidade publicitária na versão publicada do seu app na Loja, seu app não receberá anúncios ativos. Para obter mais informações, consulte [Configurar unidades publicitárias no app](set-up-ad-units-in-your-app.md).

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>Configurar controle de anúncios

Por padrão, os anúncios em faixa, os anúncios intersticiais e os anúncios nativos exibem publicidade da rede da Microsoft para anúncios pagos. Para maximizar a receita do anúncio, você pode habilitar o controle de anúncios da unidade publicitária para exibir anúncios de outras redes de publicidade pagas, como o Taboola e o Smaato. Você também pode aumentar os recursos de promoção de apps ao veicular anúncios de campanhas promocionais de aplicativos da Microsoft.

Para começar a usar o controle de anúncios em seu aplicativo UWP, [definir configurações de controle de anúncios](../publish/monetize-with-ads.md#mediation) para sua unidade publicitária. Por padrão, configuramos automaticamente as configurações de controle usando algoritmos de aprendizado de máquina para ajudá-lo a maximizar sua receita de publicidade em todos os mercados que seu aplicativo suporte. No entanto, também tem a opção de escolher manualmente as redes que deseja usar. De qualquer forma, as configurações de mediação são totalmente configuradas no serviço; você não precisa fazer alterações de código em seu app.    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>O controle em aplicativos do Windows 8.1 e do Windows Phone 8.x

Nos aplicativos do Windows 8.1 e do Windows Phone 8.x, o controle de anúncios está disponível apenas para faixas. Para usar o controle de anúncios, você deve usar a classe **AdMediatorControl** no [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk), em vez da classe **AdControl**. Depois de adicionar esse controle ao seu app, você poderá definir manualmente suas configurações no painel.

Para obter mais informações sobre como usar o controe de anúncios em um aplicativo Windows 8.1 ou Windows Phone 8.x, consulte [este artigo](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

> [!NOTE]
> A mediação de anúncios para aplicativos do Windows 8.1 e Windows Phone 8.x não está mais em desenvolvimento ativo. Para maximizar a sua receita potencial de publicidade, recomendamos que você crie aplicativos UWP que usem o controle de anúncios com anúncios em faixa, anúncios intersticiais ou anúncios nativos.

## <a name="step-4-submit-your-app-and-review-performance"></a>Etapa 4: Enviar seu app e examinar o desempenho

Após a conclusão do desenvolvimento de seu aplicativo com anúncios, você poderá [enviar seu app atualizado](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) para o painel do Centro de Desenvolvimento do Windows para que ele seja disponibilizado na Loja. Os apps que exibem anúncios devem atender aos requisitos adicionais especificados na [seção 10.10 das Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) e no [Anexo E do Contrato de Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx).

Depois que o app for publicado e estiver disponível na Loja, você poderá examinar seus [relatórios de desempenho de anúncios](../publish/advertising-performance-report.md) no painel e continuar a fazer alterações em suas configurações de controle para otimizar o desempenho de seus anúncios. Sua receita de publicidade está incluída no seu [resumo de pagamento](../publish/payout-summary.md).

<span id="additional-help" />
## <a name="additional-help"></a>Ajuda adicional

Para obter ajuda adicional usando o SDK do Microsoft Advertising, use os recursos a seguir.

|  Tarefa    | Recurso |               
|----------|-------|
| Relatar um bug ou obter suporte assistido para publicidade     | Visite a [página de suporte](https://go.microsoft.com/fwlink/p/?LinkId=331508) e escolha **Publicidade no aplicativo**.        |
| Obter suporte da comunidade     | Visite o [fórum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Baixe os projetos de exemplo que demonstram como adicionar faixa e anúncios intersticiais a aplicativos.     | Consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).       |
| Saiba mais sobre as oportunidades de monetização mais recentes para aplicativos do Windows     | Visite [Monetize seus aplicativos](https://developer.microsoft.com/store/monetize).        |

## <a name="related-topics"></a>Tópicos relacionados

* [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp)
* [Monetizar o app com anúncios](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Relatório de desempenho de anúncios](../publish/advertising-performance-report.md)

---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: "Saiba mais sobre o processo de ponta a ponta para desenvolver e publicar um aplicativo com anúncios."
title: "Fluxos de trabalho para criar aplicativos com anúncios"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 0f832674f0e635f609f09fa15acead109b1d67ff


---

# Fluxos de trabalho para criar aplicativos com anúncios




Para exibir anúncios em seus aplicativos, o aplicativo precisa ser capaz de recebe anúncios de uma rede de publicidade. A Microsoft fornece um serviço Web que permite que os desenvolvedores de aplicativos do Windows recebam anúncios. Quando os usuários clicam no anúncio em seu aplicativo, você (sendo o *fornecedor* do anúncio) ganha dinheiro do criador dos anúncios (o *anunciante*). O dinheiro ganho dos anunciantes é pago para você usando sua conta.

As etapas de alto nível a seguir descrevem o processo geral para desenvolver e publicar um aplicativo com anúncios.

1.  Estágio de desenvolvimento:

    * Configure sua conta do Centro de Desenvolvimento do Windows.
    * Desenvolva seu aplicativo usando valores do modo de teste.

2.  Pronto para lançamento:

    * Configure seu aplicativo para receber anúncios ativos.
    * Envie seu aplicativo para o Centro de Desenvolvimento do Windows e analise os dados de desempenho.

Para obter mais informações sobre cada etapa, leia sua seção correspondente abaixo.

## Configure sua conta do Centro de Desenvolvimento do Windows

Você precisa ter uma conta no Centro de Desenvolvimento do Windows para publicar seu aplicativo e receber anúncios. Isso é válido independentemente de estar usando ou não o controle de anúncios. O gerenciamento de aplicativos relacionados à publicidade também é feito no Centro de Desenvolvimento do Windows. Se você usava o Microsoft pubCenter para gerenciar a publicidade em seus aplicativos, ele foi substituído pelos recursos do Centro de Desenvolvimento do Windows

Para configurar sua conta no Centro de Desenvolvimento do Windows, visite a [home page](https://dev.windows.com/windows-apps). Informações adicionais estão disponíveis nas [páginas de ajuda](https://dev.windows.com/develop) do Centro de Desenvolvimento do Windows.

## Desenvolva seu aplicativo usando valores do modo de teste

Use as instruções nos guias passo a passo a seguir para adicionar um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para exibir anúncios em seu aplicativo:

-   [Anúncios Intersticiais](interstitial-ads.md)
-   [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
-   [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md)
-   [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md)

Ao usar um **AdControl** ou **InterstitialAd** para exibir anúncios em seu aplicativo, você deve especificar uma ID de aplicativo e a ID da unidade de anúncio em seu código para vincular seu aplicativo à sua conta do Centro de Desenvolvimento do Windows e para veicular anúncios. Enquanto você estiver desenvolvendo seu aplicativo, use os valores de ID do aplicativo de teste e ID da unidade de anúncio para ver como seu aplicativo renderiza anúncios durante o teste. Isso permite que você veja como o aplicativo está recebendo e renderizando anúncios durante o teste. Para obter mais informações, consulte [Valores de modo de teste](test-mode-values.md).

Para obter projetos de exemplo completos que demonstram como adicionar anúncios intersticiais em faixa e vídeo a aplicativos JavaScript/HTML e XAML usando C# e C++, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## Configure seu aplicativo para receber anúncios ativos

Depois que você terminar o teste de seu aplicativo e estiver pronto para enviá-lo para o Centro de Desenvolvimento do Windows, atualize o código do aplicativo para usar os valores de ID do aplicativo e ID da unidade de anúncios do [painel do Centro de Desenvolvimento do Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Se você tentar usar valores de teste em seu aplicativo dinâmico, seu aplicativo não receberá anúncios ativos. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).

## Envie seu aplicativo

Uma vez concluído o desenvolvimento de seu aplicativo, você pode publicá-lo na Windows Store usando o painel do Centro de Desenvolvimento do Windows. Além de atender aos requisitos para todos os aplicativos na Windows Store, os aplicativos que exibem anúncios devem atender a diversos requisitos adicionais. Para obter mais informações, consulte [Enviar um aplicativo com anúncios para a Windows Store](submit-an-app-with-ads-to-the-windows-store.md).

Depois que o aplicativo for publicado e disponibilizado na Windows Store, você poderá analisar seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

 

 



<!--HONumber=Aug16_HO3-->



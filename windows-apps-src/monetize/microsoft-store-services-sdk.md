---
author: mcleanbyron
Description: "O Microsoft Store Services SDK fornece bibliotecas e ferramentas que você pode usar para adicionar recursos aos seus aplicativos que ajudam você a ganhar mais dinheiro e clientes."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 98675e9cb06b05e55d49ca625818626aea5a5346

---

# Microsoft Store Services SDK

O Microsoft Store Services SDK fornece bibliotecas e ferramentas que ajudarão você a ganhar mais dinheiro e clientes nos aplicativos da Plataforma Universal do Windows (UWP), como exibir anúncios em seus aplicativos e executar experimentos com testes A/B. Este SDK será desenvolvido com o tempo para incluir novos recursos de interação e monetização.


## Recursos disponíveis no SDK

O Microsoft Store Services SDK fornece bibliotecas e ferramentas que suportam os seguintes recursos.

### Executar experimentos com testes A/B para aplicativos UWP

Execute testes A/B em seus aplicativos UWP (Plataforma Universal do Windows) para medir a eficácia de recursos em alguns clientes antes de liberar os recursos para todos. Depois de definir um experimento em seu painel do Centro de desenvolvimento, use a classe [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obter variações de sua experiência em seu aplicativo, use esses dados para modificar o comportamento do recurso que você está testando e, em seguida, use o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) para enviar eventos de exibição e de conversão ao Centro de Desenvolvimento. Por fim, use o painel para exibir os resultados e gerenciar a experiência.

Para saber mais, consulte [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md).

### Comentários de aplicativo para aplicativos UWP

Use a classe [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) em seus aplicativos UWP para direcionar os clientes do Windows 10 ao Hub de Feedback, onde eles podem enviar problemas, sugestões e aprovações. Em seguida, gerencie esses comentários no [Relatório de comentários](../publish/feedback-report.md) no painel do Centro de Desenvolvimento.

Para obter mais informações, consulte [Iniciar o Hub de comentários do seu aplicativo](launch-feedback-hub-from-your-app.md).

### Apresente anúncios em seus aplicativos

Aumente sua receita exibindo anúncios ou anúncios intersticiais de vídeo da Microsoft em aplicativos UWP. Você também pode maximizar suas taxas de preenchimento de anúncios usando a mediação de anúncios para exibi-los de vários provedores de rede de publicidade.

Para obter mais informações, consulte [Exibir anúncios no seu aplicativo](display-ads-in-your-app.md).

>**Observação**&nbsp;&nbsp;Microsoft Store Services SDK só dá suporte a aplicativos UWP. Para exibir anúncios em aplicativos Windows 8.1 e no Windows Phone 8. x, use o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).

### Referência da API

Para obter a documentação de referência sobre as APIs no Microsoft Store Services SDK, consulte [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Instalar o SDK

Para instalar o Microsoft Store Services SDK:

1.  Feche todas as instâncias do Visual Studio 2013 ou o Visual Studio 2015 e desinstale as versões anteriores do SDK Microsoft Store Engagement and Monetization, do SDK Universal Ad Client, extensão Ad Mediator ou do SDK do Microsoft Advertising.
2.  Baixar e instalar o [SDK](http://aka.ms/store-em-sdk). A instalação pode demorar alguns minutos. Aguarde até o processo terminar.
3.  Reinicie o Visual Studio.

A Microsoft lança periodicamente novas versões do Microsoft Store Services SDK com aperfeiçoamentos de desempenho e novos recursos. Se você tiver projetos existentes que usam o Microsoft Store Services SDK e deseja usar a versão mais recente, basta baixar e instalar a versão mais recente do SDK.

Os recursos de publicidade para aplicativos UWP das versões anteriores do SDK Microsoft Store Engagement and Monetization, do SDK Universal Ad Client, da extensão Ad Mediator e do Microsoft Advertising SDK agora estão incluídos no Microsoft Store Services SDK. Se tiver projetos UWP existentes que usam recursos de anúncio de uma dessas versões anteriores, você poderá continuar trabalhando com os projetos sem alterações depois de instalar o Microsoft Store Services SDK.

>**Observação**  Para instalar o Microsoft Store Services SDK com o Visual Studio 2015, você deve ter a versão 1.1 ou posterior das Ferramentas do Visual Studio para Aplicativos Universais do Windows instalada. Para obter mais informações sobre essa atualização das Ferramentas do Visual Studio para Aplicativos Universais do Windows, consulte as [notas de versão](http://go.microsoft.com/fwlink/?LinkID=624516).

## Pacotes de estrutura no SDK

As seguintes bibliotecas no Microsoft Store Services SDK são configuradas como *pacotes de estrutura*:

* Microsoft.Advertising.dll. Essa biblioteca contém as APIs de publicidade nos namespaces [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) e [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Esta biblioteca contém as APIs no namespace [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx).

Isso significa que depois de instalar o SDK no computador de desenvolvimento, essas bibliotecas serão atualizadas automaticamente por meio do Windows Update sempre que publicamos novas versões de bibliotecas com correções e melhorias de desempenho. Isso ajuda a garantir que você sempre terá a versão mais recente das bibliotecas instaladas no computador de desenvolvimento.

Além disso, após um usuário instalar uma versão do seu aplicativo que usa essas bibliotecas, as bibliotecas também serão atualizadas automaticamente em seus dispositivos sempre que publicarmos novas versões das bibliotecas com correções e melhorias de desempenho. Isso significa que os usuários sempre terão a versão mais recente das bibliotecas, sem qualquer necessidade de você publicar versões atualizadas do seu aplicativo na Loja.

No entanto, se nós lançamos uma nova versão do SDK que apresenta novas APIs ou recursos dessas bibliotecas, você precisará instalar a versão mais recente do SDK para usar esses recursos. Nesse cenário, você também precisa publicar seu aplicativo atualizado na Loja.

## Tópicos relacionados

* [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar o Hub de Feedback do seu aplicativo](launch-feedback-hub-from-your-app.md)
* [Apresentar anúncios em seus aplicativos](display-ads-in-your-app.md)



<!--HONumber=Sep16_HO2-->



---
author: mcleanbyron
Description: "O SDK de Microsoft Store Engagement and Monetization fornece bibliotecas e ferramentas que você pode usar para adicionar recursos aos seus aplicativos que ajudam você a ganhar mais dinheiro e clientes."
title: SDK de Microsoft Store Engagement and Monetization
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: 481cf2aab806a1f9ce368256a9df8930cbc756c1

---

# SDK de Microsoft Store Engagement and Monetization

O SDK de Microsoft Store Engagement and Monetization fornece bibliotecas e ferramentas que ajudarão você a ganhar mais dinheiro e clientes, como exibir anúncios em seus aplicativos e executar experimentos com testes A/B. Este SDK substitui o SDK do Microsoft Universal Ad Cliente ele evoluirá ao longo do tempo para incluir novos recursos de monetização e envolvimento.


## Recursos disponíveis no SDK

O SDK de Microsoft Store Engagement and Monetization fornece bibliotecas e ferramentas que suportam os seguintes recursos.

### Executar experimentos com testes A/B para aplicativos UWP

Execute testes A/B em seus aplicativos UWP (Plataforma Universal do Windows) para medir a eficácia de recursos em alguns clientes antes de liberar os recursos para todos. Depois de definir uma experiência em seu painel do Centro de desenvolvimento, use a classe [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) para obter variações de sua experiência em seu aplicativo, use esses dados para modificar o comportamento do recurso que você está testando e, em seguida, use o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) para enviar eventos de exibição e de conversão ao Centro de Desenvolvimento. Por fim, use o painel para exibir os resultados e gerenciar a experiência.

Para saber mais, consulte [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md).

### Comentários de aplicativo para aplicativos UWP

Use a classe [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) em seus aplicativos UWP para direcionar os clientes do Windows 10 ao Hub de comentários, onde eles podem enviar problemas, sugestões e aprovações. Em seguida, gerencie esses comentários no [Relatório de comentários](../publish/feedback-report.md) no painel do Centro de Desenvolvimento.

Para obter mais informações, consulte [Iniciar o Hub de Feedback do seu aplicativo](launch-feedback-hub-from-your-app.md).

>
            **Observação** O relatório de **Feedback** está disponível somente para contas de desenvolvedor que tenham ingressado no [Programa Insider do Centro de Desenvolvimento](../publish/dev-center-insider-program.md).

### Apresentar anúncios em seus aplicativos

Aumente sua receita exibindo anúncios em forma de banner ou anúncios intersticiais em vídeos da Microsoft em aplicativos UWP, bem como os aplicativos Windows 8.1 e Windows Phone 8.x. Você também pode maximizar suas taxas de preenchimento de anúncios usando a mediação de anúncios para exibi-los de vários provedores de rede de anúncios.

Para obter mais informações, consulte [Exibir anúncios no seu aplicativo](display-ads-in-your-app.md).

>
            **Observação** Os recursos de publicidade das versões anteriores do SDK Universal Ad Client, da extensão Ad Mediator e do Microsoft Advertising SDK agora estão incluídos no SDK de Microsoft Store Monetization and Engagement.

### Referência de API

Para obter a documentação de referência sobre as APIs no SDK, consulte [API de referência do SDK de Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Instalar o SDK

Para instalar o SDK de Microsoft Store Engagement and Monetization:

1.  Feche todas as instâncias do Visual Studio 2013 ou o Visual Studio 2015 e desinstale as versões anteriores do SDK Universal Ad Client, extensão Ad Mediator ou do SDK do Microsoft Advertising.
2.  Baixar e instalar o [SDK](http://aka.ms/store-em-sdk). A instalação pode demorar alguns minutos. Aguarde até o processo terminar.
3.  Reinicie o Visual Studio.

A Microsoft lança periodicamente novas versões do SDK de Microsoft Store Monetization and Engagement com aperfeiçoamentos de desempenho e novos recursos. Se você tiver projetos existentes que usam o SDK de Microsoft Store Monetization and Engagement e deseja usar a versão mais recente, basta baixar e instalar a versão mais recente do SDK.

Os recursos de publicidade das versões anteriores do SDK Universal Ad Client, extensão Ad Mediator e do Microsoft Advertising SDK agora estão incluídos no SDK de Microsoft Store Monetization and Engagement SDK. Se você tiver projetos do Visual Studio 2015 ou do Visual Studio 2013 que usam recursos de anúncio de uma dessas versões anteriores, você pode continuar trabalhando com seus projetos sem alterações depois que você instalar o SDK de Microsoft Store Monetization and Engagement.

>
            **Observação** Para instalar o SDK de Microsoft Store Engagement and Monetization com o Visual Studio 2015, você deve ter a versão 1.1 ou posterior das Ferramentas do Visual Studio para Aplicativos Universais do Windows instalada. Para obter mais informações sobre essa atualização das Ferramentas do Visual Studio para Aplicativos Universais do Windows, consulte as [notas de versão](http://go.microsoft.com/fwlink/?LinkID=624516).

## Pacotes de estrutura no SDK

A seguinte biblioteca no SDK de Microsoft Store Monetization and Engagement está configurada como um *Pacote de estrutura*:

* Microsoft.Advertising.dll (apenas para projetos de aplicativos UWP). Essa biblioteca contém as APIs de publicidade nos namespaces **Microsoft.Advertising** e **Microsoft.Advertising.WinRT.UI**.

Isso significa que depois de instalar o SDK no computador de desenvolvimento, essa biblioteca é atualizada automaticamente por meio do Windows Update sempre que publicamos novas versões de bibliotecas com correções e melhorias de desempenho. Isso ajuda a garantir que você sempre terá a versão mais recente da biblioteca instalada no computador de desenvolvimento.

Além disso, após um usuário instalar uma versão do seu aplicativo que usa essa biblioteca, a biblioteca também será atualizada automaticamente em seus dispositivos sempre que publicarmos novas versões da biblioteca com correções e melhorias de desempenho. Isso significa que os usuários sempre terão a versão mais recente da biblioteca, sem qualquer necessidade de você publicar versões atualizadas do seu aplicativo na Loja.

No entanto, se nós lançamos uma nova versão do SDK que apresenta novas APIs ou recursos dessa biblioteca, você precisará instalar a versão mais recente do SDK para usar esses recursos. Nesse cenário, você também precisa publicar seu aplicativo atualizado na Loja.

Outras bibliotecas no SDK, incluindo Microsoft.Advertising.dll para outras plataformas e as bibliotecas de mediação de anúncio, não são atualmente configuradas como bibliotecas de estrutura.

## Tópicos relacionados

* [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md)
* [API de referência do SDK de Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Iniciar o Hub de Feedback do seu aplicativo](launch-feedback-hub-from-your-app.md)



<!--HONumber=Jun16_HO4-->



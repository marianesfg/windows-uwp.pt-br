---
author: mcleanbyron
Description: "Você pode registrar eventos personalizados do seu aplicativo UWP e revisar esses eventos no relatório de uso no painel do Centro de Desenvolvimento do Windows."
title: Registrar eventos personalizados para o Centro de Desenvolvimento
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: e0012d586d9b79db77bdeded6f0e1d2ce848bbea

---

# <a name="log-custom-events-for-dev-center"></a>Registrar eventos personalizados para o Centro de Desenvolvimento

O [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento do Windows permite obter informações sobre eventos personalizados definidos por você no seu aplicativo da Plataforma Universal do Windows (UWP). Um evento personalizado é uma string arbitrária que representa um evento ou atividade em seu aplicativo. Por exemplo, um jogo pode definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*e assim por diante, que são registrados quando o usuário passa cada nível do jogo.

Para registrar um evento personalizado do seu aplicativo, transmita a string de evento personalizado para o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) fornecido pelo Microsoft Store Services SDK. Você pode examinar o total de ocorrências de seus eventos personalizados na seção **Eventos personalizados** do [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento.

## <a name="prerequisites"></a>Pré-requisitos

Antes que você possa revisar eventos do log personalizado no **Relatório de uso** para seu aplicativo no painel, seu aplicativo deve ser publicado na Loja.

## <a name="how-to-log-custom-events"></a>Como registrar eventos personalizados

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento. Além da API para registrar eventos personalizados, esse SDK também fornece APIs para outros recursos como execução de experiências em seus aplicativos com testes A/B e exibição de anúncios.
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.
7. Adicione a seguinte instrução à parte superior de cada arquivo de código onde deseja registrar eventos personalizados.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

8. Em cada seção do seu código onde deseja registrar um evento personalizado, obtenha um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) e, em seguida, chame o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx). Transmita a string de evento personalizado para o método.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Dec16_HO1-->



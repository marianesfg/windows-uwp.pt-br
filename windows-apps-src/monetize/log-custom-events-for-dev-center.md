---
author: mcleanbyron
Description: "Você pode registrar eventos personalizados do seu aplicativo UWP e revisar esses eventos no relatório de uso no painel do Centro de Desenvolvimento do Windows."
title: Registre eventos personalizados para o Centro de Desenvolvimento
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 61874c700ecd31c7246effef5b05ffbf1153dfd5

---

# Registre eventos personalizados para o Centro de Desenvolvimento

O [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento do Windows permite obter informações sobre eventos personalizados definidos por você no seu aplicativo da Plataforma Universal do Windows (UWP). Um evento personalizado é uma string arbitrária que representa um evento ou atividade em seu aplicativo. Por exemplo, um jogo pode definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*e assim por diante, que são registrados quando o usuário passa cada nível do jogo.

Para registrar um evento personalizado do seu aplicativo, transmita a string de evento personalizado para o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) fornecido pelo Microsoft Store Services SDK. Você pode examinar o total de ocorrências de seus eventos personalizados na seção **Eventos personalizados** do [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento.

## Pré-requisitos

Antes que você possa revisar eventos do log personalizado no **Relatório de uso** para seu aplicativo no painel, seu aplicativo deve ser publicado na Loja.

## Como registrar eventos personalizados

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento. Além da API para registrar eventos personalizados, esse SDK também fornece APIs para outros recursos como execução de experiências em seus aplicativos com testes A/B e exibição de anúncios. 
2. Abra seu projeto no Visual Studio.
3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.
4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.
7. Adicione a seguinte instrução à parte superior de cada arquivo de código onde deseja registrar eventos personalizados.

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```
8. Em cada seção do seu código onde deseja registrar um evento personalizado, obtenha um objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) e, em seguida, chame o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx). Transmita a string de evento personalizado para o método.

  ```csharp
  StoreServicesCustomEventLogger logger = StoreServicesCustomEventLogger.GetDefault();
  logger.Log("myCustomEvent");
  ```

## Tópicos relacionados

* [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->



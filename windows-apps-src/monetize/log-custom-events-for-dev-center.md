---
Description: Você pode registrar eventos personalizados do seu aplicativo da UWP e analisar esses eventos no relatório de uso do Partner Center.
title: Registrar eventos personalizados para o Partner Center
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, registrar eventos
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 368e2fb7b3c6d78f68235b829e088d79b5673cf2
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334754"
---
# <a name="log-custom-events-for-partner-center"></a>Registrar eventos personalizados para o Partner Center

O [relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no permite que o Partner Center você obter informações sobre eventos personalizados que você definiu em seu aplicativo da plataforma Universal do Windows (UWP). Um evento personalizado é uma string arbitrária que representa um evento ou atividade em seu aplicativo. Por exemplo, um jogo pode definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*e assim por diante, que são registrados quando o usuário passa cada nível do jogo.

Para registrar um evento personalizado do seu aplicativo, transmita a string de evento personalizado para o método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fornecido pelo Microsoft Store Services SDK. Você pode examinar o total de ocorrências para seus eventos personalizados na **eventos personalizados** seção o [relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no Partner Center.

> [!NOTE]
> Eventos personalizados que você faça logon Partner Center não são relacionados ao [eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx), e eles não aparecem na **Visualizador de eventos**.

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder revisar eventos de log personalizado na **relatório de uso** para seu aplicativo no Partner Center, seu aplicativo deve ser publicado na Store.

## <a name="how-to-log-custom-events"></a>Como registrar eventos personalizados

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento.

2. Abra o projeto no Visual Studio.

3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.

4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.

5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

6. Adicione a seguinte instrução à parte superior de cada arquivo de código onde deseja registrar eventos personalizados.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. Em cada seção do seu código onde deseja registrar um evento personalizado, obtenha um objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) e, em seguida, chame o método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Transmita a string de evento personalizado para o método.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > O [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) pode demorar muito tempo para carregar caso o aplicativo registre muitos eventos personalizados com nomes longos. Recomendamos que você use nomes curtos para os eventos personalizados. 

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)

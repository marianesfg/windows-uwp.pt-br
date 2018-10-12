---
author: mcleanbyron
Description: You can log custom events from your UWP app and review those events in the Usage report on the Windows Dev Center dashboard.
title: Registre eventos personalizados para o Centro de Desenvolvimento
ms.author: mcleans
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, registrar eventos
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 2b9cd4d7c527001bb382596c9c805be4ad5e7b08
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4566581"
---
# <a name="log-custom-events-for-dev-center"></a>Registrar eventos personalizados para o Centro de Desenvolvimento

O [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento do Windows permite obter informações sobre eventos personalizados definidos por você no seu aplicativo da Plataforma Universal do Windows (UWP). Um evento personalizado é uma string arbitrária que representa um evento ou atividade em seu aplicativo. Por exemplo, um jogo pode definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*e assim por diante, que são registrados quando o usuário passa cada nível do jogo.

Para registrar um evento personalizado do seu aplicativo, transmita a string de evento personalizado para o método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) fornecido pelo Microsoft Store Services SDK. Você pode examinar o total de ocorrências de seus eventos personalizados na seção **Eventos personalizados** do [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento.

> [!NOTE]
> Eventos personalizados registrados no Centro de Desenvolvimento não são relacionados a [eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx) e não aparecem no **Visualizador de Eventos**.

## <a name="prerequisites"></a>Pré-requisitos

Antes que você possa revisar eventos do log personalizado no **Relatório de uso** para seu aplicativo no painel, seu aplicativo deve ser publicado na Store.

## <a name="how-to-log-custom-events"></a>Como registrar eventos personalizados

1. Se você ainda não fez isso, [instale o Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) no computador de desenvolvimento.

2. Abra seu projeto no Visual Studio.

3. No Gerenciador de Soluções, clique com botão direito no nó **Referências** para seu projeto e clique em **Adicionar Referência**.

4. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.

5. Na lista de SDKs, clique na caixa de seleção ao lado de **Microsoft Engagement Framework** e clique em **OK**.

6. Adicione a seguinte instrução à parte superior de cada arquivo de código onde deseja registrar eventos personalizados.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. Em cada seção do seu código onde deseja registrar um evento personalizado, obtenha um objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) e, em seguida, chame o método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Transmita a string de evento personalizado para o método.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > O [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) pode demorar muito tempo para carregar caso o aplicativo registre muitos eventos personalizados com nomes longos. Recomendamos que você use nomes curtos para os eventos personalizados. 

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)

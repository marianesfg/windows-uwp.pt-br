---
author: JnHs
Description: Get detailed analytics for your Windows apps, in the dashboard or via other methods.
title: Analisar o desempenho do aplicativo
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, análise, relatórios, painel, aplicativos, dados, métricas
ms.localizationpriority: medium
ms.openlocfilehash: 090ddfdfbed1ae49e87f4dc419765e006913764f
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2892457"
---
# <a name="analyze-app-performance"></a>Analisar o desempenho do aplicativo

Você pode visualizar análises detalhadas de seus aplicativos no painel do Centro de Desenvolvimento do Windows. Estatísticas e gráficos permitem que você saiba como está andamento de seus aplicativos, desde quantos clientes você conseguiu até como eles estão usando seu aplicativo e o que eles têm a dizer sobre ele. Você também pode encontrar métricas sobre a integridade do aplicativo, o uso de anúncios e muito mais.

Você pode exibir relatórios de análise no painel ou [baixar os relatórios necessários](download-analytic-reports.md) para analisar os dados offline. Também oferecemos várias maneiras para você [acessar seus dados analíticos sem usar o painel](#no-dashboard).

## <a name="view-key-analytics-for-all-your-apps"></a>Exibir análises chave para todos os aplicativos

Para exibir análises importantes sobre seus aplicativos mais baixados, expanda **Analisar** e selecione **Visão geral**. Por padrão, a página de visão geral mostra informações sobre os cinco apps com mais aquisições durante seu ciclo de vida. Para escolher aplicativos publicados diferentes para exibição, selecione **Filtros**.

## <a name="view-individual-reports-for-each-app"></a>Exibir relatórios individuais de cada aplicativo

Nesta seção, você encontrará detalhes sobre as informações apresentadas em cada um dos seguintes relatórios:

-   [Relatório de aquisições](acquisitions-report.md)
-   [Relatório de aquisições de complemento](add-on-acquisitions-report.md)
-   [Relatório de uso](usage-report.md)
-   [Relatório de integridade](health-report.md)
-   [Relatório de classificações](ratings-report.md)
-   [Relatório de avaliações](reviews-report.md)
-   [Relatório de comentários](feedback-report.md)
-   [Relatório de análise do Xbox](xbox-analytics-report.md)
-   [Relatório de ideias de pesquisas](insights-report.md)
-   [Relatório de desempenho de anúncios](advertising-performance-report.md)
-   [Relatório de campanha publicitária](promote-your-app-report.md)


> [!NOTE]
> Dependendo dos recursos e da implementação específicos do aplicativo, você não verá dados em todos esses relatórios.

<span id="no-dashboard"/>

## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>Acessar os dados analíticos sem usar o painel do Centro de Desenvolvimento

Além de exibir relatórios no painel, você pode acessar sua análise do app em algumas maneiras diferentes.

### <a name="microsoft-store-analytics-api"></a>API de análise da Microsoft Store

Use a [API de análise da Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus apps. Essa API REST permite que você recupere dados de aplicativo e aquisições de complementos, erros e classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar seus dados analíticos do Centro de Desenvolvimento no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI para se conectar aos seus dados analíticos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para se conectar ao pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI, recomendamos que você especifique suas credenciais em um diretório do Azure AD que esteja associado com sua conta do Centro de Desenvolvimento. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que se conectar ao Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como configurar a associação, consulte [Associar o Azure Active Directory com sua conta do Centro de Desenvolvimento](associate-azure-ad-with-dev-center.md).

### <a name="dev-center-app"></a>Aplicativo do Centro de Desenvolvimento

Instale o aplicativo [Centro de Desenvolvimento](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para exibir rapidamente os detalhes sobre a integridade e o desempenho de seus aplicativos em qualquer dispositivo Windows 10.


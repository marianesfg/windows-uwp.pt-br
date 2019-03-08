---
Description: Obtenha análises detalhadas para seus aplicativos do Windows, no Partner Center ou por outros métodos.
title: Analisar o desempenho do aplicativo
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, análise, relatórios, dashboard, aplicativos, dados, as métricas
ms.localizationpriority: medium
ms.openlocfilehash: 6f76b1f897c345fb71beec8e37e592165922b2ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625441"
---
# <a name="analyze-app-performance"></a>Analisar o desempenho do aplicativo

É possível exibir análises detalhadas para seus aplicativos no [Partner Center](https://partner.microsoft.com/dashboard). Estatísticas e gráficos permitem que você saiba como está andamento de seus aplicativos, desde quantos clientes você conseguiu até como eles estão usando seu aplicativo e o que eles têm a dizer sobre ele. Você também pode encontrar métricas sobre a integridade do aplicativo, o uso de anúncios e muito mais.

Você pode exibir relatórios analíticos diretamente no Partner Center ou [Baixe os relatórios necessários](download-analytic-reports.md) para analisar os dados offline. Nós também fornecemos várias maneiras para você [acessar seus dados de análise fora do Partner Center](#outside).

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
-   [Relatório de Insights](insights-report.md)
-   [Relatório de desempenho da publicidade](advertising-performance-report.md)
-   [Relatório de campanha de anúncios](promote-your-app-report.md)


> [!NOTE]
> Dependendo dos recursos e implementação específicos do seu aplicativo, você não verá dados em todos esses relatórios.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Dados de análise de acesso fora do Partner Center

Além de exibir relatórios no Partner Center, você pode acessar a análise de aplicativos de outras maneiras.

### <a name="microsoft-store-analytics-api"></a>API de análise da Microsoft Store

Use a [API de análise da Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus apps. Essa API REST permite que você recupere dados de aplicativo e aquisições de complementos, erros e classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de desenvolvimento do Windows para o Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar seus dados de análise do Partner Center no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de desenvolvimento do Windows para o Power BI para conectar-se aos seus dados de análise](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para se conectar ao pacote de conteúdo do Centro de desenvolvimento do Windows para Power BI, é recomendável que você especifique as credenciais de um diretório do AD do Azure que está associado com sua conta no Partner Center. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que se conectar ao Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](https://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como configurar a associação, consulte [associar do Active Directory do Azure com sua conta no Partner Center](associate-azure-ad-with-dev-center.md).

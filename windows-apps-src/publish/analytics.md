---
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: Analisar o desempenho do aplicativo
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, análises, relatórios, painel, aplicativos, dados, métricas
ms.localizationpriority: medium
ms.openlocfilehash: f6a6d79745ec98af2c7f562297092eea3feda659
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964565"
---
# <a name="analyze-app-performance"></a>Analisar o desempenho do aplicativo

Você pode visualizar análises detalhadas de seus aplicativos no [Partner Center](https://partner.microsoft.com/dashboard). Estatísticas e gráficos permitem que você saiba como está andamento de seus aplicativos, desde quantos clientes você conseguiu até como eles estão usando seu aplicativo e o que eles têm a dizer sobre ele. Você também pode encontrar métricas sobre a integridade do aplicativo, o uso de anúncios e muito mais.

Você pode exibir relatórios analíticos diretamente no Partner Center ou [baixar os relatórios necessários](download-analytic-reports.md) para analisar os dados offline. Também fornecemos várias maneiras para você para [acessar seus dados analíticos fora do Partner Center](#outside).

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
-   [Relatório de feedback](feedback-report.md)
-   [Relatório de análise do Xbox](xbox-analytics-report.md)
-   [Relatório de insights](insights-report.md)
-   [Relatório de desempenho de anúncios](advertising-performance-report.md)
-   [Relatório de campanha publicitária](promote-your-app-report.md)


> [!NOTE]
> Dependendo dos recursos e da implementação específicos do aplicativo, você não verá dados em todos esses relatórios.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Acessar dados analíticos fora do Partner Center

Além de exibir relatórios no Partner Center, você pode acessar a análise de aplicativo de outras maneiras.

### <a name="microsoft-store-analytics-api"></a>API de análise da Microsoft Store

Use a [API de análise da Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus apps. Essa API REST permite que você recupere dados de aplicativo e aquisições de complementos, erros e classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de desenvolvimento do Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar os dados de análise do Partner Center no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI para se conectar aos seus dados analíticos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para se conectar ao pacote de conteúdo do Centro de desenvolvimento do Windows para Power BI, recomendamos que você especifique suas credenciais em um diretório do Azure AD associado à sua conta do Partner Center. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que se conectar ao Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como configurar a associação, consulte [Associar o Azure Active Directory com sua conta do Partner Center](associate-azure-ad-with-dev-center.md).

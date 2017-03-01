---
author: shawjohn
Description: "Você pode visualizar análises detalhadas de seus apps no painel do Centro de Desenvolvimento do Windows."
title: "Análises"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, análises, relatórios, painel, apps"
translationtype: Human Translation
ms.sourcegitcommit: b01924366a0bc2afabe2f381e72e45862f0dd682
ms.openlocfilehash: 13a37a4ae2cea67fdce843ed4e6189797d85b93e
ms.lasthandoff: 02/08/2017

---

# <a name="analytics"></a>Análises

Você pode visualizar análises detalhadas de seus apps no painel do Centro de Desenvolvimento do Windows. Estatísticas e gráficos permitem que você saiba como está andamento de seus apps, desde quantos clientes você conseguiu até como eles estão usando seu app e o que eles têm a dizer sobre ele. Você também pode encontrar informações sobre a integridade do app, o uso de anúncios e muito mais. Visualize os relatórios no painel ou [baixe os relatórios de que você precisa](download-analytic-reports.md) para analisar os dados offline. Também oferecemos várias maneiras para você [acessar seus dados analíticos sem usar o painel](#no-dashboard).

> [!NOTE]
> Além dos relatórios do painel, é possível acessar programaticamente alguns dados analíticos usando a API REST de análises da [Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="analytics-for-all-your-apps"></a>Análises para todos os seus apps

Para exibir análises importantes sobre seus apps mais baixados, no menu de navegação superior, selecione **Visão geral** > **de análises**. Por padrão, a página **Visão geral de análises** mostra informações sobre os cinco apps que têm mais aquisições durante seu ciclo de vida. Para escolher apps diferentes a serem mostrados, selecione **Alterar filtros**.

## <a name="available-reports-for-each-app"></a>Relatórios disponíveis para cada app

Nesta seção, você encontrará detalhes sobre as informações apresentadas em cada um dos seguintes relatórios:

-   [Relatório de aquisições](acquisitions-report.md)
-   [Relatório de aquisições de complemento](add-on-acquisitions-report.md)
-   [Relatório de instalações](installs-report.md)
-   [Relatório de uso](usage-report.md)
-   [Relatório de integridade](health-report.md)
-   [Relatório de classificações](ratings-report.md)
-   [Relatório de avaliações](reviews-report.md)
-   [Relatório de comentários](feedback-report.md)
-   [Relatório Canais e conversões](channels-and-conversions-report.md)
-   [Relatório de controle de anúncios](ad-mediation-report.md)
-   [Relatório de desempenho de anúncios](advertising-performance-report.md)
-   [Relatório de desempenho de afiliadas](affiliates-performance-report.md)
-   [Relatório de promoção do seu app](promote-your-app-report.md)

> [!NOTE]
> Dependendo dos recursos e implementação específicos do seu app, você não verá dados em todos esses relatórios.

## <a name="page-and-section-filters"></a>Filtros de seção e página

Cada relatório inclui filtros que você pode usar para fazer uma busca detalhada em seus dados. Próximo ao topo da página, você verá **Aplicar filtros**. Você pode usar esses filtros para limitar ou expandir o escopo de todos os gráficos e as informações contidas na página.

Dentro de cada gráfico em particular, você também pode ver os filtros de seção individuais. Eles limitarão os dados mostrados apenas para esse gráfico específico.

Os filtros específicos variam de acordo com o relatório. Os tópicos desta seção explicarão quais são os filtros disponíveis e descreverão os outros dados na página de cada relatório.

<span id="no-dashboard"/>
## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>Acessar os dados analíticos sem usar o painel do Centro de Desenvolvimento

Além dos relatórios analíticos do painel, há várias outras maneiras de acessar seus dados analíticos.

### <a name="windows-store-analytics-api"></a>API de análise da Windows Store

Use a [API de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus apps. Essa API REST permite que você recupere dados de app e aquisições de complementos, erros e classificações e avaliações de app. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu app ou serviço.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar seus dados analíticos do Centro de Desenvolvimento no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI para se conectar aos seus dados analíticos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para se conectar ao pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI, recomendamos que você especifique suas credenciais em um diretório do Azure AD que esteja associado com sua conta do Centro de Desenvolvimento. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que se conectar ao Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como associar sua conta do Centro de Desenvolvimento com um Azure AD, consulte [Gerenciar usuários de contas](manage-account-users.md).

### <a name="dev-center-app"></a>Aplicativo do Centro de Desenvolvimento

Instale o app [Centro de Desenvolvimento](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para exibir rapidamente os detalhes sobre a integridade e o desempenho de seus apps em qualquer dispositivo Windows 10.

## <a name="related-topics"></a>Tópicos relacionados
- [Publique aplicativos do Windows](index.md)


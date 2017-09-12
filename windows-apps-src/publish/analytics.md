---
author: JnHs
Description: "Obtenha análises detalhadas de seus aplicativos do Windows no painel ou por outros métodos."
title: Analisar o desempenho do aplicativo
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, análises, relatórios, painel, apps"
ms.openlocfilehash: 57e4a30258fa25411bb461cac56aa18d2f74981d
ms.sourcegitcommit: a93b1da07b386a682435de58a8129d7b4ee90c14
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2017
---
# <a name="analyze-app-performance"></a>Analisar o desempenho do aplicativo

Você pode visualizar análises detalhadas de seus aplicativos no painel do Centro de Desenvolvimento do Windows. Estatísticas e gráficos permitem que você saiba como está andamento de seus aplicativos, desde quantos clientes você conseguiu até como eles estão usando seu aplicativo e o que eles têm a dizer sobre ele. Você também pode encontrar métricas sobre a integridade do aplicativo, o uso de anúncios e muito mais.

Você pode exibir relatórios de análise no painel ou [baixar os relatórios necessários](download-analytic-reports.md) para analisar os dados offline. Também oferecemos várias maneiras para você [acessar seus dados analíticos sem usar o painel](#no-dashboard).

## <a name="view-key-analytics-for-all-your-apps"></a>Exibir análises chave para todos os aplicativos

Para exibir análises importantes sobre seus aplicativos mais baixados, expanda **Analisar** e selecione **Visão geral**. Por padrão, a página **Visão geral de análises** mostra informações sobre os cinco aplicativos que têm mais aquisições durante seu ciclo de vida. Para escolher aplicativos publicados diferentes para exibição, selecione **Filtros**.

## <a name="view-individual-reports-for-each-app"></a>Exibir relatórios individuais de cada aplicativo

Nesta seção, você encontrará detalhes sobre as informações apresentadas em cada um dos seguintes relatórios:

-   [Relatório de aquisições](acquisitions-report.md)
-   [Relatório de aquisições de complemento](add-on-acquisitions-report.md)
-   [Relatório de uso](usage-report.md)
-   [Relatório de integridade](health-report.md)
-   [Relatório de avaliações](reviews-report.md)
-   [Relatório de feedback](feedback-report.md)
-   [Relatório de desempenho de anúncios](advertising-performance-report.md)
-   [Relatório de campanha publicitária](promote-your-app-report.md)

> [!NOTE]
> Dependendo dos recursos e da implementação específicos do aplicativo, você não verá dados em todos esses relatórios.

<span id="no-dashboard"/>
## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>Acessar os dados analíticos sem usar o painel do Centro de Desenvolvimento

Além dos relatórios analíticos no painel, há várias outras maneiras de acessar seus dados de análise.

### <a name="windows-store-analytics-api"></a>API de análise da Windows Store

Use a [API de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus aplicativos. Essa API REST permite que você recupere dados de aplicativo e aquisições de complementos, erros e classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar seus dados analíticos do Centro de Desenvolvimento no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI para se conectar aos seus dados analíticos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para se conectar ao pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI, recomendamos que você especifique suas credenciais em um diretório do Azure AD que esteja associado com sua conta do Centro de Desenvolvimento. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que se conectar ao Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como associar sua conta do Centro de Desenvolvimento com um Azure AD, consulte [Gerenciar usuários de contas](manage-account-users.md).

### <a name="dev-center-app"></a>Aplicativo do Centro de Desenvolvimento

Instale o aplicativo [Centro de Desenvolvimento](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para exibir rapidamente os detalhes sobre a integridade e o desempenho de seus aplicativos em qualquer dispositivo Windows 10.


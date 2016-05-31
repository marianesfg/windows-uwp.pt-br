---
author: jnHs
Description: Você pode visualizar análises detalhadas de seus aplicativos no painel do Centro de Desenvolvimento do Windows.
title: Análises
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
---

# Análises

Você pode visualizar análises detalhadas de seus aplicativos no painel do Centro de Desenvolvimento do Windows. Estatísticas e gráficos permitem que você saiba como está andamento de seus aplicativos, desde quantos clientes você conseguiu até como eles estão usando seu aplicativo e o que eles têm a dizer sobre ele. Você também pode encontrar informações sobre a integridade do aplicativo, o uso de anúncios e muito mais. Visualize os relatórios no painel ou [baixe os relatórios de que você precisa](download-analytic-reports.md) para analisar os dados offline. Também oferecemos várias maneiras para você [acessar seus dados analíticos sem usar o painel](#no-dashboard).

> **Observação**
            &nbsp;&nbsp;Além dos relatórios do painel, é possível acessar programaticamente alguns dados analíticos usando a [API REST de análises da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

## Análises para todos os seus aplicativos


Sua página de visão geral do painel também inclui um modo de exibição acumulado para juntar detalhes sobre todos os seus aplicativos. As estatísticas mostradas na página de visão geral varia de acordo com seus aplicativos.

Ao [baixar relatórios analíticos](download-analytic-reports.md), você também tem a opção de baixar relatórios sobre todos os seus aplicativos. Você precisará acessar a página **Baixar relatórios** na seção **Análises** de um dos seus aplicativos, mas não está limitado a baixar dados apenas para esse aplicativo específico.

## Relatórios disponíveis para cada aplicativo


Nesta seção, você encontrará detalhes sobre as informações apresentadas em cada um dos seguintes relatórios:

-   [Relatório de aquisições](acquisitions-report.md)
-   [Relatório de integridade](health-report.md)
-   [Relatório de classificações](ratings-report.md)
-   [Relatório de avaliações](reviews-report.md)
-   [Relatório de feedback](feedback-report.md)
-   [Relatório de uso](usage-report.md)
-   [Relatório de aquisições de IAP](iap-acquisitions-report.md)
-   [Relatório de controle de anúncios](ad-mediation-report.md)
-   [Relatório de desempenho de anúncios](advertising-performance-report.md)
-   [Relatório de anúncios de instalação de aplicativos](app-install-ads-reports.md)
-   [Relatório de canais e conversões](channels-and-conversions-report.md)

> **Observação**
            &nbsp;&nbsp;Dependendo dos recursos específicos e da implementação de seu aplicativo, você talvez não veja dados em todos esses relatórios.

## Filtros de seção e página

Cada relatório inclui filtros que você pode usar para fazer uma busca detalhada em seus dados. Próximo ao topo da página, você verá **Aplicar filtros**. Você pode usar esses filtros para limitar ou expandir o escopo de todos os gráficos e as informações contidas na página.

Dentro de cada gráfico em particular, você também pode ver os filtros de seção individuais. Eles limitarão os dados mostrados apenas para esse gráfico específico.

Os filtros específicos variam de acordo com o relatório. Os tópicos desta seção explicarão quais são os filtros disponíveis e descreverão os outros dados na página de cada relatório.

<span id="no-dashboard"/>
## Acessar os dados analíticos sem usar o painel do Centro de Desenvolvimento

Além dos relatórios analíticos do painel, há várias outras maneiras de acessar seus dados analíticos.

### API de análise da Windows Store

Use a [API de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar programaticamente os dados analíticos de seus aplicativos. Essa API REST permite que você recupere dados de aplicativo e aquisições IAP, erros, classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

### O pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI

Use o [pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar e monitorar seus dados analíticos do Centro de Desenvolvimento no Power BI. O Power BI é um serviço de análise de negócios baseado em nuvem que oferece para você um único modo de exibição de seus dados corporativos.

Use os recursos a seguir para começar a usar o Power BI para acessar seus dados analíticos.

* [Inscreva-se no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Saiba como usar o Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Saiba como usar o pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI para se conectar aos seus dados analíticos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> **Observação**
            &nbsp;&nbsp;Para se conectar ao pacote de conteúdo do Centro de Desenvolvimento do Windows para Power BI, recomendamos que você especifique suas credenciais em um diretório do Azure AD que esteja associado com sua conta do Centro de Desenvolvimento. Se você usar suas credenciais de conta da Microsoft, seus dados analíticos não serão atualizados automaticamente no Power BI, e você terá que fazer logon no Power BI para atualizar seus dados. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode [baixá-lo gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obter mais informações sobre como associar sua conta do Centro de Desenvolvimento com um Azure AD, consulte [Gerenciar usuários de contas](manage-account-users.md).

### Aplicativo do Centro de Desenvolvimento

Instale o aplicativo [Centro de Desenvolvimento](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para exibir rapidamente os detalhes sobre a integridade e o desempenho de seus aplicativos em qualquer dispositivo Windows 10. 


<!--HONumber=May16_HO2-->



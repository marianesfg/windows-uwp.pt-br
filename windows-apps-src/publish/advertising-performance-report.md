---
author: jnHs
Description: To view performance data for the ad units in your apps, use the advertising performance report on the Windows Dev Center dashboard.
title: Relatório de desempenho de anúncios
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1a82c91aeafa253427d8e526b38b8ac304591a2
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3402005"
---
# <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios


O **Relatório de desempenho de anúncios** mostra o desempenho das [unidades de anúncio](in-app-ads.md), incluindo anúncios de comunidade. Esse relatório inclui dados de diversos provedores de anúncios em aplicativos UWP que usam a [mediação de anúncios](in-app-ads.md#mediation).

Para exibir esse relatório, expanda **Analisar** no menu de navegação esquerdo e selecione **Desempenho do anúncio**. Você pode exibir esses dados no seu painel ou baixar os dados do relatório para exibição offline clicando nos ícones de seta na página. Como alternativa, é possível recuperar de forma programática esses dados usando o método [obter dados de desempenho de anúncios](../monetize/get-ad-performance-data.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

Ao exibir os relatórios de desempenho de publicidade, observe que os dados dos relatórios dos últimos três dias podem mudar à medida que recebemos e processamos novos dados de várias fontes. Além disso, as novas declarações de dados podem ocorrer para até 90 dias retroativos.

## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é 30D (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Também é possível expandir **Filtros** para filtrar os dados dessa página por unidade publicitária, provedor de anúncios e tipo de dispositivo. Você pode escolher entre as seguintes opções:

* **Agregação**: escolha como os dados do relatório são agregados e como podem ser filtrados ainda mais. Por padrão, esse filtro é definido como **Todas as unidades de anúncio**. Como opção, é possível alterar o filtro como **Todos os aplicativos** ou **Todos os provedores de anúncios** ou optar por agregar por um aplicativo específico no qual você usa anúncios.
* **Provedores de anúncio**: filtra o relatório pelos dados de desempenho de determinados [provedores de anúncios](in-app-ads.md#paid-networks). Por padrão, o relatório mostra dados de todos os provedores de anúncios. Essa opção será desabilitada se você escolher **Todos os provedores de anúncios** na lista suspensa **Agregação**.
* **Dispositivo**: filtra o relatório pelos dados de desempenho de determinados tipos de dispositivo. Por padrão, o relatório mostra dados de todos os tipos de dispositivo.

## <a name="overall-performance"></a>Desempenho geral

Essa seção mostra as métricas de desempenho de anúncio em um gráfico ou mapa mundial para as unidades de anúncio, os aplicativos e os provedores de anúncios que você selecionou nos filtros de relatório.

Para alternar para uma exibição de dados diferente, clique em **Gráfico** ou **Mapa** na parte superior da seção **Desempenho geral**. No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países menores.

Você pode refinar os dados mostrados no gráfico ou mapa clicando no ícone de filtro próximo à lista suspensa **Gráfico** ou **Mapa**. Esse filtro permite que você escolha até seis unidades de anúncio, aplicativos ou provedores diferentes para comparação na exibição do gráfico ou mapa. O tipo de dados que você pode escolher nesse filtro depende do que você selecionou no filtro de nível superior **Agregação** na parte superior do relatório.


## <a name="overall-performance-breakdown"></a>Detalhamento de desempenho geral

Essa seção exibe uma tabela que contém todas as métricas de desempenho de anúncio para o conjunto de dados especificado pelos filtros que você selecionou no relatório.

## <a name="performance-metrics"></a>Métricas de desempenho

O relatório de **Desempenho de anúncios** inclui dados para as seguintes métricas de desempenho. Algumas métricas são mostradas apenas para certos provedores de anúncios.

|  Métrica  |  Descrição  |
|----------|---------------|
| Receita estimada  |  O montante estimado de dinheiro que você recebeu dos anúncios executados em seu aplicativo. |
| eCPM  |  O custo efetivo por milhar de impressões. |
| Solicitações  | O número de vezes que uma solicitação de anúncio foi enviada do seu aplicativo.  |
| Impressões  | O número de vezes que um anúncio foi exibido em seu aplicativo.  |
| Taxa de preenchimento  | A porcentagem de solicitações de anúncio enviadas do aplicativo em que um anúncio foi exibido.  |
| Cliques  |  O número de vezes que uma pessoa clicou em um anúncio no seu aplicativo. |
| CTR  |  Taxa de cliques, que significa o número de vezes que um anúncio foi clicado, dividido pelo número de impressões. |
| Visualização | A porcentagem de impressões de anúncios que são visíveis no seu aplicativo. Para obter mais detalhes sobre como esse valor é calculado, consulte [otimizar a visualização de suas unidades de anúncio](../monetize/optimize-ad-unit-viewability.md). |
| Créditos obtidos  | Se estiver executando uma campanha de [anúncio de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos que você ganhou para espaço publicitário promocional, mostrando anúncios de comunidade em seu aplicativo.  |
| Créditos gastos  | Se estiver executando uma campanha de [anúncio de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos gastos em anúncios para seu aplicativo.  |

## <a name="related-topics"></a>Tópicos relacionados

* [Anúncios no aplicativo](in-app-ads.md)
* [Apresentar anúncios em seu app com o SDK do Microsoft Advertising](../monetize/display-ads-in-your-app.md)
* [Otimizar a visualização das unidades de anúncio](../monetize/optimize-ad-unit-viewability.md)


 

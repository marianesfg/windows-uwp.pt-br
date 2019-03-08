---
Description: Para exibir dados de desempenho para as unidades do ad em seus aplicativos, use o relatório de desempenho de anúncios no Partner Center.
title: Relatório de desempenho de anúncios
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a96f6f6593a8ccc6714f67b6f825a6416750b432
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640271"
---
# <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios


O **relatório de desempenho de publicidade** na [Partner Center](https://partner.microsoft.com/dashboard) mostra como seu [unidades ad](in-app-ads.md) estiver executando, inclusive anúncios de comunidade. Esse relatório inclui dados de diversos provedores de anúncios em aplicativos UWP que usam a [mediação de anúncios](in-app-ads.md#mediation).

Para exibir esse relatório, expanda **Analisar** no menu de navegação esquerdo e selecione **Desempenho do anúncio**. Você pode exibir esses dados no Partner Center ou baixar os dados de relatório para exibir offline clicando nos ícones de seta para a página. Como alternativa, é possível recuperar de forma programática esses dados usando o método [obter dados de desempenho de anúncios](../monetize/get-ad-performance-data.md) na [API REST de análise](../monetize/access-analytics-data-using-windows-store-services.md).

Ao exibir os relatórios de desempenho de publicidade, observe que os dados dos relatórios dos últimos três dias podem mudar à medida que recebemos e processamos novos dados de várias fontes. Além disso, as novas declarações de dados podem ocorrer para até 90 dias retroativos.

## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é 30D (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Também é possível expandir **Filtros** para filtrar os dados dessa página por unidade publicitária, provedor de anúncios e tipo de dispositivo. Você pode escolher entre as seguintes opções:

* **Agregação**: Escolha como os dados do relatório são agregados e como ele pode ser filtrado ainda mais. Por padrão, esse filtro é definido como **Todas as unidades de anúncio**. Como opção, é possível alterar o filtro como **Todos os aplicativos** ou **Todos os provedores de anúncios** ou optar por agregar por um aplicativo específico no qual você usa anúncios.
* **Provedores de AD**: Filtrar o relatório para dados de desempenho para determinadas [provedores ad](in-app-ads.md#paid-networks). Por padrão, o relatório mostra dados de todos os provedores de anúncios. Essa opção será desabilitada se você escolher **Todos os provedores de anúncios** na lista suspensa **Agregação**.
* **dispositivo**: Filtre o relatório para dados de desempenho para determinados tipos de dispositivo. Por padrão, o relatório mostra dados de todos os tipos de dispositivo.

## <a name="overall-performance"></a>Desempenho geral

Essa seção mostra as métricas de desempenho de anúncio em um gráfico ou mapa mundial para as unidades de anúncio, os aplicativos e os provedores de anúncios que você selecionou nos filtros de relatório.

Para alternar para uma exibição de dados diferente, clique em **Gráfico** ou **Mapa** na parte superior da seção **Desempenho geral**. No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países ou regiões menores.

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
| Visualização | A porcentagem de impressões de anúncio que podem ser exibidos em seu aplicativo. Para obter mais detalhes sobre como esse valor é calculado, consulte [otimizar a visualização das suas unidades ad](../monetize/optimize-ad-unit-viewability.md). |
| Créditos obtidos  | Se estiver executando uma campanha de [anúncio de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos que você ganhou para espaço publicitário promocional, mostrando anúncios de comunidade em seu aplicativo.  |
| Créditos gastos  | Se estiver executando uma campanha de [anúncio de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos gastos em anúncios para seu aplicativo.  |

## <a name="related-topics"></a>Tópicos relacionados

* [Anúncios no aplicativo](in-app-ads.md)
* [Exibir anúncios em seu aplicativo com o Microsoft Advertising SDK](../monetize/display-ads-in-your-app.md)
* [Otimizar a visualização das suas unidades do ad](../monetize/optimize-ad-unit-viewability.md)


 

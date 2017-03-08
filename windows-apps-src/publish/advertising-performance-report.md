---
author: jnHs
Description: "Para visualizar dados de desempenho para as unidades de anúncio em seus apps, use os relatórios de desempenho de anúncios em nível de app e de conta no painel do Centro de Desenvolvimento do Windows."
title: "Relatório de desempenho de anúncios"
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d53fe17f2d2452e66a6b4f9c1609112c621ede64
ms.lasthandoff: 02/07/2017

---

# <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios


Para exibir dados de desempenho para as unidades de anúncios em seus apps, você pode usar os seguintes relatórios no painel do Centro de Desenvolvimento do Windows:

-   [Relatório de desempenho de anúncios no nível do app](advertising-performance-report.md#app-level-advertising-performance-report). Esse relatório fornece dados de desempenho para as unidades de anúncios da Microsoft no app atualmente selecionado no painel.
-   [Relatório de desempenho de anúncios no nível da conta](advertising-performance-report.md#account-level-advertising-performance-report). Esse relatório fornece dados de desempenho detalhados para unidades de anúncios da Microsoft e anúncios de comunidade para todos os apps que estão registrados em sua conta de desenvolvedor.

Por padrão, os relatórios são filtrados pelo desempenho dos últimos 30 dias, em todos os dispositivos. Para alterar esses filtros, selecione **Aplicar filtros** e escolha um intervalo de tempo diferente (um dos períodos predefinidos ou um intervalo de datas personalizado) ou escolha um tipo de dispositivo individual. 

> [!TIP]
> Para fazer uma análise mais profunda de seus dados, selecione **Baixar relatório** e, em seguida, abra o arquivo CSV (valores separados por vírgula) no Microsoft Excel ou em um programa semelhante.

As seções a seguir fornecem mais detalhes sobre esses relatórios.

## <a name="app-level-advertising-performance-report"></a>Relatório de desempenho de anúncios no nível do app

Esta página fornece dados de desempenho em forma de gráfico, mapa do mundo e tabela para as unidades de anúncios da Microsoft do app selecionado no momento no painel. Para ver esse relatório, selecione um de seus apps no painel e clique em **Análises** &gt; **Desempenho de anúncios** no painel de navegação.

Os dados são obtidos destas métricas de desempenho que rastreamos para os anúncios em seu app:

-   **Receita estimada**: a quantidade estimada de dinheiro que você recebeu dos anúncios executados em seu app.
-   **eCPM**: o custo efetivo por milhares de impressões.
-   **Solicitações**: o número de vezes que uma solicitação de anúncio foi enviada do seu app.
-   **Impressões**: a quantidade de vezes que um anúncio foi exibido em seu app.
-   **Taxa de preenchimento**: a porcentagem de solicitações de anúncio enviadas de seu app nas quais um anúncio foi exibido.
-   **Cliques**: a quantidade de vezes que alguém clicou em um anúncio em seu app.
-   **CTR**: taxa de cliques, que significa o número de vezes que um anúncio foi clicado, dividido pelo número de impressões.

Para examinar todas essas métricas de desempenho para as unidades de anúncios em seu app, consulte a tabela abaixo dos modos de exibição de gráfico e mapa.

Para analisar os dados de uma dessas métricas em um modo de exibição de gráfico ou mapa do mundo, clique em **Gráfico** ou **Mapa**. Clique nos cabeçalhos acima do gráfico ou mapa para alternar entre as métricas diferentes. Por padrão, as visualizações de gráfico e mapa exibem dados de desempenho para todas as unidades de anúncio em seu app, mas você pode clicar em **Escolher unidades de anúncio** para selecionar até seis unidades de anúncio individuais para comparar.

No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países ou regiões menores.

Para fazer uma análise mais profunda de seus dados, selecione **Baixar relatório** e, em seguida, abra o arquivo CSV (valores separados por vírgula) no Microsoft Excel ou em um programa semelhante.

## <a name="account-level-advertising-performance-report"></a>Relatório de desempenho de anúncios no nível da conta

Esta página fornece dados de desempenho detalhados para unidades de anúncios da Microsoft e anúncios de comunidade usados em apps que estão registrados em sua conta de desenvolvedor. Para exibir esse relatório, acesse a página de visão geral do painel e clique em **Desempenho de anúncios** no painel de navegação.

Esta página inclui as seções a seguir.

### <a name="microsoft-advertising"></a>Microsoft Advertising

Este relatório fornece dados de desempenho para todas as unidades de anúncios da Microsoft usadas em seus apps. O relatório também inclui dados de desempenho para qualquer unidade de anúncio do pubCenter que não foi mapeada com êxito para seus apps do Centro de Desenvolvimento.

Esse relatório mostra as mesmas sete métricas de desempenho e modos de exibição (gráfico, mapa do mundo e tabela) como o relatório de desempenho do anúncio no nível de app descrito acima. Você pode aplicar os seguintes filtros a esse relatório:

-   **Todas as unidades de anúncio**. Ao selecionar esse filtro, você pode optar por exibir dados de todas as unidades de anúncio ou de até seis unidades de anúncio específicas.
-   **Todos os apps**. Ao selecionar esse filtro, você pode optar por exibir dados de todos os apps ou de até seis apps específicos.
-   **Aplicativo individual**. Ao selecionar um app, você poderá optar por exibir dados de todas as unidades de anúncio usadas pelo app ou de até seis unidades de anúncio específicas usadas pelo app.

Se você criou unidades de anúncio para um app usando o Microsoft pubCenter, é possível que nem todas elas tenham sido mapeadas com êxito aos seus apps no Centro de Desenvolvimento. Nesse relatório, essas unidades de anúncio são associadas a nomes de app que você especificou no pubCenter, com a cadeia de caracteres **(pubCenter)** anexada ao nome do app.

Se você criou unidades de anúncio para um app usando o pubCenter e não vir dados para elas na página de relatório, clique em **Vincular uma conta do pubCenter** para vincular essa conta à sua conta do Centro de Desenvolvimento. Insira o email que você associou a essa conta do pubCenter e seu código de vinculação de conta. Para encontrar o código de vinculação de contas, entre nessa conta do pubCenter e vá para a página **Minhas informações**.

Para saber mais sobre a migração de contas do pubCenter para o Centro de Desenvolvimento, veja [Integração pubCenter-DevCenter](pubcenter-dev-center-integration.md).

Para fazer uma análise mais profunda de seus dados, selecione **Baixar relatório** e, em seguida, abra o arquivo CSV (valores separados por vírgula) no Microsoft Excel ou em um programa semelhante.

### <a name="microsoft-community-ads"></a>Anúncios da comunidade da Microsoft

Esta seção fornece dados de desempenho em forma de gráfico e mapa do mundo para anúncios de comunidade no app selecionado atualmente no painel. Para obter mais informações sobre anúncios da comunidade, veja [Sobre anúncios da comunidade](about-community-ads.md).

Os dados são obtidos destas métricas de desempenho que rastreamos para os anúncios em seu app:

-   **Solicitações**: o número de vezes que uma solicitação de anúncio da comunidade foi enviada do seu app.
-   **Taxa de preenchimento**: a porcentagem de solicitações de anúncio da comunidade enviadas de seu app nas quais um anúncio foi exibido.
-   **Cliques**: a quantidade de vezes que alguém clicou em um anúncio da comunidade em seu app.
-   **CTR**: taxa de cliques, indicando a quantidade de vezes que um anúncio da comunidade foi clicado, dividida pela quantidade de impressões.
-   **Créditos obtidos**: o número de créditos de anúncio da comunidade obtidos com esse app. Para obter mais detalhes sobre como os créditos são obtidos, veja [Sobre os anúncios da comunidade](about-community-ads.md).
-   **Créditos gastos**: o número de créditos de anúncio da comunidade gastos para este app. Para obter mais detalhes sobre como os créditos são gastos, veja [Sobre os anúncios da comunidade](about-community-ads.md).

Para analisar os dados de uma dessas métricas em um modo de exibição de gráfico ou mapa do mundo, clique em **Gráfico** ou **Mapa**. Clique nos cabeçalhos acima do gráfico ou mapa para alternar entre as métricas diferentes. No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países ou regiões menores.

## <a name="notes-about-the-reports"></a>Observações sobre os relatórios

Veja algumas coisas a ter em mente ao usar os relatórios de desempenho de anúncio.

- Pode haver diferenças entre os relatórios de desempenho de anúncios no Centro de Desenvolvimento e no pubCenter. O desempenho de anúncios no Centro de Desenvolvimento é agregado com base no UTC (não em seu fuso horário específico), enquanto os relatórios do pubCenter são agregados com base no seu fuso horário específico.
- Os dados de relatório dos últimos três dias podem mudar à medida que recebemos e processamos novos dados de várias fontes.
- Novas declarações de dados podem ocorrer até 90 dias no passado.

 

 


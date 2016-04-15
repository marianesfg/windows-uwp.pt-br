---
Description: Para visualizar dados de desempenho para as unidades de anúncio em seus aplicativos, use os relatórios de desempenho de anúncios em nível de aplicativo e de conta no painel do Centro de Desenvolvimento do Windows.
title: Relatório de desempenho de anúncios
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
---

# Relatório de desempenho de anúncios


Para exibir dados de desempenho para as unidades de anúncios em seus aplicativos, você pode usar os seguintes relatórios no painel do Centro de Desenvolvimento do Windows:

-   [Relatório de desempenho de anúncios em nível de aplicativo](advertising-performance-report.md#app-level-advertising-performance-report). Esse relatório fornece dados de desempenho para as unidades de anúncios da Microsoft no aplicativo atualmente selecionado no painel.
-   [Relatório de desempenho de anúncios em nível de conta](advertising-performance-report.md#account-level-advertising-performance-report). Esse relatório fornece dados de desempenho detalhados para unidades de anúncios da Microsoft e anúncios de comunidade para todos os aplicativos que estão registrados em sua conta de desenvolvedor.
-   [Relatório de desempenho de anúncios em nível de painel](advertising-performance-report.md#dashboard-level-advertising-performance-report). Este relatório na sua página **Visão geral do painel** fornece um resumo dos dados de desempenho para unidades de anúncios da Microsoft em todos os aplicativos que são registrados em sua conta de desenvolvedor.

Por padrão, os relatórios são filtrados pelo desempenho dos últimos 30 dias, em todos os dispositivos. Para alterar esses filtros, clique em **Filtros de página** e escolha um período diferente ou um tipo de dispositivo individual. Observe que todos os dados são agregados com base na utilização UTC, não na sua zona de tempo em particular.

As seções a seguir fornecem mais detalhes sobre esses relatórios.

## Relatório de desempenho de anúncios no nível do aplicativo

Esta página fornece dados de desempenho em forma de gráfico, mapa do mundo e tabela para as unidades de anúncios da Microsoft do aplicativo selecionado no momento no painel. Para ver esse relatório, selecione um de seus aplicativos no painel e clique em **Análises** &gt; **Desempenho de anúncios** no painel de navegação.

Os dados são obtidos destas métricas de desempenho que rastreamos para os anúncios em seu aplicativo:

-   **Receita estimada**: a quantidade estimada de dinheiro que você recebeu dos anúncios executados em seu aplicativo.
-   **eCPM**: o custo efetivo por milhares de impressões.
-   **Solicitações**: o número de vezes que uma solicitação de anúncio foi enviada do seu aplicativo.
-   **Impressões**: a quantidade de vezes que um anúncio foi exibido em seu aplicativo.
-   **Taxa de preenchimento**: a porcentagem de solicitações de anúncio enviadas de seu aplicativo nas quais um anúncio foi exibido.
-   **Cliques**: a quantidade de vezes que alguém clicou em um anúncio em seu aplicativo.
-   **CTR**: taxa de cliques, indicando a quantidade de vezes que um anúncio foi clicado, dividida pela quantidade de impressões.

Para examinar todas essas métricas de desempenho para as unidades de anúncios em seu aplicativo, consulte a tabela abaixo dos modos de exibição de gráfico e mapa.

Para analisar os dados de uma dessas métricas em um modo de exibição de gráfico ou mapa do mundo, clique em **Gráfico** ou **Mapa**. Clique nos cabeçalhos acima do gráfico ou mapa para alternar entre as métricas diferentes. Por padrão, as visualizações de gráfico e mapa exibem dados de desempenho para todas as unidades de anúncio em seu aplicativo, mas você pode clicar em **Escolher unidades de anúncio** para selecionar até seis unidades de anúncio individuais para comparar.

No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países ou regiões menores.

## Relatório de desempenho de anúncios no nível da conta

Esta página fornece dados de desempenho detalhados para unidades de anúncios da Microsoft e anúncios de comunidade usados em aplicativos que estão registrados em sua conta de desenvolvedor. Para exibir esse relatório, acesse a página de visão geral do painel e clique em **Desempenho de anúncios** no painel de navegação.

Esta página inclui as seções a seguir.

### Microsoft Advertising

Este relatório fornece dados de desempenho para todas as unidades de anúncios da Microsoft usadas em seus aplicativos. O relatório também inclui dados de desempenho para qualquer unidade de anúncio do pubCenter que não foi mapeada com êxito para seus aplicativos do Centro de Desenvolvimento.

Esse relatório mostra as mesmas sete métricas de desempenho e modos de exibição (gráfico, mapa do mundo e tabela) como o relatório de desempenho do anúncio no nível de aplicativo descrito acima. Você pode aplicar os seguintes filtros a esse relatório:

-   **Todas as unidades de anúncio**. Ao selecionar esse filtro, você pode optar por exibir dados de todas as unidades de anúncio ou de até seis unidades de anúncio específicas.
-   **Todos os aplicativos**. Ao selecionar esse filtro, você pode optar por exibir dados de todos os aplicativos ou de até seis aplicativos específicos.
-   **Aplicativo individual**. Ao selecionar um aplicativo, você poderá optar por exibir dados de todas as unidades de anúncio usadas pelo aplicativo ou de até seis unidades de anúncio específicas usadas pelo aplicativo.

Se você criou unidades de anúncio para um aplicativo usando o Microsoft pubCenter, é possível que nem todas elas tenham sido mapeadas com êxito aos seus aplicativos no Centro de Desenvolvimento. Nesse relatório, essas unidades de anúncio são associadas a nomes de aplicativo que você especificou no pubCenter, com a cadeia de caracteres **(pubCenter)** anexada ao nome do aplicativo.

Se você criou unidades de anúncio para um aplicativo usando o pubCenter e não vir dados para elas na página de relatório, clique em **Vincular uma conta do pubCenter** para vincular essa conta à sua conta do Centro de Desenvolvimento. Insira o email que você associou a essa conta do pubCenter e seu código de vinculação de conta. Para encontrar o código de vinculação de contas, entre nessa conta do pubCenter e vá para a página **Minhas informações**.

Para saber mais sobre a migração de contas do pubCenter para o Centro de Desenvolvimento, veja [Integração pubCenter-DevCenter](pubcenter-dev-center-integration.md).

### Anúncios da comunidade da Microsoft

Esta seção fornece dados de desempenho em forma de gráfico e mapa do mundo para anúncios de comunidade no aplicativo selecionado atualmente no painel. Para obter mais informações sobre anúncios da comunidade, veja [Sobre anúncios da comunidade](about-community-ads.md).

Os dados são obtidos destas métricas de desempenho que rastreamos para os anúncios em seu aplicativo:

-   **Solicitações**: o número de vezes que uma solicitação de anúncio de comunidade foi enviada do seu aplicativo.
-   **Taxa de preenchimento**: a porcentagem de solicitações de anúncio de comunidade enviadas de seu aplicativo nas quais um anúncio foi exibido.
-   **Cliques**: a quantidade de vezes que alguém clicou em um anúncio de comunidade em seu aplicativo.
-   **CTR**: taxa de cliques, indicando a quantidade de vezes que um anúncio de comunidade foi clicado, dividida pela quantidade de impressões.
-   **Créditos obtidos**: o número de créditos de anúncios de comunidade obtidos com esse aplicativo. Para obter mais detalhes sobre como os créditos são obtidos, veja [Sobre os anúncios da comunidade](about-community-ads.md).
-   **Créditos gastos**: o número de créditos de anúncios da comunidade gastos para este aplicativo. Para obter mais detalhes sobre como os créditos são gastos, veja [Sobre os anúncios da comunidade](about-community-ads.md).

Para analisar os dados de uma dessas métricas em um modo de exibição de gráfico ou mapa do mundo, clique em **Gráfico** ou **Mapa**. Clique nos cabeçalhos acima do gráfico ou mapa para alternar entre as métricas diferentes. No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países ou regiões menores.

## Relatório de desempenho de anúncios em nível de painel

A seção **Desempenho de anúncios** na página **Visão geral do painel** fornece um resumo dos dados de desempenho para todas as unidades de anúncios da Microsoft usadas em seus aplicativos. Esse relatório é semelhante à seção **Microsoft Advertising** no relatório de desempenho de anúncios em nível de conta, com as seguintes diferenças:

-   Ele fornece os dados apenas em forma de gráfico. Para uma versão em tabela dos dados, use o relatório de desempenho de anúncio a nível de conta.
-   Os únicos filtros fornecidos são para todos os aplicativos ou para aplicativos individuais. Para filtrar por unidades de anúncio, use o relatório de desempenho de anúncios no nível da conta.


 

 


<!--HONumber=Mar16_HO5-->



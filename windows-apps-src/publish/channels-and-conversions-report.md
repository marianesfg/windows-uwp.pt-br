---
Description: o relatório Canais e conversões no painel Centro de Desenvolvimento do Windows permite saber como os clientes no Windows 10 chegaram à listagem de seu aplicativo.
title: relatório Canais e conversões
ms.assetid: C359B9FB-A17B-4A8E-B8EE-19F2F98AA4FF
---

# Relatório Canais e conversões


O relatório **Canais e conversões** no painel Centro de Desenvolvimento do Windows permite saber como os clientes no Windows 10 chegaram à listagem de seu aplicativo. Ele permite acompanhar [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md) de seu aplicativo ou suas IAPs e saber quantas dessas visitas resultaram em novas aquisições. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline.

> **Importante**   Esse relatório mostra apenas os dados de exibição de página e conversão de clientes no Windows 10.

 

Nesse relatório, um *canal* se refere ao método em que um cliente chegou à página de listagem do seu aplicativo (por exemplo, navegação e pesquisa na Loja, um link de um site externo, um link de uma das suas campanhas personalizadas, etc.). Os seguintes tipos de canal estão incluídos:

-   **Tráfego da Loja:** o cliente estava navegando ou pesquisando a Loja quando visualizou a listagem do aplicativo.
-   **Site externo:** o cliente seguiu um link (sem qualquer ID da campanha personalizada) até a listagem do aplicativo em um site.
-   **Mecanismo de pesquisa:** o cliente seguiu um link até a listagem do aplicativo que foi retornada por um mecanismo de pesquisa online.
-   **Campanha personalizada:** o cliente seguiu um link que usava uma [ID da campanha personalizada](create-a-custom-app-promotion-campaign.md).

Uma *exibição de página* significa que um cliente viu a página de listagem da Loja do seu aplicativo, por meio da Loja baseada na Web ou dentro do aplicativo Loja no Windows 10.

Uma *conversão* significa que um cliente obteve recentemente uma licença para o seu aplicativo (seja paga ou gratuita) ou para um IAP.

> **Observação**  Os dados de conversão são fornecidos apenas para suas campanhas personalizadas. Para outros tipos de canal, somente os dados de exibição de página estão incluídos nesse relatório no momento.

 

## Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados nessa página por intervalo de datas e/ou por mercado.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Mercado**: a configuração padrão é **Todos os mercados**. Você poderá escolher um mercado específico, se quiser que essa página mostre somente os detalhes de clientes nesse mercado.
-   **Tipo do dispositivo**: o filtro padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente os dados de clientes que usam esse dispositivo.

As informações em todos os gráficos listados a seguir refletirão o período de tempo selecionado na seção **Aplicar filtros** e refletirá todos os outros filtros que você tenha escolhido aqui.

## Exibições e conversões de página de aplicativo por canal


O gráfico **Exibições e conversões de página de aplicativo por canal** mostra com que frequência a página de listagem do aplicativo foi visualizada e como os clientes chegaram lá. Ele também mostra o número de conversões de campanhas personalizadas durante o período de tempo selecionado.

A guia **Exibições de página** deste gráfico mostra o número de vezes em que a página de listagem do aplicativo foi visualizada durante o período de tempo selecionado. As exibições são agrupadas de acordo com o tipo de canal pelo qual o cliente encontrou a listagem do aplicativo.

A guia **Conversões** deste gráfico mostra o número de conversões (novas aquisições) no período de tempo selecionado para clientes que chegaram à listagem de seu aplicativo por meio de uma campanha personalizada.

> **Observação**  Para obter informações sobre todas as aquisições do seu aplicativo, inclusive aquelas que não ocorreram por meio de um link de campanha personalizada e aquelas de clientes em outras versões do sistema operacional, consulte o relatório [Aquisições](acquisitions-report.md).

 

## Desempenho da campanha por canal


O gráfico **Desempenho da campanha por canal** mostra o número de exibições de página para cada tipo de canal. Ele também mostra o número total de conversões de aplicativo e IAP de suas campanhas personalizadas durante o período de tempo selecionado.

## Exibições e conversões de página de aplicativo por ID de campanha


A gráfico **Exibições e conversões de página de aplicativo por ID de campanha** mostra o número de exibições de página e conversões para cada uma das [IDs de campanha](create-a-custom-app-promotion-campaign.md) durante o período de tempo selecionado.

##  Conversões de IAP por ID da campanha


O gráfico **Conversões de IAP por ID da campanha** mostra o número de conversões de IAP por ID da campanha personalizada.

Quando uma instalação de aplicativo é contabilizada como uma conversão para uma campanha personalizada, as compras de IAP nesse aplicativo também são contabilizadas como conversões para a mesma campanha personalizada.

Por padrão, o relatório inclui qualquer IAP que apresentava uma conversão resultante de um link usando-se uma ID de campanha personalizada durante o período de tempo selecionado. Para exibir dados de um IAP específico, selecione-o nos **Filtros de seção**.

## Divisão de conversões


O gráfico **Divisão de conversões** mostra mais detalhes sobre as exibições de página resultantes de cada um dos seus tipos de canal. Clique em cada tipo de canal para obter mais informações sobre as conversões desse canal:

-   **Campanha personalizada:** mostra as IDs de campanha específicas.
-   **Site externo:** mostra o domínio do site vinculado ao aplicativo.
-   **Tráfego da Loja:** mostra se o cliente estava usando o aplicativo cliente da Loja ou a Loja online.
-   **Mecanismo de pesquisa:** mostra os termos de pesquisa específicos usados pelo cliente.

Para campanhas personalizadas, você também pode ver o número de conversões de aplicativo e de IAP resultantes de cada ID de campanha específica.

 

 






<!--HONumber=Mar16_HO1-->



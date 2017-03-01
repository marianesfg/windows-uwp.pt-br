---
author: shawjohn
Description: "O relatório de Canais e conversões no painel Centro de Desenvolvimento do Windows permite saber como os clientes no Windows 10 chegaram à listagem de seu app."
title: "Relatório Canais e conversões"
ms.assetid: C359B9FB-A17B-4A8E-B8EE-19F2F98AA4FF
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, canais, conversões, relatório, análises"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 60f639c6bad73273a6cc7f83cf65fdf321211ba1
ms.lasthandoff: 02/07/2017

---

# <a name="channels-and-conversions-report"></a>Relatório Canais e conversões


O relatório de **Canais e conversões** no painel Centro de Desenvolvimento do Windows permite saber como os clientes no Windows 10 chegaram à listagem de seu app. Ele permite acompanhar [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md) de seu app ou seus complementos e saber quantas dessas visitas resultaram em novas aquisições. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibi-lo offline.

> **Importante**   Esse relatório mostra apenas os dados de exibição de página e conversão de clientes no Windows 10.

 

Nesse relatório, um *canal* se refere ao método em que um cliente chegou à página de listagem do seu app (por exemplo, navegação e pesquisa na Loja, um link de um site externo, um link de uma das suas campanhas personalizadas, etc.). Os seguintes tipos de canal estão incluídos:

-   **Tráfego da Loja:** o cliente estava navegando ou pesquisando a Loja quando visualizou os detalhes do seu app.
-   **Campanha personalizada:** o cliente seguiu um link que usava uma [ID da campanha personalizada](create-a-custom-app-promotion-campaign.md).
-   **Outros:** o cliente seguiu um link externo (sem qualquer ID de campanha personalizada) de um site para a listagem do seu app ou o cliente seguiu um link de um mecanismo de pesquisa para a listagem do seu app.

Uma *exibição de página* significa que um cliente viu a página de listagem da Loja do seu app, por meio da Loja baseada na Web ou dentro do app Loja no Windows 10.

Uma *conversão* significa que um cliente obteve recentemente uma licença para o seu app (seja paga ou gratuita) ou para um complemento.

Nós não exibimos uma taxa de conversão nesse relatório porque nossos modos de exibição de página e os números de conversão não são contagens de clientes exclusivos.

Os dados de conversão são fornecidos apenas para suas campanhas personalizadas. Para outros tipos de canal, somente os dados de exibição de página estão incluídos nesse relatório.

> **Observação** Os clientes podem chegar à listagem do seu app clicando em uma campanha personalizada que não foi criada por você. Para levar em conta essa possibilidade, marcamos cada modo de exibição de página dentro de uma sessão com a ID da campanha a partir da qual o usuário entra na Loja pela primeira vez. Em seguida, atribuímos uma conversão de seu app para essa ID de campanha se ocorrer uma aquisição de seu app dentro de 24 horas. Quando você exibe seu relatório, esse é o motivo de você poder ver as conversões atribuídas a campanhas que não são familiares para você, de poder ver um número maior de conversões totais que a divisão de conversões, e de ter conversões ou conversões de complemento sem nenhuma visualização de página. Você pode examinar a divisão de conversões por ID de campanha para ver apenas as conversões atribuídas a campanhas criadas por você para avaliar sua eficácia.


## <a name="apply-filters"></a>Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados nessa página por intervalo de datas e/ou por mercado.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Mercado**: a configuração padrão é **Todos os mercados**. Você poderá escolher um mercado específico, se quiser que essa página mostre somente os detalhes de clientes nesse mercado.
-   **Tipo do dispositivo**: o filtro padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente os dados de clientes que usam esse dispositivo.

As informações em todos os gráficos listados a seguir refletirão o período de tempo selecionado na seção **Aplicar filtros** e refletirá todos os outros filtros que você tenha escolhido aqui.

## <a name="app-page-views-and-conversions-by-channel"></a>Exibições e conversões de página de app por canal


O gráfico **Exibições e conversões de página de app por canal** mostra com que frequência a página de listagem do app foi visualizada e como os clientes chegaram lá. Ele também mostra o número de conversões de campanhas personalizadas durante o período de tempo selecionado.

A guia **Exibições de página** deste gráfico mostra o número de vezes em que a página de listagem do app foi visualizada durante o período de tempo selecionado. As exibições são agrupadas de acordo com o tipo de canal pelo qual o cliente encontrou a listagem do app.

A guia **Conversões** deste gráfico mostra o número de conversões (novas aquisições) no período de tempo selecionado para clientes que chegaram à listagem de seu app por meio de uma campanha personalizada.

Para obter informações sobre todas as aquisições do seu app, inclusive aquelas que não ocorreram por meio de um link de campanha personalizada e aquelas de clientes em outras versões do sistema operacional, consulte o [relatório Aquisições](acquisitions-report.md).

 

## <a name="total-campaign-conversions"></a>Total de conversões da campanha


O gráfico **Total de conversões da campanha** também mostra o número total de conversões de app e complemento de campanhas personalizadas durante o período de tempo selecionado.

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Exibições e conversões de página de app por ID de campanha


O gráfico **Exibições e conversões de página de app por ID de campanha** mostra o número de exibições de página e conversões todas as [IDs de campanha](create-a-custom-app-promotion-campaign.md) durante o período de tempo selecionado.

##  <a name="add-on-conversions-by-campaign-id"></a>Conversões de complementos por ID da campanha


O gráfico **Conversões de complementos por ID da campanha** mostra o número de conversões de complementos por ID da campanha personalizada.

Quando uma instalação de app é contabilizada como uma conversão para uma campanha personalizada, as compras de complementos nesse app também são contabilizadas como conversões para a mesma campanha personalizada.

Por padrão, o relatório inclui qualquer complemento que apresentava uma conversão resultante de um link usando-se uma ID de campanha personalizada durante o período de tempo selecionado. Para exibir os dados de um complemento específico, selecione-o nos **Filtros de seção**.

## <a name="conversions-breakdown-by-campaign-id"></a>Divisão de conversões por ID de campanha


O gráfico **Divisão de conversões** mostra os seguintes detalhes sobre os modos de exibição de página e conversões resultantes de campanhas personalizadas.

-   **ID:** mostra as IDs de campanha específica.
-   **Modos de exibição de página:** mostra a contagem de exibições de página marcada com a ID da campanha da primeira entrada do cliente na Loja.
-   **Conversões de app:** mostra o número de conversões de app resultante da campanha personalizada.
-   **Conversões de complemento:** mostra o número de conversões de complemento resultante da campanha personalizada.


 

 


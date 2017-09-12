---
author: jnHs
Description: "O relatório de aquisições de Complemento do painel do Centro de Desenvolvimento do Windows permite que você veja quantos complementos você vendeu, além das informações demográficas e dos detalhes da plataforma."
title: "Relatório de aquisições de complemento"
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 46d5ee3d0e0ac9c2a3599f51e17ea4d7425ab5af
ms.sourcegitcommit: a8e7dc247196eee79b67aaae2b2a4496c54ce253
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2017
---
# <a name="add-on-acquisitions-report"></a>Relatório de aquisições de complemento


O relatório de **aquisições de Complemento** do painel do Centro de Desenvolvimento do Windows permite que você veja quantos complementos você vendeu, além das informações demográficas e dos detalhes da plataforma. Ele também permite obter informações de conversão de clientes no Windows 10.

Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibir offline. Como alternativa, você pode recuperar de forma programática esses dados usando o método [obter aquisições de complemento](../monetize/get-in-app-acquisitions.md) na [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Neste relatório, uma aquisição de complemento significa que um cliente adquiriu um complemento de você (ou adquiriu sem pagar, se for oferecido gratuitamente). Várias compras do mesmo complemento consumível pelo mesmo cliente são contadas como aquisições de complementos separadas.

> [!IMPORTANT]
> O relatório de **Aquisições de complemento** não inclui dados sobre reembolsos, reversões, estornos etc. Para estimar a receita do seu aplicativo, visite [Resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Você também pode expandir **Filtros** para filtrar os dados dessa página por complementos específicos, por mercado e/ou por tipo de dispositivo.

-   **Complemento**: o filtro padrão é **Todos os complementos**, mas você pode limitar os dados a um ou mais complementos do aplicativo.
-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a aquisições em um ou mais mercados.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se desejar mostrar dados para aquisições de um determinado tipo de dispositivo apenas, você pode escolher um específico aqui.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e quaisquer filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


## <a name="add-on-acquisitions"></a>Aquisições de complemento

O gráfico **Aquisições de Complemento** mostra o número de aquisições diárias ou semanais de seus complementos durante o período de tempo selecionado. (Quando você usa **Aplicar filtros** para exibir dados por uma duração maior, os dados de aquisição serão agrupados por semana).

Você também pode ver a quantidade de aquisições do tempo de vida do aplicativo ao selecionar **Cumulativo do complemento**. Isso mostra o total cumulativo de todas as aquisições, desde quando o seu aplicativo foi publicado pela primeira vez.

Você pode filtrar os resultados se a aquisição foi originada do complemento ou da loja Web e/ou por versão do sistema operacional.


## <a name="customer-demographic"></a>Demografia do cliente

O gráfico **Customer demographic** mostra informações demográficas sobre as pessoas que adquiriram seus complementos. Você pode ver quantas aquisições (ao longo do período de tempo selecionado) foram feitas por pessoas em uma determinada faixa etária e por sexo.

> [!NOTE]
> Alguns clientes optaram por não para compartilhar essa informação. Se não for possível determinar a faixa etária ou o sexo, a aquisição será categorizada como **Desconhecida**.


## <a name="markets"></a>Mercados

O gráfico **Mercados** mostra a quantidade total de aquisições de complemento durante o período selecionado para cada mercado em que o aplicativo está disponível. 

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de aquisições ou instalações. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Exibições e conversões de página do complemento por ID de campanha

O gráfico **Visualizações e conversões de página de complemento por ID da campanha** mostra o total de conversões de complemento (aquisições) por ID da campanha durante o período selecionado, ajudando você a acompanhar conversões e visualizações de página dos clientes no Windows 10 para cada uma de suas [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md). Somente as conversões de complemento são mostradas neste gráfico.

> [!NOTE]
> Os clientes podem chegar à listagem do aplicativo ao clicar em uma campanha personalizada que não foi criada por você. Marcamos cada modo de exibição de página em uma sessão com a ID da campanha a partir da qual o cliente entrou na Loja pela primeira vez. Em seguida, nós atribuímos conversões a essa ID de campanha para todas as aquisições nas últimas 24 horas. Por isso, você pode observar um número maior de conversões totais do que o total de conversões para suas IDs de campanha, e você pode ter conversões ou conversões de complemento com nenhuma exibição de página. 


## <a name="conversions-breakdown-by-campaign-id"></a>Divisão de conversões por ID de campanha

O gráfico **Detalhamento de conversões por ID de campanha** permite que você acompanhe as conversões e as exibições de página dos clientes no Windows 10 para cada uma das suas [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md). As conversões de aplicativo e complemento são mostradas por ID da campanha.

Neste gráfico, um *modo de exibição de página* significa que um cliente visualizou a listagem da Loja do aplicativo. Uma *conversão* significa que um cliente obteve recentemente uma licença para o aplicativo ou complemento (seja paga ou gratuita).

Lembre-se que esses números de visualização de página e conversão não são contagens de clientes exclusivos. 


## <a name="top-add-ons"></a>Complementos principais

O gráfico **Complementos principais** mostra o total de aquisições de cada um dos seus complementos durante o período selecionado para que você possa verificar quais deles são mais populares. 



 

 

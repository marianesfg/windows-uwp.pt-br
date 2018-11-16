---
author: jnHs
Description: The Add-on acquisitions report in Partner Center lets you see how many add-ons you've sold, along with demographic and platform details.
title: Relatório de aquisições de complementos
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, vendas de complementos, aquisições de complementos, vendas de iap, produtos no aplicativo, iaps, complementos
ms.localizationpriority: medium
ms.openlocfilehash: 63884d9cce24e6b85f3001ac4c6eb1a07141bfd4
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6852523"
---
# <a name="add-on-acquisitions-report"></a>Relatório de aquisições de complementos


O relatório de **aquisições de complemento** no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja quantos complementos você vendeu, além demográficos e detalhes da plataforma e mostra informações de conversão para clientes no Windows 10 (incluindo o Xbox). Você também pode exibir dados de aquisição em tempo real aproximado para o último período ou setenta e duas horas.

Você pode exibir esses dados no Partner Center, ou [baixar o relatório](download-analytic-reports.md) para exibição offline. Como alternativa, você pode recuperar de forma programática esses dados usando o método [obter aquisições de complementos](../monetize/get-in-app-acquisitions.md) na [API REST de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

Neste relatório, uma aquisição de complemento significa que um cliente adquiriu um complemento de você (ou adquiriu sem pagar, se for oferecido gratuitamente). Várias compras do mesmo complemento consumível pelo mesmo cliente são contadas como aquisições de complementos separadas.

> [!IMPORTANT]
> O relatório de **Aquisições de complemento** não inclui dados sobre reembolsos, reversões, estornos etc. Para estimar a receita do seu aplicativo, visite [Resumo de pagamento](payout-summary.md). Na seção **Reservado**, clique no link **Download reserved transactions**.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar. Você também pode selecionar **H 1** ou **72h** para mostrar dados de aquisição em tempo quase real para uma hora ou setenta e duas horas. Esses períodos de tempo só se aplicam à guia **complementar diariamente** do gráfico de **aquisições de complemento** e para a guia de **aquisições** do gráfico de **mercados** . 

Você também pode expandir **Filtros** para filtrar os dados dessa página por complementos específicos, por mercado e/ou por tipo de dispositivo.

-   **Complemento**: o filtro padrão é **Todos os complementos**, mas você pode limitar os dados a um ou mais complementos do aplicativo.
-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a aquisições em um ou mais mercados.
-   **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Se você deseja mostrar dados de aquisições apenas de um determinado tipo de dispositivo (por exemplo, computador, console ou tablet), pode escolher um dispositivo específico aqui.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


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

O gráfico **Add-on page views and conversions by campaign ID** mostra o total de conversões de complementos (aquisições) por ID de campanha durante o período selecionado, ajudando você a acompanhar conversões e exibições de página de clientes do Windows 10 (incluindo o Xbox) para cada uma de suas [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md). Somente as conversões de complementos são mostradas neste gráfico.

> [!NOTE]
> Os clientes podem chegar à listagem do aplicativo ao clicar em uma campanha personalizada que não foi criada por você. Marcamos cada modo de exibição de página em uma sessão com a ID da campanha a partir da qual o cliente entrou na Loja pela primeira vez. Em seguida, nós atribuímos conversões a essa ID de campanha para todas as aquisições nas últimas 24 horas. Por isso, você pode observar um número maior de conversões totais do que o total de conversões para suas IDs de campanha, e você pode ter conversões ou conversões de complemento com nenhuma exibição de página. 


## <a name="conversions-breakdown-by-campaign-id"></a>Divisão de conversões por ID de campanha

O gráfico **Detalhamento de conversões por ID de campanha** permite que você acompanhe as conversões e as exibições de página dos clientes no Windows 10 para cada uma das suas [campanhas de promoção personalizadas](create-a-custom-app-promotion-campaign.md). As conversões de aplicativo e complemento são mostradas por ID da campanha.

Neste gráfico, um *modo de exibição de página* significa que um cliente visualizou a listagem da Loja do aplicativo. Uma *conversão* significa que um cliente obteve recentemente uma licença para o aplicativo ou complemento (seja paga ou gratuita).

Lembre-se que esses números de visualização de página e conversão não são contagens de clientes exclusivos. 


## <a name="top-add-ons"></a>Complementos principais

O gráfico **Complementos principais** mostra o total de aquisições de cada um dos seus complementos durante o período selecionado para que você possa verificar quais deles são mais populares. 



 

 

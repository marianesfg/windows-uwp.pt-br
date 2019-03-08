---
Description: O relatório de classificações no Partner Center permite ver como os clientes classificada como seu aplicativo na Store.
title: Relatório de classificações
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, classificação, estrelas em estrela, da taxa, classificados
ms.localizationpriority: medium
ms.openlocfilehash: 9b507212cd660a025bc3d858fc2938cf59d4219f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640051"
---
# <a name="ratings-report"></a>Relatório de classificações


O **classificações** de relatórios no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja como os clientes classificada como seu aplicativo na Store. 

Você pode exibir esses dados no Partner Center, ou [baixar o relatório](download-analytic-reports.md) exibir offline. Como alternativa, você pode programaticamente recuperar esses dados usando o [obter revisões de aplicativo](../monetize/get-app-reviews.md) método na [API de REST de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Para visualizar rapidamente avaliações, classificações e comentários do usuário para todos seus aplicativos nos últimos 30 dias, expanda **Interagir** no menu de navegação esquerdo e selecione **Avaliações e comentários.** 

## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar as análises. A seleção padrão é **Tempo de vida**, mas você pode optar por mostrar análises de 30 dias, 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar. Observe que os gráficos de **Análise de classificações** e **Média de classificação ao longo do tempo** sempre mostram dados dos últimos 12 meses; as opções desse período não afetarão os gráficos.

É possível expandir os **Filtros** para filtrar as análises mostradas nessa página pelas seguintes opções. Esses filtros não serão aplicado aos gráficos de **Análise de classificações** e **Média de classificação ao longo do tempo**.

-   **Mercado**: A configuração padrão é **todos os mercados**. Você pode escolher um mercado específico, se quiser que esta página mostre somente as classificações de clientes desse mercado.
-   **Tipo de dispositivo**: O filtro padrão é **todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente classificações deixadas por clientes que estão usando esse dispositivo.
-   **Versão do sistema operacional**: A configuração padrão é **todos os**. Se você quiser que essa página para mostrar apenas à esquerda de classificações por clientes nessa versão do sistema operacional, você pode escolher uma versão específica do sistema operacional.
-   **Nome da categoria**: O filtro padrão é **todos os**. Você pode optar por mostrar somente as classificações associadas com as revisões que nós identificamos como pertencente a um determinado [examine a categoria de insight](reviews-report.md#insight-categories) para mostrar apenas as revisões que podemos associadas à categoria. 

> [!TIP]
> Se você não vir quaisquer classificações na página, verifique se que seus filtros ainda não excluídas todas as suas classificações. Por exemplo, se você filtrar por um sistema operacional de destino não dá suporte a seu aplicativo, você não verá as classificações.


## <a name="rating-breakdown"></a>Divisão de classificação

O **divisão de classificação** gráfico mostra: 
- A classificação média por estrela do aplicativo.
- O total de classificações do aplicativo nos últimos 12 meses.
- O total de classificações de cada classificação por estrelas.
- O número de classificações para cada tipo de classificação (nova ou revisada) por classificação de estrelas nos últimos 12 meses.
 - **Classificações novas** são classificações que os clientes enviaram, mas que não foram alteradas.
 - **Classificações revisadas** são classificações que foram alteradas pelo cliente, até mesmo o texto da opinião.

> [!TIP]
> A classificação média que um cliente visualiza na Loja leva em consideração o mercado e o tipo de dispositivo do cliente, portanto, pode ser diferente do que você vê nesse relatório.


## <a name="average-rating"></a>Classificação média

O **classificação média** gráfico mostra como a classificação média do aplicativo foi alterado nos últimos 12 meses.

Em vez de calcular a média de todos os restantes de classificações durante os últimos 12 meses (como mostra a **análise de classificação** gráfico), o **classificação média** gráfico mostra como os clientes classificada como o aplicativo em uma determinada semana. Isso pode ajudar a identificar tendências ou determinar se classificações foram afetadas por atualizações ou outros fatores.

## <a name="rating-by-market"></a>Classificação por mercado

O **classificação por mercado** gráfico mostra uma divisão das classificações de média em cada mercado ao longo do período de tempo selecionado. Por padrão, vamos mostrar esses dados em um visual **mapa** do formulário, mas você também pode alternar para o controle no canto superior direito para exibi-lo como um **tabela**.

O **tabela** exibição mostra cinco mercados por vez, classificados em ordem alfabética ou por classificação média maior/menor. Você também pode baixar os dados deste gráfico para exibir informações para todos os mercados juntos.

Você também pode filtrar este gráfico pela **classificação**. Por padrão, as revisões com todas as classificações por estrelas são verificadas, mas você pode verificar e desmarque as classificações específicas (de 1 a 5 estrelas) se você quiser apenas ver análises associados com classificações por estrelas específicas.
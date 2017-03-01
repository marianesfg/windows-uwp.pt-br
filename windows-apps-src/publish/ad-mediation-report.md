---
author: jnHs
Description: "O relatório de controle de anúncios permite que você veja sua taxa de preenchimento efetiva e as taxas de preenchimento respectivas para as redes de publicidade que está usando."
title: "Relatório de controle de anúncios"
ms.assetid: 18A33928-B9F2-4F76-9A9C-F01FEE42FEA1
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3f3f3dd3b594e174def693b7decd21a5753172b9
ms.lasthandoff: 02/07/2017

---

# <a name="ad-mediation-report"></a>Relatório de controle de anúncios


O relatório de **controle de anúncios** permite que você veja sua taxa de preenchimento efetiva e as taxas de preenchimento respectivas para as redes de publicidade que está usando. Ele também mostra as taxas de adoção de cada uma das suas configurações de controle e dá visibilidade aos erros relatados por redes de publicidade e pelo mediador. Você pode visualizar esses dados em seu painel ou [baixar o relatório](download-analytic-reports.md) para vê-lo offline.

**Importante**  O relatório de **controle de anúncios** fornece dados apenas se você estiver usando o [controle de anúncios do Windows](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359) em seu app.

 

## <a name="page-filters"></a>Filtros de página


Na parte superior da página, você pode expandir os **Filtros de página** para filtrar todos os dados dessa página por intervalo de datas e/ou por mercado.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um mercado específico, se quiser que esta página mostre somente as classificações de clientes desse mercado.
-   **Plataforma**: o ajuste padrão é **Todas as plataformas**. Se o seu app servir para várias plataformas, você poderá escolher uma plataforma específica.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado nos **Filtros de página**. Por padrão, isso incluirá dados de todos os mercados e plataformas nos quais seus app está listado, a menos que você tenha usado os **Filtros de página** para especificar um mercado e/ou uma plataforma específicos.

## <a name="ad-mediation-performance"></a>Desempenho da controle de anúncios


O gráfico **Desempenho do controle de anúncios** mostra a taxa de preenchimento total média durante o período de tempo selecionado. Essa é a taxa média de preenchimento entre todas as sessões de usuário, independentemente da sua configuração de controle ou da frequência com que redes de publicidade diferentes foram chamadas.

Você pode clicar no título **Solicitações de controle** para ver o número médio de solicitações de controle individuais, ou clicar em **Anúncios entregues** para ver o número médio total de anúncios entregues.

## <a name="ad-provider-fill-rates"></a>Taxas de preenchimento do provedor de anúncios


O gráfico **Taxas de preenchimento de provedor de anúncios** mostra a taxa média de preenchimento de cada uma das suas redes de publicidade ao longo do período de tempo selecionado.

Informações de cada rede de publicidade são mostradas juntas para ajudá-lo a comparar o desempenho de cada rede de publicidade.

## <a name="unique-users-per-mediation-configuration"></a>Usuários exclusivos por configuração de controle


O gráfico **Usuários exclusivos por configuração de controle** mostra o número total de usuários exclusivos que receberam cada versão de sua configuração de controle durante o período de tempo selecionado.

## <a name="errors-by-ad-network"></a>Erros por rede de publicidade


O gráfico **Erros por rede de publicidade** mostra o número total de erros e solicitações de cada uma das suas redes de publicidade, juntamente com a porcentagem de solicitações que resultou em um erro.

## <a name="errors-by-type"></a>Erros por tipo


O gráfico **Erros por tipo** mostra os erros específicos ocorridos em cada rede de publicidade. Ele também mostra o percentual do total de erros dessa rede que um erro específico representa; portanto, você pode ter uma ideia de que erros estão ocorrendo com frequência para cada rede de publicidade.

 

 






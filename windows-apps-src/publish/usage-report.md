---
author: jnHs
Description: "O relatório de uso no painel do Centro de Desenvolvimento do Windows permite ver como os clientes estão usando o aplicativo."
title: "Relatório de uso"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: f15b72a814c713a389a1f6126e2261959a16ee6c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="usage-report"></a>Relatório de uso


O relatório **Uso** no painel do Centro de Desenvolvimento do Windows permite ver como os clientes estão usando o aplicativo no Windows 10 e obter informações sobre eventos personalizados definidos por você. É possível exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) a fim de exibi-lo off-line.

> **Observação**  Anteriormente, o relatório **Uso** só fornecia dados se você ativasse o SDK do Visual Studio Application Insights no aplicativo. Com o relatório **Uso** atualizado, isso não é mais necessário.

## <a name="apply-filters"></a>Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados desta página pelo intervalo de datas e/ou grupo de produtos (versões relacionadas do sistema operacional).

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 3 meses**.
-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Tipo de dispositivo**: A configuração padrão é **Tudo**, mas é possível optar por mostrar dados apenas de um tipo de dispositivo específico.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado em **Aplicar filtros**. Por padrão, isso incluirá dados de todas as versões do pacote e os tipos de dispositivo compatíveis, a menos que você tenha usado a seção **Aplicar filtros** para filtrar apenas um.

> **Observação** Somente os dados de uso dos clientes no Windows 10 estão incluídos nesse relatório.

## <a name="total-user-sessions"></a>Total de sessões de usuário

O gráfico **Total de sessões de usuário** mostra o número de sessões de usuário diário do seu aplicativo durante o período de tempo selecionado.

Cada sessão do usuário representa um período distinto quando um cliente interagiu com o aplicativo. Considera-se cada sessão do usuário encerrada após um período de inatividade, de maneira que um único cliente possa ter várias sessões de usuário ao longo do mesmo dia. Este gráfico não acompanha usuários exclusivos do aplicativo.

## <a name="active-users"></a>Usuários ativos

O gráfico **Usuários ativos** mostra o número de clientes que usaram seu aplicativo em um dia específico durante o período de tempo selecionado.

Cada usuário ativo representa um cliente que usou seu aplicativo nesse dia. Este gráfico não rastreia sessões de usuário únicos (ou seja, um cliente é representado neste gráfico se eles usou seu aplicativo apenas uma vez ou várias vezes nesse dia).

## <a name="custom-events"></a>Eventos personalizados

O gráfico **Eventos personalizados** mostra o total de ocorrências de quaisquer eventos personalizados definidos por você para o aplicativo. Isso pode incluir várias ocorrências do mesmo cliente.

Eventos personalizados são implementados usando-se o método [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) no [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).



 

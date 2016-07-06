---
author: jnHs
Description: "O relatório de uso no painel do Centro de Desenvolvimento do Windows permite ver como os clientes estão usando o aplicativo."
title: "Relatório de uso"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# Relatório de uso


O relatório **Uso** no painel do Centro de Desenvolvimento do Windows permite ver como os clientes estão usando o aplicativo e obter informações sobre eventos personalizados definidos por você. É possível exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) a fim de exibi-lo off-line.

> **Observação**  Anteriormente, o relatório **Uso** só fornecia dados se você ativasse o SDK do Visual Studio Application Insights no aplicativo. Com o relatório **Uso** atualizado, isso não é mais necessário.

## Aplicar filtros


Na parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados desta página pelo intervalo de datas e/ou grupo de produtos (versões relacionadas do sistema operacional).

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Tipo de dispositivo**: A configuração padrão é **Tudo**, mas é possível optar por mostrar dados apenas de um tipo de dispositivo específico.

As informações de todos os gráficos listados abaixo refletirão o período de tempo selecionado em **Aplicar filtros**. Por padrão, isso incluirá dados de todas as versões do pacote e os tipos de dispositivo compatíveis, a menos que você tenha usado a seção **Aplicar filtros** para filtrar apenas um.

## Total de sessões de usuário

O gráfico **Total de sessões de usuário** mostra o número de sessões de usuário diário do seu aplicativo durante o período de tempo selecionado.

Cada sessão do usuário representa um período distinto quando um cliente interagiu com o aplicativo. Considera-se cada sessão do usuário encerrada após um período de inatividade, de maneira que um único cliente possa ter várias sessões de usuário ao longo do mesmo dia. Este gráfico não acompanha usuários exclusivos do aplicativo.

## Usuários ativos

O gráfico **Usuários ativos** mostra o número de clientes que usaram seu aplicativo em um dia específico durante o período de tempo selecionado.

Cada usuário ativo representa um cliente que usou seu aplicativo nesse dia. Este gráfico não rastreia sessões de usuário únicos (ou seja, um cliente é representado neste gráfico se eles usou seu aplicativo apenas uma vez ou várias vezes nesse dia).

## Eventos personalizados

O gráfico **Eventos personalizados** mostra o total de ocorrências de quaisquer eventos personalizados definidos por você para o aplicativo. Isso pode incluir várias ocorrências do mesmo cliente.

Eventos personalizados são implementados usando-se o método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) no [SDK de Microsoft Store Engagement and Monetization](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).



 







<!--HONumber=Jun16_HO4-->



---
author: jnHs
Description: The Usage report in the Windows Dev Center dashboard lets you see how customers are using your app.
title: Relatório de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, uso, evento personalizado, relatório, telemetria, sessões de usuário
ms.localizationpriority: medium
ms.openlocfilehash: 96d36ebbaa2b7f1a650e2b0f794a1976c1f525a6
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4319458"
---
# <a name="usage-report"></a>Relatório de uso


O relatório **Uso** no painel do Centro de Desenvolvimento do Windows permite ver como os clientes estão usando o aplicativo no Windows 10 (incluindo o Xbox) e mostra informações sobre eventos personalizados definidos por você. Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para exibição offline.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, mercado e/ou por tipo de dispositivo.

-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Mercado**: o filtro padrão é **Todos os mercados**, mas você pode limitar os dados a um ou mais mercados.
-   **Tipo de dispositivo**: A configuração padrão é **Tudo**, mas é possível optar por mostrar dados apenas de um tipo de dispositivo específico (computador, console, tablet etc.)

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados (exceto **Novos usuários** no gráfico **Uso**, que não será exibido se os filtros estiverem selecionados). Algumas seções também permitem que você aplique mais filtros.

> [!IMPORTANT]
> Esse relatório inclui apenas os dados de uso de clientes no Windows 10 (incluindo o Xbox) que não se recusaram a fornecer informações de telemetria. Os dados de uso para jogos Xbox são incluídos aqui, não importa se o cliente estava ou não conectado ao Xbox Live. 


## <a name="usage"></a>Uso

O gráfico **Uso** mostra detalhes sobre como os clientes estão usando seu aplicativo durante o período selecionado. Observe que este gráfico não rastreia usuários únicos do aplicativo ou sessões de usuário único (ou seja, um usuário é representado neste gráfico caso tenha usado seu aplicativo apenas uma vez ou várias vezes).

Este gráfico tem quatro guias separadas que você pode exibir, mostrando o uso por dia ou semana (dependendo da duração selecionada).

- **Usuários**: mostra o total de **sessões de usuário** durante o período de tempo selecionado. Cada sessão do usuário representa um período distinto, a partir do qual o app é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade. Por isso, um único cliente pode ter várias sessões do usuário ao longo do mesmo dia ou semana. O total de **Usuários ativos** (qualquer cliente que usa o aplicativo no dia ou na semana) e **novos usuários** (um cliente que usou o aplicativo pela primeira vez nesse dia ou na semana) também são mostrados. Observe que se você tiver aplicado quaisquer filtros à página, você não verá **Novos usuários** neste gráfico.
- **Dispositivos**: mostra a quantidade de dispositivos diários usados para interagir com seu aplicativo por todos os usuários.
- **Duração**: mostra o total de horas de envolvimento (horas em que um usuário está usando ativamente o app).
- **Retenção**: mostra o total de **DAU/MAU** (usuários diários ativos/usuários mensais ativos) durante o período selecionado.

Quando o **30d** período é selecionado, você poderá ver marcadores em círculo ao exibir as guias de **usuários**, **dispositivos**ou **duração** . Eles representam um aumento significativo ou diminuir um valor determinado que acreditamos que você vai querer saber sobre. A data em que o círculo é exibido representa o final da semana em que detectamos uma significativo aumento ou redução em comparação comparada a semana anterior. Para ver mais detalhes sobre o que mudou, passe o mouse sobre o círculo.  

> [!TIP]
> Você pode exibir mais insights relacionados a alterações significativas nos últimos 30 dias no [relatório de ideias](insights-report.md).


## <a name="user-sessions"></a>Sessões de usuário

O gráfico **Sessões de usuário** mostra o total de sessões de usuário do seu aplicativo por mercado durante o período selecionado.

Como com as informações de **Sessões de usuário** no gráfico **Uso**, uma sessão de usuário representa um período distinto quando um cliente interagiu com o seu aplicativo, e este gráfico não rastreia usuários únicos do aplicativo.

Você pode exibir esses dados em **Mapa** visual ou alternar a configuração para exibi-la como **Tabela**. A forma de Tabela mostrará cinco mercados ao mesmo tempo, classificados em ordem alfabética ou pela quantidade maior/menor de sessões do usuário. Também é possível baixar os dados para visualização das informações de todos os mercados juntos.


## <a name="package-version"></a>Versão do pacote

O gráfico **Sessões de usuário** mostra o total de usuários diários do aplicativo por versão de pacote durante o período selecionado.

Assim como no gráfico de **Sessões do usuário**, uma sessão do usuário representa um período distinto quando um cliente interagiu com o aplicativo, e este gráfico não rastreia usuários únicos do aplicativo.


## <a name="custom-events"></a>Eventos personalizados

O gráfico **Eventos personalizados** mostra o total de ocorrências de eventos personalizados definidos por você para o aplicativo. Isso pode incluir várias ocorrências do mesmo cliente. É possível usar os filtros para selecionar os eventos personalizados específicos para os quais você deseja ver esses dados.

Eventos personalizados são implementados usando-se o método [StoreServicesCustomEventLogger](https://docs.microsoft.com/en-us/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) no [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).

Para obter mais informações, consulte [Registrar eventos personalizados do Centro de Desenvolvimento](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Detalhamento de eventos personalizados

O gráfico **Detalhamento de eventos personalizado** mostra mais detalhes sobre a frequência com que cada um dos eventos personalizados ocorreu. Isso pode ajudar você a determinar se os eventos ocorrem com mais frequência em um mercado específico, um tipo de dispositivo ou em versões do pacote.

Para cada evento, você verá o nome do evento e uma contagem que corresponde a uma combinação específica de mercado do usuário, tipo de dispositivo e versão do pacote. Normalmente, você verá um evento listado várias vezes juntamente com diferentes combinações desses fatores. 




 

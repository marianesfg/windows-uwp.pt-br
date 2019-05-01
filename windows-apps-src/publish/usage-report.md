---
Description: O relatório de uso no Partner Center permite ver como os clientes estão usando seu aplicativo.
title: Relatório de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, uso, evento personalizado, relatório, telemetria, sessões de usuário
ms.localizationpriority: medium
ms.openlocfilehash: b8f368e5af13628d5330ab96ab3b7675de410505
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788205"
---
# <a name="usage-report"></a>Relatório de uso


O **uso** de relatórios no [Partner Center](https://partner.microsoft.com/dashboard) permite que você veja como os clientes no Windows 10 (incluindo Xbox) estão usando seu aplicativo e mostra informações sobre eventos personalizados que você definiu. Você pode exibir esses dados no Partner Center, ou [baixar o relatório](download-analytic-reports.md) exibir offline.


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, mercado e/ou por tipo de dispositivo.

-   **Versão do pacote**: A configuração padrão é **todos os**. Se o aplicativo incluir mais de um pacote, será possível escolher um específico aqui.
-   **Mercado**: O filtro padrão é **todos os mercados**, mas você pode limitar os dados para um ou mais mercados.
-   **Tipo de dispositivo**: A configuração padrão é **todos os**, mas você pode optar por mostrar dados para apenas um tipo de dispositivo específico (PC, console, tablet, etc.).

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e todos os filtros selecionados (exceto **Novos usuários** no gráfico **Uso**, que não será exibido se os filtros estiverem selecionados). Algumas seções também permitem que você aplique mais filtros.

> [!IMPORTANT]
> Esse relatório inclui apenas os dados de uso de clientes no Windows 10 (incluindo o Xbox) que não se recusaram a fornecer informações de telemetria. Os dados de uso para jogos Xbox são incluídos aqui, não importa se o cliente estava ou não conectado ao Xbox Live. 


## <a name="usage"></a>Uso

O gráfico **Uso** mostra detalhes sobre como os clientes estão usando seu aplicativo durante o período selecionado. Observe que este gráfico não rastreia usuários únicos do aplicativo ou sessões de usuário único (ou seja, um usuário é representado neste gráfico caso tenha usado seu aplicativo apenas uma vez ou várias vezes).

Este gráfico tem guias separadas que você pode exibir, mostrando o uso por dia ou semana (dependendo da duração que você selecionou).

- **Usuários**: Mostra o número total de **sessões de usuário** ao longo do período de tempo selecionado. Cada sessão do usuário representa um período distinto, a partir do qual o app é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade. Por isso, um único cliente pode ter várias sessões do usuário ao longo do mesmo dia ou semana. O total de **Usuários ativos** (qualquer cliente que usa o aplicativo no dia ou na semana) e **novos usuários** (um cliente que usou o aplicativo pela primeira vez nesse dia ou na semana) também são mostrados. Observe que se você tiver aplicado quaisquer filtros à página, você não verá **Novos usuários** neste gráfico.
- **Dispositivos**: Mostra o número de dispositivos de diários usado para interagir com seu aplicativo por todos os usuários.
- **Duração**: Mostra as horas de compromisso total (horas em que um usuário está usando ativamente seu aplicativo).
- **Engagement**: Mostra os minutos de engagement média por usuário (duração média de todas as sessões de usuário). 
- **Retenção**: Mostra o número total de **MAU/DAU** (usuários ativos diariamente usuários/mensal Active Directory) ao longo do período de tempo selecionado.
- **Previsão de rotatividade**: Mostra quantos usuários podemos prever têm probabilidade de parar usando seu aplicativo em breve, com base no uso recente.

Quando o **30 1!d** período de tempo estiver selecionado, você poderá ver os marcadores de círculo ao exibir o **usuários**, **dispositivos**, ou **duração** guias. Eles representam um aumento significativo ou diminuir em um determinado valor, acreditamos que você vai querer saber sobre. A data em que o círculo aparece representa o fim da semana em que foi detectado um aumento significativo ou diminuição em comparação comparada a semana anterior que. Para ver mais detalhes sobre o que mudou, passe o mouse sobre o círculo.  

> [!TIP]
> Você pode exibir mais insights relacionados a alterações significativas nos últimos 30 dias na [relatório de Insights](insights-report.md).


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




 

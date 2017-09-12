---
author: jnHs
Description: "Para visualizar dados de desempenho para as unidades de anúncio em seus aplicativos, use os relatórios de desempenho de anúncios em nível de aplicativo e de conta no painel do Centro de Desenvolvimento do Windows."
title: "Relatório de desempenho de anúncios"
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c46533c6762ddd2f47a4dbd40c253bc3d8f346d7
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="advertising-performance-report"></a>Relatório de desempenho de anúncios


O **Relatório de desempenho de anúncios** mostra o desempenho das [unidades de anúncio](monetize-with-ads.md#available-ad-units), incluindo anúncios de comunidade e afiliadas. Esse relatório inclui dados de diversos provedores de anúncios em aplicativos UWP que usam a [mediação de anúncios](monetize-with-ads.md#mediation). 

Para exibir esse relatório, expanda **Analisar** no menu de navegação esquerdo e selecione **Desempenho do anúncio**. 

Para realizar uma análise mais profunda dos dados, fornecemos um link de **Baixar relatório** que você pode usar para baixar arquivos CSV (valores separados por vírgula), os quais podem ser abertos no Microsoft Excel ou em um programa similar. Como alternativa, você pode recuperar de forma programática esses dados usando o método [obter dados de desempenho de anúncios](../monetize/get-ad-performance-data.md) na [API REST de análise da Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Ao exibir os relatórios de desempenho de publicidade, observe que os dados dos relatórios dos últimos três dias podem mudar à medida que recebemos e processamos novos dados de várias fontes. Além disso, as novas declarações de dados podem ocorrer para até 90 dias retroativos.


## <a name="overall-performance"></a>Desempenho geral

Na parte superior do relatório, você pode usar os filtros a seguir para ajustar o escopo dos dados mostrados no relatório:

* **Data**: filtra o relatório por um período predefinido ou intervalo de datas personalizado. Por padrão, o relatório mostra os dados dos últimos 30 dias.
* **Agregação**: aqui você pode selecionar como esses dados são agregados e como podem ser filtrados ainda mais. Por padrão, o relatório mostra dados de todas as unidades de anúncio, e você verá um link inferior **Escolher unidades de anúncio** na seção, permitindo que você selecione até seis unidades de anúncio para comparar. Como opção, você pode alterar a **Agregação** para **Todos os aplicativos** ou **Todos os provedores de anúncios**. Se você fizer isso, o link nesta seção mudará para **Escolher aplicativos** ou **Escolher provedores de anúncios**, permitindo que você escolha até seis de cada para comparar. Também é possível selecionar agregar por um aplicativo específico no qual você usa anúncios.
* **Provedores de anúncio**: filtra o relatório pelos dados de desempenho de determinados provedores de anúncios. Para obter mais informações sobre os provedores de anúncios disponíveis, consulte a seção [Controle de anúncios](monetize-with-ads.md#mediation) em [Monetizar com anúncios](monetize-with-ads.md). Por padrão, o relatório mostra dados de todos os provedores de anúncios. Essa opção será desabilitada se você escolher **Todos os provedores de anúncios** na lista suspensa **Agregação**.
* **Dispositivo**: filtra o relatório pelos dados de desempenho de determinados tipos de dispositivo. Por padrão, o relatório mostra dados de todos os tipos de dispositivo.


## <a name="report-views"></a>Modos de exibição de relatório

Abaixo dos filtros, o relatório exibe dados de várias métricas de desempenho de anúncio nos formatos de gráfico, mapa-múndi e tabela das unidades publicitárias usadas no aplicativo atual.

Para analisar os dados de uma dessas métricas em um modo de exibição de gráfico ou mapa, clique em **Gráfico** ou **Mapa**. Por padrão, as visualizações de gráfico e mapa exibem dados de desempenho para todas as unidades de anúncio, aplicativos ou provedores de anúncio (dependendo das seleções no menu suspenso **Agregação**, mas você tem a opção de selecionar até seis unidades de anúncio individuais, aplicativos ou provedores de anúncio para comparar).

No modo de exibição de mapa, os tons mais escuros representam valores maiores e os tons mais claros representam valores menores. Você pode focalizar em um país ou uma região específica no mapa para analisar o valor da métrica selecionada. Você também pode ampliar qualquer área do mapa para ver dados de países menores.

Para analisar todas as métricas de desempenho das unidades publicitárias no aplicativo, consulte a tabela abaixo dos modos de exibição de gráfico e mapa.

> [!NOTE]
> Se você criou unidades de anúncio para um aplicativo usando o Microsoft pubCenter, é possível que nem todas tenham sido mapeadas com êxito para seus aplicativos no Centro de Desenvolvimento. Nesse relatório, essas unidades de anúncio são associadas a nomes de aplicativo que você especificou no pubCenter, com a cadeia de caracteres **(pubCenter)** anexada ao nome do aplicativo.


## <a name="performance-metrics"></a>Métricas de desempenho

Esse relatório pode incluir dados para as seguintes métricas de desempenho. As métricas mostradas no relatório variam de acordo com o provedor de anúncios.

|  Métrica  |  Descrição  |
|----------|---------------|
| Receita estimada  |  O montante estimado de dinheiro que você recebeu dos anúncios executados em seu aplicativo. |
| eCPM  |  O custo efetivo por milhar de impressões. |
| Solicitações  | O número de vezes que uma solicitação de anúncio foi enviada do seu aplicativo.  |
| Impressões  | O número de vezes que um anúncio foi exibido em seu aplicativo.  |
| Taxa de preenchimento  | A porcentagem de solicitações de anúncio enviadas do aplicativo em que um anúncio foi exibido.  |
| Cliques  |  O número de vezes que uma pessoa clicou em um anúncio no seu aplicativo. |
| CTR  |  Taxa de cliques, que significa o número de vezes que um anúncio foi clicado, dividido pelo número de impressões. |
| Créditos obtidos  | Nos [anúncios de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos que você ganhou para espaço publicitário promocional, mostrando anúncios de comunidade em seu aplicativo.  |
| Créditos gastos  | Nos [anúncios de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), isso indica o número de créditos gastos em anúncios para seu aplicativo.  |


## <a name="affiliates-performance"></a>Desempenho de afiliadas

Se você [aceitou o programa de anúncios de afiliadas da Microsoft](about-affiliate-ads.md), é possível consultar os dados de desempenho dos anúncios de afiliadas que aparecem no seu aplicativo aqui. Essas informações são atualizadas diariamente. 


Na parte superior do relatório, você pode usar os filtros a seguir para ajustar o escopo dos dados mostrados no relatório:
- **Data**: filtra o relatório por um período predefinido ou intervalo de datas personalizado. Por padrão, o relatório mostra os dados dos últimos 30 dias.
- **Dispositivo**: filtra o relatório pelos dados de desempenho de determinados tipos de dispositivo. Por padrão, o relatório mostra dados de todos os tipos de dispositivo.

Por padrão, esse relatório fornece um resumo dos dados de desempenho para anúncios de afiliadas em todos os aplicativos que você aceitou no programa de anúncios de afiliadas da Microsoft na forma de gráficos e tabelas. Você pode selecionar **Escolher aplicativos** a fim de selecionar até seis aplicativos para comparar.

Os dados de desempenho de afiliadas são obtidos destas sete métricas de desempenho que rastreamos para os anúncios de afiliadas em seu aplicativo:

-   **Lucros estimados (aprovados)**: o valor estimado de dinheiro que você recebeu como uma comissão para as compras aprovadas feitas por usuários clicando nos anúncios de afiliadas em seu aplicativo.
-   **Lucros estimados (aprovação pendente)**: o valor estimado de dinheiro que você poderia receber como uma comissão para as compras que estão com aprovação pendente.
-   **Impressões**: a quantidade de vezes em que um anúncio de afiliada foi exibido em seu aplicativo.
-   **Cliques**: a quantidade de vezes em que alguém clicou em um anúncio de afiliada em seu aplicativo.
-   **CTR**: taxa de cliques, indicando a quantidade de vezes em que um anúncio de afiliada foi clicado, dividida pela quantidade de impressões de anúncios de afiliadas.
-   **Compras (aprovadas)**: o número de compras aprovadas feitas por usuários clicando em anúncios de afiliadas em seu aplicativo.
-   **Compras (aprovação pendente)**: o número de compras com aprovação pendente que foram feitas por usuários clicando em anúncios de afiliadas em seu aplicativo.

> [!NOTE]
> Depois que um usuário comprar um produto na Loja, haverá um período de espera de 45 dias até que a compra seja aprovada para o programa de anúncios de afiliada. Por causa desse período de espera, os dados de **Lucros estimados (aprovados)**, **Lucros estimados (aprovação pendente)**, **Compras (aprovadas)** e **Compras (aprovação pendente)** de um dia específico podem mudar depois que as compras são aprovadas ou rejeitadas.


 

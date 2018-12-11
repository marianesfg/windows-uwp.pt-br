---
Description: The Xbox analytics report in Partner Center shows you statistics about how your customers are engaging with the Xbox features in your product.
title: Relatório de análise do Xbox
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, análise do Xbox, análise do Xbox em tempo real, estatística do Xbox
ms.localizationpriority: medium
ms.openlocfilehash: ae9bacd88f957954c5cd1d3f6ccd6d3c04a568a2
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922387"
---
# <a name="xbox-analytics-report"></a>Relatório de análise do Xbox

O relatório de **análise do Xbox** no [Partner Center](https://partner.microsoft.com/dashboard) mostra estatísticas sobre como os clientes estão participando com os recursos do Xbox em seu jogo. Ele também fornece informações de integridade do serviço para ajudar a resolver erros de cliente.

> [!IMPORTANT]
> Você só verá esse relatório se estiver publicando um jogo para Xbox ou um jogo que usa os serviços Xbox Live. Para fazer isso, você deve passar pelo [processo de aprovação de conceito](../gaming/concept-approval.md), que inclui jogos publicados por [parceiros da Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) e jogos enviados por meio do [ ID@Xbox programa](../xbox-live/developer-program-overview.md#id). Jogos publicados por meio do [Programa de criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) não são visíveis no momento nesse relatório.

Você pode exibir o relatório de **análise do Xbox** do menu de navegação esquerdo para o seu jogo, expandindo **Analisar** e selecionar a **análise do Xbox**.  Você pode exibir esses dados no Partner Center ou [baixar o relatório](download-analytic-reports.md) para exibição offline.


## <a name="overview-tab"></a>Guia Visão geral

As seções sobre a guia **Visão geral** mostram informações sobre quem os jogadores são e como eles estão interagindo com os recursos do Xbox Live.

Para muitas dessas estatísticas, vamos mostrar também a **Média de Xbox**, para que você possa ver facilmente como os clientes interagem com o Xbox em comparação com o cliente mediano do Xbox.

> [!NOTE]
> Essas estatísticas são de clientes que estão conectados ao Xbox Live, nem todos os clientes do Xbox.


### <a name="concurrent-usage"></a>Uso simultâneo

Esta seção mostra o uso de dados em tempo real aproximado (com latência de 5 a 15 minutos) sobre o número médio de clientes jogando seu jogo a cada minuto ou hora. Você pode escolher o intervalo de tempo (de **Última hora** até **Últimos 7 dias**), selecionando o ícone de filtro no canto superior direito desta seção.


### <a name="gamerscore-distribution"></a>Distribuição de pontuação

Esta seção mostra informações sobre a pontuação de seus clientes. Você pode selecionar **Todos os jogos** para ver a distribuição de pontuação total em todos os seus clientes, ou selecionar **Esse jogo** para exibir a distribuição de pontuação obtida por meio de seu jogo apenas.


### <a name="achievement-unlocks"></a>Conquistas desbloqueadas

Esta seção mostra o número total de clientes que têm desbloqueado cada conquista no intervalo de tempo especificado. Você pode escolher o intervalo de tempo (**Último dia**, **Últimos 30 dias** ou **Eterno**), selecionando o ícone de filtro no canto superior direito desta seção.


### <a name="game-statistics"></a>Estatísticas de jogo

Esta seção inclui as guias onde você pode optar por mostrar dados diferentes para os clientes do seu jogo. Observe que as estatísticas nesta seção referem-se ao uso do recurso em geral e não ao seu produto específico.

- A guia **Uso social** mostra dados relacionados a como os clientes interagem socialmente.
   - **Convites para jogar** mostra o percentual de seus clientes que enviaram convites (para qualquer jogo).
   - **Chat de terceiros** mostra a porcentagem de seus clientes que usa o bate-papo de terceiros (para qualquer jogo).
   - **Mensagens de texto** mostra a porcentagem de seus clientes que envia mensagens por meio do shell do Xbox (para qualquer jogo).
- A guia **Uso de streaming** mostra as porcentagens de clientes do seu jogo que assiste ou transmite o jogo (para qualquer jogo) no Twitch e YouTube.
- A guia **Uso de DVR de jogos** mostra dados relacionados a como seus clientes gravam e leem um jogo. Você pode ver as porcentagens de clientes que visualizaram e carregaram clipes de jogos e capturas de tela do jogo (para qualquer jogo).


### <a name="friends-and-followers"></a>Amigos e seguidores

Esta seção mostra o **Número médio de amigos** e **Número médio de seguidores** dos clientes jogando seu jogo.


### <a name="accessory-usage"></a>Uso de acessórios

Este gráfico mostra as porcentagens de clientes do seu jogo que usa discos rígidos externos e Controles Xbox Elite Wireless (no Xbox).

Esses dados não significam que esses clientes instalaram seu produto em discos rígidos externos ou usaram um controlador de elite enquanto os reproduziram. Ele refere-se a quantos clientes de seu produto usam esse recurso em geral.


### <a name="connection-type"></a>Tipo de conexão

Este gráfico mostra as porcentagens de clientes do produto que usam conexões de internet **Com fio** comparado a **Sem fio** (no Xbox).


## <a name="xbox-live-service-health-tab"></a>Guia Integridade do serviço Xbox Live

As seções na **guia de Integridade do serviço Xbox Live** o ajudam a entender o impacto de quaisquer erros de cliente do Xbox Live, incluindo a limitação de taxa. Também permite fazer busca detalhada pelo código de status e o ponto de extremidade para obter informações que ajudam você a resolver esses problemas e mantém você informado sobre a disponibilidade do serviço Xbox Live específico para chamadas do produto.

> [!NOTE]
> Ao examinar essas informações e lidar com problemas, recomendamos priorizar a limitação de taxa, pois esses erros normalmente têm o maior impacto para o cliente.


### <a name="apply-filters"></a>Aplicar filtros

Na parte superior da guia, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **30D** (30 dias), mas você pode optar por mostrar dados para **7D** (7 dias) ou um intervalo de datas personalizado que você especificar (não superior a 30 dias). Para um intervalo de datas personalizado, observe que todos os gráficos reduzirão o intervalo do gráfico para o primeiro e o último dia de dados fornecido dentro do intervalo que você inserir.

Também é possível expandir **Filtros** para filtrar os dados dessa página por versão do pacote, tipo de dispositivo e/ou por área restrita.
- **Versão do pacote**: o filtro padrão é **Todas as versões**, mas é possível limitar os dados de integridade do serviço para uma versão do pacote específica.
- **Tipo de dispositivo**: A configuração padrão é **Todos os dispositivos**, mas é possível limitar os dados de integridade de serviço para um tipo de dispositivo específico.
- **Área restrita**: A configuração padrão é **COMERCIAL**, mas você pode limitar os dados de integridade do serviço para uma área restrita específica.

As informações de todos os gráficos listados abaixo refletem o intervalo de datas e quaisquer filtros selecionados. Algumas seções também permitem que você aplique mais filtros.


### <a name="client-errors-by-service"></a>Erros do cliente por serviço

O gráfico **Erros de cliente pelo serviço** mostra o número de erros do cliente diários (4xx) entre cada serviço Xbox Live durante o período de tempo selecionado.

Você também pode exibir apenas os erros do limite de taxa selecionando **Limite de taxa**. Isso mostra o número de limitação de taxa diária (429) e taxa de limitação de erros isenta (429E) em cada serviço Xbox Live, durante o período de tempo selecionado.

> [!NOTE]
> Um código de status 429E foi retornado com êxito como um código de status 200, mas teria sido limitado pela taxa se o serviço tivesse um grande volume no momento, portanto, recomendamos que você trate-o exatamente da mesma maneira que foi imposto (429).

Por padrão, este gráfico exibe os principais seis serviços pela contagem de erros. Você pode selecionar o ícone de filtro no canto superior direito desta seção para escolher serviços diferentes. Você pode exibir erros de até seis serviços ao mesmo tempo.

> [!NOTE]
> A legenda exibe somente o prefixo específico de cada serviço (por exemplo, **presence**, em vez de **presence.xboxlive.com**). Você pode encontrar o endereço de serviço completo na tabela inferior **Erros de cliente por ponto de extremidade** na guia **Integridade do serviço Xbox Live**.


### <a name="service-availability"></a>Disponibilidade de serviço

O gráfico **Disponibilidade de serviço** mostra a disponibilidade diária em cada serviço Xbox Live durante o período de tempo selecionado. Isso é calculado como *1-(erros de servidor total (5xx)/respostas totais)* e é específico para seu produto, não para o Xbox Live como um todo.

Por padrão, este gráfico exibe os seis serviços que obtiveram a disponibilidade mais baixa. Você pode selecionar o ícone de filtro no canto superior direito desta seção para escolher serviços diferentes. Você pode exibir disponibilidade para até seis serviços ao mesmo tempo.

> [!NOTE]
> A legenda exibe somente o prefixo específico de cada serviço (por exemplo, **presence**, em vez de **presence.xboxlive.com**). Você pode encontrar o endereço de serviço completo na tabela inferior **Erros de cliente por ponto de extremidade** na guia **Integridade do serviço Xbox Live**.


### <a name="client-errors-by-endpoint"></a>Erros do cliente por ponto de extremidade

A tabela **Erros de cliente por ponto de extremidade** mostra o número de erros do cliente diários (4xx) divididos entre cada serviço Xbox Live, ponto de extremidade e código de status durante o período de tempo selecionado. Por padrão, a tabela é classificada pelo número total de respostas do serviço em ordem decrescente, mas você pode alterar a ordem de classificação clicando em qualquer um dos cabeçalhos das colunas.

Você também pode exibir apenas os erros do limite de taxa selecionando **Limite de taxa**. Isso mostra o número de limitar taxa diária (429) e taxa de limitar erros isento (429E) em cada serviço Xbox Live, ponto de extremidade e código de status durante o período de tempo selecionado.

> [!NOTE]
> Um código de status 429E foi retornado com êxito como um código de status 200, mas teria sido limitado pela taxa se o serviço tivesse um grande volume no momento, portanto, recomendamos que você trate-o exatamente da mesma maneira que foi imposto (429).










 

 

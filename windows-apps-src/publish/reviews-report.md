---
author: jnHs
Description: The Reviews report in the Windows Dev Center dashboard lets you see the reviews (comments) that customers entered when rating your app in the Store.
title: Relatório de opiniões
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.author: wdg-dev-content
ms.date: 08/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, análise, comentário, revisor
ms.localizationpriority: medium
ms.openlocfilehash: 8891aecb904f69e3f77ec5892d9234f79db46ff0
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3408247"
---
# <a name="reviews-report"></a>Relatório de opiniões


O relatório **de avaliações** no painel do Centro de desenvolvimento do Windows permite que você veja as análises (comentários) que os clientes inseriram na classificação do seu aplicativo na loja.

Você pode exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) para visualizar offline. Como alternativa, você pode recuperar programaticamente esses dados usando o método [obter avaliações de aplicativo](../monetize/get-app-reviews.md) na da [API REST de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

Você também pode responder ao cliente análises [diretamente a partir desta página](respond-to-customer-reviews.md), por meio de programação [por meio da API de análises da Microsoft Store](../monetize/submit-responses-to-app-reviews.md), ou usando o [Centro de desenvolvimento do aplicativo](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws).

> [!TIP]
> Para visualizar rapidamente avaliações, classificações e comentários do usuário para todos seus aplicativos nos últimos 30 dias, expanda **Interagir** no menu de navegação esquerdo e selecione **Avaliações e comentários.** 


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar as análises. A seleção padrão é **Tempo de vida**, mas você pode optar por mostrar análises de 30 dias, 3, 6 ou 12 meses, ou para um intervalo de datas personalizado que você especificar.

É possível expandir os **Filtros** para filtrar as análises mostradas nessa página pelas seguintes opções. Esses filtros não serão aplicado aos gráficos de **Análise de classificações** e **Média de classificação ao longo do tempo**.

-   **Classificação**: por padrão, as análises com classificação por estrelas estão marcadas, mas você pode marcar e desmarcar as classificações (de 1 a 5 estrelas) se quiser ver apenas as análises associadas a uma classificação por estrelas específica.
- **Revisar o conteúdo**: A configuração padrão é **classificações com conteúdo de crítica**, o que significa que somente classificações com conteúdo de crítica serão mostradas. Você pode selecionar **todos os** para mostrar todas as classificações, mesmo aqueles que não incluem qualquer texto de crítica escrito. Observe que o gráfico de **análise de classificações** sempre mostrará todas as análises, independentemente de sua seleção.
-   **Versão do sistema operacional**: a configuração padrão é **Todos**. Você poderá escolher a versão específica do sistema operacional se quiser que essa página mostre somente análises deixadas por clientes nessa versão do sistema operacional.
-   **Versão do pacote**: a configuração padrão é **Tudo**. Se o aplicativo incluir mais de um pacote, é possível selecionar um específico aqui para mostrar somente análises deixadas pelos clientes com esse pacote quando analisaram seu aplicativo.
-   **Respostas**: a configuração padrão é **Todos**. Você pode optar por filtrar as análises para mostrar apenas as críticas em que você [respondeu aos clientes](respond-to-customer-reviews.md), ou apenas aquelas em que você ainda não respondeu.
-   **Atualizações**: a configuração padrão é **Todas**. Você pode optar por filtrar as análises para mostrar apenas as análises atualizadas pelo cliente desde que você [respondeu a uma análise](respond-to-customer-reviews.md), ou apenas aquelas que ainda não foram atualizadas pelo cliente.
-   **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um mercado específico, se quiser que esta página mostre somente as críticas de clientes desse mercado.
-   **Tipo do dispositivo**: o filtro padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente análises deixadas por clientes que estão usando esse dispositivo.
-   **Nome da categoria**: o filtro padrão é **Todos**. Você pode escolher um específico de [categoria de informações de análise](#review-insight-categories) para mostrar apenas análises associadas nessa categoria. 

> [!TIP]
> Se você não vir quaisquer análises na página, verifique se os seus filtros não excluíram todas as análises. Por exemplo, se você filtrar por um sistema operacional de destino que não dá suporte ao seu aplicativo, você não verá quaisquer análises


## <a name="ratings-breakdown"></a>Detalhamento das classificações

O gráfico de **análise de classificações** aparece na parte superior desse relatório para que você pode obter uma visão geral a seguir: 
- A classificação média por estrela do aplicativo.
- O total de classificações do aplicativo nos últimos 12 meses.
- O total de classificações de cada classificação por estrelas.
- O número de classificações para cada tipo de classificação (nova ou revisada) por classificação de estrelas nos últimos 12 meses.
 - **Classificações novas** são classificações que os clientes enviaram, mas que não foram alteradas.
 - **Classificações revisadas** são classificações que foram alteradas pelo cliente, até mesmo o texto da opinião.

> [!TIP]
> A classificação média que um cliente visualiza na Loja leva em consideração o mercado e o tipo de dispositivo do cliente, portanto, pode ser diferente do que você vê nesse relatório.

Observe que este gráfico sempre inclui todas as análises, mesmo se você selecionou **que classificações com conteúdo de crítica** no filtro de página **revisar o conteúdo** .

Este gráfico também pode ser visto no [relatório de classificações](ratings-report.md), juntamente com mais detalhes sobre a classificação do seu aplicativo.


< span id = "categorias de informações de revisão / >

## <a name="insight-categories"></a>Categorias de informações

Os grupos de gráfico de **categorias de informações** revisões de acordo com categorias que determinamos podem ser associados com a crítica.

> [!NOTE]
> As análises com menos de 24 horas e/ou em um idioma diferente do inglês não são incluídas ao visualizar análises por categorias.

Na parte superior da página, você verá blocos coloridos representando análises por categoria. Selecione uma destas categorias para ver apenas as análises que associamos a essa categoria. Você também pode usar os [filtros de página](#apply-filters) para filtrar por categoria.

Para ver um detalhamento das avaliações por categoria, selecione **Mostrar detalhes**. 


## <a name="reviews"></a>Avaliações

Cada opinião do cliente contém:

-   O título e o texto da opinião fornecidos pelo cliente. (Críticas escritas por clientes no Windows Phone 8.1 e versões anteriores não terão um título).
-   A data da revisão.
-   O nome do revisor como ele aparece na Microsoft Store.
-   País/região do revisor.
-   A versão do pacote do aplicativo no dispositivo do cliente no momento em que a crítica foi deixada. (Essas informações não estão disponíveis para críticas enviadas online ou por clientes no Windows 8.1 e versões anteriores.)
-   A versão do sistema operacional do dispositivo que o cliente estava usando quando a crítica foi feita.
-   O nome do dispositivo que o cliente estava usando quando a crítica foi feita. (Essas informações não estão disponíveis para críticas enviadas online ou por clientes no Windows 8.1 e versões anteriores.)
-   A "contagem de utilidade" da crítica, conforme classificada por outros clientes ao ler a crítica. Esses itens são mostrados como uma série de dois números: o primeiro número mostra quantos clientes a classificaram como útil, e o segundo número é o número total de clientes que classificaram a crítica. Por exemplo, uma contagem de utilidade de 4/10 significa que, dos 10 classificadores, 4 consideraram a opinião útil e seis, não. (Se não houver votos de utilidade de uma crítica, nenhuma contagem de utilidade será exibida).

Observe que os clientes podem deixar uma classificação para o seu aplicativo sem adicionar qualquer comentário, para que você veja normalmente menos análises do que classificações.

Você pode classificar as criticas na página por data e/ou por classificação, em ordem crescente ou decrescente. Clique no link **Classificar por** para exibir opções de classificação por **Data** e/ou **classificação**.

Você também pode usar a caixa de pesquisa para procurar palavras ou frases em críticas do seu aplicativo específicas. Observe que apenas o texto revisão original escrito pelo cliente é pesquisado, mesmo se a crítica foi escrita em um idioma diferente. Texto de crítica traduzido não é pesquisado.

> [!NOTE]
> Às vezes, você pode observar que as avaliações desaparecem desse relatório. Isso pode acontecer porque a Microsoft remove revisões da Loja escritas por clientes que executam determinadas compilações do Insider e pré-lançamento do Windows 10. Fazemos isso para reduzir a possibilidade de uma análise negativa causada por um problema em uma compilação de versão de pré-lançamento do Windows. Também podemos remover avaliações da Loja que foram identificadas como spam, inadequadas, ofensivas ou que tenham outras violações de política. Esperamos que essa ação resulte em uma melhor experiência para o cliente.


## <a name="translating-reviews"></a>Traduzindo análises

Por padrão, críticas que não foram escritas em seu idioma preferencial são traduzidas para você. Se você preferir, a tradução das críticas pode ser desabilitada, desmarcando-se a caixa de seleção **Traduzir análises** na parte superior direita, acima da lista de críticas.

Observe que críticas são traduzidas por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido caso deseje compará-lo à tradução ou traduzi-lo por meio de algum outro meio.

Como observado acima, quando pesquisar as críticas, somente o texto original deixado pelo cliente é pesquisado (e não qualquer texto traduzido), mesmo se você tiver a caixa **Traduzir análises** marcada.


## <a name="responding-to-customer-reviews"></a>Respondendo às análises dos clientes

Você pode usar o painel da Microsoft Store Dev Center, a [API de análises da Microsoft Store](../monetize/submit-responses-to-app-reviews.md)ou o [aplicativo do Centro de desenvolvimento](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para enviar respostas a muitas das críticas dos clientes. Para obter mais informações, consulte [Responder às análises dos clientes](respond-to-customer-reviews.md)

Veja algumas ações adicionais que você pode realizar com base nas classificações e críticas observadas.

-   Caso veja muitas críticas sugerindo um recurso novo ou alterado ou com reclamações sobre um problema, pense em lançar uma nova versão que atenda especificamente a esses comentários (Não se esqueça de atualizar a [descrição](create-app-descriptions.md) do aplicativo para indicar que o problema foi corrigido).
-   Caso a classificação média seja alta, mas o número de transferências seja baixo, convém procurar formas de [expor seu aplicativo a mais pessoas](attract-customers-and-promote-your-apps.md), já que ele foi é bem-recebido pelos usuários que o testaram.


 

 

 

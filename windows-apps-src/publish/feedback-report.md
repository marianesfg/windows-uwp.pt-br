---
Description: O relatório de comentários no Partner Center permite que você veja os problemas, as sugestões e os votos que seus clientes do Windows 10 enviaram por meio do hub de comentários.
title: Relatório de comentários
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47eb494ac1b61caac0549f89254ae5d60a7ddf4c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259014"
---
# <a name="feedback-report"></a>Relatório de comentários

O **relatório de comentários** no Partner Center permite que você veja os problemas, as sugestões e os votos que seus clientes do Windows 10 enviaram por meio do hub de comentários. Você pode exibir esses dados no Partner Center ou exportar os dados para exibição offline.

> [!NOTE]
> Você também pode [responder aos comentários](respond-to-customer-feedback.md) diretamente desse relatório para mostrar aos clientes que a opinião deles é ouvida.

Incentivar os clientes a fornecerem comentários sobre seu aplicativo é uma ótima maneira de aprender sobre os problemas e recursos que são mais importantes para eles. Quando seus clientes sabem que podem enviar comentários diretamente, eles têm menos probabilidade de deixar esses comentários como uma análise negativa na Loja.

Você pode usar a API de comentários no [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) para permitir que os clientes [iniciem diretamente o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md). Lembre-se de que qualquer cliente que tenha baixado seu aplicativo em um dispositivo Windows 10 que dê suporte a comentários Hub tem a capacidade de deixar comentários sobre ele usando o aplicativo de Hub de Feedback. Por isso, você poderá ver os comentários dos clientes nesse relatório, mesmo se não tiver solicitado especificamente comentários de dentro de seu aplicativo.

Os comentários também podem ser úteis ao usar o [pacote de vôo](package-flights.md), já que o relatório de **comentários** mostra o pacote específico que cada cliente tinha instalado em seu dispositivo quando eles saíram dos comentários.

> [!TIP]
> Para obter uma visão rápida das revisões, das classificações e dos comentários dos usuários em todos os seus aplicativos nos últimos 30 dias **, expanda o menu de navegação** à esquerda e selecione **revisões e comentários.** 


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **Ciclo de vida**, mas você pode optar por mostrar dados de 30 dias, 3, 6 ou 12 meses.

Também é possível expandir os **Filtros** para filtrar todos os dados dessa página pelas seguintes opções.

- **Tipo de comentários**: a configuração padrão é **Todos**. Você pode selecionar **Problema** ou **Sugestão** para mostrar apenas esse tipo de comentários.
- **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente comentários deixados por clientes que estão usando esse dispositivo.
- **Versão do pacote**: a configuração padrão é **Todos os pacotes**. Você pode selecionar um de seus pacotes para mostrar apenas os comentários deixados por clientes que estavam usando esse pacote específico quando deixaram comentários.
- **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um específico para mostrar apenas os comentários dos clientes desse mercado.
- **Grupo**: a configuração padrão é **Todos**. Você pode optar por exibir apenas os comentários enviados por [Usuários do Windows Insider](https://insider.windows.com).

> [!TIP]
> Se você não exibir nenhum comentário na página, verifique se os filtros não excluíram todos os comentários. Por exemplo, se você filtrar por um **Tipo de dispositivo** não compatível com seu aplicativo, você não verá nenhum comentário


## <a name="viewing-feedback-details"></a>Exibir detalhes de comentários

Nesse relatório, você encontrará os comentários individuais deixados por seus clientes. À esquerda do texto dos comentários de cada item, você verá o número de vezes que os comentários tiveram votos a favor de outros clientes no Hub de Feedback. Você pode classificar os comentários de três maneiras:

- **Votos a favor** (padrão): mostra os comentários que tiveram votos a favor de outros clientes, começando com os comentários com mais votos a favor.
- **Mais populares**: mostra os comentários que tiveram votos a favor por outros clientes nos últimos sete dias, começando com os comentários que obtiveram a atividade mais recente.
- **Mais recentes**: mostra todos os comentários, começando com os comentários deixados mais recentemente.

Ao lado de cada comentário, você verá a data em que os comentários foram deixados e o tipo de comentário. Você também verá o mercado do cliente, o pacote específico que foi instalado no dispositivo que eles estavam usando quando saíram dos comentários, o tipo desse dispositivo e o **Windows Insider** se o cliente que envia os comentários for membro do programa Windows Insider.

Você também verá uma opção para [responder aos comentários](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Tradução de comentários

Por padrão, os comentários que não foram gravados em seu idioma preferido são traduzidos para você. Se você preferir, a tradução dos comentários poderá ser desabilitada ao desmarcar a caixa de seleção **Traduzir comentários** na parte superior direita, ao lado dos filtros de página.

Observe que os comentários são traduzidos por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido, se você quiser compará-lo com a tradução, ou traduzi-lo por meio algum outro meio.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciando o Hub de Feedback diretamente em seu aplicativo

Como observado acima, recomendamos a incorporação de um link para o Hub de Feedback diretamente em seu aplicativo para incentivar os clientes a fornecer comentários. Para obter mais informações, consulte [Iniciar o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md).

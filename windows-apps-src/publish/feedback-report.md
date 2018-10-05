---
author: jnHs
Description: The Feedback report in the Windows Dev Center dashboard lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: Relatório de comentários
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 11/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bceb1d2cc6682698d0ad06ed4b1865f3d6510442
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390894"
---
# <a name="feedback-report"></a>Relatório de comentários

O **relatório de comentários** no painel do Centro de Desenvolvimento do Windows permite que você veja problemas, sugestões e aprovações que os clientes do Windows 10 enviaram através do Hub de Feedback. Você pode exibir esses dados em seu painel ou exportar os dados para exibição offline.

> [!NOTE]
> Você também pode [responder aos comentários](respond-to-customer-feedback.md) diretamente desse relatório para mostrar aos clientes que a opinião deles é ouvida.

Incentivar os clientes a fornecerem comentários sobre seu aplicativo é uma ótima maneira de aprender sobre os problemas e recursos que são mais importantes para eles. Quando seus clientes sabem que podem enviar comentários diretamente, eles têm menos probabilidade de deixar esses comentários como uma análise negativa na Loja.

Você pode usar a API de comentários no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) para permitir que os clientes [iniciem diretamente o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md). Lembre-se de que qualquer cliente que tenha baixado seu aplicativo em um dispositivo Windows 10 que dê suporte a comentários Hub tem a capacidade de deixar comentários sobre ele usando o aplicativo de Hub de Feedback. Por isso, você poderá ver comentários de clientes nesse relatório mesmo que não tenha especificamente solicitado comentários dentro de seu aplicativo.

O feedback também pode ser importante ao usar o [pacote de liberação de versões de pré-lançamento](package-flights.md), pois o relatório de comentários mostra o pacote específico que cada cliente tinha instalado em seu dispositivo ao deixar os comentários.

> [!TIP]
> Para visualizar rapidamente avaliações, classificações e comentários do usuário em todos os seus aplicativos nos últimos 30 dias, expanda **envolver** no menu de navegação esquerdo e selecione **avaliações e comentários.** 


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **Ciclo de vida**, mas você pode optar por mostrar dados de 30 dias, 3, 6 ou 12 meses.

Também é possível expandir os **Filtros** para filtrar todos os dados dessa página pelas seguintes opções.

- **Tipo de comentários**: a configuração padrão é **Todos**. Você pode selecionar **Problema** ou **Sugestão** para mostrar apenas esse tipo de comentários.
- **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente comentários deixados por clientes que estão usando esse dispositivo.
- **Versão do pacote**: a configuração padrão é **Todos os pacotes**. Você pode selecionar um de seus pacotes para mostrar apenas os comentários deixados por clientes que estavam usando esse pacote específico quando deixaram comentários.
- **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um específico para mostrar apenas os comentários dos clientes desse mercado.
- **Grupo**: a configuração padrão é **Todos**. Você pode optar por exibir apenas os comentários enviados por [Usuários do Windows Insider](http://insider.windows.com).

> [!TIP]
> Se você não exibir nenhum comentário na página, verifique se os filtros não excluíram todos os comentários. Por exemplo, se você filtrar por um **Tipo de dispositivo** não compatível com seu aplicativo, você não verá nenhum comentário


## <a name="viewing-feedback-details"></a>Exibir detalhes de comentários

Nesse relatório, você encontrará os comentários individuais deixados por seus clientes. À esquerda do texto dos comentários de cada item, você verá o número de vezes que os comentários tiveram votos a favor de outros clientes no Hub de Feedback. Você pode classificar os comentários de três maneiras:

- **Votos a favor** (padrão): mostra os comentários que tiveram votos a favor de outros clientes, começando com os comentários com mais votos a favor.
- **Mais populares**: mostra os comentários que tiveram votos a favor por outros clientes nos últimos sete dias, começando com os comentários que obtiveram a atividade mais recente.
- **Mais recentes**: mostra todos os comentários, começando com os comentários deixados mais recentemente.

Ao lado de cada comentário, você verá a data em que os comentários foram deixados e o tipo de comentário. Você também verá o mercado do cliente, o pacote específico que foi instalado no dispositivo que foram usado quando deixaram os comentários, o tipo do dispositivo e **Do Windows Insider** se o cliente que enviou os comentários for um membro do Windows Insider programa.

Você também verá uma opção para [responder aos comentários](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Tradução de comentários

Por padrão, os comentários que não foram escritos em seu idioma preferencial são traduzidos para você. Se você preferir, a tradução dos comentários poderá ser desabilitada ao desmarcar a caixa de seleção **Traduzir comentários** na parte superior direita, ao lado dos filtros de página.

Observe que os comentários são traduzidos por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido, se você quiser compará-lo com a tradução, ou traduzi-lo por meio de algum outro meio.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciando o Hub de Feedback diretamente em seu aplicativo

Como observado acima, recomendamos a incorporação de um link para o Hub de Feedback diretamente em seu aplicativo para incentivar os clientes a fornecer comentários. Para obter mais informações, consulte [Iniciar o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md).

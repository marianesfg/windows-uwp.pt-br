---
Description: O relatório de comentários no Partner Center permite ver os problemas, sugestões e votos a favor que seus clientes Windows 10 enviados por meio do Hub de comentários.
title: Relatório de comentários
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ab7385d4c61c52b71c74fb61797be306bcc9851
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591821"
---
# <a name="feedback-report"></a>Relatório de comentários

O **relatório de comentários** no permite que o Partner Center você ver os problemas, sugestões e votos a favor que seus clientes Windows 10 enviados por meio do Hub de comentários. Você pode exibir esses dados no Partner Center ou exportar dados para exibição offline.

> [!NOTE]
> Você também pode [responder aos comentários](respond-to-customer-feedback.md) diretamente desse relatório para mostrar aos clientes que a opinião deles é ouvida.

Incentivar os clientes a fornecerem comentários sobre seu aplicativo é uma ótima maneira de aprender sobre os problemas e recursos que são mais importantes para eles. Quando seus clientes sabem que podem enviar comentários diretamente, eles têm menos probabilidade de deixar esses comentários como uma análise negativa na Loja.

Você pode usar a API de comentários no [Microsoft Store Services SDK](https://aka.ms/store-em-sdk) para permitir que os clientes [iniciem diretamente o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md). Lembre-se de que qualquer cliente que tenha baixado seu aplicativo em um dispositivo Windows 10 que dê suporte a comentários Hub tem a capacidade de deixar comentários sobre ele usando o aplicativo de Hub de Feedback. Por isso, você poderá ver os comentários do cliente neste relatório, mesmo se você não especificamente solicitado para comentários de dentro de seu aplicativo.

Comentários também podem ser útil ao usar [pacote flighting](package-flights.md), uma vez que o **comentários** relatório mostra o pacote específico que cada cliente tinha instalado em seu dispositivo quando eles deixado os comentários.

> [!TIP]
> Para obter uma visão rápida de revisões, classificações e comentários do usuário em todos os seus aplicativos nos últimos 30 dias, expanda **acionar** no menu de navegação à esquerda e selecione **revisões e comentários.** 


## <a name="apply-filters"></a>Aplicar filtros

Na parte superior da página, você pode selecionar o período para o qual você deseja mostrar os dados. A seleção padrão é **Ciclo de vida**, mas você pode optar por mostrar dados de 30 dias, 3, 6 ou 12 meses.

Também é possível expandir os **Filtros** para filtrar todos os dados dessa página pelas seguintes opções.

- **Tipo de comentário**: A configuração padrão é **todos os**. Você pode selecionar **Problema** ou **Sugestão** para mostrar apenas esse tipo de comentários.
- **Tipo de dispositivo**: A configuração padrão é **todos os dispositivos**. Você poderá escolher um tipo de dispositivo específico, se quiser que essa página mostre somente comentários deixados por clientes que estão usando esse dispositivo.
- **Versão do pacote**: A configuração padrão é **todos os pacotes**. Você pode selecionar um de seus pacotes para mostrar apenas os comentários deixados por clientes que estavam usando esse pacote específico quando deixaram comentários.
- **Mercado**: A configuração padrão é **todos os mercados**. Você pode escolher um específico para mostrar apenas os comentários dos clientes desse mercado.
- **Grupo**: A configuração padrão é **todos os**. Você pode optar por exibir apenas os comentários enviados por [Usuários do Windows Insider](https://insider.windows.com).

> [!TIP]
> Se você não exibir nenhum comentário na página, verifique se os filtros não excluíram todos os comentários. Por exemplo, se você filtrar por um **Tipo de dispositivo** não compatível com seu aplicativo, você não verá nenhum comentário


## <a name="viewing-feedback-details"></a>Exibir detalhes de comentários

Nesse relatório, você encontrará os comentários individuais deixados por seus clientes. À esquerda do texto dos comentários de cada item, você verá o número de vezes que os comentários tiveram votos a favor de outros clientes no Hub de Feedback. Você pode classificar os comentários de três maneiras:

- **Upvoted** (padrão): Mostra os comentários que tem sido upvoted por outros clientes, começando com os comentários que recebeu a maioria dos votos a favor.
- **Tendências**: Mostra os comentários que tem sido upvoted por outros clientes nos últimos sete dias, começando com os comentários que passou a atividade mais recente.
- **Mais recente**: Mostra todos os comentários, começando com os comentários deixados mais recentemente.

Ao lado de cada comentário, você verá a data em que os comentários foram deixados e o tipo de comentário. Você também verá mercado do cliente, o pacote específico que foi instalado no dispositivo que eles estavam usando quando eles deixado os comentários, o tipo de dispositivo, e **Windows Insider** se o cliente enviar comentários é um membro de o programa Windows Insider.

Você também verá uma opção para [responder aos comentários](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Traduzindo comentários

Por padrão, os comentários que não foram gravados em sua linguagem preferida é convertido para você. Se você preferir, a tradução dos comentários poderá ser desabilitada ao desmarcar a caixa de seleção **Traduzir comentários** na parte superior direita, ao lado dos filtros de página.

Observe que os comentários são traduzidos por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido, se você quiser compará-lo com a tradução, ou traduzi-lo por meio algum outro meio.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciando o Hub de Feedback diretamente em seu aplicativo

Como observado acima, recomendamos a incorporação de um link para o Hub de Feedback diretamente em seu aplicativo para incentivar os clientes a fornecer comentários. Para obter mais informações, consulte [Iniciar o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md).

---
author: andrewleader
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: Blocos secundários
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, blocos secundários, orientação, diretrizes, práticas recomendadas
ms.localizationpriority: medium
ms.openlocfilehash: 1e3d31376b9ac155dab6bffa7739cb880af1cff9
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4113244"
---
# <a name="secondary-tile-guidance"></a>Diretriz de bloco secundário


Um bloco secundário fornece uma maneira consistente e eficiente para os usuários acessarem diretamente áreas específicas dentro de um aplicativo a partir do menu Iniciar. Embora um usuário escolha ou não "fixar" um bloco secundário no menu Iniciar, as áreas fixáveis em um aplicativo são determinadas pelo desenvolvedor. Para obter um resumo mais detalhado, consulte [Visão geral de blocos secundários](secondary-tiles.md). Considere estas diretrizes quando você habilitar blocos secundários e projetar a interface do usuário associada em seu aplicativo.

> [!NOTE]
> Apenas usuários podem fixar um bloco secundário no menu Iniciar; os aplicativos não podem fixar blocos secundários de forma programada. Os usuários também controlam a remoção do bloco e podem remover um bloco secundário do menu Iniciar ou de dentro do aplicativo pai.


## <a name="recommendations"></a>Recomendações

Ao habilitar blocos secundários em seu aplicativo, considere as seguintes recomendações:

* Quando o conteúdo em foco for fixável, a barra de aplicativos deve incluir um botão "Fixar em Iniciar" para criar um bloco secundário para o usuário.
* Quando o usuário clica em "Fixar em Iniciar", é necessário chamar imediatamente a API do thread de interface do usuário para [fixar o bloco secundário](secondary-tiles-pinning.md).
* Se o conteúdo em foco já estiver fixado, substitua o botão "Fixar em Iniciar" na barra de aplicativo por um botão "Desafixar de Iniciar". O botão "Desafixar de Iniciar" deve remover o bloco secundário existente.
* Quando o conteúdo em foco não for fixável, não exiba um botão "Fixar em Iniciar" (ou mostre um botão "Fixar em Iniciar" desativado).
* Use os glifos fornecidos pelo sistema para os botões "Fixar em Iniciar" e "Desafixar de Iniciar" (consulte os membros de fixação e desafixação em Windows.UI.Xaml.Controls.Symbol ou WinJS.UI.AppBarIcon).
* Use o texto do botão padrão: "Fixar em Iniciar" e "Desafixar de Iniciar". Você terá que substituir o texto padrão ao usar os glifos fixar e desafixar fornecidos pelo sistema.
* Não use um bloco secundário como um botão de comando virtual para interagir com o aplicativo pai, como um bloco "saltar para a próxima faixa".


## <a name="additional-usage-guidance-for-devs"></a>Diretrizes de uso adicional para desenvolvedores

* Quando um aplicativo é iniciado, ele deve sempre enumerar seus blocos secundários, caso haja adições ou exclusões das quais ele não estava sabendo. Quando um bloco secundário é excluído através da barra de aplicativos da tela inicial, o Windows simplesmente remove o bloco. O próprio aplicativo é responsável por liberar qualquer recurso que tenha sido usado pelo bloco secundário. Quando os blocos secundários são copiados pela nuvem, as notificações de bloco ou selo atuais no bloco secundário, as notificações agendadas, os canais de notificação por push e os URIs usados com as notificações periódicas não são copiados com os blocos secundários e devem ser reconfigurados.
* Um aplicativo deve usar IDs exclusivas recriáveis e significativas para blocos secundários. Usar IDs de bloco secundário previsíveis que são significativas para um aplicativo ajuda o aplicativo a entender o que fazer com esses blocos quando eles são vistos em uma nova instalação em um novo computador.
  * No tempo de execução, o aplicativo pode consultar se um bloco específico existe.
  * A plataforma de bloco secundário pode ser solicitada a retornar o conjunto de todos os blocos secundários pertencentes a um aplicativo específico. O uso de IDs exclusivas significativas para esses blocos ajuda o aplicativo a examinar o conjunto de blocos secundários e executar as ações apropriadas. Por exemplo, para um aplicativo de mídia social, as IDs poderiam identificar os contatos individuais para os quais os blocos foram criados.
* Os blocos secundários, como todos os blocos na tela inicial, são saídas dinâmicas que podem ser frequentemente atualizadas com novo conteúdo. Os blocos secundários podem exibir notificações e atualizações usando os mesmos mecanismos que qualquer outro bloco. Consulte [Escolher um método de entrega de notificação](choosing-a-notification-delivery-method.md) para saber mais.


## <a name="related"></a>Relacionado

* [Visão geral de blocos secundários](secondary-tiles.md)
* [Fixar blocos secundários](secondary-tiles-pinning.md)
* [Ativos de bloco](app-assets.md)
* [Documentação sobre conteúdo de blocos](create-adaptive-tiles.md)
* [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)

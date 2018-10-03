---
author: andrewleader
Description: Secondary tiles allow users to pin specific content and deep links from your app onto their Start menu, providing easy future access to the content within your app.
title: Blocos secundários
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, blocos secundários
ms.localizationpriority: medium
ms.openlocfilehash: 7f11ca4d29f22daf953ce03436c3b786c70a9e04
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4264341"
---
# <a name="secondary-tiles"></a>Blocos secundários


Blocos secundários permitem que os usuários fixem conteúdo específico e links profundos do seu app no menu Iniciar, fornecendo acesso futuro fácil ao conteúdo em seu app.

![Captura de tela de blocos secundários](images/secondarytiles.png)

Por exemplo, os usuários podem fixar a previsão do tempo para vários locais específicos no menu Iniciar, que fornece (1) informações rápidas e fáceis em tempo real sobre o clima graças aos blocos dinâmicos e (2) um ponto de entrada rápido do clima da cidade específica de interesse. Os usuários também podem fixar ações específicas, artigos de notícias e mais itens importantes para eles.

Ao adicionar blocos secundários ao seu app, você ajuda o usuário a se envolver novamente com seu app de maneira rápida e eficiente, incentivando-o a retornar com mais frequência graças ao fácil acesso que os blocos secundários fornecem.

**Apenas usuários podem fixar um bloco secundário; os apps não podem fixar blocos secundários de forma programada sem a aprovação do usuário**. O usuário deve clicar explicitamente em um botão "Fixar" em seu app. Nesse caso, você usa a API para solicitar a criação de um bloco secundário e, em seguida, o sistema exibe uma caixa de diálogo solicitando que o usuário confirme se ele gostaria de fixar o bloco.

## <a name="quick-links"></a>Links rápidos

| Artigo | Descrição |
| --- | --- |
| [Diretrizes sobre os blocos secundários](secondary-tiles-guidance.md) | Saiba quando e onde você deve usar blocos secundários. |
| [Fixar blocos secundários](secondary-tiles-pinning.md) | Saiba como fixar um bloco secundário. |
| [Fixar a partir do aplicativo da área de trabalho](secondary-tiles-desktop-pinning.md) | Os aplicativos da área de trabalho do Windows podem fixar blocos secundários graças à Ponte de Desktop! |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Blocos secundários em relação aos blocos primários

Blocos secundários são associados a um único aplicativo pai. Eles são fixados no menu Iniciar para oferecer ao usuário uma forma consistente e eficiente de abrir diretamente uma área frequentemente usada do aplicativo pai. Isso pode ser uma subseção geral do aplicativo pai que tem conteúdo frequentemente atualizado ou um link profundo com alguma área específica do aplicativo.

Exemplos de cenários de blocos secundários incluem:

* Atualizações de clima para uma cidade específica em um aplicativo de clima
* Um resumo dos próximos eventos em um aplicativo de calendário
* Status e atualizações de um contato importante em um aplicativo de redes sociais
* Feeds específicos em um leitor de RSS
* Uma playlist de música
* Um blog

Qualquer conteúdo que mude com frequência e que o usuário queira monitorar é um bom candidato a bloco secundário. Após a fixação do bloco secundário, os usuários podem receber atualizações pelo bloco e usá-lo para lançar diretamente o aplicativo pai.

Os blocos secundários são semelhantes aos blocos primários de várias maneiras:

* Eles usam as notificações de blocos para exibir conteúdo útil.
* Eles devem incluir um logotipo de 150 x 150 pixels para o conteúdo do bloco padrão.
* Opcionalmente, eles podem incluir outros tamanhos de logotipo para permitir tamanhos de blocos maiores.
* Eles podem mostrar notificações e selos.
* Podem ser reorganizados no menu Iniciar.
* Eles são excluídos automaticamente quando o aplicativo é desinstalado.
* O selo e o texto de status detalhado do bloqueio podem ser mostrados na tela de bloqueio.

No entanto, os blocos secundários são diferentes dos blocos primários de algumas maneiras perceptíveis:

* Os usuários podem excluir os blocos secundários a qualquer momento sem excluir o aplicativo pai.
* Os blocos secundários podem ser criados no tempo de execução. Os blocos de aplicativos só podem ser criados durante a instalação.
* Um submenu solicita a confirmação do usuário antes de adicionar um bloco secundário.
* Eles não podem ser selecionados de forma programada para a tela de bloqueio por meio de uma solicitação ao usuário. O usuário deve adicionar manualmente o bloco secundário por meio da página Personalizar em Configurações do PC.

Para enviar notificações, métodos específicos são fornecidos para atualizadores de blocos e notificações e canais de notificação por push são usados com blocos secundários. Eles fazem paralelo com as versões usadas com blocos primários. Por exemplo, CreateBadgeUpdaterForApplication x CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Diretrizes sobre os blocos secundários
Para saber mais sobre quando e onde você deve usar blocos secundários e outras diretrizes de uso, consulte [Diretrizes sobre blocos secundários](secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>Fixando blocos secundários
Para saber como fixar blocos secundários, consulte [Fixar blocos secundários](secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Aplicativos da área de trabalho e blocos secundários
Para saber como usar blocos secundários de seu aplicativo da área de trabalho pela Ponte de Desktop, consulte [Fixar blocos secundários do aplicativo da área de trabalho](secondary-tiles-desktop-pinning.md).

---
Description: Saiba como usar blocos, selos, notificações do sistema e notificações para fornecer pontos de entrada em seu aplicativo e manter os usuários atualizados.
title: Blocos, selos e notificações
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb65acf606ffa44f075016720ebcd055ba5febc8
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234462"
---
# <a name="tiles-badges-and-notifications-for-windows-apps"></a>Blocos, selos e notificações para aplicativos do Windows
 

Saiba como usar blocos, selos, notificações do sistema e notificações para fornecer pontos de entrada em seu aplicativo e manter os usuários atualizados.

> **APIs importantes**: [Pacote do nuget de notificações de kit de ferramentas de comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Bloco é a representação de um aplicativo no menu Iniciar. Cada aplicativo do Windows tem um bloco. Você pode habilitar diferentes tamanhos de bloco (pequeno, médio, largo e grande).</p>

<p>Você pode usar uma <em>notificação de bloco</em> para atualizar o bloco regularmente para comunicar novas informações ao usuário, como manchetes, ou o assunto da mensagem mais recente não lida.</p>

<p>Você pode usar um <em>selo</em> para fornecer informações de status ou resumo na forma de um glifo fornecido pelo sistema ou de um número de 1 a 99. Selos também são exibidos no ícone da barra de tarefas para um aplicativo. </p>

<p>Uma <em>notificação do sistema</em> é uma notificação que seu aplicativo envia para o usuário por meio de um elemento de interface do usuário pop-up chamado <em>notificação do sistema</em> (ou <em>banner</em>). A notificação poderá ser vista se o usuário estiver em seu aplicativo ou não.</p>
<p>Uma <em>notificação por push</em> ou <em>notificação de dados brutos</em> é uma notificação enviada para seu aplicativo pelo Serviço de Notificação por Push do Windows (WNS) ou por uma tarefa em segundo plano. Seu aplicativo pode responder a essas notificações avisando o usuário que algo de interesse aconteceu (por meio de atualização de notificação, atualização de bloco ou notificação do sistema) ou de qualquer forma a sua escolha.</p>

 
## <a name="tiles"></a>Tiles
| Artigo | Descrição |
| --- | --- |
| [Criar blocos](creating-tiles.md) | Personalize o bloco padrão para seu aplicativo e forneça ativos para diferentes tamanhos de tela. |
| [Ativos de ícone de aplicativo](app-assets.md) | Os ativos de ícone de aplicativo, que são exibidos em várias formas em todo o sistema operacional Windows 10, são os cartões de chamada do seu aplicativo do Windows. Estas diretrizes detalham onde os ativos de ícone de aplicativo são exibidos no sistema e fornecem dicas de design aprofundadas sobre como criar os ícones mais elaborados. |
| [APIs de bloco primário](primary-tile-apis.md) | Solicite fixar o bloco principal do aplicativo e verifique se o bloco primário está atualmente fixado. |
| [Conteúdo do bloco](create-adaptive-tiles.md) | O conteúdo da notificação de bloco é especificado utilizando um novo recurso adaptável no Windows 10, permitindo que você elabore seu próprio conteúdo de notificação de bloco usando uma linguagem de marcação simples e flexível que se adapte a densidades de tela diferentes. Este artigo explica como criar blocos dinâmicos adaptáveis para o seu aplicativo do Windows. |
| [Esquema de conteúdo do bloco](../tiles-and-notifications/tile-schema.md) | Veja os elementos e os atributos que você usa para criar blocos adaptáveis. |
| [Modelos de blocos especiais](special-tile-templates-catalog.md) | Modelos de blocos especiais são modelos exclusivos que são animados ou apenas permitem fazer coisas que não são possíveis com blocos adaptáveis. |
| [Enviar notificação de bloco local](sending-a-local-tile-notification.md) | Saiba como enviar uma notificação de bloco local, adicionando conteúdo dinâmico avançado ao Bloco Dinâmico. |


## <a name="notifications"></a>Notificações

| Artigo | Descrição |
| --- | --- |
| [Notificações do sistema](adaptive-interactive-toasts.md) | As notificações do sistema interativas e adaptáveis permitem criar notificações pop-up flexíveis com mais conteúdo, imagens embutidas opcionais e interação do usuário opcional. |
| [Enviar uma notificação do sistema local](send-local-toast.md) | Saiba como enviar uma notificação do sistema interativa. |
| [Visualizador de notificações](notifications-visualizer.md) | O Visualizador de Notificações é um novo aplicativo do Windows [na Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ajuda os desenvolvedores com o design de blocos dinâmicos adaptáveis para o Windows 10. |
| [Escolher um método de entrega de notificação](choosing-a-notification-delivery-method.md) | Este artigo aborda as quatro opções de notificação - local, agendada, periódica e por push - que fornecem atualizações de blocos e notificação e conteúdo de notificações do sistema. |
| [Visão geral de notificações periódicas](periodic-notification-overview.md) | Notificações periódicas, que são chamadas também de notificações de sondagem, atualizam blocos e selos em um intervalo fixo, baixando o conteúdo da nuvem. |
| [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md) | Os Serviços de Notificação por Push do Windows (WNS) permitem que desenvolvedores terceirizados enviem atualizações de notificações do sistema, de blocos, de selos e brutas pelo próprio serviço de nuvem. Isso proporciona um mecanismo para entregar novas atualizações aos usuários de forma eficaz e confiável. |
| [Código gerado pelo assistente de notificação por push](the-code-generated-by-the-push-notification-wizard.md) | Ao usar um assistente no Visual Studio, você pode gerar notificações por push a partir de um serviço móvel que foi criado com os Serviços Móveis do Azure. O assistente do Visual Studio gera código para ajudá-lo a começar. Este tópico explica como o assistente modifica seu projeto, o que o código gerado faz, como usá-lo e o que pode ser feito em seguida para aproveitar ao máximo as notificações por push. Veja [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](windows-push-notification-services--wns--overview.md). |
| [Visão geral de notificação de dados brutos](raw-notification-overview.md) | As notificações brutas são notificações por push curtas com finalidade geral. Elas são estritamente para instrução e não incluem componente de interface do usuário. Assim como nas outras notificações por push, o recurso WNS envia notificações brutas de seu serviço em nuvem para o aplicativo. |

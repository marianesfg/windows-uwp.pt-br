---
author: mijacobs
Description: "Saiba como usar blocos, selos, notificações do sistema e notificações para fornecer pontos de entrada em seu aplicativo e manter os usuários atualizados."
title: "Blocos, selos e notificações"
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 4a9a1b18984ed418fc31061ff2ee392230117609
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Blocos, selos e notificações para aplicativos UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Saiba como usar blocos, selos, notificações do sistema e notificações para fornecer pontos de entrada em seu aplicativo e manter os usuários atualizados.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Bloco é a representação de um aplicativo no menu Iniciar. Todo aplicativo UWP tem um bloco. Você pode habilitar diferentes tamanhos de bloco (pequeno, médio, largo e grande).</p>

<p>Você pode usar uma <em>notificação de bloco</em> para atualizar o bloco regularmente para comunicar novas informações ao usuário, como manchetes, ou o assunto da mensagem mais recente não lida.</p>

<p>Você pode usar um <em>selo</em> para fornecer informações de status ou resumo na forma de um glifo fornecido pelo sistema ou de um número de 1 a 99. Selos também são exibidos no ícone da barra de tarefas para um aplicativo. </p>

<p>Uma <em>notificação do sistema</em> é uma notificação que seu aplicativo envia para o usuário por meio de um elemento de interface do usuário pop-up chamado <em>notificação do sistema</em> (ou <em>banner</em>). A notificação poderá ser vista se o usuário estiver em seu aplicativo ou não.</p>
<p>Uma <em>notificação por push</em> ou <em>notificação de dados brutos</em> é uma notificação enviada para seu aplicativo pelo Serviço de Notificação por Push do Windows (WNS) ou por uma tarefa em segundo plano. Seu aplicativo pode responder a essas notificações avisando o usuário que algo de interesse aconteceu (por meio de atualização de notificação, atualização de bloco ou notificação do sistema) ou de qualquer forma a sua escolha.</p>

 
## <a name="tiles"></a>Blocos 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Criar blocos](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>Personalize o bloco padrão para seu aplicativo e forneça ativos para diferentes tamanhos de tela.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Criar blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>Modelos de blocos adaptáveis são um novo recurso no Windows 10, permitindo que você elabore seu próprio conteúdo de notificação de bloco usando uma linguagem de marcação simples e flexível que se adapte a densidades de tela diferentes. Este artigo explica como criar blocos dinâmicos adaptáveis para seu aplicativo da Plataforma Universal do Windows (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Esquema de blocos adaptáveis](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>Veja os elementos e os atributos que você usa para criar blocos adaptáveis.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modelos de blocos especiais](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>Modelos de blocos especiais são modelos exclusivos que são animados ou apenas permitem fazer coisas que não são possíveis com blocos adaptáveis.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Ativos de ícone de aplicativo](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>Ativos de ícone de aplicativo, exibidos em várias formas em todo o sistema operacional Windows 10, são os cartões de chamada do seu aplicativo de Plataforma Universal do Windows (UWP). Estas diretrizes detalham onde os ativos de ícone de aplicativo são exibidos no sistema e fornecem dicas de design aprofundadas sobre como criar os ícones mais elaborados.</p></td>
</tr>
</tbody>
</table>

## <a name="notifications"></a>Notificações


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Notificações do sistema interativas e adaptáveis](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>As notificações do sistema interativas e adaptáveis permitem criar notificações pop-up flexíveis com mais conteúdo, imagens embutidas opcionais e interação do usuário opcional.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Visualizador de notificações](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>Visualizador de Notificações é um novo aplicativo da Plataforma Universal do Windows (UWP) na [Loja](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ajuda os desenvolvedores no design de blocos dinâmicos adaptáveis para Windows 10.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Escolher um método de entrega de notificação](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>Este artigo aborda as quatro opções de notificação - local, agendada, periódica e por push - que fornecem atualizações de blocos e notificação e conteúdo de notificações do sistema.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Enviar uma notificação de bloco local](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>Este artigo descreve como enviar uma notificação de bloco local para um bloco primário e um bloco secundário usando modelos de bloco adaptável.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Visão geral de notificações periódicas](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>Notificações periódicas, que são chamadas também de notificações de sondagem, atualizam blocos e selos em um intervalo fixo, baixando o conteúdo da nuvem.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Visão geral dos Serviços de Notificação por Push do Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Os Serviços de Notificação por Push do Windows (WNS) permitem que desenvolvedores terceirizados enviem atualizações de notificações do sistema, de blocos, de selos e brutas pelo próprio serviço de nuvem. Isso proporciona um mecanismo para entregar novas atualizações aos usuários de forma eficaz e confiável.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Código gerado pelo assistente de notificação por push](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>Ao usar um assistente no Visual Studio, você pode gerar notificações por push a partir de um serviço móvel que foi criado com os Serviços Móveis do Azure. O assistente do Visual Studio gera código para ajudá-lo a começar. Este tópico explica como o assistente modifica seu projeto, o que o código gerado faz, como usá-lo e o que pode ser feito em seguida para aproveitar ao máximo as notificações por push. Veja [Visão geral dos Serviços de Notificação por Push do Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Visão geral de notificações brutas](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>As notificações brutas são notificações por push curtas com finalidade geral. Elas são estritamente para instrução e não incluem componente de interface do usuário. Assim como nas outras notificações por push, o recurso WNS envia notificações brutas de seu serviço em nuvem para o aplicativo.</p></td>
</tr>
</tbody>
</table>

 

 

 





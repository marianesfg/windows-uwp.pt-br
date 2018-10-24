---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: Notificações de selo para aplicativos UWP
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc09e7a9d98d04c53aac0d76293b9d8e4dc6ed91
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5441276"
---
# <a name="badge-notifications-for-uwp-apps"></a>Notificações de selo para aplicativos UWP

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Bloco com um selo numérico exibindo<br/> o número 63 para indicar 63 emails não lidos.</div>

Um selo de notificação transmite as informações de resumo ou estado específicas de seu aplicativo. Elas podem ser numéricas (1 a 99) ou um de um conjunto de glifos fornecidos pelo sistema. Os exemplos de informações mais bem transmitidas por meio de um selo incluem o status da conexão de rede em um jogo online, o status de usuário em um aplicativo de mensagens, o número de emails não lidos em um aplicativo de email e o número de novas postagens em um aplicativo de mídia social. 

Selos de notificação aparecem no ícone de barra de tarefas ícone de seu app e no canto inferior direito do bloco inicial, independentemente de o app estar ou não em execução. Os selos podem ser exibidos em todos os tamanhos de bloco.  

> [!NOTE]
> Você não pode fornecer sua própria imagem de notificação; podem ser usadas somente imagens de notificação fornecidas pelo sistema.


## <a name="numeric-badges"></a>Selos numéricos

<table>
    <tr>
        <th>Valor</th>
        <th>Selo</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>Um número de 1 a 99. Um valor 0 é equivalente ao valor de glifo "nenhum" e eliminará o selo.</td>
        <td><img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /></td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>Qualquer número maior que 99.</td>
        <td><img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## <a name="glyph-badges"></a>Selos de glifo
Em vez de um número, um selo pode exibir um de um conjunto de glifos de status não extensíveis. 

<table>
<tr>
    <th>Status</th>
    <th>Glifo</th>
    <th>XML</th>
</tr>
<tr>
    <td>nenhum</td>
    <td>(Nenhum selo mostrado.)</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>atividade</td>
    <td><img src="images/badges/badge-activity.png" alt="Glyph" /></td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>alarme</td>
    <td><img src="images/badges/badge-alarm.png" alt="Glyph" /></td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>alerta</td>
    <td><img src="images/badges/badge-alert.png" alt="Glyph" /></td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>atenção</td>
    <td><img src="images/badges/badge-attention.png" alt="Glyph" /></td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>disponível</td>
    <td><img src="images/badges/badge-available.png" alt="Glyph" /></td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>ausente</td>
    <td><img src="images/badges/badge-away.png" alt="Glyph" /></td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>ocupado</td>
    <td><img src="images/badges/badge-busy.png" alt="Glyph" /></td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>erro</td>
    <td><img src="images/badges/badge-error.png" alt="Glyph" /></td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>newMessage</td>
    <td><img src="images/badges/badge-newMessage.png" alt="Glyph" /></td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>pausado</td>
    <td><img src="images/badges/badge-paused.png" alt="Glyph" /></td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>reproduzindo</td>
    <td><img src="images/badges/badge-playing.png" alt="Glyph" /></td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>indisponível</td>
    <td><img src="images/badges/badge-unavailable.png" alt="Glyph" /></td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## <a name="create-a-badge"></a>Criar um selo

Estes exemplos mostram como criar uma atualização de selo.

### <a name="create-a-numeric-badge"></a>Criar um selo numérico

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>Criar um selo de glifo
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>Limpar um selo

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>Obter o código de exemplo

* [Exemplo de notificações](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> Mostra como criar blocos dinâmicos, enviar atualizações de selo e exibir notificações do sistema. 

## <a name="related-articles"></a>Artigos relacionados

* [Notificações do sistema interativas e adaptáveis](adaptive-interactive-toasts.md)
* [Criar blocos](creating-tiles.md)
* [Criar blocos adaptáveis](create-adaptive-tiles.md)

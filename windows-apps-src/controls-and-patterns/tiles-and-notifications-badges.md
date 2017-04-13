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
ms.openlocfilehash: eea89ac8c4d7a02faee9277b0015d759340af6d6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="badge-notifications-for-uwp-apps"></a>Notificações de selo para aplicativos UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

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
        <td>![Um selo numérico menor do que 100.](images/badges/badge-numeric.png)</td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>Qualquer número maior que 99.</td>
        <td>![Um selo numérico maior que 99.](images/badges/badge-numeric-greater.png)</td></td>
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
    <td>![Glifo](images/badges/badge-activity.png)</td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>alarme</td>
    <td>![Glifo](images/badges/badge-alarm.png)</td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>alerta</td>
    <td>![Glifo](images/badges/badge-alert.png)</td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>atenção</td>
    <td>![Glifo](images/badges/badge-attention.png)</td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>disponível</td>
    <td>![Glifo](images/badges/badge-available.png)</td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>ausente</td>
    <td>![Glifo](images/badges/badge-away.png)</td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>ocupado</td>
    <td>![Glifo](images/badges/badge-busy.png)</td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>erro</td>
    <td>![Glifo](images/badges/badge-error.png)</td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>newMessage</td>
    <td>![Glifo](images/badges/badge-newMessage.png)</td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>pausado</td>
    <td>![Glifo](images/badges/badge-paused.png)</td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>reproduzindo</td>
    <td>![Glifo](images/badges/badge-playing.png)</td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>indisponível</td>
    <td>![Glifo](images/badges/badge-unavailable.png)</td>
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

* [Notificações do sistema interativas e adaptáveis](tiles-and-notifications-adaptive-interactive-toasts.md)
* [Criar blocos](tiles-and-notifications-creating-tiles.md)
* [Criar blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md)
---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628491"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
O item de estoque core representa o item padrão na qual um direito pode ser concedido.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

O objeto inventoryItem tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| url| cadeia de caracteres| Identificador exclusivo para este item de estoque específico.|
| itemType| cadeia de caracteres| Tipo do item. Os valores atuais são <ul><li><b>Desconhecido</b></li><li><b>Jogo</b></li><li><b>Movie</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>Pôster</b></li><li><b>Podcast</b></li><li><b>Image</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>Tema</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>Album</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>pacote</b></li><li><b>XnaCommunityGame</b></li><li><b>Promocionais</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>Aplicativo</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>Assinatura</b><br/><br/> **Observação:** Jogos são designados por **GameV2**, são consumíveis **GameConsumable**, e é durável DLC **GameContent**. |
  | Contêineres | cadeia de caracteres | Esse é o conjunto de "contêineres" que contêm esse item. Inventário de um usuário pode ser consultado para itens que pertencem a um contêiner específico. Esses contêineres são determinados quando o item é adicionado ao estoque pela compra. |
  | obtido | DateTime | Data e hora que o item foi adicionado ao estoque do usuário. |
  | startDate | DateTime | Data e hora que o item tornou-se ou se tornará disponível para uso. |
  | endDate | DateTime | Data e hora que o item tornou-se ou poderá ficar inutilizável. |
  | Estado | cadeia de caracteres | O estado do item. Valores permitidos são **Enabled**, **Suspended**, **expirado**, **cancelado**, **Renewed**.  |
  | trial | Valor booliano | Obrigatório. True se esse direito é uma versão de avaliação; Caso contrário, false. Se você compra a versão de avaliação de um direito e, em seguida, compra a versão completa, você receberá os dois. |
  | trialTimeRemaining | TimeSpan | Permite valor nulo. Quanto tempo é restantes na avaliação, em minutos. |
  | consumíveis | matriz | Se os itens consumíveis, ele contém uma representação embutida do identificador exclusivo (link) para o item de estoque de produtos de consumo, bem como sua quantidade atual. |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>Item de estoque de produtos de consumo

A entidade consumível apresenta o conjunto mínimo de propriedades para um item de consumo.

| Membro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| cadeia de caracteres| Identificador exclusivo para o item de estoque de produtos de consumo específicos.|
| quantity| inteiro com sinal de 32 bits| A quantidade atual deste item de estoque.|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>Referência

[/Users/me/Inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)

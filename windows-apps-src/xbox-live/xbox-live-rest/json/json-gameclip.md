---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
description: " GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc2ac0a635e4aacdc9eeefb5097c6bd946a518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646501"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
O objeto de clipe de jogo tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>gameClipId</b>| cadeia de caracteres| A ID atribuída ao jogo clipe.| 
| <b>state</b>| GameClipState| O estado do jogo clipe no sistema.| 
| <b>dateRecorded</b>| DateTime| A data e hora em que a gravação foi iniciada, em UTC (formato ISO 8601).| 
| <b>lastModified</b>| DateTime| Hora do clipe de jogo ou seus metadados, em UTC (formato ISO 8601) da última modificação.| 
| <b>userCaption</b>| cadeia de caracteres| A inserido pelo usuário não cadeia de caracteres localizada para o clipe de jogo.| 
| <b>type</b>| GameClipTypes| O tipo de recorte. Pode ser vários valores e poderá ser delimitada por vírgula nesse caso.| 
| <b>source</b>| GameClipSource| Como o clipe foi originado.| 
| <b>Visibilidade</b>| GameClipVisibility| A visibilidade do clipe jogo quando ela for publicada no sistema.| 
| <b>durationInSeconds</b>| inteiro sem sinal de 32 bits| A duração do clipe jogo em segundos.| 
| <b>scid</b>| cadeia de caracteres| SCID ao qual o clipe de jogo está associado.| 
| <b>rating</b>| número de ponto flutuante de precisão dupla| A classificação associada com o clipe de jogo, no intervalo 0,0 para 5.0.| 
| <b>ratingCount</b>| inteiro sem sinal de 32 bits| O número de vezes que o clipe foi classificado.| 
| <b>Modos de exibição</b>| inteiro sem sinal de 32 bits| O número de modos de exibição associado com o clipe de jogo.| 
| <b>titleData</b>| cadeia de caracteres| O recipiente de propriedades específico do título.| 
| <b>titleData</b>| cadeia de caracteres| O recipiente de propriedades específico do console.| 
| <b>thumbnails</b>| matriz de GameClipThumbnail| Matriz de objetos GameClipThumbnail.| 
| <b>gameClipUris</b>| matriz de GameClipUri| Matriz de objetos GameClipUri.| 
| <b>xuid</b>| cadeia de caracteres| XUID do proprietário do clipe jogo, o marshaling realizado como uma cadeia de caracteres.| 
| <b>clipName</b>| cadeia de caracteres| A versão localizada do nome do clipe, com base no local de entrada da solicitação como pesquisados do sistema de gerenciamento de título.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
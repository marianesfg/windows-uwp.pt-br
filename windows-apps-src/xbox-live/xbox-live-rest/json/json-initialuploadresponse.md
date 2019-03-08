---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589831"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
O objeto InitialUploadResponse tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>gameClipId</b>| cadeia de caracteres| ID atribuída para a solicitação de carregamento de dados.| 
| <b>uploadUri</b>| URI| Local no qual o clipe de jogo deve ser carregado.| 
| <b>largeThumbnailUri</b>| URI| Opcional. Local no qual a miniatura grande deve ser carregada. A presença desse campo é determinada pelo [enumeração ThumbnailSource](../enums/gvr-enum-thumbnailsource.md) valor na <b>InitialUploadRequest</b> (estará presente quando o upload for especificado).| 
| <b>smallThumbnailUri</b>| URI| Opcional. Local para o qual a miniatura pequeno deve ser carregada. A presença desse campo é determinada pelo [enumeração ThumbnailSource](../enums/gvr-enum-thumbnailsource.md) valor na <b>InitialUploadRequest</b> (estará presente quando o upload for especificado).| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
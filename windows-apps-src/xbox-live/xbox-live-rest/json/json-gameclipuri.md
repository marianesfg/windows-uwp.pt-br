---
title: GameClipUri (JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
description: " GameClipUri (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c74d0831c3b841ad2a1366bd2e03fb8a9b0448d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623491"
---
# <a name="gameclipuri-json"></a>GameClipUri (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
O objeto GameClipUri tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>uri</b>| cadeia de caracteres| O URI para o local do ativo de vídeo.| 
| <b>fileSize</b>| inteiro sem sinal de 32 bits| O tamanho total do arquivo de imagem em miniatura.| 
| <b>uriType</b>| GameClipUriType| O tipo do URI.| 
| <b>expiration</b>| DateTime| O tempo de expiração do URI, que está incluído nesta resposta. Se a URL está vazia ou considerado expirou antes de reprodução, os chamadores devem chamar a API RefreshUrl.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
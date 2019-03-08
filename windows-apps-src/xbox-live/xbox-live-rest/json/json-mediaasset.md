---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623471"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
Os ativos de mídia associados com a realização ou suas recompensas.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

O objeto MediaAsset tem a seguinte especificação.

| Membro| Tipo| Descrição|
| --- | --- | --- |
| name| cadeia de caracteres| O nome do MediaAsset, como "tile01".|
| type| Enumeração MediaAssetType| O tipo de ativo de mídia: <ul><li>ícone (0): O ícone de realização.</li><li>arte (1): O ativo de arte digital.</li></ul> | 
| url| cadeia de caracteres| A URL do MediaAsset.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

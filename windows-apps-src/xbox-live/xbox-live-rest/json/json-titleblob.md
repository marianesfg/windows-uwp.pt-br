---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612581"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
Contém informações sobre um título do armazenamento. 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
O objeto TitleBlob tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| clientFileTime| DateTime| [opcional] Data e hora do último carregamento do arquivo.| 
| displayName| cadeia de caracteres| [opcional] Nome do arquivo que é mostrado ao usuário.| 
| etag| cadeia de caracteres| Marca do arquivo usado no download e upload de solicitações.| 
| fileName| cadeia de caracteres| Nome do arquivo.| 
| size| inteiro com sinal de 64 bits| Tamanho do arquivo em bytes.| 
| smartBlobType| cadeia de caracteres| [opcional] Tipo de dados. Os valores possíveis são: configuração, json, binário.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
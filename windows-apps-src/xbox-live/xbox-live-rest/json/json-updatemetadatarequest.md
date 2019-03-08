---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597761"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
Os metadados que devem ser atualizados para um clipe. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
O objeto UpdateMetadataRequest tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| userCaption| cadeia de caracteres| Altera a usuário inserida não cadeia de caracteres localizada para o clipe de jogo.| 
| visibilidade| [Enumeração GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Altera a visibilidade do clipe jogo conforme ele é publicado no sistema.| 
| titleData| cadeia de caracteres| O recipiente de propriedades específico do título. Tamanho máximo: 10 KB.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 
Alterar a visibilidade e o nome de usuário do clipe:
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Alterando apenas as propriedades de título (Isso é apenas um exemplo, uma vez que o esquema desse campo é para o chamador):
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Referência 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   
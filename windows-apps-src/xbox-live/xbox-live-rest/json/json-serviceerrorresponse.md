---
title: ServiceErrorResponse (JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
description: " ServiceErrorResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86f9389f6f76c1c51955a6c784393e9b05909298
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597501"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse (JSON)
Quando um erro de serviço é encontrado, será retornado um código de erro HTTP apropriado. Opcionalmente, o serviço também pode incluir um objeto ServiceErrorResponse conforme definido abaixo. Em ambientes de produção, menos dados podem ser incluídos. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
O objeto ServiceErrorResponse tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| <b>errorCode</b>| inteiro com sinal de 32 bits| Código associado com o erro (pode ser nulo).| 
| <b>errorMessage</b>| cadeia de caracteres| Detalhes adicionais sobre o erro.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "errorCode": 8377,
   "errorMessage": "XUID specified in the claim does not match URI XUID."
 }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
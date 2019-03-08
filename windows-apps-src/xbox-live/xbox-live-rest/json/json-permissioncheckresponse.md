---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623361"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
Os resultados de uma verificação de um único usuário para uma configuração de permissão única em relação a um usuário de destino único. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
O objeto PermissionCheckResponse tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| IsAllowed| Valor booliano| Obrigatório. Esse membro é <b>verdadeira</b> se o usuário solicitante tem permissão para executar a ação solicitada com o usuário de destino.| 
| Resultados| Matriz de [PermissionCheckResult (JSON)](json-permissioncheckresult.md)| Opcional. Se <b>IsAllowed</b> era falsa e a verificação foi negada por algo relacionado ao solicitante, indica por que a permissão foi negada.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
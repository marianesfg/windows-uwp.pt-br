---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628351"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
Verifique os motivos de uma permissão de lote para a lista de valores de permissão para um usuário de destino único. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
O objeto PermissionCheckBatchUserResponse tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| Usuário| cadeia de caracteres| Obrigatório. Esse membro é <b>verdadeira</b> se o usuário solicitante tem permissão para executar a ação solicitada com o usuário de destino.| 
| Permissões| Matriz de [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| Obrigatório. Um [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) para cada permissão que foi pedido na solicitação original, na mesma ordem como a solicitação.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
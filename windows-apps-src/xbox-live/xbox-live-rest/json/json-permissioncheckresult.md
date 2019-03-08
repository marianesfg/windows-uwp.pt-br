---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598441"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
Os resultados de uma verificação de um único usuário para uma configuração de permissão única em relação a um usuário de destino único. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
O objeto PermissionCheckResult tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| reason| cadeia de caracteres| Opcional. Um <b>PermissionResultCode</b> valor que indica por que a permissão foi negada se <b>IsAllowed</b> era falsa.| 
| restrictedSetting| cadeia de caracteres| Opcional. Se o <b>PermissionResultCode</b> valor na <b>motivo</b> membro indica que uma verificação de privilégio para o solicitante falhou, isso indica quais privilégio falhou.| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   
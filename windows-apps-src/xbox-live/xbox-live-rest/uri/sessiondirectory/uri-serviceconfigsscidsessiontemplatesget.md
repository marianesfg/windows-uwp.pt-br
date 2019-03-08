---
title: GET (/serviceconfigs/{scid}/sessiontemplates)
assetID: 5172c7be-371b-f0b1-d1d0-f0981eb2bfa7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatesget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5cb51ea751ca843dfc2a08cda2e79f79409d97b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658081"
---
# <a name="get-serviceconfigsscidsessiontemplates"></a>GET (/serviceconfigs/{scid}/sessiontemplates)
Recupera um conjunto de modelos de sessão MPSD.

> [!IMPORTANT]
> Esse método URI requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Parâmetros de URI](#ID4ET)
  * [Códigos de status HTTP](#ID4E5)
  * [Corpo da solicitação](#ID4EFB)
  * [Corpo da resposta](#ID4EQB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 da sessão de ID.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 da sessão de ID. |

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EQB"></a>


## <a name="response-body"></a>Corpo da resposta


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EZB"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E2B"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

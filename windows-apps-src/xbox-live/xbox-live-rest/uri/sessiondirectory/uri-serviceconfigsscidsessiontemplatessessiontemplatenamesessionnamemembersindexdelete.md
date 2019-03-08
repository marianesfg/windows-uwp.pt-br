---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
assetID: 00aa2f3d-69a6-6d68-e99b-aad4b102aba3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fbe1942d32ee8facc83f1c723cee2aedaa1078d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618221"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
Remove os membros especificados de uma sessão.

> [!IMPORTANT]
> Esse método URI requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Parâmetros de URI](#ID4ET)
  * [Códigos de status HTTP](#ID4E5)
  * [Corpo da solicitação](#ID4EFB)
  * [Corpo da resposta](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão.|
| sessionName| GUID| ID exclusiva da sessão. Parte 3 do identificador de sessão.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>Corpo da solicitação
Ver a estrutura de solicitação no [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md).  
<a id="ID4EOB"></a>


## <a name="response-body"></a>Corpo da resposta
Ver a estrutura de resposta no [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EYB"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E1B"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

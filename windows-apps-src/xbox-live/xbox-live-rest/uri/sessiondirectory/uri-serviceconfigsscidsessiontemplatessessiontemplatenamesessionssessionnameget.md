---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: 6a4c4a13-c968-3271-cbc3-b742a8de98b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21d534d7b55934d7174c925838ed88980acff609
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655061"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Obtém um objeto de sessão.

> [!IMPORTANT]
> Esse método URI requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EMB)
  * [Códigos de status HTTP](#ID4EZB)
  * [Corpo da solicitação](#ID4E6B)
  * [Corpo da resposta](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST lê um documento de sessão para o nome especificado e recupera a sessão. Em caso de sucesso, retorna o objeto de sessão, com todos os seus atributos, obtida do servidor. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionAsync**. Os parâmetros para o método GET paralelo diretamente às especificadas na **MultiplayerSessionReference** objeto para a sessão passado a *sessionReference* parâmetro do  **GetCurrentSessionAsync**.

O formato de conexão para o método GET é mostrado abaixo.

```cpp
GET /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
      Content-Type: application/json

```



<a id="ID4EMB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|
| sessionTemplateName| cadeia de caracteres| Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão.|
| sessionName| GUID| ID exclusiva da sessão. Parte 3 do identificador de sessão.|

<a id="ID4EZB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4E6B"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EKC"></a>


## <a name="response-body"></a>Corpo da resposta
Ver a estrutura de resposta no [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4ETC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

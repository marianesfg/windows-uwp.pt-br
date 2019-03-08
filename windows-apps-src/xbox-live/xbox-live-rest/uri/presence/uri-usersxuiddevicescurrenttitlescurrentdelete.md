---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604211"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
Remover a presença de um título de fechamento, em vez de aguardar o [PresenceRecord](../../json/json-presencerecord.md) expirar. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EZ)
  * [Autorização](#ID4EEB)
  * [Cabeçalhos de solicitação necessários](#ID4ERD)
  * [Cabeçalhos de solicitação opcionais](#ID4EVF)
  * [Corpo da solicitação](#ID4EVG)
  * [Corpo da resposta](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário de destino.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorização
 
| Tipo| Obrigatório| Descrição| Resposta se ausente| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Sim| ID de usuário do Xbox (XUID) do chamador| 403 Proibido| 
| titleId| Sim| titleId do título| 403 Proibido| 
| deviceId| Sim para tudo, exceto Windows e Web| deviceId do chamador| 403 Proibido| 
| deviceType| Sim para tudo, exceto a Web| deviceType do chamador| 403 Proibido| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.| 
| Content-Type| cadeia de caracteres| O tipo mime do corpo da solicitação de valor de exemplo: application/json.| 
| Content-Length| cadeia de caracteres| O comprimento do corpo da solicitação. Valor de exemplo: 312.| 
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: presencebeta.xboxlive.com.| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Em caso de êxito, o código de status HTTP é retornado sem corpo de resposta.
 
No caso de erro (HTTP 4xx ou 5xx), informações de erro apropriado são retornadas no corpo da resposta.
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   
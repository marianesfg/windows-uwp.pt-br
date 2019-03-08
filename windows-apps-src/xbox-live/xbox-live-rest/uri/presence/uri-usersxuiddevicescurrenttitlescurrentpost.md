---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649521"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
Atualize um título com a presença de um usuário. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Autorização](#ID4EPB)
  * [Cabeçalhos de solicitação necessários](#ID4ENE)
  * [Cabeçalhos de solicitação opcionais](#ID4ERG)
  * [Corpo da solicitação](#ID4ERH)
  * [Corpo da resposta](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Esse URI pode ser usado por todos os títulos em plataformas não-console para adicionar e atualizar a presença, recursos avançados de presença e dados de presença de mídia para um título.
 
**SandboxId** agora é recuperado da declaração no XToken e impostas. Se o **SandboxId** não estiver presente, em seguida, os serviços de descoberta de entretenimento (EDS) gerará um erro 400 Solicitação incorreta.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário de destino.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorização
 
| Tipo| Obrigatório| Descrição| Resposta se ausente| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Sim| ID de usuário do Xbox (XUID) do chamador| 403 Proibido| 
| titleId| Sim| titleId do título| 403 Proibido| 
| deviceId| Sim para tudo, exceto Windows e Web| deviceId do chamador| 403 Proibido| 
| deviceType| Sim para tudo, exceto a Web| deviceType do chamador| 403 Proibido| 
| sandboxId| Sim para provenientes de chamadas | área restrita do chamador| 403 Proibido| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.| 
| Content-Type| cadeia de caracteres| O tipo mime do corpo da solicitação de valor de exemplo: application/json.| 
| Content-Length| cadeia de caracteres| O comprimento do corpo da solicitação. Valor de exemplo: 312.| 
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: presencebeta.xboxlive.com.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
O objeto de solicitação é uma [TitleRequest](../../json/json-titlerequest.md). Somente as propriedades, na verdade, está presentes no corpo da são atualizadas. Todas as propriedades que são não faz parte do corpo, mas existe no servidor não será modificado.
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Em caso de êxito, um código de status HTTP de 200 ou 201 criado será retornado, conforme apropriado.
 
Em caso de erro (HTTP 4xx ou 5xx), informações de erro apropriado são retornadas no corpo da resposta.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   
---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594101"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
Exclui uma mensagem do usuário na entrada do usuário. O domínio para esses URIs é `msg.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4ECB)
  * [Autorização](#ID4EPB)
  * [Corpo da solicitação](#ID4E1B)
  * [Códigos de status HTTP](#ID4EHC)
  * [Resposta do JavaScript Object Notation (JSON)](#ID4EAE)
  * [Efeito das configurações de privacidade no recurso](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários 
 
A operação de exclusão é idempotente.
 
O tipo de conteúdo só dá suporte a essa API é "application/json", que é necessário nos cabeçalhos HTTP de cada chamada. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid | inteiro sem sinal de 64 bits | O Xbox usuário ID (XUID) do player que está fazendo a solicitação. | 
| messageId | cadeia de caracteres [50] | ID da mensagem que está sendo recuperado ou excluído. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorização 
 
Você deve ter seu próprio usuário de declaração para excluir uma mensagem do usuário.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>Corpo da solicitação 
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP 
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Descrição| 
| --- | --- | --- | --- | --- | 
| 204| Êxito.| 
| 403| O XUID não pode ser convertido ou uma declaração XUID válida não foi encontrada.| 
| 404| ID da mensagem no URI não pode ser analisado ou um XUID está ausente no URI.| 
| 500| Erro geral do lado do servidor.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>Resposta do JavaScript Object Notation (JSON) 
 
No caso de erro, o serviço pode retornar um objeto errorResponse, que pode conter valores do ambiente do serviço.
 
| Propriedade| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| cadeia de caracteres| Uma indicação de onde o erro foi originado.| 
| errorCode| int| Código numérico associado ao erro (pode ser nulo).| 
| errorMessage| cadeia de caracteres| Detalhes do erro se configurada para mostrar os detalhes.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso 
 
Só é possível excluir suas próprias mensagens de usuário. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referência [códigos de status HTTP padrão](../../additional/httpstatuscodes.md)

   
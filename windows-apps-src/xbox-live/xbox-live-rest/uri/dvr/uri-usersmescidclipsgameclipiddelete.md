---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622831"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
Clipe de jogo de excluir os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4ECB)
  * [Autorização](#ID4ENB)
  * [Cabeçalhos de solicitação necessários](#ID4EYB)
  * [Cabeçalhos de solicitação opcionais](#ID4EEE)
  * [Corpo da solicitação](#ID4ENF)
  * [Códigos de status HTTP](#ID4EYF)
  * [Cabeçalhos de resposta necessária](#ID4EIAAC)
  * [Cabeçalhos de resposta opcional](#ID4E2CAC)
  * [Corpo da resposta](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
Fornece um mecanismo para excluir o vídeo do usuário do serviço GameClips. Após a exclusão, todos os metadados e os ativos de vídeo reais (gerados e originais) são removidos do sistema. Essa é uma operação permanente. 

> [!NOTE] 
> A ID do proprietário especificada deve corresponder ao chamador no token de autorização para a solicitação de exclusão seja bem-sucedida. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
| gameClipId| cadeia de caracteres| ID do clipe de jogo do recurso que está sendo acessado.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>Autorização
 
Somente a declaração Xuid é necessária para esse método.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.| 
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| OK| Exclusão bem-sucedida do clipe.| 
| 401| Não autorizado| Há um problema com o formato do token de autenticação na solicitação.| 
| 403| Proibido| Algumas necessárias declarações estão ausentes.| 
| 404| Não encontrado| O clipe especificado na URL não estava presente (ou ele foi excluído pela segunda vez).| 
| 503| Não aceitável| O serviço ou algumas dependências downstream estão inativos. Tente novamente com o comportamento padrão de retirada.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível.| 
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| cadeia de caracteres| Usada para otimização de cache. Exemplo: "686897696a7c876b7e".| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
O serviço responderá com um código de status HTTP 204 (sem conteúdo) após o êxito. Tentando excluir o mesmo objeto ou um objeto inexistentes retornarão 404.
 
No caso de erros, uma **ServiceErrorResponse** objeto será retornado.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   
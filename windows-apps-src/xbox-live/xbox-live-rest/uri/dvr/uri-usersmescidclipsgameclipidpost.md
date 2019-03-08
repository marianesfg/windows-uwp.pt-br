---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645821"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
Atualize metadados de clipe de jogo para os dados do usuário. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Comentários](#ID4EX)
  * [Parâmetros de URI](#ID4EAB)
  * [Cabeçalhos de solicitação necessários](#ID4ELB)
  * [Cabeçalhos de solicitação opcionais](#ID4EXD)
  * [Corpo da solicitação](#ID4EAF)
  * [Cabeçalhos de resposta necessária](#ID4EVF)
  * [Cabeçalhos de resposta opcional](#ID4EJAAC)
  * [Corpo da resposta](#ID4EJBAC)
  * [URIs relacionados](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Comentários
 
A API para atualizar metadados do clipe de jogo se enquadra em duas categorias, atualizando os metadados de clipes jogo de própria como o título e a acessibilidade e a atualização dos atributos públicos (como aplicar uma classificação ou incrementar a contagem de exibição) de qualquer clipe de jogo. Se o XUID no URI não corresponder a XUID na declaração, somente os dados públicos podem ser editados, e todas as solicitações para editar qualquer um dos outros dados serão negadas. No caso de vários campos estão tentando ser editado e uma delas é inválida para a solicitação, a solicitação inteira falhará.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
| gameClipId| cadeia de caracteres| ID do clipe de jogo do recurso que está sendo acessado.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| cadeia de caracteres| Codificações de compactação aceitável. Valores de exemplo: gzip, deflate, identidade.| 
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
O corpo da solicitação deve ser um [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) objeto no formato JSON. Exemplos:
 
Alterar a visibilidade e o nome de usuário do clipe:
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Alterando apenas as propriedades de título (Isso é apenas um exemplo, uma vez que o esquema desse campo é para o chamador):
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Exemplos: 1, vnext.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Cache-Control| cadeia de caracteres| Cortês solicitação para especificar o comportamento do cache.| 
| Aceitar| cadeia de caracteres| Valores aceitáveis de Content-Type. Exemplo: <b>application/json</b>.| 
| Retry-After| cadeia de caracteres| Instrui o cliente tente novamente mais tarde no caso de um servidor não disponível.| 
| Variar| cadeia de caracteres| Instrui como armazenar em cache as respostas de proxies de downstream.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| cadeia de caracteres| Usada para otimização de cache. Exemplo: "686897696a7c876b7e".| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Após a atualização bem-sucedida dos metadados de um código de status HTTP 200 será retornada.
 
Caso contrário, será retornado um objeto ServiceErrorResponse no formato JSON com um código de status HTTP apropriado.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>URIs relacionados
 
Os seguintes URIs atualizar campos públicos nos metadados. Não há nenhum corpo obrigatório para essas solicitações. Após a atualização bem-sucedida dos metadados de um código de status HTTP 200 será retornada. Caso contrário, será retornado um objeto ServiceErrorResponse no formato JSON com um código de status HTTP apropriado.
 
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} /ratings/ {valor de classificação}** -aplica-se a classificação especificada para o clipe especificado. Valor de classificação deve ser um inteiro entre 1 e 5.
   * **POSTAR /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / sinalizador** -sinaliza o clipe para conter conteúdo potencialmente questionável a ser verificado por imposição.
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / exibições** -incrementa a contagem de exibição para o clipe de jogo especificado. É recomendável que isso é chamado não correto quando a reprodução é iniciada, mas quando 75 a 80% de reprodução de foi concluída.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   
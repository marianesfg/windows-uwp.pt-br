---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594751"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
Obtém uma resposta Sim ou não sobre se o usuário tem permissão para executar a ação especificada com um usuário de destino.

  * [Parâmetros de URI](#ID4EQ)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4E2)
  * [Autorização](#ID4EDC)
  * [Cabeçalhos de solicitação necessários](#ID4EID)
  * [Corpo da solicitação](#ID4ETE)
  * [Códigos de status HTTP](#ID4E5E)
  * [Cabeçalhos de resposta necessária](#ID4ETG)
  * [Corpo da resposta](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| requestorId| cadeia de caracteres| Obrigatório. Identificador do usuário que está executando a ação. Os valores possíveis são <code>xuid({xuid})</code> e <code>me</code>. Isso deve ser um usuário que fez logon. Valor de exemplo: <code>xuid(0987654321)</code>.|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| Configuração| enumeração de cadeia de caracteres| O valor de PermissionId para verificar em relação a. Valor de exemplo: "CommunicateUsingText".|
| destino| cadeia de caracteres| Identificador do usuário no qual a ação deve ser executado. Os valores possíveis são <code>xuid({xuid})</code>. Valores de exemplo: <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>Autorização

Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xuid| inteiro com sinal de 64 bits| sim| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valores de exemplo: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor de exemplo: 1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 400| A solicitação é inválida.| Exemplos: IDs de configuração incorreta, URIs incorreto, etc.|
| 404| O usuário especificado no URI não existe.| O recurso especificado não pôde ser encontrado.|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| cadeia de caracteres| O tipo MIME do corpo da solicitação. Valor de exemplo: <code>application/json</code>|
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: 34|
| Cache-Control| cadeia de caracteres| Cortês solicitação do servidor para especificar o comportamento do cache. Exemplo: <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>Corpo da resposta

Ver [PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md).

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Enumeração PermissionId](../../enums/privacy-enum-permissionid.md)

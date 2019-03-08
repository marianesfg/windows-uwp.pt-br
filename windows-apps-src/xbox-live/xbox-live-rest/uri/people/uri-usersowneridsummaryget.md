---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613991"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
Obtém os dados de resumo sobre o proprietário da perspectiva do chamador.

  * [Parâmetros de URI](#ID4EQ)
  * [Autorização](#ID4E2)
  * [Cabeçalhos de solicitação necessários](#ID4EBC)
  * [Cabeçalhos de solicitação opcionais](#ID4EHD)
  * [Corpo da solicitação](#ID4EXE)
  * [Códigos de status HTTP](#ID4ECF)
  * [Cabeçalhos de resposta necessária](#ID4EZG)
  * [Corpo da resposta](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| ownerId| cadeia de caracteres| Identificador do usuário cujo recurso está sendo acessado. Os valores possíveis são "me", xuid({xuid}) ou gt({gamertag}). Valores de exemplo: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>Autorização

| <b>Nome</b>| <b>Tipo</b>| <b>Descrição</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| inteiro sem sinal de 64 bits| Obrigatório. Identificador de usuário do autor da chamada. Valor de exemplo: 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Dados de autorização para. Isso normalmente é um token XSTS criptografado. Valor de exemplo: <b>XBL3.0 x = [hash]; [token] </b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço ao qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valores de exemplo: 1|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. Todas as respostas serão <code>application/json</code>.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 400| Solicitação Inválida| As IDs de usuário foram malformadas.|
| 403| Proibido| Não foi possível analisar a declaração XUID do cabeçalho de autorização.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: 232.|
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Isso deve ser <b>application/json</b>.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>Corpo da resposta

Ver [PersonSummary (JSON)](../../json/json-personsummary.md).

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/summary](uri-usersowneridsummary.md)

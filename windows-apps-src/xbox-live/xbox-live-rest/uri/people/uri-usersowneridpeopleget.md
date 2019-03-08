---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623761"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
Obtém as pessoas do chamador coleção.
O domínio para esses URIs é `social.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4E5)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EJB)
  * [Autorização](#ID4ERD)
  * [Cabeçalhos de solicitação necessários](#ID4EZE)
  * [Cabeçalhos de solicitação opcionais](#ID4EYF)
  * [Corpo da solicitação](#ID4E5G)
  * [Códigos de status HTTP](#ID4EJH)
  * [Cabeçalhos de resposta necessária](#ID4EBBAC)
  * [Corpo da resposta](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

Operações GET não modificará todos os recursos, portanto, isso produzirá os mesmos resultados se executada uma vez ou várias vezes.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| ownerId| cadeia de caracteres| Identificador do usuário cujo recurso está sendo acessado. Deve corresponder ao usuário autenticado. Os valores possíveis são "me", xuid({xuid}) ou gt({gamertag}).|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| modo de exibição| cadeia de caracteres| Retorne as pessoas associadas com um modo de exibição. O valor padrão é "tudo". Os valores possíveis são: <ul><li><b>Todos os</b> -retorna todas as pessoas na lista de pessoas do usuário. Esse é o valor padrão.</li><li><b>Favoritos</b> -retorna todas as pessoas na lista de pessoas do usuário que têm o atributo favorito.</li><li><b>LegacyXboxLiveFriends</b> -retorna todas as pessoas na lista de pessoas do usuário que também são herdados amigos do Xbox LIVE.</li></br>**Observação:**  Somente o **todos os** valor terá suporte se o usuário da chamada é diferente do usuário proprietário.|
| startIndex| inteiro sem sinal de 32 bits| Retorne os itens começando no índice especificado.  
| maxItems| inteiro sem sinal de 32 bits| Número máximo de pessoas para retornar da coleção começando do índice inicial. O serviço pode fornecer um valor padrão se <b>maxItems</b> não está presente e pode retornar menos de <b>maxItems</b> (mesmo se a última página de resultados ainda não foi retornada).|

<a id="ID4ERD"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| sim| Chamador tiver a ID de usuário na Xbox do (XUID usuário).| 401 não autorizado|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| Cadeia de caracteres. Dados de autorização do Xbox LIVE. Isso normalmente é um token XSTS criptografado. Valor de exemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor padrão: 1.|
| Aceitar| Cadeia de caracteres. Tipos de conteúdo que o chamador aceita na resposta. Todas as respostas são <b>application/json</b>.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Êxito.|
| 400| Solicitação Inválida| Parâmetros de consulta ou as IDs de usuário foram malformadas.|
| 403| Proibido| Não foi possível analisar a declaração XUID do cabeçalho de autorização.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| inteiro sem sinal de 32 bits| Comprimento, em bytes, do corpo da resposta. Valor de exemplo: 22.|
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Isso sempre será <b>application/json</b>.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>Corpo da resposta

Se a chamada for bem-sucedida, o serviço retorna o número total de pessoas na coleção de pessoas do chamador e uma matriz que contém a coleção de pessoas do chamador. Ver [PeopleList (JSON)](../../json/json-peoplelist.md).

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people](uri-usersowneridpeople.md)

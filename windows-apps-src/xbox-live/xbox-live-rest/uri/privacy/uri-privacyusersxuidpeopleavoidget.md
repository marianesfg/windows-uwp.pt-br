---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641611"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
Obtém a lista de Evite para um usuário.

  * [Comentários](#ID4EQ)
  * [Parâmetros de URI](#ID4EZ)
  * [Autorização](#ID4EEB)
  * [Cabeçalhos de solicitação necessários](#ID4EJC)
  * [Códigos de status HTTP](#ID4EYD)
  * [Cabeçalhos de resposta necessária](#ID4E1F)
  * [Corpo da resposta](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Comentários

Se um destino for fornecido, retornará apenas o que o usuário se eles estiverem na lista de bloqueios, ou vazio se eles não são.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| ownerId| cadeia de caracteres| Obrigatório. Identificador do usuário cujo recurso está sendo acessado. Os valores possíveis são <code>xuid({xuid})</code>. Deve ser o usuário autenticado. Valor de exemplo: <code>xuid(2603643534573581)</code>. Tamanho máximo: nenhum. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>Autorização

Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| inteiro com sinal de 64 bits| sim| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização | cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: <code>Xauth=&lt;authtoken></code>. Tamanho máximo: nenhum.|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. Valor de exemplo: <code>application/json</code>. Tamanho máximo: nenhum.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 400| Solicitação Inválida| A ID de destino especificada no URI não é válida.|
| 403| Proibido| O proprietário especificado no URI não é o usuário autenticado.|
| 404| Não encontrado| O proprietário especificado no URI não existe.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| cadeia de caracteres| O tipo MIME do corpo da solicitação. Valor de exemplo: <code>application/json</code>. Tamanho máximo: nenhum.|
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: 34. Tamanho máximo: nenhum.|
| Cache-Control| cadeia de caracteres| Cortês solicitação do servidor para especificar o comportamento do cache. Valor de exemplo: <code>application/json</code>. Tamanho máximo: nenhum.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EYH"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)

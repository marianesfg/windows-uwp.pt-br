---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661581"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
Obtém a lista sem áudio para um usuário.

  * [Comentários](#ID4EQ)
  * [Parâmetros de URI](#ID4EZ)
  * [Efeito das configurações de privacidade no recurso](#ID4EEB)
  * [Autorização](#ID4ENB)
  * [Cabeçalhos de solicitação necessários](#ID4ESC)
  * [Corpo da solicitação](#ID4EPE)
  * [Códigos de status HTTP](#ID4E1E)
  * [Cabeçalhos de resposta necessária](#ID4E3G)
  * [Corpo da resposta](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Comentários

Se um destino for fornecido, esse URI retorna somente o usuário se o usuário é a lista sem áudio, ou vazio se o usuário não é.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| ownerId| cadeia de caracteres| Obrigatório. Identificador do usuário cujo recurso está sendo acessado. Os valores possíveis são "me" <code>xuid({xuid})</code>, ou gt({gamertag}). Deve ser o usuário autenticado. Valores de exemplo: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Tamanho máximo: nenhum. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Nenhum.

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorização

Declarações de autorização usadas | Declaração| Tipo| Necessário?| Valor de exemplo|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| inteiro com sinal de 64 bits| sim| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização | cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: <code>Xauth=&lt;authtoken></code>. Tamanho máximo: nenhum.|
| X-RequestedServiceVersion| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autorização e assim por diante. Valores de exemplo: <code>1</code>, <code>vnext</code>. Tamanho máximo: nenhum.|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. Valor de exemplo: <code>application/json</code>. Tamanho máximo: nenhum.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Solicitação bem-sucedida para a lista sem áudio.|
| 400| Solicitação Inválida| A ID de destino especificada no URI não é válida.|
| 403| Proibido| O proprietário especificado no URI não é o usuário autenticado.|
| 404| Não encontrado| O proprietário especificado no URI não existe.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| cadeia de caracteres| O tipo MIME do corpo da solicitação. Valor de exemplo: <code>application/json</code>|
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: 34|
| Cache-Control| cadeia de caracteres| Cortês solicitação do servidor para especificar o comportamento do cache. Exemplo: <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>Resposta de exemplo

Ver [UserList](../../json/json-userlist.md).


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)

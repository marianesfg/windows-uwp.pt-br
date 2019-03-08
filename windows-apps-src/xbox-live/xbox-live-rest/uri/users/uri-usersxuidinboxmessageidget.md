---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618051"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
Recupera o texto de mensagem detalhada para uma mensagem de determinado usuário, marcá-lo como lido no serviço.
O domínio para esses URIs é `msg.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Autorização](#ID4ERB)
  * [Corpo da solicitação](#ID4E3B)
  * [Efeito das configurações de privacidade no recurso](#ID4EJC)
  * [Códigos de status HTTP](#ID4EUC)
  * [Resposta do JavaScript Object Notation (JSON)](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

A operação get só pode ser executada sobre os tipos de mensagem do usuário, sistema e FriendRequest.

Esse URI requer uma atualização em Xbox.com. Atualmente, o Xbox 360 não atualizará o estado de lido/não lido até que um usuário sai e volta.

O tipo de conteúdo só dá suporte a essa API é "application/json", que é necessário nos cabeçalhos HTTP de cada chamada.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid | inteiro sem sinal de 64 bits | O Xbox usuário ID (XUID) do player que está fazendo a solicitação. |
| messageId | cadeia de caracteres [50] | ID da mensagem que está sendo recuperado ou excluído. |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Autorização

Você deve ter seu próprio usuário de declaração para recuperar uma mensagem do usuário.

<a id="ID4E3B"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Você pode recuperar apenas suas próprias mensagens de usuário.

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Descrição|
| --- | --- | --- | --- | --- |
| 200| Êxito.|
| 400| O XUID não pode ser convertido corretamente.|
| 403| O XUID não pode ser convertido ou uma declaração XUID válida não foi encontrada.|
| 404| Um XUID válida está ausente ou a ID da mensagem não pode ser encontrada ou é analisada incorretamente.|
| 500| Erro geral do lado do servidor ou o tipo de mensagem não é válido para GET.|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>Resposta do JavaScript Object Notation (JSON)

Se for chamado com êxito, o serviço retorna os dados de resultados em um formato JSON. O objeto raiz é um objeto UserMessageHeader.

#### <a name="usermessageheader"></a>UserMessageHeader

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| header| Cabeçalho|  | Objeto JSON|
| messageText| cadeia de caracteres| 256| UTF-8|

#### <a name="header"></a>Cabeçalho

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| enviado| DateTime|  | Data e hora em que a mensagem foi enviada. (Fornecido pelo serviço).|
| expiração| DateTime|  | Data e hora que a mensagem expira. (Todas as mensagens têm um tempo de vida máximo deve ser determinada no futuro.)|
| messageType| cadeia de caracteres| 13| Tipos de mensagem: Usuário, sistema, FriendRequest.|
| senderXuid| ULong|  | XUID do remetente.|
| remetente| cadeia de caracteres| 15| Gamertag do remetente.|
| hasAudio| bool|  | Se a mensagem tem um anexo de áudio (voz).|
| hasPhoto| bool|  | Se a mensagem tem um anexo de fotos.|
| hasText| bool|  | Se a mensagem contém texto.|

#### <a name="sample-response"></a>Resposta de exemplo

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>Resposta de erro

No caso de erro, o serviço pode retornar um objeto errorResponse, que pode conter valores do ambiente do serviço.

| Propriedade| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| cadeia de caracteres| Uma indicação de onde o erro foi originado.|
| errorCode| int| Código numérico associado ao erro (pode ser nulo).|
| errorMessage| cadeia de caracteres| Detalhes do erro se configurada para mostrar os detalhes.|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referência [códigos de status HTTP padrão](../../additional/httpstatuscodes.md)

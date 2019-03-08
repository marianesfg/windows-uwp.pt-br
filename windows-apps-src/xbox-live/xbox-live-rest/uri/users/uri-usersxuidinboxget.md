---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632241"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
Recupera um número especificado de resumos de mensagens de usuário do serviço.
O domínio para esses URIs é `msg.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EEB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EIC)
  * [Autorização](#ID4EGE)
  * [Efeito das configurações de privacidade no recurso](#ID4ETE)
  * [Códigos de status HTTP](#ID4E5E)
  * [Resposta do JavaScript Object Notation (JSON)](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

Um resumo de mensagem do usuário contém apenas o assunto da mensagem. Para mensagens geradas pelo usuário, isso é atualmente os primeiros 20 caracteres do texto da mensagem. Mensagens de sistema podem fornecer uma entidade alternativo, como "Sistema ao vivo".

As mensagens são retornadas na ordem inversa da ordem que foram enviadas; ou seja, as mensagens mais recentes são retornadas primeiro.

O tipo de conteúdo só dá suporte a essa API é "application/json", que é necessário nos cabeçalhos HTTP de cada chamada.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| inteiro sem sinal de 64 bits| O Xbox usuário ID (XUID) do player que está fazendo a solicitação.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| int| 100| Número máximo de mensagens retornadas.|
| continuationToken| cadeia de caracteres|  | Cadeia de caracteres retornada em uma chamada anterior de enumeração; usado para continuar a enumeração.|
| skipItems| int| 100| Número de mensagens a serem ignoradas; ignorado se continuationToken está presente.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>Autorização

Você deve ter seu próprio usuário de declaração para recuperar um resumo de mensagem do usuário.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Somente você pode enumerar suas próprias mensagens de usuário.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| A solicitação foi bem-sucedida.|
| 400| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.|
| 403| A solicitação não é permitida para o usuário ou serviço.|
| 404| Um XUID válida está ausente no URI.|
| 409| A coleção subjacente foi alterada com base no token de continuação que foi passado.|
| 416| O número de itens a serem ignoradas é maior que o número de itens disponíveis.|
| 500| Erro geral do lado do servidor.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>Resposta do JavaScript Object Notation (JSON)

Se for chamado com êxito, o serviço retorna os dados de resultados em um formato JSON.

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| resultados| Message[]| 100| Matriz de mensagens de usuário|
| pagingInfo| PagingInfo|  | Informações de paginação para o conjunto atual de resultados|

#### <a name="message"></a>Mensagem

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| header| Cabeçalho|  | Cabeçalho de mensagem do usuário|
| messageSummary| cadeia de caracteres| 20| UTF-8; Geralmente, 20 primeiros caracteres da mensagem|

#### <a name="header"></a>Cabeçalho

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| id| cadeia de caracteres| 50| Identificador de mensagem, usado para recuperar os detalhes da mensagem ou exclusão de mensagens.|
| isRead| bool|  | Sinalizador que indica que o usuário já tenha lido os detalhes da mensagem.|
| enviado| DateTime|  | Data e hora UTC em que a mensagem foi enviada. (Fornecido pelo serviço).|
| expiração| DateTime|  | Data e hora UTC que a mensagem expira. (Todas as mensagens têm um tempo de vida máximo deve ser determinada no futuro.)|
| messageType| cadeia de caracteres| 50| Tipos de mensagem: Usuário, sistema, FriendRequest, vídeo, QuickChat, VideoChat, PartyChat, título, GameInvite.|
| senderXuid| ULong|  | XUID do remetente.|
| remetente| cadeia de caracteres| 15| Gamertag do remetente.|
| hasAudio| bool|  | Se a mensagem tem um anexo de áudio (voz).|
| hasPhoto| bool|  | Se a mensagem tem um anexo de fotos.|
| hasText| bool|  | Se a mensagem contém texto.|

#### <a name="paging-info"></a>Informações de paginação

| Propriedade| Tipo| Comprimento máximo| Comentários|
| --- | --- | --- | --- |
| continuationToken| cadeia de caracteres| 100| Opcionalmente, retornado pelo servidor. Permite chamadas posteriores para continuar a enumeração.|
| totalItems| int|  | Número total de mensagens na caixa de entrada.|

#### <a name="sample-response"></a>Resposta de exemplo

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>Resposta de erro

No caso de erro, o serviço pode retornar um objeto errorResponse, que pode conter valores do ambiente do serviço.

| Propriedade| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| cadeia de caracteres| Uma indicação de onde o erro foi originado.|
| errorCode| int| Código numérico associado ao erro (pode ser nulo).|
| errorMessage| cadeia de caracteres| Detalhes do erro se configurada para mostrar os detalhes.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referência [códigos de status HTTP padrão](../../additional/httpstatuscodes.md)

---
title: POST (/serviceconfigs/{scid}/batch)
assetID: b821a6eb-1add-ef91-bdf5-10e107082197
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatchpost.html
description: " POST (/serviceconfigs/{scid}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e718fda26264667a7bf08c9254a462eb268dcd74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603441"
---
# <a name="post-serviceconfigsscidbatch"></a>POST (/serviceconfigs/{scid}/batch)
Cria uma consulta em lotes várias Xbox IDs de usuário para a configuração de serviço.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4ELB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EVB)
  * [Códigos de status HTTP](#ID4EGF)
  * [Corpo da solicitação](#ID4ENF)
  * [Corpo da resposta](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST cria consultas em lote várias Xbox IDs de usuário no nível de ID (SCID) de configuração do serviço. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**.

Para vários jogadores de 2015, você pode combinar várias consultas em uma única chamada se todas as consultas forem o mesmo mas lidar com diferentes IDs de usuário do Xbox (XUIDs), conforme representado a *xuid* parâmetro de cadeia de caracteres de consulta. Observe que os parâmetros de cadeia de caracteres de consulta e as respostas são as mesmas para consultas regulares e consultas em lote.

Para uma consulta em lotes, as sessões que pertencem a cada XUID são gravadas para a resposta na mesma ordem que o *xuid* parâmetros são apresentados na solicitação. É possível para a mesma sessão que aparecem várias vezes na resposta, uma vez para cada *xuid* que corresponde a ele.

<a id="ID4ELB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

Uma consulta pode ser modificada usando os parâmetros de cadeia de caracteres de consulta na tabela a seguir.

| <b>Parâmetro</b>| <b>Tipo</b>| <b>Descrição</b>|
| --- | --- | --- | --- | --- | --- | --- |
| Palavra-chave| cadeia de caracteres| A palavra-chave, por exemplo, "foo", que deve ser encontrado em sessões ou modelos se eles devem ser recuperadas. |
| xuid| inteiro sem sinal de 64 bits| A ID de usuário Xbox, por exemplo, "123", para as sessões para incluir na consulta. Por padrão, o usuário deve estar ativo na sessão para que ele seja incluído. |
| reservas| booliano| True para incluir as sessões para o qual o usuário é definido como um player reservado, mas não ingressou para se tornar um player de Active Directory. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| inativo| booliano| True para incluir as sessões que o usuário aceitou, mas não estiver em execução ativamente. Contagem de sessões nas quais o usuário está "pronto", mas não "ativo" como inativas. |
| privado| booliano| True para incluir as sessões privadas. Esse parâmetro é usado apenas ao consultar suas próprias sessões ou ao consultar as sessões de servidor-para-servidor um usuário específico. |
| visibilidade| cadeia de caracteres| Estado de visibilidade para as sessões. Os valores possíveis são definidos pela <b>MultiplayerSessionVisibility</b>. Se esse parâmetro é definido como "aberta", a consulta deve incluir sessões abertas somente. Se ele for definido como "private", o <i>privada</i> parâmetro deve ser definido como true. |
| version| inteiro com sinal de 32 bits| A versão de máximo de sessão que deve ser incluída. Por exemplo, um valor de 2 Especifica que apenas sessões com uma versão de sessão principal de 2 ou menor deve ser incluída. O número de versão deve ser menor ou igual a mod de versão de contrato da solicitação 100. |
| Take| inteiro com sinal de 32 bits| O número máximo de sessões para recuperar. Esse número deve ser entre 0 e 100. |


Configuração tanto *privados* ou *reservas* como true requer acesso de nível de servidor para a sessão. Como alternativa, essas configurações exigem XUID do chamador de declaração para corresponder ao filtro XUID no URI. Caso contrário, o código de status HTTP/403 é retornado, se realmente existirem tais as sessões ou não.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>Corpo da solicitação


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>Corpo da resposta

O retorno desse método é uma matriz JSON de referências de sessão, com alguns dados incluídos de sessão em linha.


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}

```


<a id="ID4EDG"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

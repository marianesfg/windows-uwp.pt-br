---
title: GET (/serviceconfigs/{scid}/sessions)
assetID: adc65d0b-58dd-bfb9-54c8-9bc9d02e68ec
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessionsget.html
description: " GET (/serviceconfigs/{scid}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e54c9cd68a899cfd040bc3e16a05f6deb2daa7c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614101"
---
# <a name="get-serviceconfigsscidsessions"></a>GET (/serviceconfigs/{scid}/sessions)
Recupera especificado informações da sessão.

> [!IMPORTANT]
> Agora, esse método requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EKB)
  * [Códigos de status HTTP](#ID4EXB)
  * [Corpo da solicitação](#ID4EAC)
  * [Corpo da resposta](#ID4ELC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST recupera informações de sessão para os filtros fornecidos. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**.


> [!NOTE] 
> Para vários jogadores de 2015, este método é chamado pelo <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>.  



> [!NOTE] 
> Todas as chamadas para esse método devem incluir uma palavra-chave, um filtro de ID de usuário do Xbox ou ambos. Se o chamador não tem as permissões corretas para o <i>privados</i> e <i>reservas</i> parâmetros, o método retorna um código de erro de 403 Proibido, se realmente existirem tais as sessões ou não.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 da sessão de ID.|
| Palavra-chave| cadeia de caracteres| Uma palavra-chave usada para filtrar resultados para apenas sessões identificados com essa cadeia de caracteres.|
| xuid| GUID| IDs de usuário do Xbox para os usuários para o qual recuperar as sessões. Os usuários devem estar ativos nas sessões.|
| reservas| cadeia de caracteres| Valor que indica se a lista de sessões inclui aqueles que os usuários não foram aceitos. Esse parâmetro só pode ser definido como true. Essa configuração exige que o chamador tenha acesso de nível de servidor à sessão ou XUID do chamador de declaração para corresponder o filtro de ID de usuário do Xbox. |
| inativo| cadeia de caracteres| Valor que indica se a lista de sessões inclui aquelas que os usuários que aceitaram, mas não estão em execução ativamente. Esse parâmetro só pode ser definido como true.|
| privado| cadeia de caracteres| Valor que indica se a lista de sessões inclui sessões privadas. Esse parâmetro só pode ser definido como true. Ele é válido somente ao consultar suas próprias sessões ou ao consultar o servidor para servidor. Definir esse parâmetro como true requer que o chamador tenha acesso de nível de servidor à sessão ou XUID do chamador de declaração para corresponder o filtro de ID de usuário do Xbox. |
| visibilidade| cadeia de caracteres| Um valor de enumeração que indica o status de visibilidade usado na filtragem de resultados. Atualmente, esse parâmetro só pode ser definido como aberto para incluir as sessões abertas. Ver <b>MultiplayerSessionVisibility</b>.|
| version| cadeia de caracteres| Um inteiro positivo que indica a versão principal de sessão ou inferior das sessões para incluir. O valor deve ser menor ou igual à versão do contrato da solicitação de módulo 100.|
| Take| cadeia de caracteres| Um inteiro positivo que indica o número máximo de sessões para recuperar.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4ELC"></a>


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


<a id="ID4EWC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EYC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

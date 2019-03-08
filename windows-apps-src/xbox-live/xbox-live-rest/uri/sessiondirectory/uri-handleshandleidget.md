---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
description: " GET (/handles/{handle-id})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 501d36f4d1ac079af15d6bb7f35a90d5328fc8db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598461"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
Recupera os identificadores especificados pela ID do identificador.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EDB)
  * [Códigos de status HTTP](#ID4EOB)
  * [Corpo da solicitação](#ID4EUB)
  * [Corpo da resposta](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST obtém a atividade atual de usuários para a sessão, para os identificadores especificados. O retorno é o objeto de sessão, com todos os seus atributos. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

O chamador desse método obtém a ID do identificador de um jogador **MultiplayerActivityDetails** objeto. Como alternativa, o chamador obtém a ID de uma ativação de protocolo depois que um usuário aceita um convite de jogo.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| handleId| GUID| A ID exclusiva do identificador para a sessão.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E5B"></a>


## <a name="response-body"></a>Corpo da resposta
Ver a estrutura de resposta no [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EKC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}](uri-handleshandleid.md)

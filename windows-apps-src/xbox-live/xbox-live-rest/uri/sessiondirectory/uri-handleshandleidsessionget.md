---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593891"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
Obtém um objeto de sessão para o identificador do identificador especificado.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EDB)
  * [Códigos de status HTTP](#ID4EOB)
  * [Corpo da solicitação](#ID4EVB)
  * [Corpo da resposta](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST recupera um objeto de sessão do servidor, usando o ponteiro do lado do serviço fornecido para a sessão (identificador). O retorno é o objeto de sessão, com todos os seus atributos. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

O chamador desse método obtém a ID do identificador de um jogador **MultiplayerActivityDetails** objeto. Como alternativa, o chamador obtém a ID de uma ativação de protocolo depois que um usuário aceita um convite de jogo.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| handleId| GUID| A ID exclusiva do identificador para a sessão.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Corpo da resposta
Ver a estrutura de resposta no [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EIC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)

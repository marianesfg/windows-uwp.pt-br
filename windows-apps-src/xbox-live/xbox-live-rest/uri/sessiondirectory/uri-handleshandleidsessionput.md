---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641601"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
Cria ou atualiza uma sessão ao desreferenciar um identificador.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4ECB)
  * [Códigos de status HTTP](#ID4ENB)
  * [Corpo da solicitação](#ID4EUB)
  * [Corpo da resposta](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST grava uma sessão nova ou atualizada para o serviço com vários participantes, usando a ID do identificador de sessão fornecido. O resultado é um objeto que representa a sessão nova ou atualizada, conforme retornado pelo servidor. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**.

O chamador desse método obtém a ID do identificador de um jogador **MultiplayerActivityDetails** objeto. Como alternativa, o chamador obtém a ID de uma ativação de protocolo depois que um usuário aceita um convite de jogo.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| handleId| GUID| A ID exclusiva do identificador para a sessão.|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Corpo da resposta

Não há objetos são enviados no corpo da resposta.

<a id="ID4EKC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)

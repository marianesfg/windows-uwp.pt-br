---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613401"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
Exclui os identificadores especificados pela ID do identificador.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EAB)
  * [Códigos de status HTTP](#ID4ELB)
  * [Corpo da solicitação](#ID4ESB)
  * [Corpo da resposta](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários
Esse método HTTP/REST exclui as alças para a ID especificada e limpa a atividade do usuário atual para a sessão. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**.  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| handleId| GUID| A ID exclusiva do identificador para a sessão.|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4ESB"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E4B"></a>


## <a name="response-body"></a>Corpo da resposta

Não há objetos são enviados no corpo da resposta.

<a id="ID4EIC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}](uri-handleshandleid.md)

---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589661"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

Remove um tíquete de correspondência.

> [!IMPORTANT]
> Esse método é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4E2)
  * [Autorização](#ID4EGB)
  * [Códigos de status HTTP](#ID4EOC)
  * [Corpo da solicitação](#ID4EXC)
  * [Corpo da resposta](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST exclui o ID do ticket especificado do hopper nomeado no nível de ID (SCID) de configuração do serviço. Esse método pode ser encapsulado por **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| name| cadeia de caracteres| O nome do hopper.|
| ticketId| GUID| A ID do tíquete.|

<a id="ID4EGB"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (ID de usuário)| sim| O usuário que fez a solicitação deve ser um membro da sessão de tíquete referenciado pelo tíquete.| 403|
| Privilégios e o tipo de dispositivo| sim| Quando deviceType do usuário é definido para o console, somente os usuários com o privilégio para múltiplos jogadores em suas declarações têm permissão para fazer chamadas para o serviço de cruzamento.| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EXC"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4ECD"></a>


## <a name="response-body"></a>Corpo da resposta

Não há objetos são enviados no corpo da resposta.

<a id="ID4EPD"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ERD"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

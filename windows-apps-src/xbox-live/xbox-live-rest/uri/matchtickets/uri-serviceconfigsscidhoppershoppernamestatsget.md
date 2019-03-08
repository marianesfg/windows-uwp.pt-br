---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621691"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Obtém as estatísticas para um hopper.

> [!IMPORTANT]
> Esse método é destinado para uso com o contrato 103 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 103 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4E5)
  * [Autorização](#ID4EJB)
  * [Códigos de status HTTP](#ID4E3C)
  * [Corpo da solicitação](#ID4EFD)
  * [Corpo da resposta](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários
Esse método HTTP/REST obtém as estatísticas do hopper nomeado no nível de ID (SCID) de configuração do serviço. Esse método pode ser encapsulado pela **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| scid| GUID| O identificador do configuração de serviço (SCID) para a sessão.|
| name| cadeia de caracteres| O nome do hopper.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (ID de usuário)| sim| O usuário que fez a solicitação deve ser um membro da sessão de tíquete referenciado pelo tíquete. | 403|
| Privilégios e o tipo de dispositivo| sim| Quando deviceType do usuário é definido para o console, somente os usuários com o privilégio para múltiplos jogadores em suas declarações têm permissão para fazer chamadas para o serviço de cruzamento. | 403|
| ID/prova do título do tipo de compra/dispositivo| sim| O título que está sendo correspondido em deve permitir emparelhamento para a declaração de título especificado, a combinação de tipo de dispositivo. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EQD"></a>


## <a name="response-body"></a>Corpo da resposta

| Membro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| cadeia de caracteres| O nome do hopper selecionado.|
| Tempo de espera| inteiro com sinal de 32 bits| Média de correspondência de tempo para o hopper (um número integral de segundos). |
| População| inteiro com sinal de 32 bits| O número de pessoas aguardando correspondências no hopper.|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ELE"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

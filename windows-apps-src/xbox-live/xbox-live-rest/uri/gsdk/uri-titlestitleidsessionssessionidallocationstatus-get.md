---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650851"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
Retorna o status de alocação do sessionhost identificado por sua ID de sessão. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
  * [Cabeçalhos de solicitação necessários](#ID4E4)
  * [Cabeçalhos de resposta necessária](#ID4EEB)
  * [Corpo da resposta](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
Nenhum.
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
Nenhum.
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Corpo da Resposta
 
Se a chamada for bem-sucedida, o serviço retornará um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | 
| description| Retorna vazio (esquerda em versões anteriores compatibilidade) de cadeia de caracteres.| 
| clusterId| Retorna vazio (esquerda em versões anteriores compatibilidade) de cadeia de caracteres.| 
| hostName| A URL do host da sessão.| 
| status| Indica em fila, atendida ou anulada.| 
| sessionHostId| ID do host da sessão.| 
| sessionId| O cliente fornecido (em tempo de alocação) ID de sessão.| 
| secureContext| O endereço de seguro de dispositivos.| 
| portMappings| Os mapeamentos de porta para a instância.| 
| Região| O local da instância.| 
| ticketId| A ID da sessão atual (esquerda em versões anteriores compatibilidade).| 
| gameHostId| O sessionHostId atual (esquerdo em versões anteriores compatibilidade).| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>Comentários
 
Um título só deverá repetir a chamada para o serviço quando os seguintes códigos de resposta são recebidos:
 
   * 200 — êxito 
   * 400 – solicitação contém parâmetros inválidos 
   * 401 – não autorizado 
   * 404 – a ID de título ou a ID do ticket era inválido ou não foi encontrado 
   * 500-Erro de servidor inesperado. 
    
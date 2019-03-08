---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632061"
---
# <a name="get-qosservers"></a>GET (/qosservers)
Chamado por um cliente para obter a lista de servidores de QoS disponíveis para uso com o Xbox Live Compute de URI. Os domínios para esses URIs são `gameserverds.xboxlive.com` e `gameserverms.xboxlive.com`.
 
  * [Cabeçalhos de solicitação necessários](#ID4EBB)
  * [Cabeçalhos de resposta necessária](#ID4EUC)
  * [Corpo da resposta](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nome do host

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
Ao fazer uma solicitação, os cabeçalhos mostrados na tabela a seguir são necessários.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | 
| Content-Type| aplicativo/json| Tipo de dados que estão sendo enviados.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Comprimento do objeto de solicitação.| 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
Uma resposta sempre incluirá os cabeçalhos mostrados na tabela a seguir.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| aplicativo/json| Tipo de dados no corpo da resposta.| 
| Content-Length|  | Comprimento do corpo da resposta.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>Corpo da Resposta
 
Se a chamada for bem-sucedida, o serviço retornará um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| Uma matriz de informações do servidor.| 
| serverFqdn| O nome de domínio totalmente qualificado do servidor.| 
| serverSecureDeviceAddress| O endereço do dispositivo segura do servidor.| 
| targetLocation| A localização geográfica do servidor.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>Consulte também
 [/qosservers](uri-qosservers.md)

  
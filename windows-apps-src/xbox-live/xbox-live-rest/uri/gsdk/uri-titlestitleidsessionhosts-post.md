---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47e3ecbf0a519b92ae467199e5d454523864310a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655141"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
Crie nova solicitação de cluster. O domínio para esses URIs é `gameserverms.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Cabeçalhos de solicitação necessários](#ID4EGB)
  * [Corpo da solicitação](#ID4E5B)
  * [Cabeçalhos de resposta necessária](#ID4ELD)
  * [Corpo da resposta](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros de URI
 
| Parâmetro| Descrição| 
| --- | --- | 
| titleId| ID do título que a solicitação deve operar.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nome do host

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
Ao fazer uma solicitação, os cabeçalhos mostrados na tabela a seguir são necessários.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | 
| Content-Type| aplicativo/json| Tipo de dados que estão sendo enviados.| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
A solicitação deve conter um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Esse é o chamador especificado identificador. Ele é atribuído ao host de sessão que é alocado e retornado. Posteriormente, você pode referenciar o sessionhost específico por este identificador. Ele deve ser exclusivo globalmente (ou seja, GUID).| 
| sandboxId| A área de segurança que você deseja que o host da sessão a ser alocado no.| 
| cloudGameId| O identificador de jogo de nuvem.| 
| Locais| A lista ordenada de locais preferenciais, de que você gostaria que a sessão a ser alocado.| 
| sessionCookie| Esse é um chamador especificado cadeia de caracteres opaca. Ele está associado com o sessionhost e pode ser referenciado em seu código de jogo. Use esse membro para passar uma pequena quantidade de informações do cliente para o servidor (o tamanho máximo é de 4KB).| 
| gameModelId| O identificador do modo de jogo.| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
Nenhum.
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>Corpo da Resposta
 
Se a chamada for bem-sucedida, o serviço retornará um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| hostName| O nome do host da instância.| 
| portMappings| Os mapeamentos de porta.| 
| Região| A instância de região é hospedado no.| 
| secureContext| O endereço de seguro de dispositivos.| 
 
<a id="ID4ESE"></a>

 
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
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Comentários
 
Um título só deverá repetir a chamada para o serviço quando os seguintes códigos de resposta são recebidos:
 
   * 200 — êxito - resposta retornada.
   * 400 — parâmetros inválidos ou o corpo da solicitação malformada.
   * 401 – não autorizado
   * 404 – id do título não tem nenhuma assinatura atribuída a ele.
   * 409 — quando a solicitação idênticas são feitas (mesma sessionId) em aproximadamente na mesma hora, essa resposta é possível. Se uma solicitação de alocação é feita e um host de sessão já tem a sessionId especificada e já está ativo, retornaremos informações detalhadas sobre esse sessionhost. Se o host da sessão, no entanto, não estiver ativo, ainda, você receberá um conflito.
   * 500-Erro de servidor inesperado.
   * 503 — nenhuma sessionhosts StandingBy. Repita a solicitação quando alguns desses recursos são gratuitos.
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>Consulte também
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  
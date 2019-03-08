---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608901"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
URI que permite que um cliente criar uma instância de servidor Xbox Live Compute. O domínio para esses URIs é `gameserverms.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Cabeçalhos de solicitação necessários](#ID4EGB)
  * [Autorização](#ID4ELD)
  * [Corpo da solicitação](#ID4EWD)
  * [Cabeçalhos de resposta necessária](#ID4EZE)
  * [Corpo da resposta](#ID4E5G)
 
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
| Agente do usuário|  | Informações sobre o agente do usuário está fazendo a solicitação.| 
| Content-Type| aplicativo/json| Tipo de dados que estão sendo enviados.| 
| Host| gameserverms.xboxlive.com|  | 
| Content-Length|  | Comprimento do objeto de solicitação.| 
| x-xbl-contract-version| 1| Versão do contrato de API.| 
| Autorização| XBL3.0 x = [hash]; [token]| Token de autenticação.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>Autorização
 
A solicitação deve incluir um cabeçalho de autorização válido Xbox Live. Se o chamador não tem permissão para acessar este recurso, o serviço retorna 403 Proibido em resposta. Se o cabeçalho é inválido ou ausente, o serviço retornará o 401 não autorizado em resposta.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
A solicitação deve conter um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Identificador da sessão do MPSD.| 
| abortIfQueued| Parâmetro opcional, que, quando definido como true informa ao GSMS não para a fila desta sessão para um recurso se ele não pode ser atendido imediatamente. Se a solicitação foi anulada porque o valor for true, o objeto de resposta irá conter <code>"fulfillmentState" : "Aborted"</code>. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
Uma resposta sempre incluirá os cabeçalhos mostrados na tabela a seguir.
 
| Cabeçalho| Valor| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | Diretivas que devem ser obedecidas por todos os mecanismos de cache ao longo da cadeia de solicitação/resposta.| 
| Content-Type| aplicativo/json| Tipo de dados na resposta.| 
| Content-Length|  | Comprimento do corpo da resposta.| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | O tipo mime do corpo da resposta.| 
| Data|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>Corpo da Resposta
 
Se a chamada for bem-sucedida, o serviço retornará um objeto JSON com os seguintes membros.
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| O intervalo em ms para sondar a conclusão de recomendado. Observe que isso não é uma estimativa de quando o cluster estará pronto, mas em vez disso, uma recomendação para a frequência com que o chamador deve sondar uma atualização de status, considerando o pool de assinaturas e taxas de solicitação e de preenchimento atual.| 
| fulfillmentState| Indica se a sessão fornecida foi alocada um recurso imediatamente, "Atendida," adicionada à fila para a disponibilidade de um recurso de futuros, "em fila", ou anulada, "Interrompida", devido à incapacidade para atender à solicitação imediatamente quando a solicitação abortIfQueued especificado como "true". | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Comentários
 
Um título só deverá repetir a chamada para o serviço quando os seguintes códigos de resposta são recebidos:
 
   * 408 – tempo limite do servidor
   * 429 — número excessivo de solicitações
   * 500-Erro de servidor
   * 502-Gateway incorreto
   * 503 – serviço não disponível
   * 504 – tempo limite do gateway
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>Consulte também
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  